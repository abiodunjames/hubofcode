---
title: Building an Image Classifier With Pytorch
layout: post
date: '2020-03-14 10:26:35'
author: 'Samuel'
categories: MachineLearning
comments: true
description: In this post, you'll learn how to train an image classifier using transferred learning with Pytorch on Google Colab. We'll use a dataset provided by [CalTech]
canonical_url: 'https://dev.to/abiodunjames/building-an-image-classifier-using-pytorch-46dk'
tags:
- Pytorch
- Deeplearning
- Computervision
- machinelearning
- GoogleColab
---


![](https://res.cloudinary.com/samueljames/image/upload/v1584088420/template.png)

In this post, you'll learn how to train an image classifier using transferred learning with Pytorch on Google Colab. We'll use a dataset provided by [CalTech](http://www.vision.caltech.edu/Image_Datasets/Caltech101/), which contains pictures of objects in 101 categories. We'll talk briefly about overfitting and how to avoid overfitting. In the end, we'll train a model capable of making generalized predictions over some selected categories of objects. If you ever feel lost at any point, feel free to take a look at the Jupyter notebook on [Github](https://github.com/abiodunjames/MachineLearning/tree/master/ImageClassificationPytorch) or [execute the Jupyter notebook](https://colab.research.google.com/drive/1m5jYxUYKsOoS2ACrjRIzby2AV6Ph2KqR) directly on Google Colab.

### Pytorch and Why

[Pytorch](https://pytorch.org/) is a deep learning framework developed by Facebook, and it's completely opensource. 

When it comes to using a deep learning framework, there are many options you can choose from, but very few (if any) offer the simplicity and flexibility Pytorch offers. Pytorch syntax is simple and pythonic (Python-based), which makes it easy for Python developers to master and get up and running quickly. 

Besides, Pytorch has excellent documentation with various tutorials and an option to try out what you learned on Google Colab in one click.



### Google Colab

I like to see Google Colab as a notebook of one click away from my browser. Google Colab allows you to write and execute Python code in your browser with zero configuration. It supports popular data science libraries and deep learning frameworks, including Pytorch, without requiring you to install anything. 

You can import datasets, train, and evaluate models by leveraging Google hardware, including GPUs and TPUs.  With all this awesomeness that Google Colab offers, all you need to use it is your google or Gmail account.

To start your first notebook on Google Colab, sign in with your Google account on [Google Colab](https://colab.research.google.com), and open a new Jupyter notebook.

### Data preprocessing

Datasets don't usually come in their best form suitable for fitting models directly. In most cases, one needs to do some level of preprocessing before training. Fortunately, we're using [CalTech](http://www.vision.caltech.edu/Image_Datasets/Caltech101/) datasets, which have been labeled for us.  However, we'll not use all classes in CalTech datasets in this post.  We'll  use a subset of the data.

First, let's download the datasets. You can run the command block below in your Jupiter notebook to get the data.

```bash
!wget http://www.vision.caltech.edu/Image_Datasets/Caltech101/101_ObjectCategories.tar.gz
!tar -xvzf ./101_ObjectCategories.tar.gz 
```

### Splitting Datasets

One of the major enemies of your algorithm is overfitting. **Overfitting** is when a model is so tuned to the training sets that it's unable to make accurate predictions  on data outside of the training sets.

The goal of any machine learning project is to be able to learn well on the training data and as well make good predictions on new data. A model that predicts well only training sets is overfitted and may not perform well in production.

But how do we know if a model is overfitted? 

There is no other way of knowing than testing the model on data it has not seen. This dataset is referred to as test datasets. 

It's a best practice to train your model on train datasets and test the accuracy of the model on test datasets.

A train dataset is the datasets used in learning (fitting the model), and a test dataset is independent of the train sets.   If your model fits the training data well and also generalizes well on independent datasets, you have achieved minimum overfitting.

Besides holding out some data for testing aside from the training datasets, we'll also hold out some independent data for validation.  The validation dataset is a sample of your dataset held out to provide an unbiased evaluation of your model while tunning the model hyperparameters.

In this example, 75% of our data goes for training, 12.5% for validation, and 12.5% for testing.

![](https://res.cloudinary.com/samueljames/image/upload/v1584054258/Untitled_Diagram-Page-3_1.png)



The python script below splits the CalTech dataset into train sets, validation sets, and test sets. 

```python
import numpy as np
import math
from typing import List
import os
import argparse
import glob
import shutil

def list_files(path):
    files = os.listdir(path)
    return np.asarray(files)

def split_files(oldpath, newpath, classes):
    for name in classes:
        full_dir = os.path.join(os.getcwd(), f"{oldpath}/{name}")

        files = list_files(full_dir)
        total_file = np.size(files,0)
        # We split data set into 3: train, validation and test
        
        train_size = math.ceil(total_file * 3/4) # 75% for training 

        validation_size = train_size + math.ceil(total_file * 1/8) # 12.5% for validation
        test_size = validation_size + math.ceil(total_file * 1/8) # 12.5x% for testing 
        
        train = files[0:train_size]
        validation = files[train_size:validation_size]
        test = files[validation_size:]

        move_files(train, full_dir, f"train/{name}")
        move_files(validation, full_dir, f"validation/{name}")
        move_files(test, full_dir, f"test/{name}")

def move_files(files, old_dir, new_dir):
    new_dir = os.path.join(os.getcwd(), new_dir);
    if not os.path.exists(new_dir):
        os.makedirs(new_dir)

    for file in np.nditer(files):
        old_file_path = os.path.join(os.getcwd(), f"{old_dir}/{file}")
        new_file_path = os.path.join(os.getcwd(), f"{new_dir}/{file}")

        shutil.move(old_file_path, new_file_path)

```



To split the data, you can run `split_files` function by passing a list of classes you want train your model and a path as  as arguments.

```python
classes = ['Leopards', 'airplanes', 'butterfly', 'camera', 'elephant', 'lamp', 'watch', 'umbrella', 'rhino'];

split_files('101_ObjectCategories', './', classes)

```



### Model Selection

Choosing a model largely depends on so many things like accuracy of the model, inference time, model size, etc.  If your goal is to compute inference on mobile devices, the size of your model might be crucial to your choice of model. Depending on your use cases, you might find yourself having to choose one model over another.

For this post, I'll use [ResNet50](https://pytorch.org/hub/facebookresearch_WSL-Images_resnext/)  (a pre-trained model) because of its high top-1 and top-5 accuracy.

```python
from torchvision import models
import torch

for param in model.parameters():
  param.requires_grad = False
 
fc_inputs = model.fc.in_features

model.fc = nn.Sequential(
    nn.Linear(fc_inputs, 2048),
    nn.ReLU(inplace=True),
    nn.Linear(2048, 10),
    nn.Dropout(0.4),
    nn.LogSoftmax(dim=1)
)
```

The code above replaces the final layer of the resnet50 model by a set of Sequential layers. The inputs to the last connected layer of the pre-trained model is passed on to a Linear layer of 2048 inputs, then fed to Dropout and ReLU layers followed by 2048 by 10 Linear Layer with 10 outputs corresponding to the classes of images in the training data sets. 

Pytorch allows you to write device-agnostic code (CPU or GPU). It’s a good practice to check for the availability of cuda so we can take advantage of it GPU if available.

```python
import torch.nn as nn

if(torch.cuda.is_available()):
    model = model.to('cuda:0')
```



### Input transformation

Before we fit the model with the training sets, we need to have some levels of transformation like resizing, normalizing, and even rotating so that we have enough variations of data. 

Pytorch `torchvision.transforms` allows you to compose your transformations painlessly.

```python

from torchvision import transforms

image_transforms = {
    'train': transforms.Compose([
              transforms.RandomResizedCrop(size=256, scale=(0.8, 1.0)),
              transforms.RandomRotation(degrees=15),
              transforms.RandomHorizontalFlip(),
              transforms.CenterCrop(size=224),
              transforms.ToTensor(),
              transforms.Normalize([0.485, 0.456, 0.406],[0.229, 0.224, 0.225])
    ]),
    'validation': transforms.Compose([
             transforms.Resize(size=256),
             transforms.CenterCrop(size=224),
             transforms.ToTensor(),
             transforms.Normalize([0.485, 0.456, 0.406],[0.229, 0.224, 0.225])              
    ]),
    'test': transforms.Compose([
             transforms.Resize(size=256),
             transforms.CenterCrop(size=224),
             transforms.ToTensor(),
             transforms.Normalize([0.485, 0.456, 0.406],[0.229, 0.224, 0.225])              
    ])
}
```

Having composed transformations for the datasets, it's time to load the data and also create an iterator for the Data Loader.

```python
import torchvision.datasets as datasets
import torch.utils.data as loader

data = {
    'train': datasets.ImageFolder(root='./train', transform=image_transforms['train']),
    'validation': datasets.ImageFolder(root='./validation', transform=image_transforms['validation']),
    'test': datasets.ImageFolder(root='./test', transform=image_transforms['test'])
}

# create a data loader instance with each dataset with a batch size of 10 and shuffling
batch_size = 10
train_data = loader.DataLoader(data['train'], batch_size=batch_size, shuffle=True)
validation_data = loader.DataLoader(data['validation'], batch_size=batch_size, shuffle=True)
test_data = loader.DataLoader(data['test'], batch_size=batch_size, shuffle=True)

```

To calculate the loss function, that is a measure of how wrong your model prediction is, we need to know the size of each data set.

```python
train_data_size = len(data['train'])
validation_data_size = len(data['validation'])
test_data_size =  len(data['test'])

```



### Optimizer

Optimizer and loss function are vital to learning your data.  If loss function is a way of measuring how wrong your predictions are, then Optimizer is that thing that tweaks your model parameters based on the output of loss function to ensure that your predictions are accurate as possible. 

For this example, we use Adam optimizer as the choice of optimization algorithm for this problem.

```python
import torch.optim as optim
loss_func = nn.NLLLoss()
optimizer = optim.Adam(model.parameters())
```



### Fitting the model

Let's define a method for fitting and validating our model.

```python

import time

def train_and_validate(model, loss_criterion, optimizer, epochs=25):
  device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
  start = time.time()
  history = []
  best_acc = 0.0
  for epoch in range(epochs):
    epoch_start = time.time()
    print(f'Epoch : {epoch+1}/{epochs}')
    model.train()
    train_loss = 0.0 
    train_acc = 0.0
    
    valid_loss = 0.0
    valid_acc  = 0.0

    for i, (inputs, labels) in enumerate(train_data):
      inputs = inputs.to(device)
      labels = labels.to(device)
      # Clean existing gradients
      optimizer.zero_grad()
      # Forward pass - compute outputs on input data using the model
      outputs = model(inputs)
      # Compute loss
      loss = loss_criterion(outputs, labels)
      # Backpropagate the gradients
      loss.backward()
      # Update the parameters
      optimizer.step()
      # Compute the total loss for the batch and it to train_loss
      train_loss += loss.item() * inputs.size(0)
      # Compute the accuracy
      ret, predictions = torch.max(outputs.data,1)
      correct_counts = predictions.eq(labels.data.view_as(predictions))

      # Convert correct_counts to float and then compute the mean
      acc = torch.mean(correct_counts.type(torch.FloatTensor))
      # Compute total accuracy in the whole batch and add to train_acc
      train_acc += acc.item() * inputs.size(0)
      print(f'Batch number: {i}, Training: Loss: {loss.item()}, Accuracy: {acc.item()}')

    with torch.no_grad():
      model.eval()
      for j, (inputs, labels) in enumerate(validation_data): 
        inputs = inputs.to(device)
        labels = labels.to(device)
        # Forward pass - compute outputs on input data using the model
        outputs = model(inputs)

        # Compute loss
        loss = loss_criterion(outputs, labels)
        # Compute the total loss for  the batch and add it to valid_loss
        valid_loss += loss.item() * inputs.size(0)
        # Calculate validation accuracy

        ret, predictions = torch.max(outputs.data, 1)
        correct_prediction_counts = predictions.eq(labels.data.view_as(predictions))

        # Convert correct_prediction_counts to float and then compute the mean
        acc = torch.mean(correct_prediction_counts.type(torch.FloatTensor))

        # Compute total accuracy in the whole batch and add to valid_acc

        valid_acc +=acc.item() * inputs.size(0)

      avg_train_loss = train_loss/train_data_size
      avg_train_acc = train_acc/train_data_size

      avg_valid_loss = valid_loss/validation_data_size
      avg_valid_acc = valid_acc/validation_data_size

      history.append([avg_train_loss, avg_valid_loss, avg_train_acc, avg_valid_acc])

      epoch_end = time.time()
    
      print(f'Epoch : {epoch}, Training: Loss: f{avg_train_loss}, Accuracy: {avg_train_acc*100}%, \n\t\tValidation : Loss : {avg_valid_loss}, Accuracy: {avg_valid_acc*100}%, Time: {epoch_end-epoch_start}s')
      
      # Save if the model has best accuracy till now

      torch.save(model.state_dict(), f'model_{epoch}.pth')
    
    return model, history

```

Having defined the `train_and_validate` method, we can train the model by running the function. 

```python
num_epochs = 30
trained_model, history = train_and_validate(model, loss_func, optimizer, num_epochs)
```



### Testing the model

In earlier paragraphs, we talked about the problem of overfitting. We also discussed briefly one way to determine if your model is able to generalize on new data sets by setting aside a test set. Let’s put our model to test by seeing how it performs on test sets.

 Remember, we should never train on test sets, and that’s why we held it out until now.

```python

def computeModelAccuracy(model, loss_criterion):
  device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
  test_acc = 0.0
  test_loss = 0.0

  with torch.no_grad():
    # Set to evaluation mode
    model.eval()
    for i, (inputs, labels) in enumerate(test_data):
      inputs = inputs.to(device)
      labels = labels.to(device)
      outputs = model(inputs)

      # Compute loss
      loss = loss_criterion(outputs, labels)

      # Compute the toal loss item 
      test_loss += loss.item() * inputs.size(0)

      ret, predictions = torch.max(outputs.data, 1)
      correct_counts = predictions.eq(labels.data.view_as(predictions))
      acc = torch.mean(correct_counts.type(torch.FloatTensor))
      test_acc +=acc.item() * inputs.size(0)

      print(f'Test Batch number: {i}, Test: Loss: {loss.item()}, Accuracy: {acc.item()}')

      # Find average test loss and test accuracy
      avg_test_loss = test_loss/test_data_size
      avg_test_acc = test_acc/test_data_size

      print(f'Test accuracy: {avg_test_acc}')
```

###  Making Predictions

To predict the class of a new image, we need to convert the image to a format that our model understands.  

```python

from PIL import Image
import requests
import matplotlib.pyplot as plt

index_to_class = {v: k for k, v in data['train'].class_to_idx.items()}
print (index_to_class)

def makePrediction(model, url):
    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    transform = image_transforms['test']

    test_image = Image.open(requests.get(url, stream=True).raw)

    plt.imshow(test_image)
    
    test_image_tensor = transform(test_image)
    test_image_tensor = test_image_tensor.view(1, 3, 224, 224).to(device)
    with torch.no_grad():
        model.eval()
        out = model(test_image_tensor)
        ps = torch.exp(out)
        
        topk, topclass = ps.topk(3, dim=1)
        for i in range(3):
            print(f"Prediction {i+1} : {index_to_class[topclass.cpu().numpy()[0][i]]}, Score: {topk.cpu().numpy()[0][i] * 100}%")
```

The function `makePrediction` above takes a URL of an image and returns the top 3 predictions of the class of the image.

For example:

```python
model = torch.load('_model_0.pt')
makePrediction(model, 'https://cdn.britannica.com/30/136130-050-3370E37A/Leopard.jpg')
```



### Conclusion

We've learned how to train an image classifier using Pytorch. We talked briefly  about splitting data sets and why overfitting should be avoided. We've explored how to train and evaluate models on Google Colab. We've pretty much covered a lot of things in this post.  To build on this knowlege, I encourage you to take a look at the references below. 

Finally, I share useful blogposts on machine learning and exciting stuff from the community every day; please follow or tweet at [@hubofml](https://twitter.com/hubofml) on twitter .

### References

[A Comprehensive Hands-on Guide to Transfer Learning with Real-World Applications in Deep Learning](https://towardsdatascience.com/a-comprehensive-hands-on-guide-to-transfer-learning-with-real-world-applications-in-deep-learning-212bf3b2f27a)

[Introduction to Optimizers](https://algorithmia.com/blog/introduction-to-optimizers)

[Introduction to Machine Learning](https://developers.google.com/machine-learning/crash-course/ml-intro)

[Image Classification Using Pretrained-Model](https://www.learnopencv.com/pytorch-for-beginners-image-classification-using-pre-trained-models/)


