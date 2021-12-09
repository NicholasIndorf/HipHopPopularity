# HipHopPopularity
![KCMakesMusic](images/KCMakesMusic.png)

##Summary
In this project, I sought to build a tool that could help my cousin, a hip hop artist, to assess whether songs he hasn't released yet had potential to be popular. 
I gathered and used only audio preview samples and associated popularity scores of recently released hip-hop tracks from Spotify. These were processed into 
spectrograms and a binary target (popular/not popular). These were fed into a neural network, which was iteratively constructed with increasing complexity. The 
most complex model, based on a Recurrent Neural Network, performed the best, with 59.8% accuracy and 0.646 ROC-AUC. This signifies an improvement over baseline of 
9.1% accuracy and 0.146. This is not a mindblowing result, but it isn't nothing! Especially considering that this model was very limited with only audio, this is a 
very promising result. I would not recommend this model for use on the individual song level, but it could still be useful for a larger number of samples. It could 
direct an artist's attention towards a subset to focus on, which saves effort and is thus still valuable. Further improvements will only make this a much more 
powerful tool at my client's disposal.

## Context
Audio classification is a classic use case for neural network models. I wondered how far I could take audio classification. 
Typically it is done with information that is easy to the human ear, such as 
[music genres](https://www.analyticsvidhya.com/blog/2021/06/music-genres-classification-using-deep-learning-techniques/))
and [human speech](https://www.kaggle.com/c/tensorflow-speech-recognition-challenge).

However, I wanted to choose a project that involved audio to make a prediction on something that a human would have difficulty doing. 
I love music, so I wanted to see if a model trained only on song samples could predict a track's popularity. This is a pretty 
common idea ([blog 1,](https://towardsdatascience.com/predicting-popularity-on-spotify-when-data-needs-culture-more-than-culture-needs-data-2ed3661f75f1#:~:text=According%20to%20Spotify%2C%20%E2%80%9Cpopularity%20is,a%20lot%20in%20the%20past.%E2%80%9D) 
[blog 2,](https://towardsdatascience.com/predicting-spotify-song-popularity-49d000f254c7) 
[blog 3](https://medium.com/m2mtechconnect/predicting-spotify-song-popularity-with-machine-learning-7a51d985359b)), but these all use Spotify's provided audio 
features (such as danceability, instrumentalness, and others). They don't use the audio samples themselves, and I thought that using the raw song samples with a 
neural net might work out better. Popularity is a tough target--the above blog posts have mixed success with more traditional (i.e. not neural network) techniques. 
Furthermore, a person can't listen to a song and say "oh yeah, that *sounds* popular". They might say that it sounds *good* but popularity is a bit more nebulous 
to quantify. 

## Business Problem
I sought to build a model that could predict whether a given previously unreleased hip hop track would be popular, based purely on the music itself.
This model would be an extremely useful tool for my cousin, who is an artist on Spotify -- under the name KC Makes Music.
He could use this tool to bounce his music off of to get a feeling for its popularity potential.

## Data Gathering
I gathered data from the [Spotify API](https://developer.spotify.com/documentation/web-api/), not only because it is the platform we are seeking to optimize on, 
but also because it is an extremely comprehensive database. I used the [Spotipy python package](https://spotipy.readthedocs.io/en/2.19.0/#) to access the API. For 
more information on how I gathered the data, please look at my data gathering section in HipHopPopularity.ipynb, and in particular the custom function `get_tracks` 
in the glossary.

## Data Understanding
This data represented 16168 tracks released 2019-2021, and included popularity scores (0-100, 100 being most popular) and 
http links for ~30 second preview mp3s. The data was not as complete as it could have been (some well-known artists were not fully represented), but it was
still quite extensive and representative of the hip hop landscape for those years. Much more exploratory information in HipHopPopularity.ipynb.

## Data Processing
Mp3s were gathered from these links and transformed into mel spectrograms. Mel spectrograms are a well-known tool to represent audio data in a visual format. 
It decomposes audio into frequencies and displays the frequency distributions over time. In this way, patterns regarding beat, timbre, etc. can be inferred 
by a model.

The associated target popularity for each preview mp3 was binned into popular and not popular. Using the training data, I determined popular to be >= 39.

## Modeling
Several models were attempted when attacking this problem. First, to get a sense for what a useless/untrained model would perform like, I used a dummy classifier
to make predictions according to the majority class of the training data. This simulates a completely untrained model. I then iterated through a simple multilayer 
perceptron, two builds of a Convolutional Neural Network, and finally a Recurrent Neural Network which has a Convolutional component at the start.

### Google Colab
The modeling section of this project was performed in Google Colab, as my computer couldn't handle it. Your mileage may vary. I noted in my notebook what 
cells are for Colab.
[Link to model .pkl files here](https://drive.google.com/drive/folders/1W1u_lJYBIv4HcS1lnXK6szcel98ZYZ6q)

## Evaluation
Evaluation was done using accuracy and ROC-AUC scores. Of paramount importance is the model's ability to separate popular and not popular. Either way it
false categorizes a song means wasted effort so minimizing both was prioritized.

Overall the Recurrent Neural Network worked the best, and seemed to pick up on some underlying patterns. It didn't work amazingly well, but +9.1% more accurate 
and +0.146 ROC-AUC means that this idea has real potential.

## Conclusion
Even though the model only learned a little bit, it still has some potential for use. Instead of being used on the individual song level, it could be used with
a collection of tracks as a first-pass seive to point out a sub-group of standout songs. My client runs a recording studio and knows many fellow artists so
this still can be a useful tool.

## Next Steps
Moving forward, I will be looking into incorporating non-audio features, different audio features, different neural architectures, and more input tracks.
Furthermore, I will use a tool like LIME to take a look under the hood to see what the model believes to be important factors for popularity.

## Author
Nicholas Indorf
email: nicholasindorf@gmail.com
linkedin: linkedin.com/nicholas-indorf-data-science
github: github.com/Nindorph

## Presentation
[Google Slides Presentation](https://docs.google.com/presentation/d/1OwtORdCqgpgshPoeff4esrDakuBMAAOAC2J39BxBg4A/edit?usp=sharing)

## Reproducibility
requirements.txt lists all the packages necessary to run this project locally.

## Directory
├── LICENSE
├── Makefile           <- Makefile with commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
├── src                <- Source code for use in this project.
│   ├── __init__.py    <- Makes src a Python module
│   │
│   ├── data           <- Scripts to download or generate data
│   │   └── make_dataset.py
│   │
│   ├── features       <- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         <- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  <- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
│
└── tox.ini            <- tox file
