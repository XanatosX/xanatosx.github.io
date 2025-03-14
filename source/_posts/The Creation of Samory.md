---
layout: post
title: The Creation of Samory
tags: 
    - Godot
    - Samory
categories:
    - Game Dev
date: 2025-03-14T20:40:27+01:00
updated: 2025-03-14T20:40:27+01:00
---
Samory: A game I did create with Godot using GDScript. Being my first project written with GDScript. 

I did try other projects before using the dotnet version of Godot but it felt like this creates a lot of overhead.  
Samory is a simple memory game with some built in decks and the possibility to side load them from you local computer.
It can be played as a hot seat game against each other or you can add some AI with various difficulties making it a singleplayer
game. The game is getting shipped as a web build or a standalone desktop for Linux or Windows.

Any links to the game, used tools or everything else can be found at the end of this post.

## The inspiration

The inspiration for the game is super simple, I did want to create a game for my son where I can use his drawn pictures,  
as memory cards. This does allow us to play with his creations. Sure I could have created physical playing cards, but that  
would be less fun as a programmer. Because I do not want to share any images of him or created by him online I did need a  
solution to create such decks without uploading the data for it, that's where the sideloading idea came from. He is also  
technically the name giver for the project. It's a combination of this name "Samuel" and the word "memory".

## The creation itself

First of all creating the game did take a lot of time for such a small project. I checked my Wakatime record clocking in at around 90 hours.  
But this time does only include the time spend in vscode so the Godot editor time does come on top. Because I learned the engine this is properly  
an additional time of 20 - 40 hours making it a total of 110 - 130 hours to create the game. This only reflects the state for the current version 0.8.0  
and might increase if more polishing is done.

As I planned the game I was sure to complete it in a few months but in the end it take about half a year, properly cause I did learn the engine while working on
it. I also did make a break cause I was distracted by gaming.

### What have I learned

#### UI

First of all I learned that it can be a bigger challenge to create a good UI on such a small project than creating the game itself.  
Getting all the settings, tutorials and popups working did took much longer than expected.

#### Code

GDScript is easy to learn if you already know a programming language, you get results with it real quick without to much overhead,
if you do not need to squeeze out every millisecond in your game and want to prevent some overhead I would suggest to you, give
GDScript at least a try its awesome.

I wasn't aware that I can await a signal, that did allow me to improve the side loading, game loading and deck viewer logic a lot.

That I have little to no knowledge on how to write shaders, but I do have the goal to get this skill into a somewhat useful state.
I do now there is a visual shader designer but I do think it's better to write them on my own and understand what I do. I might use
it after understanding how to code them.

I did make the mistake to let scripts do to many things, my current main script for the game is a beast function wise, I do feel really bad 
that I put in that much functionality instead of splitting it. Find a bug or adding a feature is not much fun for that script.
I think a better approach is to create a node per functionality and let them communicate between them instead of a big script managing all of it.

#### Editor

Resource objects can be really powerful and used as small bites of codes to achieve stuff over and over without to much Inheritance.

The best takeaway for this project is that I think I did understood how to customize the style of the UI via the editor, making it non blurry.
On previous projects I did always get a blurry UI cause I did it totally wrong.

#### Sound

A game without sound always feels boring even if the gameplay itself is fun. All the small sound details for buttons, clicks and so on are really important
to make game feel interessting.

#### Polish

As I thought the game was finished more and more ideas for quality of life features did come to my mind. There where also some bugs  
I needed to solve which weren't that easy because some parts of the game logic are really messy.

I learned a lot about optimization, as I started to make the game run smoother on mobile touch devices I learned a few tricks making the  
game less laggy. As example I did animate my shaders in the `_process` method, this did add some visual lagging to the mobile web version.  
To solve this issue I started to use the `tween` object, the performance did increase noticeable.

The project did teach me how to create a web build which does look good for desktop pcs and decent for mobile as well. To achive this
if the game does run on a mobile device it will scale up the UI and some menus will change how they get split providing more room for elements 
inside.

## Links

- [Samory Itch.io][samory-itch]
- [Samory Code][samory-code]

[samory-itch]: https://xanatos.itch.io/samory
[samory-code]: https://github.com/D-Generation-S/Samory