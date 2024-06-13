# Documentación del Pipeline ETL

## Descripción General
Este pipeline ETL está diseñado para integrar datos de múltiples fuentes, ordenarlos, unirlos y procesarlos antes de guardarlos en un dataset final. Se utilizan varias fuentes de datos, incluidas bases de datos MongoDB y PostgreSQL, así como archivos CSV.

## Estructura del Pipeline

### 1. Input de archivo CSV
**Paso:** CSV file input  
**Descripción:** Este paso carga datos desde un archivo CSV.

### 2. Cálculos y reemplazos
**Paso:** Calculator  
**Descripción:** Realiza cálculos sobre las columnas existentes.  
**Paso:** Replace_Year_Remod  
**Descripción:** Reemplaza valores en la columna Year_Remod.

### 3. Ordenar filas
**Paso:** Ordenar filas 5  
**Descripción:** Ordena las filas de acuerdo a ciertos criterios.

### 4. Unión por clave
**Paso:** Unión por clave 7  
**Descripción:** Une los datos del archivo CSV con los datos de MongoDB y PostgreSQL basándose en una clave común.

### 5. Eliminación de IDs
**Paso:** Eliminacion_IDs  
**Descripción:** Elimina las columnas de ID que no son necesarias para el análisis posterior.

### 6. Relleno de valores nulos
**Paso:** fill null  
**Descripción:** Rellena los valores nulos con un valor predefinido.

### 7. Renombrar columnas
**Paso:** Rename Columns  
**Descripción:** Renombra las columnas para que tengan nombres más significativos.

### 8. Guardar dataset
**Paso:** Save dataset  
**Descripción:** Guarda el dataset procesado en el formato deseado.

## Detalles de las Fuentes de Datos

### MongoDB Garages
**Paso:** Mongo_garages  
**Descripción:** Carga datos desde la colección 'garages' de MongoDB.

### MongoDB Bsmt
**Paso:** Mongo_bsmt  
**Descripción:** Carga datos desde la colección 'bsmt' de MongoDB.

### MongoDB Pools
**Paso:** Mongo_pools  
**Descripción:** Carga datos desde la colección 'pools' de MongoDB.

### MongoDB Misc
**Paso:** Mongo_misc  
**Descripción:** Carga datos desde la colección 'misc' de MongoDB.

### PostgreSQL Floor Detail
**Paso:** PGSQL_floordetail  
**Descripción:** Carga datos desde la tabla 'floordetail' de PostgreSQL.

### PostgreSQL Ames DB Temp
**Paso:** PgSQL_amesdbtemp  
**Descripción:** Carga datos desde la tabla 'amesdbtemp' de PostgreSQL.

### PostgreSQL Sale Property
**Paso:** PGSQL_SaleProperty  
**Descripción:** Carga datos desde la tabla 'SaleProperty' de PostgreSQL.

## Procesos de Unión
**Paso:** Unión por clave  
**Descripción:** Une diferentes conjuntos de datos a lo largo de múltiples pasos usando claves específicas para asegurar la integridad de los datos.

## Configuración y Consideraciones
- **Orden de ejecución:** El orden de los pasos es crucial para asegurar que los datos se procesen correctamente.
- **Manejo de errores:** Se deben manejar adecuadamente los errores en cada paso para garantizar la robustez del pipeline.
- **Rendimiento:** Asegurarse de que los pasos de ordenación y unión estén optimizados para manejar grandes volúmenes de datos.

## Conclusión
Este pipeline ETL de Pentaho está diseñado para procesar y transformar datos de manera eficiente, integrando múltiples fuentes de datos y asegurando la calidad y consistencia de los datos finales.
