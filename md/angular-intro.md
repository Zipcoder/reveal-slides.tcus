# Angular Introduction

-
-

## Front End Frameworks

Front-End frameworks allow us to better organize our application logic and work with larger applications in a better smarter way

Examples

* React
* Vue
* Angular

-
-

## Characteristics

* App Framework
* Two Versions
	* AngularJS (V1.0)
	* Angular (V2 and up)
* Component Based
* Modular

-
-

## Features

* Data Binding
* Templates
* CLI

-
-

## Angular CLI

* Command line interface
* Scaffold an application
* Highly Opinionated
* Requires (npm, node etc)

-

### Main CLI Commands

* ng create _{app name}_
* ng serve
* ng build
* ng g _{TYPE NAME}_

-
-

## NodeJS

* Node is a Javascript runtime (runs javascript)
* Developed by google
* Has a built-in server so we can run a server with node
* Heavily used by angular under the hood

-

### Node Angular

__Angular Development Server__

* Angular includes a development server that we will run during application development and will allow us to perform hot reload, get information about our angular application, etc.
* When we are finished with our angular application we can compile it and it will output a html, css and javascript project that we can then server as though we were never even using Angular

-
-

## Typescript

* A programming language that transpiles to Javascript, it is similar but adds a lot of missing features that Javascript doesn't have, most notably things that make the langueage more static and similar to established backed languages such as Java or C++

-
-

## Core Principles

### Modules

* An Angular applications is split up into modules, while the app will always have 1 root module, it will typically have many other feature modules which will contain feature-specific pieces of code
* Modules can import and export functionality from other modules
* We create modules using the angular CLI

-

### Components

* Angular Modules are split up into components, they will (usually) contain a TypeScirpt file where we can put our logic, an html template where we can put our html, and a styles file where we put our styles
* View encapsulation is a pretty complex topic and we won't go into it too deeply in the context of this course
