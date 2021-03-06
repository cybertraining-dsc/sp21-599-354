---
date: 2021-03-15
title: "Project: Identifying Agricultural Weeds with CNN"
linkTitle: Weed detection
tags: ["project", "ai", ""]
description: "Weed identification is an important component of agriculture, and can affect the way farmers utilize herbicide. When unable to locate weeds in a large field, farmers are forced to blanket utilize herbicide for weed control. However, this method is bad for the environment, as the herbicide can leech into the water, and bad for the farmer, because they then must pay for far more fertilizer than they really need to control weeds. This project utilizes images from the Aarhus University [^1] dataset to train a CNN to identify images of 12 species of plants. To better simulate actual rows of crops, a subset of the images for testing will be arranged in a list representing a crop row, with weeds being distributed in known locations. Then, the AI is tested on the row, and should be able to determine where in the row the weeds are located."
author: Paula Madetzke
github_url: https://github.com/cybertraining-dsc/sp21-599-354/edit/main/project/index.md
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

[![Check Report](https://github.com/cybertraining-dsc/sp21-599-354/workflows/Check%20Report/badge.svg)](https://github.com/cybertraining-dsc/sp21-599-354/actions)
[![Status](https://github.com/cybertraining-dsc/sp21-599-354/workflows/Status/badge.svg)](https://github.com/cybertraining-dsc/sp21-599-354/actions)
Status: final, Type: Project



Paula Madetzke, [sp21-599-354](https://github.com/cybertraining-dsc/sp21-599-354), [Edit](https://github.com/cybertraining-dsc/sp21-599-354/blob/main/project/index.md)

Code:
* [data_prep.ipynb](https://github.com/cybertraining-dsc/sp21-599-354/blob/main/project/code/data_prep.ipynb)
* [CNN_Code.ipynb](https://github.com/cybertraining-dsc/sp21-599-354/blob/main/project/code/CNN_Code.ipynb)

{{% pageinfo %}}

## Abstract

Weed identification is an important component of agriculture, and can affect the way farmers utilize herbicide. When unable to locate weeds in a large field, farmers are forced to blanket utilize herbicide for weed control. However, this method is bad for the environment, as the herbicide can leech into the water, and bad for the farmer, because they then must pay for far more fertilizer than they really need to control weeds. This project utilizes images from the Aarhus University [^1] dataset to train a CNN to identify images of 12 species of plants. To better simulate actual rows of crops, a subset of the images for testing will be arranged in a list representing a crop row, with weeds being distributed in known locations. Then, the AI is tested on the row, and should be able to determine where in the row the weeds are located.



## Contents

{{< table_of_contents >}}

{{% /pageinfo %}}

**Keywords:** Agriculture, CNN. 

## 1. Introduction


With a growing global population, and a changing climate that can make farm work hostile, it is increasingly important for farms to be efficient food producers. Fertilizers, pesticides, and herbicides have allowed modern farms to produce far higher yields than they would otherwise be able to. However, the environmental impact from the runoff of these chemicals when they leave the farm can be incredibly detrimental to both human and natural wellbeing. This is why agriculture is a field that is ripe for improvement from AI. There are opportunities for AI to be used in the harvesting of crops, predictive analytics, and field monitoring. In this project, AI is used in the field monitoring application. This is helpful for farmers because spot detection of weeds will help them avoid using as much herbicide. This project will explore how pictures of plants can be used to detect weeds in crop fields. The AI has been trained with a CNN on pictures of 12 seedling species including 960 individual plants representing both weeds desired crops [^1]. Although 12 species are available, only 3 will be studied in this project: black grass, shepherd’s purse, and sugar beet. These plants were chosen because their seedlings look different enough to give the AI a better chance at successfully detecting differences. Then, the AI is tested to determine the accuracy of the model. First this is accomplished by reserving a subset of the training data to be tested by the AI in order to get a base-line test of accuracy. Next, the test data will be arranged in a list to mimic a row of crops with a desired crop, and several weeds. Then the AI would go through the images and a program would display where a farmer would need to apply the herbicide, according to the AI. The true test would be to see if the program can produce a helpful map to narrow down where herbicide should be applied.

Previous similar work has been done with this dataset [^2] in Keras with CNN image recognition, while this project is implemented in pytorch. Similar agricultural image recognition with plant disease [^3] is also available to be studied.


## 2. Pre-Processing The Data

The plant dataset [^1] has three sets of image options to choose from. The first has large images of trays with multiple plants of the same species, the second has cropped images of individual seedings from
the larger picture (non-segmented), while the third is both cropped, and has the background replaced with black pixels such that only the leaves remain in the picture (segmented). To train the data,
the segmented data is used. This is because it is important for the AI to train such that it detects patterns only for the most important features of the image. If the AI were to train on the images in the
background, it might learn features of the sand or border instead of the leaves.




![Figure 1](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/large_weeds.jpg)

![Figure 2](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/non-segmented.png)

![Figure 3](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/segmented.png)

**Figure 1** Dataset Image options [^1]: Large image with multiple plants (top), non-segmented (middle), segmented (bottom)


The next component to consider is the image size to be used. The CNN requires all images to be a uniform size to run correctly, while the original segmented data contains images of different sizes, ranging from a few 10s of pixels wide to over 4000. In order to get a sense a balance between having a broad enough dataset to have a large enough representation of the plant pictures to draw meaningful conclusions and the need to have reasonable file sizes, the image’s width or height, whichever is larger, was recorded and loaded into a histogram for each plant species. From a visual examination, 300x300 pixels was selected. An additional 100 pixels of padding were added to the final image size to prevent the images from being cut off when rotated. For each image in the segmented dataset, it is expanded to 400x400 pixels, and added to the straight image folder. Then, each image is rotated and versions are saved to a rotation folder. An ideal rotation would allow a fine grain of rotation. However, some resource limitations were met with the GPU used for the neural net. At first, the images were rotated by 10 degrees each before being saved, then 30, then 60, and finally 90. Finally, a csv file with ids and labels is needed to identify the images, and separate the training and the test data. A preprocessing python script can be used to achieve all of these goals.

![Figure 4](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/black_grass_hist.png) | ![Figure 5](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/shepherds_purse_hist.png) | ![Figure 6](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/sugar_beet_hist.png)

**Figure 2** Histograms of the max(width, height) of each of the plant species in the full dataset


## 3. Running the CNN

The implementation of the CNN is heavily based upon the tutorial on MINST fashion identification [^4] where the CNN identifies 28x28 pixel images of clothing. It is a 2 layer CNN implemented in pytorch. In the original neural net, there are 70000 of these small images and 9 clothing designations. In this dataset, there are only 3 types of labels, but much larger images. The tutorial had a train image set, a validation set, and a test set. After preprocessing, there were 420 suitable 400x400 unrotated images. Without any changes besides adjusting the file paths and image size parameters, the CNN could inconsistently get approximately 50 percent accuracy with the test data, with the highest accuracy at 15 epochs. Because this implementation has relatively few images compared to the MINST clothing dataset, the next test was to remove the validation set of images, in order to use more of the prepared images for training. The result was a maximum of 79 percent accuracy at 25 epochs.

![Figure 7](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/training_images.png)

**Figure 3** Examples of prepared training images

The testing accuracy of the neural net on the test data tracked closely with the prediction accuracy when train accuracy was around 30 to 50 percent, lagging by only a few percent. However, when the training dataset reached the 70 percent accuracy range, the test set accuracy remained at only around 50 percent. Part of this could be due to the small size of the test set, or the fact each plant species had a slightly different number of suitable test images.

An interesting observation is that although the model was able to consistently reach 60 percent accuracy, it would sometimes fall into a pattern where it would categorize all or almost all of the test set as just one of the plants. There was no consistency in which plant it defaulted to, but in between runs where the model reached decent accuracy, it would repeat this behavior. Because the test set was evenly distributed between the three plants, this would cause the accuracy to go to around 30 percent.

In order to see if a visualization could help a human easily see where the hypothetical herbicide would need to be placed, a chart was created with tiles. The first row is the true layout of the three types of plants in the dataset, where each plant is assigned a color. The second row is the AI prediction of which type of plant the test set is. Even with the 79 percent test accuracy rate, it was not as clear as it could be from the image how accurate the model was. One way of making the visualization both easier to visually determine accuracy and more realistic is to have one type of plant be the dominant crop, and have patches of weeds throughout the row. The major obstacle to this was the fact that there were not enough suitable images to have a dominant plant in the test group. Too many images used in testing would have a detrimental impact on training the model.

![Figure 8](https://github.com/cybertraining-dsc/sp21-599-354/raw/main/project/images/visualization.png)

**Figure 4** The top row 0 shows the actual species distribution of the plants, while the bottom row 1 shows the AI prediction


## 4. Benchmarking

The longest operation by far was the first time reading in the images. This had a timed tqdm built in, so the stopwatch function was not used here. Subsequent running of the CNN training program must cache some of the results, so the image loading takes much less time. An initial loading of the 420 images took approximately 10 minutes. WIth the GPU accelarator enabled, the training of 25 epochs got to 6.809s while the test only took .015s. 

## 5. Possible Extension

The rotated images of the plants were ultimately not explored. This is due to the fact that the implementation that was followed required GPU resources on Google Colab. With only 420 images to feed into the model, the GPU was not strained. However, even with 90 degree rotations, it appeared that the GPU memory limit for running the entire notebook was reached before even the first epoch. If this project were to be attempted again, the images would either need to be smaller, or fewer straight images should be loaded so that their rotations would not exhaust GPU allocation. One way to go about this would be to have a rotation set with fewer images of individual plants, but to rotate them such that the rotated dataset is still around 420 images. Then, a comparison could be made between the training accuracy of 420 separate examples of each species vs fewer individual plants at a wider range of angles.

## 6. Conclusion

With 420 non-rotated plant images, a maximum accuracy of 79 percent was able to be reached in part by using the entire training dataset for training rather than validation, as the project this was based on did. With a significantly smaller number of images for the neural net to train on, even a 10 percent change in the available training images mattered in its implementation. Another main conclusion is the importance of resource use and choice in running CNNs. Although in theory there could only be benefits to adding more images to train, with large enough datasets, practical computing limitations become increasingly apparent. Future work would involve choosing a model that either did not require GUP allocation, or modifying the chosen images to take fewer computing resources.

## 7. Acknowledgments

Dr. Geoffrey Fox

Dr. Greggor von Laszewski 

## 8. References

[^1]: Aarhus University <https://vision.eng.au.dk/plant-seedlings-dataset/>

[^2]: Plant Seedling Classification <https://becominghuman.ai/plant-seedlings-classification-using-cnns-ea7474416e65>

[^3]: Oluwafemi Tairu <https://towardsdatascience.com/plant-ai-plant-disease-detection-using-convolutional-neural-network-9b58a96f2289>

[^4]: Pulkit Sharma <https://www.analyticsvidhya.com/blog/2019/10/building-image-classification-models-cnn-pytorch/>
