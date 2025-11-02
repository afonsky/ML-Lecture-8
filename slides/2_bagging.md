# What is Bagging?

#### <span style="color: red;">**Bagging**</span> = <span style="color: red;">**B**</span>ootstrap + <span style="color: red;">**agg**</span>regat<span style="color: red;">**ing**</span>


<div class="grid grid-cols-[2fr_3fr] gap-50">
<div>
<br>
  <figure>
    <img src="/Ensemble_models-Bagging.drawio.svg" style="width: 420px; position: absolute">
  </figure>
</div>

<div>

#### Recall: **bootstrap** is resampling with replacement
<br><br><br><br><br>

##### $\hat{f}^{\ast 1} (x), \hat{f}^{\ast 2} (x), \hat{f}^{\ast 3} (x), ... \hat{f}^{\ast B} (x)$

<br>

* Regression: averaging<br> $\hat{f}_{bag} (x) = \frac{1}{B} \sum\limits_{b=1}^B \hat{f}^{\ast b} (x)$
* Classification: majority vote<br>(most commonly occurring class)
</div>
</div>

---

# Bagging and Bias-Variance Trade-off

<div class="grid grid-cols-[2fr_3fr] gap-10">
<div>
  <figure>
    <img src="/Bias-Variance_4_targets.png" style="width: 260px; position: relative">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
    <a href="https://scott.fortmann-roe.com/docs/BiasVariance.html">https://scott.fortmann-roe.com/docs/BiasVariance.html</a>
    </figcaption>
  </figure>
</div>
<div>
  <figure>
    <img src="/Bias_and_variance_contributing_to_total_error.svg" style="width: 420px; position: relative">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:<br>
    <a href="https://commons.wikimedia.org/wiki/File:Bias_and_variance_contributing_to_total_error.svg">https://commons.wikimedia.org/wiki/File:Bias_and_variance_contributing_to_total_error.svg</a>
    </figcaption>
  </figure>
</div>
</div>
<br>

* By averaging the predictions of multiple models, **bagging** <span v-mark="{ at: 2, color: 'rgba(233, 159, 0, 1)', type: 'underline' }">reduces the model’s variance</span>, making it less likely to overfit
  * But does not significantly reduce bias if the individual models are weak


---

# Out-of-Bag Error Estimation in Bagging

#### The **Out-of-Bag (OOB)** set is all data not chosen in the bootstrap process 

<br>
  <figure>
    <img src="/Ensemble_models-Out-of-Bag.drawio.svg" style="width: 620px; position: relative">
  </figure>

<br>

* OOB in bagging ensembles:
  * Offers a nearly unbiased estimate of generalization error when using enough trees
  * No need for a separate validation set or cross-validation, saving computational resources
  * Commonly used for model evaluation, hyperparameter tuning, and feature selection
    * Particularly efficient for large datasets

---

# Variable Importance Measures

<br>
<div class="grid grid-cols-[3fr_2fr] gap-5">
<div>
<v-clicks depth="3">

* Decision trees are good enough for interpretation
  * In **Bagging**, when combining a large number of trees, this ability decreases
  * There is a workaround:<br> **overall summary of the importance<br> of
each predictor** using
    * **RSS** (for bagging regression trees)
    * **Gini index** (for bagging classification trees)
</v-clicks>
</div>
<div>
<div v-click at="6">
<figure>
<img src="/ISLP_figure_8.9.svg" style="width: 400px !important">
<figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
  <a href="https://www.statlearning.com/">ISLP Fig. 8.9</a>
</figcaption>
</figure>
</div>
</div>
</div>

<div v-click at="7">

##### The figure shows a variable importance plot for the `Heart` data<br> Variable importance is computed using the mean decrease in Gini index, and expressed relative to the maximum
</div>

---

# Random Forests

* [Random Forests](https://link.springer.com/article/10.1023/a:1010933404324) (L. Breiman, 2001) is an improvement over bagging - it ***decorrelates*** the trees
  * Each time a split in a tree is considered, a ***random sample*** of $m$ predictors is chosen<br> as split candidates from the full set of $p$ predictors 
<br>
  <figure>
    <img src="/RF_1.png" style="width: 650px; position: relative">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><span>Image source: <a href="https://aiml.com/what-is-random-forest-2">https://aiml.com/what-is-random-forest-2</a></span>
    </figcaption>
  </figure>

---

# Hyperparameters of Bagging and Random Forests

<br>
<div class="grid grid-cols-[2fr_2fr] gap-10">
<div>
<v-click at="1">

#### Optimized hyperparameters:
</v-click>
<v-clicks depth="2">

* Both Bagging and Random Forests:
  * **Number of trees**
  * Number of samples in bootstrap sets
  * Maximum depth of the trees and all other parameters of decision trees

* Random Forests:
  * $m$ - size of the random subset of features
  * In the case of $m=p$, we obtain Bagging
</v-clicks>
</div>
<div v-click at="2">
<figure>
<img src="/ISLP_figure_8.8.svg" style="width: 400px !important">
<figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
  <a href="https://www.statlearning.com/">ISLP Fig. 8.8</a>
</figcaption>
</figure>
</div>
</div>

---

# Performance of Bagging and Random Forests

<br>
<div class="grid grid-cols-[2fr_2fr] gap-10">
<div>

* Using a small value of $m$ in building a random forest will typically be helpful<br> when we have a large number of (possibly) correlated predictors $p$
  * Good defaults of hyperparameters for RF:
    * $m=\sqrt{p}$ (RF with classification trees)
    * $m=\frac{1}{3}$ (RF with regression trees)
* Again, in the case of $m=p$,<br> we obtain Bagging
</div>
<div>

<figure>
<img src="/ISLP_figure_8.10.svg" style="width: 400px !important">
<figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
  <a href="https://www.statlearning.com/">ISLP Fig. 8.10</a>
</figcaption>
</figure>
</div>
</div>
<br>

##### Further reading: [Breiman, L. (2001). Random forests. Machine learning, 45(1), 5-32](https://link.springer.com/article/10.1023/a:1010933404324)