# Capstone-Project

## From DNA sequence to Gene Expression Prediction

_Under instructions of professor Hae Kyung Im from University of Chicago._

### Author

- Zhongyu (Nick) Feng
- Yuejun Han

### Acknowledgement:

- Boxiang Liu (for teaching us deep learning)
- Festus Nyasimi (for providing us with Predixcan predictions)
- Saideep Gona and Temidayo Adeluwa (for helping us putting this script together) 

Copyright 2021 DeepMind Technologies Limited

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

     https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

**"Effective gene expression prediction from sequence by integrating long-range interactions"**

Žiga Avsec, Vikram Agarwal, Daniel Visentin, Joseph R. Ledsam, Agnieszka Grabska-Barwinska, Kyle R. Taylor, Yannis Assael, John Jumper, Pushmeet Kohli, David R. Kelley

## Project Introduction

This project uses two deep learning algorithms, [Enformer](https://github.com/deepmind/deepmind-research/tree/master/enformer) and [PredixCan](https://github.com/hakyimlab/PrediXcan#:~:text=PrediXcan%20is%20a%20gene%2Dbased,be%20causal%20for%20the%20phenotype.), to predict mRNA levels from DNA sequence data. These two models are very different from each other. PrediXcan is an approach that makes use of the reference transcriptome data sets to do statistical training of prediction based on the common gene variations in hundreds or thousands of individuals. This trained model is then employed to predict gene expression and to map trait-associated loci. Another approach is Enformer, which is developed upon a Neural Network architecture based on self-attention mechanism to predict gene expression at target chromosomes or specific tissues. Making predictions based on the DNA sequence only, this method is able to detect both known and unknown trait-associated variants. To learn more about them, you can also check out their article: [PrediXcan article](https://www.nature.com/articles/ng.3367), [Enformer article](https://www.nature.com/articles/s41592-021-01252-x)

## Project Goal

Our project objective is to use the developer-tuned version of the Enformer model and PrediXcan model to make gene expression predictions. We compare two models’ performance on target genes across individuals from two populations: one group of individuals with African ancestry and the other with European ancestry. The input data is obtained from the publicly available [Geuvadis dataset](https://www.internationalgenome.org/data-portal/data-collection/geuvadis), which contains gene expression measurements in LCL cell lines and DNA sequence data from more than 500 individuals across 5 different populations. 
