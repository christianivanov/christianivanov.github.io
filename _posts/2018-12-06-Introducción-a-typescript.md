---
layout: post
title: Introducción a TypeScript (Primera parte)
---

En este post me gustaría hacer una breve introducción a TypeScript y analizar los motivos por los que está teniendo tanta popularidad en la comunidad. TypeScript es una tecnología creada y mantenida por Microsoft, de la mano de [Anders Hejlsberg](https://es.wikipedia.org/wiki/Anders_Hejlsberg). En la actualidad, su última versión publicada es la [3.2](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-2.html), potencia y versatilidad desde sus inicios, TypeScript es ideal para usarlo en proyectos robustos y escalables, ya que es un <i>superseteado</i> de JavaScript en el que se añaden el tipado estático y los objetos basados en clases. Además de todo lo mencionado con anterioridad, es [open source](https://github.com/Microsoft/TypeScript). A modo de resumen, empecemos citando algunas características destacadas del lenguaje:

* Orientada a objetos: soporta todos los elementos de dicho paradigma de programación (clases, interfaces, herencia, polimorfismo, etcétera). 
* Lenguaje tipado: clasificación de las variables según los tipos de datos que usan. 
* Basado en estándares (ECMAScript).
* Compilado: puedes detectar errores antes de ejecución (no interpreta). Convierte código a JavaScript.

Para programar en TypeScript lo más recomendable es usar [Visual Studio Code](https://code.visualstudio.com), el editor de código basado en Electron y creado por Microsoft ofrece soporte para todas las versiones del lenguaje y, además, añade: Git integrado, debugging, IntelliSense y extensiones. Honestamente, para mí es uno de los mejores productos que ha hecho Microsoft en la última década.

¿Por qué usar TypeScript?
- Ideal para proyectos escalables, orientado a un entorno empresarial. TypeScript está por grandes empresas y grandes proyectos, un claro de ejemplo de ello es Google, que usa para el desarrollo de Angular dicho lenguaje.
- La [documentación](https://www.typescriptlang.org/docs/home.html) que existe sobre el proyecto es bastante y de calidad. 
- Para frontend y backend: si vienes de desarrollar en backend (como es mi caso), con TypeScript te sentirás como en casa. El lenguaje permite homogeneidad en este aspecto, aunque suela usarse para front, también puedes usarlo en el backend de tu aplicación.
- La amplia y vibrante comunidad que hay detrás de TypeScript, son unos de los motivos fundamentales del éxito del lenguaje. 
- Interoperable con JavaScript.


Los comandos básicos que debes saber para comenzar con TypeScript son:

- Instalar TypeScript, para ello tenemos que tener previamente instalado npm y Node:
{% highlight js %}
npm install -g typescript
{% endhighlight %}

- Para compilar debemos usar el comando:
{% highlight js %}
tsc <nombre_de_archivo>.ts
{% endhighlight %}

Entre las características principales del lenguaje, hemos destacado la introducción de clases, interfaces y funciones, elementos básicos en el paradigma de programación orientado a objetos. Dichos elementos en TypeScript, se presenta de la siguiente forma:

{% highlight js %}
lass Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
{% endhighlight %}

En la página oficial de TypeScript podemos mirar los [tipos de datos básicos](https://www.typescriptlang.org/docs/handbook/basic-types.html) que pueden usarse, siendo los mismos que se pueden usar en JavaScript. Se destaca el tipo any, ideal para utilizarlo con librería de terceros en JavaScript que no están adaptadas a TypeScript, eso sí, no muy recomendado si quieres evitar problemas de mutabilidad. Otro a destacatar es el tipo tuple, que representa una colección de valores de varios tipos.

Por otro lado, si quieres comenzar a trastear con el lenguaje, en la página oficial existe [una herramienta](https://www.typescriptlang.org/play/index.html) que te permitirá hacerlo. Hasta aquí el primer post de introducción al lenguaje, en el próximo se abordarán otras características del lenguaje como: decorators, generators, implementación async / await, etcétera. 

Un saludo y happy coding!




