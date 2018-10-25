# wrangle
Files dedicated to creating a tool to leverage VMAF - Netflix's open-source video quality assessment algorithm (https://github.com/Netflix/vmaf) to compare videos. One thing it will do is pull out a representative 10 frames from two local videos and provide feedback based on that. This repo will also contain 1) a set of instructions on how to you use the tool, 2) Research on how best to interpret the results. This will be done by December 14 . I'll explore a browser-based version of the tool (Javascript with JQuery and Node.js possibly), and a golang version.


# Progress so far

VMAF is an open-source library available on Github. Peruse it in the link above (^). It can be used in many ways.

I'll attempt right here to explain my understanding of how we might use it.

First - we need to understand how it works a bit. We need to test it out.

To that end Greg had a docker container set up and someone installed VMAF on it.

I then messed around with Docker for a long while (eventually learning what it is and how to use it). Much sublime containerization later, we resume our story with me figuring out what YUV files are (one of the possible inputs that VMAF takes).

These are a 'raw' type of video file. I found a way to view them eventually, but it was really hacky. I would much rather not deal with that.

Fortunately I do not have to. FFMPEG is a service (https://www.youtube.com/watch?v=MPV7JXTWPWI - this is the youtube video that i visited first on chrome on this computer so it continues popping up every time I start typing in youtube.com. And honestly this dude is really entertaining in the way that he speaks. I have half a mind to make a really terrible remix/dub of sections of his content over vaporwave or posthardcore metal). Ahem - as I was saying.

FFMPEG is a service that purports itself to be a 'complete' solution, 'cross-platform'. It's for recording, converting and streaming audio and video. We only really need to be able to (possible convert but mostly) stream audio into a system that funnels video files into VMAF (would this be a stream?) and then retrieve the results somehow.

I'm envisioning Wrangle as a system that takes in different formats of video files (lets say mp4 as the first one and go from there). The way we will input the video files is undecided as of yet, but for development I'll make a way to input via file upload in a browser.

So I'll put up a site on a localhost:8000 or something similar (either through golang or via php/node.js). Through that interface I'll be able to upload files to the program I'm creating. That's fine, at least for now.

At the end of this project I'd like to be able to take in video content via some other method (for example), a stream, an endpoint, a feed, something I don't know. I think asking Brian or Greg about that might make the most sense.

So to recap

#Intake
- individual file upload
- multiple files upload
- feed/endpoint/other

Once we have the files intooken(derwunderkind) we shall want to do things with them ~ but first ~

#Storage
- we must have a primitive database, or at least a big hashmap where we reference everything, or a custom object (I'm more partial to this one, make it iterable etc)
- We will also have to figure out where to store our results. I'm just going to say we'll store them in a similar way to the actual files, maybe in an object that references both?
- I'm a little fuzzy on this, will involve *Learning*

Next, once we figure out how to store the files and reference them, we got to figure out how to tell the program what we want to do with these files. So ...

#Instructions (how do we know what we're trying to do with these files?)
- An enumeration exists in an api or hardcoded somewhere that lists the tasks we can do, JSON and plain should formats
- these tasks will be
  - get score for one comparison (master to subject)
  - get score for many comparisons (master to subject and subject ...)
  - get abbreviated score for one comparison (master to subject), 10 samples evenly throughout
  - get abbreviated score for many comparisons (mast to subject and subject ...), 10 samples evenly throughout each

    These samples shouldn't be one frame, they should be lets say 10 frame samples. Idk. That's arbitrary.

And finally, we want to present the findings that we have in a way where we can interpret them and draw conclusions from them.

#Presentation
- probably just output as a JSON from an API if I have any say on the matter, perhaps with Express, perhaps with Golang.
