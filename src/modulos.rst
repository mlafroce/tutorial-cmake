Módulos
=======

CMake permite dividir nuestro proyecto en módulos de compilación. Estos módulos pueden agrupar bibliotecas estáticas, dinámicas, y binarios ejecutables.

Agregar subdirectorios
----------------------

Para agregar un subdirectorio, simplemente se tiene que utilizar el comando *add_subdirectory*

Por ejemplo, si tuvieramos un proyecto con una aplicación cliente y otra servidor, podríamos dividir el proyecto de la siguiente forma:

.. code-block:: cmake

	project(cli-srv-foo) # Nombre de mi proyecto
	cmake_minimum_required(VERSION 2.8)
	
	# Modulos a compilar #
	
	add_subdirectory(common) # Archivos compartidos entre cliente y servidor
	add_subdirectory(client) # Archivos del cliente
	add_subdirectory(server) # Archivos del servidor

Al ejecutar el script, CMake buscará un archivo CMakeLists.txt en los directorios *common*, *client* y *server* relativos a la ubicación del script.

Compilar distintos binarios
---------------------------

Un archivo CMakeLists puede producir distintos binarios o *targets*.

Compilar un ejecutable
^^^^^^^^^^^^^^^^^^^^^^

Para compilar un ejecutable, se utiliza la instrucción *add_executable*.
Primero se escribe el nombre que tendrá nuestro ejecutable (en el ejemplo, "server"), y luego se nombran los .cpp con el código a compilar, con la ruta relativa al script.
Recordar que entre los .cpp, debe haber una y solo una definición método "main". En nuestro ejemplo se encontraría en el archivo main.cpp.

.. code-block:: cmake

	add_executable(server
		main.cpp
	)

Si mi código contara con más archivos .cpp

.. code-block:: cmake

	add_executable(server
		main.cpp
		miClase.cpp
		paquete/otraClase.cpp # Admite navegación por subdirectorios.
	)


Compilar una biblioteca
^^^^^^^^^^^^^^^^^^^^^^^

Para compilar una biblioteca, se utiliza la instrucción *add_library*.
A diferencia de *add_executable*, esta instrucción genera una biblioteca con código objeto ya compilado.
La biblioteca no es ejecutable, pero puede *linkearse* a un ejecutable que quiera reutilizar su código.

.. code-block:: cmake

	# Estas clases podrán ser utilizadas en más de un módulo
	add_library(common
		serviceInfo.cpp
	)

Si se quiere que la biblioteca sea *dinámica* (un archivo .dll o .so), se debe agregar la palabra *SHARED* luego del nombre de la biblioteca.

.. code-block:: cmake

	# Estas clases podrán ser utilizadas en más de un módulo
	add_library(common SHARED
		serviceInfo.cpp
	)

