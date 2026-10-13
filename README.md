# StreamView Analytics

Proyecto de análisis y visualización del catálogo de contenidos de StreamView. El
repositorio contiene el flujo completo de trabajo: revisión de calidad,
limpieza e integración de datos, análisis exploratorio, cálculo de indicadores
clave (KPIs) y un modelo analítico consumido por un informe de Power BI.

## Contenido del proyecto

```text
.
├── data/
│   └── processed/              # Datos limpios y modelo analítico en CSV
├── dashboard/
│   ├── Entrega_2.pbix         # Informe de Power BI
│   └── tema_streamview.json   # Tema visual del informe
├── images/                     # Recursos gráficos del proyecto
├── notebooks/
│   ├── 01_revision_datos.ipynb
│   ├── 02_limpieza_integracion.ipynb
│   ├── 03_analisis_exploratorio.ipynb
│   ├── 04_kpis.ipynb
│   └── 05_integracion_modelo.ipynb
└── src/                        # Espacio reservado para código reutilizable
```

### Notebooks

Los notebooks deben ejecutarse en el orden indicado:

1. **`01_revision_datos.ipynb`**: carga las fuentes originales y revisa
   dimensiones, columnas, consistencia, duplicados y disponibilidad de campos.
2. **`02_limpieza_integracion.ipynb`**: depura películas y series, realiza
   validaciones de integridad y guarda los datasets procesados sin modificar
   las fuentes originales.
3. **`03_analisis_exploratorio.ipynb`**: analiza la distribución temporal del
   catálogo, géneros, popularidad, valoraciones, clasificación, duración y
   relaciones entre variables.
4. **`04_kpis.ipynb`**: calcula los indicadores del catálogo, valida los
   archivos finales y genera `kpis_catalogo.csv`.
5. **`05_integracion_modelo.ipynb`**: está reservado para la integración del
   modelo; actualmente no contiene celdas ejecutables. El modelo utilizado por
   el dashboard está materializado en los CSV de `data/processed/`.

### Datos procesados

Los archivos incluidos en `data/processed/` representan el modelo analítico
usado por el dashboard:

- `peliculas_procesadas.csv` y `series_procesadas.csv`: tablas principales de
  películas y series.
- `fact_contenidos.csv`: tabla de hechos unificada del catálogo.
- `dim_genero.csv`, `dim_genero_agrupado.csv`, `dim_pais.csv` y
  `dim_idioma.csv`: dimensiones descriptivas.
- `bridge_genero.csv` y `bridge_pais.csv`: tablas puente para las relaciones
  de contenido con géneros y países.
- `kpis_catalogo.csv`: resumen de KPIs por tipo de contenido.

El conjunto procesado incluido contiene 31.991 registros en `fact_contenidos`,
16.000 películas y 15.991 series. Las cifras pueden cambiar si se regeneran
los archivos a partir de una versión diferente de las fuentes originales.

## Requisitos

### Para ejecutar los notebooks

- Python 3.9 o superior.
- Jupyter Notebook o JupyterLab.
- Paquetes de Python:
  - `pandas`
  - `numpy`
  - `matplotlib`

### Para consultar el dashboard

- [Power BI Desktop](https://powerbi.microsoft.com/desktop/), preferentemente
  en Windows.
- Acceso a los CSV de `data/processed/`.

## Instalación

Desde la raíz del repositorio, crea un entorno virtual e instala las
dependencias:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install pandas numpy matplotlib jupyter
```

En Windows PowerShell, activa el entorno con:

```powershell
.\.venv\Scripts\Activate.ps1
```

## Ejecución desde cero

Los notebooks 01 y 02 esperan las fuentes originales en `data/` con estos
nombres exactos:

```text
data/netflix_movies_detailed_up_to_2025.csv
data/netflix_tv_shows_detailed_up_to_2025.csv
```
> [!NOTE]   
> **Nota sobre fuentes originales de data:** estos archivos corresponden a la data
> entregada por el docente para realizar el encargo. Por tratarse de material
> proporcionado para la actividad académica (de cambio constante y/o de consumo externo), normalmente no se suben ni se
> versionan en los repositorios de proyecto de datos. Todo esto para mantener y adoptar las buenas practicas de industria.

Una vez disponibles:

1. Abre el proyecto desde su carpeta raíz.
2. Inicia Jupyter:

   ```bash
   jupyter lab
   ```

3. Abre y ejecuta los notebooks en este orden:
   `01_revision_datos.ipynb`, `02_limpieza_integracion.ipynb`,
   `03_analisis_exploratorio.ipynb` y `04_kpis.ipynb`.
4. Comprueba que los archivos generados estén en `data/processed/`.
5. Abre `dashboard/Entrega_2.pbix` en Power BI Desktop y actualiza las
   consultas si Power BI solicita la ubicación de los archivos CSV.

Los notebooks están preparados para resolver las rutas relativas tanto si se
ejecutan desde la raíz del proyecto como desde `notebooks/`.

## Ejecución utilizando los datos incluidos

Si no necesitas regenerar los datos desde las fuentes originales, puedes:

1. Crear el entorno e instalar las dependencias descritas anteriormente.
2. Ejecutar directamente `03_analisis_exploratorio.ipynb` para revisar el
   análisis sobre los datos procesados.
3. Ejecutar `04_kpis.ipynb` para recalcular y validar `kpis_catalogo.csv`.
4. Abrir `dashboard/Entrega_2.pbix` para consultar el informe.

## Actualización del dashboard

El informe utiliza los archivos CSV del directorio `data/processed/`. Para
actualizarlo después de una nueva ejecución del flujo:

1. Regenera los archivos procesados con los notebooks.
2. Abre `dashboard/Entrega_2.pbix`.
3. Si las consultas muestran rutas no encontradas, corrige las rutas para que
   apunten a la copia local de `data/processed/`.
4. Selecciona **Actualizar** en Power BI Desktop.
5. Si es necesario, aplica `dashboard/tema_streamview.json` desde
   **Vista > Temas > Examinar temas**.

## Consideraciones de calidad y reproducibilidad

- Las fuentes originales se leen desde `data/` y los resultados se escriben en
  `data/processed/`, evitando sobrescribir los archivos de entrada.
- El flujo valida la unicidad de `show_id`, la cantidad de registros y algunos
  rangos de años, valoraciones y fechas.
- Las columnas de duración no deben interpretarse de la misma forma para
  películas y series; el análisis exploratorio documenta esta limitación.
- Los archivos `.pbix` requieren Power BI Desktop y no pueden ejecutarse
  directamente desde Jupyter.

## Estado del proyecto

El dashboard y los datos procesados están incluidos como entregables listos
para revisión. La carpeta `src/` queda disponible para futuras
transformaciones reutilizables o automatización del pipeline.