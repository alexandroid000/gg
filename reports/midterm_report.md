---
authors: Alli Nilles (nilles2) and Spencer Gordon (slgordo2)
title: CS 476 Intermediate Report
geometry: margin=2cm
bibliography: refs.bib
csl: ieee.csl
header-includes:
    -   \usepackage{jeffe, graphicx, mathtools, xspace}
    -   \usepackage[charter]{mathdesign}
    -   \makeatother
    -   \DeclareMathOperator{\attach}{attach}
    -   \DeclareMathOperator{\relabel}{relabel}
    -   \DeclareMathOperator{\detach}{detach}
    -   \def\Card#1{\left| #1 \right|}
    -   \def\RuleSeq{\mathrm{RuleSeq}\xspace}
    -   \def\Graphs{\mathcal{G}\xspace}
    -   \def\Concepts{\mathcal{C}\xspace}
    -   \def\Hypotheses{\mathcal{H}\xspace}
---

\newcommand{\step}[1]{\xrightarrow{#1}}

In our project proposal, motivated by applications in self-assembling robotic
system, we asked "Might it be possible to learn compact rule sets that generate
stable assemblies, or dynamics on graphs that are recurrent?"

We then proposed a framework for learning functions from rule sets to
steady-state graph dynamics. In the course of the project, we have so far
focused on surveying known computational learning theory results for learning 
grammars, as well as surveying known results in the theory of graph grammars.

We show that a particular formulation of a concept class is inherently
unpredictable, under the definition from [@kearns1994]:

> A concept class $C$ is *inherently unpredictable* if the VC dimension of $C_n$
> is polynomial in $n$, yet $C$ is not efficiently PAC learnable using any
> polynomially evaluatable hypothesis class $H$.



Graph Grammars
--------------

We retain the same definition of graph grammars [@litovsky] [@klavins]:

We consider *simple labelled graphs*, $G = (V,E,l)$ where $V$
is an indexed set of vertices, $E$ a set of edges between pairs from $V$, and
$l: V \to E$ is a labelling or coloring function.

A **rule** is a pair of graphs $r=(L,R)$ (for *left* and *right*), where $V_L =
V_R$. A *unary* rule has a vertex set of size one, a *binary* rule is on two
vertices, etc. A rule can change labels and add/delete edges, but cannot add or
remove nodes. Rules also require (implicit or explicit) *embedding mechanisms*
which specify how these graphs should affect connections to a larger graph in which
they are embedded.

A **grammar** is a set of rules. These rules are applied nondeterministically to
a graph to define a transition system. Transitions occur when the left hand sides of 
a rule matches a subgraph and replaces it with the right hand side of the rule.

### Context-Free vs. Context-Sensitive Graph Grammars

If we relax the assumption that rules cannot add or remove vertices, we can form
a natural division of graph grammars is into context-free and
context-sensitive grammars. Context-free grammars take a single labelled node
and replace it with an arbitrary graph. These grammars are also often restricted
to be *confluent*, such that the result of a sequence of rule applications does
not change the outcome of the graph transformation. This area has been the focus
of the majority of attention in the graph grammars community, but
context-sensitive graph grammars have recieved relatively little.

The application to self-assembling robotic systems motivates the choice to focus
on context-sensitive graph grammars: often, we have a collection of $n$ robots
and wish to produce a set of locally executable rules that result in the
collection of robots forming a connected assembly with certain topological or
connectivity properties.

### A more specific scope

In particular, we are interested in *programmed context-sensitive graph
replacement systems*, as described in [@gg_handbook]. This area of research
introduces control-flow statements for specifying the order of application of
rules: for instance, we may specify that while rule 1 applies, apply it; then
apply rule 2 until rule 1 is applicable again. Such "programmable" graph
rewriting specifications can be used for decision procedures: in this paper, we
will examine the case where we apply rules in a given order for a number of
iterations that is proportional to the size of the overall graph. After this
number of rewrites, we examine the graph - if it is fully connected, we consider
this an "accepting" state, and otherwise we reject.

Is this concept class efficiently PAC learnable? If not, what constraints can we
add to the concept class to make it learnable? Likewise, if not, what
augmentations to the basic PAC learning model (such as membership and
equivalence queries) do we need to add to make the concept learnable? In this
fashion we are exploring the "frontier" of learnable concept classes and
learning models in this relatively unexplored formulation of graph grammars.

Preliminary Results
--------------

Let $\RuleSeq_\Sigma$ be a sequence of rules over a finite set of labels
$\Sigma$, where a rule is one of the following:
$$\operatorname{attach}(A,B),\quad, \operatorname{relabel}(A,B),\,\text{or}\quad \operatorname{detach}(A, B)$$
for some $A,B \in \Sigma$.

Let $\Graphs_{n,\Sigma}$ denote the set of all pairs $(G,\ell)$ where
$G$ is a graph on vertex set $[n]$ and $\ell : [n] \to \Sigma$ assigns a
label to each vertex. Let
$\Graphs_{\Sigma} = \cup_{n\geq 0} \Graphs_{n,\Sigma}$.

For each rule sequence $S\in \RuleSeq$, there is a corresponding step
relation $\step{S}$ such that $$G {\step{S}} G'$$ if and
only $G$ differs from $G'$ by a single application of the first rule in
$S$ that can be applied, to the lexicographically first vertex or
vertices to which it can be applied.

For any $S\in \RuleSeq$, we define the function
$f_S : \Graphs_{\Sigma} \to \Set{0,1}$ as follows:
$$f_S(G) = \begin{cases}
    1 &\,\text{if after $100 |V(G)|$ steps of ${\step{S}}$ starting with $G$ a steady state is reached in which the graph is connected}\\
    0 &\,\text{otherwise}
  \end{cases}$$

Our concept class will be
$\Concepts_\Sigma = \Setbar{f_S}{S\in \RuleSeq_\Sigma}$. Our hypothesis
class will be $\Hypotheses_{\Sigma} = \Concepts_\Sigma$.

We will be interested in the following question: Can we PAC-learn (or
efficiently PAC-learn) a hypothesis $h \in \Hypotheses$ given
equivalence queries and memebership queries?

Citations
---------

