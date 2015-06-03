a history of software
* classical MVC
  * Models represent the domain-specific knowledge and data in an application. Think of this as being a ‘type’ of data you can model — like a User, Photo, or Todo note. Models can notify observers when their state changes.
  * Views typically constitute the user interface in an application (e.g., markup and templates), but don’t have to be. They observe Models, but don’t directly communicate with them.
  * Controllers handle input (e.g., clicks, user actions) and update Models.

User Input, User Interface(templates), Data**

* Thus, in an MVC application, user input is acted upon by Controllers which update Models. Views observe Models and update the user interface when changes occur.
* Historial GUIs
  * The first GUI’s in the 70’s used a pattern called Seperated Presentation
    * used as a means to make a clear division between domain objects which modeled concepts in the real world (e.g., a photo, a person) and the presentation objects which were rendered to the user’s screen.
  * The Smalltalk-80 implementation of MVC took this concept further and had an objective of separating out the application logic from the user interface. The idea was that decoupling these parts of the application would also allow the reuse of Models for other interfaces in the application.
    * A Domain element was known as a Model and was ignorant of the user-interface (Views and Controllers)
    * Presentation was taken care of by the View and the Controller, but there wasn’t just a single View and Controller. A View-Controller pair was required for each element being displayed on the screen.
    * The Controller’s role in this pair was handling user input (such as key-presses and click events) and doing something sensible with them.
    * The Observer pattern was used to update the View whenever the Model changed.
  * web apps start being written
    * Rasmus lerdorf
    * mysql/php
    * DHH makes rails flavored MVC

![Rails Flavored MVC] (http://addyosmani.github.io/backbone-fundamentals/img/rails_mvc.png)

      * Models represent the data in an application and are typically used to manage rules for interacting with a specific database table. You generally have one table corresponding to one model with much of your application’s business logic living within these models.
      * Views represent your user interface, often taking the form of HTML that will be sent down to the browser. They’re used to present application data to anything making requests from your application.
      * Controllers offer the glue between models and views. Their responsibility is to process requests from the browser, ask your models for data and then supply this data to views so that they may be presented to the browser.
  * Rails is actually Model2
    *[model2 vs mvc](http://stackoverflow.com/questions/796508/what-is-the-actual-difference-between-mvc-and-mvc-model2)
    * One reason for this is that Rails does not notify views from the model or controllers - it just passes model data directly to the view.
  * Front Controller
    * This pattern layers an MVC stack behind a single point of entry
    * When the Front Controller receives an HTTP request it analyzes it and decides which class (Controller) and method (Action) to invoke.
  * Classical Server side MVC
    ![Server Side MVC](http://addyosmani.github.io/backbone-fundamentals/img/webmvcflow_bacic.png)
  * initially with server side logic only, we rendered a full HTML page every time a user did something (clicked a link etc)
  * HTTP requests are too slow to make desktop speed apps in the browser, so we need to send less data, rendering the page is slow
    * cache all the things
    * make less requests
    * send less data
    * don’t rerender the page, only what has changed
      * jquery -> ajax
        * spaghetti code!
        * the DOM becomes a repository for state, how do you manage the complexity of dependent states in your UI
  * how do we make web apps more like desktop apps?
    * separate the client and the server
    * make the server just serve data, move the “app” to the client
    * MC on the server side AND MV* on the client side
  * tic tac toe
    * written in jquery
    * then a clean separation of concerns
  * backbone
    * models
    * computed properties
    * presentation related properties
      * active/selected
      * maybe these go in a “presenter” or a viewmodel
    * events/pubsub/observer pattern
      * decouples views from models
      * many views can listen to one model
        * example of an inbox
    * router/template swapper/state manager
      * compose a view
    * data binding
      * one way
      * two way
        * [angular](http://angular.github.io/angular-phonecat/step-4/app/)
        * [good use case] http://n12v.com/2-way-data-binding/>
        * With 2-way data binding, your UI can provide short feedback loops on inputs from users. Observing input on forms are common when building browser editors, or advanced control elements, like sliders that are coupled with numeric input.  

      * Backbone doesn’t have automatic data-binding but it is possible to do manually. In practice, I’ve found one-way data-binding which updates the view when changes are made to the model to be extremely useful. Data-binding from the view to the model real-world use cases are less common (for example, you want a live preview of how text will look as the user types text in an editor). It can be helpful to have binding from the view to the model but frequently the view change is initiated from user input and you want to do other logic before or in addition to actually changing the model. For example, validating input before changing or filtering a list in addition to remembering the current filter.

