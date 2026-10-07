# Tres vidas

## {Introducción @showdialog}

❤️❤️❤️ **¡Cada piloto tiene tres vidas!**

En este último desafío vas a:

* mostrar el **puntaje de cada jugador** desde el principio,
* darle **3 vidas** a cada uno,
* restar una vida cuando una carita choca con un avión,
* y terminar el juego: si un jugador se queda **sin vidas, gana el otro**.

## {Un puntaje para cada jugador}

Buscá en ``||loops:al iniciar||`` el bloque ``||info:fijar puntaje a 0||`` y sacalo (arrastralo a la caja de herramientas).

En su lugar, entrá en ``||mp:Multijugador||`` y arrastrá dos veces el bloque
``||mp:set player 1 score to 0||``:

* el primero para **player 1**
* el segundo cambiando a **player 2**

Así aparece el puntaje de cada jugador en su esquina de la pantalla 🏅.

```blocks
mp.setPlayerState(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.score, 0)
mp.setPlayerState(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.score, 0)
```

## {Tres vidas para cada uno}

Debajo, agregá otros dos bloques ``||mp:set player 1 score to 0||``, pero esta vez:

* hacé clic en la flechita de **score** y elegí **life** (vida),
* poné el número **3**.

Uno para **player 1** y otro para **player 2** ❤️.

```blocks
mp.setPlayerState(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.score, 0)
mp.setPlayerState(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.score, 0)
mp.setPlayerState(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.life, 3)
mp.setPlayerState(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.life, 3)
```

## {Perder una vida}

Buscá el evento ``||sprites:cuando el sprite de tipo Player se superpone con otherSprite del tipo Enemy||``.

Adentro, debajo del sonido, agregá ``||mp:change player 1 score by 1||`` y cambiá:

* **score** por **life**
* el **1** por **-1**
* en lugar de **player 1**, poné el bloque ``||mp:sprite player||`` (está en ``||mp:Multijugador||``) y adentro arrastrá la variable **sprite** del evento.

Así el juego sabe **qué avión** chocó y le resta la vida a ese jugador 💥.

```blocks
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 100)
    music.play(music.melodyPlayable(music.bigCrash), music.PlaybackMode.UntilDone)
    mp.changePlayerStateBy(mp.getPlayerBySprite(sprite), MultiplayerState.life, -1)
})
```

## {Cuando alguien se queda sin vidas}

Entrá en ``||mp:Multijugador||`` y arrastrá al área de trabajo el evento
``||mp:on life zero for player||`` (cuando un jugador llega a cero vidas).

Adentro poné un ``||logic:si ... si no||`` de ``||logic:Lógica||``.

```blocks
mp.onLifeZero(function (player) {
    if (true) {

    } else {

    }
})
```

## {¿Quién perdió?}

En la condición del ``||logic:si||`` poné una comparación ``||logic:0 = 0||``:

* a la izquierda: ``||mp:player number||`` (en Multijugador), con la variable **player** del evento adentro
* a la derecha: el número **1**

Eso pregunta: *¿el que se quedó sin vidas es el jugador 1?*

```blocks
mp.onLifeZero(function (player) {
    if (mp.getPlayerProperty(player, mp.PlayerProperty.Number) == 1) {

    } else {

    }
})
```

## {¡Gana el otro!}

* Adentro del **si** (perdió el jugador 1): ``||mp:game over player 2 wins||``
* Adentro del **si no** (perdió el jugador 2): ``||mp:game over player 1 wins||``

🏆 Si alguien se queda sin vidas, ¡gana su compañero!

```blocks
mp.onLifeZero(function (player) {
    if (mp.getPlayerProperty(player, mp.PlayerProperty.Number) == 1) {
        mp.gameOverPlayerWin(mp.playerSelector(mp.PlayerNumber.Two))
    } else {
        mp.gameOverPlayerWin(mp.playerSelector(mp.PlayerNumber.One))
    }
})
```

## {¡Juego completo! @showdialog}

🎮 Probalo con un compañero:

* **Jugador 1:** flechas para moverse y **espacio** para disparar.
* **Jugador 2:** **J** y **L** para moverse y **U** para disparar.

Ahora el juego termina de dos formas:

* ⏱️ **Se acaba el tiempo:** gana el que tiene más puntos.
* ❤️ **Alguien pierde sus 3 vidas:** gana el otro.

🏆 **¡Felicitaciones, piloto! Terminaste la Batalla de Aviones.**

```template
namespace SpriteKind {
    export const Laser = SpriteKind.create()
    export const Laser2 = SpriteKind.create()
}
let disparo1: Sprite = null
let disparo2: Sprite = null
let disparo3: Sprite = null
let disparo4: Sprite = null
scene.setBackgroundImage(img`
    9 9 9 9 9 9 9 9
    9 9 1 1 9 9 9 9
    9 1 1 1 1 9 9 9
    9 9 9 9 9 9 9 9
`)
let avion = sprites.create(img`
    . . . . . . . 2 2 . . . . . . .
    . . . . . . . 2 2 . . . . . . .
    . . . . . . . 8 8 . . . . . . .
    . . . . . . 5 5 5 5 . . . . . .
    . . . . . 1 5 5 5 5 1 . . . . .
    . 1 1 1 1 1 5 5 5 5 1 1 1 1 1 .
    1 1 1 1 1 1 8 8 8 8 1 1 1 1 1 1
    . . . . . 1 1 2 2 1 1 . . . . .
    . . . 1 1 1 1 2 2 1 1 1 1 . . .
    . . . . . . 1 8 8 1 . . . . . .
`, SpriteKind.Player)
mp.setPlayerSprite(mp.playerSelector(mp.PlayerNumber.One), avion)
avion.setPosition(40, 100)
controller.moveSprite(avion, 110, 0)
avion.setStayInScreen(true)
let avion2 = sprites.create(img`
    . . . . . . . 2 2 . . . . . . .
    . . . . . . . 2 2 . . . . . . .
    . . . . . . . 8 8 . . . . . . .
    . . . . . . 7 7 7 7 . . . . . .
    . . . . . 6 7 7 7 7 6 . . . . .
    . 6 6 6 6 6 7 7 7 7 6 6 6 6 6 .
    6 6 6 6 6 6 8 8 8 8 6 6 6 6 6 6
    . . . . . 6 6 2 2 6 6 . . . . .
    . . . 6 6 6 6 2 2 6 6 6 6 . . .
    . . . . . . 6 8 8 6 . . . . . .
`, SpriteKind.Player)
mp.setPlayerSprite(mp.playerSelector(mp.PlayerNumber.Two), avion2)
avion2.setPosition(120, 100)
mp.moveWithButtons(mp.playerSelector(mp.PlayerNumber.Two), 110, 0)
avion2.setStayInScreen(true)
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    disparo1 = sprites.createProjectileFromSprite(img`
        . 5 .
        . 2 .
        . 2 .
        . 5 .
    `, avion, 0, -140)
    disparo1.x += -6
    disparo1.setKind(SpriteKind.Laser)
    disparo2 = sprites.createProjectileFromSprite(img`
        . 5 .
        . 2 .
        . 2 .
        . 5 .
    `, avion, 0, -140)
    disparo2.x += 6
    disparo2.setKind(SpriteKind.Laser)
})
let carita: Sprite = null
game.onUpdateInterval(800, function () {
    carita = sprites.createProjectileFromSide(img`
        . 5 5 5 5 .
        5 5 5 5 5 5
        5 f 5 5 f 5
        5 5 5 5 5 5
        5 f f f f 5
        . 5 5 5 5 .
    `, 0, randint(50, 90))
    carita.setKind(SpriteKind.Enemy)
    carita.x = randint(10, 150)
})
info.setScore(0)
info.startCountdown(15)
sprites.onOverlap(SpriteKind.Laser, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(sprite, effects.disintegrate, 500)
    sprites.destroy(otherSprite, effects.fire, 100)
    music.play(music.melodyPlayable(music.baDing), music.PlaybackMode.UntilDone)
    mp.changePlayerStateBy(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.score, 1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 100)
    music.play(music.melodyPlayable(music.bigCrash), music.PlaybackMode.UntilDone)
})
controller.player2.onButtonEvent(ControllerButton.A, ControllerButtonEvent.Pressed, function () {
    disparo3 = sprites.createProjectileFromSprite(img`
        . 5 .
        . 2 .
        . 2 .
        . 5 .
    `, avion2, 0, -140)
    disparo3.x += -6
    disparo3.setKind(SpriteKind.Laser2)
    disparo4 = sprites.createProjectileFromSprite(img`
        . 5 .
        . 2 .
        . 2 .
        . 5 .
    `, avion2, 0, -140)
    disparo4.x += 6
    disparo4.setKind(SpriteKind.Laser2)
})
sprites.onOverlap(SpriteKind.Laser2, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(sprite, effects.disintegrate, 500)
    sprites.destroy(otherSprite, effects.fire, 100)
    music.play(music.melodyPlayable(music.baDing), music.PlaybackMode.UntilDone)
    mp.changePlayerStateBy(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.score, 1)
})
info.onCountdownEnd(function () {
    if (mp.getPlayerState(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.score) > mp.getPlayerState(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.score)) {
        mp.gameOverPlayerWin(mp.playerSelector(mp.PlayerNumber.One))
    } else {
        mp.gameOverPlayerWin(mp.playerSelector(mp.PlayerNumber.Two))
    }
    game.setGameOverEffect(true, effects.confetti)
    game.setGameOverMessage(true, "GANASTE!!")
})
```

```package
multiplayer
```
