# ANÁLISIS EXPLORATORIO
Para el análisis exploratorio, vincularemos nuestra tabla principal en BigQuery con Power BI donde a través de tablas y gráficos, podremos descubrir insights significativos, identificar relaciones y tomar decisiones informadas basadas en un análisis profundo de los datos.

##  Agrupación de datos según variables categóricas
En este paso, agruparemos los datos según variables categóricas como "artista" y "año de lanzamiento". Utilizaremos matrices en Power BI para determinar la cantidad de tracks por artista y la cantidad de tracks por año de lanzamiento.
![image](https://github.com/user-attachments/assets/aef3adbc-7f14-415f-938b-932c4643033d)

##  Visualización de Variables Categóricas
A través de gráficos de barras, visualizar las variables categóricas

### Cantidad de tracks por año de lanzamiento
Para mejorar la visualización del gráfico, hemos añadido un filtro que permite seleccionar el rango de años que deseamos observar.

![image](https://github.com/user-attachments/assets/b57aa1f2-ea3a-40f9-bd16-3c766fa95056)

### Cantidad de tracks por artista

![image](https://github.com/user-attachments/assets/e47811b9-e4cb-4f28-abff-966eed543c20)

## Calcular Medidas de Tendencia Central
Utilizando tablas en Power BI, vamos a calcular las medidas de tendencia central para las siguientes métricas:

* streams: Calcular máximo, mínimo, desviación estándar , media, mediana y moda de los streams.
* total_participation_playlist: Calcular máximo, mínimo, desviación estándar , media, mediana y moda del número total de veces que aparece en una playlist

![image](https://github.com/user-attachments/assets/25551228-e4c0-4cb0-9a8d-1a954095173b)

## Visualización de Distribución
Vamos a visualizar la distribución utilizando un histograma para las variables de streams y total_participation_playlist. Para generar este gráfico, utilizaremos código de Python dentro de Power BI. Esto nos permitirá codificar el histograma usando los datos de Power BI en lenguaje Python

EL codigo para los histogramas es el siguiente
### Histograma Total Participacion en Playlist 
```python
# Histograma Total de Participacion en Playlist 
import matplotlib.pyplot as plt
import pandas as pd
data = dataset[['total_participation_playlist']] # variable que se encuentra en Power BI
# Crea el histograma
plt.hist(data, bins=10, color='purple',alpha=0.7)
plt.xlabel('Valor')
plt.ylabel('Frecuencia')
plt.title('Histograma')
#Muestra el histograma
plt.show()
```
### Histograma Streams
``` Python
# Histograma Streams
import matplotlib.pyplot as plt
import pandas as pd
data = dataset[['streams_limpio']]
# Crea el histograma
plt.hist(data, bins=10, color='#ff5722',alpha=0.7)
plt.xlabel('Valor')
plt.ylabel('Frecuencia')
plt.title('Histograma')
#Muestra el histograma
plt.show()
```
![image](https://github.com/user-attachments/assets/30c1475d-f231-485f-88bc-2ffdc7d91fcb)

##  Visualizar el comportamiento de los datos a lo largo del tiempo
Visualizar a través de gráficos de líneas, el comportamiento de los datos a lo largo del tiempo para streams y total_participation_playlist

![image](https://github.com/user-attachments/assets/d0f5c7c0-a3dd-4b3c-a852-ece1996ef7f3)

## Crear categorías  para las variables de características en BigQuery
Vamos a regresar a BigQuery para calcular una métrica adicional. Además de segmentar, crearemos categorías para las variables de características de las canciones: alto, medio y bajo según corresponda. Para segmentar, hemos investigado para contextualizar cómo se clasifican estas características y determinar los rangos adecuados para alto, medio y bajo.

Este es el código completo de la tabla general, en el cual hemos añadido las líneas necesarias para crear las categorías de alto, medio y bajo para todas las características de las canciones, facilitando un análisis más detallado:

``` sql
----Tabla Auxiliar----
WITH
  total_canciones_solistas AS (
  SELECT
    artist_s__name_limpio,
  IF
    (artist_count = 1, artist_s__name_limpio, 'NO ES SOLISTA') AS solista,
    COUNT(*) AS total_canciones
  FROM
    `proyecto-hipotesis-lab2.dataset.track_spotify_limpia`
  GROUP BY
    artist_s__name_limpio,
    solista )
------------------------------
-------- Tabla General -------
SELECT
  tabla_spotify.track_id,
  tabla_spotify.track_name_limpio,
  tabla_spotify.artist_s__name_limpio,
  tabla_spotify.artist_count,
  IFNULL(tcs.total_canciones, 0) AS total_canciones_solista,
  tabla_spotify.released_year,
  tabla_spotify.released_month,
  tabla_spotify.released_day,
  tabla_spotify.fecha_lanzamiento,
  tabla_spotify.in_spotify_playlists,
  tabla_spotify.in_spotify_charts,
  tabla_spotify.streams_limpio,
  tabla_competition.in_apple_playlists,
  tabla_competition.in_apple_charts,
  tabla_competition.in_deezer_playlists,
  tabla_competition.in_deezer_charts,
  tabla_competition.in_shazam_charts,
  tabla_technical.mode,
  tabla_technical.bpm,
  tabla_technical.`danceability_%` AS dance_porcentaje,
  tabla_technical.`valence_%` AS valence_porcentaje,
  tabla_technical.`energy_%` AS energy_porcentaje,
  tabla_technical.`acousticness_%` AS acoustic_porcentaje,
  tabla_technical.`instrumentalness_%` AS instrumental_porcentaje,
  tabla_technical.`liveness_%` AS live_porcentaje,
  tabla_technical.`speechiness_%` AS speech_porcentaje,
  COALESCE(tabla_competition.in_apple_playlists, 0) AS apple_music_playlists,
  COALESCE(tabla_competition.in_deezer_playlists, 0) AS deezer_playlists,
  COALESCE(tabla_spotify.in_spotify_playlists, 0) AS spotify_playlists,
  COALESCE(tabla_competition.in_apple_playlists, 0) + COALESCE(tabla_competition.in_deezer_playlists, 0) + COALESCE(tabla_spotify.in_spotify_playlists, 0) AS total_playlists,

----- categorias----
  CASE
    WHEN `speechiness_%` >= 66 THEN "Alto"
    WHEN `speechiness_%` >= 33 AND `speechiness_%` < 66 THEN "Medio"
    ELSE "Bajo"
  END AS categ_speechen,

  CASE
    WHEN `danceability_%` >= 70 THEN "Alto"
    WHEN `danceability_%` >= 40 AND `danceability_%` < 70 THEN "Medio"
    ELSE "Bajo"
  END AS categ_dance,

  CASE
    WHEN `valence_%` >= 60 THEN "Alto"
    WHEN `valence_%` >= 30 AND `valence_%` < 60 THEN "Medio"
    ELSE "Bajo"
  END AS categ_valence,

  CASE
    WHEN `energy_%` >= 70 THEN "Alto"
    WHEN `energy_%` >= 40 AND `energy_%` < 70 THEN "Medio"
    ELSE "Bajo"
  END AS categ_energy,

  CASE
    WHEN `acousticness_%`  >= 70 THEN "Alto"
    WHEN `acousticness_%`  >= 30 AND `acousticness_%`  < 70 THEN "Medio"
    ELSE "Bajo"
  END AS categ_acoustic,

  CASE
    WHEN `instrumentalness_%` >= 50 THEN "Alto"
    WHEN `instrumentalness_%` >= 20 AND `instrumentalness_%` < 50 THEN "Medio"
    ELSE "Bajo"
  END AS categ_instrumental,

 CASE
    WHEN `liveness_%`>= 60 THEN "Alto"
    WHEN `liveness_%` >= 40 AND `liveness_%` < 60 THEN "Medio"
    ELSE "Bajo"
  END AS categ_live
-----------------------------
FROM
  `proyecto-hipotesis-lab2.dataset.track_technical_limpia` tabla_technical
INNER JOIN
  `proyecto-hipotesis-lab2.dataset.track_spotify_limpia` tabla_spotify
ON
  tabla_technical.track_id = tabla_spotify.track_id
INNER JOIN
  `proyecto-hipotesis-lab2.dataset.track_in_competition_limpia` tabla_competition
ON
  tabla_technical.track_id = tabla_competition.track_id
LEFT JOIN
  total_canciones_solistas AS tcs
ON
  tabla_spotify.artist_s__name_limpio = tcs.artist_s__name_limpio;
```
