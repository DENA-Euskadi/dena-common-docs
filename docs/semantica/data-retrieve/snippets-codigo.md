# Snippets de Código — Implementación del Endpoint

## Descripción

Ejemplos de código para implementar el endpoint `POST /api/retrieveData` en distintos lenguajes de programación. Cada snippet muestra cómo recibir la petición, extraer los campos clave del contexto y devolver la respuesta en el formato esperado por DENA.

---

## Java (Spring Boot)

```java
@RestController
@RequestMapping("/api")
public class RetrieveDataController {

    @PostMapping(value = "/retrieveData", 
                 consumes = MediaType.APPLICATION_JSON_VALUE,
                 produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<Map<String, Object>> retrieveData(@RequestBody Map<String, Object> request) {
        // Extraer contexto
        Map<String, Object> context = (Map<String, Object>) request.get("context");
        Map<String, Object> subjectPerson = (Map<String, Object>) context.get("subjectPerson");
        Map<String, Object> dataType = (Map<String, Object>) context.get("dataType");

        String personId = (String) subjectPerson.get("personId");
        String dataTypeId = (String) dataType.get("dataTypeId");

        // Buscar datos según tipo
        List<Map<String, Object>> dataItems = switch (dataTypeId) {
            case "ADMINISTRATIVE_RECORD"          -> fetchRecords(personId);
            case "ADMINISTRATIVE_NOTICE"          -> fetchNotices(personId);
            case "ADMINISTRATIVE_LEDGER"          -> fetchLedger(personId);
            case "PAYMENT_ONE_OFF_PAYMENT"        -> fetchOneOffPayments(personId);
            case "PAYMENT_DIRECT_DEBIT_PAYMENT"   -> fetchDirectDebits(personId);
            case "SCHEDULE"                       -> fetchSchedule(personId);
            case "PERSON_DATA"                    -> fetchPersonData(personId);
            case "PERSON_CREDENTIAL"              -> fetchCredentials(personId);
            case "PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE" -> fetchBasqueLanguageDegrees(personId);
            case "ECONOMIC_BENEFIT"               -> fetchEconomicBenefits(personId);
            case "ADMINISTRATIVE_PERSON_REGISTER" -> fetchPersonRegisters(personId);
            case "ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV" -> fetchHousingPEVRegisters(personId);
            case "ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL"   -> fetchLastWillRegisters(personId);
            case "ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV" -> fetchRevisionPEVRegisters(personId);
            case "PERSON_ENROLLMENT"              -> fetchEnrollments(personId);
            case "PERSON_ACADEMIC_ENROLLMENT"     -> fetchAcademicEnrollments(personId);
            case "PERSON_MEDICAL_DATA"            -> fetchMedicalData(personId);
            default                               -> List.of();
        };

        // Construir response
        Map<String, Object> response = Map.of(
            "context", Map.of(
                "messageType", context.get("messageType"),
                "dataType", dataType,
                "messageCorrelationId", context.get("messageCorrelationId"),
                "flowDirection", "RESPONSE",
                "subjectPerson", subjectPerson
            ),
            "data", Map.of("dataItems", dataItems),
            "code", "OK"
        );

        return ResponseEntity.ok(response);
    }

    private List<Map<String, Object>> fetchRecords(String personId) {
        return List.of(
            Map.of(
                "type", "administrativeServiceProcedureRecord",
                "oid", "EXP-OID-001",
                "id", "EXP-2024-00123",
                "service", Map.of(
                    "serviceNameByLanguage", Map.of("SPANISH", "Licencias de actividad", "BASQUE", "Jarduera-lizentziak"),
                    "originRef", Map.of("id", "SRV-LIC-ACT")
                ),
                "procedure", Map.of(
                    "serviceNameByLanguage", Map.of("SPANISH", "Solicitud de licencia", "BASQUE", "Lizentzia eskaera"),
                    "originRef", Map.of("id", "PROC-LIC-001")
                ),
                "applicationData", Map.of("applicationDate", "2024-03-14T09:00:00Z"),
                "recordData", Map.of(
                    "createdAt", "2024-03-15T10:30:00Z",
                    "lastModifiedAt", "2024-06-01T14:00:00Z",
                    "state", Map.of(
                        "stateCode", "IN_PROGRESS",
                        "description", Map.of("SPANISH", "En tramitación", "BASQUE", "Izapidetzen")
                    ),
                    "interestedParty", Map.of("personId", personId, "displayName", "Ciudadano/a")
                ),
                "urls", List.of(Map.of("url", "https://sede.miadmin.eus/expediente/EXP-2024-00123", "language", "SPANISH", "tags", List.of("default")))
            )
        );
    }
}
```

---

## C# (.NET 8 Minimal API)

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapPost("/api/retrieveData", async (HttpContext http) =>
{
    var request = await http.Request.ReadFromJsonAsync<JsonElement>();
    var context = request.GetProperty("context");
    var personId = context.GetProperty("subjectPerson").GetProperty("personId").GetString();
    var dataTypeId = context.GetProperty("dataType").GetProperty("dataTypeId").GetString();

    var dataItems = dataTypeId switch
    {
        "ADMINISTRATIVE_RECORD"          => GetRecords(personId),
        "ADMINISTRATIVE_NOTICE"          => GetNotices(personId),
        "ADMINISTRATIVE_LEDGER"          => GetLedger(personId),
        "PAYMENT_ONE_OFF_PAYMENT"        => GetOneOffPayments(personId),
        "PAYMENT_DIRECT_DEBIT_PAYMENT"   => GetDirectDebits(personId),
        "SCHEDULE"                       => GetSchedule(personId),
        "PERSON_DATA"                    => GetPersonData(personId),
        "PERSON_CREDENTIAL"              => GetCredentials(personId),
        "PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE" => GetBasqueLanguageDegrees(personId),
        "ECONOMIC_BENEFIT"               => GetEconomicBenefits(personId),
        "ADMINISTRATIVE_PERSON_REGISTER" => GetPersonRegisters(personId),
        "ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV" => GetHousingPEVRegisters(personId),
        "ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL"   => GetLastWillRegisters(personId),
        "ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV" => GetRevisionPEVRegisters(personId),
        "PERSON_ENROLLMENT"              => GetEnrollments(personId),
        "PERSON_ACADEMIC_ENROLLMENT"     => GetAcademicEnrollments(personId),
        "PERSON_MEDICAL_DATA"            => GetMedicalData(personId),
        _                                => new List<object>()
    };

    return Results.Ok(new
    {
        context = new
        {
            messageType = context.GetProperty("messageType").GetString(),
            dataType = new { dataTypeId },
            messageCorrelationId = context.GetProperty("messageCorrelationId").GetString(),
            flowDirection = "RESPONSE",
            subjectPerson = new { personId }
        },
        data = new { dataItems },
        code = "OK"
    });
});

app.Run();
```

---

## Python (FastAPI)

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Any

app = FastAPI()

@app.post("/api/retrieveData")
async def retrieve_data(request: dict[str, Any]) -> dict[str, Any]:
    context = request["context"]
    person_id = context["subjectPerson"]["personId"]
    data_type_id = context["dataType"]["dataTypeId"]

    fetchers = {
        "ADMINISTRATIVE_RECORD": fetch_records,
        "ADMINISTRATIVE_NOTICE": fetch_notices,
        "ADMINISTRATIVE_LEDGER": fetch_ledger,
        "PAYMENT_ONE_OFF_PAYMENT": fetch_one_off_payments,
        "PAYMENT_DIRECT_DEBIT_PAYMENT": fetch_direct_debits,
        "SCHEDULE": fetch_schedule,
        "PERSON_DATA": fetch_person_data,
        "PERSON_CREDENTIAL": fetch_credentials,
        "PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE": fetch_basque_language_degrees,
        "ECONOMIC_BENEFIT": fetch_economic_benefits,
        "ADMINISTRATIVE_PERSON_REGISTER": fetch_person_registers,
        "ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV": fetch_housing_pev_registers,
        "ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL": fetch_last_will_registers,
        "ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV": fetch_revision_pev_registers,
        "PERSON_ENROLLMENT": fetch_enrollments,
        "PERSON_ACADEMIC_ENROLLMENT": fetch_academic_enrollments,
        "PERSON_MEDICAL_DATA": fetch_medical_data,
    }

    data_items = fetchers.get(data_type_id, lambda _: [])(person_id)

    return {
        "context": {
            "messageType": context.get("messageType"),
            "dataType": {"dataTypeId": data_type_id},
            "messageCorrelationId": context.get("messageCorrelationId"),
            "flowDirection": "RESPONSE",
            "subjectPerson": {"personId": person_id},
        },
        "data": {"dataItems": data_items},
        "code": "OK",
    }


def fetch_records(person_id: str) -> list[dict]:
    return [
        {
            "type": "administrativeServiceProcedureRecord",
            "oid": "EXP-OID-001",
            "id": "EXP-2024-00123",
            "service": {
                "serviceNameByLanguage": {"SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak"},
                "originRef": {"id": "SRV-LIC-ACT"},
            },
            "procedure": {
                "serviceNameByLanguage": {"SPANISH": "Solicitud de licencia", "BASQUE": "Lizentzia eskaera"},
                "originRef": {"id": "PROC-LIC-001"},
            },
            "applicationData": {"applicationDate": "2024-03-14T09:00:00Z"},
            "recordData": {
                "createdAt": "2024-03-15T10:30:00Z",
                "lastModifiedAt": "2024-06-01T14:00:00Z",
                "state": {
                    "stateCode": "IN_PROGRESS",
                    "description": {"SPANISH": "En tramitación", "BASQUE": "Izapidetzen"},
                },
                "interestedParty": {"personId": person_id, "displayName": "Ciudadano/a"},
            },
            "urls": [{"url": "https://sede.miadmin.eus/expediente/EXP-2024-00123", "language": "SPANISH", "tags": ["default"]}],
        }
    ]
```

---

## Node.js (Express)

```javascript
const express = require('express');
const app = express();
app.use(express.json());

app.post('/api/retrieveData', (req, res) => {
  const { context } = req.body;
  const personId = context.subjectPerson.personId;
  const dataTypeId = context.dataType.dataTypeId;

  const fetchers = {
    ADMINISTRATIVE_RECORD: fetchRecords,
    ADMINISTRATIVE_NOTICE: fetchNotices,
    ADMINISTRATIVE_LEDGER: fetchLedger,
    PAYMENT_ONE_OFF_PAYMENT: fetchOneOffPayments,
    PAYMENT_DIRECT_DEBIT_PAYMENT: fetchDirectDebits,
    SCHEDULE: fetchSchedule,
    PERSON_DATA: fetchPersonData,
    PERSON_CREDENTIAL: fetchCredentials,
    PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE: fetchBasqueLanguageDegrees,
    ECONOMIC_BENEFIT: fetchEconomicBenefits,
    ADMINISTRATIVE_PERSON_REGISTER: fetchPersonRegisters,
    ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV: fetchHousingPEVRegisters,
    ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL: fetchLastWillRegisters,
    ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV: fetchRevisionPEVRegisters,
    PERSON_ENROLLMENT: fetchEnrollments,
    PERSON_ACADEMIC_ENROLLMENT: fetchAcademicEnrollments,
    PERSON_MEDICAL_DATA: fetchMedicalData,
  };

  const dataItems = (fetchers[dataTypeId] || (() => []))(personId);

  res.json({
    context: {
      messageType: context.messageType,
      dataType: { dataTypeId },
      messageCorrelationId: context.messageCorrelationId,
      flowDirection: 'RESPONSE',
      subjectPerson: { personId },
    },
    data: { dataItems },
    code: 'OK',
  });
});

function fetchRecords(personId) {
  return [
    {
      type: 'administrativeServiceProcedureRecord',
      oid: 'EXP-OID-001',
      id: 'EXP-2024-00123',
      service: {
        serviceNameByLanguage: { SPANISH: 'Licencias de actividad', BASQUE: 'Jarduera-lizentziak' },
        originRef: { id: 'SRV-LIC-ACT' },
      },
      procedure: {
        serviceNameByLanguage: { SPANISH: 'Solicitud de licencia', BASQUE: 'Lizentzia eskaera' },
        originRef: { id: 'PROC-LIC-001' },
      },
      applicationData: { applicationDate: '2024-03-14T09:00:00Z' },
      recordData: {
        createdAt: '2024-03-15T10:30:00Z',
        lastModifiedAt: '2024-06-01T14:00:00Z',
        state: {
          stateCode: 'IN_PROGRESS',
          description: { SPANISH: 'En tramitación', BASQUE: 'Izapidetzen' },
        },
        interestedParty: { personId, displayName: 'Ciudadano/a' },
      },
      urls: [{ url: 'https://sede.miadmin.eus/expediente/EXP-2024-00123', language: 'SPANISH', tags: ['default'] }],
    },
  ];
}

app.listen(8080);
```

---

## PHP (Laravel)

```php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::post('/api/retrieveData', function (Request $request) {
    $context = $request->input('context');
    $personId = $context['subjectPerson']['personId'];
    $dataTypeId = $context['dataType']['dataTypeId'];

    $dataItems = match ($dataTypeId) {
        'ADMINISTRATIVE_RECORD'          => fetchRecords($personId),
        'ADMINISTRATIVE_NOTICE'          => fetchNotices($personId),
        'ADMINISTRATIVE_LEDGER'          => fetchLedger($personId),
        'PAYMENT_ONE_OFF_PAYMENT'        => fetchOneOffPayments($personId),
        'PAYMENT_DIRECT_DEBIT_PAYMENT'   => fetchDirectDebits($personId),
        'SCHEDULE'                       => fetchSchedule($personId),
        'PERSON_DATA'                    => fetchPersonData($personId),
        'PERSON_CREDENTIAL'              => fetchCredentials($personId),
        'PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE' => fetchBasqueLanguageDegrees($personId),
        'ECONOMIC_BENEFIT'               => fetchEconomicBenefits($personId),
        'ADMINISTRATIVE_PERSON_REGISTER' => fetchPersonRegisters($personId),
        'ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV' => fetchHousingPEVRegisters($personId),
        'ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL'   => fetchLastWillRegisters($personId),
        'ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV' => fetchRevisionPEVRegisters($personId),
        'PERSON_ENROLLMENT'              => fetchEnrollments($personId),
        'PERSON_ACADEMIC_ENROLLMENT'     => fetchAcademicEnrollments($personId),
        'PERSON_MEDICAL_DATA'            => fetchMedicalData($personId),
        default                          => [],
    };

    return response()->json([
        'context' => [
            'messageType' => $context['messageType'],
            'dataType' => ['dataTypeId' => $dataTypeId],
            'messageCorrelationId' => $context['messageCorrelationId'],
            'flowDirection' => 'RESPONSE',
            'subjectPerson' => ['personId' => $personId],
        ],
        'data' => ['dataItems' => $dataItems],
        'code' => 'OK',
    ]);
});

function fetchRecords(string $personId): array {
    return [
        [
            'type' => 'administrativeServiceProcedureRecord',
            'oid' => 'EXP-OID-001',
            'id' => 'EXP-2024-00123',
            'service' => [
                'serviceNameByLanguage' => ['SPANISH' => 'Licencias de actividad', 'BASQUE' => 'Jarduera-lizentziak'],
                'originRef' => ['id' => 'SRV-LIC-ACT'],
            ],
            'procedure' => [
                'serviceNameByLanguage' => ['SPANISH' => 'Solicitud de licencia', 'BASQUE' => 'Lizentzia eskaera'],
                'originRef' => ['id' => 'PROC-LIC-001'],
            ],
            'applicationData' => ['applicationDate' => '2024-03-14T09:00:00Z'],
            'recordData' => [
                'createdAt' => '2024-03-15T10:30:00Z',
                'lastModifiedAt' => '2024-06-01T14:00:00Z',
                'state' => [
                    'stateCode' => 'IN_PROGRESS',
                    'description' => ['SPANISH' => 'En tramitación', 'BASQUE' => 'Izapidetzen'],
                ],
                'interestedParty' => ['personId' => $personId, 'displayName' => 'Ciudadano/a'],
            ],
            'urls' => [['url' => 'https://sede.miadmin.eus/expediente/EXP-2024-00123', 'language' => 'SPANISH', 'tags' => ['default']]],
        ],
    ];
}
```

---

## Manejo de errores (todos los lenguajes)

La respuesta de error debe seguir esta estructura:

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": null,
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Persona no encontrada en el sistema" }
}
```

### Java — Error handling

```java
@ExceptionHandler(PersonNotFoundException.class)
public ResponseEntity<Map<String, Object>> handleNotFound(PersonNotFoundException ex,
                                                          HttpServletRequest request) {
    Map<String, Object> response = Map.of(
        "context", Map.of(
            "messageType", "PERSON_FETCH_DATA",
            "flowDirection", "RESPONSE"
        ),
        "data", (Object) null,  // null explícito
        "code", "CLIENT_ERR",
        "errorId", "PERSON_NOT_FOUND",
        "details", Map.of("details", ex.getMessage())
    );
    return ResponseEntity.status(404).body(response);
}
```

### Python — Error handling

```python
from fastapi import HTTPException
from fastapi.responses import JSONResponse

@app.exception_handler(HTTPException)
async def handle_error(request, exc):
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "context": {"messageType": "PERSON_FETCH_DATA", "flowDirection": "RESPONSE"},
            "data": None,
            "code": "CLIENT_ERR" if exc.status_code < 500 else "SERVER_ERR",
            "errorId": "PERSON_NOT_FOUND" if exc.status_code == 404 else "INTERNAL_ERROR",
            "details": {"details": exc.detail},
        },
    )
```

### Node.js — Error handling

```javascript
app.use((err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  res.status(statusCode).json({
    context: { messageType: 'PERSON_FETCH_DATA', flowDirection: 'RESPONSE' },
    data: null,
    code: statusCode < 500 ? 'CLIENT_ERR' : 'SERVER_ERR',
    errorId: err.errorId || 'INTERNAL_ERROR',
    details: { details: err.message },
  });
});
```

---

## Documentación relacionada

- [endpoint-data-retrieve.md](./endpoint-data-retrieve.md) — Contrato completo del endpoint
- [validaciones.md](./validaciones.md) — Reglas de formato y validación
- [errores-troubleshooting.md](./errores-troubleshooting.md) — Guía de errores comunes










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
