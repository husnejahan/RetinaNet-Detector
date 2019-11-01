# RetinaNet-Detector
RetinaNet Detector

# Introduction to Object Detection
Here, the classification and localization task contains input single image and we are supposed to identify what class does that image belong to and where is it. But in object detection, this problem gets blown on a multiple scale. There can be any number of objects in image and each object will have different size in image, for given image we have to detect the category the object belong to and locate the object.

![GitHub Logo](/images/puppy.png)

# SSD

- Pass the image through a series of convolutional layers, yielding several sets of feature maps at different scales (e.g. 10x10, then 6x6, then 3x3, etc.)
- For each location in each of these feature maps, use a 3x3 convolutional filter to evaluate a small set of default bounding boxes. These default bounding boxes are essentially equivalent to Faster R-CNN’s anchor boxes
- For each box, simultaneously predict a) the bounding box offset and b) the class probabilities
During training, match the ground truth box with these predicted boxes based on IoU. The best predicted box will be labeled a “positive,” along with all other boxes that have an IoU with the truth >0.5
- To put it simply, SSD approach is based on a feed-forward convolutional network that produces a fixed-size collection of bounding boxes and scores for the presence of object class instances in those boxes, followed by a non-maximum suppression step to produce the final detections.
![GitHub Logo](/images/ssd1.jpeg)

# Class Imbalance Problem of One-Stage Detector

- A much larger set of candidate object locations is regularly sampled across an image (~100k locations), which densely cover spatial positions, scales and aspect ratios.
- The training procedure is still dominated by easily classified background examples. It is typically addressed via bootstrapping or hard example mining. But they are not efficient enough.
![GitHub Logo](/images/ssd.png)


# Loss
- Predicting the class of the object (n class probabilities) is a classification problem. 
- Predicting the four coordinates for the bounding box is a regression problem. We need a loss function that combines these two problems.
- The loss is a weighted sum of localization loss (bounding boxes) and confidence loss (classes): We calculate how much the predicted bounding box (default box coordinates + predicted offsets) differs from the matched ground truth bounding box (L1 loss) and how correctly the default box predicted the class (binary cross entropy).
