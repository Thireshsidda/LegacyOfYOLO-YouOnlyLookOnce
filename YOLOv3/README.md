# YOLOv3

## Paper Summary
In YOLOv2, the model predicts the object class for each grid. But, in YOLOv3, the model predicts class for each bounding box predicted. The model predicts 3 bounding boxes for each grid, objectness score, and class predictions. At the output side, the tensor is of  $\small N \times N \times (3 \times (4 + 1 + 80))$. The model outputs bounding boxes at three different scales. 

YOLOv3 uses multiple independent logistic classifiers rather than one softmax layer for each class. During training, they use binary cross-entropy loss in a `one vs. all setup`.
