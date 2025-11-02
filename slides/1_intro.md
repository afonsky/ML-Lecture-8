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

* Ensemble methods can reduce variance, handle model bias, and approximate complex relationships better than single models

* Real-world inspiration: the ability to communicate, especially as part of the decision-making process, is a characteristic human trait
  * ["Two heads are better than one"](https://en.wiktionary.org/wiki/two_heads_are_better_than_one)
  * [“Wisdom of the crowd”](https://en.wikipedia.org/wiki/Wisdom_of_the_crowd)
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

# Why Trees as Base Learners?

<center>
<figure>
  <img src="/int_vs_flex.png" style="width: 430px; position: relative">
  <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Based on  <a href="https://www.statlearning.com/">ISLP Fig. 2.7</a>
</figcaption>
</figure>
</center>

* Trees are:
  * Still being interpreted, but a fairly flexible models
  * Computationally feasible w.r.t other flexible models
  * Can handle qualitative and quantitative predictors at once


---

# Strong and Weak Base Learners

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

# Strong and Weak Base Learners

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

<div class="grid grid-cols-[4fr_3fr] gap-30">
<div>

#### A set of weak learners can be combined into a strong learner
<br>
  <figure>
    <img src="/gravity_falls_gnomes.jpg" style="width: 230px; position: relative">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
    Gravity Falls, animated television series created by Alex Hirsch for Disney Channel and Disney XD (2012-2016)
    </figcaption>
  </figure>
</div>
<div>

<br>

#### This idea is used in the following approaches

<br>
<center>

```mermaid {securityLevel: 'loose', theme: 'neutral', scale: 1.0, flowchart: {'htmlLabels': true}}
graph TD

A(<p style="width:200px;height:25px;">Ensemble Models</p>) --> B(<p style="width:70px;height:25px;">Bagging</p>)
A --> C(<p style="width:70px;height:25px;">Boosting</p>)
B --> D(<p style="width:70px;height:50px;">Random<br>Forests</p>)
A --> F(<p style="width:70px;height:25px;">Stacking</p>)
```
</center>
</div>
</div>