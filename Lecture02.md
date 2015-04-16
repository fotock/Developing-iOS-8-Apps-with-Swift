
#Today

###Continuation of Calculator Demo

Another thousand words (or so)?

###MVC

Object-Oriented Design Pattern


#Demo
###Calculator continued …
 
Array<T>
 
"Computed" properties (instance variables which are computed rather than stored)
 
switch
 
Functions as types
 
Closure syntax for defining functions "on the fly"
 
Methods with the same name but different argument types
 
More Autolayout


#MVC 
<<<<<<< HEAD
###Divide objects in your program into 3 “camps.”
=======
###Divide objects in your program into 3 "camps".
>>>>>>> pr/7
- Model
- View
- Controller


Model = What your application is (but not how it is displayed)

Controller = How your Model is presented to the user (UI logic)
 
View = Your Controller's minions

### It's all about managing communication between camps

<<<<<<< HEAD
### It’s all about managing communication between camps

=======
>>>>>>> pr/7
  Controllers can always talk directly to their Model.
  
  Controllers can also talk directly to their View.
  
  The Model and View should never speak to each other.
  
<<<<<<< HEAD
  Can the View speak to its Controller?   Sort of. Communication is “blind” and structured.
=======
  Can the View speak to its Controller?   Sort of. Communication is "blind" and structured.
>>>>>>> pr/7
  
  The Controller can drop a target on itself.
  
  Then hand out an action to the View.
  
  The View sends the action when things happen in the UI.
  
  Sometimes the View needs to synchronize with the Controller.
  
<<<<<<< HEAD
  The Controller sets itself as the View’s delegate.
  
  The delegate is set via a protocol (i.e. it’s “blind” to class).
=======
  The Controller sets itself as the View's delegate.
  
  The delegate is set via a protocol (i.e. it's "blind" to class).
>>>>>>> pr/7
  
  Views do not own the data they display.
  
  So, if needed, they have a protocol to acquire it.
  
  Controllers are almost always that data source (not Model!).
  
  Controllers interpret/format Model information for the View.
  
  Can the Model talk directly to the Controller?
  No. The Model is (should be) UI independent.
  
  So what if the Model has information to update or something?
  
<<<<<<< HEAD
  It uses a “radio station”-like broadcast mechanism.
  
  Controllers (or other Model) “tune in” to interesting stuff.
  
  A View might “tune in,” but probably not to a Model’s “station.”
=======
  It uses a "radio station"-like broadcast mechanism.
  
  Controllers (or other Model) "tune in" to interesting stuff.
  
  A View might "tune in", but probably not to a Model's "station".
>>>>>>> pr/7
  
  Now combine MVC groups to make complicated programs ...
