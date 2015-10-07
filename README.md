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
Google

// TODO

## Frontend API

### `new Aggravation ()`
Kick the app of for the first time.  Should auto-detect any login.

#### `Aggravation.auth (String provider, Function callback)`
Shows the OAuth popup for `provider` (`'google'`, or `'facebook'`) authentication.

#### `Aggravation.unauth ()`
Log the user out.

#### `Aggravation.createGame (Function callback)`
Creates a new game on the server and calls

    callback (String code, Game game)

where `code` is a unique game identifier (4 characters, to be shared with other players) and `game` is a new instance of `Game`.

#### `Aggravation.joinGame (String code, Function callback)`
Finds a game on the server with `code` and adds the user to it.

Calls `callback (String code, Game game)` in the same form as `Aggravation.createGame`.

***

### `new Game ()`
Initialize the game board and pieces.

This should never be called, except by `Aggravation.createGame` or `Aggravation.joinGame`

#### `Game.`
