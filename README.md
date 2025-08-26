# 🏛️ Legal Cases API

API REST pública para casos legales, desplegada en GitHub Pages para práctica y desarrollo.

## 🚀 URL de la API

```
https://kikeestradadev.github.io/api-exp/api.json
```

## 📦 Instalación y Despliegue

### Método 1: Deploy automático con npm (Recomendado)

```bash
# 1. Clonar el repositorio
git clone https://github.com/kikeestradadev/api-exp.git
cd api-exp

# 2. Instalar dependencias
npm install

# 3. Desplegar a GitHub Pages
npm run deploy
```

### Método 2: Deploy manual

```bash
# 1. Subir cambios a GitHub
git add .
git commit -m "Update API"
git push origin main

# 2. Activar GitHub Pages desde Settings → Pages
# Branch: main, Folder: / (root)
```

## 🛠️ Scripts disponibles

```bash
# Desplegar a GitHub Pages
npm run deploy

# Desplegar forzadamente (si hay conflictos)
npm run deploy:force

# Servir localmente en puerto 3000
npm start

# Servir localmente en puerto 8080 y abrir navegador
npm run serve

# Preview local
npm run preview

# Validar que el JSON es válido
npm run validate-json

# Probar la API en producción
npm test
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
    const response = await fetch('https://kikeestradadev.github.io/api-exp/api.json');
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
$.getJSON('https://kikeestradadev.github.io/api-exp/api.json')
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
response = requests.get('https://kikeestradadev.github.io/api-exp/api.json')
cases = response.json()

print(f"Total de casos: {len(cases)}")

# Filtrar por corte específica
san_jose_cases = [case for case in cases if 'San Jose' in case['courtName']]
print(f"Casos en San José: {len(san_jose_cases)}")
```

### cURL

```bash
# Obtener todos los casos
curl -X GET "https://kikeestradadev.github.io/api-exp/api.json" \
  -H "Accept: application/json"

# Usar jq para filtrar (si tienes jq instalado)
curl -s "https://kikeestradadev.github.io/api-exp/api.json" | jq '.[] | select(.status == 1)'
```

## 📱 Usando en Postman

### Configuración básica:

1. **Método**: GET
2. **URL**: `https://kikeestradadev.github.io/api-exp/api.json`
3. **Headers** (opcional):
   ```
   Accept: application/json
   Content-Type: application/json
   ```

### Importar colección:

Puedes importar la colección pre-configurada desde:
```
https://kikeestradadev.github.io/api-exp/examples/postman-collection.json
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

## 🔄 Flujo de Desarrollo

### Para actualizar la API:

1. **Editar datos:**
   ```bash
   # Editar api.json con nuevos datos
   npm run validate-json  # Verificar que el JSON es válido
   ```

2. **Probar localmente:**
   ```bash
   npm run serve  # Abre http://localhost:8080
   ```

3. **Desplegar:**
   ```bash
   npm run deploy  # Deploy automático a GitHub Pages
   ```

### Para desarrollo local:

```bash
# Instalar dependencias
npm install

# Iniciar servidor local
npm start

# En otro terminal, probar cambios
npm run validate-json
npm test
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
3. Edita `api.json` o añade nuevas funcionalidades
4. Valida los cambios (`npm run validate-json`)
5. Commit tus cambios (`git commit -am 'Agregar nueva funcionalidad'`)
6. Push a la rama (`git push origin feature/nueva-funcionalidad`)
7. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para detalles.

## 🔧 Troubleshooting

### Problemas comunes:

1. **Deploy falla:**
   ```bash
   npm run deploy:force
   ```

2. **JSON inválido:**
   ```bash
   npm run validate-json
   ```

3. **CORS errors en desarrollo:**
   ```bash
   npm run serve  # Usa http-server con CORS habilitado
   ```

4. **Permisos de GitHub:**
   - Verifica que tienes acceso al repositorio
   - Configura tu token de GitHub si es necesario

## 📞 Soporte

- **Issues:** https://github.com/kikeestradadev/api-exp/issues
- **Discussions:** https://github.com/kikeestradadev/api-exp/discussions
- **Wiki:** https://github.com/kikeestradadev/api-exp/wiki
