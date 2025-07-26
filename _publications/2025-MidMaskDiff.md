---
title: "MedMaskDiff: Mamba-Based Medical Semantic Image Synthesis for Segmentation"
collection: publications
category: conferences
permalink: /publication/2025-MedMaskDiff
date: 2025-07-26
venue: 'Advanced Intelligent Computing Technology and Applications'
excerpt: 'MedMaskDiff proposed is a Mamba-based semantic image synthesis model that generates medical images from masks, which utilizes an evolutionary condition-guide method to enhance the quality and medical logic of the generated target regions.'
paperurl: 'https://link.springer.com/chapter/10.1007/978-981-95-0030-7_16'
---

To protect patient privacy, the ability of diffusion models to generate medical images from noise has become a key focus for enriching datasets. However, due to the high precision required for medical image anatomical structures, generative models designed for natural scenes fail to meet the stringent standards for medical logic. We propose MedMaskDiff, a Mamba-based semantic image synthesis model that generates medical images from masks, which takes full advantage of Mamba's capability to capture long-range medical features. Additionally, it utilizes an evolutionary condition-guide method to enhance the quality and medical logic of the generated target regions. MedMaskDiff outperforms other advanced methods in synthesizing liver CT, thyroid nodule ultrasound, and low-grade intraepithelial neoplasia microscopy images. By utilizing masks from other liver CT datasets for semantic synthesis and data augmentation, comparative experiments demonstrate that MedMaskDiff effectively safeguards patient privacy while enhancing downstream medical image segmentation tasks, significantly improving the performance of segmentation models. 
