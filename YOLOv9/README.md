# YOLOv9

The paper introduces **Programmable Gradient Information (PGI)** and a new network architecture called **Generalized Efficient Layer Aggregation Network (GELAN)** to address data loss issues in deep learning networks during feature extraction and transformation. PGI is designed to preserve complete input information for reliable gradient calculations, enhancing weight updates. GELAN utilizes conventional convolution over depth-wise convolution for better parameter efficiency.
![yolov9_pgi-768x401](https://github.com/Thireshsidda/LegacyOfYOLO-YouOnlyLookOnce/assets/92287626/ad7cfd1f-f325-4401-ad10-f6d8f4ebee89)

**Figure 1: Programmable Gradient Information (PGI)**

The issue of information loss, as described by the Information Bottleneck Principle, is significant in deep neural networks, leading to unreliable gradients and poor convergence. The paper suggests reversible functions to maintain information integrity throughout the network layers, improving training effectiveness.

PGI integrates a main inference branch, an auxiliary reversible branch, and multi-level auxiliary information. The reversible branch helps retain complete data information, reducing the risk of false correlations and enhancing parameter learning without increasing inference costs, as it can be omitted during inference. The multi-level auxiliary information integrates gradient information across various prediction branches, ensuring comprehensive learning across different targets.

Experimentally, YOLOv9, utilizing GELAN and PGI, outperforms other real-time object detectors in terms of accuracy and efficiency. It surpasses models that use depth-wise convolution or rely on ImageNet pretraining. The success of YOLOv9 is attributed to its superior parameter utilization and the ability of PGI to retain crucial information for effective data-target mapping.

Furthermore, CSP blocks within GELAN enhance performance with fewer parameters, allowing for a flexible yet stable architecture design. PGI’s integration with deep supervision concepts also shows marked improvements, particularly in deep models.

## Training YOLOv9
