project (helloCmake)

cmake_minimum_required(VERSION 2.8)

# Tipo de compilación
set (CMAKE_BUILD_TYPE "Debug")

# Los siguientes flags son independientes del tipo de build.
# Sobreescribo flags de compilación en C++
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra ")
# Sobreescribo flags de C
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra ")

# Archivos de la aplicación
add_executable(hello # Nombre del ejecutable a generar
	hello.cpp # Archivo(s) fuente
)