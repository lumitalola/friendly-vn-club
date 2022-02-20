# R\*n'py guide for getting started

Abstract: In this guide I will present to you a use case for R\*n'py where you start off with a dialogue script in the format that one might write one if asked on the street to write some dialogue and end up with a short VN implemented in R\*n'py. You can follow along by downloading the assets for this guide here LINK. Following along will hopefully demonstrate to you how to write simple linear scenes in R\*n'py as well as non-linear, branching narratives. These are skills you can easily expand upon to create complex and impressive narratives!

## Starting point

You start off with the following dialogue implemented in some sort of ad-hoc dialogue format with some play script elements such as dialogue, stage directions and narration:
```
Scene 1: Smidley and Grimson meet under a streetlight
  Smidley: Ah, good. You've arrived.
  
  Grimson: Of course, of course I've arrived. Don't I always arrive? If we've agreed on it--I arrive.
  
  Smidley: Well, sure, whatever, anyway--let's get started then.
  
  [SMIDLEY and GRIMSON begin to throw rocks in a bog in order to hopefully fill it up and turn it into a walkable road, this will never work.]
 
Scene 2: Elsewhere, a businessman, Lindol, educates his son.

  Lindol: Listen, come here, listen to me now.
  
  Son: What is it, papa?
  
  Lindol: One day I'll lose you, ok, and you'll lose me. ... . I guess. I want to give you something... something to help you make it out there without me.
  
  [LINDOL hands his son a Peloppian Shortsword.]
  
  Son: Wow...
```

The events of these scenes are nonsensical, but let's see about translating them into functional R\*n'py scenes anyway

## Setting up a project

To start off we will download R\*n'Py from the official site at [https://www.renpy.org/latest.html](https://www.renpy.org/latest.html) and install it.

Afterwhich starting where we would start with any other R\*n'Py project we create a project through the launcher. (*Create New Project* in The R\*n'Py Launcher). The Launcher will create for you a directory named after the name you chose for you project in the Projects Directory of your choosing (check where this is from the Launcher preferences). The directory has the following file structure inside a folder named what you named your project:

+ game
  + audio
  + cache
  + gui
  + images
  + saves
  + tl
  + gui.rpy
  + gui.rpyc
  + options.rpy
  + options.rpyc
  + screens.rpy
  + script.rpy
  + script.rpyc
+ log.txt
 
Out of these, we are interested in the file `script.rpy`, this is where the engine will start looking for dialogue and scenes to display, so it's where we'll start writing ours.
 
 Now we also happen to have ready some textures for our 4 characters and 2 backgrounds. Unpack the `assets` folder the theme zip you downloaded before into the `game` folder, such that is in the same directory and at the same level of depth as the `script.rpy` file. After you've done this the game directory will include the a folder called `assets` with subfolders for `characters` and `backgrounds`. After this is done R\*n'Py will be able to find our textures when we reference them in our script in `script.rpy`. The `script.rpy` file comes with some contents already but for our purposes these should be erased at the start.

## R\*n'py Scripting

Before we write our dialogue however we should establish who our characters are and what kind of locations they're operating in. To define our character Smidley we will write the following line at the top of the `script.rpy` file:

```
define S = Character('Smidley', color="#000080")
```

This tells R\*n'Py that we would like a character named Smidley to appear and that we'd like their text to be navy blue ("#000080" is a hex code representation of the navy blue color, to look up hex codes for colors you can simply google it, that's what I did here lol). `S` is how we'll refer to Smidley in our script to save ourselves from typing out the remaining characters of their name but this could just as well be replaced with their whole name or another signifier entirely. We define the remaining characters similarly:
```
define G = Character('Grimson', color="#990000")
define L = Character('Lindol',  color="#50C878")
define son = Character('Son', color="#FFFF00")
```

---

Now that we have our characters we can start on the dialogue. Scripts in R\*n'Py are organized into labels, which can be thought of as scenes but are more like abstract containers for whatever action happens on screen and can span multipled scenes or be part of an incomplete one. Here we will give each of our two scenes their own label. To define a label we write: 
```
label Scene1:
  
```
Adding our narration, dialogue and stage direction we get the following label for Scene 1:
```
label Scene1:
  "Smidley and Grimson meet under a streetlight"
  
  S "Ah, good. You've arrived."
  
  G "Of course, of course I've arrived. Don't I always arrive? If we've agreed on it--I arrive."
  
  S "Well, sure, whatever, anyway--let's get started then."
  
  "SMIDLEY and GRIMSON begin to throw rocks in a bog in order to hopefully fill it up and turn it into a walkable road, this will never work."
  
```

Before this we have to specify that scene 1 is where we want our VN to start by writing:
```
label start:
  jump Scene1
```
after the character definitions. `jump` is how we tell the engine from move from one scene to another. Running this we would get the dialogue of our first screen presented to us in colors but without any images. Background images and images for our characters can be added resulting in the following:
```
label Scene1:

  scene bg streetlight
  
  "Smidley and Grimson meet under a streetlight"
  
  show smidley neutral at left
  S "Ah, good. You've arrived."
  
  show grimson sheepish at right
  G "Of course, of course I've arrived. Don't I always arrive? If we've agreed on it--I arrive."
  
  S "Well, sure, whatever, anyway--let's get started then."
  
  hide smidley
  hide grimson
  "SMIDLEY and GRIMSON begin to throw rocks in a bog in order to hopefully fill it up and turn it into a walkable road, this will never work."
```
the scene command sets as the background image our image file `bg streetlight.png` which we imported from the zip. Similarly the show command knows to look for a file called `smidley neutral.png` and positions it at the left of the screen as per our request. Other image files that start with `smidley` R\*n'Py knows to associate with our character of the same name as well and this can be used in various ways that we won't get into here. The `hide` command gets rid of the characters we've asked to be shown before.

Next we will write scene 2 similarly with the addition of a switch in emotion for Lindol during the dialogue. We'll name scene 2 `Father_and_Son` in code to demonstrate that the labels can be whatever you like as long as they don't include spaces or other forbidden characters. Doing this results in the following script for our VN-ified script (NOTE: the return at the end of scene 2, this tells the engine that our VN is over.):
```
define S = Character('Smidley', color="#000080")
define G = Character('Grimson', color="#990000")
define L = Character('Lindol',  color="#50C878")
define son = Character('Son', color="#FFFF00")

label start:
  jump Scene1

label Scene1:

  scene bg streetlight
  
  "Smidley and Grimson meet under a streetlight"
  
  show smidley neutral at left
  S "Ah, good. You've arrived."
  
  show grimson sheepish at right
  G "Of course, of course I've arrived. Don't I always arrive? If we've agreed on it--I arrive."
  
  S "Well, sure, whatever, anyway--let's get started then."
  
  hide smidley
  hide grimson
  "SMIDLEY and GRIMSON begin to throw rocks in a bog in order to hopefully fill it up and turn it into a walkable road, this will never work."
  
  jump Father_and_Son
   
label Father_and_Son:
  
  scene bg study
  
  show lindol pensive at left 
  L "Listen, come here, listen to me now."
  
  show son receptive at right
  son "What is it, papa?"
  
  show lindol forlorn at left
  L "One day I'll lose you, ok, and you'll lose me. ... . I guess. I want to give you something... something to help you make it out there without me."
  
  "LINDOL hands his son a Peloppian Shortsword."
  
  show son thankful at right
  pause # The pause command tells R*n'Py to move on from an image only after the reader clicks.
  # By the way everything in a line followed by a # is a comment. The purpose of these is to write notes on your script that don't end up
  # being shown to the reader but are there to help you keep track of things!
  
  return
```

Now running our script results in a little VN segment that seems to be quite faithful to our original script! If you only wish to create linear stories that have no player input, these are all the skills you need! However for more complex narratives with player input we must go beyond linearity.

## Beyond linearity

Here we will go over two crucial tools for creating branching narratives that the player can impact: variables (also called flags) and menus.

Before going into implementation details let's create a necessity for non-linearity in our narrative. Let's decide that scene 2 is followed by a third scene where it's revealed that the son in scene 2 is actually Grimson from the past via him mentioning receiving an object from his father as a child, and in scene 2 the player/reader gets to choose which object young Grimson's father gifts him. Then the choice the player/reader makes in scene 2 will interactively effect what happens in scene 3!

Variables/flags are simply values that we can set at a part in our label and then reference at a later point in that label or in a later label. If we wanted to define a flag birthday to hold the value "January 13th" we would write `$ birthday = "January 13th"` if we were inside of a lable, or `define birthday = "January 13th"` if we were outside of one. Then, referencing that flag later dialogue we could write a narrator line `"Erika's birthday is on [birthday]"`. If we wanted to only display a line of dialogue for a given birthday value we would write the following inside a label (here the flag `lisa_birthday` has been set to some value in the same manner that `birthday` was):
```
  if birthday == lisa_birthday :
    "Erika and Lisa share birthdays!"
```
Variables can also contain more complex data (refer to the official documentation for further help) however for our purposes this will suffice.

Next, moving onto menus, they're a list of options that pops up that the reader can click on and depending on which option they click on we can make decisions about how that affects the story. If we wanted to let the reader pick the color shirt a character is wearing we could write (the name of the label that follows the menu is arbitrary):
```
menu:
  "I wonder what color shirt Lisa will wear today?"

  "Blue":
    $ color = "blue"
  "Red":
    $ color = "red"
  
label after_menu:
  "Lisa picked the [color] shirt!"
---
Menu options can also result in us jumping to another scene as follows:
```
menu:
  "I wonder what color shirt Lisa will wear today?"
  
  "Blue":
    jump blue_shirt_scene
    
  "Red":
    jump red_shirt_scene
    
label red_shirt_scene:
  "This scene is only played if the reader picked the red shirt!"
  jump after_shirt
  
label blue_shirt_scene:
  "This scene is only played if the reader picked the blue shirt!"
  jump after_shirt
  
label after_shirt:
  "Wow! What shirt-selection fun!"
```

OK! Now that we knwo what menus and flags/variables are we are ready to non-linearize our game! Putting these concepts to work in our game we will arrive at the following script:

```
define S = Character('Smidley', color="#000080")
define G = Character('Grimson', color="#990000")
define L = Character('Lindol',  color="#50C878")
define son = Character('Son', color="#FFFF00")

label start:
  jump Scene1

label Scene1:

  scene bg streetlight
  
  "Smidley and Grimson meet under a streetlight"
  
  show smidley neutral at left
  S "Ah, good. You've arrived."
  
  show grimson sheepish at right
  G "Of course, of course I've arrived. Don't I always arrive? If we've agreed on it--I arrive."
  
  S "Well, sure, whatever, anyway--let's get started then."
  
  hide smidley
  hide grimson
  "SMIDLEY and GRIMSON begin to throw rocks in a bog in order to hopefully fill it up and turn it into a walkable road, this will never work."
  
  jump Father_and_Son
   
label Father_and_Son:
  
  scene bg study
  
  show lindol pensive at left 
  L "Listen, come here, listen to me now."
  
  show son receptive at right
  son "What is it, papa?"
  
  show lindol forlorn at left
  L "One day I'll lose you, ok, and you'll lose me. ... . I guess. I want to give you something... something to help you make it out there without me."
  
menu:
  "What item does Lindol give his son?"
  
  "A Peloppian Shortsword":
    $ item = "Peloppian Shortsword"
  
  "A Koumblai Spear":
    $ item = "Koumblai spear"

label Father_and_Son_aftermenu:
  "LINDOL hands his son a [item]."
  
  show son thankful at right
  pause
  
  jump scene_3
  
label scene_3:
  scene bg streetlight
  
  show grimson conniving
  G "It's a good thing I brought my [item] my father gifted me when I was a small child..."
  
  return
```

Run this with R\*n'Py to see if it works and feel free to experiment with using the menus with `jump` commands to de-linearize further! The R\*n'Py official documentation contains further info about these concepts and more, I highly recommend giving it a look!

---

Hopefully you were able to follow along and create something that looks like the script at the beginning in R\*n'Py! For further reading consult the official documentation ([https://www.renpy.org/doc/html/index.html](https://www.renpy.org/doc/html/index.html)) which takes some hours to read through but presents a whole world of possibilites we didn't get into in this guide. The possibilities involve introducing player choice and branching narratives as well as nicer visual effects. I would suggest at least looking at scene transitions which we did not cover in this guide, since they add quite a lot of nice liveliness to the visuals ([https://www.renpy.org/doc/html/quickstart.html#transitions](https://www.renpy.org/doc/html/quickstart.html#transitions)). R\*n'Py's visuals are also pretty customizable though it's a little bit complicated. Thank you for reading!!!
