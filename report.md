# Guía técnica para realizar proyecto ETL

## Tecnologías usadas:
- **Pentaho DI**: Pentaho Data Integration (PDI), también conocido como Kettle, es una herramienta de software de integración de datos que permite a los usuarios diseñar, implementar y mantener flujos de trabajo de integración de datos. PDI se utiliza para extraer, transformar y cargar (ETL) datos desde diversas fuentes a destinos, facilitando la limpieza, transformación y consolidación de datos.
![alt text](images_intro/image.png)
- **ElephantSQL**: ElephantSQL es un servicio de base de datos PostgreSQL basado en la nube que proporciona gestión y alojamiento de bases de datos PostgreSQL. Está diseñado para ser fácil de usar, escalable y seguro, ofreciendo capacidades como copias de seguridad automáticas, recuperación ante desastres y gestión de usuarios.
![alt text](images_intro/image-1.png)
- **MongoDB**: MongoDB es una base de datos NoSQL de código abierto orientada a documentos. En lugar de almacenar datos en tablas y filas como en las bases de datos relacionales, MongoDB almacena datos en documentos BSON (una versión binaria de JSON).
![alt text](images_intro/image-2.png)
- **CSV**: CSV es un formato de archivo simple y comúnmente utilizado para almacenar datos tabulares como en una hoja de cálculo o una base de datos. Los archivos CSV utilizan comas para separar los valores de las columnas y suelen tener una fila de encabezado que indica los nombres de las columnas.
![alt text](images_intro/image-3.png)
## Enunciado proyecto ETL:
Implementar un proceso ETL con Pentaho Data Integration (PDI) para generar un archivo CSV. Este proceso implica la extracción y transformación de datos provenientes de la base de datos relacional BD Ames, archivos CSV y MongoDB.

### Fuentes de datos:
| Fuentes                | Descripción                                                                                             |
|------------------------|---------------------------------------------------------------------------------------------------------|
| Archivo Ames Property.csv | Información general de la propiedad. (19 campos): 2930 registros PID Lot Frontage Lot Area Street Alley Lot Shape Land Contour Utilities Lot Config Land Slope Neighborhood Condition 1 Condition 2 Bldg Type House Style Overall Qual Overall Cond Year Built Year Remod/Add |
| BD Relacional          | Detalles relacionados con la construcción: PID, MS SubClass, MS Zoning, Roof Style, Roof Matl, Exterior 1st, Exterior 2nd, Mas Vnr Type, Mas Vnr Area, Exter Qual, Exter Cond, Foundation, Heating, Heating QC, Central Air, Electrical, 1st Flr SF, 2nd Flr SF, Low Qual Fin SF, Kitchen AbvGr, Kitchen Qual, TotRms AbvGrd, Functional, Fireplaces, Fireplace Qu, Paved Drive, Wood Deck SF, Open Porch SF, Enclosed Porch, 3Ssn Porch, Screen Porch, Fence |
|                        | **MSSubClass** (2 columnas): 16 filas Clase de construcción: code, description                           |
|                        | **MSZoning** (3 columnas): 8 filas id, code, description                                                |
|                        | **TypeQuality** (3 columnas): 5 filas Tipos de calidad: id, code, description                           |
|                        | **SaleProperties** (5 columnas): 2930 filas PID, Sale Date, Sale Type, Sale Condition, SalePrice        |
|                        | **FloorDetail** (5 columnas): 4182 filas PID, Floor, Bedroom, Full Bath, Half Bath                      |
| MongoDB Atlas          | Colecciones:                                                                                             |
|                        | **garage** (8 campos): 2773 documentos PID, Garage Type, Garage Yr Blt, Garage Finish, Garage Cars, Garage Area, Garage Qual, Garage Cond |
|                        | **pool** (3 campos): 13 documentos PID, Pool Area, Pool QC                                              |
|                        | **bsmt** (12 campos): 2851 documentos PID, Bsmt Qual, Bsmt Cond, Bsmt Exposure, BsmtFin Type 1, BsmtFin SF 1, BsmtFin Type 2, BsmtFin SF 2, Bsmt Unf SF, Total Bsmt SF, Bsmt Full Bath, Bsmt Half Bath |
|                        | **misc** (3 campos): 106 documentos PID, Misc Feature, Misc Val                                         |


### Flujo de datos:
Desde las fuentes hacia Pentaho DI y luego hacia el archivo transformado. 
![alt text](images_intro/image-4.png)
Para la implementación del proyecto ETL es necesario utilizar diversas fuentes de datos cada una alojada en diferentes sistemas de bases de datos con el objetivo de simular un escenario más realista. El proceso de extracción, transformación y carga (ETL) comenzará con la creación de una instancia y una base de datos en ElephantSQL. Posteriormente se procederá a la creación de las tablas que contengan la información y/o datos necesarios para realizar las transformaciones requeridas.

### Procedimiento técnico:

#### Creación de instancia y Base de datos ElephantSQL:
1. **Iniciar Sesión en ElephantSQL**: Visita el sitio web de ElephantSQL (https://www.elephantsql.com/) e inicia sesión.
![alt text](images_intro/image-5.png)
2. **Crear una Nueva Instancia**: Haz clic en "Create New Instance" para comenzar el proceso de creación de una nueva instancia.
![alt text](images_intro/image-6.png)
3. **Configurar la Instancia**: Configura los detalles de tu instancia. Elige un nombre para tu instancia, selecciona el plan de precios y la región. Para la realización de este proyecto se hará uso del plan Tiny Turtle Gratuito.
![alt text](images_intro/image-7.png)
4. **Confirmar y Crear la Instancia**: Revisa la configuración y haz clic en "Create Instance" para finalizar la creación.
5. **Acceder a los Detalles de la Instancia**: Accede a los detalles de tu nueva instancia haciendo clic en su nombre en el panel de control.
![alt text](images_intro/image-8.png)
![alt text](images_intro/image-9.png)
6. **Verificar la Información de Conexión**: Revisa la información de conexión que utilizarás para conectar tu base de datos PostgreSQL.
![alt text](images_intro/image-10.png)
7. **Acceder al Navegador de SQL**: Haz clic en "Browser" para acceder al navegador SQL y ejecutar código SQL.
![alt text](images_intro/image-11.png)
#### Conexión a Base de Datos alojada en Mongo:
1. **Descarga e instala MongoDB Compass**: Si aún no lo tienes, descarga e instala MongoDB Compass desde el sitio web oficial de MongoDB. (https://www.mongodb.com/products/tools/compass)
![alt text](images_intro/image-12.png)
2. **Abre MongoDB Compass**: Una vez instalado, abre MongoDB Compass en tu computadora.
![alt text](images_intro/image-13.png)
3. **Conexión a un servidor**: En la pantalla de inicio de MongoDB Compass, haz clic en el botón “New Connection”.
![alt text](images_intro/image-14.png)
4. **Configura la conexión**: Ingresa la información requerida para conectarte a tu servidor de MongoDB.
![alt text](images_intro/image-15.png)
5. **Conexión y exploración de la base de datos**: Haz clic en "Connect" para establecer la conexión con el servidor de MongoDB y explora la base de datos.
![alt text](images_intro/image-16.png)
#### Descarga Pentaho DI:
1. **Instalar Pentaho Data Integration**: Necesitarás Java Development Kit (JDK) versión 8 o superior instalado en tu sistema y configurado correctamente. (https://www.oracle.com/java/technologies/downloads/#java11).
![alt text](images_intro/image-17.png)
2. **Descarga e instalación del JDK**: Ejecuta el instalador de Java Development Kit 11 para poder instalar Pentaho Data Integration.
![alt text](images_intro/image-18.png)
3. **Descarga Pentaho DI**: Si aún no lo tienes, descarga e instala Pentaho DI desde el sitio web oficial (https://pentaho.com/pentaho-community-edition/).
![alt text](images_intro/image-19.png)
4. **Instalación y Uso de Pentaho DI**: Luego de descargar el archivo comprimido de Pentaho DI, extráelo y accede a la carpeta `data-integration`. Ejecuta el archivo por lotes `Spoon.bat` para iniciar Pentaho DI.

![alt text](images_intro/image-20.png)

## Descripción General pipeline
Este pipeline ETL está diseñado para integrar datos de múltiples fuentes, ordenarlos, unirlos y procesarlos antes de guardarlos en un dataset final. Se utilizan las fuentes de datos mencionadas anteriormente, que incluye bases de datos MongoDB y PostgreSQL, así como archivo CSV.

## Estructura del Pipeline
![alt text](images/image-15.png)

### 1. Input de archivo CSV
**Paso:** CSV file input  
**Descripción:** Este nodo se utiliza para obtener la información de un archivo CSV guardado de forma local llamado “AmesProperty.csv”, la ruta del nodo debe ser cambiada si se corre en otro equipo.
![alt text](images/image.png)

### 2. Cálculos y reemplazos
**Paso:** Calculator  
**Descripción:** Realiza cálculos sobre las columnas existentes.  
![alt text](images/image-1.png)

**Paso:** Replace_Year_Remod  
**Descripción:** Reemplaza valores en la columna Year_Remod.

### 3. Ordenar filas
**Paso:** Ordenar filas 5 , Ordenar garages, Ordenar bsmt, Ordenar pools, Ordenar misc.

**Descripción:** Ordena las filas de acuerdo a ciertos criterios, en este caso se ordenan de forma ascendente según el valor del "PID".
![alt text](images/image-2.png)

### 4. Unión por clave
**Paso:** Unión por clave, Unión por clave 2, Unión por clave 3, Unión por clave 4, Unión por clave 5, Unión por clave 6, Unión por clave 7.

**Descripción:** Une los datos del archivo CSV con los datos de MongoDB y PostgreSQL basándose en una clave común, en este caso la clave es "PID".
![alt text](images/image-3.png)

### 5. Eliminación de IDs
**Paso:** Eliminacion_IDs  
**Descripción:** Elimina las columnas de ID que no son necesarias para el análisis posterior, y otras columnas no requeridas resultado de todas las uniones realizadas en el flujo.
![alt text](images/image-4.png)

### 6. Relleno de valores nulos
**Paso:** fill null  
**Descripción:** Rellena los valores nulos con un valor predefinido, en este caso todos los registros numéricos con valor nulo serán reemplazados con un "0" y todas las variables categóricas con valor nulo serán reemplazadas por un "NA".
![alt text](images/image-5.png)

### 7. Renombrar columnas
**Paso:** Rename Columns  
**Descripción:** Renombra las columnas para que tengan nombres más significativos, acordes a la guía propuesta.
![alt text](images/image-6.png)

### 8. Guardar dataset
**Paso:** Save dataset  
**Descripción:** Guarda el dataset procesado en el formato deseado.

## Detalles de las Fuentes de Datos

### Conexión MongoDB
**Conexión:** Para establecer la conexión a los datasets en MongoDB debemos establecer la “Connection String” con el valor de “mongodb+srv://infrati:infrati@cluster0.v5rih5b.mongodb.net/?retryWrites=true&w=majority” en la base de datos “test”, esto se hace en todos los nodos dispuestos.
![alt text](images/image-7.png)

### MongoDB Garages
**Paso:** Mongo_garages  
**Descripción:** Carga datos desde la colección 'garages' de MongoDB.
![alt text](images/image-8.png)

### MongoDB Bsmt
**Paso:** Mongo_bsmt  
**Descripción:** Carga datos desde la colección 'bsmt' de MongoDB.
![alt text](images/image-9.png)

### MongoDB Pools
**Paso:** Mongo_pools  
**Descripción:** Carga datos desde la colección 'pools' de MongoDB.
![alt text](images/image-10.png)

### MongoDB Misc
**Paso:** Mongo_misc  
**Descripción:** Carga datos desde la colección 'misc' de MongoDB.
![alt text](images/image-11.png)

### PostgreSQL Floor Detail
**Paso:** PGSQL_floordetail  
**Descripción:** Carga datos desde la tabla 'floordetail' de PostgreSQL.
![alt text](images/image-12.png)

### PostgreSQL Ames DB Temp
**Paso:** PgSQL_amesdbtemp  
**Descripción:** Carga datos desde la tabla 'amesdbtemp' de PostgreSQL.
![alt text](images/image-13.png)

### PostgreSQL Sale Property
**Paso:** PGSQL_SaleProperty  
**Descripción:** Carga datos desde la tabla 'SaleProperty' de PostgreSQL.
![alt text](images/image-14.png)

## Procesos de Unión
**Paso:** Unión por clave  
**Descripción:** Une diferentes conjuntos de datos a lo largo de múltiples pasos usando claves específicas para asegurar la integridad de los datos.


## Conclusión
Este pipeline ETL en Pentaho Data Integration es un excelente ejemplo de cómo integrar y transformar datos de múltiples fuentes de manera eficiente y efectiva. La estructura modular, junto con los pasos de transformación y limpieza, garantiza que los datos finales sean de alta calidad y estén listos para análisis detallados. Este enfoque no solo mejora la eficiencia del proceso ETL, sino que también asegura la escalabilidad y flexibilidad necesarias para adaptarse a futuros requerimientos de datos. En este caso, se ha logrado construir un dataset estructurado de propiedades a partir de diversas fuentes de información (MongoDB, PostgreSQL y CSV) que podrá ser utilizado confiablemente en análisis de datos.





