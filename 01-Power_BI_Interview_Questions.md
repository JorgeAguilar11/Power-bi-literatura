# 🎯 POWER BI - GUÍA DE ENTREVISTA TÉCNICA

## 📋 50 Preguntas + Respuestas Completas

> **Preparación profesional** para entrevistas técnicas de Power BI  
> **Nivel:** Intermedio a Senior  
> **Enfoque:** Experiencia real y casos prácticos

---

## 📊 PREGUNTAS Y RESPUESTAS

### 1. ¿Qué es Power BI?

**Definición:**  
Power BI es una plataforma de inteligencia de negocios (BI) de Microsoft que permite conectar, transformar, modelar y visualizar datos de múltiples fuentes para crear informes y dashboards interactivos que facilitan la toma de decisiones basada en datos.

**Explicación práctica:**  
En entornos empresariales, Power BI actúa como el puente entre datos dispersos (Excel, SQL Server, APIs, SharePoint, etc.) y decisiones de negocio. Permite a usuarios técnicos y no técnicos analizar información sin necesidad de programación compleja.

**Componentes principales:**
- **Power BI Desktop:** Aplicación de escritorio para desarrollo
- **Power BI Service:** Plataforma cloud para publicación y colaboración
- **Power BI Mobile:** Apps para consumo móvil
- **Power BI Report Server:** Solución on-premise (opcional)

**Ejemplo práctico:**  
Una empresa puede conectar sus datos de ventas (SQL Server), marketing (Google Analytics) y finanzas (Excel) en un solo modelo para analizar ROI en tiempo real.

**💡 Tip de entrevista:**  
Menciona que Power BI es parte del ecosistema Microsoft (Office 365, Azure, Teams) y destaca su capacidad de self-service BI, diferenciándolo de herramientas tradicionales como Tableau o Qlik.

---

### 2. Diferencia entre Power BI Desktop y Power BI Service

**Definición:**  
- **Power BI Desktop:** Aplicación de Windows gratuita para crear y diseñar informes
- **Power BI Service:** Plataforma web en la nube para publicar, compartir y consumir informes

**Explicación práctica:**  

| Característica | Power BI Desktop | Power BI Service |
|---------------|------------------|------------------|
| **Propósito** | Desarrollo y diseño | Publicación y colaboración |
| **Modelado de datos** | Completo (Power Query, DAX, modelos) | Limitado (solo edición de datasets publicados) |
| **Visualizaciones** | Todas disponibles | Solo consumo y edición básica |
| **Actualización de datos** | Manual | Programada (con Gateway) |
| **Compartir** | No disponible | Workspaces, apps, links públicos |
| **Costo** | Gratuito | Requiere licencia Pro o Premium |

**Ejemplo práctico:**  
Un analista desarrolla un dashboard de ventas en **Desktop** con transformaciones complejas en Power Query. Luego lo publica al **Service** donde el equipo directivo puede verlo en sus navegadores, recibir actualizaciones automáticas cada mañana y suscribirse a alertas.

**💡 Tip de entrevista:**  
Resalta que Desktop es para **creadores** (developers/analysts) y Service es para **consumidores** (stakeholders). Menciona que algunas funcionalidades avanzadas como dataflows o métricas solo están en Service con Premium.

---

### 3. ¿Cuáles son los componentes principales de Power BI?

**Definición:**  
Power BI se compone de varios bloques fundamentales que trabajan juntos para el ciclo completo de BI: extracción, transformación, modelado, visualización y distribución de datos.

**Componentes clave:**

**1. Power Query (Editor de Consultas):**
- ETL (Extract, Transform, Load)
- Conectores a más de 150 fuentes de datos
- Lenguaje M para transformaciones

**2. Modelo de Datos:**
- Relaciones entre tablas (star/snowflake schema)
- Medidas y columnas calculadas con DAX
- Tablas de hechos y dimensiones

**3. DAX (Data Analysis Expressions):**
- Lenguaje de fórmulas para cálculos
- Medidas, columnas calculadas y tablas calculadas
- Funciones de inteligencia de tiempo

**4. Visualizaciones:**
- Gráficos nativos (barras, líneas, mapas, etc.)
- Custom Visuals del marketplace
- Interactividad y filtros cruzados

**5. Power BI Service:**
- Publicación de informes
- Workspaces y apps
- Capacidades de colaboración

**6. Power BI Gateway:**
- Conexión segura entre datos on-premise y cloud
- Actualización programada de datos

**Ejemplo práctico:**  
En un proyecto típico:
1. **Power Query:** Extraes datos de SQL Server y Excel, limpias nulls, cambias tipos de datos
2. **Modelo:** Creas relaciones entre tablas Ventas ↔ Productos ↔ Clientes
3. **DAX:** Defines medidas como `Total Ventas = SUM(Ventas[Importe])`
4. **Visualizaciones:** Creas un dashboard con KPIs, gráficos de tendencias
5. **Service:** Publicas y programas actualización diaria a las 6 AM
6. **Gateway:** Instalas gateway para conectar con tu base de datos on-premise

**💡 Tip de entrevista:**  
Organiza tu respuesta en el flujo lógico del trabajo: **Conectar → Transformar → Modelar → Visualizar → Compartir**. Esto demuestra que entiendes el proceso end-to-end.

---

### 4. ¿Qué es Power Query?

**Definición:**  
Power Query es el motor de ETL (Extract, Transform, Load) de Power BI, que permite conectarse a diferentes fuentes de datos, limpiarlos, transformarlos y prepararlos antes de cargarlos al modelo de datos.

**Explicación práctica:**  
Es donde realizas toda la preparación de datos ANTES de que entren al modelo. Usa el lenguaje M (fórmulas) y una interfaz visual para transformaciones. Es crítico porque un buen modelo comienza con datos bien preparados.

**Operaciones comunes en Power Query:**
- **Conectar:** A SQL, Excel, APIs REST, Web, SharePoint, etc.
- **Limpiar:** Eliminar duplicados, nulls, errores
- **Transformar:** Cambiar tipos de datos, dividir columnas, pivotear/despivotear
- **Combinar:** Merge (joins) y append (union) de tablas
- **Filtrar:** Reducir filas según criterios
- **Crear columnas:** Personalizadas, condicionales, de ejemplos

**Ejemplo práctico:**  
```m
// Ejemplo de código M en Power Query
let
    Fuente = Excel.Workbook(File.Contents("C:\Ventas.xlsx"), null, true),
    Datos = Fuente{[Name="Ventas"]}[Data],
    TipoCambiado = Table.TransformColumnTypes(Datos,{{"Fecha", type date}, {"Importe", type number}}),
    FiltradoAnio = Table.SelectRows(TipoCambiado, each [Fecha] >= #date(2024,1,1)),
    ColumnaPersonalizada = Table.AddColumn(FiltradoAnio, "Trimestre", each Date.QuarterOfYear([Fecha]))
in
    ColumnaPersonalizada
```

**Escenario real:**  
Un archivo Excel tiene fechas como texto "01-ENE-2024", importes con símbolos "$", y registros duplicados. En Power Query:
1. Cambias tipo de columna Fecha a Date
2. Remueves caracteres no numéricos de Importe
3. Eliminas duplicados basados en ID
4. Agregas columna Año/Mes para análisis temporal

**💡 Tip de entrevista:**  
- Menciona que Power Query es **declarativo** (defines qué quieres, no cómo hacerlo paso a paso)
- Resalta que las transformaciones se aplican cada vez que actualizas los datos (no son manuales)
- Diferencia entre **Merge** (similar a JOIN en SQL) y **Append** (similar a UNION)
- Destaca que es **mejor hacer transformaciones en Power Query que en DAX** (mejor rendimiento)

---

### 5. ¿Qué es DAX en Power BI?

**Definición:**  
DAX (Data Analysis Expressions) es el lenguaje de fórmulas utilizado en Power BI para crear cálculos personalizados, medidas, columnas calculadas y tablas calculadas. Es similar a Excel pero optimizado para modelos de datos relacionales.

**Explicación práctica:**  
DAX es donde construyes la lógica de negocio: KPIs, ratios financieros, análisis temporal, cálculos acumulados, etc. Es el corazón analítico de Power BI y la diferencia entre un reporte simple y uno verdaderamente útil.

**Tipos de cálculos DAX:**

**1. Medidas (Measures):**
- Cálculos dinámicos que se evalúan en tiempo de ejecución
- Responden al contexto de filtros y slicers
- **Mejor práctica:** Usar medidas en lugar de columnas calculadas cuando sea posible

```dax
Total Ventas = SUM(Ventas[Importe])
Ventas Año Anterior = CALCULATE([Total Ventas], SAMEPERIODLASTYEAR(Calendario[Fecha]))
% Crecimiento = DIVIDE([Total Ventas] - [Ventas Año Anterior], [Ventas Año Anterior])
```

**2. Columnas Calculadas:**
- Se calculan fila por fila y se almacenan en el modelo
- Aumentan el tamaño del archivo
- Útiles para categorización o lógica que no cambia con filtros

```dax
Categoria Cliente = 
    IF(Clientes[Total Compras] > 100000, "Premium",
    IF(Clientes[Total Compras] > 50000, "Oro",
    "Estándar"))
```

**3. Tablas Calculadas:**
- Crean tablas completas mediante expresiones DAX
- Útiles para tablas de calendario, consolidaciones

```dax
Calendario = 
CALENDAR(MIN(Ventas[Fecha]), MAX(Ventas[Fecha]))
```

**Funciones DAX más importantes:**

| Categoría | Funciones clave | Uso |
|-----------|----------------|-----|
| **Agregación** | SUM, AVERAGE, COUNT, MIN, MAX | Totales básicos |
| **Filtros** | CALCULATE, FILTER, ALL, ALLEXCEPT | Modificar contexto |
| **Tiempo** | TOTALYTD, SAMEPERIODLASTYEAR, DATEADD | Análisis temporal |
| **Relaciones** | RELATED, RELATEDTABLE | Navegar entre tablas |
| **Iteración** | SUMX, AVERAGEX, RANKX | Cálculos fila por fila |

**Ejemplo práctico real:**  
```dax
// KPI empresarial complejo
Margen Neto % = 
VAR Ingresos = [Total Ventas]
VAR Costos = [Total Costos]
VAR Gastos = [Total Gastos Operativos]
VAR MArgenNeto = Ingresos - Costos - Gastos
RETURN
    DIVIDE(MargenNeto, Ingresos, 0)

// Top 5 productos por ventas
Ventas Top 5 = 
CALCULATE(
    [Total Ventas],
    TOPN(5, ALL(Productos[Nombre]), [Total Ventas], DESC)
)
```

**Contexto de evaluación (crítico en entrevistas):**
- **Contexto de Fila:** Evalúa fila por fila (columnas calculadas)
- **Contexto de Filtro:** Evalúa según filtros activos (medidas)
- **CALCULATE** modifica el contexto de filtro

**💡 Tip de entrevista:**  
- **Diferencia clave:** Medidas vs Columnas Calculadas (rendimiento y uso de memoria)
- Menciona que **CALCULATE es la función más importante de DAX** (modificar contexto)
- Explica que entiendes **contexto de evaluación** (filter context vs row context)
- Da ejemplos de **time intelligence** (YTD, YoY, MoM) - muy valorado en entrevistas
- Resalta que conoces funciones X (iteradores): SUMX, COUNTX para cálculos complejos
- Menciona herramientas como **DAX Studio** para optimización de queries

---

## 🎯 PRÓXIMAS 45 PREGUNTAS

### 📊 Modelado de Datos (Preguntas 6-15)
6. ¿Qué es un modelo Star Schema vs Snowflake Schema?
7. ¿Cuándo usar relaciones bidireccionales?
8. ¿Qué son las tablas de hechos y dimensiones?
9. ¿Qué es la cardinalidad en relaciones?
10. ¿Qué es la propagación de filtros (filter propagation)?
11. ¿Cuándo usar columnas calculadas vs medidas?
12. ¿Qué son las tablas de calendario y por qué son importantes?
13. ¿Qué es la seguridad a nivel de fila (RLS)?
14. ¿Qué diferencia hay entre Import, DirectQuery y Live Connection?
15. ¿Qué son los modelos compuestos?

### 🔧 Power Query Avanzado (Preguntas 16-25)
16. ¿Qué es el folding de consultas (query folding)?
17. ¿Cuándo usar Merge vs Append?
18. ¿Qué tipos de Join existen en Power Query?
19. ¿Qué son los parámetros en Power Query?
20. ¿Cómo optimizar el rendimiento en Power Query?
21. ¿Qué es el lenguaje M?
22. ¿Cómo manejar errores en Power Query?
23. ¿Qué son las funciones personalizadas en Power Query?
24. ¿Cómo trabajar con APIs y JSON en Power Query?
25. ¿Qué son las consultas de referencia vs duplicadas?

### 📈 DAX Avanzado (Preguntas 26-35)
26. ¿Qué es el contexto de evaluación en DAX?
27. ¿Cuál es la diferencia entre ALL, ALLSELECTED y ALLEXCEPT?
28. ¿Qué hace la función CALCULATE?
29. ¿Cómo funcionan las funciones de time intelligence?
30. ¿Qué son las variables VAR en DAX?
31. ¿Cuándo usar SUMX vs SUM?
32. ¿Qué es el filter context vs row context?
33. ¿Cómo crear medidas dinámicas con SWITCH?
34. ¿Qué es USERELATIONSHIP y cuándo usarlo?
35. ¿Cómo optimizar fórmulas DAX lentas?

### 🎨 Visualización y Diseño (Preguntas 36-40)
36. ¿Cuáles son las mejores prácticas de diseño de dashboards?
37. ¿Qué son los bookmarks y cómo usarlos?
38. ¿Cómo crear drill-through pages?
39. ¿Qué son los tooltips personalizados?
40. ¿Cómo funcionan los slicers y filtros?

### ☁️ Power BI Service y Administración (Preguntas 41-45)
41. ¿Qué es un workspace en Power BI Service?
42. ¿Qué diferencia hay entre Pro y Premium?
43. ¿Cómo funciona el Gateway?
44. ¿Qué son los dataflows?
45. ¿Cómo programar actualizaciones de datos?

### 🚀 Optimización y Mejores Prácticas (Preguntas 46-50)
46. ¿Cómo reducir el tamaño de un archivo .pbix?
47. ¿Qué es el Performance Analyzer?
48. ¿Cuáles son las mejores prácticas para DAX?
49. ¿Cómo documentar y mantener un proyecto de Power BI?
50. ¿Qué errores comunes evitar en Power BI?

---

## 📚 Recursos Adicionales

### Herramientas mencionadas:
- **DAX Studio:** Para optimización de queries DAX
- **Tabular Editor:** Para modelado avanzado
- **Performance Analyzer:** Para análisis de rendimiento en Desktop

### Comunidades recomendadas:
- SQLBI (Marco Russo y Alberto Ferrari)
- Guy in a Cube (YouTube)
- Power BI Community Forums
- DAX Patterns

---

## ✅ Checklist de Preparación

- [ ] Entender el flujo completo: Conectar → Transformar → Modelar → Visualizar → Compartir
- [ ] Dominar diferencias: Medidas vs Columnas, Import vs DirectQuery, Pro vs Premium
- [ ] Practicar DAX: CALCULATE, time intelligence, contextos de evaluación
- [ ] Conocer mejores prácticas: Star schema, RLS, optimización
- [ ] Preparar ejemplos reales de proyectos anteriores

---

**Última actualización:** Enero 2026  
**Versión:** 1.0 - Primeras 5 preguntas completas

---

¿Te gustaría que desarrolle las 45 preguntas restantes con el mismo nivel de detalle?