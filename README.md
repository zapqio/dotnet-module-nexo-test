# Zapqio Runner - moduł Nexo.TestConnect

Metoda **"Who am I (test)"**: loguje się do InsERT nexo przez współdzielony `NexoClient` z paczki
[Nexo.Connection](https://github.com/zapqio/module-nexo-connection) i zwraca sygnaturę zalogowanego
operatora. Służy do sprawdzenia instalacji: SDK, danych w `nexoModule.json` i mechanizmu modułów
współdzielonych. Jest też wzorcem csproj dla własnego modułu na Nexo.Connection.

## Instalacja

`Nexo.TestConnect.zip` do `Modules\` runnera obok `Nexo.Connection.zip`, restart usługi, w panelu Web
metoda "Who am I (test)". Bez `Nexo.Connection.zip` runner zgłosi w logu
`Metoda Nexo.TestConnect z modułu Nexo.TestConnect nie została utworzona ...` i ogłosi się bez tej metody.

## Budowanie

Wymaga SDK InsERT nexo w `C:\nexoSDK_<wersja>\Bin\` (inna ścieżka: `nexoSdkBinPath` w csproj) oraz
paczek NuGet `Zapqio.Runner.Module.Core` i `Zapqio.Nexo.Connection` w źródle widocznym dla
`dotnet restore`. `dotnet publish -c Release` daje `bin\Release\Nexo.TestConnect.zip`: tylko
`Nexo.TestConnect.dll`, `.deps.json` i `##Dll`. SDK i `Nexo.Connection.dll` przychodzą w runtime
z paczki współdzielonej.
