# distinctUntilChanged

### Retorna un Observable que emite todos los elementos emitidos por el Observable fuente que sean distintos al valor anterior

### Firma

`distinctUntilChanged<T, K>(compare?: (x: K, y: K) => boolean, keySelector?: (x: T) => K): MonoTypeOperatorFunction<T>`

### Parámetros

<table>
<tr><td>compare</td><td>Opcional. El valor por defecto es <code>undefined</code>.
Función de comparación opcional para comprobar si un elemento es distinto al elemento anterior.</td></tr>

<tr><td>keySelector</td><td>Opcional. El valor por defecto es <code>undefined</code>.
Tipo: <code>(x: T) => K</code>.</td></tr>
</table>

### Retorna

`MonoTypeOperatorFunction<T>`: Un Observable que emite elementos del Observable fuente que tengan valores distintos.

# Descripción

Si se proporciona una función de comparación, se utilizará para comprobar si cada elemento se debe emitir o no.

Si no se proporciona una función de comparación, se utiliza una verificación de igualdad.

## Ejemplos

Usar `distinctUntilChanged` sin una función de comparación

[StackBlitz](https://stackblitz.com/edit/rxjs-distinctuntilchanged-1?file=index.ts)

```javascript
import { distinctUntilChanged } from "rxjs/operators";
import { of } from "rxjs";

const fruit$ = of("Fresa", "Cereza", "Cereza", "Arándano", "Arándano", "Fresa");

fruit$.pipe(distinctUntilChanged()).subscribe(console.log);
// Salida: Fresa, Cereza, Arándano, Fresa
```

Usar `distinctUntilChanged` con una función de comparación

[StackBlitz](https://stackblitz.com/edit/rxjs-distinctuntilchanged-2?file=index.ts)

```javascript
import { distinctUntilChanged } from "rxjs/operators";
import { of } from "rxjs";

const pokemon$ = of(
  { name: "Squirtle", type: "Water" },
  { name: "Bulbasaur", type: "Grass" },
  { name: "Bulbasaur", type: "Grass" },
  { name: "Charmander", type: "Fire" },
  { name: "Charmander", type: "Fire" },
  { name: "Squirtle", type: "Water" },
  { name: "Bulbasaur", type: "Grass" }
);

pokemon$
  .pipe(
    distinctUntilChanged(
      ({ name: previousName }, { name }) => previousName === name
    )
  )
  .subscribe(console.log);
/* Salida: 
  { name: "Squirtle", type: "Water" },
  { name: "Bulbasaur", type: "Grass" },
  { name: "Charmander", type: "Fire" }
  { name: "Squirtle", type: "Water" },
  { name: "Bulbasaur", type: "Grass" }
 */
```

### Ejemplos de la documentación oficial

Un ejemplo simple con números

```javascript
import { of } from "rxjs";
import { distinctUntilChanged } from "rxjs/operators";

of(1, 1, 2, 2, 2, 1, 1, 2, 3, 3, 4)
  .pipe(distinctUntilChanged())
  .subscribe((x) => console.log(x)); // 1, 2, 1, 2, 3, 4
```

Un ejemplo usando una función de comparación

```javascript
    import { of } from 'rxjs';
    import { distinctUntilChanged } from 'rxjs/operators';

    interface Person {
       age: number,
       name: string
    }

    of<Person>(
        { age: 4, name: 'Foo'},
        { age: 7, name: 'Bar'},
        { age: 5, name: 'Foo'},
        { age: 6, name: 'Foo'},
      ).pipe(
        distinctUntilChanged((p: Person, q: Person) => p.name === q.name),
      )
      .subscribe(x => console.log(x));

    // displays:
    // { age: 4, name: 'Foo' }
    // { age: 7, name: 'Bar' }
    // { age: 5, name: 'Foo' }
```

- [Documentación oficial en inglés](https://rxjs-dev.firebaseapp.com/api/operators/distinctUntilChanged)
- [Código fuente](https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/distinctUntilChanged.ts)