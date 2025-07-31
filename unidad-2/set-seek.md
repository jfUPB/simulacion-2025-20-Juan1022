# Unidad 2

## 🔎 Fase: Set + Seek

## Actividad 1 
### Introducción a los vectores

📤 Bitácora

*¿Cómo funciona la suma dos vectores en p5.js?*
*¿Por qué esta línea position = position + velocity; no funciona?*

> Bitácora (Respuesta)
> - La suma de vectores en p5.js funciona con la función .add podemos sumar 2 vectores y asi moficicamos el valor del vector original.
> - No funciona debido a que está usando mal la función de sumar, deberia ser .add()

## Actividad 2
### Repaso 

📤 Bitácora

*¿Qué tuviste que hacer para hacer la conversión propuesta?*
*Muestra el código que utilizaste para resolver el ejercicio.*

> Bitácora (Respuesta)
> 1. Reemplazar x y x por un solo vector pos()
> 2. Cambiamos x++ o x-- por metodos vectoriales como .add
> 3. agregar vectores como velocity

```javascript
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  background(255);
  walker = new Walker(); // inicializar el walker
}

function draw() {
  walker.step();  // mueve al walker
  walker.show();  // lo dibuja
}

class Walker {
  constructor() {
    this.pos = createVector(width / 2, height / 2); // posición vectorial
  }

  show() {
    stroke(0);
    point(this.pos.x, this.pos.y); // dibuja el punto en la posición actual
  }

  step() {
    // genera un nuevo paso unitario en una dirección al azar
    let step = createVector(0, 0);
    const choice = floor(random(4));
    if (choice === 0) {
      step.x = 1;
    } else if (choice === 1) {
      step.x = -1;
    } else if (choice === 2) {
      step.y = 1;
    } else {
      step.y = -1;
    }

    this.pos.add(step); // aplica el paso a la posición
  }
}

```

## Actividad 3
### Experimenta

📤 Bitácora

*¿Qué resultado esperas obtener en el programa anterior?*
*¿Qué resultado obtuviste?*
*Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.*
*¿Qué tipo de paso se está realizando en el código?*
*¿Qué aprendiste?*

> Bitácora (Respuesta)
>  - Esperaba que se creara un vector en posición 6 y 9 y que luego modificara el vector original para convertirlo en 20 o 30.
> -  El resultado que esperaba se creo el vector original y se modificó cambiendo x=20 e y=30
> ### Paso por valor
> ```
> let a = 10;
>
>function cambiarValor(x) {
>  x = 20;
>  console.log("Dentro de la función:", x); 
>}
>
>cambiarValor(a);
>console.log("Fuera de la función:", a); 
>```
> ### Paso por referencia
>```
> let v = createVector(5, 5);
>
>function mover(vector) {
>  vector.x = 10;
>}
>
>mover(v);
>console.log(v.x); // 10
```
