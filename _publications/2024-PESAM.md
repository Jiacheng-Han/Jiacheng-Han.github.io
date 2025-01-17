---
title: "PESAM: Privacy-Enhanced Segment Anything Model for Medical Image Segmentation"
collection: publications
category: conferences
permalink: /publication/2024-PESAM
excerpt: 'PESAM proposed integrates federated learning with SAM to enable collaborative optimization of medical image segmentation models while preserving data privacy, using a dynamic local aggregation algorithm to enhance performance and generalizability across heterogeneous data.'
date: 2024-08-01
venue: 'Advanced Intelligent Computing Technology and Applications (ICIC 2024)'
paperurl: 'https://link.springer.com/chapter/10.1007/978-981-97-5581-3_8'
---

The Segment Anything Model (SAM) has garnered significant attention due to its superior zero-shot generalization ability. However, the development of SAM in medical image segmentation continues to confront challenges related to data sharing restrictions imposed by data privacy protection regulations. Federated learning (FL), a distributed learning approach, allows multiple participants to collaboratively train shared models without sharing data. Therefore, integrating FL with SAM not only facilitates the training of SAM while safeguarding the privacy of medical image data but also permits SAM to assimilate a broader spectrum of feature information through the use of multi-source data. However, the extensive parameter set of SAM, combined with the data heterogeneity of medical images, significantly affects the model's segmentation performance during FL training. Therefore, we introduce PESAM, the inaugural federated medical image segmentation large-scale model method, enabling multiple healthcare organizations to collaboratively optimize SAM without consolidating patient data. We present the local aggregation algorithm (PESAM-LA). Specifically, PESAM deeply explores the parameter differences between federated aggregation after and before local update, dynamically modulating the aggregation strategy in response to this pivotal change. This adaptation enables PESAM to maximize the utility of local data resources at each node, thereby significantly enhancing the model's performance and generalizability. We conducted comprehensive experiments on liver and gallbladder segmentation tasks to confirm PESAM's effectiveness.
