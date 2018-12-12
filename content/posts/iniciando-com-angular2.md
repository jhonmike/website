---
title: "Iniciando com Angular2"
slug: "iniciando-com-angular2"
date: 2016-09-17T23:38:02-02:00
draft: false
tags: ["angular", "javascript"]
categories: ["Na Prática"]
---

#  Configuração do Projeto

Finalmente saiu a versão estável do Angular 2, depois de suas 71 releases :p agora estamos com nosso framework totalmente reescrito e componentizado.

Quero demonstrar neste post apenas como estou trabalhando com o Angular2, talvez não seja o melhor padrão ou o mais completo, mas para mim está fazendo sentido aos poucos estou melhorando, e sempre que possível vou postar novidades sobre como está o projeto e também conforme atualizações do framework vou atualizando o source do tutorial.

Requisitos mínimos NodeJS instalado e NPM. Sem mais delongas, começamos pelo package.json
```js
// package.json
{
  "name": "jm-default",
  "version": "1.0.0",
  "scripts": {
    "start": "tsc && concurrently \"npm run tsc:w\" \"npm run lite\" ",
    "lite": "lite-server",
    "postinstall": "typings install",
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "typings": "typings"
  },
  "license": "ISC",
  "dependencies": {
    "@angular/common": "^2.0.0",
    "@angular/compiler": "^2.0.0",
    "@angular/core": "^2.0.0",
    "@angular/forms": "^2.0.0",
    "@angular/http": "^2.0.0",
    "@angular/platform-browser": "^2.0.0",
    "@angular/platform-browser-dynamic": "^2.0.0",
    "@angular/router": "^3.0.0",
    "@angular/upgrade": "^2.0.0",
    "angular2-in-memory-web-api": "0.0.20",
    "core-js": "^2.4.1",
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.0-beta.12",
    "systemjs": "0.19.27",
    "zone.js": "^0.6.23"
  },
  "devDependencies": {
    "concurrently": "^2.2.0",
    "lite-server": "^2.2.2",
    "typescript": "^2.0.2",
    "typings":"^1.3.2"
  }
}
```

alguns scripts para facilitar a vida, um servidor NodeJS, TypeScript o transpiler que o Angular2 utiliza por padrão, e suas libs, bom podemos executar as instalações do NPM

```sh
npm install -g typescript
npm install
```

vamos agora criar os demais arquivos… o index.html é como qualquer index de projetos web, temos a estrutura padrão do html e algumas libs injetadas como core.js, zone.js, reflect-metadata e systemjs que são dependências para rodarmos o Angular2, não deixe de notar a tag my-app vamos falar sobre ela logo mais.

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=Edge">
  <meta charset="UTF-8">
  <title>Jhon Mike — Default Angular2</title>
  <link rel="icon" href="assets/img/favicon.png">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <!-- 1. Load libraries -->
  <!-- Polyfill(s) for older browsers -->
  <script src="node_modules/core-js/client/shim.min.js"></script>
  <script src="node_modules/zone.js/dist/zone.js"></script>
  <script src="node_modules/reflect-metadata/Reflect.js"></script>
  <script src="node_modules/systemjs/dist/system.src.js"></script>
  <!-- 2. Configure SystemJS -->
  <script src="systemjs.config.js"></script>
  <script>
    System.import('app').catch(function(err){ console.error(err); });
  </script>
</head>
<!-- 3. Display the application -->
<body>
  <my-app>
    Carregando....
  </my-app>
</body>
</html>
```

não tem segredo apenas vamos prestar atenção no pequeno trecho de script o System.import que vai fazer a importação do nosso app que esta configurado no próximo arquivo o systemjs.config.js que vai ter as configurações e injeções de outras libs do nosso app.

```js
// systemjs.config.js
(function(global) {
  System.config({
    paths: {
      // paths serve as alias
      'npm:': 'node_modules/'
    },
    map: {
      app: 'src', //app or dist
      // angular bundles
      '@angular/core': 'npm:@angular/core/bundles/core.umd.js',
      '@angular/common': 'npm:@angular/common/bundles/common.umd.js',
      '@angular/compiler': 'npm:@angular/compiler/bundles/compiler.umd.js',
      '@angular/platform-browser': 'npm:@angular/platform-browser/bundles/platform-browser.umd.js',
      '@angular/platform-browser-dynamic': 'npm:@angular/platform-browser-dynamic/bundles/platform-browser-dynamic.umd.js',
      '@angular/http': 'npm:@angular/http/bundles/http.umd.js',
      '@angular/router': 'npm:@angular/router/bundles/router.umd.js',
      '@angular/forms': 'npm:@angular/forms/bundles/forms.umd.js',
      // other libraries
      'rxjs':                       'npm:rxjs',
      'angular2-in-memory-web-api': 'npm:angular2-in-memory-web-api'
    },
    // packages tells the System loader how to load when no filename and/or no extension
    packages: {
      app: {main: './main.js', defaultExtension: 'js'},
      rxjs: {defaultExtension: 'js'},
      'angular2-in-memory-web-api': {main: './index.js', defaultExtension: 'js'}
    }
  });
})(this);
```

bom este arquivo é o nosso “autoload” vamos dizer que ele vai criar alguns alias aos módulos do npm facilitando o autoload dos mesmos na aplicação e também vamos registar em maps onde ele encontrara nosso app.

O registro dos módulos utilizados em seguida no typings.json que também é uma dependência das libs do angular, mas utilizado para compilação do código typescript, onde as libs setadas nele servem para trazer espelhos das libs originais algo tipo um pacote de interfaces que vai auxiliar o transpiler na hora da compilação

```js
// typings.json
{
  "globalDependencies": {
    "core-js": "registry:dt/core-js#0.0.0+20160602141332",
    "jasmine": "registry:dt/jasmine#2.2.0+20160621224255",
    "node": "registry:dt/node#6.0.0+20160807145350"
  }
}
```

e pra finalizar a parte de configurações vamos utilizar o tsconfig.json que vai trazer as configurações da compilação que o typescript vai executar, como versão que devera gerar, modo de importação de módulos entre outras infos

```js
// tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  "exclude": [
    "node_modules"
  ]
}
```

Agora a partir daqui iniciaremos a criação do módulo angular e os componentes iniciais

crie a pasta src dentro do seu projeto se preferir usar o padrão use app mas não esqueça de voltar aos passos anteriores e substituir o nome das pastas

dentro da pasta src vamos criar algumas arquivos como o main.ts definido no arquivo systemjs.config.js como o main do app em questão, vamos criar também o arquivo app.module.ts e o arquivo do nosso primeiro componente ou container do projeto o app.component.ts…

src/main.ts será nosso bootstrap, nosso start da aplicação

```js
// main.ts
import {platformBrowserDynamic} from '@angular/platform-browser-dynamic';
import {enableProdMode} from '@angular/core';

import {AppModule} from './app.module';

const platform = platformBrowserDynamic();
platform.bootstrapModule(AppModule);
```

src/app.module.ts, o AppModule será onde vamos registar todos os componentes do módulo e sub módulos que ficara para outro tutorial, este arquivinho tera grande chance de crescer e muito dentro do projeto, por isso sua organização e cuidados com indentação serão indispensaveis.

```js
// src/app.module.ts
import {NgModule}      from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {AppComponent}  from './app.component';

@NgModule({
    imports: [BrowserModule],
    declarations: [AppComponent],
    providers: [ ],
    bootstrap: [ AppComponent ]
})
export class AppModule { }
```

src/app.component.ts este primeiro componente não tem muito o que explicar, ele recebe a annotation Component do módulo core do Angular e setamos um selector para ele e um template apenas com uma simples string de Hello World, mas preste a atenção pois esse selector my-app é o mesmo que setamos lá no index.html e é aquela tag que sera o nosso root do projeto

```js
// src/app.component.ts
import {Component} from '@angular/core';

@Component({
    selector: 'my-app',
    template: `Hello World!`
})
export class AppComponent { }
```

obs.: Sua estrutura de pasta deve ser algo como…

```
| -node_modules
| -src
| | -app.component.ts
| | -app.module.ts
| | -main.ts
| -typings
| -index.html
| -package.json
| -systemjs.config.js
| -tsconfig.json
| -typings.json
```

bom está feito, vamos iniciar nossa aplicação com o seguinte comando:

vai abrir a tela do navegador com nosso Hello World em NG2! bom acho que próximo post poderemos criar mais componentes e usar o angular-router3 ah! sim angular3 hahaha! deixe sua sugestão nos comentários e até a próxima!
