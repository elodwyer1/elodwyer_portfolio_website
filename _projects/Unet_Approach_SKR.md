---
title: "Detecting Saturn's Low Frequency Extensions (LFEs) Using Machine Learning"
layout: single
classes: wide
author_profile: true
header:
  caption: " "
  overlay_image: /assets/images/banner_notext_nodesign.png
---

#  Detecting Saturn's Low Frequency Extensions (LFEs) Using Machine Learning
Saturn, with its intricate magnetospheric dynamics, continues to captivate scientists worldwide. Low Frequency Extensions (LFEs) in Saturn's radio emissions offer a unique window into its magnetospheric processes. Detecting and categorizing these events accurately will permit us to 

## Introduction

In a study recently published in the *Journal of Geophysical Research: Space Physics*, we applied a sophisticated machine learning approach to identify and classify LFEs within the extensive 13-year dataset collected by the Cassini spacecraft's Radio and Plasma Wave Science (RPWS) instrument. LFEs are an intensification and extension in frequency range of Saturn's auroral radio emission. These events are linked to Saturn's internal magnetosphere dynamics and the effect of the solar wind compressing the magnetosphere. This study leveraged a modified U-Net neural network architecture, specifically tailored for semantic segmentation tasks, to analyze radio spectrograms and distinguish LFEs from background noise with unprecedented precision.

## Methodology

### Data Acquisition and Preprocessing

The research utilized a curated dataset consisting of 2,044 radio spectrogram images captured by Cassini. We are using a catalog detailed in [O’Dwyer et al. (2023)](https://doi.org/10.25546/103103) which consists of a selection of LFEs detected by Cassini using a polygon selector tool for spectrograms [(Louis et al., 2022)](https://www.frontiersin.org/articles/10.3389/fspas.2022.1001166). This catalog consists of the polygon coordinates of the LFE as shown in the figure below. These images were annotated to indicate the presence or absence of LFEs with a binary mask. The dataset was divided into training (996 images), validation (537 images) and testing (511), ensuring robust model training and evaluation. To enhance model generalization, data augmentation techniques were employed, .
![model_architecture]({{'assets/images/Examples-of-the-Low-Frequency-Extension-LFE-left-hand-panels-and-NoLFE_W640' | relative_url }}){: .align-center}

The inputs to the model consist of images with seven channels in total: this corresponds to an array with dimensions
384 × 128 × 7. The first two channels comprise spectrogram images showing flux density and normalized
degree of polarization. The degree of circular polarization was included as a channel along with flux density as
it allows for the differentiation between SKR and non-cyclotron maser generated narrowband emissions centered
at 5 kHz also present in the radio observations. The 5 kHz component is more weakly circularly polarized than
the SKR, and has opposite polarization sense compared to SKR when observed from the same hemisphere.
Four channels contain spacecraft trajectory information; spacecraft median latitude, spacecraft latitude standard
deviation, spacecraft local time median and spacecraft local time standard deviation over the time range the spectrogram
occurs. Each of these four parameters contains only a single value which is repeated at each point in the
frequency × time array. The final channel is a 2-D array comprising the corresponding frequency channel that
was sampled in the flux density and degree of polarization array, repeated at each time-point. This channel gives
the convolution access to input coordinates of the LFE events in the y (frequency) dimension. The values in each
channel array are normalized to (0,1) interval.

### Model Architecture and Training

The heart of the study lies in the custom U-Net architecture, renowned for its effectiveness in image segmentation tasks. The achitechture used is drawn from the Unet structure but optimised to our unique purposes, see the figure below for the exact structure. The model's convolutional layers were initialized using Glorot uniform weights, and training spanned 132 epochs with mini-batch sizes of 8 images. During training, model performance was validated using the validation set, with weights updated iteratively to optimize predictions. 


![model_architecture]({{'assets/images/Modified-U-Net-architecture.png' | relative_url }}){: .align-center}

###  Model Training Details

The table below summarizes the key aspects of our model training, including data distribution across training, validation, and test sets, as well as the specific hyperparameters used in the training process.

| Metric           | Value                     |
|------------------|---------------------------|
| **Train count**  | 996 (49%)                 |
| **Validation count** | 537 (26%)             |
| **Test count**   | 511 (25%)                 |
| **Loss function**| Binary cross entropy      |
| **Optimizer**    | Adam                      |
| **Learning rate**| 1 × 10<sup>−5</sup>       |
| **Batch size**   | 8                         |
| **Epochs**       | 132                       |

*Note: The percentage values indicate the proportion of the total dataset for training, validation, and testing.*


### Performance Evaluation

The efficacy of the model was assessed using Intersection over Union (IoU) metrics, measuring the overlap between predicted and ground truth masks. Median IoU values of 0.97 for the testing set and 0.98 for the training set underscored the model's exceptional accuracy in identifying LFEs within Saturn's radio emissions. Examples of both well classified and poorly classified images are seen below.
![model_architecture]({{'assets/images/Examples-of-well-classified-results.png' | relative_url }}){: .align-center}
![model_architecture]({{'assets/images/Examples-of-poorly-classified-results.png' | relative_url }}){: .align-center}
## Results and Insights

### Output and Catalog Generation

Following training and validation, the model was deployed to analyze the entire Cassini/RPWS dataset, comprising over a decade of radio spectrograms. The output yielded a comprehensive catalog of 4,874 detected LFEs. These events were categorized into three types based on their duration and frequency extension: "LFE," "LFEm," and "LFEsm," each providing unique insights into Saturn's magnetospheric activities.

### Frequency-Time Analysis

Detailed analysis of the frequency-time coordinates of predicted LFEs revealed intriguing patterns. Comparisons with training LFEs showed a consistent linear relationship between frequency extension (Δf) and duration (Δt), affirming the model's capability to capture and classify varying types of LFEs with high accuracy.
![model_architecture]({{'assets/images/Scatter-plots-of-delta-f-in-kHz-versus-delta-t-in-hours-for-a-Low-Frequency.png' | relative_url }}){: .align-center}

### Scientific Implications

The study's findings offer significant implications for our understanding of Saturn's magnetospheric dynamics. By providing a detailed catalog of LFEs and their precise coordinates, researchers worldwide can now delve deeper into statistical studies and explore the breadth of radio signatures arising from complex interactions within Saturn's magnetosphere. In the image below, we see examples of the model's excellent classifications of LFEs on unseen data.
![model_architecture]({{'assets/images/Six-examples-of-models-prediction-on-unseen-data-from-the-Cassini-Radio-and-Plasma-Wave.png' | relative_url }}){: .align-center}

## Conclusion

This study represents an application of machine learning techniques to planetary science, unlocking new avenues for research into Saturn's enigmatic radio emissions. The availability of the LFE catalog in Time Frequency Catalogue (TFCAT) format on [Zenodo](https://zenodo.org/record/8314868) facilitates open access and collaborative exploration within the scientific community.

### Availability of Data and Code

For transparency and reproducibility, the research code and dataset details are openly accessible on [GitHub](https://github.com/elodwyer1/Unet_Application_to_Saturn_Kilometric_Radiation). Researchers and enthusiasts alike are encouraged to explore and build upon this work to further unravel the mysteries of Saturn's magnetosphere.

Stay tuned for future updates as we continue to unravel the secrets of Saturn's radio emissions using advanced data science and machine learning techniques.

