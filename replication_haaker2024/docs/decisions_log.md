# Decisions Log: Replicación Haaker (2024)

Este documento registra cada decisión no trivial y cada discrepancia con el
paper, con su justificación o posible causa. Se actualiza fase por fase.

Referencia: Haaker, D. (2024). "The Effects of Health Insurance Expansion on
Domestic Violence: A Regression Discontinuity Analysis in Peru". Universidad
de Barcelona, Master in Institutions and Political Economy.

---

## Fase 0: Setup y lectura

### Resumen del diseño del paper

El paper estima, mediante un RDD nítido (sharp RDD), el efecto causal de ser
elegible para el Seguro Integral de Salud (SIS) sobre indicadores de violencia
doméstica y familiar contra la mujer en el Perú.

Supuestos y decisiones clave del diseño de la autora:

1. **Variable de asignación (running variable).** El Índice de Focalización de
   Hogares (IFH) reconstruido a partir de la ENDES 2012-2014 siguiendo la
   metodología SISFOH (2010). El IFH es una suma ponderada de características
   del hogar (materiales de vivienda, servicios básicos, hacinamiento,
   educación del jefe de hogar, tenencia de bienes, afiliación a seguro de los
   miembros, etc.). Los pesos dependen del área geográfica.

2. **Centrado y tratamiento.** El IFH se centra restando el umbral regional, de
   modo que la running variable queda centrada en 0. El tratamiento es
   elegibilidad: D = 1 si IFH_centrado <= 0 (el hogar cae bajo o en el umbral
   regional y por tanto es elegible para el SIS).

3. **Muestra.** Mujeres de 15 a 49 años, en área urbana, casadas o convivientes
   (unidas), que respondieron el módulo de violencia doméstica de la ENDES.
   Se excluyen las beneficiarias del programa Juntos (para evitar confusión con
   otra intervención de transferencias condicionadas). Pooled cross-section de
   los años 2012, 2013 y 2014, con variable indicadora de año. N objetivo
   aproximado: 20,421 mujeres en la muestra completa.

4. **Efectos fijos y clusters.** La especificación incluye efectos fijos de
   cluster y de año. Los clusters se definen como 15 conglomerados de distritos
   similares. El criterio exacto de agrupación se documentará como supuesto si
   no es plenamente reconstruible desde el texto (ver Fase 3).

5. **Especificación de estimación.** OLS local dentro del bandwidth óptimo por
   MSE, con dummy de elegibilidad, IFH, interacción IFH x elegibilidad,
   controles de edad y educación, y efectos fijos de cluster y de año:

   Y_ict = a + tau*D_ict + b1*IFH_ict + b2*(IFH_ict * D_ict) + theta_ict
           + phi_c + gamma_t + e_ict

   donde tau es el efecto de interés. Se reporta, junto a cada coeficiente, el
   N dentro del bandwidth, el bandwidth óptimo por MSE y la media local de la
   variable dependiente.

6. **Resultados de referencia.**
   - Muestra completa (Tablas II y III): efecto nulo en todos los outcomes.
     Sirve como control de calidad del pipeline.
   - Heterogeneidad por edad (Tabla IV): para mujeres 15-28, control behaviour
     cae 13.6 pp (sig. al 1%) y any domestic violence cae 9.8 pp (sig. al 5%).
   - Heterogeneidad por edad sobre violencia familiar (Tabla V): para 15-28,
     violence outside family cae 3.4 pp (sig. al 10%).
   - Heterogeneidad por educación (Tablas VI y VII): family violence cae 7.4 pp
     para sin/primaria y para superior.
   - Heterogeneidad por educación dentro de jóvenes 15-28 (Tablas VIII y IX):
     control behaviour -12.5 pp (secundaria) y -17.8 pp (superior).
   - Mecanismos (Tablas D.I a D.V): visita a establecimiento de salud, abuso de
     alcohol del esposo (sig. solo en superior, -12.7 pp), empoderamiento
     (última palabra sobre compras diarias, alrededor de -4.2 pp).
   - Validez (Tabla/Figura III, E.I, E.II): density test sin rechazo de
     manipulación, balance de covariables sin significancia, placebos con
     cutoffs aleatorios sin significancia.

### Estado del entorno (verificado en Fase 0)

- Stata NO está disponible en este entorno de ejecución (no hay binario
  `stata`, `stata-mp` ni `stata-se`).
- No están instalados los paquetes Python `rdrobust`, `rddensity` ni
  `pyreadstat`.
- No se detecta una skill/librería `inei-microdatos` instalada en el entorno.

Implicación: salvo que se habilite Stata, la estimación RDD se replicará en
Python con el paquete `rdrobust` de Python, documentando la equivalencia con la
especificación de la autora. Pendiente de confirmación del usuario (ver
preguntas abiertas abajo).

### Insumos pendientes solicitados al usuario (bloquean el avance)

Conforme a la Fase 0, antes de descargar cualquier dato se requieren dos
insumos que no se pueden generar sin información externa:

1. **Documento metodológico SISFOH (2010)** con la tabla completa de pesos del
   IFH por característica y por área geográfica, y los umbrales regionales. Sin
   esto NO se pueden poblar `weights/sisfoh_ifh_weights.csv` ni
   `weights/sisfoh_thresholds.csv`, y NO se debe inventar ningún peso ni umbral.

2. **Confirmación del stack de estimación**: Stata, Python o ambos (y, si se
   espera Stata, habilitarlo en el entorno, pues hoy no está disponible).

Hasta recibir estos insumos, el proyecto queda detenido en el criterio de
aceptación de la Fase 0.

### Decisiones de scaffolding

- Se creó la estructura de carpetas indicada en el prompt bajo
  `replication_haaker2024/`.
- Se usan rutas relativas al proyecto en todo el código.
- Convención de texto: sin em dashes; LaTeX con UTF-8 acentuado directo.
