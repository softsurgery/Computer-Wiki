**PCA is a dimensionality reduction technique** that transforms high-dimensional data into a lower-dimensional form while preserving as much variance (information) as possible.

---

### **Simple Analogy**
Imagine you have 3D data points (like points in space). PCA finds the "best viewpoints" to look at this data:
1. **First viewpoint** shows the most spread/variation
2. **Second viewpoint** shows the next most (perpendicular to first)
3. And so on...

You can then choose to keep only the first 2 viewpoints (reducing 3D → 2D) while losing minimal information.

---

### **How PCA Works**

1. **Center the data** - Subtract the mean from each feature    
2. **Compute covariance matrix** - Measures how features vary together
    
3. **Calculate eigenvectors & eigenvalues** of covariance matrix
    
    - Eigenvectors = principal components (directions of maximum variance)
        
    - Eigenvalues = amount of variance along each direction
        
4. **Sort components** by eigenvalues (highest variance first)
    
5. **Project data** onto top *k* components
    

---

### **Key Properties**

- **Unsupervised** - No labels needed
    
- **Linear transformation** - Finds best linear subspaces
    
- **Orthogonal components** - Each PC is perpendicular to others
    
- **Variance preservation** - First PC captures most variance, second captures next most, etc.
    

---

### **Common Applications**

- **Data visualization** (reduce to 2D/3D for plotting)
    
- **Noise reduction** (remove low-variance components)
    
- **Feature extraction** (create uncorrelated features)
    
- **Data compression** (store only important components)
    
- **Speeding up ML algorithms** (fewer dimensions = faster training)
    

---

### **Example**

If you have 100 features (dimensions), PCA might show that:

- First 5 PCs capture 90% of variance
    
- You can use just these 5 instead of 100 with minimal information loss
    

---

### **Limitations**

- **Linear assumptions** - Cannot capture complex nonlinear relationships
    
- **Variance ≠ importance** - High variance doesn't always mean most informative
    
- **Interpretability** - PCs are linear combinations of original features (harder to interpret)
    

---

### **Python Example (scikit-learn)**

python

from sklearn.decomposition import PCA

# Reduce 30 features to 2 components
pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X)

print(f"Explained variance ratio: {pca.explained_variance_ratio_}")

**PCA is essentially finding the "most informative perspectives" of your data and discarding less important ones.**