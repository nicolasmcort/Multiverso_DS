# Multiverso_DS

<br>
<img src="https://www.pngkey.com/png/detail/268-2688228_universidad-nacional-colombia-logo.png" width="230" alt="Logo Universidad Nacional de Colombia">
<br>
UNIVERSIDAD NACIONAL DE COLOMBIA 
<br>
Estructuras de Datos
<br>
2016699
<br><br>
Autores: 

- Manuel Federico Castro Suárez (mcastrosu@unal.edu.co)
- Ever Nicolás Muñoz Cortés (evmunoz@unal.edu.co)
- Brayan Camilo Gamba Herrera (bgambah@unal.edu.co)

Docente: Jhon Alexander López Fajardo 
<br><br>

---

## Contenido

- [1. Estructura del Proyecto](#1-estructura-del-proyecto)
  - [1.1 Objetivo y Estructura](#11-objetivo-y-estructura)
  - [1.2 Layout del Proyecto](#12-layout-del-proyecto)
- [2. Ejecución Local](#2-ejecución-local)
- [3. Instrucciones de Uso](#3-instrucciones-de-uso)
- [4. Ejemplos de Uso](#4-ejemplos-de-uso)

    
<br><br>

---


## 1. Estructura del Proyecto
---
#### 1.1 Objetivo y Estructura
- Objetivo: Crear una estructura de datos en la que se realicen operaciones básicas sobre nodos (inserciones, eliminaciones, etc), también llamadaos universos, además de búsquedas y recorridos sobre la misma.
- Estructura: Los nodos van a formar una [Zeolita Axial](https://www.pngwing.com/en/free-png-ydyqk), en la cual cada uno tiene máximo 6 punteros hacia otros nodos. Cada nodo va a tener datos random de una lista extensa sobre referencias del curso de Estructuras de Datos





#### 1.2 Layout del Proyecto

``` txt
.
├───chemestry/
│   │
│   ├───dataset_1_1GB.csv                      # Dataset de 1.1GB (parte del dataset original)
│   ├───definitions.h                          # Archivo de cabecera con definiciones de estructuras (ej: SHM, estructuras de datos), constantes, etc.
│   ├───index_builder.c                        # Código para el programa que crea/actualiza la tabla hash (hash_index.bin).
│   ├───main.c                                 # Código del proceso principal. Encargado de la lógica de procesos (SHM, fork, wait, etc), y orquestación.
│   ├───MurmurHash2.c                          # Implementación de la función hash MurmurHash2.
│   ├───ui_process.c                           # Código del proceso UI. Gestiona la entrada/salida del usuario.
│   └───worker_process.c                       # Código del proceso Worker. Contiene la lógica principal de búsqueda en el archivo indexado.
```

<br><br>

---

## 2. Ejecución Local
Comandos para ejecutar el programa en la terminal de Linux:
``` txt
// Crear el archivo binario 
gcc index_builder.c MurmurHash2.c -o index_builder
./index_builder 

// Realizar las búsquedas 
gcc main.c worker_process.c ui_process.c MurmurHash2.c -o main  
./main
```

<br><br>

---

## 3. Instrucciones de Uso
El proceso ui_process presentará el siguiente menú. Debes ingresar el número de la opción deseada (1, 2, o 3).
```txt
--- Sistema de búsqueda de moléculas ---
1. Ingresar criterio de búsqueda (product_smiles):
   Clave actual: [VACÍO]
2. Realizar búsqueda
3. Salir
Selección:
```
- Selecciona la opción 1 e ingresa la cadena completa del `product_smiles`. Este valor se almacena como la clave actual.
- Selecciona la opción 2. El proceso UI envía la clave actual al proceso Worker. El Worker realiza la búsqueda indexada y devuelve el registro.
- Selecciona la opción 3 para terminar el sistema.
  
<br><br>

---

## 4. Ejemplos de Uso

> **IMPORTANTE:** Para fines de desarrollo, testing y gestión eficiente de recursos en el entorno local, la implementación utiliza un subconjunto (1.1 GB) del dataset original (1.8 GB).

### Ejemplo 1: 
product_smiles = O(C(=O)C2(CC1=CC=CC=C1)CCC2)CC3=C[NH]C(=N3)CC4=CC=CC=C4
``` txt
#MAIN -> Memoria Compartida creada y adjunta.
#WORKER -> Tabla hash cargada en RAM (0.023 MB). Esperando comandos...
#UI -> Interfaz iniciada y conectada a Worker.

--- Sistema de búsqueda de moléculas ---
1. Ingresar criterio de búsqueda (product_smiles):
   Clave actual: [VACÍO]
2. Realizar búsqueda
3. Salir
Selección: 1
Ingrese product_smiles (ej: O(C(=O)C2(CC1...)): O(C(=O)C2(CC1=CC=CC=C1)CCC2)CC3=C[NH]C(=N3)CC4=CC=CC=C4

--- Sistema de búsqueda de moléculas ---
1. Ingresar criterio de búsqueda (product_smiles):
   Clave actual: O(C(=O)C2(CC1=CC=CC=C1)CCC2)CC3=C[NH]C(=N3)CC4=CC=CC=C4
2. Realizar búsqueda
3. Salir
Selección: 2

Buscando 'O(C(=O)C2(CC1=CC=CC=C1)CCC2)CC3=C[NH]C(=N3)CC4=CC=CC=C4'. Esperando resultado del Worker...
#WORKER -> Buscando: 'O(C(=O)C2(CC1=CC=CC=C1)CCC2)CC3=C[NH]C(=N3)CC4=CC=CC=C4'...
#WORKER -> Búsqueda finalizada en 0.0000780 segundos.
Coincidencia encontrada:
"""4E583CDF5D62B693_2F298919F17BFE25_6031_UN""","""4E583CDF5D62B693""","""O(C(=O)C2(CC1=CC=CC=C1)CCC2)CC3=C[NH]C(=N3)CC4=CC=CC=C4""","""C23H24N2O2""",360.4548,8,1,3,478.77,0.6795,4,27,4,4,4,3,1,0,"""""",6031,"""2BFCE7D1BAF62175""","""OC(=O)C2(CC1=CC=CC=C1)CCC2""","""04D56EC84B8DDF50""","""OCC1=C[NH]C(=N1)CC2=CC=CC=C2"""

Búsqueda finalizada.

--- Sistema de búsqueda de moléculas ---
1. Ingresar criterio de búsqueda (product_smiles):
   Clave actual: O(C(=O)C2(CC1=CC=CC=C1)CCC2)CC3=C[NH]C(=N3)CC4=CC=CC=C4
2. Realizar búsqueda
3. Salir
Selección: 3
Comando de salida enviado al Worker. Terminando...
#WORKER -> Comando EXIT recibido. Terminando...
#MAIN -> Programa finalizado y recursos liberados.
#MAIN -> Desmontando y eliminando memoria compartida de mensajes...
```
---
