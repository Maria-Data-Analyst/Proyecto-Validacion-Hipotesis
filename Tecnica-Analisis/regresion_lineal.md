# REGRESIÓN LINEAL 
La regresión lineal es un método estadístico que se utiliza para modelar la relación lineal entre una variable dependiente (o respuesta) y una o más variables independientes (o predictores). Su objetivo es encontrar una relación lineal que mejor describa cómo cambia la variable dependiente a medida que cambian las variables independientes.

Para ver el código puede consultar el siguiente [Notebook](https://github.com/Maria-Data-Analyst/Proyecto-Validacion-Hipotesis/blob/main/regresion_lineal_hipotesis.ipynb)

Aplicaremos la regresión lineal para cada una de las hipótesis: 

## Hipótesis 1: Las canciones con un mayor BPM (Beats Por Minuto) tienen más éxito en términos de streams en Spotify

![Captura de pantalla 2024-09-11 160633](https://github.com/user-attachments/assets/e44e153c-8f9c-4b10-a4b7-2f11a8961dbb)

Los resultados del modelo de regresión lineal no apoyan la hipótesis de que las canciones con un mayor BPM tienen más éxito en términos de streams en Spotify. El coeficiente negativo sugiere una relación inversa como ya habiamos visto en la correlación, pero el valor p extremadamente alto y el R-squared muy bajo indican que esta relación no es significativa ni explicativa. Por lo tanto, no hay evidencia suficiente en estos datos para concluir que el BPM tenga un impacto importante en los streams de Spotify. 

**Hipótesis Rechazada**

## Hipótesis 2: Las canciones más populares en el ranking de Spotify también tienen un comportamiento similar en otras plataformas como Deezer

#### Variables utilizadas:
- **Variables independientes**:
  - `in_apple_charts`: Ranking de la canción en Apple
  - `in_deezer_charts`: Ranking de la canción en Deezer
- **Variable dependiente**:
  - `in_spotify_charts`: Ranking de la canción en Spotify

#### Resultados del Modelo de Regresión:

- **Intercepto (const)**: `0.4955`
- **Coeficiente (in_apple_charts)**: `0.1460`
- **Coeficiente (in_deezer_charts)**: `1.4769`
- **Valor p (in_apple_charts)**: `0.0000`
- **Valor p (in_deezer_charts)**: `0.0000`
- **R-squared**: `0.4816`
- **Adj. R-squared**: `0.4805`

#### Interpretación de Resultados:

1. **Coeficientes**:
   - **Coeficiente de `in_apple_charts`**: `0.1460`
     - Por cada unidad de incremento en el ranking de Apple, se espera que el ranking en Spotify aumente en aproximadamente `0.1460` unidades, manteniendo el ranking en Deezer constante.
   - **Coeficiente de `in_deezer_charts`**: `1.4769`
     - Por cada unidad de incremento en el ranking de Deezer, se espera que el ranking en Spotify aumente en aproximadamente `1.4769` unidades, manteniendo el ranking en Apple constante.

2. **Valor p**:
   - Ambos valores p (`in_apple_charts` y `in_deezer_charts`) son `0.0000`, que es mucho menor que el nivel de significancia común de `0.05`. Esto indica que ambos coeficientes son estadísticamente significativos, sugiriendo que los rankings en Apple y Deezer tienen una influencia significativa en el ranking en Spotify.

3. **R-squared y Adj. R-squared**:
   - El **R-squared** de `0.4816` indica que aproximadamente el `48.16%` de la variabilidad en el ranking de Spotify puede ser explicada por los rankings en Apple y Deezer.
   - El **Adj. R-squared** de `0.4805` indica que el modelo sigue siendo razonablemente bueno para predecir los rankings en Spotify después de ajustar por el número de variables independientes. La ligera disminución en comparación con el R-squared sugiere que el modelo no ha sido significativamente sobreajustado

#### Conclusión:

Los resultados del modelo sugieren que hay una relación significativa entre los rankings en otras plataformas y el ranking en Spotify. En particular, el ranking en Deezer tiene una mayor influencia en el ranking en Spotify en comparación con el ranking en Apple. Dado que ambos coeficientes son significativos y el modelo explica una parte sustancial de la variabilidad en los rankings de Spotify, se puede concluir que las canciones populares en Spotify tienden a ser también populares en otras plataformas, aunque la influencia de Deezer es más fuerte en este caso.

**Hipótesis Validada**

## Hipótesis 3:La presencia de una canción en un mayor número de playlists se relaciona con un mayor número de streams

![Captura de pantalla 2024-09-11 163548](https://github.com/user-attachments/assets/d27f1d6f-201a-4ec7-8595-91db8b14a693)

### Resultados del Modelo de Regresión

- **Coeficiente (total_participation_playlist)**: `49,766.43`
- **Valor p (total_participation_playlist)**: `0.0000`
- **R-squared**: `0.6132`
- **Adj. R-squared**: `0.6127`

### Interpretación de los Resultados


1. **Coeficiente (total_participation_playlist)**:
   - El coeficiente de `total_participation_playlist` es `49,766.43`. Esto significa que por cada unidad adicional en el número de playlists en las que una canción aparece, se espera que el número de streams aumente en `49,766.43` unidades

2. **Valor p (total_participation_playlist)**:
   - El valor p para `total_participation_playlist` es `0.0000`, lo que indica que el coeficiente es estadísticamente significativo. En otras palabras, hay una alta probabilidad de que la relación entre el número de playlists y el número de streams no sea debida al azar.

3. **R-squared (R²)**:
   - El **R-squared** de `0.6132` indica que aproximadamente el `61.32%` de la variabilidad en el número de streams puede ser explicado por el número de playlists en las que aparece la canción. Esto sugiere que el modelo tiene una capacidad moderada para explicar las diferencias en los streams basándose en la participación en playlists.

4. **Adj. R-squared (R² ajustado)**:
   - El **Adj. R-squared** de `0.6127` ajusta el **R-squared** por el número de predictores en el modelo. La diferencia entre el **R-squared** y el **Adj. R-squared** es pequeña, lo que indica que el modelo no está sobreajustado y que la inclusión de la variable `total_participation_playlist` es justificada.

### Conclusión

Los resultados sugieren que hay una relación positiva significativa entre el número de playlists en las que aparece una canción y el número de streams que recibe. En promedio, cada incremento en el número de playlists está asociado con un aumento considerable en el número de streams. El modelo explica más del 60% de la variabilidad en el número de streams, lo cual es bastante bueno, lo que indica que la presencia en múltiples playlists es un factor importante para el éxito de una canción en términos de streams.

**Hipótesis Validada**

## Hipótesis 4: Los artistas con un mayor número de canciones en Spotify tienen más streams

![Captura de pantalla 2024-09-11 171737](https://github.com/user-attachments/assets/0ddecf43-1045-4244-b44c-8f91c47d585b)

### Interpretación de los Resultados del Modelo

1. **Correlación**:  
   La correlación de 0.7810 indica una fuerte relación positiva entre el número de canciones y el número total de streams. Esto significa que a medida que un artista tiene más canciones en Spotify, tiende a tener más streams.

2. **Coeficiente (num_canciones)**:  
   El coeficiente de 500,241,167.86 indica que, en promedio, por cada canción adicional que un artista tiene en Spotify, se espera que sus streams aumenten en aproximadamente 500 millones. Este coeficiente sugiere que artistas con más canciones tienden a recibir más streams.

3. **Valor p (num_canciones)**:  
   El valor p es 0.0000, lo que significa que el número de canciones es una variable altamente significativa para predecir los streams. Es decir, hay una probabilidad prácticamente nula de que la relación observada ocurra por azar.

4. **R-squared (0.6099)**:  
   El valor de R² del 60.99% indica que el número de canciones explica aproximadamente el 61% de la variación en el número total de streams. Aunque es un valor razonablemente alto, sugiere que hay otros factores (el 39% restante) que también influyen en los streams.

### Conclusiones

El análisis muestra que el número de canciones que un artista tiene en Spotify es un predictor altamente significativo del número de streams. Esto refuerza la hipótesis de que los artistas con más canciones tienden a tener más éxito en términos de streams. Además el valor de R-squared del 60.99% indica que el número de canciones explica una porción significativa de la variación en los streams. 

**Hipótesis Validada**

## Hipótesis 5: Las características de la canción influyen en el éxito en términos de streams en Spotify

### Variables Independientes y Dependiente:

- **Variables independientes (X):**  
  - danceability_porcentaje  
  - valence_porcentaje  
  - energy_porcentaje  
  - acoustic_porcentaje  
  - instrumental_porcentaje  
  - speechen_porcentaje  
  - liveness_porcentaje  

- **Variable dependiente (y):**  
  - streams_limpio (Éxito en términos de streams en Spotify)

## Resultados del Modelo de Regresión:
 
- **Coeficiente (danceability_porcentaje):** -4,090,915.1465  
- **Valor p (danceability_porcentaje):** 0.0044  
- **Coeficiente (valence_porcentaje):** 170,213.2475  
- **Valor p (valence_porcentaje):** 0.8542  
- **Coeficiente (energy_porcentaje):** -1,112,923.6877  
- **Valor p (energy_porcentaje):** 0.4491  
- **Coeficiente (acoustic_porcentaje):** -1,098,375.8000  
- **Valor p (acoustic_porcentaje):** 0.2192  
- **Coeficiente (instrumental_porcentaje):** -4,294,662.2598  
- **Valor p (instrumental_porcentaje):** 0.0504  
- **Coeficiente (speechen_porcentaje):** -5,783,769.8365  
- **Valor p (speechen_porcentaje):** 0.0021  
- **Coeficiente (liveness_porcentaje):** -2,504,991.4363  
- **Valor p (liveness_porcentaje):** 0.0626  
- **R-squared:** 0.0291  
- **Adj. R-squared:** 0.0219  

### Análisis de Resultados del Modelo de Regresión

#### 1. Coeficientes y valores p:
Los coeficientes indican el cambio esperado en los streams cuando la característica aumenta en una unidad, mientras que los valores p determinan si ese coeficiente es estadísticamente significativo.

- **danceability_porcentaje:**  
  - Coeficiente: -4,090,915.1465  
  - Valor p: 0.0044  
La **danceabilidad** tiene un efecto negativo y significativo en los streams (p < 0.05). Esto sugiere que, en general, un aumento en la "danceabilidad" de una canción tiende a estar asociado con una disminución en el número de streams. Sin embargo, la correlación de Pearson entre danceability y streams es -0.1054, lo que indica que la relación es bastante débil y el impacto del cambio en la danceabilidad sobre los streams es leve.


- **valence_porcentaje:**  
  - Coeficiente: 170,213.2475  
  - Valor p: 0.8542  
  El **valence** no es significativo (p > 0.05), lo que sugiere que su impacto en los streams es poco relevante.

- **energy_porcentaje:**  
  - Coeficiente: -1,112,923.6877  
  - Valor p: 0.4491  
  La **energía** no tiene un impacto significativo (p > 0.05) sobre los streams.

- **acoustic_porcentaje:**  
  - Coeficiente: -1,098,375.8000  
  - Valor p: 0.2192  
  El **acousticness** tampoco es significativo (p > 0.05), pero tiene un coeficiente negativo, lo que sugiere una tendencia a que canciones más acústicas podrían tener menos streams.

- **instrumental_porcentaje:**  
  - Coeficiente: -4,294,662.2598  
  - Valor p: 0.0504  
  El **instrumental** está cerca de ser significativo (p ≈ 0.05), lo que sugiere que las canciones con mayor contenido instrumental tienden a tener menos streams. No obstante, la Correlación de Pearson de -0.0449 es bastante baja, lo que indica que la relación entre el contenido instrumental y los streams es débil.

- **speechen_porcentaje:**  
  - Coeficiente: -5,783,769.8365  
  - Valor p: 0.0021  
  El **speecheness** (contenido hablado) es significativo (p < 0.05) y tiene un efecto negativo, lo que indica que canciones con más contenido hablado tienden a tener menos streams.Sin embargo, como la Correlación de Pearson: -0.1123 es baja indica que la relación es bastante débil 

- **liveness_porcentaje:**  
  - Coeficiente: -2,504,991.4363  
  - Valor p: 0.0626  
  La **liveness** muestra un efecto negativo en los streams y está cerca de ser significativa (p ≈ 0.06). Esto sugiere que las canciones con un mayor porcentaje de "liveness" podrían tener menos streams. Sin embargo, la Correlación de Pearson de -0.0483 es muy baja, indicando que la relación entre la liveness y los streams es débil


### 2. R-squared y Adj. R-squared:
- **R-squared:** 0.0291  
  El modelo explica solo el **2.91%** de la variabilidad en los streams, lo que indica que las características de la canción tienen un impacto limitado sobre el éxito en términos de streams. Es probable que haya otros factores más relevantes que influyan en el éxito de las canciones.

- **Adj. R-squared:** 0.0219  
  El **R-cuadrado ajustado** también es bajo, lo que indica que el modelo no mejora significativamente cuando se ajusta por el número de variables.

### Conclusión:
- Las características **danceability_porcentaje** y **speechen_porcentaje** tienen un impacto negativo y estadísticamente significativo en el número de streams.
- Las características **valence_porcentaje**, **energy_porcentaje**, **acoustic_porcentaje** y **liveness_porcentaje** no tienen un impacto significativo en los streams.
- El modelo, en general, explica una porción muy pequeña de la variabilidad en los streams, por lo que sería útil considerar otras variables adicionales, como factores externos (popularidad del artista, marketing, etc.), para mejorar la precisión del modelo.
  
**Hipótesis Rechazada**



