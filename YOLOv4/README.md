# YOLOv4

## Paper Summmary
Alexey Bochkovskiy collaborated with the authors of CSPNet(Nov 2019) Chien-Yao Wang and Hong-Yuan Mark Liao, to develop YOLOv4. The only similarity between YOLOv4 and its predecessors was that it was built using the Darknet framework. They experimented with many new ideas and later published them in a separate paper, [Scaled-YOLOv4](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=ljmswJ0AAAAJ&citation_for_view=ljmswJ0AAAAJ:zYLM7Y9cAGgC).

![Screenshot-from-2023-12-26-13-22-05-768x225](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/27034daa-23c9-49b9-9210-1a1044dc2d14)

**Figure 1: One-stage VS. Two-stage detector architecture comparison** [(Source)](https://arxiv.org/abs/2004.10934)

Generally, an object detector comprises an ImageNet pre-trained backbone for feature extraction and a Head for doing the bounding box regression and classification. But, from YOLOv3 and YOLOv4, the authors introduced another part of the structure called the `Neck`, placed between the backbone and the head. The use of this block is to collect features of different stages from the backbone and pass them to the head. In YOLOv4, we had `CSPDarkNet53` as the backbone, Spatial Pyramid Pooling (SPP), and Modified PANet as the Neck and the YOLOV3 head. In the paper, they introduced two concepts: `bag of freebies` and `bag of specials`. The concept of “Bag of Freebies” refers to techniques that modify the training approach or raise the training cost without affecting the cost during inference. Data augmentation is a typical example of such a method. In contrast, “Bag of Specials” includes methods that marginally increase the cost at inference time but substantially enhance accuracy. These methods involve expanding the receptive field, integrating features, and various post-processing techniques. Specific examples like `SPP (Spatial Pyramid Pooling)` and `PANet (Path Aggregation Network)` were elaborated on in the YOLOv5 section.

**Cross Stage Partial Network** was designed for faster inference while keeping the accuracy of the original model. It is an additional component that can be integrated into your existing setup. The concept behind CSPNet is that architectures like ResNet and DensNet have many skip connections. A large amount of gradient information is reused during the gradient updation of different layers, which results in different layers repeatedly learning copied gradient information. To stop this duplicate gradient, CSPNet divides the input into two parts, one passes through the usual in-between layer, and the 2nd part gets concatenated after that block for further operations. 

![Screenshot-from-2023-12-26-13-23-43](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/70564a41-ca8d-4a9c-8aef-797dbdfbc987)


**Figure 2: Different kinds of feature fusion strategies in CSPNet**  [(Source)](https://arxiv.org/abs/1911.11929)

The authors experimented with many architectures such as ResNet50, ResNext50, VGG16, DarkNet53, etc., but later, a variant of DarkNet53 with the CSPNet was used for the backbone. Numerous studies demonstrated that the CSPResNext50 was better than CSPDarkNet53 for object classification. However, through experimentation, they discovered that the `CSPDarkNet53` is more suited for Object Detection tasks.

They introduced `Spatial Pyramid Pooling(SPP)` as in YOLOV3-SPP for the multi-scale feature pooling. They use PANet for the feature pyramid instead of FPN from the YOLOV3 model. In the literature, two types of attention modules used in an object detector are channel-wise attention and point-wise attention. `Spatial Attention Module (SAM)` and Squeeze-and-Extraction are examples, respectively. SAM is used in YOLOv4 because it improves accuracy and decreases the inference latency. Finally, for the detection head, they use anchors as in YOLOv3. Above mentioned integrations are a part of the bag of specials, where CSPNet cuts the computations cost while maintaining the accuracy, SPP block helps to increase the receptive field, and PANet helps for better feature aggregation. 

As a `bag of freebies` method, the authors introduced the mosaic data augmentation, which combines 4 images in one image. By doing that, they add contextual information and decrease the mini-batch size for batch normalization. They also used MixUp and CutMix alongside other geometrical augmentations except for mosaic. They used cIOU loss and cross mini-batch normalization for better detection, replacing normal Dropout with the DropBlock. 

![Screenshot-from-2023-12-26-13-26-24](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/70598099-1eb4-47ea-94f1-6617be32c323)

**Figure 10: Different Augmentation techniques used in YOLOv4** [(Source)](https://arxiv.org/abs/1911.11929)


## Training YOLOv4

Custom training of YOLOv4 on pothole detection involves using YOLOv4 and the darknet framework. The dataset contained 1265 training images, 401 validation images, and 118 validation images. 








