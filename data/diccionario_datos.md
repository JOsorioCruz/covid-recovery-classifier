# Diccionario de datos: Casos positivos de COVID-19 en Colombia (subconjunto Bolívar y Cartagena D.T. y C.)

## 1. Procedencia del dataset y justificación del cambio

Este proyecto **no partió de un dataset propio de la Actividad 1**, porque dicho archivo y su
diccionario de datos no fueron entregados ni elaborados previamente. Para no incumplir la regla de
no inventar datos, se sustituyó el insumo de la Actividad 1 por un **dataset público, real y verificable**,
tomado directamente de la fuente oficial, en lugar de generar datos sintéticos.

- **Fuente:** Instituto Nacional de Salud (INS) de Colombia.
- **Nombre original del dataset:** *Casos positivos de COVID-19 en Colombia*.
- **Portal:** Datos Abiertos Colombia: <https://www.datos.gov.co/Salud-y-Protecci-n-Social/Casos-positivos-de-COVID-19-en-Colombia-/gt2j-8ykr>
- **Identificador del recurso (Socrata):** `gt2j-8ykr`
- **Método de obtención:** descarga programática vía la API pública de Socrata (`https://www.datos.gov.co/resource/gt2j-8ykr.json`), sin alterar ningún valor.
- **Filtro aplicado:** solo se conservaron los registros donde `departamento_nom` es `BOLIVAR` o
  `CARTAGENA` (Cartagena es un Distrito Turístico y Cultural con código DIVIPOLA propio, 13001,
  independiente del departamento de Bolívar, según la división oficial DIVIPOLA). El filtro se hizo por
  pertinencia regional (el autor cursa Ingeniería de Software en la Universidad de Cartagena) y por
  tamaño manejable para un notebook académico; no se alteró, complementó ni infirió ningún valor
  de las filas conservadas.
- **Catálogo oficial de variables consultado:** *"Sobre el dataset de casos de COVID-19 en Colombia"*,
  INS, disponible en <https://www.ins.gov.co/BibliotecaDigital/dataset-casos.pdf>. Los significados de
  columnas y códigos que se listan abajo provienen de ese documento oficial, no de una suposición.

## 2. Estructura general

- **Unidad de observación:** un caso confirmado de COVID-19 (una fila = una persona con caso positivo).
- **Cobertura temporal:** desde el inicio de la pandemia (2020) hasta la fecha de corte de la fuente.
- **Cobertura geográfica:** departamento de Bolívar y distrito de Cartagena de Indias.
- **Número de columnas:** 23 (todas heredadas de la fuente original, sin agregar columnas nuevas en el dataset crudo).

## 3. Variables originales

| # | Campo (nombre en el archivo) | Etiqueta oficial | Tipo (fuente) | Descripción / categorías oficiales |
|---|---|---|---|---|
| 1 | `fecha_reporte_web` | Fecha cargue web | Fecha (texto DD/MM/AAAA en la fuente; llega como texto ISO en la API) | Fecha en la que el caso se cargó al reporte web del INS. |
| 2 | `id_de_caso` | Caso | Número | Identificador consecutivo único del caso. |
| 3 | `fecha_de_notificaci_n` | Fecha Not | Fecha | Fecha de notificación del caso al sistema de vigilancia. |
| 4 | `departamento` | Departamento | Texto (código DIVIPOLA) | Código DIVIPOLA del departamento. Nota oficial: 13001 = Cartagena D.T. y C. (independiente de Bolívar). |
| 5 | `departamento_nom` | Departamento_nom | Texto | Nombre del departamento/distrito. |
| 6 | `ciudad_municipio` | Ciudad_municipio | Texto (código DIVIPOLA) | Código DIVIPOLA del municipio. |
| 7 | `ciudad_municipio_nom` | Ciudad_municipio_nom | Texto | Nombre del municipio. |
| 8 | `edad` | Edad | Número | Edad del paciente en el momento del reporte. |
| 9 | `unidad_medida` | unidad_medida | Texto/código | 1 = Años, 2 = Meses, 3 = Días. Indica en qué unidad está expresada `edad`. |
| 10 | `sexo` | Sexo | Texto | F = Femenino, M = Masculino. |
| 11 | `fuente_tipo_contagio` | Fuente_tipo_contagio | Texto | Relacionado, Importado, En estudio, Comunitario. |
| 12 | `ubicacion` | Ubicacion | Texto | Casa, Hospital, Hospital UCI, Fallecido, N/A (N/A = fallecido no COVID). |
| 13 | `estado` | Estado | Texto | Leve, Moderado, Grave, Fallecido, N/A. |
| 14 | `pais_viajo_1_cod` | Pais_viajo_1_cod | Texto | Código ISO del país de viaje reciente (si aplica). |
| 15 | `pais_viajo_1_nom` | Pais_viajo_1_nom | Texto | Nombre del país de viaje reciente (si aplica). |
| 16 | `recuperado` | Recuperado | Texto | Recuperado, Fallecido, N/A, o vacío. Vacío = caso activo. N/A = fallecido no COVID. |
| 17 | `fecha_inicio_sintomas` | Fecha_inicio_sintomas | Fecha | Vacío = caso asintomático. |
| 18 | `fecha_muerte` | Fecha_muerte | Fecha | Fecha declarada de fallecimiento (solo si aplica). |
| 19 | `fecha_diagnostico` | Fecha_diagnostico | Fecha | Fecha del diagnóstico positivo. |
| 20 | `fecha_recuperado` | Fecha_recuperado | Fecha | Fecha de recuperación (solo si aplica). |
| 21 | `tipo_recuperacion` | Tipo_recuperacion | Texto | PCR (segunda prueba negativa) o Tiempo clínico (21 días sin síntomas / sin hospitalización, en menores de 70 años). |
| 22 | `per_etn_` | Pertenencia étnica | Número/código | 1-Indígena, 2-ROM, 3-Raizal, 4-Palenquero, 5-Negro, 6-Otro. |
| 23 | `nom_grupo_` | Nombre del grupo étnico | Texto | Nombre textual del grupo étnico (cuando aplica). |

## 4. Notas oficiales relevantes para la limpieza (tomadas del documento del INS)

- Las celdas **vacías** en `recuperado` **no son un error**: representan casos activos. No deben
  tratarse igual que un dato faltante por error de captura.
- Las celdas **vacías** en `fecha_inicio_sintomas` representan casos **asintomáticos**, no un dato
  perdido.
- El valor `N/A` en `estado`, `ubicacion` y `recuperado` tiene un significado propio: **fallecido por
  causa distinta a COVID-19**, no debe confundirse con un nulo genérico.
- Pueden existir casos "Recuperado" con `ubicacion` = Hospital o Hospital UCI (permanecen
  hospitalizados por otra causa distinta a COVID). Esto **no es una inconsistencia**, es un caso real
  documentado por la fuente.
- `departamento_nom` presenta variaciones de mayúsculas/minúsculas para un mismo valor real
  (ej. "Cundinamarca" y "CUNDINAMARCA") en el dataset nacional completo; en el subconjunto
  filtrado se revisa si esto también ocurre para Bolívar/Cartagena.
- Cartagena se contabiliza como entidad territorial independiente de Bolívar (código DIVIPOLA
  13001), por lo cual el subconjunto trabajado corresponde a dos entidades territoriales distintas
  aunque geográficamente pertenezcan a la misma región Caribe.

Este diccionario reemplaza, para efectos de esta actividad, al diccionario de datos que normalmente
se retomaría de la Actividad 1.

## 5. Columnas del dataset preparado en la Actividad 2

El archivo `covid19_bolivar_cartagena_preparado.csv` tiene 33 columnas. En la Actividad 2 eliminé `pais_viajo_1_cod`, `pais_viajo_1_nom` y `nom_grupo_`, renombré `fecha_de_notificaci_n` a `fecha_notificacion`, `departamento` a `departamento_cod_divipola`, `ciudad_municipio` a `municipio_cod_divipola`, `ciudad_municipio_nom` a `municipio_nom` y `per_etn_` a `pertenencia_etnica_cod`, y convertí `fuente_tipo_contagio` en columnas binarias. Estas son las columnas que construí:

| Campo | Tipo | Definición |
|---|---|---|
| `edad_anios` | Número | Edad unificada en años según `unidad_medida`: meses / 12 y días / 365. |
| `edad_atipica_iqr` | Booleano | Verdadero si `edad_anios` supera Q3 + 1,5 × IQR. |
| `alerta_muerte_antes_diagnostico` | Booleano | Verdadero si `fecha_muerte` es anterior a `fecha_diagnostico`. |
| `dias_sintomas_a_diagnostico` | Número | Días entre `fecha_inicio_sintomas` y `fecha_diagnostico`. Vacío si el caso es asintomático o no tiene fecha de diagnóstico. |
| `dias_diagnostico_a_recuperacion` | Número | Días entre `fecha_diagnostico` y `fecha_recuperado`. |
| `recuperacion_prolongada` | Booleano | Verdadero si `dias_diagnostico_a_recuperacion` es mayor que 21. Cuando los días están vacíos queda en falso, por eso en la Actividad 3 solo se usa dentro de los casos recuperados. |
| `caso_activo` | Booleano | Verdadero si `recuperado` está vacío. |
| `nivel_riesgo` | Texto | Regla simple, no clínica oficial: Alto si `estado` es Grave o Fallecido o la edad es de 70 años o más; Medio si `estado` es Moderado o la edad es de 60 años o más; Bajo en otro caso. |
| `sexo_cod` | Entero | F = 0, M = 1. |
| `estado_severidad_cod` | Entero | Leve = 1, Moderado = 2, Grave = 3, Fallecido = 4. |
| `nivel_riesgo_cod` | Entero | Bajo = 0, Medio = 1, Alto = 2. |
| `contagio_Comunitaria`, `contagio_Importado`, `contagio_Relacionado` | Entero 0/1 | Codificación one-hot de `fuente_tipo_contagio`. En cada fila exactamente una vale 1. |

En la Actividad 2 también imputé con la moda, el código 6, los 37 valores faltantes de `pertenencia_etnica_cod`, y reconstruí como Fallecido el `estado`, la `ubicacion` y el campo `recuperado` de 1.090 registros que tenían `fecha_muerte`.
