---
title: 'Cómo Obtener Subtítulos de un Video Usando JavaScript en la Consola'
date: '2024-06-08'
layout: post
tags: [Javascript, Truco]
excerpt: >-
  Aprende cómo extraer y descargar subtítulos de videos utilizando JavaScript directamente desde la consola del navegador.
description: >-
  Este tutorial te enseñará cómo utilizar JavaScript en la consola del navegador para extraer y descargar subtítulos de videos que ya los tienen disponibles.
image: '/images/blogs/bajarsubtitulos.webp'
thumb_image: '/images/blogs/bajarsubtitulos.webp'
style: code
author: fabiansato
---

## Introducción

En este tutorial aprenderás cómo extraer y descargar subtítulos de videos utilizando JavaScript directamente desde la consola del navegador. Este método es útil para videos que ya tienen subtítulos embebidos y te permite guardarlos fácilmente en tu dispositivo.

## Requisitos

- Navegador web (preferiblemente Chrome o Firefox)
- Video con subtítulos embebidos

## Pasos

### 1. Abrir el Video y Activar Subtítulos

Primero, abre el video del cual quieres extraer los subtítulos y asegúrate de que los subtítulos están activados.

### 2. Abrir la Consola del Navegador

Abre las herramientas de desarrollo de tu navegador. Puedes hacerlo presionando `Ctrl+Shift+I` (Windows/Linux) o `Cmd+Opt+I` (Mac), y luego seleccionando la pestaña "Consola".

### 3. Ejecutar el Script de JavaScript

Copia y pega el siguiente script en la consola y presiona Enter:


#### Ejemplo de Script para YouTube:
```javascript
// Asegúrate de que los subtítulos estén activados en el video antes de ejecutar el script

(function() {
    function downloadFile(filename, content) {
        const a = document.createElement('a');
        a.href = URL.createObjectURL(new Blob([content], { type: 'text/plain' }));
        a.setAttribute('download', filename);
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }

    function getSubtitles() {
        const subtitles = [];
        const tracks = document.querySelectorAll('track');
        
        tracks.forEach(track => {
            if (track.kind === 'subtitles' || track.kind === 'captions') {
                fetch(track.src)
                    .then(response => response.text())
                    .then(text => {
                        subtitles.push(text);
                        // Download each subtitle track
                        downloadFile(`subtitles_${track.label || track.srclang}.vtt`, text);
                    })
                    .catch(err => console.error('Error fetching subtitles:', err));
            }
        });

        if (subtitles.length === 0) {
            console.log('No subtitles found.');
        }
    }

    getSubtitles();
})();

```
#### Adaptación a Otras Plataformas:
```javascript
(function() {
    function downloadFile(filename, content) {
        const a = document.createElement('a');
        a.href = URL.createObjectURL(new Blob([content], { type: 'text/plain' }));
        a.setAttribute('download', filename);
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }

    function getSubtitles() {
        const subtitles = [];
        // Modifica el selector para encontrar las etiquetas de subtítulos en la plataforma
        const subtitleElements = document.querySelectorAll('track, .subtitle-element-selector');
        
        subtitleElements.forEach(subtitleElement => {
            const src = subtitleElement.src || subtitleElement.getAttribute('data-src');
            if (src) {
                fetch(src)
                    .then(response => response.text())
                    .then(text => {
                        subtitles.push(text);
                        // Download each subtitle track
                        const label = subtitleElement.label || subtitleElement.getAttribute('data-label') || 'subtitle';
                        downloadFile(`subtitles_${label}.vtt`, text);
                    })
                    .catch(err => console.error('Error fetching subtitles:', err));
            }
        });

        if (subtitles.length === 0) {
            console.log('No subtitles found.');
        }
    }

    getSubtitles();
})();

```
## Explicación del Script

`downloadFile(filename, content)`
Esta función crea y descarga un archivo con el contenido proporcionado.

`getSubtitles()`
 Esta función busca todos los elementos <track> en el DOM, verifica si son subtítulos o captions, y descarga el archivo de subtítulos.

## Conclusión

Con este simple script, puedes extraer y descargar subtítulos de cualquier video que los tenga embebidos en la página. Esto puede ser especialmente útil para estudiar y tener una referencia escrita de los contenidos de video.
Recursos Adicionales

¡Feliz aprendizaje!