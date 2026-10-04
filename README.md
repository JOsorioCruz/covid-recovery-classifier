# covid-recovery-classifier

## Actividad evaluativa 3: Modelado supervisado básico e interpretación de desempeño

**Asignatura:** Ciencia de Datos II
**Autor:** Jairo Alonso Osorio Cruz
**Programa:** Ingeniería de Software, Universidad de Cartagena, semestre VI

## Objetivo

En esta actividad construyo un modelo supervisado básico de clasificación con el dataset que preparé en la Actividad 2. Separo los datos en entrenamiento y prueba, calculo métricas iniciales e interpreto el desempeño frente a una problemática concreta.

La pregunta que trabajo es si, con la información disponible el día del diagnóstico, un modelo sencillo puede señalar qué casos de COVID-19 en Bolívar y Cartagena tendrán una recuperación prolongada, es decir, de más de 21 días. Saberlo ayudaría a planear la capacidad de seguimiento de casos.

## Datos

El dataset es *Casos positivos de COVID-19 en Colombia*, del Instituto Nacional de Salud, publicado en [Datos Abiertos Colombia](https://www.datos.gov.co/Salud-y-Protecci-n-Social/Casos-positivos-de-COVID-19-en-Colombia-/gt2j-8ykr). Lo filtré a Bolívar y Cartagena y lo limpié en la Actividad 2, cuyo repositorio es [covid19-bolivar-cartagena-data-prep](https://github.com/JOsorioCruz/covid19-bolivar-cartagena-data-prep). El diccionario de datos está en `data/diccionario_datos.md`.

## Estructura

```
data/
  covid19_bolivar_cartagena_preparado.csv   Dataset preparado en la Actividad 2 (203.862 filas, 33 columnas)
  diccionario_datos.md                      Diccionario de datos
notebooks/
  actividad3_modelado_supervisado.ipynb     Notebook con todo el proceso y sus salidas
figuras/                                    Gráficas generadas por el notebook
requirements.txt                            Versiones de las librerías que usé
```

## Cómo ejecutar el notebook

Lo trabajé en PyCharm sobre macOS con Python 3.13. Desde la terminal:

```zsh
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/actividad3_modelado_supervisado.ipynb
```

En PyCharm basta con seleccionar ese `.venv` como intérprete del proyecto, abrir el notebook y usar Run All. El notebook usa una semilla fija, así que al ejecutarlo de nuevo da los mismos resultados.

## Resultados principales

La variable objetivo es la recuperación prolongada. La población son los 199.129 casos recuperados con días de recuperación calculados, de los cuales el 9,71 % fue prolongado. Usé una división 80/20 estratificada y comparé tres modelos en el conjunto de prueba:

| Modelo | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Referencia (DummyClassifier) | 0,903 | 0,000 | 0,000 | 0,000 |
| Regresión logística | 0,626 | 0,146 | 0,590 | 0,235 |
| Árbol de decisión, profundidad 4 | 0,815 | 0,273 | 0,547 | 0,364 |

- El modelo de referencia tiene la accuracy más alta sin detectar un solo caso prolongado, por eso evalué con recall, precision y F1.
- El árbol de decisión fue el mejor modelo base. Entre los casos que marca hay 2,81 veces más casos prolongados que en una selección al azar, aunque unos 7 de cada 10 son falsas alarmas.
- No observé sobreajuste, pero sí señales de subajuste. El árbol se apoya casi por completo en el periodo del diagnóstico, lo que sugiere que capta cambios en la forma de cerrar los casos más que una condición clínica.

Mi conclusión es que el modelo puede apoyar una planeación agregada del seguimiento, siempre con revisión humana, pero no decisiones sobre personas concretas. Para mejorarlo harían falta variables clínicas y una validación por años.
