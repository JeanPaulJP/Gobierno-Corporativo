# Expediente Corporativo GDI — Skill para Claude Code

**Versión:** 5.1 · Formato V24 · Marco legal: LGSM · CCF · DOF 2016-01-29

Skill para [Claude Code](https://claude.com/claude-code) que genera el **expediente corporativo COMPLETO** de una sociedad mercantil mexicana. Entrega **siempre los 6 documentos** en formato Word (`.docx`) con el diseño oficial del despacho.

## Los 6 documentos

| # | Documento | Fundamento / Contenido |
|---|-----------|------------------------|
| 1 | **Títulos Accionarios** | Art. 125 LGSM |
| 2 | **Libro de Registro de Accionistas / Socios / Asociados** | Según el tipo societario |
| 3 | **Libro de Variaciones de Capital** | Narrativa + desglose de capital + notas + firmas |
| 4 | **Tabla de Tenencia Accionaria Vigente** | Corte a la fecha de la estructura vigente |
| 5 | **Apoderados y Poderes Vigentes** | Consejo/administrador y poderes en vigor |
| 6 | **Historial Societario** | Cronología de actos corporativos |

Los documentos 1 los produce `gen_titulos.js`; los documentos 2 a 6 los produce `gen_libros.js`. Ambos scripts van embebidos dentro de `SKILL.md`.

## Qué hace

- Analiza documentos corporativos que se suban: actas constitutivas, asambleas, escrituras de reforma, Constancias de Situación Fiscal (CSF).
- O guía una captura manual de datos cuando no hay documentos.
- Aplica las convenciones de formato y las reglas de corrección acumuladas del despacho (fechas de asiento con +20/25 días de inscripción RPC, glosa "Distrito Federal (Hoy Ciudad de México)", cargas legales sobre acciones, alertas por datos faltantes, reconciliación de porcentajes al 100%, etc.).

## Instalación

Coloca `SKILL.md` dentro de una carpeta con el nombre del skill:

```
~/.claude/skills/expediente-corporativo-gdi/
└── SKILL.md
```

O instala el paquete `expediente-corporativo-gdi-v5.1.skill` (es un ZIP que contiene el mismo `SKILL.md`).

## Uso

En Claude Code, menciona cualquiera de estos disparadores o sube documentos corporativos:

> nueva sociedad · expediente corporativo · títulos accionarios · libro de acciones · libro de socios · tenencia accionaria · apoderados y poderes · historial societario

El skill genera los 6 documentos y los entrega en formato `.docx`.

## Notas de la versión 5.1

- Los 6 documentos se generan en la carpeta de red compartida del despacho (`BASE_OUT` apunta ahí en ambos scripts).
- El RFC va debajo del nombre en los cuadros accionarios (salto real vía helper `mlRuns`).
- El Libro de Variaciones de Capital **no** lleva cuadro accionario ni domicilios: eso va solo en el Libro de Registro.
- El porcentaje de capital es control interno de reconciliación y **no** se imprime en el título.
