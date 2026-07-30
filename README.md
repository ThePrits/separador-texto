# Divisor de texto para TTS

Aplicación web (un solo archivo HTML) para dividir textos largos en bloques de tamaño controlado, respetando el final de las frases.

## Características

- **Corte inteligente**
  1. Prioriza cortar después de `.` `!` `?` (final de frase).
  2. Si no es posible, corta en el último espacio.
  3. Solo como último recurso parte a mitad de palabra.
- Límite de caracteres configurable (presets 500 / 1000 / 2000 / 5000 o valor personalizado entre 50 y 50000).
- Botón **Copiar** por bloque y **Copiar todos** (separados por `---`).
- **Modo oscuro y modo claro**, con detección de preferencia del sistema y persistencia en `localStorage`.
- El límite de caracteres también se recuerda entre sesiones.
- Atajo de teclado: `Ctrl` / `Cmd` + `Enter` para procesar.
- Interfaz responsive (dos columnas en pantallas grandes, una en móviles).
- Sin dependencias externas.

## Cómo usar

1. Abre `index.html` en el navegador.
2. Pega el texto completo en el área izquierda.
3. Elige el límite de caracteres (o deja el valor por defecto).
4. Pulsa **Procesar** (o `Ctrl`/`Cmd` + `Enter`).
5. Copia los bloques generados uno a uno o todos juntos.


## Algoritmo de división

```
mientras quede texto:
  si longitud ≤ límite → último bloque
  si no:
    ventana = primeros N caracteres
    minPos  = 40 % de N  (evitar cortes demasiado tempranos)
    1. buscar el último . ! ? en [minPos … N)
       e incluir comillas/espacios posteriores de la frase
    2. si no hay → último espacio en [minPos … N)
    3. si no hay → corte duro en N
    añadir el trozo y continuar con el resto
```

Antes de dividir, el texto se normaliza: cualquier secuencia de espacios en blanco (espacios, tabuladores, saltos de línea) se colapsa en un único espacio.
