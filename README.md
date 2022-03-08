# Design-Patterns-In-JS

Design pattern describes a repeatedly presenting issue during software designing, as well the solution to it. Applying design pattern enables developers to reuse it to solve a specified design issue. Design patterns help designers communicate architectural knowledge, help people learn a new design paradigm, and help new developers avoid traps and pitfalls that have traditionally been learned only by costly experiences. In this paper, we briefly introduce the concept of software design patterns and give research on some design patterns, including Observer Pattern, Decorator Pattern, Factory Method Pattern, and Abstract Factory Pattern.

## 1. The Constructor Pattern

In classical object-oriented programming languages, a constructor is a special method used to initialize a newly created object once the memory has been allocated for it. With ES2015+ the syntax for creating classes with constructors was introduced to JavaScript. This allows the creation of objects as an instance of a class using the default constructor.

In JavaScript, almost everything is an object, and classes are syntactic sugar for JavaScript’s prototypal approach to inheritance. With classic JavaScript, we were most often interested in object constructors.

Object constructors are used to creating specific types of objects - both preparing the object for use and accepting arguments which a constructor can use to set the values of member properties and methods when the object is first created.

```js
function Human(name, age, occupation)
{ 
  this.name = name; 
  this.age = age;
  this.occupation = occupation;
  this.describe = function(){
    console.log(`${this.name} is a ${this.age} year old ${this.occupation}`);
  }
}
var person = new Human("Prakhar", "21", "Engineer");
person.describe();
```

## 2.  The Module Pattern

Modules are an integral piece of any robust application's architecture and typically help in keeping the units of code for a project both cleanly separated and organized.

In JavaScript, there are several options for implementing modules. These include:

- The Module pattern

- Object literal notation

- AMD modules

- CommonJS modules

- JavaScript modules

JavaScript modules are also known as “ES modules” or “ECMAScript modules” have already been introduced in an earlier section JavaScript Modules and ES2015+. We will primarily use ES modules for the examples in this section. Before ES2015, CommonJS modules or AMD modules were popular alternatives as they allowed you to export the contents of a module. We will be exploring AMD, CommonJS, and UMD modules later on in the book in the section Modular Design Patterns for Classic JavaScript. First, let us understand the Module Pattern and its origins.
The Module pattern is based in part on object literals and so it makes sense to refresh our knowledge of them first.

```js
function StudentDetails() {
  var name: "Prakhar";
  var age = 21;
  var branch= "CSE";
  
  return {
    name: name,
    age: age,
    branch: branch
  }
}

var newStudent = StudentDetails()

var userName = newStudent.name;
var userAge = newStudent.age;
var userbranch = newStudent.branch;
view raw
```

## 3. The Singleton Pattern

The Singleton pattern is a design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system. Classically, the Singleton pattern can be implemented by creating a class with a method that creates a new instance of the class if one doesn't exist. In the event of an instance already existing, it simply returns a reference to that object.

Singletons differ from static classes (or objects) as we can delay their initialization, generally because they require some information that may not be available during initialization time. They don't provide a way for code that is unaware of a previous reference to them to easily retrieve them. This is because it is neither the object nor "class" that's returned by a Singleton, it's a structure. Think of how closure variables aren't closures - the function scope that provides the closure is the closure.

ES2015+ allows us to implement the singleton pattern to create a global instance of a JavaScript class that is instantiated once. The singleton instance can be exposed through a module export. This makes access to it more explicit and controlled and differentiates it from other global variables. You cannot create a new instance of the class but can read/modify the instance using public get and set methods defined in the class.

```js
let instance = null;
class Printer {
  constructor(pages) {
    this.display= function(){
      console.log(`You are connected to the printer. You want to print ${pages} pages.`)
    }
  }

  static getInstance(numOfpages){
    if(!instance){
      instance = new Printer(numOfpages);
    }
    return instance;
  }
}

var obj1 = Printer.getInstance(2)
console.log(obj1)
obj1.display()
var obj2 = Printer.getInstance(3)
console.log(obj2)
obj2.display()
console.log(obj2 == obj1) 
```

## 4. The Observer Pattern

The Observer pattern is a design pattern that allows one object to be notified when another object changes, without requiring the object to know about its dependents. Often this is a pattern where an object (known as a subject) maintains a list of objects depending on it (observers), automatically notifying them of any changes to its state. In modern frameworks, the observer pattern is used to notify components of state changes.

When a subject needs to notify observers about something interesting happening, it broadcasts a notification to the observers (which can include specific data related to the topic of the notification). When we no longer wish for a particular observer to be notified of changes by the subject they are registered with, the subject can remove them from the list of observers.

It's useful to refer back to published definitions of design patterns that are language agnostic to get a broader sense of their usage and advantages over time. The definition of the Observer pattern provided in the GoF book, Design Patterns: Elements of Reusable Object-Oriented Software, is:

"One or more observers are interested in the state of a subject and register their interest with the subject by attaching themselves. When something changes in our subject that the observer may be interested in, a notification message is sent which calls the update method in each observer. When the observer is no longer interested in the subject's state, they can simply detach themselves."

We can now expand on what we've learned to implement the Observer pattern with the following components:

**Subject**
maintains a list of observers, facilitates adding or removing observers.

**Observer**
 provides an updated interface for objects that need to be notified of a Subject's changes of state.

**Concrete Subject**
broadcasts notifications to observers on changes of state, stores the state of Concrete Observers.

**Concrete Observer**
stores a reference to the Concrete Subject, implements an update interface for the Observer to ensure state is consistent with the Subject's
ES2015+ allows us to implement the observer pattern using JavaScript classes for observers and subjects with methods for notify and update.

```js
class Subject{
    constructor(){
        this.observerList = []
        this.newArticlePosted = false
        this.articleName = null
    }
    subscribe(observer){
        this.observerList.push(observer)
    }
 unsubscribe(observer){
        this.observerList = this.observerList.filter(obs => obs !== observer)
    }
    notify(){
        if(this.newArticlePosted){
            this.observerList.forEach(subscriber => subscriber.update())
        }
        else{
            return 
        }
    }
    getUpdate(){
        return this.articleName
    }
    postNewArticle(articleName){
        this.articleName = articleName
        this.newArticlePosted = true
        this.notify()
    }
}

class Observer{
    constructor(){
        this.subject = new Subject()
    }
    update(){
        if(subject.getUpdate() == null){
            console.log("No new article")
        }else{
            console.log(`The new article ${subject.getUpdate()} is posted`)
        }
    }
    setSubject(subject){
        this.subject = subject
    }
}
var subject = new Subject()
var observer = new Observer()
observer.setSubject(subject)
subject.subscribe(observer)
observer.update()
subject.postNewArticle("Dark matter") 

```

## 5. The Prototype Pattern

The GoF refers to the prototype pattern as one which creates objects based on a template of an existing object through cloning.

We can think of the prototype pattern as being based on prototypal inheritance where we create objects which act as prototypes for other objects. The prototype object itself is effectively used as a blueprint for each object the constructor creates. If the prototype of the constructor function used contains a property called name for example (as per the code sample lower down), then each object created by that same constructor will also have this same property.

Reviewing the definitions for this pattern in existing (non-JavaScript) literature, we may find references to classes once again. The reality is that prototypal inheritance avoids using classes altogether. There isn't a "definition" object nor a core object in theory. We're simply creating copies of existing functional objects.

One of the benefits of using the prototype pattern is that we're working with the prototypal strengths JavaScript has to offer natively rather than attempting to imitate features of other languages. With other design patterns, this isn't always the case.

Not only is the pattern an easy way to implement inheritance, but it can also come with a performance boost: when defining a function in an object, they're all created by reference (so all child objects point to the same function) instead of creating their individual copies.

With ES2015+ we can use classes and constructors to create objects. While this ensures that our code looks cleaner and follows OOAD principles, the classes and constructors get compiled down to functions and prototypes internally. This ensures that we are still working with the prototypal strengths of JavaScript and the accompanying performance boost.

```js
var car = {
    drive(){
        console.log("Started Driving")
        },
    brake(){
        console.log("Stopping the car")
    },
    numofWheels: 4  
} 

const car1 = Object.create(car);
car1.drive();
car1.brake();
console.log(car1.numofWheels);

const car2 = Object.create(car)
car2.drive();
car2.brake();
console.log(car2.numofWheels); 
```

## References

- [Learning JavaScript Design Patterns, Patterns.dev.](https://www.patterns.dev/posts/classic-design-patterns/#modulepatternjavascript)
- [The Constructor Pattern, Patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/#constructorpatternjavascript)
- [The Module Pattern, Patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/#modulepatternjavascript)
- [The Singleton Pattern, Patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/#singletonpatternjavascript)
- [The Observer Pattern, Patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/#observerpatternjavascript)
- [The Prototype Pattern, Patterns.dev](https://www.patterns.dev/posts/classic-design-patterns/#prototypepatternjavascript)
- [Prototype Pattern](https://www.educative.io/collection/page/5429798910296064/5725579815944192/5151110961561600)
- [Singleton Pattern](https://www.educative.io/collection/page/5429798910296064/5725579815944192/4890148815765504)
- [Constructor Pattern](https://www.educative.io/collection/page/5429798910296064/5725579815944192/5920633608208384)
- [Observer Pattern](https://www.educative.io/collection/page/5429798910296064/5725579815944192/6140123809841152)
- [Module Pattern](https://javascript.plainenglish.io/data-hiding-with-javascript-module-pattern-62b71520bddd)
