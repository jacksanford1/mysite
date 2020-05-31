---
title: "Using ML to Find the Funniest Friend in Friends"
date: 2020-05-30T23:07:39-07:00
draft: true
---

I finished Andrew Ng’s courses on machine learning (which I loved) and decided to try to apply what I learned. What draws me to machine learning is how applicable it is to so many different fields. Because of ML’s breadth of relevance, I decided I should work on a project that was somewhat original, something that had never been touched by ML before.

I settled on finding the funniest friend in Friends because it seemed like something that a human could do fairly easily (it would just take a ton of time) and also because no one knew the answer but a lot of people are probably interested (or at least a few people).

In order to find the funniest friend in Friends, we need to know who generates the most laughter. There are two caveats which I explain in the fun article. The first is that we assume audience laughter (and length of laughter) maps to how funny a spoken line is. The second caveat is that we can’t capture purely non-verbal humor, which turns out to be ~3% of the show (this is a rough calculation based on laughter’s proximity to spoken lines).

In order to attribute a sequence of laughter to a particular character, I decided that the character whose spoken line ends closest to the beginning of that sequence of laughter will be given attribution for the laughter. If that doesn’t make sense or doesn’t seem reasonable, I go deeper into this later.

Using this method, there are three main parts to figuring out which character is responsible for which laughter. First is identifying who is speaking when. Second is identifying laughter and when it occurs. Third is piecing together the first two parts to figure out who is responsible for each instance of laughter.

### Part 1

I talked the project over with some friends who know more about ML than I do. They told me that the first part of the project (who is speaking when) is non-trivial when it comes to voice recognition. Detecting and differentiating human voices is still a difficult problem for ML. I read some papers and articles and it seemed to confirm my friends’ suspicions. [Here](https://towardsdatascience.com/ai-to-detect-speaker-in-a-speech-a1dae5b597b0) is an example of an article that can identify different human voices with 80-85% accuracy. For my project, I didn't think 80-85% accuracy would be high enough to draw any sort of conclusions about the funniest friend. Note: If you are reading this and believe you know an approach that can identify different voices with greater than 95% accuracy, please reach out! I would love to test your model on my data now that I have a labeled dataset.

With voice recognition ML not likely to perform accurately enough, I decided to try some different approaches. Because Friends is so popular, there are [fan-created script files](https://fangj.github.io/friends/) for every episode in the show. These script files tell you who is speaking and what they say:

There have even been some [data science articles](https://towardsdatascience.com/the-one-with-all-the-friends-analysis-59dafcec19c5) written that make use of this script dataset in order to find interesting facts like who had the most speaking lines across all 10 seasons of the show.

These script files are great but the don’t tell us exactly WHEN any of these lines are said during the episode. Luckily, there are also publicly available subtitles files for each episode of the show. They tell you what is said and when it is said, but not WHO said it:

If we could somehow combine the script file and subtitles file, we could get the WHO and the WHEN that we need in order to figure out who speaks before laughter. What the script and subtitles files have in common is that they both contain the WHAT, as in the spoken lines. So we basically need to do a JOIN on the spoken lines to combine the WHO from the script with the WHEN from the subtitles.

As you can see from the two images above, the script and subtitles lines are similar to each other, but by no means are they a perfect match.

I decided to use a series of 4 algorithms to join the script and subtitles files of each episode based on their spoken line similarities.

#### Algorithm #1
One character word/phrase match

We look for consecutive words (between 1 and 5) in the script that are spoken by only one character and we see if those consecutive words also appear an equal or lesser amount of times in the subtitles file. We then assign that single character to the lines containing the consecutive words in the subtitles. An example from season 1 episode 1 is that Ross is the only character to say “should I know” in the episode, and these three words appear together once in the script and once in the subtitles. Therefore we ascribe the subtitles line containing “should I know” to Ross. You may notice that I implemented the “single” word match before the “phrase” match, but I should go back and combine the two into a single algorithm.

We get about 80% of all the lines in the subtitles after algorithm #1 and those lines were also about 99% accurate in terms of getting the character right.

#### Algorithm #2
Local one character phrase match

This is the most complicated algorithm. The idea is that we can now start to zoom in on certain areas of the script where we have unknown lines between two known lines. Once we have a smaller area of the script, we can do a one-character phrase match in that smaller area and find matches we would not have found if we were looking across the entire episode. We know exactly which line we are looking for in the subtitles and we also know where the closest known line above and below that line is (in the subtitles). However, we don’t have our bearings in the script file at all. To get our bearings, we take the word/phrase that gives us the known line above in the subtitles and same for the known line below. We then search the script file for a pattern that looks something like “known-line above word”, then unknown line, then “known-line below word.” Because known lines can have the same discovery word (for example, 4 different lines in the subtitles file have the discovery word “daddy”), this process is not flawless. But once we choose a “range” of the script to search, we simply execute algorithm #2 locally in that range in order to match characters to even more lines. To show how this algorithm is different from algorithm #1, imagine the phrase “How are you?” is said 5 times throughout an episode by 3 different characters. Algorithm #2 would not be able to match any characters in this situation. But imagine we search locally between lines 45 and 55 in the script and we find that “How are you?” is said only once by one character. We can then use that phrase to match a character to a subtitle line where we couldn’t before.

After Algorithm #2 we end up with 93% of lines predicted and 98% accuracy on those predictions. Algorithm #2 gets us an extra 13% of lines with ~92% accuracy. Not as good as the first two algorithms, but not terrible either.

#### Algorithm #3
Exact Speaker Count

For those lines that we can’t get with Algorithm #2, we also use local search to go back and see how many speaking lines are in between the two known lines in the script vs. in the subtitles. Subtitles lines tend to be shorter so we would often expect there to be more subtitles lines in between two known lines than we would script lines. However, when the two match up, we can simply map the order of speakers in the script to the order of lines in the subtitles. For example, we know lines 90 and 93. Lines 91 and 92 happen to be “Hello”, first said by Rachel (line 91), then Ross (line 92). We can’t get these lines using one-character exact word/phrase match because the lines are identical. However, we can see that there are two missing lines in the subtitles and there are also two lines between our known lines in the script. We make an assumption and we assign the first line in the subtitles to Rachel and the second line to Ross. This algorithm has a small but meaningful contribution.

After Algorithm #3, we now know 95% of lines with 98% accuracy.

Rarely, a subtitles line will put the name of the character before the line. We also incorporate this data however it only adds about 0.2% more lines than we had before.

With 98% accuracy on 95% of lines, I presume we are outperforming what a voice recognition ML algorithm could get us (If you think that is wrong, I would love to talk to you). The last 5% of lines tend to be extremely difficult to label. For example, Rachel will walk into a room of 3 friends and they will also say “Hi” to each other, nearly simultaneously. Without using a beastly combination of voice recognition, on-screen character (image) recognition, and text processing with the subtitles and script files I believe it would be very difficult for a machine to label these lines accurately. Therefore, I took it upon myself and hand-labeled the remaining 5% of lines (hopefully with near 100% accuracy). This means that overall, the first part of the project will have lines that are labeled with ~98% accuracy.

From here, I do a quick check of all unique names across the 10 seasons and I assign any aliases to the correct characters. Then upload the labeled data into a SQLite database.

### Part 2
Recognizing and labeling laughter (Using actual machine learning!)

For recognizing laughter, there is no text-based file that I’m aware of that could give us the data we need. So we must go to the actual audio.

I had to start with the video files of each episode for all 10 seasons. From here, we can use VLC on Mac to convert video files to audio (MP3).

Once we have the audio files, there is a transform we can apply that I found helps the machine learning algorithm tremendously.

We take the stereo audio track and split it into a dual-channel mono track, then invert one of the channels which effectively cancels out anything that was center-panned in the original stereo track. This is usually the character vocals. I use a free software called Audacity to accomplish this. This gives us an audio track that effectively silences the character speech and leaves nearly all of the audience laughter alone. [Here](https://www.techwalla.com/articles/how-to-remove-vocals-from-songs-on-apple-or-mac-computers) is the method.

I will note that this method doesn’t work anywhere near perfectly (a lot of vocals come through still) but it can make a tremendous difference for helping the ML algorithm isolate laughter. If you look at an audio track before and after this transformation, you can see the difference and you can see the laughter much more easily (point out laughter instances).

[non inverted spectrogram]

[inverted spectrogram]

Now we have the base audio that we will eventually feed to a ML model.

In order for the model to learn, we will need to teach the model examples of laughter and examples of non-laughter. In order to get training examples, I listened to 20 entire episodes in Audacity and used the labeling functionality to mark laughter ranges (we want to know exactly when the laughter begins and ends). You can easily export the labels and save them in text files.

While I was taking Andrew Ng’s Deep Learning course on Coursera, there was a very relevant project on detecting trigger words (for example, “Hey Siri”) from audio clips. The ML model used a 1-D convolutional layer and 2 GRU layers. I thought this would be as good a model as any to start with.

For ease of use, I decided to cut the 22-minute episodes into 10-second increments. I played around with overlapping the clips just in case the model had trouble with the very beginning or end of a clip but eventually went for no overlap.

In order to use this model, we have to translate our audio into numerical data. We can represent the audio clip to the model as a number of time steps (splitting up the 10-second clip into roughly 10 millisecond increments) and displaying the values of different frequencies at each time step. I ended up with 257 frequencies per time step and 861 time steps. So a 10 second clip would now become a numpy array with shape (861, 257) representing the values of 257 unique frequencies at each of 861 time steps in the clip. Put those 10-second clips together and we have a full episode that is ready to be fed into the model.

Of course the model needs the “answers” to the training episodes as well in order to learn. To generate the y input, we use those text files full of hand-labeled laughter ranges. At each timestep of each 10-second clip, we basically ask the question, “Is this timestep inside a laughter range or outside?” and we label it “1” if it is laughter, and 0 if not. For a 10-second clip, the y value will be of shape (861, 1) because we simply have a binary output at each timestep in the clip.

Note: I ended up doing everything in Part 2 using Kaggle notebooks. I ran into disk space and RAM issues so many times that I likely wouldn’t use a Kaggle notebook again. From what I understand, Google Colab notebooks suffer similar restrictions. The appeal of these options is that you get free GPU time (30 hours) and unlimited access to 16 CPU cores. In the future, I would likely look towards the AWS EC2 P3 instance or another paid computing option to save myself the memory headaches. Another interesting observation was that using a GPU vs. not using a GPU on a Kaggle notebook only had about a 15% improvement in runtime. So for this project, a GPU may not have even been necessary.

I saved the X and y datasets as numpy arrays in order to move between Kaggle notebooks.

Then I took the X and y inputs and used train_test_split from sklearn (twice) to split the data into 60% train, 20% development and 20% test.

From there we define our model beginning with a 1-D convolutional layer and 2 GRU layers followed by a dense layer. I experimented with more or less GRU layers and also different combinations of dropout and batch normalization layers but this combination seemed to work best. It happens to be the exact same implementation from a layers standpoint used in Andrew Ng’s example. Maybe he knows what he’s talking about after all.

We use the Adam algorithm for gradient descent optimization as well as a binary cross-entropy loss function.

Then we can finally train the model!

When I was experimenting with different hyper parameters and layer combinations I found that using 15 epochs was more than enough to figure out if the model was trending better or worse than past models. This took between 15-30 mins for most combinations. For the final model I decided to use, I trained for 100 epochs which took between 5 and 6 hours on a GPU.

The best model was able to identify laughter with 95% accuracy. I wasn’t thrilled with this outcome and so I tried many things to improve the accuracy. But what I eventually figured out was that a lot of the error was my own fault (i.e. human error). I didn’t realize this until I went back and labeled the laughter for an episode that I had already labeled a week prior. I then compared the labels and found that they only had 97% overlap! Meaning that the “human” level error for identifying laughter in this situation is ~97%, so it is unreasonable to expect a machine to do better than 97%. In that light, 95% for the algorithm looks like a more positive outcome.

When you think of 95% accuracy, you may think this means that the ML gets 19 out of 20 laughs correct, and then completely misses a laugh, bringing a lot of error to our end goal of who is the funniest character. In practice, what 95% actually looks like is that the algorithm never misses laughter but instead labels 95% of each laughter instance correctly. So instead of missing 1 out of 20 laughs, we are “missing” by plus or minus a few dozen milliseconds at the beginning or end of each laugh. Even a human can’t label laughter down to the millisecond very accurately so the ML’s output is actually quite practical to use. Note: Because we end up using laughter length to determine the funniest character, it actually doesn’t matter if we miss 1 out of 20 laughs entirely or mislabel 5% of every laugh sequence.

It’s also interesting to note that our precision score ends up higher than our recall score, meaning that we aren’t usually labeling a lot of instances as laughter when they aren’t (low false positives), but more often we are missing laughter when it is there (higher false negatives). Of course, most of this effect could be due to the human (me) labeling for too long after the laughter ends by a few dozen milliseconds.

Once we have the trained model, I use it to predict laughter for all 214 remaining episodes.

With the predictions in hand, there are effectively two post-processing steps I take in order to make the laughter ranges even more accurate. Anything under 400 milliseconds is too short to be a standalone laughter instance, so I decided it either needs to join together with a close-by laughter instance, or it needs to be removed. Any gap of 100ms or less between two laughter instances is much too short to be meaningful, so we combine those two laughter instances into one longer laughter instance.

We take our processed predictions and store them in SQLite where they are now ready to be combined with the character data from Part 1.

### Part 3

We bring in both datasets and format them to be compatible with each other.

Then we use a simple minimization function to decide who caused each instance of laughter. We take the beginning timestamp of the laughter instance and we find the minimum distance to the end of a character line. Whichever character’s line ends closest to the beginning of the laughter (within +/- 3 seconds) is the character to whom we attribute the laughter instance. (Need to know how accurate this?)

Now we have the exact data we need to answer “Who is the Funniest Friend in Friends?”

But first, in order to see how it’s working in practice, we’ll create custom subtitles for each episode that will give us the data in real time.

SRT files (subtitles) are just text files with a standardized format for displaying subtitles on a video. We convert our timestamps and speaking lines into SRT format and we can feed it right in to any video file.

Once we can see that the labels are working well in practice, all that’s left is to calculate the answer to our original question.

Like I mention in the fun article, it would be unfair to simply add up the seconds of laughter for each character across all 10 seasons to see who is funniest. That’s because, as we can see from the line count, some characters have many more lines than others. So I believe a more fair way to calculate the funniest character would be to see how much laughter follows each line spoken by a character on average. The character with the most seconds of laughter following each of their lines on average, wins.

We can see that it turns out to be Joey with 1.27 seconds of laughter per line on average. (Say something about confidence intervals? Exclude non-verbal humor?)

How accurate are these findings?

So we know Part 1 predicted the correct character for subtitles lines ~98% of the time. And we know for Part 2, our ML model was ~95% accurate in labeling each ~10ms timestep of laughter correctly. For Part 3, deciding who is responsible for each laugh, I did some manual testing on a few episodes (few hundred examples) and found that we attribute the correct character to laughter about 95% of the time (excluding non-verbal cues). But our final metric, Joey’s 1.27 seconds of laughter per line, is a bit more complicated. It combines getting the character correct (~95%) and getting the length of laughter correct (~95%). So on a timestep by timestep basis, the probability of both events being correct is 90.25% (95% * 95%).  

With this in mind, we can calculate confidence intervals around the 1.27 seconds of laughter per line. We calculate the accuracy to be 1.27 +/- 0.02 seconds with 95% confidence. This means that there’s a greater than 95% chance that Joey is the funniest character because Chandler’s range of 1.22 +/- 0.02 can’t surpass Joey’s within our 95% confidence interval. For Chandler and Phoebe, it is reasonable that Phoebe could actually be the 2nd funniest character because the ranges are so close.

Another example with even tighter ranges is the “funniest season” breakdown. The “seconds of laughter” per season have a 95% confidence interval of +/- 3 seconds, a very tight interval. This is because we are able to identify laughter with 95% accuracy (thanks to the ML algorithm) but moreover it is because we test over 500k examples per season. Remember we look for laughter in ~10 millisecond increments in every episode. This high “n” massively contributes to the tight confidence intervals.
