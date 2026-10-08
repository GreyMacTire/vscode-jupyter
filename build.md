# Локальная сборка корпоративного VSIX (ms-toolsai.jupyter)

## Окружение

- Node.js **22.15.1** (см. `.nvmrc`), npm 10.x;
- доступ к `registry.npmjs.org` и `github.com` (бинарники zeromq);
- `vsce` глобально (один раз): `npm i -g @vscode/vsce`.

## Шаги

```bash
nvm use
npm ci --ignore-scripts --prefer-offline --no-audit
npx vscode-dts 1.105.0            # API-типы версии релиза (вместо dev — важно!)
node ./build/ci/postInstall.js     # патчи node_modules + загрузка zmq-бинарников
npm run package                    # → ms-toolsai-jupyter-insiders.vsix
```

## Важно

- **Не запускать `npm run postinstall`**: его шаг `download-api` скачает свежий
  dev-`vscode.d.ts`, и сборка упадёт на тестовых моках (`QuickPick.prompt`).
- Если загрузка zmq-бинарников падает с 403 — повторить шаг `postInstall.js`
  или задать `GITHUB_TOKEN`.
- После установки VSIX отключить автообновление расширений
  (`extensions.autoUpdate: false`), иначе VS Code заменит сборку версией из marketplace.

## Установка

```bash
code --install-extension ms-toolsai-jupyter-insiders.vsix
```
