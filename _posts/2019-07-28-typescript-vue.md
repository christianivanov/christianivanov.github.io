---
layout: post
title: TypeScript + VueJs
---

En el último [meetup de Sevilla TypeScript](https://www.meetup.com/es-ES/Sevilla-TypeScript/events/261518440/) se ha hablado de las ventajas de utilizar TypeScript con Vue.js, además de cómo iniciar un proyecto haciendo énfasis en las diferencias que existen respecto a otras tecnologías. En este post, vamos a ahondar un poco en este asunto, tratando de mostrar cuáles son las ventajas y desventajas de utilizar Vue.js con TypeScipt.

**¿Qué es Vue?**

Vue.js es un framework progresivo para construir interfaces de usuario bastante versátil, es [Open Source](https://github.com/vuejs/vue) y cuenta más de 140.000 estrellas en GitHub. Fue desarrollado por [Evan You](https://twitter.com/youyuxi) con la idea de obtener lo mejor de otras tecnologías como Angular o React, quedándose solo con lo imprescindible, haciendo así una tecnología bastante ligera (el Core de VueJS tan solo ocupa unos 20KB).

Características:
- Es reactivo.
- Es progresivo, como hemos mencionado anteriormente, podemos decidir qué partes de la tecnología incluimos en nuestro proyecto. Está diseñado para ser adoptado incrementalmente.
- Al igual que React, el uso del DOM virtual hace que sea sensación de renderizado sea bastante fluida.

**Integración con TypeScript**

Aunque la integración es cada vez mejor (sobre todo desde a partir de la versión 3.x), quizás habría que ser prudente a la hora de llevar código con ambas tecnologías a producción. Otro aspecto que destacar es que hasta ahora la documentación que puedes encontrar sobre ambas tecnologías es escasa aunque, sin duda, si tuviera que empezar un proyecto a día de hoy con Vue lo haría con TypeScript.

La configuración necesaria para iniciar un proyecto, tal y como se recomienda en la web oficial, es la siguiente:

{% highlight javascript %}
// tsconfig.json
{
  "compilerOptions": {
    // this aligns with Vue's browser support
    "target": "es5",
    // this enables stricter inference for data properties on `this`
    "strict": true,
    // if using webpack 2+ or rollup, to leverage tree shaking:
    "module": "es2015",
    "moduleResolution": "node"
  }
}
{% endhighlight javascript %}

Por otro lado, para crear nuestro primer proyecto será necesario solo introducir en consola lo siguiente:

{% highlight shell %}
# 1. Install Vue CLI, if it's not already installed
npm install --global @vue/cli

# 2. Create a new project, then choose the "Manually select features" option
vue create my-project-name
{% endhighlight shell%}

Si quieres practicar con código, te recomiendo que uses [este repositorio](https://github.com/microsoft/TypeScript-Vue-Starter) de iniciación.

**¿Qué lo diferencia de React?**

Lo primero que debemos mencionar es que, en términos de popularidad, actualmente React gana la partida. Esto significa que tendrás más documentación o ejemplos al respecto que te ayudarán a solucionar problemas más rápidamente ya tiene detrás a Facebook respaldando la tecnología además de una gran comunidad, aunque la popularidad de Vue está en constante crecimiento. Por otro lado, en cuanto a tecnología móvil, la que ofrece React es más madura, es decir, si quieres montar un proyecto tanto para web como para mobile la recomendación es React. Otra diferencia significativa es que, aunque ambas tecnologías tienen una arquitectura basada en componentes, en Vue se usa HTML para las plantillas (aunque también puedes usar otros estilos como JSX) en un único documento mientras que en React se utiliza JavaScript.

**Conclusiones**

Como hemos visto Vue propone una gran solución con la premisa de que a veces, *menos, es más*. Vue es una gran herramienta, que te abre grandes posibilidades con muy poco, sobre todo si posees menos experiencia en el desarrollo en la parte del frontend. Por todo lo citado con anterioridad, diría que Vue se ajusta a tus necesidades si tienes un proyecto de pequeña o mediana escala, mientras que React estaría recomendado para grandes proyectos. Este post ha sido bastante introductorio, sin embargo, más adelante habrá una segunda parte con otro ejemplo más avanzado.

Happy coding!




















