
Algorithm used to count matched column items in a tabular dataset.


## Vocab

Minimum Support
- This defines how often a group of item must appear together in the db to be considered frequent.
Minimum Confidence
- This measures the reliability of a rule. If we make a rule like "if a customer buys x, they will also buy Y," the confidence is the percentage of times this rule turns out to be true.

## Components of the algorithms

1. Candidate Generation(Join step)
2. The Prune Step(Core intuition)
3. Counting and filtering
4. Generating Association rules
### Candidate Generation

TO find larger groups of frquent items, the algorithm takes the frequent itemsets it has already found and joins them together to form larger "candidates". 

For example, if it knows that pairs of items like {Bread, Butter} and {Bread, Milk} are frequent, it will combine them to cerate a size-3 candidate: {Bread, Butter, Milk}.

### The Prune Step(Core Intuition)
Clever shortcut that makes the algo efficient. The intuition: if a small group of items is not frequent, then any larger group containing it cannot be frequent either. Acting as a filtering of any useless data points that will definitely not going to show up in the next final result.

Before checking the dataset, the algorithm looks at the candidates. If any smaller subset of a candidate was previously found to be infrequent, the algo immediately deletes that candidate.

### Counting and Filtering
After the pruning(filtering), the algo will check the actual transaction data to count exactly how many times those combination appear. If a candidate's count meets the minimum support threshold, it becomes an official "frequent itemset". 

The algo repeats the join, prune, and count steps until it can't find any more frequent itemsets

### Generating Association Rules
Once the frequent itemsets are found, the final step is to generate the actual "if X, then Y" rules. 
It takes a frequent group, like {Bread, Butter, Milk}, and breaks it into possible rules, like "if you buy {Bread, Milk}, you will also buy {Butter}". It will calculate the confidence for each rule. 

If the rule meets the minimum confidence threshold, it is considered a valid and useful association rule.

See [[Example of Apriori Algo]] for step by step example of this algo logic


## Apriori-gen
This is a little optimization trick to prevent db query from going back to the original list of transactions every time it needs to count transactions. 

This is basically a map of item to the transactions that it was found in as well as the number of transactions that it was found in. 

For example: if the frequent itemset {Bread} appears in transactions 0,1,2,3,4,5, it is stored as {item: Bread, count: 6, transaction: \[0,1,2,3,4,5\]}

### Constants
- $I = {i_{1},i_{2},\dots,i_{m}}$
- $D$ is a set of transactions where each transaction T is a set of items such that $I\subseteq I$; each T has associated a unique identifier called TID.
- X --> Y is an association rule where $X\in I,\;Y\in I$ and $X\cap Y=\emptyset$
- X --> Y holds in D with confidence c if at least c% of the transactions that contain X also contain Y.
- X --> Y has support s if s% transactions in D contain $X\cup Y$.


## Algo

### Vocab
- k-itemset: an item set of size k such that its elements are sorted lexicographically
- LK: set of k-itemsets with minimum support, each of which contains the items and a count.
- Ck: set of candidate k-itemsets, each of which contains the items and a count.

%% Main %%
$L_1$ = {$large\;1-itemsets$};
for ( k = 2; $L_{k-1}$ ≠ Ø; k++ ) do begin
	$C_k$ = `apriori-gen`($L_{k-1}$);
	for all transactions $t\in D$ do begin
		$C_t$=subset($C_{k}$, t);
		for all candidates $c\in C_{t}$ do
			c.count++;
	end
	$L_{k}=\{ c\in C_{k}\;|\;\\c.count≥minsup \}$
end
$Answer = \bigcup_{k}L_{k}$;

%% Apriori-gen %%
insert into $C_{k}$
select $p.item_1,\;p.item_{2},\;\dots,\;p.item_{k-1},\;q.item_{k-1}$
from $L_{k-1}p,\;L_{k-1}\;q$
where $p.item_{1}=q.item_{1},\;\dots,\;p.item_{k-2} = q.item_{k-2}, \;p.item_{k-1}<q.item_{k-1}$

%% Apriori-gen(prune step) %%
forall itemsets $c\in C_{k}$ do
	forall (k-1)-subsets s of c do
		if ($s\notin L_{k-1}$) then
			delete c from $C_k$;

