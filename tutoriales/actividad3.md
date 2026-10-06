# Lluvia de caritas

## {Introducción @showdialog}

😈 ¡Se acercan las caritas!

En este desafío vas a hacer que caigan enemigos del cielo **cada cierto tiempo** y en **lugares al azar**.

## {Un evento que se repite}

Entrá en ``||game:Juego||`` y arrastrá al área de trabajo
``||game:en cada intervalo de 500 ms||``.

Cambiá el tiempo a **800** ms. Todo lo que pongas adentro se va a repetir cada 0,8 segundos ⏱️.

```blocks
game.onUpdateInterval(800, function () {

})
```

## {Crear una carita}

Entrá en ``||sprites:Sprites||`` y arrastrá
``||variables:establecer projectile en||`` ``||sprites:proyectil desde el costado con vx 50 vy 50||``
adentro del intervalo.

* Cambiale el nombre a **carita**.
* Poné **vx 0** para que no vaya a los costados.
* Dibujá tu enemigo 😈.

```blocks
let carita: Sprite = null
game.onUpdateInterval(800, function () {
    carita = sprites.createProjectileFromSide(img`
        . 5 5 5 5 .
        5 5 5 5 5 5
        5 f 5 5 f 5
        5 5 5 5 5 5
        5 f f f f 5
        . 5 5 5 5 .
    `, 0, 50)
})
```

## {Velocidad al azar}

Cada carita va a caer a una velocidad distinta 🎲.

Entrá en ``||math:Matemáticas||`` y arrastrá
``||math:elegir al azar de 0 a 10||`` al lugar de **vy**.

Poné de **50** a **90**.

```blocks
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
})
```

## {Es un enemigo}

Entrá en ``||sprites:Sprites||`` y arrastrá
``||sprites:establecer carita tipo a Enemigo||``.

```blocks
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
})
```

## {Un lugar al azar}

Si probás ahora, todas las caritas caen por el mismo lugar. ¡Vamos a cambiarlo!

Entrá en ``||sprites:Sprites||`` y arrastrá
``||sprites:establecer carita x a 0||``.

En lugar del 0, poné ``||math:elegir al azar de 10 a 150||``.

```blocks
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
```

## {¡A probar! @showdialog}

🎮 ¿Caen caritas desde distintos lugares y a distintas velocidades?

Por ahora los láseres las atraviesan... ¡eso lo arreglamos en el próximo desafío!

🧪 **Experimentá:** ¿qué pasa si cambiás 800 por 300?
