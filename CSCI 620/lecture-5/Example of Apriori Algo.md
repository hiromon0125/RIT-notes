
# Step 0: Starting Point/Givens
- Input: 
	- List of transactions/orders(simplified to D), a user-specified minimum support threshold (`minsup`), and a user-specified minimum confidence threshold(`minconf`).
	- List of transations($T$):
		- T1: {Bread, Butter, Milk Pasta}
		- T2: {Bread, Butter, Milk}
		- T3: {Bread, Diaper, Pasta}
		- T4: {Butter, Milk, Diaper}
		- T5: {Bread, Butter, Milk, Diaper}
	- For the example we will use a `minsup` of 3, meaning an item or group of items must appear in at least 3 transactions to be considered frequent.
	- For later we will also need `minconf` which will be 75% for the purpose of this example.

# Step 1: Finding Frequent 1-itemsets(k=1)
- Input:
	- Transactions: $T$
	- minsup: 3
- Computation work:
	- The algo will count the occurrences of each individual item across all transations and filters out any that fall below our minsup of 3.
		- Bread: 4 instances
		- Butter: 4 instances
		- Milk: 4 instances
		- Diaper: 3 instances
		- Pasta: **2 instances**
	- Since Pasta is 2 which is below the minsup, it fails to meet minsup thus will be filtered out
- Output:
	- Generates the set $L_1$ (frequent 1-itemsets): 
		- {Bread: 4}, 
		- {Butter: 4}, 
		- {Milk: 4}, 
		- {Diaper: 3}

# Step 2: Candidate Generation for Pairs (k=2) -- The Join Step
- Input: the frequent items from $L_1$
- Computation:
	- The algo performs a "Join step" (Apriori-gen), combining the items in $L_1$ with each other to create all possible pairs(2-itemsets)
		- {Bread, Butter}
		- {Bread, Milk}
		- {Bread, Diaper}
		- {Butter, Milk}
		- {Butter, Diaper}
		- {Milk, Diaper}
- Output:
	- All candidates $C_2$: 
		- \[{Bread, Butter}, 
		- {Bread, Milk}, 
		- {Bread, Diaper}, 
		- {Butter, Milk}, 
		- {Butter, Diaper}, 
		- {Milk, Diaper}]

# Step 3: Filtering Pairs(k=2) - The Prune & Count Step
- Input: 
	- the candidate in $C_{2}$ 
	- the transactions $T$
- Computation
	1. Prune
		1. Since all individual items in the pairs belog to $L_1$ no action
	2. Count
		1. Count how many pair appears together
			- {Bread, Butter}: Appears in T1, T2, T5 (Count = 3)
			- {Bread, Milk}: Appears in T1, T2, T5 (Count = 3)
			- {Bread, Diaper}: Appears in T3, T5 (Count = 2)
			- {Butter, Milk}: Appears in T1, T2, T4, T5 (Count = 4)
			- {Butter, Diaper}: Appears in T4, T5 (Count = 2)
			- {Milk, Diaper}: Appears in T4, T5 (Count = 2)
		2. Drop low counts
			- {Bread, Butter}: 3 --> Keep
			- {Bread, Milk}: 3 --> Keep
			- {Bread, Diaper}: 2 --> Drop
			- {Butter, Milk}: 4 --> Keep
			- {Butter, Diaper}: 2 --> Drop
			- {Milk, Diaper}: 2 --> Drop
- Output
	- Set $L_2$ (frequent 2-itemsets):
		- {Bread, Butter}: 3
		- {Bread, Milk}: 3
		- {Butter, Milk}: 4

# Step 4: Finding Triplets (k=3) - Join and Prune
- Input: The frequent pairs from $L_{2}$
- Computation:
	1. Join: 
		1. The algo will join items in $L_2$ that share a common first item to create size-3 candidates ($C_3$). 
		2. Creats $C_3$ = {Bread, Butter, Milk} by combining {Bread, Butter}, {Bread, Milk}. 
		3. Note {Butter, Milk} has no other pairs that start with Butter, so we skip
	2. Prune:
		1. The also checks if all size-2 subsets of $C_3$ = {Bread, Butter, Milk} exist in $L_2$.
		2. The subsets are {Bread, Butter}, {Butter, Milk}, and {Butter, Milk}
	3. Count:
		1. Count the instance of {Bread, Butter, Milk}.
		2. Appears Three times: T1, T2, T5 (Count=3)
		3. Since it meets our minsup it is kept
- Output:
	- $L_3$: {Bread, Butter, Milk: 3}. 
	- Also algo stops because we only have length 1 itemset


# Step 5: Generating Association Rules
- Input: 
	- The frequent itemsets found, largest set found: $I$ = {Bread, Butter, Milk}
	- a user-specified Minimum confidence threshold (`minconf`). 
- Computation:
	- The algorithm computes the confidence for each possible rule by taking a subset(S) of the frequent itemset and pointing it to the rtemaining item(i). 
	- The formula is  $$\text{Confidence}(S)=\frac{\text{count}(I)}{\text{count}(S)}$$
	- Rule 1: {Bread, Butter} --> Milk: 100%
		- The count of $I$ or {Bread, Butter, Milk} is 3.
		- The count of S or {Bread, Butter} is 3.
		- Thus 3/3 = 100%
	- Rule 2: {Bread, Milk} --> Butter: 100%
		- The count of $I$ is 3.
		- The count of S or {Bread, Milk} is 3
		- Thus 3/3 = 100%
	- Rule 3: {Butter, Milk} --> Bread: 75%
		- The count of S or {Butter, Milk} is 4
		- Thus 3/4 = 75%
- Output:
	- Since all of the rules meets 75% `misconf` threshold, the final output is all three valid association rules telling us exactly how customer purchases drive other purchases