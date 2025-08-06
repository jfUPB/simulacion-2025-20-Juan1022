# Unidad 2


## 🛠 Fase: Apply

## Actividad 08
### Creación de obra generativa
Vas a crear una obra generativa interactivo en tiempo real que utilice los conceptos de motion 101, vectores y algunos algoritmos de la unidad anterior. Vas a probar un algorimo para calcular la aceleración diferente a los que analizaste en esta unidad.

📤 Bitácora (Respuesta).

>**Concepto**
>
>Mi obra generativa simula el comportamiento de mosquitos atraídos por una fuente de luz. Usando los principios de Motion 101, cada insecto se mueve de forma >independiente aplicando vectores de posición, velocidad y aceleración.
>
>A medida que los mosquitos se acercan al foco de luz —controlado por el cursor del usuario—, su trayectoria se ajusta para alcanzarlo. Una vez llegan, quedan >“pegados” y comienzan a orbitar lentamente a su alrededor, representando el efecto de atracción incontrolable hacia la luz.
>
>El usuario puede interactuar con el sistema modificando en tiempo real la velocidad global y activando o desactivando la lámpara, lo que transforma el >comportamiento colectivo de los insectos. La pieza refleja cómo pequeños agentes responden a estímulos externos, generando patrones naturales, caóticos y >orgánicos.
> - Click derecho	Encender o apagar la luz
> - Flecha arriba	Aumentar la velocidad de los mosquitos
> - Flecha abajo	Disminuir la velocidad de los mosquitos
> - 1 a 9 (teclado numérico)	Establecer velocidad exacta (de 1 a 9)
> - 0 (teclado numérico)	Activar modo de desaceleración gradual
> - R	Reiniciar (eliminar todos los mosquitos)

>**¿Cómo piensas aplicar el marco MOTION 101 y por qué?**
>
>R/ Aprovecharé la facilidad que ofrece el marco Motion 101 para construir sistemas dinámicos, utilizando los principios de posición, velocidad y aceleración. Al estar estos componentes separados de forma estructurada, resulta sencillo manipular e intervenir cada uno de ellos de manera independiente. Esto me permite experimentar libremente con sus valores y adaptarlos al concepto que quiera desarrollar.

>
>**¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?**
>
>Usaré el algoritmo de aceleración (seek) el cual está basado en la **atracción hacia un punto**, calculando el vector de dirección hacia el objetivo y aplicandolo como aceleración, es el que más se acopla al concepto de bichos que quiero plantear.
>
>

**Codigo**

``` js
let mosquitos = [];
let luz;
let luzActiva = true;
let velocidadGlobal = 1;
let reduciendo = false;
let acelerando = false;
let frenando = false;

function setup() {
  createCanvas(800, 800);
  luz = createVector(width / 2, height / 2);
}

function draw() {
  background(0);

  // La luz sigue al mouse
  luz.set(mouseX, mouseY);

  // Dibujar halo de luz (farol)
  if (luzActiva) {
    noStroke();
    for (let r = 120; r > 30; r -= 10) {
      fill(255, 255, 100, map(r, 120, 30, 10, 80));
      ellipse(luz.x, luz.y, r, r);
    }
  }

  // Dibujar el foco de luz
  fill(luzActiva ? color(255, 255, 100) : 100);
  stroke(0);
  strokeWeight(1);
  ellipse(luz.x, luz.y, 30, 30);

  // Crear nuevos mosquitos
  if (frameCount % 5 === 0 && velocidadGlobal > 0) {
    mosquitos.push(new Mosquito());
  }

  // Control de velocidad global
  if (reduciendo && velocidadGlobal > 0) {
    velocidadGlobal -= 0.01;
    velocidadGlobal = max(velocidadGlobal, 0);
  }
  if (acelerando) {
    velocidadGlobal += 0.05;
    velocidadGlobal = min(velocidadGlobal, 10);
  }
  if (frenando) {
    velocidadGlobal -= 0.05;
    velocidadGlobal = max(velocidadGlobal, 0);
  }

  // Mostrar y actualizar mosquitos
  for (let m of mosquitos) {
    m.actualizar();
    m.mostrar();
  }

  // Eliminar mosquitos muertos
  mosquitos = mosquitos.filter(m => m.vivo || m.orbitando);

  // Mostrar velocidad
  fill(255);
  noStroke();
  textSize(16);
  text("Velocidad: " + velocidadGlobal.toFixed(2), 10, height - 10);
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

  if (key === 'r' || key === 'R') {
    mosquitos = [];
  }
}

function keyReleased() {
  if (keyCode === UP_ARROW) acelerando = false;
  if (keyCode === DOWN_ARROW) frenando = false;
}

function mousePressed() {
  if (mouseButton === LEFT) {
    luzActiva = !luzActiva;
    return false;
  }
}

class Mosquito {
  constructor() {
    this.pos = this.generarPosicionInicial();
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.vivo = true;
    this.orbitando = false;
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
    if (!this.vivo && !this.orbitando) return;

    if (this.orbitando) {
      let velocidad = luzActiva ? this.velocidadAngular : this.velocidadAngular * 0.25;
      this.anguloOrbita += velocidad * this.sentido;
      let x = luz.x + cos(this.anguloOrbita) * this.radioOrbita;
      let y = luz.y + sin(this.anguloOrbita) * this.radioOrbita;
      this.pos.set(x, y);
      return;
    }

    if (luzActiva) {
      let dir = p5.Vector.sub(luz, this.pos);
      dir.setMag(0.05 * velocidadGlobal);
      this.acc = dir;
    } else {
      this.acc = createVector(0, 0);
    }

    this.vel.add(this.acc);
    this.vel.limit(2 * velocidadGlobal);
    this.pos.add(this.vel);

    if (luzActiva && p5.Vector.dist(this.pos, luz) < 20) {
      this.orbitando = true;
      this.radioOrbita = 20 + random(10, 30);
      this.anguloOrbita = atan2(this.pos.y - luz.y, this.pos.x - luz.x);
      this.vel.set(0, 0);
      this.acc.set(0, 0);
    }

    if (
      this.pos.x < -50 || this.pos.x > width + 50 ||
      this.pos.y < -50 || this.pos.y > height + 50
    ) {
      this.vivo = false;
    }
  }

  mostrar() {
    noStroke();
    if (this.orbitando) {
      fill(200, 200, 0); // Orbitando la luz
    } else {
      fill(150, 150, 255); // Mosquito en movimiento
    }
    ellipse(this.pos.x, this.pos.y, 5, 5);
  }
}

```
https://editor.p5js.org/Juan1022/full/UYMUOXsBI

<img width="965" height="835" alt="image" src="https://github.com/user-attachments/assets/da8418f1-4b9e-485d-a72f-c1734adc0824" />


