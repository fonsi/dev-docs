# Mapping

### map
A partir de un stream de datos se obtiene otro stream al que se le ha aplicado la función de mapping a cada uno de los valores del stream original

```javascript
from([1, 2, 3]).pipe(
    map(value => value * 10)
)
// los valores del stream resultante serían [10, 20, 30]
```

## Higher-Order Observables
Mientras que el operador *map* transforma los valores del stream original en otros valores usando la función de mapping,
un High-Order Observable lo que hace es crear otro observable por cada uno de los valores del stream original.

Sería lo mismo que subscribirse a un observable dentro de otro subscribe,
pero esto es un anti-pattern:
```javascript
// nested subscribes anti-pattern
from(['url1', 'url2', 'url3']).subscribe((url) => {
    http$(url).subscribe((result) => {
        console.log(result);
    });
})
```
Para evitar esto tenemos las funciones de *concatMap*, *mergeMap* y *switchMap*

### concatMap
Para entender *concatMap* primero hay que entender *concat*

#### concat
El operador *concat* nos permite concatenar dos o más observables de forma que cuando
el primero de ellos se completa se sigue emitiendo los valores del segundo. Y así sucesivamente.
```javascript
const obs1$ = from([1, 2]);
const obs2$ = from([3, 4]);
const result$ = concat(obs1$, obs2$).subscribe(console.log);
// esto imprime 1, 2, 3, 4
```
Lo más importante aquí es el detalle de que *result$* internamente no se subscribe a *obs2$* hasta que *obs1$* ha completado.

#### concatMap
Con el concepto de *concat* en mente es fácil comprender como funciona *concatMap*.

Por cada uno de los valores de un observable generaremos un nuevo observable pero no nos subscribiremos a él hasta que el anterior se haya completado.

Podemos decir que los observables internos se procesan de forma secuencial
```javascript
from(['url1', 'url2', 'url3'])
    .pipe(
        concatMap((url) => {
            return http$(url);
        })
    )
    .subscribe((result) => {
        console.log(result);
    })
```
En este ejemplo la petición a *url2* no se haría hasta que se completara la petición a *url1*. Y lo mismo para *url3*.


### mergeMap

#### merge
Cuando hacemos *merge* de varios observables el resultado es otro observable que va emitiendo cada uno de los valores
que emitan los observables originales a medida que vayan ocurriendo.

El observable resultante no completará hasta que todos los observables originales hayan completado.

```javascript
const obs1$ = interval(100).pipe(take(2));
const obs2$ = interval(100).pipe(take(4));

merge(obs1$, obs2$).subscribe(console.log);
/* 
0 // obs1
0 // obs2
1 // obs1
1 // obs2
2 // obs2
3 // obs2
*/
```

#### mergeMap
*mergeMap* se basa en la misma lógica que *merge* y por lo tanto, al contrario que *concatMap* todos los observables internos
son suscritos al inicio y van emitiendo valores en paralelo.

```javascript
from([1, 2, 3]).pipe(
  mergeMap((arrayValue) => {
      const intervalTime = arrayValue * 100;
      const valuesToTake = arrayValue;
      return interval(intervalTime).pipe(
        take(valuesToTake),
        map((interval) => {
          console.log(`Array value: ${arrayValue} - Iteration: ${interval + 1} - Time: ${intervalTime * (interval + 1)}ms`)
        })
      )
  })
).subscribe();
/* 
'Array value: 1 - Iteration: 1 - Time: 100ms'
'Array value: 2 - Iteration: 1 - Time: 200ms'
'Array value: 3 - Iteration: 1 - Time: 300ms'
'Array value: 2 - Iteration: 2 - Time: 400ms'
'Array value: 3 - Iteration: 2 - Time: 600ms'
'Array value: 3 - Iteration: 3 - Time: 900ms'
*/
```

### switchMap