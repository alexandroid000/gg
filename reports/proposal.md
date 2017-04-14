---
geometry: margin=2cm
bibliography: refs.bib
csl: ieee.csl
---
\begin{centering}

\Large{\textbf{CS 446 Project Proposal} \\ Alli Nilles (nilles2), Spencer Gordon
(slgordo2)\\ \today}
\normalsize

\end{centering}

Definitions
-----------

In this project, we will be using the definition of **graph grammars**, also
known as *graph rewriting systems*, as introduced in [@litovsky] and applied to
robotics applications in [@klavins].

Specifically, we will consider *simple labelled graphs*, $G = (V,E,l)$ where $V$
is an indexed set of vertices, $E$ a set of edges between pairs from $V$, and
$l: V \to E$ is a labelling or coloring function.

A **rule** is a pair of graphs $r=(L,R)$ (for *left* and *right*), where $V_L =
V_R$. A *unary* rule has a vertex set of size one, a *binary* rule is on two
vertices, etc. A rule can change labels and add/delete edges.

A **grammar** is a set of rules. These rules are applied nondeterministically to
a graph to define a transition system. Vertices may represent robots, other
agents, or structures such as proteins.

Background
----------

Algorithms exist to synthesize rule sets that produce certain graphs: given a
graph $G$, they produce a grammar $\Phi$ such that the only stable assemblies of
$\Phi$ are isomorphic to $G$.

However, these grammars are not always as compact as hand-engineered grammars.
Additionally, we have no automatic way of generating grammars that lead to
interesting and useful dynamic patterns on graphs. For example, see Figure 
\ref{ratchet_rules}, which defines a "ratcheting" mechanism that moves a triangle down
a path, with only four rules.

![The rules and resulting trajectory for a "ratcheting" mechanism.
\label{ratchet_rules}](ratchet_rules.pdf){width=14cm}

Might it be possible to learn compact rule sets that generate stable assemblies,
or dynamics on graphs that are recurrent? We have been unable to find projects
applying machine learning to this specific problem, so this project will be a
first step in investigating approaches and perhaps coming to some conclusions
about what is learnable in this context.

Somewhat related are machine learning approaches for interactive proof
assistants. Automatic theorem provers use a set of premises in a theory, along
with an automatic deduction algorithm, to attempt to prove a theorem. But if the
provers are supplied with too many premises, their performance can degrade, and
often only a small set of premises is necessary to solve a problem.
Multiple efforts have been made to machine learn which premises to supply, using
techniques such as kernel methods and neural networks to much success [@premise11]
[@premiseNN]. The parallel with our project is this: we are attempting to learn
statements in a formal language (premises, or rules), which will be used to
automatically transform state (of a proof, or graph), until some desired final
condition is reached. In automatic theorem proving, the desired state is a proof
of the theorem, and in our case there are several possibly desireable behaviors
on graphs that we will target: creating stable assemblies, creating graphs with
certain connectivity properties, and possibly creating graphs with recurrent
dynamics under rule application (such as the ratchet example).

We would appreciate any suggestions for further reading, as we have so far been
unable to find more closely related projects in the literature.

Problem Statement
---------------

**Target Concepts:**

There are several types of target concepts that we are deciding between, and
researching the learnability of.

1.  learning to classify given rule sets based on whether or not they produce
    target dynamics. These target dynamics may include:
    -   creation of stable assemblies: there are parts of the graph that do not
        change after repeated application of rules (measured in simulation)
    -   creation of target graph connectivities: we may wish to learn local
        rules that minimize the average distance between any two nodes while
        being frugal with the number of edges. This can be measured with the
        cost function:
        $$ C(G) = \sum_{i \neq j} dist(i,j) + \alpha |E| $$
        where $i,j$ are nodes in the graph, $dist$ is the minimum length path
        between them, and $\alpha$ is a weight parameter. There are
        game-theoretic results on the optimal graph formation for different
        values of $\alpha$, to which we could compare our results [@egerstedt].
    -   creation of recurrent dynamics: we may wish the underlying graph to
        return to certain states (ideally periodically, though we will not
        enforce this at first). In simulation, we can measure whether the graph
        has this behavior under a given rule set.
2.  inferring underlying rules given intermediate states of a graph.
    Given a collection of graphs produced by the same rule set from the same
    initial condition, can we learn the underlying rule set? We may supply the
    number of rewrites that have been done on each graph to the learning
    algorithm. One challenge with this target concept is that since graph
    rewrites apply to subgraphs in a larger graph, and multiple rules may match
    at any given stage (and are applied nondeterministically), they have
    *interleaving semantics*.

**Training Set:**

One challenge with this project is that we lack a large corpora of examples in
graph rewriting systems: theorem provers have large libraries of already
machine-verified proofs to train on. To this end, we will create a
graph-rewriting simulator with a constrained set of options for rules and
initial conditions, and simulate the results of a large set of automatically
generated rules.

We will constrain the scope of this project in the following ways (subject to
change as the project progresses):

-   limit the size of each rule set to $\leq 5$ rules. This also helps
    facilitate the goal of learning *concise* rule sets.
-   Each node in a graph may have one of five labels, $\{a,b,c,d,e\}$.
-   We will only consider binary and ternary rules.
-   We will have a constrained set of possible initial conditions:
    -   disconnected graphs with homogenous labels
    -   disconnected graphs with randomly assigned labels
    -   fully-connected graphs with homogenous labels
    -   fully-connected graphs with randomly assigned labels

Even with these constraints, for each rule we have $\sum_{|V| = 1,2,3} (|V|*2^{\binom{|V|}{2}} 5^{|V|})^2 = 9,010,000$
different options as a combination of the number of vertices, edges, and vertex
labels. And we may have between one and five rules in each set, leading to a
truly impractical number of possible rule sets to learn over. Thus, we will have to
create a smaller training set using a combination of already known rule sets,
random sampling, and perhaps automatic generation (using heuristics) of rule
sets with known behaviors.

**Learning Algorithm:**

We are not sure of the best learning algorithm for this task, so we will most
likely try several, including random forests and neural networks, and compare
and analyze the results.

### References / Reading List
