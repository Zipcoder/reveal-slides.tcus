# Generics: Defining generic classes



-
-
## What we'll cover
<p class="fragment fade-up">Non-Generic vs Generic Structure</p>
<p class="fragment fade-up">Abstracting a Generic Structure </p>

-
-
## Defining generic classes
* You know to define a generic class when classes share signatures but have variable Type


-
## Defining a non-Generic Structure
```java
public class BlackJackGameEngine {
  private BlackJackGame game;
  private List<BlackJackPlayer> players;
  public BlackJackGameEngine(BlackJackGame game, List<BlackJackPlayer> players) {
    this.game = game;
    this.players = players;
  }

  public void start() {
    game.addPlayers(players);

    while(!game.isOver()) {
      game.evaluateTurns(players);
    }
  }
}
```




-
## Defining a non-Generic Structure
```java
public class GoFishGameEngine {
  private GoFishGame game;
  private List<GoFishPlayer> players;
  public GoFishGameEngine(GoFishGame game, List<GoFishPlayer> players) {
    this.game = game;
    this.players = players;
  }

  public void start() {
    game.addPlayers(players);

    while(!game.isOver()) {
      game.evaluateTurns(players);
    }
  }
}
```









-
-
### Abstracting a Generic Structure
* To abstract each of the previously mentioned classes we must first ensure each variable type conforms to a common base-type, or interface
* Variable types in this example include:
  1. GoFishPlayer
  2. GoFishGame
  3. GoFishGameEngine
  4. BlackJackPlayer
  4. BlackJackGame
  5. BlackJackGameEngine


-
#### Abstracting Generic Structures<br>Player Interface
* Ensuring `GoFishPlayer` and `BlackJackPlayer` conform to a common `Player` interface

```java
public interface Player {
  void play();
}
```
```java
public class GoFishPlayer implements Player {
  // definition omitted for brevity
}
```
```java
public class BlackJackPlayer implements Player {
  // definition omitted for brevity
}
```



-
#### Abstracting Generic structures<br>Game Interface
* Ensuring `GoFishGame` and `BlackJackGame` conform to a common `Game` interface

```java
public interface Game {
  void evaluateTurn(Player player);
  void addPlayer(Player player);

  Boolean isOver();
}
```
```java
public class GoFishGame implements Game {
  // definition omitted for brevity
}
```
```java
public class BlackJackGame implements Game {
  // definition omitted for brevity
}
```


-
### Noticing Design Flaw
* Notice that the `Game` interface enforces mediation of a `Player` but ignores specific player-type.
* This design flaw allows for strange orientation

```java
public void demo() {
  Player player = new BlackJackPlayer();
  Game game = new GoFishGame();

  // no exception thrown; should be impossible behavior
  game.addPlayer(player);
}
```





-
#### Parameterizing Game Interface
* A specific player-type can be enforced by _parameterizing_ `Game` interface
```java
public interface Game<PlayerType extends Player> {
    void evaluateTurn(PlayerType player);
    void addPlayer(PlayerType player);
    void addPlayers(Iterable<? extends PlayerType> player);
    Boolean isOver();
}
```

-
#### Parameterizing GoFish and BlackJack
* It follows that the implementing classes should be parameterized respectively

```java

public class GoFishGame implements Game<GoFishPlayer> {
  // definition omitted for brevity
}
```
```java
public class BlackJackGame implements Game<BlackJackPlayer> {
  // definition omitted for brevity
}
```


-
### Noticing Design Advantage<br>Part 1
* Notice that the `Game` interface refuses mediation of invalid `Player`-types

```java
public void demo() {
  Player player = new BlackJackPlayer();
  Game<GoFishPlayer> game = new GoFishGame();
  game.addPlayer(player); // compile-time exception
}
```







-
### Noticing Design Advantage<br>Part 2
* Notice that the `Game` interface refuses mediation of invalid `Player`-types

```java
public void demo() {
  Player player = new GoFishPlayer();
  Game<BlackJackPlayer> game = new BlackJackGame();
  game.addPlayer(player); // compile-time exception
}
```







-
### Noticing Design Advantage<br>Part 3
* Notice that the `Game` interface enforces valid `Player`-types

```java
public void demo() {
    GoFishPlayer player = new GoFishPlayer();
    Game<GoFishPlayer> game = new GoFishGame();
    game.addPlayer(player); // valid
}
```







-
### Abstracting Generic structures<br>Game Engine Interface
* Creating `GameEngineInterface` for `GoFishGameEngine` and `BlackJackGameEngine` to conform to.

```java
public interface GameEngineInterface {
  void start();
  Game getGame();
  Iterable<Player> getPlayers();
}
```


-
### Abstracting Generic Structures<br>Game Engine Interface
* Defining _abstract_ `GameEngine`

```java
public abstract class GameEngine implements GameEngineInterface {
  private Game game;
  private List<Player> players;
  public GameEngine(Game game, List<Player> players) {
    this.game = game;
    this.players = players;
  }

  public void start() {
    game.addPlayers(players);

    while(!game.isOver()) {
      game.evaluateTurns(players);
    }
  }
}
```




-
### Abstracting Generic Structures<br>Parameterizing Game Engine Interface
* Parameterizing _abstract_ `GameEngineInterface`


```java
public interface GameEngineInterface<
        PlayerType extends Player,
        GameType extends Game<PlayerType>> {

    void start();
    GameType getGame();
    Iterable<PlayerType> getPlayers();
}
```


-
### Abstracting Generic Structures<br>Parameterizing Game Engine Class
* It follows that implementing classes should be parameterized respectively

```java
public abstract class GameEngine<
        PlayerType extends Player,
        GameType extends Game<PlayerType>>
        implements GameEngineInterface<PlayerType, GameType> {

    private GameType game;
    private List<PlayerType> players;
    public GameEngine(GameType game, List<PlayerType> players) {
        this.game = game;
        this.players = players;
    }

    public void start() {
      // definition omitted
    }
}
```




-
### Abstracting Generic Structures
* Parameterizing `GoFishGameEngine`

```java
public class GoFishGameEngine extends GameEngine<GoFishPlayer, GoFishGame> {
  public GoFishGameEngine(GoFishGame game, List<GoFishPlayer> players){
    super(game, players);
  }
}
```




-
### Former Definition
* `GoFishGameEngine`

```java
public class GoFishGameEngine {
  private GoFishGame game;
  private List<GoFishPlayer> players;
  public GoFishGameEngine(GoFishGame game, List<GoFishPlayer> players) {
    this.game = game;
    this.players = players;
  }

  public void start() {
    game.addPlayers(players);

    while(!game.isOver()) {
      game.evaluateTurns(players);
    }
  }
}
```


-
### Abstracting Generic Structures
* Parameterizing `BlackJackGameEngine`

```java
public class BlackJackGameEngine extends GameEngine<BlackJackPlayer, BlackJackGame> {
  public BlackJackGameEngine(BlackJackGame game, List<BlackJackPlayer> players){
    super(game, players);
  }
}
```





-
### Former Definition
* `BlackJackGameEngine`

```java
public class BlackJackGameEngine {
  private BlackJackGame game;
  private List<BlackJackPlayer> players;
  public BlackJackGameEngine(BlackJackGame game, List<BlackJackPlayer> players) {
    this.game = game;
    this.players = players;
  }

  public void start() {
    game.addPlayers(players);

    while(!game.isOver()) {
      game.evaluateTurns(players);
    }
  }
}
```
