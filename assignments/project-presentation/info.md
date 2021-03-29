# Using AI to Detect Plants and Weeds

Paula Madetzke

## Problem

Weed identification is an important component of agriculture, and can effect the way utilize herbicide. When unable to locate weeds in a large field, farmers are forced to blanket utilize herbicide for weed control. However, this method is bad for the environment, as the herbicide can leech into the water, and bad for the farmer, because they then must pay for far more fertilizer than they really need to control weeds.

## Dataset and Algorithm

This project utilizes images from the Aarhus University [^1] dataset to train a CNN to identify images of 12 species of plants. To better simulate actual rows of crops, a subset of the images for testing will be arranged in a list representing a crop row, with weeds being distributed in known locations. Then, the AI will be tested on the row, and should be able to determine where in the row the weeds are located.

## Existing Efforts

Previous similar work has been done with this dataset [^2] in Keras with CNN image recogniton, while this project is implemented in pytorch. Similar agricultural image recognition with plant disease [^3] is also available to be studied.

## Estimated Timeline

* Week 1: Compile, organize and clean data
* Week 2-3: Implement CNN to train AI with majority of data/test subset of it
* Week 3-4: Create and test designed crop "rows", visualize results
* Week 5: Write Report




## Resources

[^1]: Aarhus University <https://vision.eng.au.dk/plant-seedlings-dataset/>

[^2]: Plant Seedling Classification <https://becominghuman.ai/plant-seedlings-classification-using-cnns-ea7474416e65>

[^3]: Oluwafemi Tairu <https://towardsdatascience.com/plant-ai-plant-disease-detection-using-convolutional-neural-network-9b58a96f2289>