#DOM

### innerText vs textContent
La principal diferencia entre innertText y textContent es que innertText tiene en cuenta los estilos aplicados.
En el siguiente ejemplo:
```
<div>Hello <span style="display:none;">World</span><div>
```
innertText devolvería 'Hello' mientras que textContent devolvería helloWorld.

[Ejemplo en Codepen](https://codepen.io/fonsirs/pen/gOmbmeX)

Como consecuencia de esto textContent es mucho más eficiente.

Por otro lado, textContent es una propieda de Nodo mientras que innerText sólo de HTMLElement