# DOCUMENTO FUENTE: AUDITORÍA, REMEDIACIÓN Y ENDURECIMIENTO ARQUITECTÓNICO
## Sistema de Gestión y Soporte Técnico Automotriz

---

### METADATOS DEL DOCUMENTO
* **Sistema:** Plataforma Web de Gestión de Taller y Soporte Técnico Automotriz
* **Tipo de Documento:** Informe Técnico de Arquitectura, Ciberseguridad, Invariantes de Dominio y Remediación Integral
* **Fecha:** Septiembre de 2026
* **Enfoque de Ingeniería:** Clean Architecture, Defensa en Profundidad (OWASP Top 10), Atomicidad Transaccional y Diseño por Capacidades (HATEOAS-lite)
* **Audiencia Objetivo:** Evaluadores Académicos, Arquitectos de Software, Auditores de Seguridad y Estudiantes de Análisis de Sistemas
* **Uso Previsto:** Fuente oficial de conocimiento para Google NotebookLM, Claude y Gamma para generación de resúmenes, audio overviews (podcast de debate técnico), guías de sustentación y diapositivas ejecutivas.

---

## 1. CONTEXTO DE NEGOCIO Y DOMINIO OPERATIVO

### 1.1. La Problemática del Taller Tradicional
La gestión operativa de un taller automotriz tradicional enfrenta graves problemas de pérdida de trazabilidad:
1. **Desconexión entre áreas:** La recepción registra las fallas en papel o notas informales; el jefe de taller distribuye el trabajo verbalmente; los mecánicos cambian piezas sin registrar tiempos exactos; y el cliente desconoce el avance real de su vehículo.
2. **Fuga de ingresos y fraude:** No existe certeza de si un repuesto instalado realmente se requería, si fue facturado o si una intervención posterior corresponde a un reclamo legítimo de garantía.
3. **Vulnerabilidad legal:** Ante una queja o demanda por daños mecánicos, el taller carece de un expediente cronológico inmutable (historia clínica vehicular) que demuestre qué técnico intervino el motor, qué pruebas realizó y en qué fecha exacta se entregó la unidad.

### 1.2. Propósito y Alcance del Software
El **Sistema Web de Soporte Técnico Automotriz** digitaliza el núcleo operativo del taller a través de los siguientes módulos funcionales:
* **Recepción y Registro de Clientes y Vehículos:** Captura de datos personales del propietario y ficha técnica del vehículo (Placa, VIN, Marca, Modelo, Año).
* **Órdenes de Servicio (Lifecycle):** Seguimiento del ciclo de vida del vehículo mediante estados secuenciales y atómicos:
  $$\text{RECEIVED} \longrightarrow \text{IN\_DIAGNOSIS} \longrightarrow \text{IN\_REPAIR} \longrightarrow \text{READY} \longrightarrow \text{DELIVERED}$$
* **Asignación de Mecánicos:** Distribución controlada del trabajo por parte del Jefe de Taller, aplicando la regla de negocio: *un mecánico no puede atender más de una orden activa de manera simultánea*.
* **Diagnóstico Técnico:** Registro formal de hallazgos iniciales y componentes a reparar realizado por el mecánico responsable.
* **Intervenciones y Repuestos:** Detalle del trabajo físico ejecutado (horas de mano de obra) y catálogo de repuestos consumidos.
* **Gestión de Garantías:** Emisión de pólizas sobre intervenciones realizadas (Mano de obra o Repuestos) con vigencia calculada en meses.
* **Línea de Tiempo Clínica (Timeline):** Historial cronológico consolidado por vehículo que unifica órdenes, diagnósticos, intervenciones y garantías.

### 1.3. Arquitectura Tecnológica Real del Sistema
A diferencia de los supuestos iniciales emitidos en auditorías preliminares de caja negra (que asumieron incorrectamente un stack basado en Node.js/Express), el sistema está construido sobre una arquitectura limpia y robusta:
* **Backend:** Lenguaje **Go 1.25** implementando Clean Architecture con separación estricta de responsabilidades:
  - `domain`: Entidades puras, constantes de estado, máquinas de estado e invariantes de negocio.
  - `usecase`: Casos de uso de la aplicación, autorización transaccional y orquestación.
  - `repository`: Adaptadores de persistencia relacional con consultas SQL parametrizadas.
  - `transport/http`: Enrutamiento HTTP nativo (`net/http.ServeMux`), controladores, rate limiting y middlewares.
* **Frontend:** Single Page Application (SPA) desarrollada en **React 18** con **TypeScript**, empaquetada con **Vite** y estructurada por funcionalidades modulares (`features`).
* **Base de Datos:** **MySQL 8.4** con claves foráneas, restricciones de unicidad y scripts DDL automáticos.
* **Infraestructura y Despliegue:** Contenerización integral con **Docker & Docker Compose**, utilizando **Nginx** como servidor web y proxy inverso.

---

## 2. RADIOGRAFÍA DE LAS VULNERABILIDADES AUDITADAS (ANÁLISIS CRÍTICO)

La auditoría inicial detectó 12 anomalías que revelan una premisa arquitectónica fundamental: **el sistema original confiaba la seguridad a la interfaz gráfica en lugar de imponer invariantes inquebrantables en el backend**.

```
                                  VULNERABILIDADES AUDITADAS
                                              |
      +---------------------------------------+---------------------------------------+
      |                                       |                                       |
  PILAR A:                                PILAR B:                                PILAR C:
Control de Acceso (OWASP A01)     Integridad Operativa & Datos        Seguridad Perimetral & Higiene
- Fuga de datos personales        - Cruce y edición entre técnicos    - Fuerza bruta sin Rate Limiting
- Acceso directo por URL          - Órdenes entregadas editables      - Credenciales idénticas
- BOLA en cambio de estados       - Historial desordenado             - Entradas sin sanitizar
                                  - Desincronización de paneles       - Falta de modelo de capacidades
                                  - Garantías desconectadas
```

### Tabla de Evaluación Crítica de Hallazgos y Brechas de Remediación

| Hallazgo Auditado | Comportamiento Inicial Vulnerable | Diagnóstico Superficial | Enfoque de Endurecimiento Exhaustivo (Arquitectura Objetivo) |
| :--- | :--- | :--- | :--- |
| **1. Catálogos Expuestos (A01)** | `GET /api/customer`, `GET /api/vehicle`, etc. devuelven toda la BD a cualquier rol. | Ocultar botones en React y agregar `requireAdministrator` plano en backend. | **Autorización por Recurso:** Los catálogos globales son exclusivamente del Administrador. El técnico solo recibe los datos específicos del vehículo dentro del payload de su orden asignada (`GET /api/service-order/:id`). |
| **2. BOLA en Cambio de Estado** | `POST /api/service-order/:id/status` permitía a `lramirez` avanzar órdenes de `jperez`. | Comprobar `actorUserID == assignedTechnician` en memoria antes de guardar. | **Atomicidad Anti-TOCTOU:** Autorización + validación de estado + transición + auditoría dentro de una transacción SQL única con bloqueo pesimista (`SELECT ... FOR UPDATE`). |
| **3. Edición de Órdenes Ajenas** | Campos de diagnóstico e intervención activos para cualquier mecánico en la UI. | Ocultar formularios con condiciones ternarias en React. | **Invariante en Backend:** Cada endpoint mutador (`/diagnostic`, `/intervention`) impone verificación estricta de propiedad; el frontend consume capacidades explícitas (`permissions`). |
| **4. Modificación de Órdenes Cerradas** | Órdenes en estado `DELIVERED` permitían registrar diagnósticos y repuestos. | Comprobación aislada `if status == DELIVERED`. | **Máquina de Estados de Dominio:** Políticas formales (`CanAddDiagnostic`, `CanAddIntervention`, `CanModify`) centralizadas en `domain/service_order.go`. |
| **5. Historial Desordenado** | Transiciones en orden aleatorio con marcas de segundo duplicadas. | `ORDER BY changed_at`. | **Orden Determinista:** `ORDER BY changed_at ASC, id ASC` garantizando secuencialidad estricta en el registro pericial de auditoría. |
| **6. Desincronización de Técnicos** | El panel decía "No hay técnicos ocupados" mientras había órdenes en curso. | Corregir JOIN en SQL superficialmente. | **Definición de Dominio:** Se formaliza qué significa estar ocupado (`is_busy = true` solo si la orden está en `IN_DIAGNOSIS` o `IN_REPAIR`). |
| **7. Trazabilidad de Garantías** | Columna "Garantía" vacía en intervenciones cubiertas. | Enlace manual en React. | **Resolución en Dominio:** El backend cruza la cobertura y expone la vigencia temporal de la póliza asociada a la intervención. |
| **8. Ataques de Fuerza Bruta** | Intentos infinitos sin retardo en `/api/session`. | Límite simple en memoria de 5 req / 15 min. | **Rate Limiter Dual (IP + Username):** Sliding window thread-safe con recolección de basura (`time.Ticker`), extracción segura tras Nginx proxy y respuestas anti-enumeración. |
| **9. Credenciales Idénticas** | `admin`, `jperez` y `lramirez` usaban `Admin2026*`. | Omitido en la propuesta superficial. | **Remediación Explícita:** Passwords e identificadores criptográficos independientes por usuario en semillas SQL, forzando políticas de no repudio. |
| **10. Ingesta de Payloads / Sanitización** | Inserción literal de `<script>` y `' OR '1'='1`. | Intentar filtrar con Regex. | **Parametrización SQL + Validación de Esquema + CSP:** Consultas preparadas en Go para neutralizar inyecciones SQL, validación de formatos en frontera y escape contextual en cliente. |
| **11. Inyección de Parámetros No Autorizados (Parameter Tampering)** | Envío de `technicianId` malicioso en el cuerpo JSON para suplantar al autor de un trabajo. | Ignorado en el análisis preliminar. | **Decoder Estricto + Identidad en Servidor:** `decoder.DisallowUnknownFields()` rechaza cualquier campo espurio con 400 Bad Request, y el autor se toma irrevocablemente de la sesión JWT. |
| **12. Asignación Múltiple Concurrente** | Posibilidad de asignar múltiples mecánicos activos a una orden o saturar a un técnico. | Comprobación superficial `SELECT` previa. | **Doble Cerrojo en Base de Datos:** Columna `active_order_marker` con restricción `UNIQUE` y `CHECK` condicional a `is_active = 1`, garantizando $1:1$ atómico a nivel de motor SQL. |
| **13. Transición a Fase Operativa sin Técnico Asignado (Orden Huérfana)** | Permitía pasar de `RECEIVED` a `IN_DIAGNOSIS` o `IN_REPAIR` con técnico "Sin asignar". | Máquina de estados ciega que solo evaluaba el estado siguiente sin precondiciones de recursos. | **Invariante Operativa de Dominio:** Error `ErrTechnicianRequired` (HTTP 422), cálculo de permisos en backend `canAdvance = false` en ausencia de técnico asignado, y banner de bloqueo en frontend. |

---

## 3. ARQUITECTURA DE REMEDIACIÓN OBJETIVO (PRINCIPIOS Y DISEÑO)

### 3.1. El Principio Fundamental
> **"El frontend puede ocultar una operación para guiar al usuario; el backend debe impedirla de forma inquebrantable."**

Ningún control cosmético en el cliente sustituye una invariante en el servidor. El flujo de autorización de cada mutación en el backend sigue una secuencia estricta:

```text
HTTP Request
     │
     ▼
¿Autenticado? (Token Bearer válido)
     │
     ▼
¿Autorizado para este recurso? (Admin O Técnico asignado)
     │
     ▼
¿Existe la orden? (Locking pesimista FOR UPDATE en Transacción SQL)
     │
     ▼
¿El estado actual permite la operación? (Máquina de estados de dominio)
     │
     ▼
¿Invariante de dominio satisfecha? (Validación de reglas)
     │
     ▼
Mutación Atómica + Registro de Transición de Auditoría
     │
     ▼
Commit de Transacción
```

### 3.2. Modelo de Capacidades en Respuestas de API (HATEOAS-lite)
Para evitar que React duplique reglas de negocio complejas o mantenga lógica inconsistente con el backend, la API entrega junto a la orden un bloque formal de **capacidades**:

```json
{
  "id": "e3b0c442-98fc-11ee-b9d1-0242ac120002",
  "orderNumber": "OS-0002",
  "status": "IN_REPAIR",
  "vehiclePlate": "ABC123",
  "assignedTechnician": {
    "id": "aaaaaaaa-aaaa-4aaa-8aaa-aaaaaaaaaaa1",
    "name": "Juan Perez"
  },
  "permissions": {
    "canAdvance": true,
    "canAddDiagnostic": false,
    "canAddIntervention": true,
    "canAssign": false
  }
}
```
* **En el Frontend:** Los componentes simplemente consumen `order.permissions.canAddIntervention` para renderizar el formulario o mostrar una insignia de solo lectura.
* **En el Backend:** Si un atacante envía una petición directa ignorando la UI, el backend aplica la misma invariante y responde `403 Forbidden` o `409 Conflict`.

### 3.3. Prevención de Condiciones de Carrera (TOCTOU) en Reasignaciones
En un taller de alto volumen, puede ocurrir que un administrador reasigne una orden al Técnico B en el milisegundo exacto en que el Técnico A intenta registrar una intervención. Para evitar transiciones zombis:
* La consulta de asignación y orden se realiza con bloqueo pesimista:
  ```sql
  SELECT so.id, so.status, a.technician_id
  FROM service_order so
  LEFT JOIN assignment a ON a.service_order_id = so.id AND a.is_active = 1
  WHERE so.id = ?
  FOR UPDATE;
  ```
* Se verifica al actor dentro de la misma transacción. Si el Técnico A perdió la asignación, la transacción aborta limpiamente con `403 Forbidden`.

### 3.4. Máquina de Estados Centralizada en Dominio Go
En `internal/domain/service_order.go` se establecen las reglas definitivas del ciclo de vida:

```go
func (s ServiceOrderStatus) CanAddDiagnostic() bool {
    return s == StatusReceived || s == StatusInDiagnosis
}

func (s ServiceOrderStatus) CanAddIntervention() bool {
    return s == StatusInDiagnosis || s == StatusInRepair
}

func (s ServiceOrderStatus) CanModify() bool {
    return s != StatusDelivered
}
```

### 3.5. Rate Limiting en Go 1.25 con Protección Anti-Enumeración
* **Estructura Sliding Window:** Almacena marcas de tiempo de intentos por clave combinada: `ip:username`.
* **Manejo Seguro de Proxies:** Detección de la IP real considerando cabeceras de confianza `X-Forwarded-For` detrás de Nginx.
* **Recolección de Basura Automática:** Rutina en segundo plano con `time.NewTicker` que elimina claves inactivas cada 10 minutos para evitar saturación de memoria RAM.
* **Respuestas Uniformes:** Respuestas idénticas para credenciales incorrectas, usuarios inexistentes y cuentas bloqueadas, eliminando vectores de enumeración de usuarios.
* **Nota de Escalabilidad Horizontal:** La implementación en memoria es óptima para despliegues de una sola réplica (como Docker Compose local). Para arquitecturas multi-instancia escaladas horizontalmente, se documenta la necesidad de migrar el estado del limitador a Redis o al API Gateway perimetral.

---

## 4. MATRIZ DE SEGURIDAD Y VERIFICACIÓN (TESTING DE REGRESIÓN)

Se establece una suite integral de pruebas automatizadas en Go (`go test -race ./...`) que cubre:

### 4.1. Matriz de Autorización por Endpoint

| Endpoint Mutador | Rol Admin | Técnico Asignado | Técnico No Asignado | Anónimo |
| :--- | :---: | :---: | :---: | :---: |
| `POST /api/customer` | 201 Created | 403 Forbidden | 403 Forbidden | 401 Unauthorized |
| `GET /api/customer` (Global) | 200 OK | 403 Forbidden | 403 Forbidden | 401 Unauthorized |
| `GET /api/service-order/:id` | 200 OK | 200 OK | 200 OK (Solo lectura) | 401 Unauthorized |
| `POST /api/service-order/:id/status` | 200 OK | 200 OK | 403 Forbidden | 401 Unauthorized |
| `POST /api/service-order/:id/diagnostic` | 403 Forbidden | 201 Created | 403 Forbidden | 401 Unauthorized |
| `POST /api/service-order/:id/intervention`| 403 Forbidden | 201 Created | 403 Forbidden | 401 Unauthorized |
| `POST en orden DELIVERED` | 409 Conflict | 409 Conflict | 403 / 409 | 401 Unauthorized |

### 4.2. Pruebas de Resistencia Concurrente (`go test -race`)
* Simulación de 50 peticiones simultáneas de reasignación vs. cambios de estado.
* Garantía de cero escrituras dobles y cero estados inconsistentes en la base de datos.
* Prueba de saturación de 100 intentos en el login para verificar el bloqueo estricto en el 6º intento (`429 Too Many Requests`).

---

## 5. BANCO DE PREGUNTAS Y ARGUMENTACIÓN PARA LA SUSTENTACIÓN

### P1: ¿Por qué la solución no fue simplemente "arreglar los endpoints que fallaron"?
> **Respuesta:** Porque en ciberseguridad, parchar síntomas aislados solo pospone la brecha. Si solo arreglábamos `POST /status`, el mismo fallo de BOLA habría reaparecido en intervenciones o diagnósticos. Implementamos un modelo de invariantes de dominio y autorización transaccional atómica que erradica la **clase completa** de vulnerabilidades de control de acceso.

### P2: ¿Por qué no usar expresiones regulares para evitar SQL Injection?
> **Respuesta:** Las expresiones regulares son una defensa equivocada y frágil contra SQL Injection. La única protección matemáticamente segura es la **parametrización de consultas preparadas** en el driver de base de datos (`QueryContext(..., "WHERE id = ?", id)`). Los datos del usuario nunca se interpretan como código SQL, sin importar los caracteres que contengan. Las regex se reservan únicamente para validar formatos de negocio (como placas o teléfonos).

### P3: ¿Por qué exponer un objeto `permissions` en lugar de que React decida?
> **Respuesta:** Para preservar el principio de "Única Fuente de Verdad" (Single Source of Truth). Si React calcula las reglas por su cuenta, cualquier cambio en las políticas del taller requeriría actualizar dos aplicaciones independientes. Al exponer capacidades desde el backend, el frontend se convierte en un cliente ligero que refleja fielmente las facultades del usuario sin duplicar lógica.

### P4: ¿Cómo garantiza la base de datos que un auto entregado no sea alterado?
> **Respuesta:** Mediante una regla de dominio inmutable validada en la capa de casos de uso y respaldada por la atomicidad de la transacción. Al intentar registrar un diagnóstico o repuesto sobre una orden con estado `DELIVERED`, el sistema rechaza la operación con un error de conflicto (`409`), preservando la inmutabilidad de la historia clínica.

---
*Fin del Documento Fuente Oficial de Arquitectura y Remediación.*
