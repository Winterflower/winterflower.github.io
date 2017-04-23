---
layout: post
title: Exploring the Perceptron
---

_I used Gaston Sanchez' very helpful ["Mathjax with Jekyll](http://gastonsanchez.com/visually-enforced/opinion/2014/02/16/Mathjax-with-jekyll/)" post to write up the mathematics in this post. If you are writing a mathematics-heavy post, you may also want to look at the ["Jekyll Extras" documentation](https://jekyllrb.com/docs/extras/) and at Fong Chun Chan's ["R Markdown to Jekyll: 'Protecting Your Math Equations'"](http://tinyheero.github.io/2015/12/06/rmd-to-jekyll-protect-eqn.html)_

_As another side note: this is by no means amazing production-ready code, most of this is just exploring and rambling, so please take it as such! Now on to the perceptron!_

The Rosenblatt Perceptron was one of the first algorithms to allow computer programs to learn from data and use the learned "knowledge" to classify previously unseen examples. It was based on an early model of the neuron - the McCullock-Pitts(MCP) Neuron. The MCP neuron receives input data at its dendrites, combines this input data into a value ( we will come back to this part later ) and then fires a signal if a certain threshold has been exceeded. 

A similar idea is echoed in the Rosenblatt Perceptron. The algorithm receives a set of input data \\(\mathbf{x}\\). We then feed a linear combination, \\(z\\), of the input data \\(\mathbf{x}\\) and some weight vectors \\(\mathbf{w}\\) into an activation function \\(\phi(z)\\). If \\(\phi(z)>\theta\\), the neuron 'fires' and we classify the training data into class 1. If the activation function does not exceed the threshold \\(\theta\\), we classify the input example as -1. These class labels do not necessarily correspond to any 'real world' concepts of 1 and -1, but will prove useful later, when we examine how the perceptron algorithm learns from data. 

One of the obvious questions arising from this is 
    'how do we know the appropriate values to choose for \\(\theta\\) and \\(\mathbf{w}\\)?'.

Provided we can supply examples of input data \\(\mathbf{x}\\) labelled with the true class label, we can train the perceptron algorithm to learn the values of \\(\mathbf{w}\\) and \\(\theta\\). In fact, we can rewrite \\(\mathbf{w}\\) to include \\(\theta\\). Let's take a look at an example where the input data is two-dimensional \\(\mathbf{x}=(x\_{1}, x\_{2})\\) and the weight vector is \\(\mathbf{w}=( w\_{1}, w\_{2} ) \\). We can then write the activation function \\(\phi(w\_1x\_1+w\_2x\_2)=\phi(\mathbf{w}\^{T}\mathbf{x})=\phi(z)>\theta\\). We can also move the \\(\theta\\) to the other side of the equation to get \\(\phi(z)-\theta=w\_1x\_1+w\_2x\_2-\theta\geq 0 \\). In fact, if we rewrite \\(\mathbf{x}=(x\_1,x\_2, 1)\\) and \\(\mathbf{w}=(w\_1,w\_2,-\theta)\\), and define \\(w\_0=-\theta\\) and \\(x\_0=1\\) we can express \\(z\\) as \\(\mathbf{w}\^{T}\mathbf{x}\\).  

Now that we have examined notation involved in the perceptron, let's take a look at the perceptron algorithm.

#### Perceptron Algorithm

1. Initialise the values of \\(\mathbf{w}\\) to \\((0,0,0)\\).
2. For each training data example \\(\mathbf{x^i}\\), we compute 
    * \\(\hat{y}\\) the precited class of the example 
    * we update each entry of the weight vector \\(\mathbf{w}\\), using the formula
        \\[ w\_j := w\_j + \delta w\_j \\]
    * \\(\delta w\_j\\) can be computed as follows:
        \\[ \delta w\_j = \eta (y^i-\hat{y})x\_j^i, \\] where \\(\eta\\) is the learning rate and is a floating number between 0.0 and 1.0. We'll examine how this works in a later post. 
3. Continue iterating over the training data until all examples are classified correctly. 

A (super quick and dirty, dont-use-this-in-prod) example implementation of the algorithm (in Python ) and a small training example is given below:

```python
def fit(data, eta, max=10):
    """
    data : list of tuples in the format (x0, x1, c)
    eta: floating number between 0.0 and 1.0 
    """

    w = [0,0,0]
    misclassified = 0
    for point in data:
        cp = w[0]*1+w[1]*point[0]+w[2]*point[1]
        if cp!=point[2]:
            misclassified+=1
        w[0] = w[0]+eta*(point[2]-cp)*1
        w[1] = w[1]+eta*(point[2]-cp)*point[0]
        w[2] = w[2]+eta*(point[2]-cp)*point[1]
    epochs=0
    while epochs<=max and misclassified>0:
        misclassified=0
        for point in data:
            cp = w[0]*1+w[1]*point[0]+w[2]*point[1]
            predicted_class = 1 if cp>0 else -1
            if predicted_class!=point[2]:
                misclassified+=1
            w[0] = w[0]+eta*(point[2]-cp)*1
            w[1] = w[1]+eta*(point[2]-cp)*point[0]
            w[2] = w[2]+eta*(point[2]-cp)*point[1]
        print w
        epochs+=1
    print 'Finished'
    print w

def main():
    data=[(0.5, 0.5, 1), (-0.5, -0.5, -1)]
    eta = 0.5
    fit(data, eta)

if __name__=='__main__':
    main()
            
```

In the next post, we will refactor this implementation a la OOP ( the old Java is hard to lose ) and write some tests to test for regressions. 

### References

_Python: Deeper Insights into Machine Learning_ by John Hearty, David Julian and Sebastian Raschka






