# Proyecto Titanic: predicción de supervivencia

Proyecto reproducible de clasificación binaria con scikit-learn. Compara cinco modelos para predecir `Survived` y cumple los entregables indicados en el documento `3proyecto.pdf`.

## Resultado principal

El criterio definido sobre validación cruzada seleccionó **HistGradientBoosting**:

- F1 promedio de validación cruzada: `0.759 ± 0.033`.
- ROC-AUC promedio de validación cruzada: `0.877 ± 0.019`.
- F1 en prueba: `0.738`.
- ROC-AUC en prueba: `0.814`.
- Brecha F1 entrenamiento/prueba: `+0.160`.

La brecha indica sobreajuste. El resultado se presenta como comparación académica y no como modelo listo para producción.

## Estructura

```text
Proyecto3IA/
├── Titanic-Dataset.csv
├── Titanic_Supervivencia.ipynb
├── reporte_titanic.html
├── reporte_titanic.pdf
├── README.md
├── requirements.txt
├── VALIDACION_ENTREGABLES.md
└── resultados/
    ├── comparacion_modelos.csv
    ├── comparacion_modelos.png
    ├── comparacion_train_test.csv
    ├── comparacion_train_test.png
    ├── distribucion_survived.png
    ├── matriz_confusion.png
    ├── metricas_modelos.csv
    ├── resumen_ejecucion.json
    └── validacion_cruzada.csv
```

## Requisitos

- Python 3.11 o 3.12.
- El archivo `Titanic-Dataset.csv` en la misma carpeta que el notebook.
- Dependencias incluidas en `requirements.txt`.

No se requiere una GPU.

## Instalación en Windows

Abra PowerShell en la carpeta del proyecto.

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Si PowerShell bloquea la activación:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

## Uso con Jupyter

Inicie Jupyter:

```powershell
python -m jupyter notebook
```

Abra `Titanic_Supervivencia.ipynb` y seleccione **Kernel > Restart Kernel and Run All Cells**. El notebook debe ejecutar 16 celdas de código sin intervención adicional.

Para validar desde la terminal:

```powershell
python -m jupyter nbconvert `
  --execute `
  --to notebook `
  --inplace Titanic_Supervivencia.ipynb `
  --ExecutePreprocessor.timeout=600
```

## Uso en Google Colab

1. Abra [Google Colab](https://colab.research.google.com/).
2. Seleccione **Archivo > Subir notebook**.
3. Suba `Titanic_Supervivencia.ipynb`.
4. En el panel de archivos de Colab, suba `Titanic-Dataset.csv`.
5. Ejecute **Entorno de ejecución > Ejecutar todas**.
6. Descargue el notebook ejecutado desde **Archivo > Descargar > Descargar .ipynb**.

Las versiones de Colab pueden diferir de `requirements.txt`, pero el código usa interfaces compatibles de scikit-learn moderno.

## Flujo metodológico

1. Carga y validación del CSV.
2. Exploración de dimensiones, tipos, duplicados y faltantes.
3. Análisis de la distribución de `Survived`.
4. Selección de siete predictores.
5. Partición 80/20 estratificada con semilla `42`.
6. Preprocesamiento dentro de `ColumnTransformer` y `Pipeline`.
7. Validación cruzada estratificada de cinco folds.
8. Entrenamiento de cinco clasificadores.
9. Evaluación con exactitud, precisión, sensibilidad, F1 y ROC-AUC.
10. Selección por F1 promedio de validación cruzada.
11. Matriz de confusión y análisis train/test.
12. Exportación de resultados reproducibles.

## Variables

**Numéricas:** `Pclass`, `Age`, `SibSp`, `Parch`, `Fare`.

**Categóricas:** `Sex`, `Embarked`.

**Excluidas:**

- `PassengerId`: identificador.
- `Name`: texto de alta cardinalidad.
- `Ticket`: alta cardinalidad y formato heterogéneo.
- `Cabin`: 687 valores faltantes y alta cardinalidad.

## Preprocesamiento

- Numéricas: imputación por media y `StandardScaler`.
- Categóricas: imputación con `Unknown`.
- Codificación: `OneHotEncoder(handle_unknown="ignore")`.
- Todos los pasos se ajustan dentro del pipeline para evitar fuga de datos.

## Modelos comparados

- Regresión logística.
- K vecinos más cercanos.
- Árbol de decisión.
- Bosque aleatorio.
- HistGradientBoosting.

## Solución de problemas

### `No se encontró Titanic-Dataset.csv`

Compruebe que el CSV esté en la misma carpeta desde la que se ejecuta el notebook.

### `ModuleNotFoundError`

Active el entorno virtual e instale las dependencias:

```powershell
python -m pip install -r requirements.txt
```

### Error de kernel

Registre el entorno:

```powershell
python -m ipykernel install --user --name titanic-proyecto
```

Después seleccione el kernel `titanic-proyecto` en Jupyter.

### Ejecución lenta

La validación cruzada entrena cada pipeline cinco veces. En un equipo convencional debe tardar menos de algunos minutos.

### Resultados ligeramente distintos

Use las versiones de `requirements.txt`. La semilla está fijada, pero diferentes versiones numéricas pueden producir variaciones menores.

## Interpretación responsable

El dataset es pequeño e histórico. El modelo detecta asociaciones predictivas, no relaciones causales. No debe utilizarse para inferir por qué una persona sobrevivió ni para decisiones reales sin validación externa, análisis de sesgos y revisión de dominio.

## Segunda versión recomendada

- Crear `FamilySize` e `IsAlone`.
- Extraer títulos de `Name`.
- Derivar información de cubierta desde `Cabin`.
- Ajustar hiperparámetros mediante validación anidada.
- Calibrar probabilidades.
- Evaluar métricas por sexo y clase.

La evidencia completa de cumplimiento está en `VALIDACION_ENTREGABLES.md`.
