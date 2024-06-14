# Documentación del Pipeline ETL

## Descripción General
Este pipeline ETL está diseñado para integrar datos de múltiples fuentes, ordenarlos, unirlos y procesarlos antes de guardarlos en un dataset final. Se utilizan varias fuentes de datos, incluidas bases de datos MongoDB y PostgreSQL, así como archivos CSV.

## Estructura del Pipeline
![alt text](image-15.png)

### 1. Input de archivo CSV
**Paso:** CSV file input  
**Descripción:** Este nodo se utiliza para obtener la información de un archivo CSV guardado de forma local llamado “AmesProperty.csv”, la ruta del nodo debe ser cambiada si se corre en otro equipo.
![alt text](image.png)

### 2. Cálculos y reemplazos
**Paso:** Calculator  
**Descripción:** Realiza cálculos sobre las columnas existentes.  
![alt text](image-1.png)

**Paso:** Replace_Year_Remod  
**Descripción:** Reemplaza valores en la columna Year_Remod.

### 3. Ordenar filas
**Paso:** Ordenar filas 5 , Ordenar garages, Ordenar bsmt, Ordenar pools, Ordenar misc
**Descripción:** Ordena las filas de acuerdo a ciertos criterios, en este caso se ordenan de forma ascendente según el valor del "PID".
![alt text](image-2.png)

### 4. Unión por clave
**Paso:** Unión por clave, Unión por clave 2, Unión por clave 3, Unión por clave 4, Unión por clave 5, Unión por clave 6, Unión por clave 7.
**Descripción:** Une los datos del archivo CSV con los datos de MongoDB y PostgreSQL basándose en una clave común, en este caso la clave es "PID".
![alt text](image-3.png)

### 5. Eliminación de IDs
**Paso:** Eliminacion_IDs  
**Descripción:** Elimina las columnas de ID que no son necesarias para el análisis posterior, y otras columnas no requeridas resultado de todas las uniones realizadas en el flujo.
![alt text](image-4.png)

### 6. Relleno de valores nulos
**Paso:** fill null  
**Descripción:** Rellena los valores nulos con un valor predefinido, en este caso todos los registros numéricos con valor nulo serán reemplazados con un "0" y todas las variables categóricas con valor nulo serán reemplazadas por un "NA".
![alt text](image-5.png)

### 7. Renombrar columnas
**Paso:** Rename Columns  
**Descripción:** Renombra las columnas para que tengan nombres más significativos, acordes a la guía propuesta.
![alt text](image-6.png)

### 8. Guardar dataset
**Paso:** Save dataset  
**Descripción:** Guarda el dataset procesado en el formato deseado.

## Detalles de las Fuentes de Datos

### Conexión MongoDB
**Conexión:** Para establecer la conexión a los datasets en MongoDB debemos establecer la “Connection String” con el valor de “mongodb+srv://infrati:infrati@cluster0.v5rih5b.mongodb.net/?retryWrites=true&w=majority” en la base de datos “test”, esto se hace en todos los nodos dispuestos.
![alt text](image-7.png)

### MongoDB Garages
**Paso:** Mongo_garages  
**Descripción:** Carga datos desde la colección 'garages' de MongoDB.
![alt text](image-8.png)

### MongoDB Bsmt
**Paso:** Mongo_bsmt  
**Descripción:** Carga datos desde la colección 'bsmt' de MongoDB.
![alt text](image-9.png)

### MongoDB Pools
**Paso:** Mongo_pools  
**Descripción:** Carga datos desde la colección 'pools' de MongoDB.
![alt text](image-10.png)

### MongoDB Misc
**Paso:** Mongo_misc  
**Descripción:** Carga datos desde la colección 'misc' de MongoDB.
![alt text](image-11.png)

### PostgreSQL Floor Detail
**Paso:** PGSQL_floordetail  
**Descripción:** Carga datos desde la tabla 'floordetail' de PostgreSQL.
![alt text](image-12.png)

### PostgreSQL Ames DB Temp
**Paso:** PgSQL_amesdbtemp  
**Descripción:** Carga datos desde la tabla 'amesdbtemp' de PostgreSQL.
![alt text](image-13.png)

### PostgreSQL Sale Property
**Paso:** PGSQL_SaleProperty  
**Descripción:** Carga datos desde la tabla 'SaleProperty' de PostgreSQL.
![alt text](image-14.png)

## Procesos de Unión
**Paso:** Unión por clave  
**Descripción:** Une diferentes conjuntos de datos a lo largo de múltiples pasos usando claves específicas para asegurar la integridad de los datos.

## Configuración y Consideraciones
- **Orden de ejecución:** El orden de los pasos es crucial para asegurar que los datos se procesen correctamente.
- **Manejo de errores:** Se deben manejar adecuadamente los errores en cada paso para garantizar la robustez del pipeline.
- **Rendimiento:** Asegurarse de que los pasos de ordenación y unión estén optimizados para manejar grandes volúmenes de datos.

## Conclusión
Este pipeline ETL de Pentaho está diseñado para procesar y transformar datos de manera eficiente, integrando múltiples fuentes de datos y asegurando la calidad y consistencia de los datos finales.
