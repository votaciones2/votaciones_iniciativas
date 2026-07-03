# Votación Big Rocks — v2 (proyecto nuevo desde cero)

Front en **GitHub Pages** + API en **Google Apps Script** + datos en **Google Sheets**.

Qué hace:
- 5 big rocks visibles a la vez (acordeón); cada una se vota por separado a medida que se expone.
- 4 criterios por big rock (1 a 5), con textos ancla en 1 y en 5 y etiqueta "25 pts".
- Votante Público o Comité técnico (el comité ingresa un código).
- Puntaje sobre 100, ponderado: Comité 30% · Público 70% (configurable en la hoja).
- Resultados: KPIs, podio top 3, resto del ranking y observaciones.

Archivos:
- `Codigo.gs` → Apps Script (API)
- `config.js`, `index.html`, `resultados.html` → GitHub Pages (front)

---

## Parte A — Google Sheet + Apps Script (haz esto primero)

1. Crea una hoja NUEVA: sheets.new → nómbrala (ej. "Votación Big Rocks").
2. Dentro de la hoja: **Extensiones → Apps Script** (desde la hoja, para que quede vinculado).
3. Borra el contenido de `Código.gs` y pega **`Codigo.gs`**. Guarda.
4. Selector de función → **`inicializar`** → **Ejecutar** → autoriza permisos.
   Esto crea 4 pestañas: `Config`, `BigRocks`, `Criterios`, `Respuestas`.
5. **Implementar → Nueva implementación** → engrane → **Aplicación web**:
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquier usuario**
6. Copia la URL que termina en **`/exec`**.

> Prueba: abre `TU_URL/exec?action=config` en el navegador.
> Debe salir un JSON con las 5 big rocks y los 4 criterios.

## Parte B — GitHub Pages

1. En `config.js`, reemplaza `PEGA_AQUI_TU_URL_EXEC` por tu URL `/exec`.
2. Crea un repo nuevo (público), p. ej. `votacion-bigrocks`.
3. Sube `config.js`, `index.html`, `resultados.html`.
4. Settings → Pages → Branch `main` → `/ (root)` → Save.
5. Tu app queda en `https://TU_USUARIO.github.io/votacion-bigrocks/`
   y los resultados en `.../resultados.html`.

(Recuerda: si editas archivos y no ves el cambio, Ctrl+F5.)

---

## Configuración desde la hoja (sin tocar código)

**Pestaña `Config`:**
| clave | valor |
|---|---|
| pesoComite | 30 ← % del comité; el público pesa el resto |
| codigoComite | 2468 ← CÁMBIALO antes de la sesión y entrégalo solo a los 4 del comité |
| bloquearRepetidos | SI ← NO para permitir revotar |

**Pestaña `BigRocks`:** edita nombres, oculta con `activa = FALSO`.
**Pestaña `Criterios`:** edita nombres, pts y los textos ancla (texto1 / texto5).
**Pestaña `Respuestas`:** se llena sola. Una fila por persona por big rock:
Fecha · Nombre · Área · Tipo · BigRock · las 4 notas · Puntaje100 · Observaciones.

## Cómo se calcula el puntaje

- Cada voto: suma de los 4 criterios (4–20) convertida a escala 100. Ej: 5+4+4+3 = 16 → 80/100.
- Por big rock: promedio del comité y promedio del público por separado, luego
  **final = comité × 30% + público × 70%**.
- Si un grupo aún no vota, se muestra el promedio del otro con la marca "Pendiente comité/público".

## Cambios de código
Tras editar `Codigo.gs`: Implementar → Administrar implementaciones → lápiz → Versión: **Nueva** → Implementar.
Tras editar el front: sube el archivo al repo (Pages se actualiza en ~1 min).
