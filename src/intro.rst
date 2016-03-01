Introducción
============

¿Qué es CMake?
--------------

CMake (o CrossMake) es un generador de archivos de compilación *multiplataforma*, ya sea makefiles para sistemas unix o proyectos para entornos de desarrollo como MS Visual Studio, Eclipse, Code::Blocks, etc. Posee una sintaxis sencilla para generar módulos y facilita el uso de bibliotecas dinámicas y estáticas, entre otras de sus utilidades, permitiendo una distribución prolija del código fuente, y el uso de *unit tests*.

Alcance del tutorial
--------------------

Debido a que la documentación oficial utiliza un orden del temario que no considero óptimo, y que la documentation respecto a modularización puede resultar algo confusa, este tutorial tiene como fin enseñar desde realizar una aplicación básica con uno o varios archivos fuente a realizar un script en el que genere uno o varios ejecutables, utilizando bibliotecas estáticas o dinámicas, y pudiendo incorporar alguna suite de testing, como puede ser Googletest. 

Los ejemplos del tutorial fueron escritos en C++ sobre un sistema unix (Ubuntu). Para compilar se utilizó g++ y el comando de consola *cmake* (versión 2.8.12), aunque los scripts son totalmente compatibles con la versión gráfica de la herramienta, *cmake-gui*

Hola CMake
==========

Generar un script del proyecto
------------------------------

Como en cualquier lenguaje nuevo, comenzamos con un sencillo “Hola mundo” para compilar. Se supone que el lector tiene un ambiente de desarrollo con un compilador de C/C++ y la herramienta CMake ya instalados.

Nuestro código a compilar será el siguiente archivo, hello.cpp

Todos los módulos de compilación se configuran en un archivo llamado *CMakeLists.txt*. Para compilarlo escribiremos las siguientes lineas en el archivo CMakeLists:

.. code-block:: cmake

	# Le asigno un nombre al proyecto, opcionalmente le puedo pasar el lenguaje. Todos los archivos CMakeLists deben contener un proyecto
	project (helloCmake) 
	
	# Archivos de la aplicación
	add_executable(hello    # Nombre del ejecutable a generar
		hello.cpp           # Archivo(s) fuente
	)

Generar un script de compilación
--------------------------------

Para generar el script de compilación a partir del archivo CMakeLists, ejecutaremos por consola el siguiente comando, ubicados en el directorio con los archivos de código fuente y de configuración:

.. code-block:: bash

	cmake .

Notese que “.” es el directorio en el que se encuentra el archivo CMakeLists. Es una buena práctica generar una carpeta aparte (por ejemplo “build”), posicionarse dentro de esta carpeta y correr el comando haciendo referencia a la ruta del directorio con el archivo de configuración (por ejemplo, si mis fuentes y configuración está en una carpeta *src*, y construyo una carpeta build a la misma altura que src, debería correr *cmake ../src*)

Una vez generado el script de compilación, ya se puede correr *make* para compilar los archivos fuente. En nuestro caso de prueba, se generará un archivo ejecutable con el nombre *hello*. Cabe destacar que previo a la compilación, se chequea el timestamp del código fuente, para ver si es necesario recompilarlo o si se puede reutilizar el archivo objeto ya existente, acelerando el proceso de compilación.

Definir variables
-----------------

CMake permite definir y sobreescribir *variables* para configurar el script de compilación que se generará.

.. code-block:: cmake

	set (HELLO_VAR "Hola, soy una variable :D")

Las variables declaradas pueden accederse al igual que una variable de bash, utilizando la sintaxis **${<Nombre de la variable>}**. Por ejemplo, si escribimos

.. code-block:: cmake

	set (HELLO_VAR "Hola, soy una variable :D")
	message ("HELLO_VAR contiene: {HELLO_VAR}")

Nuestro script imprimirá por pantalla la linea *"HELLO_VAR contiene: Hola, soy una variable :D"*.

Las variables también se pueden definir mediante la linea de comando, anteponiendo el indicador "-D"

.. code-block:: bash

	cmake . -DFOO_VAR="Soy foo"

Define a la variable *FOO_VAR*. Las variables definidas por linea de comando quedan guardadas junto a otras configuraciones en el archivo *CMakeCache.txt*. 

Definiendo flags de compilación
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

En el siguiente ejemplo se muestra como sobreescribir las variables que definen los flags de compilación.

.. code-block:: cmake

	# Tipo de compilación
	set (CMAKE_BUILD_TYPE “Debug”)
	
	# Los siguientes flags son independientes del tipo de build.
	# Sobreescribo flags de compilación en C++
	set (CMAKE_CXX_FLAGS “${CMAKE_CXX_FLAGS} -Wall -Wextra ")
	
	# Sobreescribo flags de C
	set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra ”)

