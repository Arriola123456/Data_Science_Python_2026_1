# Diccionario de Variables — `Final_Dataset_16_abril.xlsx`

Este documento describe el significado de cada variable incluida en el set
de datos, las preguntas de la encuesta que están detrás de las variables
derivadas y las decisiones metodológicas más relevantes para su correcta
interpretación.

---

## 1. Identificadores del Panel

| Variable | Descripción |
|----------|-------------|
| **`n`** | Número correlativo de observación (índice de fila). No tiene interpretación sustantiva. |
| **`id`** | Identificador único de cada celda del panel. Se construye concatenando el nombre de la región, el código de UBIGEO y el año (por ejemplo: `AMAZONAS_1_2024`). Es práctico para hacer cruces rápidos con otros datasets. |
| **`region`** | Nombre del departamento del Perú al que corresponde la observación (ej. *Lima*, *Arequipa*, *Áncash*). |
| **`ubigeo`** | Código numérico del departamento (1 al 25), equivalente a los dos primeros dígitos del UBIGEO oficial del INEI. Es la variable **transversal** (*cross-sectional*) del panel. |
| **`year`** | Año al que corresponde la observación. Es la variable **temporal** (*time-series*) del panel. Abarca el período 2004-2024. |

La estructura es, por tanto, un **panel balanceado** con 25 unidades
(departamentos) observadas durante 21 años.

---

## 2. Variables Principales

### 2.1 `mig_int` — Stock de migrantes internacionales

Número total de personas **nacidas fuera del Perú** que residen en cada
departamento en el año correspondiente. Sigue el estándar recomendado por
Naciones Unidas para la medición del stock de migrantes, basado en el
lugar de nacimiento (no en la nacionalidad ni en el tiempo de residencia
en el país).

#### Metodología por período

Dado que las preguntas disponibles en las encuestas cambian a lo largo
del tiempo, la variable se construyó combinando tres fuentes y
metodologías distintas:

| Período | Fuente | Método de identificación |
|---------|--------|--------------------------|
| **2007 y 2017** | Censos Nacionales de Población y Vivienda | Pregunta directa sobre país de nacimiento, aplicada al universo de la población. Es la fuente más confiable porque no se trata de una muestra y la pregunta pregunta por la nacionalidad/lugar de nacimiento de manera explícita. |
| **2004-2006 y 2008-2016** | ENAHO, Módulo 02 (Características de los Miembros del Hogar) | Combinación de **P208A1** y **P208A2**. |
| **2018-2024** | ENAHO Módulo 04 (Salud) + ENPOVE 2018/2022 + ENAHOPV 2024 | Criterio B (explicado más abajo), empalmado con encuestas dedicadas a la población venezolana. |

#### Detalle para 2004-2016 (excepto 2007)

En estos años, el Módulo 02 de la ENAHO sí contenía dos preguntas sobre
el lugar de nacimiento de cada miembro del hogar:

> **P208A1:** *¿Nació en este distrito?*
>
> **P208A2:** *¿En qué provincia y distrito nació?* (se registra el
> código de distrito).

Cuando la respuesta a P208A1 era "no", el entrevistador anotaba en
P208A2 el código del distrito de nacimiento. La ENAHO codifica
internamente el lugar de nacimiento con el UBIGEO oficial de seis
dígitos (`DDPPDD`), donde los dos primeros dígitos corresponden al
departamento peruano (rango válido `01` a `25`).

Los **nacidos en el extranjero** aparecen con códigos cuyos dos primeros
dígitos **no son un departamento peruano válido** (típicamente comienzan
en `00`, por ejemplo `004037`, `004009`, `006022`). Por lo tanto, el
migrante internacional en estos años se identifica como:

> Toda persona con **P208A2** registrada con un código que **no
> corresponde a un UBIGEO departamental peruano**.

El resultado se pondera con el factor de expansión poblacional de la
ENAHO (`FACPOB07`) y se agrega por departamento de residencia.

#### Detalle para 2018-2024 — Criterio B

A partir de 2018 la ENAHO **eliminó** las preguntas P208A1 y P208A2 del
Módulo 02. El único bloque migratorio que persiste en la ENAHO se
trasladó al Módulo 04 (Salud) y consta de cuatro preguntas:

> **P401F:** *Hace 5 años, ¿vivía en este distrito?* (1 = Sí, 2 = No,
> 3 = Aún no había nacido)
>
> **P401G:** *¿En qué distrito, provincia y departamento vivía hace 5
> años?*
>
> **P401G1:** *Cuando usted nació, ¿vivía su madre en este distrito?*
> (1 = Sí, 2 = No, 3 = No sabe)
>
> **P401G2:** *¿En qué distrito y provincia vivía su madre?* (al momento
> del nacimiento de la persona).

**¿Por qué se eligió el Criterio B (P401G1 / P401G2) y no P401F / P401G?**

1. **Invariancia en el tiempo.** La pregunta P401F ("vivía hace 5 años")
   tiene una **ventana móvil**: en la ENAHO 2018 captura llegadas del
   período 2013-2018, mientras que en la ENAHO 2024 captura llegadas
   del período 2019-2024. Esta diferencia hace que la serie **no sea
   comparable año a año**. En cambio, la pregunta por dónde vivía la
   madre al momento del nacimiento es un **atributo permanente** de la
   persona y produce una medida de stock estable.
2. **Cobertura de todos los grupos etarios.** El Criterio B incluye a
   los menores de 5 años. El Criterio A los excluye mecánicamente
   (`P401F = 3`).
3. **Cobertura de migrantes de cualquier antigüedad.** Un migrante que
   llegó al Perú hace 10 años aparece en el Criterio B pero no en el
   Criterio A.
4. **Alineación con el estándar ONU**, que define el stock por lugar de
   nacimiento y no por antigüedad de residencia.

El Criterio B identifica como migrante internacional a toda persona
cuya madre **residía fuera del Perú** al momento de su nacimiento. Esta
condición se captura en los microdatos cuando:

- **P401G1 tiene valor 2** (la madre no vivía en el distrito de la
  persona cuando ésta nació), **y**
- **P401G2 contiene un código cuyos dos primeros dígitos son `00`**,
  es decir, un código que no corresponde a ningún departamento peruano
  y que el INEI utiliza para identificar países extranjeros.

Es un **proxy**: se asume que si la madre vivía afuera al momento del
nacimiento, la persona nació afuera. Los casos atípicos (madre viajó al
Perú sólo para dar a luz, o viceversa) son raros y en general se
compensan.

#### Empalme con encuestas dedicadas

Para los años 2018, 2022 y 2024 el INEI levantó encuestas específicas
sobre la **población venezolana** residente en el Perú, cuya
representatividad en los departamentos seleccionados es muy superior a
la de la ENAHO:

| Año | Encuesta dedicada | Departamentos cubiertos |
|-----|--------------------|------------------------|
| 2018 | ENPOVE 2018 | 6 departamentos (Arequipa, Callao, Cusco, La Libertad, Lima, Tumbes) |
| 2022 | ENPOVE 2022 | 9 departamentos (Áncash, Arequipa, Callao, Ica, La Libertad, Lambayeque, Lima, Piura, Tumbes) |
| 2024 | ENAHOPV 2024 | 9 departamentos (Áncash, Arequipa, Callao, Ica, La Libertad, Lambayeque, Lima, Piura, Tumbes) |

La regla de empalme aplicada en estos años es:

- En los departamentos cubiertos por la encuesta dedicada, el stock se
  toma **íntegramente** de la ENPOVE / ENAHOPV, sumando ponderadamente
  a todos los residentes registrados (por diseño, todos ellos son
  migrantes).
- En los departamentos **no cubiertos** por la encuesta dedicada, el
  stock se calcula con la ENAHO aplicando el Criterio B.

#### Cuadro resumen — Stock de migrantes en Lima (departamento 15)

El cuadro siguiente corresponde al departamento de Lima, donde se
concentra el 85–90 % del stock nacional en todos los años. Para el
período 2018-2024 se reproducen los valores calculados con la
metodología descrita; para 2004-2017 los valores provienen de la fuente
censal o del método P208A1/P208A2, según el año.

| Año | Stock estimado en Lima | Fuente/método |
|-----|------------------------:|---------------|
| 2018 | 593 594 | ENPOVE 2018 (departamento cubierto) |
| 2019 | 201 631 | ENAHO, Criterio B |
| 2020 |  86 839 | ENAHO, Criterio B (⚠ disrupción COVID) |
| 2021 | 151 625 | ENAHO, Criterio B |
| 2022 | 742 078 | ENPOVE 2022 (departamento cubierto) |
| 2023 | 229 292 | ENAHO, Criterio B |
| 2024 | 759 111 | ENAHOPV 2024 (departamento cubierto) |

Se aprecia claramente un **patrón alternante**: los años con encuesta
dedicada (2018, 2022, 2024) muestran un stock muy superior al de los
años adyacentes que solo disponen de ENAHO. Esta forma de "serrucho"
**no refleja movimientos reales** del stock: es un artefacto del cambio
de instrumento entre años.

#### Posibles errores en el cálculo de `mig_int`

1. **Subdetección en los años con ENAHO sola (2019, 2020, 2021, 2023).**
   El marco muestral de la ENAHO cubre viviendas particulares con
   residentes habituales. Los hogares colectivos (hoteles, albergues,
   viviendas compartidas sin contrato estable) quedan fuera, y son
   justamente donde los migrantes recién llegados se concentran. Por
   eso los valores de 2019-2021 y 2023 son entre **3 y 5 veces**
   menores que los de 2018, 2022 y 2024.
2. **Disrupción del trabajo de campo en 2020.** La ENAHO 2020 tuvo un
   operativo parcialmente telefónico por la emergencia sanitaria; la
   muestra efectiva fue menor y la aplicación del Módulo 04 se vio
   afectada. El valor de Lima en 2020 (86 839) probablemente subestima
   severamente el stock real.
3. **Discontinuidad metodológica.** La serie combina tres instrumentos
   de naturaleza distinta (censos, ENAHO con P208A1/A2, ENAHO-M04 +
   ENPOVE/ENAHOPV). Los niveles **no son estrictamente comparables**
   entre períodos; las tendencias deben interpretarse con cautela.
4. **Carácter proxy del Criterio B (2018 en adelante).** La condición
   "la madre vivía afuera al momento del nacimiento" no es equivalente
   exacta a "nació afuera". Hay falsos positivos (madre que viajó a
   Perú para dar a luz) y falsos negativos (madre peruana con
   nacimiento accidental en el exterior), aunque el sesgo neto se
   estima pequeño.
5. **Codificación opaca de los países de nacimiento.** Los códigos
   `00XXXX` que identifican países extranjeros en P208A2 y P401G2 no
   están publicados en un diccionario público. La validación empírica
   (el código `004037` es con casi total certeza Venezuela, por
   frecuencia y contexto) es inevitable pero introduce incertidumbre.
6. **ENPOVE y ENAHOPV capturan exclusivamente a la población
   venezolana.** En los departamentos cubiertos por estas encuestas,
   los migrantes de otros orígenes (colombianos, chilenos, argentinos,
   estadounidenses, chinos, etc.) quedan **subcontados** respecto a la
   definición amplia de "migrante internacional". Esta pérdida es
   pequeña en magnitud (los venezolanos son la abrumadora mayoría en
   2018-2024), pero conceptualmente el instrumento ya no mide
   "stock de extranjeros" sino "stock venezolano + extranjeros no
   venezolanos captados por ENAHO en departamentos no cubiertos".
7. **Recalibraciones del factor de expansión.** Post-Censo 2017 el INEI
   revisó los factores de expansión de la ENAHO. Comparar niveles
   absolutos pre- y post-calibración puede introducir saltos
   artificiales.
8. **Diferencia Censo vs. encuesta muestral.** Los valores de 2007 y
   2017 provienen de un Censo (universo observado directamente); los
   demás años son estimaciones ponderadas de una muestra. La varianza
   estadística es estructuralmente distinta.

---

### 2.2 `housing_price` — Precio promedio de alquiler mensual por dormitorio

Mide el **precio promedio mensual, en soles corrientes, por habitación
de uso exclusivo para dormir**, en el departamento y año
correspondientes. No es el alquiler bruto de la vivienda completa sino
una medida normalizada por tamaño del inmueble.

La base de cálculo es la pregunta siguiente del Módulo 01 de la ENAHO
(Características de la Vivienda y del Hogar):

> **P106:** *Si usted alquilara esta vivienda, ¿cuánto cree que le
> pagarían de alquiler mensual (en S/.)?*

Esta pregunta se formula incluso a los hogares que no son inquilinos
(propietarios, ocupantes, etc.), lo que permite contar con una
valuación del mercado residencial amplio.

El cálculo sigue los siguientes pasos:

1. Se toma el alquiler estimado `P106` declarado por cada vivienda.
2. Se divide por el **número de habitaciones utilizadas exclusivamente
   para dormir**, según el propio Módulo 01 de la ENAHO. Esto permite
   comparar precios entre viviendas de tamaños distintos.
3. Se calcula el **promedio departamental** del precio por dormitorio,
   ponderado con el factor de expansión del hogar.

#### Restricciones muestrales aplicadas

Es importante notar que el precio **no se calcula sobre el universo
total de viviendas**. Se aplican dos filtros para mejorar la
comparabilidad y evitar sesgos por inmuebles no residenciales:

- **Sólo zonas urbanas.** Se excluyen las viviendas rurales porque el
  mercado de alquiler rural tiene dinámicas muy distintas y, en
  muchas regiones, es prácticamente inexistente o sin precios
  comparables.
- **Sólo casas independientes y departamentos.** Se excluyen viviendas
  improvisadas, cuartos en quinta, locales no construidos para
  habitar y otras categorías no estándar, que distorsionan la
  estimación del precio de mercado formal.

Por tanto, el valor refleja el precio del mercado urbano formal de
vivienda residencial, y no el costo promedio del total del parque
habitacional del departamento.

---

### 2.3 `housing_price_RMV2007` — Precio de vivienda deflactado en RMV de enero 2007

Precio de la vivienda expresado en términos de **Remuneraciones Mínimas
Vitales (RMV) a valores de enero de 2007**. Como la RMV de enero de
2007 era de **S/ 500**, la conversión consiste en dividir el precio
nominal de cada año por la RMV vigente en ese momento y multiplicar por
500 (o, equivalentemente, expresar el precio en múltiplos de la RMV de
2007).

#### Propósito

Permite **comparar el costo real de la vivienda a lo largo del tiempo**
neutralizando:

- La inflación general acumulada entre 2004 y 2024.
- El crecimiento del poder adquisitivo mínimo legal.

Una variación en `housing_price_RMV2007` indica un cambio **real** en
el costo de alquilar un dormitorio respecto al ingreso mínimo, mientras
que una variación en `housing_price` puede deberse a la mera
depreciación de la moneda.

---

## 3. Controles

Variables agregadas a nivel departamental y anual, utilizadas como
controles demográficos, sociales y económicos en los análisis.

| Variable | Descripción |
|----------|-------------|
| **`rurality`** | Porcentaje de la población del departamento que reside en zonas rurales (clasificación oficial del INEI). Valores entre 0 y 100. |
| **`median_age`** | Edad mediana de los habitantes del departamento. La mitad de la población tiene una edad menor y la otra mitad, una edad mayor que este valor. Es menos sensible a los extremos que la edad media. |
| **`share_old_pop`** | Proporción (o porcentaje) de la población que es **adulto mayor**. La definición estándar del INEI considera adulto mayor a la persona de 60 años o más. |
| **`avg_HHsize`** | Tamaño promedio del hogar en el departamento: número promedio de miembros por hogar. |
| **`share_workingage_pop`** | Proporción de la **Población en Edad de Trabajar (PET)**. En la definición oficial del INEI, la PET comprende a las personas de 14 años a más. |
| **`annual_median_income`** | Ingreso mediano anual (en soles corrientes) de los hogares (o de los individuos, según el nivel del cálculo). La mediana se prefiere sobre el promedio porque es robusta frente a valores extremos. |
| **`population`** | Población total estimada del departamento. Proviene de las proyecciones oficiales del INEI basadas en Censos y ENAHO. |
| **`poverty_rate`** | Porcentaje de la población del departamento clasificado como **pobre monetario**, según la línea de pobreza oficial del INEI (construida a partir del gasto per cápita del hogar frente a una canasta básica mínima). |

---

## 4. Covariables Educativas

Proporciones de la población del departamento según el **nivel
educativo alcanzado**, siguiendo las 11 categorías estándar derivadas
del Módulo 03 de la ENAHO (Educación). Las 11 variables suman
aproximadamente 1 (o 100 %, según la escala) en cada departamento-año.

| Variable | Nivel educativo alcanzado |
|----------|----------------------------|
| **`d_educ_1`** | Sin nivel |
| **`d_educ_2`** | Inicial |
| **`d_educ_3`** | Primaria incompleta |
| **`d_educ_4`** | Primaria completa |
| **`d_educ_5`** | Secundaria incompleta |
| **`d_educ_6`** | Secundaria completa |
| **`d_educ_7`** | Superior no universitaria incompleta |
| **`d_educ_8`** | Superior no universitaria completa |
| **`d_educ_9`** | Superior universitaria incompleta |
| **`d_educ_10`** | Superior universitaria completa |
| **`d_educ_11`** | Maestría / doctorado |

### Interpretación

- Los niveles **1 a 6** corresponden al ciclo de educación básica
  regular (inicial, primaria y secundaria).
- Los niveles **7 y 8** corresponden a institutos técnicos o pedagógicos
  (educación superior no universitaria).
- Los niveles **9 y 10** corresponden a la educación universitaria de
  pregrado.
- El nivel **11** agrupa la educación de posgrado (maestría y
  doctorado), que en la ENAHO suele tener muy pocas observaciones por
  departamento — los coeficientes asociados deben leerse con cautela.

Una lectura rápida de la distribución ayuda a caracterizar el capital
humano del departamento: una región con `d_educ_3` y `d_educ_5` altos
concentra su población en ciclos educativos incompletos, mientras que
`d_educ_10` y `d_educ_11` elevados indican presencia de capital humano
altamente calificado.
