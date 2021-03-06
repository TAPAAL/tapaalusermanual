# Verification 
In this section we will explain the verification part of TAPAAL.



## Query Dialog 

TAPAAL has capabilities to verify queries about your model. A query is created by pressing the `New Query` button in the query secion of the left-hand pane. This will open the query dialog which is displayed in the following screenshot.

![The query dialog](images/verification/tapaal_1.4_query_dialogue.png)

The query dialog contains a wealth of options. First of all you can specify a name of your query in the very top of the dialog. To perform the actual verification TAPAAL will translate your model to a Network of Timed Automata (NTA) and then use the UPPAAL verification engine on the produced NTA. In case you are interested in seeing the NTA produced by TAPAAL you can click the `Save UPPAAL XML` button which will export the NTA TAPAAL creates during the verification.

## k-Boundedness Checking 
![The boundedness checking part of the query dialog.](images/verification/boundedness_checker.png)

The query dialog allows you to specify the extra number of tokens that TAPAAL is allowed to use during the verification. Because TAPN models can produce additional tokens by firing transitions (e.g. a transition that has a single input place and two output places) the verifier may need to use additional tokens compared to those that are currently in the net. By specifying some extra number of tokens you can ask TAPAAL to check if your net is `k-`bounded for this number of tokens (i.e. k = number of tokens currently in model + number of additional tokens allowed). 

Simply put if your model is `k`-bounded then `k` is an upper bound on the number of tokens that your model can contain in any given marking. For instance, assume that your model currently contains 3 tokens and you ask TAPAAL if the model is bounded if you allow it to use 7 additional tokens. If the answer is yes then you know that in any given marking your model will not contain more than 10 tokens, thus the model is 10-bounded. In fact, it is not possible in this case to reach a marking where the model contains 11 or more tokens. On the other hand, if the answer is no the only thing you know is that it is possible to reach a marking with 11 or more tokens, but this tells you nothing about whether an upper bound actually exist.

If you get confirmation from TAPAAL that your model is indeed bounded for some number of tokens, then you can ask TAPAAL to find the least amount of additional tokens needed to make the net bounded. Generally, the more additional tokens you allow TAPAAL to use the longer the verification of your queries will take. Thus, you may have allowed TAPAAL to use 100 additional tokens but in reality only 10 additional tokens are needed. By letting TAPAAL find the least amount of additional tokens needed you may experience slightly faster verification time.

## Query Construction
![The query construction part of the query dialog](images/verification/tapaal_1.4_query_construction.png)

Constructing the query is done using the query construction buttons seen in the figure above. This is done by clicking on something in the text field that you want to change and then pressing one of the buttons below the text field to change the selection. Queries in TAPAAL are created by first specifying which type of query you want to verify. The options here are:

  * (EF) There exists some reachable marking that satisfies
  * (EG) There exists a trace on which every marking satisfies
  * (AF) On all traces there is eventually a marking that satisfies
  * (AG) All reachable markings satisfy

Following this, you specify the predicate to check. This is given as a boolean predicate consisting of some combination of conjunction, disjunction, negation and atomic propositions.  Atomic propositions are of the form 

<place_name> <operator> `n`

where <place_name> is the name of a place, <operator> is one of the operators <, <=, =, =>, > and `n` is an integer. Thus, in the proposition we can only talk about the number of tokens in a place and not their ages.

For instance, in the figure above a proposition `Garbage = 0` is specified which means that the place `Garbage` must contain zero tokens. 

Since, the query type is EG, the full query specified simply means: There exists a trace on which every marking satisfies that the `Garbage` place contains zero tokens. In other words, it is possible to avoid placing any tokens in the `Garbage` place.

## Constructing queries
The above discussion used a query that was already constructed. We will now look at how to construct such queries. If you create a new query in TAPAAL, the query construction part of the query dialog will look as in the following figure.

![The query construction part of the query dialog for a new query.](images/verification/tapaal_1.4_new_query.png)

Specifically, we will demonstrate how to construct the query `EG (P0 = 0 and P1 > 0)` (for this discussion we assume you have two places named `P0` and `P1` in your TAPN model). The expression `<*>` in the query text field is a placeholder which is the starting point for creating a query. Clicking on the placeholder selects it and enables the quantification buttons beneath the query text field. Selecting the EG quantifier puts the `EG` quantifier in front of the placeholder, so the query now reads `EG <*>`.

Next, select the placeholder by clicking on it and afterwards click the `And` button, the query now reads `EG (<*> and <*>)`. Select the first placeholder, construct the first atomic proposition `P0 = 0` using the comboboxes in the predicates section of the query dialog and subsequently press the `Add Predicate to Query` button. This replaces the currently selected placeholder with the chosen atomic proposition and the query now reads `EG (P0=0 and <*>)`. Select the placeholder and repeat the process for the atomic proposition `P1 > 0`. 

Note that you are not limited to replacing placeholders. For instance, if you wanted to say change the conjunction to a disjunction, you simply click on the `and` in the query text field to select the conjunction and then press the `Or` button. Similarly, if you want to add and additional conjunct to the conjunction, you simply click it (or one of the conjuncts) and press the `And` button, which adds a new conjunct to the query. Changing an atomic proposition is also as easy as clicking it and changing the propositions using the comboboxes in the predicates section.

Note that should you make a mistake the query construction has undo/redo support using the buttons in the `Editing` section. Likewise you can also delete the current selection (i.e. replace it with a placeholder) or reset the entire query (again, replace the entire query with a placeholder) in case you want to start over. Finally, expert users have the option of manually entering the query by clicking the `Edit Query` button, which makes the query text field editable. However, if you make a mistake when manually entering the query TAPAAL will inform you that it could not parse the query you entered. You will then have the option to edit the query again or revert back to whatever query was present before you pressed the `Edit Query` button.

## Verification Options 
The query dialog allows you to fine tune your verification by setting various verification options.

![The verification options of the query dialog.](images/verification/verification_options.png)

## Analysis Options 
The first options is the `Analysis Options` where you can specify the search strategy to use. The possible search strategies are:

  * Breadth First Search
  * Depth First Search
  * Random Depth First Search
  * Search by Closest to Target First

The chosen search strategy determine how the UPPAAL verification engine performs the search.

## Trace Options 
The `Trace Options` allow you to instruct TAPAAL to find a trace for your query. For instance, if you are trying to verify that it is always the case that there is at most 1 token in some specified place and you get a negative answer then you might want to see a trace that tells you how it is possible to put more than 1 token in the place. In the event that you do get a trace, TAPAAL will open the trace in simulation mode allowing you to step through it.
Note that a current limitation of UPPAAL is that it is not always possible to obtain a trace.



## Reduction Options 
Finally there is the `Reduction Options`, which allows you to choose which reduction method to use and whether or not to use symmetry reduction. The possible reduction methods are:

  * Standard
  * Optimised Standard
  * Broadcast Reduction
  * Broadcast Degree 2 Reduction

The difference between these reductions are rather technical and thus left out here. The interested reader is referred to the list of published paper involving TAPAAL which can be found [Here](http://tapaal.net/features/). Note, that the Standard and Optimised Standard reduction methods does not in general support queries of the type EG and AF.

The use of symmetry reduction can greatly reduce the verification time of your models. Note however that currently it is not possible to obtain a trace if symmetry reduction is used.
