# Aplicar Técnica de Análisis
En este paso, aplicaremos técnicas de análisis para encontrar información que valide o refute las hipótesis planteadas.

## Agrupación de Datos por Categorías y Cálculo del Promedio de Streams
En este paso, construiremos tablas en Power BI para visualizar el promedio de streams por categoría (alto, medio y bajo) de cada característica de la canción.

![image](https://github.com/user-attachments/assets/b4d30d1e-c517-43c8-b251-a1fbfb74ac59)

Estas tablas nos permiten observar qué segmento tiene más streams y la diferencia entre ellos. Sin embargo, estos resultados no son suficientes para validar las hipótesis, ya que no muestran la variación dentro de los grupos.
## Validar las hipótesis 
En el paso anterior, calculamos las correlaciones para nuestras hipótesis. Ahora, crearemos un scatterplot en Power BI para visualizar esas correlaciones y determinar la validez o el rechazo de cada hipótesis

### Hipótesis 1
Las canciones con un mayor BPM (Beats Por Minuto) tienen más éxito en términos de cantidad de streams:
- **Correlación de Pearson (BigQuery)**: -0.0024

  ![image](https://github.com/user-attachments/assets/afb12325-0143-42ac-9314-04909ee4e7ca)

**Conclusión: RECHAZADA**  
Los resultados obtenidos muestran que la correlación entre el BPM y la cantidad de streams es cercana a 0, lo que sugiere que no existe una relación lineal significativa entre estos dos variables. Además, el análisis del gráfico de dispersión revela una línea de tendencia casi horizontal y una notable dispersión de datos a lo largo del eje x (BPM). En consecuencia, los hallazgos no respaldan la hipótesis de que las canciones con un mayor BPM tienen más éxito en términos de cantidad de streams

### Hipótesis 2 
Las canciones más populares en el ranking de Spotify también muestran un comportamiento similar en otras plataformas como Deezer. Para explorar esta hipótesis en mayor detalle, se analizó  el comportamiento de las canciones lanzadas en el año 2023.

#### Spotify y Deezer
- **Correlación de Pearson General**:0.6001
  
  ![image](https://github.com/user-attachments/assets/0e1011db-99ac-440b-aebe-fa8c9db44588)


- **Correlación de Pearson solo para las canciones lanzadas en el año 2023**: 0.7295
  
  ![image](https://github.com/user-attachments/assets/a5a8dbc0-4fcb-401b-8114-50f525267832)


#### Spotify y Apple

- **Correlación de Pearson General**:0.5512
  
  ![image](https://github.com/user-attachments/assets/38d08715-9b83-4418-aef3-950d5e2ea52e)


- **Correlación de Pearson solo para las canciones lanzadas en el año 2023**: 0.7142

    ![image](https://github.com/user-attachments/assets/15fda496-94cc-4fe6-bac7-38cac259bb1a)

#### Spotify y Shazam

- **Correlación de Pearson General**:0.6029

![image](https://github.com/user-attachments/assets/ecfc2c71-ddc5-4be6-9fb9-e314dbc697cd)



- **Correlación de Pearson solo para las canciones lanzadas en el año 2023**: 0.7173

 ![image](https://github.com/user-attachments/assets/08cb56bc-8142-4cff-9db2-768b79ee9006)



### **Conclusión: VALIDADA**  
Se observa una correlación de Pearson significativa entre el ranking de Spotify y Deezer, Apple y Shazam para todas las canciones, superando 0.5, lo cual indica una correlación positiva moderada entre las posiciones de las canciones en estas plataformas.

Particularmente notable es el incremento de la correlación para las canciones lanzadas en el año 2023, alcanzando más de 0.7. Este aumento sugiere una correlación positiva más robusta entre el desempeño de las canciones en Spotify y Deezer, Apple y Shazam durante ese año específico.

Estos resultados respaldan la hipótesis de que las canciones más populares en el ranking de Spotify también exhiben un comportamiento similar en Deezer, Apple y Shazam. El análisis del gráfico de dispersión muestra una línea de tendencia claramente positiva, reforzando la correlación observada entre Spotify y cada una de estas plataformas

### Hipótesis 3
La presencia de una canción en un mayor número de playlists se relaciona con un mayor número de streams. Se investigó especialmente esta relación para las canciones lanzadas en el año 2023, analizando su correlación como nuevas en el mercado musical

- **Correlación de Pearson General**:0.7830

  ![image](https://github.com/user-attachments/assets/8d868156-bb54-4d7f-a92e-365b3bf0d403)


- **Correlación de Pearson solo para las canciones lanzadas en el año 2023**: 0.7846

  ![image](https://github.com/user-attachments/assets/576072b8-7949-4d28-b37f-2988007f3b37)

#### **Conclusión: VALIDADA**  

Se ha encontrado una correlación significativa entre la presencia de una canción en un mayor número de playlists y el número de streams que genera. Tanto para todas las canciones en general como para aquellas lanzadas específicamente en el año 2023, se observa una correlación de Pearson de 0.7830 y 0.7846 respectivamente, indicando una fuerte relación positiva entre estos dos factores.

El análisis de los gráficos de dispersión confirma esta correlación, mostrando una línea de tendencia claramente ascendente. Esto respalda la hipótesis de que las canciones que aparecen en más playlists tienden a generar más streams, especialmente evidente para las canciones nuevas introducidas en el mercado musical durante el año 2023




