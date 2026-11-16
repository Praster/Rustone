<div align="center">

<img width="300" height="300" alt="Logotipo R verde neón con botón de reproducción" src="https://github.com/user-attachments/assets/12335f11-5ade-4cd1-a408-70d845c6bf5e" />


# Rustone

**Un reproductor de música de escritorio con interfaz propia, hecho 100 % en Rust.**

`Rust` · `egui / eframe` · `rodio` · `Windows · macOS · Linux`

</div>

---

Rustone es un reproductor de música de escritorio con una interfaz oscura y animada, pensada desde cero: carátulas que reaccionan al pasar el ratón, una vista ampliada del reproductor con degradado del color de la portada, una barra de progreso en forma de onda y un botón de búsqueda con esquinas cortadas inspirado en su logo.

> Proyecto personal y educativo. No está afiliado a YouTube, Google, Spotify ni Deezer. Lee el apartado [Aviso legal](#aviso-legal) antes de usarlo o redistribuirlo.

## Características

**Descubrir**
- Página de inicio con listas del momento y novedades que se actualizan solas, accesos rápidos, «Reproducido recientemente» y «Tus favoritas».
- Páginas por categoría/género y búsqueda de canciones y artistas con sus carátulas.
- Soporte de títulos y artistas en japonés, coreano, chino, árabe, tailandés… (usa las fuentes del sistema).

**Reproducción**
- Audio en *streaming*: no se guardan archivos de música en el disco.
- Cola de reproducción con panel «A continuación».
- **Modo aleatorio real**: baraja lo que queda de la cola, de modo que «siguiente» siempre reproduce la primera de «A continuación»; al desactivarlo vuelve al orden original.
- Anterior / siguiente / pausa / salto a cualquier punto de la canción y control de volumen.
- Vista ampliada animada (también se abre haciendo clic en la barra del reproductor).

**Biblioteca**
- «Me gusta» y playlists propias, con portada formada por las 4 primeras canciones y canciones reordenables con flechas.
- Importar playlists desde un enlace de Spotify (públicas, hasta 100 canciones) o de YouTube Music.
- Borrar el historial de reproducción reciente.
- Panel de **caché**: muestra qué guarda la app y permite borrarla sin tocar tu biblioteca.

**Interfaz**
- Iconos dibujados a mano (no dependen de ninguna fuente ni emoji).
- Menú contextual de canción tipo desplegable, diálogo de nueva playlist, animaciones suaves y color dominante extraído de cada carátula.

## Cómo funciona

| Pieza | Qué hace |
|---|---|
| **Interfaz** | `eframe` / `egui` en modo inmediato, con todo el código en `src/main.rs`. |
| **Datos de canciones** | Consultas a la API web de YouTube Music para buscar canciones y artistas. |
| **Listas del inicio** | Rankings públicos de Deezer como lista de títulos, que se resuelven a la versión oficial en YouTube Music. |
| **Importar playlists** | Lee la página pública de Spotify (solo títulos y artistas) o la lista de YouTube Music y busca cada canción. |
| **Audio** | `yt-dlp` obtiene la URL del flujo de audio y `ffmpeg` lo decodifica a PCM, que `rodio` reproduce. |
| **Persistencia** | Biblioteca, caché de novedades y registro de errores en JSON dentro de la carpeta de datos del usuario. |

En Windows la app descarga sola `yt-dlp` y `ffmpeg` la primera vez y mantiene `yt-dlp` actualizado.

## Tecnologías

| Crate | Uso |
|---|---|
| [`eframe`](https://crates.io/crates/eframe) / `egui` 0.29 | Ventana e interfaz gráfica |
| [`egui_extras`](https://crates.io/crates/egui_extras) | Carga de imágenes (carátulas, logo) |
| [`rodio`](https://crates.io/crates/rodio) 0.20 | Salida de audio |
| [`ureq`](https://crates.io/crates/ureq) | Peticiones HTTP |
| [`serde`](https://crates.io/crates/serde) / `serde_json` | Serialización de la biblioteca y lectura de JSON |
| [`image`](https://crates.io/crates/image) | Decodificación de carátulas y color dominante |
| [`ab_glyph`](https://crates.io/crates/ab_glyph) | Fuentes del sistema para alfabetos no latinos |
| [`dirs`](https://crates.io/crates/dirs) | Carpeta de datos por sistema operativo |
| [`chrono`](https://crates.io/crates/chrono) | Fechas y refresco periódico |
| [`zip`](https://crates.io/crates/zip) *(Windows)* | Descompresión de ffmpeg |
| [`winresource`](https://crates.io/crates/winresource) *(Windows, build)* | Icono del `.exe` |

Herramientas externas: [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) y [`ffmpeg`](https://ffmpeg.org/).

## Requisitos

1. **Rust** estable: <https://rustup.rs> (en Windows, además las *Build Tools* de Visual Studio con «Desarrollo de escritorio con C++»).
2. **Linux:** `sudo apt install libasound2-dev pkg-config libx11-dev libxkbcommon-dev libwayland-dev libgl1-mesa-dev ffmpeg`
3. **macOS:** `brew install ffmpeg`
4. **Windows:** nada más; `yt-dlp` y `ffmpeg` se descargan solos.

## Instalación y uso

```bash
git clone https://github.com/<tu-usuario>/rustone.git
cd rustone
cargo run --release
```

Si algo falla, ejecuta `cargo run` (sin `--release`) para ver los errores; también se guardan en `crash.log`.

### Dónde se guardan los datos
- Windows: `%APPDATA%\rustone`
- macOS: `~/Library/Application Support/rustone`
- Linux: `~/.local/share/rustone`

Contiene `library.json` (Me gusta, playlists, recientes), `feeds.json` (caché de novedades), `crash.log` y, en Windows, los ejecutables de `yt-dlp` y `ffmpeg`.

## Estructura del proyecto

```
rustone/
├─ assets/        logo e icono de la aplicación
├─ src/main.rs    toda la aplicación (interfaz, reproductor, biblioteca)
├─ build.rs       incrusta el icono en el .exe de Windows
└─ Cargo.toml
```

## Aviso legal

Rustone **no aloja, almacena ni distribuye música**. Obtiene el audio en tiempo real de plataformas de terceros mediante `yt-dlp`, y eso **puede incumplir los términos de servicio** de dichas plataformas y, según tu país, la normativa de propiedad intelectual. Se ofrece únicamente con fines educativos y de uso personal; **el uso es responsabilidad de cada usuario**.

No tiene ninguna relación con YouTube, Google, Spotify ni Deezer, y todas las marcas pertenecen a sus propietarios. Si eres titular de derechos y quieres que algo se retire, abre un *issue*.

## Licencia

Elige una licencia antes de publicar (por ejemplo MIT o Apache-2.0) y añade el archivo `LICENSE`. Sin licencia, el código queda con todos los derechos reservados.
