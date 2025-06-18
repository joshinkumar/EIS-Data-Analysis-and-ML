EIS to Pathogen Concentration Prediction
This Python script processes Electrochemical Impedance Spectroscopy (EIS) data stored in .PSSESSION files from a PalmSens potentiostat, generates synthetic data to augment the dataset, trains a Random Forest (RF) model to predict pathogen concentrations, and performs SHAP analysis to identify important features. The code is designed for biosensing applications, specifically for detecting H5N1 concentrations using capacitance features (CRe/F and -CIm/F).
Features

Parses .PSSESSION Files: Extracts EIS data (frequency, impedance, capacitance) from PalmSens .PSSESSION files, which are JSON-formatted and UTF-16 encoded.
Synthetic Data Generation: Uses Gaussian Process Regression (GPR) to create realistic synthetic EIS spectra for non-blank and blank samples, addressing small dataset limitations.
Machine Learning: Trains an RF regressor to predict pathogen concentrations (log10 scale) from capacitance features.
Feature Importance: Applies SHAP analysis to identify the most influential frequencies for CRe/F and -CIm/F.
Visualization: Generates SHAP summary plots to visualize feature importance.

Dataset

Input: 27 .PSSESSION files (9 H5N1 concentrations × 3 replicates) stored in a specified directory.
Features: Real (CRe/F) and negative imaginary (-CIm/F) capacitance at 51 frequencies, yielding 102 features per sample.
Target: Log10 of H5N1 concentration (10^3x to 10^10x dilutions of H5N1 stock of 2x10^10 copies/ml) for non-blank samples; 0 for blank (PBS) samples.
File Naming: Files follow the format H5N1_10^{exponent}x for non-blanks or Blank_PBS for blanks.

Requirements

Python 3.11+
Libraries:pip install numpy pandas scikit-learn shap matplotlib


.PSSESSION files from a PalmSens potentiostat, stored in a local directory.

Installation

Clone the repository:git clone https://github.com/your-username/eis-to-concentration.git
cd eis-to-concentration


Install dependencies:pip install -r requirements.txt


Place .PSSESSION files in the directory specified in the script (default: C:\Users\joshi\Desktop\...\ALL_DATA).

Usage

Update the directory path in the script to point to your .PSSESSION files.
Run the script:python ML_Project_EIS2Conc.py


Outputs:
Console: Test Mean Squared Error (MSE) for the RF model.
File: shap_summary_plot.png showing feature importance via SHAP analysis.



Code Structure

Imports: Loads required libraries (numpy, pandas, scikit-learn, shap, matplotlib).
EIS Data Extraction: extract_eis_data function parses .PSSESSION files, computes capacitance (CRe/F, -CIm/F) from impedance data.
Concentration Extraction: get_concentration extracts H5N1 concentrations from filenames.
Data Preparation: Concatenates CRe/F and -CIm/F into feature vectors, assigns log10(concentration) targets.
Synthetic Data: Generates 100 non-blank and 20 blank synthetic samples using GPR for non-blanks and multivariate normal sampling for blanks.
Model Training: Trains an RF regressor (n_estimators=1000) on combined original and synthetic data.
Evaluation: Computes MSE on a test set (9 samples).
SHAP Analysis: Generates feature importance plots using SHAP.

Notes

Unique Contribution: The extract_eis_data function is tailored to handle .PSSESSION files, which are not widely supported in open-source tools, making this code valuable for PalmSens users.
Customization: Adjust n_synthetic_non_zero and n_synthetic_blanks for different synthetic data sizes.
Validation: Add Cole-Cole plots (plt.plot(X[:, :51], X[:, 51:])) to verify synthetic data quality.
Limitations: Assumes consistent file naming and 51 frequency points per .PSSESSION file.

License
MIT License. See LICENSE for details.
Acknowledgments
Developed for biosensing research at Washington University in St. Louis. Contact for support or contributions.
