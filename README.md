# GodWeer / Excel Client

Fabric-мод Excel Client для Minecraft 1.21.11 + Electron-лаунчер, который сам
скачивает ванильный Minecraft, Fabric и мод, а затем запускает игру.

## Структура

- `src/` — Fabric-мод `gg.excelclient` (Minecraft 1.21.11, Fabric Loader 0.19.3)
- `loader/` — Electron-лаунчер (мод грузится из `client/excelclient.jar`)
- `publish.ps1` — сборка мода + лаунчера и публикация релиза на GitHub

## Запуск лаунчера (для игроков)

1. Установите Java 21+ (JDK): https://adoptium.net
2. Распакуйте `ExcelClient-Setup-*.exe` и запустите, либо из исходников:
   ```
   cd loader
   npm install
   npm start
   ```
3. Вход: логин `Admin`, пароль `1337`.
4. Укажите никнейм и (если Java не найдена) путь к `java.exe`.
5. Нажмите «Запуск». При первом запуске лаунчер скачает:
   - ванильный клиент 1.21.11, ассеты (~430 МБ) и библиотеки (~120 МБ),
   - Fabric Loader 0.19.3 и Fabric API,
   - `excelclient.jar` (последняя версия из GitHub Release).

## Сборка мода

```
java -jar gradle/wrapper/gradle-wrapper.jar build --no-daemon
```
Результат: `build/libs/excelclient-<version>.jar`.

## Публикация релиза

Нужен токен GitHub с правом `repo`:

```powershell
$env:GITHUB_TOKEN = "ghp_..."
.\publish.ps1            # сборка и релиз (мод + лаунчер)
.\publish.ps1 -Draft     # черновик
.\publish.ps1 -SkipLauncher  # только мод
```

Релиз должен содержать два ассета:
- `excelclient-<version>.jar` — клиентский мод
- `ExcelClient-Setup-<version>.exe` — инсталлятор лаунчера

В лаунчере укажите репозиторий в формате `owner/repo` в настройках, чтобы
включить проверку обновлений (кнопка «Проверить обновления»).

Версия клиента задаётся в `loader/config.js` (`VERSIONS.clientVersion`),
обязательно синхронизируйте её с `gradle.properties` (`mod_version`)
и `fabric.mod.json` (`version`).

## Сеть

- `loader/config.js` — версии и настройки по умолчанию
- `loader/launcher-engine.js` — скачивание и запуск Minecraft
- `loader/updater.js` — обновления через GitHub Releases
- `loader/app.js` — Electron main + IPC
