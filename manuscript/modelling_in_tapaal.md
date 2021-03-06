## Modelling in TAPAAL 
The main window of TAPAAL contains all the tools needed to draw Timed-Arc Petri Nets (TAPN).

![The main window in TAPAAL.](images/modelling/modelling/tapaal_modelling.png)

TAPAAL supports drawing/modelling of all the features explained in the section modelling features. It is possible to have multiple models open, and each model has its own drawing surface contained within a tab. Note that models in different tabs can not communicate or synchronize in any way. The left-hand pane contains the query list and the integer constant list. Integer constants will be covered later.

The query list allows you to create and save any number of queries for your model. For more information regarding the construction of queries please refer to the section verification section. 

### Modelling Tools
The modelling toolbar contains all the tools needed to draw TAPN models.

![The modelling toolbar of TAPAAL](images/modelling/modelling/modelling_toolbar.png)

The modelling tools are (from left to right in the toolbar): 

  * Select components
  * Add a place
  * Add a transition
  * Add a normal arc
  * Add a transport arc
  * Add an inhibitor arc
  * Add an annotation (like the textbox in the main window screenshot above)
  * Add a token
  * Delete a token

Tokens can also be added or deleted by placing the cursor over a place and scrolling up or down on the scrollwheel on your mouse.

### Editing Models
When you draw your models you often want to edit certain aspects, such as renaming a place or transition, adding invariants, changing an interval, etc.
This can be done by double clicking the thing you want to edit.

### Editing Places 
Double clicking on a place brings up the place dialog where you can edit the attributes of the place.

![The dialog for editing places.](images/modelling/modelling/place_dialog.png)

This dialog allows you to rename the place, add or remove tokens. Further, you can also specify invariants of the place. Note that in invariants it is possible to make use of integer constants which we will cover later in this section.

### Editing Transitions 
Double clicking a transition brings up the transition dialog where you can edit the attributes of the transition.

![The dialog for editing transitions.](images/modelling/modelling/transition_dialog.png)


This dialog allows you to rename the transition as well as specify the rotation of the transition. Note that you can also rotate a transition by placing the cursor over it and using the scrollwheel on your mouse.

### Editing Arcs 
Double clicking an input arc (of any type) brings up the arc dialog where you can edit the attributes of the arc.

![The dialog for editing arcs.](images/modelling/modelling/arc_dialog.png)

The main purpose of this dialog is to allow you to change the time interval on the arc. It is possible to use both strict and non-strict bounds on the interval. Note that as for place invariants, it is possible to use integer constants in these intervals.

### Integer Constants 
As mentioned in the beginning of this section the left-hand pane of the main window contains the integer constant list. In TAPAAL it is possible to define integer constants which can then be used in invariants and time intervals. This is useful if you are using the same constant over and over and want an easy way to change the value if needed.

To demonstrate the usefulness of these constants consider the model in the screenshot of the main window in the beginning of this section which contains 4 integer constants: `deadline`, `min`, `periodA` and `periodB`. This model is the webserver example which is one of the examples include in TAPAAL. To open it go to File -> Example nets -> webserver.

It models two users `A` and `B` who are continuously making requests to a webserver with a specified period and the webserver must then try to handle all the requests from the users. However, if a request is not handled before a specified deadline (in this example the deadline is the same for all requests), it will be dropped.

In this example, the `deadline` constant represents the deadline for the requests and `periodA` and `periodB` are the periods with which user `A` and `B` produce new requests. Notice how these constants are used in multiple places. If we want to see what happens if we increase the deadline for handling requests, we can easily do this by only changing the value of the deadline constant from e.g. 5 to 6. Thus, we only need to change it in one place instead of every place where the constant is used.

![The dialog for editing constants](images/modelling/modelling/constant_dialog.png)

Changing the value of a constant is done by double clicking it in the constant list which will bring up the constant dialog where you can change the name of the constant as well as the value.

Note that integer constants are only syntactic sugar, meaning that whenever you leave the modelling mode of TAPAAL (be it for simulation or verification), TAPAAL will substitute the values of constants in guards and invariants. 

