Handbook of Graph Grammars and Computing by Graph Transformation - Rozenberg - 1997
===================================================================================

volume 1. very helpful overview

Concept Formation Using Graph Grammars - Jonyer Holder Cook - 2002
==================================================================

Abstract
--------

Recognizing the expressive power of graph representation and the ability of
certain graph grammars to generalize, we attempt to use graph grammar
learning for concept formation. In this paper we describe our initial progress
toward that goal, and focus on how certain graph grammars can be learned from
examples. We also establish grounds for using graph grammars in machine
learning tasks. Several examples are presented to highlight the validity of
the approach.

Notes
-----

Context-free, infers productions that can create graph from single node

MDL-Based Context-Free Graph Grammar Induction and Applications - Jonyer Holder Cook - 2003
===========================================================================================

Abstract: We present an algorithm for the inference of context-free graph gramma
rs from examples. The algorithm builds on an earlier system for frequent
substructure discovery, and is biased toward grammars that minimize description
length. Grammar features include recursion, variables and relationships. We
present an illustrative example, demonstrate the algorithm's ability to learn in
the presence of noise, and show real-world examples.


Learning Node Replacement Graph Grammars in Metabolic Pathways - Kukluk You Holder Cook - 2007
==============================================================================================

This paper describes graph-based relational, unsupervised learning algorithm to
infer node replacement graph grammar and its application to metabolic pathways.
We search for frequent subgraphs and then check for overlap among the instances
of the subgraphs in the input graph. If subgraphs overlap by one node, we
propose a node replacement graph grammar production. We also can infer a
hierarchy of productions by compressing portions of a graph described by a
production and th en inferring new productions on the compressed graph. We show
learning curves and how the learning process changes when we increase the size
of a sample set. We examine how computation time changes with an increased
number of nodes in the input graphs. We inferred graph grammars from metabolic
pathways which do not change more with increased number of graphs in the input
set. It indicates that graph grammars found represent the input sets well.

Inferring the Structure of Graph Grammars from Data - Doshi Huang Oats - 
=========================================================================

Abstract
--------

Graphs can be used to represent such diverse entities as chemical compounds,
transportation networks, and the world wide web. Stochastic graph grammars are
compact representations of probability distributions over graphs. We present an
algorithm for inferring stochastic graph grammars from data. That is, given a
set of graphs that, for example, correspond to a set of chemical compounds, all
of which have some desirable property, the algorithm uncovers the structure
shared by the graphs and represents it in the form of a stochastic graph
grammar. The inferred grammar assigns high probability to the graphs from which
it was learned and low probability to other graphs. We report results of
preliminary experiments in which inferred graph grammars are compared to target
grammars used to generated training data.

Notes
----

Seems to be a student research paper.

Thermodynamic Graph-Rewriting - Danos Harmer Honorato-Zimmer - 2015
===================================================================

Abstract
--------

We develop a new thermodynamic approac to stochastic graph-rewriting. The
ingredients are a finite set of reversible graph-rewriting rules G (called
generating rules), a finite set of connected graphs P (called energy patterns),
and an energy cost function $\epsilon$ which associates real values to each of
these energy patterns. The idea is that $G$ defines the qualitative dynamics, by
showing which transformations are possible, while $P$ and $\epsilon$ allow one
to attach an energy to the reachable graphs and, thereby, describe their
long-term probability distribution $\pi$. Given $G$ and $P$, we construct a
finite set of rules $G_P$ which (i) has the same qualitative transition system
as $G$; and (ii) when equipped with rates according to $\epsilon$, defines a
continuous-time Markov chain of which $\pi$ is the unique fixed point. The
construction relies on the use of site graphs and a technique of 'growth policy'
for quantitative rule refinement which is of independent interest.

This division of labour between the qualitative and long-term quantitative
aspects of the dynamics leads to intuitive and concise descriptions for
realistic models. It also guarantees thermodynamical consistency (aka detailed
balance), otherwise known to be undecidable, which is important for some
applications. Finally, it leads to parsimonious parameterizations of models,
again an important point in some applications.

Learning to Transform Linguistic Graphs - Jijkoun Rijke - 2007
==============================================================

We argue in favor of the use of labeled directed graph to represent various
types of linguistic structures, and illustrate how this allows one to view NLP
tasks as graph transformations. We present a general method for learning such
transformations from an annotated corpus and describe experiments with two
applications of the method: identification of non-local dependencies (using Penn
Treebank data) and semantic role labeling (using Proposition Bank data). 
