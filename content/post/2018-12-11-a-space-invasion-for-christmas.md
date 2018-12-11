---
title: A Space Invasion for Christmas
subtitle: "HTML5 & Canvas: A Christmas Miracle"
summary: "A quick little tutorial overcoming some audio issues with Canvas & HTML5."
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

Swapping the sprites was a straightforward redefining of rects against a new spritesheet, but the original code had no sound effects.

## Adding Audio

The W3 Schools Article on game sound presents this in a straightforward manner, providing functions that can be trivially added to your code to add sound:

```
function sound(src) {
  this.sound = document.createElement("audio");
  this.sound.src = src;
  this.sound.setAttribute("preload", "auto");
  this.sound.setAttribute("controls", "none");
  this.sound.style.display = "none";
  document.body.appendChild(this.sound);
  this.play = function(){
    this.sound.play();
  }
  this.stop = function(){
    this.sound.pause();
  }
}
```

It was then as simple as creating a new sound object and playing it during the appropriate function:

```
    backgroundMusic = new sound("rockinround.mp3");
```

```
    if (!backgroundMusicStarted) {
      backgroundMusic.play();
      backgroundMusicStarted = true;
    }
```

I loaded up the game, created sound objects for my sound effects, fired a few lasers and - they wouldn't overlap.

## An HTML5 Audio Object Cannot Overlap Itself

I was referencing the same audio object for each sound effect call, therefore I couldn't play it again until it has finished playing - i.e., I couldn't have multiple overlapping sound effects.

[Steven Lambert's blog](http://blog.sklambert.com/html5-canvas-game-html5-audio-and-finishing-touches) held the answer: a Sound Pool. Essentially, we create a queue of sound objects. If we fire a laser, we see if current object is playing; if it is, we move to the next object and play it, otherwise we play the current one. My sound effect is short so I don't even bother checking to see if the previous one is done, I just jump to the next one. When I reach the end of my queue, I go back to the beginning.

```
/**
 * A sound pool to use for the sound effects
 */
function SoundPool(maxSize, src) {
  var size = maxSize; // Max sounds allowed in the pool
  var pool = [];
  this.pool = pool;
  var currSound = 0;
  /*
   * Populates the pool array with the given sound
   */
  for (var i = 0; i < size; i++) {
    theSound = new Audio(src);
    // theSound.volume = .12;
    theSound.load();
    pool[i] = theSound;
  }
  /*
   * Plays a sound
   */
  this.get = function() {
    pool[currSound].play();
    
    currSound = (currSound + 1) % size;
  };
}
```

I sent the game off to Mark and he reported back - no sound in Safari. The second roadblock:

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
and the audio was working.

The final project is visible - and playable - on [GitHub](https://github.com/joshpsawyer/space-invaders-xmas). For licensing reasons, the background music is a public domain midi, but it is particularly awesome if you're playing it with [Father Christmas by The Kinks](https://www.youtube.com/watch?v=l-oVPVsCqs4).

<iframe src="https://giphy.com/embed/d2Zktmc1QMCTXtfi" width="480" height="360" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>