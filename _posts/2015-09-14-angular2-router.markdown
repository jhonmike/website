---
published: true
title: Angular 2 Router
layout: post
tags: [angular2, ng2, typescript, webcomponent, ecmascript6, router]
categories: [Angular2]
permalink: angular2-router
---
![Angular 2 Router](/public/image/angular2.png)

Preparando o projeto, primeiro o NPM, crie o arquivo package.json

{% highlight ruby %}
touch package.json
{% endhighlight %}

./package.json

{% highlight json %}
{
	"name": "Angular2-Router",
	"version": "1.0.0",
	"description": "Router do angular 2",
	"homepage": "https://jhonmike.github.io",
	"devDependencies": {
		"connect": "^3.4.0",
		"del": "^1.2.0",
		"gulp": "^3.9.0",
		"gulp-typescript": "^2.8.0",
		"open": "0.0.5",
		"serve-static": "^1.10.0"
	},
	"dependencies": {
		"angular2": "2.0.0-alpha.36",
		"systemjs": "0.18.4"
	}
}
{% endhighlight %}

Instale o NPM usando o comando:

{% highlight ruby %}
npm install
{% endhighlight %}

Crie a pasta src e os arquivos index.html, main.html e main.ts dentro da mesma

{% highlight ruby %}
mkdir src
cd src
touch index.html main.html main.ts
{% endhighlight %}

./src/index.html
{% highlight html %}
<!doctype html>
<html lang="pt-br">
	<head>
		<base href="/">
		<meta charset="UTF-8">
		<title>Angular 2 Router</title>
	</head>
	<body>
		<main></main>
	</body>
	<script src="lib/traceur-runtime.js"></script>
	<script src="lib/system-csp-production.js"></script>
	<script>
		System.config({defaultJSExtensions: true});
	</script>
	<script src="lib/angular2.dev.js"></script>
	<script src="lib/router.dev.js"></script>
	<script>
		System.import('main').catch(console.log.bind(console));
	</script>
</html>
{% endhighlight %}

./src/main.html
{% highlight html %}
<nav>
	<a [router-link]="['/home']">Home</a>
	<a [router-link]="['/about']">About</a>
</nav>
<router-outlet></router-outlet>
{% endhighlight %}

./src/main.ts
{% highlight javascript %}
import {Component, View, bootstrap, CORE_DIRECTIVES} from 'angular2/angular2';
import {RouteConfig, ROUTER_DIRECTIVES, ROUTER_BINDINGS} from 'angular2/router';

import {Home} from './components/home/home';
import {About} from './components/about/about';

@Component({
	selector: 'main'
})
@RouteConfig([
	{ path: '/', redirectTo: '/home' },
	{ path: '/home', component: Home, as: 'home' },
	{ path: '/about', component: About, as: 'about' }
])
@View({
	templateUrl: 'main.html',
	directives: [CORE_DIRECTIVES, ROUTER_DIRECTIVES]
})
export class Main {
	constructor() {}
}

bootstrap(Main, [ROUTER_BINDINGS]);
{% endhighlight %}

Na pasta src, crie a pasta components e dentro da mesma a pasta about e home e os arquivos das mesma

{% highlight ruby %}
mkdir components
cd components
mkdir home
cd home
touch home.html home.ts
cd ..
mkdir about
cd about
touch about.html about.ts
{% endhighlight %}

./src/components/home/home.html
{% highlight html %}
<h2>Home</h2>
{% endhighlight %}

./src/components/home/home.ts
{% highlight javascript %}
import {Component, View} from 'angular2/angular2';

@Component({
	selector: 'home'
})
@View({
	templateUrl: './components/home/home.html'
})
export class Home {}
{% endhighlight %}

./src/components/about/about.html
{% highlight html %}
<h2>About</h2>
{% endhighlight %}

./src/components/about/about.ts
{% highlight javascript %}
import {Component, View} from 'angular2/angular2';

@Component({
	selector: 'about'
})
@View({
	templateUrl: './components/about/about.html'
})
export class About {}
{% endhighlight %}

para facilitar a compilação e execução do projeto vamos utilizar o gulp.js, crie o arquivo gulpfile.js na raiz do projeto

./gulpfile.js
{% highlight javascript %}
var gulp = require('gulp');

var PATHS = {
	src: {
		js: 'src/**/*.ts',
		html: 'src/**/*.html'
	},
	lib: [
		'node_modules/angular2/node_modules/traceur/bin/traceur-runtime.js',
		'node_modules/angular2/bundles/angular2.dev.js',
		'node_modules/angular2/bundles/router.dev.js',
		'node_modules/systemjs/dist/system-csp-production.js'
	],
	angular2: 'node_modules/angular2/bundles/typings/angular2/angular2.d.ts',
	router: 'node_modules/angular2/bundles/typings/angular2/router.d.ts'
};

gulp.task('libs', function () {
	return gulp.src(PATHS.lib).pipe(gulp.dest('dist/lib'));
});

gulp.task('html', function () {
	return gulp.src(PATHS.src.html).pipe(gulp.dest('dist'));
});

gulp.task('js', function () {
	var typescript = require('gulp-typescript');
	var tsResult = gulp.src([PATHS.src.js, PATHS.angular2, PATHS.router])
		.pipe(typescript({
			noImplicitAny: true,
			module: 'system',
			target: 'ES5',
			emitDecoratorMetadata: true,
			experimentalDecorators: true
		}));

	return tsResult.js.pipe(gulp.dest('dist'));
});

gulp.task('default', ['libs', 'html', 'js'], function () {
	var http = require('http');
	var connect = require('connect');
	var serveStatic = require('serve-static');
	var open = require('open');

	var port = 9000, app;

	gulp.watch(PATHS.src.html, ['html']);
	gulp.watch(PATHS.src.js, ['js']);

	app = connect().use(serveStatic(__dirname + '/dist'));
	http.createServer(app).listen(port, function () {
		open('http://localhost:' + port);
	});
});

gulp.task('clean', function (done) {
	var del = require('del');
	del(['dist'], done);
});
{% endhighlight %}