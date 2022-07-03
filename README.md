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

## Project Goal

This project uses two deep learning algorithms, [Enformer](https://github.com/deepmind/deepmind-research/tree/master/enformer) and [PredixCan](https://github.com/hakyimlab/PrediXcan#:~:text=PrediXcan%20is%20a%20gene%2Dbased,be%20causal%20for%20the%20phenotype.), to predict mRNA levels from DNA sequence data. These two models are very different from each other. Knowing that PredixCan loses some prediction accuracy when the input data is non-European ancestry, we are comparing two models' prediction to gain some insights about how to improve the model in future. 

