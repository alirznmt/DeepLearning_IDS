# **Network Anomaly Detection using Deep Learning**

## **📌 Overview**
This project implements **network anomaly detection** using deep learning models. The dataset used is **CICIDS2017**, and various models, including **2D CNN, ResNet-1D, and a Diffusion Model**, are trained and evaluated to detect network intrusions. The models are tested on a dataset with **15 selected network traffic features**, and the results are compared using standard classification metrics.

## **📥 Dataset Information**
This project primarily uses **CICIDS2017** dataset. To ensure a fair comparison between different models, the dataset was:
1. **Downloaded using Preprocess_CICIDS2017_Download.ipynb**  
   - Extracted only **15 key features** for training.
   - You can manually download the preprocessed CSV file from:  
     🔗 [Download Preprocessed CICIDS2017 CSV](https://drive.google.com/drive/folders/1P-k3BaHqmmvr0ZcVRuQqAyvgbyzGgD-c?usp=sharing)  

2. **Preprocessed and Normalized**  
   - Converted IP addresses to numerical values.  
   - Encoded categorical labels (attack types).  
   - Normalized numeric features using **MinMaxScaler**.  
   - Data was split into **train & test sets** and stored as pickle files.  

3. **Pickle Files for ResNet-1D & IMDiffusion**  
   - The preprocessed dataset was saved in pickle format for fair comparison between **CNN-2D (15 Features & 64 Features), ResNet-1D, and IMDiffusion.**  
   - Due to storage limitations, pickle files are **not included in this repository**.  
   - 📩 **To obtain the pickle files, please email: [alinmt94@gmail.com](mailto:alinmt94@gmail.com)**  
## **🚀 Models Implemented**
### **1️⃣ 2D CNN (15 Features)**
- Uses **an 8×8 reshaped feature matrix** for classification.
- Trained using `cnn2d_15Features(main Theme).ipynb`.

### **2️⃣ 2D CNN (64 Features)**
- Trained using **64 selected network traffic features**.
- The feature space is transformed into an **8×8 grid** before feeding into the CNN.
- This model is evaluated against the 15-feature CNN to understand the effect of additional features.
- Trained using `cnn2d_64Features.ipynb`.

### **3️⃣ ResNet-1D**
- Processes raw network traffic features **without reshaping**.
- Uses **residual connections** to improve training efficiency.
- Trained using `resnet1d_train.ipynb`.

### **4️⃣ IMDiffusion Model**
- Uses **self-supervised learning** to detect anomalies.
- Implemented in the `imdiffusion` folder.
- You can check the results of IMDiffusion in the ensemble_residual directory and the CICIDS.csv file.
- **Weights & large dataset files are not included** due to size constraints.  
  - 📩 **To request model weights, email: [alinmt94@gmail.com](mailto:alinmt94@gmail.com)**.

