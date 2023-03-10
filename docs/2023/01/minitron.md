![TIC-80](//cacilhas.info/img/tic80.png)

This is a sample code for learning, written in [Moonscript](https://moonscript.org/) for [TIC-80](https://tic80.com/):

    -- title:   MiniTron
    -- author:  Montegasppa <montegasppa@cacilhas.info>
    -- desc:    A very spartan Tron implementation
    -- license: 3-Clause BSD
    -- version: 0.1
    -- script:  moon
    export ^
    local  *
    
    local cycle, dir, tic, score
    
    size = 2
    
    move = ->
      with cycle
        next = switch dir
          when 0
            x: .x, y: .y-size
          when 1
            x: .x, y: .y+size
          when 2
            x: .x-size, y: .y
          when 3
            x: .x+size, y: .y
        reset! if pix(next.x, next.y) != 0
        rect .x, .y, size, size, 9
        cycle = next
      score += 1
    
    BOOT = ->
      cycle = x: 120, y: 68
      dir, tic, score = 3, 0, 0
      cls!
      rectb 0, 0, 240, 136, 4
      rectb 1, 1, 238, 134, 4
    
    TIC = ->
      poke 0x3ffb, 0
      rect cycle.x, cycle.y, size, size, 3
      for i = 0, 3 do dir = i if btn i
      tic += 1
      if tic == 10
        move!
        tic = 0
    
    OVR = ->
      print "Score: #{score}", 3, 128, 3, false, 1, true

Download the [card](//cacilhas.info/misc/minitron.tic)!

* * *

Also in [DEV.to](https://dev.to/cacilhas/a-minitron-in-47-lines-10d6).