---
title: Writing games
date: 2023-06-10
tags: career education-and-culture fantasy-console game-engine lua rust scratch
image: //img.montegasppa.cc/itch-io-icon.png
Itch.io: https://itch.io/
permalink: /2023/06/writing-games.html
---
[Arkanoid]: https://spectrumcomputing.co.uk/entry/0000255
[Arkanoid NG]: https://cacilhas.itch.io/arkanoid-ng
[atari-missile-command]: https://www.arcade-history.com/?page=detail&id=1644
[Casual Funny Games]: https://scratch.mit.edu/studios/30951170
[casual-on-itch]: https://cacilhas.itch.io/casual-funny-games
[Catcheese]: https://cacilhas.itch.io/catcheese
[CuteMaze]: https://gottcode.org/cutemaze/
[fantasy consoles]: https://en.wikipedia.org/wiki/Fantasy_video_game_console
[Fairy Tale]: https://cacilhas.itch.io/fairy-tale
[ferris]: //img.montegasppa.cc/ferris-the-crab.png =84x56
[GDevelop]: https://gdevelop.io/
[gdevelop5]: //img.montegasppa.cc/gdevelop.png =84x84
[Get Out!]: https://cacilhas.itch.io/get-out
[Godot Engine]: https://godotengine.org/
[godot-image]: //img.montegasppa.cc/godot.png =84x84
[image]: {{{image}}} =128x128
[Itch.io]: https://itch.io/
[Kenney’s assets]: https://kenney.nl/assets
[Kodumaro Catch Game]: https://cacilhas.itch.io/catch-game
[kodumaro-nonogram]: https://cacilhas.itch.io/nonogram
[Kodumaro Tetris]: https://cacilhas.itch.io/tetris
[Missile Command]: https://cacilhas.itch.io/missile
[Moonscript]: https://moonscript.org/
[msx-sokoban]: https://msxgamesworld.com/software.php?id=1518
[my personal showcase]: https://cacilhas.itch.io/
[Nanpurë]: https://cacilhas.itch.io/numples
[nonogram]: https://en.wikipedia.org/wiki/Nonogram
[Raylib]: https://www.raylib.com/
[Rust]: https://www.rust-lang.org/
[Rust applications]: https://crates.io/users/cacilhas
[scratch-cat]: //img.montegasppa.cc/scratch-cat.png =84x84
[Standstill]: https://cacilhas.itch.io/standstill
[sudoku]: https://sudoku.com/
[Tetris]: https://tetris.com/
[TIC-80]: http://tic80.com/
[tic-80-image]: //img.montegasppa.cc/tic80.png =84x84
[tic-80-on-itch]: https://nesbox.itch.io/tic80
[Scratch]: https://scratch.mit.edu/
[snake]: https://cacilhas.itch.io/snake
[Sokoban]: https://cacilhas.itch.io/sokoban

:right ![Itch.io][image]

:first I’m simply fascinated by indie game development. It’s free from a bunch
of programming concerns, and allows us to explore new horizons.

I’m tired of development risks due to **money** – always money! Nothing is more
important in our Capitalist system than money, not culture, not even human
lives; money is in the society’s kernel, and it leads to a lotta stress. I’m
really tired of it.

I’d really take a way out if I find it, and indie game development seems to be
a promising one. Anyway it had never been for me.

Nevertheless, I’ve been using game development for study and to keep my mind
sharp since ever.

Said that, let’s get into some side projects I’ve been playing around with. I’m
not getting into many details, otherwise I couldn’t quote them all. If you’re
looking for more detail about any game or code here, leave a comment. 😉


### Itch.io

IMHO, [Itch.io][] is the best game release platform, and I’ve been using it as
[my personal showcase][]. Every game we’re talking about in this post you can
find there.


### Scratch games

:right ![Scratch cat][scratch-cat]

I’m talking about [Scratch][] first to sweep it away, ’cause Scratch earns its
own whole post.

I think Scratch is a great sandbox to test concepts and the place for sharing
ideas.

I have a Scratch project called [Casual Funny Games][], and we can get into it
in another post. Even on Itch.io I have a [page for it][casual-on-itch].


### TIC-80 games

:right ![TIC-80][tic-80-image]

I love [fantasy consoles][], and my favourite is [TIC-80][]: it has an old home
computer feeling which no other console manages to emulate. Therefore I
recommend purchasing its [payed version][tic-80-on-itch].

I’ve published some TIC-80 games of my own, all of them developed using
[Moonscript][]:

- [Sokoban][] is a classic among classics! I literally copied every detail from
  the [MSX implementation][msx-sokoban].
- [Arkanoid NG][] is my TIC-80 implementation of the classic [Arkanoid][], one
  of the first breakout games.
- [Missile Command][] is a clone of the
  [Atari’s same-title shoot’em up game][atari-missile-command]. Don’t let
  missiles destroy your builds!
- [Kodumaro Tetris][]: who doesn’t known the Russian [Tetris][]!? In here I took
  care of the presentation.

I don’t wanna talk about [this one][snake]… 😒


### Godot Engine games

:right ![Godot Engine][godot-image]

I reckon [Godot Engine][] is the best balanced game engine available:

- Unity 3D is easy and widely used, but lacks security; I wouldn’t recommend.
- Unreal Engine is the most powerful, but requires too much hardware resources
  both to develop and to play games.
- UPBGE is an eternal doubt.
- In GDevelop, simple things become a challenge to be achieved.
- Construct3 and Game Maker are more expensive than they actually deliver, as
  well as so many others.
- Scratch aims for Education, not commercial purposes.

I got some Godot games I’ve been working on:

- [Standstill][]: a tower-defence game. I created it to test
  [Kenney’s assets][], which are amazing. This is one of my games I’m proud of.
- [Catcheese][] is a simple maze game based on [CuteMaze][].
- [Fairy Tale][] intends to be an immersive adventure game, but I’ve miserably
  failed. It’s still funny though. 😬
- [Kodumaro Catch Game][] is what it says: a catch game. More precisely a 1D
  catch game (only moves side to side). I made the game in about one hour and
  then I spent my free time during two days trying to catch smoke. It is
  unnecessarily heavyweight, but have some cool details.


### Raylib games

:right ![Ferris the Crab][ferris]

If you need performance, I recommend [Raylib][] with [Rust][].

Raylib is exceptional for 2D games. Not that good for 3D, but still gets the job
done.

- [Nanpurë][] is a colourful [sudoku][] – instead of numbers, you got colours.
- [Nonogram][kodumaro-nonogram] is a random implementation for [nonogram][]. It
  doesn’t form meaningful pictures, but has the advantage to be able to be
  played forever.

I have a lot of other [Rust applications][], but only these two games.


### GDevelop games

:right ![GDevelop5][gdevelop5]

I couldn’t leave [GDevelop][] out. It is nothing its authors claim it is:

- It’s **not easy**.
- It’s **not no-code**.
- It’s **not RAD**.

Even so it’s really amusing, I usually have a lotta good time developing in
GDevelop.

I used to have a GDevelop game in Itch.io, [Get Out!][], however it has a very
sad story: its code wasn’t published to any remote repository, and my hard disk
got fried, so I **lost the whole code**. 😭

It’s still [published][Get Out!], but I cannot do any changes anymore.


-----

Do you like any of those games? Want any more details? Code? Development
process? Leave a comment!

:small Also in <a href="https://dev.to/cacilhas/writing-games-5clg">DEV.to</a>.
