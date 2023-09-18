### Probability 

##### Definitions:
- Sample space (S): Set containing all possible outcomes 
- Event space: Set containing all interested events 
- Probability (P): is a function that maps event (A) that belongs to S, to [0,1]
- Probability sample: consists of S and P
- Disjoint Events: A, B are disjoint events if $P(A\cap B) = 0$
- Independent Events: A, B are independent if $P(A\cap B) = P(A)P(B)$
- Conditional Probability: $P(A\mid B) = P(A\cap B)/P(B)$
- Bayes Rule: $ P(A\mid B) = P(A)P(B\mid A)/P(B)$
- Random Variable: $f:S\mapsto \mathbb{R}$
- Expected Value: $E(X) = \sum xP(x)$
- Linearity of Expected Value: $E(X+Y)=E(X)+E(Y)$, $E(cX)=cE(X)$
- Variance: $E[X^2]-E[X]^2$
- Stochastic Process: Random Variables evolving over time. 
- Markov Process: A system where past, future are conditionally independent given the present  

##### Axioms of Probability: 
- $P(\varnothing) = 0, P(S) = 1$, ($\varnothing$ is the empty set)
- Probabilities are additive for union of disjoint events 

##### Discrete vs Continuous: 
- Probability Mass Function (PMF) vs Probability Density Function (PDF) 
- PMF at any point can be non-zero, PDF at any point is zero (only non-zero on a range)
- Summation vs Integration 

##### Heuristics: 
- Probability Distributions are useful to model data with 
- Ex: Number of emails in 1 hour, here event rate (say per second) is small, but overall number is not 
- This is a good usecase to model the data with poisson's distribution

#### Resources: 
[Harvard: Statistics 101](https://www.youtube.com/playlist?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo)
