# Stock de migrantes internacionales en el Perú, 2018-2024

Este directorio contiene el código y la documentación para estimar el
**stock anual de migrantes internacionales** (personas nacidas en el
extranjero que residen en el Perú) a nivel **nacional y departamental**
para el período **2018–2024**.

Fuente: microdatos del INEI, accedidos mediante el paquete
[`inei-microdatos`](https://github.com/fiorellarmartins/inei-microdatos).

---

## 1. Entregables

| Archivo | Descripción |
|---------|-------------|
| `calcular_stock_migrantes.py` | Script principal. Itera años 2018-2024, descarga módulos, aplica el criterio metodológico y exporta a Excel. |
| `stock_migrantes_2018_2024.xlsx` | Tabla final. Filas: 25 departamentos + total nacional. Columnas: años 2018-2024. |
| `downloads/` | Caché local de zips descargados del portal del INEI. |
| `README.md` | Este documento. |

---

## 2. Fuentes de datos

| Encuesta | Años | Cobertura | Rol en el cálculo |
|----------|------|-----------|-------------------|
| **ENAHO** (Encuesta Nacional de Hogares) | 2018-2024 | Nacional, todos los departamentos | Identificación de migrantes mediante Criterio B en años sin ENPOVE, y complemento en años con ENPOVE para departamentos no cubiertos. |
| **ENPOVE** (Población Venezolana) | 2018, 2022 | 6-8 departamentos seleccionados | En años cubiertos, sustituye a ENAHO en los departamentos del marco muestral. |
| **ENAHOPV** (ENAHO dirigida a población venezolana) | 2024 | 9 departamentos | Reemplaza a ENPOVE en 2024. Misma lógica de empalme. |

---

## 3. Definición operacional

**Migrante internacional** = persona nacida fuera del Perú residente en el
territorio nacional al momento de la encuesta. Siguiendo el estándar ONU,
la medida de *stock* se basa en el lugar de nacimiento, no en la
nacionalidad ni en el año de llegada.

### 3.1 Criterio B (ENAHO)

La ENAHO no tiene pregunta directa de país de nacimiento, pero el Módulo
04 (Salud) contiene dos variables que, combinadas, sirven como proxy
robusto:

| Variable | Pregunta |
|----------|----------|
| `P401G1` | ¿Su madre vivía en este distrito cuando Ud. nació? (1=Sí, 2=No, 3=No sabe) |
| `P401G2` | UBIGEO donde vivía la madre al nacer la persona |

El INEI codifica a los países extranjeros en `P401G2` con prefijo **`00`**
(el dígito inicial `00` no es un código departamental válido del UBIGEO
peruano, que va de `01` a `25`).

**Criterio B operacional:**

```
migrante_i  =  (P401G1_i == 2)  AND  (zfill6(P401G2_i).str[:2] == '00')
```

Se eligió por encima de los alternativos (por ejemplo `P401F`/`P401G`,
"vivía hace 5 años") porque:

- es **invariante en el tiempo**: "nacer en el extranjero" es una
  característica permanente, mientras que "vivir afuera hace 5 años" tiene
  una ventana móvil que produce series no comparables entre años;
- **incluye a los niños**: no excluye a menores de 5 años;
- **cubre migrantes de cualquier antigüedad** en el país.

### 3.2 ENPOVE / ENAHOPV

Estas encuestas están **dirigidas íntegramente a la población venezolana
residente en el Perú**; por construcción cada residente registrado es un
migrante internacional. El cálculo es directo:

```
stock_dept  =  Σ  factor_expansión_i     (i en residentes del dept.)
```

- ENPOVE 2018 → variable `FACTOR_DE_EXPANSION`.
- ENPOVE 2022 → variable `factorfinal`.
- ENAHOPV 2024 → variable `FACTOR`.

---

## 4. Metodología de empalme

Para cada año `t ∈ {2018, 2019, …, 2024}`:

### Si `t ∈ {2018, 2022, 2024}` (años con encuesta dedicada)

1. Se carga la ENPOVE/ENAHOPV del año `t` y se agrega por departamento
   usando su propio factor de expansión → `stock_pv_t`.
2. Se identifica el conjunto `D_PV(t)` de departamentos presentes en la
   muestra de esa encuesta dedicada.
3. Se carga la ENAHO del mismo año `t`, se aplica el Criterio B y se
   agrega por departamento → `stock_enaho_t`.
4. El stock final por departamento `d` es:

   ```
   stock_t(d)  =  stock_pv_t(d)           si d ∈ D_PV(t)
                  stock_enaho_t(d)        en otro caso
   ```

### Si `t ∈ {2019, 2020, 2021, 2023}` (años sin ENPOVE/ENAHOPV)

```
stock_t(d)  =  stock_enaho_t(d)    ∀ d  (Criterio B, ENAHO)
```

### Agregación

La tabla final se construye por departamento (UBIGEO de 2 dígitos). La
columna `Departamento` se construye mapeando `UBIGEO[:2]` a los 25
departamentos del Perú. La fila `TOTAL NACIONAL` es la suma vertical.

---

## 5. Decisiones de ponderación

- Se utiliza el **factor de expansión poblacional** de cada encuesta, sin
  recalibraciones externas.
- En ENAHO se usa **`FACPOB07`** del Módulo 02 (factor a nivel persona).
  Se mergea con el Módulo 04 por las llaves
  `{AÑO, CONGLOME, VIVIENDA, HOGAR, CODPERSO}`.
- El nivel de agregación "departamento" se obtiene de los **primeros
  dos dígitos de UBIGEO** (codificación oficial INEI, `01`–`25`).

---

## 6. Supuestos y limitaciones

1. **Proxy vs definición directa.** Criterio B asume que si la madre
   vivía en el extranjero al momento del nacimiento, la persona también
   nació allí. Los falsos positivos (madre extranjera que viajó a Perú
   para dar a luz) y falsos negativos (madre peruana con nacimiento en
   el exterior) son raros y compensatorios.
2. **Cobertura.** La ENAHO representa a la población en viviendas
   particulares residentes habituales. Excluye hogares colectivos
   (albergues, hoteles, instituciones). Esto subestima la población
   migrante recién llegada sin vivienda estable.
3. **Representatividad de ENPOVE/ENAHOPV.** Se asume, por instrucción
   metodológica, que en los departamentos cubiertos la encuesta agota el
   universo de migrantes (no hay doble conteo ni omisión).
4. **ENAHO 2020.** El trabajo de campo fue parcialmente interrumpido y
   reemplazado por recolección telefónica por COVID-19. Las cifras de
   2020 deben interpretarse con esa salvedad.
5. **Sin identificación de país de origen.** El Criterio B detecta
   "nacido en el extranjero" pero no discrimina nacionalidades. Los
   códigos `00XXXX` de P401G2 del INEI no están publicados en un
   diccionario accesible.

---

## 7. Cómo reproducir los resultados

### Requisitos

```bash
pip install inei-microdatos pandas openpyxl
```

Python 3.9 o superior. En la primera corrida se descargan ~1.5 GB de
módulos del portal del INEI; corridas posteriores usan la caché local
(`downloads/`).

### Ejecución

```bash
python calcular_stock_migrantes.py
```

El script imprime el progreso año por año y, al finalizar, guarda el
Excel `stock_migrantes_2018_2024.xlsx` en el mismo directorio.

---

## 8. Estructura del Excel de salida

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `Departamento` | texto | Nombre del departamento (o `TOTAL NACIONAL`) |
| `2018` … `2024` | entero | Stock estimado de migrantes internacionales |

Una sola hoja, `Stock Migrantes`, con 26 filas (25 departamentos + total).
