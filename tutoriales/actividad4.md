# ¡Impacto!

## {Introducción @showdialog}

💥 Ahora vas a programar las **superposiciones**: qué pasa cuando dos sprites se tocan.

* Si un **láser** toca una carita → la carita explota y ganás un punto.
* Si una carita choca con tu **avión** → ¡crash!

## {El láser toca una carita}

Entrá en ``||sprites:Sprites||`` y arrastrá al área de trabajo
``||sprites:al superponerse sprite de tipo Jugador con otherSprite de tipo Jugador||``.

Cambiá los tipos: el primero a **Laser** y el segundo a **Enemigo**.

```blocks
namespace SpriteKind {
    export const Laser = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.Laser, SpriteKind.Enemy, function (sprite, otherSprite) {

})
```

## {¡Que exploten!}

Adentro, poné dos veces ``||sprites:destruir sprite con efecto||``:

* **sprite** (el láser) con efecto **desintegrar** y **500** ms.
* **otherSprite** (la carita) con efecto **fuego** 🔥 y **100** ms.

Arrastrá la variable **sprite** u **otherSprite** desde el evento a cada bloque.

```blocks
namespace SpriteKind {
    export const Laser = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.Laser, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(sprite, effects.disintegrate, 500)
    sprites.destroy(otherSprite, effects.fire, 100)
})
```

## {Sonido y puntaje}

Debajo agregá:

* ``||music:reproducir sonido ba ding hasta que termine||`` 🔔
* ``||info:cambiar puntaje por 1||``

```blocks
namespace SpriteKind {
    export const Laser = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.Laser, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(sprite, effects.disintegrate, 500)
    sprites.destroy(otherSprite, effects.fire, 100)
    music.play(music.melodyPlayable(music.baDing), music.PlaybackMode.UntilDone)
    info.changeScoreBy(1)
})
```

## {Puntaje inicial}

Al final de ``||loops:al iniciar||``, poné ``||info:establecer puntaje a 0||``.

Así el puntaje aparece en la pantalla desde el principio 🏅.

```blocks
info.setScore(0)
```

## {Choque con el avión}

Arrastrá otro ``||sprites:al superponerse sprite de tipo Jugador con otherSprite de tipo Enemigo||``.

Adentro:

* ``||sprites:destruir otherSprite con efecto desintegrar||`` **100** ms
* ``||music:reproducir sonido big crash hasta que termine||`` 💥

```blocks
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 100)
    music.play(music.melodyPlayable(music.bigCrash), music.PlaybackMode.UntilDone)
})
```

## {¡A probar! @showdialog}

🎮 Disparale a las caritas. ¿Explotan? ¿Sube el puntaje?

¿Qué pasa si una carita choca con tu avión?

💡 **Desafío extra:** ¿cómo harías para perder una vida cuando una carita te choca? Buscá en ``||info:Info||``.
