## Cat or Dog classifier

This repo contains a pipeline for a cat-or-dog classifier.  It is intended to serve as
an example of how to train a classifier on imagenet features.  The features
are the activity of the penultimate layer of a convolutional neural network trained
on the imagenet database.

to run the pipeline:

```brain4k local-path-to-this-repo```

### Requirements
- brain4k
- caffe, including python wrappers
- scikit-learn

Installing caffe is quite involved, so you may find it easier to use a
[caffe docker image](https://registry.hub.docker.com/u/tleyden5iwx/caffe/)
