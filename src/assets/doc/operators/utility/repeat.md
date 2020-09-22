# repeat

### Retorna un Observable que se resuscribe <code>count</code> veces al flujo fuente cuando el Observable fuente se completa

Returns an Observable that will resubscribe to the source stream when the source stream completes, at most count times.

### Firma

`repeat<T>(count: number = -1): MonoTypeOperatorFunction<T>`

### Parámetros

<table>
<tr><td>count</td><td>Opcional. El valor por defecto es -1.

El número de veces que se repiten los valores del Observable fuente. Si se especifica el valor 0, se producirá un Observable vacío.

</td></tr>
</table>

### Retorna

`MonoTypeOperatorFunction<T>`: Un Observable que se resuscribirá `count` veces al flujo fuente cuando el flujo fuente se complete.

### Descripción

Repite todos los valores emitidos por la fuente. Es como el operador `retry`, para casos que no sean de error.

<img src="assets/images/marble-diagrams/utility/repeat.png" alt="Diagrama de canicas del operador repeat">

De forma similar a `retry`, este operador repite el flujo de elementos emitidos por la fuente, para casos que no sean de error. `repeat` puede ser útil para crear Observables que deban tener alguna clase de patrón repetido.

Nota: `repeat(0)` retorna un Observable vacío y `repeat()` se repetirá para siempre.

## Ejemplos

Repetir una petición AJAX

[StackBlitz](https://stackblitz.com/edit/rxjs-repeat-1?file=index.ts)

```javascript
import { repeat } from "rxjs/operators";
import { of } from "rxjs";

const ghibliFilm$ = ajax.getJSON(
  "https://ghibliapi.herokuapp.com/films/58611129-2dbc-4a81-a72f-77ddfc1b1b49"
);

ghibliFilm$
  .pipe(
    map(({ title }) => title),
    repeat(3)
  )
  .subscribe(console.log);
// Output: My Neighbor Totoro, My Neighbor Totoro, My Neighbor Totoro
```

Retornar un Observable vacío

[StackBlitz](https://stackblitz.com/edit/rxjs-repeat-2?file=index.ts)

```javascript
import { repeat } from "rxjs/operators";
import { of } from "rxjs";

const pokemon$ = of("Squirtle", "Bulbasaur", "Charmander");

pokemon$
  .pipe(repeat(0))
  .subscribe(console.log, console.error, () => console.log("Flujo completado"));
// Salida: Flujo completado
```

Repetir un flujo de mensajes de forma infinita

[StackBlitz](https://stackblitz.com/edit/rxjs-repeat-3?file=index.ts)

```javascript
import { repeat } from "rxjs/operators";
import { of } from "rxjs";

const answer$ = of("La respuesta es 42");

answer$.pipe(repeat()).subscribe(console.log);
// Salida: La respuesta es 42, La respuesta es 42, La respuesta es 42, La respuesta es 42...
```

#### Ejemplos de la documentación oficial

Repetir un flujo de mensajes

```javascript
import { of } from "rxjs";
import { repeat, delay } from "rxjs/operators";

const source = of("Repeat message");
const example = source.pipe(repeat(3));
example.subscribe((x) => console.log(x));

// Results
// Repeat message
// Repeat message
// Repeat message
```

Repetir 3 valores, 2 veces

```javascript
import { interval } from "rxjs";
import { repeat, take } from "rxjs/operators";

const source = interval(1000);
const example = source.pipe(take(3), repeat(2));
example.subscribe((x) => console.log(x));

// Results every second
// 0
// 1
// 2
// 0
// 1
// 2
```

- [Documentación oficial en inglés](https://rxjs-dev.firebaseapp.com/api/operators/repeat)
- [Código fuente](https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/repeat.ts)