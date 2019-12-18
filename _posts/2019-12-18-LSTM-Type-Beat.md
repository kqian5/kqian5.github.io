---
layout: post
title: LSTM Type Beat
categories: Projects
---

*generating novel piano music...*

![GoogleClips](/public/images/score.jpg){:class="img-responsive"}

<!--more-->

Team: Kyle Qian, Christie Gahm, Dylan Ngo

Tools: Python, Tensorflow, Keras

***Introduction***

As a violinist and classical music enthusiast, it’s always interesting for me to think about how composers actually composed these immensely complex concertos and symphonies. Even more remarkable, is the idea of using computers to generate music. Inspired by this idea, we chose to implement a learning model based on an architecture described by Douglas Eck and Jürgen Schmidhuber in their paper. The study explained the benefits of using a Long Short-Term Memory (LSTM) structure as opposed to vanilla recurrent neural networks because of its ability to retain a “global” music structure. Furthermore, this model will allow our algorithm to account for several notes prior to our last note when generating the next chord. This “global” scope will potentially allow music themes, sequences, and unique melodies to be created. Our passion for music inspired us to explore the intersection of computer science and music by solving this unsupervised learning problem.

***Preprocessing***

We decided to use the pretty_midi library on the MAESTRO dataset because its tools were very cohesive with our method of composing music. With the library, we converted midi files into information relevant to our model by only extracting the pitches and rests. After extracting, we converted the raw chords into timeframed, tokenized ids, where each unique chord had a unique id.

***Model***

The following architecture was used:

Embedding       →     256 LSTM         →    0.3 Dropout       →    512 LSTM           →    0.3 Dropout       →       Vocabulary Size Dense Layer       →    Softmax

***Design Choices***

We experimented with a series of hyperparameters before deciding on these choices. We set the embedding size to 40, window size to 50, batch size to 50, and used Tensorflow’s Keras Adam optimizer.
    

***Challenges***

One of our biggest problems was creating a piece of music that continuously created notes after generating a “rest” note. Much of our training music consisted of songs that had multiple seconds of rest throughout the piece. As a result, our model was trained to predict that a rest was most probable to follow another rest, which would compose a piece that would continuously be rests. To remedy this, we decided to sample through our data 70% of the time rather than taking the arg max of the probability distribution every time. It is important that we understand this tradeoff and reconcile the benefits of it.

In addition, because our data set was so large, there were often rounding errors and the probabilities returned from the model did not sum to 1 (often was .9999). We simply distributed the remainder to other probabilities, which did not affect the results drastically as this was a very small value.

Another challenge we faced was that our LSTM model could not retain as much previous information as we originally wanted. As a result, our model did not learn song structure. The most noticeable effect was the abrupt endings because the model did not know when the song would end.

Finally, music composition, like any arbitrary artwork, is subjectively analyzed and therefore it is a bit difficult for programmers to decide upon the metrics used to measure model performance. For now, we did not implement any quantifiable metric to evaluate our final production.

***Conclusion***

We generated a series of MIDI files by feeding the model an initial note/chord and handpicked a few results that we found most musically pleasing. These pieces seem to be generally composed of similar sounding chords, which shows that the LSTM architecture does capture some form of the structure of piano compositions. There were many miscellaneous lessons that we learned throughout our project. Aside from big picture concepts such as layers, architecture, and generation, we learned the importance of preprocessing and the benefits of saving preprocessed data and inputs to save an immense amount of time. 
