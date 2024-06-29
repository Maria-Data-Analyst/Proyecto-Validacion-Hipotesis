# Proyecto Validación de Hipotesis

## Introducción
Una discográfica se enfrenta al desafío de lanzar un nuevo artista en el escenario musical global. Para ello, ha formulado una serie de hipótesis sobre los factores que influyen en el éxito de una canción en términos de streams en Spotify. Estas hipótesis son:

* Las canciones con un mayor BPM (Beats Por Minuto) tienen más éxito en términos de cantidad de streams.
* Las canciones más populares en el ranking de Spotify también muestran un comportamiento similar en otras plataformas como Deezer.
* La presencia de una canción en un mayor número de playlists se relaciona con un mayor número de streams.
* Los artistas con un mayor número de canciones en Spotify obtienen más streams.
* Las características específicas de una canción influyen en su éxito en términos de cantidad de streams.
  
Para llevar a cabo el análisis de estas hipótesis, la discográfica pone a disposición un [dataset](https://github.com/Maria-Data-Analyst/Proyecto-Validacion-Hipotesis/blob/Consultas-Query/track_in_spotify%20-%20spotify.csv) de Spotify con información sobre las canciones más escuchadas en 2023. El objetivo de este análisis es validar o refutar estas hipótesis mediante el estudio de los datos, y proporcionar recomendaciones estratégicas basadas en los hallazgos. En última instancia, se busca que la discográfica y el nuevo artista puedan tomar decisiones informadas que aumenten sus posibilidades de éxito.
### Índice

A continuación, se detallan los pasos necesarios para alcanzar los objetivos propuestos en el proyecto:

1. **Procesamiento y Preparación de la Base de Datos**
   - Recopilación y limpieza de datos.
   - Transformación y estructuración de la información.
   
2. **Análisis Exploratorio de Datos**
   - Exploración de tendencias y patrones.
   - Identificación de variables relevantes.
   
3. **Aplicación de Técnicas de Análisis**
   - Selección y aplicación de métodos analíticos adecuados.
   - Interpretación de resultados preliminares.
   
4. **Presentación de Resultados**
   - Comunicación clara de hallazgos y conclusiones.
   - Recomendaciones estratégicas basadas en los resultados obtenidos.
### Herramientas
- BigQuery
- Power BI
- Python
- Google Colab
  
# Procesamiento y Preparación de la Base de Datos

## Importación de datos

Inicialmente, se trabajará desde BigQuery para hacer la limpieza y la preparación de los datos. Por lo tanto, aquí se subirán las tablas del dataset mencionado anteriormente.

## Identificar y manejar los valores nulos

Este procedimiento se realizara en las tres tablas del dataset. 

Para este paso utilizaremos las siguientes consultas: 

```sql
-------- CONSULTA DE NULOS PARA LA TABLA TRACK IN SPOTIFY -----
SELECT
  COUNTIF(track_id IS NULL) AS nulos_track_id,
  COUNTIF(track_name IS NULL) AS nulos_name,
  COUNTIF(artist_s__name IS NULL) AS nulos_artist_name,
  COUNTIF(artist_count IS NULL) AS nulos_artist_count,
  COUNTIF(released_year IS NULL) AS nulos_releaed_year,
  COUNTIF(released_month IS NULL) AS nulos_released_month,
  COUNTIF(released_day IS NULL) AS nulos_released_day,
  COUNTIF(in_spotify_playlists IS NULL) AS nulos_spotify_playlist,
  COUNTIF(in_spotify_charts IS NULL) AS nulos_spotify_charts,
  COUNTIF(streams IS NULL) AS nulos_streams
FROM
  `proyecto-hipotesis-lab2.dataset.track_in_spotify`;

--------------------------------------------------------------------
----- CONSULTA DE NULOS PARA LA TABLA TRACK IN COMPETITION--------------
SELECT
  COUNTIF(track_id IS NULL) AS nulos_track_id,
  COUNTIF(in_apple_playlists IS NULL) AS nulos_in_apple_playlists,
  COUNTIF(in_apple_charts IS NULL) AS nulos_in_apple_charts,
  COUNTIF(in_deezer_playlists IS NULL) AS nulos_in_deezer_playlists,
  COUNTIF(in_deezer_charts IS NULL) AS nulos_in_deezer_charts,
  COUNTIF(in_shazam_charts IS NULL) AS nulos_in_shazam_charts
FROM
  `proyecto-hipotesis-lab2.dataset.track_in_competition` ;

--------------------------------------------------------------------
----- CONSULTA DE NULOS PARA LA TABLA TRACK TECHNICAL INFO --------------
SELECT
  COUNTIF(track_id IS NULL) AS nulos_track_id,
  COUNTIF(bpm IS NULL) AS nulos_bpm,
  COUNTIF(`key` IS NULL) AS nulos_key,
  COUNTIF(mode IS NULL) AS nulos_mode,
  COUNTIF(`danceability_%` IS NULL) AS nulos_dance,
  COUNTIF(`valence_%` IS NULL) AS nulos_valence,
  COUNTIF(`energy_%` IS NULL) AS nulos_energy,
  COUNTIF(`acousticness_%` IS NULL) AS nulos_acoustic,
  COUNTIF(`instrumentalness_%` IS NULL) AS nulos_instrumental,
  COUNTIF(`liveness_%` IS NULL) AS nulos_liveness,
  COUNTIF(`speechiness_%` IS NULL) AS nulos_speech


FROM
  `proyecto-hipotesis-lab2.dataset.track_technical_info `
```


De la consulta obtuvimos que la variable **key** tenía 95 valores nulos, lo que representa el 10 % de los datos. Por lo tanto, se tomó la decisión de eliminar esta variable. También encontramos 50 valores nulos en la variable **in_shazam_charts**; sin embargo, decidimos conservarla ya que necesitaremos esta variable más adelante.

## Identificar y manejar valores duplicados
vrificamos los duplicados de todas las tablas con las siguientes Queries : 

```sql
---------------------------------------------------------------------
  ---CONSULTA PARA ENCONTRAR DUPLICADOS EN LA TABLA TECHNICAL
SELECT
  track_id,
  COUNT (*) AS cantidad
FROM
  `proyecto-hipotesis-lab2.dataset.track_technical_info `
GROUP BY
  track_id
HAVING
  COUNT (*)>1;
----------------------------------------------------------------------
    ---CONSULTA PARA VER LOS DUPLICADOS EN LA TABLA SPOTIFY --
SELECT
 *
FROM (
  SELECT
 *,
    COUNT(*) OVER (PARTITION BY track_name, artist_s__name) AS cantidad
  FROM
    `proyecto-hipotesis-lab2.dataset.track_in_spotify` )
WHERE
  cantidad > 1
ORDER BY
  track_name;
------------------------------------------------------------------
---CONSULTA PARA VALIDAR DUPLICADOS EN LA TABLA COMPETITION----
SELECT
  track_id,
  COUNT (*) AS cantidad
FROM
  `proyecto-hipotesis-lab2.dataset.track_in_competition`
GROUP BY
  track_id
HAVING
  COUNT (*)>1;
  ------------------------------------------------------------------
```

De estas consultas obtuvimos los siguientes duplicados 


   




