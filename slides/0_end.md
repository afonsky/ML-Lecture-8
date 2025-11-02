# Conclusions
<v-clicks depth="3">

* We can combine multiple weak learners (classifiers or regressors)
    * Hopefully, each learner makes different mistakes

* Two ways so far to obtain different ensemble models from the same family:
	* Independent training using randomized dataset and/or training
		* **Bagging**, **random forests**, extremely randomized trees
	* Add learners that focus on examples ensemble gets wrong
		* **Boosting**
	* Both typically use a large amount of learners

* **Gradient boosting** is a boosting technique<br> that directly optimizes a ***differentiable loss function*** using ***gradient descent***

* We combine learners of different types using **stacking**
</v-clicks>

---
layout: end
hideInToc: true
---