---
layout: post
title: TypeScript + React
---

En el segundo meetup de [Sevilla TypeScript](https://www.meetup.com/es-ES/Sevilla-TypeScript/events/258304766/) realicé una ponencia introductoria sobre las ventajas de usar React con TypeScript. Con este post pretendo hacer una extensión de la introducción que hice en aquella ocasión. Para ello, vamos a aprender cómo iniciar nuestro primer proyecto con TypeScript y React. 

**¿Qué es React?**

Es una librería escrita en JavaScript, una de las más populares, pues cuenta con más de 100.000 stars en [GitHub](https://github.com/facebook/react). Está creado y matenido por Facebook, pensado para construir interfaces (UI) en la capa de front-end. Determina como los datos son almacenados mediante *props* y *state*. Las *props* (propiedades) son los atributos que se pueden definir en un componente, mientras que el *state* se definiría como la representación de dicho componente en un determinado momento, que irá mutando en función de su ciclo de vida.

**¿Cuáles son sus ventajas?**

Es declarativo, es decir, como hemos comentado previamente contamos con el estado de una aplicación y sus componentes responden a ese estado, esta circunstancia hace que el código sea mucho más predictivo y fácil de debbugear.  Otra de sus ventajas se muestra en el rendimiento, ya que antes se realizan las operaciones sobre el DOM virtual, eso provoca que la sensación de renderizado sea mucho más fluida. 

**¿Cómo creamos nuestra primera aplicación?**

Desde octubre del año pasado, esta tarea es bastante sencilla, gracias a que [create-react-app](https://github.com/facebook/create-react-app) cuenta con soporte oficial para TypeScript. Como puedes intuir leyendo su nombre, esta app nos permitirá crear una aplicación básica en react con el propósito de aprender cómo funciona en un entorno de desarrollo cómodo o crear componentes nuevos que luego podremos usar en nuestros proyectos.

Para crea nuestra nueva app, solo tendremos que tener instalado una versión de Node >= 6 y usar el comando:

{% highlight shell %}
npx create-react-app my-app --typescript
{% endhighlight %}

Sí, así de sencillo. Esto nos generará un proyecto con la siguiente estructura:

{% highlight shell %}
my-app
├── README.md
├── node_modules
├── package.json
├── tsconfig.json (opciones de compilación en TS)
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── serviceWorker.js
{% endhighlight shell%}

Y, además, nos generará un archivo tsconfig.json, donde pondremos todas las instrucciones que el compilador deberá obedecer.

Una vez con el proyecto creado, veamos cuáles son las ventajas de usar TypeScript con react con un sencillo ejemplo. La más importante, sin duda, el tipado. Es decir, podremos declarar componentes e intuir más fácilmente cuál será su funcionamiento dependiendo del tipo de parámetro que recibe. Veámoslo con un sencillísimo ejemplo, vamos a crear un nuevo componente que recibirá un número que será, en este caso, nuestra edad:

{% highlight react %}
 function DiMiEdad({edad}: {edad: number}){
      return <div>Hola! Tengo: {edad} años</div>
    }
{% endhighlight react%}

Cuando tratemos de invocarlo, en cuanto escribamos la primera letra, el editor nos sugerirá la función que vamos a usar, indicándonos qué tipo parámetro acepta, para no llevarnos a confusiones:

![la magia ocurre](/assets/ts04.png)

Esta la clave de todo, lo que hará que nos evitemos un quebradero de cabeza, lo que le da toda la potencia necesaria a nuestro proyecto para hacer que, cuando otras personas o incluso nosotros mismos trabajemos en ello, todo sea mucho más fácil. El ejemplo es bastante sencillo, pero se intuye que las posibilidades, en este sentido, son muchísimas para hacer que todo sea más robusto y fácilmente escalable.

Por otro lado, si tratamos de asignarle a edad un valor de tipo string, el compilador intervendrá y no hará su cometido:

{% highlight shell %}
Failed to compile.

/Users/christianivanov/Documents/ejemplo react2/my-app/src/App.tsx
TypeScript error: Type 'string' is not assignable to type 'number'.  TS2322

    11 |           <p>
    12 |             Edit <code>src/App.tsx</code> and save to reload.
  > 13 |             <DiMiEdad edad = {'nombre'}/>
       |                       ^
    14 |           </p>
    15 |           <a
    16 |             className="App-link"
{% endhighlight shell%}

Todo esto nos ayudará a prevenir errores no deseados. Grandes empresas como AirBnb han sido capaces de [reducir sus errores](https://twitter.com/swyx/status/1093670844495089664) en producción usando TypeScript un 38%, una cifra bastante significativa, que nos deja evidencias significativas de las ventajas de usar TypeScript con otras tecnologías para minimizar errores.

¡Esto es todo por ahora! Solo quería mostrar mediante un pequeño ejemplo las posibilidades de juntar TypeScript con React. En futuros posts, seguiremos aprendiendo como combinar ambas tecnologías.

Happy coding!




















