---
title: Music Generation for js13kGames
image: /assets/img/music_sunshine.jpg
---

[js13kGames](http://js13kgames.com/) is a yearly game development competition created by [Andrzej Mazur](https://twitter.com/end3r). It runs for a month and has one main constraint: don’t go over 13k. It’s quite similar to a lot of demo competitions, but for JavaScript games.

It’s particularly interesting to take a look at the older entries of this competition. If you didn’t, [take a look at it](http://2013.js13kgames.com/#winners). But there’s always one thing that bother me: most, if not all, of the entries don’t have any kind of music. Sometimes they have sound effects, but no music. And what’s a game with no music? Even oldies like Pong or Tetris had some.

So with that problem in mind, I searched for some resources to learn how to make a song synthesizer that could fit in a 13k video game. And I found one: [js-sonant](https://gitorious.org/js-sonant) may be a quite old project, and it’s not supported any more, but it sure has a lot of potential. It can be as small as 4k embedded with a 2-3 minutes song, generates 16-bit music with delays, stereo effects, etc… I don’t even have to think at how small it is to appreciate the music. It’s probably a matter of tastes, but I really like the kind of songs it generates. Take that all old-school 8-bit songs generators!

So, with that technical basis, I created my small library. And here it is, a library for video game music created for js13kGames: [Sonant-X](https://github.com/nicolas-van/sonant-x). I mostly added some missing features, like the possibility to create single sounds (for sound effects, in addition to music). It was also necessary to be able to display a “Loading” animation or whatever during the song generation (the original library was freezing the browser during multiple seconds). All that is now ready to use and documented, it’s also shipped with an application to directly compose music in the browser and export it in JSON. And if you want to know if it still fits 4k: [yes](http://nicolas-van.github.io/sonant-x/min.html) (of course with appropriate minification and compression).

Now that I have all that I just have to make a video game with it, isn’t it? Yeah, except I don’t have any more time unfortunately :) Too much work and my client wants me to finish the program for last week. The usual stuff you know… So for the future I’ll continue to support Sonant-X as I will certainly use it. But in the mean time, if someone wants to use it for js13kGames or a similar competition, I’ll just be happy to know it was useful to someone :)
