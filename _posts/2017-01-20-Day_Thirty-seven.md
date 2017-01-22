---
layout: post
title: 8th Light apprenticeship - Day Thirty-seven
categories: 8thLight apprenticeship
---

### Pills
- TableViews in JavaFX.

This week I have been acquainting myself with [JavaFX](http://docs.oracle.com/javase/8/javase-clienttechnologies.htm),
a framework for creating graphical user interfaces. As I mentioned in a previous
post, it is nice to see that JavaFX has a similar process in defining and updating
a view as does a web application: a tree-like structure of nodes containing other
nodes is given to the 'engine' for rendering, much like the DOM in a browser. All
these nodes are Java classes, which can display dynamic or static content, contain
and arrange children nodes, and get updated after user-triggered events.

One powerful class I used to display data is TableView, which was not very
straightforward to grasp at first, so I am writing about it to hopefully further
improve my understanding.

A TableView looks exactly as you would imagine it: a number of columns (one per
field) to be displayed and a number of rows (one per item) to be displayed. Each
column has a header to label what the column shows, so displaying a collection of
Cat objects would look like this:

#### Cats Table

|Name|Softness|Age|Highest jump(m)
|---|---|---|---|
|Fluffy|Clouds|22|1.4|
|Claw|SandPaper|5|2.1|
|...|...|...|...|

If you have not seen TableView before and you are trying to guess how to use it,
you might think of it as an HTML table, where you have to 'manually' (or with a
loop in a templating language) enter new rows and fill them with columns and data.

That is *not* how TableView works. Since our table is going to display a
collection of objects (one per row) and their properties (one per column), we simply
pass a list of objects to the TableView when we initialise it and it will create
one row per object. We then create as many TableColumn objects as we need, give
them a label for the header, and a method to retrieve the data from the object.

The following is what this hierarchy looks like:

                  ...parent node (container for the table) ...
                                      |
                                      |
                                      |
                                  TableView(given a list of objects to display)
                                  /   |   \
                                 /    |    \
                                /     |     \
                               /      |      \
                              /       |       \
                             /        |        \
                    TableColumn  TableColumn  TableColumn
                         |            |            |
                     TableCell    TableCell    TableCell
                         |            |            |
                     TableCell    TableCell    TableCell
                         |            |            |
                        ...          ...          ...

The list of objects given to TableView has to be an ObservableList, which is
simply made by wrapping your standard List into an observable one using `FXCollections.type_of_list`.
This allows the TableView to refresh data displayed in case anything is updated.
For example, if you were to initialise a TableView with an observable list of Cat
objects, the TableView would first render a table given the current state of the
cats, but if any should change in the future, the table will refresh itself.
This mechanism is referred to as data binding and can be 'turned off'.

Generating a TableView has the following steps:

  - Initialise the TableView node.
  - Set the tables items to an observable list of objects.
  - Create as many TableColumn objects as necessary and add them to the TableView.
  - Set the label on the columns.
  - Define how a column will generate a cell. This is a callback that will be
  invoked by the column and should return a TableCell object. This is optional as
  it defaults to the standard cell that simply displays text. Call
  `columnInstance.setCellFactory(callback goes here)` to set this.
  - Define how the column will get data out of the object to then feed the cell
  with. This is also a callback: `columnInstance.setCellValueFactory(callback goes here)`
