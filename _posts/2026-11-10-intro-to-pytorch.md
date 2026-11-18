---
layout: post
title: "Intro to Deep Learning using Pytorch"
date: 2026-11-10 13:14:26 -0400
tags: WIP DL pytorch intro jupyter
categories: research
giscus_comments: true
---

<!--toc:start-->

- [Learning Objectives](#learning-objectives)
- [What is Deep Learning](#what-is-deep-learning)
- [Simple Feedforward Networks](#simple-feedforward-networks)
- [U-Net](#u-net)
- [Objectives](#objectives)
<!--toc:end-->

The motivation for this post is to elucidate the basic of deep learning to
myself. If any others benefit from this post, then that is always a plus. I will
using Russell and Norvig 4e as the source material along with anything that I
find useful in Deep Learning by Ian Goodfellow.

## Learning Objectives

I hope to focus on what a robotics engineering focusing on robotic manipulation
would need. This lens should sufficiently distinguish my post from others and
make it most apt for my needs.

1. Define what deep learning is.
2. Construct a couple models focused on robotics applications.

- U-Net
- Transformer
- Variational Auto-Encoder
- LSTM
- Diffusion Model

1. Explain the design choices behind each of the models chosen.

## What is Deep Learning

For a roboticist, deep learning provides a convenient way to develop algorithms
for performing complex manipulation tasks that would normally be impossible
because there are too many factors and scenarios for the ordinary programmer to
consider. Therefore, roboticist opt for machine learning methods where they can
take a set of data and then have a robot "learn" an algorithm (ie a set of
instructions given a specific program state or value of a variable"). More
specifically, we develop a template program with a bunch of knobs that can be
turned to produce the result we want. Here we tune or "learn" the program by
running the data through the template program and then compare it to the results
that we expect it to have. This means we need to have data that also includes
the expect prediction in order for us to compare it with what our model outputs.
In some ways, this is similar to debugging where we have a program and we run it
to see what it outputs and then we will tweak it until we get the output we
expect. The difference is that we are typing in a programming language (ie code)
whereas when we learn an algorithm, the computer is thinking in terms of
weights. It's important to remember that when we program, we are really working
with numbers and when we program in human readable programming language then we
are really performing mathematical operations on numbers. This is a brief
overview of machine learning and I would suggest looking online for more
information as there is a plethora of materials available on the Internet.

Now let's get into deep learning! Deep learning is a type of machine learning
that uses complex algebraic circuits with tunable connection strengths [^1].
Deep learning a method where we do algorithm tuning mentioned earlier but we
focus on doing it in a certain way. In deep learning where stack a bunch of
"layers" together to perform a very complex transformation on the data that will
generate a prediction based on our input. There's a lot there so let's break it
down. We start with what is called a neural network which a big collection of
these functions called neurons that take an input value in and then pass them
through a function to produce a transformation o the data. We can do this
process multiple times and this results in a very complex math function that we
wouldn't have been able to come up with on our own. The network here indicates
that part of the input or output to one function can be used to affect the input
or output of other functions so all these functions have an effect on each
other, hence the name network. The neural part of the name is synonymous with
the neural cells in our brains because they take an input value in and the
produce some output value that is passed along to another function in a chain
[^2]. This process is an algorithm because we put the input through the
beginning of the neural network and then it will procedurally go through a
series of transformations to generate a prediction or output. This type of
programming is different than traditionally programming because we don't really
know what the tuning up or down for a function in a network does besides whether
the whole network predicts better or worse. So we use the value of whether the
network predicts worse or better to tune some functions up or down automatically
using an optimizer function. We say that the model predicts worse or better
using a loss function which is just the number we calculate to tell us whether
our model is performing how we want or not. Generally, this number is how far
off our prediction is or the number of wrong predictions but we can choose to
calculate other numbers as well. The relevance of this number will have a strong
effect on whether our model does a good job or not.

[^1]: TODO: need to explain this sentence

[^2]: Another way this could be stated is the neural network is a complex dynamical system](#2-another-way-this-could-be-stated-is-the-neural-network-is-a-complex-dynamical-system)

## Simple Feedforward Networks

## U-Net

## Objectives

This intro post is focused on providing a succint tutorial on how to use pytorch
based on the
[tutorial](https://docs.pytorch.org/tutorials/beginner/basics/quickstart_tutorial.html)
already available on the pytorch tutorials page. I will try to explain it
further so there is additional explanation for why we do certain things and how
they can be repurposed.

I have included the jupyter notebook below to make it easier to follow along.
This is just a static display of the code and if you want more details, as
always feel free to drop a comment or email me and I'll try to get back to you.

In order to build a basic ML pipeline using Pytorch, you follow the basic
architecture below.

Before you start, you want to make sure you have a dataset in mind along along
with all the relevant considerations like how you want to split it for training,
validation, and testing. You may want to inspect the data to see what kind of
architecture would best fit it. If you are dealing with images for example, it
might be best to use a U-Net which is a type of Convolutional Neural Net (CNN).

First, you import all your libraries. So you will need to import torch and make
sure that your python points to the right direction. You can choose to import
the submodules within pytorch directly into the script (eg
`from torch import nn`) or type the full name out (eg `torch.nn`).

Second, you subclass the `nn.Module` base class and build whatever model you
want. In this case, we named it a generic name, `NeuralNet`, although if it's a
specific model then be more specific. When subclassing `nn.Module`, make sure to
call `super().__init__()`. You will also need to define a `forward` method so
you can train the model. You can add hooks and extra things as well that will be
called everytime you run a forward pass on a model training. In this case, we
define a basic neural net that uses a series of linear transforms with ReLU
activation functions. This is one of the most simple Deep Learning models we can
make and it is an example of a multi-layer perceptron (MLP) or Feedforward
Neural Network.

We basically flatten the input, an image, into a 1D vector and then pass it
through a series of linear transformations that will be tuned using an optimizer
and loss function. We lose 2D spatial relationships between neighboring pixels
when we flatten the input image but it makes the model very simple. We dealing
with more complex images than the FashionMNIST dataset which is black and white,
we will consider using more complex models.

Third, we define a way to train the model using the training data we decided
before starting. Pytorch comes with a dataloader but there are other libraries
that also provide dataloaders for model training like huggingface_hub. We define
the training loop by setting a batch size (ie how many images we want to run
through the model before deciding whether we should increase or decrease the
weights of some or every model parameter). We set the batch size and make sure
every tensor/vector is on the same computing device (ie gpu). We run the input
through the model and compare the model prediction with the correct prediction.
Then we run the optimization function to update the model's weights/params.

We will define something similar for the testing function, except we remove all
the logic to update the weights.

Fourth, we implement the whole program logic to run the previous steps we
previously mentioned so that our program has a place to start and a set of steps
to actually follow. Before, we were just defining a bunch of classes and
functions.

Finally, after we trained our model, we can run the model on our compute and see
what it says on a new result that may **or may not** be in our dataset.

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/blog.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/blog.ipynb %}{% endcapture %} <!-- rumdl-disable-line line-length -->
{% if notebook_exists == "true" %}
{% jupyter_notebook jupyter_path %}
{% else %}

<p>Sorry, the notebook you are looking for does not exist.</p> <!-- rumdl-disable-line no-inline-html -->
{% endif %}
{:/nomarkdown}
