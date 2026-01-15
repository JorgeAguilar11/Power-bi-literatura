# 📈 DAX Avanzado (Preguntas 26-35)

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