---
layout: post
title: Introducción a TypeScript (segunda parte)
---

En esta segunda parte de introducción a TypeScript me gustaría seguir abordando algunos conceptos o características del lenguaje que me resultan interesante. En la primera parte, se introdujeron de manera muy elemental los elementos más básicos. Hicimos énfasis en los tipos básicos y la orientación del lenguaje al paradigma de programación basado en objetos. También se mencionó por qué usar TypeScript en detrimento de otras tecnologías. Dicho esto, sigamos analizando más características:

**Módulos**

Introducido con ECMAScript 2015. Permite exportar (<span style="color:red">export</span>) e importar (<span style="color:red">import</span>) clases, funciones o variables de un proyecto a otro, es decir, proporciona visibilidad. En el siguiente ejemplo se muestra algunas de sus posibilidades:

{% highlight js %}
export interface StringValidator {
    isAcceptable(s: string): boolean;
}

export const numberRegexp = /^[0-9]+$/;

export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}

import { ZipCodeValidator } from "./ZipCodeValidator";

let myValidator = new ZipCodeValidator();

{% endhighlight %}


**Decorators**

Son funciones que pueden ser usadas para hacer anotaciones sobre clases, variables o métodos (inyección de dependencias, unit test, etcétera). También son usadas para generar metadatos, que pueden usarse por otras librerías con posterioridad. Básicamente, es una función que recibe un constructor. Veamos un sencillo ejemplo de ello, declarando el siguiente decorador:

{% highlight js %}
function logType(target : any, key : string) {
      var t = Reflect.getMetadata("design:type", target, key);
      console.log(`${key} type: ${t.name}`);
    }
{% endhighlight %}

Una vez definido, podemos emplearlo de la siguiente manera:

{% highlight js %}
  class Demo{ 
      @logType // apply property decorator
      public attr1 : string;
    }
{% endhighlight %}

Mostrándose así el siguiente mensaje por consola:

{% highlight js %}
attr1 type: String
{% endhighlight %}

Se trata de un ejemplo bastante básico y sencillo de su uso. En un futuro dedicaré un post aparte solo a este apartado.

**Generators**

Usa la sintaxis <span style="color:red">function*</span> para ser creado. Devuelve un generator object. Siguen la interfaz iterator. Por tanto, provee los siguientes métodos: _next_, _return_ y _throw_.

{% highlight js %}
function* idMaker(){
  let index = 0;
  while(index < 3)
    yield index++;
}

let gen = idMaker();

console.log(gen.next()); // { value: 0, done: false }
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { done: true }
{% endhighlight %}

Devuelve junto con un resultado el estado de la iteración, si termina devuelve _false_ y en caso contrario _true_.

**Asyc/Await**

Introducido en la versión 1.7, permite escribir código asíncrono. Con <span style="color:red">async</span> declaramos la función asíncrona, que devuelve una <span style="color:red">promise</span>, mientras que el operador  <span style="color:red">await</span> es usado para esperar dicha <span style="color:red">promise</span>.

{% highlight js %}
// printDelayed is a 'Promise<void>'
async function printDelayed(elements: string[]) {
    for (const element of elements) {
        await delay(400);
        console.log(element);
    }
}

async function delay(milliseconds: number) {
    return new Promise<void>(resolve => {
        setTimeout(resolve, milliseconds);
    });
}

printDelayed(["Hello", "beautiful", "asynchronous", "world"]).then(() => {
    console.log();
    console.log("Printed every element!");
});
{% endhighlight %}

En el ejemplo anterior, cada input será pintado cada 400 milisegundos.


**Generics**

Permite diseñar clases y métodos que aplazan la especificación de uno o varios tipos hasta que el código declare y cree una instancia de dicha clase o método.

{% highlight js %}
/** A class definition with a generic parameter */
class Queue<T> {
  private data = [];
  push = (item: T) => this.data.push(item);
  pop = (): T => this.data.shift();
}

const queue = new Queue<number>();
queue.push(0);
queue.push("1"); // ERROR : cannot push a string. Only numbers allowed

// ^ if that error is fixed the rest would be fine too
{% endhighlight %}

**Union & Intersection types**

Entramos en los tipos más avanzados. Los tipo _Intersection_ pueden combinar más de un tipo y see usan mediante <span style="color:red">&</span>. Por otro lado, la _Union_ son entidades que pueden ser de un tipo u otro, dependiendo de su propósito. Dicha Union se representa mediante el símbolo <span style="color:red">|</span>. Veámoslo con un ejemplo:

{% highlight js %}
interface Bird {
    fly();
    layEggs();
}

interface Fish {
    swim();
    layEggs();
}

//Union (objects wich are either A or B)
declare var pet1: fish | bird;

pet1.layEggs(); // Ok
pet1.swim(); // ERROR: Property swim does not exist on type 'Bird'

//Intersection  (objects wich are both A and B)
declare var pet2: fish & bird;

pet2.layEggs(); // Ok
pet2.swim(); // Ok

{% endhighlight %}

Como vemos, en la intersección al invocar un método u otro, ambos serán válidos ya que _pet2_ puede ser de tipo _fish_ y _bird_. No obstante, no sucede lo mismo con _pet1_, ya que el método swim() no sabemos si estamos haciendo referencia a una entidad tipo _Fish_ o _Bird_ (si fuese 'Bird' provocaríamos un error al llamarlo).

**Control Flow Analysis & Type Guards**

Existen dos tipos de programadores: los que les gusta que los lenguajes sean fuertemente tipados y los que no. Para los que preferimos el primer caso, TypeScript proporciona herramientas para evitar problemas de mutabilidad. En primer lugar, el _control flow_ analiza el flujo del programa para determinar de la manera más exacta el tipo de una variable que es declarado como Union Type. Un ejemplo:

{% highlight js %}
let command: string | string[];

command = "pwd";
command.toUpperCase; // Here, command is of type 'string'

command = ["ls", "-la"];
command.join(" "); // Here, command is of type 'string[]'
{% endhighlight %}

En el primer caso, declaramos command como una variable de tipo String, por tanto, el compilador entenderá como siempre tendrá que tratarlo como tal, de modo que podremos invocar el método toUpperCase() para convertirlo en mayúscula. Por otro lado, si volvemos a declarar la misma variable como String[], ya dejará de ser tratado como String.

Además, esto nos permite limitar el tipo de un objeto dentro de un bloque condicional, por ejemplo:

{% highlight js %}
function composeCommand(
  command: string | string[]
): string {
  if (typeof command === "string") {
    return command;
  }

  return command.join(" ");
}
{% endhighlight %}

En este caso, tenemos una función que puede recibir un tipo String o String[] y en la que vamos a determinar su comportamiento en función del tipo, algo muy útil en términos de seguridad.

Respecto a los Type Guards, es una expresión que realiza una verificación en tiempo de ejecución para garantizar el tipo en algún contexto y que se puede analizar en el Control Flow para ser lo más específico posible. Imaginemos que tenemos una aplicación para gestionar un videoclub en la que tenemos clientes de dos tipos: free o premium.

{% highlight js %}
function isUserPremiumUser(user:User): user is PremiumUser{
    return user.plan=== 'premium';
}

function getUserEmailRecapFrequency(user:User): number{
    if(isPremiumUser(user)){
        return user.customization.recap_freq;
    }else{
        return -1;
    }
}
{% endhighlight %}

Aquí indicamos que si el usuario es premium, lo devuelva como tal y, además, actúe de una manera determinada (return user.customization.recap_freq;), sino que devuelva -1. TypeScript, *lenguaje fuertemente tipado*.

Esto es todo por ahora, pero hay [mucho más](https://github.com/Microsoft/TypeScript/wiki/Roadmap). Poco a poco iremos abordando, de forma más concreta, cada una de sus características. Espero que os haya resultado útil.

Happy coding!

















