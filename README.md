# Micro Front-end: Dashboard [![Build Status](https://app.travis-ci.com/martins86/mfe-workspace-dashboard.svg?token=ifxsnzyowyXksHqjSXVp&branch=master)](https://app.travis-ci.com/martins86/mfe-workspace-dashboard)

## Informações

Angular - [Link](https://angular.io/) <br />
Angular Module Federation - [Angular Architects](https://www.angulararchitects.io/en/aktuelles/the-microfrontend-revolution-part-2-module-federation-with-angular/) - [Npmjs](https://www.npmjs.com/package/@angular-architects/module-federation-tools/v/12.4.0) <br />
ESLint builder - [Link](https://github.com/angular-eslint/angular-eslint) <br />
Prettier - [Link](https://prettier.io/) <br />
Lint Staged - [Link](https://github.com/okonet/lint-staged#readme) <br />
Husky - [Link](https://typicode.github.io/husky/#/) <br />

<br>

## Dependências Globais

```sh
## Instalando o Angular Global
npm install -g @angular/cli@12.2.13
```

<br>

## Comandos Usados

```sh
## Criando workspace do dashboard
ng new mfe-workspace-dashboard --create-application=false --commit=true --prefix=mfe
```

```sh
## Acessando o workspace
cd mfe-workspace-dashboard

# Adicionando eslint e incluindo no cli angular.json
ng add @angular-eslint/schematics
ng config cli.defaultCollection @angular-eslint/schematics
```

```sh
## Criando .eslintrc.json root
{
  "root": true,
  "ignorePatterns": ["projects/**/*"],
  "overrides": [
    {
      "files": ["*.ts"],
      "parserOptions": {
        "project": ["tsconfig.json", "e2e/tsconfig.json"],
        "createDefaultProgram": true
      },
      "extends": [
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates"
      ],
      "rules": {}
    },
    {
      "files": ["*.html"],
      "extends": ["plugin:@angular-eslint/template/recommended"],
      "rules": {}
    }
  ]
}
```

```sh
## Gerando MFE app dashboard com eslint, routing e style
ng g @angular-eslint/schematics:app dashboard --routing=true --strict=true --style=scss --prefix=mfe
```

```sh
## Instalando o ngx build plus e o module federation
npm install ngx-build-plus@12.2.0
npm install @angular-architects/module-federation@12.5.3
npm install @angular-architects/module-federation-tools@12.5.3
```

```sh
## Adicionando a configuração do module federation no mfe dashboard com a porta 5001
ng add @angular-architects/module-federation --project=dashboard --port=5001
```

```sh
## Server ou Build do dashboard
ng serve --project=dashboard --port=5001 --host=0.0.0.0 --disable-host-check --open

ng build --project=dashboard --base-href ./ --single-bundle=true --output-hashing=none --vendor-chunk=false --aot
```

```sh
## Editando o .karma.conf.js
# Adicionando mínimo de cobertura esperado
thresholds: {
  emitWarning: false,
  global: {
    statements: 80,
    lines: 80,
    branches: 80,
    functions: 80,
  },
  each: {
    statements: 80,
    lines: 80,
    branches: 80,
    functions: 80,
  },
},

# Adicionando coverage
reporters: ["progress", "coverage", "kjhtml"],

# Adicionando ChromeHeadless e tolerância para Timeout
customLaunchers: {
  ChromeHeadlessNoSandbox: {
    base: "ChromeHeadless",
    flags: ["--headless", "--no-sandbox", "--remote-debugging-port=9222"],
  },
},
browserDisconnectTolerance: 8,
browserNoActivityTimeout: 60000,
browserDisconnectTimeout: 20000,
captureTimeout: 210000,
```

```sh
## Adicionando configuração do prettier
# Criando o .prettierignore
build
coverage
node_modules
dist

# Criando o .prettierrc.json
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": true
}
```

```sh
## Adicionando o Prettier, editando e checando
npm install prettier --save-dev --save-exact

# Editando e checando
npx prettier --write --ignore-unknown .
npx prettier --check .
```

```sh
## Adicionando scripts do prettier no package.json
npm set-script prettier-write "npx prettier --write --ignore-unknown ."
npm set-script prettier-check "npx prettier --check ."
```

```sh
## Criando o .lintstagedrc.json
{
  "*.{js,jsx,ts,tsx,md,html,scss,json}": [
    "npm run prettier-write",
    "git add",
    "npm run prettier-check"
  ]
}
```

```sh
## Adicionando o Lint Staged
npm install lint-staged --save-dev
```

```sh
## Adicionando o Husky
npm install husky --save-dev
npx husky install
```

```sh
## Configurando o Husky
npx husky add .husky/pre-commit "npm run pre-commit"

# Adicionando scripts para husky no package.json
npm set-script test "ng test --no-watch --no-progress --code-coverage --browsers ChromeHeadlessNoSandbox"
npm set-script pre-commit "npx --no-install lint-staged && npm run lint && npm run test"
npm set-script postinstall "npx husky install"
```

```sh
## Adicionando scripts de serve:dashboard, test:dev e build:dashboard no package.json
npm set-script serve:dashboard "ng serve --project=dashboard --port=5001 --host=0.0.0.0 --disable-host-check --open"
npm set-script test:dev "ng test --code-coverage --progress --browsers Chrome"
npm set-script build:dashboard "ng build --project=dashboard --base-href ./ --single-bundle=true --output-hashing=none --vendor-chunk=false --aot"

```

```sh
## Adicionando o Travis CI
# Criando o Token GITHUB_TOKEN_TRAVIS
https://github.com/settings/tokens

# martins86 / mfe-workspace-dashboard
https://app.travis-ci.com/github/martins86/mfe-workspace-dashboard

# Criando o .travis.yml
language: node_js

os: linux

node_js:
  - node

dist: trusty

sudo: required

addons:
  chrome: stable

cache:
  yarn: true
  directories:
    - node_modules

install:
  - npm install -g @angular/cli@12.2.13
  - npm install

before_install:
  - export DISPLAY=:99.0
  - export NODE_OPTIONS=--openssl-legacy-provider
  - sh -e /etc/init.d/xvfb start

script:
  - npm run lint
  - npm run test
  - npm run build:dashboard
  - cd dist/dashboard
  - cp index.html 404.html

branches:
  only:
    - master

env:
  - EMBER_VERSION=release

jobs:
  fast_finish: true
  allow_failures:
    - env: EMBER_VERSION=release

deploy:
  provider: pages
  skip_cleanup: true
  github_token: $GITHUB_TOKEN_TRAVIS
  local_dir: dist/dashboard
  on:
    branch: master
```

```sh
## Adicionando o modulo, rotas e component do página dashboard
ng g m pages/dashboard/ --module app --routing --project dashboard
ng g c pages/dashboard/ --project dashboard
```
