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

```package
multiplayer
```
