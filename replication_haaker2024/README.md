# Replicación de Haaker (2024)

Replicación del RDD nítido de Haaker, D. (2024), "The Effects of Health
Insurance Expansion on Domestic Violence: A Regression Discontinuity Analysis
in Peru" (Universidad de Barcelona, Master in Institutions and Political
Economy), usando microdatos ENDES 2012-2014.

## Estado

En curso. Fase 0 (setup) completada salvo dos insumos que dependen del usuario
(documento SISFOH con pesos y umbrales del IFH, y confirmación del stack de
estimación). Ver `docs/decisions_log.md`.

## Estructura

```
replication_haaker2024/
  README.md
  data/raw/         microdatos ENDES descargados, sin tocar
  data/interim/     archivos intermedios por modulo y anio
  data/processed/   dataset analitico final (panel pooled 2012-2014)
  src/              scripts del pipeline (01..08)
  weights/          pesos SISFOH del IFH y umbrales regionales (a poblar)
  output/tables/    tablas reproducidas
  output/figures/   RD plots y density
  output/logs/      logs de ejecucion
  docs/             decisions_log.md y variable_mapping.md
```

## Pipeline (previsto)

1. `src/01_download_endes.py` descarga ENDES 2012-2014.
2. `src/02_build_ifh.py` reconstruye el IFH y lo centra en el umbral regional.
3. `src/03_build_sample_and_outcomes.py` construye muestra, outcomes y covariables.
4. `src/04_rdd_main` RDD principal (Tablas II y III).
5. `src/05_rdd_heterogeneity` heterogeneidad (Tablas IV-IX).
6. `src/06_mechanisms` mecanismos (Tablas D.I-D.V).
7. `src/07_validity_robustness` validez y robustez del paper.
8. `src/08_compare_to_paper` comparación paper vs réplica.

## Dependencias

Pendiente de confirmar el stack (Stata con `rdrobust`/`rddensity`, o Python con
`rdrobust`). En este entorno Stata no está disponible; ver `docs/decisions_log.md`.

## Fuentes de datos

ENDES (Encuesta Demográfica y de Salud Familiar), INEI Perú, años 2012, 2013 y
2014. Microdatos públicos.
