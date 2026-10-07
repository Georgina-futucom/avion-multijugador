# Contrarreloj

## {Introducción @showdialog}

⏱️ ¡Último desafío!

Vas a agregar una **cuenta regresiva**. Cuando se termine el tiempo, el juego va a comparar los puntajes y anunciar al **ganador** 🏆.

## {Iniciar la cuenta regresiva}

Al final de ``||loops:al iniciar||``, poné
``||info:iniciar cuenta regresiva 15 (s)||`` de ``||info:Info||``.

```blocks
info.startCountdown(15)
```

## {Cuando termina el tiempo}

Entrá en ``||info:Info||`` y arrastrá al área de trabajo
``||info:al terminar la cuenta regresiva||``.

Adentro, poné un ``||logic:si ... si no||`` de ``||logic:Lógica||``.

```blocks
info.onCountdownEnd(function () {
    if (true) {

    } else {

    }
})
```

## {¿Quién tiene más puntos?}

En la condición del ``||logic:si||``, poné una comparación ``||logic:0 > 0||``.

* A la izquierda: ``||mp:puntaje del jugador 1||``
* A la derecha: ``||mp:puntaje del jugador 2||``

Los dos bloques de puntaje están en ``||mp:Multijugador||``.

```blocks
info.onCountdownEnd(function () {
    if (mp.getPlayerState(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.score) > mp.getPlayerState(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.score)) {

    } else {

    }
})
```

## {Anunciar al ganador}

Entrá en ``||mp:Multijugador||`` y arrastrá ``||mp:fin del juego, gana jugador 1||``:

* Adentro del **si**: gana el **jugador 1**.
* Adentro del **si no**: gana el **jugador 2**.

```blocks
info.onCountdownEnd(function () {
    if (mp.getPlayerState(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.score) > mp.getPlayerState(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.score)) {
        mp.gameOverPlayerWin(mp.playerSelector(mp.PlayerNumber.One))
    } else {
        mp.gameOverPlayerWin(mp.playerSelector(mp.PlayerNumber.Two))
    }
})
```

## {¡A festejar!}

Debajo del **si ... si no**, todavía dentro del evento, agregá de ``||game:Juego||``:

* ``||game:usar efecto confeti para ganar||`` 🎉
* ``||game:usar mensaje "GANASTE!!" para ganar||``

```blocks
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

## {¡Juego terminado! @showdialog}

🏆 **¡Felicitaciones, piloto!** Terminaste tu Batalla de Aviones.

🎮 Jugá una partida con un compañero. ¿Quién ganó?

🤔 **Para pensar:** si los dos empatan, ¿quién gana? ¿Cómo lo arreglarías?

🎨 Ideas para personalizarlo: más tiempo, caritas más rápidas, vidas, nuevos sonidos o un fondo animado.

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
```

```package
multiplayer
```
