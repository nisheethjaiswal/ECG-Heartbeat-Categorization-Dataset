# ECG Heartbeat Categorization
This project examines tree models, neural networks and a sequence model combining convolutional and LSTM layers for classifying ECG heartbeat signals using the MIT-BIH arrhythmia dataset avbvailable on Kaggle. 
The project is stuctured in four notebooks and an architectural decision document for easier collaboration and reuse. 
#### * EDA - Initial Data Exploration
#### * ETL - Extract, Transform, Load
#### * Feature Engineering
#### * Model Definition/Training/Evaluation


### Dataset: https://www.kaggle.com/shayanfazeli/heartbeat
Arrhythmia Dataset

    Number of Samples: 109446
    Number of Categories: 5
    Sampling Frequency: 125Hz
    Data Source: Physionet's MIT-BIH Arrhythmia Dataset
    Classes: ['N': 0, 'S': 1, 'V': 2, 'F': 3, 'Q': 4]

## Models
* Decision Tree
* Random Forest
* XGBoost
* Small Neural Network
* Sequeunce Model

# Sequence Model Architecture
![model_arch](https://user-images.githubusercontent.com/64613694/128560026-7ea3a892-081a-4ec5-a22d-966dafd45038.png)


## Reference for sequence model: https://arxiv.org/abs/1801.10033 
#### Cardiac Arrhythmia Detection from ECG Combining Convolutional and Long Short-Term Memory Networks
Philip Warrick (1), Masun Nabhan Homsi (2) ((1) PeriGen. Inc., Montreal, Canada, (2) Simon Bolivar University, Caracas, Venezuela)

