# ASD-diagnosis-DTI-and-graph-analysis
The proposed pipeline integrates:

DTI data from ABIDE-II 
Pre-processing
DTI metrics extraction
Inter-regional correlation calculation
Sparsification 
Graph-theoretical feature extraction
Machine learning-based classification

The goal is to identify robust neuroimaging biomarkers for ASD and improve diagnostic accuracy.

This study has taken the Diffusion tensor imaging data from the six sites of ABIDE-II database (https://fcon_1000.projects.nitrc.org/indi/abide/) and were pre-processed using a standard pipeline. This dataset includes total 283 subjects 154 ASD and 129 TD. Then, brain White matter (WM) was parcellated into 50 WM regions using the Johns Hopkins University WM atlas, and region wise (tracts) diffusion metrics fractional anisotropy (FA), mean diffusivity (MD), axial diffusivity (AD) and radial diffusivity (RD) were extracted. These metrics were concatenated into regional feature vectors to compute inter-regional correlation matrices using the Pearson correlation coefficient, Spearman rank correlation coefficient, and biweight midcorrelation coefficient. The correlation matrices were sparsified using hard and complementary thresholding, from which graph networks were generated and nine graph-theoretical features were extracted for each brain regions. So total 450 (50*9) features per subject. Finally, machine learning models such as LR, SVM, RF, XGBoost, and 1D-CNN were applied for automated ASD classification. 


**Logistic Regression (LR)**

This implementation of Logistic Regression is integrated within a nested cross-validation (CV) framework to ensure robust and unbiased performance evaluation. Prior to classification, features are normalized using MinMax scaling. Feature selection is performed using Random Forest-based Recursive Feature Elimination with Cross-Validation (RFECV), and the top-N ranked features (varying from 5 to 450) are evaluated.

An inner GridSearchCV is used to optimize hyperparameters such as regularization strength (C), penalty type (L1, L2, ElasticNet), and solver. The model is then evaluated on the outer test folds using multiple metrics including accuracy, precision, recall, F1-score, and ROC-AUC.

This approach allows Logistic Regression to act as both a baseline model and an interpretable classifier, while still benefiting from optimized feature subsets and hyperparameters.

**Random Forest (RF)**

The Random Forest model is used both as a feature selector and as a standalone classifier. In the feature selection stage, RF is embedded within RFECV to rank features based on importance, enabling the selection of the most informative graph-theoretical features.

When used as a classifier, Random Forest leverages its ensemble of decision trees to capture non-linear relationships and feature interactions in the high-dimensional feature space. The model is evaluated under the same nested CV framework, ensuring fair comparison with other models.

Its inherent ability to handle noisy data and provide feature importance scores makes it particularly suitable for analyzing complex brain connectivity patterns derived from DTI.

**Support Vector Machine (SVM)**

The Support Vector Machine model is implemented within the same nested cross-validation pipeline, ensuring consistency across experiments. After MinMax normalization and RF-based feature selection, SVM is trained on optimized feature subsets.

Hyperparameters such as kernel type (linear/RBF), regularization parameter (C), and kernel coefficient (gamma) are tuned using GridSearchCV in the inner loop. The model aims to find an optimal separating hyperplane that maximizes the margin between ASD and TD classes.

SVM is particularly effective for this study due to its strength in handling high-dimensional data and small sample sizes, making it well-suited for neuroimaging-based classification tasks.

**Extreme Gradient Boosting (XGBoost)**

XGBoost is implemented as an advanced ensemble learning method within the same nested CV framework, allowing fair comparison with traditional machine learning models. After preprocessing and RFECV-based feature selection, XGBoost is trained on the selected feature subsets.

Hyperparameter tuning is performed using GridSearchCV to optimize parameters such as learning rate, number of estimators, maximum tree depth, and regularization terms. XGBoost builds trees sequentially, where each new tree corrects errors from previous ones, enabling it to model complex non-linear relationships in brain connectivity data.

Its regularization mechanisms and efficient gradient boosting strategy make it highly effective for improving classification performance and reducing overfitting in high-dimensional neuroimaging datasets.

**1D Convolutional Neural Network (1D-CNN)**

A 1D-CNN model is used to learn discriminative patterns from graph-theoretical features derived from DTI data. The input features are normalized and reshaped into sequential format, enabling convolutional layers to capture local feature dependencies. The model consists of stacked Conv1D, Batch Normalization, MaxPooling, and Dropout layers followed by dense layers for classification. Performance is evaluated using 5-fold stratified cross-validation across multiple feature subsets, using metrics such as accuracy, F1-score, AUC, and specificity.
