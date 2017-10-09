AngularJS notes from Xing
=========================

## Intro

Angular CLI (command line interface); TypeScript.

需要Nodejs和npm, 用ng new porj-name来创建新proj, ng serve -o automatically open your browser on http://localhost:4200/.

最开始的component叫app.root, app/assets放图片等各种东西，app/environments放每一个destination environment的文件，favicon.ico是出现在浏览器tab上的图片。。。更多的上tutorial看

## Basic

'npm start' runs the compiler in watch mode, Component里添加property：

	export class AppComponent {
		title = 'Tour of Heroes';
  		hero = 'Windstorm';
	}

@Component decorator里data binding：

	template: `<h1>{{title}}</h1><h2>{{hero}} details!</h2>`

声明class的时候，用：

	export class Hero {
  		id: number;
  		name: string;
	}

对class初始化用

	hero: Hero = {
  		id: 1,
  		name: 'Windstorm'
	};

数据双向绑定（textbox和property）：

	<div>
  		<label>name: </label>
  		<input [(ngModel)]="hero.name" placeholder="name">
	</div>

在app.module.ts里import FormsModule from @angular/forms，然后在@NgModule metadata里的import array里加上FormsModule

## Master/Detail

ul tag是unordered list，配合li tag一起用

用这个格式将所有内容放到一个unordered list里面，

	<li *ngFor="let hero of heroes">
		<span class="badge">{{hero.id}}</span> {{hero.name}}
	</li>


怎么让你显示的list和特殊的那个产生联系，用click：

	<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
		...
	</li>

	onSelect(hero: Hero): void {
  		this.selectedHero = hero;
	}

	让你的selectedHero在没有值得时候不产生错误

	<div *ngIf="selectedHero">

## Multiple Components

当多个component要用同一个class的时候，单独为这个class创建一个ts文件

在AppComponent中将selectedHero绑定HeroDetailComponent中的hero：

	<hero-detail [hero]="selectedHero"></hero-detail>

同时在HeroDetailComponent需要使用：

	import { Component, Input } from '@angular/core';
	@Input() hero: Hero;

另外，所有新的component需要在AppModule中的NgMododule中声明

## Service

	import { Injectable } from '@angular/core';

	@Injectable()
	export class HeroService {
	}

Service里export class的函数：

	getHeroes(): Hero[] {
    	return HEROES;
	}

在component import service：

	import { HeroService } from './hero.service';

在provider中加service，在Component的export class里面加service：

	constructor(private heroService: HeroService) { }

The ngOnInit lifecycle hook

A Promise essentially promises to call back when the results are ready. You ask an asynchronous service to do some work and give it a callback function. The service does that work and eventually calls the function with the results or an error.

	getHeroes(): Promise<Hero[]> {
  		return Promise.resolve(HEROES);
	}

	getHeroes(): void {
  		this.heroService.getHeroes().then(heroes => this.heroes = heroes);
	}

## Routing

在app.module里加：

	import { RouterModule }   from '@angular/router';

import:

	RouterModule.forRoot([
  		{
    		path: 'heroes',
    		component: HeroesComponent
  		}
	])

component：

	<a routerLink="/heroes">Heroes</a>
	<router-outlet></router-outlet>

Redirect：

	{
  		path: '',
  		redirectTo: '/dashboard',
  		pathMatch: 'full'
	},

### Navigating to hero details



















