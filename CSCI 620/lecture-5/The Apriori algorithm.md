
Algorithm used to count matched column items in a tabular dataset.

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

