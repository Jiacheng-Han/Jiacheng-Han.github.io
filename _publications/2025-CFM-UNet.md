---
title: "CFM-UNet: coupling local and global feature extraction networks for medical image segmentation"
collection: publications
category: manuscripts
permalink: /publication/2025-CFM-UNet
date: 2025-07-01
venue: 'Scientific Reports'
excerpt: 'CFM-UNet proposed integrates CNN-based Bottle2neck blocks for local feature extraction and Mamba-based visual state space blocks for global feature extraction. These parallel frameworks perform feature fusion through our designed SEF block, achieving complementary advantages'
paperurl: 'https://www.nature.com/articles/s41598-025-92010-y'
citation: 'Niu, K., Han, J. & Cai, J. CFM-UNet: coupling local and global feature extraction networks for medical image segmentation. Sci Rep 15, 22236 (2025). https://doi.org/10.1038/s41598-025-92010-y'
---

In medical image segmentation, traditional CNN-based models excel at extracting local features but have limitations in capturing global features. Conversely, Mamba, a novel network framework, effectively captures long-range feature dependencies and excels in processing linearly arranged image inputs, albeit at the cost of overlooking fine spatial relationships and local pixel interactions. This limitation highlights the need for hybrid approaches that combine the strengths of both architectures. To address this challenge, we propose CNN-Fusion-Mamba-based U-Net (CFM-UNet). The model integrates CNN-based Bottle2neck blocks for local feature extraction and Mamba-based visual state space blocks for global feature extraction. These parallel frameworks perform feature fusion through our designed SEF block, achieving complementary advantages. Experimental results demonstrate that CFM-UNet outperforms other advanced methods in segmenting medical image datasets, including liver organs, liver tumors, spine, and colon polyps, with notable generalization ability in liver organ segmentation.
