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
