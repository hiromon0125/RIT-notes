
# Data preparation(NOTE 4)
### Definition
Data preparation consists of transforming data into a suitable format for analysis by cleaning, integrating, transforming and reducing data.
### Main questions
- What data a certain dataset contains?
- What are its attributes and how they relate?
- Types of attributes:
	- Nominal: name or class of object, e.g., Ford, Buick, …
	- Ordinal: intrinsic order (and transitivity) but do not know the distance between values, e.g., hot/warm/cool/cold.
	- Interval: known ranking order and distance between values, e.g., temperature, calendar time, currency.
	- Ratio: units on the same scale, e.g., elapsed time.
# Descriptive statistics 
## Measures
- Central tendency: They show how aggregated the data points are.
- Variation: They show how spread out the data points are from the mean.
### Arithmetic mean
- Having N data points each of which takes value xi , the mean is computed as follows:
$$\overline{x} = \frac{1}{N} \sum_{i=1}^{N} x_i$$
### Geometric mean
- Same notation as before:
$$\begin{aligned} \left(\prod_{i=1}^n x_i\right)^{\frac{1}{n}} &= \sqrt[n]{x_1x_2\cdots x_n} \ &\exp\left(\frac{\sum_{i=1}^n \ln a_i}{n}\right) \end{aligned}$$
### Mode
- Having N data points each of which takes value yi , the mode is the most frequent value.
- There can be multiple modes.
- you must start from the histogram, which we will discuss below.
- The mode is the value that occurs most often in a set of data points. To compute the mode,
### Median
- Having N data points each of which takes value yi , the median is computed as follows:
$$\widetilde{y} = y_{(N+1)/2}$$
- However, this is problematic when having odd/even numbers.
### Quartiles
- First quartile: middle data point between smallest and median.
- Third quartile: middle data point between median and highest.
### Quantiles
- If we have y1, …, yn, the p-th quantile can be computed as:
- k = floor(n \* p/100)
- Any quantile is the average number between position k and position k+1.
## SQL implementation
- Conceptually, we want something like:
```sql
-- Unfortunately, this doesn't work.
SELECT value FROM Relation
ORDER BY value ASC
OFFSET k LIMIT 2
```
### Window functions in SQL (I)
- They compute values over a set of related tuples (the "window") while preserving the original tuples, unlike aggregation which collapses them.
```sql
SELECT value, ROW_NUMBER() OVER ( ORDER BY value) AS position FROM Relation
```
### Window functions in SQL (II)
- It is also possible to compute unrestricted aggregations and still preserve the tuples.
```sql
SELECT value, COUNT(*) OVER() AS total_count FROM Relation
```
>This will add a total\_count to each tuple that corresponds to the size of the relation.
### Window functions in MongoDB (I)
```js
db.Collection.aggregate([
      {$setWindowFields:
             {sortBy: { value: 1 },
             output: {
                    position:
                          {$documentNumber: {}}}
      }])
```
### Window functions in MongoDB (II)
- Using {$count: {}} counts the number of documents and adds
- the count field to every other document.
- No sortBy field is equivalent to OVER() in SQL:
```json
{$setWindowFields:
        output: {
                 cnt_field:{$count: {}},
                 min_field:{$min: {"$value"}},
                 avg_field:{$avg: {"$value"}}, …
        }
}
```
### Variance and standard deviation
- Having N data points each of which takes value xi , the variance is computed as follows:
$$\mathrm{Var}(X) = \frac{1}{n} \sum_{i=1}^n (x_i - \mu)^2$$
$$Var(X) = \frac{\sum x^2}{n} - \mu^2$$

> Variance is the expectation of the squared deviation of a random variable from its mean. Informally, it measures how far a set of (random) numbers are spread out from their average value. 
> The standard deviation is a measure that is used to quantify the amount of variation or dispersion of a set of data values. 
# Charts
## Histogram
![](CSCI%20620/conv_lectures/4%20-%20Preparation/_page_18_Figure_0.jpeg)
> A histogram is a representation of the distribution of numerical data. In SQL and MongoDB, this is just a GROUP BY clause to count occurrences. When computing mode, you must choose the one with the largest frequency.
## Scatter plot
![](_page_19_Figure_0.jpeg)
> A scatter plot represents the relationship between two variables. 
## Time series
![](CSCI%20620/conv_lectures/4%20-%20Preparation/_page_20_Figure_0.jpeg)
> A time series is similar to a scatter plot but the X-axis is related to time.

## Box and whisker
![](CSCI%20620/conv_lectures/4%20-%20Preparation/_page_21_Figure_0.jpeg)
> A box and whisker plot—also called a box plot—displays the five-number summary of a set of data. The five-number summary is the minimum, first quartile, median, third quartile, and maximum.
# Scaling

## Feature scaling

| Name               | Formula                                                  |
| ------------------ | -------------------------------------------------------- |
| Min-max            | $x' = \frac{x - \min(x)}{\max(x) - \min(x)}$             |
| Mean               | $x' = \frac{x - \mathrm{average}(x)}{\max(x) - \min(x)}$ |
| Z-score (standard) | $z = \frac{x - \mu}{\sigma}$                             |
| Z-score (squared)  | $z^2 = \frac{(x - \mu)^2}{\mathrm{Var}(X)}$              |
| Robust             | $x' = \frac{x - Q_2(x)}{Q_3(x) - Q_1(x)}$                |
# Binning

## Equal width
- For a given k, we create k buckets.
- Let's x\_max and x\_min be the max and min values we want to bin.
- We'll compute buckets as follows:
	- width = (x_max - x_min)/k
	- \[x_min, x_min + width)
	- \[x_min + width, x_min + width * 2)
	- ...
	- \[x_min + width * (k-1), x_max + tiny_value)
- The bin id for a value x is FLOOR((x – x\_min)/width).
## Equal depth
- If there are n values, we want to create buckets of size n/k.
- If a value is in position p, its bin id is CEIL(p \* k/n).
# Fixing data issues (integration)

## Codes
- (CSCI-)320, 420, 620, …
- 14534, 14623, …
> There are attributes for which it does not make sense to compute certain descriptive statistics like codes: it does not make sense to compute the mean of a set of zip codes.
## Unusual values
- When we analyze using descriptive statistics, some of these values are unusual.
- They cannot be automatically discarded, you need to analyze them and decide.
- Examples:
	- Salaries, release dates of movies, bills, etc.
## Missing values
- You can fill in holes using different strategies, e.g., adding the mean/mode of the attribute.
	- This may introduce bias!
- Placeholders for missing values, e.g., death year of an actor is set to 9999 (meaning it is still alive).
- De-referencing: using another source to include the values.
	- The other source may also have issues.
	- How to connect them? Using ids is preferred, beware when using names, titles, etc.
# Conclusions
## Data preparation
We have analyzed how to prepare data.
## Legal, privacy and ethics
- Data fixes may introduce biases (especially when searching for patterns)
- Several missing values replaced by certain numbers can deviate the descriptive statistics of an attribute
# Frequent itemset mining(NOTE 5)

## Definition
Given a number of transactions containing a number of items, extract the items that frequently appear together in such transactions a certain number of times.
# Introduction
## History: bar codes!
"Progress in bar-code technology has made it possible for retail organizations to collect and store massive amounts of sales data, referred to as the basket data. A record in such data typically consists of the transaction date and the items bought in the transaction. Successful organizations view such databases as important pieces of the marketing infrastructure. They are interested in instituting information driven marketing processes, managed by database technology, that enable marketers to develop and implement customized marketing programs and strategies."
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
## New threats to privacy
- Bias: patterns may be used for guessing confidential properties, and they may lead to stereotypes and prejudices
	- Different results based on race or ethnic group
- Combining patterns: may lead to a disclosure of individual information, either with certainty, or with a high probability
	- A study with 10 anonymous people (2 females and 8 males); there are 7 cases of disease A; none of the females has disease A; we know Mr. X was part of the study.
# Clustering (NOTE 6)
## Definition
Cluster analysis or clustering is the task of grouping a set of objects such that objects in the same group (cluster) are more similar (in some sense) to each other than to those in other groups (clusters). - Wikipedia
## What is a cluster?
- "(…) The notion of cluster cannot be precisely defined. Clustering is in the eye of the beholder, and as such, researchers have proposed many induction principles and models whose corresponding optimization problem can only be approximately solved by an even larger number of algorithms."
## Cluster models
- Connectivity models based of connectivity distance
- Centroid models based on central individuals and the distance to other individuals
- Density models based on connected and dense regions in a space
- Graph-based models based on cliques and their relaxations
# The K-Means Algorithm
## Basic idea
- We are going to work with points in a given space ℝ<sup>n</sup>
- Given a k (the number of clusters to be generated), we store k centroids that will define the central points of the cluster
- We assign points to clusters
- We reassess centroids based on current assignment
## Basic Idea in Action
![](CSCI%20620/conv_lectures/6%20-%20Clustering/_page_8_Figure_0.jpeg)
Figure (a) represents the original points and (c) the clustering after random assignment and how we progress.
## Formal statement
- A set of points  $x^{(1)}$ , ...,  $x^{(m)}$
- Each  $\mathbf{x}^{(i)} \in \mathbb{R}^n$
- $^{\bullet}$  Our goal: compute  $\mu_{1}\text{, }...\text{, }\mu_{k}$  centroids and a label  $c^{(i)}$  for each point
- $^{\bullet} \text{ Each } \mu_i {\in } \ \mathbb{R}^n$
- $\|x^{(i)} - \mu_i\|$  is the Euclidean distance between the points, i.e., Sqrt(( $x^{(i)}[0]$ - $\mu_i[0]$ )^2 + ( $x^{(i)}[1]$ - $\mu_i[1]$ )^2) + ...)
- Any distance can be used
## Algorithm 
1.  Initialize cluster centroids $\mu_1, \mu_2, \dots, \mu_k \in \mathbb{R}^n$ randomly. 
2. Repeat until convergence: { 
	For every i, set $c^{(i)} := \arg\min_{j} ||x^{(i)} - \mu_j||^2.$ 
	For each j, set $\mu_j := \frac{\sum_{i=1}^m 1\{c^{(i)} = j\}x^{(i)}}{\sum_{i=1}^m 1\{c^{(i)} = j\}}.$ 
}
## Pseudo-code in Python-like (I)
```python
# Function: K Means
#------
# K-Means is an algorithm that takes in a dataset and a constant
# k and returns k centroids (which define clusters of data in the
# dataset which are similar to one another).
def kmeans (dataset, k):
	# Initialize centroids randomly
	numFeatures = dataset.getNumFeatures ()
	centroids= getRandomCentroidsnumFeatures,)
	# Initialize book keeping vars.
	iterations = 0
	oldcentroids = None
	# Run the main k-means algorithm
	while not shouldstop(oldCentroids, centroids, iterations) :
		# Save old centroids for convergence test. Book keeping.
		oldentroids = centroids
		iterations += 1
		# Assign labels to each datapoint based on centroids
		labels =getlabelsdataset, centroids)
		# Assign centroids based on datapoint labels
		centroids = getCentroids(dataset, labels, k)
		# We can get the labels too by calling getLabels(dataset, centroids)
		return centroids
```
## Pseudo-code in Python-like (II)
```python
# Function: Should Stop
# -----------------
# Returns True or False if k-means is done. K-means terminates either
# because it has run a maximum number of iterations OR the centroids
# stop changing.
def shouldStop(oldCentroids, centroids, iterations):
	if iterations › MAX ITERATIONS: return True 
	return oldcentroids == centroids

# Function: Get Labels
# ---------
# Returns a label for each piece of data in the dataset.
def getLabels(dataset, centroids):
	# For each element in the dataset, chose the closest centroid.
	# Make that centroid the element's label.

# Function: Get Centroids
# -------------
# Returns k random centroids, each of dimension n.
def getCentroids(dataset, labels, k):
	# Each centroid is the geometric mean of the points that
	# have that centroid's label. Important: If a centroid is empty (no points hav
	# that centroid's label) you should randomly re-initialize it.
```
## Example (I)
![](CSCI%20620/conv_lectures/6%20-%20Clustering/_page_13_Figure_0.jpeg)
## Example (II)
- Points: (1, 2), (1, 3), (2, 3), (2, 4), (4, 6), (5, 6), (6, 6), (6, 8), (7, 7)
- Manhattan distance: d((a, b), (x, y)) = |a x| + |b – y|
- k=2
- We are going to limit to x(i) ℕ<sup>2</sup>
- Random centroids: $\mu_{1}$ = (2, 8), $\mu_{2}$ = (8, 1)
## Example (III)
- Distances ($\mu_{1}$ | $\mu_{2}$):

|     | 1      | 2      | 4       | 5      | 6      | 7      |
| --- | ------ | ------ | ------- | ------ | ------ | ------ |
| 2   | 7 \| 8 |        |         |        |        |        |
| 3   | 6 \| 9 | 5 \| 8 |         |        |        |        |
| 4   |        | 4 \| 9 |         |        |        |        |
| 6   |        |        | 4 \| 11 | 5 \| 8 | 6 \| 7 |        |
| 7   |        |        |         |        |        | 6 \| 7 |
| 8   |        |        |         |        | 4 \| 9 |        |
- Recomputing centroids: $\mu_{1}$ = (34/9, 45/9) = (4, 5), $\mu_{2}$ = (2, 2) (randomly reinitialized since no points were assigned to 2)
## Example (IV)
![](CSCI%20620/conv_lectures/6%20-%20Clustering/_page_16_Figure_0.jpeg)
## Example (V)
- Centroids: $\mu_{1}$ = (4, 5), $\mu_{2}$ = (2, 2)
- Distances ($\mu_{1}$ | $\mu_{2}$):

|     | 1      | 2      | 4      | 5      | 6       | 7       |
| --- | ------ | ------ | ------ | ------ | ------- | ------- |
| 2   | 6 \| 1 |        |        |        |         |         |
| 3   | 5 \| 2 | 4 \| 1 |        |        |         |         |
| 4   |        | 3 \| 2 |        |        |         |         |
| 6   |        |        | 1 \| 6 | 2 \| 5 | 3 \| 8  |         |
| 7   |        |        |        |        |         | 5 \| 10 |
| 8   |        |        |        |        | 5 \| 10 |         |
- Recomputing centroids: $\mu_{1}$ = (28/5, 33/5) = (6, 7), $\mu_{2}$ = (6/4, 12/4) = (1, 3)
## Example (VI)
![](CSCI%20620/conv_lectures/6%20-%20Clustering/_page_18_Figure_0.jpeg)
## Example (VII)
- Centroids: $\mu_{1}$ = (6, 7), $\mu_{2}$ = (1, 3)
- Distances ($\mu_{1}$ | $\mu_{2}$):

|     | 1       | 2      | 4      | 5      | 6       | 7       |
| --- | ------- | ------ | ------ | ------ | ------- | ------- |
| 2   | 10 \| 1 |        |        |        |         |         |
| 3   | 8 \| 0  | 8 \| 1 |        |        |         |         |
| 4   |         | 8 \| 2 |        |        |         |         |
| 6   |         |        | 3 \| 6 | 2 \| 7 | 1 \| 8  |         |
| 7   |         |        |        |        |         | 1 \| 10 |
| 8   |         |        |        |        | 1 \| 10 |         |
- Recomputing centroids: $\mu_{1}$ = (28/5, 33/5) = (6, 7), $\mu_{2}$ = (6/4, 12/4) = (1, 3); same centroids so stop
## Example (VIII)

![](CSCI%20620/conv_lectures/6%20-%20Clustering/_page_20_Figure_0.jpeg)
# Evaluating Clusters
Sum of squared errors
$$SSE = \sum_{i=1}^{k} \sum_{x_j \in C_i} (x_j - \mu_i)^2$$
This is used as a method of measuring the variation within clusters.
## Example
° Centroids:  $\mu_1=(6,7), \ \mu_2=(1,3). \ \{(1,2), (1,3), (2,3), (2,4)\}$  are assigned to  $\mu_2$ , and  $\{(4,6), (5,6), (6,6), (6,8), (7,7)\}$  are assigned to  $\mu_1$ 

|   | 1 | 2 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|
| 2 | 1 |   |   |   |   |   |
| 3 | 0 | 1 |   |   |   |   |
| 4 |   | 2 |   |   |   |   |
| 6 |   |   | 3 | 2 | 1 |   |
| 7 |   |   |   |   |   | 1 |
| 8 |   |   |   |   | 1 |   |
• SSE =  $1^2 + 0^2 + 1^2 + 2^2 + 3^2 + 2^2 + 1^2 + 1^2 + 1^2 = 22$ 
## Finding a good k
![](CSCI%20620/conv_lectures/6%20-%20Clustering/_page_24_Figure_0.jpeg)
Note that in k-means we need to provide a k (the number of clusters); however, it is unclear how many clusters we should have for a given set of points. We need to find what it is called the "knee": the region where the change is slope begins to level off.
## Intra- and inter-cluster distances
- There are two criteria to assess cluster quality:
	- Intercluster dissimilarity
	- Intracluster similarity
- Ideally, the distance between clusters should be large, while the distance within clusters should be very small

![](_page_25_Picture_5.jpeg)
## Silhouette coefficient
- For each x(i), a(x(i)) is the average distance between x(i) and all the points classified in cluster c(i) (same cluster as x(i))
- For each x(i) and cluster c(k) (other than c(i)), let d(x(i), c(k)) the average distance between x(i) and all the points classified in c(k)
- Let b(x(i)) = d(x(i), c(k)) such that it is minimum
- $S(x^{i}))$ = b(x(i)) a($x^{i}$)/max{a($x^{i}$), b($x^{i}$)}
- S $\in$ \[-1, 1] = ∑ S($x^{i}$/m (recall that we have m points)
- The closer S = 1 the better
## Example

-  C1: (1, 2), (1, 3); C2: (3, 4), (4, 5); C3: (7, 7), (8, 7)
-  S((1, 2)) = 5 - 1 / 5; S((1, 3)) = 4 - 1 / 4
- $S((3, 4)) = 3.5 - 2 / 3.5$;  $S((4, 5)) = 5.5 - 2 / 5.5$ 
- $S((7,7)) = 6 - 1 / 6$$;  $S((8,7)) = 7 - 1 / 7$ 
- $S = (4/5 + 3/4 + 1.5/3.5 + 3.5/5.5 + 5/6 + 6/7)/6$
	 $= (0.8 + 0.75 + 0.43 + 0.64 + 0.83 + 0.86)/6 = 0.72$
## Mutual information (I)
- Let  $X = \{x^{(1)}, ..., x^{(m)}\}$  be a set of elements (points in our case)
- $^{\circ}$  Let U = {U\_1, ..., U\_R} and V = {V\_1, ..., V\_C} be two partitions, where U\_i (i=1..R) and V\_k (k=1..C) are clusters of elements
- These partitions are also called assignments, e.g.,  $x^{(1)}\in U_1,\, x^{(2)}\in U_1,\, x^{(3)}\in U_2$  ...
## Mutual information (II)
- Each partition is pairwise disjoint, i.e., a given elements cannot belong to two or more clusters at the same time
- More formally, $U_{i}\ \cap\ U_{k} = \emptyset$ and $V_{i}\ \cap\ V_{k} = \emptyset$
- Each partition is complete, i.e., all elements are found in U and V
- More formally, $U_{1}\ \cap\ U_{2} \cdots \cap\ U_{R} = X$ and $V_{1}\ \cap\ V_{2} \cdots \cap\ V_{C} = X$
## Mutual information (III)
- The probability of an element assigned to a cluster  $U_i$  (similarly  $V_i$ ) is:
	- $P_{U}(i) = \mid U_{i} \mid / m$  (similarly  $P_{V}(k) = \mid V_{k} \mid / m)$
- The entropy associated with a partition is:
	- H(U) =  $\sum_{i=1}^{R}$  P<sub>U</sub>(i) log P<sub>U</sub>(i) (similarly H(V) =  $\sum_{k=1}^{C}$  P<sub>V</sub>(k) log P<sub>V</sub>(k))
- The mutual information between U and V is as follows:
	- MI(U, V) =  $\sum_{i=1}^{R} \ \sum_{k=1}^{C} \ \mathrm{P_{UV}(i,\,k)} \log \ (\mathrm{P_{UV}(i,\,k)/(P_{U}(i)} \ \mathrm{P_{V}(k)))}$
	- where  $P_{UV}(i,\,k)=\,|\,U_{i}\cap V_{k}^{}\,|\,/m$
- Special case:
	- If p (any probability) = 0, then, we assume p log p = 0
- MI(U, V) is non-negative and upper bounded by H(U) and H(V)
## Example (I)
- Assume the following partitions (assignments):
- U={U1 = {x(1), x(3), x(7)}, U2 = {x(4), x(5)}, U3 = {x(2), x(6), x(8)}}
- $V = \{ V_1 = \{x^{(3)}, x^{(4)}, x^{(7)}\}, V_2 = \{x^{(2)}, x^{(5)}\}, V_3 = \{x^{(1)}\}, V_4 = \{x^{(6)}, x^{(8)}\}\}$
- Pu(1)=3/8, Pu(2) = 2/8, Pu(3) = 3/8
- Pv(1)=3/8, Pv(2)=2/8, Pv(3)=1/8, Pv(4)=2/8
- H(U) = -(3/8 log 3/8 + 2/8 log 2/8 + 3/8 log 3/8) = 1.08
- H(V) = -(3/8 log 3/8 + 2/8 log 2/8 + 1/8 log 1/8 + 2/8 log 2/8) = 1.32
## Example (II)
- Puv:

|     | V1  | V2  | V3  | V4  |
| --- | --- | --- | --- | --- |
| U1  | 2/8 | 0/8 | 1/8 | 0/8 |
| U2  | 1/8 | 1/8 | 0/8 | 0/8 |
| U3  | 0/8 | 1/8 | 0/8 | 2/8 |
- MI(U, V) = 2/8 log 2/8/(3/8 \* 3/8) + 0 + 1/8 log 1/8/(3/8 \* 1/8) + 0 + 1/8 log 1/8/(2/8 \* 3/8) + 1/8 log 1/8/(2/8 \* 2/8) + 0 + 0 + 0 + 1/8 log 1/8/(3/8 \* 1/8) + 0 + 2/8 log 2/8/(3/8 \* 2/8) = 0.76
## Example (III)
- Assume the following partitions (assignments):
- U={U1 = {x(1), x(3), x(7)}, U2 = {x(4), x(5)}, U3 = {x(2), x(6), x(8)}} • Y={Y1 = {x(1)}, Y2 = {x(2)}, Y3 = {x(3), x(4)}, Y4 = {x(5), x(6)}, Y5 = {x(7)}, Y6 = {x(8)}}
- Pu(1)=3/8, Pu(2) = 2/8, Pu(3) = 3/8
- Py(1)=1/8, Py(2)=1/8, Py(3)=2/8, Py(4)=2/8, Py(5)=1/8, Py(6)=1/8
- H(U) = -(3/8 log 3/8 + 2/8 log 2/8 + 3/8 log 3/8) = 1.08
- H(Y) = -(1/8 log 1/8 + 1/8 log 1/8 + 2/8 log 2/8 + 2/8 log 2/8 + 1/8 log 1/8 + 1/8 log 1/8) = 1.73
## Example (IV)
• Puy:

|     | Y1  | Y2  | Y3  | Y4  | Y5  | Y6  |
| --- | --- | --- | --- | --- | --- | --- |
| U1  | 1/8 | 0/8 | 1/8 | 0/8 | 1/8 | 0/8 |
| U2  | 0/8 | 0/8 | 1/8 | 1/8 | 0/8 | 0/8 |
| U3  | 0/8 | 1/8 | 0/8 | 1/8 | 0/8 | 1/8 |
- MI(U, Y) = 1/8 log 1/8/(3/8\*1/8) + 0 + 1/8 log 1/8/(3/8\*2/8) + 0 + 1/8 log 1/8/(3/8\*1/8) + 0 + 0 + 0 + 1/8 log 1/8/(2/8\*2/8) + 1/8 log 1/8/(2/8\*2/8) + 0 + 0 + 0 + 1/8 log 1/8/(3/8\*1/8) + 0 + 1/8 log 1/8/(3/8\*2/8) + 0 + 1/8 log 1/8/(3/8\*1/8) = 0.74
## Distance based on MI
- Having partitions U, V and Y, we cannot compare their mutual information directly but using a metric
- H(U) = 1.08, H(V) = 1.32, H(Y) = 1.73, MI(U, V) = 0.76, MI(U, Y) = 0.74
- Distance: D(U, V) = 1 (MI(U, V) / Max(H(U), H(V)))
- D(U, V) = 0.42, D(U, Y) = 0.57
- U and V are closer than U and Y
The mutual information of U with respect to V and Y seem very similar. However, they are not if we consider the following distance.
## Adjustment for chance (I)

| $U\backslash V$ | $V_1$           | $V_2$    |     | $V_C$    | Sums                   |
|-----------------|-----------------|----------|-----|----------|------------------------|
| $U_1$           | n <sub>11</sub> | $n_{12}$ |     | $n_{1C}$ | $a_1$                  |
| $U_2$           | $n_{21}$        | $n_{22}$ |     | $n_{2C}$ | $a_2$                  |
| :               | :               | :        | ٠., | :        |                        |
| $U_R$           | $n_{R1}$        | $n_{R2}$ |     | $n_{RC}$ | $a_R$                  |
| Sums            | $b_1$           | $b_2$    |     | $b_C$    | $\sum_{ij} n_{ij} = N$ |
## Adjustment for chance (II)
$$\mathbf{E}\{I(\mathbf{U},\mathbf{V})\} = \sum_{i=1}^{R} \sum_{j=1}^{C} \sum_{n_{ij}=\max(a_i+b_j-N,0)}^{\min(a_i,b_j)} \frac{n_{ij}}{N} \log(\frac{N.n_{ij}}{a_ib_j}) \frac{a_i!b_j!(N-a_i)!(N-b_j)!}{N!n_{ij}!(a_i-n_{ij})!(b_j-n_{ij})!(N-a_i-b_j+n_{ij})!}$$
$$AMI(\mathbf{U}, \mathbf{V}) = \frac{I(\mathbf{U}, \mathbf{V}) - \mathbf{E}\{I(\mathbf{U}, \mathbf{V})\}}{\max\{H(\mathbf{U}), H(\mathbf{V})\} - \mathbf{E}\{I(\mathbf{U}, \mathbf{V})\}}$$
"To correct the measures for randomness it is necessary to specify a model according to which random partitions are generated. Such a common model is the "permutation model" (Lancaster, 1969, p. 214), in which clusterings are generated randomly subject to having a fixed number of clusters and points in each clusters."
# Hierarchy of clusters
- There are two choices: agglomerative and divisive.
- In agglomerative, each point forms its own cluster and they get aggregated.
- In divisive, all points form a single cluster and they get split.
- The key is the linkage method: having two clusters A and B with points assigned to each of them, decide d(A, B), the distance between A and B.
## Linkage methods (excerpt)

| Names                                                                   | Formula                                                                                                                                                                                                                                                                            |
| ----------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maximum or complete-linkage<br>clustering                               | $\max_{a \in A, b \in B} d(a,b)$                                                                                                                                                                                                                                                   |
| Minimum or single-linkage<br>clustering                                 | $\min_{a\in A,b\in B}d(a,b)$                                                                                                                                                                                                                                                       |
| Unweighted average linkage clustering (or UPGMA)                        | $\frac{1}{ A \cdot B }\sum_{a\in A}\sum_{b\in B}d(a,b).$                                                                                                                                                                                                                           |
| Weighted average linkage clustering (or WPGMA)                          | $d(i \cup j,k) = \frac{d(i,k) + d(j,k)}{2}.$                                                                                                                                                                                                                                       |
| Centroid linkage clustering, or<br>UPGMC                                | $\| \mu_A - \mu_B\ \|^2$ where $\mu_A$ and $\mu_B$ are the centroids of A resp. B.                                                                                                                                                                                                 |
| Median linkage clustering, or<br>WPGMC                                  | $d(i \cup j, k) = d(m_{i \cup j}, m_k)$ where $m_{i \cup j} = \frac{1}{2} \left(m_i + m_j\right)$                                                                                                                                                                                  |
| Versatile linkage clustering <sup>[9]</sup>                             | $\sqrt[p]{\frac{1}{ A \cdot B }\sum_{a\in A}\sum_{b\in B}d(a,b)^p}, p\neq 0$                                                                                                                                                                                                       |
| Ward linkage, [10] Minimum<br>Increase of Sum of Squares<br>(MISSQ)[11] | $\frac{ \|A\|  \cdot  \|B\| }{ \|A \cup B\| } \| \mu_A - \mu_B\ \|^2 = \sum_{x \in A \cup B} \| x - \mu_{A \cup B}\|^2 - \sum_{x \in A} \|x - \mu_A\|^2 - \sum_{x \in B} \|x- \mu_B\|^2$                                                                                           |
| Minimum Error Sum of<br>Squares (MNSSQ) <sup>[11]</sup>                 | $\sum_{x \in A \cup B} \lVert x - \mu_{A \cup B} \rVert^2$                                                                                                                                                                                                                         |
| Minimum Increase in Variance<br>(MIVAR) <sup>[11]</sup>                 | $\begin{split} &\frac{1}{ A \cup B } \sum_{x \in A \cup B} \| x - \mu_{A \cup B}\|^2 - \frac{1}{ A } \sum_{x \in A} \ x - \mu_A\|^2 - \frac{1}{ B } \sum_{x \in B} \ x - \mu_B\|^2 \\ &= \operatorname{Var}(A \cup B) - \operatorname{Var}(A) - \operatorname{Var}(B) \end{split}$ |
## Example (I)
- Cluster A: p1=(1, 2), p2=(2, 3)
- Cluster B: p3=(8, 7), p4=(7, 8), p5=(9, 9)
- Cluster C: p6=(4, 1), p7=(5, 2), p8=(6, 1)
- Minimum Linkage (Single Linkage): Smallest distance between any two points.
- Maximum Linkage (Complete Linkage): Largest distance between any two points.
- Average Linkage: Mean of all pairwise distances.
## Example (II)
```
• A and B:
 • p1-p3: |1-8| + |2-7| = 12
 • p1-p4: |1-7| + |2-8| = 12
 • p1-p5: |1-9| + |2-9| = 15
 • p2-p3: |2-8| + |3-7| = 10
```
- p2-p4: |2-7| + |3-8| = 10
- p2-p5: |2-9| + |3-9| = 13
- Min: 10
- Max: 15
- Mean: (12 + 12 + 15 + 10 + 10 + 13)/6 = 72/6 = 12
## Example (III)
```
• A and C:
 • p1-p6: |1-4| + |2-1| = 4
 • p1-p7: |1-5| + |2-2| = 4
 • p1-p8: |1-6| + |2-1| = 6
 • p1-p6: |2-4| + |3-1| = 4
```
- p1-p7: |2-5| + |3-2| = 4
- p1-p8: |2-6| + |3-1| = 6
- Min: 4
- Max: 6
- Mean: (4 + 4 + 6 + 4 + 4 + 6)/6 = 28/6 = 4.67
## Example (IV)
```
• B and C:
 • p3-p6: |8-4| + |7-1| = 10
 • p3-p7: |8-5| + |7-2| = 8
 • p3-p8: |8-6| + |7-1| = 8
 • p4-p6: |7-4| + |8-1| = 10
 • p4-p7: |7-5| + |8-2| = 8
 • p4-p8: |7-6| + |8-1| = 8
 • p5-p6: |9-4| + |9-1| = 13
 • p5-p7: |9-5| + |9-2| = 11
 • p5-p8: |9-6| + |9-1| = 11
• Min: 8
• Max: 13
• Mean: (10 + 8 + 8 + 10 + 8 + 8 + 13 + 11 + 11)/9 = 87/9 = 9.67
```
# Conclusions
![](_page_46_Figure_0.jpeg)
We have studied a technique to cluster points together.
## Fairness in clustering
- Group fairness for sensitive attributes (gender, ethnicity, religion…)
- Choosing a cluster with a high gender or ethnic skew for positive or negative actions, e.g., interview shortlisting or high scrutiny
- Could also lead to reinforcement of societal stereotypes: more black people go to jail
# Rule mining -- classification (NOTE 7)

## Definition
Given some tabular data, extract classification rules that describe the data based on a target attribute (label).
Definition of the problem.
## Concept learning systems
- A concept learning system learns a description of a target concept from labeled examples.
- Two options:
- Decision tree induction based on probabilities and impurity
- Sequential covering based on example covering and impurity
## Formal statement
- A set of tuples D, each of which contains n attributes A = {a1, a2, … , an}, e.g., current weather.
- Each tuple also contains lj, the class label or target concept, e.g., play golf is yes. L = {l1, l2, …, lm}.
- We will learn rules of the form IF a1=v1 AND a2=v2 AND … l=l1.
- These rules will be accompanied by quality measurements describing how they fit with the provided data.
## Decision Trees
### Attribute selection
- The main concept is how to select the attribute that best classifies the remaining tuples.
- There are different metrics one can use, but the main component of all of them is AVC-sets.
### **AVC-set**
- AVC stands for Attribute-Value, Class label.
- For a given attribute a, it counts how many tuples of each a value and class label.
- Within an AVC-set, we use AVC\[vi, li] to refer to the count of tuples such that a=vi and class label is li.
- $^{\circ}$  Within an AVC-set, we use AVC\[\*, li] to refer to the accumulated number of tuples for class label li, and AVC\[vi, \*] to refer to the accumulated number of tuples for value vi.
- AVC\[\*, \*] is the total number of tuples.
### Example

| id | Outlook  | Temp | Humid  | Wind   | Play  |
|----|----------|------|--------|--------|-------|
| 1  | Sunny    | Hot  | High   | Weak   | No    |
| 2  | Sunny    | Hot  | High   | Strong | No    |
| 3  | Overcast | Hot  | High   | Weak   | Yes   |
| 4  | Rain     | Mild | High   | Weak   | Yes   |
| 5  | Rain     | Cool | Normal | Weak   | Yes   |
| 6  | Rain     | Cool | Normal | Strong | No    |
| 7  | Overcast | Cool | Normal | Strong | Yes   |
| 8  | Sunny    | Mild | High   | Weak   | No    |
| 9  | Sunny    | Cool | Normal | Weak   | Yes   |
| 10 | Rain     | Mild | Normal | Weak   | Maybe |
| 11 | Sunny    | Mild | Normal | Strong | Maybe |
| 12 | Overcast | Mild | High   | Strong | Yes   |
- If attribute is Outlook and class label is Play, the AVC-set is as follows:

| Outlook\Play | No  | Maybe | Yes |
| ------------ | --- | ----- | --- |
| Sunny        | 3   | 1     | 1   |
| Overcast     | 0   | 0     | 3   |
| Rain         | 1   | 1     | 2   |

- We have that AVC\[\*, No] = 4, AVC\[\*, Yes] = 6, AVC\[Sunny, \*] = 5 and AVC\[Rain, \*] = 4. AVC\[\*, \*] = 12.
- Let's assume Outlook=Sunny is chosen. If the next attribute is Temp and class label is Play, the AVC-set is as follows:

| Temp\Play | No | Maybe | Yes |
|-----------|----|-------|-----|
| Hot       | 2  | 0     | 0   |
| Mild      | 1  | 1     | 0   |
| Cool      | 0  | 0     | 1   |
## Gini index
- Having an AVC-set, the Gini index is computed as follows:
$$Gini(AVC) = 1 - \sum_{j=1}^{m} \left( \frac{AVC[*, lj]}{AVC[*, *]} \right)^{2}$$
 Assuming AVC corresponds to attribute a, the Gini index of a is as follows:
$$\begin{aligned} &Gini\_a(AVC) \\ &= \sum_{i=1}^{n} \frac{AVC[vi, *]}{AVC[*, *]} \left(1 - \sum_{j=1}^{m} \left(\frac{AVC[vi, lj]}{AVC[vi, *]}\right)^{2}\right) \end{aligned}$$
- It measures impurity; it quantifies how mixed the labels are in a set of tuples. The lower the impurity, the better.
- The Gini gain (reduction in impurity) is: െ \_
- The attribute with the best gain (maximum) is chosen.
### Example

| Outlook\Play | No  | Maybe | Yes |
| ------------ | --- | ----- | --- |
| Sunny        | 3   | 1     | 1   |
| Overcast     | 0   | 0     | 3   |
| Rain         | 1   | 1     | 2   |
- Gini(S) = 
$$1 - ((4/12)^2 + (2/12)^2 + (6/12)^2) = 1 - (1/9 + 1/36 + 1/4) = 1 - (4+1+9)/36 = 0.61$$
- Gini\_Outlook(S) = 
$$5/12 * (1 - ((3/5)^2 + (1/5)^2 + (1/5)^2)) + 3/12 * (1 - (3/3)^2) + 4/12 * (1 - ((1/4)^2 + (1/4)^2 + (2/4)^2)) = 0.44$$

- Gain = 0.61 - 0.44 = 0.17

Another example:

| Temp\Play | No  | Maybe | Yes |
| --------- | --- | ----- | --- |
| Hot       | 2   | 0     | 1   |
| Mild      | 1   | 2     | 2   |
| Cool      | 1   | 0     | 3   |
- Gini(S) = 
$$1 - ((4/12)^2 + (2/12)^2 + (6/12)^2) = 1 - (1/9 + 1/36 + 1/4) = 1 - (4+1+9)/36 = 0.61$$
- Gini\_Temp(S) = 
$$3/12 * (1 - ((2/3)^2 + (1/3)^2)) + 5/12 * (1 - ((1/5)^2 + (2/5)^2 + (2/5)^2)) + 4/12 * (1 - ((1/4)^2 + (3/4)^2)) = 0.50$$
- Gain = 0.61 - 0.50 = 0.11
Outlook is preferred over Temp.
### Tree construction
- In each step, the best attribute is chosen.
- When the current is not the root, we filter D such that the tuples match the attributes selected.
- For instance, if Outlook was the root and we are in the Outlook=Sunny branch, we only use tuples in D such that Outlook=Sunny for the current node in the tree.
## Sequential Covering
This is the roadmap we will follow.
### If-then rules
- Assume a rule r as follows: If a=vi Then l=lj.
- We define the positives covered by r as:
$$p(r,D) = \left| \left\{ t \mid t \in D \land t(a) = v_i \land t(l) = l_j \right\} \right|$$
- And the negatives covered by r as:
$$n(r,D) = \left| \left\{ t \mid t \in D \land t(a) = v_i \land t(l) \neq l_j \right\} \right|$$
- The Gini index can be computed as follows:
$$Gini(r,D) = 1 - \left( \left( \frac{p(r,D)}{p(r,D) + n(r,D)} \right)^2 + \left( \frac{n(r,D)}{p(r,D) + n(r,D)} \right)^2 \right)$$
### Example
- Rule r0: If Outlook=Sunny Then Play=No
- p(r0, D) = 3, n(r0, D) = 2
- Gini(r0, D) = 1 ((3/5)^2 + (2/5)^2) = 0.48
### Adding new terms
- Having a rule r, we add a new term to the left-hand side, getting r'. How do we determine whether it is better to keep r or to use r' instead?
- "FOIL gain": FOIL(r,r',D) = p(r',D) (Gini(r,D) Gini(r',D))
- Original FOIL gain uses log function; this is the Gini index adaption.
### Example(I)
- Rule r0: If Outlook=Sunny Then Play=No
- p(r0, D) = 3, n(r0, D) = 2
- Gini(r0, D) = 1 ((3/5)^2 + (2/5)^2) = 0.48
- Rule r1: If Outlook=Sunny and Temp=Hot Then Play=No
- p(r1, D) = 2, n(r1, D) = 0
- Gini(r1, D) = 1 ((2/2)^2 + (0/2)^2) = 0
- FOIL(r0, r1, D) = 2 \* (0.48 0) = 0.96
- Conclusion: r1 is better than r0!
### Example(II)
- Rule r0: If Outlook=Sunny Then Play=No
- p(r0, D) = 3, n(r0, D) = 2
- Gini(r0, D) = 1 ((3/5)^2 + (2/5)^2) = 0.48
- Rule r2: If Outlook=Sunny and Temp=Mild Then Play=No
- p(r2, D) = 1, n(r2, D) = 1
- Gini(r2, D) = 1 ((1/2)^2 + (1/2)^2) = 0.5
- FOIL(r0, r2, D) = 1 \* (0.48 0.5) = -0.02
- Conclusion: r0 is better than r2!
### Learn rules
- Starting with If True Then l=lj.
- For every attribute a, add a=vi to the left-hand side: If a=vi Then l=lj. The rule more informative (if any) remains.
- Do this recursively until all the gains obtained are negatives (the rule is the possible best).
- Remove from D those covered by the rule and restart the process until D is empty.
# Rule evaluation
## Positives/negatives
- Having r as If a=vi Then l=lj, we define true positives, false positives, true negatives and false negatives as follows:
$$TP = p(r,D) = \left| \left\{ t \mid t \in D \land t(a) = v_i \land t(l) = l_j \right\} \right|$$
$$FP = n(r,D) = \left| \left\{ t \mid t \in D \land t(a) = v_i \land t(l) \neq l_j \right\} \right|$$
$$TN = \left| \left\{ t \mid t \in D \land t(a) \neq v_i \land t(l) \neq l_j \right\} \right|$$
$$FN = \left| \left\{ t \mid t \in D \land t(a) \neq v_i \land t(l) = l_j \right\} \right|$$
## If-then rules from decision trees
- Each path in the tree forms an if-then rule. The antecedent is the combination of attribute-value pairs combined by conjunction.
- To measure quality, we need to define a class label for the consequent. There are two options:
- The leaf has a single class label: trivial.
- The leaf has multiple class labels: use a criterion (typically majority) to decide the class label of the rule.
## Measurements
- Precision: TP / (TP + FP)
- Recall: TP/(TP + FN)
- Specificity: TN/(FP + TN)
- Accuracy: (TP + TN)/((TP + FN) + (FP + TN))
- Error rate: (FP + FN)/((TP + FN) + (FP + TN))
This is the roadmap we will follow.
# Conclusions
We have studied techniques to classify tuples based on a target label.