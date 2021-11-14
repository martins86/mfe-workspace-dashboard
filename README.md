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
