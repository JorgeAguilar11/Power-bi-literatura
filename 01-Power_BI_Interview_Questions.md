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

## 📊 Modelado de Datos (Preguntas 6-15)

### 6. ¿Qué es un modelo Star Schema vs Snowflake Schema?

**Definición:**  
Son dos arquitecturas de modelado dimensional para organizar datos en un data warehouse o modelo de BI:

- **Star Schema (Esquema Estrella)**: Una tabla de hechos central conectada directamente a múltiples tablas de dimensiones desnormalizadas.
- **Snowflake Schema (Esquema Copo de Nieve)**: Similar al Star Schema, pero las dimensiones están normalizadas en múltiples tablas relacionadas.

**Explicación Práctica:**

**Star Schema:**
- Tabla de hechos: `Ventas` → conecta a dimensiones: `Producto`, `Cliente`, `Fecha`, `Tienda`
- Las dimensiones contienen todos los atributos (ej: `Producto` tiene marca, categoría, subcategoría)
- Más redundancia, pero más rápido

**Snowflake Schema:**
- La dimensión `Producto` se divide en: `Producto` → `Subcategoría` → `Categoría`
- Menos redundancia, pero más joins necesarios
- Más complejo de consultar

**Ejemplo:**
```
Star Schema:
Ventas ─┬─ DimProducto (ID, Nombre, Categoría, Subcategoría, Marca)
        ├─ DimCliente (ID, Nombre, Ciudad, País)
        └─ DimFecha (Fecha, Año, Mes, Trimestre)

Snowflake Schema:
Ventas ─ DimProducto ─ DimSubcategoría ─ DimCategoría
```

**💡 Tip de entrevista:**  
Menciona: "En Power BI se recomienda **Star Schema** porque es más eficiente para el motor de Analysis Services. Snowflake requiere más relaciones y afecta el rendimiento. La desnormalización en dimensiones mejora la velocidad de consultas DAX."

---

### 7. ¿Cuándo usar relaciones bidireccionales?

**Definición:**  
Las **relaciones bidireccionales** permiten que los filtros se propaguen en ambas direcciones entre dos tablas (de la tabla A a B y de B a A).

**Explicación Práctica:**

**Por defecto**, Power BI usa relaciones unidireccionales (filtro fluye del lado "uno" al lado "muchos").

**Usar bidireccionales cuando:**
- Necesitas que ambas tablas se filtren mutuamente
- Trabajas con modelos many-to-many
- Necesitas que una tabla de hechos filtre a otra

**Precauciones:**
- Pueden causar ambigüedad en el contexto de filtro
- Afectan el rendimiento
- Pueden crear dependencias circulares

**Ejemplo:**
```
Escenario: Tienes Ventas y Presupuesto (dos tablas de hechos) conectadas a DimProducto.
Sin bidireccional: Filtrar Presupuesto no afecta Ventas
Con bidireccional: Filtrar Presupuesto también filtra Ventas
```

**💡 Tip de entrevista:**  
Menciona: "Las relaciones bidireccionales deben usarse con **precaución**. Prefiero resolver escenarios many-to-many usando una tabla puente o funciones DAX como `CROSSFILTER()` para tener más control. Solo las uso cuando es estrictamente necesario y el modelo es simple."

---

### 8. ¿Qué son las tablas de hechos y dimensiones?

**Definición:**  
Son los dos tipos principales de tablas en un modelo dimensional:

- **Tabla de Hechos (Fact Table)**: Contiene métricas numéricas y claves foráneas. Representa eventos o transacciones.
- **Tabla de Dimensiones (Dimension Table)**: Contiene atributos descriptivos que proporcionan contexto a los hechos.

**Explicación Práctica:**

**Tabla de Hechos:**
- Contiene **medidas** (ventas, cantidad, costos)
- Gran volumen de registros
- Claves foráneas a dimensiones
- Ejemplos: `Ventas`, `Inventario`, `Transacciones`

**Tabla de Dimensiones:**
- Contiene **atributos** (nombres, categorías, fechas)
- Menos registros
- Se usan para filtrar y agrupar
- Ejemplos: `Producto`, `Cliente`, `Fecha`, `Tienda`

**Ejemplo:**
```
Tabla de Hechos (Ventas):
- VentaID, FechaID, ProductoID, ClienteID, Monto, Cantidad

Tabla de Dimensión (Producto):
- ProductoID, NombreProducto, Categoría, Marca, Precio

Relación: Ventas[ProductoID] → Producto[ProductoID]
```

**💡 Tip de entrevista:**  
Menciona: "Las tablas de hechos deben ser **lo más angostas posible** (solo IDs y métricas). Las dimensiones son el lado 'uno' de las relaciones. Un buen diseño implica colocar las medidas DAX sobre hechos y los atributos descriptivos en dimensiones para optimizar el rendimiento."

---

### 9. ¿Qué es la cardinalidad en relaciones?

**Definición:**  
La **cardinalidad** define el tipo de relación entre dos tablas, indicando cuántos registros de una tabla se relacionan con cuántos registros de otra.

**Explicación Práctica:**

**Tipos de cardinalidad en Power BI:**

1. **Uno a Muchos (1:N)** - La más común
   - Un registro en tabla A → muchos en tabla B
   - Ejemplo: `Producto (1) → Ventas (N)`

2. **Muchos a Uno (N:1)** - Inversa de 1:N
   - Es lo mismo que 1:N, solo que invertida

3. **Uno a Uno (1:1)** - Rara
   - Un registro en A → un registro en B
   - Ejemplo: `Empleado → DetallesEmpleado`

4. **Muchos a Muchos (N:M)** - Compleja
   - Muchos registros en A → muchos en B
   - Ejemplo: `Estudiantes → Cursos` (requiere tabla puente)

**Ejemplo:**
```DAX
// 1:N (Ideal)
DimProducto[ProductoID] → FactVentas[ProductoID]

// N:M (Compleja - evitar o usar tabla puente)
Actores ←→ Películas (requiere tabla ActoresPelículas)
```

**💡 Tip de entrevista:**  
Menciona: "La cardinalidad **1:N es la recomendada** en Power BI. Las relaciones N:M funcionan pero pueden afectar el rendimiento. En estos casos, prefiero crear una **tabla puente** que convierta el N:M en dos relaciones 1:N."

---

### 10. ¿Qué es la propagación de filtros (filter propagation)?

**Definición:**  
La **propagación de filtros** es el mecanismo por el cual los filtros aplicados en una tabla se transmiten automáticamente a tablas relacionadas siguiendo las relaciones del modelo.

**Explicación Práctica:**

**Dirección de propagación:**
- Del lado "uno" → hacia el lado "muchos"
- En una relación `Producto (1) → Ventas (N)`, filtrar un producto filtra automáticamente las ventas

**Principio clave:**
- Los filtros fluyen **downstream** (aguas abajo)
- No fluyen "upstream" por defecto (a menos que la relación sea bidireccional)

**Visualización:**
```
DimCliente (1) ──→ Ventas (N) ──→ [Filtros se aplican]
DimProducto (1) ──→ Ventas (N) ──→ [Filtros se aplican]
DimFecha (1) ──────→ Ventas (N) ──→ [Filtros se aplican]
```

**Ejemplo:**
```
Escenario:
- Seleccionas "Categoría = Electrónica" en un slicer
- La tabla DimProducto se filtra
- El filtro se propaga a FactVentas automáticamente
- Las medidas muestran solo ventas de electrónica

Sin propagación, tendrías que filtrar manualmente cada tabla.
```

**💡 Tip de entrevista:**  
Menciona: "La propagación de filtros es fundamental para que los modelos funcionen. Power BI gestiona esto automáticamente mediante las relaciones. Es importante entender la **dirección del filtro** para diagnosticar por qué un slicer no está funcionando como esperamos."

---

### 11. ¿Cuándo usar columnas calculadas vs medidas?

**Definición:**  
- **Columnas Calculadas**: Se evalúan **fila por fila** en tiempo de actualización de datos y se almacenan en el modelo.
- **Medidas**: Se evalúan en **tiempo de consulta** según el contexto del visual y no se almacenan.

**Explicación Práctica:**

**Usa Columnas Calculadas cuando:**
- Necesitas el resultado para filtrar, agrupar o segmentar
- El cálculo es por fila (ej: calcular margen = precio - costo)
- Necesitas usar el valor en una relación
- Ejemplos: categorías, etiquetas, clasificaciones

**Usa Medidas cuando:**
- Necesitas agregaciones (SUM, AVG, COUNT)
- El resultado depende del contexto del visual
- Quieres optimizar el rendimiento (no ocupan espacio)
- Ejemplos: Total Ventas, % de participación, KPIs

**Ejemplo:**
```DAX
// Columna Calculada (se almacena en cada fila)
Margen = Ventas[PrecioVenta] - Ventas[Costo]

// Medida (se calcula en tiempo de consulta)
Total Ventas = SUM(Ventas[Monto])
Margen % = DIVIDE([Total Ventas] - [Total Costo], [Total Ventas])

// ❌ Mal uso: Columna calculada para total (consume memoria)
TotalVentasCol = SUM(Ventas[Monto])  // ¡Usar medida!

// ✅ Buen uso: Columna calculada para clasificación
CategoriaPrecio = IF(Producto[Precio] > 1000, "Premium", "Estándar")
```

**💡 Tip de entrevista:**  
Menciona: "Regla general: **columnas calculadas para atributos, medidas para métricas**. Las columnas aumentan el tamaño del modelo y se calculan solo en refresh. Las medidas son dinámicas y se recalculan con cada interacción. Siempre prefiero medidas cuando es posible por rendimiento."

---

### 12. ¿Qué son las tablas de calendario y por qué son importantes?

**Definición:**  
Una **tabla de calendario** (o tabla de fechas) es una tabla de dimensión que contiene una fila por cada fecha en un rango específico, con columnas adicionales para año, mes, trimestre, día de la semana, etc.

**Explicación Práctica:**

**Por qué son esenciales:**
- Necesarias para usar **funciones de inteligencia de tiempo** (time intelligence) en DAX
- Permiten análisis consistentes por períodos
- Facilitan filtros y agrupaciones por tiempo
- Evitan problemas con fechas faltantes en datos

**Creación:**
```DAX
Calendario = 
ADDCOLUMNS(
    CALENDAR(DATE(2020,1,1), DATE(2026,12,31)),
    "Año", YEAR([Date]),
    "Mes", FORMAT([Date], "MMM"),
    "Trimestre", "T" & QUARTER([Date]),
    "DiaSemana", FORMAT([Date], "dddd"),
    "NúmeroMes", MONTH([Date])
)
```

**Ejemplo:**
```DAX
// Sin tabla de calendario - NO funciona correctamente
VentasAñoAnterior = CALCULATE([Total Ventas], ??? ) // Difícil

// Con tabla de calendario - Funciona perfectamente
VentasAñoAnterior = CALCULATE([Total Ventas], SAMEPERIODLASTYEAR(Calendario[Fecha]))
YTD Ventas = TOTALYTD([Total Ventas], Calendario[Fecha])
```

**Relación:**
```
Calendario[Fecha] (1) → Ventas[Fecha] (N)
```

**💡 Tip de entrevista:**  
Menciona: "La tabla de calendario es **obligatoria para análisis temporal serio**. Debe marcarse como 'tabla de fechas' en Power BI. Incluyo siempre columnas como semana fiscal, año fiscal, días laborables. Las funciones como `SAMEPERIODLASTYEAR`, `TOTALYTD` requieren una tabla de calendario correctamente configurada."

---

### 13. ¿Qué es la seguridad a nivel de fila (RLS)?

**Definición:**  
**Row-Level Security (RLS)** es una característica que restringe el acceso a datos a nivel de fila basándose en roles de usuario, permitiendo que diferentes usuarios vean diferentes subconjuntos de datos en el mismo reporte.

**Explicación Práctica:**

**Cómo funciona:**
1. Defines **roles** en Power BI Desktop
2. Creas **filtros DAX** para cada rol
3. Asignas **usuarios o grupos** a roles en Power BI Service
4. Los filtros se aplican automáticamente cuando el usuario accede al reporte

**Casos de uso:**
- Gerentes regionales ven solo su región
- Vendedores ven solo sus clientes
- Departamentos ven solo su información

**Ejemplo:**
```DAX
// Rol: Gerente Regional
[Región] = USERPRINCIPALNAME()

// Rol: Vendedor
[Email Vendedor] = USERPRINCIPALNAME()

// Rol: País Específico
[País] = "México"

// Rol Dinámico con tabla de seguridad
[País] IN 
VALUES('TablaSeguridad'[País]) 
WHERE 'TablaSeguridad'[Email] = USERPRINCIPALNAME()
```

**Configuración:**
1. Modeling → Manage Roles → Create
2. Define filtro DAX por tabla
3. En Service: Settings → Security → Asigna usuarios

**💡 Tip de entrevista:**  
Menciona: "RLS se define en **Desktop** pero se aplica en **Service**. Es importante usar `USERPRINCIPALNAME()` para filtros dinámicos. Recomiendo crear una **tabla de seguridad** que mapee usuarios a dimensiones. Siempre pruebo los roles usando 'View as' en Desktop antes de publicar."

---

### 14. ¿Qué diferencia hay entre Import, DirectQuery y Live Connection?

**Definición:**  
Son tres **modos de conectividad** de datos en Power BI, que determinan cómo se almacenan y consultan los datos.

**Explicación Práctica:**

| **Característica** | **Import** | **DirectQuery** | **Live Connection** |
|-------------------|------------|-----------------|---------------------|
| **Datos** | Se copian al modelo | Quedan en origen | Quedan en origen |
| **Rendimiento** | Rápido | Depende de origen | Rápido |
| **Actualización** | Programada | Tiempo real | Tiempo real |
| **Tamaño** | Limitado (1GB gratis) | Sin límite | Sin límite |
| **DAX** | Completo | Limitado | Completo |
| **Power Query** | Sí | Limitado | No |
| **Origen** | Cualquiera | SQL, SAP, etc. | SSAS, Azure AS, PBI Datasets |

**Import (Importar):**
- Datos se cargan en memoria comprimida
- Mejor rendimiento
- Requiere refresh programado
- **Caso de uso**: Reportes con millones de filas, análisis complejos

**DirectQuery:**
- Consulta la fuente en tiempo real
- Datos siempre actualizados
- Más lento (depende del origen)
- **Caso de uso**: Dashboards en tiempo real, datos sensibles que no se pueden copiar

**Live Connection:**
- Conecta a modelos existentes (SSAS, Azure AS, Power BI Datasets)
- No puede modificar el modelo
- Usa el motor del servidor
- **Caso de uso**: Reutilizar modelos corporativos, separar capa semántica

**Ejemplo:**
```
Import: 
- Ventas históricas (5 años) → Se importan y comprimen
- Refresh diario a las 6 AM

DirectQuery:
- Dashboard de stock en almacén → Consulta SQL Server en tiempo real
- Limitaciones: No puedes crear columnas calculadas complejas

Live Connection:
- Reporte de ventas conectado a dataset corporativo
- El dataset está en Power BI Service, el reporte solo consume
```

**💡 Tip de entrevista:**  
Menciona: "**Import es el modo preferido** por rendimiento y flexibilidad. DirectQuery lo uso cuando necesito datos en tiempo real o cuando los datos son muy grandes para importar. Live Connection es ideal para **gobernanza centralizada**: un equipo mantiene el modelo, otros crean reportes. También puedo usar **modelos compuestos** que combinan Import y DirectQuery."

---

### 15. ¿Qué son los modelos compuestos?

**Definición:**  
Los **modelos compuestos (Composite Models)** permiten combinar múltiples fuentes de datos con diferentes modos de conectividad (Import + DirectQuery) en un solo modelo de Power BI.

**Explicación Práctica:**

**Antes de modelos compuestos:**
- Un modelo era 100% Import o 100% DirectQuery
- No podías mezclar modos

**Con modelos compuestos puedes:**
- Tener tablas en Import y otras en DirectQuery en el mismo modelo
- Agregar tablas calculadas a modelos DirectQuery
- Combinar datos de múltiples fuentes DirectQuery
- Crear relaciones entre tablas de diferentes modos

**Configuración por tabla:**
- Se establece en "Modo de almacenamiento" (Storage Mode)
- Opciones: Import, DirectQuery, Dual (automático según contexto)

**Ejemplo:**
```
Escenario: Dashboard de Ventas + Inventario

TablaVentas (Import - histórico)
- Datos de 3 años
- Importados y comprimidos
- Refresh nocturno

TablaInventario (DirectQuery - tiempo real)
- Conectada a SQL Server
- Stock actual en tiempo real
- Se consulta cada vez

TablaProductos (Dual)
- Se comporta como Import o DirectQuery según necesidad
- Reduce queries innecesarias

Relación: Ventas ← Productos → Inventario
```

**Beneficios:**
- Lo mejor de ambos mundos
- Optimización de rendimiento
- Flexibilidad máxima

**Consideraciones:**
- Más complejo de configurar
- Puede afectar rendimiento si no se diseña bien
- Las relaciones entre Import y DirectQuery tienen reglas especiales

**💡 Tip de entrevista:**  
Menciona: "Los modelos compuestos son poderosos para **optimizar rendimiento y actualización**. Por ejemplo, mantengo dimensiones y hechos históricos en Import (rápido) y datos operacionales en DirectQuery (tiempo real). El modo **Dual** es inteligente: actúa como Import cuando es posible y DirectQuery cuando es necesario. Es clave entender que las relaciones pueden tener **cardinalidad limitada** cuando cruzan modos."

---

## 🔧 Power Query Avanzado (Preguntas 16-25)

### 16. ¿Qué es el folding de consultas (query folding)?

**Definición:**  
**Query Folding** es la capacidad de Power Query para traducir transformaciones del lenguaje M a consultas nativas de la fuente de datos (SQL, por ejemplo), permitiendo que el procesamiento ocurra en el servidor de origen en lugar del equipo local.

**Explicación Práctica:**

**Cómo funciona:**
- Power Query intenta "empujar" las transformaciones a la fuente de datos
- Cuando hay folding: La base de datos hace el trabajo pesado
- Sin folding: Power Query procesa localmente (más lento)

**Operaciones que permiten folding:**
- ✅ Filtrar filas
- ✅ Seleccionar columnas
- ✅ Ordenar
- ✅ Group By
- ✅ Joins (Merge)
- ✅ Agregar columnas simples

**Operaciones que rompen folding:**
- ❌ Agregar columnas personalizadas con código M complejo
- ❌ Usar funciones personalizadas
- ❌ Cambiar tipos de datos (a veces)
- ❌ Transformaciones de texto complejas

**Ejemplo:**
```M
// ✅ Con Folding (se traduce a SQL)
Source = Sql.Database("servidor", "BD"),
FiltraVentas = Table.SelectRows(Source, each [Monto] > 1000),
SeleccionaColumnas = Table.SelectColumns(FiltraVentas, {"Fecha", "Monto"})
// SQL generado: SELECT Fecha, Monto FROM Ventas WHERE Monto > 1000

// ❌ Sin Folding (se procesa localmente)
AgregaColumna = Table.AddColumn(FiltraVentas, "Custom", 
    each if [Status] = "A" then "Activo" else "Inactivo")
// Power Query debe traer todos los datos y procesarlos localmente
```

**Verificar folding:**
- Click derecho en un paso → "View Native Query"
- Si aparece, hay folding ✅
- Si está deshabilitado, no hay folding ❌

**💡 Tip de entrevista:**  
Menciona: "El query folding es crítico para el **rendimiento con grandes volúmenes**. Siempre verifico que mis transformaciones mantengan el folding usando 'View Native Query'. Evito columnas personalizadas complejas al inicio del proceso y las coloco después de que los datos ya estén filtrados. En proyectos reales, esto puede significar la diferencia entre segundos y horas de procesamiento."

---

### 17. ¿Cuándo usar Merge vs Append?

**Definición:**  
Son dos operaciones fundamentales para combinar datos en Power Query:

- **Merge**: Combina tablas **horizontalmente** (agrega columnas) - Similar a JOIN en SQL
- **Append**: Combina tablas **verticalmente** (agrega filas) - Similar a UNION en SQL

**Explicación Práctica:**

**Usa MERGE cuando:**
- Necesitas enriquecer una tabla con columnas de otra
- Tienes una columna en común (clave)
- Quieres hacer un JOIN (Left, Right, Inner, etc.)
- Ejemplo: Agregar nombre del cliente a ventas usando ClienteID

**Usa APPEND cuando:**
- Las tablas tienen la misma estructura (mismas columnas)
- Quieres apilar datos de múltiples fuentes
- Necesitas consolidar información
- Ejemplo: Combinar ventas de enero + febrero + marzo

**Ejemplo:**

**MERGE (Horizontal):**
```
Tabla Ventas:                Tabla Clientes:
ClienteID | Monto           ClienteID | Nombre
101       | 500      +      101       | Ana
102       | 300             102       | Luis

Resultado:
ClienteID | Monto | Nombre
101       | 500   | Ana
102       | 300   | Luis
```

**APPEND (Vertical):**
```
Ventas Enero:              Ventas Febrero:
Fecha    | Monto          Fecha    | Monto
01/01    | 500            01/02    | 800
02/01    | 300     +      02/02    | 400

Resultado:
Fecha    | Monto
01/01    | 500
02/01    | 300
01/02    | 800
02/02    | 400
```

**💡 Tip de entrevista:**  
Menciona: "**Merge es para JOIN, Append es para UNION**. En Power Query, prefiero hacer Merge en lugar de usar RELATED en DAX cuando es posible, porque aprovecha el query folding. Para Append, es útil cuando consolido múltiples archivos Excel o CSVs con la misma estructura usando 'Append Queries as New'."

---

### 18. ¿Qué tipos de Join existen en Power Query?

**Definición:**  
Los **tipos de Join** (al hacer Merge) determinan qué filas se incluyen en el resultado basándose en las coincidencias entre las claves de ambas tablas.

**Explicación Práctica:**

**6 tipos de Join en Power Query:**

1. **Left Outer** (más común)
   - Todas las filas de la tabla izquierda
   - Solo coincidencias de la derecha
   - Uso: Mantener todos los registros principales

2. **Right Outer**
   - Todas las filas de la tabla derecha
   - Solo coincidencias de la izquierda
   - Uso: Menos común

3. **Full Outer**
   - Todas las filas de ambas tablas
   - Coincidan o no
   - Uso: No perder ningún registro

4. **Inner**
   - Solo filas que coinciden en ambas tablas
   - Uso: Filtrar por existencia

5. **Left Anti**
   - Filas de la izquierda que NO tienen coincidencia en la derecha
   - Uso: Encontrar registros faltantes

6. **Right Anti**
   - Filas de la derecha que NO tienen coincidencia en la izquierda
   - Uso: Menos común

**Ejemplo:**
```
Tabla A (Ventas):         Tabla B (Clientes):
ClienteID | Monto         ClienteID | Nombre
1         | 100           1         | Ana
2         | 200           3         | Luis
4         | 300

Left Outer:               Inner:                Left Anti:
ClienteID | Monto | Nom   ClienteID | Monto     ClienteID | Monto
1         | 100   | Ana   1         | 100       2         | 200
2         | 200   | null  4         | 300
4         | 300   | null  

Full Outer:
ClienteID | Monto | Nom
1         | 100   | Ana
2         | 200   | null
3         | null  | Luis
4         | 300   | null
```

**💡 Tip de entrevista:**  
Menciona: "**Left Outer es el join más común** en BI porque queremos mantener todas las transacciones aunque no tengan coincidencia en dimensiones. **Left Anti es muy útil** para QA: encontrar ventas sin cliente, productos sin categoría, etc. En entrevistas, siempre menciono que entiendo la diferencia entre Inner (restrictivo) y Outer (inclusivo)."

---

### 19. ¿Qué son los parámetros en Power Query?

**Definición:**  
Los **parámetros** son valores reutilizables que se pueden usar en múltiples consultas y transformaciones, permitiendo crear soluciones dinámicas y fáciles de mantener.

**Explicación Práctica:**

**Casos de uso:**
- Rutas de archivo que cambian
- Fechas de inicio/fin para filtros
- Servidor de base de datos (Desarrollo/Producción)
- Umbrales o valores de negocio
- Filtros dinámicos

**Tipos de parámetros:**
- Texto
- Número
- True/False
- Fecha
- Lista

**Creación:**
1. Home → Manage Parameters → New
2. Define nombre, tipo, valor actual
3. Opcional: valores sugeridos

**Ejemplo:**
```M
// Crear parámetro
RutaArchivo = "C:\Datos\Ventas.xlsx" meta [IsParameterQuery=true]

// Usar parámetro en Source
Source = Excel.Workbook(File.Contents(RutaArchivo))

// Parámetro de fecha para filtro
FechaInicio = #date(2025, 1, 1) meta [IsParameterQuery=true]
FiltroPorFecha = Table.SelectRows(Source, each [Fecha] >= FechaInicio)

// Parámetro para conexión dinámica
Servidor = "servidor-prod" meta [IsParameterQuery=true]
BaseDatos = "Ventas" meta [IsParameterQuery=true]
Source = Sql.Database(Servidor, BaseDatos)
```

**Ventajas:**
- Cambiar una vez, afecta todas las consultas
- Facilita migraciones (Dev → Prod)
- Permite crear plantillas reutilizables
- Los usuarios finales pueden modificarlos (con cuidado)

**💡 Tip de entrevista:**  
Menciona: "Los parámetros son fundamentales para **soluciones escalables**. Los uso para rutas de archivo, conexiones de BD y filtros de fecha. En proyectos corporativos, creo parámetros para diferenciar ambientes (Dev/QA/Prod). También son útiles para crear **informes parametrizados** donde el usuario puede cambiar valores sin tocar el código M."

---

### 20. ¿Cómo optimizar el rendimiento en Power Query?

**Definición:**  
La **optimización en Power Query** implica aplicar técnicas y mejores prácticas para reducir el tiempo de procesamiento y el consumo de recursos durante la transformación de datos.

**Explicación Práctica:**

**Mejores prácticas de rendimiento:**

1. **Mantén Query Folding**
   - Filtra y selecciona columnas lo antes posible
   - Verifica folding con "View Native Query"

2. **Filtra datos temprano**
   - Aplica filtros en los primeros pasos
   - Reduce el volumen de datos lo antes posible

3. **Elimina columnas innecesarias**
   - "Remove Other Columns" en lugar de eliminar una por una
   - Menos columnas = menos memoria

4. **Evita columnas personalizadas complejas**
   - Rompen el folding
   - Mueve al final del proceso si es posible

5. **Desactiva carga de consultas auxiliares**
   - Click derecho → "Enable Load" (desmarcar)
   - Solo carga las tablas finales

6. **Usa referencias en lugar de duplicados**
   - Reutiliza pasos sin duplicar datos

7. **Buffer solo cuando sea necesario**
   - `Table.Buffer()` guarda en memoria
   - Usa solo si accedes múltiples veces

8. **Agrupa operaciones**
   - Combina múltiples transformaciones en un paso

**Ejemplo:**
```M
// ❌ MAL - Sin optimizar
Source = Sql.Database("server", "DB"),
// Trae todas las columnas y filas primero
AddCustom = Table.AddColumn(Source, "Custom", each ...),
FilterRows = Table.SelectRows(AddCustom, each [Fecha] >= #date(2025,1,1)),
RemoveColumns = Table.RemoveColumns(FilterRows, {"Col1", "Col2", "Col3"})

// ✅ BIEN - Optimizado
Source = Sql.Database("server", "DB"),
// Filtra PRIMERO (aprovecha folding)
FilterRows = Table.SelectRows(Source, each [Fecha] >= #date(2025,1,1)),
// Selecciona solo columnas necesarias (mantiene folding)
SelectColumns = Table.SelectColumns(FilterRows, {"Fecha", "Monto", "Cliente"}),
// Custom al final (después de reducir datos)
AddCustom = Table.AddColumn(SelectColumns, "Custom", each ...)
```

**💡 Tip de entrevista:**  
Menciona: "La regla de oro es **filtrar primero, transformar después**. Siempre empiezo eliminando columnas y filas innecesarias para reducir el volumen. Verifico el query folding constantemente. En proyectos con millones de registros, desactivar la carga de consultas intermedias puede reducir el tiempo de refresh significativamente. También uso el **Query Diagnostics** para identificar cuellos de botella."

---

### 21. ¿Qué es el lenguaje M?

**Definición:**  
**M** (también llamado Power Query Formula Language) es el lenguaje de programación funcional usado por Power Query para definir transformaciones de datos. Es case-sensitive y se ejecuta secuencialmente.

**Explicación Práctica:**

**Características clave:**

- **Funcional**: Las funciones no tienen efectos secundarios
- **Case-sensitive**: `Table.SelectRows` ≠ `table.selectrows`
- **Secuencial**: Cada paso referencia pasos anteriores
- **Declarativo**: Describes QUÉ quieres, no CÓMO

**Estructura básica:**
```M
let
    Paso1 = Fuente,
    Paso2 = Transformación(Paso1),
    Paso3 = OtraTransformación(Paso2)
in
    Paso3
```

**Elementos principales:**
- `let ... in`: Estructura de consulta
- `each`: Función anónima para iterar filas
- `=>`: Define funciones personalizadas
- `#table`, `#date`, `#duration`: Literales de tipo

**Ejemplo:**
```M
let
    // Fuente de datos
    Source = Excel.Workbook(File.Contents("C:\Ventas.xlsx")),
    
    // Selecciona hoja
    Hoja = Source{[Name="Ventas"]}[Data],
    
    // Promueve headers
    Headers = Table.PromoteHeaders(Hoja),
    
    // Filtra filas con each
    Filtrado = Table.SelectRows(Headers, each [Monto] > 1000),
    
    // Agrega columna calculada
    ConCategoria = Table.AddColumn(Filtrado, "Categoría", 
        each if [Monto] > 5000 then "Alto" else "Normal"),
    
    // Cambia tipo
    TiposCambiados = Table.TransformColumnTypes(ConCategoria, {
        {"Fecha", type date},
        {"Monto", type number}
    })
in
    TiposCambiados

// Función personalizada
(precio as number) => 
    if precio > 100 then precio * 0.9 else precio
```

**💡 Tip de entrevista:**  
Menciona: "El lenguaje M es **fundamental para transformaciones avanzadas**. Aunque Power Query tiene interfaz visual, conocer M me permite hacer transformaciones que no son posibles con clicks. Uso M para crear funciones reutilizables, manejar errores con `try...otherwise`, y depurar problemas. El operador `each` es esencial para trabajar con filas, y equivale a `(_) =>`."

---

### 22. ¿Cómo manejar errores en Power Query?

**Definición:**  
El **manejo de errores** en Power Query permite controlar qué sucede cuando una transformación falla, evitando que todo el proceso se detenga y permitiendo soluciones alternativas.

**Explicación Práctica:**

**Técnicas de manejo de errores:**

1. **Try...Otherwise** (a nivel de valor)
```M
= try [Monto] / [Cantidad] otherwise 0
```

2. **Remove Errors** (elimina filas con errores)
```M
= Table.RemoveRowsWithErrors(tabla)
```

3. **Replace Errors** (reemplaza errores con valor)
```M
= Table.ReplaceErrorValues(tabla, {{"Columna", "Reemplazo"}})
```

4. **Keep Errors** (muestra solo filas con errores para debug)
```M
= Table.SelectRowsWithErrors(tabla)
```

5. **Conditional Logic** (prevención)
```M
= if [Columna] <> null then [Columna] else "Default"
```

**Ejemplo:**
```M
// Try...Otherwise en columna calculada
TablaSafe = Table.AddColumn(Source, "División", 
    each try [Monto] / [Cantidad] otherwise null)

// Manejo de errores en conversión de tipos
ConvertirFecha = Table.TransformColumns(Source, {
    {"Fecha", each try Date.From(_) otherwise null}
})

// Función personalizada con manejo de errores
ObtenerPrecio = (producto as text) =>
    let
        Resultado = try Table.SelectRows(Productos, 
            each [Nombre] = producto){0}[Precio]
        otherwise 0
    in
        Resultado

// Replace errors en múltiples columnas
SinErrores = Table.ReplaceErrorValues(Source, {
    {"Columna1", 0},
    {"Columna2", "N/A"},
    {"Columna3", #date(2025, 1, 1)}
})

// Keep Errors para diagnóstico
ErroresEncontrados = Table.SelectRowsWithErrors(Source)
```

**Errores comunes:**
- División por cero
- Conversión de tipos fallida
- Valores null en operaciones
- Claves no encontradas en lookup

**💡 Tip de entrevista:**  
Menciona: "El manejo de errores es crítico en **entornos de producción**. Uso `try...otherwise` para conversiones de tipos que pueden fallar (fechas, números). Para debugging, primero uso `Table.SelectRowsWithErrors()` para ver qué filas tienen problemas. En transformaciones complejas, prefiero manejar errores explícitamente en lugar de dejar que falle el refresh completo. También documento por qué se reemplaza un error con un valor específico."

---

### 23. ¿Qué son las funciones personalizadas en Power Query?

**Definición:**  
Las **funciones personalizadas** son bloques de código M reutilizables que aceptan parámetros y retornan un valor, permitiendo encapsular lógica compleja para usar en múltiples consultas.

**Explicación Práctica:**

**Cuándo crear funciones:**
- Lógica que se repite en múltiples consultas
- Cálculos complejos reutilizables
- Procesamiento de múltiples archivos similares
- Encapsular reglas de negocio

**Sintaxis:**
```M
(parámetro1 as tipo, parámetro2 as tipo) as tipo_retorno =>
    let
        lógica = ...,
        resultado = ...
    in
        resultado
```

**Invocar función:**
```M
= Table.AddColumn(tabla, "NuevaCol", each MiFunción([Columna]))
```

**Ejemplo:**
```M
// Función simple: Calcular descuento
fnDescuento = (precio as number, porcentaje as number) as number =>
    precio * (1 - porcentaje / 100)

// Usar función
= Table.AddColumn(Productos, "PrecioFinal", 
    each fnDescuento([Precio], [Descuento]))

// Función compleja: Obtener datos de archivo Excel
fnCargarExcel = (rutaArchivo as text, nombreHoja as text) as table =>
    let
        Source = Excel.Workbook(File.Contents(rutaArchivo)),
        Hoja = Source{[Name=nombreHoja]}[Data],
        Headers = Table.PromoteHeaders(Hoja),
        Tipos = Table.TransformColumnTypes(Headers, {
            {"Fecha", type date},
            {"Monto", type number}
        })
    in
        Tipos

// Usar función para múltiples archivos
VentasEnero = fnCargarExcel("C:\Enero.xlsx", "Ventas")
VentasFebrero = fnCargarExcel("C:\Febrero.xlsx", "Ventas")

// Función con manejo de errores
fnDivisionSegura = (numerador as number, denominador as number) as number =>
    try numerador / denominador otherwise 0

// Función que retorna tabla
fnTop10Clientes = (tablaVentas as table) as table =>
    let
        Agrupado = Table.Group(tablaVentas, {"Cliente"}, {
            {"Total", each List.Sum([Monto]), type number}
        }),
        Ordenado = Table.Sort(Agrupado, {{"Total", Order.Descending}}),
        Top10 = Table.FirstN(Ordenado, 10)
    in
        Top10
```

**Invocar función en múltiples filas:**
```M
// Procesar múltiples archivos listados
Archivos = Table.FromList({"Enero.xlsx", "Febrero.xlsx", "Marzo.xlsx"}),
Expandir = Table.AddColumn(Archivos, "Datos", 
    each fnCargarExcel("C:\" & [Column1], "Ventas")),
ExpandirTabla = Table.ExpandTableColumn(Expandir, "Datos", {"Fecha", "Monto"})
```

**💡 Tip de entrevista:**  
Menciona: "Las funciones personalizadas son clave para **DRY (Don't Repeat Yourself)**. Las uso para procesar múltiples archivos con la misma estructura, aplicar reglas de negocio consistentes, y encapsular transformaciones complejas. Un caso común es crear una función que cargue y estandarice archivos Excel de diferentes períodos. Importante: las funciones personalizadas **rompen el query folding**, así que las uso después de filtrar datos."

---

### 24. ¿Cómo trabajar con APIs y JSON en Power Query?

**Definición:**  
Power Query puede conectarse a **APIs REST** y procesar datos en formato **JSON**, permitiendo integrar fuentes de datos web y servicios cloud en tus modelos.

**Explicación Práctica:**

**Conectar a API:**
```M
// Conexión básica a API
Source = Json.Document(Web.Contents("https://api.ejemplo.com/ventas"))

// Con autenticación (en Data Source Settings)
// API Key en header
Source = Json.Document(
    Web.Contents("https://api.ejemplo.com/datos",
    [Headers=[Authorization="Bearer TOKEN_AQUÍ"]]
))
```

**Procesar JSON:**
```M
// JSON simple: [{"id": 1, "nombre": "Ana"}, {"id": 2, "nombre": "Luis"}]
Source = Json.Document(File.Contents("C:\datos.json")),
ToTable = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
Expandir = Table.ExpandRecordColumn(ToTable, "Column1", {"id", "nombre"})

// JSON anidado: {"data": {"users": [...]}}
Source = Json.Document(Web.Contents("https://api.ejemplo.com")),
Data = Source[data],
Users = Data[users],
ToTable = Table.FromList(Users, Splitter.SplitByNothing()),
Expandir = Table.ExpandRecordColumn(...)
```

**Paginación de API:**
```M
// Función para manejar paginación
fnObtenerTodasPaginas = (url as text) as table =>
    let
        Contenido = Json.Document(Web.Contents(url)),
        Datos = Contenido[data],
        ToTable = Table.FromList(Datos, Splitter.SplitByNothing()),
        PaginaSiguiente = try Contenido[next_page] otherwise null,
        Resultado = if PaginaSiguiente <> null 
                    then Table.Combine({ToTable, fnObtenerTodasPaginas(PaginaSiguiente)})
                    else ToTable
    in
        Resultado
```

**Ejemplo Completo:**
```M
let
    // 1. Llamar API REST
    url = "https://api.github.com/repos/microsoft/PowerBI-Desktop/issues",
    
    // 2. Headers de autenticación (opcional)
    Source = Json.Document(
        Web.Contents(url, [
            Headers=[
                #"User-Agent"="PowerBI",
                #"Accept"="application/json"
            ]
        ])
    ),
    
    // 3. Convertir lista JSON a tabla
    ToTable = Table.FromList(Source, Splitter.SplitByNothing()),
    
    // 4. Expandir columnas del record
    ExpandRecord = Table.ExpandRecordColumn(ToTable, "Column1", 
        {"id", "title", "state", "created_at", "user"}),
    
    // 5. Expandir columna anidada (user)
    ExpandUser = Table.ExpandRecordColumn(ExpandRecord, "user", 
        {"login"}, {"username"}),
    
    // 6. Cambiar tipos
    TiposCambiados = Table.TransformColumnTypes(ExpandUser, {
        {"id", Int64.Type},
        {"created_at", type datetime}
    })
in
    TiposCambiados
```

**Tips para JSON:**
- `Table.FromRecords()` para listas de objetos
- `Table.FromList()` + `ExpandRecordColumn()` para flexibilidad
- `Json.Document()` lee JSON desde web o archivo
- Usa "View Native Query" no funciona con APIs (sin folding)

**💡 Tip de entrevista:**  
Menciona: "Trabajar con APIs es cada vez más común en BI moderno. Uso `Web.Contents()` para conectar APIs REST y `Json.Document()` para parsear la respuesta. Lo complicado es el **JSON anidado**: hay que expandir records y listas cuidadosamente. Para APIs con paginación, creo funciones recursivas que consolidan todas las páginas. Importante: las credenciales de API se gestionan en 'Data Source Settings' para seguridad."

---

### 25. ¿Qué son las consultas de referencia vs duplicadas?

**Definición:**  
Son dos formas de reutilizar consultas existentes en Power Query:

- **Referencia (Reference)**: Crea una nueva consulta que **reutiliza los pasos** de la original. Cambios en la original afectan a la referencia.
- **Duplicar (Duplicate)**: Crea una **copia independiente** completa. Los cambios no se sincronizan.

**Explicación Práctica:**

**Referencia:**
- Click derecho en consulta → "Reference"
- Hereda todos los pasos de la consulta original
- Más eficiente (no duplica datos en memoria)
- Útil para crear variaciones (filtros diferentes de la misma base)

**Duplicar:**
- Click derecho en consulta → "Duplicate"
- Copia todos los pasos como nuevos
- Consultas independientes
- Útil para plantillas o cuando necesitas modificar la base

**Ejemplo:**

**Escenario: Tabla de ventas que necesitas para dos reportes**

```M
// Consulta Original: VentasBase
Source = Excel.Workbook(...),
Ventas = Source{[Name="Ventas"]}[Data],
Headers = Table.PromoteHeaders(Ventas),
Tipos = Table.TransformColumnTypes(...)

// Referencia 1: Ventas2025
// Reference a VentasBase
= VentasBase  // Hereda todos los pasos
// Agregar filtro adicional
Filtrar2025 = Table.SelectRows(VentasBase, each [Año] = 2025)

// Referencia 2: VentasTop10
// Reference a VentasBase
= VentasBase
Top10 = Table.FirstN(Table.Sort(VentasBase, {{"Monto", Order.Descending}}), 10)

// Duplicado: VentasParaOtroProceso
// Es copia completa independiente
= VentasBase  // Pero los pasos están copiados, no referenciados
// Puedes modificar Source sin afectar VentasBase original
```

**Estructura en memoria:**

```
REFERENCIA:
VentasBase (se procesa 1 vez)
    ↓ referencia
Ventas2025 (solo aplica filtro adicional)
    ↓ referencia
VentasTop10 (solo aplica top)

DUPLICADO:
VentasBase (se procesa)
VentasDuplicada (se procesa independientemente - duplica datos)
```

**Cuándo usar cada una:**

| **Situación** | **Usar** |
|--------------|----------|
| Misma fuente, diferentes filtros | Referencia |
| Crear tabla auxiliar temporal | Referencia (+ disable load) |
| Tablas de hechos y dimensiones del mismo source | Referencia |
| Quieres modificar la fuente sin afectar otras | Duplicar |
| Plantilla para múltiples archivos | Duplicar |
| Testear cambios sin romper otras consultas | Duplicar |

**💡 Tip de entrevista:**  
Menciona: "**Referencia es la opción por defecto** porque es más eficiente: Power Query solo procesa la consulta original una vez y las referencias solo aplican pasos adicionales. Uso referencias para crear tablas de hechos y dimensiones de la misma fuente. Para optimizar, marco las consultas intermedias como 'Enable Load = False' para que no se carguen al modelo. Solo duplico cuando necesito una copia completamente independiente o cuando voy a modificar pasos iniciales sin afectar otras consultas."

---


## **26. ¿Qué es el contexto de evaluación en DAX?**

### 📖 Definición
El **contexto de evaluación** es el entorno en el que se evalúa una expresión DAX. Determina qué datos se consideran al calcular un resultado. Es el concepto más importante para entender DAX.

### 🔧 Explicación Práctica

**Existen dos tipos de contexto:**

1. **Filter Context (Contexto de Filtro)**
   - Determina QUÉ filas son visibles
   - Creado por: Slicers, filtros, visual, WHERE en SQL
   - Afecta a medidas
   - Se propaga por relaciones

2. **Row Context (Contexto de Fila)**
   - Evaluación fila por fila
   - Creado por: Columnas calculadas, iteradores (SUMX, FILTER)
   - No se propaga automáticamente por relaciones
   - Necesita RELATED para acceder a tablas relacionadas

### 💡 Ejemplo
```DAX
// FILTER CONTEXT - En una medida
Total Ventas = SUM(Ventas[Monto])
// Si hay un slicer en "Categoría = Electrónica"
// Filter context: solo filas de Electrónica
// Resultado: suma solo ventas de electrónica

// ROW CONTEXT - En columna calculada
Margen = Ventas[Precio] - Ventas[Costo]
// Se evalúa fila por fila
// [Precio] y [Costo] de la fila actual

// COMBINACIÓN - Iterador con ambos contextos
Ventas por Cantidad = 
SUMX(
    Ventas,  // Crea ROW CONTEXT (itera cada fila)
    [Precio] * [Cantidad]  // Usa el row context
)
// Si hay filtros activos, SUMX respeta el FILTER CONTEXT
```

**Contexto en acción:**
```DAX
// En visual con slicer de Año = 2025
Total Ventas = SUM(Ventas[Monto])
// Filter context: Año = 2025
// Resultado: suma solo ventas de 2025

// CALCULATE modifica filter context
Ventas Totales Global = CALCULATE(
    SUM(Ventas[Monto]),
    ALL(Calendario)  // Ignora filtro de año
)
// Resultado: suma TODAS las ventas, ignorando el slicer
```

### 🎯 Tip de Entrevista
**Menciona:** "El contexto es **el corazón de DAX**. Filter context determina qué filas están disponibles y row context evalúa fila por fila. La función `CALCULATE` es poderosa porque **modifica el filter context**. Un error común es esperar que row context acceda automáticamente a tablas relacionadas - para eso se necesita `RELATED`. Entender cómo se combinan ambos contextos es lo que diferencia a un usuario básico de uno avanzado."

---

## **27. ¿Cuál es la diferencia entre ALL, ALLSELECTED y ALLEXCEPT?**

### 📖 Definición
Son funciones que **modifican el contexto de filtro** removiendo filtros de columnas o tablas completas:

- **ALL**: Elimina todos los filtros de la tabla/columna especificada
- **ALLSELECTED**: Elimina filtros internos del visual, mantiene externos (slicers, filtros de página)
- **ALLEXCEPT**: Elimina todos los filtros EXCEPTO los de las columnas especificadas

### 🔧 Explicación Práctica

### **ALL(Tabla o Columna)**
```DAX
// Ignora todos los filtros
% del Total = 
DIVIDE(
    SUM(Ventas[Monto]),
    CALCULATE(SUM(Ventas[Monto]), ALL(Ventas))
)
// Denominador siempre es el total absoluto

// ALL en columna específica
Ventas Sin Filtro Categoría = 
CALCULATE(
    SUM(Ventas[Monto]),
    ALL(Producto[Categoría])
)
```

### **ALLSELECTED(Tabla o Columna)**
```DAX
// Respeta filtros externos (slicers), ignora filtros del visual
% del Total Filtrado = 
DIVIDE(
    SUM(Ventas[Monto]),
    CALCULATE(SUM(Ventas[Monto]), ALLSELECTED(Ventas))
)
// Si hay slicer de Año = 2025, denominador es total de 2025
// En una tabla con productos, cada % es respecto al total visible
```

### **ALLEXCEPT(Tabla, Columna1, Columna2, ...)**
```DAX
// Elimina todos los filtros EXCEPTO los especificados
Ventas por Categoría Sin Otros Filtros = 
CALCULATE(
    SUM(Ventas[Monto]),
    ALLEXCEPT(Producto, Producto[Categoría])
)
// Mantiene filtro de Categoría, ignora Marca, Color, etc.
```

### 💡 Ejemplo Comparativo
```DAX
// Escenario: Visual con Productos, Slicer de Año = 2025

// ALL - Ignora TODO
Total Global = CALCULATE([Total Ventas], ALL(Ventas))
// Resultado: Total de todos los años y productos

// ALLSELECTED - Respeta slicers
Total Año Seleccionado = CALCULATE([Total Ventas], ALLSELECTED(Ventas))
// Resultado: Total de 2025 (respeta slicer), todos los productos

// ALLEXCEPT - Mantiene columnas específicas
Total Mismo Producto = CALCULATE(
    [Total Ventas], 
    ALLEXCEPT(Ventas, Producto[NombreProducto])
)
// Resultado: Total del producto en la fila, todos los años
```

**Tabla comparativa:**

| **Función** | **Filtros de Visual** | **Slicers** | **Filtros de Página** |
|-------------|----------------------|-------------|----------------------|
| ALL | ❌ Elimina | ❌ Elimina | ❌ Elimina |
| ALLSELECTED | ❌ Elimina | ✅ Mantiene | ✅ Mantiene |
| ALLEXCEPT | ❌ Elimina (excepto especificadas) | ❌ Elimina (excepto especificadas) | ❌ Elimina (excepto especificadas) |

### 🎯 Tip de Entrevista
**Menciona:** "**ALL** es para cálculos absolutos (% del gran total). **ALLSELECTED** es ideal para cálculos de porcentaje que respetan la interacción del usuario con slicers. **ALLEXCEPT** es un atajo cuando quieres mantener algunos filtros pero eliminar otros - es más limpio que múltiples ALL en CALCULATE. Un caso común: uso `ALLSELECTED` para crear % del total en tablas donde quiero que el denominador respete los filtros de página pero no los de la tabla."

---

## **28. ¿Qué hace la función CALCULATE?**

### 📖 Definición
**CALCULATE** es la función más importante de DAX. Evalúa una expresión modificando el contexto de filtro mediante filtros adicionales, removiendo filtros, o ambos.

### 🔧 Explicación Práctica

**Sintaxis:**
```DAX
CALCULATE(<expresión>, <filtro1>, <filtro2>, ...)
```

**Qué hace CALCULATE:**
1. Toma el contexto actual
2. Aplica/modifica filtros
3. Evalúa la expresión en el nuevo contexto
4. Transición de contexto (convierte row context en filter context)

**Usos principales:**

### **1. Agregar filtros**
```DAX
Ventas Electrónica = 
CALCULATE(
    SUM(Ventas[Monto]),
    Producto[Categoría] = "Electrónica"
)

Ventas 2025 = 
CALCULATE(
    [Total Ventas],
    Calendario[Año] = 2025
)
```

### **2. Remover filtros**
```DAX
Ventas Sin Filtro Región = 
CALCULATE(
    [Total Ventas],
    ALL(Cliente[Región])
)
```

### **3. Combinar filtros**
```DAX
Ventas Premium México = 
CALCULATE(
    [Total Ventas],
    Producto[Categoría] = "Premium",
    Cliente[País] = "México",
    Calendario[Año] IN {2024, 2025}
)
```

### **4. Modificar relaciones**
```DAX
Ventas por Fecha Envío = 
CALCULATE(
    [Total Ventas],
    USERELATIONSHIP(Ventas[FechaEnvío], Calendario[Fecha])
)
```

### **5. Transición de contexto**
```DAX
// En columna calculada
Ventas del Producto = 
CALCULATE([Total Ventas])
// CALCULATE convierte row context en filter context
// Filtra automáticamente por el ProductoID de la fila actual
```

### 💡 Ejemplo Avanzado
```DAX
// Medida compleja con CALCULATE
Ventas YoY Growth = 
VAR VentasActual = [Total Ventas]
VAR VentasAñoAnterior = 
    CALCULATE(
        [Total Ventas],
        SAMEPERIODLASTYEAR(Calendario[Fecha])
    )
VAR Crecimiento = 
    DIVIDE(
        VentasActual - VentasAñoAnterior,
        VentasAñoAnterior
    )
RETURN
    Crecimiento

// CALCULATE con múltiples modificaciones
Ventas Comparativas = 
CALCULATE(
    [Total Ventas],
    Producto[Categoría] = "Electrónica",  // Agrega filtro
    ALL(Cliente[Región]),                  // Remueve filtro
    Calendario[Año] >= 2023                // Agrega filtro
)
```

**Filtros en CALCULATE:**
```DAX
// Filtro de tabla
CALCULATE([Total], FILTER(Producto, [Precio] > 100))

// Filtro de columna (más eficiente)
CALCULATE([Total], Producto[Categoría] = "Tech")

// Tabla virtual como filtro
CALCULATE([Total], SUMMARIZE(...))

// Funciones modificadoras de contexto
CALCULATE([Total], ALL(...), ALLEXCEPT(...))
```

### 🎯 Tip de Entrevista
**Menciona:** "**CALCULATE es el 80% de las medidas avanzadas**. Su poder está en modificar el contexto de filtro dinámicamente. Lo uso para crear comparativas (año anterior, presupuesto), aplicar filtros específicos sin afectar el modelo, y hacer transición de contexto en columnas calculadas. Entender que CALCULATE evalúa filtros de **forma independiente y luego los intersecta** es clave. También sé que los filtros de columna (`Tabla[Columna] = "Valor"`) son más eficientes que `FILTER`."

---

## **29. ¿Cómo funcionan las funciones de time intelligence?**

### 📖 Definición
Las **funciones de time intelligence** son un conjunto de funciones DAX especializadas para cálculos temporales (YTD, año anterior, trimestre móvil, etc.). Requieren una tabla de calendario correctamente configurada.

### 🔧 Explicación Práctica

**Requisitos obligatorios:**
1. Tabla de calendario continua (sin fechas faltantes)
2. Marcada como "Tabla de fechas" en Power BI
3. Relación 1:N con tablas de hechos
4. Columna de tipo Date

**Funciones principales:**

### **1. Comparaciones de períodos**
```DAX
// Año anterior
Ventas Año Anterior = 
CALCULATE(
    [Total Ventas],
    SAMEPERIODLASTYEAR(Calendario[Fecha])
)

// Mes anterior
Ventas Mes Anterior = 
CALCULATE(
    [Total Ventas],
    DATEADD(Calendario[Fecha], -1, MONTH)
)

// Trimestre anterior
Ventas Trimestre Anterior = 
CALCULATE(
    [Total Ventas],
    DATEADD(Calendario[Fecha], -1, QUARTER)
)
```

### **2. Acumulados (Year/Quarter/Month to Date)**
```DAX
// Year to Date
YTD Ventas = 
TOTALYTD([Total Ventas], Calendario[Fecha])

// Quarter to Date
QTD Ventas = 
TOTALQTD([Total Ventas], Calendario[Fecha])

// Month to Date
MTD Ventas = 
TOTALMTD([Total Ventas], Calendario[Fecha])
```

### **3. Promedios móviles**
```DAX
// Promedio últimos 3 meses
Promedio 3 Meses = 
CALCULATE(
    AVERAGE(Ventas[Monto]),
    DATESINPERIOD(
        Calendario[Fecha],
        LASTDATE(Calendario[Fecha]),
        -3,
        MONTH
    )
)
```

### **4. Rango de fechas personalizado**
```DAX
// Últimos 30 días
Ventas Últimos 30 Días = 
CALCULATE(
    [Total Ventas],
    DATESINPERIOD(
        Calendario[Fecha],
        MAX(Calendario[Fecha]),
        -30,
        DAY
    )
)

// Mismo período año anterior (rango)
Ventas Mismo Período Año Anterior = 
CALCULATE(
    [Total Ventas],
    DATESBETWEEN(
        Calendario[Fecha],
        DATEADD(MIN(Calendario[Fecha]), -1, YEAR),
        DATEADD(MAX(Calendario[Fecha]), -1, YEAR)
    )
)
```

### 💡 Ejemplo Completo - Dashboard de Ventas
```DAX
// 1. Total actual
Total Ventas = SUM(Ventas[Monto])

// 2. Año anterior
Ventas LY = 
CALCULATE([Total Ventas], SAMEPERIODLASTYEAR(Calendario[Fecha]))

// 3. Crecimiento YoY
YoY Growth = 
DIVIDE([Total Ventas] - [Ventas LY], [Ventas LY])

// 4. YTD actual
YTD = TOTALYTD([Total Ventas], Calendario[Fecha])

// 5. YTD año anterior
YTD LY = 
CALCULATE([YTD], SAMEPERIODLASTYEAR(Calendario[Fecha]))

// 6. Moving Annual Total (últimos 12 meses)
MAT = 
CALCULATE(
    [Total Ventas],
    DATESINPERIOD(
        Calendario[Fecha],
        LASTDATE(Calendario[Fecha]),
        -12,
        MONTH
    )
)

// 7. Promedio diario del mes
Promedio Diario Mes = 
AVERAGEX(
    DATESMTD(Calendario[Fecha]),
    [Total Ventas]
)
```

**Funciones útiles de manipulación de fechas:**
- `SAMEPERIODLASTYEAR()` - Mismo período año anterior
- `DATEADD()` - Desplazar período
- `PARALLELPERIOD()` - Período paralelo completo
- `TOTALYTD/QTD/MTD()` - Acumulados
- `DATESINPERIOD()` - Rango dinámico
- `DATESBETWEEN()` - Rango específico
- `LASTDATE()`, `FIRSTDATE()` - Primera/última fecha del contexto

### 🎯 Tip de Entrevista
**Menciona:** "Las funciones de time intelligence **simplifican enormemente los cálculos temporales**. Sin ellas, tendría que escribir CALCULATE con filtros complejos. La clave es tener una tabla de calendario bien diseñada y marcada correctamente. Uso `SAMEPERIODLASTYEAR` para comparativas YoY, `TOTALYTD` para acumulados, y `DATESINPERIOD` para ventanas móviles. Para casos más complejos como años fiscales, puedo usar el parámetro opcional de fin de año: `TOTALYTD([Ventas], Calendario[Fecha], "03-31")`."

---

## **30. ¿Qué son las variables VAR en DAX?**

### 📖 Definición
Las **variables (VAR)** permiten almacenar valores intermedios en una medida o columna calculada, mejorando legibilidad, rendimiento y facilitando el debugging.

### 🔧 Explicación Práctica

**Sintaxis:**
```DAX
MiMedida = 
VAR NombreVariable = <expresión>
VAR OtraVariable = <expresión>
RETURN
    <expresión usando variables>
```

**Ventajas:**

1. **Rendimiento**: La expresión se calcula UNA VEZ y se reutiliza
2. **Legibilidad**: Código más claro y mantenible
3. **Debugging**: Facilita identificar errores
4. **Reutilización**: Usa el mismo cálculo múltiples veces

### 💡 Ejemplos

### **Sin Variables (Mal)**
```DAX
Margen % = 
DIVIDE(
    SUM(Ventas[Precio]) - SUM(Ventas[Costo]),
    SUM(Ventas[Precio])
)
// Problema: Calcula SUM(Ventas[Precio]) DOS veces
```

### **Con Variables (Bien)**
```DAX
Margen % = 
VAR TotalVentas = SUM(Ventas[Precio])
VAR TotalCosto = SUM(Ventas[Costo])
VAR Margen = TotalVentas - TotalCosto
RETURN
    DIVIDE(Margen, TotalVentas)
// Cada cálculo se ejecuta UNA vez, mejor rendimiento
```

### **Caso Avanzado: Comparativas**
```DAX
YoY Growth = 
VAR VentasActual = [Total Ventas]
VAR VentasAñoAnterior = 
    CALCULATE(
        [Total Ventas],
        SAMEPERIODLASTYEAR(Calendario[Fecha])
    )
VAR DiferenciaAbsoluta = VentasActual - VentasAñoAnterior
VAR DiferenciaRelativa = DIVIDE(DiferenciaAbsoluta, VentasAñoAnterior)
RETURN
    DiferenciaRelativa
```

### **Variables con Tablas**
```DAX
Top 5 Clientes Ventas = 
VAR TablaTop5 = 
    TOPN(
        5,
        VALUES(Cliente[Nombre]),
        [Total Ventas],
        DESC
    )
VAR VentasTop5 = 
    CALCULATE(
        [Total Ventas],
        TablaTop5
    )
RETURN
    VentasTop5
```

### **Variables para Debugging**
```DAX
Medida Compleja Debug = 
VAR Paso1 = SUM(Ventas[Monto])
VAR Paso2 = CALCULATE([Total Ventas], ALL(Producto))
VAR Paso3 = DIVIDE(Paso1, Paso2)
// Puedo comentar RETURN y retornar cualquier VAR para debuggear
RETURN
    Paso3
// Para debug: RETURN Paso1 o RETURN Paso2
```

### **Ámbito de Variables**
```DAX
Medida con Alcance = 
VAR VariableExterna = [Total Ventas]  // Disponible en todo el scope
RETURN
    CALCULATE(
        VAR VariableInterna = VariableExterna * 1.1  // Solo disponible aquí
        RETURN VariableInterna,
        ALL(Producto)
    )
```

### 🎯 Tip de Entrevista
**Menciona:** "Uso variables en **prácticamente todas mis medidas complejas**. Mejoran el rendimiento porque DAX no recalcula la expresión cada vez que se usa. Son esenciales para debugging: puedo aislar cada paso y verificar resultados intermedios. Una buena práctica es nombrar variables descriptivamente (VentasActual, VentasLY, Crecimiento) en lugar de Var1, Var2. Las variables también capturan el contexto en el momento de su definición, lo cual es útil en cálculos con múltiples CALCULATE."

---

## **31. ¿Cuándo usar SUMX vs SUM?**

### 📖 Definición
- **SUM**: Función de agregación simple que suma una columna
- **SUMX**: Iterador que evalúa una expresión fila por fila y luego suma los resultados

### 🔧 Explicación Práctica

**Diferencia fundamental:**
- **SUM** opera sobre una columna existente
- **SUMX** permite calcular algo por cada fila y luego sumar

### **Usa SUM cuando:**
- Solo necesitas sumar una columna existente
- No hay cálculo por fila
- Más eficiente

```DAX
Total Ventas = SUM(Ventas[Monto])
Total Cantidad = SUM(Ventas[Cantidad])
```

### **Usa SUMX cuando:**
- Necesitas calcular algo por fila primero
- Multiplicas columnas
- Aplicas lógica condicional por fila
- Trabajas con tablas virtuales

```DAX
// Multiplicar columnas (no existe columna precalculada)
Total Revenue = 
SUMX(
    Ventas,
    Ventas[Precio] * Ventas[Cantidad]
)

// Con descuento por fila
Revenue con Descuento = 
SUMX(
    Ventas,
    Ventas[Precio] * Ventas[Cantidad] * (1 - Ventas[Descuento%])
)

// Con lógica condicional
Ventas con Bonus = 
SUMX(
    Ventas,
    VAR Monto = Ventas[Precio] * Ventas[Cantidad]
    VAR Bonus = IF(Monto > 1000, Monto * 0.1, 0)
    RETURN Monto + Bonus
)
```

### 💡 Ejemplo Comparativo

**Escenario: Calcular revenue total**

```DAX
// Tabla Ventas:
// Precio | Cantidad
// 10     | 5
// 20     | 3
// 15     | 2

// ❌ SUM NO FUNCIONA para multiplicar
Revenue Incorrecto = SUM(Ventas[Precio]) * SUM(Ventas[Cantidad])
// = (10+20+15) * (5+3+2) = 45 * 10 = 450 ❌ INCORRECTO

// ✅ SUMX es correcto
Revenue Correcto = 
SUMX(
    Ventas,
    [Precio] * [Cantidad]
)
// = (10*5) + (20*3) + (15*2) = 50 + 60 + 30 = 140 ✅ CORRECTO
```

### **Familia de Iteradores**
```DAX
// SUMX - Suma iterando
Total = SUMX(Tabla, [Columna] * 2)

// AVERAGEX - Promedio iterando
PromedioMargen = AVERAGEX(Ventas, [Precio] - [Costo])

// COUNTX - Cuenta iterando (con condición)
ProductosCaros = COUNTX(FILTER(Producto, [Precio] > 100), [ProductoID])

// MINX / MAXX - Mínimo/Máximo iterando
MejorMargen = MAXX(Ventas, [Precio] - [Costo])

// CONCATENATEX - Concatena iterando
ListaClientes = CONCATENATEX(Cliente, [Nombre], ", ")

// PRODUCTX - Multiplica iterando (raro)
ProductoTotal = PRODUCTX(Tabla, [Valor])
```

### **SUMX con Tabla Virtual**
```DAX
// Sumar solo ventas de productos premium
Ventas Premium = 
SUMX(
    FILTER(Ventas, RELATED(Producto[Categoría]) = "Premium"),
    [Precio] * [Cantidad]
)

// Sumar top 10 productos
Ventas Top 10 = 
SUMX(
    TOPN(10, Producto, [Total Ventas], DESC),
    [Total Ventas]
)
```

### 🎯 Tip de Entrevista
**Menciona:** "**SUM es más eficiente, pero SUMX es más flexible**. Uso SUM cuando la columna ya existe y solo necesito agregarla. SUMX es necesario cuando hago cálculos fila por fila como `Precio * Cantidad * (1 - Descuento)`. Un error común es usar `SUM(A) * SUM(B)` cuando se necesita `SUMX(Tabla, [A] * [B])`. Entender iteradores es clave para DAX avanzado - AVERAGEX, COUNTX, MAXX funcionan bajo el mismo principio."

---

## **32. ¿Qué es el filter context vs row context?**

### 📖 Definición
Son los dos tipos fundamentales de contexto de evaluación en DAX:

- **Filter Context**: Determina qué filas están disponibles para agregación (SUM, COUNT, etc.)
- **Row Context**: Permite evaluar expresiones fila por fila

### 🔧 Explicación Práctica

### **FILTER CONTEXT**

**Qué lo crea:**
- Slicers y filtros en reportes
- Filtros de visual
- Filtros de página/reporte
- CALCULATE
- Relaciones del modelo

**Características:**
- Se propaga automáticamente por relaciones
- Afecta a medidas
- Es acumulativo (múltiples filtros se intersectan)

```DAX
// Medida con filter context
Total Ventas = SUM(Ventas[Monto])
// El contexto determina qué filas se suman
// Si hay slicer de "Año = 2025", solo suma ese año
```

### **ROW CONTEXT**

**Qué lo crea:**
- Columnas calculadas (automático, fila actual)
- Funciones iteradoras: SUMX, FILTER, ADDCOLUMNS
- Funciones de tabla: GENERATE, GENERATESERIES

**Características:**
- NO se propaga por relaciones (necesitas RELATED)
- Evalúa fila por fila
- Permite acceder a columnas de la fila actual

```DAX
// Columna calculada con row context
Margen = Ventas[Precio] - Ventas[Costo]
// Evalúa cada fila independientemente
// [Precio] y [Costo] son de la fila actual

// Iterador con row context
Total Revenue = 
SUMX(
    Ventas,  // Crea row context para cada fila de Ventas
    [Precio] * [Cantidad]  // Accede a columnas de la fila actual
)
```

### 💡 Ejemplos Contrastados

### **1. Acceso a Tablas Relacionadas**
```DAX
// ❌ EN MEDIDA - No hay row context
Precio Promedio = AVERAGE(Ventas[Precio])  // ✅ Funciona
Categoría = Producto[Categoría]  // ❌ ERROR - No hay fila específica

// ✅ EN COLUMNA CALCULADA - Hay row context
Categoría Producto = RELATED(Producto[Categoría])  // ✅ Funciona
// RELATED usa row context para encontrar el producto relacionado
```

### **2. Combinación de Contextos**
```DAX
// Row context + Filter context
Ventas del Producto = 
SUMX(
    Producto,  // Row context: itera cada producto
    CALCULATE([Total Ventas])  // Filter context: filtra ventas del producto actual
)
// CALCULATE hace transición de contexto:
// Convierte row context en filter context
```

### **3. FILTER Requiere Row Context**
```DAX
// FILTER crea row context
Ventas Productos Caros = 
CALCULATE(
    [Total Ventas],
    FILTER(
        Producto,  // Tabla para iterar
        [Precio] > 100  // Se evalúa para cada fila (row context)
    )
)
```

### **4. Iteradores Mantienen Ambos**
```DAX
// Si hay slicer de Año = 2025 (filter context)
Revenue 2025 = 
SUMX(
    Ventas,  // Row context de cada fila
    [Precio] * [Cantidad]  // Usa row context
)
// SUMX respeta el filter context (solo filas de 2025)
// Y crea row context para evaluar la expresión
```

### **Tabla Comparativa**

| **Aspecto** | **Filter Context** | **Row Context** |
|------------|-------------------|-----------------|
| **Creado por** | Slicers, filtros, CALCULATE | Columnas calculadas, iteradores |
| **Afecta a** | Medidas (agregaciones) | Acceso a columnas de fila |
| **Propagación** | Sí (por relaciones) | No (necesita RELATED) |
| **Visibilidad** | Qué filas están disponibles | Una fila a la vez |
| **Modificar** | CALCULATE, ALL, FILTER | Iteradores (SUMX, FILTER) |

### 🎯 Tip de Entrevista
**Menciona:** "Entender la diferencia entre **filter y row context es lo que separa usuarios básicos de avanzados**. Filter context es 'qué datos veo' (afectado por slicers), row context es 'estoy evaluando esta fila específica'. El error más común es esperar que row context acceda automáticamente a tablas relacionadas - para eso está `RELATED`. `CALCULATE` es poderoso porque puede hacer **transición de contexto**: convierte row context en filter context, lo que permite agregar dentro de iteradores."

---

## **33. ¿Cómo crear medidas dinámicas con SWITCH?**

### 📖 Definición
**SWITCH** es una función que evalúa una expresión contra múltiples valores y retorna el resultado correspondiente. Es ideal para crear medidas dinámicas controladas por slicers o parámetros.

### 🔧 Explicación Práctica

**Sintaxis:**
```DAX
SWITCH(
    <expresión>,
    <valor1>, <resultado1>,
    <valor2>, <resultado2>,
    ...,
    <resultado_por_defecto>
)
```

### **Caso de Uso: Slicer de Métricas**

```DAX
// 1. Crear tabla de métricas (Enter Data)
Métricas = {
    ("Ventas", 1),
    ("Costo", 2),
    ("Margen", 3),
    ("Cantidad", 4)
}

// 2. Crear medida dinámica
Medida Seleccionada = 
SWITCH(
    SELECTEDVALUE(Métricas[Métrica]),
    "Ventas", [Total Ventas],
    "Costo", [Total Costo],
    "Margen", [Total Ventas] - [Total Costo],
    "Cantidad", SUM(Ventas[Cantidad]),
    BLANK()  // Default si no hay selección
)

// 3. Agregar slicer con Métricas[Métrica]
// El usuario puede cambiar dinámicamente qué ve
```

### 💡 Ejemplos Avanzados

### **1. Switch con TRUE (Múltiples Condiciones)**
```DAX
Clasificación Ventas = 
VAR TotalVentas = [Total Ventas]
RETURN
SWITCH(
    TRUE(),
    TotalVentas > 1000000, "Platinum",
    TotalVentas > 500000, "Gold",
    TotalVentas > 100000, "Silver",
    "Bronze"
)
// Evalúa condiciones en orden y retorna primer TRUE
```

### **2. Formato Dinámico**
```DAX
Formato Dinámico = 
VAR Metrica = SELECTEDVALUE(Métricas[Métrica])
VAR Valor = [Medida Seleccionada]
RETURN
SWITCH(
    Metrica,
    "Ventas", FORMAT(Valor, "$#,##0"),
    "Margen %", FORMAT(Valor, "0.0%"),
    "Cantidad", FORMAT(Valor, "#,##0"),
    FORMAT(Valor, "General Number")
)
```

### **3. Períodos Dinámicos**
```DAX
// Tabla de períodos
Períodos = {
    ("Mes Actual", 1),
    ("YTD", 2),
    ("Año Anterior", 3),
    ("YTD Año Anterior", 4)
}

// Medida dinámica de período
Ventas Período Dinámico = 
SWITCH(
    SELECTEDVALUE(Períodos[Período]),
    "Mes Actual", [Total Ventas],
    "YTD", [YTD Ventas],
    "Año Anterior", [Ventas LY],
    "YTD Año Anterior", [YTD LY],
    BLANK()
)
```

### **4. Cálculos Comparativos Dinámicos**
```DAX
Comparativa Dinámica = 
VAR TipoComparacion = SELECTEDVALUE(Comparaciones[Tipo])
VAR Actual = [Total Ventas]
VAR Anterior = 
    SWITCH(
        TipoComparacion,
        "vs Mes Anterior", [Ventas Mes Anterior],
        "vs Año Anterior", [Ventas LY],
        "vs Presupuesto", [Presupuesto]
    )
VAR Diferencia = Actual - Anterior
VAR Porcentaje = DIVIDE(Diferencia, Anterior)
RETURN
    SWITCH(
        SELECTEDVALUE(Comparaciones[Mostrar]),
        "Diferencia", Diferencia,
        "Porcentaje", Porcentaje,
        "Ambos", Diferencia & " (" & FORMAT(Porcentaje, "0.0%") & ")"
    )
```

### **5. SWITCH vs IF Anidados**
```DAX
// ❌ IF Anidados (difícil de leer)
Categoría = 
IF([Ventas] > 1000000, "A",
    IF([Ventas] > 500000, "B",
        IF([Ventas] > 100000, "C", "D")
    )
)

// ✅ SWITCH con TRUE (más limpio)
Categoría = 
SWITCH(
    TRUE(),
    [Ventas] > 1000000, "A",
    [Ventas] > 500000, "B",
    [Ventas] > 100000, "C",
    "D"
)
```

### **Combinación con Field Parameters (Power BI)**
```DAX
// Power BI permite crear parámetros de campo directamente
// El usuario puede arrastrar campos dinámicamente
// SWITCH complementa esto para lógica adicional
```

### 🎯 Tip de Entrevista
**Menciona:** "**SWITCH es fundamental para dashboards interactivos**. Lo uso con slicers para crear medidas dinámicas donde el usuario elige qué métrica ver (Ventas, Margen, Cantidad). `SWITCH(TRUE(), ...)` es mi forma preferida de reemplazar IF anidados - es más legible y mantenible. En proyectos reales, creo tablas desconectadas con opciones de métricas/períodos y uso SWITCH con `SELECTEDVALUE()` para hacer dashboards verdaderamente dinámicos. Es más eficiente que crear múltiples visuales con diferentes medidas."

---

## **34. ¿Qué es USERELATIONSHIP y cuándo usarlo?**

### 📖 Definición
**USERELATIONSHIP** activa temporalmente una relación inactiva en el modelo de datos durante la evaluación de una medida, permitiendo usar múltiples relaciones entre las mismas tablas.

### 🔧 Explicación Práctica

**El problema:**
- Power BI solo permite UNA relación activa entre dos tablas
- A veces necesitas múltiples perspectivas de fechas
- Ejemplo: Ventas por fecha de pedido vs fecha de envío vs fecha de pago

**La solución:**
- Crea múltiples relaciones (solo una activa)
- Marca las adicionales como inactivas (línea punteada)
- Usa USERELATIONSHIP en medidas para activarlas temporalmente

### 💡 Ejemplo: Fechas Múltiples

**Escenario: Tabla Ventas con múltiples fechas**

```
Modelo:
Calendario ─(Activa)──→ Ventas[FechaPedido]
Calendario ─(Inactiva)─→ Ventas[FechaEnvío]
Calendario ─(Inactiva)─→ Ventas[FechaPago]
```

```DAX
// Medida por defecto (usa relación activa - FechaPedido)
Ventas por Fecha Pedido = 
SUM(Ventas[Monto])

// Activar relación por FechaEnvío
Ventas por Fecha Envío = 
CALCULATE(
    SUM(Ventas[Monto]),
    USERELATIONSHIP(Ventas[FechaEnvío], Calendario[Fecha])
)

// Activar relación por FechaPago
Ventas por Fecha Pago = 
CALCULATE(
    SUM(Ventas[Monto]),
    USERELATIONSHIP(Ventas[FechaPago], Calendario[Fecha])
)
```

### **Caso de Uso: Análisis de Demora**
```DAX
// Días entre pedido y envío
Días Promedio Envío = 
VAR TablaDiferencias = 
    ADDCOLUMNS(
        Ventas,
        "Días", Ventas[FechaEnvío] - Ventas[FechaPedido]
    )
RETURN
    AVERAGEX(TablaDiferencias, [Días])

// Ventas enviadas en el período (diferente a pedidas)
Ventas Enviadas en Período = 
CALCULATE(
    [Total Ventas],
    USERELATIONSHIP(Ventas[FechaEnvío], Calendario[Fecha])
)
```

### **Caso de Uso: Presupuesto vs Actual**
```DAX
// Modelo:
// Calendario ─(Activa)──→ Ventas[Fecha]
// Calendario ─(Inactiva)─→ Presupuesto[Fecha]

// Ventas (usa relación activa automáticamente)
Total Ventas = SUM(Ventas[Monto])

// Presupuesto (necesita activar relación inactiva)
Total Presupuesto = 
CALCULATE(
    SUM(Presupuesto[Monto]),
    USERELATIONSHIP(Presupuesto[Fecha], Calendario[Fecha])
)

// Variancia
Variancia = [Total Ventas] - [Total Presupuesto]

// Variancia %
Variancia % = DIVIDE([Variancia], [Total Presupuesto])
```

### **Múltiples USERELATIONSHIP**
```DAX
// Comparar ventas por diferentes fechas
Análisis Fechas = 
VAR PorPedido = [Total Ventas]  // Activa por defecto
VAR PorEnvío = 
    CALCULATE(
        [Total Ventas],
        USERELATIONSHIP(Ventas[FechaEnvío], Calendario[Fecha])
    )
VAR PorPago = 
    CALCULATE(
        [Total Ventas],
        USERELATIONSHIP(Ventas[FechaPago], Calendario[Fecha])
    )
RETURN
    "Pedido: " & PorPedido & 
    " | Envío: " & PorEnvío & 
    " | Pago: " & PorPago
```

### **Alternativa: Tablas de Calendario Separadas**
```DAX
// En algunos casos, es mejor crear calendarios separados
CalendarioPedido ─→ Ventas[FechaPedido]
CalendarioEnvío ─→ Ventas[FechaEnvío]
CalendarioPago ─→ Ventas[FechaPago]

// Ventaja: Slicers independientes para cada fecha
// Desventaja: Más complejo, más tablas
```

### 🎯 Tip de Entrevista
**Menciona:** "**USERELATIONSHIP resuelve el problema de múltiples relaciones de fecha**. Power BI solo permite una relación activa entre dos tablas, pero con USERELATIONSHIP puedo tener múltiples relaciones inactivas y activarlas según necesidad. Caso típico: Ventas por fecha de pedido (activa) vs fecha de envío (inactiva). Lo uso dentro de CALCULATE para cambiar temporalmente qué relación se usa. Alternativa: crear múltiples tablas de calendario, pero USERELATIONSHIP es más simple cuando solo necesitas cambiar en medidas, no en slicers."

---

## **35. ¿Cómo optimizar fórmulas DAX lentas?**

### 📖 Definición
La **optimización de DAX** implica identificar y corregir medidas o columnas que causan lentitud en reportes, aplicando mejores prácticas de escritura y diseño.

### 🔧 Explicación Práctica

### **1. Herramientas de Diagnóstico**

**Performance Analyzer (Power BI Desktop)**
- Ver → Performance Analyzer
- Graba qué visuales y consultas son lentas
- Identifica medidas problemáticas

**DAX Studio (herramienta externa)**
- Analiza queries DAX
- Mide tiempos de ejecución
- Identifica cuellos de botella
- Server Timings y Query Plan

### **2. Optimizaciones Fundamentales**

#### **A. Variables para Reutilización**
```DAX
// ❌ MAL - Calcula 3 veces
Margen = 
DIVIDE(
    [Total Ventas] - [Total Costo],
    [Total Ventas]
) + 
IF([Total Ventas] > 100000, 0.05, 0)

// ✅ BIEN - Calcula 1 vez
Margen = 
VAR Ventas = [Total Ventas]
VAR Costo = [Total Costo]
VAR MargenBase = DIVIDE(Ventas - Costo, Ventas)
VAR Bonus = IF(Ventas > 100000, 0.05, 0)
RETURN MargenBase + Bonus
```

#### **B. Filtros de Columna vs FILTER**
```DAX
// ❌ LENTO - FILTER itera toda la tabla
Ventas Premium = 
CALCULATE(
    [Total Ventas],
    FILTER(Producto, [Categoría] = "Premium")
)

// ✅ RÁPIDO - Filtro de columna (más eficiente)
Ventas Premium = 
CALCULATE(
    [Total Ventas],
    Producto[Categoría] = "Premium"
)

// ✅ FILTER solo cuando necesitas lógica compleja
Ventas Complejas = 
CALCULATE(
    [Total Ventas],
    FILTER(Producto, [Precio] > 100 && [Stock] > 0)
)
```

#### **C. Usar TREATAS en lugar de FILTER para Relaciones**
```DAX
// ❌ Menos eficiente
Ventas Categoría = 
CALCULATE(
    [Total Ventas],
    FILTER(
        ALL(Producto),
        Producto[Categoría] = SELECTEDVALUE(Categorías[Categoría])
    )
)

// ✅ Más eficiente
Ventas Categoría = 
CALCULATE(
    [Total Ventas],
    TREATAS(
        VALUES(Categorías[Categoría]),
        Producto[Categoría]
    )
)
```

#### **D. Evitar Iteradores Innecesarios**
```DAX
// ❌ Innecesario - Ya existe columna
Total Monto = SUMX(Ventas, [Monto])

// ✅ Directo
Total Monto = SUM(Ventas[Monto])

// ✅ SUMX solo cuando calculas algo
Revenue = SUMX(Ventas, [Precio] * [Cantidad])
```

### **3. Optimizaciones de Modelo**

#### **A. Medidas vs Columnas Calculadas**
```DAX
// ❌ Columna calculada (consume memoria)
Ventas[TotalPorProducto] = 
CALCULATE([Total Ventas], ALL(Ventas), VALUES(Ventas[ProductoID]))
// Se guarda en cada fila → millones de filas = problema

// ✅ Medida (calcula bajo demanda)
Total Por Producto = 
CALCULATE([Total Ventas], ALLEXCEPT(Ventas, Ventas[ProductoID]))
```

#### **B. Reducir Cardinalidad**
- Elimina columnas innecesarias
- Agrupa valores cuando sea posible
- Usa tipos de datos más pequeños
- Evita columnas de texto largo

#### **C. Relaciones Correctas**
- Usa relaciones 1:N (evita N:M cuando sea posible)
- Evita relaciones bidireccionales innecesarias
- Desactiva relaciones no usadas

### **4. Técnicas Avanzadas**

#### **A. Dividir Medidas Complejas**
```DAX
// En lugar de una medida gigante, crear medidas base
_Base Ventas = SUM(Ventas[Monto])
_Base Costo = SUM(Ventas[Costo])
_Base Margen = [_Base Ventas] - [_Base Costo]

// Medida final usa las bases
Margen % = DIVIDE([_Base Margen], [_Base Ventas])
// Nota: _ indica medida oculta/interna
```

#### **B. Materializar Cálculos en Power Query**
```DAX
// Si Precio * Cantidad se usa mucho
// En lugar de:
Revenue = SUMX(Ventas, [Precio] * [Cantidad])

// Crear columna en Power Query:
// Revenue = [Precio] * [Cantidad]
// Luego en DAX:
Revenue = SUM(Ventas[Revenue])
// Trade-off: más espacio, pero más rápido
```

#### **C. Usar Valores Específicos en lugar de ALL**
```DAX
// ❌ Menos eficiente
Ventas Todas Regiones = 
CALCULATE([Total Ventas], ALL(Cliente[Región]))

// ✅ Más específico si hay pocos valores
Ventas Todas Regiones = 
CALCULATE(
    [Total Ventas],
    Cliente[Región] IN {"Norte", "Sur", "Este", "Oeste"}
)
```

### **5. Checklist de Optimización**

✅ **Usar variables** para expresiones repetidas  
✅ **Filtros de columna** en lugar de FILTER cuando sea posible  
✅ **Medidas** en lugar de columnas calculadas para agregaciones  
✅ **Evitar iteradores** innecesarios (SUMX cuando basta SUM)  
✅ **Relaciones correctas** (1:N, unidireccionales)  
✅ **Eliminar columnas** no usadas  
✅ **SELECTEDVALUE** en lugar de VALUES + HASONEVALUE  
✅ **Materializar en Power Query** cálculos simples repetitivos  
✅ **Dividir medidas complejas** en componentes  
✅ **Performance Analyzer** para identificar problemas  

### 🎯 Tip de Entrevista
**Menciona:** "La optimización de DAX empieza con **variables para evitar recálculo**. Uso filtros de columna (`Tabla[Col] = "Valor"`) en lugar de FILTER cuando es posible - son órdenes de magnitud más rápidos. Siempre mido con **Performance Analyzer** para identificar cuellos de botella reales, no optimizo prematuramente. En proyectos grandes, considero materializar cálculos simples en Power Query (como Precio * Cantidad) si se usan frecuentemente. Para diagnóstico profundo, uso **DAX Studio** para ver query plans y tiempos de motor."

---

## 🎓 **Resumen de Conceptos Clave de DAX Avanzado**

✅ **Contexto** (Filter + Row) es el concepto fundamental  
✅ **ALL vs ALLSELECTED vs ALLEXCEPT** para modificar filtros  
✅ **CALCULATE** modifica contexto y hace transición  
✅ **Time Intelligence** requiere tabla de calendario correcta  
✅ **Variables VAR** mejoran rendimiento y legibilidad  
✅ **SUMX** cuando calculas por fila, **SUM** para columnas simples  
✅ **Filter context** no se propaga, **row context** necesita RELATED  
✅ **SWITCH** para medidas dinámicas e interactivas  
✅ **USERELATIONSHIP** para múltiples relaciones de fecha  
✅ **Optimización**: variables, filtros de columna, medidas base  

---

**💡 Consejo Final**: DAX avanzado se domina entendiendo el **contexto de evaluación**. En entrevistas, demuestra que comprendes cómo CALCULATE modifica contextos, cuándo usar iteradores vs agregadores simples, y cómo optimizar para rendimiento. Los entrevistadores valoran experiencia con time intelligence, medidas dinámicas y técnicas de optimización aplicadas a proyectos reales.
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
## **41. ¿Qué es un workspace en Power BI Service?**

### 📖 Definición
Un **workspace** (espacio de trabajo) es un contenedor en Power BI Service donde se organizan, colaboran y comparten reportes, dashboards, datasets y dataflows. Es la unidad fundamental de colaboración y gestión de contenido en Power BI.

### 🔧 Explicación Práctica

**Estructura jerárquica:**
```
Organización
├─ Workspace "Ventas"
│  ├─ Dataset: Ventas2025
│  ├─ Report: Dashboard Ventas
│  ├─ Dashboard: Ejecutivo
│  └─ Dataflow: ETL Ventas
├─ Workspace "Marketing"
│  ├─ Dataset: Campañas
│  └─ Report: Performance Marketing
└─ Workspace "Finanzas"
   └─ ...
```

### **Tipos de Workspaces:**

### **1. Mi Espacio de Trabajo (My Workspace)**
```
Características:
- Personal, privado
- Solo tú puedes acceder
- No se puede compartir con otros
- Ideal para desarrollo/pruebas
- No tiene roles de acceso

Uso: Desarrollo personal, pruebas, reportes no publicados
```

### **2. Workspaces (Colaborativos)**
```
Características:
- Múltiples usuarios con diferentes roles
- Contenido compartido
- Control de acceso granular
- Respaldados por capacidad (Pro/Premium)
- Permiten colaboración

Uso: Proyectos de equipo, producción, distribución
```

### **Roles en Workspaces:**

| **Rol** | **Ver** | **Editar** | **Publicar** | **Compartir** | **Gestionar Acceso** | **Actualizar Dataset** |
|---------|---------|------------|--------------|---------------|---------------------|----------------------|
| **Admin** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Member** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **Contributor** | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **Viewer** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |

**Detalles de roles:**
```
Admin:
- Control total del workspace
- Agregar/remover usuarios
- Eliminar workspace
- Actualizar configuración

Member:
- Crear y publicar contenido
- Modificar contenido existente
- Compartir contenido fuera del workspace

Contributor:
- Crear y publicar contenido
- No puede compartir fuera del workspace
- Ideal para desarrolladores

Viewer:
- Solo lectura
- Consume reportes y dashboards
- No puede modificar nada
```

### 💡 Ejemplo de Estructura Organizacional

```
EMPRESA ABC

┌─────────────────────────────────────┐
│ Workspace: "Ventas - Producción"    │
│ Roles:                              │
│ - Admin: Gerente BI                 │
│ - Member: Analistas BI (3)          │
│ - Viewer: Gerentes Ventas (10)      │
│ Contenido:                          │
│ - Dataset: VentasConsolidadas       │
│ - Report: Dashboard Ventas Diario   │
│ - Dashboard: Ejecutivo CEO          │
└─────────────────────────────────────┘
```

### **Mejores Prácticas:**

```
✅ Nomenclatura:
- "Área - Ambiente" (ej: "Ventas - Prod", "Ventas - Dev")
- Descriptivo y consistente

✅ Estructura:
- Separar desarrollo y producción
- Un workspace por área/proyecto
- No mezclar contenido no relacionado

✅ Seguridad:
- Principio de mínimo privilegio
- Usar grupos de seguridad de AD
- Revisar accesos periódicamente

✅ Organización:
- Workspace para datos compartidos (datasets)
- Workspace para reportes específicos de área
- Workspace de "Sandbox" para pruebas
```

### 🎯 Tip de Entrevista
**Menciona:** "Los workspaces son **fundamentales para la colaboración y gobierno**. Implemento una estrategia que separa desarrollo, QA y producción. Uso **roles granulares**: Admin para líderes técnicos, Member para analistas, Contributor para desarrolladores, Viewer para consumidores. Best practice: asigno acceso mediante **grupos de AD** en lugar de usuarios individuales. Para grandes organizaciones, creo workspaces por área de negocio y un workspace central para datasets compartidos. Importante: workspace en **Premium** no requiere licencias Pro para viewers."

---

## **42. ¿Qué diferencia hay entre Pro y Premium?**

### 📖 Definición
**Power BI Pro** y **Power BI Premium** son dos modelos de licenciamiento que determinan las capacidades, límites y costos:

- **Pro**: Licencia por usuario (~$10/mes)
- **Premium Per User**: Licencia por usuario con características avanzadas (~$20/mes)
- **Premium Per Capacity**: Capacidad dedicada (~$5,000+/mes)

### 🔧 Explicación Práctica

### **Tabla Comparativa:**

| **Característica** | **Pro** | **Premium Per User** | **Premium Capacity** |
|-------------------|---------|---------------------|---------------------|
| **Costo** | $10/usuario/mes | $20/usuario/mes | $5,000+/mes |
| **Consumidor necesita licencia** | ✅ Pro | ✅ PPU | ❌ No (gratis) |
| **Actualización/día** | 8 | 48 | Ilimitado |
| **Tamaño dataset** | 1 GB | 100 GB | 400 GB |
| **Paginated Reports** | ❌ | ✅ | ✅ |
| **Deployment Pipelines** | ❌ | ✅ | ✅ |
| **Incremental Refresh** | ❌ | ✅ | ✅ |
| **AI/AutoML** | ❌ | ✅ | ✅ |
| **XMLA Endpoint** | ❌ | ✅ | ✅ |

### 💡 Ejemplo de Decisión

**Escenario 1: Startup (20 usuarios)**
```
Opción: Pro
- 20 usuarios × $10 = $200/mes
Justificación: Pocos usuarios, no necesita características avanzadas
```

**Escenario 2: Empresa (5,000 usuarios)**
```
Opción: Premium Capacity P3
- Capacidad = $20,000/mes
- Consumidores gratis
Justificación: 5,000 Pro = $50,000/mes vs P3 = $20,000/mes
Ahorro + Características enterprise
```

### 🎯 Tip de Entrevista
**Menciona:** "La elección depende de **escala y características**. **Pro** para equipos pequeños (<100 usuarios). **Premium Per User** cuando necesitas características avanzadas pero usuarios limitados. **Premium Capacity** se justifica con **distribución masiva** (>500 usuarios) - consumidores no necesitan licencia, el ROI es claro. Otras razones: incremental refresh para big data, paginated reports, deployment pipelines, y capacidad dedicada para performance predecible."

---

## **43. ¿Cómo funciona el Gateway?**

### 📖 Definición
El **Power BI Gateway** es un puente que conecta Power BI Service (cloud) con fuentes de datos on-premises, facilitando actualizaciones y consultas DirectQuery de manera segura.

### 🔧 Explicación Práctica

### **Arquitectura:**

```
POWER BI SERVICE (Cloud)
         ↓ HTTPS (puerto 443)
ON-PREMISES DATA GATEWAY
         ↓ Conexiones locales
FUENTES DE DATOS (SQL, Oracle, archivos, etc.)
```

### **Tipos de Gateway:**

**1. Standard (Enterprise)**
- Múltiples usuarios y datasets
- Para producción
- Instalación en servidor

**2. Personal**
- Un usuario únicamente
- Para desarrollo/pruebas
- Instalación en PC personal

### 💡 Configuración Paso a Paso

**PASO 1: Instalación**
```
1. Descargar de powerbi.microsoft.com/gateway
2. Requisitos: Windows Server, 8GB+ RAM, SSD
3. Instalar y registrar con cuenta Power BI
4. Recovery key (¡guardar seguro!)
```

**PASO 2: Agregar Fuente de Datos**
```
Power BI Service → Manage gateways:
1. Seleccionar Gateway
2. Add data source
3. Configurar: Server, Database, Credentials
4. Test connection
```

**PASO 3: Configurar Dataset**
```
Dataset Settings → Gateway connection:
1. Seleccionar Gateway
2. Seleccionar fuente de datos
3. Scheduled refresh → Configurar horarios
```

### **Mejores Prácticas:**

```
✅ Hardware:
- Servidor dedicado (no PC usuario)
- 16-32 GB RAM
- SSD para performance
- Múltiples cores (4-8)

✅ Seguridad:
- Usar cuentas de servicio
- Recovery key en lugar seguro
- Auditoría de logs

✅ Alta Disponibilidad:
- Cluster de gateways
- Failover automático
- Balanceo de carga

✅ Performance:
- Gateway cerca de fuentes de datos
- Optimizar queries
- Monitorear recursos
```

### 🎯 Tip de Entrevista
**Menciona:** "El Gateway es el **puente seguro entre cloud y on-premises**. Lo instalo en **servidor dedicado** (16GB RAM, SSD) en la misma red que fuentes para minimizar latencia. Uso **cuentas de servicio**, implemento **alta disponibilidad con cluster**, y **monitoreo constante**. Para troubleshooting verifico: Gateway online, credenciales válidas, conectividad, permisos. En grandes organizaciones, separo gateways por ambiente (Dev/Prod) y por criticidad."

---

## **44. ¿Qué son los dataflows?**

### 📖 Definición
Los **dataflows** son recursos de ETL en Power BI Service que permiten crear, reutilizar y centralizar transformaciones de datos en la nube, funcionando como capa de datos común que múltiples datasets consumen.

### 🔧 Explicación Práctica

**Concepto:**
```
Antes (sin Dataflows):
Dataset A → [ETL] → Fuente
Dataset B → [ETL] → Fuente  ← Duplicación
Dataset C → [ETL] → Fuente

Con Dataflows:
Dataflow [ETL] → Dataset A
               → Dataset B  ← Single source of truth
               → Dataset C
```

### **Beneficios:**

```
✅ Reutilización: ETL definido una vez
✅ Gobernanza: Lógica centralizada
✅ Performance: Cómputo en Azure
✅ Colaboración: Separación de responsabilidades
```

### 💡 Ejemplo de Uso

**Centralizar Dimensión Cliente:**
```
Problema:
- 5 datasets usan tabla Clientes
- Transformaciones duplicadas
- Inconsistencias

Solución:
1. Crear Dataflow "MaestroClientes"
2. Conectar a CRM y transformar
3. Datasets consumen el dataflow
4. Cambios en un solo lugar
```

### **Arquitectura:**

```
FUENTES → DATAFLOW (Power Query en cloud) 
       → STORAGE (Azure Data Lake, CDM)
       → CONSUMIDORES (Datasets, Apps, Excel)
```

### **Mejores Prácticas:**

```
✅ Diseño modular: Separar extracción y transformación
✅ Premium: Enhanced Compute Engine
✅ Incremental refresh para tablas grandes
✅ Documentar y certificar dataflows
✅ Versioning con deployment pipelines
```

### 🎯 Tip de Entrevista
**Menciona:** "Los dataflows son **fundamentales para arquitecturas escalables**. Los uso para crear **capa de datos común** que múltiples datasets consumen, evitando duplicación. Implemento arquitectura de capas: dataflow staging (extracción raw), dimensiones (limpias), hechos (enriquecidos). En Premium, habilito **Enhanced Compute Engine**. Facilita **separación de roles**: data engineers gestionan dataflows, analistas consumen en datasets. Beneficio: **single source of truth**."

---

## **45. ¿Cómo programar actualizaciones de datos?**

### 📖 Definición
La **programación de actualizaciones** (scheduled refresh) configura cuándo y con qué frecuencia Power BI actualiza los datos de un dataset, automatizando el mantenimiento de reportes actualizados.

### 🔧 Explicación Práctica

### **Configuración:**

**Power BI Service → Dataset → Settings:**
```
1. Gateway connection (si on-premises):
   - Seleccionar Gateway
   - Configurar credenciales

2. Scheduled refresh:
   - Keep data up to date: ON
   - Frequency: Daily/Weekly
   - Time zone: Apropiada
   - Times:
     * Pro: Hasta 8/día
     * Premium: 48+/día

3. Notifications:
   - Email on failure
   - Additional recipients
```

### **Frecuencia por Licencia:**

| **Licencia** | **Refreshes/Día** | **Intervalo Mínimo** |
|-------------|-------------------|---------------------|
| **Pro** | 8 | 30 minutos |
| **Premium/PPU** | 48+ | Sin límite real |

### 💡 Estrategias de Refresh

**Por Frecuencia de Datos:**
```
Tiempo Real:
- DirectQuery (no requiere refresh)
- O Premium: cada 30 min

Diarios:
- 1-2 refreshes/día
- Después de ETL origen (6 AM)

Semanales:
- 1 vez/semana (Lunes 7 AM)
```

**Por Horario de Negocio:**
```
Dashboard usado 9 AM por ejecutivos:

Schedule:
- 06:00 AM → Antes de llegada
- 12:00 PM → Medio día
- 06:00 PM → Fin de día

Evitar:
- Horas pico de uso
- Múltiples refreshes simultáneos
```

### **Incremental Refresh (Premium):**

```
Para tablas muy grandes:

Configuración:
1. Parámetros: RangeStart, RangeEnd
2. Filtrar: [Fecha] >= RangeStart and <= RangeEnd
3. Manage Incremental Refresh:
   - Archive: 5 years
   - Refresh: last 30 days

Resultado:
Sin incremental: 10M filas = 3 horas
Con incremental: 300K filas = 10 minutos
```

### **Troubleshooting:**

```
Error: "Unable to connect"
✅ Verificar: Gateway online, credenciales, conectividad

Error: "Timeout expired"
✅ Solución: Optimizar queries, incremental refresh

Error: "Memory limit"
✅ Solución: Reducir columnas/filas, optimizar tipos
```

### **API para Refresh Programático:**

```PowerShell
# PowerShell - Trigger refresh
Invoke-RestMethod -Method Post `
    -Uri "https://api.powerbi.com/v1.0/myorg/groups/$WorkspaceId/datasets/$DatasetId/refreshes" `
    -Headers @{Authorization = "Bearer $token"}
```

### 🎯 Tip de Entrevista
**Menciona:** "Programo refreshes considerando **frecuencia de cambio, complejidad del dataset, y horarios de uso**. Para datos diarios, refresh después del ETL origen (6 AM) y antes de usuarios. Key: **escalonar refreshes** para no sobrecargar gateway. Implemento **alertas de fallo** al equipo. Para tablas muy grandes uso **incremental refresh** (Premium) - solo actualizo últimos 30 días en lugar de años completos, reduciendo tiempo de 3 horas a 15 minutos. Monitoreo refresh history para detectar degradación."

---

## 🎓 **Resumen de Conceptos Clave**

✅ **Workspaces**: Colaboración con roles granulares  
✅ **Pro vs Premium**: Escalar según usuarios y características  
✅ **Gateway**: Puente seguro cloud ↔ on-premises  
✅ **Dataflows**: ETL reutilizable, single source of truth  
✅ **Scheduled Refresh**: Automatizar según licencia y necesidad  
✅ **Alta disponibilidad**: Gateway cluster, múltiples refreshes  
✅ **Incremental refresh**: Big data eficiente (Premium)  
✅ **Monitoreo**: Alertas, logs, performance  
✅ **Gobierno**: Separación ambientes, control acceso  
✅ **Seguridad**: Roles, credenciales, auditoría  

---

**💡 Consejo Final**: La administración efectiva requiere **planificación de gobernanza, seguridad y performance**. En entrevistas, demuestra comprensión del **ciclo de vida completo**: desarrollo → producción, seguridad (roles, RLS), automatización (refreshes). Los entrevistadores valoran experiencia **enterprise**: múltiples workspaces, dataflows centralizados, gateway HA, elección de licenciamiento. Menciona consideraciones de **costo, escala y gobierno**.
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
---

## 📚 Recursos Adicionales

### Herramientas mencionadas:
- **DAX Studio:** Para optimización de queries DAX
- **Tabular Editor:** Para modelado avanzado
- **Performance Analyzer:** Para análisis de rendimiento en Desktop
- **Best Practices Analyzer:** Para identificación automática de problemas
- **VertiPaq Analyzer:** Para análisis de tamaño de modelo
- **Query Diagnostics:** Para troubleshooting de Power Query

### Comunidades recomendadas:
- SQLBI (Marco Russo y Alberto Ferrari)
- Guy in a Cube (YouTube)
- Power BI Community Forums
- DAX Patterns
- Power BI Tips by Mike Carlo y Seth Bauer

### Documentación oficial:
- Microsoft Power BI Documentation
- DAX Guide (dax.guide)
- Power Query M Reference

---

## ✅ Checklist de Preparación para Entrevistas

### Fundamentos (Preguntas 1-5)
- [ ] Explicar qué es Power BI y sus componentes principales
- [ ] Diferenciar Desktop vs Service
- [ ] Describir flujo: Conectar → Transformar → Modelar → Visualizar → Compartir
- [ ] Entender Power Query y lenguaje M
- [ ] Dominar conceptos básicos de DAX

### Modelado de Datos (Preguntas 6-15)
- [ ] Star Schema vs Snowflake Schema
- [ ] Tipos de relaciones y cardinalidad
- [ ] Tablas de hechos vs dimensiones
- [ ] Propagación de filtros y relaciones bidireccionales
- [ ] Medidas vs columnas calculadas
- [ ] Tabla de calendario y su importancia
- [ ] Row-Level Security (RLS)
- [ ] Import vs DirectQuery vs Live Connection
- [ ] Modelos compuestos

### Power Query Avanzado (Preguntas 16-25)
- [ ] Query Folding y su importancia
- [ ] Merge vs Append y tipos de Join
- [ ] Parámetros en Power Query
- [ ] Optimización de rendimiento en ETL
- [ ] Lenguaje M y sintaxis básica
- [ ] Manejo de errores (try...otherwise)
- [ ] Funciones personalizadas
- [ ] Trabajar con APIs y JSON
- [ ] Referencias vs consultas duplicadas

### DAX Avanzado (Preguntas 26-35)
- [ ] Contexto de evaluación (Filter + Row context)
- [ ] ALL, ALLSELECTED, ALLEXCEPT
- [ ] CALCULATE y modificación de contexto
- [ ] Time Intelligence y funciones temporales
- [ ] Variables VAR y su beneficio
- [ ] SUMX vs SUM y familia de iteradores
- [ ] Diferencia entre filter context y row context
- [ ] SWITCH para medidas dinámicas
- [ ] USERELATIONSHIP para múltiples fechas
- [ ] Optimización de fórmulas DAX

### Visualización y Diseño (Preguntas 36-40)
- [ ] Mejores prácticas de diseño de dashboards
- [ ] Bookmarks y navegación interactiva
- [ ] Drill-through pages
- [ ] Tooltips personalizados
- [ ] Slicers y filtros: tipos y configuración
- [ ] Jerarquía visual y regla del 5 segundos
- [ ] Paleta de colores y accesibilidad
- [ ] Mobile layouts

### Power BI Service y Administración (Preguntas 41-45)
- [ ] Workspaces y roles (Admin/Member/Contributor/Viewer)
- [ ] Pro vs Premium vs Premium Per User
- [ ] Gateway: instalación y configuración
- [ ] Dataflows para ETL centralizado
- [ ] Scheduled refresh y su configuración
- [ ] Incremental refresh (Premium)

### Optimización y Mejores Prácticas (Preguntas 46-50)
- [ ] Reducir tamaño de archivo .pbix
- [ ] Performance Analyzer para troubleshooting
- [ ] Mejores prácticas de DAX
- [ ] Documentación y mantenimiento de proyectos
- [ ] Errores comunes y cómo evitarlos
- [ ] Versionado y deployment pipelines

### Preparación General
- [ ] Preparar 3-5 ejemplos de proyectos reales con:
  - Problema de negocio
  - Solución técnica implementada
  - Desafíos enfrentados
  - Resultados e impacto
- [ ] Practicar explicar conceptos técnicos de forma simple
- [ ] Conocer trade-offs de decisiones técnicas
- [ ] Estar listo para preguntas de troubleshooting
- [ ] Tener preguntas preparadas para el entrevistador

---

## 🎯 Estrategia para la Entrevista

### Durante la entrevista:
1. **Escucha activa:** Asegúrate de entender completamente la pregunta
2. **Estructura tu respuesta:** Definición → Explicación práctica → Ejemplo real
3. **Menciona trade-offs:** Demuestra pensamiento crítico
4. **Usa el método STAR:** Situación, Tarea, Acción, Resultado
5. **Pregunta cuando sea necesario:** Mejor clarificar que asumir

### Frases que demuestran experiencia:
- "En un proyecto anterior..."
- "La decisión dependió de..."
- "El trade-off fue..."
- "Lo implementé de esta manera porque..."
- "El impacto en el negocio fue..."

### Red flags a evitar:
- ❌ "No sé" sin intentar razonar
- ❌ Respuestas genéricas sin ejemplos
- ❌ Solo teoría sin práctica
- ❌ No conocer limitaciones de soluciones propuestas
- ❌ Criticar herramientas sin fundamento

---

**Última actualización:** Enero 2025  
**Versión:** 2.0 - 50 Preguntas Completas  
**Autor:** Preparación profesional para entrevistas técnicas de Power BI

---

## 📌 Notas Finales

Este documento cubre los aspectos más importantes para entrevistas técnicas de Power BI desde nivel intermedio hasta senior. Cada pregunta incluye:
- ✅ Definición clara del concepto
- ✅ Explicación práctica con contexto real
- ✅ Ejemplos de código cuando aplica
- ✅ Tips específicos para entrevistas
- ✅ Mejores prácticas de la industria

**Recuerda:** La experiencia práctica y la capacidad de resolver problemas reales valen más que memorizar respuestas. Prepara ejemplos de tus proyectos y enfócate en comunicar el **valor de negocio** de tus soluciones técnicas.

**¡Éxito en tu entrevista! 🚀**
