# Spring Web MVC Primer

-

##What we'll cover: 
<ul>
<li class="fragment fade-up"> Spring and its features</li>
<li class="fragment fade-up"> The Model View Controller Pattern</li>
<li class="fragment fade-up"> Spring Web MVC and its components</li>
</ul>

-

The Java ecosystem is filled with frameworks, such as **Jersey** and **RestEasy**, which allow you to develop **REST** applications. 

**Spring Web MVC** is one such popular web framework that simplifies Web and REST application development.

-
-

## Spring

The **Spring** Framework has become a de facto standard for building Java/Java EE–based enterprise applications. 

Originally written by Rod Johnson in 2002, the Spring Framework is one of the suite of projects owned and maintained by Pivotal Software Inc. (http://spring.io).

-

![Spring Modules](./img/spring_modules.png)

-
-

## Dependency Injection

-

At the heart of the Spring Framework lies **Dependency Injection (DI)**. 

As the name suggests, Dependency Injection allows dependencies to be injected into components that need them. This relieves those components from having to create or locate their dependencies, allowing them to be loosely coupled.

-
## Dependency Injection: an example

To better understand **DI**, consider the scenario of purchasing a product in an online retail store: 

Completing a purchase is typically implemented using a component such as an **OrderService**. 

The **OrderService** itself would interact with an **OrderRepository** that would create order details in a database and a **NotificationComponent** that would send out the order confirmation to the customer. 

-
-
## Dependency Injection: an example

In a traditional implementation, the **OrderService** creates (typically in its constructor) instances of OrderRepository and **NotificationComponent** and uses them. Even though there is nothing wrong with this approach, it can lead to hard-to-maintain, hard-to-test, and highly coupled code.

-

**DI**, by contrast, allows us to take a different approach when dealing with dependencies. 

With DI,
you let an external process such as Spring create dependencies, manage dependencies, and inject those dependencies into the objects that need them. 

So with DI, Spring would create the **OrderRepository** and **NotificationComponent** and then hand over those dependencies to the **OrderService**. 

-

**OrderService** is now relieved of having to deal with **OrderRepository**/**NotificationComponent** creation, making it easier to test. 

Each component can now evolve independently, making development and maintenance easier. 

Also, it's easier to swap these dependencies with different implementations, or use these components in a different context.

-
-

## Spring Web MVC Overview

-

**Spring Web MVC**, part of the Spring Framework’s Web module, is a popular technology for building Web-based applications. 

It is based on the model-view-controller architecture and provides a rich set of **annotations** and components. Over the years, the framework has evolved; it currently provides a rich set of configuration annotations and features such as flexible view resolution and powerful data binding.

-
-

## Model View Controller Pattern

-

The **Model View Controller**, or **MVC**, is an architectural pattern for building decoupled Web applications. This pattern decomposes the UI layer into the following three components:

* **Model**— Represents data or state. In a Web-based banking application, information representing accounts, transactions, and statements are examples of the model.
* **View**— Provides a visual representation of the model. It's what the user interacts with.
* **Controller**— Responsible for handling user actions such as button clicks. It then interacts with services or repositories to prepare the model, and hands the prepared model over to an appropriate view.

-

![MVC](./img/mvc.png)

-
-

## Spring Web MVC Architecture

-

Spring’s Web MVC implementation revolves around the **DispatcherServlet**—an implementation of the **FrontController Pattern** that acts as an entry point for handling requests.

-

![MVC](./img/spring_mvc.png)

-

+ **1.** The interaction begins with the **DispatcherServlet** receiving the request from the client.
+ **2.** **DispatcherServlet** queries one or more **HandlerMapping** to figure out a **Handler** that can service the request. A **Handler** is a generic way of addressing a **Controller** and other HTTP-based endpoints that Spring Web MVC supports.
+ **3.** The **HandlerMapping** component uses the request path to determine the right Handler and passes it to the **DispatcherServlet**. The **HandlerMapping** also determines a list of **Interceptors** that need to get executed before (Pre-) and after (Post-) **Handler** execution.

-

+ **4.** The **DispatcherServlet** then executes the **Pre-Process Interceptors** if any are appropriate and passes the control to the **Handler**.
+ **5.** The **Handler** interacts with any **Service**(s) needed and prepares the model.
+ **6.** The **Handler** also determines the name of the view that needs to get rendered in the output and sends it to **DispatcherServlet**. The **Post-Process Interceptors** then get executed.

-

+ **7.** This is followed by the **DispatcherServlet** passing the logical View name to a **ViewResolver**, which determines and passes the actual **View** implementation.
+ **8.** The **DispatcherServlet** then passes the control and model to the **View**, which generates response. This **ViewResolver** and **View** abstraction allows the **DispatcherServlet** to be decoupled from a particular **View** implementation.
+ **9.** The **DispatcherServlet** returns the generated response over to the client.


