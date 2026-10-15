# Segmentación de clientes bancarios por preferencias de consumo

**Pregunta de negocio:** ¿qué grupos de clientes existen según los rubros en los que concentran sus compras con tarjeta, y qué oferta tiene sentido para cada uno?

**Herramientas:** Python · Pandas · Scikit-learn (K-Means, PCA) · Matplotlib · Seaborn

## Resultado

6 segmentos estables a partir de 21,905 clientes con consumo recurrente:

| Segmento | % clientes | Qué lo distingue |
|---|---|---|
| Diversificado | 33% | Consumo repartido entre muchos rubros; ticket mediano más alto |
| Restaurantes y bares | 23% | 48% de sus transacciones en restaurantes; el segmento más joven |
| Supermercado | 22% | 65% en supermercados; ticket más bajo |
| Salud | 9% | 59% en salud |
| Vehículos y repuestos | 8% | 60% en vehículos y repuestos |
| Entretenimiento | 5% | 75% en entretenimiento; la mayor frecuencia de compra |

Los 18,083 clientes ocasionales (1–2 meses con consumo en el año) quedan como un grupo aparte, para campañas de activación.

## Datos

339,476 registros de consumo con tarjeta de crédito y débito de 40,000 clientes, entre noviembre de 2016 y octubre de 2017, en 21 rubros comerciales. Los datos fueron proporcionados por el Programa de Especialización Analítica de DMC y no se publican en este repositorio.

## Método

1. **Calidad de datos:** se corrige un rubro con caracteres corruptos y se excluyen los registros sin rubro (0.07%). Las variables demográficas con nulos se usan solo para describir los segmentos.
2. **Población:** clientes con 3 o más meses de consumo (55% de los clientes y 91% de las transacciones), para que el perfil refleje una preferencia y no una compra aislada.
3. **Variables:** proporción de las transacciones de cada cliente en cada rubro. Mide preferencia, no volumen, y no requiere estandarizar ni eliminar outliers.
4. **Modelo:** K-Means con k de 2 a 10, evaluado con el método del codo, el coeficiente de silueta, el tamaño del segmento más grande y la estabilidad entre semillas. Se eligió k = 6.
5. **Perfilamiento:** composición del consumo, índice frente al cliente promedio, actividad, ticket y perfil demográfico de cada segmento.

## Limitaciones

- La silueta del modelo es 0.25: los segmentos tienen fronteras difusas, algo habitual en datos de consumo.
- Los segmentos se basan en número de transacciones, no en monto.
- Un solo año de historia y sin datos de respuesta a campañas. Las ofertas sugeridas son hipótesis para validar con un piloto con grupo de control.

## Cambios frente a la versión original (2020)

Esta versión corrige errores del análisis original: el filtro de actividad se aplicaba a clientes equivocados por un desalineamiento de filas, el clustering no usaba las componentes del PCA que luego se interpretaban, se excluía a los clientes más activos (11 y 12 meses) y la eliminación de outliers descartaba al 26% de la base.

## Cómo reproducirlo

```
pip install pandas scikit-learn matplotlib seaborn
```

Coloca el archivo de datos en `data/02dataBaseConsumo.txt` y ejecuta `segmentacion-kmeans.ipynb`.
