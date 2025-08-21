# Unidad 3


## 🛠 Fase: Apply

# Problema de los N-Cuerpos.
Diseña e implementa tu obra generativa interactiva en tiempo real.

Debe ser interactiva.

Debes usar al menos dos algoritmos diferentes de la unidad 1, además de random.

**Explica cómo modelaste el problema de los n-cuerpos en tu obra.**
>
>**R/** Modelé el problema de los N-cuerpos a partir de la idea fundamental que plantea la física: cada cuerpo ejerce una fuerza de atracción sobre los demás, proporcional a su masa e inversamente proporcional al cuadrado de la distancia que los separa. En mi simulación, cada letra, número o carácter de la palabra que el usuario introduce se convierte en un “cuerpo” dentro del espacio.

Copia el enlace a tu simulación en p5.js.
>
>https://editor.p5js.org/Juan1022/full/9OSX3vAVa
>
Copia el código.

``` java

let bodies = [];
let input, button;
let palabra = "hOLA3pR";

function setup() {
  createCanvas(800, 600);

  // Input de palabra
  input = createInput(palabra);
  input.position(20, height + 20);

  // Botón de simular
  button = createButton('Simular');
  button.position(input.x + input.width + 10, height + 20);
  button.mousePressed(crearCuerpos);

  // Enter 
  input.elt.addEventListener("keydown", (e) => {
    if (e.key === "Enter") crearCuerpos();
  });

  crearCuerpos();
}

function draw() {
  background(20);


  fill(255);
  textAlign(CENTER, CENTER);
  textSize(32);
  text(palabra, width / 2, 40);

  // Calcular fuerzas entre cuerpos
  for (let i = 0; i < bodies.length; i++) {
    for (let j = 0; j < bodies.length; j++) {
      if (i != j) {
        let force = bodies[j].attract(bodies[i]);
        bodies[i].applyForce(force);
      }
    }
  }

  
  for (let b of bodies) {
    b.update();
    b.show();
  }
}

function crearCuerpos() {
  palabra = input.value().trim();
  if (palabra.length === 0) palabra = "NBody";

  bodies = [];
  for (let i = 0; i < palabra.length; i++) {
    let x = random(width);
    let y = random(height);
    let m = random(15, 25);
    bodies.push(new Particle(x, y, m, palabra[i]));
  }
}

// CParticula
class Particle {
  constructor(x, y, m, letra) {
    this.pos = createVector(x, y);
    this.vel = this.levyFlight(); 
    this.acc = createVector(0, 0);
    this.mass = m;
    this.letra = letra;
  }

  //  Levy Flight
  levyFlight() {
    let angle = random(TWO_PI);
    let step = this.levyStep(1, 30); // rango de pasos
    return p5.Vector.fromAngle(angle).mult(step);
  }

  // Distribución de Levy (aprox usando Cauchy)
  levyStep(minStep, maxStep) {
    let beta = 1.5; // parámetro de la distribución
    let u = randomGaussian(0, 1);
    let v = randomGaussian(0, 1);
    let step = u / pow(abs(v), 1 / beta);
    step = constrain(abs(step), minStep, maxStep);
    return step;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  attract(other) {
    let G = 1;
    let force = p5.Vector.sub(this.pos, other.pos);
    let distance = constrain(force.mag(), 10, 50);
    let strength = (G * this.mass * other.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);

    // Rebote en bordes
    if (this.pos.x < 0 || this.pos.x > width) this.vel.x *= -1;
    if (this.pos.y < 0 || this.pos.y > height) this.vel.y *= -1;
  }

  show() {
    noStroke();
    let c = this.letra;
    let vocales = "aeiouAEIOU";
    let esVocal = vocales.includes(c);
    let esNumero = !isNaN(c);

    push();
    translate(this.pos.x, this.pos.y);

    if (esNumero) {
      fill(255);
      ellipse(0, 0, this.mass * 2);
      fill(0);
      ellipse(0, 0, this.mass);
    } else if (esVocal && c === c.toUpperCase()) {
      fill(0, 150, 150);
      triangle(-this.mass, this.mass, this.mass, this.mass, 0, -this.mass * 0.5);
    } else if (esVocal && c === c.toLowerCase()) {
      fill(0, 200, 255);
      arc(0, 0, this.mass * 2, this.mass * 2, 0, PI, PIE);
    } else if (!esVocal && c === c.toUpperCase()) {
      fill(255, 150, 0);
      ellipse(0, 0, this.mass * 2);
    } else if (!esVocal && c === c.toLowerCase()) {
      fill(255, 255, 100);
      arc(0, 0, this.mass * 2, this.mass * 2, 0, PI);
    } else {
      fill(150);
      rectMode(CENTER);
      rect(0, 0, this.mass * 2, this.mass * 2);
    }

    pop();
  }
}

```

Captura una imagen representativa de tu ejemplo.

<img width="916" height="794" alt="image" src="https://github.com/user-attachments/assets/0393e441-3f56-436f-b3a4-8a36bd57fedc" />
