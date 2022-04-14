[TIC-80](https://tic80.com/) is a free fantasy console for making, playing, and sharing tiny games.

It has lots of built-in tools for development, like code, sprites, maps, sound editors, a command line, and a Surf browser.

Its main development language is [Lua](https://www.lua.org/), but it supports a bunch of other languages too, such as [Moonscript](https://moonscript.org/), [Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript), [Wren](http://wren.io/), and [Squirrel](http://www.squirrel-lang.org/).

> Talking about Wren by the way, you can check the amazing [Dome](https://domeengine.com/) game engine.

Aiming to build a brief presentation, I’m coding a turning Yin-Yang symbol.

### Opening TIC-80

Once you open the application, you got the prompt:

    TIC-80 tiny computer
    version 0.90.1723 (9c38a80)
    https://tic80.com (C) 2017-2021
    
    hello! type help for help
    
    >

Or something like this. In order to start a new project, use the `NEW` command, informing the language you mean to use:

    > NEW moon
    new cart is created

### Coding headers and background

Now press `Escape` to enter the code editor. It starts with a boilerplate we don’t need, let’s delete the code, keeping only the headers:

    -- title:  game title
    -- author: game developer
    -- desc:   short description
    -- script: moon

Update the headers to reflect our intension:

    -- title:  Yin Yang
    -- author: Kodumaro
    -- script: moon
    export ^

We’re gonna do some black magic hereafter… This code was taken from the [TIC-80’s wiki](https://github.com/nesbox/TIC-80/wiki/Sky-gradient), and create a beautiful blue background gradient:

    fst_tick = true
    
    SCN==>
     poke 0x3fc1,@
     poke 0x3fc2,@+120
    
    TIC=->
     if fst_tick
      cls!
      fst_tick=false

The `fst_tick` variable grants the `cls!` (clear screen) is called only once.

**Note:** TIC-80 code cannot be larger than 65 536 characters, so avoid unnecessary whitespaces.

If nothing is wrong, when you press C-r, you’re gonna get the following background:

![Sky Gradient](//cacilhas.info/img/tic80/sky-gradient.png)

`Escape` leads you back to the prompt, `escape` again to the code editor.

### Creating the Yin-Yang symbol

Now we need to initialise the variables:

*   `c` for the screen centre – TIC-8 resolution is 240×136
*   `sin` and `cos` for the trigonometrical functions – taken from `math` package
*   `pi` for π – taken from `math` package
*   `tau` for τ – calculated from `pi`
*   `angle` to control the turning angle
*   `rm` and `rM` for smaller and greater circle radii respectively
*   `yin` and `yang` for the colours

In TIC-80, colours are stored in a palette, defaults to:

![Sweetie-16](//cacilhas.info/img/tic80/sweetie-16.png)

So the variables are initialised:

    c=x:120,y:68
    :sin,:cos,:pi=math
    tau=pi*2
    angle=0
    rm=3
    rM=20
    yin=15
    yang=12

The black eye centre is calculated as a 2D vector of the current angle, away from the screen centre using the greater radius:

     c1=x:c.x+cos(angle)*rM,y:c.y+sin(angle)*rM

The white eye centre is 180° away from the other one:

     a2=angle+pi

And the centre is calculated from the angle `a2`:

      c2=x:c.x+cos(a2)*rM,y:c.y+sin(a2)*rM

Now we need to draw four circles:

1.  A big dark one in `c1`
2.  A big light one in `c2`
3.  A small light one in `c1`
4.  A small dark one in `c2`

Which, using the TIC-8 assets, is:

     circ c1.x,c1.y,rM,yin
     circ c2.x,c2.y,rM,yang
     circ c1.x,c1.y,rm,yang
     circ c2.x,c2.y,rm,yin

At the function end, the angle is updated by 180° a second – since TIC-80 framerate is 60Hz, 3° a tick:

     angle=(angle+pi/60)%tau

Running you must get the turning symbol:

![Sky Gradient](//cacilhas.info/img/tic80/yin-yang.png)

### Resouces

Download the Moonscript code from [here](//cacilhas.info/misc/tic80/yin-yang.moon), or the cartridge from [here](//cacilhas.info/misc/tic80/yin-yang.tic).