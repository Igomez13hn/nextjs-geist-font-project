# Sistema de Fidelidad - Supermercado
## Visual Basic 2010 con SQLite

Este es un sistema completo de fidelidad para supermercados desarrollado en Visual Basic 2010 que utiliza SQLite como base de datos. El sistema calcula puntos de fidelidad basado en la regla: **1 punto por cada $1 gastado**.

## Características Principales

### 🎯 Funcionalidades
- **Gestión de Clientes**: Registro y administración de información de clientes
- **Registro de Compras**: Captura de transacciones con cálculo automático de puntos
- **Cálculo de Puntos**: 1 punto por cada dólar gastado (automático)
- **Historial de Compras**: Visualización del historial completo por cliente
- **Puntos Totales**: Seguimiento acumulativo de puntos por cliente

### 🎨 Interfaz de Usuario
- Diseño moderno y limpio con colores profesionales
- Tipografía Segoe UI para mejor legibilidad
- Botones con estilo flat y colores distintivos
- Formularios responsivos y fáciles de usar

## Estructura del Proyecto

```
SupermarketFidelity/
├── DBHelper.vb              # Manejo de base de datos SQLite
├── Customer.vb              # Clase modelo para clientes
├── MainForm.vb              # Formulario principal (dashboard)
├── CustomerForm.vb          # Gestión de clientes
├── PurchaseForm.vb          # Registro de compras
├── SupermarketFidelity.vbproj # Archivo de proyecto
└── My Project/              # Configuración del proyecto
    ├── Application.Designer.vb
    ├── Application.myapp
    ├── AssemblyInfo.vb
    ├── Resources.Designer.vb
    ├── Resources.resx
    ├── Settings.Designer.vb
    └── Settings.settings
```

## Requisitos del Sistema

### Software Necesario
- **Visual Studio 2010** o superior
- **.NET Framework 4.0** Client Profile o superior
- **System.Data.SQLite** (librería SQLite para .NET)

### Dependencias
- System.Data.SQLite.dll (debe estar referenciada en el proyecto)

## Instalación y Configuración

### 1. Preparar el Entorno
```bash
# Asegúrate de tener Visual Studio 2010 instalado
# Descarga System.Data.SQLite desde: https://system.data.sqlite.org/
```

### 2. Configurar el Proyecto
1. Abre Visual Studio 2010
2. Abre el archivo `SupermarketFidelity.vbproj`
3. Agrega referencia a `System.Data.SQLite.dll`:
   - Click derecho en "References" en el Solution Explorer
   - Selecciona "Add Reference"
   - Busca y agrega `System.Data.SQLite.dll`

### 3. Compilar y Ejecutar
1. Presiona `F5` o click en "Start Debugging"
2. El sistema creará automáticamente la base de datos `loyalty.db`
3. Las tablas se crearán automáticamente al iniciar

## Uso del Sistema

### 1. Pantalla Principal
- **Gestionar Clientes**: Accede al módulo de registro de clientes
- **Registrar Compra**: Accede al módulo de registro de compras
- **Salir**: Cierra la aplicación

### 2. Gestión de Clientes
- **Campos requeridos**: Nombre (obligatorio)
- **Campos opcionales**: Email, Teléfono
- **Lista de clientes**: Visualiza todos los clientes registrados
- **Funciones**: Guardar, Limpiar campos, Cerrar

### 3. Registro de Compras
- **Seleccionar cliente**: Lista desplegable con todos los clientes
- **Ingresar monto**: Cantidad gastada en la compra
- **Cálculo automático**: Los puntos se calculan automáticamente (1 punto = $1)
- **Historial**: Muestra todas las compras del cliente seleccionado
- **Puntos totales**: Muestra el acumulado de puntos del cliente

## Base de Datos

### Estructura de Tablas

#### Tabla: Customers
```sql
CREATE TABLE Customers (
    CustomerID INTEGER PRIMARY KEY AUTOINCREMENT,
    Name TEXT NOT NULL,
    Email TEXT,
    Phone TEXT
);
```

#### Tabla: Purchases
```sql
CREATE TABLE Purchases (
    PurchaseID INTEGER PRIMARY KEY AUTOINCREMENT,
    CustomerID INTEGER NOT NULL,
    Amount REAL NOT NULL,
    Points INTEGER NOT NULL,
    PurchaseDate DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY(CustomerID) REFERENCES Customers(CustomerID)
);
```

### Archivo de Base de Datos
- **Nombre**: `loyalty.db`
- **Ubicación**: Directorio de la aplicación
- **Tipo**: SQLite 3
- **Creación**: Automática al iniciar la aplicación

## Lógica de Negocio

### Cálculo de Puntos
```vb
' Regla: 1 punto por cada dólar gastado
Dim pointsEarned As Integer = CInt(Math.Floor(purchaseAmount))

' Ejemplo:
' $15.99 = 15 puntos
' $50.00 = 50 puntos
' $0.99 = 0 puntos
```

### Validaciones Implementadas
- **Cliente**: Debe seleccionarse un cliente válido
- **Monto**: Debe ser un número positivo mayor a 0
- **Nombre del cliente**: Campo obligatorio al registrar
- **Conexión a BD**: Manejo de errores de conexión

## Características Técnicas

### Manejo de Errores
- Try-Catch en todas las operaciones de base de datos
- Mensajes de error descriptivos para el usuario
- Validación de entrada de datos

### Seguridad
- Consultas parametrizadas para prevenir SQL injection
- Validación de tipos de datos
- Manejo seguro de conexiones (Using statements)

### Rendimiento
- Conexiones de base de datos optimizadas
- Carga eficiente de datos en controles
- Liberación automática de recursos

## Personalización

### Colores del Sistema
```vb
' Colores principales utilizados
BackColor = Color.FromArgb(247, 247, 247)  ' Fondo claro
Primary = Color.FromArgb(52, 152, 219)     ' Azul principal
Success = Color.FromArgb(46, 204, 113)     ' Verde éxito
Warning = Color.FromArgb(241, 196, 15)     ' Amarillo advertencia
Danger = Color.FromArgb(231, 76, 60)       ' Rojo peligro
```

### Modificar Regla de Puntos
Para cambiar la regla de cálculo de puntos, modifica en `PurchaseForm.vb`:
```vb
' Cambiar esta línea en CalculatePoints() y btnRecordPurchase_Click()
Dim pointsEarned As Integer = CInt(Math.Floor(purchaseAmount)) ' Actual: 1 punto = $1
' Por ejemplo, para 2 puntos por dólar:
' Dim pointsEarned As Integer = CInt(Math.Floor(purchaseAmount * 2))
```

## Solución de Problemas

### Error: "System.Data.SQLite not found"
**Solución**: Descargar e instalar System.Data.SQLite desde el sitio oficial

### Error: "Database is locked"
**Solución**: Cerrar todas las instancias de la aplicación y reiniciar

### Error: "Could not load file or assembly"
**Solución**: Verificar que todas las referencias estén correctamente configuradas

## Futuras Mejoras

### Funcionalidades Sugeridas
- [ ] Sistema de canje de puntos
- [ ] Reportes de ventas y puntos
- [ ] Backup automático de base de datos
- [ ] Importar/Exportar datos
- [ ] Sistema de niveles de fidelidad
- [ ] Notificaciones por email
- [ ] Dashboard con gráficos

### Mejoras Técnicas
- [ ] Migración a versiones más recientes de .NET
- [ ] Implementar patrón MVVM
- [ ] Agregar logging avanzado
- [ ] Tests unitarios
- [ ] Configuración externa (app.config)

## Soporte

Para soporte técnico o consultas sobre el sistema:
- Revisar este README
- Verificar la configuración de referencias
- Comprobar permisos de escritura en el directorio de la aplicación

## Licencia

Este proyecto es de código abierto y puede ser modificado según las necesidades específicas del negocio.

---

**Desarrollado para Visual Basic 2010 con SQLite**  
**Sistema de Fidelidad - Supermercado v1.0**
