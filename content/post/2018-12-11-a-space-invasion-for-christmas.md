---
title: A Space Invasion for Christmas
author: 'Josh  P. Sawyer'
date: '2018-12-11'
slug: a-space-invasion-for-christmas
categories:
  - Games
tags:
  - html5
  - canvas
  - javascript
image:
  caption: ''
  focal_point: ''
---

This [cool guy that I know](https://twitter.com/markahinchliffe) reached out to me about a quick little project - A Christmas Themed Space Invaders - with a very quick turnaround time.

Some googling lead to an [MIT licensed Space Invaders](https://codepen.io/adelciotto/pen/BHuGL) by [John Resig](http://ejohn.org/) - who incidentally has [a totally kick-ass project on Ukiyo-e](https://ukiyo-e.org/).

Swapping the sprites was a straightforward redefining of rects against a new spritesheet, but the original code had no sound effects. I ran into two roadblocks with this one:

##
## iOS Requires A User to Invoke Audio

On Windows, I can preload an <audio> tag and play it as soon as the game begins - not so with iOS, which specifially requires tying it to a user event. [This blog entry explains the issue succinctly](http://blog.gopherwoodstudios.com/2012/07/enabling-html5-audio-playback-on-ios.html).

The fix is simple: I already had an event listener waiting for keyboard input, so I tied audio loading to that first 'enter' which allowed me to play it at from code without further user interaction. I tied an explicit ```loadSounds()``` function to a keydown event listener.

```
function loadSounds() {
  if (!soundLoaded) {
    bulletAudio = new SoundPool(6,"bullet_effect_final.wav");
    explosionAudio = new SoundPool(6,"explosion_effect.wav");
    playerDeathAudio = new SoundPool(3,"player_death.mp3");
    backgroundMusic = new sound("background.mp3");
    recordScratchAudio = new sound("scratch_v2.mp3");
    soundLoaded = true;
  }
}
```
```
  document.addEventListener('keydown', loadSounds);

```
and my audio worked on iOS!

The final project is visible - and playable - on [GitHub](https://github.com/joshpsawyer/space-invaders-xmas).

<iframe src="https://giphy.com/embed/d2Zktmc1QMCTXtfi" width="480" height="360" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>