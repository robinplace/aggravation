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

## Frontend API

### `new Aggravation ()`
Kick the app of for the first time.  Should auto-detect any login.

#### `Aggravation.auth (String provider, Function callback)`
Shows the OAuth popup for `provider` (`'google'`, or `'facebook'`) authentication.

Calls `callback` with whatever account data Firebase gives it (uid, name, profileUrl, etc.).

#### `Aggravation.unauth ()`
Log the user out.

#### `Aggravation.createGame (Function callback)`
Creates a new game on the server and calls

    callback (String code, Game game)

where `code` is a unique game identifier (4 characters, to be shared with other players) and `game` is a new instance of `Game`.

#### `Aggravation.joinGame (String code, Function callback)`
Finds a game on the server with `code` and adds the user to it.

Calls `callback` in the same form as `Aggravation.createGame`.

***
***

### `new Game (arguments)`
Initialize the game board and pieces.

This should never be instantiated, except by `Aggravation.createGame` or `Aggravation.joinGame`.  It's arguments and such are not dictated in this spec.

#### `Game` Events

- `winner` Someone said they won.  `move` would always be triggered immediately before this event.

  Calls `callback (Winning winning)`.
- `move` Another player moved their piece.  Calls `callback (Winning winning)`.

- 

// TODO more

#### `Game.quit (callback)`
Leave the game and close the chat connection.

***
***

### `new Winning (arguments)`
An object representing a proposed win by a player.

This gives the user a few options.  Someday there should be some kind of voting between players, but for now, everything is final.

This should never be instantiated, except by `Game`.  It's arguments and such are not dictated in this spec.

#### `Winning` Events
- `yeahRight`  Somebody overturned the claim.
  Calls `callback (User overturner`.

- `keepPlaying`  Somebody chose to keep playing for second.
  Calls `callback (User continuer)`.

- `goodGame`  Somebody ceded the game.
  Calls `callback (User ceder)` (hahaha cede-er).

#### `User Winning.getWinner ()`
The user who may have won.

#### `Winning.yeahRight (callback)`
Overturn the winning claim and revert the last (winning) move.

These functions call the `callback` on success with no arguments.

#### `Winning.keepPlaying (callback)`
Continue playing without the winner until the next player wins.

#### `Winning.goodGame (callback)`
Leave the game.  Don't close the chat connection, though.