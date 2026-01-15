# 🔧 Power Query Avanzado (Preguntas 16-25)

---

## **16. ¿Qué es el folding de consultas (query folding)?**

### 📖 Definición
**Query Folding** es la capacidad de Power Query para traducir transformaciones del lenguaje M a consultas nativas de la fuente de datos (SQL, por ejemplo), permitiendo que el procesamiento ocurra en el servidor de origen en lugar del equipo local.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "El query folding es crítico para el **rendimiento con grandes volúmenes**. Siempre verifico que mis transformaciones mantengan el folding usando 'View Native Query'. Evito columnas personalizadas complejas al inicio del proceso y las coloco después de que los datos ya estén filtrados. En proyectos reales, esto puede significar la diferencia entre segundos y horas de procesamiento."

---

## **17. ¿Cuándo usar Merge vs Append?**

### 📖 Definición
Son dos operaciones fundamentales para combinar datos en Power Query:

- **Merge**: Combina tablas **horizontalmente** (agrega columnas) - Similar a JOIN en SQL
- **Append**: Combina tablas **verticalmente** (agrega filas) - Similar a UNION en SQL

### 🔧 Explicación Práctica

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

### 💡 Ejemplo

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

### 🎯 Tip de Entrevista
**Menciona:** "**Merge es para JOIN, Append es para UNION**. En Power Query, prefiero hacer Merge en lugar de usar RELATED en DAX cuando es posible, porque aprovecha el query folding. Para Append, es útil cuando consolido múltiples archivos Excel o CSVs con la misma estructura usando 'Append Queries as New'."

---

## **18. ¿Qué tipos de Join existen en Power Query?**

### 📖 Definición
Los **tipos de Join** (al hacer Merge) determinan qué filas se incluyen en el resultado basándose en las coincidencias entre las claves de ambas tablas.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
```
Tabla A (Ventas):         Tabla B (Clientes):
ClienteID | Monto         ClienteID | Nombre
1         | 100           1         | Ana
2         | 200           3         | Luis
4         | 300

Left Outer:               Inner:                Left Anti:
ClienteID | Monto | Nom   ClienteID | Monto     ClienteID | Monto
1         | 100   | Ana   1         | 100       2         | 200
2         | 200   | null  
4         | 300   | null  

Full Outer:
ClienteID | Monto | Nom
1         | 100   | Ana
2         | 200   | null
3         | null  | Luis
4         | 300   | null
```

### 🎯 Tip de Entrevista
**Menciona:** "**Left Outer es el join más común** en BI porque queremos mantener todas las transacciones aunque no tengan coincidencia en dimensiones. **Left Anti es muy útil** para QA: encontrar ventas sin cliente, productos sin categoría, etc. En entrevistas, siempre menciono que entiendo la diferencia entre Inner (restrictivo) y Outer (inclusivo)."

---

## **19. ¿Qué son los parámetros en Power Query?**

### 📖 Definición
Los **parámetros** son valores reutilizables que se pueden usar en múltiples consultas y transformaciones, permitiendo crear soluciones dinámicas y fáciles de mantener.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "Los parámetros son fundamentales para **soluciones escalables**. Los uso para rutas de archivo, conexiones de BD y filtros de fecha. En proyectos corporativos, creo parámetros para diferenciar ambientes (Dev/QA/Prod). También son útiles para crear **informes parametrizados** donde el usuario puede cambiar valores sin tocar el código M."

---

## **20. ¿Cómo optimizar el rendimiento en Power Query?**

### 📖 Definición
La **optimización en Power Query** implica aplicar técnicas y mejores prácticas para reducir el tiempo de procesamiento y el consumo de recursos durante la transformación de datos.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "La regla de oro es **filtrar primero, transformar después**. Siempre empiezo eliminando columnas y filas innecesarias para reducir el volumen. Verifico el query folding constantemente. En proyectos con millones de registros, desactivar la carga de consultas intermedias puede reducir el tiempo de refresh significativamente. También uso el **Query Diagnostics** para identificar cuellos de botella."

---

## **21. ¿Qué es el lenguaje M?**

### 📖 Definición
**M** (también llamado Power Query Formula Language) es el lenguaje de programación funcional usado por Power Query para definir transformaciones de datos. Es case-sensitive y se ejecuta secuencialmente.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "El lenguaje M es **fundamental para transformaciones avanzadas**. Aunque Power Query tiene interfaz visual, conocer M me permite hacer transformaciones que no son posibles con clicks. Uso M para crear funciones reutilizables, manejar errores con `try...otherwise`, y depurar problemas. El operador `each` es esencial para trabajar con filas, y equivale a `(_) =>`."

---

## **22. ¿Cómo manejar errores en Power Query?**

### 📖 Definición
El **manejo de errores** en Power Query permite controlar qué sucede cuando una transformación falla, evitando que todo el proceso se detenga y permitiendo soluciones alternativas.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "El manejo de errores es crítico en **entornos de producción**. Uso `try...otherwise` para conversiones de tipos que pueden fallar (fechas, números). Para debugging, primero uso `Table.SelectRowsWithErrors()` para ver qué filas tienen problemas. En transformaciones complejas, prefiero manejar errores explícitamente en lugar de dejar que falle el refresh completo. También documento por qué se reemplaza un error con un valor específico."

---

## **23. ¿Qué son las funciones personalizadas en Power Query?**

### 📖 Definición
Las **funciones personalizadas** son bloques de código M reutilizables que aceptan parámetros y retornan un valor, permitiendo encapsular lógica compleja para usar en múltiples consultas.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo
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

### 🎯 Tip de Entrevista
**Menciona:** "Las funciones personalizadas son clave para **DRY (Don't Repeat Yourself)**. Las uso para procesar múltiples archivos con la misma estructura, aplicar reglas de negocio consistentes, y encapsular transformaciones complejas. Un caso común es crear una función que cargue y estandarice archivos Excel de diferentes períodos. Importante: las funciones personalizadas **rompen el query folding**, así que las uso después de filtrar datos."

---

## **24. ¿Cómo trabajar con APIs y JSON en Power Query?**

### 📖 Definición
Power Query puede conectarse a **APIs REST** y procesar datos en formato **JSON**, permitiendo integrar fuentes de datos web y servicios cloud en tus modelos.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo Completo
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

### 🎯 Tip de Entrevista
**Menciona:** "Trabajar con APIs es cada vez más común en BI moderno. Uso `Web.Contents()` para conectar APIs REST y `Json.Document()` para parsear la respuesta. Lo complicado es el **JSON anidado**: hay que expandir records y listas cuidadosamente. Para APIs con paginación, creo funciones recursivas que consolidan todas las páginas. Importante: las credenciales de API se gestionan en 'Data Source Settings' para seguridad."

---

## **25. ¿Qué son las consultas de referencia vs duplicadas?**

### 📖 Definición
Son dos formas de reutilizar consultas existentes en Power Query:

- **Referencia (Reference)**: Crea una nueva consulta que **reutiliza los pasos** de la original. Cambios en la original afectan a la referencia.
- **Duplicar (Duplicate)**: Crea una **copia independiente** completa. Los cambios no se sincronizan.

### 🔧 Explicación Práctica

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

### 💡 Ejemplo

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

### **Cuándo usar cada una:**

| **Situación** | **Usar** |
|--------------|----------|
| Misma fuente, diferentes filtros | Referencia |
| Crear tabla auxiliar temporal | Referencia (+ disable load) |
| Tablas de hechos y dimensiones del mismo source | Referencia |
| Quieres modificar la fuente sin afectar otras | Duplicar |
| Plantilla para múltiples archivos | Duplicar |
| Testear cambios sin romper otras consultas | Duplicar |

### 🎯 Tip de Entrevista
**Menciona:** "**Referencia es la opción por defecto** porque es más eficiente: Power Query solo procesa la consulta original una vez y las referencias solo aplican pasos adicionales. Uso referencias para crear tablas de hechos y dimensiones de la misma fuente. Para optimizar, marco las consultas intermedias como 'Enable Load = False' para que no se carguen al modelo. Solo duplico cuando necesito una copia completamente independiente o cuando voy a modificar pasos iniciales sin afectar otras consultas."

---

## 🎓 **Resumen de Conceptos Clave de Power Query**

✅ **Query Folding**: Verificar siempre para optimizar rendimiento  
✅ **Merge = JOIN horizontal**, **Append = UNION vertical**  
✅ **Left Outer** es el join más común  
✅ **Parámetros** para soluciones dinámicas y escalables  
✅ **Filtrar primero**, transformar después  
✅ **Lenguaje M** es case-sensitive y funcional  
✅ **Try...otherwise** para manejo robusto de errores  
✅ **Funciones personalizadas** para reutilización (rompen folding)  
✅ **Web.Contents() + Json.Document()** para APIs  
✅ **Referencias** sobre duplicados (rendimiento)  

---

**💡 Consejo Final**: Power Query es la **primera línea de defensa** para calidad y rendimiento de datos. En entrevistas, demuestra que entiendes el impacto de tus transformaciones en el rendimiento general del modelo. Los entrevistadores valoran experiencia con fuentes complejas (APIs, JSON) y optimización avanzada.