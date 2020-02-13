Este proyecto esta realizado con **React** + **GrapQL** + **Django**.
A continuacion se presenta un manual para configurar tu entorno de desarrollo desde cero.
>_**Para gestionar los paquetes de Python, en este proyecto usamos [Anaconda](https://www.anaconda.com/).**_
_**Para poder instalar React, en este proyecto usamos [Nodejs](https://nodejs.org/es/).**_
_**Para gestionar los paquetes de Nodejs, en este proyecto usamos [npm](https://www.npmjs.com/).**_

# 1. Configuracion directorio `Django`
## 1.1 Entorno virtual
Lo primero que haremos es crear un entorno virtual utilizando **`Anaconda`**, en este entonrno instalaremos nuestros paquetes de forma local, y no de manera global en nuestra PC.
> A nuestro entorno lo llamaremos _`djangoenv`_, sientete libre de cambiarlo por el nombre que quieras.

Ejecutamos el siguiente comando:
``` sh
conda create --name djangoenv python = 3.8
```
Con el siguiente comando activamos nuestro entorno para trabajar sobre él:
``` bash
conda activate djangoenv
```
Y cuando quiera desactivarlo:
``` bash
conda deactivate
```
## 1.2 Instalación de **`Django`**
Dentro de nuestro nuevo entorno virtual, instalamos _`django`_ usando el siguiente comando:
``` sh
conda install -c conda-forge django
```
## 1.3 Directorio de trabajo
A diferencia de otros frameworks como Laravel, el directorio de nuestro proyecto Django no necesita estár en la carpeta `www/` de nuestro servidor **Apache**, puede crear un proyecto **Django** en el directorio que quiera, con el comando `cd` navegamos al directorio elegido:
``` bash
cd Documentos/www/
```
Creamos y accedemos a la carpeta  que va a contener todo nuestro proyecto:
``` bash
mkdir projectRGD
cd projectRGD/
```
## 1.4 Comando `git init`
En la carpeta actual escribiremos el siguiente comando, el cual nos permitira hacer un control de versiones en **`git`** de nuestro proyecto:
``` sh
git init
```
Luego manualmente creamos los archivos **`.gitignore`** y **`readme.md`**, nuestro directorio debe verse asi:
``` diff
projectRGD/
+	/* .gitignore
+	/* readme.md
```
## 1.5 Creacion del proyecto **`Django`**
Con el siguiente comando creamos un proyecto **`django`** al cual llamaremos _`project_start`_, sientete libre de llamarlo de otra forma:
``` bash
django-admin startproject project_start
```
Esto crea en el directorio una nueva carpeta llamada _`project_start`_ que contiene varios subcarpetas y archivos, se vee asi:
``` diff
projectRGD/
+	project_start/
+		project_start/
+			/* __init__.py
+			/* asgi.py
+			/* settings.py
+			/* urls.py
+			/* wsgi.py
+		/* manage.py
	/* .gitignore
	/* readme.md
```
Podra notar que hay dos carpetas llamadas _`project_start`_, la carpeta padre contendrá todas las aplicaciones de nuestro proyecto, la carpeta hijo contiene archivos de configuracion general de nuestro proyecto, si desea puede modificar el nombre de la carpeta padre, pero no modifique el nombre de la carpeta hijo, eso generaria errores.

 Con el comando _`cd`_ accedemos a la carpeta _`project_start`_ padre:
``` bash
cd project_start
```
> Recuerda que `project_start` es solo el nombre del proyecto, sientete libre de elegir el que quieras.

Ahora vemos el siguiente contenido en el directorio actual:
``` diff
project_start/
	project_start/
		/* __init__.py
		/* asgi.py
		/* settings.py
		/* urls.py
		/* wsgi.py
	/* manage.py
```
> En este punto ya podemos probar el proyecto con un **servidor de desarrollo** proporcionado por **Django**, para eso usamos en siguiente comando: **`python manage.py runserver`**.
## 1.6 Creacion de una aplicación
> ### Aplicacion vs Proyecto
> Un proyecto puede contener una o mas aplicaciones, las aplicaciones son pequeñas partes del software que esta diseñada para un uso especifico.

Crearemos una aplicacion que contendra nuestros directorios de **`React`**, la llamaremos _`frontend`_, usaremos el siguiente comando:
``` bash
python manage.py startapp frontend
```
Este comando creo los siguientes directorios y archivos:
``` diff
project_start/
+	frontend/
+		migrations/
+			/* __init__.py
+		/* __init__.py
+		/* admin.py
+		/* apps.py
+		/* models.py
+		/* test.py
+		/* views.py
	project_start/
		/* __init__.py
		/* asgi.py
		/* settings.py
		/* urls.py
		/* wsgi.py
	/* manage.py
```
# 2. Configuracion directorio `React`
## 2.1 Iniciando
Accedemos a la carpeta _`frontend`_, que es el directorio donde trabajaremos con **`React`**:
``` bash
cd frontend/
```
Iniciamos el proyecto con el siguiente comando:
``` bash
npm init
```
Manualmente creamos un archivo _`.gitignore`_.
Luego creamos las siguentes carpetas:
``` bash
mkdir templates/
mkdir templates/frontend/
mkdir static/
mkdir static/frontend/
mkdir src/
```
>La carpeta _`template/frontend/`_ es la carpeta predeterminada de **`Django`** para los templates, y es donde ubicaremos nuestro _`index.html`_ que usara nuestra aplicacion **`React`**. La carpeta _`static/frontend/`_ es la carpeta predeterminada de **`Django`** para los archivos estaticos(imagenes, scripts, hojas de estilo). La carpeta _`src/`_ contendra nuestro codigo **`.jsx`**.

Creamos un archivo **`.html`** en _`template/frontend/`_ y le agregamos el siguiente codigo:
``` html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title>React Starter</title>
</head>
<body>
  <div id="root"></div>
  <script src="../dist/bundle.js"></script>
</body>
</html>
```
Puedes nombrer tu script como quieras, en este tutorial usaremos _`bundle`_.

## 2.2 Babel
En el terminal ejecuta el siguiente codigo:
``` bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/preset-react
```
Ahora cree un archivo llamado _`babel.config.json`_.
Aqui le decimos a **`Babel`** que vamos a usar preajustes **`env`** y **`react`**. Dentro del archivo escribimos: 
``` json
{
  "presets": ["@babel/env", "@babel/preset-react"]
}
```
## 2.3 Webpack
En el terminal ejecuta el siguiente codigo:
``` bash
npm install --save-dev webpack webpack-cli webpack-dev-server style-loader css-loader babel-loader
```
Cree un nuevo archivo llamado _`webpack.config.js`_. Este archivo exporta un objeto con la configuracion de webpack. En al archivo escribimos lo siguiente:
``` js
const path = require("path");
const webpack = require("webpack");

module.exports = {
  entry: "./src/index.js",
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /(node_modules|bower_components)/,
        loader: "babel-loader",
        options: { presets: ["@babel/env"] }
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  resolve: { extensions: ["*", ".js", ".jsx"] },
  output: {
    path: path.resolve(__dirname, "dist/"),
    publicPath: "/dist/",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: path.join(__dirname, "public/"),
    port: 3000,
    publicPath: "http://localhost:3000/dist/",
    hotOnly: true
  },
  plugins: [new webpack.HotModuleReplacementPlugin()]
};
```
## 2.4 React
En el terminal ejecuta el siguiente codigo:
``` bash
npm install --save react react-dom
```
Cree un archivo index.js en la carpeta src y escriba el siguiente codigo:

``` js
import React from "react";
import ReactDOM from "react-dom";
import App from "./App.js";
ReactDOM.render(<App />, document.getElementById("root"));
```





















