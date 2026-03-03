A **statistic** is a function of data, and **statistics** is the study of the collection and analysis of data.

In statistics, we focus on experiments with nondeterministic outputs. In this case, we denote $S$ as the outcome (or sample) space, containing every possible outcome. An **event** is a collection (subset) of outcomes in $S$. If the outcome of the experiment is in an event, we say that event has occurred.

**Probability** is a real-valued function $P(X)$ that assigns a decimal in $[0, 1)$ to each event such that the sum of the probabilities of all events equals one and probability acts as a linear transform over disjoint events.

We can derive from the above that $P(A)=1-P(A')$as the two are disjoint but sum to the sample space, and that $P(\emptyset)=0$. Since all subsets $A$ of another set $C$ have some other set $B$ such that $A+B=C$, we have $P(A)\leq P(C)$.

The **Inclusion-Exclusion Principle** states that for any two events, the probability of either of them happening is the sum of the probabilities of the individual events minus the probability of both of them happening. This is generalizable to any number of events, but is written as follows in the two-event case: $$P(A\cup B)=P(A)+P(B)-P(A\cap B)$$