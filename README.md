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
paczek NuGet `Zapqio.Runner.Module.Core` i `Zapqio.Nexo.Connection` w źródle widocznym dla
`dotnet restore`. `dotnet publish -c Release` daje `bin\Release\Nexo.TestConnect.zip`: tylko
`Nexo.TestConnect.dll`, `.deps.json` i `##Dll`. SDK i `Nexo.Connection.dll` przychodzą w runtime
z paczki współdzielonej.
