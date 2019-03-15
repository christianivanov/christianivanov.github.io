---
layout: post
title: Analizando el compilador de TypeScript
---

Siempre me ha apasionado la programación a bajo nivel. Si quieres saber cómo funciona un lenguaje, estudia su compilador. Es por ello que la curiosidad me ha conducido a analizar el compilador de TypeScript, donde he encontrado algunas cosas interesantes. En primer lugar, veamos en un diagrama la estructura del compilador de TypeScript, para un posterior análisis:

![Arquitectura del compilador](https://raw.githubusercontent.com/wiki/Microsoft/TypeScript/images/architecture.png)

Como podemos intuir, el Core es la parte más importante. Cuenta con el *Type Resolver* (checker.ts) que valida cada operación semántica y genera el diagnóstico adecuado en caso de error. También podemos ver el *Emitter*, encargado de convertir el input (archivos .ts and .d.ts) en archivos JavaScript o el *Language Service*, que soporta las operaciones de edición como autocompletado de sintaxis, re-factoring o el soporte incremental de compilación. Por otro lado, el *Standalone Compiler* se encarga de la integración con otros motores, por ejemplo, con Nodejs. La siguiente capa con *VSShim* y *tsserver* tiene la función de integrarse con Visual Studio y otros editores de texto para hacer que la experiencia sea mucho más sencilla. Por último, nos encontramos con el editor, donde daremos todas las instrucciones necesarias para que el Core haga que la magia suceda.

**Estructura de datos: Abstract Syntax Tree (AST)**

En cuanto a la estructura de datos, nos encontramos con el *Abstract Syntax Tree* que es el encargado de clasificar, mediante nodos, cada elemento generado por nuestro código. Con el *SourceFile* se va referenciando cada uno de los identificadores mencionados. Por tanto, un programa (unidad de compilación) está compuesto por un conjunto de SourceFile. Otra pieza destacada es el *Symbol*, que conecta cada uno de los nodos con los que estén relacionados dentro de la misma entidad. Se define de la siguiente forma:

{% highlight js %}
  function Symbol(this: Symbol, flags: SymbolFlags, name: __String) {
       this.flags = flags;
       this.escapedName = name;
       this.declarations = undefined;
       this.valueDeclaration = undefined;
       this.id = undefined;
       this.mergeId = undefined;
       this.parent = undefined;
}
{% endhighlight %}

Son creados por el *Binder*, cuya principal función es recopilar varias piezas de código para otorgarle coherencia o cohesión. Citando un ejemplo del libro de Remo Jansen [Learning TypeScript](https://www.amazon.es/Learning-TypeScript-2-x-captivating-applications/dp/1788391470/ref=dp_ob_title_bk), el lenguaje tiene una característica denomina 'declaration merging' que permite precisamente hacer esta fusión, veámoslo un ejemplo con código:

{% highlight js %}
 interface Person {
       name: string;
}

interface Person {
       surname: string;
}

const person: Person = { name: "Christian", surname: "Ivanov" };
{% endhighlight %}

En este caso, el comportamiento esperado del *Binder* es unificar las propiedades del tipo declarado previamente y mergearlo.

**Repasando el proceso de compilación**

Todo el proceso comienza con el procesamiento de todos los archivos ubicados en <span style="color:red">/// <reference path=... /></span> e <span style="color:red">import</span>. Como hemos mencionado con anterioridad, el parser crea los nodos, que no es más que una representación abstracta del input que ha procesado en formato de árbol. Una vez hecho, se generan los <span style="color:red">Symbol</span>s. Cada Symbol es creado para identificar cada uno de los elementos procesados, pudiendo tener el mismo nombre, pero diferente alcance. Al ser generados, se crea un *SourceFile* mediante el uso de la API *createSourceFile*. Por lo que un programa es, básicamente, un conjunto de SourceFiles junto con una configuración de compilación (*CompilerOptions*). Cuando el programa se ha instanciado, entra en acción el *TypeChecker*, que es el core del compilador. Entre sus funciones, se encuentra: recopilar todos los símbolos creados en los diferentes SourceFiles, mergearlo en una unidad y determinar su tipo, para localizar errores. Estando todo validado, el *Emitter* genera el output, archivos de tipo .js, .jsx, .d.ts, y .js.map (recuerda que el compilador de TypeScript, transcompila todo a JavaScript).

Sin más, el propósito de este post solo era echar un pequeño vistazo a la capa más superficial del compilador, si quieres ver cómo sería el resultado del AST que hemos mencionado, podéis echarle un vistazo al siguiente [enlace](https://ts-ast-viewer.com). Por otro lado, para interactuar con la API que el mismo compilador proporciona, podéis también echarle un ojo a [ts-morph](https://github.com/dsherret/ts-morph), un proyecto que nos permitirá leer y manipular el AST.

Happy coding!

















