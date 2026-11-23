---
name: clipper
description: Selecciona y edita momentos de vídeos largos o locales para Shorts, Reels y TikTok. Usa el motor Clipper y un montaje explícito con gancho permanente blanco y negro, subtítulos legibles y encuadre revisado. También adapta la edición a referencias y preferencias del usuario, sin prometer viralidad.
---

# Clipper: selección y edición de clips

Usa el criterio del editor para elegir, construir y verificar cada historia. El motor corta y renderiza; una puntuación automática no demuestra que un clip vaya a funcionar.

## Preferencias de este usuario

Lee [references/editorial-style.md](references/editorial-style.md) antes de seleccionar o montar. El estilo por defecto es un gancho **permanente**, Arial en negrita, en negro sobre blanco, mayúsculas selectivas, uno o dos emojis estilo iPhone y franjas redondeadas ajustadas a cada línea. El plano principal ocupa aproximadamente el 56 % de la altura, el tamaño intermedio aumentado un 5 %; el borde inferior de la caja del gancho toca exactamente el borde superior del plano y los subtítulos quedan al 77 % de su altura, dentro del vídeo; el fondo lleva un desenfoque visual suave, guardado como 20 en la escala local del renderizador. Lee también [references/caption-style.md](references/caption-style.md): los subtítulos actuales aparecen una palabra cada vez, en mayúsculas, condensados y con borde y sombra negros. Al preparar los tiempos, marca con keyword: true las palabras clave para resaltarlas en amarillo según la guía. Mantén estas preferencias salvo que el usuario pida otra cosa. No copies identidades, logos ni marcas de agua de las referencias.

## Vídeos de juegos con cámara del streamer

Usar el modo `layout: "gaming"` del renderizador y leer [el formato gaming](references/editorial-style.md#formato-gaming-con-webcam) cuando el contenido combine gameplay y webcam. La composición es streamer arriba y juego abajo, con aproximadamente 40 % / 60 % como punto de partida. Este modo sustituye el encuadre IRL de 1076 px y su fondo global; mantiene el idioma y los hooks narrativos aprobados. Elegir recortes independientes y sincronizados, y conservar la acción y la interfaz relevante. Las proporciones se ajustan al juego y a la escena.

## Idioma predeterminado

**Inglés para audio, subtítulos y gancho**, salvo que el usuario pida español explícitamente o el creador sea español. No deducir la nacionalidad por el nombre ni elegir doblaje español porque la conversación esté en español. Comprobar las pistas disponibles y usar la inglesa original cuando exista. Si no existe audio en el idioma solicitado, indicar esa limitación; no inventar una pista ni sintetizar una voz por defecto.

Verificar el idioma de la pista realmente descargada antes de transcribir. En `scripts/transcribe_window.py`, pasar `--language en` para inglés y `--language es` para español: el valor por omisión de ese script antiguo es `es` y no debe decidir el idioma del encargo.

## Entorno instalado

Python del motor:

```powershell
& "C:/Users/Casa/.codex/skills/clipper/engine/.venv/Scripts/python.exe" -m clipper.cli doctor
```

FFmpeg y FFprobe son necesarios. La instalación actual usa CPU; no requiere TYPESAFE_API_KEY para que Codex elija los momentos. Evita reinstalar el motor al empezar una tarea. El código original está en `engine/`; el renderizador del estilo del usuario está en `scripts/render_editorial.py`. El `render` original mantiene el título solo tres segundos y elimina pausas automáticamente: **no lo uses para el estilo actual**.

## Analizar y seleccionar

- Inspecciona el vídeo y su audio, no solo el título o la transcripción. Lee el contenido completo antes de asegurar que has elegido los mejores momentos del vídeo entero. Si solo has analizado una parte, delimítala.
- Busca un conflicto, sorpresa, reacción, contraste o desenlace concreto. El clip debe entenderse sin conocer al protagonista. No fuerces una cantidad de clips ni una duración.
- Empieza cerca del conflicto, con una frase comprensible. Construye contexto mínimo, desarrollo y recompensa. El gancho debe prometer lo que realmente muestra el fragmento.
- Los 15–35 segundos son una referencia inicial, no una obligación. Conserva una reacción o pausa que aporte valor; elimina repeticiones y esperas que no aporten.
- Se pueden unir intervalos del mismo vídeo si forman una historia coherente. Mantén el significado y el orden causal. Una anticipación debe identificarse y no falsear quién reacciona a qué.

## Escribir el gancho de texto

Antes de redactarlo, lee [references/hooks-sourcex.md](references/hooks-sourcex.md). Escribir el comienzo de una historia que alguien contaría a un amigo: persona concreta, acción o decisión y una circunstancia que cambia lo esperado. Evitar escoger una regla, etiqueta o resumen estático como sustituto de esa historia. Considerar hasta cinco enfoques que la escena permita, elegir por naturalidad, especificidad, curiosidad y lectura, y conservar la fidelidad como condición previa. Guardar la justificación y los tiempos de recompensa en `hook_editorial` según la guía. Para calibrar ejemplos concretos, consultar [la biblioteca de ejemplos](references/hooks-sourcex.md#biblioteca-de-ejemplos).

La preferencia por reservar el remate no prohíbe anticipar todo resultado general: una reacción, mecanismo o prueba puede seguir pendiente. Explica esa elección cuando sea relevante. No copies como hechos las acusaciones, cifras o sentimientos que otro editor haya añadido a un rótulo. Usa el análisis guardado de las referencias; no vuelvas a procesar los once vídeos para cada clip. Mantén el diseño visual ya aprobado salvo que el usuario lo cambie.

## Descargar y transcribir

Para el flujo original:

```powershell
& "C:/Users/Casa/.codex/skills/clipper/engine/.venv/Scripts/python.exe" -m clipper.cli prepare "VIDEO" --out "CARPETA_DE_TRABAJO"
```

En vídeos largos y CPU, primero comprueba si hay subtítulos de plataforma útiles. Declara su procedencia y verifica los fragmentos elegidos contra el audio. No presentes una transcripción automática como revisada palabra por palabra. Si falla la descarga de YouTube, comprueba yt-dlp y Deno/Node; pide el archivo local solo si no puedes resolverlo con las herramientas disponibles.

Para transcribir o revisar una ventana con el modelo local, usa `scripts/transcribe_window.py`. Produce palabras con tiempos absolutos en JSON y utiliza CPU INT8 para evitar la lentitud de FLOAT32. Configura `HF_HOME` dentro de una carpeta con escritura. No sobrescribas transcripciones mejores con una salida peor. Marca o deja sin transcribir las voces ininteligibles; no inventes diálogo.

## Planificar y renderizar

Escribe un plan JSON con `source`, `words`, `hook`, `segments` y encuadres deliberados. Lee el esquema de [references/editorial-style.md](references/editorial-style.md). Los tiempos son segundos absolutos del original. Las palabras usan `s`, `e`, `w`. Los intervalos elegidos determinan los cortes: el renderizador no borra silencios por umbral.

```powershell
& "C:/Users/Casa/.codex/skills/clipper/engine/.venv/Scripts/python.exe" "C:/Users/Casa/.codex/skills/clipper/scripts/render_editorial.py" "PLAN.json" --output "SALIDA.mp4"
```

Decide el encuadre de cada escena antes de renderizar. Un rostro detectado no equivale al sujeto importante: conserva también manos, objetos y la reacción de quien escucha. Usa fondo desenfocado si el recorte quitaría contexto. No estires caras ni añadas zooms o cortes periódicos por obligación.

## Verificar y entregar

Comprueba el vídeo completo con audio cuando el entorno permita verlo y escucharlo. Revisa continuidad, sentido del diálogo, gancho cumplido, desenlace visible, subtítulos, encuadre, texto en zonas seguras, voz y nivel. FFprobe o capturas aisladas no sustituyen esa revisión. Si no puedes escuchar o ver todo, indica exactamente qué comprobaste y qué queda pendiente.

Entrega MP4 vertical 9:16, tiempos de origen y descripción breve de los cambios. Conserva el plan para iterar sin perder decisiones. No publiques ni programes contenido salvo petición explícita. No atribuyas visitas a una técnica sin métricas comparables. Los cambios de estilo quedan guardados en esta skill; no equivalen a entrenamiento permanente del modelo.


## Alcance y publicación preferidos (20 de noviembre de 2026)

Revisar todo el vídeo solicitado y crear todos los momentos que tengan una historia y recompensa suficientes, sin limitarse a diez clips ni forzar una cuota. Si se entrega un avance, continuar el resto del encargo hasta cubrirlo y distinguir la revisión completa de una búsqueda por transcripción o muestras.

En próximos encargos de clipping, preparar también una propuesta concreta de publicación en TikTok y YouTube Shorts: archivos, cuentas verificadas, títulos, descripción y hashtags, y fechas a las 10:00, 17:00 y 20:00 Europe/Madrid, distribuyendo tres al día como preferencia inicial. El usuario quiere aprobar una vez el lote antes de activar la publicación. Prefiere las plataformas directamente y rechaza depender de vidIQ para publicar. Comprobar primero la conexión y la capacidad real de programar; si falta acceso, explicarlo sin afirmar que existe automatización. Presentar el lote y su calendario para aprobación y ejecutar la programación solo tras aprobarlo. Confirmar el resultado con las plataformas. Una entrega local no implica publicación; no prometer envíos futuros a Content Rewards sin un flujo comprobado.
