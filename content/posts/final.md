---
title: "Wrapping things up | GSoC22@Pitivi"
date: 2022-11-10T21:06:06+05:30
draft: false
ShowToc: true
TocOpen: true
enableEmoji: true
---

# Overview
GSoC 2022 is nearing its end and now it's time to wrap things up. My task was to 
Improve the Timeline component of Pitivi and this is my status report. 

# Work Report
## Fix a dot appearing when clicking on the timeline - [!436](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/436)
Pitivi uses a custom widget for showing the selected area while dragging & selecting. This Marquee rectangle gets drawn when the user performs a drag action. We only moved this rectangle to a new position when the user starts dragging. So when there occurs
a mouse click, the Marquee box get's drawn at the previous drag_end position which appears as a dot. 

It was fixed by adding the code to move the marquee to the callback of the event when a user first clicks. Now when clicking, the box gets drawn under the mouse.

## More precise audio waveforms - [!444](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/444)
Pitivi now shows 5x more detailed waveforms for audio!
![image](/blog/static/pitivi-waveforms.jpg)
These are some snaps of before and after (top & bottom respectively).
## Bring back the auto aligner :star: - [!445](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/445)
If I had to pick a favorite, this is definitely it. Developing AutoAligner was just pure bliss.
To be honest, this was the scariest task on my to-do list because I thought it would be a very math-intensive & complex feature to implement. 

Pitivi had this cool feature some time before but it got lost when the method of 
waveform generation and its caching changed. 

![image](/blog/static/align.gif)

This feature allows users to align audio clips recorded from different sources.
Here is the basic idea behind how it currently works:

Consider the two waves given below. 

- To calculate how well these waves match,
we add the products of values from each wave at a given time. This final sum can be used to measure the cross-correlation
between these two waves.
![image](/blog/static/corr1.png)
- Then the second wave is shifted with respect to the first one so that the pair of values from each wave at a given time is 
different than before and the calculation is repeated.
![image](/blog/static/corr2.png)
As we can see the correlation value is highest when the peaks of waves are most similar.

These videos helped a lot in understanding the basics: 
- https://youtu.be/RO8s1TrElEw
- https://youtu.be/TS8HHW-HlGA

This is the basic idea behind the alignment. The actual calculations are handled by [SciPy](https://scipy.org/) which 
makes the alignment process very fast :zap:.


## Allow separate selection of ungrouped clips - [!448](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/448)
We use [`GES.Layer.get_clips_in_interval`](https://gstreamer.freedesktop.org/documentation/gst-editing-services/geslayer.html?gi-language=python#ges_layer_get_clips_in_interval) to get the clips in a layer within an interval. The x coordinates of drag-selection are converted to 
time units(start & end) and they are passed to the above-mentioned function to get the clips that are to be selected.

The problem with this is: although Audio & Video clips reside in the same layer, they exist in different halves of the layer.
Video clips are drawn in the upper half and Audio clips are in the lower half of the layer. So even when the user meant to just select the video
clip by dragging over it, the function also returns audio clips, if any, overlapping with the same interval.

This was fixed by calculating the position of the selection-rectangle edges relative to the layer and deciding which half does the selection area starts/ends. Then we filter the clips returned from the `get_clips_in_interval` and return only the to-be-selected clips.

## Migrate to GStreamer's new mono-repo  - [!451](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/451)
As of 28 September 2021, Gstreamer modules have been merged into a single git repository to simplify development workflows and CI. We are 
working on porting to monorepo and updating the GNOME SDK used in flatpak.

## Others 
### Update colors when the theme is changed - [!427](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/427)
### Fix ticks in ruler disappearing at some intervals - [!437](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/437)
### Fix ctrl+click clears the whole selection - [!440](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/440)
### Fix waveforms not scaling when changing clip speed - [!443](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/443)
### Modify how transitions are represented - [!450](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests/450)
### [List of all merge requests](https://gitlab.gnome.org/GNOME/pitivi/-/merge_requests?scope=all&state=all&author_username=thejaskiranps)

## Rest of the issues
My GSoC proposal had some more issues mentioned mostly relating to dragging events. These were unable to complete as Pitivi is being
ported to Gtk4.0 parallelly. The common source of the issues was that the drag events won't get registered if the mouse pointer leaves the 
source element. In Gtk4 these events get delivered until the mouse is released.

So I started trying to help Aryan with porting stuff and currently, I am working on finding a replacement for `gtksink` which works in gtk4.

## Finalizing
Debugging issues in such a large codebase (relative to what I have worked on till now) was very hard, but the joy and excitement you get
when you find the source and fix those bugs makes them totally worth it. After completing the WIP issues, I would like to continue 
contributing and help in finishing the gtk4 port. 

Finally my mentors - Alexandru Băluț &  Fabián Orccón,
I cannot thank them enough for their support and guidance. They made a safe and friendly environment for me to ask for help without any
resistance. When I get stuck on something they helped a lot to guide me to the solution. Once again, Thank you so much!

That's it!  I'm concluding here with a smile on my face :)

See you soon :wave: