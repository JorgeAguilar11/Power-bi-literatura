# 🎨 Visualización y Diseño (Preguntas 36-40)

---

## **36. ¿Cuáles son las mejores prácticas de diseño de dashboards?**

### 📖 Definición
Las **mejores prácticas de diseño** son principios y técnicas que maximizan la efectividad de los dashboards, asegurando que comuniquen información de manera clara, rápida y accionable para los usuarios.

### 🔧 Explicación Práctica

### **1. Jerarquía Visual y Estructura**

**Layout en formato F o Z:**
- Los usuarios escanean en patrón F (izquierda → derecha, arriba → abajo)
- KPIs principales arriba a la izquierda
- Detalles en la parte inferior

**Estructura típica:**
```
┌─────────────────────────────────────┐
│  KPI 1    KPI 2    KPI 3    KPI 4  │  ← Métricas principales
├─────────────────────────────────────┤
│ Filtros / Slicers                   │  ← Controles
├──────────────────┬──────────────────┤
│                  │                  │
│  Gráfico         │  Gráfico         │  ← Visualizaciones
│  Principal       │  Secundario      │     principales
│                  │                  │
├──────────────────┴──────────────────┤
│  Tabla de Detalles                  │  ← Detalles
└─────────────────────────────────────┘
```

### **2. Principios de Diseño Visual**

**Regla del 5 Segundos:**
- El mensaje principal debe ser claro en 5 segundos
- Evita información innecesaria

**Espaciado y Alineación:**
- Usa márgenes consistentes (10-20px)
- Alinea elementos en grilla
- Agrupa elementos relacionados

**Paleta de Colores:**
```
✅ BIEN:
- 2-3 colores principales (marca corporativa)
- Gris para elementos secundarios
- Rojo para alertas, verde para positivo
- Alto contraste para accesibilidad

❌ MAL:
- Colores brillantes sin propósito
- Más de 5 colores diferentes
- Bajo contraste (difícil de leer)
```

### **3. Elección de Visuales**

| **Para mostrar** | **Usar** | **Evitar** |
|------------------|----------|------------|
| Tendencias temporales | Gráfico de líneas | Pie chart |
| Comparación de categorías | Barras horizontales | 3D charts |
| Composición/porcentajes | Stacked bar, Treemap | Pie con +5 segmentos |
| Valores únicos (KPI) | Card, KPI visual | Tabla de 1 fila |
| Distribución | Histograma, Box plot | Barras |
| Relación entre 2 variables | Scatter plot | Barras agrupadas |

### **4. Mejores Prácticas de KPIs**

```DAX
// KPI con contexto visual
Ventas KPI = 
VAR Actual = [Total Ventas]
VAR Target = [Presupuesto]
VAR Varianza = DIVIDE(Actual - Target, Target)
VAR Icono = 
    SWITCH(
        TRUE(),
        Varianza >= 0.1, "▲ ",
        Varianza <= -0.1, "▼ ",
        "■ "
    )
RETURN
    Icono & FORMAT(Actual, "$#,##0K")
```

**Diseño de KPI:**
- Número grande y visible
- Indicador de tendencia (↑↓)
- Comparativa (vs período anterior, vs target)
- Color según rendimiento

### **5. Optimización de Performance**

**Reducir Carga Visual:**
- Máximo 6-8 visuales por página
- Usar bookmarks para alternar vistas
- Evitar tablas con miles de filas visibles

**Interactividad Inteligente:**
- Slicers principales en todas las páginas
- Sync slicers entre páginas relacionadas
- Drill-through para detalles

### **6. Accesibilidad**

✅ **Implementar:**
- Alt text en todos los visuales
- Orden de tabulación lógico
- Suficiente contraste de color (WCAG 2.1)
- Tamaño de fuente mínimo 10pt
- Evitar solo color para transmitir información

### **7. Mobile First**

- Crear layout para móvil separado
- KPIs principales arriba
- Máximo 3-4 visuales por página móvil
- Botones grandes (touch-friendly)

### 💡 Ejemplo de Checklist Pre-Publicación

```
□ ¿Se entiende el mensaje en 5 segundos?
□ ¿Los KPIs están arriba/izquierda?
□ ¿Hay título claro en cada página?
□ ¿Los slicers están agrupados y visibles?
□ ¿Se usó paleta de colores consistente?
□ ¿Hay tooltips explicativos?
□ ¿Performance <3 segundos por visual?
□ ¿Funciona en móvil?
□ ¿Hay alt text en visuales clave?
□ ¿Se probó con datos reales del cliente?
```

### 🎯 Tip de Entrevista
**Menciona:** "Sigo el principio de **'menos es más'**: cada visual debe tener un propósito claro. Uso la **regla del 5 segundos** - el usuario debe captar el mensaje principal rápidamente. Para paleta de colores, me adhiero a la marca corporativa y uso colores semánticos (rojo = malo, verde = bueno). Diseño con **jerarquía visual**: KPIs arriba, detalles abajo. Siempre creo un layout para **móvil** porque muchos ejecutivos consumen reportes en tablets. Y critical: **optimizo performance** - máximo 8 visuales por página y uso bookmarks para alternar vistas."

---

## **37. ¿Qué son los bookmarks y cómo usarlos?**

### 📖 Definición
Los **bookmarks (marcadores)** capturan el estado actual de una página de reporte (filtros, visuales visibles, selecciones) permitiendo crear experiencias interactivas como botones de navegación, alternar vistas, y storytelling guiado.

### 🔧 Explicación Práctica

**Los bookmarks guardan:**
- ✅ Filtros y slicers aplicados
- ✅ Visibilidad de visuales
- ✅ Spotlight y focus mode
- ✅ Cross-highlighting entre visuales
- ❌ NO guardan cambios de datos

### **Casos de Uso Principales:**

### **1. Navegación entre Páginas**
```
┌──────────────────────────┐
│ [Ventas] [Productos]     │  ← Botones con bookmarks
│ [Clientes] [Regiones]    │
└──────────────────────────┘

Crear:
1. View → Bookmarks → Add
2. Nombrar: "Nav_Ventas"
3. Insertar botón
4. Action → Bookmark → "Nav_Ventas"
```

### **2. Alternar entre Vistas (Vista Toggle)**
```
Escenario: Cambiar entre Tabla y Gráfico

Setup:
1. Crear dos visuales en el mismo espacio (superpuestos)
   - Visual A: Tabla
   - Visual B: Gráfico
2. Bookmark 1: "Vista_Tabla"
   - Tabla visible, Gráfico oculto
3. Bookmark 2: "Vista_Gráfico"
   - Tabla oculto, Gráfico visible
4. Botones:
   - "Ver Tabla" → Bookmark: Vista_Tabla
   - "Ver Gráfico" → Bookmark: Vista_Gráfico
```

### **3. Limpiar Filtros**
```DAX
// Bookmark que captura estado sin filtros
Nombre: "Reset_Filtros"
- Desmarcar todos los slicers
- Guardar bookmark
- Botón "Reset" → Este bookmark
```

### **4. Storytelling / Presentación Guiada**
```
Bookmarks para narración:
1. "Inicio" - Vista general
2. "Problema" - Destaca área problemática
3. "Análisis" - Drill-down en detalle
4. "Solución" - Recomendaciones

Botones: [Anterior] [Siguiente]
```

### **5. Escenarios What-If Visuales**
```
Bookmarks con diferentes combinaciones:
- "Escenario_Optimista"
- "Escenario_Base"
- "Escenario_Pesimista"

(Cada uno con diferentes parámetros seleccionados)
```

### 💡 Ejemplo Paso a Paso: Menu Interactivo

**Crear menú de navegación con bookmarks:**

```
PASO 1: Diseñar páginas
- Página: "Dashboard_Ventas"
- Página: "Dashboard_Marketing"
- Página: "Dashboard_Finanzas"

PASO 2: Crear bookmarks de navegación
View → Bookmarks:
- Bookmark: "Ir_Ventas" (en página Ventas)
- Bookmark: "Ir_Marketing" (en página Marketing)
- Bookmark: "Ir_Finanzas" (en página Finanzas)

PASO 3: Crear página de inicio con menú
Insertar 3 botones:
┌────────────┐
│   VENTAS   │ → Action: Bookmark → "Ir_Ventas"
├────────────┤
│ MARKETING  │ → Action: Bookmark → "Ir_Marketing"
├────────────┤
│  FINANZAS  │ → Action: Bookmark → "Ir_Finanzas"
└────────────┘

PASO 4: Agregar botón "Home" en cada página
Bookmark: "Ir_Home" (página de inicio)
Botón en cada dashboard → Action: "Ir_Home"
```

### **Configuración Avanzada de Bookmarks**

**Opciones al crear bookmark:**
```
Bookmark Properties:
□ Data - Captura filtros y slicers
□ Display - Captura visibilidad de objetos
□ Current page - Incluye navegación a página
□ All visuals - Afecta todos los visuales
□ Selected visuals - Solo visuales seleccionados
```

**Best practices:**
```
✅ Nombra bookmarks descriptivamente: "Vista_Tabla_Productos"
✅ Agrupa bookmarks relacionados
✅ Usa "Update" para modificar bookmark existente
✅ Combina con Selection Pane para controlar visibilidad
✅ Prueba la experiencia completa de usuario
```

### **Bookmarks + Selection Pane**

```
Selection Pane (View → Selection):
Controla visibilidad de cada objeto

Workflow:
1. Ocultar/mostrar visuales en Selection Pane
2. Crear bookmark
3. El bookmark captura esa configuración
```

### **Trucos Avanzados:**

```
1. Overlay Menus
   - Crear panel flotante con botones
   - Bookmark para mostrar/ocultar menú

2. Drill-Down Simulado
   - Bookmark nivel 1: Vista resumen
   - Bookmark nivel 2: Vista detalle
   - Botones para navegar entre niveles

3. Filtros Pre-aplicados
   - Bookmark con región = "Norte"
   - Botón "Ver Norte" activa ese filtro

4. Dashboard Dinámico
   - Alternar entre diferentes conjuntos de KPIs
   - Sin crear múltiples páginas
```

### 🎯 Tip de Entrevista
**Menciona:** "Los bookmarks son fundamentales para **experiencias interactivas avanzadas**. Los uso principalmente para tres cosas: **navegación** (menús personalizados sin mostrar todas las pestañas), **alternar vistas** (cambiar entre tabla/gráfico sin duplicar espacio), y **storytelling** (guiar al usuario por hallazgos clave). La combinación de bookmarks con **Selection Pane** permite controlar exactamente qué se muestra. Un caso avanzado: crear un dashboard que parece tener múltiples páginas pero en realidad es una sola con bookmarks que muestran/ocultan grupos de visuales - esto optimiza el performance."

---

## **38. ¿Cómo crear drill-through pages?**

### 📖 Definición
**Drill-through** permite al usuario hacer clic derecho en un dato específico de un visual y navegar a una página de detalle filtrada automáticamente por ese valor, proporcionando análisis contextual profundo.

### 🔧 Explicación Práctica

**Diferencia conceptual:**
- **Drill-down**: Navegar jerarquías dentro del mismo visual (Año → Trimestre → Mes)
- **Drill-through**: Saltar a otra página con contexto del elemento seleccionado

### **Cómo Funcionar Drill-Through:**

```
Página Resumen: Ventas por Producto
Usuario: Click derecho en "Laptop XYZ"
Menú contextual: "Drill through → Detalle Producto"
→ Navega a página "Detalle Producto"
→ Automáticamente filtrada por "Laptop XYZ"
```

### 💡 Ejemplo Paso a Paso: Drill-Through de Productos

**PASO 1: Crear página de destino (Detalle)**

```
1. Crear nueva página: "Detalle_Producto"
2. Diseñar dashboard de detalle:
   - KPIs específicos del producto
   - Gráfico de ventas por mes
   - Tabla de clientes que compraron
   - Distribución geográfica
   - Reviews y ratings
```

**PASO 2: Configurar campo de drill-through**

```
En página "Detalle_Producto":

1. Panel "Drill through" (lado derecho)
2. Arrastrar campo(s) a "Add drill-through fields here"
   - Arrastrar: Producto[NombreProducto]

Power BI añade automáticamente:
- Botón "Back" (regresar)
- Filtro por el producto seleccionado
```

**PASO 3: Usar desde página origen**

```
En página "Dashboard_Ventas":

1. Tener visual con Producto[NombreProducto]
2. Usuario: Click derecho en un producto
3. Aparece: "Drill through → Detalle_Producto"
4. Clic → Navega con filtro aplicado
```

### **Configuración Avanzada:**

### **1. Múltiples Campos de Drill-Through**
```
Drill-through fields:
- Producto[NombreProducto]
- Producto[Categoría]

Resultado:
- Si usuario selecciona producto específico → filtra por producto
- Si selecciona solo categoría → filtra por categoría
- Ambos seleccionados → filtra por ambos
```

### **2. Cross-Report Drill-Through**
```
Permite drill-through entre diferentes reportes:

Configuración (Power BI Service):
1. Workspace con múltiples reportes
2. En página destino: Marcar "Cross-report"
3. Configurar campos de drill-through
4. Usuarios pueden drill desde cualquier reporte del workspace
```

### **3. Mantener Filtros Adicionales**
```
Drill through settings:
□ Keep all filters

✅ Marcado: Mantiene otros filtros (fecha, región, etc.)
❌ Desmarcado: Solo aplica filtro de drill-through
```

### **4. Botón Personalizado de Regreso**
```
Por defecto, Power BI añade botón "Back"

Personalizar:
1. Eliminar botón automático
2. Insertar botón custom
3. Action → Back → Type: Back
4. Diseñar según branding
```

### **Casos de Uso Comunes:**

### **Ejemplo 1: Análisis de Clientes**
```
Página Resumen: Top 10 Clientes

Drill-through:
- Campo: Cliente[NombreCliente]
- Página destino: "Detalle_Cliente"
  - Histórico de compras
  - Productos favoritos
  - Tendencia de gasto
  - Información de contacto
```

### **Ejemplo 2: Análisis Geográfico**
```
Página Resumen: Mapa de ventas

Drill-through:
- Campo: Geografia[Ciudad]
- Página destino: "Detalle_Ciudad"
  - Ventas por tienda
  - Productos más vendidos en esa ciudad
  - Comparativa con otras ciudades
  - Demografía
```

### **Ejemplo 3: Análisis de Campañas**
```
Página Resumen: Performance de Campañas

Drill-through:
- Campo: Marketing[CampañaID]
- Página destino: "Detalle_Campaña"
  - ROI detallado
  - Breakdown por canal
  - Conversión por etapa
  - Timeline de resultados
```

### **Mejores Prácticas:**

```
✅ Usar drill-through para:
- Detalles que no caben en página principal
- Análisis contextual profundo
- Mantener páginas principales limpias

✅ En página de drill-through incluir:
- Contexto claro (título dinámico con selección)
- Botón back visible
- Información relevante al filtro aplicado
- Breadcrumb o indicador de navegación

✅ Nombrar páginas descriptivamente:
- "Detalle_Producto" (claro)
- vs "Página 2" (confuso)

✅ Considerar:
- Ocultar página de drill-through del tab navigator
  (Click derecho en pestaña → Hide page)
- Los usuarios solo acceden vía drill-through
```

### **Título Dinámico en Página Drill-Through:**
```DAX
// Medida para título dinámico
Título Dinámico = 
"Análisis de: " & 
SELECTEDVALUE(
    Producto[NombreProducto],
    "Múltiples Productos"
)

// Usar en text box o card visual
```

### 🎯 Tip de Entrevista
**Menciona:** "Drill-through es esencial para **mantener dashboards limpios sin sacrificar profundidad**. La página principal muestra el resumen, y drill-through lleva a análisis detallado. Lo configuro arrastrando campos al panel de drill-through y diseñando la página destino con información relevante al contexto. Best practice: ocultar páginas de drill-through del navegador para que usuarios solo accedan vía contexto. También uso **títulos dinámicos** con `SELECTEDVALUE()` para mostrar claramente qué se está analizando. Cross-report drill-through es poderoso en organizaciones grandes donde múltiples reportes están relacionados."

---

## **39. ¿Qué son los tooltips personalizados?**

### 📖 Definición
Los **tooltips (información sobre herramientas)** son ventanas emergentes que aparecen al pasar el mouse sobre un visual. Los **tooltips personalizados** son páginas de reporte completas diseñadas específicamente para aparecer como tooltip, permitiendo mostrar información rica y contextual sin ocupar espacio en el dashboard.

### 🔧 Explicación Práctica

**Tooltips por defecto vs Personalizados:**

```
Default Tooltip:
Producto: Laptop XYZ
Ventas: $50,000
↓
Personalizado Tooltip:
┌──────────────────────────┐
│ Laptop XYZ               │
│ ──────────────────────── │
│ Ventas:    $50,000       │
│ vs LY:     +15% ↑        │
│ Margen:    $12,500       │
│ Stock:     45 unidades   │
│ [Mini gráfico tendencia] │
└──────────────────────────┘
```

### 💡 Ejemplo Paso a Paso: Crear Tooltip Personalizado

**PASO 1: Crear página de tooltip**

```
1. Crear nueva página: "Tooltip_Producto"

2. Format → Canvas Settings:
   - Type: "Tooltip"
   - Size: Small (320x240) o Medium (480x360)

3. Diseñar contenido (ejemplo):
   ┌─────────────────────┐
   │ Producto Dinámico   │ ← Título con SELECTEDVALUE
   ├─────────────────────┤
   │ KPI: Ventas         │
   │ KPI: Margen %       │
   │ Mini gráfico línea  │
   │ Icono indicador     │
   └─────────────────────┘
```

**PASO 2: Configurar campos de contexto**

```
En página tooltip:

Panel "Tooltips":
- Add fields: Producto[ProductoID] o [NombreProducto]

Esto asegura que el tooltip se filtre por el producto
sobre el que el usuario pasa el mouse
```

**PASO 3: Aplicar en visual origen**

```
En página principal:

1. Seleccionar visual (ej: gráfico de barras de ventas)
2. Format → General → Tooltips
3. Type: "Report page"
4. Page: "Tooltip_Producto"

Listo! Al pasar mouse sobre barras, aparece tooltip personalizado
```

### **Elementos Comunes en Tooltips Personalizados:**

### **1. Título Dinámico**
```DAX
Título Tooltip = 
VAR Seleccionado = SELECTEDVALUE(Producto[NombreProducto])
RETURN
IF(
    HASONEVALUE(Producto[NombreProducto]),
    Seleccionado,
    "Múltiples Productos"
)
```

### **2. KPIs Contextuales**
```DAX
// Medidas que funcionan en tooltip
Ventas Producto = [Total Ventas]  // Se filtra automáticamente
YoY Growth = [YoY Growth %]       // Ya definida previamente
Margen = [Total Margen]
Ranking = 
    RANKX(
        ALL(Producto),
        [Total Ventas],
        ,
        DESC
    )
```

### **3. Mini Visuales**
```
Efectivos en tooltips:
- Line chart pequeño (tendencia)
- Gauge (progress vs target)
- Cards con iconos
- Small bar chart (top 3 items)

Evitar:
- Tablas grandes
- Gráficos complejos
- Múltiples visuales (mantener simple)
```

### **Casos de Uso Avanzados:**

### **Ejemplo 1: Tooltip de Producto en Ventas**
```
Página principal: Ventas por Categoría (gráfico de barras)

Tooltip muestra al hover sobre "Electrónica":
- Total ventas de electrónica
- Top 3 productos de la categoría
- Tendencia últimos 6 meses
- % del total de ventas
```

### **Ejemplo 2: Tooltip Geográfico en Mapa**
```
Página principal: Mapa de ventas por país

Tooltip muestra al hover sobre un país:
- Nombre del país
- Ventas totales
- # de clientes
- Tasa de crecimiento
- Mini gráfico de ciudades principales
```

### **Ejemplo 3: Tooltip de Cliente en Tabla**
```
Página principal: Tabla de clientes

Tooltip muestra al hover sobre cliente:
- Información de contacto
- Total compras lifetime
- Última compra (fecha)
- Producto favorito
- Segmento (Premium/Regular)
```

### **Configuración Avanzada:**

### **1. Múltiples Tooltips para Diferentes Visuales**
```
Crear páginas tooltip separadas:
- "Tooltip_Producto" → Para gráficos de producto
- "Tooltip_Cliente" → Para gráficos de cliente
- "Tooltip_Geografía" → Para mapas

Cada visual usa el tooltip apropiado
```

### **2. Tooltips Condicionales**
```DAX
// Mostrar diferentes información según contexto
Tooltip Info = 
SWITCH(
    TRUE(),
    [Total Ventas] > 100000, "Cliente Premium - Contactar!",
    [Total Ventas] > 50000, "Cliente Regular",
    "Cliente Nuevo"
)
```

### **3. Tamaños de Tooltip**
```
Canvas Settings → Size:
- Small: 320x240 (rápido, info concisa)
- Medium: 480x360 (más detalles)
- Custom: Define dimensiones exactas

Recomendación: Small para dashboards densos
```

### **Mejores Prácticas:**

```
✅ Diseño:
- Fondo contrastante pero no distractor
- 2-4 elementos máximo (evitar sobrecarga)
- Fuentes legibles (mínimo 10-11pt)
- Espacio en blanco adecuado

✅ Contenido:
- Información complementaria (no redundante)
- Métricas que no caben en visual principal
- Contexto adicional útil
- Comparativas (vs LY, vs target)

✅ Performance:
- Evitar muchas medidas complejas
- No usar tablas grandes
- Diseño simple (carga rápida)

✅ Experiencia:
- Consistencia en múltiples tooltips
- Información relevante al contexto
- No duplicar lo que ya se ve en el visual
```

### **Tooltip con Imágenes:**
```
Caso: Tooltip de producto con imagen

1. Tener columna: Producto[URLImagen]
2. Visual: Image
3. Configurar: Image URL = Producto[URLImagen]
4. En tooltip page, imagen se filtra automáticamente

Resultado: Al hover sobre producto, se muestra su imagen
```

### **Desactivar Tooltips:**
```
Si no quieres tooltip en un visual específico:

Format → General → Tooltips
Type: "None"
```

### 🎯 Tip de Entrevista
**Menciona:** "Los tooltips personalizados son **una forma elegante de añadir profundidad sin desorden**. Los uso para mostrar información contextual que no cabe en el dashboard principal - por ejemplo, en un mapa de ventas, el tooltip muestra detalles del país: top productos, tendencia, KPIs clave. La clave es **mantenerlos simples y rápidos**: 2-3 KPIs, un mini gráfico, título dinámico. Configuro el tipo de canvas como 'Tooltip' y tamaño Small para performance. Un caso avanzado: uso diferentes tooltips personalizados para diferentes tipos de visuales en el mismo reporte, cada uno mostrando información relevante al contexto."

---

## **40. ¿Cómo funcionan los slicers y filtros?**

### 📖 Definición
**Slicers** y **filtros** son mecanismos para controlar qué datos se muestran en los visuales:

- **Slicers**: Controles visuales en el canvas que los usuarios pueden interactuar directamente
- **Filtros**: Controles en el panel lateral (Filters pane) menos visibles pero más poderosos

### 🔧 Explicación Práctica

### **Niveles de Filtros en Power BI:**

```
┌────────────────────────────────┐
│ Filtros de VISUAL              │  ← Afecta solo UN visual
├────────────────────────────────┤
│ Filtros de PÁGINA              │  ← Afecta todos los visuales de la página
├────────────────────────────────┤
│ Filtros de REPORTE             │  ← Afecta todas las páginas
├────────────────────────────────┤
│ Drill-through                  │  ← Filtros de contexto específico
└────────────────────────────────┘

Prioridad: Visual > Página > Reporte
```

### **SLICERS:**

### **Tipos de Slicers:**

```
1. List (Vertical/Horizontal)
┌──────────┐
│ ☐ Norte  │
│ ☐ Sur    │
│ ☐ Este   │
│ ☐ Oeste  │
└──────────┘

2. Dropdown
┌──────────────────┐
│ Región ▼         │
└──────────────────┘

3. Between (Rango numérico)
┌──────────────────┐
│ Precio           │
│ [100] ─── [500] │
└──────────────────┘

4. Before/After (Fechas)
┌──────────────────┐
│ Fecha            │
│ Before 2025-12-31│
└──────────────────┘

5. Relative Date
┌──────────────────┐
│ Últimos 30 días  │
│ This Month       │
│ Last Quarter     │
└──────────────────┘

6. Relative Time (con horas)
┌──────────────────┐
│ Últimas 24 horas │
└──────────────────┘

7. Tile (Visual con imágenes)
┌─────┬─────┬─────┐
│ 🖥️  │ 📱  │ ⌚   │
│ PC  │Phone│Watch│
└─────┴─────┴─────┘
```

### **Configuración Avanzada de Slicers:**

### **1. Slicer Sync (Sincronización)**
```
View → Sync Slicers

Permite:
- Sincronizar un slicer entre múltiples páginas
- Selección en una página afecta otras páginas

Configuración:
┌─────────────────────────────────┐
│ Slicer: Región                  │
├──────────┬──────────┬───────────┤
│ Página   │ Sync?    │ Visible?  │
├──────────┼──────────┼───────────┤
│ Ventas   │ ✓        │ ✓         │
│ Marketing│ ✓        │ ✓         │
│ Finanzas │ ✓        │ □         │ ← Sincroniza pero no se muestra
└──────────┴──────────┴───────────┘
```

### **2. Comportamiento de Selección**
```
Format → Selection → Selection:
- Single select (solo uno a la vez)
- Multi-select (Ctrl+Click)
  - Con: Ctrl + Click
  - Sin: Click múltiple directo

Show "Select All":
- ☑ Permite seleccionar todos los items
```

### **3. Filtro de Búsqueda**
```
Format → Slicer settings:
☑ Show search box

Útil para:
- Listas largas de productos
- Nombres de clientes
- Códigos
```

### **4. Interacciones de Slicers**
```
Format → Edit interactions

Controla cómo slicer afecta cada visual:
- Filter (filtra normalmente)
- Highlight (resalta sin filtrar)
- None (no afecta)

Ejemplo:
Slicer "Región":
- Gráfico ventas → Filter ✓
- KPI total global → None ✗ (no se filtra)
- Mapa → Filter ✓
```

### **FILTROS (Filters Pane):**

### **Tipos de Filtros:**

### **1. Basic Filtering**
```
Lista de valores:
☑ México
☐ USA
☐ Canadá
```

### **2. Advanced Filtering**
```
Operadores:
- Contains / Does not contain
- Starts with / Ends with
- Is / Is not
- Is blank / Is not blank

Ejemplo:
Show items when value:
"contains" "Premium"
```

### **3. Top N Filtering**
```
Show items: Top 10
By value: [Total Ventas]
```

### **4. Relative Date Filtering**
```
Show items when value:
"is in the last" 30 "days"

Options:
- Days, Weeks, Months, Years
- Last, This, Next
- Calendar vs Fiscal
```

### **5. Advanced Date Filtering**
```
- Is on or after: 2025-01-01
- Is before: 2025-12-31
- Between: 2025-01-01 AND 2025-03-31
```

### 💡 Ejemplo: Dashboard de Ventas con Filtros y Slicers

```
┌──────────────────────────────────────────────────────┐
│ SLICERS (Visibles):                                  │
│ [Año: 2025▼] [Región: ☐N ☐S ☐E ☐O] [Búsqueda: __] │
├──────────────────────────────────────────────────────┤
│ KPIs y Visuales (filtrados por slicers)             │
└──────────────────────────────────────────────────────┘

FILTROS DE PÁGINA (Panel lateral):
Visual filters: (vacío)
Page filters:
- Estado ≠ "Cancelado"  ← Oculto del usuario
- Empleado = Vendedor Activo

Report filters:
- Empresa = "Corporativo"  ← Aplica a todo el reporte
```

### **Mejores Prácticas:**

### **Cuándo usar Slicers vs Filtros:**

| **Usar Slicers** | **Usar Filtros Panel** |
|------------------|------------------------|
| Usuario necesita interacción visible | Filtros fijos que no cambian |
| Filtros comunes (fecha, región) | Limitar datos por seguridad |
| Necesita verse qué está filtrado | Filtros técnicos (Estado ≠ Error) |
| Pocos valores (2-20 items) | Filtros de muchos valores |

### **Organización de Slicers:**
```
✅ BIEN:
- Agrupados en la parte superior
- Alineados y con espacio consistente
- Etiquetas claras
- Colores acordes al tema

❌ MAL:
- Dispersos por todo el dashboard
- Diferentes tamaños sin razón
- Colores llamativos que distraen
- Sin indicar qué está filtrado
```

### **Optimización de Performance:**
```
✅ Reducir items en slicers:
- Usar dropdown para listas largas (>20 items)
- Filtrar slicer con filtros de página
- Considerar jerarquías

✅ Limitar sincronización:
- Sync slicers solo en páginas necesarias
- Desmarcar páginas donde no se usa

✅ Usar filtros de página para:
- Restricciones permanentes
- Filtros que no cambian frecuentemente
```

### **Slicers Avanzados:**

### **1. Hierarchical Slicer (Jerarquía)**
```
Producto
├─ Electrónica
│  ├─ Computadoras
│  └─ Móviles
└─ Ropa
   ├─ Hombre
   └─ Mujer

Permite drill-down en el slicer mismo
```

### **2. Numeric Range Slicer**
```DAX
// Crear bins para rango
Rango Precio = 
SWITCH(
    TRUE(),
    Producto[Precio] < 100, "0-100",
    Producto[Precio] < 500, "100-500",
    Producto[Precio] < 1000, "500-1K",
    "1K+"
)

Usar en slicer para filtrado por rangos
```

### **3. Dynamic Slicer Titles**
```DAX
Título Dinámico = 
"Región Seleccionada: " & 
IF(
    ISFILTERED(Geografia[Región]),
    CONCATENATEX(VALUES(Geografia[Región]), [Región], ", "),
    "Todas"
)
```

### 🎯 Tip de Entrevista
**Menciona:** "Uso **slicers para filtros que usuarios necesitan cambiar frecuentemente** (fecha, región, categoría) y **filtros de panel para restricciones técnicas** o de seguridad que no deberían modificarse. Key: **sync slicers** entre páginas para consistencia - el usuario filtra una vez y afecta todas las vistas relevantes. Para performance, uso dropdown en listas largas y limito la sincronización solo a páginas necesarias. Configuración importante: **Edit interactions** para controlar qué visuales son afectados por cada slicer - por ejemplo, un KPI de 'Total Global' no debe filtrarse por región. También uso relative date slicers ('Últimos 30 días') para análisis dinámicos."

---

## 🎓 **Resumen de Conceptos Clave de Visualización y Diseño**

✅ **Jerarquía visual**: KPIs arriba, detalles abajo, regla del 5 segundos  
✅ **Paleta consistente**: 2-3 colores corporativos, alto contraste  
✅ **Bookmarks**: Navegación, alternar vistas, storytelling  
✅ **Drill-through**: Detalles contextuales sin saturar dashboard  
✅ **Tooltips personalizados**: Información rica sin ocupar espacio  
✅ **Slicers visibles** para interacción frecuente  
✅ **Filtros panel** para restricciones técnicas  
✅ **Sync slicers** para consistencia entre páginas  
✅ **Edit interactions** para control fino  
✅ **Mobile layout** para ejecutivos en movimiento  

---

**💡 Consejo Final**: El diseño efectivo de dashboards combina **estética profesional con usabilidad intuitiva**. En entrevistas, demuestra que entiendes que cada elemento visual tiene un propósito - no se trata de llenar espacio sino de comunicar insights claramente. Los entrevistadores valoran experiencia con **interactividad avanzada** (bookmarks, drill-through, tooltips) y comprensión de **principios de UX/UI** aplicados a BI. Siempre menciona que diseñas con el usuario final en mente y consideras performance y accesibilidad.