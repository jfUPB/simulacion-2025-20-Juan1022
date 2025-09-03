# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> 
>Los triángulos del grupo 1 (orbiters) se posicionan en un círculo usando el seno y el coseno de un ángulo (x = cos(orb.angle) * pulseRadius). Esta es la forma más común de crear un >movimiento circular perfecto. Además, las rotaciones de los triángulos (rotate(angle + 90)) también se basan en el cálculo de ángulos.

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> 
> Para la Unidad 3, apliqué el concepto de fuerzas, específicamente la atracción gravitacional. Creé un grupo de triángulos que son atraídos hacia un punto en el centro del lienzo.
>La "masa" de este punto central no es fija; cambia en función de la amplitud del archivo de audio. Cuando el sonido es fuerte, la atracción gravitacional aumenta, haciendo que los >triángulos se muevan más rápido y cerca del centro. Cuando el sonido es suave, la atracción disminuye, permitiendo que los triángulos se alejen y se muevan de forma más relajada.

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> 
>Usé las funciones translate() y rotate() de p5.js para hacer que los triángulos "bailaran" en la obra.
>translate() me permitió mover cada triángulo a su propia posición en el espacio, como si estuvieran bailando por el lienzo.
>rotate() hizo que giraran sobre su propio eje, lo que, combinado con su movimiento, creó la ilusión de un baile o un giro dinámico.

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> 
> De la Unidad 1, utilicé el concepto de aleatoriedad para crear un gradiente de color orgánico en el fondo de la obra y lo bailarines dentro de la obra.
Específicamente, me basé en el ruido de Perlin para generar dos paletas de colores: una fría (azules y morados) y una cálida (rojos y naranjas). El ruido de Perlin me permite obtener valores que cambian de manera suave y predecible, lo que hace que la transición entre los colores sea fluida y no abrupta.
## ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
> Día 5: La Conexión Final: El Cerebro del Visualizador
>Hoy me enfoqué en el "cerebro" del visualizador: la conexión entre la música y el código. Descubrí que la clave para que la obra reaccione al sonido es el objeto FFT (Transformada Rápida >de Fourier) de la librería p5.sound.js. Este objeto es el encargado de "escuchar" la canción y descomponerla en sus partes más pequeñas.
>
>Lo que hago es tomar dos propiedades principales del sonido:
>
>Amplitud (Volumen): Con la función fft.getEnergy('bass'), obtengo un valor de 0 a 255 que me dice qué tan fuerte es el bajo en la canción. Con este valor, mapeo la velocidad de los >triángulos, el tamaño de la vibración del círculo exterior, y hasta el color del degradado del fondo, haciéndolo más rojo cuando el bajo es más fuerte.
>
>Frecuencia (Tono): Para el tono, uso fft.getEnergy() para las frecuencias medias y agudas, y fft.getCentroid() para el tono general. Con esto, hago que la figura del centro cambie: si >predominan los graves, es un círculo; si son medios, un pentágono; y si son agudos, un triángulo. También uso el tono para que los colores de los triángulos cambien de azul (para tonos >graves) a rojo (para tonos agudos).

## Enlace a la obra en el editor de p5.js

https://editor.p5js.org/Juan1022/full/vrqFFnfDD

## Código de la obra 

``` js
let centerType = 0; // 0 = pentágono, 1 = triángulo, 2 = círculo
let orbiters = [];
let orbiters2 = [];
let numOrbiters = 8;
let orbitRadius1 = 10;
let orbitRadius2 = 180;
let orbitLimit = 250;

let audioStarted = false;

// Variables para el visualizador de audio
let mySound;
let fft;

function preload() {
  mySound = loadSound('DAN.mp3'); 
}

function setup() {
  createCanvas(600, 600);
  angleMode(DEGREES);
  colorMode(HSB, 360, 100, 100);

  fft = new p5.FFT();
  
  for (let i = 0; i < numOrbiters; i++) {
    let angle = map(i, 0, numOrbiters, 0, 360);
    orbiters.push({ angle: angle });
  }

  for (let i = 0; i < numOrbiters; i++) {
    let angle = map(i, 0, numOrbiters, 0, 360);
    let x = cos(angle) * orbitRadius2;
    let y = sin(angle) * orbitRadius2;
    orbiters2.push({
      pos: createVector(x, y),
      vel: p5.Vector.random2D().mult(2),
      acc: createVector(0, 0),
      history: []
    });
  }
}

function draw() {
  if (mySound.isLoaded() && !audioStarted) {
    mySound.loop();
    audioStarted = true;
  }
  
  // Analiza el espectro de audio
  fft.analyze();
  
  // --- Mapeo de sonido a propiedades visuales ---
  let bassEnergy = fft.getEnergy('bass');
  let midEnergy = fft.getEnergy('mid');
  let trebleEnergy = fft.getEnergy('treble');
  let spectralCentroid = fft.getCentroid();
  let maxCentroid = 8000;
  
  // *** CREAR EL DEGRADADO DE FONDO ***
  let colorFrom, colorTo;
  let bassMapped = map(bassEnergy, 0, 255, 0, 1);
  
  // Color de inicio (arriba): Transiciona de azul a rojo
  let hueFrom = map(bassMapped, 0, 1, 220, 360);
  colorFrom = color(hueFrom, 100, 80);
  
  // Color de fin (abajo): Transiciona de morado a naranja
  let hueTo = map(bassMapped, 0, 1, 260, 20);
  colorTo = color(hueTo, 100, 80);
  
  // Dibuja cada línea del degradado en el fondo
  for (let y = 0; y < height; y++) {
    let inter = map(y, 0, height, 0, 1);
    let c = lerpColor(colorFrom, colorTo, inter);
    stroke(c);
    line(0, y, width, y);
  }

  // Ahora el resto del código se dibuja encima del degradado
  translate(width / 2, height / 2);

  // Asignar el tipo de figura central según la banda de frecuencia dominante
  if (bassEnergy > midEnergy && bassEnergy > trebleEnergy) {
    centerType = 2; // Círculo (tonos graves)
  } else if (midEnergy > bassEnergy && midEnergy > trebleEnergy) {
    centerType = 0; // Pentágono (tonos medios)
  } else {
    centerType = 1; // Triángulo (tonos agudos)
  }
  
  // Mapear la energía a variables de control
  let speedFactor = map(bassEnergy, 0, 255, 0.5, 3);
  
  // --- Ondulación del círculo exterior: ahora depende de los bajos ---
  noFill();
  stroke(150);
  strokeWeight(2);
  
  let waveAmount = map(bassEnergy, 0, 255, 10, 80);
  
  beginShape();
  let waveform = fft.waveform();
  for (let i = 0; i < waveform.length; i++) { 
    let angle = map(i, 0, waveform.length, 0, 360);
    let r = orbitLimit + waveform[i] * waveAmount;
    let x = cos(angle) * r;
    let y = sin(angle) * r;
    vertex(x, y);
  }
  endShape(CLOSE);

  // --- centro ---
  noFill();
  stroke(255);
  strokeWeight(2);
  if (centerType === 0) {
    polygon(0, 0, 50, 5);
  } else if (centerType === 1) {
    polygon(0, 0, 60, 3);
  } else if (centerType === 2) {
    ellipse(0, 0, 80);
  }

  // --- color dinámico basado en el centroide espectral ---
  let hueValue = map(spectralCentroid, 0, maxCentroid, 200, 0); 
  let c = color(hueValue, 100, 100);

  // --- orbiters grupo 1 (Estrella Pulsante) ---
  let pulseRadius = map(bassEnergy, 0, 255, 10, 60);

  for (let orb of orbiters) {
    let x = cos(orb.angle) * pulseRadius;
    let y = sin(orb.angle) * pulseRadius;

    push();
    translate(x, y);
    rotate(orb.angle + frameCount * speedFactor);
    fill(c);
    noStroke();
    triangle(-10, 8, 10, 8, 0, -12);
    pop();

    orb.angle += 0.5 * speedFactor;
  }

  // --- orbiters grupo 2 (Vórtice que se Arremolina) ---
  let centerMass;
  if (centerType === 0) {
    centerMass = 1000;
  } else if (centerType === 1) {
    centerMass = 500;
  } else if (centerType === 2) {
    centerMass = 2500;
  }

  for (let orb of orbiters2) {
    let dir = createVector(0, 0).sub(orb.pos);
    let d = constrain(dir.mag(), 50, 300);
    let strength = (centerMass * speedFactor) / (d * d);
    dir.setMag(strength);
    orb.acc.add(dir);
    orb.vel.add(orb.acc);
    orb.pos.add(orb.vel);
    orb.acc.mult(0);

    if (orb.pos.mag() > orbitLimit) {
      orb.pos.setMag(orbitLimit);
      orb.vel.mult(-1);
    }

    orb.history.push(orb.pos.copy());
    if (orb.history.length > 50) {
      orb.history.shift();
    }

    noFill();
    stroke(hueValue, 80, 100, 0.5);
    beginShape();
    for (let v of orb.history) {
      vertex(v.x, v.y);
    }
    endShape();

    noStroke();
    push();
    translate(orb.pos.x, orb.pos.y);
    let angle = orb.vel.heading();
    rotate(angle + 90);
    fill(c);
    triangle(-10, 8, 10, 8, 0, -12);
    pop();
  }
}

function polygon(x, y, radius, npoints) {
  let angle = 360 / npoints;
  beginShape();
  for (let a = 0; a < 360; a += angle) {
    let sx = x + cos(a) * radius;
    let sy = y + sin(a) * radius;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}


```

<img width="1200" height="1200" alt="image" src="https://github.com/user-attachments/assets/266e33fe-a886-4d7c-97bc-4dcd1315d414" />






