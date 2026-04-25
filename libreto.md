# Libreto — Video explicativo `emergency_access_peru`

**Duración objetivo:** ≤ 4 minutos (240 s)
**Ritmo de habla:** ~150 palabras por minuto (técnico/explicativo)
**Presupuesto narrado total:** ~570 palabras (deja margen para clicks, pausas y transiciones)

---

## ✅ Los 5 puntos obligatorios que el video cubre

> Estos son los puntos que el rubric exige; cada uno tiene un bloque dedicado abajo.

| # | Tema requerido | Bloque del libreto | Tiempo |
|---|---|---|---|
| 1 | **Brief explanation of the repository structure** | Bloque 2 | 0:15 – 0:45 |
| 2 | **Explanation of your methodology** | Bloque 3 | 0:45 – 1:25 |
| 3 | **The pipeline or main scripts** | Bloque 4 | 1:25 – 2:10 |
| 4 | **The Streamlit app running** | Bloque 5 | 2:10 – 3:10 |
| 5 | **The main outputs** | Bloque 6 | 3:10 – 3:45 |

Plus una **intro** (0:00–0:15) y un **cierre con limitaciones** (3:45–4:00) para enmarcar la presentación.

---

## 📋 Estructura completa en 7 bloques

| # | Tiempo | Duración | Sección | Palabras | ¿Qué se muestra en pantalla? | ¿Cubre punto? |
|---|---|---|---|---|---|---|
| 1 | 0:00 – 0:15 | 15 s | Intro | ~38 | Título del repo en GitHub o README en VSCode | — (encuadre) |
| 2 | 0:15 – 0:45 | 30 s | Estructura del repositorio | ~75 | VSCode con árbol de carpetas expandido | **#1** |
| 3 | 0:45 – 1:25 | 40 s | Metodología | ~100 | Tab *Datos y Metodología* § 6 (fórmulas LaTeX) | **#2** |
| 4 | 1:25 – 2:10 | 45 s | Pipeline / scripts principales | ~110 | Carpeta `src/` + terminal con `python -m src.*` | **#3** |
| 5 | 2:10 – 3:10 | 60 s | App Streamlit corriendo | ~135 | Tour por los 4 tabs en `localhost:8501` | **#4** |
| 6 | 3:10 – 3:45 | 35 s | Outputs principales | ~85 | Gráfico 1 + sección Hallazgos del README | **#5** |
| 7 | 3:45 – 4:00 | 15 s | Cierre + limitaciones | ~33 | README sección *Limitaciones* | — (cierre) |

**Total:** ~575 palabras a 150 wpm → **3:50–4:00 minutos** con margen para clicks y pausas.

---

## 🎬 Guión completo

### 1 · Intro · `0:00 – 0:15` (~38 palabras)

> **📺 Pantalla:** abre el repo `emergency_access_peru` en GitHub (o el README en VSCode). Mantén el título visible.

> **🎙 Narración:**
>
> "Hola. Este es mi proyecto de Tarea 2: un **análisis geoespacial de la desigualdad en el acceso a servicios de emergencia** en salud entre los 1 873 distritos del Perú. En 4 minutos te explico cómo está construido y qué encontré."

---

### 2 · Estructura del repositorio · `0:15 – 0:45` ✅ punto #1 (~75 palabras)

> **📺 Pantalla:** VSCode con el árbol de carpetas expandido. Apunta con el cursor a `app.py` en la raíz, después a `src/`, `data/`, `output/`, `docs/`.

> **🎙 Narración:**
>
> "El repositorio se organiza así: `app.py` en la raíz es la app Streamlit. La carpeta `src/` contiene los cinco módulos del pipeline — data_loader, cleaning, geospatial, metrics y visualization. Los datos crudos van a `data/raw/` — gitignorados — y los limpios a `data/processed/` como parquet. Todas las salidas — seis figuras, cinco mapas y tres tablas CSV — quedan en `output/`. La metodología y el diccionario de datos viven en `docs/`, y la FAQ completa está en el README."

---

### 3 · Metodología · `0:45 – 1:25` ✅ punto #2 (~100 palabras)

> **📺 Pantalla:** click al tab **📚 Datos y Metodología** de la app. Scroll a la **sección 6** — *Cálculo del índice*. Mantén visibles las fórmulas LaTeX renderizadas (especialmente la 6.1).

> **🎙 Narración:**
>
> "La metodología combina **tres dimensiones por distrito**: oferta — densidad de IPRESS de emergencia por kilómetro cuadrado; actividad — total de atenciones reportadas a SUSALUD en 2024; y acceso — fracción de centros poblados a 30 kilómetros o menos de un IPRESS de emergencia. Cada dimensión la normalizo a `[0, 1]` con **min-max**, usando `log1p` para las dos primeras por estar muy sesgadas. El **índice de cobertura** es el promedio simple de las tres: vive entre 0 y 1, donde **1 es el mejor atendido**. También construí una especificación alternativa con umbral de 15 kilómetros para medir sensibilidad."

---

### 4 · Pipeline y scripts principales · `1:25 – 2:10` ✅ punto #3 (~110 palabras)

> **📺 Pantalla:** vuelve a VSCode y abre brevemente `src/data_loader.py`, `src/cleaning.py`, `src/geospatial.py`. Después abre la terminal con el comando `python -m src.cleaning` (no hace falta correrlo de nuevo, solo mostrarlo).

> **🎙 Narración:**
>
> "El pipeline son cinco scripts en `src/` que corren en orden. **`data_loader.py`** descarga los cuatro datasets — MINSA, SUSALUD, INEI y el repo del curso. **`cleaning.py`** los limpia: arregla un bug del MINSA donde las columnas NORTE y ESTE están **invertidas**, parsea el marcador de anonimización `NE_0001` de SUSALUD como nulo, y deriva el UBIGEO desde el campo `CÓDIGO` del INEI. Después **`geospatial.py`** hace los GeoDataFrames, reproyecta a UTM 18S, y con un **KDTree** calcula la distancia desde cada uno de los 136 mil centros poblados al IPRESS de emergencia más cercano. **`metrics.py`** calcula el índice; **`visualization.py`** genera figuras y mapas."

---

### 5 · App Streamlit corriendo · `2:10 – 3:10` ✅ punto #4 (~135 palabras)

> **📺 Pantalla:** ya con la app abierta en `localhost:8501`, haz un **tour rápido por los 4 tabs**: empieza en *Datos y Metodología*, después *Análisis Estático* (zoom al gráfico 1), después *Resultados Geoespaciales*, y termina en *Exploración Interactiva* moviendo el slider y cambiando el departamento.

> **🎙 Narración:**
>
> "La app Streamlit es el entregable interactivo. Se levanta con `streamlit run app.py` y tiene **cuatro tabs**.
>
> **Datos y Metodología** documenta todo: motivación, las cuatro preguntas de investigación, decisiones metodológicas numeradas **D1 a D12**, fórmulas LaTeX explícitas y limitaciones.
>
> **Análisis Estático** tiene seis figuras con narrativa interpretativa debajo. La principal es el gráfico 1 — un **histograma bimodal** del índice con tabla integrada que lista distritos aislados totales y los del top 10 %.
>
> **Resultados Geoespaciales** embebe el mapa folium interactivo con los 787 IPRESS de emergencia clusterizados.
>
> **Exploración Interactiva** permite filtrar por departamento, alternar entre baseline y alternativa y mover un slider de top N. El mapa está **cacheado**: cambiar filtros es instantáneo."

---

### 6 · Outputs principales · `3:10 – 3:45` ✅ punto #5 (~85 palabras)

> **📺 Pantalla:** vuelve al tab *Análisis Estático*, gráfico 1 (histograma bimodal con tabla). Si te queda tiempo, scroll rápido al gráfico 3 (descomposición) y luego al gráfico 6 (baseline vs alternativa).

> **🎙 Narración:**
>
> "Los **outputs principales**: el **78 % de distritos** del Perú no tiene IPRESS de emergencia propio. La distribución del índice es bimodal — 479 distritos en cero, 632 en un tercio. La mediana de distancia desde un centro poblado al IPRESS de emergencia más cercano es **23 kilómetros**; el máximo, 377. Los mejor atendidos son **Jesús María, Bellavista y Arequipa cercado**, entre 0.94 y 0.97. La correlación de Spearman entre las dos especificaciones es **0.891**: las conclusiones cualitativas son robustas."

---

### 7 · Cierre + limitaciones · `3:45 – 4:00` (~33 palabras)

> **📺 Pantalla:** vuelve al README del repo (sección *Limitaciones*) o al título.

> **🎙 Narración:**
>
> "Las principales limitaciones: distancia **euclidiana** en vez de tiempo real, **peso uniforme** por centro poblado y un solo año. Todo el código y las decisiones están documentados en el repositorio. Gracias."

---

## 🧪 Tips de grabación

1. **Practica leyendo en voz alta con cronómetro.** Si te pasas, recorta en el bloque 5 (Streamlit) — es el más flexible.
2. **Pre-carga todos los tabs de la app** antes de grabar (especialmente *Exploración Interactiva* con un filtro aplicado) para que el caché de folium ya esté caliente y el mapa aparezca instantáneo.
3. **Resolución 1920 × 1080** mínimo para que el texto y las fórmulas se lean bien.
4. **Zoom / resaltado** cuando hables de:
   - Fórmulas LaTeX (§ 6 del tab *Datos*) — zoomea al bloque renderizado.
   - Gráfico 1 de *Análisis Estático* — la tabla integrada es el punto fuerte.
   - Los dos pileups en `x = 0` y `x = 1/3`.
5. **Si grabas pantalla y audio por separado**, sincroniza con claqueta al inicio: un click visible y audible.
6. **Desactiva notificaciones**, cierra tabs ajenas y otras apps visibles.
7. **Reinicia Streamlit antes de grabar.** Si la app está corriendo de antes y hubo cambios al código o a los archivos en `output/`, el caché de `@st.cache_data` puede mostrar errores stale. `Ctrl + C` en la terminal y volver a lanzar `.venv\Scripts\streamlit.exe run app.py` limpia todo.

---

## ✅ Checklist pre-grabación

- [ ] App Streamlit **recién reiniciada** y corriendo sin errores en `localhost:8501`
- [ ] Los 4 tabs cargados al menos una vez (parquets en caché)
- [ ] Tab *Exploración Interactiva* precargado con un filtro aplicado (mapa folium en caché)
- [ ] VSCode abierto en el repo con `app.py` visible en la raíz **y** el árbol de `src/` expandido (data_loader, cleaning, geospatial, metrics, visualization)
- [ ] Carpetas `output/figures/`, `output/maps/`, `output/tables/` con sus archivos generados (verificable con `ls output/*/`)
- [ ] Archivo `docs/data_dictionary.md` accesible (entregable explícito de la T1)
- [ ] README abierto en otra pestaña (para el cierre)
- [ ] Repo `emergency_access_peru` en GitHub abierto en una tercera pestaña (para la intro)
- [ ] Micrófono probado sin ruido de fondo
- [ ] Notificaciones de Windows / Slack / correo silenciadas
- [ ] Cronómetro visible o grabador con HUD de tiempo

---

## ✂️ Versión abreviada (si te pasas del tiempo)

| Si estás sobrado | Corta de aquí |
|---|---|
| Bloque 4 (pipeline) | Quita el detalle de los *quirks* (`NE_0001`, NORTE/ESTE) — solo menciona los 5 scripts en orden |
| Bloque 5 (Streamlit) | Salta los detalles de cada tab y solo demuestra los 4 con clicks rápidos |
| Bloque 6 (outputs) | Quita los nombres específicos de distritos; deja solo "78 %", "23 km" y "0.891" |

---

## 🎯 Mensajes clave que **no** pueden faltar

Si tuvieras que reducir el video a 60 segundos, estos son los 5 puntos obligatorios mínimos:

1. **El repo se organiza en `app.py` (Streamlit) + `src/` (pipeline) + `data/` + `output/` + `docs/`.**
2. **El índice de cobertura vive en `[0, 1]`** y combina oferta + actividad + acceso, normalizadas con min-max.
3. **El pipeline son 5 scripts en orden:** `data_loader` → `cleaning` → `geospatial` → `metrics` → `visualization` (naming exacto al rubric).
4. **La app tiene 4 tabs**, con el mapa interactivo cacheado para evitar lag.
5. **78 % de distritos sin emergencia propia**; mediana de distancia 23 km; Spearman entre especificaciones 0.891.

---

## 🔗 Mapeo libreto ↔ rubric

> Por si el profesor pide ver explícitamente cómo se cubre cada punto del rubric:

| Rubric requirement | Bloque del video | Sección del repo en pantalla |
|---|---|---|
| Repository structure | Bloque 2 (0:15–0:45) | VSCode tree + README |
| Methodology | Bloque 3 (0:45–1:25) | App § 6 *Cálculo del índice* (LaTeX) |
| Pipeline / main scripts | Bloque 4 (1:25–2:10) | `src/*.py` en VSCode |
| Streamlit app running | Bloque 5 (2:10–3:10) | App en `localhost:8501`, los 4 tabs |
| Main outputs | Bloque 6 (3:10–3:45) | Tab *Análisis Estático* gráfico 1 + README *Hallazgos* |
