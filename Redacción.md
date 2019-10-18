# Introducción al netcoding

> Por Andrés Millán Muñoz y Ricardo Ruiz

En este documento, investigaremos el funcionamiento de un servidor de un juego online. Analizaremos el netcode de un juego online PvP (Player versus Player): qué tipo de conectividad se ofrece, cómo se gestiona la latencia y la pérdida de paquetes, interpolación de cliente/servidor...
Para ello, usaremos el juego **Overwatch** como ejemplo.


<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Modelos de red](#modelos-de-red)
    - [Servidores dedicados](#servidores-dedicados)
    - [Peer to Peer](#peer-to-peer)
    - [Client Hosted](#client-hosted)
- [Algunos conceptos básicos](#algunos-conceptos-básicos)
- [Overwatch](#overwatch)
  - [Latencia](#latencia)
    - [Favour the shooter](#favour-the-shooter)
    - [Interpolación del retardo](#interpolación-del-retardo)
    - [Adaptive delay interpolation](#adaptive-delay-interpolation)
    - [Extrapolación](#extrapolación)
    - [No registrations (No reg)](#no-registrations-no-reg)

<!-- /code_chunk_output -->


## Modelos de red

#### Servidores dedicados

<!---
TODO netcode 101 11:38
-->

#### Peer to Peer

<!---
TODO netcode 101 13:31
-->

<!---
TODO Ejemplo Super Smash Bros Ultimate
-->

#### Client Hosted

<!---
TODO netcode 101 12:40
-->


## Algunos conceptos básicos

**Ping**
: Latencia entre el servidor y el cliente. Para medirla, el cliente envía una señal *ICMP echo request*, y el servidor responde a la petición. De esta forma, se calcula el tiempo que tarda un paquete en enviarse del cliente al servidor y volver.

<!---
TODO dibujo ping (netcode 101 2:15)
-->

**Routing**
: Para enviar un paquete, necesitamos trazar una ruta entre el servidor más cercano y nuestro cliente. Nuestro router intentará usar el camino más rápido, siempre que sea posible.

<!---
TODO dibujo routing, ejemplo tracert
-->

**Simulación**
: Tanto los clientes como el servidor mantiene una instancia de los eventos que ocurren en el juego. Por tanto, en cada extremo, el juego se puede ejecutar independientemente de lo que ocurra en los otros puntos. En estas instancias se pueden cargar modelos, texturas, partículas, físicas, y todo lo necesario.

<!--- Esto en verdad inserra un espacio que queda genial-->

**Frecuencia de actualización** (upate rate)
: Es el número de veces en las que un sistema refresca su buffer. Cuando hablamos de la simulación del servidor, se le suele llamar **tickrate**. Cuanto mayor es el tickrate, mayor es el número de veces que nuestro cliente manda y recibe información del servidor, por lo que más precisa es la concordancia cliente-servidor.
A la hora de referirnos a la simulación, esta frecuencia puede estar ligada al **framerate** (imágenes por segundo mostradas en pantalla).

<!---
TODO dibujo 30 vs 60. Vídeo Splatoon sobre tickrate bajo.
-->

Para mantener una simulación correcta, es importante que el servidor termine los cálculos correspondientes dentro de la ventana de procesamiento. Es decir: si nuestro servidor tiene un tickrate de 60Hz, eso nos dice que se actualiza 60 veces en un segundo. Por tanto, cada 16.66 milisegundos. Dispone de algo menos de ese valor para terminar el porcesamiento.
Si esto no ocurre, podríamos notar una respuesta algo lenta, y los datos se acumularían para el siguiente tick.

<p align="center">
    <img width="800"src="./img/tickrate_calculo.png">
</p>

Estos datos acumulados podrían reflejarse en los clientes como una recepción masiva de daño en un instante, o saltos en las posiciones de los jugadores.

**Pérdida de paquetes**
: El envío de información a través de la red no es perfecto. Por el camino se puede producir pérdida de información. Esto es un aspecto absolutamente importante en un juego competitivo. Cada pulsación importa, por lo que perder inputs sería catastrófico. Veremos cómo los juegos implementan técnicas de mitigación.


## Overwatch

<p align="center">
    <img width="200"src="./img/Overwatch_circle_logo.svg">
</p>

### Latencia


Es muy importante mantener una latencia baja a la hora de crear una buena experiencia de usuario. A partir de ciertos milisegundos de retraso, el juego puede volverse poco fluido, y tardar en responder, lo cual afecta al usuario en gran medida.

<!---
TODO Vídeo sobre alto ping
-->

Además, debemos tener en cuenta la distancia entre los clientes. Si un jugador P1 se encuentra en España, y P2 en Estados Unidos, obligatoriamente tendrán un ping mayor que dos personas que se encuentren en España. Para solventar esto, se centralizan los servidores por regiones. Usualmente: Europa, Asia y las Américas.

Aún así, no es posible garantizar que todos los usuarios de una región tengan acceso a una conexión estable. No podemos controlar el hardware ni la estabilidad de la red. Por ello, se utilizan diferentes técnicas para mitigar la diferencia de ping.

Debido a la inmensa relevancia que tiene en un juego competitivo online, nos centraremos en las técnicas que utilizan. Aunque nos centraremos en Overwatch, veremos también otras implementaciones de otros juegos.


Tickrate de 60Hz

#### Favour the shooter

#### Interpolación del retardo

#### Adaptive delay interpolation

#### Extrapolación

#### No registrations (No reg)