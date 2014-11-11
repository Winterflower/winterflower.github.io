---
layout: post
title: Introductory Tutorial
---
Prerequisites:

* Scikit-learn
* the [Tutorial kit] (https://github.com/Winterflower/mockmail-intro/releases/tag/v1.0)

1. Download the Tutorial kit and unzip it
2. Change to the root of the Tutorial kit directory
3. Open the the file introductory_tutorial.py using your favorite text editor
(I quite like [Atom](https://atom.io/), but you are welcome to use anything you like)


##1. Importing required modules
For this tutorial, we will require three modules

1. numpy
2. sklearn.naive_bayes
3. text_adapter

Import these into your Python script.

```python
import numpy as np  #too lazy to type numpy every time
import text_adapter
from sklearn.naive_bayes import BernoulliNB
```
In the last statement we choose to import only
the class BernoulliNB, because we will not be
needing the other `sklearn.naive_bayes` classes.

##2. Preprocessing the training data for our classifier
The training data (the HAM and SPAM emails)  have been
provided for you in the script.

```python
spam_emails=["Hello send your password", "hello please click link", "click link",
"your password here", "send password"]
ham_emails=["hello reset your password", "password email", "warm hello" ]
```


Now, our goal is to
process the data into a format accepted by the BernoulliNB
class.

If we navigate to the [documentation for BernoulliNB]
(http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.BernoulliNB.html)
we can see that the _fit_ function accepts as an argument a Numpy matrix where
each sample is represented by a row and a Numpy array of class labels (let's
denote SPAM by 0 and HAM by 1).

1. Concatenate the SPAM and HAM emails into one list for easy processing

```python
training_data=spam_emails+ham_emails
```

2. Create a one-dimensional Numpy array of class labels

We have to assign a class label (0 or 1) to each email in the `training_data`
list we created above. An easy way to do this is to use a mixture
of repetition and list concatenation. An example is shown below, but
you are welcome to come up with your own.

```python
training_labels=np.array([1]*len(ham_emails)+[0]*len(spam_emails))
```

3. Create a a Numpy feature matrix

To save time, you can create the feature matrix using the
`create_scikit_matrix` method in the module _text_adapter_.
Unless you used a different import statement, do
not forget to tell Python that the method `create_scikit_matrix`
lives in the module _text_adapter_. Do this by calling the method as follows
`text_adapter.create_scikit_matrix`. If you leave, the _text_adapter_ part
out of the method call, Python will be unhappy.

```python
training_data_matrix=text_adapter.create_scikit_matrix(training_data)
```


##3. Creating and training our classifier
Now comes the really fun part! So far, we have
preprocessed our data and we have a Numpy matrix with our
feature_vectors and a Numpy array with class labels. We are ready to
do some machine learning!

1. First, let's use the Scikit-learn API to create an instance
of the BernoulliNB class. For future use, we will store
this object in a variable called `classifier`

```python
classifier=BernoulliNB()
```
2. Next, train the classifier by using our training data and training class labels.
```python
classifier.fit(training_data_matrix, training_labels)
```
Hooray!
3. Test your classifier on a new incoming email

```python
test_email="hello please send password"
```

To compute the posterior probability that this email is spam,
we have to first convert it to a feature vector. As a reminder,
we are using the words that occur in the training data set as our features.
If the word is present in the email, we give it a value of 1. Else
the value is 0.

```python
#compute the dictionary of words based on the training set we defined above
test_feature_vector=np.array(text_adapter.binary_feature_vector_builder(training_dictionary,test_email))
```

In Scikit-learn, probability of a datapoint belonging to a certain
class (in our case HAM or SPAM) can be calculated by calling
the method `predict_proba()` and passing the feature vector as an argument.

```python
print classifier.predict_proba(test_feature_vector)
```

If you execute the statement above, you will notice that it returns
two probabilities. The first probability is for class 0 (SPAM) and
the second for class 1 (HAM).
