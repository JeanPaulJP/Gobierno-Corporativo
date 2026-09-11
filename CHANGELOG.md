# CHANGELOG — expediente-corporativo-gdi

Formato: una entrada por versión. La versión vigente es la de la línea 7 de `SKILL.md`.

## v5.3 — septiembre 2026
- **Corrección 23:** nuevo **Documento 7 — Beneficiario Controlador (PLD)**, obligatorio para sociedades y fideicomisos. Función `buildBeneficiarioControlador()` en `gen_libros.js`; marco fijo `BC_MARCO`/`BC_CRITERIO` (CFF 32-B Ter/Quáter/Quinquies; RMF 2.8.1.20-2.8.1.23; LFPIORPI + reforma jul-2025; GAFI 24 y 25; Acuerdo 115/2026, DOF 07-ago-2026, vigor 30-nov-2026). Obligación de mapeo de la cadena hasta persona física (participación efectiva = producto de porcentajes). Nueva carpeta de salida `Beneficiario_Controlador`.
- Archivos que toca: `SKILL.md` (front-matter → 7 documentos; regla 23; `buildBeneficiarioControlador`, `BENEFICIARIO_CONTROLADOR`, `BC_MAPEO`, `BC_CONCLUSION`, DOC 6 en `main()`).
- Rompe: nada. Aditivo.

## v5.2 — septiembre 2026
- **Corrección 22:** números a letras siempre calculados con `numeroALetras(num, genero)` y `tituloEnLetras(t)`; se elimina la dependencia de diccionarios fijos (`enPalabras`, `numWord`, arreglo `numWord2`) que imprimían `undefined` con sociedades distintas de Administradora GDI o del 5º título en adelante. Patrón `enPalabras[x] || numeroALetras(x)`.
- Se corrigen referencias muertas al script inexistente `gen_libros_asiento.js` → `gen_libros.js`.
- Archivos que toca: `SKILL.md` (`gen_titulos.js`).
- Rompe: nada. Corrige defecto.

## v5.1 — agosto 2026
- Versión base: genera 6 documentos (Títulos, Libro de Registro, Variaciones de Capital, Tenencia, Apoderados, Historial). Correcciones 1-21.
