# Frequent itemset mining

## Definition
Given a number of transactions containing a number of items, extract the items that frequently appear together in such transactions a certain number of times.

# Introduction
## History: bar codes!
"Progress in bar-code technology has made it possible for retail organizations to collect and store massive amounts of sales data, referred to as the basket data. A record in such data typically consists of the transaction date and the items bought in the transaction. Successful organizations view such databases as important pieces of the marketing infrastructure. They are interested in instituting informationdriven marketing processes, managed by database technology, that enable marketers to develop and implement customized marketing programs and strategies."

- R. Agrawal, R. Srikant: Fast Algorithms for Mining Association Rules in Large Databases. VLDB 1994: 487-499.

Frequent itemset mining was motivated by basket data: data collected from purchases in supermarkets.

## Formal statement 
- I = {i1, i2, … , im} is a set of items, e.g., Beer or Diapers.
- D is a set of transactions where each transaction T is a set of items such that $T \subseteq I$; each T has associated a unique identifier called TID.
- X $\rightarrow$ Y is an association rule where X $\subset$ I, Y $\subset$ I and $X\cap Y=\emptyset$.
- X $\rightarrow$ Y holds in D with confidence c if at least c% of the transactions that contain X also contain Y.
- X $\rightarrow$ Y has support s if s% transactions in D contain X $\cup$ Y.

Given a set of transactions D, we wish to generate all association rules X Y that have support and confidence greater than the userspecified minimum support (called minsup) and minimum confidence (called minconf), respectively.
# The Apriori algorithm

## Notation
- k-itemset: an item set of size k such that its elements are sorted lexicographically.
- Lk: set of k-itemsets with minimum support, each of which contains the items and a count.
- Ck: set of candidate k-itemsets, each of which contains the items and a count.
## Algorithm
![[Pasted image 20260505160017.png]]
## Apriori-gen (join step)
![[Pasted image 20260505160123.png]]
To generate the candidates, we first perform a join step…
## Apriori-gen (prune step)
- forall itemsets  $c \in C_k$ do
	 forall (k-1)-subsets s of c do 
		 if  $(s \notin L_{k-1})$  then 
			 delete c from  $C_k$;

## Example
![](CSCI%20620/conv_lectures/5%20-%20Itemset%20mining/_page_11_Figure_0.jpeg)
Let's find the frequent itemsets using a minsup of 3.

k=1
- L1: ~~{Beer; 2}~~ {Bread; 6} {Butter; 5} {Diapers; 3} {Milk; 4}
k=2
- C2: L1 Join L1= {Bread, Butter; 0} {Bread, Diapers; 0} {Bread, Milk; 0} {Butter, Diapers; 0} {Butter, Milk; 0} {Diapers, Milk; 0}
- The prune step does not apply: all 1-itemsets are in L1
- Check transactions:
{Bread, Butter; 5} {Bread, Diapers; 3} {Bread, Milk; 4} ~~{Butter, Diapers; 2}~~ {Butter, Milk; 3} {Diapers, Milk; 3}
k=3
- C3: L2 Join L2= {Bread, Butter, Diapers; 0} {Bread, Butter, Milk; 0} {Bread, Diapers, Milk; 0}
- Prune step: {Bread, Butter, Diapers; 0} --> {Butter, Diapers} was not in L2.
- Check transactions: {Bread, Butter, Milk; 3} {Bread, Diapers, Milk; 3}
k=4
- C4: $\emptyset$ 
## Generate association rules
- Given I, a frequent itemset of size k, we will compute  $S \to i$, where S is a subset of I of size k-1, and  $i = I \setminus S$  is a single item.
- Metrics:
	- Support(S  $\rightarrow$  i) = count(I)/count(T)
	- Confidence(S  $\rightarrow$  i) = count(I) / count(S)
	- Lift(S  $\rightarrow$  i) = Confidence(S  $\rightarrow$  i) / (count(i)/count(T))
- Minimum confidence: minconf.
- S  $\rightarrow$  i is valid if Confidence(S  $\rightarrow$  i)  $\geq$ = minconf.
### Example
- Result: {Bread; 6} {Butter; 5} {Diapers; 3} {Milk; 4} {Bread, Butter; 5} {Bread, Diapers; 3} {Bread, Milk; 4} {Butter, Milk; 3} {Diapers, Milk; 3} {Bread, Butter, Milk; 3} {Bread, Diapers, Milk; 3}
- Let's focus on {Bread, Butter, Milk; 3}
- ~~{Bread, Butter} Milk: 3 / 5 = 60%~~
- ~~{Bread, Milk} Butter: 3 / 4 = 75%~~
- {Butter, Milk} Bread: 3 / 3 = 100%
> Let's find the association rules using a minconf of 80%.

# Optimizations

## Avoid checking transactions
- For each itemset, we will store the transactions where such itemset is included, i.e., intersection of the previous transactions
- In L1: {Bread; 6; 0, 1, 2, 3, 4, 5}
- In L2: {Bread, Butter; 5; 0, 1, 3, 4, 5}
- Apriori-gen is responsible for intersecting and counting (number of transactions greater or equal than the minimum support)
- No CK will be generated but will directly generate LK
> Our goal here is to avoid checking the transaction database when counting frequent itemsets.
## New join step
- We will keep all itemsets sorted by including an artificial count
- In L1: {Beer; 2; …; **1**} {Bread; 6; …; **2**} {Butter; 5; …; **3**} {Diapers; 3; …; **4**} {Milk; 4; …; **5**}
- Joining (L(K-1) join L(K-1)):
	For each $p \in L(K-1),$ ascending order
		For each $q \in L(K-1)$, ascending order, q.order > p.order
			Once we find the first combination it does not join:
				Break
- By doing this, we also preserve the order in LK
> We will be sorting items to improve performance.
## New join step: example
- p = \[1, 2, 3] and q = \[1, 2, 4]: can join
- p = \[2, 3, 4] and q = \[2, 4, 5]: cannot join and, since processed in order, no other q will join with p
## New prune step
- When computing L2, L1 does not need to be checked

## Lattice
![](CSCI%20620/conv_lectures/5%20-%20Itemset%20mining/_page_21_Figure_0.jpeg)

# Conclusions

## Market Basket Analysis
We have studied a technique to discover frequent itemsets that was originally devised to analyze supermarket transactions.

## Existing threats to privacy
- Secondary use of personal information: may correlate or disclose confidential, sensitive facts about individuals
	- Certain individuals always buy beer and diapers
- Handling misinformation: individuals should be able to challenge the correctness of data about themselves
	- James Russell Wiggings was fired based on info obtained from Equifax about Wiggings' conviction for drug possession; this was actually James Ray Wiggings
- Granulated access to personal information: on a need-toknow basis, and limited to relevant information only
	- Background check when hiring a worker but not health-related issues
> Taken from: Ljiljana Brankovic and Vladimir Estivill-castro. Privacy issues in knowledge discovery and data mining. In Proc. of Australian Institute of Computer Ethics Conference. 89-99, 1999.

## New threats to privacy
- Bias: patterns may be used for guessing confidential properties, and they may lead to stereotypes and prejudices
	- Different results based on race or ethnic group
- Combining patterns: may lead to a disclosure of individual information, either with certainty, or with a high probability
	- A study with 10 anonymous people (2 females and 8 males); there are 7 cases of disease A; none of the females has disease A; we know Mr. X was part of the study.
> Taken from: Ljiljana Brankovic and Vladimir Estivill-castro. Privacy issues in knowledge discovery and data mining. In Proc. of Australian Institute of Computer Ethics Conference. 89-99, 1999.

## Another example

T1 -> {A, B, C, D}
T2 -> {A, B, D, E}
T3 -> {A, C, D, E}
T4 -> \{A, B, C, E\}
T5 -> \{A, B, D\}
T6 -> {C, D, E}
T7 -> {A, B, C, E}
T8 -> {A, C, D}
T9 -> {A, B, C}
