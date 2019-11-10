# BenniesScript
Roll20 script to ease dealing Savage Worlds Bennies to players.

## Setup
You need a Roll20 Pro Subscription to be able to use API Scripts.

You will need to install two scripts :
* apicmd.js that you can find [apicmd GitHub](https://gist.github.com/goblinHordes/7424738)
* this SWBennies.js script from [Gronyon's GitHub](https://github.com/gronyon/BenniesScript)

apicmd is used to parse the BenniesScript input (why reinvent the wheel when it has already been created and nicely round ?).

You will also need to create a Deck of "cards" for your bennies, in your Roll20 game.
* Name of the deck : it will appear in your players hand of cards and will also be used in this script API commands. Something like "Bennies" should do.
* Set the Deck to unlimited.

![Benny deck setup](doc-assets/benny-deck.png)

* Create one card, give it a name. This name can be used in the API (see below).
* Upload a visual for your card and the deck background. I personnaly use the same image.

![Benny card setup](doc-assets/benny-card.png)


## API Commands
### Dealing bennies to a player
This will give a single benny to the given player :
```javascript
!bennies-deal --player PlayerDisplayName --deck NameOfTheBennyDeck
```

![Dealt one benny](doc-assets/deal-1.png)

![One benny in hand](doc-assets/gronyon-hand-1.png)

If player display name contain spaces or special characters, use quotes (same for your bennies deck name if needed) :
```javascript
!bennies-deal --player "A player name" --deck "Wonderful Bennies of Awesome"
```

If you ever need to give more than one benny to a player :
```javascript
!bennies-deal --player PlayerDisplayName --deck NameOfTheBennyDeck --quantity 4
```

![Dealt four bennies](doc-assets/deal-4.png)

![Four bennies in hand](doc-assets/gronyon-hand-4.png)


And you can short it all :
```javascript
!bennies-deal -p PlayerDisplayName -d NameOfTheBennyDeck -q 2
```

If you don't provide a player name, the command will list all online players and let you pick one :
```javascript
!bennies-deal -d NameOfTheBennyDeck
```

![Player name omitted will list all players](doc-assets/omit-player-name.png)

### Dealing bennies to every player
Sometimes you want to reward each player with a benny, for example when a Joker was drawn. Use "all" for the player name.
```javascript
!bennies-deal --player all --deck NameOfTheBennyDeck
```

You could also grant multiple bennies to each player:
```javascript
!bennies-deal -p all -deck NameOfTheBennyDeck -q 3
```

### Using bennies (players)
No need to use script api here. Players will simply drag and drop their benny card on the table.

![Drag and drop a benny](doc-assets/drag-and-drop.png)

If that was a mistake, they can simply pick it back.

![Pick the benny back](doc-assets/oops.png)

If it wasn't, Game Master can simply delete the benny from the table.

![Delete benny from table](doc-assets/pick-it.png)


### Reseting bennies at the start of a game
At the start of each session, you want to refill each player to its standard starting pool.

You can use the reset command, which syntax looks exactly as the deal command, but it will reset to the given quantity instead of adding.
```javascript
!bennies-reset -p PlayerDisplayName -d NameOfTheBennyDeck
```
Bennies will be reset to 3.

If player has Luck or similar Edge, you can set the quantity :
```javascript
!bennies-reset -p PlayerDisplayName -d NameOfTheBennyDeck -q 4
```

![Reseting bennies](doc-assets/benny-reset-4.png)

## Setting up macros

You can of course create macro so you don't have to type the commands each time :

![Reset macro](doc-assets/simple-macro.png)

Note : sometimes when you put more than one command in a single macro, it might stop to the first line and ignore the rest. When you insert an empty line on top, I find it does always execute all lines.

You can add this macro to your macro bar. However macro bar becomes quickly overburdained.

You can use UTF-8 characters that look like icons for macro names. You can find such icons on sites like [utf8icons](https://www.utf8icons.com/subsets/miscellaneous-symbols-and-pictographs/100) or [emojipedia](https://emojipedia.org/). For example, here I used a "cookie" for the macro name (the macro will ask me to pick a player, and will deal her 1 benny) :

![Macro with Cookie name](doc-assets/macro-with-utf8-icons.png)

This will nicely display in your macro bar :

![Cookie macro displayed in macro bar](doc-assets/icons-in-macro-bar.png)

### Using a Macro Character Sheet
To save macro bar space, and for macro you don't need that often, you can create a fake character, e.g. named "Benny Dealer" and have her Abilities to store the macros.

![Macro Character](doc-assets/benny-dealer-character.png)

Have each ability display in the Token Action bar.

Put a token of this character somewhere hidden on your game table, and you can roll your macros from the token bar.

![Token Bar](doc-assets/benny-dealer-in-game.png)


## Handling different type of bennies
If your game is using different types of bennies (like in Deadlands), for example, a Classic benny that can reroll a trait, a Soak benny that can only soak wounds, and a royal benny that can be used for both, you can use a different deck for each type of benny, and change the Deck name in the command.
```javascript
!bennies-deal -p PlayerDisplayName -d Royal
```

Those bennies will appear in separate sections (one per Deck) in the player's hand :

![One deck per type of bennies](doc-assets/multi-bennies-as-multi-deck.png)

Or you can put all bennies as different cards in the same unique deck. Each card must have a different name.

![All bennies in same deck](doc-assets/multi-benny.png)

You can then deal a specific benny to a player by adding a --card argument :
```javascript
!bennies-deal -p Dude -d Bennies --card Royal
```

Bennies will all appear in the same section :

![Single benny section](doc-assets/single-benny-section.png)

To reset bennies, you will have to specify each type of benny for each player :
```javascript
!bennies-reset -p Dude -d Bennies --card Classic --quantity 3
!bennies-reset -p Dude -d Bennies --card Soak --quantity 1
!bennies-reset -p Dude -d Bennies --card Royal --quantity 0
```

## Advanced usage
### Using player IDs
Instead of giving bennies to a player by giving her name, you can use her ID:
```javascript
!bennies-deal -p "-L8nrK8CDBqUbROE_YvW" -d NameOfTheBennyDeck
```
Note that you must use quotes around the id, to avoid the leading hyphen of Roll20 Id to be identified as an option to the command line.
