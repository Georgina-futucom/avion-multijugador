# ¡Fuego láser!

## {Introducción @showdialog}

⚡ Tu avión ya vuela. ¡Ahora necesita defenderse!

En este desafío vas a programar un **doble láser** que se dispara con el botón **A**.

## {Evento del botón A}

Los **eventos** son bloques que se ejecutan cuando algo pasa en el juego.

Entrá en ``||controller:Controlador||`` y arrastrá al área de trabajo
``||controller:al presionar botón A||``.

```blocks
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {

})
```

## {El primer láser}

Entrá en ``||sprites:Sprites||`` y arrastrá
``||variables:establecer projectile en||`` ``||sprites:proyectil desde mySprite con vx 50 vy 50||``
adentro del evento.

* Cambiale el nombre a **disparo1**.
* Elegí **avion** como el sprite que dispara.
* Poné **vx 0** y **vy -140**: el número negativo hace que el láser suba ⬆️.
* Dibujá un láser finito y brillante.

```blocks
let avion: Sprite = null
let disparo1: Sprite = null
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    disparo1 = sprites.createProjectileFromSprite(img`
        . 5 .
        . 2 .
        . 2 .
        . 5 .
    `, avion, 0, -140)
})
```

## {Un láser a cada lado}

El láser sale del centro. Vamos a correrlo a la **izquierda** del avión.

Entrá en ``||sprites:Sprites||`` y arrastrá
``||sprites:cambiar disparo1 x por -6||``.

```blocks
let avion: Sprite = null
let disparo1: Sprite = null
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    disparo1 = sprites.createProjectileFromSprite(img`
        . 5 .
        . 2 .
        . 2 .
        . 5 .
    `, avion, 0, -140)
    disparo1.x += -6
})
```

## {Un tipo nuevo: Laser}

Para que el juego sepa que es un láser del jugador 1, le damos un **tipo** propio.

Entrá en ``||sprites:Sprites||`` y arrastrá
``||sprites:establecer disparo1 tipo a||``.

En la lista elegí **Agregar un nuevo tipo...** y escribí **Laser**.

```blocks
namespace SpriteKind {
    export const Laser = SpriteKind.create()
}
let avion: Sprite = null
let disparo1: Sprite = null
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    disparo1 = sprites.createProjectileFromSprite(img`
        . 5 .
        . 2 .
        . 2 .
        . 5 .
    `, avion, 0, -140)
    disparo1.x += -6
    disparo1.setKind(SpriteKind.Laser)
})
```

## {El segundo láser}

Ahora hacé lo mismo para el láser de la **derecha**:

* Clic derecho sobre los tres bloques del primer láser → **Duplicar**.
* Cambiá **disparo1** por una variable nueva **disparo2**.
* Cambiá el **-6** por **6**.

```blocks
namespace SpriteKind {
    export const Laser = SpriteKind.create()
}
let avion: Sprite = null
let disparo1: Sprite = null
let disparo2: Sprite = null
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
```

## {¡A probar! @showdialog}

🎮 Apretá **A** (o la tecla **espacio**) en el simulador.

¿Salen dos láseres hacia arriba, uno de cada lado del avión? ¡Bien hecho, piloto!

```template
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
```
