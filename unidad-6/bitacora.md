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


Ya con todos esto cambios este es el resultado final, hubo cosas que me quedaron pendientes por implementar y es que generén lineas por donde vayan pasando dependiendo de su frecuencia para moldear el grosor de cada una.

# Resultados:

https://editor.p5js.org/Juan1022/full/5hZB14EZA

# Codigos: 

``` sketch

// --- CONFIGURACIÓN GLOBAL ---
let gusanos = [];
let song;       
let fft;        
let factorRuido = 0.005; 
let tiempoOffset = 0; 
let lideres = []; 
let isPaused = false; // 🚨 NUEVO: Estado de pausa

// Variables de Flow Field
let flowfield; 
let resolution = 40; 
let radius = 1;      


// Define los rangos de frecuencia (Bandas de colores sutiles)
const BANDS = [
  { color: 'rgb(80, 0, 150)', min: 20, max: 60, name: 'Graves' },      
  { color: 'rgb(0, 50, 200)', min: 60, max: 250, name: 'MediosBajos' },   
  { color: 'rgb(0, 100, 255)', min: 250, max: 2000, name: 'Medios' },  
  { color: 'rgb(50, 150, 255)', min: 2000, max: 6000, name: 'AgudosMedios' },
  { color: 'rgb(150, 200, 255)', min: 6000, max: 20000, name: 'Agudos' } 
];


function preload() {
  song = loadSound('BG.mp3'); 
}

// --- FUNCIÓN: Inicializa la grilla ---
function initializeFlowField(resolution) {
  let cols = floor(width / resolution);
  let rows = floor(height / resolution);
  let vectors = [];
  
  for (let i = 0; i < cols * rows; i++) {
    vectors.push(createVector(0, 0)); 
  }
  return { vectors: vectors, resolution: resolution, cols: cols, rows: rows };
}

function setup() {
  createCanvas(800, 600);
  background(240);
  
  initSimulation();
  
  userStartAudio(); 
  song.loop(); 
  
  loop(); // Asegura que el bucle inicie
}

// FUNCIÓN PRINCIPAL DE INICIALIZACIÓN
function initSimulation() {
  flowfield = initializeFlowField(resolution);
  gusanos = [];
  lideres = [];
  tiempoOffset = 0;

  for (let i = 0; i < 50; i++) {
    let x = random(width);
    let y = random(height);
    let bandaElegida = random(BANDS); 
    gusanos.push(new Gusano(x, y, bandaElegida.color, bandaElegida));
  }
  
  if (!fft) {
     fft = new p5.FFT(0.8, 1024); 
  }
}

// --- FUNCIÓN PARA PAUSAR/REANUDAR (llamada por el botón) ---
function togglePause() {
  isPaused = !isPaused;
  let button = select('#pauseButton');

  if (isPaused) {
    noLoop(); // Detiene el bucle draw (y por lo tanto el movimiento)
    if (song.isPlaying()) {
      song.pause(); // Pausa la música
    }
    button.html('Reanudar');
  } else {
    loop(); // Reanuda el bucle draw
    if (getAudioContext().state === 'running' && !song.isPlaying()) {
      song.loop(); // Reanuda la música
    }
    button.html('Pausar');
  }
}

// --- FUNCIÓN: Dibuja el Flow Field con Flechas de COLOR ---
function drawFlowField() {
  let res = flowfield.resolution;
  let cols = flowfield.cols;
  
  push();
  stroke(0, 200, 200, 120); 
  strokeWeight(1);

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < flowfield.rows; j++) {
      let index = i + j * cols;
      let v = flowfield.vectors[index];
      
      let x = i * res + res / 2;
      let y = j * res + res / 2;
      
      if (v.mag() > 0.05) { 
        push();
        translate(x, y);
        let len = res * 0.4; 
        rotate(v.heading());
        
        line(0, 0, len, 0); 
        
        line(len, 0, len - 4, -2);
        line(len, 0, len - 4, 2);

        pop();
      } else {
        stroke(0, 200, 200, 40); 
        point(x, y);
        stroke(0, 200, 200, 120); 
      }
    }
  }
  pop();
}


// --- FUNCIÓN: Modifica la grilla con el mouse (Moldeado) ---
function updateFlowField() {
  if (mouseIsPressed && mouseButton === LEFT) {
    let mx = mouseX;
    let my = mouseY;
    let res = flowfield.resolution;
    let cols = flowfield.cols;

    let mouseVector = createVector(pmouseX - mouseX, pmouseY - mouseY);
    mouseVector.normalize();
    mouseVector.mult(-1); 

    let currentRadius = radius; 

    for (let i = -currentRadius; i <= currentRadius; i++) {
      for (let j = -currentRadius; j <= currentRadius; j++) {
        let col = floor(mx / res) + i;
        let row = floor(my / res) + j;
        
        if (col >= 0 && col < cols && row >= 0 && row < flowfield.rows) {
          let index = col + row * cols;
          let currentVector = flowfield.vectors[index];

          currentVector.lerp(mouseVector, 0.2); 
        }
      }
    }
  }
}

function draw() {
  background(240, 50); 
  
  // 🚨 Toda la lógica de movimiento, audio y tiempo solo se ejecuta si NO está pausado.
  if (!isPaused) {
    updateFlowField(); 
    
    if (song.isPlaying()) {
      fft.analyze();
    }
    tiempoOffset += 0.005;
  }
  
  drawFlowField(); 

  // 2. ACTUALIZAR Y DIBUJAR LÍDERES
  for (let i = lideres.length - 1; i >= 0; i--) {
    if (!isPaused) {
      lideres[i].actualizar();
    }
    lideres[i].dibujar(); 
    if (!isPaused && !lideres[i].isAlive()) {
      lideres.splice(i, 1); 
    }
  }

  // 3. ITERAR Y APLICAR LÓGICA POR GUSANO
  for (let gusano of gusanos) {
    
    let aperturaBoca = 0.2; 
    
    if (!isPaused && song.isPlaying()) {
        let energiaBanda = fft.getEnergy(gusano.freqRange.min, gusano.freqRange.max);
        
        aperturaBoca = map(energiaBanda, 0, 150, 0.2, PI * 0.9);
        aperturaBoca = constrain(aperturaBoca, 0.2, PI * 0.9);
    } 
    
    // Aplicar movimiento solo si no está pausado
    if (!isPaused) {
      let fuerzaTotal = createVector(0, 0);
      
      let fuerzaFlow = gusano.steerFlowField(flowfield); 
      fuerzaFlow.mult(4.0); 
      fuerzaTotal.add(fuerzaFlow);

      let fuerzaSeparacion = gusano.separate(gusanos);
      fuerzaSeparacion.mult(2.5); 
      fuerzaTotal.add(fuerzaSeparacion);

      for (let lider of lideres) {
          let fuerzaLider = gusano.attractToLeader(lider);
          fuerzaLider.mult(2.5); 
          fuerzaTotal.add(fuerzaLider);
      }
      
      let nx = gusano.pos.x * factorRuido;
      let ny = gusano.pos.y * factorRuido;
      let anguloRuido = noise(nx, ny, tiempoOffset + gusano.noiseOffset) * TWO_PI * 4;
      let fuerzaPerlin = p5.Vector.fromAngle(anguloRuido);
      fuerzaPerlin.mult(0.1); 
      fuerzaTotal.add(fuerzaPerlin);

      gusano.aplicarFuerza(fuerzaTotal);
      gusano.actualizar(); 
    }

    gusano.dibujar(aperturaBoca); 
  }
  
  if (getAudioContext().state !== 'running') {
    textSize(16);
    fill(0);
    textAlign(CENTER);
    text("Haz clic para desbloquear el audio. Presiona 'R' para Resetear el Lienzo. Clic en un gusano para cambiar su banda de frecuencia.", width / 2, height / 2);
  }
}

// --- CREACIÓN DEL LÍDER Y CAMBIO DE COLOR/BANDA ---
function mousePressed() {
  userStartAudio();
  
  let particleClicked = false;
  
  for (let i = 0; i < gusanos.length; i++) {
    let gusano = gusanos[i];
    let d = dist(mouseX, mouseY, gusano.pos.x, gusano.pos.y);
    
    if (d < gusano.diametroCabeza / 2) {
      
      let currentBandIndex = BANDS.findIndex(band => band.name === gusano.freqRange.name);
      let nextBandIndex = (currentBandIndex + 1) % BANDS.length;
      
      gusano.changeBand(BANDS[nextBandIndex]);
      
      particleClicked = true;
      break; 
    }
  }
  
  if (!particleClicked) {
     lideres.push(new Lider(mouseX, mouseY));
  }
  
  // 🚨 SI ESTÁ PAUSADO Y SE CAMBIÓ EL COLOR, FORZAMOS UN REDIBUJADO
  if (isPaused && particleClicked) {
      redraw();
  }
}

function keyPressed() {
  if (key === 'R' || key === 'r') {
    resetSketch();
  }
}

function resetSketch() {
  initSimulation();
  background(240); 
  // Si estaba pausado, se queda pausado después del reset
  if (isPaused) {
    redraw();
  }
}
```


``` LIDER
// --- CLASE LIDER (ATTRACTION POINT) ---
class Lider {
  constructor(x, y) {
    this.pos = createVector(x, y);
    // 🚨 Duración modificable aquí: 200 frames (aproximadamente 3-4 segundos)
    this.maxLifetime = 200; 
    this.lifetime = this.maxLifetime;
    this.baseWidth = 30; // Ancho base de la "fogata"
    this.maxHeight = 60; // Altura máxima de la "fogata"
  }

  // 🚨 Dónde modificar la duración:
  // Cambia el valor de 'this.maxLifetime' arriba. 
  // Un valor de 200 = unos 3-4 segundos a 60 fps.

  actualizar() {
    this.lifetime--;
  }

  dibujar() {
    // El factor de vida (0 a 1)
    let lifeFactor = map(this.lifetime, 0, this.maxLifetime, 0, 1);
    
    // El alfa (transparencia) disminuye al final
    let alpha = map(this.lifetime, 0, this.maxLifetime, 0, 255);
    
    // El tamaño de la fogata se encoge con el tiempo
    let currentWidth = this.baseWidth * lifeFactor;
    let currentHeight = this.maxHeight * lifeFactor;
    
    // Color cálido (Naranja/Rojo/Amarillo)
    let r = map(lifeFactor, 0, 1, 100, 255);
    let g = map(lifeFactor, 0, 1, 0, 150);
    
    fill(r, g, 0, alpha); // Color de la llama
    noStroke();
    
    push();
    translate(this.pos.x, this.pos.y);
    
    // Simula una forma de llama vertical usando un óvalo y un punto superior
    ellipse(0, 0, currentWidth, currentWidth / 2); // Base del fuego
    
    // Forma triangular o de gota para el cuerpo principal de la llama
    beginShape();
    vertex(-currentWidth / 2, 0);
    // Usa ruido para que la punta de la llama fluctúe
    let noiseX = noise(frameCount * 0.1) * 10 - 5;
    let noiseY = noise(frameCount * 0.1 + 100) * 10 - 5;
    vertex(noiseX, -currentHeight + noiseY); // Punta fluctuante
    vertex(currentWidth / 2, 0);
    endShape(CLOSE);
    
    pop();
  }

  isAlive() {
    return this.lifetime > 0;
  }
}

```


``` GUSANO
// --- CLASE GUSANO (WORM) ---
class Gusano {
  constructor(x, y, colorStr, freqRange) {
    // Propiedades de movimiento
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxVel = 2; 
    this.maxForce = 0.2; 

    // Rastro y Apariencia
    this.rastro = [];
    this.maxRastro = 25;
    this.diametroCabeza = 30;
    this.color = color(colorStr);
    
    // Audio y Ruido
    this.freqRange = freqRange;
    this.noiseOffset = random(1000);
  }

  // MÉTODO: Cambia la banda de frecuencia del gusano
  changeBand(newBand) {
    this.freqRange = newBand;
    this.color = color(newBand.color);
  }

  aplicarFuerza(fuerza) {
    this.acc.add(fuerza);
  }

  // LÓGICA DE STEERING: Flow Field Dibujado (Lee el vector de la grilla)
  steerFlowField(flowfield) {
    if (!flowfield || flowfield.cols === 0 || flowfield.rows === 0) {
      return createVector(0, 0);
    }
    
    let resolution = flowfield.resolution;
    let col = floor(this.pos.x / resolution);
    let row = floor(this.pos.y / resolution);
    
    col = constrain(col, 0, flowfield.cols - 1);
    row = constrain(row, 0, flowfield.rows - 1);

    let index = col + row * flowfield.cols;
    let desired = flowfield.vectors[index].copy();

    desired.setMag(this.maxVel);
    
    let steer = p5.Vector.sub(desired, this.vel);
    steer.limit(this.maxForce * 1.5); 
    return steer;
  }

  // LÓGICA DE STEERING: Atracción por Líder
  attractToLeader(lider) {
    if (!lider) return createVector(0, 0);

    let d = p5.Vector.dist(this.pos, lider.pos);
    let desiredSeparation = this.diametroCabeza * 10; 

    if (d > 0 && d < desiredSeparation) {
      let desired = p5.Vector.sub(lider.pos, this.pos); 
      desired.normalize();
      
      let fuerzaMag = map(d, 0, desiredSeparation, this.maxVel * 1.5, 0); 
      desired.setMag(fuerzaMag);

      let steer = p5.Vector.sub(desired, this.vel);
      steer.limit(this.maxForce * 1.5); 
      return steer;
    }
    return createVector(0, 0);
  }
  
  // LÓGICA DE FLOCKING: Separación
  separate(gusanos) {
    let desiredSeparation = this.diametroCabeza * 1.5;
    let steer = createVector(0, 0);
    let count = 0;

    for (let other of gusanos) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if ((d > 0) && (d < desiredSeparation)) {
        let diff = p5.Vector.sub(this.pos, other.pos);
        diff.normalize();
        diff.div(d); 
        steer.add(diff);
        count++;
      }
    }

    if (count > 0) {
      steer.div(count);
    }

    if (steer.mag() > 0) {
      steer.normalize();
      steer.mult(this.maxVel);
      steer.sub(this.vel);
      steer.limit(this.maxForce * 2); 
    }
    return steer;
  }

  actualizar() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxVel); 
    this.pos.add(this.vel);
    this.acc.mult(0);

    this.rastro.push(this.pos.copy());

    if (this.rastro.length > this.maxRastro) {
      this.rastro.shift(); 
    }

    this.envolverBordes();
  }

  envolverBordes() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }

  // Dibuja el gusano (boca más grande con círculo negro)
  dibujar(apertura) { 
    noStroke();
     
    // DIBUJAR EL RASTRO (CUERPO)
    for (let i = 0; i < this.rastro.length - 1; i++) {
      let r = this.rastro[i];
      let diametro = map(i, 0, this.maxRastro, 4, this.diametroCabeza * 0.8);
      
      let c = color(this.color); 
      c.setAlpha(100); 
      fill(c);
      
      ellipse(r.x, r.y, diametro, diametro);
    }
     
    // DIBUJAR LA CABEZA
    let cabeza = this.rastro[this.rastro.length - 1];
    if (!cabeza) return;
    
    noStroke();
    fill(this.color);
    ellipse(cabeza.x, cabeza.y, this.diametroCabeza, this.diametroCabeza);

    let d = this.diametroCabeza;

    // Ojos
    fill(0);
    ellipse(cabeza.x - d / 6, cabeza.y - d / 8, d / 8, d / 8);
    ellipse(cabeza.x + d / 6, cabeza.y - d / 8, d / 8, d / 8);

    // BOCA: Usa el parámetro 'apertura'
    stroke(0);
    strokeWeight(1.5);
    noFill();
    push();
    translate(cabeza.x, cabeza.y + d / 10);
    
    const bocaSize = d / 3; 

    // Dibuja el arco de la boca
    arc(0, 0, bocaSize, bocaSize, PI - apertura / 2, PI + apertura / 2);
    
    // Círculo negro dentro de la boca
    noStroke();
    fill(0); 
    let diametroInternoBoca = map(apertura, 0.2, PI * 0.9, 0, bocaSize * 0.9);
    ellipse(0, 0, diametroInternoBoca, diametroInternoBoca);
    
    pop();
  }
}
```

<img width="1009" height="719" alt="image" src="https://github.com/user-attachments/assets/4412401d-3870-4f4b-80c6-e71f2200d509" />


<img width="776" height="756" alt="image" src="https://github.com/user-attachments/assets/7d7d0e8c-7bd3-4a99-b7aa-ec7e2291001d" />























