---
published: true
title: Instalando NodeJs 4.0 no Ubuntu
layout: post
tags: [nodejs 4.0, iojs, linux, ubuntu, ecmascript6]
categories: [NodeJs]
permalink: instalando-nodejs-4
---
![Instalando NodeJS 4](/public/image/nodejs4.png)
Lançado hoje à tarde a versão mais recente do NodeJS, a versão 4.0, para os desinformados que estavam apenas acompanhando o node em sua versão 0.12 pesquisem sobre "iojs" que vão entender bem o que aconteceu e porque o NodeJS deu este salto de versão que foi, na minha opnião, uma das melhores conquistas da comunidade Node. Vamos iniciar a instalação.

* Baixe o NodeJS 4.0 do site oficial, [https://nodejs.org/](https://nodejs.org)
* Extraia o tar.gz que você baixo
* Recomendo agora que mova esta pasta para o diretorio usr do linux com o seguinte comando

{% highlight ruby %}
sudo mv /pasta/do/nodejs /usr/local/node
{% endhighlight %}

* Logo em seguida vamos adicionar os bin para que o node seja reconhecido em seu terminal

{% highlight ruby %}
sudo ln -s /usr/local/node/bin/node /usr/local/bin/node
sudo ln -s /usr/local/node/bin/npm /usr/local/bin/npm
{% endhighlight %}

* Teste

{% highlight ruby %}
node -v
sudo npm install npm -g
npm -v
{% endhighlight %}

[Dúvidas e comentários em issues do blog](https://github.com/jhonmike/jhonmike.github.io/issues)
