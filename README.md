# Proyecto Validación de Hipotesis

## Introducción
Una discográfica se enfrenta al desafío de lanzar un nuevo artista en el escenario musical global. Para ello, ha formulado una serie de hipótesis sobre los factores que influyen en el éxito de una canción en términos de streams en Spotify. Estas hipótesis son:

* Las canciones con un mayor BPM (Beats Por Minuto) tienen más éxito en términos de cantidad de streams.
* Las canciones más populares en el ranking de Spotify también muestran un comportamiento similar en otras plataformas como Deezer.
* La presencia de una canción en un mayor número de playlists se relaciona con un mayor número de streams.
* Los artistas con un mayor número de canciones en Spotify obtienen más streams.
* Las características específicas de una canción influyen en su éxito en términos de cantidad de streams.
  
Para llevar a cabo el análisis de estas hipótesis, la discográfica pone a disposición un [dataset](https://github.com/Maria-Data-Analyst/Proyecto-Validacion-Hipotesis/tree/main/dataset) de Spotify con información sobre las canciones más escuchadas en 2023. El objetivo de este análisis es validar o refutar estas hipótesis mediante el estudio de los datos, y proporcionar recomendaciones estratégicas basadas en los hallazgos. En última instancia, se busca que la discográfica y el nuevo artista puedan tomar decisiones informadas que aumenten sus posibilidades de éxito.
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
Inicialmente, se llevarán a cabo consultas individuales para cada punto de esta sección. Posteriormente, combinaremos todas las consultas para crear una vista consolidada y procesada de cada una de las tablas, con el objetivo de unirlas en una tabla general.
## Importación de datos
Importamos nuestro dataset mencionado anteriormente a la herramienta BigQuery como tablas para poder trabajr con su información.

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


De la consulta obtuvimos que la variable **key** de la tabla Technical info tenía 95 valores nulos, lo que representa el 10 % de los datos. Por lo tanto, se tomó la decisión de eliminar esta variable. También encontramos 50 valores nulos en la variable **in_shazam_charts** de la tabla COmpetition ; sin embargo, decidimos conservarla ya que necesitaremos esta variable más adelante. Para la tabla Spotify no encontramos valores nulos.

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

De las consultas aplicadas solo se obtuvo canciones duplicadas en la tabla de Spotify

[![Captura-de-pantalla-2024-06-30-193055.png](https://i.postimg.cc/Qdy1CrVk/Captura-de-pantalla-2024-06-30-193055.png)](https://postimg.cc/w1D74ZX7)
   
Para descartar estos duplicados, primero necesitamos más información. Por lo tanto, profundizamos en las características de las canciones en las demás tablas utilizando la siguiente consulta: 
```sql
----- CONSULTA PARA RECTIFICAR INFORMACION 
----- DE DUPLICADOS HALLADOS EN LA TABLA SPOTIFY 
----- EN LAS TABLAS TECHNICAL INFO Y COMPETITION

WITH duplicados AS (
  SELECT
    *,
    COUNT(*) OVER (PARTITION BY track_name, artist_s__name) AS cantidad
  FROM
    `proyecto-hipotesis-lab2.dataset.track_in_spotify`
)

SELECT 
  d.*,
  t1.*,
  t2.*
FROM 
  duplicados AS d
LEFT JOIN 
  `proyecto-hipotesis-lab2.dataset.track_technical_info ` AS t1
ON 
  d.track_id = t1.track_id
LEFT JOIN 
  `proyecto-hipotesis-lab2.dataset.track_in_competition` AS t2
ON 
  d.track_id = t2.track_id
WHERE 
  d.cantidad > 1  -- Filtra solo los registros que son duplicados
ORDER BY 
  d.track_name;

  ---------------------------------------------------------------------
```
Como resultado, observamos que la canción tiene el mismo nombre y es del mismo artista, pero presentan características diferentes. Por lo tanto, no se consideran duplicados y se mantienen en la base de datos.

[![Captura-de-pantalla-2024-06-30-195631.png](https://i.postimg.cc/nLr2BkTW/Captura-de-pantalla-2024-06-30-195631.png)](https://postimg.cc/zLmT1Tsw)

## Identificar y manejar datos discrepantes en variables categóricas
Para este paso de la limpieza, nos enfocaremos en remover los caracteres especiales de los nombres de los artistas y de sus canciones con la siguiente consulta : 
```sql
-- CONSULTA PARA REMOVER LOS CARACTERES ESPECIALES DE LOS NOMBRES DE LOS ARTISTAS Y LAS CANCIONES ---
SELECT
  track_name,
  REGEXP_REPLACE(track_name,r'[^a-zA-Z0-9]', ' ') AS track_name_limpia,
  artist_s__name,
  REGEXP_REPLACE(artist_s__name,r'[^a-zA-Z0-9]', ' ') AS artist_s__name_limpia,
FROM
  `proyecto-hipotesis-lab2.dataset.track_in_spotify`
```
Para verificar que la consulta funcionara dejamos la variable original al lado de la nueva variable limpia 
[![Captura-de-pantalla-2024-06-30-200647.png](https://i.postimg.cc/mks7XNNB/Captura-de-pantalla-2024-06-30-200647.png)](https://postimg.cc/r0fdz4PP)

## Identificar y manejar datos discrepantes en variables numéricas
Observando las variables de la tabla "Track in Spotify", encontramos que la variable streams es de tipo String. Esto podría deberse a la presencia de algún valor no numérico que está afectando la variable completa. Para solucionar esto, realizaremos una consulta para identificar dichos valores: 
```sql
---CONSULTA PARA ENCONTRAR CADENA DE TEXTO ----
SELECT 
track_id,
streams
FROM `proyecto-hipotesis-lab2.dataset.track_in_spotify` 
WHERE NOT REGEXP_CONTAINS(streams, r'^[0-9]+$')
```
[![Captura-de-pantalla-2024-06-30-202317.png](https://i.postimg.cc/zX9qGt8H/Captura-de-pantalla-2024-06-30-202317.png)](https://postimg.cc/jLQp8hWR)

Despues de identificar el valor discrepante en la variable utilizamos la siguiente consulta para convertir toda la variable en **Integer** y así más adelante poder realizar  operaciones matemáticas con ella 

```sql
---CONSULTA PARA CONVERTIR LA VARIABLE STREAM DE TIPO STRING A INTEGER ----
SELECT 
  *,
  IF(REGEXP_CONTAINS(streams, r'^[0-9]+$'), CAST(streams AS INT64), NULL) AS stream_limpio
FROM 
  `proyecto-hipotesis-lab2.dataset.track_in_spotify`
```

## Crear nuevas variables
Parte del proceso de procesamiento de datos implica crear nuevas variables que nos ayuden a comprender y gestionar mejor los datos disponibles. En este sentido, vamos a crear dos nuevas variables: una para la fecha de lanzamiento de la canción y otra para la participación total en playlists.

Comenzaremos con la fecha_lanzamiento : 

```sql
--- CONSULTA PARA CREAR LA VARIBLE FECHA DE LANZAMIENTO ---
SELECT  
track_name,
artist_s__name,
CAST(CONCAT(released_year, '-',released_month, '-',released_day)AS DATE) AS fecha_lanzamiento
FROM `proyecto-hipotesis-lab2.dataset.track_in_spotify` 
```

Esta consulta nos permite obtener el siguiente resultado 

[![Captura-de-pantalla-2024-06-30-204433.png](https://i.postimg.cc/59rVC73h/Captura-de-pantalla-2024-06-30-204433.png)](https://postimg.cc/23nMpxgG)

Seguimos con participación total en playlists

```sql
SELECT 
 s.track_id,
  s.artist_s__name,
  s.track_name,
  COALESCE(c.in_apple_playlists, 0) AS apple_music_playlists,
  COALESCE(c.in_deezer_playlists, 0) AS deezer_playlists,
  COALESCE(s.in_spotify_playlists, 0) AS spotify_playlists,
  COALESCE(c.in_apple_playlists, 0) + COALESCE(c.in_deezer_playlists, 0) + COALESCE(s.in_spotify_playlists, 0) AS total_playlists
FROM 
  `proyecto-hipotesis-lab2.dataset.track_in_competition` AS c
LEFT JOIN 
  `proyecto-hipotesis-lab2.dataset.track_in_spotify` AS s
ON 
  c.track_id = s.track_id
```

Con esta consulta, podemos observar la participación en playlists de cada aplicación y el total acumulado 
[![Captura-de-pantalla-2024-07-02-213758.png](https://i.postimg.cc/R0597Cnt/Captura-de-pantalla-2024-07-02-213758.png)](https://postimg.cc/w1VSHp2x)


## Unir tablas
Para unir las tablas, primero crearemos vistas limpias correspondientes a cada una utilizando las consultas anteriores de la siguiente manera:

### Vista limpia track_in_spotify
  * Limpieza de caracteres especiales para las varibles track_name y artist__s__name
  * creación de la variable fecha_lanzamiento
  * Identificar valores atípicos en la variable **streams** y convertirlos en valores nulos. Para todos los demás valores, realizar la conversión a tipo **Integer**
  * Manejar el valor null de Streams

``` sql
---- Vista limpia de la tabla track_in_spotify ----
SELECT 
track_id,
REGEXP_REPLACE(track_name,r'[^a-zA-Z0-9]', ' ') AS track_name_limpio,
REGEXP_REPLACE(artist_s__name,r'[^a-zA-Z0-9]', ' ') AS artist_s__name_limpio,
artist_count,
released_year,
released_month,
released_day,
CAST(CONCAT(released_year, '-',released_month, '-',released_day)AS DATE) AS fecha_lanzamiento,
in_spotify_playlists,
in_spotify_charts,
 IF(REGEXP_CONTAINS(streams, r'^[0-9]+$'), CAST(streams AS INT64), NULL) AS streams_limpio

 FROM `proyecto-hipotesis-lab2.dataset.track_in_spotify` 
 WHERE 
 IF(REGEXP_CONTAINS(streams, r'^[0-9]+$'), CAST(streams AS INT64), NULL) IS NOT NULL;

```

## Vista limpia de la tabla track_technical_info
  * Remover la variable key debido a la cantidad de nulos que presenta 

``` sql
--- Vista limpia tabla track_technical_limpia---
 SELECT 
*
EXCEPT(key)
 FROM `proyecto-hipotesis-lab2.dataset.track_technical_info ` 
```
## Vista limpia de la tabla track_in_competition
No es necesario hacer cambios en esta tabla por ahora. Sin embargo, la crearemos como una vista para tenerla lista, lo que nos permitirá editarla o complementarla en el futuro

``` sql
----- Tabla track in competition -------
SELECT
  *,
FROM `proyecto-hipotesis-lab2.dataset.track_in_competition`
```

Con estas tres vistas limpias, creamos el siguiente código para conformar la vista de la tabla general, en la que también añadimos la variable total_playlist

## Vista de la tabla_general

``` sql
---- Tabla General---
SELECT
  tabla_spotify.track_id,
  tabla_spotify.track_name_limpio,
  tabla_spotify.artist_s__name_limpio,
  tabla_spotify.artist_count,
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
  COALESCE(tabla_competition.in_apple_playlists, 0) + COALESCE(tabla_competition.in_deezer_playlists, 0) + COALESCE(tabla_spotify.in_spotify_playlists, 0) AS total_playlists
FROM
  `proyecto-hipotesis-lab2.dataset.track_technical_limpia` tabla_technical
INNER JOIN
  `proyecto-hipotesis-lab2.dataset.track_spotify_limpia` tabla_spotify
ON
  tabla_technical.track_id = tabla_spotify.track_id
INNER JOIN
  `proyecto-hipotesis-lab2.dataset.track_in_competition_limpia` tabla_competition
ON
  tabla_technical.track_id = tabla_competition.track_id;
```
