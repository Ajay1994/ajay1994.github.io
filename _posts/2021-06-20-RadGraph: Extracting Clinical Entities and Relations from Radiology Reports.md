---
layout: post
title: "RadGraph: Extracting Clinical Entities and Relations from Radiology Reports"
date: 2021-06-20
---

#### Paper Summary
##### Written By: Ajay Jaiswal
------------------

Automated for labelling or extracting structured information from MIMIC-CXR and CheXpert involves:
### 1. Automated Radiology report labellers:
   * Chexpert: A large chest radiograph dataset with uncertainty labels and expert comparison. CoRR, abs/1901.07031, 2019 
   * Negbio: a high-performance tool for negation and uncertainty detection in radiology reports. AMIA, 2018
   * Chexpert++: Approximating the chexpert labeler for speed, differentiability, and probabilistic output. 2020
   * Chexbert: combining automatic labelers and expert annotations for accurate radiology report labeling using bert. 2020
   * Visualchexbert: addressing the discrepancy between radiology report labels and image labels. 2021
   
### 2. Extraction of more fine-grained information:
   * Enhancing the expressiveness and usability of structured image reporting systems. 2000
   * Information extraction from multi-institutional radiology reports. 2016
   * Understanding spatial language in radiology: representation framework, annotation, and spatial relation extraction from chest x-ray reports using deep learning. 2020
   * Toward complete structured information extraction from radiology reports using machine learning. 2019
   * Extracting clinical terms from radiology reports with deep learning. 2021

 The development of automated approaches for structuring large amounts of clinically relevant information in reports is primarily limited by two factors:
 * First, the choice of information extraction schema, such as the 14 medical conditions proposed by Irvin et al., limits the amount of information extracted from reports.
 * Second, there is a limited number of datasets with dense report annotations, which are expensive to obtain given the amount of time and expertise required by medical experts to procure such annotations.
