# Evidencias de la unidad 7

## Actividad 1

**Analiza la técnica: para 3-4 ejemplos que te llamen la atención, describe brevemente cómo la manipulación visual de la palabra refuerza o representa su significado. ¿Qué elementos gráficos o tipográficos utiliza**

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


**Genera tus propias ideas (estáticas):** elige 2-3 palabras diferentes. Para cada una, piensa y describe (o haz un boceto muy simple) cómo podrías representarla visualmente siguiendo el concepto “Word as Image”, sin pensar aún en animación o física. ¿Cómo alterarías las letras o la composición para evocar el significado?

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


<img width="715" height="458" alt="image" src="https://github.com/user-attachments/assets/9a926f33-4afd-4dc2-91f9-8da51a178901" />


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


### Experimento 2: Crear Constraints

En este experimento volví a crear un motor con gravedad y uní una pesa a un punto fijo con una cuerda. Es una prueba básica para ver cómo se conectan las formas y cómo se controla el movimiento en lugar de solo dejar que caiga, usando el Constraint de Matter.js. Lo divertido aquí es que el péndulo tiene una restricción rígida, así que la pesa siempre debe mantener la misma distancia del punto de anclaje, y por eso la oscilación es tan precisa y el movimiento es totalmente dinámico.

<img width="702" height="431" alt="image" src="https://github.com/user-attachments/assets/ca3b272c-f57a-4b9f-a912-4efa32ad4647" />


``` Experimento 2
// Alias de módulos de Matter.js
const { Engine, World, Bodies, Constraint, Mouse, MouseConstraint } = Matter;

let engine;
let world;
let bob;            // El cuerpo del péndulo (la pesa)
let constraint;     // La "cuerda" (Constraint)
let mConstraint;    // Para agarrar y mover con el mouse
let canvas;         // Referencia al canvas de p5

function setup() {
  canvas = createCanvas(600, 400);

  // 1. Configuración del Motor
  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 0.8; // Gravedad ligeramente ajustada para la oscilación

  // Solución para que el mouse funcione: cuerpo estático invisible
  let ceiling = Bodies.rectangle(width / 2, -50, width, 100, { 
    isStatic: true,
    render: { visible: false } 
  });
  World.add(world, ceiling);
  
  // 2. Crear la Pesa (Bob)
  bob = Bodies.circle(300, 250, 25, { restitution: 0.8, friction: 0.01 });
  World.add(world, bob);

  // 3. CREAR LA RESTRICCIÓN (CONSTRAINT)
  let pivotPoint = { x: width / 2, y: 50 }; // El punto fijo (anclaje)
  
  let constraintOptions = {
    pointA: pivotPoint,         // El punto fijo del que cuelga
    bodyB: bob,                 // El objeto que cuelga (la pesa)
    stiffness: 0.9,             // Rigidez (cuerda que no se estira)
    length: 200                 // Largo de la "cuerda"
  };
  constraint = Constraint.create(constraintOptions);
  World.add(world, constraint);

  // 4. Implementar MouseConstraint (para poder arrastrar)
  let canvasMouse = Mouse.create(canvas.elt); 
  canvasMouse.pixelRatio = pixelDensity();
  
  let mouseOptions = {
    mouse: canvasMouse,
    constraint: { 
      stiffness: 0.2, 
      render: { 
        visible: true,          // Hacemos visible la línea de agarre
        lineWidth: 3,           
        strokeStyle: '#00FF00'  
      }
    }
  };
  mConstraint = MouseConstraint.create(engine, mouseOptions);
  World.add(world, mConstraint);
}

function draw() {
  background(240); 
  Engine.update(engine);

  drawConstraint(constraint);
  drawBob(bob);
  drawPivot(constraint.pointA);
}

// Funciones de Dibujo

function drawConstraint(c) {
  let posA = c.pointA;
  let posB = c.bodyB.position;
    
  stroke(50); 
  strokeWeight(2);
  line(posA.x, posA.y, posB.x, posB.y);
}

function drawBob(body) {
  let pos = body.position;
  let r = body.circleRadius;

  push();
  translate(pos.x, pos.y);
  fill(0, 150, 255); // Pesa azul
  stroke(0);
  ellipse(0, 0, r * 2); 
  pop();
}

function drawPivot(pos) {
  fill(255, 0, 0); // Anclaje rojo
  noStroke();
  ellipse(pos.x, pos.y, 10);
}
``` 

### Conceptos Basicos de Matter.js

Se me hizo facil ver los elementos relacionandolos a creaciónd e videojuegos.

**Engine (Motor):** Es el que hace todos los cálculos de física en cada momento: cuánta gravedad hay, dónde chocan las cosas y a qué velocidad deben moverse. Sin él, nada pasa.

**World (Mundo):** Es el contenedor de todo. Aquí es donde agregas la gravedad, las paredes, los personajes y todos los objetos que van a interactuar. Es tu área de juego.

**Body (Cuerpo):** Son los objetos que ves (círculos, cuadrados, etc.). Pueden ser dinámicos (se mueven, caen) o estáticos (están fijos, como el suelo o una pared).

**Constraint (Vínculo):** Es una conexión que limita el movimiento. Lo usas para atar dos cuerpos entre sí (como en un auto) o para sujetar un cuerpo a un punto fijo (como la cadena de un péndulo o una bola de demolición).

**MouseConstraint (Vínculo con el Mouse):** Es un tipo especial de Constraint que te permite agarrar cualquier Body con el ratón, moverlo y soltarlo, introduciendo una interacción directa con el mundo de física.

### Problemas

El mayor problema fue hacer que el ratón funcionara consistentemente. Aunque el código del MouseConstraint estaba ahí, al principio tuve dos fallos principales:

**Conflicto de Clics:** El ratón no agarraba los objetos porque mi función mousePressed() se ejecutaba primero y creaba un círculo nuevo en lugar de dejarme interactuar con los que ya existían. Tuve que cambiar el código para que solo creara círculos si hacía clic en un espacio vacío.

# Apply 🛠
## Actividad 03 
### Diseño

Para esta etapa estaba deciciendo entre 2 palabras que me gustaron mucho de la fase de investigación, las cuales eran pills y bed, al final sentí que pills iba a ser mucho más divertido de hacer, entonces empecé a hacer bocetos sobre como seria la animación.

><img width="330" height="142" alt="image" src="https://github.com/user-attachments/assets/d9bd2928-60fd-4e43-9684-388fbd3ff2ea" />
> La idea por la cuál me decanté.


A pesar de ya tener una vaga idea de por lo menos los elementos que estarián presente en la obra que serían agua y pastillas, debia explorar como seria la interacción de estos 2.


<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/5eba7874-65b8-402d-af33-f81693dd2ea4" />

## Palabra Elegida
> # Pills

## Concepto:¿Cómo la animación física representa el significado de la palabra?

>En la palabra “Pills”, que significa pastillas en español, las letras “i” y “l” se representan en forma >de cápsulas, evocando la apariencia real de las pastillas. La interacción del usuario simula su proceso >de uso: al disolverse en agua, se activa una animación que reproduce la efervescencia característica de >este tipo de medicamentos. De esta forma, la palabra “Pills” adquiere una mayor fuerza visual y >conceptual, conectando directamente su significado con la obra.

## Aspectos técnicos 

>### **1. Formación de las Letras con Matter.js**
>
>**P y S (Estructuras Estáticas):** Se implementaron como pequeños rectángulos estáticos                   >(Bodies.rectangle, isStatic: true) que funcionan como marcos invisibles para dibujar las letras con p5.js.
>
>**I y LL (Píldoras Dinámicas):**  Son los únicos cuerpos con física activa. Se construyeron con >Bodies.circle (punto de la “I”) y Bodies.rectangle (líneas y “L”).
>
>**Vasos (Contenedores):**  Cada vaso se compone de tres cuerpos estáticos invisibles (dos paredes >inclinadas y una base) que limitan el movimiento de las píldoras y el agua.
>
> ### **2. Propiedades Físicas Principales** 
>
>**Inicio Estático:**  Las píldoras comienzan con isStatic: true y se activan al hacer clic                >(Body.setStatic(false)).
>
>**Rigidez Total:**  inertia: Infinity evita deformaciones o rotaciones indeseadas, conservando la forma >tipográfica.
>
>**Flotabilidad Simulada:**  Al sumergirse bajo el nivel del agua, se aplica una ligera fuerza ascendente  >(Body.applyForce) que imita el efecto de efervescencia.
>
>**Baja Fricción:**  frictionAir reducido para un movimiento más suave y natural.
>
>### **3. Interacción y Restricciones** 
>No se emplearon Constraints explícitos. La interacción se maneja con MouseConstraint, permitiendo al >usuario arrastrar las píldoras incluso después de caer en el vaso.

## Código final.

``` PILLS:CODE
const { Engine, World, Bodies, Body, Mouse, MouseConstraint } = Matter;

let engine;
let world;
let mConstraint;
let pills = [];
let statics = [];
let glassesData = [];

let canvasWidth = 900;
let canvasHeight = 600;
const GLASS_PROPS = {
    w: 80,
    h: 120,
    baseY: 450,
    waterLevelYOffset: 20
};

const LETTER_SPACING = 120;

const PILLS_Y_CENTER = GLASS_PROPS.baseY - GLASS_PROPS.h - 50;

const STATIC_LETTER_Y_OFFSET = 110;

let bubbles = [];

class Burbuja {
    constructor(x, y) {
        this.x = x + random(-4, 4);
        this.y = y;
        this.r = random(1.5, 4);
        this.life = 200;
        this.speedY = random(0.8, 2.5);
    }

    move() {
        this.y -= this.speedY;
        this.x += sin(frameCount * 0.1 * this.r * 0.2) * 0.5;
        this.life -= 2.5;
    }

    show() {
        noStroke();
        fill(255, 255, 255, this.life);
        ellipse(this.x, this.y, this.r * 2);
    }

    isFinished() {
        return this.life < 0;
    }
}

function setup() {
    let canvas = createCanvas(canvasWidth, canvasHeight);
    
    textFont('Courier New');
    textSize(100);

    engine = Engine.create();
    world = engine.world;
    world.gravity.y = 1;

    createPillsAndStatics();

    let canvasMouse = Mouse.create(canvas.elt);
    canvasMouse.pixelRatio = pixelDensity();
    mConstraint = Matter.MouseConstraint.create(engine, {
        mouse: canvasMouse,
        constraint: {
            stiffness: 0.2,
            render: {
                visible: false
            }
        }
    });
    World.add(world, mConstraint);
    
    let floor = Bodies.rectangle(width / 2, height + 10, width, 20, { isStatic: true });
    World.add(world, floor);
}

function mousePressed() {
    for (let p of pills) {
        let pos = p.body.position;
        let r = p.type === 'roundPill' ? p.body.circleRadius : 25;
        let d = dist(mouseX, mouseY, pos.x, pos.y);

        if (d < r * 2 && p.body.isStatic) {
            Body.setStatic(p.body, false);
            p.isPillActive = true;
            
            return;
        }
    }
}

function createGlass(x, yBase) {
    const w = GLASS_PROPS.w;
    const h = GLASS_PROPS.h;
    
    const baseW = w * 0.8;
    let baseBody = Bodies.rectangle(x, yBase + 10, baseW, 20, { isStatic: true, label: 'glass-wall' });
    
    const wallThickness = 15;
    const wallHeight = h;
    const angleLeft = -0.1;
    const wallYCenter = yBase - h / 2 + 10;
    const wallXOffset = (w / 2) * 0.9;
    
    let wallLeft = Bodies.rectangle(x - wallXOffset, wallYCenter, wallThickness, wallHeight, {
        isStatic: true,
        angle: angleLeft,
        label: 'glass-wall'
    });
    
    const angleRight = 0.1;
    let wallRight = Bodies.rectangle(x + wallXOffset, wallYCenter, wallThickness, wallHeight, {
        isStatic: true,
        angle: angleRight,
        label: 'glass-wall'
    });

    World.add(world, [baseBody, wallLeft, wallRight]);
    
    return {
        x: x,
        yBase: yBase,
        w: w,
        h: h,
        waterLevelY: yBase - h + GLASS_PROPS.waterLevelYOffset
    };
}

function createPillsAndStatics() {
    const ELEMENTS = [
        { letter: 'P', type: 'STATIC' },
        { letter: 'I', type: 'DYNAMIC_I' },
        { letter: 'L', type: 'DYNAMIC_L1' },
        { letter: 'L', type: 'DYNAMIC_L2' },
        { letter: 'S', type: 'STATIC' }
    ];
    
    const numElements = ELEMENTS.length;
    const gap = LETTER_SPACING;
    const totalWidth = (numElements - 1) * gap;
    const startX = (width - totalWidth) / 2;
    
    const letterSize = 100;
    
    const L_PILL_HEIGHT = 100;
    const I_LINE_HEIGHT = 70;
    const I_DOT_RADIUS = 15;

    const pillOptions = {
        isStatic: true,
        frictionAir: 0.02,
        density: 0.001,
        restitution: 0.1,
        inertia: Infinity,
        angularDamping: 0
    };

    for (let i = 0; i < numElements; i++) {
        let currentX = startX + i * gap;
        let element = ELEMENTS[i];
        
        let glassData = null;

        if (element.type.startsWith('DYNAMIC')) {
            glassData = createGlass(currentX, GLASS_PROPS.baseY);
            glassesData.push(glassData);
        }
        
        if (element.type === 'STATIC') {
            let staticBody = Bodies.rectangle(currentX, PILLS_Y_CENTER + 5 + STATIC_LETTER_Y_OFFSET, 1, 1, {
                isStatic: true,
                isSensor: true,
                label: element.letter
            });
            statics.push({ body: staticBody, letter: element.letter, size: letterSize, glassRef: glassData });
            World.add(world, staticBody);
            
        } else if (element.type === 'DYNAMIC_I') {
            let iDotBody = Bodies.circle(currentX, PILLS_Y_CENTER - 45, I_DOT_RADIUS, pillOptions);
            let iLineBody = Bodies.rectangle(currentX, PILLS_Y_CENTER + 20, 15, I_LINE_HEIGHT, pillOptions);
            
            pills.push({ body: iDotBody, letter: 'i-dot', type: 'roundPill', color: color(255, 60, 60), isEffervescent: false, isPillActive: false, glassRef: glassData });
            pills.push({ body: iLineBody, letter: 'i-line', type: 'capsule', color1: color(255, 192, 203), color2: color(255, 192, 203), isEffervescent: false, isPillActive: false, glassRef: glassData });
            
            World.add(world, [iDotBody, iLineBody]);
            
        } else if (element.type === 'DYNAMIC_L1' || element.type === 'DYNAMIC_L2') {
            let angle = element.type === 'DYNAMIC_L1' ? -0.1 : 0.1;
            let lBody = Bodies.rectangle(currentX, PILLS_Y_CENTER + 5, 15, L_PILL_HEIGHT, {...pillOptions, angle: angle});
            
            let color1 = element.type === 'DYNAMIC_L1' ? color(255, 0, 0) : color(50, 50, 255);
            
            pills.push({ body: lBody, letter: 'L', type: 'capsule', color1: color1, color2: color(255), isEffervescent: false, isPillActive: false, glassRef: glassData });
            
            World.add(world, lBody);
        }
    }
}

function draw() {
    background(220, 220, 250);
    Engine.update(engine);

    drawWater();
    drawGlass();


    for (let s of statics) {
        drawLetterBody(s, color(50));
    }
    
    for (let p of pills) {
        const g = p.glassRef;
        
        if (g) {
            if (p.isPillActive && p.body.position.y >= g.waterLevelY && !p.isEffervescent) {
                p.isEffervescent = true;
                Body.applyForce(p.body, p.body.position, {x: 0, y: -0.01});
            }
        }
        
        drawPillBody(p);
    }

    for (let p of pills) {
        let pos = p.body.position;
        const g = p.glassRef;
        
        if (g) {
            if (p.isEffervescent && pos.y >= g.waterLevelY - 50 && pos.x > g.x - g.w/2 && pos.x < g.x + g.w/2) {
                if ((p.letter === 'i-line' || p.letter === 'L' || p.letter === 'i-dot') && frameCount % 3 === 0) {
                    bubbles.push(new Burbuja(pos.x, pos.y + 10));
                }
                Body.applyForce(p.body, p.body.position, {x: random(-0.0001, 0.0001), y: -0.00001});
            }
        }
    }

    for (let i = bubbles.length - 1; i >= 0; i--) {
        bubbles[i].move();
        bubbles[i].show();
        if (bubbles[i].isFinished()) {
            bubbles.splice(i, 1);
        }
    }
}

function drawGlass() {
    for (let g of glassesData) {
        const x = g.x;
        const w = g.w;
        const h = g.h;
        const yBase = g.yBase;
        const topY = yBase - h;

        push();
        noFill();
        stroke(100, 100, 150);
        strokeWeight(4);
        
        beginShape();
        vertex(x - w / 2, topY);
        vertex(x + w / 2, topY);
        vertex(x + w / 2 - 10, yBase);
        vertex(x - w / 2 + 10, yBase);
        endShape(CLOSE);
        
        line(x - w / 2 + 10, yBase, x + w / 2 - 10, yBase);
        
        pop();
    }
}

function drawWater() {
    for (let g of glassesData) {
        const x = g.x;
        const w = g.w;
        const yBase = g.yBase;
        const waterLevelY = g.waterLevelY;
        
        push();
        noStroke();
        fill(50, 150, 255, 180);
        
        beginShape();
        vertex(x - w / 2, waterLevelY);
        vertex(x + w / 2, waterLevelY);
        vertex(x + w / 2 - 10, yBase);
        vertex(x - w / 2 + 10, yBase);
        endShape(CLOSE);
        
        pop();
        
        push();
        stroke(255);
        strokeWeight(2);
        noFill();
        beginShape();
        curveVertex(x - w / 2, waterLevelY);
        curveVertex(x - w / 2, waterLevelY);
        curveVertex(x - w / 4, waterLevelY + sin(frameCount * 0.08) * 2);
        curveVertex(x, waterLevelY + sin(frameCount * 0.08 + PI/2) * 2);
        curveVertex(x + w / 4, waterLevelY + sin(frameCount * 0.08 + PI) * 2);
        curveVertex(x + w / 2, waterLevelY);
        curveVertex(x + w / 2, waterLevelY);
        endShape();
        pop();
    }
}

function drawPillBody(pillObj) {
    let pos = pillObj.body.position;
    let angle = pillObj.body.angle;

    push();
    translate(pos.x, pos.y);
    rotate(angle);
    
    noStroke();
    rectMode(CENTER);

    let baseAlpha = pillObj.body.isStatic ? 150 : 255;
    
    let alpha = baseAlpha;
    if (pillObj.isEffervescent) {
        alpha = map(sin(frameCount * 0.1), -1, 1, 200, 255);
    }

    if (pillObj.type === 'roundPill') {
        let r = pillObj.body.circleRadius;
        fill(red(pillObj.color), green(pillObj.color), blue(pillObj.color), alpha);
        ellipse(0, 0, r * 2);
        stroke(0, 0, 0, 80);
        strokeWeight(1.5);
        line(-r * 0.6, 0, r * 0.6, 0);
        
    } else if (pillObj.type === 'capsule') {
        let w = 15;
        let h = pillObj.body.bounds.max.y - pillObj.body.bounds.min.y;
        let radius = w / 2;
        let half_h = h / 2;
        
        fill(red(pillObj.color2), green(pillObj.color2), blue(pillObj.color2), alpha);
        rect(0, 0, w, h, radius);

        fill(red(pillObj.color1), green(pillObj.color1), blue(pillObj.color1), alpha);
        rect(0, -half_h / 2, w, half_h + 1, radius, radius, 0, 0);
        
        stroke(0, 0, 0, 80);
        strokeWeight(1.5);
        line(-w / 2, 0, w / 2, 0);
    }
    
    pop();
}

function drawLetterBody(letterObj, colorVal) {
    let pos = letterObj.body.position;
    let angle = letterObj.body.angle;
    
    push();
    translate(pos.x, pos.y);
    rotate(angle);
    
    textSize(letterObj.size);
    textAlign(CENTER, CENTER);
    fill(colorVal);
    noStroke();
    text(letterObj.letter, 0, letterObj.size * 0.05);
    pop();
}
```
## Captura de pantalla y video

### Captura de pantalla.

<img width="1121" height="751" alt="image" src="https://github.com/user-attachments/assets/133adba9-4c72-4069-b160-3c7f2f7c318e" />

### GIF

![Pills](https://github.com/user-attachments/assets/989e32f9-f1c9-4019-91c1-07b08ac5b12f)

# Autoevaluación
### Nota: 4.8

> ## Investigación, Análisis e ideación.
> Durante la investigación, prioricé comprender a fondo los conceptos presentes en los ejemplos propuestos en la unidad, tanto en el uso de técnicas visuales como en la aplicación de la libreria Matter.js dentro de p5.js
>
> A partir de esto, desarrollé mi propio proceso de ideación, acompañado de bocetos que documentan la evolución creativa de la obra, Estos, junto con los ejemplos técnicos estudiados, me permitieron enfocar y orientar de manera más clara la fase de Apply.
> 

> ## Matter.js: Fundamentos técnicos.
> Durante esta actividad me aseguré de estudiarla y re estudiarla para poder aterrizar mis ideas conceptuales y hasta que punto podia llevar esta herramienta, lo que me llevó a tener una idea muy clara con lo que estaba trabajando.
>
> En la bitácora documento todo el proceso de **Experimentación práctica** , **Evidencia del funcionamiento** , **Comprensión de los elementos y conceptos**
>
> ## Fase de Apply
>
> En esta fase la cuál es la culminación de todo el proyecto se evidencia todo el proceso de estudio de esta libreria y de mi proceso de ideación.
>
> Cumplo con:
> - Palabra elegida y conceptualización
> - Implementaciones técnicas de Matter.js
> - Entregables (Capturas, GIF, Código completo y funcional)

## Conclusión

Se realizaron todas las actividades estipuladas, evidenciando a través de la bitácora y del proyecto una comprensión tanto teórica como técnica de Matter.js.

Si bien el proyecto final no incluye una gran cantidad de elementos de la librería, considero que los implementados son suficientes para demostrar un estudio profundo y completo de su funcionamiento. No obstante, estimo que mi calificación debería ser 4.8 en lugar de 5.0, ya que reconozco que pude haber incorporado más componentes de Matter.js dentro de la obra.




