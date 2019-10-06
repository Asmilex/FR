# Notas

## Netcode analysis por el equipo de Overwatch (2016)

- Tickrate (frecuencia de actualización del servidor): 60hz
- Tiempo de respuesta: primero se ejecuta la simulación (client side), y después se interpola con el servidor (Ejemplo: moverse hacia delante. Se debe ver primero en tu cliente). Si hay algún impedimento el servidor rechazará lo que dicte el cliente y recalculará la posición de nuevo. Entre estos impedimentos se encuentran, por ejemplo: parálisis producida por un enemigo, o problemas técnicos de la red como pérdida de paquetes. De todas formas, en casi cualquier circunstancia el servidor y la simulación concuerdan.
- Proyectiles calculados server-side. Estos proyectiles son dependientes de la velocidad, no del lag del cliente.
- Hacen una demostración de cómo funciona el retardo (Procesamiento del cliente + envío + procesamiento del server + respuesta)
- Fecuencia de actualización de la simulación ligado al framerate del cliente.
- Si el servidor detecta problemas con tu conectividad, intenta simular inputs duplicando los anteriores a la vez que te corrige.
- **Favor the shooter**. Es decir, si usas un arma hitscan, el daño se registra automáticamente y se envía la confirmación al servidor.
    - Los enemigos ven una versión pasada tuya. Interactúan con tu personaje del pasado reciente. Por tanto, si doblas una esquina, es posible que el enemigo te mate. Tu tú del pasado no había cruzado la esquina.
    - Interpolación del retardo: en vez de mostrar el último input de los otros jugadores, se envía el anterior. De esa forma, se interpolan las acciones más recientes con el fin de suavizar la experiencia.
    - Adaptive delay interpolation: si se mantiene una conexión estable con el cliente, se muestra la penúltima acción de los jugadores (1 update rate). Esto minimiza la distancia temporal entre lo que experimenta el jugador y lo que ven el resto de personas.
    - Noreg (de No Registration): aunque en principio se favorece al atacante, si el rival ha ejecutado una técnica que mitiga el daño (por ejemplo: Reaper escapando con Wraith Form de un McCree), el cliente producirá un hitmarker, pero el servidor no aceptará del daño producido. Esto se hace para mitigar los efectos de la implementación de favor the shooter. (https://www.youtube.com/watch?v=38o0QhqeWg8)
    - Servidor tiene buffer de 4 últimos comandos (48ms) (2016).
    - Extrapolación: si el atacante tiene un alto ping, su versión de la simulación estará muy atrasada. Existe un límite para evitar que ocurran fallos debido a esto. Por ello, si el cliente del atacante detecta que su ping es más alto de ciertos milisegundos (250), se intentará predecir si el disparo habría acertado en el servidor. Esto se hace para evitar problemas como que seas disparado detrás de columnas
