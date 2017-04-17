---
author: Alli Nilles (`nilles2`) and Spencer Gordon (`slgordo2`)
title: CS 476 Intermediate Report
geometry: margin=2cm
bibliography: refs.bib
csl: ieee.csl
header-includes:
    -   \usepackage{jeffe, graphicx, mathtools, tikz, xspace}
    -   \usetikzlibrary{arrows.meta}
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

We show that a particular formulation of a concept class  is inherently
unpredictable, under the definition from [@kearns1994]:

> A concept class $C$ is *inherently unpredictable* if the VC dimension of $C_n$
> is polynomial in $n$, yet $C$ is not efficiently PAC learnable using any
> polynomially evaluatable hypothesis class $H$.

Specifically, we show that learning graph grammars which lead to to stable,
connected assemblies is inherently unpredictable by reduction to learning
DFAs. We also outline the next areas of investigation in this project.

Refinement of Project Scope
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
entire collection of robots forming a connected assembly with certain topological or
connectivity properties.

### Our concept class of interest

In particular, we are interested in *programmed context-sensitive graph
replacement systems*, as described in [@gg_handbook]. This area of research
introduces control-flow statements for specifying the order of application of
rules: for instance, we may specify that while rule 1 applies, apply it
repeatedly; then
apply rule 2 until rule 1 is applicable again. Such "programmable" graph
rewriting specifications can be used for decision procedures.

In this paper, we
will examine the case where we apply a given sequence of rules for a number of
iterations that is proportional to the size of the overall graph. After this
number of rewrites, we examine the graph - if it is connected, we consider
this an "accepting" state, and otherwise we reject.

Additionally, we will consider only *node-preserving* rewrites, in which the
number of nodes on the left and right sides of the rewrite are the same. Edges
may be added and removed in a rewrite, and labels may change.

Is this concept class efficiently PAC learnable? If not, what constraints can we
add to the concept class to make it learnable? Likewise, if not, what
augmentations to the basic PAC learning model (such as membership and
equivalence queries) do we need to add to make the concept learnable? In this
fashion we are exploring the "frontier" of learnable concept classes and
learning models in this relatively unexplored formulation of graph grammars.



Definitions
-----------


Let $\RuleSeq_\Sigma$ be a sequence of rules over a finite set of labels
$\Sigma$, where a rule is a node-preserving graph rewrite.

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
    1 &\,\text{if after $N |V(G)|$ steps of ${\step{S}}$ starting with $G$ a
    stable, connected graph is formed.}\\
    0 &\,\text{otherwise}
  \end{cases}$$

Our concept class will be
$\Concepts_\Sigma = \Setbar{f_S}{S\in \RuleSeq_\Sigma}$. Our hypothesis
class will be $\Hypotheses_{\Sigma} = \Concepts_\Sigma$.

**Definition [@kearns1994]:** a concept class $C$ over instance space $X$
PAC-reduces to the concept class $C'$ over instance space $X'$ if the following
conditions are met:

-   *Efficient Instance Transformation:* There exists a mapping $G: X_n \to
    X'_{p(n)}$ for all $n$ and some polynomial $p$ which is computable in
    polynomial time.
-   *Existence of Image Concept:* For all $c \in C_n$, there is a concept $c'
    \in C'_{p(n)}$ such that $size(c') \leq q(size(c))$ for some polynomials $p$
    and $q$. Additionally, for all $x \in X_n$, $c(x) = 1$ if and only if
    $c'(G(x)) = 1$.

Preliminary Work
----------------

**Theorem:** the class of deterministic finite automata PAC-reduces to the class of
node-conserving graph rewriting rulesets which
achieve full connectivity after $k |V(G)|$ steps on graph $G$.

**Proof:**

We describe for each DFA $D$ a polynomially sized ruleset that
simulates $D$ on transformed instances.

### Instance Transformation

We define a transformation $G: \{0,1\}^n
\to G_{p(n)}$ from the space of binary DFA inputs of length $n$ to the space of
graphs with a number of vertices polynomial in $n$.

Take an instance $x \in \{0,1\}^n$. For each character $s \in x$, create a pair of
vertices. Label one vertex $a$, and label the other with $b_0$ if $s=0$ and
$b_1$ if $s=1$. Add one more vertex to the graph and label it $\lambda$.

An example input transformation for the input $1011$ would be:

\begin{tikzpicture}
[vertex/.style={circle, draw, fill=black, inner sep=0pt, minimum width=4pt}]

\node[vertex] (1) [label=left:$\lambda$] {};
\node[vertex] (2) [label=left:$a$] at (0,-1) {};
\node[vertex] (3) [label=left:$a$] at (0,-2) {};
\node[vertex] (4) [label=left:$a$] at (0,-3) {};
\node[vertex] (5) [label=left:$a$] at (0,-4) {};

\node[vertex] (6) [label=right:$b_1$] at (1,-1) {};
\node[vertex] (7) [label=right:$b_0$] at (1,-2) {};
\node[vertex] (8) [label=right:$b_1$] at (1,-3) {};
\node[vertex] (9) [label=right:$b_1$] at (1,-4) {};

\end{tikzpicture}

### Image Concept

We create a rule sequence which exactly simulates the allowed transitions in the DFA.

First, assign to each state in the DFA a unique label, using the label $\lambda$
for the start state.

If the DFA transitions from the state with label $S$ to the state with label $T$
when it consumes character $s$, add the following rule to the rule sequence:

\begin{tikzpicture}
[vertex/.style={circle, draw, fill=black, inner sep=0pt, minimum width=4pt}]

\node[vertex] (s) [label=left:$S$] {};
\node[vertex] (t) [label=left:$a$] at (0,-1) {};
\node[vertex] (c) [label=right:$b_s$] at (1,-1) {};

\node[vertex] (s2) [label=left:$c$] at (4,0) {};
\node[vertex] (t2) [label=left:$T$] at (4,-1) {};
\node[vertex] (c2) [label=right:$c$] at (5,-1) {};

\draw[thick] (s2) -- (t2);
\draw[thick] (s2) -- (c2);
\draw[thick] (c2) -- (t2);

\draw[-{Straight Barb[left]}] (2,-0.5) -- (3,-0.5);

\end{tikzpicture}

where $c$ is a reserved label, not corresponding to any of the states of the DFA.

Additionally, add the following rule to the rule sequence:

\begin{tikzpicture}
[vertex/.style={circle, draw, fill=black, inner sep=0pt, minimum width=4pt}]

\node[vertex] (c) [label=below:$c$] {};
\node[vertex] (d) [label=below:$c$] at (1,0) {};

\node[vertex] (c2) [label=below:$c$] at (4,0) {};
\node[vertex] (d2) [label=below:$c$] at (5,0) {};

\draw[thick] (c2) -- (d2);

\draw[-{Straight Barb[left]}] (2,0) -- (3,0);

\end{tikzpicture}

Citations
---------

