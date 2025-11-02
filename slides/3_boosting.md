# What is Boosting?

#### **Boosting** is yet another approach for improving the predictions resulting from a decision tree

<div class="grid grid-cols-[3fr_3fr] gap-20">
<div>
<br>
  <figure>
    <img src="/Ensemble_models-Boosting.drawio.svg" style="width: 430px; position: relative">
  </figure>
</div>

<div>

#### How does boosting works:
<div class="bg-orange-100">

1. Train weak learners sequentially
1. Adjust weights
1. Aggregate predictions
</div>
<br>

* Data reweighting is based on residuals
* Final classifier is weighted sum of the individual classifiers
</div>
</div>
<br>

<div class="grid grid-cols-[3fr_3fr] gap-70">
<div>

#### Advantages of boosting:
<div class="bg-green-100">

* Reduces both bias and variance
* Works well with weak learners
</div>
</div>

<div>

#### Drawbacks of boosting:
<div class="bg-red-100">

* Sensitive to outliers
* Computationally expensive
</div>
</div>
</div>

---

# Hyperparameters of Boosting

<div class="grid grid-cols-[3fr_4fr] gap-20">
<div>

* The number of trees $B$

* The shrinkage parameter $\lambda$<br> (a small positive number)
  * Controls the rate<br> at which boosting learns

* The number $d$ of splits in each tree<br> (a.k.a. *interaction depth*)
  * Controls the complexity<br> of the boosted ensemble
</div>
<div>
<br>
  <figure>
  <img src="/ISLP_figure_8.11.svg" style="width: 500px !important">
  <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
    <a href="https://www.statlearning.com/">ISLP Fig. 8.11</a>
  </figcaption>
  </figure>
</div>
</div>

---

# Boosting with AdaBoost

#### <span style="color: red;">**AdaBoost**</span> = <span style="color: red;">**Ada**</span>ptive + <span style="color: red;">**Boost**</span>ing


<div class="grid grid-cols-[7fr_3fr]">
<div>
<br>

* Let $Y = \pm 1$, $G(X)$ is a classifier on $X_{N \times p}$
* Model yields a sequence of weak classifiers, $G_m (X)$
* Prediction:
	* $\hat{Y} | X := \mathrm{sign}(\sum\limits_{m=1}^M \alpha_m G_m(X))$
		* This is an additive expansion of simple basis functions
* Model weights, $\alpha_m$:
	* $\alpha_m$ are learnt and give higher weight to better models
* Observation weights, $w_i$:
	* Each $G_m$ increases weights for misclassifications in $G_{m-1}$
</div>
<div>
<br>
    <figure>
    <img src="/ESL_figure_10.1.png" style="width: 400px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=357">ESL Fig. 10.1</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Stumps as Weak Learners for AdaBoost

<div class="grid grid-cols-[3fr_3fr] gap-20">
<div>

#### Stump is 1-level decision tree ($d = 1$)
<br>
<center>

```mermaid {securityLevel: 'loose', theme: 'neutral', scale: 1.0, flowchart: {'htmlLabels': true}}
graph TD

A(<p style="width:70px;height:25px;">Root</p>) --> B(<p style="width:70px;height:25px;">Leaf 1</p>)
A --> C(<p style="width:70px;height:25px;">Leaf 2</p>)
style B fill:#77cc46ff,stroke:#37661bff,stroke-width:4px
style C fill:#77cc46ff,stroke:#37661bff,stroke-width:4px
```
</center>

* A decision boundary based on one feature
* Decision stump is a weak learner
* Building blocks of AdaBoost algorithm

<br>

</div>
<div>
  <figure>
    <img src="/AdaBoost_stump.jpg" style="width: 350px; position: relative border:3px">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><span>Image source: Perplexity Pro<br> Prompt: <samp>fantasy style cartoon tree stump, high quality, 4K resolution</samp></span>
    </figcaption>
  </figure>
</div>
</div>

---

# Build Decision Trees with AdaBoost

* Suppose we have training data $\{(x_i, y_i)\}_{i=1}^N, y_i \in \{1, -1\}$

* Initialize equal weights for all observations: $w_i = 1 / N$

* At each iteration $t$:
  1. **Train a stump** $G_t$ using data weighted by $w_i$
  1. **Compute the misclassification error** adjusted by $w_i$
  1. **Compute the weight of the current tree** $\alpha_t$
  1. **Reweight each observation** based on chosen quality metrics (like prediction accuracy)

---

# AdaBoost.M1 Algorithm (Freund and Schapire, 1997)
<div class="bg-orange-100">
<v-clicks depth="3">

1. Let weights $w_i := \frac{1}{N}$, $i = 1:N$
2. For $m$ in $1$ to $M$:
	1. Fit classifier $G_m$ using previous weights $w_i$
	2. Compute model error, weighted misclassification, $\mathrm{err}_m := \frac{\sum_i w_i \cdot I (y_i \neq G_m(x_i))}{\sum_i w_i}$
	3. Compute model weight, $\alpha_m := \log \frac{1 - \mathrm{err}_m}{\mathrm{err}_m}$
		* This $\log$ value minimizes the expected exponential loss
	4. Update observation weights, $w_i \leftarrow w_i \cdot e^{\alpha I (y_i \neq G_m(x_i))}$, $\forall i$
		* no update for correct predictions or $\alpha_m = 0 \Leftrightarrow \mathrm{err}_m = 0.5$
		* increase by $e^{\alpha_m} > 1$ for misclassifications with $\alpha_m > 0 ~\Leftrightarrow~ \mathrm{err}_m > 0.5$
		* increase by $e^{\alpha_m} \in [0,1]$ for misclassifications with $\alpha_m < 0 ~\Leftrightarrow~ \mathrm{err}_m < 0.5$
3. Return $G(x) := \mathrm{sign}[\sum_m \alpha_m G_m(x)]$
</v-clicks>
</div>

##### Note: $w_i$ weights (for training observations) are discarded and not used in model inferencing
---

# AdaBoost Ex. on Simulated Data

<div class="grid grid-cols-[2fr_2fr]">
<div>
<br>
<br>

#### Comparison of test error rates:
* A single stump: 48.5%
* Single large classification tree (244 nodes): 24.7%
* AdaBoost after 400 iterations: 5.8%
</div>
<div>
    <figure>
    <img src="/ESL_figure_10.2.png" style="width: 380px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=359">ESL Fig. 10.2</a>
    </figcaption>
  </figure>
</div>
</div>
<br>

#### AdaBoost: “best off-the-shelf classifier in the world” (1996)

---
zoom: 1.1
---

# AdaBoost: Summary

<br>

* Iteratively train weak learners (decision stumps) to form a strong model:
  * Trees with high accuracy are given more weights in the final model
  * Misclassified data get higher weights in the next iteration

* AdaBoost is the equivalent to *additive training with the **exponential loss***
  * See [Friedman, J., Hastie, T., & Tibshirani, R. (2000). Additive logistic regression: a statistical view of boosting. The annals of statistics, 28(2), 337-407.](https://projecteuclid.org/journals/annals-of-statistics/volume-28/issue-2/Additive-logistic-regression--a-statistical-view-of-boosting-With/10.1214/aos/1016218223.full)