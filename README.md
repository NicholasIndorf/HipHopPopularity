# HipHopPopularity

I sought to build a model that could predict whether a given previously unreleased hip hop track would be popular, based purely on the music itself.
This model would be an extremely useful tool for my cousin, who is an artist on Spotify -- under the name KC Makes Music.

I gathered data from the Spotify API, not only because it is the platform we are seeking to optimize on, but also because it is an extremely comprehensive database.
This data represented 16168 tracks released 2019-2021, and included popularity scores (0-100, 100 being most popular) and 
http links for ~30 second preview mp3s. The data was not as complete as it could have been (some well-known artists were not fully represented), but it was
still quite extensive and representative of the hip hop landscape for those years.

Mp3s were gathered from these links and transformed into mel spectrograms. Mel spectrograms are a well-known tool to represent audio data in a visual format. 
It decomposes audio into frequencies and displays the frequency distributions over time. In this way, patterns regarding beat, timbre, etc. can be inferred 
by a model.

The associated target popularity for each preview mp3 was binned into popular and not popular. Using the training data, I determined popular to be >= 39.

Several models were attempted when attacking this problem. First, to get a sense for what a useless/untrained model would perform like, I used a dummy classifier
to make predictions according to the underlying distribution of the training data. This simulates a completely untrained model. ******????????

Evaluation was done using accuracy and ROC-AUC scores. Of paramount importance is the model's ability to separate popular and not popular. A false positive 
means that 
and for this task
ROC-AUC is best equipped. 
