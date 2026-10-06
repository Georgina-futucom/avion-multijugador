# Copiloto

## {Introducción @showdialog}

👥 ¡Llegó tu compañero de vuelo!

En este desafío vas a sumar un **segundo avión** para jugar de a dos.
Cada jugador va a tener su propio puntaje. ¿Quién derribará más caritas?

## {Avión del jugador 1}

Primero le avisamos al juego que **avion** es del **jugador 1**.

Entrá en ``||mp:Multijugador||`` y arrastrá
``||mp:establecer sprite del jugador 1 a avion||`` justo debajo de donde creás el avión.

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

* ``||mp:establecer sprite del jugador 2 a avion2||``
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
``||mp:mover jugador 2 con botones||``. Apretá el **+** y poné **vx 110** y **vy 0**.

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
``||mp:cambiar puntaje del jugador 1 por 1||`` de ``||mp:Multijugador||``.

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

En la copia, cambiá **Laser** por **Laser2** y **jugador 1** por **jugador 2**.

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

```package
multiplayer
```
