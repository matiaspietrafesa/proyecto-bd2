# 🔧 Sistema de Gestión de Reparaciones - DeviceService

Un sistema avanzado de gestión para el servicio técnico de dispositivos futuristas, implementado en SQL Server con análisis de costos, control de calidad y seguimiento histórico.

## 🎯 Descripción del Proyecto

Este proyecto modela un sistema empresarial de reparación de dispositivos especializados que incluye:

- **Gestión de Empleados**: Técnicos (T) y Control de Calidad (C)
- **Catálogo de Productos**: Dispositivos futuristas con stock y costos
- **Trazabilidad de Unidades**: Seguimiento por número de serie
- **Sistema de Reparaciones**: Workflow completo con estados
- **Control de Calidad**: Validación por empleados especializados
- **Análisis de Costos**: Reportes financieros y operacionales

## 🗃️ Estructura de la Base de Datos

### Entidades Principales

#### 👨‍💼 **Empleado**
```sql
- IdEmp (PK, IDENTITY)
- NomEmp, FchNacEmp, SueldoEmp
- TipoEmp: 'T' (Técnico) | 'C' (Control Calidad)
```

#### 🛸 **Producto** 
```sql
- IdProd (PK, IDENTITY)
- DscProd (Descripción del dispositivo)
- StkProd (Stock disponible)
- CostoProd (Costo del producto)
```

#### 🔢 **Unidad**
```sql
- NumSerie (PK, CHAR(10))
- IdProd (PK, FK)
- FchFab (Fecha fabricación)
- FchVto (Fecha vencimiento)
```

#### 🔨 **Repara**
```sql
- IdRepara (PK, IDENTITY)
- NumSerie, IdProd (FK compuesta a Unidad)
- IdEmp (FK a Empleado técnico)
- IdEmpQA (FK a Empleado QA - opcional)
- FchRepara, CostoRepara
- StsRepara: 'Iniciado' | 'En testing' | 'Terminado' | 'Cancelado'
```

#### 📊 **Tablas de Historial**
```sql
- HistoricoReparacion: Tracking de cambios de estado
- HistoricoEliminacionReparaciones: Auditoría de eliminaciones
```

## 🛸 Catálogo de Productos Futuristas

El sistema maneja dispositivos de ciencia ficción:

- **Hiperpropulsor X**: Sistema de propulsión avanzado
- **Motor de Plasma**: Generador de energía cuántica  
- **Escudo Cuántico**: Protección dimensional
- **Sensor Gravitacional**: Detector de anomalías espaciales
- **Drone Autónomo Estelar**: Explorador robótico
- **Nave Interceptor Mk II**: Vehículo de combate
- **Cristal de Energía Zeta**: Fuente de poder místico

## 📁 Archivos del Proyecto

### **DDL** - Definición del Schema
- Creación de base de datos `DeviceService`
- Definición completa de tablas con constraints
- Relaciones FK complejas (incluyendo auto-referencias)
- Validaciones de dominio (CHECK constraints)

### **DML** - Datos de Prueba
- 9 empleados con diferentes roles y sueldos
- 7 productos futuristas con stock y costos
- 10 unidades con números de serie únicos
- 20 reparaciones con diferentes estados y costos

### **Consultas SQL Avanzadas**

#### 🎯 Índices Estratégicos
```sql
-- Optimización por descripción de producto
CREATE INDEX idx_producto_DscProd ON Producto(DscProd);

-- Análisis salarial y tipo de empleado
CREATE INDEX idx_empleado_SueldoEmp ON Empleado(SueldoEmp);
CREATE INDEX idx_empleado_TipoEmp ON Empleado(TipoEmp);

-- Búsquedas de reparación por estado y costo
CREATE INDEX idx_Repara_StsRepara ON Repara(StsRepara);
CREATE INDEX idx_Repara_CostoRepara ON Repara(CostoRepara);
```

## 🔍 Consultas Implementadas

### **Consulta A**: Análisis de Control de Calidad
```sql
-- Por cada producto muestra:
-- - Reparaciones CON control de calidad
-- - Reparaciones SIN control de calidad  
-- - Reparaciones con costo > $100
```

### **Consulta B**: Empleado Más Productivo
```sql
-- Empleado que realizó más reparaciones
-- Incluye datos completos + count de reparaciones
-- Ordenado por productividad DESC
```

### **Consulta C**: Análisis de Costos por Producto
```sql
-- Costo total de reparaciones por producto
-- Correlación entre costo del producto y reparaciones
-- Identificación de productos problemáticos
```

## ⚡ Características Técnicas Avanzadas

### Estados de Reparación con Workflow
```
Iniciado → En testing → Terminado
    ↓
 Cancelado (en cualquier momento)
```

### Control de Calidad Dual
- **Técnicos (T)**: Realizan las reparaciones
- **Control Calidad (C)**: Validan el trabajo (IdEmpQA)
- **Sistema Opcional**: Algunas reparaciones no requieren QA

### Validaciones de Negocio
- Constraint de tipo empleado: `TipoEmp IN ('T','C')`
- Estados válidos de reparación con CHECK constraint
- Unique constraint para evitar duplicados en reparaciones
- Fechas de vencimiento opcionales (productos sin caducidad)

## 📊 Casos de Uso Empresariales

### Para Gerencia
- ✅ **Análisis de Productividad**: Empleados más eficientes
- ✅ **Control de Costos**: Productos con mayor gasto en reparaciones
- ✅ **Calidad**: Porcentaje de reparaciones con QA
- ✅ **Tendencias**: Productos más problemáticos

### Para Técnicos
- ✅ **Workflow**: Estados claros de progreso
- ✅ **Trazabilidad**: Historial por número de serie
- ✅ **Asignación**: Distribución de carga de trabajo

### Para Control de Calidad
- ✅ **Validación**: Sistema de aprobación dual
- ✅ **Métricas**: Reparaciones pendientes de QA
- ✅ **Auditoría**: Histórico de cambios

## 🎯 Modelo de Dominio

### Relaciones Complejas
```
Empleado 1:N Repara (como técnico)
Empleado 1:N Repara (como QA) [opcional]
Producto 1:N Unidad
Unidad 1:N Repara (clave compuesta)
```

### Integridad Referencial
- **FK Múltiples**: Una reparación referencia empleado técnico Y empleado QA
- **Clave Compuesta**: (NumSerie, IdProd) como FK
- **Constraint Unique**: Evita reparaciones duplicadas exactas
- **Valores Opcionales**: IdEmpQA puede ser NULL

## 🔧 Tecnologías y Técnicas

- **SQL Server** con características avanzadas
- **IDENTITY** para PKs auto-incrementales
- **CHECK Constraints** para validación de dominio
- **Índices Estratégicos** para optimización
- **Subconsultas Correlacionadas** en reportes
- **GROUP BY** con agregaciones complejas
- **TOP 1** para rankings

---

Este proyecto demuestra el diseño de una base de datos empresarial compleja con workflow de procesos, control de calidad, análisis de costos y optimización de consultas para un dominio técnico especializado.