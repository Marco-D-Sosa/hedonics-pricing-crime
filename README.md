# Modelo de Precios de Alquileres Temporarios en CABA

![Mapa de calor espacial de los precios de alquiler en CABA](map_price.png)

*(Leyenda: Mapa de calor a nivel barrio que ilustra la distribución espacial de los precios de alquileres temporarios en la Ciudad Autónoma de Buenos Aires)*

## 📊 Resumen Ejecutivo (Valor para el Negocio)

La ubicación es uno de los principales determinantes del valor de una propiedad. Este proyecto construye un **modelo de precios hedónicos** (una regresión que descompone un precio en el valor que el mercado asigna a cada atributo) para alquileres temporarios en la Ciudad Autónoma de Buenos Aires (CABA). Mide cuánto valen, dentro del precio de un alquiler, factores como la seguridad del barrio y el ingreso local.

**Qué demuestra este proyecto:**
* **Construcción de un dataset desde cero:** Construí, limpié y consolidé un dataset de panel propio con más de **145.000 observaciones**, integrando fuentes de datos públicas dispares.
* **Modelos de precios para el sector inmobiliario:** Un modelo hedónico espacial que aísla el efecto de cada atributo de ubicación sobre el precio, un enfoque ampliamente utilizado en la valuación inmobiliaria.
* **Flujo de trabajo automatizado y reproducible:** Todo el análisis se ejecuta desde un único script maestro, tanto en Python como en Stata.
* **Ejemplo de hallazgo:** Un **aumento del 1% en el delito local se asocia con una caída del 0,065% en los precios de los alquileres temporarios**.

**Aplicación de negocio:** Valuación inmobiliaria y fijación de precios para plataformas de alquiler y administradores de propiedades, en todos los casos donde sea necesario incorporar factores geoespaciales al precio.

## 🧠 Dataset y Metodología

Dado que los precios de los alquileres temporarios reflejan la disposición a pagar por una vivienda transitoria en una ubicación específica, los datos capturan de manera inherente la prima o la penalización asociada a los atributos del barrio, incluida la seguridad percibida.

Para facilitar la transparencia y la reproducibilidad, construí el dataset de forma independiente mediante scraping, limpieza e integración de:
* Datos de alquileres temporarios de **Inside Airbnb**.
* Registros oficiales de delitos, características geográficas e indicadores socioeconómicos (por ejemplo, el ingreso familiar promedio) del **Portal de Datos Abiertos del Gobierno de la Ciudad de Buenos Aires**.

*Accedé al dataset público y limpio alojado en Kaggle para su descarga automatizada:* [marcodiazzz/buenos-aires-rentals-and-crime](https://www.kaggle.com/datasets/marcodiazzz/buenos-aires-rentals-and-crime)

## 🐍 Flujo de Trabajo en Python (Totalmente Automatizado)

El pipeline de Python está completamente automatizado: se encarga de la descarga de datos, el procesamiento y la generación de resultados, todo en memoria o mediante impresiones en consola.

### 1. Requisitos

Navegá al directorio de Python e instalá las dependencias necesarias:

```bash
cd Python
pip install -r requirements.txt
```

### 2. Ejecución

Ejecutá el script maestro. Este se encargará de preparar los datos y llamará secuencialmente a los módulos requeridos (`hedonic_crime_model_1.py` y `hedonic_crime_model_2.py`).

```bash
python Master.py
```

## 📈 Flujo de Trabajo en Stata

A diferencia del pipeline de Python, el flujo de trabajo en Stata requiere descargar manualmente la base de datos y configurar un directorio de trabajo local.

### 1. Dependencias

El código depende de los siguientes paquetes externos de Stata: `geodist`, `spmap` y `outreg2`.

Instalación: Los comandos de instalación están incluidos al comienzo del archivo `Master.do`, actualmente comentados con un asterisco (`*`). Si necesitás instalarlos, simplemente quitá el `*` y ejecutá esas líneas antes de continuar.

### 2. Configuración de los Datos

1. Descargá la base de datos requerida desde [Kaggle](https://www.kaggle.com/datasets/marcodiazzz/buenos-aires-rentals-and-crime).
2. Colocá la base de datos descargada directamente dentro de la carpeta `Stata/`, junto a los archivos `.do`.
3. Abrí `Master.do`. Al comienzo del archivo, ubicá la variable de ruta designada y reemplazala por la ruta absoluta local al directorio `Stata/`. Esto solo hay que configurarlo una vez.

### 3. Ejecución

Ejecutá el script maestro. Cargará la base de datos, generará los directorios de salida necesarios y llamará secuencialmente a `hedonic_crime_model_1.do` y `hedonic_crime_model_2.do`.

```stata
do Master.do
```
