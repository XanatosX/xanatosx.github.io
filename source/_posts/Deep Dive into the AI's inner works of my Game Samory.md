---
layout: post
title: A Deep Dive into the AI Mechanics Behind Samory
tags: 
    - Godot
    - Samory
categories:
    - Game Dev
date: 2025-03-15T20:41:57+01:00
updated: 2025-03-15T20:41:57+01:00
---

As the title suggests with this post I want to make a deep dive into the AI of Samory. I properly did totally over engineer this for a game like
that but cause it was a hobby project time and money was no concern.

As a player you do notice there are three different difficulties for the AI which do behave a little bit different and as there name suggest,
are more or less easy to beat.

The system I was aiming for should be easy to implement and easy to create new difficult levels or tweak the ai also I did want an ai which does not cheat.

## Preparation to get the AI working

To get the AI working I did need a reliable way of identifying a card, to achieve this cards do get a ID this id is unique per card type and theoretically per game round.
As a deck is getting loaded all cards will get there id starting from 0 after that the whole deck is copied to create two pairs. Before placing the deck it does get shuffled
and placed right after.
As the cards get placed they will get a grid position assigned, which is a object containing a `x` and `y` int component, this is unique per card and represents the position,
inside of the grid. This starts with `0,0` in the left corner and ends with `n,m` in the bottom right. This Information can be used to let the AI identify a card, the id 
does allow it to check if cards a matching while the position does allow the AI to precisely reveal a card.

To store this information I created a  `CardInformationResource` which the AI can use. You can check that resource [here][card-information-resource].

On top of the information where to find a card the AI does need to know if it is currently playing, therefor the AI is listening to the `PlayerChanged` signal,
the player resource does contain a method which tells the AI node if it is a player or not.

## The two parts of the AI

The game AI itself is splittet into two components, one component is the memory, remembering cards already revealed by the AI or any other player, this system
does prevent the AI from cheating because it is only aware of cards which were shown already.
If a card is getting removed from the board it will also be removed from the AI's memory to prevent further issues.

The second part of the AI are actions, a action will describe some sort of logic to decide which card should be turned.

## How does the AI Work

If a round should be played by the AI it will get the `AIDifficultyResource` object of that player.

> The [AIDifficultyResource][ai-difficulty-resource] resource does contain the name of the ai as a translation key,
> , along with a set of actions the AI can perform. Additionally, it contains an instance of a blackboard for providing a memory to the AI.

After getting that information it will start a timer to appear somewhat random to the player how long it takes to reveal a card, this should fake the illusion of "thinking".
If the timer triggers the AI will reveal a card and increase a number to keep track of the number of revealed cards for the current round.
If this number does reach 2 the AI will stop the timer and block any revealing, now the player need to end the round or  wait for n seconds.

If the AI does find a matching pair it will be informed by the `card_was_identically` signal, which does reset the counter to 0 and starting the timer again. This
allows the AI to continue playing.

### The Blackboard or Memory

The blackboard is an object which works as the AI memory, this board is unique per AI agent. The blackboard does store information of card id's and there position. To prevent
the AI from having a perfect memory it can hold a defined number of cards based on the AI difficulty. To ensure that only the oldest card will be removed the blackboard uses a 
list for all the card information, if a card is getting revealed it will be put at the end of the list, if this card is already known it will be moved from there current position to
the end as well. If the memory reaches it threshold the card which is first in the list will be removed. This does prevent an unlimited memory for the AI agent.

You can check the [code here][game-ai-blackboard]

### The Actions of an AI agent

Let's talk about the actions of the AI. Each AI agent can have "n" different actions, each action does have a probability how often it can be used. In case of an AI term the AI will
get all there actions this agent can perform, after that it will run a function on the action to check if the action can run with the information stored in the blackboard and with the
reference to the card grid. This method can check stuff like

- Are there any cards stored in the blackboard?
- Are there any cards on the grid which aren't stored in the blackboard already?
- Are there any cards on the grid?

Each valid action will be put into a list of valid actions. Those actions will not be put "n" times in a list based on there probability. Now a random number generator will pick a number
which defines the action chosen. The AI agent will now run the action, which does reveal a card. This process get's repeated until the round does end.

As example checkout the [open random card][game-ai-action] action

## Final words

You liked what you have read, you might be interested in reading [The Creation of Samory][the-creation-of-samory]

## Links

- [Samory Itch.io][samory-itch]
- [Samory Code][samory-code]

[card-information-resource]: https://github.com/D-Generation-S/Samory/blob/main/src/game/ai/CardInformationResource.gd
[game-ai]: https://github.com/D-Generation-S/Samory/blob/main/src/game/ai/GameAI.g
[game-ai-blackboard]: https://github.com/D-Generation-S/Samory/blob/main/src/game/ai/blackboard.gd
[game-ai-action]: https://github.com/D-Generation-S/Samory/blob/main/src/game/ai/OpenRandomCard.gd
[ai-difficulty-resource]: https://github.com/D-Generation-S/Samory/blob/main/src/templates/AiDifficultyResource.gd
[the-creation-of-samory]: https://xanatosx.github.io/2025/03/14/The%20Creation%20of%20Samory/
[samory-itch]: https://xanatos.itch.io/samory
[samory-code]: https://github.com/D-Generation-S/Samory