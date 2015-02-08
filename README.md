

## Kitten vs bear classifier

This repo contains a [brain4k](https://github.com/shuggiefisher/brain4k) pipeline for a kitten-or-bear classifier.  It is intended to serve as
an example of how to train a classifier on imagenet features.  The features
are the activity of the penultimate layer of a convolutional neural network trained
on the imagenet database.

to execute the pipeline within a docker container:

```
sudo docker run -ti tleyden5iwx/caffe-cpu-master /bin/bash
pip install git+https://github.com/shuggiefisher/brain4k.git
git clone https://github.com/shuggiefisher/kitten-or-bear-classifier.git local-path-to-this-repo
brain4k local-path-to-this-repo
```

### Requirements
- [brain4k](https://github.com/shuggiefisher/brain4k)
- caffe, including python wrappers
- scikit-learn
- matplotlib

Installing caffe is quite involved, so you may find it easier to use a
[caffe docker image](https://registry.hub.docker.com/u/tleyden5iwx/caffe/)





# The Pipeline

<a href='http://g.gravizo.com/g?digraph G { compound=true; ranksep=1; ratio=compress; size=728; subgraph cluster_1 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "1_Extracted image features" [style="filled,rounded",color=green,label="Extracted image features",fontname="Verdana",fontsize=12,shape=rectangle]; "1_kitten-or-bear images" [style="filled,rounded",color=orange,label="kitten-or-bear images",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Neural network pretrained on imagenet"; } subgraph cluster_2 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "2_Image features and ground truth labels" [style="filled,rounded",color=green,label="Image features and ground truth labels",fontname="Verdana",fontsize=12,shape=rectangle]; "2_kitten-or-bear images" [style="filled,rounded",color=orange,label="kitten-or-bear images",fontname="Verdana",fontsize=12,shape=rectangle];"2_Extracted image features" [style="filled,rounded",color=orange,label="Extracted image features",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Join image labels and extracted features"; } "1_Extracted image features" -> "2_Extracted image features" [ltail=cluster_1, lhead=cluster_2]; subgraph cluster_3 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "3_Test and Training image features" [style="filled,rounded",color=green,label="Test and Training image features",fontname="Verdana",fontsize=12,shape=rectangle]; "3_Image features and ground truth labels" [style="filled,rounded",color=orange,label="Image features and ground truth labels",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Split into test and training set"; } "2_Image features and ground truth labels" -> "3_Image features and ground truth labels" [ltail=cluster_2, lhead=cluster_3]; subgraph cluster_4 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "4_Image classifier test results" [style="filled,rounded",color=green,label="Image classifier test results",fontname="Verdana",fontsize=12,shape=rectangle];"4_kitten-or-bear classifier" [style="filled,rounded",color=green,label="kitten-or-bear classifier",fontname="Verdana",fontsize=12,shape=rectangle]; "4_Test and Training image features" [style="filled,rounded",color=orange,label="Test and Training image features",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Multinomial Naive Bayes Classifier"; } "3_Test and Training image features" -> "4_Test and Training image features" [ltail=cluster_3, lhead=cluster_4]; subgraph cluster_5 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "5_Image classifier confusion metric" [style="filled,rounded",color=green,label="Image classifier confusion metric",fontname="Verdana",fontsize=12,shape=rectangle];"5_Image classifier confusion graph" [style="filled,rounded",color=green,label="Image classifier confusion graph",fontname="Verdana",fontsize=12,shape=rectangle]; "5_Image ground truth labels" [style="filled,rounded",color=orange,label="Image ground truth labels",fontname="Verdana",fontsize=12,shape=rectangle];"5_Image classifier test results" [style="filled,rounded",color=orange,label="Image classifier test results",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Confusion matrix"; } "4_Image classifier test results" -> "5_Image classifier test results" [ltail=cluster_4, lhead=cluster_5]; start -> "1_kitten-or-bear images" [lhead=cluster_1]; "5_Image classifier confusion metric" -> end [ltail=cluster_5]; start [shape=Mdiamond]; end [shape=Msquare]; }'>
<img alt="Pipeline diagram" src='http://g.gravizo.com/g?digraph G { compound=true; ranksep=1; ratio=compress; size=728; subgraph cluster_1 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "1_Extracted image features" [style="filled,rounded",color=green,label="Extracted image features",fontname="Verdana",fontsize=12,shape=rectangle]; "1_kitten-or-bear images" [style="filled,rounded",color=orange,label="kitten-or-bear images",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Neural network pretrained on imagenet"; } subgraph cluster_2 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "2_Image features and ground truth labels" [style="filled,rounded",color=green,label="Image features and ground truth labels",fontname="Verdana",fontsize=12,shape=rectangle]; "2_kitten-or-bear images" [style="filled,rounded",color=orange,label="kitten-or-bear images",fontname="Verdana",fontsize=12,shape=rectangle];"2_Extracted image features" [style="filled,rounded",color=orange,label="Extracted image features",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Join image labels and extracted features"; } "1_Extracted image features" -> "2_Extracted image features" [ltail=cluster_1, lhead=cluster_2]; subgraph cluster_3 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "3_Test and Training image features" [style="filled,rounded",color=green,label="Test and Training image features",fontname="Verdana",fontsize=12,shape=rectangle]; "3_Image features and ground truth labels" [style="filled,rounded",color=orange,label="Image features and ground truth labels",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Split into test and training set"; } "2_Image features and ground truth labels" -> "3_Image features and ground truth labels" [ltail=cluster_2, lhead=cluster_3]; subgraph cluster_4 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "4_Image classifier test results" [style="filled,rounded",color=green,label="Image classifier test results",fontname="Verdana",fontsize=12,shape=rectangle];"4_kitten-or-bear classifier" [style="filled,rounded",color=green,label="kitten-or-bear classifier",fontname="Verdana",fontsize=12,shape=rectangle]; "4_Test and Training image features" [style="filled,rounded",color=orange,label="Test and Training image features",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Multinomial Naive Bayes Classifier"; } "3_Test and Training image features" -> "4_Test and Training image features" [ltail=cluster_3, lhead=cluster_4]; subgraph cluster_5 { style=filled; color=lightgrey; fontname="Helvetica"; fontsize=18; "5_Image classifier confusion metric" [style="filled,rounded",color=green,label="Image classifier confusion metric",fontname="Verdana",fontsize=12,shape=rectangle];"5_Image classifier confusion graph" [style="filled,rounded",color=green,label="Image classifier confusion graph",fontname="Verdana",fontsize=12,shape=rectangle]; "5_Image ground truth labels" [style="filled,rounded",color=orange,label="Image ground truth labels",fontname="Verdana",fontsize=12,shape=rectangle];"5_Image classifier test results" [style="filled,rounded",color=orange,label="Image classifier test results",fontname="Verdana",fontsize=12,shape=rectangle]; label = "Confusion matrix"; } "4_Image classifier test results" -> "5_Image classifier test results" [ltail=cluster_4, lhead=cluster_5]; start -> "1_kitten-or-bear images" [lhead=cluster_1]; "5_Image classifier confusion metric" -> end [ltail=cluster_5]; start [shape=Mdiamond]; end [shape=Msquare]; }' width=728/>
</a>


# Pipeline performance metrics


![Confusion Matrix Caption](kitten-or-bear-classifier/metrics/figures/image_classifier_confusion.png)

Confusion matrix:

[[12 11]
 [ 8  6]]
