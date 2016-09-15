---
layout: post
title:  "Iniciando com Angular2"
subtitle: "part01 - Estrutura do Projeto"
date:   2016-09-15 01:47:01
categories: [javascript]
---
![Iniciando com Angular2](/assets/img/iniciando-com-angular2.png)

Finalmente saiu a versão estavel do Angular2 a 2.0.0, boatos aparte talvez pra um futuro post, e vamos lá!

Quero demostrar neste post apenas como estou trabalhando com o Angular2, talvez não seja o melhor padrão ou o mais completo, mas pra mim esta fazendo sentido aos poucos estou melhorando, e sempre que possivel vou postar novidades sobre como esta o projeto!

Obs.: não estou usando o `angular-cli`, ele nao estava atualizando juntamente com as releases e na minha opnião estava trazendo muito lixo para o projeto, eu sei que tudo que tem lá é muito util, mas eu gosto mas de elaborar o padrao do zero, com o minimo de dependencias dentro do possivel e aos poucos ir crescendo o projeto e meu conhecimento dentro da ferramenta!

Sem mais delongas, começamos pelo `package.json`
{% highlight javascript %}
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
    "bootstrap": "^3.3.6",
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
{% endhighlight %}
alguns scripts para facilitar a vida, um servidor node, typescript o transpile que o angular2 utiliza por padrao, e suas libs, um bootstrap v3 ainda só pra dar uma cara pro projeto no futuro, bom podemos executar as instalações do NPM
{% highlight bash %}
npm install -g typescript
npm install //dentro da pasta do projeto com o arquivo package.json
{% endhighlight %}

`index.html`
{% highlight html %}
<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="X-UA-Compatible" content="IE=Edge">
        <meta charset="UTF-8">
        <title>Jhon Mike - Default Angular2</title>
        <link rel="icon" href="assets/img/favicon.png">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="node_modules/bootstrap/dist/css/bootstrap.min.css">
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
            Carregando.... (aqui seria legal fazer um loading pra mostrar antes de carregar sua aplicação!)
        </my-app>
    </body>
</html>
{% endhighlight %}
bom o `index.html` não tem segredo apenas vamos prestar atenção no System.import que o proximo arquivo o `systemjs.config.js` vai explicar melhor oque ele faz.

`systemjs.config.js`
{% highlight javascript %}
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
{% endhighlight %}
bom este arquivo é o nosso "autoload" vamos dizer que ele vai criar alguns alias aos modules npm facilitando o autoload dos mesmos na aplicação e tambem não tem muito segredo basta ler com atenção que vai conseguir entender, o resgistro dos modules utilizados!

`typings.json`
{% highlight javascript %}
{
    "globalDependencies": {
        "core-js": "registry:dt/core-js#0.0.0+20160602141332",
        "jasmine": "registry:dt/jasmine#2.2.0+20160621224255",
        "node": "registry:dt/node#6.0.0+20160807145350"
    }
}
{% endhighlight %}

e pra finalizar a parte de configurações vamos utilizar o `tsconfig.json` que vai trazer as configurações da copilação que o typescript vai executar

`tsconfig.json`
{% highlight javascript %}
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
{% endhighlight %}

Agora apartir daqui iniciariamos a criação do module angular e os componentes iniciais

crie a pasta `src` dentro do seu projeto se preferir usar o padrao use `app` mas não esqueça de voltar aos passos anteriores e substituir o nome das pastas

dentro da pasta `src` vamos criar algumas arquivos como o `main.ts` definido no arquivo `systemjs.config.js` como o main do app em questão, vamos criar tambem o arquivo `app.module.ts` e o arquivo do nosso primeiro componente ou container do ng2 o `app.component.ts`...

`src/main.ts`
{% highlight javascript %}
import {platformBrowserDynamic} from '@angular/platform-browser-dynamic';
import {enableProdMode} from '@angular/core';

import {AppModule} from './app.module';

const platform = platformBrowserDynamic();
platform.bootstrapModule(AppModule);
{% endhighlight %}

`src/app.module.ts`
{% highlight javascript %}
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
{% endhighlight %}

`src/app.component.ts`
{% highlight javascript %}
import {Component} from '@angular/core';

@Component({
    selector: 'my-app',
    template: `Hello World!`
})
export class AppComponent { }
{% endhighlight %}

obs.: Sua estrutura de pasta deve ser..
{% highlight %}
-node_modules
-src
    -app.component.ts
    -app.module.ts
    -main.ts
-typings
-index.html
-package.json
-systemjs.config.js
-tsconfig.json
-typings.json
{% endhighlight %}

bom esta feito, vamos iniciar nossa aplicação com o seguinte comando!

{% highlight bash %}
npm start
{% endhighlight %}
vai abrir a tela do navegador com nosso Hello World em NG2! bom acho que proximo post poderemos criar mais componentes e usar o angular-router3 a sim angular3 hahaha! deixe sua sujestão nos comentarios e até a proxima!
