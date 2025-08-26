# 🏛️ Legal Cases API

API REST pública para casos legales, desplegada en GitHub Pages para práctica y desarrollo.

## 🚀 URL de la API

```
https://TU-USUARIO.github.io/api-exp/api.json
```

> **Nota:** Reemplaza `TU-USUARIO` con tu nombre de usuario de GitHub.

## 📦 Instalación y Despliegue

### 1. Subir a GitHub

```bash
# Inicializar repositorio (si no lo has hecho)
git init
git add .
git commit -m "Initial commit: Legal Cases API"

# Crear repositorio en GitHub y subir
git remote add origin https://github.com/TU-USUARIO/api-exp.git
git branch -M main
git push -u origin main
```

### 2. Activar GitHub Pages

1. Ve a tu repositorio en GitHub
2. Click en **Settings** (Configuración)
3. Scroll hasta **Pages** en el menú lateral
4. En **Source**, selecciona **Deploy from a branch**
5. Selecciona **main** branch y **/ (root)**
6. Click **Save**

### 3. Esperar el despliegue

GitHub Pages tardará unos minutos en estar disponible. Recibirás la URL final que será:
```
https://tu-usuario.github.io/api-exp/
```

## 🔗 Endpoints

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/api.json` | Obtiene todos los casos legales |

## 📊 Estructura de Datos

```json
{
  "id": 1,
  "clientId": 10,
  "caseNumber": "CASE-2025-0001",
  "caseName": "Tax Appeal: Erick vs. Private Party",
  "description": "Descripción detallada del caso...",
  "filingDate": "2025-02-24 16:55:00",
  "courtName": "Cartago Family Court",
  "createdAt": "2025-02-24 18:48:00",
  "updatedAt": "2025-03-26 01:48:00",
  "status": 1,
  "clientName": "Erick Poveda"
}
```

### Campos

- **id**: Identificador único del caso
- **clientId**: ID del cliente
- **caseNumber**: Número de caso único
- **caseName**: Nombre descriptivo del caso
- **description**: Descripción detallada
- **filingDate**: Fecha de presentación
- **courtName**: Nombre de la corte
- **createdAt**: Fecha de creación del registro
- **updatedAt**: Fecha de última actualización
- **status**: Estado del caso (0: Inactivo, 1: Activo)
- **clientName**: Nombre del cliente

## 🔧 Ejemplos de Uso

### JavaScript (Fetch)

```javascript
async function getCases() {
  try {
    const response = await fetch('https://tu-usuario.github.io/api-exp/api.json');
    const cases = await response.json();
    console.log('Total de casos:', cases.length);
    return cases;
  } catch (error) {
    console.error('Error:', error);
  }
}

// Filtrar casos activos
getCases().then(cases => {
  const activeCases = cases.filter(case => case.status === 1);
  console.log('Casos activos:', activeCases.length);
});
```

### jQuery

```javascript
$.getJSON('https://tu-usuario.github.io/api-exp/api.json')
  .done(function(data) {
    $('#caseCount').text(data.length);
    
    // Mostrar casos en una tabla
    data.forEach(function(case) {
      $('#casesTable').append(`
        <tr>
          <td>${case.caseNumber}</td>
          <td>${case.clientName}</td>
          <td>${case.caseName}</td>
          <td>${case.status === 1 ? 'Activo' : 'Inactivo'}</td>
        </tr>
      `);
    });
  })
  .fail(function() {
    console.log('Error al cargar los casos');
  });
```

### Python (requests)

```python
import requests
import json

# Obtener datos de la API
response = requests.get('https://tu-usuario.github.io/api-exp/api.json')
cases = response.json()

print(f"Total de casos: {len(cases)}")

# Filtrar por corte específica
san_jose_cases = [case for case in cases if 'San Jose' in case['courtName']]
print(f"Casos en San José: {len(san_jose_cases)}")
```

### cURL

```bash
# Obtener todos los casos
curl -X GET "https://tu-usuario.github.io/api-exp/api.json" \
  -H "Accept: application/json"

# Usar jq para filtrar (si tienes jq instalado)
curl -s "https://tu-usuario.github.io/api-exp/api.json" | jq '.[] | select(.status == 1)'
```

## 📱 Usando en Postman

### Configuración básica:

1. **Método**: GET
2. **URL**: `https://tu-usuario.github.io/api-exp/api.json`
3. **Headers** (opcional):
   ```
   Accept: application/json
   Content-Type: application/json
   ```

### Pruebas en Postman:

```javascript
// Test script para validar la respuesta
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("Response has cases", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.be.an('array');
    pm.expect(jsonData.length).to.be.greaterThan(0);
});

pm.test("Each case has required fields", function () {
    const jsonData = pm.response.json();
    const firstCase = jsonData[0];
    
    pm.expect(firstCase).to.have.property('id');
    pm.expect(firstCase).to.have.property('caseNumber');
    pm.expect(firstCase).to.have.property('clientName');
    pm.expect(firstCase).to.have.property('status');
});
```

## 🌐 CORS y Acceso

GitHub Pages configura automáticamente los headers CORS necesarios, por lo que puedes consumir esta API desde:

- ✅ Cualquier sitio web
- ✅ Aplicaciones frontend (React, Vue, Angular)
- ✅ Aplicaciones móviles
- ✅ Herramientas como Postman
- ✅ Scripts de servidor

## 🔍 Filtros y Búsquedas

Como es un archivo JSON estático, el filtrado se hace en el cliente:

```javascript
// Buscar por nombre de cliente
const searchClient = (cases, name) => 
  cases.filter(c => c.clientName.toLowerCase().includes(name.toLowerCase()));

// Filtrar por estado
const activeCases = cases.filter(c => c.status === 1);

// Filtrar por corte
const courtCases = cases.filter(c => c.courtName.includes('San Jose'));

// Filtrar por fecha (casos recientes)
const recentCases = cases.filter(c => {
  const caseDate = new Date(c.filingDate);
  const monthAgo = new Date();
  monthAgo.setMonth(monthAgo.getMonth() - 1);
  return caseDate > monthAgo;
});
```

## 📈 Estadísticas Rápidas

```javascript
function getStats(cases) {
  return {
    total: cases.length,
    active: cases.filter(c => c.status === 1).length,
    inactive: cases.filter(c => c.status === 0).length,
    uniqueClients: new Set(cases.map(c => c.clientId)).size,
    courts: new Set(cases.map(c => c.courtName)).size,
    caseTypes: new Set(cases.map(c => c.caseName.split(':')[0])).size
  };
}
```

## 🚀 Próximos Pasos

Para expandir esta API puedes:

1. **Añadir más endpoints** creando archivos JSON adicionales
2. **Implementar versionado** creando carpetas como `/v1/`, `/v2/`
3. **Añadir documentación interactiva** con Swagger/OpenAPI
4. **Migrar a una API real** usando Node.js, Python, etc.

## 🤝 Contribución

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -am 'Agregar nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto es de dominio público y se puede usar libremente para fines educativos y de práctica.
