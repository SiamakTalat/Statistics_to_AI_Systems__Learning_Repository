# Multivariate Statistical Analysis

Multivariate statistical analysis is the branch of statistics used when **multiple variables interact simultaneously and their joint structure carries more information than isolated pairwise analysis**.

Unlike univariate (single variable) or bivariate (two variable) methods, multivariate methods explicitly model:

- Covariance structure (how variables co-vary)
- Latent dimensions (hidden patterns)
- Group separation and classification
- Joint dependence and correlations
- High-dimensional geometry
- Multi-response inference
- Variable redundancy and compression

This module is foundational for:
- Dimensionality reduction and feature compression
- Feature extraction and latent structure discovery
- Pattern recognition and clustering
- High-dimensional AI datasets
- Structural health monitoring (SHM)
- Surrogate modeling for expensive simulations
- Multivariate optimization indicators

For modern AI and engineering datasets, this is one of the most valuable advanced statistics modules.

---

## Learning Objectives

After completing this lecture, you will be able to:

1. **Understand the covariance structure** of multivariate data and its role in dimensionality reduction
2. **Apply Principal Component Analysis (PCA)** for variance compression and visualization
3. **Implement Factor Analysis** to discover latent underlying factors
4. **Conduct Multivariate ANOVA (MANOVA)** to test hypotheses about multiple response variables simultaneously
5. **Use Canonical Correlation Analysis (CCA)** to relate two blocks of variables
6. **Perform cluster analysis** and Linear Discriminant Analysis (LDA) for unsupervised and supervised classification
7. **Choose and interpret** appropriate multivariate methods for engineering and AI applications

---

## 1. Foundations of Multivariate Analysis

### 1.1 Data Structure

In multivariate analysis, we observe **p variables for n samples** simultaneously.

**Data Matrix:**
$$X \in \mathbb{R}^{n \times p}$$

- **Rows:** Individual observations (samples, experiments, designs)
- **Columns:** Variables or features

**Example:** For 1,000 FEM simulations with 59 input parameters, X is 1000 × 59.

### 1.2 Sample Covariance Matrix

The covariance matrix captures relationships among all variables:

$$S=\frac{1}{n-1}(X-\bar{X})^T(X-\bar{X})$$

**Properties:**
- **Symmetric:** S^T = S
- **Size:** p × p
- **Diagonal elements:** Variance of each variable
- **Off-diagonal elements:** Covariances between variable pairs

**Key Insight:** Most multivariate methods are based on decomposing or transforming the covariance matrix S.

**Check for Understanding:**
- If the covariance matrix has very large values in some off-diagonal elements, what does this suggest about variable relationships?

---

## 2. Principal Component Analysis (PCA)

PCA is the most important multivariate **dimensionality-reduction method**. It transforms correlated variables into a smaller set of uncorrelated components while preserving as much variance as possible.

### 2.1 Core Idea

Imagine a scatter plot of data points in p-dimensional space. PCA finds new axes (principal components) such that:
- The first axis captures the direction of maximum variance
- The second axis captures maximum remaining variance (orthogonal to first)
- And so on...

This allows you to reduce p variables to k < p components with minimal information loss.

### 2.2 Mathematical Formulation

**First Principal Component:**
$$z_1 = Xw_1$$

where w₁ solves the optimization:
$$\max_{\|w\|=1} w^T S w$$

(Maximize variance of the projection w^T X subject to unit norm constraint.)

**Eigenvalue Problem:**
$$Sw = \lambda w$$

**Solution:** w₁ is the eigenvector with largest eigenvalue λ₁ of the covariance matrix S.

**Subsequent Components:** w₂ is the eigenvector with second-largest eigenvalue, and so on. The components are **orthogonal** (uncorrelated).

### 2.3 Explained Variance Ratio

Each component's importance is measured by its explained variance ratio:

$$\text{EVR}_j = \frac{\lambda_j}{\sum_{k=1}^{p}\lambda_k}$$

**Interpretation:** EVR_j tells you what fraction of total variance is captured by component j.

**Cumulative EVR:** Sum of first k components' EVR tells you how much information is retained if you use only k components.

**Example:** If the first 3 principal components have cumulative EVR = 0.92, then these 3 components capture 92% of the variation in the original 59 variables.

### 2.4 When to Use PCA

- **Dimensionality reduction:** Reduce 59 features to 10 principal components
- **Visualization:** Project high-dimensional data to 2D or 3D for plotting
- **Denoising:** Keep only components with large variances (remove noise)
- **Feature compression:** Reduce computational cost
- **Multicollinearity handling:** Principal components are uncorrelated by construction

### 2.5 Practical Example

For your 59-input FEM datasets:
1. Compute covariance matrix S (59 × 59)
2. Find eigenvalues and eigenvectors
3. Sort by eigenvalue (descending)
4. Plot cumulative EVR curve
5. Choose k where cumulative EVR ≥ 0.9 or 0.95

**Check for Understanding:**
- If you keep only the first 5 principal components with cumulative EVR = 0.80, and then reconstruct the original data, how much information is lost?

---

## 3. Factor Analysis

Factor analysis models **observed variables as arising from a smaller set of latent (hidden) factors**, plus unique noise.

### 3.1 Model Structure

$$X = \Lambda F + \epsilon$$

where:
- **X:** p × 1 observed variable vector
- **Λ (Lambda):** p × m loading matrix (how factors influence observed variables)
- **F:** m × 1 latent factor vector (m < p)
- **ε:** p × 1 unique noise vector (variable-specific error)

### 3.2 Covariance Structure

The model implies a specific covariance structure:

$$\Sigma = \Lambda \Lambda^T + \Psi$$

where **Ψ** (Psi) is the diagonal unique variance matrix.

**Interpretation:** Total variance = variance explained by common factors + unique variance.

### 3.3 Factor Loadings

The loading matrix Λ shows:
- How much each observed variable depends on each latent factor
- High loading: variable is strongly influenced by that factor
- Low loading: variable is weakly related to that factor

### 3.4 PCA vs. Factor Analysis

| Aspect | PCA | Factor Analysis |
|--------|-----|-----------------|
| Goal | Variance compression | Latent cause modeling |
| Interpretation | Components are weighted combinations of variables | Factors are latent "causes" |
| Use case | Dimensionality reduction | Understanding underlying mechanisms |
| Model | Deterministic decomposition | Probabilistic latent variable model |

### 3.5 Applications

- **Engineering:** Latent modes of structural vibration (mode shapes are latent factors)
- **Risk analysis:** Hidden risk factors driving multiple observable risk indicators
- **Survey analysis:** Latent constructs (e.g., "overall quality") underlying multiple survey items
- **Sensor networks:** Latent physical phenomena measured by multiple sensors

**Example in SHM:** Multiple accelerometers measure vibrations caused by a few fundamental vibrational modes. Factor analysis recovers these latent modes.

---

## 4. Multivariate Analysis of Variance (MANOVA)

MANOVA extends ANOVA (Lecture 4) to **multiple dependent variables** simultaneously.

### 4.1 What MANOVA Tests

Instead of testing whether one response differs across groups, MANOVA tests whether a **vector of means** differs across groups.

**Hypotheses:**
$$H_0: \mu_1 = \mu_2 = \cdots = \mu_g$$

where each μ_i is a **vector of means** for all response variables.

$$H_1: \text{At least one group mean vector differs}$$

### 4.2 Model Form

$$Y = XB + E$$

where:
- **Y:** n × r multivariate response matrix (n observations, r responses)
- **X:** n × (g+1) design matrix (group indicators)
- **B:** (g+1) × r coefficient matrix
- **E:** n × r error matrix

### 4.3 Test Statistics

MANOVA uses the **Sum of Squares and Crossproducts (SSCP)** matrices instead of univariate sums of squares.

Common test statistics:

1. **Wilks' Lambda:**
$$\Lambda = \frac{|W|}{|T|}$$
   where W = within-group SSCP, T = total SSCP
   - Small Λ → reject H₀ (groups differ)
   - Large Λ → fail to reject H₀ (no difference)

2. **Pillai's Trace:** Complementary to Wilks' (sum of squared correlations)
3. **Hotelling-Lawley Trace:** Ratio of between to within variance
4. **Roy's Largest Root:** Largest eigenvalue (tests hardest-to-distinguish groups)

### 4.4 When to Use MANOVA

- **Multiple outputs simultaneously:** Compare algorithms on accuracy AND speed simultaneously
- **Correlated responses:** Responses are related (univariate ANOVA treats them independently)
- **Engineering:** Compare designs on displacement + stress + weight simultaneously
- **Experimental design:** Multiple measured outcomes from one experiment

**Example:** In structural optimization, you might measure displacement, stress, and cost for 5 different designs. MANOVA tests whether designs differ significantly across all three metrics jointly.

**Check for Understanding:**
- Why test multiple responses jointly rather than separately with multiple ANOVAs?

---

## 5. Canonical Correlation Analysis (CCA)

CCA measures **relationships between two blocks of variables**.

### 5.1 Problem Setup

Suppose you have:
- **X block:** p variables (e.g., design parameters)
- **Y block:** q variables (e.g., structural responses)

**Goal:** Find linear combinations of X and Y that are maximally correlated.

### 5.2 Canonical Variables

CCA finds combinations:
$$U = a^T X \quad \text{(canonical variate from X block)}$$
$$V = b^T Y \quad \text{(canonical variate from Y block)}$$

**Optimization:**
$$\max \operatorname{Corr}(U,V) = \max \frac{\operatorname{Cov}(a^T X, b^T Y)}{SD(a^T X) \cdot SD(b^T Y)}$$

### 5.3 Canonical Correlations

The first canonical correlation ρ₁ is the maximum achievable correlation. Subsequent correlations ρ₂, ρ₃, ... measure secondary relationships (orthogonal to previous ones).

### 5.4 When to Use CCA

- **Relating two variable sets:** How do input parameters relate to output responses?
- **Feature set fusion:** Integrate two data sources (e.g., multiple sensor modalities)
- **Multimodal learning:** Relate images and text, audio and video, etc.
- **FEM surrogate:** Relate 59 input design parameters to 20 response quantities

**Example in Your Work:** For FEM surrogate modeling with 59 inputs and multiple structural response measures, CCA reveals the strongest relationships between the input space and output space.

---

## 6. Cluster Analysis

Cluster analysis **groups similar observations** without pre-assigned labels (unsupervised learning).

### 6.1 Clustering Objectives

Group samples so that:
- **Within-cluster similarity:** High (samples in same cluster are similar)
- **Between-cluster similarity:** Low (samples in different clusters are dissimilar)

### 6.2 k-Means Clustering

The most popular clustering algorithm minimizes within-cluster variance:

$$\min_{C_1,...,C_k} \sum_{j=1}^{k}\sum_{x_i\in C_j}\|x_i-\mu_j\|^2$$

where:
- **C_j:** Cluster j
- **μ_j:** Center (centroid) of cluster j
- **k:** Number of clusters (must be specified)

**Algorithm:**
1. Initialize k centroids randomly
2. Assign each point to nearest centroid
3. Recompute centroids based on assigned points
4. Repeat steps 2-3 until convergence

### 6.3 Distance Metrics

Different distance measures yield different clusterings:

- **Euclidean:** Standard distance; assumes isotropic clusters
- **Manhattan:** City-block distance; robust to outliers
- **Cosine:** Angle similarity; good for high-dimensional data
- **Mahalanobis:** Accounts for variable correlations

### 6.4 Determining Optimal k

- **Elbow method:** Plot within-cluster variance vs. k; choose where curve bends
- **Silhouette score:** Measure of how well-separated clusters are
- **Domain knowledge:** Often k is determined by problem context

### 6.5 Applications

- **Design family grouping:** Cluster FEM designs into similar families
- **Failure mode discovery:** Group similar failures together
- **GAN synthetic validation:** Do synthetic samples cluster like real data?
- **Structural regimes:** Identify different operational states

**Check for Understanding:**
- If you increase k (number of clusters), what happens to within-cluster variance? When should you stop?

---

## 7. Linear Discriminant Analysis (LDA)

LDA is used for **supervised group separation and classification** when class labels are known.

### 7.1 Core Idea

Find a projection direction that **maximizes class separation**: classes should be far apart, within-class spread should be small.

### 7.2 Fisher's Criterion

Maximize the ratio of between-class to within-class scatter:

$$\max_w \frac{w^T S_B w}{w^T S_W w}$$

where:
- **S_B:** Between-class scatter matrix (covariance of class means)
- **S_W:** Within-class scatter matrix (pooled within-class covariance)

### 7.3 Generalized Eigenvalue Problem

The solution satisfies:
$$S_B w = \lambda S_W w$$

The eigenvectors are the **discriminant directions**. The first discriminant direction (largest eigenvalue) provides maximum class separation.

### 7.4 Classification Rule

For a new observation x:
1. Compute its projection: z = w^T x
2. Compare z to class means (also projected)
3. Assign to nearest class

### 7.5 When to Use LDA

- **Classification:** Predict class membership (defect type, damage state)
- **Dimensionality reduction:** Find most discriminative directions (like PCA but supervised)
- **Interpretability:** Understand which features separate classes
- **Feature extraction:** Create discriminative features for downstream models

**Example in SHM:** Given sensor measurements and known damage states (healthy, minor crack, major crack), LDA finds the directions that best separate these states. New observations can be classified automatically.

**Check for Understanding:**
- How does LDA differ from PCA in terms of what it optimizes?

---

## 8. Multidimensional Scaling (MDS)

MDS maps high-dimensional data into lower dimensions **while preserving pairwise distances** or dissimilarities.

### 8.1 Problem Formulation

Given a distance/dissimilarity matrix D with elements d_ij (distance between observations i and j):

**Goal:** Find low-dimensional representations z₁, z₂, ..., z_n (typically in 2D or 3D) that preserve distances.

**Optimization:**
$$\text{Stress}=\sqrt{\sum_{i<j}(d_{ij}-\|z_i-z_j\|)^2}$$

Minimize stress (residual distance distortion).

### 8.2 Advantages vs. PCA

- **PCA:** Preserves variance; linear transformation
- **MDS:** Preserves pairwise distances; nonlinear; useful for any distance metric

### 8.3 Applications

- **Visualization:** Plot high-dimensional data in 2D to understand geometry
- **Manifold inspection:** Understand the shape and structure of data cloud
- **Similarity maps:** Which designs are most similar?
- **Optimizer behavior:** Map algorithm performance space
- **Design-space exploration:** Visualize how designs cluster and relate

**Example:** For your FEM designs, MDS can create a 2D map where similar designs cluster together, allowing visual exploration of the design space.

---

## 9. Practical Guide: Choosing Multivariate Methods

### 9.1 Quick Decision Guide

| Goal | Method | Use When |
|------|--------|----------|
| Compress p dimensions to k | PCA | Variance compression; visualization |
| Find latent causes | Factor Analysis | Latent factors explain observed variables |
| Test multiple responses | MANOVA | Multiple dependent variables across groups |
| Relate two variable blocks | CCA | Inputs ↔ Outputs; multimodal fusion |
| Find natural groups | Cluster Analysis | Unsupervised grouping; no labels |
| Separate known classes | LDA | Supervised classification; interpretability |
| Visualize distances | MDS | High-dimensional visualization |

### 9.2 Sequential Workflow Example

For FEM surrogate development:
1. **Explore:** Use PCA on 59 inputs → identify key variations
2. **Relate:** Use CCA to relate input principal components to output responses
3. **Cluster:** Use k-means to group similar designs
4. **Classify:** Use LDA if designs have known categories (material types, failure modes)

---

## 10. Practical Use in AI and Engineering

### 10.1 Highest-Value Multivariate Tools

For modern AI and engineering workflows:

1. **PCA:** Feature compression; visualization; multicollinearity handling
2. **CCA:** Input-output block analysis; surrogate development
3. **Cluster analysis:** Design family discovery; operational mode identification
4. **LDA:** Damage classification; defect state prediction
5. **MDS:** Design-space visualization and exploration
6. **MANOVA:** Multi-objective response comparison

### 10.2 Applications

These methods directly support:
- **FEM surrogate compression:** Reduce 59 inputs via PCA
- **SHM sensor fusion:** Combine multiple sensors via CCA or PCA
- **Multi-objective optimization:** Compare designs on multiple responses (MANOVA)
- **GAN latent analysis:** Understand synthetic data structure
- **Structural regime discovery:** Cluster different operational states
- **Feature reduction before ML:** PCA or LDA preprocessing
- **Damage classification:** LDA for SHM state prediction

---

## Key Takeaways

1. **Covariance matrix** is central to multivariate analysis; decomposing it reveals structure and relationships.

2. **PCA** reduces dimensionality by finding maximum-variance directions; useful for compression, visualization, and handling multicollinearity.

3. **Factor Analysis** models observed variables as arising from latent factors; interprets hidden mechanisms.

4. **MANOVA** extends ANOVA to multiple responses; tests whether groups differ jointly across multiple metrics.

5. **CCA** reveals relationships between two blocks of variables; essential for surrogate modeling and multimodal fusion.

6. **Cluster Analysis** groups similar observations unsupervised; useful for design exploration and pattern discovery.

7. **LDA** performs supervised classification by maximizing class separation; combines dimensionality reduction with discrimination.

8. **MDS** preserves pairwise distances in lower dimensions; enables intuitive visualization of high-dimensional geometry.

---

## Connection to Next Modules

Multivariate techniques provide foundations for Lecture 8 (Time-Series Analysis), where you'll apply these methods to sequential data. Clustering and classification from this lecture extend to Lecture 17 (Probabilistic Machine Learning) and Lecture 18 (Reinforcement Learning), where latent variable models become central. Feature reduction via PCA and LDA feeds into all downstream machine learning tasks in later lectures.
