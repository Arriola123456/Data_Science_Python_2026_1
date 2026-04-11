# LIBRETO — Video Explicativo Tarea 1 (Maximo 3 minutos)

> **Instrucciones para grabar:** Tener abiertos antes de empezar:
> 1. El editor de codigo con `scraper.py` visible.
> 2. Una terminal lista en la carpeta `Scraping_data/`.
> 3. La carpeta `output/` vacia (borrar el Excel previo si existe).
>
> **Tip:** Mientras el script corre (~1.5 min), aprovechas ese tiempo para
> explicar lo que va apareciendo en pantalla. No hay tiempo muerto.

---

## [0:00 - 0:20] INTRO — Que hace el proyecto (20 seg)

**DECIR:**

> "Este proyecto hace web scraping de los resultados del examen de admision
> 2026-II de San Marcos. El script entra a la pagina oficial, recorre las
> 111 carreras publicadas, extrae todos los postulantes y consolida todo en
> un unico archivo Excel."

**MOSTRAR:** La pagina web `https://admision.unmsm.edu.pe/Website20262/A/A.html`
abierta en el navegador, señalando la lista de carreras.

---

## [0:20 - 0:50] CODIGO — Estructura general (30 seg)

**MOSTRAR:** El archivo `scraper.py` en el editor. Hacer scroll rapido mostrando
las 4 funciones.

**DECIR:**

> "El codigo tiene 4 funciones principales.
>
> Primero, `crear_driver` — abre Chrome en modo headless, es decir, sin
> ventana visible.
>
> Segundo, `obtener_enlaces_carreras` — navega a la pagina principal y
> recolecta los 111 links de carreras usando un selector CSS.
>
> Tercero, `extraer_postulantes_de_carrera` — esta es la funcion clave,
> que ahora les explico.
>
> Y cuarto, `exportar_a_excel` — toma toda la data y la guarda en un xlsx
> con pandas."

---

## [0:50 - 1:20] CODIGO — La funcion clave (30 seg)

**MOSTRAR:** Hacer zoom en la funcion `extraer_postulantes_de_carrera`, lineas 88-162.

**DECIR:**

> "Esta funcion resuelve tres problemas de la pagina.
>
> Primero, la tabla usa DataTables que solo muestra 50 registros por pagina.
> La solucion: usamos la API JavaScript de DataTables —
> `tabla.rows().every()` — que recorre todos los registros en memoria,
> sin importar la paginacion.
>
> Segundo, los nombres estan codificados en Base64. Pero como Selenium
> ejecuta el JavaScript de la pagina, cuando leemos `innerText` los nombres
> ya vienen decodificados.
>
> Tercero, el puntaje y el merito no se muestran visualmente en la celda.
> Estan escondidos en los atributos `data-score` y `data-merit` del HTML.
> Los extraemos con `getAttribute`."

**SEÑALAR** en el codigo las lineas 152-153 donde se lee `data-score` y `data-merit`.

---

## [1:20 - 1:25] EJECUCION — Lanzar el script (5 seg)

**MOSTRAR:** La terminal.

**DECIR:**

> "Ahora ejecutamos el script."

**EJECUTAR:**

```bash
python scraper.py
```

---

## [1:25 - 2:30] EJECUCION — Mientras corre, explicar lo que se ve (65 seg)

> El script tarda aproximadamente 1.5 minutos. Aprovechas para narrar
> lo que va apareciendo en la terminal.

**Cuando aparezca el encabezado:**

> "Ahi vemos que se conecta a la URL de San Marcos y arranca Chrome
> en modo headless."

**Cuando aparezca "111 carreras encontradas":**

> "Ya recolecto los 111 enlaces de carreras. Ahora va a entrar a cada
> una y extraer a todos los postulantes."

**Mientras van apareciendo las carreras una por una:**

> "Aca vemos como va carrera por carrera. Por ejemplo, Administracion
> tiene 553 postulantes, Derecho tiene mas de 1800...
>
> Fijense que cada carrera muestra la cantidad de postulantes extraidos.
> Si alguna fallara, apareceria un mensaje de error pero el script
> continuaria con la siguiente gracias al try-except.
>
> *(esperar a que pase Medicina Humana)*
>
> Medicina Humana con 4,350 postulantes — es la carrera con mas
> demanda."

**Cuando termine y muestre el resumen:**

> "Listo. 111 carreras procesadas, cero errores, mas de 25 mil
> postulantes extraidos. Y el Excel ya esta guardado en la carpeta
> output."

---

## [2:30 - 3:00] OUTPUT — Mostrar el Excel (30 seg)

**ABRIR** el archivo `output/resultados_sanmarcos.xlsx` en Excel.

**DECIR:**

> "Aca esta el archivo final. Tiene 7 columnas: Carrera, Codigo,
> Apellidos y Nombres — que se decodificaron correctamente del Base64 —,
> Escuela, Puntaje — que se extrajo del atributo `data-score` —,
> Merito y Observacion.
>
> *(hacer scroll rapido para mostrar el volumen de datos)*
>
> Son mas de 25 mil filas, una por cada postulante de las 111 carreras.
> Y con esto concluye la tarea de web scraping."

---

## RESUMEN DE TIEMPOS

| Segmento | Duracion | Contenido |
|---|---|---|
| 0:00 - 0:20 | 20 seg | Intro: que hace el proyecto |
| 0:20 - 0:50 | 30 seg | Codigo: las 4 funciones |
| 0:50 - 1:20 | 30 seg | Codigo: funcion clave (3 problemas resueltos) |
| 1:20 - 1:25 | 5 seg | Ejecutar `python scraper.py` |
| 1:25 - 2:30 | 65 seg | Narrar mientras corre (aprovecha el tiempo) |
| 2:30 - 3:00 | 30 seg | Abrir Excel y mostrar resultado |
| **Total** | **3:00** | |
