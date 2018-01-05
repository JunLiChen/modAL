# modAL
Modular Active Learning framework for Python3

## Page contents
- [Introduction](#introduction)  
- [Active learning from bird's-eye view](#active-learning)  
- [modAL in action](#modAL-in-action)
  - [From zero to one in a few lines of code](#initialization)  
  - [Replacing parts quickly](#replacing-parts)  
  - [Replacing parts with your own solutions](#replacing-parts-with-your-own-solutions)  
  - [Working examples](#working-examples)  
- [About the developer](#about-the-developer)

## Introduction<a name="introduction"></a>
modAL is an active learning framework for Python3, designed with *modularity, flexibility* and *extensibility* in mind. Built on top of scikit-learn, it allows you to rapidly create active learning workflows with nearly complete freedom. What is more, you can easily replace parts with your custom built solutions, allowing you to design novel algorithms with ease.

## Active learning from bird's-eye view<a name="active-learning"></a>
Let's take a look at a general active learning workflow!

![](img/active_learning.png)

The key components of any workflow are the **model** you choose, the **uncertainty** measure you use and the **query** strategy you apply to request labels. With modAL, instead of choosing from a small set of built-in components, you have the freedom to seamlessly integrate scikit-learn or Keras models into your algorithm and easily tailor your custom query strategies and uncertainty measures.

## modAL in action<a name="modAL-in-action"></a>
Let's see what modAL can do for you!

### From zero to one in a few lines of code<a name="initialization"></a>
Active learning with a scikit-learn classifier, for instance RandomForestClassifier, can be as simple as the following.
```python
from modAL.models import ActiveLearner
from sklearn.ensemble import RandomForestClassifier

# initializing the learner
learner = ActiveLearner(
    predictor=RandomForestClassifier(),
    X_initial=X_train, y_initial=y_train
)

# query for labels
query_idx, query_inst = learner.query(X_pool)

# ...obtaining new labels from the Oracle...

# supply label for queried instance
learner.teach(X_pool[query_idx], y_new)
```

### Replacing parts quickly<a name="replacing-parts"></a>
If you would like to use different uncertainty measures and query strategies than the default uncertainty sampling, you can either replace them with several built-in strategies or you can design your own by following a few very simple design principles. For instance, replacing the default uncertainty measure to classification entropy looks the following.
```python
from modAL.models import ActiveLearner
from modAL.uncertainty import entropy_sampling
from sklearn.ensemble import RandomForestClassifier

learner = ActiveLearner(
    predictor=RandomForestClassifier(),
    query_strategy=entropy_sampling,
    X_initial=X_train, y_initial=y_train
)
```

### Replacing parts with your own solutions<a name="replacing-parts-with-your-own-solutions"></a>
modAL was designed to make it easy for you to implement your own query strategy. For example, implementing and using a simple random sampling strategy is as easy as the following.
```python
import numpy as np

def random_sampling(classifier, X_pool):
    n_samples = len(X_pool)
    query_idx = np.random.choice(range(n_samples))
    return query_idx, X_pool[query_idx]

learner = ActiveLearner(
    predictor=RandomForestClassifier(),
    query_strategy=random_sampling,
    X_initial=X_train, y_initial=y_train
)
```
For more details on how to implement your custom strategies, visit the page [Extending modAL](Extending-modAL)!

### Working examples<a name="working-examples"></a>
To see modAL in *real* action, visit the examples section!  
- [Pool-based sampling](Pool-based-sampling)  
- [Stream-based sampling](Stream-based-sampling)  
- [Active regression](Active-regression)  
- [Query by committee](Query-by-committee)  
- [Bootstrapping and bagging](Bootstrapping-and-bagging)  
- [Keras integration](Keras-integration)

## About the developer<a name="about-the-developer">
modAL is developed by me, [Tivadar Danka](https://www.tivadardanka.com) (aka [cosmic-cortex](https://github.com/cosmic-cortex) in GitHub). I have a PhD in pure mathematics, but I fell in love with biology and machine learning right after I finished my PhD. I have changed fields and now I work in the [Bioimage Analysis and Machine Learning Group of Peter Horvath](http://group.szbk.u-szeged.hu/sysbiol/horvath-peter-lab-index.html), where I am working to develop active learning strategies for intelligent sample analysis in biology. During my work I realized that in Python, creating and prototyping active learning workflows can be made really easy and fast with scikit-learn, so I ended up developing a general framework for this. The result is modAL :) If you have any questions, requests or suggestions, you can contact me at <a href="mailto:85a5187a@opayq.com">85a5187a@opayq.com</a>! I hope you'll find modAL useful!
