---
published: true
title: Iniciando com Angular 2
layout: post
tags: [angular2, ng2, typescript, webcomponent, ecmascript6]
categories: [Angular2]
permalink: iniciando-com-angular2
---
![Iniciando com Angular 2](/public/image/angular2.png)

Vamos começar com um rápido tutorial de Angular 2, de início uma breve observação, vamos utilizar o TypeScript da Microsoft, resumidamente TypeScript é uma linguagem para desenvolvimento em JavaScript, ela possibilita escrevermos código EcmaScript6 que não é totalmente compatível com os navegadores atuais, e por fim ela compila todo nosso código em JavaScript "Compatível" e como a sintaxe do Angular 2 fica muito mais elegante e tipada com EcmaScript 6 vamos utiliza-lo em nosso tutorial.

O TypeScript possue um gerenciador de pacotes próprio o TSD vamos instalar com o npm para termos acesso a algumas ferramentas e tambem vamos instalar seu compilador

{% highlight ruby %}
npm install tsd -g
npm install typescript -g
{% endhighlight %}

Com o TSD instalado acesse a pasta do seu futuro projeto, e baixe os seguintes repositórios

{% highlight ruby %}
tsd install angular2 es6-promise rx rx-lite
{% endhighlight %}

Para facilitar o desenvolvimento do projeto utilize o editor de código [vscode](https://www.visualstudio.com/en-us/products/code-vs.aspx), o vscode reconheçe a sintaxe do TypeScript ou instale alguns plugins em seu Sublime ou no Atom, vai do editor que você usa por default nos seus projetos.

Na pasta do projeto crie o arquivo main.ts e adicione uma espécie de comentário que ira instruir a compilação do TypeScript.

{% highlight javascript %}
/// <reference path="typings/angular2/angular2.d.ts" />
{% endhighlight %}

Agora podemos importar alguns dependências para nosso componente Angular 2

{% highlight javascript %}
import {Component, View, bootstrap} from 'angular2/angular2';
{% endhighlight %}

Agora algumas annotations que definira algumas instruções para nosso primeiro componente e a class do nosso componente.

{% highlight javascript %}
@Component({
    selector: 'meu-componente'
})
@View({
    template: '<h1>Meu Primeiro Componente<h1>'
})
class MeuComponente {}

bootstrap(MeuComponente);
{% endhighlight %}

Vamos para nosso index.html

{% highlight html %}
<!DOCTYPE html>
<html>
    <head>
        <title>Iniciando com Angular 2</title>
        <script src="https://github.jspm.io/jmcriffey/bower-traceur-runtime@0.0.91/traceur-runtime.js"></script>
        <script src="https://jspm.io/system@0.16.js"></script>
        <script src="https://code.angularjs.org/2.0.0-alpha.36/angular2.dev.js"></script>
    </head>
    <body>
        <meu-componente></meu-componente>
        <script>System.import('main');</script>
    </body>
</html>
{% endhighlight %}

Basta rodar o compilador agora pra criar seu arquivo JS, o comando a seguir criará um arquivo js compatível com os atuais navegadores.

{% highlight ruby %}
tsc -m commonjs -t es5 --emitDecoratorMetadata main.ts
{% endhighlight %}

Agora só visualizar o resultado

[Source no GITHUB](https://github.com/jhonmike/blog-iniciando-com-angular2)

[Dúvidas e comentários em issues do blog](https://github.com/jhonmike/jhonmike.github.io/issues)
