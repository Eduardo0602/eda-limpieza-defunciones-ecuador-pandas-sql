# CLAUDE.md — Contexto del Proyecto
# EDA y Limpieza de Datos: Defunciones Generales Ecuador 2021
# Última actualización: [10/03/2026]

---

> Las reglas universales, mi perfil y el comportamiento esperado están en:
> `~/proyectos/CLAUDE.md` (CLAUDE.md global)
> Este archivo contiene SOLO el contexto específico de este proyecto.

---

## IDENTIFICACIÓN

| Campo | Valor |
|---|---|
| **Nombre del repositorio** | `eda-limpieza-defunciones-ecuador-pandas-sql` |
| **Fase del portafolio** | Fase 0 |
| **Proyecto número** | Proyecto 0.2 |
| **Fecha de inicio** | [fecha de hoy] |
| **Fecha objetivo de cierre** | [indistinto] |

---

## OBJETIVO DEL PROYECTO

Análisis Exploratorio de Datos completo sobre el registro de defunciones
generales de Ecuador 2021 (INEC). El objetivo NO es modelar — es demostrar
que sé convertir datos sucios en datos confiables y documentar ese proceso
con rigor matemático. Incluye auditoría de calidad, pipeline de limpieza
reproducible, y análisis con Pandas + SQL.

---

## DATASET

| Campo | Valor |
|---|---|
| **Nombre** | Registro Estadístico de Defunciones Generales 2021 |
| **Fuente** | INEC Ecuador — descargado desde anda.inec.gob.ec/anda/index.php/catalog/930 |
| **Tamaño** | 107,649 filas × 45 columnas |
| **Separador** | Punto y coma (;) |
| **Archivo principal** | `data/raw/EDG_2021_act_CSV.csv` |
| **Diccionario** | `data/raw/Diccionario_de_variables_EDG_2021.xlsx` |
| **Notas** | Datos de registros administrativos — problemas reales de calidad esperados. Incluye impacto COVID-19. Códigos CIE-10 para causas de muerte. |

---

## ESTADO ACTUAL

- **Última sesión:** [10/03/2026]
- **Próximo paso exacto:** Crear notebook 02_pipeline_limpieza.ipynb — resolver los 9 problemas de calidad en orden de severidad
- **Bloqueado en:** Nada

---

## DECISIONES TÉCNICAS TOMADAS

- Separador del CSV es `;` — usar `pd.read_csv(..., sep=";")` siempre
- Notebooks los creo yo en Jupyter, no Claude Code (lección del Proyecto 0.1)
- Git commits desde Claude Code directamente (no mezclar con Git Bash)
- Usar slugify() para nombres de figuras desde el inicio (lección del Proyecto 0.1)

---

## ESTRUCTURA DE ARCHIVOS

- Datos crudos en: `data/raw/EDG_2021_act_CSV.csv`
- Diccionario en: `data/raw/Diccionario_de_variables_EDG_2021.xlsx`
- Datos limpios irán en: `data/processed/`
- Funciones de limpieza irán en: `src/data.py`
- Visualizaciones irán en: `src/visualization.py`

---

## LECCIONES DEL PROYECTO 0.1

- Claude Code es frágil editando notebooks — cambios en .ipynb los hago yo en Jupyter
- Claude Code funciona bien para: crear .py, ejecutar comandos, auditar, editar texto plano
- No dejar figuras duplicadas: usar slugify() desde el inicio
- Hacer git commit y push desde Claude Code directamente

---

## NOTEBOOKS PLANIFICADOS

1. `01_auditoria_calidad.ipynb` — Inspección inicial + mapa de nulos + tabla de auditoría
2. `02_pipeline_limpieza.ipynb` — Función limpiar_dataset() + comparación antes/después
3. `03_eda_sql.ipynb` — Análisis exploratorio + consultas SQL + visualizaciones

---

## CONTEXTO GLOBAL

Ver perfil completo en: `../CLAUDE.md` (directorio padre)