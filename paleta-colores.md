# Paleta de colores — FMA Adoption Intelligence
**MIAD 2026 · Universidad de los Andes**

---

## Color principal (Navy)

| Nombre | Hex | Uso |
|---|---|---|
| Navy principal | `#1A3A6B` | Encabezados, botones primarios, sidebar, tablas |
| Navy oscuro (hover) | `#122D55` | Estado hover de botones y elementos navy |
| Navy claro (fondo) | `#EEF3FB` | Fondo de badges, áreas destacadas suaves |
| Navy borde | `#C0D4F0` | Bordes de tarjetas y badges sobre fondo claro |

> Este es el color identitario del proyecto. En slides: usar como color de encabezados de sección, fondos de diapositiva de portada y elementos de acento.

---

## Fondo y superficie

| Nombre | Hex | Uso |
|---|---|---|
| Fondo de página | `#F1F5F9` | Fondo base del sitio (gris muy claro, casi blanco azulado) |
| Blanco puro | `#FFFFFF` | Tarjetas, secciones alternadas, superficies elevadas |
| Borde general | `#E2E8F0` | Líneas divisoras, bordes de tarjetas y tablas |

> En slides: usar `#F1F5F9` como fondo de diapositivas de contenido. Las "tarjetas" se simulan con un rectángulo blanco `#FFFFFF` sobre ese fondo.

---

## Tipografía

| Nombre | Hex | Uso |
|---|---|---|
| Texto principal | `#1E293B` | Cuerpo de texto, títulos oscuros |
| Texto secundario | `#64748B` | Descripciones, subtítulos, etiquetas |
| Texto tenue | `#94A3B8` | Metadatos, placeholders, información de apoyo |

> En slides: usar `#1E293B` para titulares y `#64748B` para el cuerpo. Nunca texto negro puro (#000) — se siente demasiado pesado contra los fondos de la paleta.

---

## Estado: éxito / aprobado

| Nombre | Hex | Uso |
|---|---|---|
| Verde texto | `#059669` | Texto de badges "Cumple", valores positivos |
| Verde fondo | `#ECFDF5` | Fondo de badges verdes, alertas positivas |

> En slides: para slides de resultados positivos, KPIs que cumplen umbral, íconos de checkmark.

---

## Estado: advertencia / parcial

| Nombre | Hex | Uso |
|---|---|---|
| Ámbar texto | `#B45309` | Texto de badges "Parcial", alertas de atención |
| Ámbar fondo | `#FFFBEB` | Fondo de badges ámbar, avisos de precaución |
| Ámbar borde | `#FDE68A` | Borde de elementos de advertencia |

> En slides: para limitaciones conocidas, métricas que están en zona de monitoreo, notas de cautela.

---

## Estado: alerta / no cumple

| Nombre | Hex | Uso |
|---|---|---|
| Rojo texto | `#7F1D1D` | Texto de badge "No cumple" |
| Rojo fondo | `#FEF2F2` | Fondo de badge rojo |
| Rojo borde | `#FECACA` | Borde de elementos de alerta |

> En slides: solo para casos de incumplimiento documentado. No abusar — pierde impacto.

---

## Sidebar / elementos de navegación

| Nombre | Hex | Uso |
|---|---|---|
| Sidebar fondo | `#1A3A6B` | Fondo del panel de navegación lateral |
| Acento azul claro | `#5BA4FF` | Punto de marca, borde del ítem activo en sidebar |
| Texto inactivo | `rgba(255,255,255, 0.65)` | Links de navegación no activos |
| Hover sidebar | `rgba(255,255,255, 0.08)` | Fondo hover de items del sidebar |
| Activo sidebar | `rgba(255,255,255, 0.12)` | Fondo del ítem de navegación seleccionado |

> En slides: el `#5BA4FF` funciona bien como color de acento secundario sobre fondos oscuros navy — úsalo para subrayados, íconos o números de slide sobre slides de portada navy.

---

## Tipografía recomendada

**Fuente:** Inter (Google Fonts — gratis)
- Títulos de sección: **Inter 700** (Bold)
- Subtítulos y etiquetas: **Inter 600** (SemiBold)
- Cuerpo: **Inter 400** (Regular)
- Metadatos / secundario: **Inter 300** (Light)

> Descarga: [fonts.google.com/specimen/Inter](https://fonts.google.com/specimen/Inter)
> En PowerPoint / Keynote: instalar la fuente antes de abrir el archivo para que cargue correctamente.

---

## Combinaciones recomendadas para slides

### Diapositiva de portada
- Fondo: `#1A3A6B` (navy)
- Título: `#FFFFFF`
- Subtítulo: `rgba(255,255,255, 0.70)`
- Acento / línea decorativa: `#5BA4FF`

### Diapositiva de contenido estándar
- Fondo: `#F1F5F9`
- Tarjeta de contenido: `#FFFFFF` con borde `#E2E8F0`
- Título de sección: `#1A3A6B`
- Texto: `#1E293B`
- Texto secundario: `#64748B`

### Diapositiva de métricas / KPIs
- Número grande: `#1A3A6B`
- Etiqueta: `#64748B`
- Borde inferior del KPI: `#5BA4FF` o `#C0D4F0`

### Diapositiva de resultado positivo
- Badge / indicador: fondo `#ECFDF5`, texto `#059669`

### Diapositiva de limitación / riesgo
- Badge / indicador: fondo `#FFFBEB`, texto `#B45309`

---

*Paleta exportada desde FMA Adoption Intelligence — github-pages/style.css*
