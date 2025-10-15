# Evidencias de la unidad 7

## Actividad 1

**- Analiza la técnica: para 3-4 ejemplos que te llamen la atención, describe brevemente cómo la manipulación visual de la palabra refuerza o representa su significado. ¿Qué elementos gráficos o tipográficos utiliza**

>### **Vertigo**
>
><img width="478" height="475" alt="image" src="https://github.com/user-attachments/assets/3a75849d-0588-4d6a-9c8b-3c4ded954295" />
>
>En este ejemplo, Ji Lee utiliza la perspectiva para crear un efecto de vacío, haciendo que las letras parezcan caer y provocando una sensación de vértigo.
>

>### **Protest**
>
><img width="480" height="483" alt="image" src="https://github.com/user-attachments/assets/24ea2d42-35df-471e-89d5-1c0fe2071594" />
>
> En este ejemplo, Ji Lee logra evocar la atmósfera característica de una protesta simplemente alterando la forma de las letras “t”. A través de este sutil cambio, y en conjunto con la idea del movimiento, el sonido y las pancartas, consigue transmitir con fuerza el espíritu colectivo y enérgico de la manifestación.
>

>### **Memory**
>
><img width="475" height="477" alt="image" src="https://github.com/user-attachments/assets/be22bb10-96ed-4575-a226-9f26cffb42a2" />
>
> Este ejemplo me llamó mucho la atención porque, con solo jugar con la opacidad de las letras, logra transmitir un mensaje potente y profundamente narrativo, sin necesidad de imágenes, únicamente a través de las palabras.


**-Genera tus propias ideas (estáticas):** elige 2-3 palabras diferentes. Para cada una, piensa y describe (o haz un boceto muy simple) cómo podrías representarla visualmente siguiendo el concepto “Word as Image”, sin pensar aún en animación o física. ¿Cómo alterarías las letras o la composición para evocar el significado?

Para esta actividad hice una lluvia de ideas con varios conceptos.

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/90379844-a885-4e2e-baba-402f2f966a94" />

Luego de revisarlos y ver la viabilidad de cada uno, me decante por:

>### **Pills**
>
><img width="330" height="142" alt="image" src="https://github.com/user-attachments/assets/d9bd2928-60fd-4e43-9684-388fbd3ff2ea" />
>
> En este ejemplo la idea es que el canva se llene de agua y al llenarse las letras que son "pastillas" se disuelvan.

>### **Bed**
>
><img width="188" height="120" alt="image" src="https://github.com/user-attachments/assets/2dc510fe-37ab-4ec2-9e0d-0a5c9258b954" />
>
> En esta palabra la idea es que haya un switch que controle la luz de la habitación y cuando esté apagado se ponga musica de dormir y se generen Zzz desde la cama, simulando a alguien durmiendo. 


>### **Distance**
>
><img width="583" height="98" alt="image" src="https://github.com/user-attachments/assets/c672ad25-3c55-4fe9-b35d-55b20510b6da" />
>
> Aquí simplemente se irán distanciando las letras.


## Actividad 2.

1. Ejemplos basicos para entender elementos de Matter.js, realicé 2 ejemplos uno para explorar Creación de formas, comportamiento de cuerpos cayendo, constraints, bodies.

Para esto me basé en 2 ejemplos "Beach Balls" y "Slingshot Game".

### Experimento 1

En este experimento creé un motor con gravedad y le inyecte círculos que rebotan. Es una prueba básica para ver cómo se crean formas y como se implementa la gravedad en un mundo usando matter.js.Lo divertido aquí es que las pelotas tienen propiedades mcomo density y restitution, así que el rebote y las colisiones son dinámicas.

``` Experimento 1
// Alias de los módulos de Matter.js
const { Engine, World, Bodies, Mouse, MouseConstraint } = Matter;

let engine;
let world;
let balls = [];     // Array para almacenar las pelotas de playa
let ground;         // Cuerpo estático (el suelo)
let mConstraint;    // Restricción del ratón para interactuar

function setup() {
  createCanvas(600, 400);

  // 1. Configuración del Motor (Engine)
  engine = Engine.create();
  world = engine.world;
  
  // 2. Crear el Suelo Estático
  let groundOptions = {
    isStatic: true,
    friction: 0.8,
    restitution: 0.3
  };
  // Cuerpo estático en la parte inferior del canvas
  ground = Bodies.rectangle(width / 2, height - 10, width, 20, groundOptions);
  World.add(world, ground);

  // 3. Implementar MouseConstraint (Interacción)
  // Objeto ratón de Matter.js que rastrea el mouse en el canvas
  let canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity(); // Ajuste para pantallas de alta resolución

  let mouseOptions = {
    mouse: canvasMouse,
    constraint: { stiffness: 0.2, render: { visible: false } }
  };
  // Crea la restricción que permite "agarrar" los cuerpos
  mConstraint = MouseConstraint.create(engine, mouseOptions);
  World.add(world, mConstraint);
  
  // 4. Crear Pelotas de Playa Iniciales
  for (let i = 0; i < 10; i++) {
    addBeachBall(random(50, width - 50), random(-200, 0));
  }
}

function draw() {
  background(173, 216, 230); // Fondo azul claro

  // 5. Ejecutar la Simulación de Física
  // Esto actualiza posiciones, velocidades y chequea colisiones
  Engine.update(engine);

  // Dibujar cuerpos
  drawGround();
  
  for (let ball of balls) {
    ball.show();
  }
}

// 6. Lógica de Interacción con el Mouse: Crear o Agarrar
function mousePressed() {
  // Solo crea una nueva pelota si el MouseConstraint NO está agarrando un cuerpo.
  // Esto permite que el ratón agarre y arrastre cuerpos existentes.
  if (mConstraint.body == null) {
    if (mouseX > 0 && mouseX < width && mouseY > 0 && mouseY < height) {
      addBeachBall(mouseX, mouseY);
    }
  }
}

// ----------------------------------------------------
// Clases y Funciones de Dibujo
// ----------------------------------------------------

function addBeachBall(x, y) {
  let r = random(20, 35);
  let b = new BeachBall(x, y, r);
  balls.push(b);
  World.add(world, b.body);
}

class BeachBall {
  constructor(x, y, r) {
    this.r = r;
    
    let options = {
      // Alto rebote para el efecto "playa"
      restitution: 0.95, 
      // Baja densidad para el efecto "ligero"
      density: 0.0005, 
      friction: 0.01 
    };
    this.body = Bodies.circle(x, y, r, options);
    this.color = color(random(255), random(255), random(255));
  }

  show() {
    let pos = this.body.position;
    let angle = this.body.angle;

    push();
    translate(pos.x, pos.y);
    rotate(angle);

    // Dibujo de la pelota
    fill(this.color);
    stroke(0);
    strokeWeight(2);
    ellipse(0, 0, this.r * 2);

    pop();
  }
}

function drawGround() {
  let pos = ground.position;
  rectMode(CENTER);
  noStroke();
  // Color marrón/arena para el suelo
  fill(194, 178, 128); 
  rect(pos.x, pos.y, width, 20);
}

```









