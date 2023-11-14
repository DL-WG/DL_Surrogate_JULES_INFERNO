# Deep Learning surrogate models for speeding up JULES INFERNO forecasts

## About the project
Wildfire models are pivotal in the prevention, detection, and mitigation of wildfires. JULES-INFERNO, an empirical model forecasting wildfire emissions and burnt areas globally, faces challenges due to high data dimensionality and system complexity, making its application to fire risk forecasting with unseen initial conditions arduous. Running JULES-INFERNO for a 30-year prediction typically consumes about a week on High-Performance Computing (HPC) clusters. To address this computational bottleneck, this study introduces two data-driven models employing Deep Learning techniques to serve as surrogates for the JULES-INFERNO model, significantly accelerating global wildfire forecasting.

## Getting Started

### Hardware requirement

*   Programming language: Python (3.5 or higher)
*   Platform: Google Colab Pro
*   GPU: P100
*   Google Drive space: 100GB

### Software requirement

| Package Requirement                        |
|--------------------------------------------|
| os                                         |
| pandas                                     |
| math                                       |
| numpy                                      |
| netCDF4                                    |
| seaborn                                    |
| matplotlib                                 |
| tensorflow                                 |
| keras                                      |
| sklearn                                    |
| scipy                                      |
| joblib                                     |
| xarray                                     |
| random                                     |
| time                                       |
| skimage                                    |

## Dataset
All data for this project are from the JULES-INFERNO project. Available  at the following URL:
(https://imperiallondon-my.sharepoint.com/:f:/r/personal/mk1812_ic_ac_uk/Documents/Data/JULES-INFERNO?csf=1&web=1&e=3YXM21)

Four climate variables were taken into consideration :
 - Area burnt (The target variable)
 - Temperature
 - Soil Moisture
 - Vegetation density

Data were extracted from the outputs of 5 simulations of JULES - INFERNO.
 - Simulation 1 to 3 : P1(``LGM/output/p1``), P2(``LGM/output/p2``), P3(``LGM/output/p3``) = Models training - 1960 to 1990
 - Simulation 4 : P4(``LGM/output/p4``) = Models validation - 1960 to 1990
 - Simulation 5 : P5(``LGM/output/p5``) = Models testing - 1990 - 2020

For each simulation, the forecasts of JULES - INFERNO were monthly, annualy or daily outputed. In this suty we used the monthly outputs. Therefore for each simulations  each dataset contains 360 snapshots. Each snapshot contains multiple variables with 7771 values corresponding to land points. With the latitude and longitude variables, each environmental variable was reorganized into grid box of 112*192.

Each set was driven by the same climatic conditions (1961 to 1990). However, JULES-INFERNO applied different initial internal states to ensure the robustness of its model; the internal state of P_1 was randomized, and the initial internal states of the subsequent experimental results were all the last internal states of the previous one. Therefore, although each result was for 1961-1990, there was variability. 

## Implementation

### Processing - Compression

`Data_Processing` notebooks describes how to extract the specified features from the original data (netCDF4 files), reogranize the variables snapshots into grid box and generate CSV files for storage.

`Encoders` notebooks describes how we built the encoders to compress the data, the different methods we tried, the results we got on the data and save the best compressors.

### Models CAE - LSTM

`Models_selection_CAE_LSTM` describes the first type of models we built to predict the future forecast using a compressor and LSTM models. Those two methods were combined, and use here only the previous snapshots of the area burnt to predict the next ones. The notebooks shows the differents strategies we used and the results we got.  

`Models_selection_Multi_CAE_LSTM` is basically the same notebooks as `Models_selection_CAE_LSTM` but now the models use the four environmental variables as inputs instead of only the area burnt snapshots.  

### Models ConvLSTM

This directory contains `Models_selection_ConvLSTM` notebooks which basically the same structure of `Models CAE - LSTM` but combined into one notebook. The strategy is now applid to the second method used in this project : Convolutional LSTM.

### Fine - tuning & Predictions results

This folder show the results of the models on the validation dataset (4th simulation) `Predictions_models`. it shows the prediction with graphs, and highlights the limits of the models facing new data period.

`Fine_tuning` shows the strategy used to correct the loss of performances of the models facing new data period. It compares the models performances before and after finne-tuning on the same data. 

## Contact

Hector Chassagnon - hectorchassagnon@gmail.com<br>
Sibo Cheng - sibo.cheng@imperial.ac.uk<br>
Matthewm Kasoar - m.kasoar12@imperial.ac.uk<br>
