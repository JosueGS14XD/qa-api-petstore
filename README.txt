# QA API - PetStore (Postman)

Repositorio público con pruebas de servicios REST para la API **PetStore** documentada en:
https://petstore.swagger.io/

## Objetivo
Validar el flujo principal de gestión de una mascota mediante:
1. Añadir una mascota a la tienda.
2. Consultar la mascota por ID.
3. Actualizar nombre y estado a **Sold**.
4. Consultar mascotas por **Status**.

---

## Estructura del proyecto
- `postman/collections/`: Colección exportada de Postman con requests + scripts de tests.
- `postman/environments/`: Environment exportado con variables reutilizables.
- `evidencias/`: Capturas que sustentan ejecución (request/response/tests).

---

## Requisitos
- Postman Desktop (recomendado)
- Conexión a internet
- URL base: `https://petstore.swagger.io/v2`

---

## Variables utilizadas (Environment)
En `PetStore-env.postman_environment.json` se definen:

- `baseUrl`: `https://petstore.swagger.io/v2`
- `petId`: ID generado al crear la mascota (se guarda automáticamente)
- `petName`: nombre base de la mascota (se guarda automáticamente)

> Nota: `petId` y `petName` se setean automáticamente desde el request 01.

---

## Importación en Postman
1. Abrir Postman.
2. Click en **Import**.
3. Importar:
   - `postman/collections/PetStore.postman_collection.json`
   - `postman/environments/PetStore-env.postman_environment.json`
4. En la esquina superior derecha seleccionar el environment: **PetStore-env**

---

## Ejecución paso a paso (manual)
En la colección ejecutar en orden:

### 01 - POST Add Pet
- Método: POST
- Endpoint: `{{baseUrl}}/pet`
- Resultado esperado:
  - Status 200
  - Body en JSON
  - Se guardan variables:
    - `petId = response.id`
    - `petName = response.name`

### 02 - GET Get pet by id
- Método: GET
- Endpoint: `{{baseUrl}}/pet/{{petId}}`
- Resultado esperado:
  - Status 200
  - `response.id == petId`
  - `response.name == petName`

### 03 - PUT Update pet to sold
- Método: PUT
- Endpoint: `{{baseUrl}}/pet`
- Body: actualiza `name` y `status="sold"`
- Resultado esperado:
  - Status 200
  - `response.id == petId`
  - `response.name` actualizado
  - `response.status == "sold"`

### 04 - GET Get pets by status (Sold)
- Método: GET
- Endpoint: `{{baseUrl}}/pet/findByStatus?status=sold`
- Resultado esperado:
  - Status 200
  - Respuesta es un arreglo
  - El `petId` aparece dentro del listado (validación recomendada)

---

## Ejecución por Runner (Collection Run)
1. Abrir la colección **PetStore**.
2. Click en **Run**.
3. Seleccionar environment **PetStore-env**.
4. Ejecutar toda la colección.

Resultado esperado:
- 4/4 requests con tests en **PASSED**.

---

## Evidencias
Las capturas en `evidencias/` documentan:
- Request + Response
- Tests ejecutados correctamente
- Variables generadas (petId/petName)

---

## Notas técnicas
- La API PetStore puede retornar datos en XML o JSON según headers.
- Para evitar fallos en tests, se recomienda forzar JSON usando:
  - Header: `Accept: application/json`
  - Header: `Content-Type: application/json` (para POST/PUT)
