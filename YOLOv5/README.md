# YOLOv5

## Paper Summary
![image-20231219163621511-768x758](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/22841057-63f2-4291-8a27-4202cf7a38d6)

## Model Architecture:
The YOLOv5 model is divided into 3 parts, Backbone, Neck, and Head. Each convolution is followed by batch normalization (BN) and SiLU activation. Below is the 3000-foot overview of the YOLOv5 Model Architecture
- Backbone: **CSP-DarkNet53**
- Neck: **SPPF and CSP-PANet**
- Head: **YOLOv3 Head**
