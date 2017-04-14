Idea for PAC-learning
=====================

Let $\RuleSeq_\Sigma$ be a sequence of rules over a finite set of labels
$\Sigma$, where a rule is one of the following:
$$\operatorname{attach}(A,B),\quad, \operatorname{relabel}(A,B),\,\text{or}\quad \operatorname{detach}(A, B)$$
for some $A,B \in \Sigma$.

Let $\Graphs_{n,\Sigma}$ denote the set of all pairs $(G,\ell)$ where
$G$ is a graph on vertex set $[n]$ and $\ell : [n] \to \Sigma$ assigns a
label to each vertex. Let
$\Graphs_{\Sigma} = \cup_{n\geq 0} \Graphs_{n,\Sigma}$.

For each rule sequence $S\in \RuleSeq$, there is a corresponding step
relation ${\xrightarrow{S}}$ such that $$G {\xrightarrow{S}} G'$$ if and
only $G$ differs from $G'$ by a single application of the first rule in
$S$ that can be applied, to the lexicographically first vertex or
vertices to which it can be applied.

For any $S\in \RuleSeq$, we define the function
$f_S : \Graphs_{\Sigma} \to \Set{0,1}$ as follows:
$$f_S(G) = \begin{cases}
    1 &\,\text{if after $100\Card{V(G)}$ steps of ${\xrightarrow{S}}$ starting with $G$ a steady state is reached in which the graph is connected}\\
    0 &\,\text{otherwise}
  \end{cases}$$

Our concept class will be
$\Concepts_\Sigma = \Setbar{f_S}{S\in \RuleSeq_\Sigma}$. Our hypothesis
class will be $\Hypotheses_{\Sigma} = \Concepts_\Sigma$.

We will be interested in the following question: Can we PAC-learn (or
efficiently PAC-learn) a hypothesis $h \in \Hypotheses$ given
equivalence queries and memebership queries?
