---
title: Custom Vision API Python tutorial | Microsoft Docs
description: Explore a basic Windows app that uses the Custom Vision API in Microsoft Cognitive Services. Create a project, add tags, upload images, train your project, and make a prediction using the default endpoint.
services: cognitive-services
author: areddish
manager: chbuehle

ms.service: cognitive-services
ms.technology: custom vision service
ms.topic: article
ms.date: 12/22/2017
ms.author: areddish
---

# Custom Vision API Python Tutorial
Explore a basic python script that uses the Computer Vision API to create a project; add tags to it; upload images; train the project; obtain the default prediction endpoint URL for the project; and use the endpoint to programmatically test an image. You can use this open-source example as a template for building your own app using the Custom Vision API.

## Prerequisites

To use the tutorial, you will need to do the following:

- Install either Python 2.7+ or Python 3.5+.
- Install pip.
- Install git.

### Platform requirements
This example has been developed for the Python.

### Get the Custom Vision SDK

- Install the Preview Python SDK for the Custom Vision API as follows from github:

```
pip install "git+https://github.com/Azure/azure-sdk-for-python#egg=azure-cognitiveservices-vision-customvision&subdirectory=azure-cognitiveservices-vision-customvision"
```

If you encounter a Filename too long error, make sure you have long path support in git enabled.

```
git config --system core.longpaths true
```

## Step 1: Prepare the keys and the images needed for the example

Custom Vision Service can be found by clicking here: https://customvision.ai

Obtain your training and prediction key by logging into the Custom Vision Service and navigating to account settings.

This example uses the images from [here](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images). 


## Step 2: Create a Custom Vision Service project

Begin by creating a sample.py script file and adding the contents below.

```Python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageUrlCreateEntry

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Project")
```

## Step 3: Add tags to your project

To add tags to your project, insert the following code to create two tags.

```Python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## Step 4: Upload images to the project

To add the images we have to the project, insert the following code after the tag creation. This will upload the image with the corresponding tag.

```Python
base_image_url = "https://raw.githubusercontent.com/Microsoft/Cognitive-CustomVision-Windows/master/Samples/"

print ("Adding images...")
for image_num in range(1,10):
    image_url = base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(image_url, [ hemlock_tag.id ] ) ])

for image_num in range(1,10):
    image_url = base_image_url + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(image_url, [ cherry_tag.id ] ) ])


# Alternatively, if the images were on disk in a folder called Images along side the sample.py then
# they could be added by the following.
#
#import os
#hemlock_dir = "Images\\Hemlock"
#for image in os.listdir(os.fsencode("Images\\Hemlock")):
#    with open(hemlock_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data.read(), [ hemlock_tag.id ])
#
#cherry_dir = "Images\\Japanese Cherry"
#for image in os.listdir(os.fsencode("Images\\Japanese Cherry")):
#    with open(cherry_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data.read(), [ cherry_tag.id ])
```

## Step 5: Train the project

Now that we've added tags and images to the project, we can train it. This creates the first iteration in the project. We can then mark this iteration as the default iteration.

```Python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status == "Training"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Make it the default project endpoint
trainer.update_iteration(project.id, iteration.id, is_default=True)
print ("Done!")
```

## Step 6: Get and use the default prediction endpoint

We are now ready to use the model for prediction. First we obtain the endpoint associated with the default iteration. Then we send a test image to the project using that endpoint.

```Python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint, it can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Alternatively, if the images were on disk in a folder called Images along side the sample.py then
# they could be added by the following.
#
# Open the sample image and get back the prediction results.
# with open("Images\\test\\test_image.jpg", mode="rb") as test_data:
#     results = predictor.predict_image(project.id, test_data.read(), iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag + ": {0:.2f}%".format(prediction.probability * 100))
```

## Step 7: Run the example

The prediction results appear on the console.

```
python sample.py
```
