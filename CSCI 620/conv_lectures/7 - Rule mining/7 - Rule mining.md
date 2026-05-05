# Rule mining (classification)

ROCHESTER INSTITUTE OF TECHNOLOGY DEPT. OF COMPUTER SCIENCE  $R \cdot I \cdot T$ 

These slides correspond to the unit of frequent itemset mining.

#### Definition

Given some tabular data, extract classification rules that describe the data based on a target attribute (label).
Definition of the problem.

This is the roadmap we will follow.
#### Concept learning systems

- A concept learning system learns a description of a target concept from labeled examples.
- Two options:
- Decision tree induction based on probabilities and impurity
- Sequential covering based on example covering and impurity

#### Formal statement

- A set of tuples D, each of which contains n attributes A = {a1, a2, … , an}, e.g., current weather.
- Each tuple also contains lj, the class label or target concept, e.g., play golf is yes. L = {l1, l2, …, lm}.
- We will learn rules of the form IF a1=v1 AND a2=v2 AND … l=l1.
- These rules will be accompanied by quality measurements describing how they fit with the provided data.

Definitions.
## Decision Trees

This is the roadmap we will follow.

#### Attribute selection

- The main concept is how to select the attribute that best classifies the remaining tuples.
- There are different metrics one can use, but the main component of all of them is AVC-sets.

#### **AVC-set**

- AVC stands for Attribute-Value, Class label.
- For a given attribute a, it counts how many tuples of each a value and class label.
- Within an AVC-set, we use AVC\[vi, li] to refer to the count of tuples such that a=vi and class label is li.
- $^{\circ}$  Within an AVC-set, we use AVC\[\*, li] to refer to the accumulated number of tuples for class label li, and AVC\[vi, \*] to refer to the accumulated number of tuples for value vi.
- AVC\[\*, \*] is the total number of tuples.

#### Example

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
### Example (I): AVC-sets

- If attribute is Outlook and class label is Play, the AVC-set is as follows:

| Outlook\Play | No | Maybe | Yes |
|--------------|----|-------|-----|
| Sunny        | 3  | 1     | 1   |
| Overcast     | 0  | 0     | 3   |
| Rain         | 1  | 1     | 2   |

- We have that AVC\[\*, No] = 4, AVC\[\*, Yes] = 6, AVC\[Sunny, \*] = 5 and AVC\[Rain, \*] = 4. AVC\[\*, \*] = 12.

### Example (II): AVC-sets
- Let's assume Outlook=Sunny is chosen. If the next attribute is Temp and class label is Play, the AVC-set is as follows:

| Temp\Play | No | Maybe | Yes |
|-----------|----|-------|-----|
| Hot       | 2  | 0     | 0   |
| Mild      | 1  | 1     | 0   |
| Cool      | 0  | 0     | 1   |
## Gini index (I)
- Having an AVC-set, the Gini index is computed as follows:
$$Gini(AVC) = 1 - \sum_{j=1}^{m} \left( \frac{AVC[*, lj]}{AVC[*, *]} \right)^{2}$$

 Assuming AVC corresponds to attribute a, the Gini index of a is as follows:
$$\begin{aligned} &Gini\_a(AVC) \\ &= \sum_{i=1}^{n} \frac{AVC[vi, *]}{AVC[*, *]} \left(1 - \sum_{j=1}^{m} \left(\frac{AVC[vi, lj]}{AVC[vi, *]}\right)^{2}\right) \end{aligned}$$

#### Gini index (II)
- It measures impurity; it quantifies how mixed the labels are in a set of tuples. The lower the impurity, the better.
- The Gini gain (reduction in impurity) is: െ \_
- The attribute with the best gain (maximum) is chosen.
### Example (I)

| Outlook\Play | No | Maybe | Yes |
|--------------|----|-------|-----|
| Sunny        | 3  | 1     | 1   |
| Overcast     | 0  | 0     | 3   |
| Rain         | 1  | 1     | 2   |

- Gini(S) = 
$$1 - ((4/12)^2 + (2/12)^2 + (6/12)^2) = 1 - (1/9 + 1/36 + 1/4) = 1 - (4+1+9)/36 = 0.61$$

- Gini\_Outlook(S) = 
$$5/12 * (1 - ((3/5)^2 + (1/5)^2 + (1/5)^2)) + 3/12 * (1 - (3/3)^2) + 4/12 * (1 - ((1/4)^2 + (1/4)^2 + (2/4)^2)) = 0.44$$

- Gain = 0.61 - 0.44 = 0.17

### Example (II)

| Temp\Play | No | Maybe | Yes |
|-----------|----|-------|-----|
| Hot       | 2  | 0     | 1   |
| Mild      | 1  | 2     | 2   |
| Cool      | 1  | 0     | 3   |

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

### Example (I)
- Rule r0: If Outlook=Sunny Then Play=No
- p(r0, D) = 3, n(r0, D) = 2
- Gini(r0, D) = 1 ((3/5)^2 + (2/5)^2) = 0.48
- Rule r1: If Outlook=Sunny and Temp=Hot Then Play=No
- p(r1, D) = 2, n(r1, D) = 0
- Gini(r1, D) = 1 ((2/2)^2 + (0/2)^2) = 0
- FOIL(r0, r1, D) = 2 \* (0.48 0) = 0.96
- Conclusion: r1 is better than r0!

### Example (II)
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

This is the roadmap we will follow.
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
#### Discrimination issues
- "Rather dangerously, learning from historical data may mean to discover traditional prejudices that are endemic in reality, and to assign to such practices the status of general rules, maybe unconsciously, as these rules can be deeply hidden within a classifier."
- Direct discriminatory rules:
	- If City=NYC Then Loan=Reject (conf: 20%)
	- If Race=Black and City=NYC Then Loan=Reject (conf: 75%)
- Indirect discriminatory rules:
	- If Zipcode=10451 and City=NYC Then Loan=Reject (conf: 95%)
	- If Zipcode=10451 and City=NYC Then Race=Black (conf: 80%)
Salvatore Ruggieri, Dino Pedreschi, Franco Turini: Data mining for discrimination discovery. ACM Trans. Knowl. Discov. Data 4(2): 9:1-9:40 (2010)
#### Privacy
- "Sometimes the privacy of the people being data mined needs to be considered. This necessitates that the output of data mining algorithms be modified to preserve privacy while simultaneously not ruining the predictive power of the model."
- "Conflicts arise when trying to balance privacy requirements with the accuracy of the model."
Sam Fletcher and Md. Zahidul Islam. 2019. Decision Tree Classification with Differential Privacy: A Survey. ACM Comput. Surv. 52, 4, Article 83