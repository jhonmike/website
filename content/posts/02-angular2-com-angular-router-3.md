---
title: "Angular2 com Angular Router 3"
slug: "angular2-com-angular-router-3"
date: 2016-09-19T02:06:40-02:00
draft: false
tags: ["angular", "javascript"]
categories: ["Na Prática"]
---

# Part 02 - Configuração das rotas

Bom vamos dar continuidade à base do projeto do primeiro post, partindo agora para as rotas e criando nossa primeira tela.

Já configuramos o module router na part1 do tutorial, agora vamos botar em prática e aplicar essas rotas iniciando com a criação do arquivo src/app.rounting.ts nele terá a configuração das rotas.

```js
// src/app.rounting.ts
import {Routes, RouterModule} from '@angular/router';

const appRoutes: Routes = [ ];

export const routing = RouterModule.forRoot(appRoutes);
```

e agora vamos adicioná-lo em nosso modulo principal do projeto, o src/app.module.ts.

```js
// src/app.module.ts
import {NgModule}      from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {routing}       from "./app.rounting"; // <<

import {AppComponent}  from './app.component';

@NgModule({
    imports: [
        BrowserModule,
        routing // <<
    ],
    declarations: [AppComponent],
    providers: [ ],
    bootstrap: [ AppComponent ]
})
export class AppModule { }
```

algo necessário para as rotas funcionar é utilizar a tag base do html5 no head de seu arquivo index.html

```html
<base href="/">
```

até agora apenas criamos nosso arquivo de configurações das rotas e o adicionamos ao módulo do projeto, antes de iniciar a criação das primeiras telas vamos adicionar a tag router-outlet do modulo router do angular dentro do componente src/app.component.ts

```js
// src/app.component.ts
import {Component} from '@angular/core';

@Component({
    selector: 'my-app',
    template: `<router-outlet></router-outlet>` // <<
})
export class AppComponent { }
```

a partir de agora podemos iniciar a criação da primeira tela do nosso projeto e configurar a nossa primeira rota, vamos criar nada mais nada menos que o componente HOME, para organizar melhor os componentes vamos usar a pasta screen para componentes referenciados em rotas, agora crie o arquivo src/screen/home/home.component.ts

```js
// src/screen/home/home.component.ts
import {Component} from "@angular/core";

@Component({
    selector: 'app-home',
    template: `<h1>Page Home</h1>`
})
export class HomeComponent { }
```

por que colocamos o componente home dentro de uma pasta chamada home? apenas porque caso eu queira separar e criar o arquivo home.component.html com o template de nosso componente ou um home.component.css e até nossos testes home.component.spec.ts que será legal deixar todo agrupado com nosso componente, pelo menos essa é a base para componentização.

e agora dando continuidade, vamos registrar nosso component no app.module.ts

```js
// app.module.ts
import {NgModule}      from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {routing}       from "./app.rounting";

import {AppComponent}  from './app.component';
import {HomeComponent} from "./screen/home/home.component"; // <<

@NgModule({
    imports: [
        BrowserModule,
        routing
    ],
    declarations: [
        AppComponent,
        HomeComponent // <<
    ],
    providers: [ ],
    bootstrap: [ AppComponent ]
})
export class AppModule { }
```

agora basta editar nosso arquivo app.rounting.ts e adicionar nossa rota home

```js
// app.rounting.ts
import {Routes, RouterModule} from '@angular/router';

import {HomeComponent} from "./screen/home/home.component";

const appRoutes: Routes = [
    {
        path: '',
        component: HomeComponent
    }
];

export const routing = RouterModule.forRoot(appRoutes);
```

agora basta editar nosso arquivo app.rounting.ts e adicionar nossa rota home

```js
// app.rounting.ts
import {Routes, RouterModule} from '@angular/router';

import {HomeComponent} from "./screen/home/home.component";

const appRoutes: Routes = [
    {
        path: '',
        component: HomeComponent
    }
];

export const routing = RouterModule.forRoot(appRoutes);
```

acho que aqui já podemos rodar e testar nossa primeira tela do projeto executando o comando:

```sh
npm start
```

espero que ajude o pessoal que está começando com o NG2 e qualquer dúvida sintam-se à vontade de comentar, até a próxima!
