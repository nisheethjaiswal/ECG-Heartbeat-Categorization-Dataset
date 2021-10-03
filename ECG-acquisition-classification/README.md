# Single Lead ECG Signal acquisition and classification using Deep Convolutional Networks
Single Lead ECG signal Acquisition and Arrhythmia Classification using Deep Learning

This project contain two parts,

1. ECG signal Acquasition

2. Arrhythmia classification of the acquired ECG signal.

## Files

- [`ecg`](ecg) : This folder contains all the files for training and testing of the deep learning algorithm and this code will be used to classifiy the acquired signals. The code in this folder is developed by the awesome team of Awni et al, StanfordML Group. I have just modified it to work with python3 and few other changes for my comfort of use.

- [`arduino.ino`](arduino.ino) : Aurdino code for the 4th order Notch filter, displaying ECG signal in computer, calculating instantanious BPM, etc.

- [`classification.ipynb`](classification.ipynb)(*Not yet tested*) : Code to automaticaly create a file of the acquired ECG signal by reading from the serial moniter, convert that into required format and classify.

- [`val.json`](val.json) : Json file that specifies the path of the saved .mat file of the acquired signal. **Please change the path of the .mat filein this Json file for testing according to your present working directory**

## Dependensies

- [Tensorflow](http://tensorflow.org)

- [Keras](http://keras.io)

- [Arduino](https://www.arduino.cc)

## About ECG Singnal Acquasition: Filters, Amplifiers, their circuits and their values

Electrocardiogram (ECG) is a recording - a graph of voltage versus time - of the electrical activity of the heart using electrodes placed on the skin. These electrodes detect the small electrical changes that are a consequence of cardiac muscle depolarization followed by repolarization during each cardiac cycle (heartbeat).

The prototype developed here focuses on the acquisition of the heart’s electrical activity with a simple electrical circuit. This electrocardiograph joins Arduino features with analog circuit and acquires, amplifies and filters a very weak signal obtained from the body.

The acquired ECG signal is used to monitor and display the heart rate in real time. The following cardiac cycle shows the generation of different waves in an ecg signal.

- The P-wave represents the wave of depolarization that spreads from the SA node throughout the atria.

- The QRS complex reflects the rapid depolarization of right and left ventricles. Since the ventricles are the largest parts of the heart, in terms of mass, the QRS complex usually has much larger amplitude than the P-wave.

- The T-wave represents the ventricular repolarization of the ventricles.

<p align="center">
    <img src="images/heart_cycle.png" width="275" />
</p>

### Block Diagram

<p align="center">
    <img src="images/block_diagram.png" width="700" />
</p>

### Discussions on Design with Illustrations; Circuit Connections

Single Channel acquisition is done with electrodes on both the arms which are used to drive the circuit above. Reference is given to the right leg. The signal has an amplitude between 1 and 5mV which are difficult to visualize directly. For this reason, the system was designed to have a gain of around 1000.

The bandwidth of a normal ECG is not very wide and the fundamental frequency is approximately 1 Hz. The harmonic content decreases until is practicably negligible beyond 60Hz however, the system higher cutoff frequency was fixed to be 150Hz as part of the QRS complex is lost if filtered at lower cutoff frequencies.

<p align="center">
    <img src="images/Frequency-spectrum.png" width="800" />
</p>

ECG signal gets interfered by many artifacts which need to be filtered out. Main sources of noise in the ECG signal are Baseline wander (low-frequency noise), Power line  interference  
( 50Hz noise from a power line), Muscle noise (EMG interference) an electrical interference from other equipment. Various filters are used to remove these disturbances.

To remove the common mode noise INA128U (Instrumentation Amplifier) was used having a high CMRR of 120db. The gain of the amplifier was set to 29 (so that output will be between +9 and -9)

<p align="center">
    <img src="images/4.png" width="300" />
</p>

To remove the baseline noise a high pass filter of cutoff frequency 0.05 Hz is used. A second order Butterworth filter was designed using opamp OPA27, a low noise amplifier to reduce noise amplification. The function of this filter is to eliminate the direct voltage in the signal and the resulting signal would be between + and - 5mv. 

<p align="center">
    <img src="images/1.png" width="300" />
</p>

An amplification stage with a gain of 41. This gives a total gain of 1255.

<p align="center">
    <img src="images/3.png" width="300" />
</p>

A low pass filter with 3db bandwidth at 150 Hz. This ensures unwanted frequencies in the ECG spectrum to be filtered off.

<p align="center">
    <img src="images/2.png" width="300" />
</p>

Power line interference(50 Hz) is a major design artifact. To remove this an Arduino(nano) was programmed to function as a second order IIR notch filter of 2Hz bandwidth. Cutoff frequencies are 48Hz and 52Hz. Coefficients are derived from Matlab.

y = 0.6022 * x - 0.6022 * x1 + 0.6022 * x2 + 0.6022 * y1 - 0.2043 * y2;

X is the output from the low pass filter.
Y is the output from the notch filter.

**Heart Rate Monitor:**

The time interval between two high amplitude R waves (R-R Interval) is used to calculate the instantaneous heart rate. Thresholding was done in Arduino to detect the R peaks and thereby calculate heart rate.

A 16x2 LCD display is connected to Arduino to display beats per minute(BPM).

**Our Circuit**

<p align="center">
    <img src="images/circuit.jpg" width="500" />
</p>

### Results

Single lead ECG detection using three electrodes was performed and the resulting ECG signal after passing through the cascade of the above filters was visualized using Arduino Serial plotter. Signal was sampled at a frequency of 500Hz. The entire circuit is assembled on a breadboard and powered through two 9v batteries making the prototype portable. Any abnormal heartbeat is indicated to the subject. The placement of electrodes also plays a role in the generation of different waves. We placed the positive electrode on the left hip, a negative electrode on the right shoulder and a electrode from the left hand wrist is given as a ground reference. This placement of electrodes is along the axis of heart alignment. Our observed ECG is 

<p align="center">
    <img src="images/signal.png" width="900" />
</p>

Here we take a look at each individual heartbeat, plotting all the 70 heartbeats in the sampled data (9000 data points) synchronized by their R peak, looks like below.

<p align="center">
    <img src="images/graph1.png" width="800" />
</p>

Here is the variation of Heart Rate throughout the sampled data,

<p align="center">
    <img src="images/graph2.png" width="800" />
</p>

## About Classification of Arrhythmias from ECG Signals

A 1D convolutional deep neural network is developed to detect arrhythmias in arbitrary length ECG time-series.

A convolutional DNN is trained on [this dataset](https://physionet.org/challenge/2017/) to detect arrhythmias, which takes as input the raw ECG data (sampled at 300 Hz, or 300 samples per second) and outputs one prediction every 256 samples (or every 1.28 s), which is being called as the output interval. The network takes as input only the raw ECG samples and no other patient- or ECG-related features. The network architecture has 34 layers; to make the optimization of such a network tractable, we employed shortcut connections in a manner similar to the residual network architecture.

Distinct from some other recent DNN approaches, no significant preprocessing of ECG data, such as Fourier or wavelet transforms, is needed to achieve strong classification performance.

<p align="center">
    <img src="images/deepnet.png" width="200" />
</p>

You can download the single lead ECG signals for training here: https://physionet.org/challenge/2017/training2017.zip

For detailed information please refer to [Cardiologist-level arrhythmia detection and classification in ambulatory electrocardiograms using a deep neural network](https://stanfordmlgroup.github.io/projects/ecg2/) by Awni Y. Hannun*, Pranav Rajpurkar*, Masoumeh Haghpanahi*, Geoffrey H. Tison*, Codie Bourn, Mintu P. Turakhia, Andrew Y. Ng.
