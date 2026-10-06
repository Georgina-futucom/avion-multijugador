# Despegue

## {Bienvenida @showdialog}

✈️ **¡Bienvenido a la Batalla de Aviones!**

En este mapa vas a programar, paso a paso, un juego para **dos jugadores**.

En este primer desafío vas a elegir el cielo, dibujar tu avión y hacerlo volar.

## {El fondo del cielo}

Entrá en ``||scene:Escena||`` y arrastrá el bloque
``||scene:establecer imagen de fondo a||`` adentro de ``||loops:al iniciar||``.

Hacé clic en el cuadrado gris y dibujá un cielo 🌤️ o elegí uno de la **Galería**.

```blocks
scene.setBackgroundImage(img`
    9 9 9 9 9 9 9 9
    9 9 1 1 9 9 9 9
    9 1 1 1 1 9 9 9
    9 9 9 9 9 9 9 9
`)
```

## {Crear el avión}

Entrá en ``||sprites:Sprites||`` y arrastrá
``||variables:establecer mySprite en||`` ``||sprites:sprite de tipo Jugador||``
debajo del fondo.

Hacé clic en la flechita de **mySprite** → **Cambiar nombre de la variable** → escribí **avion**.

Después hacé clic en el cuadrado gris y dibujá tu avión ✈️ **mirando hacia arriba**.

```blocks
scene.setBackgroundImage(img`
    9 9 9 9 9 9 9 9
    9 9 1 1 9 9 9 9
    9 1 1 1 1 9 9 9
    9 9 9 9 9 9 9 9
`)
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
```

## {Ubicar el avión}

El avión tiene que arrancar **abajo** de la pantalla.

Entrá en ``||sprites:Sprites||`` y arrastrá
``||sprites:establecer posición de avion a x 40 y 100||``.

🧭 La pantalla mide 160 de ancho y 120 de alto. El punto (0, 0) es la esquina de arriba a la izquierda.

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
avion.setPosition(40, 100)
```

## {Moverse a los costados}

Entrá en ``||controller:Controlador||`` y arrastrá
``||controller:mover avion con botones||``.

Apretá el **+** del bloque y poné **vx 110** y **vy 0**.
Así el avión solo se mueve a la **izquierda** y a la **derecha** ⬅️➡️.

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
avion.setPosition(40, 100)
controller.moveSprite(avion, 110, 0)
```

## {No salirse de la pantalla}

Entrá en ``||sprites:Sprites||`` y buscá
``||sprites:establecer avion permanecer en pantalla activado||``.

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
avion.setPosition(40, 100)
controller.moveSprite(avion, 110, 0)
avion.setStayInScreen(true)
```

## {¡A probar! @showdialog}

🎮 Mové el avión con las flechas en el simulador.

¿Se mueve solo a los costados? ¿Se queda dentro de la pantalla?

¡Despegue exitoso! Apretá **Listo** para desbloquear el siguiente desafío.
