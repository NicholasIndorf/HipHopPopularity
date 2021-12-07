# HipHopPopularity

## Context
Audio classification is a classic use case for neural network models. In a previous project, for example, we built a classifier to distinguish between music genres 
from [this dataset](http://marsyas.info/downloads/datasets.html). This has been done in multiple blog posts 
([blog 1,](https://medium.com/swlh/music-genre-classification-using-transfer-learning-pytorch-ea1c23e36eb8) 
[blog 2](https://www.analyticsvidhya.com/blog/2021/06/music-genres-classification-using-deep-learning-techniques/)) 
and would seem to be a fairly well-known project. In addition, there are audio classification competitions going right now on Kaggle that would be prime candidates 
for a similar kind of workflow. [This speech recognition competition from Google Brain](https://www.kaggle.com/c/tensorflow-speech-recognition-challenge) and [this 
bird call identification competition from Cornell Lab of Ornithology](https://www.kaggle.com/c/birdsong-recognition) are two big examples.

With these in mind, I wondered how far I could take audio classification. Genres, human speech, and bird calls involve classes whose differences are plain to the 
human ear. Jazz sounds different than metal, syllables sound different from one another, and different birds have different calls. These are all obvious to a human 
observer. I knew that machines have a higher capacity for differentiation than humans have, and I found it peculiar that in the realm of audio, that we gave 
machines problems that the human ear could solve effortlessly. The most difficult audio example I could find was [differentiating COVID-19 coughs from normal 
coughs,](https://www.kaggle.com/andrewmvd/covid19-cough-audio-classification) but a trained respiratory doctor could probably assess decently well. In comparison, 
we have models that can predict whether [water wells in Tanzania are functional or not](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/) 
based on a slew of datapoints about the well. In the field of neural networks, [Nature published a deep neural network that can look at an ecg to make predictions about a heart diagnosis.](https://www.nature.com/articles/s41467-020-15432-4) 
A human would have significant difficulty sifting through either data set to make such predictions (except maybe a cardiologist for the second one).

Therefore, I wanted to choose a project that involved audio to make a prediction on something that a human would have difficulty doing. I wanted to push the 
envelope and do something ambitious. I love music, so I wanted to see if a model trained only on song samples could predict a track's popularity. This is a pretty 
common idea ([blog 1,](https://towardsdatascience.com/predicting-popularity-on-spotify-when-data-needs-culture-more-than-culture-needs-data-2ed3661f75f1#:~:text=According%20to%20Spotify%2C%20%E2%80%9Cpopularity%20is,a%20lot%20in%20the%20past.%E2%80%9D) 
[blog 2,](https://towardsdatascience.com/predicting-spotify-song-popularity-49d000f254c7) 
[blog 3](https://medium.com/m2mtechconnect/predicting-spotify-song-popularity-with-machine-learning-7a51d985359b)), but these all use Spotify's provided audio 
features (such as danceability, instrumentalness, and others). They don't use the audio samples themselves, and I thought that using the raw song samples witha 
neural net might work out better. Popularity is a tough target--the above blog posts have mixed success with more traditional (i.e. not neural network) techniques. 
Furthermore, a person can't listen to a song and say "oh yeah, that *sounds* popular". They might say that it sounds *good* but popularity is a bit more nebulous to 
quantify. Thus, if neural networks are typically used with audio data that is relatively easy for the human ear to classify, this might be a tough project that 
doesn't work out to make a useful tool in the end.

I had faith though. A properly trained neural network is a powerful thing, and if it didn't work out in the end, I might have answered a different but similarly 
interesting question about the limits of data science in audio neural networks.

## Business Problem
I sought to build a model that could predict whether a given previously unreleased hip hop track would be popular, based purely on the music itself.
This model would be an extremely useful tool for my cousin, who is an artist on Spotify -- under the name KC Makes Music.

## Data Gathering
I gathered data from the Spotify API, not only because it is the platform we are seeking to optimize on, but also because it is an extremely comprehensive database.

## Data Understanding
This data represented 16168 tracks released 2019-2021, and included popularity scores (0-100, 100 being most popular) and 
http links for ~30 second preview mp3s. The data was not as complete as it could have been (some well-known artists were not fully represented), but it was
still quite extensive and representative of the hip hop landscape for those years.

## Data Processing
Mp3s were gathered from these links and transformed into mel spectrograms. Mel spectrograms are a well-known tool to represent audio data in a visual format. 
It decomposes audio into frequencies and displays the frequency distributions over time. In this way, patterns regarding beat, timbre, etc. can be inferred 
by a model.

The associated target popularity for each preview mp3 was binned into popular and not popular. Using the training data, I determined popular to be >= 39.

## Modeling
Several models were attempted when attacking this problem. First, to get a sense for what a useless/untrained model would perform like, I used a dummy classifier
to make predictions according to the underlying distribution of the training data. This simulates a completely untrained model. ******????????

## Evaluation
Evaluation was done using accuracy and ROC-AUC scores. Of paramount importance is the model's ability to separate popular and not popular. A false positive 
means that 
and for this task
ROC-AUC is best equipped. 
