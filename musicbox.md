* [Return to home](/index.md)

# Music Box: Algorithmic Music - Using rules and random numbers to compose songs

This project generates music without human intervention given simple limitations and random numbers, allowing unique songs to be created in seconds. Algorithms were written in the programming language C to create rock and pop style songs. These songs are played back using visual programming language Max, so that compositions can be heard using samples of acoustic instruments. This project will demonstrate these methods can be used to produce a virtually endless amount of unique songs.

* [Link to project code](https://github.com/cadenkesey/musicbox)

<iframe width="560" height="315" src="https://www.youtube.com/embed/MM8mgYC7VCY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/fyaTH6KR6Uc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Introduction

My goal was to algorithmically generate a full song without the need for any human intervention. Basic parameters are given by the user and then a song is generated based on those parameters. There should not be any need for musical background in order to use the program. I wanted to design a product that is easy to use for someone with no musical background or beginners who are just starting to compose. This means it could potentially be educational as well as provide useful tools for somebody who is already an experienced composer. Rhythm and catchiness will be the main focus of the songs generated. This is in contrast to many of the other available programs for algorithmically generated music. Many of these programs have a focus on classical or jazz music, which I do not think reflects the tastes of the average user. That is why I wanted my algorithmically generated music to be based around the musical structures and patterns found in rock and pop. The biggest focus is the rhythm section which includes the drums and bass. This is what differentiates the application, since rock and pop have a stronger emphasis on repetition. This focus and style also affects the instrumentation used. One thing I noticed when looking at programs for algorithmic music generation is that the song needs to be exported to a different program to be heard at all or it simply sounds bad when played directly from the program. I think this is because the people that developed these programs do not see sound quality as the main focus since they intend it to be an aid for composition. This creates a lack of accessibility to the general user who may not have access to other programs and also limits the user base to composers. I believe the song should sound good right out of the box as well. This is why I used samples of real instruments that are played back with MIDI so that users can get immediate results.

The final product includes basic parameters that the user can tweak and input. This allows it to be educational because various examples of different types of musical ideas can be quickly produced. Basic parameters include the tempo and the random number generation seed. That means adjusting one of these parameters will generate an immediate example. It also allows an experienced composer to have many different options to work with. I focused on creating a simple user interface. The program gets parameters from the user which then go into a “blackbox”. What comes out of the blackbox are notes defined by integers for their value and floats for their length in milliseconds. The user is able to input a seed to ensure that they can reach the same result again. Otherwise even with the same tempo the blackbox will likely generate different results each time. The song is converted to MIDI format through Max and then is put through visualizer which allows the user to see what notes are being played. The notes then trigger samples of real instruments which are played back. This way the program can be a useful tool for anyone who is trying to generate music, whether it be an experienced composer, a beginner, or even game developers and filmmakers who would like to have original songs to use in their projects.

 My motivation for this project comes from my own desire to see a useful tool for algorithmic music. I have always had an interest in procedural generation, particularly when it came to music, but it has always been disappointing to find that the majority of pre-existing programs for algorithmic music are either not user-friendly or require some kind of subscription to use. While the concepts I am drawing from are not necessarily novel or particularly innovative, what separates my project from others will be the focus on accessibility and user interface. Additionally, creating my algorithms for music generation developed a product that is unique and hopefully can provide others with interesting musical ideas.
 
## Implementation and Results

In order to create this application I worked with the visual programming language Max. Max is designed for music and multimedia. Max is modular meaning it is made up of independent objects which are then connected in order to create a program. Max allows for third-party development of external objects which can be written in C. This allowed me to write my own object that generated notes. The reason I wanted to use Max is because it has a lot of digital signal processing functions and is well-suited for working with audio and music.

A big challenge for me was learning to use Max and learning to write externals.  While externals for Max are written in C, which I did have experience with, Max itself was brand new to me. The fact that I already had experience in C was helpful in learning to write externals though. This was the first hurdle to understanding how much I could accomplish with this project.

The bulk of the programming was writing an external for Max in C that allowed me to create a kind of “blackbox” which I called the music box, which takes in user parameters and outputs a song. In order to program an external I used Microsoft Visual Studio. Programming directly in Max entailed creating a user interface, visualization, and outputting sound.

###### The input parameters in Max (play/pause, rng seed, tempo)
![Image1](/images/image_1.png)

###### An abridged look at the initialization of a music box object using Max inlets and outlets, Max clocks, and loading in note names with a hashtable.
![Image2](/images/image_2.png)

The first obstacle was determining how to handle the creation of notes. MIDI is a standard for transmitting musical information and I knew that it would be the best output for my application, but I also knew that it would be a hassle to figure out how that would work in C. I figured out that Max could take numbers and pack them together into a proper MIDI format so that virtual instruments would be able to interpret the information as actual notes. This way my object only had to output an integer to represent the note and a float to represent how long to hold it. Once I had this in mind, I knew I would want to use a struct in C to keep the note value and length together, which lent itself well to moving a step further to linked lists for a phrase of notes.

###### The note struct in C
![Image3](/images/image_3.png)

###### Packaging the note value and length into MIDI format using Max
![Image4](/images/image_4.png)

Next I had to determine how much of the generated music would be predefined. I wrote code to load in notes from a text file with a hashtable to convert alphabetical note names to integers. C doesn’t have hash tables so I wrote a simple implementation that fit what I needed perfectly. This allowed me to define patterns for the kick and snare, which the program would pick and pair together randomly. I also was able to load in two chord progressions for each song, one for the “verse” and one for the “chorus”. With those aspects predefined in text files, the song would have enough structure to sound coherent. Writing enough patterns would be the main challenge to making sure generated songs didn’t all sound the same.

###### A list of chord progressions for the verses in a text file
![Image5](/images/image_5.png)

Then came the algorithms for the bass and melody. Since the chords and drums were all structured, I used these as the basis for the algorithms. The bass is designed to play the root or the main note of each chord whenever the kick plays. Between these the bass will randomly play notes from the chord with random lengths. The melody is similar except with less structure. The melody is given the whole A minor scale to use and will generate random notes with random lengths. The only exception is moving up an octave any time a note coincides with the snare drum.

The basis for the creation of the melody was the idea that the snare drum provides the “backbeat” in rock and pop music. By matching the melody to this backbeat to some degree, the rhythm of the song is emphasized and further cohesion is created between the instruments of the song. Even when playing just the bass and melody without the accompaniment of the other instruments, the songs will have a perceptible groove.

###### Each time the melody’s clock activates, this method is called to play the current note
![Image6](/images/image_6.png)

###### Abridged code for generating the melody based on the snare drum backbeat
![Image7](/images/image_7.png)

The final step was adding nice sounding virtual instruments to make the song sound more like rock. Since I wanted the instruments to sound like real acoustic instruments, I found some high quality virtual instruments to use. I ran the output of these through a reverb plugin as well in order to add another layer of cohesion to the sound. This was important to me since having songs sound good right out of the box was a crucial aspect of the project.

###### Virtual instruments and effects for the melody (Electric guitar) in Max
![Image6](/images/image_8.png)

###### The entire Max application including note and data visualization
![Image7](/images/image_9.png)

## Summary and Future Work

The biggest challenge was breaking the project down into the smallest pieces I could since there was so much I didn’t know how to tackle. I had to start with pieces that I knew I would be able to scale up. This went from deciding what data structures to use to hold notes and how to keep track of a measure, then up to putting multiple measures for an instrument together to create a musical phrase, then putting phrases together to create songs that had two distinct sections. It was also a good learning experience with abstraction, since the only reason I was able to make four different instruments was by figuring out what commonalities would be shared between them. This allowed me to add instruments easily.

Even with the amount of structure set in place, the majority of the songs were not all that pleasing to listen to. I think more work could have gone into the melody, perhaps by increasing the likelihood of playing the notes from the underlying chord progression. I would have liked to add a user input to change keys since songs are limited to A minor. It would be fairly easy to switch to other minor keys but I would have also liked to allow the user to generate songs in major which I did not even experiment with. Along those lines, I would have liked to be able to randomly generate chord progressions. This would make the songs less predetermined. With randomly generated chord progressions I would more easily be able to expand to major key and perhaps even different modes and other interesting scales. I would have liked to make it so that the drums were procedurally generated although that might be fairly challenging.

Overall the project was a great experience and I am happy with the final result. It was rewarding to be able to bring it through all of the steps of development from the initial concept to the final product. Developing the algorithms for music generation was also exciting as I was able to boil down some basic ideas for how songs are structured and work that into my application. While there is so much that I would have liked to add, the final product was still so satisfying to listen to. It’s so interesting to find one of the generated songs that I really liked and think about how no human actually wrote that song. It’s a unique experience and I was happy to have worked on this project.

### Bibliography

- Cycling ‘74. “Max API.” https://cycling74.com/sdk/max-sdk-8.0.3/html/index.html
- Cycling ‘74. “Max 8 Documentation.” https://docs.cycling74.com/max8
- Fox, C. (2006). Genetic Hierarchical Music Structures. FLAIRS Conference 2006, 243-247
- Franklin, J. A. (2006). Recurrent Neural Networks for Music Computation. INFORMS Journal on Computing, 18(3), 321–338.
- Papadopoulos, George, Geraint Wiggins (1999). AI Methods for Algorithmic Composition: A Survey, a Critical View and Future Prospects. Proceedings from the AISB’99 Symposium on Musical Creativity, Edinburgh, Scotland, 110-117.
- Wooller, R., Brown, A., Miranda, E., Berry, R., Diederich, J. (2005). A Framework for Comparison of Process in Algorithmic Music Systems. Generative Arts Practice, 5-7.
