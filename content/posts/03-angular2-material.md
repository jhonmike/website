---
title: "Angular2 Material"
slug: "angular2-material"
date: 2016-09-25T01:06:40-02:00
draft: false
tags: ["angular", "javascript"]
categories: ["Na Prática"]
---

# Part 03 - Dando uma cara para o projeto no estilo google app

Nesse pequeno e simples tutorial vamos dar continuidade no projetinho que você pode conferir a part1 e a part2, então vamos dar uma cara ao projeto usando o Angular Material 2 desda instalação até adicionarmos alguns componentes em nosso template.

Adicionando os módulos que vamos usar no package.json, detalhe não vamos adicionar todos os módulos do angular material, apenas o core e os 5 primeiros componentes que peguei da lista no atual readme do projeto no caso o core e os componentes md-button, md-card, md-checkbox, md-icon, md-input e md-radio.

```sh
npm install --save @angular2-material/core @angular2-material/button @angular2-material/card @angular2-material/checkbox @angular2-material/icon @angular2-material/input @angular2-material/radio
```

Vamos também adicionar o modulo HammerJS ele é uma dependência do componente md-icon e é usando para dar suporte em gestos no touch.

```sh
npm install hammerjs --save
typings install dt~hammerjs --save --global
```

Nosso package.json deve ficar assim:

```js
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
    "@angular2-material/button": "^2.0.0-alpha.8-2", <<
    "@angular2-material/card": "^2.0.0-alpha.8-2", <<
    "@angular2-material/checkbox": "^2.0.0-alpha.8-2", <<
    "@angular2-material/core": "^2.0.0-alpha.8-2", <<
    "@angular2-material/icon": "^2.0.0-alpha.8-2", <<
    "@angular2-material/input": "^2.0.0-alpha.8-2", <<
    "@angular2-material/radio": "^2.0.0-alpha.8-2", <<
    "angular2-in-memory-web-api": "0.0.20",
    "core-js": "^2.4.1",
    "hammerjs": "^2.0.8", <<
    "reflect-metadata": "^0.1.3",
    "rxjs": "5.0.0-beta.12",
    "systemjs": "0.19.27",
    "zone.js": "^0.6.23"
  },
  "devDependencies": {
    "concurrently": "^2.2.0",
    "lite-server": "^2.2.2",
    "typescript": "^2.0.2",
    "typings": "^1.3.2"
  }
}
```

agora vamos adicionar os módulos no systemjs.config.js para que o autoload dos módulos funcionem corretamente, fique de olho nos itens com o comentário // <<

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
      'angular2-in-memory-web-api': 'npm:angular2-in-memory-web-api',
      '@angular2-material/core': 'npm:@angular2-material/core/core.umd.js', // <<
      '@angular2-material/card': 'npm:@angular2-material/card/card.umd.js', // <<
      '@angular2-material/button': 'npm:@angular2-material/button/button.umd.js', // <<
      '@angular2-material/icon': 'npm:@angular2-material/icon/icon.umd.js', // <<
      '@angular2-material/radio': 'npm:@angular2-material/radio/radio.umd.js', // <<
      '@angular2-material/checkbox': 'npm:@angular2-material/checkbox/checkbox.umd.js', // <<
      '@angular2-material/input': 'npm:@angular2-material/input/input.umd.js' // <<
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

no arquivo app.module.ts temos que registar os módulos dos componentes e suas providers necessárias, ele deve ficar da seguinte forma:

```js
// app.module.ts
import {NgModule}      from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {MdUniqueSelectionDispatcher} from "@angular2-material/core";
import {MdCardModule} from "@angular2-material/card";
import {MdButtonModule} from "@angular2-material/button";
import {MdIconModule} from "@angular2-material/icon";
import {MdRadioModule} from "@angular2-material/radio";
import {MdCheckboxModule} from "@angular2-material/checkbox";
import {MdInputModule} from "@angular2-material/input";
import {routing}       from "./app.rounting";

import {MdIconRegistry} from '@angular2-material/icon';

import {AppComponent}  from './app.component';
import {HomeComponent} from "./screen/home/home.component";

@NgModule({
    imports: [
        BrowserModule,
        MdCardModule,
        MdButtonModule,
        MdIconModule,
        MdRadioModule,
        MdCheckboxModule,
        MdInputModule,
        routing
    ],
    declarations: [
        AppComponent,
        HomeComponent
    ],
    providers: [MdIconRegistry, MdUniqueSelectionDispatcher],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

pouco antes de começarmos a adicionar componentes em nosso template, adicione a fonte do material icon no seu index.html

```html
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

e agora pra finalizar vamos criar um pequeno template com alguns componentes que instalamos de exemplo, editando o arquivo screen/home/home.component.ts

```js
import {Component} from "@angular/core";

@Component({
    selector: 'app-home',
    template: `
        <div class="app-content">
            <md-card>
                <button md-button>FLAT</button>
                <button md-raised-button>RAISED</button>
                <button md-fab>
                    <md-icon class="md-24">add</md-icon>
                </button>
                <button md-raised-button color="primary">PRIMARY</button>
                <button md-button disabled>OFF</button>
                <button md-mini-fab [disabled]="isDisabled"><md-icon>check</md-icon></button>
            </md-card>
            <md-card>
               <md-card-subtitle>Subtitle first</md-card-subtitle>
               <md-card-title>Card with title</md-card-title>   
               <md-card-content>
                    <p>This is supporting text.</p>
                    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do 
                    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad</p>
               </md-card-content>
               <md-card-actions>
                    <button md-button>LIKE</button>
                    <button md-button>SHARE</button>
               </md-card-actions>
            </md-card>
            <md-card>
                <md-checkbox [checked]="true" align="end">
                  I come after my label.
                </md-checkbox>
                <md-checkbox [checked]="false" aria-label="My standalone checkbox"></md-checkbox>
                <md-radio-group>
                    <md-radio-button value="option_1">1</md-radio-button>
                    <md-radio-button value="option_2">2</md-radio-button>
                </md-radio-group>
            </md-card>
            <md-card>
                <md-input placeholder="amount" align="end">
                    <span md-prefix>$&nbsp;</span>
                    <span md-suffix>.00</span>
                </md-input>
                <md-input placeholder="Company">
                </md-input>
                <md-input placeholder="Company (disabled)" disabled value="Google">
                </md-input>
            </md-card>
        </div>
    `,
    styles: [`
        .app-content {
            padding: 20px;
        }
        .app-content md-card {
            margin: 20px;
        }
    `]
})
export class HomeComponent { }
```

o resultado esperado seria algo como:

![screenshot](https://cdn-images-1.medium.com/max/800/1*VWVlF2s2cLj567GzDUN5fQ.png)

espero que tenha ajudado principalmente quem esta começando e até a próxima.
