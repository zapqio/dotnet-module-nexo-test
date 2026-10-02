# Zapqio Runner - moduł Nexo.TestConnect

Metoda **"Who am I (test)"**: loguje się do InsERT nexo przez współdzielony `NexoClient` z paczki
[Nexo.Connection](https://github.com/zapqio/dotnet-module-nexo-connection) i zwraca sygnaturę zalogowanego
operatora. Służy do sprawdzenia instalacji: SDK, danych w `nexoModule.json` i mechanizmu modułów
współdzielonych. Jest też wzorcem csproj dla własnego modułu na Nexo.Connection.

Od Nexo.Connection 1.1.0 taka metoda (**„Nexo: Who am I”**) jest w samej paczce Connection, więc do sprawdzenia
instalacji ten moduł nie jest już potrzebny - instaluje ją `install-nexo.ps1` z repo Connection. To repo zostaje
jako wzorzec csproj i kodu modułu konsumenckiego.

## Instalacja

`Nexo.TestConnect.zip` do `Modules\` runnera obok `Nexo.Connection.zip` i `Nexo.Sdk.zip` (SDK InsERT
w wersji Subiekta, pakuje je `update-nexo-sdk.ps1` z repo Nexo.Connection), restart usługi, w panelu Web
metoda "Who am I (test)". Bez którejś z tych dwóch paczek runner zgłosi w logu
`Metoda Nexo.TestConnect z modułu Nexo.TestConnect nie została utworzona ...` i ogłosi się bez tej metody.
SDK w innej wersji niż Subiekt kończy zadanie błędem `SDK <wersja> (paczka Nexo.Sdk) nie połączył się
z Subiektem: ...` z instrukcją, co uruchomić.

## Budowanie

Wymaga SDK InsERT nexo w `C:\nexoSDK_<wersja>\Bin\` (inna ścieżka: `nexoSdkBinPath` w csproj) oraz
publicznych paczek `Zapqio.Runner.Module.Core` `1.2.0` i `Zapqio.Nexo.Connection` `1.1.3`
z nuget.org. Wymagane jest .NET SDK 8 lub nowsze. `dotnet publish -c Release` daje `bin\Release\Nexo.TestConnect.zip`: tylko
`Nexo.TestConnect.dll`, `.deps.json` i `##Dll`. SDK i `Nexo.Connection.dll` przychodzą w runtime
z paczki współdzielonej.

## Wdrażanie z Weba

Wybierz tryb budowania w konfiguracji repozytorium:

- **Runner**: po zainstalowaniu Connection uruchom na serwerze `install-nexo-build.ps1` z repo
  `dotnet-module-nexo-connection` (albo użyj `install-nexo.ps1 -BuildTools` przy pierwszej instalacji).
  Przygotuje referencje SDK oraz NuGet dla konta usługi. Web wysyła źródła, runner buduje i instaluje ZIP.
- **CI**: workflow `.github/workflows/build.yml` buduje moduł przy pushu na `main` oraz ręcznie przez
  `workflow_dispatch`. Przywraca Core i Connection z nuget.org, pobiera SDK wskazane w faktycznie
  przywróconej paczce Connection i publikuje artefakt `zapqio-module` z plikiem `module.zip`. Zaczekaj na udany build dokładnie tego
  commita, który wybierasz do wdrożenia. Artefakt jest przechowywany przez 30 dni; po jego wygaśnięciu
  uruchom workflow ponownie dla tego samego commita/refa. Pull requesty nie publikują paczek do wdrażania.

W obu trybach na docelowym runnerze nadal są potrzebne `Nexo.Connection.zip`, `Nexo.Sdk.zip`
i poprawny `Config\nexoModule.json`; instaluje je `install-nexo.ps1`. CI sprawdza budowanie i format
paczki, a połączenie z bazą nexo sprawdza się już na docelowym runnerze metodą „Who am I (test)”.

Wersje paczek ustala `Nexo.TestConnect.csproj`; workflow odczytuje wynik restore zamiast utrzymywać
osobny numer Connection. Zmianę wersji połącz z weryfikacją zgodności ZIP-a Connection na serwerze.
Wersję SDK do kompilacji wskazuje przywrócona paczka, a SDK runtime dobiera instalator do bazy nexo.
