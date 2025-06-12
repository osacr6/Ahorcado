# Proyecto Final

| Curso    | Programaci�n Avanzada   |
| :------- | :---------------------- |
| C�digo   | SC-601                  |
| Profesor | Luis Andr�s Rojas Matey |

<br />

## Introducci�n

En el juego conocido como [**Ahorcado**](<https://es.wikipedia.org/wiki/Ahorcado_(juego)>) se trata de adivinar una palabra indicando las letras que la componen, con un n�mero m�ximo de intentos.

<br />

## Objetivo

Aplicar los conocimientos adquiridos para desarrollar una aplicaci�n web con arquitectura **MVC** (_Model-View-Controller_) del juego **Ahorcado**.

<br />

## Especificaciones funcionales

Se deben precargar en una base de datos una cantidad de al menos 100 palabras (esto ser� el "diccionario de palabras"), que cumplan con los siguientes requisitos:

- Cada palabra debe tener una longitud (cantidad de letras) entre 5 y 10.

- Debe haber al menos 3 palabras que empiecen con cada una de las letras.

- Las palabras ser�n en idioma espa�ol.

- Al menos la mitad de las palabras deben tener tilde.

- No se debe distinguir entre may�sculas y min�sculas (es decir, ser�n tratadas como _case insensitive_).

Al ejecutar la aplicaci�n web, se le deben proveer cuatro opciones al usuario:

1. Agregar una palabra nueva al diccionario.

2. Crear un jugador nuevo.

3. Crear una partida nueva.

4. Mostrar el escalaf�n de jugadores.

<br />

### Agregar una palabra nueva al diccionario

Esta parte le permite al usuario agregar nuevas palabras al diccionario (el cual a su vez est� persistente en la base de datos), por lo cual podr�n ser escogidas cuando se crea una partida nueva.

Cualquier palabra ingresada debe cumplir con los requerimientos anteriormente mencionados. As� mismo, no se debe permitir agregar una palabra que ya est� guardada con anterioridad.

Para efectos pr�cticos, las palabras que contengan una letra con tilde, ser� guardada as� (con tilde) en el diccionario. Sin embargo, a la hora de validar si la palabra existe, esta no debe tomar en cuenta la tilde. Por ejemplo: si en el diccionario ya existe la palabra `�rbol`, no debe permitir al usuario agregar una nueva palabra si digita `arbol` o `arb�l`.

<br />

### Crear un jugador nuevo

Esta secci�n permite crear un jugador a partir de dos datos que se deben solicitar al usuario:

- `Identificaci�n`. Es un n�mero entero positivo �nico (por ejemplo, un n�mero de c�dula).

- `Nombre`. Es un _string_ con el nombre del jugador.

Cada jugador tendr� asociado su propio `Marcador`, el cual ser� la suma de los puntos seg�n los resultados de las partidas, asignados de la siguiente forma (m�s adelante se especificar� en qu� consisten los niveles):

- 1 punto positivo (`+1`) por cada partida ganada en nivel `F�cil`.

- 2 puntos positivos (`+2`) por cada partida ganada en nivel `Normal`.

- 3 puntos positivos (`+3`) por cada partida ganada en nivel `Dif�cil`.

- 1 punto negativo (`-1`) por cada partida perdida en nivel `F�cil`.

- 2 puntos negativos (`-2`) por cada partida perdida en nivel `Normal`.

- 3 puntos negativos (`-3`) por cada partida perdida en nivel `Dif�cil`.

Este `Marcador` puede ser negativo en caso que el jugador haya perdido m�s partidas de las que ha ganado.

<br />

### Crear una partida nueva

Cuando el usuario selecciona esta opci�n, se le solicitar� que escoja el jugador (de los creados previamente), esto se har� presentado la concatenaci�n de su `Identificaci�n` y `Nombre`. Por ejemplo: `123456789 - Ana`.

Una vez escogido el jugador, se debe mostrar la opci�n de escoger el nivel. Este nivel est� condicionado �nicamente al tiempo disponible para terminar la partida:

- `F�cil`: 1.5 minutos (1 minuto con 30 segundos).

- `Normal`: 1 minuto.

- `Dif�cil`: 0.5 minutos (30 segundos).

Cuando se haya escogido el nivel, la aplicaci�n debe seleccionar aleatoriamente una palabra del diccionario, deshabilitando o eliminando esta palabra para que no pueda ser seleccionada de nuevo. Esta palabra estar� "escondida" para el jugador, por lo que �nicamente se mostrar�n "subrayados" (_underscores_) para representar la cantidad de letras de dicha palabra. Por ejemplo, suponiendo que la palabra seleccionada del diccionario es la palabra `�rbol` (de 5 letras), entonces se debe mostrar algo similar a esto (5 subrayados o _underscores_):

| \_ \_ \_ \_ \_ |
| :------------: |

En este momento, se debe mostrar el tiempo disponible seg�n el nivel escogido. Este tiempo ser� una cuenta regresiva que se actualizar� cada segundo.

Para que el jugador pueda seleccionar una letra, se deben presentar como botones de forma individual y en may�sculas, por lo que se tendr� que presionar la letra que se desea escoger. Es decir, algo similar a esto, donde cada letra es un bot�n (pero no necesariamente en este orden o divisi�n):

|                                                 |
| :---------------------------------------------: |
| \| A \| - \| B \| - \| C \| - \| D \| - \| E \| |
| \| F \| - \| G \| - \| H \| - \| I \| - \| J \| |
| \| K \| - \| L \| - \| M \| - \| N \| - \| � \| |
| \| O \| - \| P \| - \| Q \| - \| R \| - \| S \| |
| \| T \| - \| U \| - \| V \| - \| W \| - \| X \| |
|                \| Y \| - \| Z \|                |

Cada vez que el jugador presiona una letra, suceder� lo siguiente:

- El bot�n de la letra presionada se deshabilitar� o esconder�, ya que no se podr� volver a presionar.

- Si la letra escogida no es parte de la palabra, entonces el jugador perder� uno de sus intentos (m�s adelante se explica c�mo funcionan estos intentos).

- Si la letra es parte de la palabra, entonces la letra aparecer� en vez del subrayado (_underscore_) en el sitio o sitios donde dicha letra se halle en la palabra. Ejemplo: si la palabra es `�rbol` y el jugador escoge la letra `A`, entonces se debe mostrar as� (notar la tilde):

| � \_ \_ \_ \_ |
| :-----------: |

El jugador tendr� �nicamente 5 intentos fallidos, es decir, si selecciona 5 letras que no son parte de la palabra, ya no podr� seleccionar m�s palabras. La cantidad de intentos disponibles debe estar siempre visible (similar comportamiento que el contador del tiempo regresivo explicado anteriormente).

El juego finaliza cuando se da alguno de los tres escenarios siguientes:

1. Una victoria del jugador cuando logra seleccionar todas las letras correspondientes a la palabra.

2. Una p�rdida cuando el jugador utiliza sus 5 intentos.

3. Una p�rdida cuando el tiempo (el contador regresivo) llegue a cero (0).

En cualquier caso (cuando se finaliza la partida), la p�gina debe indicar el resultado (victoria o p�rdida) y mostrar la palabra completa (que estaba parcial o totalmente escondida).

En todo momento tambi�n se debe tener la opci�n de "nuevo intento", el cual ser� un bot�n que permite crear una nueva partida con el mismo jugador y el mismo nivel de juego previamente seleccionados. Eso s�, si la partida actual no ha finalizado y se presiona este bot�n, entonces se da por terminada (la partida) y se establece como una p�rdida para el jugador.

<br />

### Mostrar escalaf�n de jugadores

Esta es una tabla que muestra la lista de jugadores (`Identificaci�n` y `Nombre`), ordenados por su `Marcador` de forma descendente. Adem�s debe incluir la cantidad de puntos ganados y perdidos. Por ejemplo:

| Identificaci�n | Nombre  | Marcador | \|  | Ganadas | Perdidas |
| -------------: | :------ | :------: | :-: | :-----: | :------: |
|      123456789 | Ana     |    10    | \|  |   15    |    5     |
|      987654321 | Braulio |    4     | \|  |   10    |    6     |
|      304560765 | Carlos  |    -2    | \|  |    6    |    8     |

<br />

## Especificaciones t�cnicas

Esta aplicaci�n debe ser creada utilizando `ASP.NET Core MVC` con el lenguaje `C#` del `.NET Framework 4.8.1`.

Para la persistencia de los datos, se debe utilizar una base de datos **SQL** (como `SQLite`, `MariaDB`, `MySQL`, `PostgreSQL`, `SQL Server`, etc.). As� mismo, se debe utilizar la herramienta `Entity Framework`, ya sea en modo "_Code First_", o bien, "_Database First_".

Cualquier otro aspecto (por ejemplo, estilos, paquetes de `NuGet`, uso de un API propio, etc.), queda a criterio de cada grupo.

<br />

## Entregables

Debido a que este proyecto se debe hacer seg�n los grupos establecidos previamente, el �nico entregable (es decir, lo �nico que se debe subir al **Campus Virtual**) es el v�nculo (_link_) al repositorio en l�nea de **Git** (como por ejemplo, a `GitHub` o `GitLab`). Este v�nculo debe ser subido por <ins>solo uno de los miembros del grupo</ins>. Este repositorio puede ser privado pero <ins>deber� ser p�blico</ins> al momento que les llegue el turno de la exposici�n del proyecto (ya sea en la semana 14 o 15), para que el profesor pueda tener acceso al mismo.

En el repositorio debe estar lo siguiente:

- Todo el c�digo fuente del proyecto, excepto los archivos compilados (es decir, sin las carpetas `bin` ni `obj`).

- Un archivo `README.md` (escrito en **Markdown**) en la ra�z del proyecto, que contenga lo siguiente:

  - Los nombres y carn�s de los integrantes del grupo. <ins>Estos ser�n los �nicos que ser�n tomados en cuenta para la calificaci�n</ins>.

  - El nombre de usuario y correo de **Git** de cada integrante.

  - Un diagrama (puede utilizar, por ejemplo, [**Mermaid**](https://mermaid.js.org/)) de la definici�n de la base de datos con sus tablas, columnas, tipos de campos, llaves, referencias, etc.

  - Todas las referencias de sitios webs con _snippets_ de c�digo utilizados, as� como los _prompts_ (tanto de entrada como salida) de los agentes de AI, que se hayan utilizado.

<br />

## Actividades
- Asignado > Daniel
- Creaci�n de modelo Palabra.
- Agregar una palabra nueva al diccionario.

<br />

- Asignado > Fabian
- Creaci�n de modelo Jugador.
- Crear un jugador nuevo.
- Mostrar el escalaf�n de jugadores.

<br />

- Asignado > Ronal
- Creaci�n de modelo Partida.
- Crear una Partida nueva.

<br />

- Asignado > Eddier
- Esquema de la base de datos usando[**Mermaid**](https://mermaid.js.org/).
- Configuraci�n de Entity Framework usando MySQL.
- Desarrollo del script de precarga de 100 palabras.






