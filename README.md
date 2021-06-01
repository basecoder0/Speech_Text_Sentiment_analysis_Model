# STSaM_Demo
Speech Text Sentiment analysis Model

# Datasets used to train this model
SAVEE
https://www.kaggle.com/barelydedicated/savee-database

RAVDESS EMOTION DATA SET/SONG DATA SET
https://zenodo.org/record/1188976#.YLLSFLdKiUk


# Concept
My goal for this project is to create a Neural Network model that is able to generalize on a specific
mood/sentiment based on the speech and text input. The concept behind this idea is that through functional API I can
build a model that takes two distinct inputs, one for processing the text and the other for speech. Using a combination
of CNN, MLP, and RNN Neural Networks, I should be able to predict an emotion by concatenating the results from each
individually processed input.

# Data Preprocessing
Observing the image below, the a goal is to take audio input from the user and convert that into trainable data to be consumed
by the Neural Network. Using a the librosa library, it will allow me to convert sound waves into a Mel-Frequency Cepstral Coefficient (MFCC)
representation that can be used by the model. To get a decent understanding of what MFCC are, here is a link to the wiki page, that briefly goes over the 
concept [MFCC wiki](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum).

![Sound wave to MFCC](./images/Sound%20wav%20to%20mfcc.PNG)

Some challenges I ran into when building this model, was first figuring out how exactly to process the sound wavs. What I mean by this is that I am not just
converting sound waves into MFCCS and thats it, nope, some other factors to consider is sample rate, duration of the actual audio file itself, how many seconds
to offset the audio by before it starts reading the wav file, and resample type. While I myself wasn't exactly too sure on how to go about this process, I did take 
note from an already established project that I used as a baseline for this project. Here is a link to their [GitHub](https://github.com/MITESHPUTHRANNEU/Speech-Emotion-Analyzer). In the image below you can see the setup I used for preprocessing my audio files, more details will be in the ipynb file

![Librosa arguments](./images/librosa_load.PNG)

Preprocessing text is a bit simpler, as I use a standard vectorization method (one-hot encoding) to transform the
text into a usable format that can be processed by the model. Currently I am training this model using the IMDB movie
reviews dataset, the features for the encoded string is currently at 10000. I could possibly use an encoding method with
smaller features but for the sake of testing this idea, this is the state for how the text should be transformed when being fed into the model.
Feel free to modify it as you see fit.

# Model Architecture
![Model Architecture](./images/Model%20Architecture.PNG)

# Audio Layer (CNN) Processing
Convolutional layers are great at scanning and capturing interesting features on each
channel in a 2D space. 1D Convolutional layers are great at scanning 1D sequences of data based on a timeseries, in the
case of this project signals. Much like the 2D Convolutional layers, the 1D layers with 128 units, will sample the raw data (MFCC
features) with a shape of (216,1) using a kernel of size 5 as well as keeping the padding the ‘same’ to preserve the input
shape. 
-  I pad the input to maintain the frequency spectrum as different emotions can be represented by different
periods and amplitudes in a sound wave, since convolutional layers downsize a sample based on the kernel used, it would ultimately be removing the
context which can describe an emotion, so I want to preserve as much as we can. 

![Periods and Frequencies](./images/periods%20and%20frequencies.PNG)

-  I apply a Dropout layer, for the purpose of overfitting as well as a maxpooling layer to down sample our dataset by a factor of 8.

-  After extracting all the interesting features using the 1D Convolutional layers, I flatten the features into a
shape (,3456) tensor.

- Lastly the data is then processed through the Dense layers where the features are applied to an output
layer using softmax. 

# Experiments/Results


