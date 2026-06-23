---
name: analista-datos-electoral
description: Investigación de datos para estrategia política. Descubre tendencias reales de demanda ciudadana con DataForSEO (volumen de búsqueda), valida qué pregunta la gente con Perplexity, y mide qué se vuelve viral con Apify (hooks, comentarios, sentimiento). Úsala para research anti-sesgo de demanda y de contenido. Incluye reglas estrictas de integridad de datos.
---

# Analista de datos electoral

Propósito: producir hallazgos accionables a partir de datos reales, evitando el sesgo del analista (no seguir solo las hipótesis que ya traía el cliente).

## Flujo de trabajo

1. DESCUBRIMIENTO DE DEMANDA (DataForSEO).
   - Herramienta: kw_data_google_ads_search_volume (location_name del país, language_code local).
   - El MCP devuelve maximo 10 keywords por llamada: lanzar varias llamadas en paralelo, en lotes de 10.
   - Barrer AMPLIO y a propósito por fuera de la hipótesis inicial: salud, seguridad, costo de vida, empleo, educación, vivienda, movilidad, servicios y trámites, pensiones, subsidios, economía digital, deudas y fraude, salud mental, mujer y familia. El objetivo es encontrar lo que el cliente no pidió.
   - Registrar volumen del último mes y pico, para ver estacionalidad y tendencia.
   - Agrupar en clústeres y sumar búsquedas relacionadas para ver el tamaño real de cada frente.
   - Marcar banderas huérfanas: alto volumen sin dueño político.
   - Complementos útiles: dataforseo_labs_google_keyword_ideas y keyword_suggestions para expandir; kw_data_dfs_trends_explore para curva temporal.

2. QUE PREGUNTA LA GENTE (Perplexity).
   - Para los clústeres top, preguntar qué dudan, preguntan o reclaman las personas sobre el tema, las preguntas frecuentes, las quejas y los miedos. Citar fuentes con fecha.

3. QUE SE VUELVE VIRAL (Apify).
   - Actor de descubrimiento: clockworks/tiktok-scraper (búsqueda por término o hashtag). Hacer fetch-actor-details para el input schema. Pedir pocos resultados por término y quedarse con los de mayor reproducciones.
   - Extraer por video: handle del autor, caption (el hook), playCount, diggCount, commentCount, webVideoUrl y la URL de portada (videoMeta.coverUrl o similar) para poder mostrar el post.
   - Comentarios y sentimiento: clockworks/tiktok-comments-scraper sobre los videos top. Resumir sentimiento y dejar comentarios representativos verbatim.
   - Otros actores: instagram-post-scraper, instagram-comment-scraper, grow_media/youtube-channel-video-scraper (sortOrder popular), thedoor/facebook-page-scraper.

4. SINTESIS.
   - Cruce de volumen de búsqueda (demanda) x preguntas frecuentes (intención) x viralidad (qué formato y hook funciona) x sentimiento.
   - Salida: tabla de frentes con dato real, qué pregunta la gente, hook que funciona, sentimiento, y oportunidad.

## Integridad de datos (reglas duras)

1. Nunca confiar en números de seguidores o vistas reportados por un subagente sin verificarlos con scraping en vivo. En el pasado hubo errores graves (handles equivocados, cifras infladas). Verificar siempre el handle real y el número en vivo.
2. Reportar solo lo que la herramienta devolvió. Si un dato no salió, decirlo. No completar con estimaciones disfrazadas de dato.
3. Conservar las URLs exactas (post y portada) para evidencia visual.
4. Los datasets grandes de Apify se desbordan en contexto: que el subagente analice y devuelva solo el resumen curado y las URLs, no el dataset crudo.
5. Distinguir siempre dato verificado de inferencia.

## Regla de estilo

Nunca usar em dashes (el caracter de raya larga). Usar comas, dos puntos, parentesis o puntos.
