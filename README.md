sibe
====

A simple Machine Learning library.

A simple neural network:
```haskell
module Main where
  import Sibe
  import Numeric.LinearAlgebra
  import Data.List

  main = do
    let learning_rate = 0.5
        (iterations, epochs) = (2, 1000)
        a = (logistic, logistic') -- activation function and the derivative
        rnetwork = randomNetwork 0 2 [(8, a)] (1, a) -- two inputs, 8 nodes in a single hidden layer, 1 output

        inputs = [vector [0, 1], vector [1, 0], vector [1, 1], vector [0, 0]] -- training dataset
        labels = [vector [1], vector [1], vector [0], vector [0]] -- training labels

        -- initial cost using crossEntropy method
        initial_cost = zipWith crossEntropy (map (`forward` rnetwork) inputs) labels

        -- train the network
        network = session inputs rnetwork labels learning_rate (iterations, epochs)

        -- run inputs through the trained network
        -- note: here we are using the examples in the training dataset to test the network,
        --       this is here just to demonstrate the way the library works, you should not do this
        results = map (`forward` network) inputs

        -- compute the new cost
        cost = zipWith crossEntropy (map (`forward` network) inputs) labels
```

See other examples:
```
# Simplest case of a neural network
stack exec example-xor

# Naive Bayes document classifier, using Reuters dataset
# using Porter stemming, stopword elimination and a few custom techniques.
# The dataset is imbalanced which causes the classifier to be biased towards some classes (earn, acq, ...)
# to workaround the imbalanced dataset problem, there is a --top-ten option which classifies only top 10 popular
# classes, with evenly split datasets (100 for each), this increases F Measure significantly, along with ~10% of improved accuracy
# N-Grams don't seem to help us much here (or maybe my implementation is wrong!), using bigrams increases
# accuracy, while decreasing F-Measure slightly.
stack exec example-naivebayes-doc-classifier -- --verbose
stack exec example-naivebayes-doc-classifier -- --verbose --top-ten
```
