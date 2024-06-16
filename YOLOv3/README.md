# YOLOv3

## Paper Summary
In YOLOv2, the model predicts the object class for each grid. But, in YOLOv3, the model predicts class for each bounding box predicted. The model predicts 3 bounding boxes for each grid, objectness score, and class predictions. At the output side, the tensor is of  $\small N \times N \times (3 \times (4 + 1 + 80))$. The model outputs bounding boxes at three different scales. 

YOLOv3 uses multiple independent logistic classifiers rather than one softmax layer for each class. During training, they use binary cross-entropy loss in a `one vs. all setup`.

YOLOv3 uses the `DarkNet-53` as a backbone for feature extraction. The architecture has `alternative 1×1 and 3×3 convolution` layers and skip/residual connections inspired by the ResNet model. They also added the idea of `FPN` to leverage the benefit from all the prior computations and fine-grained features early on in the network. Although DarkNet53 is smaller than ResNet101 or RestNet-152, it is faster and has equivalent or better performance.

Similar to YOLOv2, YOLOv3 also uses k-means to find the bound box before the anchors. In this model, they used three prior boxes for different scales, unlike YOLOv2.
