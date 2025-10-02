# Evidencias de la unidad 6

# Actividad 01
En esta actividad propondré que te encuentres de nuevo el trabajo de Tyler Hobbs y específicamente que mires su artículo sobre campos de flujo.

### Tyler Hobbs
Me gusta mucho como a traves de simples lineas con multiples variaciones que pueden generar sensaciones o incluso recrear esteticas muy unicas. 

Me llamaron mucho la atención estas dos piezas

<img width="735" height="770" alt="image" src="https://github.com/user-attachments/assets/f7163162-b79c-4fbd-b934-3ef381b5f150" />

Es muy cool la manera en la que pudo recrear algo que me llevo a pensar en la estetica tan unica de Tim Burton y son simplemente lineas. WOW

<img width="1217" height="579" alt="image" src="https://github.com/user-attachments/assets/a6c22246-6018-4cf4-ba84-fd1c12f350e5" />

Esta parte del video me hizzo darme cuenta como los artistas han estado evolucionando y adaptandose a las nuevas tecnologias que tienen mucho que ofrecer, y veo como el arte encontró una vertiente de la mano de las matematicas.

# Actividad 02

En esta actividad quiero que investigues alrededor de estas dos preguntas:

- ¿Qué es una fuerza de dirección (steering force)?

- ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?

- ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?

## RESPUESTA
Leyendo sobre steering forces, entendí que básicamente son fuerzas “inventadas” que usamos para guiar a un agente hacia donde queremos que vaya. No son como la gravedad o la fricción que vienen del entorno, sino que nacen de la “intención” del propio agente. La fórmula es sencilla: velocidad deseada menos la velocidad actual, y eso nos da el empujoncito que el agente necesita para ajustar su rumbo.

La diferencia con lo que ya habíamos visto en simulaciones (tipo gravedad, atracción, etc.) es que esas fuerzas afectan a todos los objetos por igual y de manera externa. En cambio, la steering force es personal: depende del objetivo del agente, de si quiere buscar algo, escapar o seguir un camino. Me gustó eso porque lo hace parecer más “inteligente” y no solo un objeto que reacciona a lo que hay alrededor.

También descubrí que todo esto viene del trabajo de Craig Reynolds en los años 80. Él creó los famosos Boids, que son pájaros virtuales que vuelan en bandada con solo unas cuantas reglas locales (separarse, alinearse y cohesionarse). Lo que me sorprendió es que con reglas tan simples y steering forces, el movimiento colectivo se ve natural, casi biológico. O sea, no hay un líder diciéndoles qué hacer, cada uno sigue sus reglas y de ahí sale el comportamiento del grupo.

# Actividad 3

Evaluando el codigo princpial de Flow Fields.

1. Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.

2. Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.

3. Lista los parámetros clave identificados (resolución, maxspeed, maxforce).

4. Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.

# RESPUESTA

**Respuesta #1**

El campo de flujo está representado como una rejilla 2D (grid) de vectores. En el código original cada celda contiene un vector (normalmente una dirección) que describe “la corriente” en esa porción del espacio. Los índices de la rejilla se calculan a partir de la posición del agente dividiendo por la escala (resolución) del campo.

Los vectores se generan evaluando una función de ruido/seno/ruido Perlin u otra fórmula suave en cada celda para obtener un ángulo y convertirlo en un vector (cos(ang), sin(ang)). Eso produce un campo suave de direcciones en toda la pantalla: cada celda almacena un vector unitario que indica la dirección preferida en esa zona.


**Respuesta #2**

El agente primero revisa en qué celda del campo está parado. Luego, toma el vector de esa celda como si fuera “la recomendación” del entorno. Ese vector se convierte en su dirección deseada. Después calcula su steering force restando la velocidad actual de esa dirección deseada. Eso lo obliga a corregir su rumbo poco a poco, sin cambiar de manera brusca. Al final, limita esa fuerza con maxforce para que no sea demasiado exagerada y mantiene la velocidad bajo maxspeed.


**Respuesta #3**

**- Resolución:** define el tamaño de las celdas de la grilla. Si la resolución es baja, hay pocas celdas y el flujo se ve más simple; si es alta, el campo es más detallado.

**- maxspeed:** la velocidad máxima que puede alcanzar un agente. Evita que salgan disparados de forma irreal.

**- maxforce:** la fuerza máxima de giro o corrección. Controla qué tan brusco o suave es el cambio de dirección cuando el agente se ajusta al vector del flujo.


**Respuesta #4**

<img width="800" height="292" alt="image" src="https://github.com/user-attachments/assets/21def6bf-118a-465f-af9f-3278f49aa723" />

Al multiplicar el ángulo, los vectores cambian más rápido y los agentes se mueven con giros más bruscos, como si la corriente tuviera remolinos.
El comportamiento colectivo se vuelve más caótico

``` 
let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI*5);
this.field[i][j] = p5.Vector.fromAngle(angle);
```

# Apply

## Ideación

### Primera Idea
Mis primeras ideas estaban condicionadas en que no quería hacer nada con partículas. Sin embargo, luego de mucho pensar, no se me ocurrió cómo relacionar los conceptos de flow field y flocking a algo fuera de esto. La mayoría de ejemplos eran así: los del libro eran más o menos, en esencia, partículas, y los de Tyler eran algo parecido a partículas pintando. Tras mi fracaso con esta condición, decidí usarlas.

Mi primera idea era hacer una red neuronal inspirada en un proyecto que realizó Tyler usando "random angle per vector". Por esta red neuronal, creada desde esta misma lógica, pasarían partículas recorriéndolas y ubicándose en distintas posiciones del canvas, las cuales serían las tareas que tengo por hacer. El usuario añadiría las tareas que tiene por hacer y, de esta manera, la distribución siempre sería diferente. También, el usuario iba a poder modificar la "importancia" de la tarea, y esta iba modificando el flujo para generar atracción en cierta tarea u otra. Esto era en base a una canción que se usa para estudiar, donde solo hay un beat. Sin embargo, tras presentarle la idea al profe, me di cuenta de que esto debía surgir como otro instrumento visual, y lo mío, aunque tenía coherencia con lo que genera la canción, no se estaba integrando junto con ella.

<img width="1133" height="633" alt="image" src="https://github.com/user-attachments/assets/5d1d38ec-3ac4-4063-89a2-e52864da24c0" />

### Segunda Idea (Ganadora).
Tras la interención del profe cambié de idea y de canción, entonces trabajé con una canción que se llama Down By the river que hace parte de la banda sonora de Baldurs Gate 3 y está presente en el editor de personajes, esto me conecto con dos cosas, la primera era que relacioné los flow fields a rios, segundo al estar en el edito de personajes pensé en que el usuario podria modificar valores para hacer modificar su propia obra, esto conectaba con lo que habia dicho el profe algunas clases atrás, por lo que empecé el proceso.

<img width="895" height="494" alt="image" src="https://github.com/user-attachments/assets/89e55933-f45a-46f4-b96e-ae03a7dc204d" />

En mis primeros experimentos descrubrí que podia mapear las frecuencias de la canción, sabiendo categoricé 5 tipos de particulas desde los mas graves a los mas agudos y a cada una le asigné un color, de esta manera las caritas abririán la boca cuando estuviera presente el rango asignado.

Ya para este punto tenia el funcionamiento bien planteado y la estetica que queria transmitir, sin emabrgo tenia un problema:

<img width="142" height="401" alt="image" src="https://github.com/user-attachments/assets/9e6a6e7d-e7cc-418a-a2ba-e7225acb4202" />

Las particulas se quedaban atrapadas en los bordes lo que hacia que se viera tosco, entonces lo primero que se me ocurrió fué agrandar el lienzo, pero esto no solucionaria nada ya que la canción dura 2 minutos y en esos 2 minutos volveria a pasar, asi que para hacerlo mas fluido acercandome a las simulaciones de un rio, hice que al atravesas el lienzo saliera al lado contrario de este mismo, asi podia generar bucles.
Tambien añadí un lider que hace que cada vez que doy click uso uno de los conceptos de flocking para agruparlos.

























