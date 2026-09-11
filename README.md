# Expediente Corporativo GDI — Skill para Claude Code

Skill que genera el **expediente corporativo COMPLETO (7 documentos)** de una sociedad mercantil mexicana o fideicomiso, en formato Word (.docx) con el diseño del despacho.

> **La fuente única del skill es [`SKILL.md`](./SKILL.md).** Este README es solo la portada.
> La versión vigente está en la línea 7 de `SKILL.md` y en el [`CHANGELOG.md`](./CHANGELOG.md).

## Los 7 documentos
1. Títulos Accionarios (Art. 125 LGSM) — solo sociedades por acciones
2. Libro de Registro de Accionistas / Socios
3. Libro de Variaciones de Capital
4. Tabla de Tenencia Accionaria Vigente
5. Apoderados y Poderes Vigentes
6. Historial Societario
7. **Beneficiario Controlador (PLD)** — CFF, LFPIORPI y Acuerdo 115/2026

*(Sociedades de partes sociales —S. de R.L., S.C., A.C.— no llevan el documento 1 de títulos.)*

## Instalación
Coloca `SKILL.md` en `~/.claude/skills/expediente-corporativo-gdi/SKILL.md`, o instala el paquete `expediente-corporativo-gdi.skill` (es un ZIP que contiene ese mismo `SKILL.md`).

## Uso
En Claude Code, menciona: *nueva sociedad · expediente corporativo · títulos accionarios · libro de socios · beneficiario controlador · documentos societarios*, o sube documentos corporativos.

## Versionado
- La versión vive en la línea 7 de `SKILL.md` y en `CHANGELOG.md`.
- El paquete `.skill` debe regenerarse en cada cambio (su `SKILL.md` interno debe tener el mismo hash que el `SKILL.md` de la raíz).
- Ver el historial completo de correcciones dentro de `SKILL.md`.
