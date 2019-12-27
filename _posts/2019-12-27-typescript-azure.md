---
layout: post
title: Trabajando con Azure Functions y TypeScript
---

[Azure Functions](https://azure.microsoft.com/es-es/services/functions/) es una solución de Microsoft para el procesamiento de datos, APIS y microservicios de computación sin servidor. Posee una serie de ventajas y características a tener en cuenta:
- Ofrece una escalabilidad flexible basado en el volumen de trabajo.
- Cuenta con un modelo de programación integrado basado en desencadenadores y enlaces que permiten conectarse a diferentes eventos.
- Un repertorio de herramientas DevOps integradas, además de una experiencia de programación End-to-End que conduce desde el desarrollo y compilación hasta la depuración e integración continua además de la entrega continua (CI/CD).

Se basa en una arquitectura Serverless basada en microservicios (“Funciones como servicio”), por lo que recomendable es dividir las funciones, que se regularán en función de la demanda. Por lo que es auto-escalado, pagamos por servicio y no nos obliga a gestionar la infraestructura.

**Requisitos**

Para hacer una demostración de cómo funciona Azure Functions con TypeScript y familiarizarnos con el entorno, vamos a necesitar las siguientes herramientas:
* Visual Studio Code.
* Extensión de [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para VS Code.
* Tener instalado [Nodejs](https://nodejs.org/en/download/)

Posteriormente, debemos tener instalada la versión correspondiente de Azure Functions Core Tools que nos permitirá trabajar en local, conectarnos en nuestra cuenta de Azure y, además, desplegar nuestros function projects.

Para ello, en MacOS utilizaremos el siguiente comando:

{% highlight shell %}
brew tap azure/functions
brew install azure-functions-core-tools@3
{% endhighlight %}

**Creando un Functions project en local**

El procedimiento para crear nuestro proyecto en local es bastante intuitivo, solo tendremos que ejecutar el siguiente comando:

{% highlight shell %}
func init MyFunctionDemo
{% endhighlight %}

Y seleccionar qué tipo de proyecto queremos crear, el asistente nos ofrecerá las siguientes opciones:

{% highlight shell %}
Select a worker runtime:
1. dotnet
2. node
3. python
4. powershell
Choose option: 2
node
Select a Language:
1. javascript
2. typescript
Choose option: 2
typescript
{% endhighlight %}

Ello te generará un archivo package.json y tsconfig.json que te ayudarán a transcompilar todo el proyecto a JavaScript. Para comprobar que todo ha sido instalado correctamente debemos ejecutar el siguiente comando el el directorio donde tengamos el proyecto:

{% highlight shell %}
func start
{% endhighlight %}

Donde podremos comprobar que el servicio está corriendo en la dirección: http://0.0.0.0:7071

Para crear nuestro primer ejemplo usaremos el comando: 

{% highlight shell %}
func new
{% endhighlight %}

Se seleccionamos la octava opción: 8. HTTP trigger, nos generará un archivo index.ts con el siguiente código:

{% highlight javascript %}
import { AzureFunction, Context, HttpRequest } from "@azure/functions"

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
    context.log('HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));

    if (name) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};

export default httpTrigger;
{% endhighlight %}

Si ejecutamos mediante consola los siguientes comandos:

{% highlight bash %}
npm install
npm start 
{% endhighlight %}

Debería darnos el siguiente output:

{% highlight bash %}
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
{% endhighlight %}

De esta forma, podremos comprobar que la aplicación está perfectamente desplegada haciendo una petición a:

{% highlight bash %}
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Mola
{% endhighlight %}

**Desplegar la aplicación en Azure**

Por último, para desplegar nuestra aplicación en Azure, debemos utilizar el siguiente comando:

{% highlight bash %}
func azure functionapp publish <FunctionAppName>
{% endhighlight %}

Este comando desplegará automáticamente tu Function en tu cuenta. Incluso podríamos configurar el entorno para desplegar nuestra aplicación en un [entorno de staging](https://docs.microsoft.com/en-us/azure/app-service/deploy-staging-slots) o usar Circle-CI y JEST comprobar que todo está correcto antes de hacer push al branch de producción.

Hasta aquí esta primera toma de contacto con TypeScript y Azure, sin duda habrá más posts similares en un futuro.

Happy coding!

