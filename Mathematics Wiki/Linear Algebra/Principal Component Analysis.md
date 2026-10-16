Principal Component Analysis (PCA) is a technique used to reduce the number of variables in a dataset while preserving as much information as possible.

## 1. A simple example

Imagine we have information about five students:

|Student|Study hours|Exam score|
|---|---|---|
|A|1|50|
|B|2|60|
|C|3|70|
|D|4|80|
|E|5|90|

* Notice that as study hours increase, exam scores also increase. These two variables follow the same pattern.
* PCA helps us combine them into a new variable called a principal component.
* Instead of working with two variables, we can represent the main pattern using just one component.

## 2. How does PCA work?

PCA follows four main steps:
1. Standardize the data: Put variables on comparable scales when necessary.
2. Find relationships: Identify how variables vary together.
3. Calculate principal components: Find new directions that capture the greatest variance.
4. Reduce dimensions: Keep the most informative components and discard less informative ones.

## 3. A numerical example

Let's subtract the mean of each column from every value.
- Average study hours = 3
- Average exam score = 70

Our centered dataset becomes:

|Student|Study hours − 3|Exam score − 70|
|---|---|---|
|A|-2|-20|
|B|-1|-10|
|C|0|0|
|D|1|10|
|E|2|20|

The second column is exactly ten times the first column. Both variables describe the same pattern.

Therefore, PCA can represent this dataset with just one principal component, PC1, without losing any variance.

- Before PCA: 2 variables.
- After PCA: 1 component.
- Variance preserved: 100% in this example.

This is an idealized example. Real datasets usually contain more complex relationships.

## 4. Try it in Python

You can use the `scikit-learn` library to perform PCA.

```python
import numpy as np

from sklearn.preprocessing import StandardScaler

from sklearn.decomposition import PCA
```
#### Study hours and exam scores

```python
X = np.array([
	[1, 50],
	[2, 60],
	[3, 70],
	[4, 80],
	[5, 90]
])
```
#### Standardize the data

```python
X_scaled = StandardScaler().fit_transform(X)
```
#### Reduce 2 variables to 1 component

```python
pca = PCA(n_components=1)
X_reduced = pca.fit_transform(X_scaled)

print("Original shape:", X.shape)
print("Reduced shape:", X_reduced.shape)
print("Explained variance:", pca.explained_variance_ratio_)
print(X_reduced)
```

The output will show:

- `Original shape: (5, 2)` — five students and two variables.
- `Reduced shape: (5, 1)` — five students and one component.
- `Explained variance: [1.]` — 100% of the variance is preserved.

## 5. Important concepts to remember

| Term                     | Meaning                                                                   |
| ------------------------ | ------------------------------------------------------------------------- |
| Variance                 | How much the data varies                                                  |
| PC1                      | Component capturing the greatest variance                                 |
| PC2                      | Component capturing the greatest remaining variance, perpendicular to PC1 |
| Eigenvalue               | The variance captured by a component                                      |
| Explained variance ratio | The proportion of total variance captured by a component                  |

In one sentence: PCA transforms many potentially related variables into fewer new variables that preserve as much of the original data's variation as possible.