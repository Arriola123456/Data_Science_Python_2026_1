# Libreto — Video explicativo `emergency_access_peru`

**Duración objetivo:** ≤ 4 minutos (240 s)
**Ritmo de habla asumido:** ~150 palabras por minuto (ritmo técnico/explicativo)
**Presupuesto total de palabras:** ~560 palabras (deja margen para pausas, clicks y transiciones)

---

## 📋 Estructura en 7 bloques

| # | Tiempo | Duración | Sección | Palabras | ¿Qué se muestra en pantalla? |
|---|---|---|---|---|---|
| 1 | 0:00 – 0:25 | 25 s | Intro y problema | ~60 | Título del repo en GitHub / README |
| 2 | 0:25 – 0:55 | 30 s | Datasets y pipeline de ingesta | ~75 | Tab *Datos y Metodología* § 3 (Fuentes) |
| 3 | 0:55 – 1:25 | 30 s | Limpieza y *quirks* del dato | ~75 | Tab *Datos y Metodología* § 4 y § 7 |
| 4 | 1:25 – 2:10 | 45 s | Índice de cobertura + fórmula | ~110 | Tab *Datos y Metodología* § 6 (LaTeX) |
| 5 | 2:10 – 3:10 | 60 s | Demo: Análisis Estático + Exploración | ~150 | Tab *📊* gráfico 1 → Tab *🔍* mapa |
| 6 | 3:10 – 3:45 | 35 s | Hallazgos principales | ~90 | Tab *Análisis Estático* / README |
| 7 | 3:45 – 4:00 | 15 s | Cierre y limitaciones | ~40 | README sección *Limitaciones* |

**Total estimado:** 540–580 palabras, 3:55–4:00 minutos.

---

## 🎬 Guión completo

### 1 · Introducción y problema · `0:00 – 0:25` (60 palabras)

> **📺 Pantalla:** abre el repo `emergency_access_peru` en GitHub o el README en VSCode. Mantén visible el título.

> **🎙 Narración:**
>
> "Hola, este es mi proyecto para la **Tarea 2 de Data Science**: un análisis geoespacial de la desigualdad en el acceso a servicios de emergencia en salud entre los **1 873 distritos del Perú**. La pregunta de fondo es simple: cuando una persona tiene una emergencia en un distrito rural, **¿puede llegar a tiempo** a un servicio que realmente atienda?"

---

### 2 · Datasets y pipeline de ingesta · `0:25 – 0:55` (~75 palabras)

> **📺 Pantalla:** abre la app Streamlit en `http://localhost:8501`, Tab **📚 Datos y Metodología**. Haz scroll hasta la sección 3 (tabla de Fuentes).

> **🎙 Narración:**
>
> "Integro **cuatro datasets abiertos**: el directorio IPRESS del MINSA con 20 mil establecimientos, la producción de emergencias 2024 de SUSALUD con casi 16 millones de atenciones, el shapefile INEI con 136 mil centros poblados, y los límites distritales del repo del curso. El pipeline de ingesta descarga todo a `data/raw/` con un solo comando y es **completamente reproducible**."

---

### 3 · Limpieza y hallazgos del dato fuente · `0:55 – 1:25` (~75 palabras)

> **📺 Pantalla:** Tab **📚 Datos y Metodología**, scroll a § 4 (Resumen de limpieza) y luego § 7 (Quirks).

> **🎙 Narración:**
>
> "La limpieza reveló **tres** sorpresas en los datos fuente: primero, en el CSV del MINSA las columnas `NORTE` y `ESTE` están **invertidas** — lo que dice NORTE es longitud y viceversa. Segundo, SUSALUD anonimiza celdas sensibles con el marcador `NE_0001` que parseamos como nulo. Tercero, los centros poblados del INEI **no traen UBIGEO**: se deriva de los primeros 6 dígitos del campo `CÓDIGO`. Todo queda documentado en el pipeline y en la sección de decisiones metodológicas."

---

### 4 · El índice de cobertura · `1:25 – 2:10` (~110 palabras)

> **📺 Pantalla:** scroll a § 6 del Tab — mostrar las fórmulas LaTeX renderizadas (especialmente 6.1 y 6.3).

> **🎙 Narración:**
>
> "El indicador central es un **índice de cobertura de emergencia** en escala **cero a uno**, donde **uno es el mejor atendido** y **cero el peor**. Es el promedio simple de tres componentes normalizadas a `[0, 1]` con min-max y log-transform: **oferta** de emergencia por kilómetro cuadrado, **actividad** total de atenciones, y **acceso** medido como la fracción de centros poblados dentro de 30 kilómetros de un IPRESS de emergencia.
>
> Para medir sensibilidad, construí una **especificación alternativa** con umbral de 15 kilómetros y oferta por centro poblado. La correlación de Spearman entre ambos rankings es **0.891** — alta, pero con reordenamientos en el tramo medio del ranking."

---

### 5 · Demo: Análisis Estático + Exploración Interactiva · `2:10 – 3:10` (~150 palabras)

> **📺 Pantalla:** click al Tab **📊 Análisis Estático**. Zoom al gráfico 1 (histograma con tabla integrada) y dile al espectador qué mirar. Luego click al Tab **🔍 Exploración Interactiva** y mueve el slider.

> **🎙 Narración:**
>
> "En el tab **Análisis Estático**, el **gráfico 1** muestra la distribución del índice. Es **bimodal por diseño**: hay dos picos estructurales en `x = 0` y en `x = 1/3`. El primero son **479 distritos totalmente aislados**; el segundo, **632 distritos sin emergencia propia pero con acceso a un IPRESS vecino**. Juntos son **el 59 % del país**. La tabla integrada al pie del gráfico lista ejemplos concretos de ambos extremos.
>
> En el tab **Exploración Interactiva** puedo filtrar por departamento y alternar entre especificaciones. El mapa está **cacheado**: el primer render toma uno o dos segundos, pero después cambiar filtros es instantáneo. Los distritos con contorno negro son los **10 peor atendidos del filtro actual**."

---

### 6 · Hallazgos principales · `3:10 – 3:45` (~90 palabras)

> **📺 Pantalla:** Tab **📊 Análisis Estático**, gráficos 2 y 3 en secuencia rápida, o sección "Hallazgos" del README.

> **🎙 Narración:**
>
> "Los hallazgos: el **78 % de los distritos** no tiene ningún IPRESS con actividad de emergencia reportada en 2024. La mediana de distancia desde un centro poblado al IPRESS de emergencia más cercano es **23 kilómetros**; el máximo llega a **377** en la Amazonía. Los mejor atendidos son **Jesús María, Bellavista y Arequipa cercado**. Los peor atendidos están en **Piura, Puno, Huánuco y Huancavelica**. Lo más revelador: la **varianza intra-departamental es grande**, así que las intervenciones deben ser **distritales**, no por bloques."

---

### 7 · Cierre y limitaciones · `3:45 – 4:00` (~40 palabras)

> **📺 Pantalla:** vuelve al README del repo (sección *Limitaciones*) o al título.

> **🎙 Narración:**
>
> "Las principales limitaciones: usamos **distancia euclidiana** en vez de tiempo real de viaje, **peso uniforme** por centro poblado, y un solo año. Todo el código, las decisiones metodológicas y los resultados están documentados en el repositorio. Gracias."

---

## 🧪 Tips de grabación

1. **Practica leyendo en voz alta con cronómetro.** Si te pasas de 4 minutos, recorta en el bloque 5 (demo) — es el más flexible.
2. **Pre-carga todos los tabs de la app** antes de grabar (especialmente *Exploración Interactiva* con algún filtro aplicado) para que el caché ya esté caliente y el mapa aparezca instantáneo.
3. **Resolución 1920 × 1080** como mínimo para que el texto de la app y las fórmulas se lean bien.
4. **Zoom / resaltado** cuando hables de:
   - Fórmulas LaTeX (§ 6 del tab Datos) — hacé zoom al bloque `st.latex(...)`.
   - Gráfico 1 de Análisis Estático — la tabla al pie es la estrella.
   - Los dos pileups en `x = 0` y `x = 1/3`.
5. **Si grabás en dos tomas** (pantalla por un lado + audio por otro), sincronizá con claqueta al inicio: un click visible y audible.
6. **Desactivá notificaciones** del sistema, cerrá tabs ajenas y otras apps visibles.

---

## ✅ Checklist pre-grabación

- [ ] App Streamlit corriendo sin errores en `localhost:8501`
- [ ] Los 4 tabs cargados al menos una vez (para tener los parquets en caché)
- [ ] Tab *Exploración Interactiva* precargado con un filtro aplicado (para que el mapa folium esté caché)
- [ ] README abierto en otra pestaña para mostrar al cierre
- [ ] Repo `emergency_access_peru` en GitHub abierto en una tercera pestaña para la intro
- [ ] Micrófono probado sin ruido de fondo
- [ ] Notificaciones de Windows / Slack / correo silenciadas
- [ ] Pantalla limpia (sin tabs ajenas, sin barras de notificación abiertas)
- [ ] Cronómetro visible o usar la grabadora con HUD de tiempo

---

## 📝 Versión abreviada (por si te pasas del tiempo — puedes cortar estas líneas)

| Si necesitas recortar | Corta de aquí |
|---|---|
| Bloque 3 (quirks) | Elimina la mención del `NE_0001`; deja solo la inversión NORTE/ESTE |
| Bloque 4 (fórmula) | Elimina la oración sobre especificación alternativa; la cubres en el 5 |
| Bloque 5 (demo) | Salta el mapa interactivo; solo menciona el histograma bimodal |
| Bloque 6 (hallazgos) | Elimina los nombres específicos; deja solo el 78 % y la mediana de 23 km |

---

## 🎯 Mensajes clave que **no** deben faltar

Si tuvieras que reducir el video a 60 segundos, estos son los 4 puntos obligatorios:

1. **El problema es estructural:** 78 % de distritos no tiene emergencia propia.
2. **El índice vive en `[0, 1]`** — interpretable directamente, no es un z-score abstracto.
3. **La distribución es bimodal** con dos pileups en 0 y 1/3 — consecuencia del diseño, no artefacto.
4. **La sensibilidad está medida** — Spearman 0.891 significa que las conclusiones cualitativas son robustas.
