# Data Science Project: Wind Turbine Fault Classification

## Introduction

In August of this year, I worked on my first real-world data science project after completing my master's in Big Data and AI for Society at the University of Pisa. This project not only tested the skills I gained during my studies but also allowed me to support a friend with their postgraduate thesis focused on developing a digital twin for the power and control system of a wind turbine.

A digital twin is a virtual representation of a physical system, designed to simulate and predict its behavior under various operating conditions. In this case, the digital twin aimed to model and classify potential faults in a wind turbine, enabling better maintenance strategies and improving system reliability.

The project's goal was to analyze simulated turbine data stored in a 4GB .mat file, identify key features associated with different fault types, and build a classification model to accurately predict these faults. This process involved challenges such as understanding the dataset, performing feature engineering, and selecting machine learning techniques that could align with the high accuracy demands of a digital twin.

By the end of this project, we aimed to:

1. Enhance the fault-detection capabilities of the digital twin.
2. Validate the effectiveness of the chosen techniques in replicating and predicting turbine behavior.

## Problem Context
The project focuses on classifying faults in wind turbines. My friend provided me with a dataset he simulated for his postgraduate thesis. The data aimed to replicate real-world turbine faults and included the following characteristics:

1. **27 Cases**: 
   - Case `1` represents normal operation (OK), while the remaining `26` represent different fault types.
2. **Simulation Process**:
   - Each case was simulated under varying conditions, resulting in **1,100 samples per case**.
   - For each sample, a signal was generated for **120 seconds** and then transformed using **Power Spectral Density (PSD)** to analyze its frequency components. This technique identifies the frequencies with the highest power for each fault condition.
3. **Challenges**:
   - Some simulated cases might not contain clear distinguishing features, making it uncertain whether meaningful patterns could be extracted.

This setup presented an interesting challenge: working with frequency-domain data and ensuring the model could distinguish between subtle variations in the power spectral density to accurately classify each fault type.

![classifications](https://github.com/user-attachments/assets/25558e5e-1152-4cc1-90f3-0d826411643e)

My role was to:
1. Understand and preprocess the large dataset.
2. Engineer meaningful features to improve model performance.
3. Build and evaluate models to classify turbine faults accurately.

## Data Understanding and Preprocessing

### 1. Class Distribution and Imbalance
The final dataset consisted of five selected classes, representing a total of **1,500 curves** (300 curves per class). 

The data was stratified and split into:
- **49% Training**
- **30% Testing**
- **21% Validation**

Given the inherent class imbalance, strategies like **undersampling** and **stratified k-fold cross-validation** were considered to ensure fair model evaluation.

### 2. Data Transformation
The data presented a logarithmic distribution, which required specific transformations to stabilize variance and reduce skewness. For this, I applied the PowerTransformer using the Yeo-Johnson method: This transformation ensured that the model could better interpret the variations across features without losing critical information contained in the curves.

### 3. Feature Engineering and Dimensionality Reduction
Feature engineering and selection were fundamental to improve model performance.
1. Welch Method: Used to extract features that reflected variations in signal power due to simulated faults.
2. Correlation Filtering: Removed 89 columns with correlations greater than 0.8 or less than -0.8 to avoid redundancy and multicollinearity.
After these steps and with the approval of the engineer in charge of the project, I retained a final set of 10 features, which captured the most meaningful variations in the signal.

## Modeling: Support Vector Machine (SVM)
To classify the faults, I selected Support Vector Machine (SVM) due to its effectiveness in handling high-dimensional datasets and non-linear relationships. 

The process involved:
Defining a grid of hyperparameters for optimization using Bayesian Search:

![image](https://github.com/user-attachments/assets/e07dea6f-84dd-4a1d-87e1-f2a57e398368)

Performing stratified cross-validation with scikit-optimize:

![image](https://github.com/user-attachments/assets/8f8ef117-2dc7-4ccf-9c8d-d92ebc97ee56)


## Model Results
After training, the SVM achieved the following results on the test set:

Accuracy: 91.78%

![image](https://github.com/user-attachments/assets/92b754ed-41c8-412b-afd1-8c1febc146b7)

The RBF kernel performed the best, balancing accuracy and stability across the folds.

## Final correlation matrix for selected classes
![image](https://github.com/user-attachments/assets/eb8d2882-80ca-43ef-b46c-60b68751db3a)


## Lessons Learned
1. Distribution Awareness: Ignoring the underlying distribution can lead to suboptimal models. The use of PowerTransformer was crucial to ensure that the features were normalized and ready for modeling.
2. Hyperparameter Tuning: Bayesian search efficiently optimized the model, highlighting the significance of carefully selecting hyperparameters like C, gamma, and kernel.

## Final Thoughts
This experience marked a turning point in my journey as a data scientist. Not only did I gain technical skills, but I also learned how to navigate ambiguity and solve problems creatively. Most importantly, I contributed to a project that had real-world significance.

If you want to read the thesis with all the information please feel free to read my friends publication:

http://hdl.handle.net/10017/63573
