# YOLOv4

## Paper Summmary
Alexey Bochkovskiy collaborated with the authors of CSPNet(Nov 2019) Chien-Yao Wang and Hong-Yuan Mark Liao, to develop YOLOv4. The only similarity between YOLOv4 and its predecessors was that it was built using the Darknet framework. They experimented with many new ideas and later published them in a separate paper, [Scaled-YOLOv4](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=ljmswJ0AAAAJ&citation_for_view=ljmswJ0AAAAJ:zYLM7Y9cAGgC).

![Screenshot-from-2023-12-26-13-22-05-768x225](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/27034daa-23c9-49b9-9210-1a1044dc2d14)

**Figure 1: One-stage VS. Two-stage detector architecture comparison** [(Source)](https://arxiv.org/abs/2004.10934)

Generally, an object detector comprises an ImageNet pre-trained backbone for feature extraction and a Head for doing the bounding box regression and classification. But, from YOLOv3 and YOLOv4, the authors introduced another part of the structure called the `Neck`, placed between the backbone and the head. The use of this block is to collect features of different stages from the backbone and pass them to the head. In YOLOv4, we had `CSPDarkNet53` as the backbone, Spatial Pyramid Pooling (SPP), and Modified PANet as the Neck and the YOLOV3 head. In the paper, they introduced two concepts: `bag of freebies` and `bag of specials`. The concept of “Bag of Freebies” refers to techniques that modify the training approach or raise the training cost without affecting the cost during inference. Data augmentation is a typical example of such a method. In contrast, “Bag of Specials” includes methods that marginally increase the cost at inference time but substantially enhance accuracy. These methods involve expanding the receptive field, integrating features, and various post-processing techniques. Specific examples like `SPP (Spatial Pyramid Pooling)` and `PANet (Path Aggregation Network)` were elaborated on in the YOLOv5 section.

