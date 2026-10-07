# Copiloto

## {Introducción @showdialog}

👥 ¡Llegó tu compañero de vuelo!

En este desafío vas a sumar un **segundo avión** para jugar de a dos.
Cada jugador va a tener su propio puntaje. ¿Quién derribará más caritas?

## {Avión del jugador 1}

Primero le avisamos al juego que **avion** es del **jugador 1**.

Entrá en ``||mp:Multijugador||`` y arrastrá el bloque
``||mp:set player 1 sprite to||`` justo debajo de donde creás el avión.

⚠️ Ese bloque viene con un **objeto gris** adentro. Arrastrá ese objeto gris a la caja de herramientas para borrarlo y, en su lugar, poné la variable ``||variables:avion||`` (está en ``||variables:Variables||``).

Tiene que quedar: **set player 1 sprite to avion**.

💬 Los bloques de Multijugador aparecen en inglés: *player* = jugador, *sprite* = personaje.

```blocks
let avion = sprites.create(img`
    . . . 2 2 . . .
    . . . 8 8 . . .
    . . 5 5 5 5 . .
    1 1 5 5 5 5 1 1
    1 1 1 8 8 1 1 1
    . . . 2 2 . . .
    . . 1 1 1 1 . .
    . . 1 . . 1 . .
`, SpriteKind.Player)
mp.setPlayerSprite(mp.playerSelector(mp.PlayerNumber.One), avion)
```

## {Crear el segundo avión}

Al final de ``||loops:al iniciar||``, creá otro sprite de tipo **Jugador** llamado **avion2**.

Dibujalo con **otros colores** para distinguirlo ✈️.

Después agregá:

* ``||mp:set player 1 sprite to||``: cambiá **player 1** por **player 2**, borrá el objeto gris y poné la variable **avion2**
* ``||sprites:establecer posición de avion2 a x 120 y 100||``

```blocks
let avion2 = sprites.create(img`
    . . . 2 2 . . .
    . . . 6 6 . . .
    . . 7 7 7 7 . .
    6 6 7 7 7 7 6 6
    6 6 6 2 2 6 6 6
    . . . 2 2 . . .
    . . 6 6 6 6 . .
    . . 6 . . 6 . .
`, SpriteKind.Player)
mp.setPlayerSprite(mp.playerSelector(mp.PlayerNumber.Two), avion2)
avion2.setPosition(120, 100)
```

## {Mover el segundo avión}

Entrá en ``||mp:Multijugador||`` y arrastrá
``||mp:move player 1 with buttons||``. Cambiá **player 1** por **player 2**, apretá el **+** y poné **vx 110** y **vy 0**.

Después agregá ``||sprites:establecer avion2 permanecer en pantalla activado||``.

```blocks
let avion2 = sprites.create(img`
    . . . 2 2 . . .
    . . . 6 6 . . .
    . . 7 7 7 7 . .
    6 6 7 7 7 7 6 6
    6 6 6 2 2 6 6 6
    . . . 2 2 . . .
    . . 6 6 6 6 . .
    . . 6 . . 6 . .
`, SpriteKind.Player)
mp.setPlayerSprite(mp.playerSelector(mp.PlayerNumber.Two), avion2)
avion2.setPosition(120, 100)
mp.moveWithButtons(mp.playerSelector(mp.PlayerNumber.Two), 110, 0)
avion2.setStayInScreen(true)
```

## {El láser del jugador 2}

Hacé clic derecho sobre el evento ``||controller:al presionar botón A||`` → **Duplicar**.

En la copia:

* Cambiá el evento por ``||controller:al presionar jugador 2 botón A||``.
* Usá **disparo3** y **disparo4** en lugar de 1 y 2.
* Elegí **avion2** como el sprite que dispara.
* Creá un tipo nuevo llamado **Laser2**.

```blocks
namespace SpriteKind {
    export const Laser2 = SpriteKind.create()
}
let avion2: Sprite = null
let disparo3: Sprite = null
let disparo4: Sprite = null
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
```

## {Un puntaje para cada uno}

Buscá el evento ``||sprites:al superponerse Laser con Enemigo||``.

Sacá el bloque ``||info:cambiar puntaje por 1||`` y en su lugar poné
``||mp:change player 1 score by 1||`` de ``||mp:Multijugador||``.

```blocks
namespace SpriteKind {
    export const Laser = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.Laser, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(sprite, effects.disintegrate, 500)
    sprites.destroy(otherSprite, effects.fire, 100)
    music.play(music.melodyPlayable(music.baDing), music.PlaybackMode.UntilDone)
    mp.changePlayerStateBy(mp.playerSelector(mp.PlayerNumber.One), MultiplayerState.score, 1)
})
```

## {Puntos del jugador 2}

Duplicá ese evento completo.

En la copia, cambiá **Laser** por **Laser2** y **player 1** por **player 2**.

```blocks
namespace SpriteKind {
    export const Laser2 = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.Laser2, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(sprite, effects.disintegrate, 500)
    sprites.destroy(otherSprite, effects.fire, 100)
    music.play(music.melodyPlayable(music.baDing), music.PlaybackMode.UntilDone)
    mp.changePlayerStateBy(mp.playerSelector(mp.PlayerNumber.Two), MultiplayerState.score, 1)
})
```

## {¡A probar! @showdialog}

🎮 En el simulador aparecen **dos controles**.

* Jugador 1: flechas y **espacio**.
* Jugador 2: teclas **J** y **L** para moverse, y **U** para disparar.

¿Cada avión suma sus propios puntos? ¡Desafiá a un compañero!

```template
namespace SpriteKind {
    export const Laser = SpriteKind.create()
}
let disparo1: Sprite = null
let disparo2: Sprite = null
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
avion.setPosition(40, 100)
controller.moveSprite(avion, 110, 0)
avion.setStayInScreen(true)
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
    info.changeScoreBy(1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 100)
    music.play(music.melodyPlayable(music.bigCrash), music.PlaybackMode.UntilDone)
})
```

```package
multiplayer
```
