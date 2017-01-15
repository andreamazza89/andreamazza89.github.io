
Two main groups:

  - package cohesion (which class goes in which package)
  - package coupling (how packages should relate to each other)

PACKAGE COHESION

  REUSE-RELEASE EQUIVALENCE PRINCIPLE
    _Either all of the classes in a package are reusable or none of them are_
    Packages should be support reuse, guaranteeing support and clear versioning.

  COMMON REUSE PRINCIPLE
    All of the classes in a package should be reused together.
    A good rule of thumb is that a diagram of class dependencies should give a
    single cluster (all classes connected somehow).
    This also prevents a scenario whereby the user of a package has to update to
    a new package even though the class that was updated in the package was not
    used.

  COMMON CLOSURE PRINCIPLE
    All classes in a package should be closed together against the same kinds of
    changes. A change that affects a package affects all classes in the package
    and no other packages.

    This is the single responsibility principle at a package level.

----------------------------------------------------------------------------

PACKAGE COUPLING

  ACYCLIC-DEPENDENCIES PRINCIPLE
    No package should be dependent on a package that depends on it. This is to
    avoid a situation whereby a package update creates integration conflicts.

  STABLE DEPENDENCIES PRINCIPLE
    Any package should only depend on packages that are more stable than it.
    A stable package is one that depends on few others (if any) but many depend
    on it. Think about it: if several packages depend on package X, the owners of
    X will have to think twice before changing it. Take the String class; imagine
    the repercussions of this suddenly changing API. String is a very stable
    package.

    High level design should exist in code within the most stable packages, although
    the Open-Closed principle should be adopted to enable some flexibility in the
    design.

  STABLE ABSTRACTION PRINCIPLE
    A package should be as abstract as it is stable.

    The more stable a package is the more adverse to change it is, so this should
    be more abstract than concrete.


Ends with a very scientific look at packages metrics, which is interesting but
probably only applicable in massive systems, with a lot of experience behind you.

In general, it is difficult to put these principles in practice for a relative
beginner, as you would not just sit down and plan the packages in your project;
instead, packages would emerge as the project grows and you would use these
principles to rearrange them in the best manner.
