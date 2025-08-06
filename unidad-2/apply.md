# Unidad 2


## 🛠 Fase: Apply

## Actividad 08
### Creación de obra generativa
Vas a crear una obra generativa interactivo en tiempo real que utilice los conceptos de motion 101, vectores y algunos algoritmos de la unidad anterior. Vas a probar un algorimo para calcular la aceleración diferente a los que analizaste en esta unidad.

📤 Bitácora (Respuesta).
Describe el concepto de tu obra generativa.
¿Cómo piensas aplicar el marco MOTION 101 y por qué?
¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?
El contenido generado debe ser interactivo. Puedes utilizar mouse, teclado, cámara, micrófono, etc, para variar los parámetros del algoritmo en tiempo real.
El código de la aplicación.
Un enlace al proyecto en el editor de p5.js.
Una captura de pantalla representativa de tu pieza de arte generativo.


``` js
let hormigas = [];
let centro;
let centroActivo = true;
let velocidadGlobal = 1;
let reduciendo = false;
let acelerando = false;
let frenando = false;

function setup() {
  createCanvas(800, 800);
  centro = createVector(width / 2, height / 2);
}

function draw() {
  background(0);

  // El centro sigue el mouse
  centro.set(mouseX, mouseY);

  // Dibujar halo de luz (lámpara)
  if (centroActivo) {
    noStroke();
    for (let r = 120; r > 30; r -= 10) {
      fill(255, 255, 100, map(r, 120, 30, 10, 80)); // Halo progresivo
      ellipse(centro.x, centro.y, r, r);
    }
  }

  // Dibujar el centro (bombillo)
  fill(centroActivo ? color(255, 255, 100) : 100); // Amarillo brillante
  stroke(0);
  strokeWeight(1);
  ellipse(centro.x, centro.y, 30, 30);

  // Crear nuevas hormigas
  if (frameCount % 5 === 0 && velocidadGlobal > 0) {
    hormigas.push(new Hormiga());
  }

  // Modificar velocidad progresivamente
  if (reduciendo && velocidadGlobal > 0) {
    velocidadGlobal -= 0.01;
    velocidadGlobal = max(velocidadGlobal, 0);
  }
  if (acelerando) {
    velocidadGlobal += 0.05;
  }
  if (frenando) {
    velocidadGlobal -= 0.05;
    velocidadGlobal = max(velocidadGlobal, 0);
  }

  // Actualizar y mostrar hormigas
  for (let h of hormigas) {
    h.actualizar();
    h.mostrar();
  }

  // Eliminar hormigas muertas
  hormigas = hormigas.filter(h => h.viva || h.orbitando);
}

function keyPressed() {
  if (keyCode >= 97 && keyCode <= 105) {
    velocidadGlobal = keyCode - 96;
    reduciendo = false;
  }

  if (keyCode === 96) {
    reduciendo = true;
  }

  if (keyCode === UP_ARROW) {
    acelerando = true;
  }

  if (keyCode === DOWN_ARROW) {
    frenando = true;
  }
}

function keyReleased() {
  if (keyCode === UP_ARROW) {
    acelerando = false;
  }

  if (keyCode === DOWN_ARROW) {
    frenando = false;
  }
}

function mousePressed() {
  if (mouseButton === LEFT) {
    centroActivo = !centroActivo;
    return false;
  }
}

class Hormiga {
  constructor() {
    this.pos = this.generarPosicionInicial();
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.conHoja = true;
    this.viva = true;
    this.orbitando = false;

    // Para órbita
    this.anguloOrbita = 0;
    this.radioOrbita = 0;
    this.velocidadAngular = random(0.01, 0.03);
    this.sentido = random([1, -1]);
  }

  generarPosicionInicial() {
    let lado = floor(random(4));
    if (lado === 0) return createVector(0, random(height));
    if (lado === 1) return createVector(width, random(height));
    if (lado === 2) return createVector(random(width), 0);
    return createVector(random(width), height);
  }

  actualizar() {
    if (!this.viva && !this.orbitando) return;

    if (this.orbitando) {
      let velocidad = centroActivo ? this.velocidadAngular : this.velocidadAngular * 0.25;
      this.anguloOrbita += velocidad * this.sentido;
      let x = centro.x + cos(this.anguloOrbita) * this.radioOrbita;
      let y = centro.y + sin(this.anguloOrbita) * this.radioOrbita;
      this.pos.set(x, y);
      return;
    }

    if (centroActivo && this.conHoja) {
      let dir = p5.Vector.sub(centro, this.pos);
      dir.setMag(0.05 * velocidadGlobal);
      this.acc = dir;
    } else {
      this.acc = createVector(0, 0);
    }

    this.vel.add(this.acc);
    this.vel.limit(this.conHoja ? 1.5 * velocidadGlobal : 3 * velocidadGlobal);
    this.pos.add(this.vel);

    if (this.conHoja && centroActivo && p5.Vector.dist(this.pos, centro) < 20) {
      this.conHoja = false;
      this.orbitando = true;
      this.radioOrbita = 20 + random(10, 30);
      this.anguloOrbita = atan2(this.pos.y - centro.y, this.pos.x - centro.x);
      this.vel.set(0, 0);
      this.acc.set(0, 0);
    }

    if (
      this.pos.x < -50 || this.pos.x > width + 50 ||
      this.pos.y < -50 || this.pos.y > height + 50
    ) {
      this.viva = false;
    }
  }

  mostrar() {
    noStroke();
    if (this.orbitando) {
      fill(255, 255, 0); // Amarillo brillante
    } else if (this.conHoja) {
      fill(255, 0, 0); // Rojo
    } else {
      fill(0, 180, 0); // Verde
    }
    ellipse(this.pos.x, this.pos.y, 5, 5);
  }
}
```
