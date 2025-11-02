# What is Gradient Boosting?

* **Gradient boosting** is a boosting technique<br> that directly optimizes a ***differentiable loss function*** using ***gradient descent***

* Rather than simply reweighting data points, as in AdaBoost, gradient boosting fits new models to the gradient (direction of steepest descent) of the loss function with respect to the current model's output

* In each round, a new weak model is trained to predict the **residual errors** from the ensemble so far, and these predictions are added to the existing ensemble to improve performance

* Gradient boosting is more flexible, can use various types of loss functions, and is less sensitive to noise than some other boosting methods

---

# Gradient Boosting Decision Trees

<div v-click at="1">

* Use additive model to train sequentially:
  * Start from constant prediction, **add a new decision tree** $f_i$ **each time**:
</div>

<div v-click at="2">

$$\hat{y}_i^{(0)} = 0\\\\[6pt]$$
</div>
<div v-click at="3">

$$\hat{y}_i^{(1)} = f_1 (x_i) = \hat{y}_i^{(0)} + f_1 (x_i)\\\\[6pt]$$
</div>

<div v-click at="3">

$$\hat{y}_i^{(2)} = f_1 (x_i) + f_2 (x_i) = \hat{y}_i^{(1)} + f_2 (x_i)\\\\[6pt]$$
</div>

<div v-click at="4">

$$...\\\\[6pt]$$
</div>

<div v-click at="5">

$$\hat{y}_i^{(t)} = \sum_{k=1}^t f_k (x_i) = \hat{y}_i^{(t-1)} + f_t (x_i)$$
</div>

<div v-click at="6">
<br>

##### Slides 27-36, 38 are based on [https://web.stanford.edu/class/cs246/slides/15-dt.pdf](https://web.stanford.edu/class/cs246/slides/15-dt.pdf)
</div>

---

# How to Decide which $f$ to Add?

* Prediction at round $t$ is: $\hat{y}_i^{(t)} = \hat{y}_i^{(t-1)} + f_t (x_i)$
  * Where we need to decide what $f_t$ to add

* Goal: find tree $f_t$ that minimizes loss $\ell$:
$$\mathrm{obj}^{(t)} = \sum_{i=1}^n \ell \big( y_i, \hat{y}_i^{(t)} \big) + \omega (f_t),$$

#### where:
* $y_i$ is the ground-truth label
* $\hat{y}_i^{(t-1)} + f_t (x_i)$ is the prediction made at round $t$
* $\omega (f_t)$ is the model complexity

---

# How to Decide which $f$ to Add?
<center>

### $\mathrm{obj}^{(t)} = \sum_{i=1}^n \ell \big( y_i, \hat{y}_i^{(t)} \big) + \omega (f_t)$
</center>
<br>
<v-clicks depth="2">

* Take Taylor expansion of the objective:
  * $g(x + \Delta) \approx g(x) + g^{\prime}(x)\Delta + \frac{1}{2} g^{\prime\prime}(x)\Delta^2$

* So, we get the approximate objective:
</v-clicks>

<v-click at="3">

$$\mathrm{obj}^{(t)} = \sum_{i=1}^n \bigg[ \ell \big( y_i, \hat{y}_i^{(t)} \big) + g_i f_t (x_i) + \frac{1}{2} h_i f_t^2 (x_i) \bigg] + \omega (f_t),$$
</v-click>

<v-click at="4">

#### where:
* $g_i = \partial_{\hat{y}^{(t-1)}} \ell \big( y_i, \hat{y}_i^{(t-1)} \big)$
* $h_i = \partial_{\hat{y}^{(t-1)}}^2 \ell \big( y_i, \hat{y}_i^{(t-1)} \big)$
</v-click>

---

# Find $f_t$ among Trees

* New goal: find tree $f_t$ that:
$$\mathrm{obj}^{(t)} = \sum_{i=1}^n \bigg[ g_i f_t (x_i) + \frac{1}{2} h_i f_t^2 (x_i) \bigg] + \omega (f_t)$$

* Why spend so much efforts to derive the objective, why not just grow trees?
  * **Theoretical benefit**: know what we are learning
  * Engineering benefit:
    * $g$ and $h$ comes from definition of loss function
    * Learning $f_t$ only depends on the objective via $g$ and $h$
    * We can now directly learn trees that optimize the loss
      * rather than using some heuristic procedure

---

# Define a Tree

* Every leaf $j$ have a weight $w_j$
  * We will predict $w_j$ for any data belongs to leaf $j$

<br>
<div class="grid grid-cols-[2fr_3fr]">
<div>
    <figure>
    <img src="/Gradient_Boosting_Tree.png" style="width: 220px !important">
    </figure>
</div>
<div>

#### $f_t (x) = w_{q(x)},$
<br>

##### where $q(x)$ indicate the leaf node that data point $x$ belongs to

<div style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://web.stanford.edu/class/cs246/slides/15-dt.pdf">https://web.stanford.edu/class/cs246/slides/15-dt.pdf</a>
    </div>
</div>
</div>
<br>

* Define complexity of tree $f$ as: $~~\Omega (f) = \gamma \cdot T + \frac{1}{2} \lambda \sum\limits_j^T w_j^2,$

#### where:
* $T$ is number of leaves of tree $f$
* $\gamma$ is cost adding a leaf to the tree $f$

---

# Revisiting the Objective

#### **Define:**

* The set of examples in the leaf $j$:
#### $I_j = \big\{ i ~|~ q(x_i) = j \big\},$

#### where $q(x)$ denotes the leaf that data point $x$ belongs to
<br>

* The parameters that depend on the loss:
#### $G_j = \sum_{i \in I_j} g_i$
#### $H_j = \sum_{i \in I_j} h_i$
<br>

* Then the objective function becomes:
$$\mathrm{obj}^{(t)} = \sum_{j=1}^T \bigg[ G_j w_j + \frac{1}{2} (H_j + \lambda) w_j^2 \bigg] + \gamma T$$

---

# How to Find a Single Tree $f_t$?

#### Given a tree $f_t$, we know how to:

* Calculate the score for $f$:
$$\mathrm{obj} = - \frac{1}{2} \sum_{j=1}^T \frac{G_j^2}{H_j + \lambda} + \gamma T$$
<br>

* And then set optimal weights for the chosen $f$:
$$w_j^{\ast} = -  \frac{G_j}{H_j + \lambda}$$
<br>

#### In principle we could:
* Enumerate possible tree structures $f$ and take the one that minimizes $\mathrm{obj}$

---

# How to Find a Single Tree $f_t$?

* In practice we grow tree greedily:
  * Start with tree with depth $0$
  * For each leaf node in the tree, try to add a split
  * The change of the objective after adding a split is:
$$\mathrm{gain} = \frac{1}{2} \bigg[ \underset{\textcolor{grey}{\mathrm{score~of~left~child}}}{\frac{G_L^2}{H_L + \lambda}} +
\underset{\textcolor{grey}{\mathrm{score~of~right~child}}}{\frac{G_R^2}{H_R + \lambda}} - 
\underset{\textcolor{grey}{\mathrm{score~if~we~do~not~split}}}{\frac{(G_L + G_R)^2}{H_L + H_R + \lambda}} \bigg] - \gamma$$
  * Take the split that gives best gain

<br>

* Next: how to find the best split?

---

# How to Find the Best Split?
<br>

* **For each node, enumerate over all features**
  * For each feature, sort the instances by feature value
  * Use a linear scan to decide the best split along that feature
  * Take the best split solution along all the features

* **Pre-stopping**:
  * Stop split if the best split have negative gain
  * But maybe a split can benefit future splits
* **Post-Prunning**:
  * Grow a tree to maximum depth, recursively prune all the leaf splits with negative gain

---

# Summary: GBDT Algorithm

* Add a new tree $f_t (x)$ in each iteration
  * Compute necessary statistics for our objective
    * $g_i = \partial_{\hat{y}^{(t-1)}} \ell \big( y_i, \hat{y}_i^{(t-1)} \big)$
    * $h_i = \partial_{\hat{y}^{(t-1)}}^2 \ell \big( y_i, \hat{y}_i^{(t-1)} \big)$
  * Greedily grow the tree that minimizes the objective:
  $\mathrm{obj} = - \frac{1}{2} \sum\limits_{j=1}^T \frac{G_j^2}{H_j + \lambda} + \gamma T$
* Add $f_t (x)$ to our ensemble model  $y^{(t)} = y^{(t-1)} + \epsilon f_t (x_i),$

  #### where $\epsilon$ is called step-size or shrinkage, usually set around `0.1`
  * Its goal is to prevent overfitting
* Repeat until we use $M$ ensemble of trees

---
zoom: 1.2
---

# Gradient Boosting Realizations
<br>

* [XGBoost](https://xgboost.readthedocs.io/) - eXtreme Gradient Boosting (2014)

* [LightGBM](https://github.com/microsoft/LightGBM) - Light Gradient-Boosting Machine (2016)

* [CatBoost](https://catboost.ai/)

---

# XGBoost

* XGBoost is a highly scalable implementation of gradient boosted decision trees with regularization


#### Widely used by data scientists and provides state-of-the-art results on many problems!

* System optimizations:
  * **Parallel tree constructions** using column block structure
  * **Distributed computing** for training very large models using a cluster of machines
  * **Out-of-core computing** for very large datasets that don’t fit into memory