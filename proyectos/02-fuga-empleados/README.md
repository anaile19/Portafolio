# Fuga de empleados: factores, costo y modelo de riesgo

**Preguntas de negocio:** ¿qué características acompañan al abandono?, ¿cuánto cuesta la rotación? y ¿qué empleados activos tienen mayor riesgo de irse?

**Herramientas:** Python · Pandas · SciPy · Scikit-learn · Matplotlib · Seaborn · Tableau

## Resultado

- **Tasa de abandono:** 16.1% (237 de 1,470 empleados), con un costo de reemplazo estimado de **USD 2.72 millones**.
- **Factores con mayor efecto en el modelo:** hacer horas extra (multiplica las probabilidades relativas de abandono por 4.6), viajar con frecuencia (×2.6), baja satisfacción con el entorno (×2.3) y el puesto (*Laboratory Technician* ×2.2).
- **Modelo:** la regresión logística obtuvo un **ROC-AUC de 0.83 en validación cruzada y 0.79 en el conjunto de prueba**. Contactando al 20% de empleados con mayor riesgo se captura el 56% de las salidas.
- **Priorización:** 170 empleados activos (14%) en riesgo alto concentran el 39% del costo esperado de la rotación.

## Datos

1,470 empleados con 31 variables: datos demográficos, puesto, salario, satisfacción y antigüedad. Es una versión en español, preparada por DS4B, del conjunto ficticio *IBM HR Analytics Employee Attrition*. El archivo no se publica en este repositorio.

## Método

1. **Calidad de datos:** se eliminan las variables con más de 60% de nulos, las de un solo valor y `sexo`, por su codificación inconsistente. Los nulos restantes se imputan dentro del pipeline del modelo.
2. **Análisis de factores:** tasas de abandono por categoría con pruebas chi-cuadrado y V de Cramér, y comparación de variables numéricas con la prueba U de Mann-Whitney.
3. **Costo de la rotación:** porcentajes de costo de reemplazo por tramo salarial del estudio del Center for American Progress (2012).
4. **Modelo:** partición estratificada 70/30 y comparación de un modelo base, un árbol de decisión, un random forest y una regresión logística con validación cruzada de 5 particiones. Se ajusta la regularización y se elige el umbral con predicciones fuera de muestra.
5. **Interpretación:** odds ratios de la regresión logística.
6. **Uso:** probabilidad de abandono y costo esperado para cada empleado activo, con predicciones fuera de muestra, exportados para el dashboard de Tableau.

## Dashboard

[Dashboard en Tableau Public]([https://public.tableau.com/app/profile/eliana.salazar.labor./viz/PracticaDashboard_17098436278170/Dashboard1](https://public.tableau.com/views/Riesgodefugadeempleados/Dashboard1?:language=es-ES&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link): tasa de abandono, empleados en riesgo y costo esperado por puesto.

## Limitaciones

- Datos ficticios, de un solo corte y sin fechas: el modelo identifica asociaciones, no causas.
- Los costos de reemplazo vienen de un estudio de Estados Unidos y se asume que los salarios están en dólares.
- Con 237 salidas, las métricas tienen variabilidad: la desviación del ROC-AUC entre particiones ronda 0.05.

## Cambios frente a la versión original

La versión original seguía la plantilla del curso. Esta versión agrega pruebas estadísticas, compara cuatro modelos con validación cruzada (el árbol de decisión original obtiene un ROC-AUC de 0.69 con este método), quita del modelo variables derivadas del salario, imputa dentro del pipeline, califica a los empleados con predicciones fuera de muestra y corrige la moneda de los costos (dólares, no euros).

## Cómo reproducirlo

```
pip install pandas scipy scikit-learn matplotlib seaborn
```

Coloca el archivo de datos en `data/AbandonoEmpleados.csv` y ejecuta `fuga-empleados.ipynb`.
