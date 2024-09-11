# Pruebas de Significancia

En este análisis se realizaron dos pruebas estadísticas para entender si es significativa la diferencia entre los streams promedio de cada grupo (Alto, Medio y Bajo) en función de las características de la canción: **danceability**, **valence**, **energy**, **acousticness**, **instrumentalness**, **liveness** y **speechiness**.

- Para la característica **Speechiness**, utilizamos el **test de Mann-Whitney U** dado que solo tiene dos grupos: Alto y Bajo.
- Para las demás características, empleamos el **test de Kruskal-Wallis**, ya que las variables están segmentadas en tres grupos: Alto, Medio y Bajo.

Puedes revisar el código completo en el siguiente [notebook](https://github.com/Maria-Data-Analyst/Proyecto-Validacion-Hipotesis/blob/main/Hito2_hipotesis.ipynb).

## Resultados

### Característica: `categ_speechen`
- **Mann-Whitney U Statistic**: 16,739.0  
- **P-value**: 0.0001307

### Característica: `categ_dance`
- **Kruskal-Wallis Statistic**: 7.278  
- **P-value**: 0.0263

### Característica: `categ_valence`
- **Kruskal-Wallis Statistic**: 3.219  
- **P-value**: 0.1999

### Característica: `categ_energy`
- **Kruskal-Wallis Statistic**: 1.063  
- **P-value**: 0.5879

### Característica: `categ_acoustic`
- **Kruskal-Wallis Statistic**: 1.554  
- **P-value**: 0.4598

### Característica: `categ_instrumental`
- **Kruskal-Wallis Statistic**: 1.101  
- **P-value**: 0.5766

### Característica: `categ_liveness`
- **Kruskal-Wallis Statistic**: 0.309  
- **P-value**: 0.8568

## Conclusiones

Los resultados de las pruebas estadísticas y las correlaciones revelan lo siguiente:

- **Danceability**: Aunque la prueba de Kruskal-Wallis mostró una diferencia significativa en los streams promedio entre los grupos `Alto`, `Medio` y `Bajo` (P-value = 0.0263), la correlación de Pearson (-0.1054) indica que este efecto es **inverso y débil**. Es decir, **a mayor danceability**, hay una ligera tendencia a que los streams **disminuyan**.

- **Speechiness**: La prueba de Mann-Whitney U reveló una diferencia significativa entre los grupos `Alto` y `Bajo` (P-value = 0.00013). Sin embargo, la correlación de Pearson (-0.1123) sugiere que este impacto es **negativo y débil**. Es decir, **a mayor speechiness**, hay una pequeña tendencia a que los streams **disminuyan**.

- **Valence, Energy, Acousticness, Instrumentalness y Liveness**: No se encontraron diferencias estadísticamente significativas en los streams promedio entre los grupos categorizados. Tampoco se observan correlaciones significativas con los streams.

En resumen, **Danceability** y **Speechiness** influyen significativamente en los streams, pero la correlación negativa y débil sugiere que, aunque influyen, lo hacen de forma **inversa** y con **muy poca fuerza**. Las demás características no muestran un impacto significativo en los streams.



