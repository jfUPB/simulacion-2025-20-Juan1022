# Unidad 1 - Continuación del Set + Seek 

## 🔎 Fase: Set + Seek

## Actividad 06
### Distribución personalizada: Lévy flight

*Analicemos juntos y detenidamente el concepto de Lévy flight.*
- Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.
- Explica por qué usaste esta técnica y qué resultados esberabas obtener.
- Copia el código en tu bitácora.
- Coloca en enlace a tu sketch en p5.js en tu bitácora.
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.
  

  >**📝 Bitácora (Respuesta):**
  >
  > Usé el ejemplo de la actividad anterior en la que por medio de una distribución gaussiana hice un cielo de estrellas, siguiendo este concepto de galaxia, pensé
  > en que esta nueva distribución quedaba perfecto para generar "nebulosas" las cuales tendrián un comportamiento parecido a lo que dicta esta distirbución.
  >
  >Esperaba obtener varios puntos disperos con acumulaciones de varios en algunos lugares del canva
  >
  >
```javascript
  
  let stars = [];
  let clouds = [];

function setup() {
  createCanvas(400, 400);
  background(0);
  generateStars();
  generateNebulas();
  noLoop();
}

function draw() {
  background(0);

  // Dibujar estrellas
  noStroke();
  fill(180, 200, 30, 120);
  for (let star of stars) {
    circle(star.x, star.y, random(1, 2.5));
  }

  // Nubes de nebulosa
  noStroke();
  for (let p of clouds) {
    let c = chooseColor();
    fill(c[0], c[1], c[2], 60); 
    circle(p.x, p.y, random(2, 4)); 
  }
}

function generateStars() {
  stars = [];
  for (let i = 0; i < 1000; i++) {
    let x = random(width);
    let y = randomGaussian(100, 50);
    stars.push({ x, y });
  }
}

function generateNebulas() {
  clouds = [];
  
  let centers = 5;
  for (let j = 0; j < centers; j++) {
    let pos = createVector(random(width), random(height));
    
    for (let i = 0; i < 2000; i++) {
      let step = levy();
      let angle = random(TWO_PI);
      pos.add(cos(angle) * step, sin(angle) * step);
      
      if (pos.x > 0 && pos.x < width && pos.y > 0 && pos.y < height) {
        clouds.push(pos.copy());
      }

      // Reiniciar el origen para romper acumulación
      if (random() < 0.005) {
        pos = createVector(random(width), random(height));
      }
    }
  }
}

function levy() {
  return pow(random(1), -1.5);
}

function chooseColor() {
  let choice = floor(random(3));
  if (choice === 0) return [0, 100, 255];   
  if (choice === 1) return [150, 0, 255];   
  return [0, 200, 255];                     // Celeste
}

function mousePressed() {
  generateNebulas();
  redraw();
}
```
https://editor.p5js.org/Juan1022/full/MFznNy6mt

<img width="498" height="492" alt="image" src="https://github.com/user-attachments/assets/7d506282-6655-40d4-b12f-faefb9aa1468" />

## Actividad 07
### Ruido Perlin
*Analicemos junto el concepto de ruido Perlin analizando la figura 0.4: “A graph of Perlin noise values over time (left) and of random noise values over time (right)”.*

- Crea un nuevo sketch en p5.js donde los visualices.
- Explica el concepto qué resultados esberabas obtener.
- Copia el código en tu bitácora.
- Coloca en enlace a tu sketch en p5.js en tu bitácora.
- Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

>**📝 Bitácora (Respuesta):**
>
> A diferencia del random que puede generar valores bastantes dispersos y desorganizados, el ruido de perlín genera cambios mas suaves y organicos, asi que con este experimento, usando como base el ruido de Perlin espero generar figuras, movimientos y texturas más organicas.
>
>
```javascript
function setup() {
  createCanvas(600, 400);
  noFill();
}

function draw() {
  background(0, 100, 150, 40); // Azul oscuro translúcido, simula el agua

  stroke(255, 255, 255, 70); // Blanco suave, semi-transparente
  strokeWeight(1.5);

  for (let y = 0; y < height; y += 10) {
    beginShape();
    for (let x = 0; x < width; x += 5) {
      let nx = x * 0.01;
      let ny = y * 0.01;
      let t = frameCount * 0.01; // tiempo
      let offset = noise(nx, ny + t) * 40;
      vertex(x, y + offset);
    }
    endShape();
  }
}
```

https://editor.p5js.org/Juan1022/full/sbxB1SJiq

<img width="746" height="494" alt="image" src="https://github.com/user-attachments/assets/956bebad-becb-4d33-b5cf-898b8947d46d" />
