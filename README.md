# Aggravation

## Specs

### Game Mechanics
- RNG Dice
  ??? - Open for interface suggestions.

- Marble locations
  Colored marbles should be able to be moved between the board's dots.

- Marble displacement
  Moving a marble onto a occupied dot should displace that marble and places it just outside the dot.

- Winning
  Upon a player moving all their marbles home, a screen should be shown allowing:
  1. Good Game (shows a woohoo screen, game over)
  2. Play for Second (winner dropped out, game continues)
  3. Yea, right (reverts the winning move, game continues)

- Turn highlight (but no enforcement req'd)
  Keep track when a player moves and pass the turn

### Graphics
Minimal.  That's all.

### Extra
- Chat window
- Login / profile icons

### Touch Interaction
- Swipe/flick up for chat
- Touch zoom and pan
- Tap pick up and place marble


## Firebase API Specs
    https://blazing-torch-1472.firebaseio.com
### Authentication
*Google* OAuth

*Facebook* OAuth `// TODO get an Fb key`

***
***
***

## Frontend API

### `new App ()`
Kick the app of for the first time.  Should auto-detect any login and set `App.user`.

***
#### `User App.user`
Stores the currently logged in user data (or `null`).

***
#### `App.auth (String provider)`
Shows the OAuth popup for `provider` (`'google'`<del>, or `'facebook'`</del>) authentication.  Fires `App.trigger ('auth', User user)` on success.

***
#### `App.unauth ()`
Log the user out.  Fires `App.trigger ('unauth')` on sucess.

***
#### `App.on ('auth', function (User user) {})`
The user has been authenticated successfully

***
#### `App.on ('unauth', function () {})`
The user logged out.

***
#### `App.createGame ()`
Creates a new game on the server.

Fires `App.trigger ('newGame', String code, Game game)` then `App.trigger ('joinGame', String code, Game game)` on success where `code` is a unique game identifier (4 characters, to be shared with other players) and `game` is a new instance of `Game`.

***
#### `App.joinGame (String code)`
Finds a game on the server with `code` and joins the user to it.

Fires `App.trigger ('joinGame', String code, Game game)`  like `App.createGame`.

***
#### `App.on ('newGame', function (String code, Game game) {})`
This user has created a new game.

***
#### `App.on ('joinGame', function (String code, Game game) {})`
This user has joined a game.

***
***
### `new Game (arguments)`
Initialize the game board and pieces.

This should never be instantiated, except by `App.createGame` or `App.joinGame`.  It's arguments and such are not dictated in this spec.

***

####  `Game.on ('winner', User winner, Winning winning)`
Someone said they won.  `move` would always be triggered immediately before this event.

***
#### `Game.on ('move', User mover, Integer marble, Integer hole)`
Another player moved their piece.

****
#### `Game.on ('bump', User bumper, User bumpee, Integer marble, Integer hole)`
A player's marble displaced another one.

This should place the existing marble just to the side (and if `bumpee === App.user` show a "You been bumped!" popup).


***
#### `Game.move (Integer marble, Integer hole)`
Move our `marble` to `hole`.  Will fire `trigger ('move', App.user, marble, hole)` for all users (and possibly `trigger ('bump', App.user, bumpee, marble, hole)`).

***
#### `Game.quit ()`
Leave the game and close the chat connection.  Fires `trigger ('quit', App.user)` for all users on success.

***
#### `Game.on ('quit', User quitter)`
Some user (maybe this user) has quit.

***
***

### `new Winning (arguments)`
An object representing a proposed win by a player.

This gives the user a few options.  Someday there should be some kind of voting between players, but for now, everything is final.

This should never be instantiated, except by `Game`.  It's arguments and such are not dictated in this spec.

***
#### `User Winning.winner`
The user who may have won.

***
#### `on ('yeahRight', User overturner')`
Somebody overturned the claim.

***
#### `on ('keepPlaying', User continuer')`
Somebody chose to keep playing for second (or third, or...).

***
#### `on ('goodGame', User ceder')`
Somebody ceded the game.

***
#### `Winning.yeahRight ()`
Overturn the winning claim and revert the last (winning) move.

These functions fire `trigger (name)` on success with no arguments.

***
#### `Winning.keepPlaying ()`
Continue playing without the winner until the next player wins.

***
#### `Winning.goodGame ()`
Leave the game.  Don't close the chat connection, though.