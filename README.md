# Analisis de datos proteomicos PRIDE
## Bioinformatica – ULPGC
Autores: Adrian Ojeda y Raul Mendoza

Este repositorio contiene un cuaderno de Jupyter para resolver paso a paso los ejercicios de proteomica del Bloque 1 de Bioinformatica (ULPGC), usando datos publicos de PRIDE (salida de MaxQuant).

El objetivo es trabajar como en una practica de laboratorio de informatica: cargar un fichero de peptidos, explorar las columnas mas importantes y aplicar analisis basicos de identificacion y cuantificacion de peptidos y proteinas.

## Contenido

- `Proteomica_Ejercicios_Adrian_Ojeda_Raul_Mendoza.ipynb`  
  Cuaderno principal con todo el flujo de trabajo:
  - Carga del fichero de peptidos exportado de MaxQuant (peptides_esenciales.txt).
  - Localizacion de las columnas clave (`Sequence`, `Proteins`, `Gene names`, `PEP` e intensidades LFQ).
  - Resolucion de cada ejercicio con explicaciones detalladas en tono de alumno de grado.

- `peptides_esenciales.txt` (no se incluye aqui)  
  Fichero que el estudiante debe preparar a partir del `peptides.txt` original descargado de PRIDE.

  Este fichero debe contener, como minimo, las siguientes columnas:
  - `Sequence`
  - `Proteins`
  - `Gene names`
  - `PEP`
  - Todas las columnas de intensidad, por ejemplo:
    - `LFQ intensity A1`
    - `LFQ intensity A2`
    - ...
    - `LFQ intensity A10` (y mas, si existen)

## Preparacion de los datos (resumen)

1. Descargar de PRIDE un dataset de proteomica cuantitativa procesado con MaxQuant.
2. Obtener el fichero `peptides.txt` de resultados.
3. Abrirlo con una hoja de calculo o con R/Python y quedarse solo con las columnas esenciales:
   - `Sequence`
   - `Proteins`
   - `Gene names`
   - `PEP`
   - Todas las columnas de intensidades LFQ que se van a usar.
4. Guardar el resultado como `peptides_esenciales.txt` en formato tabulado (separado por tabuladores).

Este fichero `peptides_esenciales.txt` debe estar en la misma carpeta que el cuaderno de Jupyter.

## Ejercicios cubiertos en el cuaderno

El cuaderno resuelve de forma programatica (y comentada) los ejercicios propuestos en la practica:

1. **Identificacion de un peptido concreto**
   - Seleccion de un peptido por indice de fila.
   - Extraccion de su `Sequence`, `Proteins` y `Gene names`.
   - Comentario biologico sobre el hecho de que se identifican peptidos y no proteinas directamente.

2. **Evaluacion de la confianza de la identificacion (PEP)**
   - Lectura del valor `PEP` del peptido seleccionado.
   - Clasificacion basica:
     - PEP < 0.01: identificacion confiable (alta confianza).
     - PEP entre 0.01 y 0.05: confianza moderada.
     - PEP > 0.05: identificacion poco confiable.
   - Explicacion de lo que significa PEP como probabilidad posterior de error.

3. **Comparacion de intensidades entre muestras para un peptido**
   - Comparacion de las intensidades LFQ del mismo peptido en dos muestras, por ejemplo:
     - `LFQ intensity A1`
     - `LFQ intensity A10`
   - Deteccion de ausencia (0 o NaN) en alguna de las muestras.
   - Comentario sobre que un valor 0 o NaN suele indicar no deteccion o estar por debajo del limite de deteccion.

4. **Identificacion de peptidos con valores faltantes**
   - Busqueda de peptidos con al menos un valor 0 o NaN en las columnas de intensidades LFQ.
   - Discusion sobre las posibles causas:
     - No expresion real de la proteina en esa condicion.
     - Peptido por debajo del limite de deteccion del instrumento.
   - Comentario sobre la dificultad de tratar valores faltantes (MNAR) en proteomica.

5. **Comparacion basada en proteinas (o genes)**
   - Seleccion de un gen a partir del peptido del ejercicio 1.
   - Filtrado de todos los peptidos asociados a ese gen (`Gene names`).
   - Definicion de dos grupos de muestras, por ejemplo:
     - Grupo A: `A1`–`A10`
     - Grupo B: `A11`–`A20`
   - Calculo de la media de intensidades (ignorando ceros) por peptido en cada grupo.
   - Calculo de la media global por proteina en cada grupo y breve interpretacion:
     - En que grupo parece mas abundante la proteina.

6. **Razonamiento sobre el numero de peptidos por proteina**
   - Explicacion de por que es importante identificar mas de un peptido por proteina:
     - Aumenta la confianza en la identificacion.
     - Reduce la probabilidad de falsos positivos.
     - Mejora la cuantificacion a nivel de proteina.

7. **Comparacion estadistica entre dos grupos (t-test por peptido)**
   - Definicion de dos grupos de muestras, por ejemplo:
     - Grupo 1: `A1`–`A5`
     - Grupo 2: `A6`–`A10`
   - Preparacion de los datos:
     - Sustituir 0 por NaN.
     - Aplicar log2 a las intensidades.
   - Calculo de la media log2 por peptido en cada grupo.
   - Aplicacion de un t-test de Student (independiente) para comparar los grupos por peptido.
   - Obtencion de p-values y seleccion de peptidos con p < 0.05.
   - Comentarios sobre posibles extensiones (FDR, volcano plots, etc.), aunque no se implementan en detalle.

## Requisitos

Para ejecutar el cuaderno se necesitan las siguientes herramientas:

- Python 3.x
- Jupyter Notebook o JupyterLab
- Paquetes de Python:
  - `pandas`
  - `numpy`
  - `scipy`

Se pueden instalar con:

```bash
pip install pandas numpy scipy
```

## Como ejecutar el cuaderno

1. Clonar o descargar este repositorio en tu maquina.
2. Copiar el fichero `peptides_esenciales.txt` a la misma carpeta donde esta el cuaderno `Proteomica_Ejercicios_Adrian_Ojeda_Raul_Mendoza.ipynb`.
3. Abrir una terminal en esa carpeta y lanzar Jupyter:

```bash
jupyter notebook
```

4. Abrir el cuaderno `Proteomica_Ejercicios_Adrian_Ojeda_Raul_Mendoza.ipynb`.
5. Ejecutar las celdas en orden (Kernel -> Restart & Run All) o una por una.

Si el nombre del fichero de entrada es diferente, modificar la variable:

```python
ruta_fichero = "peptides_esenciales.txt"
```

en la primera celda de codigo donde se hace `read_csv`.

## Notas finales

- Todo el codigo esta comentado con un tono de alumno de grado, explicando que hace cada paso y como se interpreta biologicamente.
- El objetivo principal no es solo obtener resultados numericos, sino entender mejor como se pasa de un fichero de peptidos de PRIDE a conclusiones biologicas sobre presencia y abundancia de proteinas.
