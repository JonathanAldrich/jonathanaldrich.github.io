---
title: Support Gold Open Access for OOPSLA and PACM PL!
tags: community
layout: default
comments: true
exclude_from_nav: true
---


A Day in Jar Hell, and How Better Language Design, Specification, and Architecture Could Help
============================================================

**Note to the reader:**
This started out as a quick set of thoughts on module and build system
design, driven by a recent experience, and ended up as a much
larger-scale vision connecting a bunch of research we've done in the
past with opportunities for future research at the intersection of
language design, specification, and architecture.  As is maybe common
to visions, there is a fair amount of speculative argument; one reason
why this is a private post and not yet ready to be a paper or a public
post.  Please help me improve it by telling me where my arguments are
weak, hard to understand, or could be improved by synergistic ideas.


I spent a good portion of today in Jar Hell.  I was working on a project
based on System X, whose name I will omit here--because despite my
criticisms, I admire the work and its authors.  System X is a PL tool
that is well-engineered in lots of ways, and I'm happy to be building
on it in general, but it has some build issues.  The latest pre-built
release runs on both Java 7 and Java 8, but doesn't handle input
written in Java 8 (remember this is a PL tool).  So, I decided to build
it myself and see if I could find a workaround.

This turned out to be a challenge.  The repository included two likely
branches, "develop" and "master," but neither one built the pre-built
release (or anything within 3 months worth of commits of the pre-built
release, actually).  The project dependencies were documented in the
build files, but without version information or pre-built jars, so I
had to experiment with different versions of the jars it depended on.
I was able to find the workaround I was looking for in both branches,
but neither branch was as suitable for my needs as the pre-built jar.
My eventual solution was to take the .class files that contained the
workaround and slot them into the pre-built jar, in place of existing
class files.  This didn't work for the "master" branch, but I got
lucky, and it did work for the "develop" branch!  Problem solved...but
hours lost.  Is there a better way?

There are a lot of partial solutions.  My life would have been a lot
easier if System X had a clear mapping between branches in the source
repository and pre-built releases, and if it was using a build system
like Maven or Gradle that specifies the version of the libraries that
System X depends on and downloads them automatically from an internet
repository.  But Maven and Gradle are not the full solution.  To see
why, let's come up with a list of requirements.  The first two are
provided by Maven and Gradle:

* Store the version of dependencies in build files

* Download the correct version of libraries during the build process

But the existence of these tools didn't help me much, since System X
didn't use them.  Furthermore, when I did get a pre-built System X jar,
I couldn't identify how to build it myself from the sources.  This
motivates additional criteria:

* Dependency management should be integrated with programming
languages and virtual machines, so that every project in the language,
or targeting the virtual machine, uses it consistently.  This property
strengthens the overall ecosystem, allowing developers within the
ecosystem to rely on good tooling across the ecosystem, not just in
their own project.

* Versioning information should be part of the language, so that if
you have source code, you have a way of representing what release of
the system it builds.  For System X, this would have assured that if
the pre-built jar was based on some version of the repository, I
would be able to identify it because it that version would be tagged
with the release number in the pre-built jar.

I don't know of a practical language that does this last thing, but
there are some languages and VMs that integrate dependency management,
for example Ruby Gems and Node.js.  Even if you have all of the above,
however, it's not enough.  One of the problems I faced was how to know
whether my patch was compatible with the interface of the library I
was patching.  Java doesn't have a well-defined interface structure at
the level of jars, but this is an important requirement:

* It should be possible to specify the signature of a library in the
programming language.  The type system should be able to enforce
signature conformance, and replacing a library with another one that
conforms to the same signature should be guaranteed not to produce
link errors.  Documented relationships between library versions
(e.g. backwards compatibility) should be enforced by the language.

Functional languages, most notably Standard ML, have been leaders in
terms of providing expressive signatures for library modules.  However,
in Standard ML, linking is done statically at the top level; modules
cannot be manipulated in first-class ways.  The ability to do this is
required by many important real-world frameworks that dynamically load
plugin modules when the user requests them:

* It should be possible to load and link library modules dynamically.
Modules should be first-class in order to support the flexibility that
is used in many modern Java libraries.  Dynamic linking should be
type-safe when linking to a statically-known signature.

One of the most sophisticated theories that supports ML-like module
signatures as well as dynamic linking is Scala.  Scala's solution is
not the only one--there is excellent module system research by Derek
Dreyer's group, for example, that also fits in this space--but Scala
has a pleasant advantage of providing a uniform type system that can
express object types as well as module signatures.  However, Scala's
type system is undecidable; while the undecidability may not come up
in practice much, it makes Scala's foundation a risky one to build an
ecosystem on:

* Module conformance to signatures, as well as signature subtyping,
should be decidable.

Fortunately, in work currently under review, my collaborators
Julian Mackay, Alex Potanin, Lindsay Groves, and I present
a system that preserves much of the expressiveness used
by Scala programs in practice, while restricting programs enough to
ensure that the type system is decidable.

It's not only important to manage dependencies between projects, and
the signatures of redistributable jars.  Large projects need to
manage internal dependencies as well.  Some tools provide this
(e.g. Microsoft uses a home-grown tool to manage dependencies within
Windows), but it can be done more consistently if provided by the
language's module and build system:

* Dependency management, mediated by signature conformance, should
be provided hierarchically, so that it can limit dependencies
between sub-modules within a project to a specified dependency
graph and to limited interfaces along each edge in that dependency
graph.

These requirements, perhaps most particularly the last one, begin to
raise the level of abstraction from that typical of build systems to
that of software architecture, in particular of module views:
specifying constraints on the interactions between modules.

I believe there are significant advantages to taking an integrated
approach to the above questions.  The issues mentioned above are
handled by a large set of tools today: dependency management build
tools, advanced module systems, object type systems, and sub-module
dependency checking tools.  Because these tools do not know about
each other, there are semantic gaps: dependency management tools do
not know about type systems, module systems do not enforce versioning
and are either static, inexpressive, or undecidable, and today's
sub-module dependency checking tools re-invent facilities that
should be provided as generalizations of the other approaches.
Combining them poses interesting challenges in language design:
really a combination of a module system design and an architectural
description language, something that has not been done well before.


Extensions to Component and Connector Views, and to Deployment Views
--------------------------------------------------------------------

Here I will speculate a bit more, but I believe there are also strong
benefits to extending the integrated approach above to Component and
Connector (C&C) Views, and to Deployment Views of Architecture.

We described how module systems should track dependencies on other
modules, but in many cases modules depend on a specific platform or
on hardware resources.  For example, some code in a web-based system
might be designed to run on a browser or mobile device; other,
computationally-intense code might be designed to run on a graphics
co-processor.  Wyvern modules can declare dependencies on other
modules, but they can also declare a depencency on a target platform
such as Java or Python.  We could generalize this to other platform
dependencies.  Modules are instantiated into running components, and
a deployment architecture shows the allocation of these run-time
components on to particular hardware platforms.  By linking the
module system, the module view of architecture, and the deployment
view of architecture, we can get an end-to-end guarantee that the
resources the code depends on being available are explicitly
declared and are definitely provided by the platform we are deploying
to.  A system that did not include each of these components would
not be able to provide such an end-to-end guarantee.  Thus:

* Modules should manifest their dependencies on particular platforms
(e.g. virtual machines such as the JVM or browser) as well as
platform-specific resources (e.g. a graphics co-processor).

* The language should distinguish between code modules, which are
stateless (cf. Szyperski's Component Software book), from run-time
component instances which may have state.

* The language should provide a declarative way to specify
deployments, allowing language tools to verify that platform
and resource dependencies are fulfilled.

A first step towards this vision is supported by the module system
developed by Darya Melicher, Alex Potanin, and I for Wyvern,
in which code modules are stateless but may be instantiated into
run-time components that are stateful.  Our design was motivated by
security considerations but (not entirely coincidentally!) also fits
well into the architecture vision described here.  The way that the
language distinguishes stateful objects and modules from stateless
ones is influenced by the immutability system concurrently developed
by Michael Coblenz, Joshua Sunshine, Brad Myers, and I (and vice
versa).  I've also been working with Mitchell Plamann, Darya, and
Alex on an approach to declaring platform dependencies for Wyvern's
modules, but we have not yet expanded this to consider other platform
resources, or considered deployment views that would support
checking that a particular deployment is sensible.

The architecture literature on Component and Connector (C&C) views
has emphasized the importance of ensuring that inter-component
communication is explicit.  This is the root of the communication
integrity property that was at the core of my own dissertation.
Dependency management solves a module-focused version of the
communication integrity property, but the run-time view is quite
important too.  One reason comes from distributed systems.  If a
component is stateful, then it must be placed on one machine or
another; if we try to place it on both, we get two copies of the
state which we must then keep synchronized with a connector, or
we must live with them diverging.  Connectors in distributed
systems are highly problematic in general: they require a great
deal of boilerplate code for constructing, sending, receiving,
and deconstructing messages, and they give up a language-supported
typechecking model (if we are lucky, a generally-weaker
tool-supported typechecking model may stand in).  This motivates
more requirements, with corresponding research challenges:

* Modules should explicitly specify the stateful components they
will depend on when they are instantiated (into a component
themselves).

* The language should provide a connector facility that can provide
type-safe communication in a distributed system, automating
boilerplate.

There are some non-architecture-based distributed communication
frameworks (Heather Miller's work is a good recent example) that
solve the second problem.  However, taking an architectural approach
in combination with a module system (like Wyvern's) that meets the
first property has significant potential advantages.  First, the
design of the system is explicitly documented in the architecture,
making it easier to understand a system's design, and thus hopefully
making it easier to evolve the system.  Second, an architectural
approach separates communication from computation more cleanly: a
component doesn't depend on a particular communication library, it
just declares an ordinary PL interface that is suitable for
distributed communication (e.g. it does not rely on shared data,
it is aware of any asynchrony and that errors or delays may occur).
This means that it is easier to change the connector technology
used, or to switch between a distributed and local connection.
Making it clear whether communication across an interface includes
shared mutable state also sheds light on how difficult it would be
to distribute communication across that interface; the less mutable
state the easier it will be.  My paper on Language Support for
Connector Abstractions takes a first step towards this approach, but
an approach that is better integrated with the other ideas in this
train could be much more beneficial.


Connections to other Plaid group research
-----------------------------------------

There are potential connections to other work in the group, as well:

* Module systems need good typing interfaces, but also good behavioral
specs.  While we may not implement these right away, there are
synergies with our research on typestate and pre- and post-condition
specifications that could be investigated.  Note especially that we
have been building linear type systems and logics; these dovetail
nicely with the above goals related to making stateful communication
explicit.

* As explored in my dissertation, as well as prior work that studied
shared variables as connectors and subsequent work with Marwan,
connections can involve shared data (or not); ownership is a natural
way of allowing, but limiting, sharing.

* Software architecture description languages can be viewed as a kind of
domain-specific language, and can benefit from Wyvern's support of the
same.

* Architecture description languages characterize endpoints of
communication using ports that have required and provided methods.
There is an interesting connection to trait systems, in which a trait
may provide a set of methods and require another set.  Trait
composition isn't much like architecture, but Du Li, Alex Potanin,
and I have been investigating delegation as an alternative to traits
or inheritance.  It's possible that an architecture-like delegation
mechanism could make the idea of ports in software architecture
concrete, as well as replacing inheritance approaches with something
more flexible and more loosely-coupled.


A Way Forward
-------------

The essay above is perhaps overly long, but maybe that is because it
attempts to tie together not only a set of ideas that 5 of my current
Ph.D. students (in addition to other collaborators) are working on,
but also makes some kind of connection to work by nearly all of my
past Ph.D. students.  Although these projects were planned somewhat
independently, I don't think it's entirely a coincidence that they are
connected.  I would argue that in some form, there is value to
combining these ideas, and that we can explore ways of getting even
more value from their union than we did from their parts.

We should certainly not attempt to combine ~15 dissertations worth of
work immediately, but I hope the above points out some opportunities,
as well as a longer-term direction that we can pursue going forward
in the group.  I think Wyvern is a good vehicle for many of those
explorations, and I hope we will use it where possible, adapting it
as necessary--though of course it's also true that some efforts in
the group can continue to be done independently when that best
facilitates each student's research goals.
