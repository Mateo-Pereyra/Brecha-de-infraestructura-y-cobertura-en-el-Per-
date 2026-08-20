# Brecha-de-infraestructura-y-cobertura-en-el-Perú-
Grupo: ROMPEBRECHAS

Proyecto de limpieza e integración de datos sobre brechas territoriales en la oferta educativa peruana, desarrollado como entregable del laboratorio "Limpieza inicial de datos e integración básica".

1. Problema y unidad de análisis

El sistema educativo peruano distribuye sus servicios de forma desigual en el territorio. El proyecto se concentra en Educación Inicial a nivel distrital, por ser el nivel con la brecha más severa y más rural del sistema (ver el detalle cuantitativo en la sección 1 del notebook).

Pregunta de investigación: ¿existen clusters de distritos con perfiles similares de oferta de Educación Inicial, y qué distritos concentran las mayores brechas según densidad de servicios, escolarización, gestión y ruralidad?

La unidad de análisis principal es el distrito (agregando servicios educativos); la unidad de análisis de cada fuente individual varía y se detalla en la sección 2.

2. Fuentes de datos
#	Archivo	Contenido	Registros	Campos	Unidad de análisis
F1	Padron_web.dbf	MINEDU–ESCALE, Padrón de Instituciones Educativas	180,792	45	Servicio educativo (todos los niveles)
F2	Lineal_3AP_1.dbf	MINEDU–ESCALE, Censo Escolar, Cédula 3AP	37,354	193	Servicio educativo de Educación Primaria
F4	denuncias_escolares.xlsx	MINEDU–SíseVe, reportes de violencia escolar	48,273	8	Caso reportado de violencia escolar
F3	(pendiente)	INEI, proyecciones de población distrital	—	—	Distrito

Llave de integración principal: COD_MOD + ANEXO (une F1 ↔ F2, match 100%). CODGEO (UBIGEO) es la llave de agregación a nivel distrital. F4 no trae llave de registro única; se integra por agregación usando DRE + UGEL normalizado (ver sección 9 del notebook).

Nota importante sobre F2. El archivo disponible (Lineal_3AP_1.dbf) es la Cédula 3AP del Censo Escolar, que cubre Educación Primaria (NROCED == '3AP', NIV_MOD == 'B0' en el 100% de las filas) — no es la Cédula 1A de Educación Inicial que se planeaba usar originalmente. Esto se verificó directamente sobre el archivo real y se documenta con detalle en el notebook (sección "Cambio de alcance 2"). Como consecuencia:

Los indicadores de Educación Inicial de este proyecto se calculan solo con el padrón (F1), que no depende de esa cédula.
F2 (Cédula 3AP) se usa como una segunda línea de análisis, en paralelo, para Educación Primaria — con la misma llave y el mismo nivel de rigor.
Si el proyecto necesita condiciones de funcionamiento específicas de Inicial, se requiere conseguir la Cédula 1A real (no está entre los archivos entregados).
3. Contenido del notebook (ROMPEBRECHAS.ipynb)

El notebook sigue el flujo del laboratorio del curso (inspección → problemas → limpieza → integración), aplicado a las fuentes reales del proyecto:

Importación de datos (F1, F2, F4)
Tamaño y tipos de cada fuente
describe() y estadísticas descriptivas
Valores faltantes
Variables categóricas y su distribución
Columnas comunes entre F1 y F2
Integración F1 ↔ F2 (COD_MOD + ANEXO)
Revisión del resultado de la integración
Diccionario de datos (F1 documentado a mano; F2 documentado por patrón de nomenclatura de sus 193 preguntas)
Exportación de resultados (resultado_laboratorio_integracion.xlsx)

Sección del informe (entregables del curso):

Fuentes de datos y llave de integración (## 2)
Unidad de análisis (## 3)
Columnas comunes y llaves (## 4)
Tabla resumen de cada fuente (## 5)
Diccionario de datos (## 6)
Problemas encontrados (## 7), incluida la subsección 7.7 con el perfilamiento específico de F4 (denuncias escolares): duplicados sin ID de caso, ambigüedad de llave UGEL↔DRE, formato de texto no uniforme, categoría cruzada "con uso de armas", outliers por fecha/UGEL.
Decisiones de limpieza justificadas (## 8), con una tabla decisiones (DataFrame: variable, problema, decisión, justificación) que cubre F1, F2 y F4.
Base integrada inicial (## 9): integración F1↔F2 registro a registro (100% match) y prueba de integración F1↔F4 por UGEL normalizada (98.2% de UGEL, 92.8% de filas con cobertura).
Referencias (## 10), formato APA 7.ª edición.
4. Cómo ejecutar
Colocar en la misma carpeta que el notebook:
Padron_web.dbf
Lineal_3AP_1.dbf
denuncias_escolares.xlsx
Instalar dependencias: pip install pandas numpy dbfread openpyxl
Ejecutar el notebook de principio a fin (Run All). Fue verificado de punta a punta con los tres archivos reales, sin errores.
El notebook exporta resultado_laboratorio_integracion.xlsx con tres hojas: resumen_fuentes, diccionario_datos y primaria_integrada.
5. Pendientes conocidos
F3 (INEI, población distrital) aún no está incorporada; es necesaria para convertir conteos en tasas por distrito.
Si se requiere retomar el enfoque de condiciones de funcionamiento de Inicial, hace falta la Cédula 1A real (el archivo entregado es la 3AP, de Primaria).
La sección 7 ("Problemas encontrados") conserva observaciones generales de una versión previa del proyecto (p. ej. sobre TDOCENTE/TALUMNO/TSECCION) que deben revisarse contra el hallazgo ya documentado de que ninguna fuente actual trae variables de matrícula, docentes o secciones.

Integrantes: Mateo Pereyra · Raúl Porras · Mauro Martíne
