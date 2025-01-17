---
title: "GM-UNet: GAN-Based VM-UNet Achieves Higher Accuracy"
collection: publications
category: conferences
permalink: /publication/2024-VM-UNet
date: 2024-8-17
excerpt: 'In the field of medical image segmentation, the Ushape architecture model has been widely explored. Recently, a novel architecture called Mamba, based on the State Space Models (SSMs), has emerged. Subsequently, a Visual State Space (VSS) block based on visual Mamba has also become a new core module of Ushape architecture models represented by VM-UNet. Although VSS block possesses linear computational complexity, it constructs only four directional linear sequences when processing images, thus inherently lacking the capability to capture local features of the target image effectively. To enhance the capability of VM-UNet in capturing image details, this paper introduces a GM-UNet model, which integrates VM-UNet into the framework of Generative Adversarial Networks (GANs): using VM-UNet as the generator network for image segmentation and employing the encoder of U-Net as the discriminator network. By incorporating the image features extracted by the discriminator and introducing weighted adversarial loss into the network training, the network is trained until the discriminator struggles to distinguish the source of segmented images, thereby obtaining an image segmentation network with superior capability in capturing image details. Experimental results on segmenting liver CT images demonstrate that the GM-UNet model outperforms VM-UNet in various performance metrics and exhibits strong competitiveness compared to other classic segmentation models based on Ushape architecture.'
venue: '2024 11th International Conference on Behavioural and Social Computing (BESC)'
paperurl: 'https://ieeexplore.ieee.org/abstract/document/10780733'
citation: 'J. Han, J. Cai, M. Zhu, Z. Gao, C. Li and Y. Miao, "GM-UNet: GAN-Based VM-UNet Achieves Higher Accuracy," 2024 11th International Conference on Behavioural and Social Computing (BESC), Harbin, China, 2024, pp. 1-6, doi: 10.1109/BESC64747.2024.10780733.'
---

The contents above will be part of a list of publications, if the user clicks the link for the publication than the contents of section will be rendered as a full page, allowing you to provide more information about the paper for the reader. When publications are displayed as a single page, the contents of the above "citation" field will automatically be included below this section in a smaller font.
