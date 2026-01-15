# ☁️ Power BI Service y Administración (Preguntas 41-45)

---

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