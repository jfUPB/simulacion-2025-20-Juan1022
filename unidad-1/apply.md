# Unidad 1

## 🛠 Fase: Apply

## Actividad 08
### Creación de obra generativa

*Vas a crear una obra generativa interactiva en tiempo real utilizando los conceptos de aleatoriedad que has aprendido en esta unidad.*
*Tu obra debe:*

- Usar al menos tres conceptos estudiados en esta unidad COMBINADOS de manera creativa y coherente.
- Tu obra de ser interactiva y generativa en tiempo real. Puedes usar el mouse, el teclado o cualquier otro sensor de entrada para interactuar con la obra.

Reporta en tu bitácora lo siguiente:

- Un texto donde expliques el concepto de obra generativa.
- Copia el código en tu bitácora.
- Coloca en enlace a tu sketch en p5.js en tu bitácora.
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

>**📝 Bitácora (Respuesta):**
>
> Una obra generativa es una obra que no es manual, no es creada desde cero por ti. Una obra generativa se crea a sí misma gracias a condiciones o procesos que un artista le imponga, de esta manera el resultado final dado por condiciones y algoritmos es unico y cambiante.
>
```javascript


let campoDeFlujo;
let peces = [];
let anzueloX, anzueloY;
let burbujas = [];
let algas = [];
let probabilidadBurbuja = 0.05; 
let pecesLuminosos = [];

function setup() {
  createCanvas(640, 480);
  campoDeFlujo = new CampoDeFlujo(20);
  for (let i = 0; i < 80; i++) {
    peces.push(new Pez(random(width), random(height), random(2, 4), random(0.1, 0.4)));
  }
  for (let i = 0; i < width; i += 20) {
    algas.push(new Alga(i, height, random(0.03, 0.1), random(2, 8), random(15, 30)));
  }
}

function draw() {
  dibujarFondoDegradado(color(0, 60, 100), color(0, 150, 200));

  actualizarAnzuelo();
  dibujarAnzuelo();

  campoDeFlujo.actualizar();

  if (frameCount % 20 === 0) {
    peces.push(new Pez(random(width), random(height), random(2, 4), random(0.1, 0.4)));

    if (random(1) < probabilidadLevy()) {
      pecesLuminosos.push(new PezLuminoso(random(width), random(height)));
    }
  }

  for (let pez of peces) {
    pez.seguir(campoDeFlujo);
    pez.evitarAnzuelo(createVector(anzueloX, anzueloY));
    pez.actualizar();
    pez.mostrar();
  }

  for (let pezL of pecesLuminosos) {
    pezL.evitarAnzuelo(createVector(anzueloX, anzueloY));
    pezL.actualizar();
    pezL.mostrar();
  }

  if (random() < probabilidadBurbuja) {
    burbujas.push(new Burbuja(random(width), height));
  }
  for (let i = burbujas.length - 1; i >= 0; i--) {
    burbujas[i].actualizar();
    burbujas[i].mostrar();
    if (burbujas[i].fueraDePantalla()) {
      burbujas.splice(i, 1);
    }
  }

  for (let alga of algas) {
    alga.mostrar();
  }
}

function actualizarAnzuelo() {
  anzueloX = mouseX;
  anzueloY = mouseY;
}

function dibujarAnzuelo() {
  stroke(255);
  strokeWeight(2);
  line(anzueloX, 0, anzueloX, anzueloY);
  fill(255);
  ellipse(anzueloX, anzueloY, 10);
}

function probabilidadLevy() {
  let paso = pow(random(1), -1.5);
  return constrain(map(paso, 1, 10, 0.02, 0.1), 0.02, 0.1);
}

function dibujarFondoDegradado(c1, c2) {
  for (let y = 0; y < height; y++) {
    let inter = map(y, 0, height, 0, 1);
    let c = lerpColor(c1, c2, inter);
    stroke(c);
    line(0, y, width, y);
  }
}

class CampoDeFlujo {
  constructor(r) {
    this.resolucion = r;
    this.columnas = floor(width / this.resolucion);
    this.filas = floor(height / this.resolucion);
    this.campo = this.crearArray2D(this.columnas);
    this.inicializar();
  }

  crearArray2D(n) {
    let arr = new Array(n);
    for (let i = 0; i < n; i++) {
      arr[i] = [];
    }
    return arr;
  }

  inicializar() {
    noiseSeed(floor(random(1000)));
    let xoff = 0;
    for (let i = 0; i < this.columnas; i++) {
      let yoff = 0;
      for (let j = 0; j < this.filas; j++) {
        let theta = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        this.campo[i][j] = p5.Vector.fromAngle(theta);
        yoff += 0.1;
      }
      xoff += 0.1;
    }
  }

  actualizar() {
    this.inicializar();
  }

  buscar(posicion) {
    let col = constrain(floor(posicion.x / this.resolucion), 0, this.columnas - 1);
    let fil = constrain(floor(posicion.y / this.resolucion), 0, this.filas - 1);
    return this.campo[col][fil].copy();
  }
}

class Pez {
  constructor(x, y, velocidadMax, fuerzaMax) {
    this.posicion = createVector(x, y);
    this.velocidad = p5.Vector.random2D();
    this.aceleracion = createVector(0, 0);
    this.radio = constrain(randomGaussian() * 1 + 4, 2, 7);
    this.velocidadMax = velocidadMax;
    this.fuerzaMax = fuerzaMax;
  }

  actualizar() {
    this.velocidad.add(this.aceleracion);
    this.velocidad.limit(this.velocidadMax);
    this.posicion.add(this.velocidad);
    this.aceleracion.mult(0);
  }

  aplicarFuerza(f) {
    this.aceleracion.add(f);
  }

  seguir(campo) {
    let deseado = campo.buscar(this.posicion);
    deseado.mult(this.velocidadMax);
    let direccion = p5.Vector.sub(deseado, this.velocidad);
    direccion.limit(this.fuerzaMax);
    this.aplicarFuerza(direccion);
  }

  evitarAnzuelo(anzuelo) {
    let d = p5.Vector.dist(this.posicion, anzuelo);
    if (d < 80) {
      let fuga = p5.Vector.sub(this.posicion, anzuelo);
      fuga.setMag(this.velocidadMax);
      let direccion = p5.Vector.sub(fuga, this.velocidad);
      direccion.limit(this.fuerzaMax * 2);
      this.aplicarFuerza(direccion);
    }
  }

  mostrar() {
    let angulo = this.velocidad.heading() + PI / 2;
    fill(255, 150);
    stroke(0);
    push();
    translate(this.posicion.x, this.posicion.y);
    rotate(angulo);
    beginShape();
    vertex(0, -this.radio * 2);
    vertex(-this.radio, this.radio * 2);
    vertex(this.radio, this.radio * 2);
    endShape(CLOSE);
    pop();
  }
}

class PezLuminoso {
  constructor(x, y) {
    this.posicion = createVector(x, y);
    this.velocidad = p5.Vector.random2D().mult(2);
    this.color = color(0, 255, 0);
    this.radio = 4;
  }

  evitarAnzuelo(anzuelo) {
    let d = p5.Vector.dist(this.posicion, anzuelo);
    if (d < 80) {
      let fuga = p5.Vector.sub(this.posicion, anzuelo);
      fuga.setMag(2);
      let direccion = p5.Vector.sub(fuga, this.velocidad);
      direccion.limit(0.4);
      this.velocidad.add(direccion);
    }
  }

  actualizar() {
    this.posicion.sub(this.velocidad);
    this.limites();
  }

  mostrar() {
    let angulo = this.velocidad.heading() + PI / 2;
    fill(this.color);
    stroke(0);
    push();
    translate(this.posicion.x, this.posicion.y);
    rotate(angulo);
    beginShape();
    vertex(0, -this.radio * 2);
    vertex(-this.radio, this.radio * 2);
    vertex(this.radio, this.radio * 2);
    endShape(CLOSE);
    pop();
  }

  limites() {
    if (this.posicion.x < 0) this.posicion.x = width;
    if (this.posicion.x > width) this.posicion.x = 0;
    if (this.posicion.y < 0) this.posicion.y = height;
    if (this.posicion.y > height) this.posicion.y = 0;
  }
}

class Burbuja {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.radio = random(4, 10);
    this.velocidad = random(1, 3);
  }

  actualizar() {
    this.y -= this.velocidad;
  }

  mostrar() {
    noFill();
    stroke(255, 150);
    strokeWeight(1);
    ellipse(this.x, this.y, this.radio);
  }

  fueraDePantalla() {
    return this.y < -this.radio;
  }
}

class Alga {
  constructor(x, y, velocidad, amplitud, segmentosAltura) {
    this.x = x;
    this.y = y;
    this.velocidad = velocidad;
    this.amplitud = amplitud;
    this.fase = random(TWO_PI);
    this.segmentosAltura = int(segmentosAltura);
  }

  mostrar() {
    stroke(0, 255, 0, 120);
    strokeWeight(2);
    noFill();
    beginShape();
    for (let i = 0; i < this.segmentosAltura; i++) {
      let desplazamiento = sin(this.fase + frameCount * this.velocidad + i) * this.amplitud;
      vertex(this.x + desplazamiento, this.y - i * 10);
    }
    endShape();
  }
}
```

https://editor.p5js.org/Juan1022/full/LPNWg5OV3

<img width="773" height="574" alt="image" src="https://github.com/user-attachments/assets/1fe03c8a-38bf-4519-94da-fb1897db77bb" />


