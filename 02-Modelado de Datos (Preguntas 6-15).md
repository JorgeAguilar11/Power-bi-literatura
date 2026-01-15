# 📊 Modelado de Datos (Preguntas 6-15)

---

## **6. ¿Qué es un modelo Star Schema vs Snowflake Schema?**

### 📖 Definición
Son dos arquitecturas de modelado dimensional para organizar datos en un data warehouse o modelo de BI:

- **Star Schema (Esquema Estrella)**: Una tabla de hechos central conectada directamente a múltiples tablas de dimensiones desnormalizadas.
- **Snowflake Schema (Esquema Copo de Nieve)**: Similar al Star Schema, pero las dimensiones están normalizadas en múltiples tablas relacionadas.

### 🔧 Explicación Práctica

**Star Schema:**
- Tabla de hechos: `Ventas` → conecta a dimensiones: `Producto`, `Cliente`, `Fecha`, `Tienda`
- Las dimensiones contienen todos los atributos (ej: `Producto` tiene marca, categoría, subcategoría)
- Más redundancia, pero más rápido

**Snowflake Schema:**
- La dimensión `Producto` se divide en: `Producto` → `Subcategoría` → `Categoría`
- Menos redundancia, pero más joins necesarios
- Más complejo de consultar

### 💡 Ejemplo
```
Star Schema:
Ventas ─┬─ DimProducto (ID, Nombre, Categoría, Subcategoría, Marca)
        ├─ DimCliente (ID, Nombre, Ciudad, País)
        └─ DimFecha (Fecha, Año, Mes, Trimestre)

Snowflake Schema:
Ventas ─ DimProducto ─ DimSubcategoría ─ DimCategoría
```

### 🎯 Tip de Entrevista
**Menciona:** "En Power BI se recomienda **Star Schema** porque es más eficiente para el motor de Analysis Services. Snowflake requiere más relaciones y afecta el rendimiento. La desnormalización en dimensiones mejora la velocidad de consultas DAX."

---

## **7. ¿Cuándo usar relaciones bidireccionales?**

### 📖 Definición
Las **relaciones bidireccionales** permiten que los filtros se propaguen en ambas direcciones entre dos tablas (de la tabla A a B y de B a A).

### 🔧 Explicación Práctica

**Por defecto**, Power BI usa relaciones unidireccionales (filtro fluye del lado "uno" al lado "muchos").

**Usar bidireccionales cuando:**
- Necesitas que ambas tablas se filtren mutuamente
- Trabajas con modelos many-to-many
- Necesitas que una tabla de hechos filtre a otra

**Precauciones:**
- Pueden causar ambigüedad en el contexto de filtro
- Afectan el rendimiento
- Pueden crear dependencias circulares

### 💡 Ejemplo
```
Escenario: Tienes Ventas y Presupuesto (dos tablas de hechos) conectadas a DimProducto.
Sin bidireccional: Filtrar Presupuesto no afecta Ventas
Con bidireccional: Filtrar Presupuesto también filtra Ventas
```

### 🎯 Tip de Entrevista
**Menciona:** "Las relaciones bidireccionales deben usarse con **precaución**. Prefiero resolver escenarios many-to-many usando una tabla puente o funciones DAX como `CROSSFILTER()` para tener más control. Solo las uso cuando es estrictamente necesario y el modelo es simple."

---

## **8. ¿Qué son las tablas de hechos y dimensiones?**

### 📖 Definición
Son los dos tipos principales de tablas en un modelo dimensional:

- **Tabla de Hechos (Fact Table)**: Contiene métricas numéricas y claves foráneas. Representa eventos o transacciones.
- **Tabla de Dimensiones (Dimension Table)**: Contiene atributos descriptivos que proporcionan contexto a los hechos.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
```
Tabla de Hechos (Ventas):
- VentaID, FechaID, ProductoID, ClienteID, Monto, Cantidad

Tabla de Dimensión (Producto):
- ProductoID, NombreProducto, Categoría, Marca, Precio

Relación: Ventas[ProductoID] → Producto[ProductoID]
```

### 🎯 Tip de Entrevista
**Menciona:** "Las tablas de hechos deben ser **lo más angostas posible** (solo IDs y métricas). Las dimensiones son el lado 'uno' de las relaciones. Un buen diseño implica colocar las medidas DAX sobre hechos y los atributos descriptivos en dimensiones para optimizar el rendimiento."

---

## **9. ¿Qué es la cardinalidad en relaciones?**

### 📖 Definición
La **cardinalidad** define el tipo de relación entre dos tablas, indicando cuántos registros de una tabla se relacionan con cuántos registros de otra.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
```DAX
// 1:N (Ideal)
DimProducto[ProductoID] → FactVentas[ProductoID]

// N:M (Compleja - evitar o usar tabla puente)
Actores ←→ Películas (requiere tabla ActoresPelículas)
```

### 🎯 Tip de Entrevista
**Menciona:** "La cardinalidad **1:N es la recomendada** en Power BI. Las relaciones N:M funcionan pero pueden afectar el rendimiento. En estos casos, prefiero crear una **tabla puente** que convierta el N:M en dos relaciones 1:N."

---

## **10. ¿Qué es la propagación de filtros (filter propagation)?**

### 📖 Definición
La **propagación de filtros** es el mecanismo por el cual los filtros aplicados en una tabla se transmiten automáticamente a tablas relacionadas siguiendo las relaciones del modelo.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
```
Escenario:
- Seleccionas "Categoría = Electrónica" en un slicer
- La tabla DimProducto se filtra
- El filtro se propaga a FactVentas automáticamente
- Las medidas muestran solo ventas de electrónica

Sin propagación, tendrías que filtrar manualmente cada tabla.
```

### 🎯 Tip de Entrevista
**Menciona:** "La propagación de filtros es fundamental para que los modelos funcionen. Power BI gestiona esto automáticamente mediante las relaciones. Es importante entender la **dirección del filtro** para diagnosticar por qué un slicer no está funcionando como esperamos."

---

## **11. ¿Cuándo usar columnas calculadas vs medidas?**

### 📖 Definición
- **Columnas Calculadas**: Se evalúan **fila por fila** en tiempo de actualización de datos y se almacenan en el modelo.
- **Medidas**: Se evalúan en **tiempo de consulta** según el contexto del visual y no se almacenan.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "Regla general: **columnas calculadas para atributos, medidas para métricas**. Las columnas aumentan el tamaño del modelo y se calculan solo en refresh. Las medidas son dinámicas y se recalculan con cada interacción. Siempre prefiero medidas cuando es posible por rendimiento."

---

## **12. ¿Qué son las tablas de calendario y por qué son importantes?**

### 📖 Definición
Una **tabla de calendario** (o tabla de fechas) es una tabla de dimensión que contiene una fila por cada fecha en un rango específico, con columnas adicionales para año, mes, trimestre, día de la semana, etc.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "La tabla de calendario es **obligatoria para análisis temporal serio**. Debe marcarse como 'tabla de fechas' en Power BI. Incluyo siempre columnas como semana fiscal, año fiscal, días laborables. Las funciones como `SAMEPERIODLASTYEAR`, `TOTALYTD` requieren una tabla de calendario correctamente configurada."

---

## **13. ¿Qué es la seguridad a nivel de fila (RLS)?**

### 📖 Definición
**Row-Level Security (RLS)** es una característica que restringe el acceso a datos a nivel de fila basándose en roles de usuario, permitiendo que diferentes usuarios vean diferentes subconjuntos de datos en el mismo reporte.

### 🔧 Explicación Práctica

**Cómo funciona:**
1. Defines **roles** en Power BI Desktop
2. Creas **filtros DAX** para cada rol
3. Asignas **usuarios o grupos** a roles en Power BI Service
4. Los filtros se aplican automáticamente cuando el usuario accede al reporte

**Casos de uso:**
- Gerentes regionales ven solo su región
- Vendedores ven solo sus clientes
- Departamentos ven solo su información

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "RLS se define en **Desktop** pero se aplica en **Service**. Es importante usar `USERPRINCIPALNAME()` para filtros dinámicos. Recomiendo crear una **tabla de seguridad** que mapee usuarios a dimensiones. Siempre pruebo los roles usando 'View as' en Desktop antes de publicar."

---

## **14. ¿Qué diferencia hay entre Import, DirectQuery y Live Connection?**

### 📖 Definición
Son tres **modos de conectividad** de datos en Power BI, que determinan cómo se almacenan y consultan los datos:

### 🔧 Explicación Práctica

| **Característica** | **Import** | **DirectQuery** | **Live Connection** |
|-------------------|------------|-----------------|---------------------|
| **Datos** | Se copian al modelo | Quedan en origen | Quedan en origen |
| **Rendimiento** | Rápido | Depende de origen | Rápido |
| **Actualización** | Programada | Tiempo real | Tiempo real |
| **Tamaño** | Limitado (1GB gratis) | Sin límite | Sin límite |
| **DAX** | Completo | Limitado | Completo |
| **Power Query** | Sí | Limitado | No |
| **Origen** | Cualquiera | SQL, SAP, etc. | SSAS, Azure AS, PBI Datasets |

### **Import (Importar)**
- Datos se cargan en memoria comprimida
- Mejor rendimiento
- Requiere refresh programado
- **Caso de uso**: Reportes con millones de filas, análisis complejos

### **DirectQuery**
- Consulta la fuente en tiempo real
- Datos siempre actualizados
- Más lento (depende del origen)
- **Caso de uso**: Dashboards en tiempo real, datos sensibles que no se pueden copiar

### **Live Connection**
- Conecta a modelos existentes (SSAS, Azure AS, Power BI Datasets)
- No puede modificar el modelo
- Usa el motor del servidor
- **Caso de uso**: Reutilizar modelos corporativos, separar capa semántica

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "**Import es el modo preferido** por rendimiento y flexibilidad. DirectQuery lo uso cuando necesito datos en tiempo real o cuando los datos son muy grandes para importar. Live Connection es ideal para **gobernanza centralizada**: un equipo mantiene el modelo, otros crean reportes. También puedo usar **modelos compuestos** que combinan Import y DirectQuery."

---

## **15. ¿Qué son los modelos compuestos?**

### 📖 Definición
Los **modelos compuestos (Composite Models)** permiten combinar múltiples fuentes de datos con diferentes modos de conectividad (Import + DirectQuery) en un solo modelo de Power BI.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "Los modelos compuestos son poderosos para **optimizar rendimiento y actualización**. Por ejemplo, mantengo dimensiones y hechos históricos en Import (rápido) y datos operacionales en DirectQuery (tiempo real). El modo **Dual** es inteligente: actúa como Import cuando es posible y DirectQuery cuando es necesario. Es clave entender que las relaciones pueden tener **cardinalidad limitada** cuando cruzan modos."

---

## 🎓 **Resumen de Conceptos Clave de Modelado**

✅ **Star Schema** sobre Snowflake (rendimiento)  
✅ **Relaciones bidireccionales** con precaución  
✅ **Hechos = métricas, Dimensiones = contexto**  
✅ **Cardinalidad 1:N** es la ideal  
✅ **Propagación de filtros** funciona downstream  
✅ **Medidas** sobre columnas calculadas (rendimiento)  
✅ **Tabla de calendario** es obligatoria  
✅ **RLS** se define en Desktop, se aplica en Service  
✅ **Import** por defecto, DirectQuery cuando sea necesario  
✅ **Modelos compuestos** para escenarios híbridos  

---

**💡 Consejo Final**: En entrevistas, siempre relaciona conceptos de modelado con **impacto en rendimiento** y **casos de uso reales**. Los entrevistadores buscan experiencia práctica, no solo teoría.