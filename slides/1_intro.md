# Motivation: Linearly Inseparable Data

<br>
<center>
  <figure>
    <img src="/Linearly_inseparable_data_1.png" style="width: 720px; position: absolute">
  </figure>
</center>

---

# Model 1

<br>
<center>
  <figure>
    <img src="/Linearly_inseparable_data_2.png" style="width: 720px; position: absolute">
  </figure>
</center>

---

# Model 2

<br>
<center>
  <figure>
    <img src="/Linearly_inseparable_data_3.png" style="width: 720px; position: absolute">
  </figure>
</center>

---

# Combined Models

<br>
<center>
  <figure>
    <img src="/Linearly_inseparable_data_4.png" style="width: 720px; position: absolute">
  </figure>
</center>

---

# Introduction to Ensemble Learning

<div class="grid grid-cols-[4fr_3fr] gap-30">
<div>
<v-clicks depth="3">

* Ensemble models combine several base learners to improve overall prediction accuracy and robustness
  * Base learners are usually decision trees
    * But why?

* Ensemble methods can reduce variance, handle model bias, and approximate complex relationships better than single models

* Real-world inspiration: the ability to communicate, especially as part of the decision-making process, is a characteristic human trait
  * "Two heads are better than one"
</v-clicks>
</div>
<div>
<br>
<div v-click at="2">
  <figure>
    <img src="/ensemble_of_fantasy_trees.jpeg" style="width: 350px; position: relative">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><span>Image source: Perplexity Pro<br> Prompt: <samp>ensemble of fantasy forest trees playing violins, pipes, and drums, high-quality cartoon, 4K resolution</samp></span>
    </figcaption>
  </figure>
</div>
</div>
</div>

---

# Weak and Strong Base Learners

<br>
<div class="grid grid-cols-[3fr_4fr_3fr] gap-5">
<div>
<div v-click at="2">
  <figure>
    <img src="/strong_and_weak_dog_meme_1.jpg" style="width: 300px; position: relative; border:3px solid #0099ff">
  </figure>
</div>
</div>
<div>
  <figure>
    <img src="/roc_curve.svg" style="width: 350px; position: relative">
  </figure>
</div>
<div>
<div v-click at="3">
  <figure>
    <img src="/strong_and_weak_dog_meme_2.jpg" style="width: 300px; position: relative; border:3px solid #ff9900">
  </figure>
</div>
</div>
</div>

<div v-click at="4">
    <Arrow x1="303" y1="150" x2="410" y2="230" width=1 color="#0099ff"/>
    <Arrow x1="677" y1="200" x2="603" y2="190" width=1 color="#ff9900"/>
<div style="color:#b3b3b3ff; font-size: 11px;">Images sources:<br> Strong and weak dog meme (left and right)</div>
<div style="color:#b3b3b3ff; font-size: 11px;"><a href="https://upload.wikimedia.org/wikipedia/commons/1/13/Roc_curve.svg">https://upload.wikimedia.org/wikipedia/commons/1/13/Roc_curve.svg</a>(center)</div>
</div>

---

# Weak and Strong Base Learners

* Let:
  * $\textcolor{green}{\delta} \in (0, 1)$ be **tolerance of uncertainty**
  * $\textcolor{orange}{\epsilon} \in (0, 1)$ be **tolerance in inaccuracy**
  * $\textcolor{violet}{\gamma} \in (0, \frac{1}{2})$ be **achievable improvement in accuracy**
<br>

* Then for any choice of $\textcolor{green}{\delta}$, $\textcolor{orange}{\epsilon}$, we have some $m = m_{\mathcal H}(\textcolor{green}{\delta}, \textcolor{orange}{\epsilon}) \in \N$ such that,<br> after learning on
at least $m$ i.i.d. samples from the true distribution, we have:

<br>
<div class="grid grid-cols-[3fr_3fr] gap-5">

<div>
<center>

### for **weak learner**

</center>
<br>

#### $\mathbb{P} \big(\mathrm{Accuracy}(f) > \textcolor{violet}{\frac{1}{2} + \gamma}\big) \geq \textcolor{green}{1 - \delta}$
</div>

<div>
<center>

### for **strong learner**
</center>
<br>

#### $\mathbb{P} \big(\mathrm{Accuracy}(f) > \textcolor{orange}{1 - \epsilon}\big) \geq \textcolor{green}{1 - \delta}$
</div>
</div>
<br>

##### Based on [https://cs229.stanford.edu/cs229-notes-decision_trees.pdf](https://cs229.stanford.edu/cs229-notes-decision_trees.pdf)

---

# Hidden Strength of Weak Learners

* A set of weak learners can be combined into a strong learner

* This idea is used in 3 approaches:
  * Bagging
    * Random Forest
  * Boosting
  * Stacking

---

# Why Trees?

* Trees are:
  * Flexible models
  * Computationally feasible w.r.t other flexible models
  * Can handle qualitative and quantitative predictors at once