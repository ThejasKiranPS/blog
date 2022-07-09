---
title: "Pitivi GSoC22 update - Improved audiopreviewer"
date: 2022-07-09T0:18:15+05:30
draft: false
---

[Intro =>](https://thejaskiranps.github.io/blog/posts/ep0_the_journey_begins/)

Hey there!
It's been a month since I started working on Pitivi and here's my progress.

### More precise audio waveform
Pitivi now produces 5x more detailed waveforms for audio ([MR not yet merged](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/444)).
![before and after](/blog/static/pitivi-waveforms.jpg)
These are the some snaps of before and after (top & bottom respectively). If you have any suggestions or ideas
for improving the audio preview then please do mention them [here](https://gitlab.gnome.org/GNOME/pitivi/-/issues/2532). We would love to hear it!

During this I also found another bug which were causing inacurrate rendering of waveforms 
while changing the speed of clip and fixed that too.

[Click here](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests?scope=all&state=merged&author_username=thejaskiranps) to see all other minor fixes.

And of course all of these were done with huge help from my mentors Fabián Orccón and Alexandru Băluț.

Next we are planning to [bring back the autoaligner](https://gitlab.gnome.org/GNOME/pitivi/-/issues/1345). It allows one-click alignment of audio clips with different recordings of the same audio.
