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

This study has taken the Diffusion tensor imaging data from the six sites of ABIDE-II database (https://fcon_1000.projects.nitrc.org/indi/abide/) and were pre-processed using a standard pipeline. This dataset includes total 283 subjects 154 ASD and 129 TD. Then, brain White matter (WM) was parcellated into 50 WM regions using the Johns Hopkins University WM atlas, and region wise (tracts) diffusion metrics fractional anisotropy (FA), mean diffusivity (MD), axial diffusivity (AD) and radial diffusivity (RD) were extracted. These metrics were concatenated into regional feature vectors to compute inter-regional correlation matrices using the Pearson correlation coefficient, Spearman rank correlation coefficient, and biweight midcorrelation coefficient. The correlation matrices were sparsified using hard and complementary thresholding, from which graph networks were generated and nine graph-theoretical features were extracted for each brain regions. So total 450 (50*9) features per subject. Finally, machine learning models were applied for automated ASD classification. 
