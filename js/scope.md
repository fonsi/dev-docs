# Scope

### var / let / const

**var**: 
- scope global si declarada fuera de una función
- scope de funcion si declarada dentro
- se puede volver a declarar y se puede modificar
- el hoisting mueve la declaración de la variable al inicio del fichero y la inicializa como undefined

**let**:
- scope de bloque
- puede ser modificada pero no se puede volver a declarar (dentro del mismo scope)
- el hoisting mueve la declaración de la variable al inicio del fichero pero no la inicializa, por lo que da error si la intentas usar.

**const**:
- scope de bloque
- no se puede modificar ni volver a declarar (dentro del mismo scope)
- el hoisting mueve la declaración de la variable al inicio del fichero pero no la inicializa, por lo que da error si la intentas usar.

### Hoisting

Hoisting se refiere al comportamiento de JS de guardar en memoria la declaración de funciones y variables durante la fase de "compilación". Normalmente se explica como que las variables se "mueven" al principio de su scope pero realmente el código queda donde lo habías escrito.

Esto permite usar funciones y variables declaradas como var antes de su declaración.

Notar que sólo se "mueve" la declaración. Las variables tendrán valor undefined hasta que se llegue a la linea donde se ejecuta la asignación.

En el caso de variables declaradas con let y const, si se hace hoisting de su declaración pero no se inicializan. Por lo que a la práctica no pueden ser usadas antes de su declaración.