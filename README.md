---
name: expediente-corporativo-gdi
description: "Genera el expediente corporativo COMPLETO (7 documentos) de una sociedad mercantil mexicana: (1) Títulos Accionarios (Art. 125 LGSM), (2) Libro de Registro de Accionistas o Socios, (3) Libro de Variaciones de Capital, (4) Tabla de Tenencia Accionaria Vigente, (5) Apoderados y Poderes Vigentes, (6) Historial Societario y (7) Beneficiario Controlador (PLD — CFF, LFPIORPI y Acuerdo 115/2026), todo en formato Word (.docx) con el diseño oficial del despacho. Entrega SIEMPRE los 7. Analiza documentos corporativos que el usuario suba (actas constitutivas, asambleas, escrituras de reforma, Constancias de Situación Fiscal) o guía una captura manual de datos. Úsalo cuando el usuario mencione: nueva empresa, nueva sociedad, expediente corporativo, títulos accionarios, generar títulos, libro de acciones, libro de socios, tenencia accionaria, apoderados y poderes, historial societario, beneficiario controlador, documentos societarios, o cuando suba documentos corporativos de una sociedad mercantil mexicana."
---

# SKILL: Administración Societaria — Expediente Corporativo Completo
**Versión:** 5.3 · última actualización: septiembre 2026 | **Formato:** V24 | **Marco:** LGSM · CCF · DOF 2016-01-29 · LFPIORPI/Acuerdo 115/2026

**Historial de correcciones incorporadas (no repetir estos errores — ver detalle en cada
regla más abajo):**
1. Nomenclatura correcta de libros (Accionistas/Socios/Asociados, nunca "Registro de Acciones").
2. `domBlock` solo lleva domicilio del titular, nunca quién lo representa.
3. Fecha de firma del asiento = fecha de INSCRIPCIÓN en el RPC (si está inscrito) o fecha
   del acto (si no lo está), + 20/25 días — nunca la fecha del instrumento a secas. Los
   movimientos que afectan ÚNICAMENTE el capital VARIABLE nunca requieren inscripción RPC
   (solo el capital fijo la requiere) — usar directamente la fecha del acto/instrumento
   como base de esos +20/25 días.
4. Glosa "Distrito Federal (Hoy Ciudad de México)" condicionada a la fecha del HECHO
   descrito (no la de firma ni la de generación del documento) frente al corte DOF
   29-ene-2016.
5. El `lugar` de cada asiento sigue el domicilio SOCIAL vigente de la Sociedad en esa
   fecha, no el lugar de la firma del instrumento ni el domicilio de un accionista.
6. El campo `nota` de cada accionista en el Título Accionario es solo para cargas legales
   sobre las acciones (usufructo, nuda propiedad, prenda, embargo) — nunca para
   representación legal ni historia corporativa del accionista.
9. El texto del campo `nota` en el Título Accionario debe iniciar con la etiqueta simple
   "NOTA:" (nunca "PROPIEDAD DESMEMBRADA:" ni otras etiquetas técnicas) y limitarse a los
   hechos de la carga legal (qué, quién, cuándo, instrumento) — sin agregar explicaciones de
   procedimiento o de derechos de voto (p. ej. la mención al artículo 23 LGSM sobre el
   ejercicio del voto del usufructuario). Ese tipo de explicación procedimental sí puede ir en
   el `notaBlock()` de los Libros (Variaciones de Capital / Registro de Accionistas), pero no
   en el título individual.
7. `tablaAccionistas2S` (tabla de accionistas de dos series en los asientos) lleva una
   columna adicional "CAPITAL TOTAL" por accionista (Serie A + Serie B), además del
   desglose por serie — no basta con mostrar el desglose y el total general al pie.
8. Ante cambios significativos de estructura accionaria (aumento/disminución/reclasificación
   con entrada o salida de accionistas), por defecto cancelar y reemitir TODOS los títulos
   vigentes con numeración limpia y consecutiva, en vez de conservar numeración histórica y
   solo añadir títulos incrementales — salvo que el usuario indique lo contrario.
10. Si al construir un asiento (Libro de Variaciones de Capital o Libro de Registro de
    Accionistas/Socios) falta un dato o documento de un accionista/socio — típicamente su
    domicilio por no tener CSF —, insertar un `alertaBlock()` inmediatamente después del
    `domBlock()` de esa persona, con el texto en color rojo (`B00020`) y la etiqueta
    "⚠️ ALERTA:". Esto aplica incluso si esa persona ya no es accionista vigente (p. ej. causó
    baja por cesión/cancelación de acciones): sus datos y la alerta correspondiente deben
    seguir apareciendo en los asientos históricos donde participó, aunque no se le vaya a
    emitir o renovar Título Accionario. La falta de un dato NUNCA es motivo para omitir a la
    persona del asiento — se documenta la carencia, no se oculta.
11. **PORCENTAJES DE LOS TÍTULOS: usar los del documento y verificar que sumen 100%.** Los % se
    transcriben de la escritura (por accionista) — no se recalculan a capricho. Regla de oro doble:
    (a) los % de TODOS los títulos suman 100%; (b) al agregarse por accionista reconcilian EXACTO
    con el % de la escritura. Si un accionista tiene su participación repartida en varios títulos
    (Serie A y Serie B, o bloques de distinta naturaleza), su % de escritura se DISTRIBUYE entre
    esos títulos en proporción a las acciones de cada uno, de modo que la suma por accionista = su
    % de escritura y el gran total = 100%. Ejemplo real GDI: Simón 56.25% (35,000 A + 7,550,076 B)
    → título A 0.26% + título B 55.99% = 56.25%; con Roberto usufructo 0.11%, Roberto Serie B 33.64%
    y Grupo Invermem 10.00% → total 100.00%. Verificar la suma ANTES de entregar. NOTA: desde la
    versión 4.5 el % es solo un control interno de reconciliación y NO se imprime en el Título
    (la columna "% Capital" fue eliminada — ver corrección 16).
12. **BASE POR SERIE EN EL TÍTULO.** La frase "de un total de N acciones" de un Título Accionario
    se refiere al total de LA SERIE de ese título (Serie A = capital fijo; Serie B = capital
    variable), NUNCA al gran total de la sociedad. No mezclar fija y variable. Redactar
    "correspondientes al capital social FIJO/VARIABLE" (no "parte fijo/fija", que falla en género).
13. **FECHAS COMPLETAS EN LETRA Y DATOS REGISTRALES VERBATIM.** Todas las fechas (instrumentos,
    asambleas, inscripciones, firmas de asientos) se escriben completas y en letra ("cuatro de
    junio de dos mil cuatro", no "04/jun/2004"). Los datos del Registro Público de Comercio (nombre
    exacto del registro —incluida la fórmula "adscrito a los Municipios de X y Y" cuando así
    aparezca—, partida, volumen, libro, fecha y folio mercantil electrónico), del instrumento y del
    notario se transcriben TAL CUAL, sin abreviar ni parafrasear. Si dos escrituras difieren en el
    mismo dato, usar la del instrumento más reciente y señalar la discrepancia al usuario.
14. **ANÁLISIS HOJA POR HOJA + OCR EN ESPAÑOL.** Las escrituras suelen estar escaneadas: aplicar
    OCR en español (instalar `spa.traineddata` desde github si falta) y revisar TODAS las páginas
    de cada documento, no solo las aparentemente relevantes (los ANTECEDENTES traen los datos de
    inscripción del RPC; los acuerdos y tablas de capital pueden estar dispersos). Para tablas con
    porcentajes/cifras finas, renderizar la página a PNG y leerla con el tool `view` en lugar de
    confiar solo en el OCR. Guardar el texto OCR en `/tmp/ocr/` porque los PDFs originales en
    `/mnt/user-data/uploads` pueden ser reemplazados cuando el usuario sube documentos nuevos.
15. **FECHA DE EMISIÓN DEL TÍTULO EN LETRA + NÚMERO.** El campo `fecha_emision_doc` (línea
    "Fecha:" del Título Accionario, bloque "Se hace constar") se escribe SIEMPRE con el día y el
    año en letra seguidos de su número entre paréntesis, con el formato:
    "<día en letra> (<día núm>) de <mes> de <año en letra> (<año núm>)." — p. ej.
    "cuatro (4) de febrero de dos mil catorce (2014)." El mes va solo en letra. Esta regla de
    FORMATO aplica únicamente a la fecha de emisión del Título; los asientos de los Libros conservan
    su propia convención (MAYÚSCULAS con número: "MÉXICO, DISTRITO FEDERAL A 04 DE FEBRERO DE 2014").
    **QUÉ FECHA usar (VALOR, no formato):** la fecha de emisión del Título = la MISMA fecha de firma
    del ÚLTIMO asiento que fija la estructura vigente, es decir protocolización/inscripción del
    último movimiento + 20/25 días (regla 3) — NO la fecha de protocolización a secas. Todo el
    expediente (Títulos, firma de asientos, "Corte al" de Tenencia y Apoderados) debe llevar esa
    MISMA fecha para quedar consistente. Ejemplo GDI: último movimiento protocolizado 29/04/2026 →
    firma de Asientos 4 y 5 = 28/05/2026 → los Títulos, la Tenencia y los Apoderados también van
    al 28/05/2026.
16. **LOS TÍTULOS ACCIONARIOS YA NO LLEVAN COLUMNA DE % (PORCENTAJE).** La tabla resumen del
    Título (`summaryTable`) tiene solo 4 columnas: "V. No. Título | Serie | Acciones | Numeración".
    Se eliminó la columna "% Capital". El campo `porcentaje` puede seguir en el modelo de datos como
    dato interno de reconciliación (validación 11), pero NO se imprime en ningún Título. No agregar
    de vuelta la columna de %.
17. **EL EXPEDIENTE SE ENTREGA COMPLETO: 6 DOCUMENTOS.** Desde la versión 4.6, además de (1)
    Títulos, (2) Libro de Registro y (3) Libro de Variaciones, el skill genera y entrega SIEMPRE:
    (4) Tabla de Tenencia Accionaria Vigente, (5) Apoderados y Poderes Vigentes, y (6) Historial
    Societario. Correr `gen_titulos.js` (doc 1) y `gen_libros.js` (docs 2-6). Entregar los 6 aunque
    el usuario solo pida "los títulos" o "el libro" — es la instrucción permanente del despacho.
18. **EL LIBRO DE VARIACIONES DE CAPITAL NO LLEVA CUADRO ACCIONARIO NI DOMICILIOS.** En
    `buildLibroVariaciones` cada asiento contiene ÚNICAMENTE: narrativa del acto + desglose de
    capital (`capitalBreakdown`) + notas legales aplicables (p. ej. usufructo) + firmas. NUNCA
    incluir `tablaAccionistas*` (cuadro de accionistas) ni `domBlock` (nombres con domicilios) — eso
    pertenece EXCLUSIVAMENTE al Libro de Registro de Accionistas/Socios. El de Variaciones solo
    documenta los movimientos de CAPITAL, no la relación de accionistas ni sus domicilios.
20. **APODERADOS Y PODERES: DOCUMENTO INFORMATIVO, SIN FIRMAS, POR INSTRUMENTO.** El documento de
    Apoderados es solo para conocimiento: NO lleva firmas. Estructura: (I) Órgano de gobierno +
    accionistas (Consejo desde `CONSEJO_VIGENTE`; accionistas desde la estructura vigente), y
    (II) apoderados con poder notarial vigente, relacionados POR INSTRUMENTO en `APODERADOS_INSTRUMENTOS`
    (una entrada por escritura de poderes, cada una con uno o más `grupos` = {desc de facultades,
    lista de nombres}); los nombres se imprimen en tabla de 3 columnas vía `nombresTable`. Incluir
    TODOS los apoderados vigentes de TODAS las escrituras de poderes (salvo revocación expresa) y
    cerrar con la nota que recomienda revocar para depurar el padrón. Si no hay datos de apoderados,
    dejar `APODERADOS_INSTRUMENTOS` vacío → se imprime la ALERTA. La alerta NO debe llevar doble
    etiqueta (el bloque ya antepone "⚠️ ALERTA:").
21. **SALIDA EN LA CARPETA COMPARTIDA DE RED.** `BASE_OUT` (en ambos scripts) apunta SIEMPRE a
    `\\10.1.100.14\Doc_Legal\Documentación legal\Claude (Libros Corporativos y Títulos Accionarios)`.
    El skill crea una subcarpeta por sociedad (`NOMBRE_ARCHIVO`) y dentro subcarpetas por documento
    (Titulos_Accionarios, Libro_Registro_Socios, Libro_Variaciones_Capital, Tenencia_Accionaria,
    Apoderados_y_Poderes, Historial_Societario, Beneficiario_Controlador). Así los 7 documentos quedan en la red y tanto el
    usuario como su jefe los ven. En JS la ruta va con doble backslash escapado:
    `"\\\\10.1.100.14\\Doc_Legal\\Documentación legal\\Claude (Libros Corporativos y Títulos Accionarios)"`.
    Si la red no está disponible, avisar al usuario y ofrecer guardar temporalmente en local.
22. **NÚMEROS A LETRAS SIEMPRE CALCULADOS, NUNCA POR DICCIONARIO FIJO (v5.2).** Antes, el número
    de acciones y el número de título se buscaban en diccionarios de pocos valores (`enPalabras`,
    `numWord`, arreglo `numWord2`); con cualquier sociedad distinta de Administradora GDI —o del
    5º título en adelante— imprimían `undefined`. Ahora existe la función `numeroALetras(num, genero)`
    (género "f" para acciones, "m" para el número de título) y `tituloEnLetras(t)`, que convierten
    CUALQUIER entero a letras en español con el género correcto. `enPalabras` se conserva solo como
    override manual: el patrón es `enPalabras[x] || numeroALetras(x)`. Verificado que reproduce
    exactamente las frases previas del diccionario. NUNCA volver a usar arreglos/diccionarios fijos
    de números sin salida alterna.
23. **APARTADO OBLIGATORIO DE BENEFICIARIO CONTROLADOR (PLD) — DOCUMENTO 7 (v5.3).** TODO expediente
    corporativo (sociedades Y fideicomisos) debe incluir el documento "Beneficiario Controlador",
    generado por `buildBeneficiarioControlador()` (gen_libros.js). Marco fijo (`BC_MARCO`/`BC_CRITERIO`):
    CFF arts. 32-B Ter/Quáter/Quinquies; Reglas 2.8.1.20-2.8.1.23 RMF; LFPIORPI + reforma jul-2025 +
    Reglamento; GAFI 24 y 25; y **Acuerdo 115/2026 (SHCP, DOF 07-ago-2026, vigor 30-nov-2026, Cap. III
    Quinquies, arts. 23 Quinquies a 23 Quinquies 3)**. Criterio en cascada: (I) persona física con ≥25%;
    (II) control por otros medios; (III) administración de mayor grado. **OBLIGACIÓN DE MAPEO:** cuando un
    accionista/socio sea persona moral, hay que TRAZAR su estructura hasta la(s) persona(s) física(s)
    (participación efectiva indirecta = producto de porcentajes a lo largo de la cadena), llenando
    `BENEFICIARIO_CONTROLADOR` (nombre, curp, rfc, nac, pct efectivo, criterio) y `BC_MAPEO` (sociedad,
    %, titularidad última). Datos personales de cada BC: nombre, CURP, RFC, nacionalidad, % (obtenerlos
    de la CSF/KYC/actas). Las cargas por usufructo se atribuyen al usufructuario mientras siga como
    titular en las tenencias vigentes; la nuda propiedad se anota como referencia. Registro del BC:
    conservar mínimo 10 años. Ver plantilla detallada de fideicomiso en la carpeta maestra de red
    "PLANTILLA Expediente Fideicomiso".
19. **EN LOS CUADROS ACCIONARIOS, EL RFC VA DEBAJO DEL NOMBRE (salto de línea real).** docx NO
    rompe línea con un "\n" suelto dentro de un `<w:t>` (lo renderiza pegado). Las celdas
    (`hdrCell`/`dataCell`) usan el helper `mlRuns()`, que parte el texto por "\n" y emite un
    `<w:br/>` real por salto. Así, la celda del titular muestra el nombre en un renglón y "RFC: ..."
    debajo. No volver a meter el nombre y el RFC en un solo run sin `mlRuns`.

## CUÁNDO USAR ESTE SKILL

Cuando el usuario escriba cualquiera de estas frases (o similares):
- "nueva empresa", "nueva sociedad", "expediente corporativo"
- "títulos accionarios", "generar títulos"
- "libro de acciones", "libro de socios"
- "documentos societarios", "documentación corporativa"
- O invoque el skill directamente

---

## QUÉ HACE ESTE SKILL

Guía al usuario a través de un flujo conversacional estructurado para capturar los datos de cualquier sociedad mercantil mexicana y genera automáticamente los documentos corporativos Word (.docx) correspondientes:

**EXPEDIENTE COMPLETO POR DEFECTO = 7 documentos.** Siempre generar y entregar los 7, aunque el usuario no los liste uno por uno.

- **Grupo A** (S.A., S.A. de C.V., S.A.P.I., S.A.S., S. en C. por A.):
  1. Títulos Accionarios individuales (Art. 125 LGSM) — uno por accionista, con cuponera de 9 dividendos  *(script gen_titulos.js)*
  2. Libro de Registro de Acciones (Art. 128 LGSM)  *(gen_libros.js)*
  3. Libro de Variaciones de Capital  *(gen_libros.js)*
  4. Tabla de Tenencia Accionaria Vigente  *(gen_libros.js — `buildTenencia`)*
  5. Apoderados y Poderes Vigentes  *(gen_libros.js — `buildApoderados`)*
  6. Historial Societario  *(gen_libros.js — `buildHistorial`)*
  7. Beneficiario Controlador (PLD)  *(gen_libros.js — `buildBeneficiarioControlador`)*

- **Grupo B** (S. de R.L., S.C., S. en N.C., S. en C.S., A.C.):
  1. Libro de Registro de Socios / Asociados
  2. Libro de Variaciones de Capital (excepto A.C.)
  3. Tabla de Tenencia (Partes Sociales) Vigente
  4. Apoderados y Poderes Vigentes
  5. Historial Societario
  6. Beneficiario Controlador (PLD)

**Datos que capturan los documentos 4-7** (además de EMP/SOCIOS): la Tenencia se agrega automáticamente desde la estructura vigente de accionistas/socios (agregando `anotacion` a un socio si tiene usufructo u otra carga); Apoderados usa `CONSEJO_VIGENTE` (nombre/cargo/designación) + `APODERADOS` (si va vacío se imprime una ALERTA pidiendo revisar la escritura de poderes); Historial usa el arreglo `HISTORIAL` (línea de tiempo: fecha, instrumento, movimiento, asiento). El **Beneficiario Controlador** (regla 23) usa `BENEFICIARIO_CONTROLADOR` (personas físicas que son BC, con CURP/RFC/nacionalidad/% y criterio), `BC_MAPEO` (cadena de control cuando hay socios personas morales) y `BC_CONCLUSION`; el marco normativo y el criterio (`BC_MARCO`/`BC_CRITERIO`) son fijos. Ver los datos DEMO en `gen_libros.js`.

---

## REGISTRO DE TIPOS SOCIETARIOS (SOCIEDAD_REGISTRY)

| Tipo | puede_titulos | termino_socio | termino_partes | Marco |
|------|--------------|---------------|----------------|-------|
| S.A. | SÍ | Accionista | Acciones | LGSM 87-206 |
| S.A. de C.V. | SÍ | Accionista | Acciones | LGSM 87-206, 213-221 |
| S.A.P.I. | SÍ | Accionista | Acciones | LGSM 87-206 |
| S.A.P.I. de C.V. | SÍ | Accionista | Acciones | LGSM 87-206 |
| S.A.S. | SÍ | Accionista | Acciones | LGSM 260-264 |
| S. en C. por A. | SÍ | Accionista | Acciones | LGSM 207-211 |
| S. de R.L. | NO | Socio | Partes Sociales | LGSM 58-86 |
| S. de R.L. de C.V. | NO | Socio | Partes Sociales | LGSM 58-86, 213-221 |
| S. en N.C. | NO | Socio | Participación | LGSM 25-57 |
| S. en C.S. | NO | Socio | Participación | LGSM 51-57 |
| S.C. | NO | Socio | Participación | CCF 2688-2720 |
| A.C. | NO | Asociado | Aportación | CCF 2670-2687 |

---


---

## CARGA DE DOCUMENTOS CORPORATIVOS

Esta sección aplica cuando el usuario quiere alimentar el sistema con documentos reales de una empresa (actas, escrituras, estatutos, CSFs, etc.) en lugar de capturar datos manualmente.

---

### OPCIÓN A — SUBIR ARCHIVOS DIRECTAMENTE AL CHAT

El usuario puede adjuntar archivos directamente arrastrándolos al chat o usando el botón de adjuntos.

**Formatos soportados:**
| Formato | Método de lectura |
|---------|------------------|
| PDF nativo (texto seleccionable) | `pdftotext` o `pymupdf` |
| PDF escaneado (imagen) | OCR con `tesseract` |
| JPG / PNG | Claude lo lee directamente (multimodal) |
| DOCX (Word) | `python-docx` |

**Flujo al recibir archivos adjuntos:**
1. Identificar los archivos en `/uploads/` del entorno de trabajo
2. Procesarlos con el extractor adecuado según el tipo
3. Clasificar cada documento (ver tabla más abajo)
4. Extraer los datos relevantes
5. Confirmar hallazgos con el usuario antes de generar

---

### OPCIÓN B — CONECTAR CARPETA DE LA LAPTOP

Permite a Claude leer documentos directamente desde una carpeta del equipo del usuario, sin necesidad de subirlos uno a uno.

**Instrucciones para Claude:**

```
1. Cargar la herramienta:
   ToolSearch("select:mcp__cowork__request_cowork_directory")

2. Llamarla para pedir al usuario que seleccione su carpeta:
   mcp__cowork__request_cowork_directory()
   → El sistema mostrará un diálogo para que el usuario elija la carpeta

3. Una vez conectada, listar los archivos:
   bash: ls -la "/ruta/montada/"

4. Mostrar al usuario la lista de archivos encontrados:
   📂 Carpeta conectada: [nombre]
   Archivos encontrados:
   • acta_constitutiva.pdf
   • asamblea_2012.pdf
   • csf_simon.pdf
   ...

5. Preguntar: ¿Proceso todos los archivos o selecciona cuáles?

6. Procesar en orden cronológico recomendado:
   Acta Constitutiva → Asambleas (por fecha) → CSFs → Otros
```

---

⛔ **ERROR FRECUENTE — "no puede leer archivos de mi red de la oficina / de mi laptop"**
(incidente real: usuario con documentos en una carpeta compartida de red de la oficina y
en carpetas de su laptop distintas a la carpeta ya conectada — Claude no podía leerlos).

**Causa real:** tener UNA carpeta conectada (p. ej. "TITULOS ACCIONARIOS") NO da acceso al
resto de la computadora ni a unidades de red. Cada carpeta o unidad distinta que el usuario
quiera usar debe conectarse POR SEPARADO con `request_cowork_directory`. Esto no es un
error del skill — es el modelo de permisos de Cowork: solo se puede leer lo que el usuario
sube al chat o lo que conecta explícitamente.

**Qué debe hacer Claude cuando el usuario menciona archivos en OTRA ubicación** (otra
carpeta del equipo, un disco distinto, o una unidad de red compartida):

1. NO asumir que ya tiene acceso solo porque hay una carpeta conectada de un proyecto
   anterior. Preguntar primero: "¿Esos documentos están en una carpeta o unidad distinta a
   la que ya tenemos conectada? Si es así, necesito que la conectes por separado."
2. Volver a llamar `mcp__cowork__request_cowork_directory()` para que el usuario seleccione
   ESA carpeta o unidad específica (el diálogo de selección de Windows permite navegar a
   cualquier unidad, incluida una unidad de red YA MAPEADA con letra de unidad, p. ej.
   `Z:\`).
3. **Unidades de red (network shares) — límite real que hay que explicarle al usuario:**
   - Si la carpeta de red YA está mapeada como unidad con letra (`Z:\`, `\\servidor\carpeta`
     visible en "Este equipo" de Windows), sí se puede seleccionar y conectar normalmente.
   - Si NO está mapeada (solo aparece como ruta UNC sin letra de unidad, o requiere VPN/
     credenciales que Windows no tiene guardadas), Cowork no puede alcanzarla directamente.
     En ese caso, decirle al usuario con claridad: "no puedo conectar una unidad de red que
     tu Windows no tenga mapeada como unidad local — o mapea la unidad primero (pídele a tu
     equipo de IT si no sabes cómo), o copia esos archivos a una carpeta local (por ejemplo
     el Escritorio o Documentos) y conecta esa carpeta, o súbelos directamente al chat."
   - NUNCA prometer acceso a rutas de red que no se han probado — verificar SIEMPRE con
     `ls -la` después de conectar, antes de decir que ya se puede proceder.
4. Después de conectar cualquier carpeta nueva, listar su contenido con `ls -la` y
   confirmarlo con el usuario antes de seguir — igual que en el flujo normal de OPCIÓN B.
5. Si el usuario ya subió archivos al chat pero Claude no los encuentra en `/uploads/`,
   esperar unos segundos (el adjunto puede tardar en sincronizar) y volver a listar la
   carpeta antes de reportar el problema — no reportar "no puedo leer el archivo" en el
   primer intento sin reintentar.

---

### LECTURA DE DOCUMENTOS — COMANDOS BASH

#### PDF Nativo (texto seleccionable)
```bash
pdftotext "/ruta/archivo.pdf" - 2>/dev/null
# o con pymupdf (más preciso, mantiene estructura):
python3 -c "
import fitz
doc = fitz.open('/ruta/archivo.pdf')
for page in doc:
    print(page.get_text())
"
```

#### PDF Escaneado (solo imágenes, sin texto seleccionable)
⚠️ La MAYORÍA de las escrituras están escaneadas. Analizar SIEMPRE cada documento HOJA POR HOJA
(todas las páginas), no solo las que parezcan relevantes: los ANTECEDENTES traen datos de
inscripción del RPC y las tablas/acuerdos de capital pueden estar dispersos (corrección 14).
```bash
# 1) ¿Está escaneado? (si devuelve ~0 caracteres → sí → OCR)
pdftotext "/ruta/archivo.pdf" - 2>/dev/null | tr -d '\f\n ' | wc -c

# 2) Instalar idioma español de tesseract si falta (método probado, sin sudo, desde github):
mkdir -p /tmp/tessdata
curl -sL -o /tmp/tessdata/spa.traineddata \
  "https://raw.githubusercontent.com/tesseract-ocr/tessdata_fast/main/spa.traineddata"
cp /usr/share/tesseract-ocr/5/tessdata/{osd,eng}.traineddata /tmp/tessdata/ 2>/dev/null
export TESSDATA_PREFIX=/tmp/tessdata
tesseract --list-langs      # debe listar: eng, osd, spa

# 3) OCR completo en español. Guardar a /tmp/ocr/ porque los PDFs en /mnt/user-data/uploads
#    pueden ser reemplazados cuando el usuario sube documentos nuevos (los .txt en /tmp persisten).
mkdir -p /tmp/ocr
python3 -c "
import fitz, pytesseract, os, io
from PIL import Image
os.environ['TESSDATA_PREFIX']='/tmp/tessdata'
doc = fitz.open('/ruta/archivo.pdf'); out=[]
for i, page in enumerate(doc):
    pix = page.get_pixmap(dpi=200)   # 190-250; documentos largos: bajar dpi para no exceder tiempo
    img = Image.open(io.BytesIO(pix.tobytes('png')))
    out.append(f'\n===== PÁGINA {i+1} =====\n' + pytesseract.image_to_string(img, lang='spa'))
open('/tmp/ocr/archivo.txt','w').write('\n'.join(out))
"

# 4) TABLAS CON PORCENTAJES / cifras finas: el OCR a veces corta la columna de %. Para
#    transcribir EXACTO (corrección 11), renderizar la página/recorte a PNG y LEERLO con 'view':
python3 -c "
import fitz
p = fitz.open('/ruta/archivo.pdf')[13]                 # página objetivo
r = p.rect; crop = fitz.Rect(0, r.height*0.28, r.width, r.height*0.70)
p.get_pixmap(dpi=250, clip=crop).save('/tmp/tabla.png')
"
#   → luego: view /tmp/tabla.png  y transcribir porcentajes/valores tal cual.
```

#### Documento Word (.docx)
```bash
python3 -c "
from docx import Document
doc = Document('/ruta/archivo.docx')
for para in doc.paragraphs:
    if para.text.strip():
        print(para.text)
"
```

#### Imagen JPG / PNG
Usar el tool `Read` directamente — Claude lo ve de forma nativa (multimodal). No requiere bash.

---

### CLASIFICACIÓN AUTOMÁTICA DE DOCUMENTOS

Al procesar el texto extraído, identificar el tipo de documento según estas palabras clave:

| Tipo | Palabras clave principales |
|------|--------------------------|
| **Acta Constitutiva** | "se constituye", "acta constitutiva", "objeto social", "escritura de constitución", "socios fundadores" |
| **Asamblea Extraordinaria** | "asamblea general extraordinaria", "reforma", "modificación", "aumento de capital", "disminución de capital", "fusión", "escisión" |
| **Asamblea Ordinaria** | "asamblea general ordinaria", "estados financieros", "consejo de administración", "renovación", "comisario" |
| **Aumento de Capital** | "aumento de capital", "nuevas acciones", "suscripción", "emisión de acciones" |
| **Disminución de Capital** | "reducción de capital", "amortización de acciones", "reembolso" |
| **Cesión / Compraventa** | "cede", "cesión de acciones", "compraventa de acciones", "transmisión", "endoso" |
| **Donación / Nuda Propiedad** | "donación", "nuda propiedad", "usufructo", "usufructuario" |
| **Protocolización Notarial** | "escritura pública", "notario", "protocolo", "primer testimonio" |
| **Libro de Registro** | "libro de registro", "folio", "registro de acciones", "registro de socios" |
| **Título Accionario** | "título accionario", "ampara", "acciones nominativas", "cupones" |
| **Poder Notarial** | "poder", "apoderado", "representa", "facultades" |
| **CSF / SAT** | "constancia de situación fiscal", "servicio de administración tributaria", "RFC" |

---

### EXTRACCIÓN DE DATOS POR TIPO DE DOCUMENTO

#### Acta Constitutiva / Protocolización
Extraer obligatoriamente:
- **Denominación social** y tipo (S.A. de C.V., S. de R.L., etc.)
- **Número de Escritura Pública** (E.P. N°)
- **Fecha** del instrumento
- **Notario**: nombre completo, número de notaría, estado/ciudad
- **RPC**: folio mercantil, fecha de inscripción, lugar
- **Capital social**: fijo, variable (si aplica), total
- **Valor nominal** por acción/parte
- **Series** (I, II, A, B, etc.)
- **Socios fundadores**: nombre, RFC, número de acciones, serie, valor
- **Objeto social** (resumen)
- **Domicilio social** de la empresa
- **Duración** (años)

#### Asamblea (cualquier tipo)
Extraer:
- **Fecha de la asamblea**
- **Tipo** (ordinaria / extraordinaria)
- **Instrumento de protocolización** (si ya fue ante notario)
- **Resoluciones**: lista numerada de lo que se acordó
- **Cambios en capital** (si aplica): montos anteriores y nuevos
- **Cambios en socios** (si aplica): quién entra, sale, o cambia participación
- **Cambios en el consejo** (si aplica): nuevos cargos
- **Cambios en estatutos** (si aplica): qué artículos se modificaron

#### Cesión / Compraventa / Donación de Acciones
Extraer:
- **Cedente** (quien vende/cede/dona): nombre, RFC
- **Cesionario** (quien recibe): nombre, RFC
- **Acciones transmitidas**: número, serie, valor nominal
- **Precio o valor de cesión**
- **Fecha** del acto
- **Instrumento** de protocolización
- ¿Hubo desmembramiento? (nuda propiedad / usufructo)

#### Tabla de resumen por documento procesado

Tras procesar cada documento, mostrar:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  DOCUMENTO PROCESADO: [nombre del archivo]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Tipo:        [Acta Constitutiva / Asamblea / etc.]
📅 Fecha:       [dd/mm/aaaa]
📝 Instrumento: E.P. N° [X], Notario N° [X], [lugar]
💰 Capital:     Fijo $XX / Variable $XX / Total $XX
👥 Socios:      [lista breve]
⚠️  Alertas:    [si hay inconsistencias o datos faltantes]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### FLUJO COMPLETO CON DOCUMENTOS

```
INICIO
  │
  ├─► ¿El usuario sube archivos? → Procesar con extractores según tipo
  │
  └─► ¿El usuario quiere conectar carpeta?
        │
        └─► request_cowork_directory() → listar archivos → ordenar cronológicamente
              │
              └─► Para cada documento:
                    1. Extraer texto
                    2. Clasificar tipo
                    3. Extraer datos estructurados
                    4. Acumular historial societario
              │
              └─► Mostrar resumen del expediente acumulado
              │
              └─► Validar: ¿capital consistente? ¿socios sin gaps? ¿dominios faltantes?
              │
              └─► Generar 3 documentos con estructura de carpetas
```

---

### REGLAS DE PRIORIDAD EN EXTRACCIÓN

1. **El texto del instrumento notarial es fuente primaria** — siempre preferir sobre otras fuentes
2. **Si hay conflicto entre documentos** → señalarlo como alerta y preguntar al usuario cuál prevalece
3. **Socios sin RFC** → marcar como `RFC_PENDIENTE` y avisar al usuario
4. **Domicilios faltantes** → marcar como `PENDIENTE DE CONSTANCIA DE SITUACIÓN FISCAL`
5. **Capital inconsistente** → sumar acciones de la tabla y comparar con el total declarado; alertar si difieren
6. **Fechas ambiguas** → siempre pedir confirmación
7. **Porcentajes** → usar los del documento (por accionista), NO recalcular a capricho. Verificar que
   los % de TODOS los títulos SUMEN 100% y que, agregados por accionista, reconcilien con la escritura.
   Si un accionista está en varios títulos, distribuir su % entre ellos según acciones (ver corrección 11)
8. **Datos registrales/notariales** → transcribir VERBATIM (nombre exacto del RPC, partida, volumen,
   libro, fecha, folio mercantil electrónico, notario), sin abreviar ni parafrasear (ver corrección 13)
9. **Base por serie en los títulos** → "de un total de N" = total de la serie del título, no el gran
   total; no mezclar capital fijo y variable (ver corrección 12)
10. **Fechas completas en letra, sin abreviaturas** en todo el expediente (ver corrección 13)
11. **Análisis hoja por hoja + OCR en español** de documentos escaneados (ver corrección 14)
12. **Decisiones de formato no cubiertas por una regla explícita** (numeración de títulos tras cambio
    de estructura, fecha base de +20/25 días sin evidencia de inscripción RPC, etc.) → preguntar al
    usuario antes de generar, no asumir

---

## FLUJO DE TRABAJO (EJECUTAR SIEMPRE EN ESTE ORDEN)

### ► FASE 1 — INICIO

Al activarse el skill, mostrar exactamente esto y comenzar la Fase 2:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ADMINISTRACIÓN SOCIETARIA · GDI
  Expediente Corporativo Completo v3.0
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Guiaré el proceso de captura de datos y generaré
automáticamente todos los documentos corporativos
que correspondan al tipo de sociedad.

Escriba "cancelar" en cualquier momento para salir.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### ► FASE 2 — DATOS DE LA SOCIEDAD

Hacer las siguientes preguntas en bloques lógicos. Esperar respuesta antes de continuar.

**Bloque A — Identidad:**
```
📋 BLOQUE 1 DE 4 — DATOS DE LA SOCIEDAD

1. ¿Cuál es la denominación social? (sin el tipo societario)
2. ¿Cuál es el tipo societario?
   S.A. / S.A. de C.V. / S.A.P.I. / S.A.P.I. de C.V. / S.A.S. /
   S. en C. por A. / S. de R.L. / S. de R.L. de C.V. /
   S. en N.C. / S. en C.S. / S.C. / A.C.
3. ¿Cuál es el RFC de la sociedad?
4. ¿Cuál es el domicilio social completo?
```

Tras recibir el tipo societario, confirmar inmediatamente:
```
✅ Tipo reconocido: [TIPO]
📄 Se generarán: [lista de documentos según SOCIEDAD_REGISTRY]
⚖️  Marco: [marco legal]
```

**Bloque B — Capital:**
```
📋 BLOQUE 2 DE 4 — CAPITAL SOCIAL

5. ¿Cuál es el capital social total? (en pesos M.N.)
6. ¿El capital es variable? Si es así: ¿cuánto es la parte fija y cuánto la variable?
7. ¿Cuántas [acciones/partes sociales] se han emitido en total?
8. ¿Cuál es el valor nominal por [acción/parte]? (ej: $1.00 M.N.)
```

**Bloque C — Constitución:**
```
📋 BLOQUE 3 DE 4 — DATOS DE CONSTITUCIÓN

9.  Fecha de constitución (DD/MM/AAAA)
10. Duración de la sociedad (ej: 99 años)
11. Objeto social (descripción breve)
12. Instrumento notarial: Escritura Pública N°___, Notario N°___, Notaría ___, Ciudad ___, Fecha ___
13. Registro Público de Comercio: Folio ___, Ciudad ___, Fecha ___
```

**Bloque D — Representación:**
```
📋 BLOQUE 4 DE 4 — REPRESENTACIÓN LEGAL

14. Nombre del Presidente del Consejo de Administración (o Administrador Único)
15. Cargo exacto (default: "Presidente del Consejo de Administración")
16. Nombre del Secretario del Consejo de Administración
17. Cargo exacto (default: "Secretario del Consejo de Administración")
```

### ► FASE 3 — SOCIOS / ACCIONISTAS

Usar el término correcto del SOCIEDAD_REGISTRY.

Para cada socio/accionista, preguntar:
```
👤 [ACCIONISTA/SOCIO] N° [número]

a) Nombre completo (persona física o moral)
b) RFC
c) Número de [acciones/partes/aportación]
d) Porcentaje de participación (%)
[Solo Grupo A:]
e) Número de título (consecutivo, ej: 0001)
f) Serie (default: I)
g) ¿Las acciones corresponden al capital fijo o variable?
[Todos:]
h) Nacionalidad (default: Mexicana)
i) Domicilio
j) Fecha de emisión / ingreso (DD/MM/AAAA)
k) Nota especial (usufructo, nuda propiedad, etc.) — puede quedar en blanco
```

Después de cada uno: "¿Hay otro [accionista/socio] que registrar? (sí / no)"

Al terminar, mostrar tabla resumen:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RESUMEN DE [ACCIONISTAS/SOCIOS]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
N° | Nombre | RFC | [Acciones] | %   [| Título N°]
── | ─────  | ─── | ──────────── | ──  [| ─────────]
...

Total capturado : [suma acciones] / [total declarado]
% acumulado     : [suma]%  [✅ 100% / ❌ diferencia]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### ► FASE 4 — VALIDACIÓN LEGAL

Ejecutar TODAS las validaciones. Con errores ❌: NO generar hasta que se corrijan.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
VALIDACIÓN LEGAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅/❌  [1] Tipo societario válido
✅/❌  [2] Capital social > 0
✅/❌  [3] Capital fijo + variable = capital total (±$1.00)
✅/❌  [4] Al menos 1 socio/accionista
✅/❌  [5] Suma de acciones = total declarado (±1)
✅/❌  [6] Suma de porcentajes por accionista = 100% (±0.1%)
✅/❌  [7] Sin RFC duplicados
✅/❌  [8] Sin números de título duplicados [solo Grupo A]
✅/❌  [9] Sin rangos de acciones solapados [solo Grupo A]
✅/❌ [10] Acciones × valor nominal ≤ capital total
✅/❌ [11] Suma de % de TODOS los títulos = 100% (±0.05%) y reconcilian por accionista con la escritura [Grupo A] (corrección 11)
✅/❌ [12] Cada título referencia el total de SU serie en "de un total de N" (no el gran total; no mezcla fija/variable) [Grupo A] (corrección 12)
✅/❌ [13] Fechas en letra y sin abreviaturas; datos del RPC/notario transcritos verbatim (corrección 13)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Si todo pasa:
```
✅ Todos los datos son válidos.
¿Confirmas que debo generar el expediente corporativo? (sí / no)
```

### ► FASE 5 — GENERACIÓN DE DOCUMENTOS

Cuando el usuario confirme los datos, ejecutar los scripts Node.js con bash:

```bash
mkdir -p /tmp/gen_corp
cd /tmp/gen_corp && npm install docx
# Escribir gen_titulos.js y gen_libros.js (ver templates al final del skill)
node gen_titulos.js
node gen_libros.js
```

Los scripts crean automáticamente:
```
[BASE_OUT]/
└── [NOMBRE_ARCHIVO]/
    ├── Titulos_Accionarios/
    │   └── Titulos_Accionarios_[NOMBRE].docx
    ├── Libro_Variaciones_Capital/
    │   └── Libro_Variaciones_Capital_[NOMBRE].docx
    └── Libro_Registro_Socios/
        └── Libro_Registro_Socios_[NOMBRE].docx
```

⚠️ `BASE_OUT` debe ser la ruta Linux de la carpeta conectada del usuario  
⚠️ `NOMBRE_ARCHIVO` = nombre corto de la empresa sin espacios (ej: `"EMPRESA_XYZ"`)

---

## LECTURA DE CONSTANCIAS DE SITUACIÓN FISCAL (CSF)

### ¿Cuándo aplica?

Siempre que el usuario cargue un archivo que sea una **Constancia de Situación Fiscal** (emitida por el SAT). Puede ser PDF, imagen (JPG, PNG) o cualquier formato legible. Esta sección también aplica cuando el usuario dice frases como:
- "aquí está la constancia"
- "adjunto la CSF de..."
- "sube la constancia fiscal"
- "tengo la constancia de situación fiscal"

### ¿Qué extraer?

De cada CSF, extraer los siguientes campos con **precisión textual exacta** (transcribir literalmente, sin abreviar):

| Campo | Ubicación en la CSF |
|-------|---------------------|
| RFC | Encabezado o datos generales |
| Nombre / Razón Social | Datos del contribuyente |
| Calle y número exterior/interior | Domicilio fiscal |
| Colonia | Domicilio fiscal |
| Municipio o Alcaldía | Domicilio fiscal |
| Estado | Domicilio fiscal |
| Código Postal | Domicilio fiscal |

### Formato de domicilio fiscal para asientos

Construir el domicilio en este formato exacto (una sola línea, sin saltos):

```
[CALLE] [N° EXTERIOR][, INT. N° INTERIOR si aplica], COLONIA [COLONIA], [MUNICIPIO O ALCALDÍA], [ESTADO], C.P. [CÓDIGO POSTAL]
```

Ejemplo:
```
INSURGENTES SUR 1602, INT. PISO 9, COLONIA CRÉDITO CONSTRUCTOR, BENITO JUÁREZ, CIUDAD DE MÉXICO, C.P. 03940
```

### Mapeo a accionistas

- Comparar el **RFC de la CSF** con los RFC de los accionistas/socios registrados.
- Si el RFC coincide → asignar el domicilio a ese accionista.
- Si no coincide → notificar al usuario: *"El RFC [RFC_CSF] de la CSF no coincide con ningún accionista registrado. ¿Desea registrarlo de todas formas?"*
- Si hay más de un accionista sin CSF → pedir las constancias pendientes antes de regenerar los documentos.

### Bloque de domicilio en los asientos (formato ASIENTO)

Una vez que el domicilio esté disponible, en cada asiento de los libros corporativos el bloque de domicilio se redacta así:

> **[NOMBRE COMPLETO EN MAYÚSCULAS]** de nacionalidad mexicana, ha señalado como su domicilio para efectos de este asiento el ubicado en [DOMICILIO COMPLETO].

Si el accionista es **persona moral** (S.A., S.A. de C.V., etc.):

> **[RAZÓN SOCIAL EN MAYÚSCULAS]** sociedad de nacionalidad mexicana, ha señalado como su domicilio fiscal para efectos de este asiento el ubicado en [DOMICILIO COMPLETO].

### Flujo tras recibir una o más CSFs

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  CONSTANCIAS DE SITUACIÓN FISCAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

1. Leer cada CSF y mostrar tabla de confirmación:

```
📋 CONSTANCIAS PROCESADAS

RFC             | Nombre / Razón Social        | Domicilio Fiscal
──────────────  | ───────────────────────────  | ─────────────────
[RFC_1]         | [NOMBRE_1]                   | [DOMICILIO_1]
[RFC_2]         | [NOMBRE_2]                   | [DOMICILIO_2]
...
```

2. Mostrar estado de cobertura:

```
✅ Con domicilio: [N] accionista(s)
⏳ Pendientes  : [N] accionista(s) — [RFC / Nombre de los pendientes]
```

3. Si todos los accionistas tienen domicilio → preguntar:

```
¿Desea que regenere los documentos corporativos con los domicilios capturados? (sí / no)
```

4. Si hay pendientes → indicar cuáles faltan y esperar las constancias restantes antes de regenerar.

5. Al confirmar regeneración:
   - Sustituir todos los bloques `PENDIENTE DE CONSTANCIA DE SITUACIÓN FISCAL` por el domicilio real.
   - Regenerar usando el mismo script Node.js (`gen_libros.js`) con las variables de domicilio actualizadas.
   - Presentar los documentos al usuario.

### Nota técnica — variables de domicilio en el script

En el script `gen_libros.js`, el domicilio de cada accionista se pasa como la constante `DOM_PENDING`. Al regenerar, esta constante debe reemplazarse por un objeto de domicilios indexado por RFC:

```javascript
const DOMICILIOS = {
  "RFC_1": "CALLE X N° Y, COLONIA Z, ALCALDÍA/MUNICIPIO, ESTADO, C.P. XXXXX",
  "RFC_2": "...",
  // ...
};
// Uso: DOMICILIOS[socio.rfc] || "PENDIENTE DE CONSTANCIA DE SITUACIÓN FISCAL"
```

---

---

## CÓDIGO NODE.JS — GENERADORES COMPLETOS

### ⚠️ REGLA CRÍTICA — DOF 2016-01-29

**Documentos anteriores al 29/01/2016** → "Delegación X", "Distrito Federal"  
**Documentos del 29/01/2016 en adelante** → "Alcaldía X", "Ciudad de México"

Esta regla aplica **por asiento** según la fecha del instrumento, no la fecha actual.

### ESTRUCTURA DE CARPETAS AUTOMÁTICA

```
BASE_OUT / NOMBRE_ARCHIVO / Titulos_Accionarios /       ← gen_titulos.js
BASE_OUT / NOMBRE_ARCHIVO / Libro_Variaciones_Capital / ← gen_libros.js
BASE_OUT / NOMBRE_ARCHIVO / Libro_Registro_Socios /     ← gen_libros.js
```

### INSTALACIÓN (UNA SOLA VEZ)

```bash
mkdir -p /tmp/gen_corp && cd /tmp/gen_corp && npm install docx
```

---

### TEMPLATE A — TÍTULOS ACCIONARIOS (gen_titulos.js)

Cambiar: `NOMBRE_ARCHIVO`, `BASE_OUT`, objeto `EMP`, array `SOCIOS`.

```javascript
"use strict";
const { 
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  AlignmentType, WidthType, BorderStyle, ShadingType, VerticalAlign,
  PageBreak
} = require("docx");
const fs = require("fs");
const path = require("path");

// ─────────────────────────────────────────────────────
// CONSTANTES FORMAT_LOCK_V16
// ─────────────────────────────────────────────────────
const TW = 10224;           // content width DXA (12240 - 2*1008)
const NAVY = "1A2B4A";
const GOLD = "B0860C";
const LGRAY = "F4F6FA";     // light blue-gray background
const CCCC = "CCCCCC";      // cell border gray
const FONT = "Times New Roman";
const NO_BORDER = { style: BorderStyle.NONE, size: 0, color: "FFFFFF" };
const NAVY_BORDER = { style: BorderStyle.SINGLE, size: 6, color: NAVY };
const GRAY_BORDER = { style: BorderStyle.SINGLE, size: 1, color: CCCC };

function run(txt, opts = {}) {
  return new TextRun({
    text: txt,
    font: FONT,
    size: opts.size || 14,
    bold: opts.bold || false,
    italics: opts.italic || false,
    color: opts.color || "000000",
    underline: opts.underline ? {} : undefined,
  });
}

function para(runs, align = AlignmentType.LEFT, spacing = { before: 0, after: 0 }) {
  return new Paragraph({
    children: Array.isArray(runs) ? runs : [runs],
    alignment: align,
    spacing,
  });
}

function emptyPara(spacingAfter = 0) {
  return new Paragraph({ children: [new TextRun({ text: "", font: FONT, size: 14 })], spacing: { before: 0, after: spacingAfter } });
}

// ─────────────────────────────────────────────────────
// FULL-WIDTH TABLE HELPERS
// ─────────────────────────────────────────────────────
function navyHeaderTable(lines) {
  // lines = [{text, bold, size, color}]
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [TW],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [new TableCell({
        width: { size: TW, type: WidthType.DXA },
        shading: { fill: NAVY, type: ShadingType.CLEAR },
        borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER },
        margins: { top: 80, bottom: 80, left: 160, right: 160 },
        children: lines.map(l => para(
          run(l.text, { bold: l.bold || false, size: l.size || 14, color: "FFFFFF" }),
          AlignmentType.CENTER,
          { before: 0, after: l.after || 0 }
        )),
      })],
    })],
  });
}

function titleRow(titleText, numText) {
  // "TÍTULO ACCIONARIO DEFINITIVO" left | "N° X (WORD)" right
  // both cells have bottom border navy
  const botNavy = { style: BorderStyle.SINGLE, size: 8, color: NAVY };
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [6339, 3885],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [
        new TableCell({
          width: { size: 6339, type: WidthType.DXA },
          borders: { top: NO_BORDER, bottom: botNavy, left: NO_BORDER, right: NO_BORDER },
          margins: { top: 50, bottom: 50, left: 0, right: 0 },
          children: [para(run(titleText, { bold: true, size: 18, color: NAVY }), AlignmentType.LEFT, { before: 0, after: 0 })],
        }),
        new TableCell({
          width: { size: 3885, type: WidthType.DXA },
          borders: { top: NO_BORDER, bottom: botNavy, left: NO_BORDER, right: NO_BORDER },
          margins: { top: 50, bottom: 50, left: 0, right: 0 },
          children: [para(run(numText, { bold: true, size: 18, color: GOLD }), AlignmentType.RIGHT, { before: 0, after: 0 })],
        }),
      ],
    })],
  });
}

function descriptionBox(runs, extraPara) {
  const children = [new Paragraph({ children: runs, alignment: AlignmentType.BOTH, spacing: { before: 0, after: 0 } })];
  if (extraPara) children.push(extraPara);
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [TW],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [new TableCell({
        width: { size: TW, type: WidthType.DXA },
        shading: { fill: LGRAY, type: ShadingType.CLEAR },
        borders: { top: NO_BORDER, bottom: NO_BORDER, left: NAVY_BORDER, right: NO_BORDER },
        margins: { top: 55, bottom: 55, left: 120, right: 80 },
        children,
      })],
    })],
  });
}

// Inner key-value table (for sections I-IV)
function kvTable(width, rows, labelW, valueW) {
  return new Table({
    width: { size: width, type: WidthType.DXA },
    columnWidths: [labelW, valueW],
    borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER,
               insideH: { style: BorderStyle.SINGLE, size: 4, color: "AAAAAA" },
               insideV: { style: BorderStyle.SINGLE, size: 4, color: "AAAAAA" } },
    rows: rows.map(([label, value]) => new TableRow({
      children: [
        new TableCell({
          width: { size: labelW, type: WidthType.DXA },
          borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER },
          margins: { top: 40, bottom: 40, left: 80, right: 60 },
          children: [para(run(label, { bold: true, size: 13, color: "333333" }), AlignmentType.LEFT, { before: 0, after: 0 })],
        }),
        new TableCell({
          width: { size: valueW, type: WidthType.DXA },
          borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER },
          margins: { top: 40, bottom: 40, left: 80, right: 60 },
          children: [para(run(value, { size: 13 }), AlignmentType.LEFT, { before: 0, after: 0 })],
        }),
      ],
    })),
  });
}

function sectionLabel(text) {
  return para(run(text, { bold: true, size: 14, color: NAVY, underline: true }), AlignmentType.LEFT, { before: 30, after: 0 });
}

// Main 2-column table with sections I-IV
function mainSectionsTable(s, emp) {
  const LW = 5316, RW = 4908;
  const innerLW = 5156; // LW - 70 - 90 padding
  const innerRW = 4748; // RW - 70 - 90 padding
  const llabel = 2000, lvalue = 3156;
  const rlabel = 1700, rvalue = 3048;

  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [LW, RW],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [
        // LEFT: I. ACCIONISTA + II. SOCIEDAD
        new TableCell({
          width: { size: LW, type: WidthType.DXA },
          borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: GRAY_BORDER },
          margins: { top: 50, bottom: 50, left: 70, right: 90 },
          children: [
            sectionLabel("I.  ACCIONISTA"),
            kvTable(innerLW, [
              ["Nombre:", s.nombre],
              ["R.F.C.:", s.rfc],
              ["Nacionalidad:", s.nacionalidad],
              ["Domicilio:", s.domicilio],
            ], llabel, lvalue),
            emptyPara(0),
            sectionLabel("II.  SOCIEDAD"),
            kvTable(innerLW, [
              ["Denominación:", emp.nombre_completo],
              ["Domicilio:", emp.domicilio],
              ["Duración:", emp.duracion],
            ], llabel, lvalue),
          ],
        }),
        // RIGHT: III. CONSTITUCIÓN + IV. CAPITAL SOCIAL
        new TableCell({
          width: { size: RW, type: WidthType.DXA },
          borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER },
          margins: { top: 50, bottom: 50, left: 70, right: 90 },
          children: [
            sectionLabel("III.  CONSTITUCIÓN"),
            kvTable(innerRW, [
              ["Constitución:", emp.fecha_constitucion],
              ["Instrumento:", emp.instrumento_corto],
              ["Reg. Público de Comercio:", emp.rpc_corto],
            ], rlabel, rvalue),
            emptyPara(0),
            sectionLabel("IV.  CAPITAL SOCIAL"),
            kvTable(innerRW, [
              ["Capital total:", fmtMXN(emp.capital_total)],
              ["Parte fija:", fmtMXN(emp.capital_fijo)],
              ["Parte variable:", fmtMXN(emp.capital_variable)],
              ["Total acciones:", fmtNum(emp.total_acciones)],
              ["Valor nominal:", fmtMXN(emp.valor_nominal)],
            ], rlabel, rvalue),
          ],
        }),
      ],
    })],
  });
}

function objetoSocialTable(texto) {
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [TW],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [new TableCell({
        width: { size: TW, type: WidthType.DXA },
        borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER },
        margins: { top: 40, bottom: 40, left: 80, right: 80 },
        children: [new Paragraph({
          children: [
            run("Objeto Social: ", { bold: true, size: 13, color: "333333" }),
            run(texto, { size: 13 }),
          ],
          alignment: AlignmentType.BOTH,
          spacing: { before: 0, after: 0 },
        })],
      })],
    })],
  });
}

function summaryTable(s, emp, numDesde, numHasta) {
  // Sin columna de % (removida por indicación del despacho). 4 columnas que suman TW=10224.
  const cols = [1224, 2600, 3000, 3400];
  const hdrs = ["V. No. Título", "Serie", "Acciones", "Numeración"];
  const vals = [
    s.titulo,
    `Serie ${s.serie} — ${s.tipo_capital.toLowerCase()}`,
    `${fmtNum(s.acciones)} (${enPalabras[s.acciones] || numeroALetras(s.acciones)} ${s.serie === "A" ? "acciones" : "acciones"})`,
    `Del ${fmtNum(numDesde)} al ${fmtNum(numHasta)}`,
  ];
  const hdrRows = [new TableRow({
    children: hdrs.map((h, i) => new TableCell({
      width: { size: cols[i], type: WidthType.DXA },
      shading: { fill: NAVY, type: ShadingType.CLEAR },
      borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER },
      margins: { top: 40, bottom: 40, left: 80, right: 80 },
      children: [para(run(h, { bold: true, size: 13, color: "FFFFFF" }), AlignmentType.CENTER, { before: 0, after: 0 })],
    })),
  })];
  const valRows = [new TableRow({
    children: vals.map((v, i) => new TableCell({
      width: { size: cols[i], type: WidthType.DXA },
      borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER },
      margins: { top: 40, bottom: 40, left: 80, right: 80 },
      children: [para(run(v, { size: 13 }), AlignmentType.CENTER, { before: 0, after: 0 })],
    })),
  })];
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: cols,
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [...hdrRows, ...valRows],
  });
}

function derechosObligTable() {
  const DERECHOS = [
    "1. Voto en Asambleas (un voto por acción).",
    "2. Percibir dividendos en proporción a su participación.",
    "3. Suscripción preferente en aumentos de capital.",
    "4. Cuota de liquidación correspondiente.",
    "5. Derecho de información sobre estados financieros.",
  ];
  const OBLIGACIONES = [
    "1. Exhibir el título ante la Sociedad para anotaciones.",
    "2. Notificar cambios de domicilio a la Sociedad.",
    "3. Sujetarse a las resoluciones de la Asamblea.",
    "4. Responsabilidad limitada al importe suscrito.",
    "5. Observar limitaciones estatutarias de transmisión.",
  ];
  const half = Math.floor(TW / 2); // 5112
  const mkCell = (title, items) => new TableCell({
    width: { size: half, type: WidthType.DXA },
    borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER },
    margins: { top: 40, bottom: 40, left: 100, right: 80 },
    children: [
      para(run(title, { bold: true, size: 13, color: NAVY }), AlignmentType.LEFT, { before: 0, after: 20 }),
      ...items.map(it => para(run(it, { size: 12 }), AlignmentType.LEFT, { before: 0, after: 10 })),
    ],
  });
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [half, TW - half],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [mkCell("VI. DERECHOS", DERECHOS), mkCell("OBLIGACIONES", OBLIGACIONES)],
    })],
  });
}

function seHaceConstarTable(lugar, fecha) {
  const constText = [
    run("Se hace constar que las acciones amparadas por el presente título se encuentran ", { size: 13, italic: true }),
    run("íntegramente pagadas y liberadas", { size: 13, italic: true, bold: true }),
    run(", sin adeudo por concepto de su suscripción.", { size: 13, italic: true }),
  ];
  const lugarFecha = [
    run(`Lugar: ${lugar}.\n`, { size: 13, bold: true }),
    run(`Fecha: ${fecha}.`, { size: 13, bold: true }),
  ];
  const lw = 6800, rw = TW - 6800;
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [lw, rw],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [
        new TableCell({
          width: { size: lw, type: WidthType.DXA },
          borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: NO_BORDER },
          margins: { top: 50, bottom: 50, left: 80, right: 80 },
          children: [new Paragraph({ children: constText, alignment: AlignmentType.BOTH, spacing: { before: 0, after: 0 } })],
        }),
        new TableCell({
          width: { size: rw, type: WidthType.DXA },
          borders: { top: GRAY_BORDER, bottom: GRAY_BORDER, left: GRAY_BORDER, right: GRAY_BORDER },
          margins: { top: 50, bottom: 50, left: 80, right: 80 },
          children: [new Paragraph({ children: lugarFecha, alignment: AlignmentType.RIGHT, spacing: { before: 0, after: 0 } })],
        }),
      ],
    })],
  });
}

function signatureTable(emp) {
  const half = Math.floor(TW / 2);
  const botLine = { style: BorderStyle.SINGLE, size: 8, color: NAVY };
  const mkSig = (nombre, cargo) => new TableCell({
    width: { size: half, type: WidthType.DXA },
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER },
    margins: { top: 0, bottom: 0, left: 200, right: 200 },
    children: [
      new Paragraph({ children: [new TextRun({ text: "", font: FONT, size: 14 })], spacing: { before: 0, after: 500 }, border: { bottom: botLine } }),
      para(run(nombre, { bold: true, size: 14, color: NAVY }), AlignmentType.CENTER, { before: 60, after: 0 }),
      para(run(cargo, { size: 12, color: "555555" }), AlignmentType.CENTER, { before: 0, after: 0 }),
    ],
  });
  return new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [half, TW - half],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [
        mkSig(emp.presidente, emp.cargo_presidente),
        mkSig(emp.secretario, emp.cargo_secretario),
      ],
    })],
  });
}

// ─────────────────────────────────────────────────────
// CUPONES PAGE
// ─────────────────────────────────────────────────────
function cuponesPage(s, emp) {
  const CW = Math.floor(TW / 3); // 3408
  const cuponesTbl = [];
  const DASH_BORDER = { style: BorderStyle.DASHED, size: 6, color: "999999" };
  const SOLID_SM = { style: BorderStyle.SINGLE, size: 4, color: "AAAAAA" };

  for (let row = 0; row < 3; row++) {
    const cells = [];
    for (let col = 0; col < 3; col++) {
      const cupNum = row * 3 + col + 1;
      cells.push(new TableCell({
        width: { size: CW, type: WidthType.DXA },
        borders: {
          top: DASH_BORDER,
          bottom: DASH_BORDER,
          left: DASH_BORDER,
          right: DASH_BORDER,
        },
        margins: { top: 80, bottom: 80, left: 120, right: 120 },
        children: [
          // Cupón number + label
          new Paragraph({
            children: [
              run(`${cupNum}  `, { size: 20, bold: true, color: NAVY }),
              run("CUPÓN DE DIVIDENDO", { size: 12, bold: false, color: "444444" }),
            ],
            spacing: { before: 0, after: 30 },
          }),
          // Thin separator
          new Paragraph({
            children: [new TextRun({ text: "", font: FONT, size: 4 })],
            border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: "AAAAAA" } },
            spacing: { before: 0, after: 60 },
          }),
          new Paragraph({ children: [run(`Accionista: `, { size: 12, bold: true }), run(s.nombre, { size: 12 })], spacing: { before: 0, after: 20 } }),
          new Paragraph({ children: [run(`R.F.C.:  `, { size: 12, bold: true }), run(s.rfc, { size: 12 })], spacing: { before: 0, after: 20 } }),
          new Paragraph({ children: [run(`Título N°: `, { size: 12, bold: true }), run(`${s.titulo}   `, { size: 12 }), run("Serie: ", { size: 12, bold: true }), run(s.serie, { size: 12 })], spacing: { before: 0, after: 20 } }),
          new Paragraph({ children: [run(`Acciones: `, { size: 12, bold: true }), run(`${fmtNum(s.acciones)}   `, { size: 12 }), run("Capital ", { size: 12, bold: true }), run(s.tipo_capital, { size: 12 })], spacing: { before: 0, after: 20 } }),
          new Paragraph({ children: [run(`Ejercicio / Concepto: `, { size: 12, bold: true }), run("___________________________", { size: 12 })], spacing: { before: 0, after: 20 } }),
          new Paragraph({ children: [run(`Importe: $ `, { size: 12, bold: true }), run("___________________________", { size: 12 })], spacing: { before: 0, after: 40 } }),
          // Signature line
          new Paragraph({
            children: [new TextRun({ text: "", font: FONT, size: 4 })],
            border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: "AAAAAA" } },
            spacing: { before: 0, after: 30 },
          }),
          para(run("Firma autorizada", { size: 12, italic: true, color: "666666" }), AlignmentType.LEFT, { before: 0, after: 0 }),
        ],
      }));
    }
    cuponesTbl.push(new TableRow({
      height: { value: 3900, rule: "exact" },
      children: cells,
    }));
  }

  const cuponesHeader = new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [TW],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: [new TableRow({
      children: [new TableCell({
        width: { size: TW, type: WidthType.DXA },
        shading: { fill: NAVY, type: ShadingType.CLEAR },
        borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER },
        margins: { top: 80, bottom: 80, left: 160, right: 160 },
        children: [
          para(run("CUPONES PARA PAGO DE DIVIDENDOS", { bold: true, size: 18, color: "FFFFFF" }), AlignmentType.LEFT, { before: 0, after: 12 }),
          para(run(`${emp.nombre_completo}   ·   R.F.C.: ${emp.rfc}`, { size: 14, color: "FFFFFF" }), AlignmentType.LEFT, { before: 0, after: 0 }),
          para(run(`Título N° ${s.titulo} (${tituloEnLetras(s.titulo)})   ·   ${fmtNum(s.acciones)} Acciones Serie ${s.serie}   ·   ${s.nombre.toUpperCase()}`, { bold: true, size: 14, color: "FFFFFF" }), AlignmentType.LEFT, { before: 0, after: 0 }),
        ],
      })],
    })],
  });

  const cuponesGrid = new Table({
    width: { size: TW, type: WidthType.DXA },
    columnWidths: [CW, CW, CW],
    borders: { top: NO_BORDER, bottom: NO_BORDER, left: NO_BORDER, right: NO_BORDER, insideH: NO_BORDER, insideV: NO_BORDER },
    rows: cuponesTbl,
  });

  return [cuponesHeader, cuponesGrid];
}

// ─────────────────────────────────────────────────────
// HELPERS
// ─────────────────────────────────────────────────────
function fmtMXN(v) { return `$${v.toLocaleString("en-US", { minimumFractionDigits: 2, maximumFractionDigits: 2 })} M.N.`; }
function fmtNum(n) { return n.toLocaleString("en-US"); }

// Convierte CUALQUIER entero >= 0 a letras en español.
// genero: "f" (por defecto, para "acciones") | "m" (para "número de título", "millones").
// Reproduce exactamente las frases del diccionario enPalabras y nunca imprime "undefined".
function numeroALetras(num, genero = "f") {
  num = Math.round(Number(num));
  if (!isFinite(num) || num < 0) return String(num);
  if (num === 0) return "cero";
  const uni = ["", "uno", "dos", "tres", "cuatro", "cinco", "seis", "siete", "ocho", "nueve",
    "diez", "once", "doce", "trece", "catorce", "quince", "dieciséis", "diecisiete", "dieciocho", "diecinueve",
    "veinte", "veintiuno", "veintidós", "veintitrés", "veinticuatro", "veinticinco", "veintiséis", "veintisiete", "veintiocho", "veintinueve"];
  const dec = ["", "", "", "treinta", "cuarenta", "cincuenta", "sesenta", "setenta", "ochenta", "noventa"];
  const cenM = ["", "ciento", "doscientos", "trescientos", "cuatrocientos", "quinientos", "seiscientos", "setecientos", "ochocientos", "novecientos"];
  const cenF = ["", "ciento", "doscientas", "trescientas", "cuatrocientas", "quinientas", "seiscientas", "setecientas", "ochocientas", "novecientas"];
  // g: "f" | "m" (género de centenas/decenas); apoc: true → "uno"→"un", "veintiuno"→"veintiún" (antes de mil/millón)
  function grupo(x, g, apoc) {
    if (x === 0) return "";
    if (x === 100) return "cien";
    const c = Math.floor(x / 100), r = x % 100;
    const cen = (g === "f" ? cenF : cenM)[c];
    let resto = "";
    if (r > 0 && r < 30) {
      resto = uni[r];
      if (r === 1) resto = apoc ? "un" : (g === "f" ? "una" : "uno");
      if (r === 21) resto = apoc ? "veintiún" : (g === "f" ? "veintiuna" : "veintiuno");
    } else if (r >= 30) {
      const d = Math.floor(r / 10), u = r % 10;
      if (u === 0) resto = dec[d];
      else {
        let uu = uni[u];
        if (u === 1) uu = apoc ? "un" : (g === "f" ? "una" : "uno");
        resto = `${dec[d]} y ${uu}`;
      }
    }
    return (cen && resto) ? `${cen} ${resto}` : (cen || resto);
  }
  let out = "";
  const millones = Math.floor(num / 1000000);
  const miles = Math.floor((num % 1000000) / 1000);
  const resto = num % 1000;
  if (millones > 0) out += millones === 1 ? "un millón" : `${grupo(millones, "m", true)} millones`;
  if (miles > 0) out += (out ? " " : "") + (miles === 1 ? "mil" : `${grupo(miles, genero, true)} mil`);
  if (resto > 0) out += (out ? " " : "") + grupo(resto, genero, false);
  return out.trim();
}
// Número de título en letras y MAYÚSCULAS (masculino: "N° 5 (CINCO)"). Nunca "undefined".
function tituloEnLetras(t) { return numeroALetras(parseInt(t, 10) || 0, "m").toUpperCase(); }

const enPalabras = {
  25000: "veinticinco mil",
  15000: "quince mil",
  10000: "diez mil",
  5555: "cinco mil quinientas cincuenta y cinco",
  50000: "cincuenta mil",
  55555: "cincuenta y cinco mil quinientas cincuenta y cinco",
};

const titNums = {
  1: "UNO", 2: "DOS", 3: "TRES", 4: "CUATRO", 5: "CINCO",
};
const numWord = { "0001": "UNO", "0002": "DOS", "0003": "TRES", "0004": "CUATRO" };

// ─────────────────────────────────────────────────────
// DATOS CORPORATIVOS
// ─────────────────────────────────────────────────────
// ─────────────────────────────────────────────────────
// NOMBRE DE CARPETA (sin tipo societario, sin espacios)
// ─────────────────────────────────────────────────────
const NOMBRE_ARCHIVO = "ADMINISTRADORA_GDI";
const BASE_OUT = "\\\\10.1.100.14\\Doc_Legal\\Documentación legal\\Claude (Libros Corporativos y Títulos Accionarios)";

const EMP = {
  nombre_completo: "ADMINISTRADORA GDI, S.A. DE C.V.",
  rfc: "AGD040604P41",
  domicilio: "Naucalpan de Juárez, Estado de México",
  duracion: "99 (noventa y nueve) años",
  fecha_constitucion: "04 de junio de 2004",
  instrumento_corto: "E.P. N° 34,475, Notaría N° 93, México, Distrito Federal (Hoy Ciudad de México), Lic. Pedro Porcayo Vergara, 4 de junio de 2004",
  rpc_corto: "Registro Público de Comercio de Naucalpan de Juárez, Partida 702, Vol. 55, Libro Primero de Comercio, 18 de noviembre de 2004. Folio Mercantil Electrónico: 15254*7",
  capital_total: 55555,
  capital_fijo: 50000,
  capital_variable: 5555,
  total_acciones: 55555,
  valor_nominal: 1.00,
  objeto: "La prestación de servicios especializados, complementarios o compartidos, de asesoría y consultoría a personas físicas y morales, en materia administrativa, legal, contable, fiscal, financiera, de gobierno corporativo, de auditoría, de contraloría, de tesorería, de supervisión, de cobranza, de operación, de recursos humanos, de reclutamiento, de capacitación, de tecnologías de la información e informática, de planeación estratégica, de gestoría, de tramitación y de relaciones gubernamentales.",
  presidente: "SIMÓN GALANTE ZAGA",
  cargo_presidente: "Presidente del Consejo de Administración",
  secretario: "EDUARDO ZAGA COJAB",
  cargo_secretario: "Secretario del Consejo de Administración",
  lugar_emision: "Naucalpan de Juárez, Estado de México",
  // FORMATO OBLIGATORIO letra + número: "<día en letra> (<día núm>) de <mes> de <año en letra> (<año núm>)".
  // Ej.: "cuatro (4) de febrero de dos mil catorce (2014)". Ver corrección 15.
  fecha_emision_doc: "veintiocho (28) de mayo de dos mil veintiséis (2026)",
};

// ⚠️ 'serie_total' = TOTAL de acciones de LA SERIE del título (Serie A = total del capital
//    fijo; Serie B = total del capital variable). El título dice "… de un total de {serie_total}
//    acciones que integran la Serie X". NUNCA usar el gran total de la sociedad (corrección 12).
// ⚠️ 'porcentaje' = participación de ESE título en el capital. La suma de TODOS los títulos debe
//    dar 100% (validación [11]) y, agregada por accionista, reconciliar con el % de la escritura.
//    Si un accionista tiene su participación en varios títulos (Serie A y Serie B, o bloques de
//    distinta naturaleza), su % de escritura se DISTRIBUYE entre esos títulos según sus acciones
//    (ej. GDI vigente: Simón 56.25% = título A 0.26% + título B 55.99%). (Corrección 11.)
const SOCIOS = [
  {
    titulo: "0001", nombre: "SIMÓN GALANTE ZAGA", rfc: "GAZS720203HF1",
    nacionalidad: "Mexicana", domicilio: "Naucalpan de Juárez, Estado de México",
    acciones: 25000, serie: "A", tipo_capital: "Fijo", serie_total: 50000, porcentaje: 45.00,
    numDesde: 1, numHasta: 25000,
    nota: null,
  },
  {
    titulo: "0002", nombre: "ROBERTO GALANTE TOTAH", rfc: "GATR360131D52",
    nacionalidad: "Mexicana", domicilio: "Naucalpan de Juárez, Estado de México",
    acciones: 15000, serie: "A", tipo_capital: "Fijo", serie_total: 50000, porcentaje: 27.00,
    numDesde: 25001, numHasta: 40000,
    nota: "NOTA: El titular conserva únicamente el USUFRUCTO VITALICIO de estas acciones. La NUDA PROPIEDAD fue donada al C. Simón Galante Zaga mediante Asamblea General Ordinaria de fecha 07 de marzo de 2022, protocolizada en la Escritura Pública N° 15,792 de fecha 21 de septiembre de 2022, ante el Lic. José Manuel Gómez del Campo Gurza, Notario N° 149 de Metepec, Estado de México.",
  },
  {
    titulo: "0003", nombre: "ALBERTO GALANTE ZAGA", rfc: "GAZA620123MV4",
    nacionalidad: "Mexicana", domicilio: "Naucalpan de Juárez, Estado de México",
    acciones: 10000, serie: "A", tipo_capital: "Fijo", serie_total: 50000, porcentaje: 18.00,
    numDesde: 40001, numHasta: 50000,
    nota: null,
  },
  {
    titulo: "0004", nombre: "GRUPO INVERMEM, S.A. DE C.V.", rfc: "GIN100121D83",
    nacionalidad: "Mexicana", domicilio: "Naucalpan de Juárez, Estado de México",
    acciones: 5555, serie: "B", tipo_capital: "Variable", serie_total: 5555, porcentaje: 10.00,
    numDesde: 1, numHasta: 5555,
    nota: null,
  },
];

// ⛔ REGLA OBLIGATORIA — campo `nota` de cada socio en SOCIOS:
// SOLO para cargas o limitaciones legales sobre las acciones de ESE título
// (usufructo, nuda propiedad, prenda, embargo, litigio). NUNCA para indicar
// quién representa legalmente a un accionista persona moral, ni para narrar
// su historia corporativa (cambios de razón social, transformaciones, etc.).
// Ese tipo de información va en el Libro de Registro de Accionistas/Socios,
// no en el título accionario. (Incidente corregido: Operaciones y Servicios
// Los Veneros, S.A. de C.V. — se había puesto ahí la representación legal y
// la historia de transformación societaria de un accionista persona moral.)

// ─────────────────────────────────────────────────────
// GENERAR TÍTULO POR ACCIONISTA
// ─────────────────────────────────────────────────────
function buildTitle(s, emp, isFirst) {
  const tituloNum = parseInt(s.titulo);
  const numWord2 = tituloEnLetras(s.titulo);

  // Description paragraph runs
  // ⚠️ "correspondientes al capital social FIJO/VARIABLE" (no "parte fijo/fija" — falla en género).
  // "de un total de N" usa el total de LA SERIE del título (s.serie_total), NUNCA el gran total
  // de la sociedad: no mezclar capital fijo y variable (corrección 12).
  const descRuns = [
    run("Este título definitivo ampara "),
    run(`${fmtNum(s.acciones)} (${enPalabras[s.acciones] || numeroALetras(s.acciones)}) ACCIONES ORDINARIAS NOMINATIVAS DE LA SERIE ${s.serie}`, { bold: true }),
    run(`, correspondientes al capital social `),
    run(s.tipo_capital.toUpperCase(), { bold: true }),
    run(`, con valor nominal de $1.00 M.N. (un peso 00/100, Moneda Nacional) cada una, identificadas con los números del `),
    run(`${fmtNum(s.numDesde)} al ${fmtNum(s.numHasta)}`, { bold: true }),
    run(`, de un total de `),
    run(`${fmtNum(s.serie_total)} (${enPalabras[s.serie_total] || numeroALetras(s.serie_total)})`, { bold: true }),
    run(` acciones que integran la Serie ${s.serie} (capital ${s.tipo_capital.toLowerCase()}) de la Sociedad. `),
    run("Íntegramente pagadas y liberadas.", { bold: true }),
  ];

  let notaPara = null;
  if (s.nota) {
    notaPara = new Paragraph({
      children: [run(s.nota, { size: 13, italic: true, color: "7B3F00" })],
      alignment: AlignmentType.BOTH,
      spacing: { before: 40, after: 0 },
    });
  }

  const elements = [];

  if (!isFirst) {
    elements.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  }

  elements.push(navyHeaderTable([
    { text: emp.nombre_completo, bold: true, size: 20, after: 16 },
    { text: `R.F.C.: ${emp.rfc}`, size: 14, after: 0 },
  ]));
  elements.push(emptyPara(0));
  elements.push(titleRow("TÍTULO ACCIONARIO DEFINITIVO", `N° ${tituloNum} (${numWord2})`));
  elements.push(descriptionBox(descRuns, notaPara));
  elements.push(mainSectionsTable(s, emp));
  elements.push(objetoSocialTable(emp.objeto));
  elements.push(summaryTable(s, emp, s.numDesde, s.numHasta));
  elements.push(derechosObligTable());
  elements.push(seHaceConstarTable(emp.lugar_emision, emp.fecha_emision_doc));
  elements.push(emptyPara(300));
  elements.push(signatureTable(emp));
  elements.push(emptyPara(0));

  // Cupones page
  elements.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  const [hdr, grid] = cuponesPage(s, emp);
  elements.push(hdr);
  elements.push(grid);

  return elements;
}

// ─────────────────────────────────────────────────────
// GENERAR DOCUMENTO COMPLETO
// ─────────────────────────────────────────────────────
async function main() {
  const allElements = [];
  SOCIOS.forEach((s, i) => {
    buildTitle(s, EMP, i === 0).forEach(el => allElements.push(el));
  });

  const doc = new Document({
    sections: [{
      properties: {
        page: {
          size: { width: 12240, height: 15840 },
          margin: { top: 1008, bottom: 1008, left: 1008, right: 1008 },
        },
      },
      children: allElements,
    }],
  });

  const buf = await Packer.toBuffer(doc);

  // ── Crear estructura de carpetas y guardar ──────────────────
  const carpetaEmpresa  = path.join(BASE_OUT, NOMBRE_ARCHIVO);
  const carpetaTitulos  = path.join(carpetaEmpresa, "Titulos_Accionarios");
  fs.mkdirSync(carpetaTitulos, { recursive: true });

  const outPath = path.join(carpetaTitulos, `Titulos_Accionarios_${NOMBRE_ARCHIVO}.docx`);
  fs.writeFileSync(outPath, buf);
  console.log(`✅ ${outPath}`);
}

main().catch(e => { console.error(e); process.exit(1); });

```

---

### TEMPLATE B — LIBROS CORPORATIVOS (gen_libros.js)

Genera dos documentos en dos subcarpetas. Cambiar: `NOMBRE_ARCHIVO`, `BASE_OUT`, constantes `DOM_*`, arrays `SOCIOS_*`, firmantes, y las funciones `buildLibroVariaciones` / `buildLibroRegistro`.

**Regla inamovible:** `capitalBreakdown()` → SOLO en `buildLibroVariaciones`, NUNCA en `buildLibroRegistro`.

**Reglas de corrección obligatorias (incidentes previos — NO repetir):**

1. **Nombre del libro:** el libro que registra a los titulares del capital se llama
   **"Libro de Registro de Accionistas"** para sociedades cuyo capital se representa por
   acciones (S.A., S.A. de C.V., S.A.P.I., S.A.S., S. en C. por A.), o **"Libro de Registro
   de Socios"** para sociedades de partes sociales/participaciones (S. de R.L., S.C., S. en
   N.C., S. en C.S.) o **"Libro de Registro de Asociados"** (A.C.). NUNCA llamarlo "Libro de
   Registro de Acciones" — ese nombre es incorrecto. El nombre de carpeta/archivo debe
   reflejar el término correcto: `Libro_Registro_Accionistas_[NOMBRE]` o
   `Libro_Registro_Socios_[NOMBRE]`.

2. **domBlock() — solo domicilio, nunca representante:** el bloque de cierre por cada
   accionista/socio (`domBlock`) debe contener ÚNICAMENTE el nombre y el domicilio de la
   persona física o moral titular. NUNCA agregar quién la representa (no usar un
   `repBlock` ni frases como "representada por..." en ese bloque). Si es relevante mencionar
   al representante, hacerlo solo dentro del párrafo narrativo del asiento, nunca en el
   bloque de domicilio.

3. **Fecha de firma del asiento ≠ fecha del instrumento — y el PUNTO DE PARTIDA correcto
   depende de si el documento quedó inscrito en el Registro Público de Comercio (RPC):**
   la fecha que se usa en `firmasBlock(lugar, fecha, ...)` NUNCA debe ser la misma fecha
   del acta/escritura que motiva el asiento. Se calcula sumando entre 20 y 25 días
   naturales, PERO el punto de partida de esa suma es:
   - **Si el documento SÍ quedó inscrito en el RPC** (verifica siempre si hay folio/fecha
     de inscripción en el instrumento o en un oficio posterior): usar la **fecha de
     INSCRIPCIÓN**, no la fecha del instrumento/acta. Ejemplo real: Póliza Mercantil de
     fecha 1 de junio de 2007, inscrita en el RPC el 30 de julio de 2007 → la fecha de
     firma del asiento se cuenta desde el 30 de julio (→ 24 de agosto de 2007), NO desde
     el 1 de junio.
   - **Si NO quedó inscrito** (no hay constancia de inscripción, o el tipo de acto no
     requiere inscripción — p. ej. una Asamblea Ordinaria que solo aprueba informes y
     cambios de comisario): usar la **fecha del documento/acto que originó el cambio**
     (fecha del acta o de la escritura). Ejemplo: acta de asamblea de fecha 29 de mayo,
     sin inscripción aplicable → firma del asiento con fecha 22 de junio (24 días
     después).
   Esta regla aplica a AMBOS libros (Variaciones de Capital y Registro de Socios/
   Accionistas). Calcular la fecha con aritmética real de calendario (no aproximar a "un
   mes después"), y SIEMPRE revisar primero si el instrumento indica folio y fecha de
   inscripción en el RPC antes de decidir el punto de partida.

4. **"Distrito Federal" lleva la glosa "(Hoy Ciudad de México)" ÚNICAMENTE cuando el
   HECHO o documento que el texto describe es de fecha POSTERIOR al 29-ene-2016 (DOF).
   Si el hecho es ANTERIOR a esa fecha (p. ej. una sociedad constituida en 2007), el texto
   debe decir "Distrito Federal" a secas, SIN glosa, en TODAS sus apariciones dentro de
   ese asiento — no solo en algunas.**
   Aclaración importante confirmada por el usuario: la fecha relevante es la del HECHO
   histórico que el texto narra (p. ej. la fecha de constitución, o la fecha del domicilio
   que se está describiendo), NO la fecha de firma del asiento (que ya lleva el ajuste de
   +20/25 días de la regla 3) ni la fecha en que se genera el documento. Ejemplo: en el
   expediente de Operaciones y Servicios Los Veneros, S.A. de C.V. (constituida el 1 de
   junio de 2007), TODAS las menciones de "Distrito Federal" relacionadas con hechos de
   2007 o 2010 deben quedar sin glosa, incluyendo el campo `lugar` del `firmasBlock`
   ("MÉXICO, DISTRITO FEDERAL", sin "(Hoy...)") y los domicilios de esa época.
   Implementación:
   - Define, por cada asiento, una fecha de HECHO (`HECHO_ASIENTO_N`) = la fecha real del
     acontecimiento que ese asiento describe (constitución, reforma, asamblea, etc.) —
     distinta de la fecha de firma ajustada.
   - `fixDF(texto, fechaHecho)` glosa solo si `fechaHecho > 29-ene-2016`.
   - Cubre tanto el texto en formato oración ("Distrito Federal") como en MAYÚSCULAS
     ("DISTRITO FEDERAL"), replicando la glosa con el mismo estilo de mayúsculas/minúsculas.
   - Aplica la función a TODA cadena que pueda contener "Distrito Federal" antes de
     insertarla en el documento: narrativa, domicilios (`domBlock`) y el campo `lugar` de
     `firmasBlock` — no solo a la narrativa.
   - No vuelve a glosar si el texto ya contiene "(Hoy" inmediatamente después.

   > ⚠️ Este punto se corrigió dos veces en el expediente de Los Veneros: primero se
   > implementó sin condición (glosa siempre), y resultó incorrecto — la regla correcta,
   > confirmada por el usuario, es la condicional aquí descrita, basada en la fecha del
   > HECHO, no en la fecha de firma ni en la fecha de generación del documento.

5. **Validar siempre el domicilio SOCIAL vigente de la Sociedad (no el de los
   accionistas) antes de escribir el campo `lugar` de cada `firmasBlock`:** el `lugar` de
   cada asiento debe corresponder al domicilio social de la Sociedad EN LA FECHA de ese
   asiento — es decir, hay que reconstruir la línea de tiempo de domicilios sociales
   (Cláusula Cuarta de los Estatutos y sus reformas) y usar el que estaba vigente en cada
   momento, no el lugar donde físicamente se firmó el instrumento ni el domicilio de algún
   accionista. Si el domicilio social cambió en una reforma anterior y no ha cambiado de
   nuevo, TODOS los asientos posteriores a esa reforma deben usar el domicilio nuevo,
   incluso si la Asamblea correspondiente se celebró en otro lugar. Revisar esto contra el
   historial completo del expediente antes de generar, no solo contra el asiento
   inmediatamente anterior.



```javascript
"use strict";
const { 
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  AlignmentType, WidthType, BorderStyle, ShadingType, VerticalAlign,
  PageBreak, HeadingLevel
} = require("docx");
const fs = require("fs");
const path = require("path");

// ─────────────────────────────────────────────────────────────────────
// CONSTANTES DE FORMATO — ASIENTO (Legal 8.5×14", Times New Roman 11pt)
// ─────────────────────────────────────────────────────────────────────
const FONT  = "Times New Roman";
const SZ_BODY  = 22;   // 11pt (half-points)
const SZ_TITLE = 28;   // 14pt
const SZ_TABLE = 20;   // 10pt en tabla
const BLK      = "B4C6E7";
const BLACK_BRD = { style: BorderStyle.SINGLE, size: 4, color: "000000" };

const PAGE   = { width: 12240, height: 20160 };          // legal
const MARGIN = { top: 993, bottom: 993, left: 1701, right: 1701 };
const TBL_W  = 7202;

// ─────────────────────────────────────────────────────────────────────
// ⚠️  ALERTA LEGAL — REGLA DOF 2016-01-29
// Docs ANTES de 2016-01-29 → "Delegación X", "Distrito Federal"
// Docs DESDE  2016-01-29  → "Alcaldía X",   "Ciudad de México"
// ─────────────────────────────────────────────────────────────────────
const DOM_PENDING    = "PENDIENTE DE CONSTANCIA DE SITUACIÓN FISCAL";

// Pre-2016: Distrito Federal / Delegación
const DOM_SIMON_DF   = "Av. Santa Fe número 481, Piso 7, Colonia Cruz Manca, Delegación Cuajimalpa de Morelos, Distrito Federal, C.P. 05349";
const DOM_ROBERTO_DF = "Av. Santa Fe número 481, Piso 7, Colonia Cruz Manca, Delegación Cuajimalpa de Morelos, Distrito Federal, C.P. 05349";
const DOM_INVERMEM_DF= "Av. Paseo de las Palmas 1755, Colonia Lomas de Chapultepec VIII Sección, Delegación Miguel Hidalgo, Distrito Federal, C.P. 11000";

// Post-2016: Ciudad de México / Alcaldía
const DOM_SIMON   = "Av. Santa Fe número 481, Piso 7, Colonia Cruz Manca, Alcaldía Cuajimalpa de Morelos, Ciudad de México, C.P. 05349";
const DOM_ROBERTO = "Av. Santa Fe número 481, Piso 7, Colonia Cruz Manca, Alcaldía Cuajimalpa de Morelos, Ciudad de México, C.P. 05349";
const DOM_ALBERTO = DOM_PENDING;
const DOM_INVERMEM= "Av. Paseo de las Palmas 1755, Colonia Lomas de Chapultepec VIII Sección, Alcaldía Miguel Hidalgo, Ciudad de México, C.P. 11000";

// ─────────────────────────────────────────────────────────────────────
// HELPERS
// ─────────────────────────────────────────────────────────────────────
function fmtMXN(v) {
  return `$ ${v.toLocaleString("en-US", { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`;
}
function fmtNum(n) { return n.toLocaleString("en-US"); }

function r(text, opts = {}) {
  return new TextRun({
    text,
    font: FONT,
    size:    opts.size    || SZ_BODY,
    bold:    opts.bold    || false,
    italics: opts.italic  || false,
    color:   opts.color   || "000000",
  });
}

// Párrafo de cuerpo — doble espacio (line 360)
function bodyPara(runs, align = AlignmentType.BOTH, spaceBefore = 0, spaceAfter = 0) {
  return new Paragraph({
    children: Array.isArray(runs) ? runs : [runs],
    alignment: align,
    spacing: { before: spaceBefore, after: spaceAfter, line: 360, lineRule: "auto" },
  });
}

// Párrafo en blanco compacto (para separaciones mínimas)
function blankPara() {
  return new Paragraph({
    children: [new TextRun({ text: "", font: FONT, size: SZ_BODY })],
    spacing: { before: 0, after: 0, line: 240, lineRule: "auto" },
  });
}

function tituloAsiento(num) {
  return new Paragraph({
    children: [r(`ASIENTO ${num}`, { size: SZ_TITLE, bold: true })],
    alignment: AlignmentType.CENTER,
    spacing: { before: 0, after: 200, line: 240 },
  });
}

// Convierte "\n" en verdaderos saltos de línea dentro de una celda
// (docx NO rompe línea con "\n" suelto → hay que emitir un <w:br/> por cada salto).
// Sirve para poner el RFC debajo del nombre en los cuadros accionarios.
function mlRuns(text, opts = {}) {
  return String(text).split("\n").map((linea, i) => {
    const o = {
      text: linea, font: FONT,
      size: opts.size || SZ_TABLE, bold: opts.bold || false,
      italics: opts.italic || false, color: opts.color || "000000",
    };
    if (i > 0) o.break = 1;   // salto de línea ANTES de esta línea
    return new TextRun(o);
  });
}

function hdrCell(text, width) {
  return new TableCell({
    width: { size: width, type: WidthType.DXA },
    shading: { fill: BLK, type: ShadingType.CLEAR },
    borders: { top: BLACK_BRD, bottom: BLACK_BRD, left: BLACK_BRD, right: BLACK_BRD },
    verticalAlign: VerticalAlign.CENTER,
    margins: { top: 60, bottom: 60, left: 108, right: 108 },
    children: [new Paragraph({
      children: mlRuns(text, { size: SZ_TABLE, bold: true }),
      alignment: AlignmentType.CENTER,
      spacing: { before: 0, after: 0 },
    })],
  });
}

function dataCell(text, width, align = AlignmentType.CENTER, bold = false) {
  return new TableCell({
    width: { size: width, type: WidthType.DXA },
    borders: { top: BLACK_BRD, bottom: BLACK_BRD, left: BLACK_BRD, right: BLACK_BRD },
    verticalAlign: VerticalAlign.CENTER,
    margins: { top: 40, bottom: 40, left: 108, right: 108 },
    children: [new Paragraph({
      children: mlRuns(text, { size: SZ_TABLE, bold }),
      alignment: align,
      spacing: { before: 0, after: 0 },
    })],
  });
}

// ── Tabla accionistas 2 series (Serie A Fijo + Serie B Variable)
function tablaAccionistas2S(socios) {
  // FORMAT_LOCK_V17: se agrega columna "CAPITAL TOTAL" (Serie A + Serie B por
  // accionista) — el usuario pidió ver el capital total por renglón, no solo
  // el desglose por serie. Ancho total = TBL_W (7202 DXA), 5 columnas.
  const C = [2900, 1150, 1050, 1050, 1052];
  const totalDe = s => (s.val_serieI || 0) + (s.val_serieII || 0);
  return new Table({
    width: { size: TBL_W, type: WidthType.DXA },
    columnWidths: C,
    alignment: AlignmentType.CENTER,
    rows: [
      new TableRow({
        tableHeader: true,
        height: { value: 1000, rule: "atLeast" },
        children: [
          hdrCell("ACCIONISTAS", C[0]),
          hdrCell("NÚMERO DE\nACCIONES", C[1]),
          hdrCell("VALOR SERIE A\n(CAPITAL FIJO)", C[2]),
          hdrCell("VALOR SERIE B\n(CAPITAL VARIABLE)", C[3]),
          hdrCell("CAPITAL\nTOTAL", C[4]),
        ],
      }),
      ...socios.map(s => new TableRow({
        children: [
          dataCell(`${s.nombre}\nRFC: ${s.rfc}`, C[0], AlignmentType.LEFT, true),
          dataCell(s.acc_total != null ? fmtNum(s.acc_total) : "-", C[1]),
          dataCell(s.val_serieI != null && s.val_serieI > 0 ? fmtMXN(s.val_serieI) : "-", C[2]),
          dataCell(s.val_serieII != null && s.val_serieII > 0 ? fmtMXN(s.val_serieII) : "-", C[3]),
          dataCell(fmtMXN(totalDe(s)), C[4], AlignmentType.CENTER, true),
        ],
      })),
      new TableRow({
        children: [
          dataCell("TOTAL", C[0], AlignmentType.CENTER, true),
          dataCell(fmtNum(socios.reduce((s, x) => s + (x.acc_total || 0), 0)), C[1], AlignmentType.CENTER, true),
          dataCell(fmtMXN(socios.reduce((s, x) => s + (x.val_serieI || 0), 0)), C[2], AlignmentType.CENTER, true),
          dataCell(fmtMXN(socios.reduce((s, x) => s + (x.val_serieII || 0), 0)), C[3], AlignmentType.CENTER, true),
          dataCell(fmtMXN(socios.reduce((s, x) => s + totalDe(x), 0)), C[4], AlignmentType.CENTER, true),
        ],
      }),
    ],
  });
}

// ── Tabla accionistas 1 serie (solo Serie I fijo)
function tablaAccionistas1S(socios) {
  const C = [3570, 1649, 1983];
  return new Table({
    width: { size: TBL_W, type: WidthType.DXA },
    columnWidths: C,
    alignment: AlignmentType.CENTER,
    rows: [
      new TableRow({
        tableHeader: true,
        height: { value: 800, rule: "atLeast" },
        children: [
          hdrCell("ACCIONISTAS", C[0]),
          hdrCell("NÚMERO DE\nACCIONES", C[1]),
          hdrCell("VALOR TOTAL DE LAS\nACCIONES SERIE I", C[2]),
        ],
      }),
      ...socios.map(s => new TableRow({
        children: [
          dataCell(`${s.nombre}\nRFC: ${s.rfc}`, C[0], AlignmentType.LEFT, true),
          dataCell(s.acc_total != null ? fmtNum(s.acc_total) : "-", C[1]),
          dataCell(s.val_serieI != null ? fmtMXN(s.val_serieI) : "-", C[2]),
        ],
      })),
      new TableRow({
        children: [
          dataCell("TOTAL", C[0], AlignmentType.CENTER, true),
          dataCell(fmtNum(socios.reduce((s, x) => s + (x.acc_total || 0), 0)), C[1], AlignmentType.CENTER, true),
          dataCell(fmtMXN(socios.reduce((s, x) => s + (x.val_serieI || 0), 0)), C[2], AlignmentType.CENTER, true),
        ],
      }),
    ],
  });
}

// ── Bloque de domicilio: NOMBRE en negrita + texto domicilio
function domBlock(nombre, domicilio) {
  return bodyPara([
    r(nombre + " ", { bold: true }),
    r(`de nacionalidad mexicana, ha señalado como su domicilio para efectos de este asiento el ubicado en ${domicilio}.`),
  ], AlignmentType.BOTH, 0, 60);   // spaceAfter=60 → separación mínima entre bloques
}

// ── Bloque de nota: NOTA en negrita + texto
function notaBlock(texto) {
  return bodyPara([
    r("NOTA: ", { bold: true }),
    r(texto),
  ], AlignmentType.BOTH, 100, 0);
}

// ── Bloque de ALERTA: dato o documento faltante para completar el asiento (regla 10)
// Insertar inmediatamente después del domBlock() de la persona afectada, aun si ya
// causó baja como accionista/socio.
function alertaBlock(texto) {
  return bodyPara([
    r("⚠️ ALERTA: ", { bold: true, color: "B00020" }),
    r(texto, { color: "B00020" }),
  ], AlignmentType.BOTH, 100, 60);
}

// ── Firmas: lugar + fecha + 2 firmas — SIN blankParas internos para evitar overflow
function firmasBlock(lugar, fecha, presidente, secretario) {
  const half = 4419;
  function sigCell(persona) {
    return new TableCell({
      width: { size: half, type: WidthType.DXA },
      borders: {
        top:     { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        bottom:  { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        left:    { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        right:   { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
      },
      margins: { top: 0, bottom: 0, left: 300, right: 300 },
      children: [
        // espacio para firma manuscrita
        new Paragraph({ children: [new TextRun({ text: "", font: FONT, size: SZ_BODY })], spacing: { before: 0, after: 360 } }),
        // línea de firma
        new Paragraph({
          children: [new TextRun({ text: "", font: FONT, size: SZ_BODY })],
          border: { bottom: { style: BorderStyle.SINGLE, size: 6, color: "000000" } },
          spacing: { before: 0, after: 100 },
        }),
        new Paragraph({ children: [r(persona.nombre, { bold: true })], alignment: AlignmentType.CENTER, spacing: { before: 0, after: 0 } }),
        new Paragraph({ children: [r(persona.cargo, { bold: true })], alignment: AlignmentType.CENTER, spacing: { before: 0, after: 0 } }),
        new Paragraph({ children: [r("ADMINISTRACIÓN", { bold: true })], alignment: AlignmentType.CENTER, spacing: { before: 0, after: 0 } }),
      ],
    });
  }
  return [
    new Paragraph({
      children: [r(`${lugar} A ${fecha}`, { bold: true })],
      alignment: AlignmentType.CENTER,
      spacing: { before: 200, after: 0, line: 240 },
    }),
    new Table({
      width: { size: half * 2, type: WidthType.DXA },
      columnWidths: [half, half],
      borders: {
        top:     { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        bottom:  { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        left:    { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        right:   { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        insideH: { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
        insideV: { style: BorderStyle.NONE, size: 0, color: "FFFFFF" },
      },
      rows: [new TableRow({ children: [sigCell(presidente), sigCell(secretario)] })],
    }),
  ];
}


// ── Desglose de capital (a/b/c) — formato "En tal virtud..."
function capitalBreakdown(fijo, variable, total) {
  const items = [];
  items.push(bodyPara([
    r("En tal virtud, el capital social quedó integrado a esa fecha de la siguiente manera:"),
  ], AlignmentType.BOTH, 100, 0));
  items.push(bodyPara([
    r("a)\t", { bold: true }),
    r(fijo),
  ], AlignmentType.BOTH, 0, 0));
  if (variable) {
    items.push(bodyPara([
      r("b)\t", { bold: true }),
      r(variable),
    ], AlignmentType.BOTH, 0, 0));
    items.push(bodyPara([
      r("c)\t", { bold: true }),
      r(total),
    ], AlignmentType.BOTH, 0, 60));
  } else {
    items.push(bodyPara([
      r("b)\t", { bold: true }),
      r(total),
    ], AlignmentType.BOTH, 0, 60));
  }
  return items;
}

// ─────────────────────────────────────────────────────────────────────
// DATOS — ADMINISTRADORA GDI, S.A. DE C.V.
// ─────────────────────────────────────────────────────────────────────
// ─────────────────────────────────────────────────────
// NOMBRE DE CARPETA (sin tipo societario, sin espacios)
// ─────────────────────────────────────────────────────
const NOMBRE_ARCHIVO = "ADMINISTRADORA_GDI";
const BASE_OUT = "\\\\10.1.100.14\\Doc_Legal\\Documentación legal\\Claude (Libros Corporativos y Títulos Accionarios)";

const EMP = { nombre: "ADMINISTRADORA GDI, S.A. DE C.V.", rfc: "AGD040604P41" };

// ASIENTO 1 — Constitución 04/06/2004 (50 acc originales × $500 c/u = $50,000)
// Distribución: Simón 50%, Roberto 30%, Alberto 20%
const SOCIOS_2004 = [
  { nombre: "SIMÓN GALANTE ZAGA",     rfc: "GAZS720203HF1", acc_total: 50, val_serieI: 25000 },
  { nombre: "ROBERTO GALANTE TOTAH",  rfc: "GATR360131D52", acc_total: 30, val_serieI: 15000 },
  { nombre: "ALBERTO GALANTE ZAGA",   rfc: "GAZA620123MV4", acc_total: 20, val_serieI: 10000 },
];

// ASIENTO 2 — Reforma estatutaria 06/09/2010 / protocolización 31/08/2012
// Cambio valor nominal $500→$1 (50 acc → 50,000 acc Serie I)
// + Aumento capital variable $5,555 (5,555 acc Serie II) — entra GRUPO INVERMEM
const SOCIOS_2010 = [
  { nombre: "SIMÓN GALANTE ZAGA",            rfc: "GAZS720203HF1", acc_total: 25000, val_serieI: 25000, val_serieII: 0 },
  { nombre: "ROBERTO GALANTE TOTAH",         rfc: "GATR360131D52", acc_total: 15000, val_serieI: 15000, val_serieII: 0 },
  { nombre: "ALBERTO GALANTE ZAGA",          rfc: "GAZA620123MV4", acc_total: 10000, val_serieI: 10000, val_serieII: 0 },
  { nombre: "GRUPO INVERMEM, S.A. DE C.V.",  rfc: "GIN100121D83",  acc_total: 5555,  val_serieI: 0,     val_serieII: 5555 },
];

// ASIENTO 3 — Modificación objeto social 01/07/2021 (sin cambio de capital)
// ASIENTO 4 — Donación nuda propiedad 07/03/2022 (sin cambio de montos totales)
const SOCIOS_2022 = [
  { nombre: "SIMÓN GALANTE ZAGA",            rfc: "GAZS720203HF1", acc_total: 25000, val_serieI: 25000, val_serieII: 0 },
  { nombre: "ROBERTO GALANTE TOTAH",         rfc: "GATR360131D52", acc_total: 15000, val_serieI: 15000, val_serieII: 0 },
  { nombre: "ALBERTO GALANTE ZAGA",          rfc: "GAZA620123MV4", acc_total: 10000, val_serieI: 10000, val_serieII: 0 },
  { nombre: "GRUPO INVERMEM, S.A. DE C.V.",  rfc: "GIN100121D83",  acc_total: 5555,  val_serieI: 0,     val_serieII: 5555 },
];

const PDTE_2004 = { nombre: "SIMÓN GALANTE ZAGA",   cargo: "PRESIDENTE DEL CONSEJO DE" };
const SRIO_2004 = { nombre: "ROBERTO GALANTE TOTAH", cargo: "SECRETARIO DEL CONSEJO DE" };
const PDTE_2022 = { nombre: "SIMÓN GALANTE ZAGA",   cargo: "PRESIDENTE DEL CONSEJO DE" };
const SRIO_2022 = { nombre: "EDUARDO ZAGA COJAB",   cargo: "SECRETARIO DEL CONSEJO DE" };

// ─────────────────────────────────────────────────────────────────────
// LIBRO DE VARIACIONES DE CAPITAL — 4 ASIENTOS
// ─────────────────────────────────────────────────────────────────────
function buildLibroVariaciones() {
  const ch = [];

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 1 — Constitución 04/06/2004
  // ══════════════════════════════════════════════════════════════
  ch.push(tituloAsiento(1));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante "),
    r("Escritura Pública Número 34,475", { bold: true }),
    r(", de fecha "),
    r("04 de junio de 2004", { bold: true }),
    r(", ante la fe del Licenciado Pedro Porcayo Vergara, Notario Público Número 93 de México, Distrito Federal (Hoy Ciudad de México), inscrita en el Registro Público de Comercio de Naucalpan de Juárez, Estado de México, cuyo primer testimonio quedó debidamente inscrito bajo la Partida 702, Volumen 55, Libro Primero de Comercio, el 18 de noviembre de 2004, Folio Mercantil Electrónico 15254*7, se constituyó "),
    r("ADMINISTRADORA GDI, S.A. DE C.V.", { bold: true }),
    r(", con un capital social de "),
    r("$50,000.00 (CINCUENTA MIL PESOS 00/100 MONEDA NACIONAL)", { bold: true }),
    r(", representado por 100 (cien) acciones nominativas de la Serie I, correspondientes al capital fijo, con un valor nominal de "),
    r("$500.00 (QUINIENTOS PESOS 00/100 MONEDA NACIONAL)", { bold: true }),
    r(" cada una, las cuales quedaron debidamente suscritas y pagadas por las personas que a continuación se indican:"),
  ]));
  ch.push(blankPara());
  capitalBreakdown(
    "Capital Social Fijo: $50,000.00 (CINCUENTA MIL PESOS 00/100 MONEDA NACIONAL) representado por 100 (cien) acciones de la Serie I, nominativas, con valor nominal de $500.00 (QUINIENTOS PESOS 00/100 MONEDA NACIONAL) cada una, totalmente suscritas y pagadas.",
    null,
    "Capital Social Total: $50,000.00 (CINCUENTA MIL PESOS 00/100 MONEDA NACIONAL) representado por 100 (cien) acciones."
  ).forEach(e => ch.push(e));
  firmasBlock("DISTRITO FEDERAL", "04 DE JUNIO DE 2004", PDTE_2004, SRIO_2004).forEach(e => ch.push(e));

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 2 — Reforma + Aumento capital variable  (protoc. 31/08/2012)
  // ══════════════════════════════════════════════════════════════
  ch.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  ch.push(tituloAsiento(2));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante Acta de Asamblea General Extraordinaria de Accionistas de fecha "),
    r("06 de septiembre de 2010", { bold: true }),
    r(", la cual se encuentra debidamente protocolizada en la "),
    r("Escritura Pública Número 25,135", { bold: true }),
    r(", de fecha 31 de agosto de 2012, pasada ante la fe del Licenciado Alfredo Caso Velázquez, Notario Público Número 17 del Estado de México, inscrita en el Registro Público de Comercio bajo el Folio Mercantil Electrónico 15254*7 con fecha 27 de febrero de 2013, la Asamblea resolvió lo siguiente: "),
    r("i)", { bold: true }),
    r(" Reforma de los Artículos Sexto, Séptimo y Octavo de los Estatutos Sociales; "),
    r("ii)", { bold: true }),
    r(" Cambio del valor nominal de las acciones de "),
    r("$500.00 (QUINIENTOS PESOS)", { bold: true }),
    r(" a "),
    r("$1.00 (UN PESO 00/100 MONEDA NACIONAL)", { bold: true }),
    r(" por acción, con lo cual el capital fijo quedó representado por 50,000 (cincuenta mil) acciones nominativas de la Serie I; "),
    r("iii)", { bold: true }),
    r(" Aumento de capital social en su parte variable por la suma de "),
    r("$5,555.00 (CINCO MIL QUINIENTOS CINCUENTA Y CINCO PESOS 00/100 MONEDA NACIONAL)", { bold: true }),
    r(", correspondiente a la Serie II, mediante la emisión de 5,555 (cinco mil quinientas cincuenta y cinco) acciones con valor nominal de "),
    r("$1.00 (UN PESO 00/100 MONEDA NACIONAL)", { bold: true }),
    r(" cada una, admitiéndose como nuevo accionista a "),
    r("GRUPO INVERMEM, S.A. DE C.V.", { bold: true }),
    r(" En consecuencia, el capital social de la Sociedad quedó de la siguiente forma:"),
  ]));
  ch.push(blankPara());
  capitalBreakdown(
    "Capital Social Fijo: $50,000.00 (CINCUENTA MIL PESOS 00/100 MONEDA NACIONAL) representado por 50,000 (cincuenta mil) acciones de la Serie I, nominativas, con valor nominal de $1.00 (UN PESO 00/100 MONEDA NACIONAL) cada una.",
    "Capital Social Variable: $5,555.00 (CINCO MIL QUINIENTOS CINCUENTA Y CINCO PESOS 00/100 MONEDA NACIONAL) representado por 5,555 (cinco mil quinientas cincuenta y cinco) acciones de la Serie II, nominativas, con valor nominal de $1.00 (UN PESO 00/100 MONEDA NACIONAL) cada una, totalmente suscritas y pagadas por GRUPO INVERMEM, S.A. DE C.V.",
    "Capital Social Total: $55,555.00 (CINCUENTA Y CINCO MIL QUINIENTOS CINCUENTA Y CINCO PESOS 00/100 MONEDA NACIONAL) representado por 55,555 (cincuenta y cinco mil quinientas cincuenta y cinco) acciones."
  ).forEach(e => ch.push(e));
  firmasBlock("NAUCALPAN DE JUÁREZ, ESTADO DE MÉXICO", "31 DE AGOSTO DE 2012", PDTE_2004, SRIO_2004).forEach(e => ch.push(e));

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 3 — Modificación objeto social 01/07/2021
  // ══════════════════════════════════════════════════════════════
  ch.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  ch.push(tituloAsiento(3));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante Acta de Asamblea General Extraordinaria de Accionistas de fecha "),
    r("01 de julio de 2021", { bold: true }),
    r(", la cual se encuentra debidamente protocolizada en la "),
    r("Escritura Pública Número 64,769", { bold: true }),
    r(", de fecha 03 de agosto de 2021, pasada ante la fe del Licenciado José Daniel Labardini Schettino, Notario Público Número 86 de la Ciudad de México, inscrita en el Registro Público de Comercio bajo el Folio Mercantil Electrónico 15254 con fecha 06 de octubre de 2021, la Asamblea resolvió lo siguiente: "),
    r("i)", { bold: true }),
    r(" Reforma de la Cláusula Tercera (Objeto Social) de los Estatutos Sociales, para adecuarla a la Ley Federal del Trabajo y a la normatividad aplicable a la prestación de servicios especializados, complementarios o compartidos (LPAFL); "),
    r("ii)", { bold: true }),
    r(" Adición de la Cláusula Tercera Bis a los Estatutos Sociales; "),
    r("iii)", { bold: true }),
    r(" Otorgamiento de nuevos poderes a diversas personas. El capital social de la Sociedad no sufrió modificación alguna, quedando de la siguiente forma:"),
  ]));
  ch.push(blankPara());
  capitalBreakdown(
    "Capital Social Fijo: $50,000.00 (CINCUENTA MIL PESOS 00/100 MONEDA NACIONAL) representado por 50,000 (cincuenta mil) acciones de la Serie I, nominativas, con valor nominal de $1.00 (UN PESO 00/100 MONEDA NACIONAL) cada una.",
    "Capital Social Variable: $5,555.00 (CINCO MIL QUINIENTOS CINCUENTA Y CINCO PESOS 00/100 MONEDA NACIONAL) representado por 5,555 (cinco mil quinientas cincuenta y cinco) acciones de la Serie II, nominativas, con valor nominal de $1.00 (UN PESO 00/100 MONEDA NACIONAL) cada una.",
    "Capital Social Total: $55,555.00 (CINCUENTA Y CINCO MIL QUINIENTOS CINCUENTA Y CINCO PESOS 00/100 MONEDA NACIONAL) representado por 55,555 (cincuenta y cinco mil quinientas cincuenta y cinco) acciones. El capital social no sufrió modificación en el presente asiento."
  ).forEach(e => ch.push(e));
  firmasBlock("CIUDAD DE MÉXICO", "03 DE AGOSTO DE 2021", PDTE_2004, SRIO_2004).forEach(e => ch.push(e));

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 4 — Donación nuda propiedad + Nuevo Consejo 07/03/2022
  // ══════════════════════════════════════════════════════════════
  ch.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  ch.push(tituloAsiento(4));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante Acta de Asamblea General Ordinaria de Accionistas de fecha "),
    r("07 de marzo de 2022", { bold: true }),
    r(", la cual se encuentra debidamente protocolizada en la "),
    r("Escritura Pública Número 15,792", { bold: true }),
    r(", de fecha 21 de septiembre de 2022, pasada ante la fe del Licenciado José Manuel Gómez del Campo Gurza, Notario Público Número 149 del Estado de México, la Asamblea resolvió lo siguiente: "),
    r("i)", { bold: true }),
    r(" El C. "),
    r("Roberto Galante Totah", { bold: true }),
    r(" dona la nuda propiedad de "),
    r("15,000 (quince mil) acciones nominativas de la Serie I", { bold: true }),
    r(", con valor nominal de $1.00 (un peso 00/100 M.N.) cada una, a favor del C. "),
    r("Simón Galante Zaga", { bold: true }),
    r(", conservando el donante el usufructo vitalicio de las mismas; "),
    r("ii)", { bold: true }),
    r(" Renovación del Consejo de Administración: Presidente: "),
    r("Simón Galante Zaga", { bold: true }),
    r("; Secretario: "),
    r("Eduardo Zaga Cojab", { bold: true }),
    r("; "),
    r("iii)", { bold: true }),
    r(" Nombramiento del C. "),
    r("Hilarión Arturo Pérez Nava", { bold: true }),
    r(" como Comisario de la Sociedad. El capital social de la Sociedad no sufrió modificación en su monto total, quedando de la siguiente forma:"),
  ]));
  ch.push(blankPara());
  capitalBreakdown(
    "Capital Social Fijo: $50,000.00 (CINCUENTA MIL PESOS 00/100 MONEDA NACIONAL) representado por 50,000 (cincuenta mil) acciones de la Serie I, nominativas, con valor nominal de $1.00 (UN PESO 00/100 MONEDA NACIONAL) cada una.",
    "Capital Social Variable: $5,555.00 (CINCO MIL QUINIENTOS CINCUENTA Y CINCO PESOS 00/100 MONEDA NACIONAL) representado por 5,555 (cinco mil quinientas cincuenta y cinco) acciones de la Serie II, nominativas, con valor nominal de $1.00 (UN PESO 00/100 MONEDA NACIONAL) cada una.",
    "Capital Social Total: $55,555.00 (CINCUENTA Y CINCO MIL QUINIENTOS CINCUENTA Y CINCO PESOS 00/100 MONEDA NACIONAL) representado por 55,555 (cincuenta y cinco mil quinientas cincuenta y cinco) acciones. El monto total del capital social no sufrió modificación en el presente asiento."
  ).forEach(e => ch.push(e));
  ch.push(blankPara());
  ch.push(notaBlock(
    "Se hace constar que mediante la donación de nuda propiedad descrita en el presente asiento, " +
    "el C. Roberto Galante Totah conserva el usufructo vitalicio de 15,000 (quince mil) acciones Serie I, " +
    "correspondiéndole al C. Simón Galante Zaga la nuda propiedad de dichas acciones. " +
    "El C. Roberto Galante Totah continuará asistiendo y votando en las Asambleas en su carácter de usufructuario, " +
    "conforme al artículo 23 de la Ley General de Sociedades Mercantiles. " +
    "(E.P. N° 15,792, Notario N° 149 del Estado de México, de fecha 21 de septiembre de 2022.)"
  ));
  firmasBlock("NAUCALPAN DE JUÁREZ, ESTADO DE MÉXICO", "21 DE SEPTIEMBRE DE 2022", PDTE_2022, SRIO_2022).forEach(e => ch.push(e));

  return ch;
}


// ─────────────────────────────────────────────────────────────────────
// LIBRO DE REGISTRO DE SOCIOS — 4 ASIENTOS (misma historia corporativa)
// Narrativa: "hacemos constar que mediante [instrumento]... las personas
//             que a continuación se indican quedaron registradas como socios"
// ─────────────────────────────────────────────────────────────────────
function buildLibroRegistro() {
  const ch = [];

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 1 — Constitución / Socios fundadores 04/06/2004
  // ══════════════════════════════════════════════════════════════
  ch.push(tituloAsiento(1));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante "),
    r("Escritura Pública Número 34,475", { bold: true }),
    r(", de fecha "),
    r("04 de junio de 2004", { bold: true }),
    r(", ante la fe del Licenciado Pedro Porcayo Vergara, Notario Público Número 93 de México, Distrito Federal (Hoy Ciudad de México), inscrita en el Registro Público de Comercio de Naucalpan de Juárez, Estado de México, cuyo primer testimonio quedó debidamente inscrito bajo la Partida 702, Volumen 55, Libro Primero de Comercio, el 18 de noviembre de 2004, Folio Mercantil Electrónico 15254*7, se constituyó "),
    r("ADMINISTRADORA GDI, S.A. DE C.V.", { bold: true }),
    r(", con un capital social de "),
    r("$50,000.00 (CINCUENTA MIL PESOS 00/100 MONEDA NACIONAL)", { bold: true }),
    r(", representado por 100 (cien) acciones nominativas de la Serie I, correspondientes al capital fijo, con un valor nominal de "),
    r("$500.00 (QUINIENTOS PESOS 00/100 MONEDA NACIONAL)", { bold: true }),
    r(" cada una, las cuales quedaron debidamente suscritas y pagadas por las personas que a continuación se indican:"),
  ]));
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(tablaAccionistas1S(SOCIOS_2004));
  ch.push(blankPara());
  ch.push(domBlock("SIMÓN GALANTE ZAGA",    DOM_SIMON_DF));
  ch.push(domBlock("ROBERTO GALANTE TOTAH", DOM_ROBERTO_DF));
  ch.push(domBlock("ALBERTO GALANTE ZAGA",  DOM_ALBERTO));
  firmasBlock("DISTRITO FEDERAL", "04 DE JUNIO DE 2004", PDTE_2004, SRIO_2004).forEach(e => ch.push(e));

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 2 — Admisión de nuevo socio / Reforma estatutaria (protoc. 31/08/2012)
  // ══════════════════════════════════════════════════════════════
  ch.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  ch.push(tituloAsiento(2));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante Acta de Asamblea General Extraordinaria de Accionistas de fecha "),
    r("06 de septiembre de 2010", { bold: true }),
    r(", la cual se encuentra debidamente protocolizada en la "),
    r("Escritura Pública Número 25,135", { bold: true }),
    r(", de fecha 31 de agosto de 2012, pasada ante la fe del Licenciado Alfredo Caso Velázquez, Notario Público Número 17 del Estado de México, inscrita en el Registro Público de Comercio bajo el Folio Mercantil Electrónico 15254*7 con fecha 27 de febrero de 2013, la Asamblea resolvió lo siguiente: "),
    r("i)", { bold: true }),
    r(" Cambio del valor nominal de las acciones de "),
    r("$500.00 (QUINIENTOS PESOS)", { bold: true }),
    r(" a "),
    r("$1.00 (UN PESO 00/100 MONEDA NACIONAL)", { bold: true }),
    r(" por acción, con lo cual el capital fijo quedó representado por 50,000 (cincuenta mil) acciones nominativas de la Serie I; "),
    r("ii)", { bold: true }),
    r(" Aumento de capital social en su parte variable mediante la emisión de 5,555 (cinco mil quinientas cincuenta y cinco) acciones de la Serie II, con valor nominal de "),
    r("$1.00 (UN PESO 00/100 MONEDA NACIONAL)", { bold: true }),
    r(" cada una, admitiéndose como "),
    r("nuevo accionista", { bold: true }),
    r(" a "),
    r("GRUPO INVERMEM, S.A. DE C.V.", { bold: true }),
    r(", quien suscribió y pagó la totalidad de las acciones emitidas. En consecuencia, el registro de socios quedó de la siguiente forma:"),
  ]));
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(tablaAccionistas2S(SOCIOS_2010));
  ch.push(blankPara());
  ch.push(domBlock("SIMÓN GALANTE ZAGA",             DOM_SIMON_DF));
  ch.push(domBlock("ROBERTO GALANTE TOTAH",          DOM_ROBERTO_DF));
  ch.push(domBlock("ALBERTO GALANTE ZAGA",           DOM_ALBERTO));
  ch.push(domBlock("GRUPO INVERMEM, S.A. DE C.V.",   DOM_INVERMEM_DF));
  firmasBlock("NAUCALPAN DE JUÁREZ, ESTADO DE MÉXICO", "31 DE AGOSTO DE 2012", PDTE_2004, SRIO_2004).forEach(e => ch.push(e));

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 3 — Modificación objeto social 01/07/2021
  //             (sin cambios en la estructura de socios)
  // ══════════════════════════════════════════════════════════════
  ch.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  ch.push(tituloAsiento(3));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante Acta de Asamblea General Extraordinaria de Accionistas de fecha "),
    r("01 de julio de 2021", { bold: true }),
    r(", la cual se encuentra debidamente protocolizada en la "),
    r("Escritura Pública Número 64,769", { bold: true }),
    r(", de fecha 03 de agosto de 2021, pasada ante la fe del Licenciado José Daniel Labardini Schettino, Notario Público Número 86 de la Ciudad de México, inscrita en el Registro Público de Comercio bajo el Folio Mercantil Electrónico 15254 con fecha 06 de octubre de 2021, la Asamblea resolvió lo siguiente: "),
    r("i)", { bold: true }),
    r(" Reforma de la Cláusula Tercera (Objeto Social) y adición de la Cláusula Tercera Bis de los Estatutos Sociales; "),
    r("ii)", { bold: true }),
    r(" Otorgamiento de nuevos poderes a diversas personas. La estructura de socios de la Sociedad no sufrió modificación alguna, quedando registrada de la siguiente forma:"),
  ]));
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(tablaAccionistas2S(SOCIOS_2010));
  ch.push(blankPara());
  ch.push(domBlock("SIMÓN GALANTE ZAGA",             DOM_SIMON));
  ch.push(domBlock("ROBERTO GALANTE TOTAH",          DOM_ROBERTO));
  ch.push(domBlock("ALBERTO GALANTE ZAGA",           DOM_ALBERTO));
  ch.push(domBlock("GRUPO INVERMEM, S.A. DE C.V.",   DOM_INVERMEM));
  firmasBlock("CIUDAD DE MÉXICO", "03 DE AGOSTO DE 2021", PDTE_2004, SRIO_2004).forEach(e => ch.push(e));

  // ══════════════════════════════════════════════════════════════
  // ASIENTO 4 — Donación nuda propiedad + Nuevo Consejo 07/03/2022
  // ══════════════════════════════════════════════════════════════
  ch.push(new Paragraph({ children: [new PageBreak()], spacing: { before: 0, after: 0 } }));
  ch.push(tituloAsiento(4));
  ch.push(bodyPara([
    r("Los Suscritos en nuestro carácter de Presidente y Secretario del Consejo de Administración, hacemos constar que mediante Acta de Asamblea General Ordinaria de Accionistas de fecha "),
    r("07 de marzo de 2022", { bold: true }),
    r(", la cual se encuentra debidamente protocolizada en la "),
    r("Escritura Pública Número 15,792", { bold: true }),
    r(", de fecha 21 de septiembre de 2022, pasada ante la fe del Licenciado José Manuel Gómez del Campo Gurza, Notario Público Número 149 del Estado de México, la Asamblea resolvió lo siguiente: "),
    r("i)", { bold: true }),
    r(" El C. "),
    r("Roberto Galante Totah", { bold: true }),
    r(" dona la nuda propiedad de "),
    r("15,000 (quince mil) acciones nominativas de la Serie I", { bold: true }),
    r(", con valor nominal de $1.00 cada una, a favor del C. "),
    r("Simón Galante Zaga", { bold: true }),
    r(", conservando el donante el usufructo vitalicio de las mismas; "),
    r("ii)", { bold: true }),
    r(" Renovación del Consejo de Administración: Presidente: "),
    r("Simón Galante Zaga", { bold: true }),
    r("; Secretario: "),
    r("Eduardo Zaga Cojab", { bold: true }),
    r("; "),
    r("iii)", { bold: true }),
    r(" Nombramiento de "),
    r("Hilarión Arturo Pérez Nava", { bold: true }),
    r(" como Comisario de la Sociedad. La estructura de socios de la Sociedad no sufrió modificación en cuanto al número de acciones o valor del capital, quedando registrada de la siguiente forma:"),
  ]));
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(blankPara());
  ch.push(tablaAccionistas2S(SOCIOS_2022));
  ch.push(blankPara());
  ch.push(notaBlock(
    "Por virtud de la donación de nuda propiedad celebrada en la Asamblea de fecha 07 de marzo de 2022, " +
    "protocolizada mediante E.P. N° 15,792 de fecha 21 de septiembre de 2022 ante el Notario N° 149 del Estado de México, " +
    "el C. Roberto Galante Totah conserva el usufructo vitalicio de dichas acciones, " +
    "correspondiendo la nuda propiedad al C. Simón Galante Zaga. " +
    "El usufructuario ejerce los derechos de voto en asambleas conforme al Art. 23 LGSM."
  ));
  ch.push(blankPara());
  ch.push(domBlock("SIMÓN GALANTE ZAGA",             DOM_SIMON));
  ch.push(domBlock("ROBERTO GALANTE TOTAH",          DOM_ROBERTO));
  ch.push(domBlock("ALBERTO GALANTE ZAGA",           DOM_ALBERTO));
  ch.push(domBlock("GRUPO INVERMEM, S.A. DE C.V.",   DOM_INVERMEM));
  firmasBlock("NAUCALPAN DE JUÁREZ, ESTADO DE MÉXICO", "21 DE SEPTIEMBRE DE 2022", PDTE_2022, SRIO_2022).forEach(e => ch.push(e));

  return ch;
}

// ═════════════════════════════════════════════════════════════════════
// DOCS 3-5: TENENCIA ACCIONARIA / APODERADOS Y PODERES / HISTORIAL
// (mismo estilo "libro": página legal, TBL_W, encabezados BLK, firmasBlock)
// ═════════════════════════════════════════════════════════════════════

// Encabezado común: título + "DENOMINACIÓN · R.F.C." + párrafos introductorios
function docHeader(titulo, introParas) {
  const out = [
    new Paragraph({ children: [r(titulo, { size: SZ_TITLE, bold: true })], alignment: AlignmentType.CENTER, spacing: { before: 0, after: 80, line: 240 } }),
    new Paragraph({ children: [r(`${EMP.nombre}  ·  R.F.C.: ${EMP.rfc}`, { bold: true })], alignment: AlignmentType.CENTER, spacing: { before: 0, after: 120, line: 240 } }),
  ];
  (introParas || []).forEach(p => out.push(p));
  return out;
}

// ── DATOS (DEMO) para los 3 documentos — el usuario los reemplaza por los reales ──
// Tenencia: la tabla se AGREGA por accionista desde SOCIOS_2022 (estructura vigente).
//   Para anotar a un accionista (p. ej. usufructo), agregar `anotacion: "..."` a su
//   objeto en SOCIOS_2022 y aparecerá junto a su nombre.
const TENENCIA_CORTE = "28 de mayo de 2026 (fecha del Asiento 5 — última variación de capital registrada).";
const TENENCIA_NOTA  = "El capital social total vigente es de $ 55,555.00 (Capital Fijo $ 50,000.00 + Capital Variable $ 5,555.00), representado por 55,555 acciones con valor nominal de $1.00 M.N. cada una.";

const APODERADOS_CORTE = "28 de mayo de 2026.";
const CONSEJO_INTRO = "Conforme al Art. 10 y 142-150 de la Ley General de Sociedades Mercantiles, el Consejo de Administración representa a la Sociedad por disposición estatutaria y legal, sin requerir poder notarial autónomo, salvo que la Asamblea le confiera facultades adicionales expresas. Consejo vigente designado en Asamblea General Ordinaria de fecha 07 de marzo de 2022, protocolizada en la Escritura Pública N° 15,792 de fecha 21 de septiembre de 2022, ante el Notario Público N° 149 de Metepec, Estado de México:";
const CONSEJO_VIGENTE = [
  { nombre: "SIMÓN GALANTE ZAGA",         cargo: "Presidente del Consejo de Administración", designacion: "E.P. 15,792" },
  { nombre: "EDUARDO ZAGA COJAB",         cargo: "Secretario del Consejo de Administración", designacion: "E.P. 15,792" },
  { nombre: "HILARIÓN ARTURO PÉREZ NAVA", cargo: "Comisario",                                designacion: "E.P. 15,792" },
];
const PODERES_INTRO = "Poderes otorgados por la Sociedad ante Notario Público, con vigencia indefinida, para ejercerse conjunta o separadamente. Se relacionan por instrumento (del más reciente al más antiguo). Salvo indicación en contrario, NO comprenden actos de dominio, ni facultad para suscribir títulos de crédito, ni para comprometer el patrimonio de la Sociedad.";
// APODERADOS_INSTRUMENTOS: una entrada por ESCRITURA de poderes. Cada instrumento tiene uno o
// más `grupos`, y cada grupo describe sus facultades + la lista de apoderados a favor de quienes
// se otorgaron. Si el arreglo queda VACÍO se imprime la ALERTA (dato pendiente). Los nombres se
// presentan en tabla de 3 columnas. DEMO abreviada — reemplazar con los datos reales de la sociedad.
const APODERADOS_INSTRUMENTOS = [
  {
    instrumento: "Escritura Pública N° 73,049 del 22 de mayo de 2024.",
    grupos: [
      { desc: "Poder para pleitos y cobranzas, actos de administración (incl. materia laboral / representación patronal), administración fiscal limitada y poder especial de trámites y servicios. A favor de:",
        nombres: ["Ma. Dolores Domínguez Vázquez", "Miguel Ángel Martínez Ramírez", "Carlos Alberto Jasinto Dávila", "Silvia Josefina Gancedo Ávila", "Óscar Adrián Enríquez Delgado"] },
      { desc: "Poder limitado para actos de administración de carácter fiscal (RFC, e.firma y trámites ante autoridades fiscales). A favor de:",
        nombres: ["Ma. Dolores Domínguez Vázquez", "Alejandrina Martínez Hernández", "Fernando Ruiz Elizarraraz"] },
    ],
  },
];
const APODERADOS_NOTA = "Los poderes se otorgaron con vigencia indefinida y sin que se localizara cláusula de revocación en los instrumentos analizados; por ello, formalmente todas las personas listadas conservan sus facultades. Se recomienda otorgar una escritura de REVOCACIÓN expresa para depurar el padrón y dejar vigentes únicamente a los apoderados actuales.";
const APODERADOS_ALERTA = "consta que se otorgaron poderes, pero el expediente no incluyó el detalle de quiénes son los apoderados ni sus facultades. Debe revisarse la escritura de poderes para completar el listado de apoderados vigentes, sus facultades y confirmar si siguen vigentes.";

const HISTORIAL_INTRO = "Línea de tiempo completa de la Sociedad. Incluye TODOS los eventos societarios documentados, incluyendo aquellos que, por no tener efecto en el capital social o en la titularidad de acciones (cambios de consejo, comisario, apoderados, objeto social, etc.), no generan asiento propio en el Libro de Variaciones de Capital ni en el Libro de Registro de Accionistas, conforme a la Ley General de Sociedades Mercantiles.";
const HISTORIAL = [
  { fecha: "04/06/2004", instrumento: "E.P. 34,475, Notario 93, México, D.F.", movimiento: "Constitución de la Sociedad", asiento: "Asiento 1" },
  { fecha: "06/09/2010 (protoc. 31/08/2012, E.P. 25,135)", instrumento: "Notario 17, Tlalnepantla de Baz, Edo. Méx.", movimiento: "Reforma estatutaria; cambio de valor nominal; aumento de capital variable — admisión de Grupo Invermem, S.A. de C.V.", asiento: "Asiento 2" },
  { fecha: "01/07/2021 (protoc. 03/08/2021, E.P. 64,769)", instrumento: "Notario 86, Ciudad de México", movimiento: "Reforma de Objeto Social; otorgamiento de nuevos poderes a diversas personas", asiento: "Sin asiento (sin efecto de capital/titularidad) — ver Documento de Apoderados" },
  { fecha: "07/03/2022 (protoc. 21/09/2022, E.P. 15,792)", instrumento: "Notario 149, Metepec, Edo. Méx.", movimiento: "Donación de nuda propiedad (Roberto Galante Totah → Simón Galante Zaga, 15,000 acciones Serie A)", asiento: "Asiento 3" },
  { fecha: "07/03/2022 (protoc. 21/09/2022, E.P. 15,792)", instrumento: "Notario 149, Metepec, Edo. Méx.", movimiento: "Renovación del Consejo de Administración y nombramiento de Comisario", asiento: "Sin asiento (acto administrativo)" },
];

// Tabla compacta de nombres en N columnas (para listas largas de apoderados)
function nombresTable(nombres, cols) {
  cols = cols || 3;
  const colW = Math.floor(TBL_W / cols);
  const rows = [];
  for (let i = 0; i < nombres.length; i += cols) {
    const cells = [];
    for (let c = 0; c < cols; c++) cells.push(dataCell(nombres[i + c] || "", colW, AlignmentType.LEFT));
    rows.push(new TableRow({ children: cells }));
  }
  return new Table({ width: { size: TBL_W, type: WidthType.DXA }, columnWidths: Array(cols).fill(colW), rows });
}

// ── DOC 3: Tabla de Tenencia Accionaria Vigente ──
function buildTenencia() {
  const socios = SOCIOS_2022;
  const grandAcc = socios.reduce((s, x) => s + (x.acc_total || 0), 0);
  const capFijo  = socios.reduce((s, x) => s + (x.val_serieI  || 0), 0);
  const capVar   = socios.reduce((s, x) => s + (x.val_serieII || 0), 0);
  const capTot   = capFijo + capVar;
  const ch = docHeader("TABLA DE TENENCIA ACCIONARIA VIGENTE", [
    bodyPara([r("Corte al: ", { bold: true }), r(TENENCIA_CORTE)], AlignmentType.BOTH, 0, 160),
  ]);
  const C = [3200, 1500, 1502, 1000];
  const rows = [new TableRow({ tableHeader: true, children: [
    hdrCell("ACCIONISTA / RFC", C[0]), hdrCell("ACCIONES TOTALES", C[1]),
    hdrCell("CAPITAL (VALOR NOMINAL)", C[2]), hdrCell("% CAPITAL", C[3]),
  ] })];
  let sumPct = 0;
  socios.forEach(s => {
    const cap = (s.val_serieI || 0) + (s.val_serieII || 0);
    const pct = grandAcc ? (s.acc_total / grandAcc * 100) : 0;
    sumPct += pct;
    const nombre = s.nombre + (s.anotacion ? ` (${s.anotacion})` : "");
    rows.push(new TableRow({ children: [
      dataCell(`${nombre}\nRFC: ${s.rfc}`, C[0], AlignmentType.LEFT, true),
      dataCell(fmtNum(s.acc_total), C[1]),
      dataCell(fmtMXN(cap), C[2]),
      dataCell(pct.toFixed(2) + "%", C[3]),
    ] }));
  });
  rows.push(new TableRow({ children: [
    dataCell("TOTAL", C[0], AlignmentType.CENTER, true),
    dataCell(fmtNum(grandAcc), C[1], AlignmentType.CENTER, true),
    dataCell(fmtMXN(capTot), C[2], AlignmentType.CENTER, true),
    dataCell(sumPct.toFixed(2) + "%", C[3], AlignmentType.CENTER, true),
  ] }));
  ch.push(new Table({ width: { size: TBL_W, type: WidthType.DXA }, columnWidths: C, rows }));
  ch.push(notaBlock(TENENCIA_NOTA));
  return ch;
}

// ── DOC 4: Apoderados y Poderes Vigentes ──
function buildApoderados() {
  const ch = docHeader("APODERADOS Y PODERES VIGENTES", [
    bodyPara([r("Documento informativo (para conocimiento).", { italic: true })], AlignmentType.CENTER, 0, 140),
  ]);
  // ── I. Órgano de gobierno y accionistas ──
  ch.push(bodyPara([r("I. Órgano de gobierno y accionistas (representación orgánica)", { bold: true })], AlignmentType.LEFT, 120, 80));
  ch.push(bodyPara([r(CONSEJO_INTRO)]));
  ch.push(blankPara());
  const C = [2900, 2900, 1402];
  const rowsC = [new TableRow({ tableHeader: true, children: [
    hdrCell("NOMBRE", C[0]), hdrCell("CARGO", C[1]), hdrCell("DESIGNACIÓN", C[2]),
  ] })];
  CONSEJO_VIGENTE.forEach(m => rowsC.push(new TableRow({ children: [
    dataCell(m.nombre, C[0], AlignmentType.LEFT, true), dataCell(m.cargo, C[1], AlignmentType.LEFT), dataCell(m.designacion, C[2]),
  ] })));
  ch.push(new Table({ width: { size: TBL_W, type: WidthType.DXA }, columnWidths: C, rows: rowsC }));
  ch.push(blankPara());
  // Accionistas (tomados de la estructura vigente)
  ch.push(bodyPara([r("Accionistas:", { bold: true })], AlignmentType.LEFT, 60, 40));
  const Ca = [4300, 2902];
  const rowsAcc = [new TableRow({ tableHeader: true, children: [hdrCell("ACCIONISTA", Ca[0]), hdrCell("R.F.C.", Ca[1])] })];
  SOCIOS_2022.forEach(s => rowsAcc.push(new TableRow({ children: [
    dataCell(s.nombre + (s.anotacion ? ` (${s.anotacion})` : ""), Ca[0], AlignmentType.LEFT, true), dataCell(s.rfc, Ca[1]),
  ] })));
  ch.push(new Table({ width: { size: TBL_W, type: WidthType.DXA }, columnWidths: Ca, rows: rowsAcc }));
  ch.push(blankPara());
  // ── II. Apoderados con poder notarial vigente ──
  ch.push(bodyPara([r("II. Apoderados con poder notarial vigente", { bold: true })], AlignmentType.LEFT, 120, 80));
  ch.push(bodyPara([r(PODERES_INTRO)]));
  ch.push(blankPara());
  if (!APODERADOS_INSTRUMENTOS || APODERADOS_INSTRUMENTOS.length === 0) {
    ch.push(alertaBlock(APODERADOS_ALERTA));
  } else {
    APODERADOS_INSTRUMENTOS.forEach((ins, i) => {
      ch.push(bodyPara([r(String.fromCharCode(65 + i) + ") " + ins.instrumento, { bold: true })], AlignmentType.LEFT, 80, 40));
      ins.grupos.forEach(g => {
        ch.push(bodyPara([r(g.desc)], AlignmentType.BOTH, 20, 40));
        ch.push(nombresTable(g.nombres, 3));
      });
      ch.push(blankPara());
    });
    ch.push(notaBlock(APODERADOS_NOTA));
  }
  // Documento SÓLO informativo (para conocimiento): NO lleva firmas.
  return ch;
}

// ── DOC 5: Historial Societario ──
function buildHistorial() {
  const ch = docHeader("HISTORIAL SOCIETARIO", [
    bodyPara([r(HISTORIAL_INTRO)]),
  ]);
  const C = [2000, 2200, 2202, 800];
  const rows = [new TableRow({ tableHeader: true, children: [
    hdrCell("FECHA", C[0]), hdrCell("DOCUMENTO / INSTRUMENTO", C[1]), hdrCell("MOVIMIENTO", C[2]), hdrCell("ASIENTO / LIBRO", C[3]),
  ] })];
  HISTORIAL.forEach(h => rows.push(new TableRow({ children: [
    dataCell(h.fecha, C[0], AlignmentType.LEFT), dataCell(h.instrumento, C[1], AlignmentType.LEFT),
    dataCell(h.movimiento, C[2], AlignmentType.LEFT), dataCell(h.asiento, C[3], AlignmentType.LEFT),
  ] })));
  ch.push(new Table({ width: { size: TBL_W, type: WidthType.DXA }, columnWidths: C, rows }));
  return ch;
}

// ─────────────────────────────────────────────────────────────────────
// DOC 6: BENEFICIARIO CONTROLADOR (PLD) — Acuerdo 115/2026
// (marco fijo; EDITAR por sociedad BENEFICIARIO_CONTROLADOR / BC_MAPEO / BC_CONCLUSION
//  con el resultado del mapeo de la cadena de control hasta persona física)
// ─────────────────────────────────────────────────────────────────────
const BC_MARCO = "El presente apartado se formula conforme a los artículos 32-B Ter, 32-B Quáter y 32-B Quinquies del Código Fiscal de la Federación; las Reglas 2.8.1.20 a 2.8.1.23 de la Resolución Miscelánea Fiscal; la Ley Federal para la Prevención e Identificación de Operaciones con Recursos de Procedencia Ilícita (LFPIORPI), su reforma de julio de 2025 y su Reglamento; las Recomendaciones 24 y 25 del GAFI; y el Acuerdo 115/2026 (SHCP, DOF 07 de agosto de 2026; en vigor el 30 de noviembre de 2026), que regula al beneficiario controlador en su Capítulo III Quinquies (artículos 23 Quinquies a 23 Quinquies 3).";
const BC_CRITERIO = "Es beneficiario controlador la persona física que, directa o indirectamente, obtiene el beneficio de la persona moral o ejerce su control último, conforme al siguiente orden de prelación: (I) posee el 25% o más de la participación; (II) ejerce el control por otros medios (imponer decisiones, designar a la administración o ejercer el voto respecto de más del 50%); o (III) en su defecto, ocupa la administración de mayor grado. La información y documentación soporte debe obtenerse, conservarse y mantenerse actualizada durante la relación, y el registro del beneficiario controlador debe conservarse por un mínimo de diez años (Art. 10 Septies 3 del Acuerdo 115/2026).";
// EDITAR por sociedad: personas físicas que resultan beneficiario controlador (tras el mapeo).
// { nombre, curp, rfc, nac, pct, criterio }
const BENEFICIARIO_CONTROLADOR = [];
// EDITAR por sociedad: mapeo de accionistas personas morales hasta persona física.
// { sociedad, pct, titularidad }
const BC_MAPEO = [];
const BC_CONCLUSION = ""; // EDITAR (opcional)

function buildBeneficiarioControlador() {
  const ch = docHeader("BENEFICIARIO CONTROLADOR", [
    bodyPara([r("Marco normativo. ", { bold: true }), r(BC_MARCO)]),
    bodyPara([r("Criterio de identificación. ", { bold: true }), r(BC_CRITERIO)]),
  ]);
  ch.push(blankPara());
  ch.push(bodyPara([r("Beneficiarios controladores identificados:", { bold: true })]));
  const C = [2400, 2200, 1000, 1602];
  const rows = [new TableRow({ tableHeader: true, children: [
    hdrCell("NOMBRE", C[0]), hdrCell("CURP / RFC", C[1]), hdrCell("NACIONALIDAD", C[2]), hdrCell("% Y/O CRITERIO", C[3]),
  ] })];
  BENEFICIARIO_CONTROLADOR.forEach(b => rows.push(new TableRow({ children: [
    dataCell(b.nombre, C[0], AlignmentType.LEFT),
    dataCell(`${b.curp || ""}\n${b.rfc || ""}`, C[1], AlignmentType.LEFT),
    dataCell(b.nac || "Mexicana", C[2], AlignmentType.CENTER),
    dataCell(`${b.pct || ""}\n${b.criterio || ""}`, C[3], AlignmentType.CENTER),
  ] })));
  ch.push(new Table({ width: { size: TBL_W, type: WidthType.DXA }, columnWidths: C, rows }));
  if (BC_MAPEO.length) {
    ch.push(blankPara());
    ch.push(bodyPara([r("Mapeo de la cadena de control (hasta persona física). ", { bold: true }),
      r("Los accionistas/socios personas morales no son beneficiario controlador final; se traza su estructura hasta las personas físicas que las controlan:")]));
    const M = [3200, 1000, 3002];
    const mrows = [new TableRow({ tableHeader: true, children: [
      hdrCell("ACCIONISTA / SOCIO (PERSONA MORAL)", M[0]), hdrCell("%", M[1]), hdrCell("TITULARIDAD ÚLTIMA (PERSONA FÍSICA)", M[2]),
    ] })];
    BC_MAPEO.forEach(m => mrows.push(new TableRow({ children: [
      dataCell(m.sociedad, M[0], AlignmentType.LEFT), dataCell(m.pct || "", M[1], AlignmentType.CENTER), dataCell(m.titularidad, M[2], AlignmentType.LEFT),
    ] })));
    ch.push(new Table({ width: { size: TBL_W, type: WidthType.DXA }, columnWidths: M, rows: mrows }));
  }
  if (BC_CONCLUSION) { ch.push(blankPara()); ch.push(bodyPara([r("Conclusión. ", { bold: true }), r(BC_CONCLUSION)])); }
  return ch;
}

// ─────────────────────────────────────────────────────────────────────
// MAIN
// ─────────────────────────────────────────────────────────────────────
async function main() {
  // ── Crear estructura de carpetas ───────────────────────────────────
  const carpetaEmpresa   = path.join(BASE_OUT, NOMBRE_ARCHIVO);
  const carpetaVariaciones = path.join(carpetaEmpresa, "Libro_Variaciones_Capital");
  const carpetaRegistro    = path.join(carpetaEmpresa, "Libro_Registro_Socios");
  const carpetaTenencia    = path.join(carpetaEmpresa, "Tenencia_Accionaria");
  const carpetaApoderados  = path.join(carpetaEmpresa, "Apoderados_y_Poderes");
  const carpetaHistorial   = path.join(carpetaEmpresa, "Historial_Societario");
  const carpetaBC          = path.join(carpetaEmpresa, "Beneficiario_Controlador");
  fs.mkdirSync(carpetaVariaciones, { recursive: true });
  fs.mkdirSync(carpetaRegistro,    { recursive: true });
  fs.mkdirSync(carpetaTenencia,    { recursive: true });
  fs.mkdirSync(carpetaApoderados,  { recursive: true });
  fs.mkdirSync(carpetaHistorial,   { recursive: true });
  fs.mkdirSync(carpetaBC,          { recursive: true });

  const sectionProps = {
    page: {
      size: { width: PAGE.width, height: PAGE.height },
      margin: { top: MARGIN.top, bottom: MARGIN.bottom, left: MARGIN.left, right: MARGIN.right },
    },
  };

  // ── DOC 1: Libro de Variaciones de Capital ─────────────────────────
  const doc1 = new Document({
    sections: [{ properties: sectionProps, children: buildLibroVariaciones() }],
  });
  const buf1 = await Packer.toBuffer(doc1);
  const out1 = path.join(carpetaVariaciones, `Libro_Variaciones_Capital_${NOMBRE_ARCHIVO}.docx`);
  fs.writeFileSync(out1, buf1);
  console.log(`✅ ${out1}`);

  // ── DOC 2: Libro de Registro de Socios ────────────────────────────
  const doc2 = new Document({
    sections: [{ properties: sectionProps, children: buildLibroRegistro() }],
  });
  const buf2 = await Packer.toBuffer(doc2);
  const out2 = path.join(carpetaRegistro, `Libro_Registro_Socios_${NOMBRE_ARCHIVO}.docx`);
  fs.writeFileSync(out2, buf2);
  console.log(`✅ ${out2}`);

  // ── DOC 3: Tenencia Accionaria Vigente ────────────────────────────
  const doc3 = new Document({ sections: [{ properties: sectionProps, children: buildTenencia() }] });
  const out3 = path.join(carpetaTenencia, `Tenencia_Accionaria_Vigente_${NOMBRE_ARCHIVO}.docx`);
  fs.writeFileSync(out3, await Packer.toBuffer(doc3));
  console.log(`✅ ${out3}`);

  // ── DOC 4: Apoderados y Poderes Vigentes ──────────────────────────
  const doc4 = new Document({ sections: [{ properties: sectionProps, children: buildApoderados() }] });
  const out4 = path.join(carpetaApoderados, `Apoderados_y_Poderes_Vigentes_${NOMBRE_ARCHIVO}.docx`);
  fs.writeFileSync(out4, await Packer.toBuffer(doc4));
  console.log(`✅ ${out4}`);

  // ── DOC 5: Historial Societario ───────────────────────────────────
  const doc5 = new Document({ sections: [{ properties: sectionProps, children: buildHistorial() }] });
  const out5 = path.join(carpetaHistorial, `Historial_Societario_${NOMBRE_ARCHIVO}.docx`);
  fs.writeFileSync(out5, await Packer.toBuffer(doc5));
  console.log(`✅ ${out5}`);

  // ── DOC 6: Beneficiario Controlador (PLD) ─────────────────────────
  const doc6 = new Document({ sections: [{ properties: sectionProps, children: buildBeneficiarioControlador() }] });
  const out6 = path.join(carpetaBC, `Beneficiario_Controlador_${NOMBRE_ARCHIVO}.docx`);
  fs.writeFileSync(out6, await Packer.toBuffer(doc6));
  console.log(`✅ ${out6}`);
}

main().catch(e => { console.error(e); process.exit(1); });

```

---

### PATRÓN DE ASIENTO (referencia rápida)

**buildLibroVariaciones** (CON capitalBreakdown):
```javascript
ch.push(tituloAsiento(N));
ch.push(bodyPara([r("Los Suscritos..."), ...]));
capitalBreakdown(textoFijo, textoVariable_o_null, textoTotal).forEach(e => ch.push(e));
ch.push(bodyPara([r("Quedando la estructura...")]));
ch.push(blankPara());
ch.push(tablaAccionistas1S(SOCIOS)); // o tablaAccionistas2S
ch.push(blankPara());
ch.push(domBlock("NOMBRE SOCIO", DOM_SOCIO));
// notaBlock() solo si aplica (nuda propiedad, cesiones especiales)
firmasBlock(lugar, fecha, presidente, secretario).forEach(e => ch.push(e));
```

**buildLibroRegistro** (SIN capitalBreakdown):
```javascript
ch.push(tituloAsiento(N));
ch.push(bodyPara([r("Los Suscritos..."), ...]));
ch.push(blankPara()); ch.push(blankPara()); ch.push(blankPara());
ch.push(tablaAccionistas1S(SOCIOS)); // o tablaAccionistas2S
ch.push(blankPara());
ch.push(domBlock("NOMBRE SOCIO", DOM_SOCIO));
firmasBlock(lugar, fecha, presidente, secretario).forEach(e => ch.push(e));
```
