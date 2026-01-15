# 🚀 Optimización y Mejores Prácticas (Preguntas 46-50)

---

## **46. ¿Cómo reducir el tamaño de un archivo .pbix?**

### 📖 Definición
Reducir el tamaño del archivo .pbix mejora el rendimiento, velocidad de carga y facilita el almacenamiento y distribución del reporte.

### 🔧 Técnicas de Optimización

### **1. Eliminar Columnas Innecesarias**
```
Power Query → Select Columns → Remove Other Columns
- Solo cargar columnas que realmente uses
- Eliminar IDs innecesarios después de crear relaciones
- Cada columna = más memoria
```

### **2. Optimizar Tipos de Datos**
```DAX
// ❌ MAL: Text consume más memoria
Cantidad: "100", "200" (Text)

// ✅ BIEN: Tipo correcto
Cantidad: 100, 200 (Whole Number)

Regla: Usar el tipo de dato más pequeño posible
- Whole Number en vez de Decimal si no hay decimales
- Date en vez de DateTime si no necesitas hora
- Text solo cuando sea necesario
```

### **3. Reducir Cardinalidad**
```
// Columnas con valores únicos infinitos
// ❌ MAL: 
- TransacciónID (único por fila)
- Timestamp preciso a milisegundos
- Descripciones largas únicas

// ✅ BIEN:
- Categorías agrupadas
- Fechas (no datetime)
- Códigos estandarizados
```

### **4. Filtrar Datos en Power Query**
```M
// Filtrar antes de cargar
= Table.SelectRows(Source, each [Fecha] >= #date(2023, 1, 1))
= Table.SelectRows(Source, each [Estado] <> "Cancelado")

// Cargar solo últimos 3 años en vez de 10 años históricos
```

### **5. Usar Medidas en lugar de Columnas Calculadas**
```DAX
// ❌ Columna calculada (se guarda en cada fila)
Total = Ventas[Precio] * Ventas[Cantidad]

// ✅ Medida (se calcula bajo demanda)
Total = SUMX(Ventas, [Precio] * [Cantidad])
```

### **6. Deshabilitar Auto Date/Time**
```
File → Options → Data Load:
☐ Auto date/time

Esto crea tablas ocultas por cada columna de fecha
Usar tabla de calendario propia en su lugar
```

### **7. Comprimir al Guardar**
```
File → Options → Data Load:
☑ Reduce file size by compressing the data model

Activa compresión adicional
```

### 💡 Checklist de Reducción

```
□ Eliminar columnas no usadas
□ Tipos de datos óptimos
□ Filtrar datos históricos innecesarios
□ Medidas en lugar de columnas calculadas
□ Deshabilitar Auto Date/Time
□ Eliminar tablas/consultas no cargadas sin propósito
□ Evitar columnas con alta cardinalidad
□ Considerar agregación previa en Power Query
```

### 🎯 Tip de Entrevista
**Menciona:** "Para reducir tamaño: **elimino columnas innecesarias, optimizo tipos de datos, filtro histórico innecesario**. Uso medidas en lugar de columnas calculadas. Deshabilito Auto Date/Time y uso tabla de calendario propia. Para archivos muy grandes, considero agregaciones o modelo compuesto con DirectQuery. Verifico con VertiPaq Analyzer qué tablas/columnas consumen más memoria."

---

## **47. ¿Qué es el Performance Analyzer?**

### 📖 Definición
**Performance Analyzer** es una herramienta integrada en Power BI Desktop que mide el tiempo que tarda cada visual en renderizar, identificando cuellos de botella de rendimiento.

### 🔧 Cómo Usar

**Activar:**
```
View → Performance Analyzer → Start Recording
```

**Proceso:**
1. Iniciar grabación
2. Interactuar con el reporte (cambiar slicers, navegar páginas)
3. Detener grabación
4. Analizar resultados

### **Métricas que Muestra:**

```
Visual Name: "Gráfico de Ventas"
├─ DAX query: 1,234 ms        ← Tiempo calculando medidas
├─ Visual display: 456 ms     ← Tiempo renderizando
└─ Other: 89 ms               ← Procesamiento adicional
Total: 1,779 ms
```

### **Interpretar Resultados:**

```
DAX query alto (>1 segundo):
✅ Optimizar medidas DAX
✅ Revisar contextos de filtro
✅ Usar variables para reutilizar cálculos

Visual display alto:
✅ Reducir puntos de datos en gráfico
✅ Simplificar visual
✅ Considerar agregación

Total >3 segundos:
⚠️ Usuario nota la lentitud
🔴 >5 segundos = Problema crítico
```

### 💡 Ejemplo de Uso

```
Escenario: Dashboard lento al cambiar slicer de fecha

Performance Analyzer revela:
- Visual "KPI Ventas": DAX query 4,500 ms ← PROBLEMA
- Visual "Tabla Top 10": DAX query 234 ms ← OK
- Visual "Mapa": Visual display 2,100 ms ← PROBLEMA

Acciones:
1. Optimizar medida del KPI (variables, filtros)
2. Reducir detalle del mapa (agregación)
```

### **Exportar para Análisis Profundo:**

```
Performance Analyzer → Export
- Genera JSON
- Abrir en herramientas externas (DAX Studio)
- Análisis detallado de query plans
```

### 🎯 Tip de Entrevista
**Menciona:** "Performance Analyzer es mi **primera herramienta para troubleshooting de lentitud**. Identifico qué visuales son lentos y si el problema es DAX query (optimizar medidas) o Visual display (reducir complejidad). Target: <1 seg por visual, <3 seg total. Para análisis profundo, exporto y uso DAX Studio para ver query plans. Combino con Best Practices Analyzer para identificación automática de problemas."

---

## **48. ¿Cuáles son las mejores prácticas para DAX?**

### 📖 Mejores Prácticas Esenciales

### **1. Usar Variables**
```DAX
// ❌ Sin variables (calcula 3 veces)
Margen % = DIVIDE([Total Ventas] - [Total Costo], [Total Ventas])

// ✅ Con variables (calcula 1 vez)
Margen % = 
VAR Ventas = [Total Ventas]
VAR Costo = [Total Costo]
RETURN DIVIDE(Ventas - Costo, Ventas)
```

### **2. Filtros de Columna vs FILTER**
```DAX
// ✅ RÁPIDO: Filtro de columna
Ventas México = CALCULATE([Total], Cliente[País] = "México")

// ❌ LENTO: FILTER innecesario
Ventas México = CALCULATE([Total], FILTER(Cliente, [País] = "México"))

// ✅ FILTER solo para lógica compleja
Ventas Calificadas = 
CALCULATE([Total], FILTER(Ventas, [Precio] > 100 && [Estado] = "OK"))
```

### **3. Evitar Iteradores Innecesarios**
```DAX
// ❌ SUMX innecesario
Total = SUMX(Ventas, [Monto])

// ✅ SUM directo
Total = SUM(Ventas[Monto])

// ✅ SUMX solo cuando calculas
Revenue = SUMX(Ventas, [Precio] * [Cantidad])
```

### **4. Medidas Base Reutilizables**
```DAX
// Medidas base (ocultas con _)
_Base Ventas = SUM(Ventas[Monto])
_Base Costo = SUM(Ventas[Costo])
_Base Cantidad = SUM(Ventas[Cantidad])

// Medidas compuestas
Margen = [_Base Ventas] - [_Base Costo]
Precio Promedio = DIVIDE([_Base Ventas], [_Base Cantidad])
```

### **5. Naming Conventions**
```DAX
// Convenciones claras
[Total Ventas]           ← Medida
Ventas[Monto]            ← Columna de tabla
'Tabla Calendario'[Fecha]← Tabla con espacios
_BaseVentas              ← Medida oculta/interna
```

### **6. Documentación en Medidas**
```DAX
YoY Growth = 
/* Calcula crecimiento año sobre año
   Actualizado: 2025-01-14
   Autor: Jorge Aguilar
*/
VAR Actual = [Total Ventas]
VAR LY = CALCULATE([Total Ventas], SAMEPERIODLASTYEAR('Fecha'[Date]))
RETURN DIVIDE(Actual - LY, LY)
```

### **7. Usar SELECTEDVALUE**
```DAX
// ❌ Complejo
Titulo = IF(HASONEVALUE(Producto[Nombre]), VALUES(Producto[Nombre]), "Múltiples")

// ✅ Simple
Titulo = SELECTEDVALUE(Producto[Nombre], "Múltiples")
```

### 🎯 Tip de Entrevista
**Menciona:** "Mis principios DAX: **1) Variables siempre para reutilización, 2) Filtros de columna sobre FILTER, 3) Medidas base reutilizables, 4) Naming conventions consistentes, 5) Documentar medidas complejas**. Evito iteradores innecesarios y prefiero SUM sobre SUMX cuando es posible. Uso Performance Analyzer y DAX Studio para identificar cuellos de botella."

---

## **49. ¿Cómo documentar y mantener un proyecto de Power BI?**

### 📖 Estrategia de Documentación

### **1. Documentación Técnica**

**A. README del Proyecto**
```markdown
# Dashboard de Ventas - Q1 2025

## Propósito
Dashboard ejecutivo para monitorear KPIs de ventas

## Fuentes de Datos
- SQL Server: Ventas.Transacciones
- Excel: Presupuestos_2025.xlsx
- API: Clientes desde CRM

## Actualizaciones
- Programado: Diario 6:00 AM
- Gateway: Gateway-Prod-01
- Contacto refresh: bi-team@empresa.com

## Cambios Recientes
2025-01-14: Agregado análisis por región
2025-01-10: Optimización de medidas YoY
```

**B. Diccionario de Datos**
```
Tabla: FactVentas
- VentaID: Identificador único de transacción
- FechaID: FK a DimFecha
- ClienteID: FK a DimCliente
- Monto: Valor de venta en USD

Medidas Clave:
- [Total Ventas]: SUM(Ventas[Monto])
- [YoY Growth]: Crecimiento vs año anterior (%)
```

### **2. Documentación en Power BI**

**Descriptions en Medidas:**
```
Click derecho en medida → Properties → Description:
"Calcula ventas YTD del año actual. Requiere tabla de calendario."
```

**Nombres Descriptivos:**
```
✅ BIEN:
- "Total Ventas 2025"
- "Crecimiento YoY %"
- "Top 10 Productos por Margen"

❌ MAL:
- "Medida 1"
- "Calculo Final 2"
- "Nueva Medida Test"
```

### **3. Versionado**

**Git para .pbix:**
```
Estructura:
/PowerBI-Ventas/
├── README.md
├── /dev/
│   └── Dashboard_Ventas_v1.2.pbix
├── /prod/
│   └── Dashboard_Ventas_v1.0.pbix
└── /docs/
    └── diccionario_datos.md

Commits descriptivos:
"feat: Agregado análisis regional"
"fix: Corregido cálculo YoY para Q4"
"perf: Optimizado DAX en tabla productos"
```

**Deployment Pipelines (Premium):**
```
Development → Test → Production
- Promover versiones entre ambientes
- Comparación automática de cambios
- Rollback si hay problemas
```

### **4. Mantenimiento**

**Checklist Mensual:**
```
□ Revisar refresh failures
□ Verificar performance de visuales
□ Actualizar documentación si hay cambios
□ Revisar accesos de usuarios (remover inactivos)
□ Backup de .pbix
□ Verificar uso del reporte (métricas Service)
```

**Logging de Cambios:**
```
Tabla dentro del .pbix:
Versión | Fecha      | Cambio              | Autor
1.0     | 2025-01-01 | Release inicial     | Jorge
1.1     | 2025-01-10 | Optimización DAX    | Ana
1.2     | 2025-01-14 | Análisis regional   | Jorge
```

### 🎯 Tip de Entrevista
**Menciona:** "Documento en **tres niveles: técnico (README, diccionario datos), dentro de Power BI (descriptions en medidas), y versionado (Git + deployment pipelines)**. Mantengo changelog de versiones, backup mensual, y revisión de performance. Para equipos grandes, implemento naming conventions y templates estandarizados. Uso deployment pipelines (Premium) para Dev→Test→Prod con control de cambios."

---

## **50. ¿Qué errores comunes evitar en Power BI?**

### 📖 Errores Críticos y Cómo Evitarlos

### **1. Modelado**

```
❌ Relaciones Bidireccionales Innecesarias
- Causa ambigüedad y afecta performance
✅ Usar unidireccionales + CALCULATE cuando necesario

❌ Relaciones Muchos a Muchos sin Tabla Puente
- Puede causar resultados incorrectos
✅ Crear tabla puente para resolver

❌ Snowflake Schema en lugar de Star Schema
- Más relaciones = más lento
✅ Desnormalizar dimensiones

❌ No marcar Tabla de Calendario
- Time Intelligence no funciona
✅ Marcar como "Date Table"
```

### **2. DAX**

```
❌ No usar Variables
Margen = ([Ventas] - [Costo]) / [Ventas] + [Ventas] * 0.1
// Calcula [Ventas] 3 veces!

✅ Usar Variables
Margen = 
VAR V = [Ventas]
VAR C = [Costo]
RETURN (V - C) / V + V * 0.1

❌ FILTER cuando basta filtro de columna
CALCULATE([Total], FILTER(ALL(Tabla), [Col] = "Valor"))

✅ Filtro directo
CALCULATE([Total], Tabla[Col] = "Valor")

❌ SUMX cuando basta SUM
Total = SUMX(Ventas, [Monto])

✅ SUM directo
Total = SUM(Ventas[Monto])
```

### **3. Power Query**

```
❌ No verificar Query Folding
- Todo se procesa localmente = lento

✅ Mantener folding
- Filtrar y seleccionar columnas primero
- Evitar columnas personalizadas complejas al inicio

❌ Columnas calculadas en Power Query para agregaciones
Total = SUM(...)  // ❌ NO en Power Query

✅ Medidas DAX para agregaciones
Total = SUM(Ventas[Monto])  // ✅ En DAX

❌ Duplicar consultas en lugar de referenciar
- Duplica procesamiento

✅ Usar referencias
- Reutiliza pasos sin duplicar
```

### **4. Visualización**

```
❌ Muchos Visuales por Página (>10)
- Performance afectado
✅ Máximo 6-8 visuales, usar bookmarks para alternar

❌ Pie Charts con >5 Segmentos
- Difícil de leer
✅ Barras horizontales o treemap

❌ 3D Charts
- Distorsión visual, menos preciso
✅ 2D siempre

❌ Sin Alt Text
- Accesibilidad
✅ Agregar texto alternativo en todos los visuales

❌ Colores sin Significado
✅ Paleta consistente, semántica (rojo=malo, verde=bueno)
```

### **5. Administración**

```
❌ Publicar en "Mi Espacio de Trabajo"
- No se puede compartir
✅ Workspace colaborativo

❌ No programar Refresh
- Datos desactualizados
✅ Scheduled refresh configurado + alertas

❌ Todos con Rol Admin
- Riesgo de seguridad
✅ Roles apropiados (Admin/Member/Contributor/Viewer)

❌ Gateway en PC de Usuario
- No confiable
✅ Gateway en servidor dedicado

❌ No tener Backup
- Pérdida de trabajo
✅ Backup mensual de .pbix + versionado Git
```

### **6. Performance**

```
❌ Auto Date/Time Activado
- Crea tablas ocultas por cada fecha
✅ Desactivar + usar tabla calendario propia

❌ Columnas Calculadas para Totales
- Consume memoria
✅ Medidas para agregaciones

❌ No revisar Performance Analyzer
- No sabes qué es lento
✅ Análisis regular de performance

❌ Importar Columnas Innecesarias
- Aumenta tamaño
✅ "Remove Other Columns" en Power Query
```

### **7. Seguridad**

```
❌ No implementar Row-Level Security
- Todos ven todos los datos
✅ RLS para restringir acceso por usuario

❌ Compartir con "Toda la Organización" sin necesidad
- Datos sensibles expuestos
✅ Compartir solo con usuarios necesarios

❌ Credenciales en Parámetros Visibles
- Riesgo de seguridad
✅ Credenciales en Gateway/Service settings
```

### 💡 Checklist Pre-Publicación

```
MODELADO
□ Relaciones 1:N cuando posible
□ Star schema implementado
□ Tabla calendario marcada correctamente

DAX
□ Variables en medidas complejas
□ Naming conventions consistente
□ Medidas documentadas

POWER QUERY
□ Query folding verificado
□ Solo columnas necesarias cargadas
□ Datos filtrados en origen

VISUALES
□ <8 visuales por página
□ Performance Analyzer <3 seg por visual
□ Alt text en visuales clave
□ Mobile layout creado

ADMINISTRACIÓN
□ Publicado en workspace apropiado
□ Refresh programado y probado
□ Roles de acceso configurados
□ Documentación actualizada
□ Backup realizado
```

### 🎯 Tip de Entrevista
**Menciona:** "Los errores más comunes: **1) Relaciones incorrectas (bidireccionales innecesarias, M:M sin tabla puente), 2) DAX ineficiente (sin variables, FILTER innecesario), 3) No verificar query folding, 4) Demasiados visuales por página, 5) Auto Date/Time activo, 6) No implementar RLS**. Mi proceso: checklist pre-publicación, Performance Analyzer, Best Practices Analyzer, peer review de DAX crítico, y testing con usuarios antes de producción."

---

## 🎓 **Resumen Final de Mejores Prácticas**

### **Rendimiento**
✅ Variables en DAX para reutilización  
✅ Filtros de columna sobre FILTER  
✅ Query folding en Power Query  
✅ Medidas sobre columnas calculadas  
✅ Performance Analyzer regular  

### **Modelado**
✅ Star schema sobre snowflake  
✅ Relaciones 1:N cuando posible  
✅ Tabla de calendario marcada  
✅ Tipos de datos óptimos  
✅ Solo columnas necesarias  

### **Gobernanza**
✅ Documentación completa  
✅ Versionado (Git + pipelines)  
✅ Naming conventions  
✅ Roles apropiados  
✅ RLS implementado  

### **Mantenimiento**
✅ Backup mensual  
✅ Refresh programado + alertas  
✅ Monitoreo de performance  
✅ Revisión de accesos  
✅ Changelog actualizado  

---

## 📚 **Preparación para Entrevistas - Puntos Clave**

**Conceptos que DEBES dominar:**
1. **Contexto DAX** (filter + row) - Pregunta más común
2. **CALCULATE** - Función más importante
3. **Query Folding** - Optimización crítica
4. **Star Schema** - Modelado estándar
5. **Gateway** - Conexión on-premises
6. **Pro vs Premium** - Licenciamiento
7. **RLS** - Seguridad básica
8. **Time Intelligence** - Análisis temporal
9. **Dataflows** - Arquitecturas escalables
10. **Performance Analyzer** - Troubleshooting

**Siempre menciona:**
- ✅ Experiencia REAL con ejemplos específicos
- ✅ Trade-offs en decisiones técnicas
- ✅ Impacto en performance/costo
- ✅ Mejores prácticas aplicadas
- ✅ Solución de problemas reales

**Evita:**
- ❌ Respuestas genéricas de manual
- ❌ "Nunca he usado eso"
- ❌ Solo teoría sin práctica
- ❌ No conocer limitaciones

---

**💡 Consejo Final para Entrevistas**: Relaciona cada concepto técnico con **casos de uso reales y business value**. No basta saber qué es CALCULATE - explica CÓMO lo usaste para resolver un problema específico de negocio y qué impacto tuvo. Los entrevistadores buscan experiencia práctica, capacidad de resolver problemas, y entendimiento de trade-offs técnicos. **¡Buena suerte! 🚀**