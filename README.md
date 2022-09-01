# Emotion Recognition
This repository is a fork from the work of [TideDancer](https://github.com/TideDancer/interspeech21_emotion) that replicates the experimental results of the [paper](https://www.isca-speech.org/archive/interspeech_2021/cai21b_interspeech.html):
***Speech Emotion Recognition with Multi-task Learning, X. Cai et al., INTERSPEECH 2021***

I replicated the code in the `notebooks` folder where I organized the code into two Jupyter notebooks:
- [Preprocessing.ipynb](https://github.com/florin-git/Speech-Emotion-Recognition-with-Multi-task-Learning/blob/main/notebooks/Preprocessing.ipynb)
- [Training.ipynb](https://github.com/florin-git/Speech-Emotion-Recognition-with-Multi-task-Learning/blob/main/notebooks/Training.ipynb)

## Set up environment
In the `notebooks` there is a `requirements.txt` file with all the packages used.
```bash
pip install -r requirements.txt
```

## Prepare datasets
1. Obtain IEMOCAP dataset from https://sail.usc.edu/iemocap/.
2. Extract and save wav files at some path, assuming named as /wav_path/.
3. Replace the '/path_to_wavs' text in ./iemocap/\*.csv, with the actual path just saved all the wav files. You can use the following command.
```bash
for f in iemocap/*.csv; do sed -i 's/\/path_to_wavs/\/wav_path/' $f; done
```

In my notebooks I store the IEMOCAP data in a folder called 'data' that contains the IEMOCAP dataset as it was just downloaded:
```
IEMOCAP_full_release
│
└───Session1
│   │ ...
│   └───sentences
│       │ ...
│       └───wav
│
└───Session2
│   │ ...
│   └───sentences
│       │ ...
│       └───wav
│
└───...
└───...
│   
```


**Note**: The iemocap/*.csv has 20 files, corresponding to the data split into 10 folds, according to session ID (01F, 01M, ..., 05F, 05M). For each fold, use the other 9 sessions as training, and test on the selected session. For example, for the fold 01F, we use 01F as test set and remaining 9 sessions as training set. Two csv files for each fold, one for training and one for testing. The names are: iemocap_01F.train.csv and iemocap_01F.test.csv. The csv file has 3 columns: file, emotion, text. The column 'file' indicates where to store the wav file; the column 'emotion' is the emotion label (we use 4 labels: e0 (Neutral), e1 (Happy), e2 (Angry), e3 (Sad)); the column 'text' is for transcript. For example:
```bash
file,emotion,text
/path/to/Ses01F_impro01_F000.wav,e0,"EXCUSE ME ."
/path/to/Ses01F_impro01_F001.wav,e0,"YEAH ."
...
```

## Reproduce using checkpoints
To reproduce the paper's results using the checkpoints:
1. Download the checkpoints from this [Google Drive](https://drive.google.com/drive/folders/1Ndybde47HDy8O7aiNvT14pT7FqkqOsSX?usp=sharing), save them at ./ckpts/
2. Run the following:
```bash
for s in 01F 01M 02F 02M 03F 03M 04F 04M 05F 05M; do bash reproduce.sh ckpts/$s/ $s > output/$s.eval; done
```
This will generate evaluation results using the downloaded checkpoints. The evaluation results are saved in output/ folder, named as 01F.eval, 01M.eval, etc.
The last line in each *.eval file, indicates number of correct predictions and accuracy for that folder (10-folds in total).
If you sum the correct number and divide by 5531 (total utterance number), you should get the accuraccy slightly above 0.78.

## Note
For further details please refer to the [TideDancer](https://github.com/TideDancer/interspeech21_emotion) code.

