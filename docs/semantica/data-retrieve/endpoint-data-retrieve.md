# Endpoint DATA-RETRIEVE — Especificación para Administraciones

## Endpoint

```
POST /api/retrieveData
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

---

## Request

> Clases Java: [`DN00PersonFetchInteropRequest`]({{ repos.common_interop_api_blob }}/denaCommonInteropAPIModelClasses/src/main/java/dena/api/common/model/interop/person/client/fetch/request/DN00PersonFetchInteropRequest.java) · [`DN00InteropContext`]({{ repos.common_interop_api_blob }}/denaCommonInteropAPIModelClasses/src/main/java/dena/api/common/model/interop/context/DN00InteropContext.java)

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "ADMINISTRATIVE_RECORD" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "REQUEST",
    "originPartyId": "DENA-CORE",
    "destinationPartyId": "ADMIN-001",
    "subjectPerson": { "personId": "12345678A" },
    "administration": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
    "interopRouteData": [
      { "denaComponentId": "apiGateway", "timestamp": "2026-06-01T10:00:00Z" }
    ]
  },
  "consentOid": "CONSENT-OID-2026-001",
  "protocol": {
    "urls": [],
    "timeOut": "30s"
  },
  "data": {
    "person": "PERSON-OID-001"
  }
}
```

| Campo | Obligatorio | Descripción |
|-------|:-----------:|-------------|
| `context.messageType` | ✅ | Tipo de mensaje (`PERSON_FETCH_DATA`). Ver [`DN00InteropMessageType`]({{ repos.common_interop_api_blob }}/denaCommonInteropAPIModelClasses/src/main/java/dena/api/common/model/interop/context/DN00InteropMessageType.java) |
| `context.dataType.dataTypeId` | ✅ | Tipo de dato. Ver tabla de tipos abajo |
| `context.messageCorrelationId` | ✅ | UUID de correlación para trazabilidad |
| `context.flowDirection` | ✅ | `REQUEST` |
| `context.originPartyId` | ❌ | Identificador del origen (ej: `DENA-CORE`) |
| `context.destinationPartyId` | ❌ | Identificador del destino (ej: `ADMIN-001`) |
| `context.subjectPerson.personId` | ✅ | DNI/NIE/NIF de la persona |
| `context.administration.administrationId` | ❌ | Identificador de la administración |
| `context.administration.dir3Code` | ❌ | Código DIR3 |
| `context.interopRouteData` | ❌ | Traza de componentes DENA |
| `consentOid` | ❌ | OID del consentimiento otorgado |
| `protocol.urls` | ❌ | URLs de plantilla del protocolo |
| `protocol.timeOut` | ❌ | Timeout (ej: `"30s"`) |
| `data` | ✅ | Payload de la petición |

### Valores de `dataTypeId`

| Valor | Tipo de dato |
|-------|-------------|
| `ADMINISTRATIVE_RECORD` | Expediente administrativo |
| `ADMINISTRATIVE_NOTICE` | Notificación administrativa |
| `ADMINISTRATIVE_LEDGER` | Libro de registro (entrada/salida) |
| `PAYMENT_ONE_OFF_PAYMENT` | Pago único |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | Domiciliación bancaria |
| `SCHEDULE` | Cita / elemento de agenda |
| `PERSON_DATA` | Datos de persona |
| `PERSON_CREDENTIAL` | Credencial / carné |
| `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | Acreditación de euskera |
| `ECONOMIC_BENEFIT` | Prestación económica |
| `ADMINISTRATIVE_PERSON_REGISTER` | Inscripción en registro |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` | Inscripción vivienda PEV |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` | Últimas voluntades |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV` | Revisión PEV |
| `PERSON_ENROLLMENT` | Matrícula |
| `PERSON_ACADEMIC_ENROLLMENT` | Matrícula académica |
| `PERSON_MEDICAL_DATA` | Datos médicos |

---

## Response exitosa (HTTP 200)

> Clases Java: [`DN00PersonFetchInteropResponse`]({{ repos.common_interop_api_blob }}/denaCommonInteropAPIModelClasses/src/main/java/dena/api/common/model/interop/person/client/fetch/response/DN00PersonFetchInteropResponse.java)

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "ADMINISTRATIVE_RECORD" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" },
    "administration": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" }
  },
  "data": {
    "dataItems": [
      {
        "type": "administrativeServiceProcedureRecord",
        "oid": "EXP-OID-001",
        "id": "EXP-2024-00123",
        "service": {
          "serviceNameByLanguage": { "SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak" },
          "originRef": { "id": "SRV-LIC-ACT" }
        },
        "procedure": {
          "serviceNameByLanguage": { "SPANISH": "Solicitud de licencia de apertura", "BASQUE": "Irekiera-lizentzia eskaera" },
          "originRef": { "id": "PROC-LIC-APER" }
        },
        "applicationData": {
          "applicationDate": "2024-03-14T09:00:00Z"
        },
        "recordData": {
          "createdAt": "2024-03-15T10:30:00Z",
          "lastModifiedAt": "2024-06-01T14:00:00Z",
          "state": {
            "stateCode": "IN_PROGRESS",
            "description": { "SPANISH": "En tramitación", "BASQUE": "Izapidetzen" }
          },
          "interestedParty": {
            "personId": "12345678A",
            "displayName": "Juan García"
          }
        },
        "descriptionByLanguage": { "SPANISH": "Licencia de apertura de actividad", "BASQUE": "Irekiera-lizentzia" },
        "urls": [
          { "url": "https://sede.miadmin.eus/expediente/EXP-2024-00123", "language": "SPANISH", "tags": ["default"] },
          { "url": "https://egoitza.miadmin.eus/espedientea/EXP-2024-00123", "language": "BASQUE", "tags": ["default"] }
        ],
        "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
        "aboutPersonRef": { "personId": "12345678A" }
      }
    ]
  },
  "code": "OK",
  "errorId": null,
  "details": null
}
```

## Response sin datos (HTTP 200)

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "ADMINISTRATIVE_RECORD" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": { "dataItems": [] },
  "code": "OK"
}
```

## Response de error (HTTP 4xx/5xx)

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

### Códigos de estado (`code`)

| Código | Descripción |
|--------|-------------|
| `OK` | Mensaje procesado correctamente |
| `CLIENT_ERR` | Error del cliente (petición malformada, persona no encontrada) |
| `SERVER_ERR` | Error del servidor (error interno) |
| `QUEUED` | Mensaje encolado para procesamiento asíncrono |

---

## Tipos de objeto en `dataItems`

| `type` (discriminador) | `dataTypeId` | Documentación |
|------------------------|--------------|---------------|
| `administrativeServiceProcedureRecord` | `ADMINISTRATIVE_RECORD` | [expediente.md](./data/expediente.md) |
| `administrativeNotice` | `ADMINISTRATIVE_NOTICE` | [notificacion.md](./data/notificacion.md) |
| `administrativeLedger` | `ADMINISTRATIVE_LEDGER` | [libro-registro.md](./data/libro-registro.md) |
| `oneOffPayment` | `PAYMENT_ONE_OFF_PAYMENT` | [pago.md](./data/pago.md) |
| `directDebitPayment` | `PAYMENT_DIRECT_DEBIT_PAYMENT` | [pago.md](./data/pago.md) |
| `scheduleItem` | `SCHEDULE` | [cita.md](./data/cita.md) |
| `personData` | `PERSON_DATA` | [persona.md](./data/persona.md) |
| `personCredential` | `PERSON_CREDENTIAL` | [credencial.md](./data/credencial.md) |
| `personCredentialForBasqueLanguageDegree` | `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | [credencial.md](./data/credencial.md) |
| `economicBenefit` | `ECONOMIC_BENEFIT` | [prestacion-economica.md](./data/prestacion-economica.md) |
| `administrativePersonRegister` | `ADMINISTRATIVE_PERSON_REGISTER` | [inscripcion-registro.md](./data/inscripcion-registro.md) |
| `administrativePersonRegisterForHousingPEV` | `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` | [inscripcion-registro.md](./data/inscripcion-registro.md) |
| `administrativePersonRegisterForLastWill` | `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` | [inscripcion-registro.md](./data/inscripcion-registro.md) |
| `administrativePersonRegisterForRevisionPEV` | `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV` | [inscripcion-registro.md](./data/inscripcion-registro.md) |
| `personEnrollment` | `PERSON_ENROLLMENT` | [matricula.md](./data/matricula.md) |
| `personAcademicEnrollment` | `PERSON_ACADEMIC_ENROLLMENT` | [matricula.md](./data/matricula.md) |
| `personMedicalData` | `PERSON_MEDICAL_DATA` | [datos-medicos.md](./data/datos-medicos.md) |

---

## Ejemplos por tipo de dato

### Expediente (`ADMINISTRATIVE_RECORD`)

```json
{
  "type": "administrativeServiceProcedureRecord",
  "oid": "EXP-OID-001",
  "id": "EXP-2024-00123",
  "service": {
    "serviceNameByLanguage": { "SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak" },
    "originRef": { "id": "SRV-LIC-ACT" }
  },
  "procedure": {
    "serviceNameByLanguage": { "SPANISH": "Solicitud de licencia de apertura", "BASQUE": "Irekiera-lizentzia eskaera" },
    "originRef": { "id": "PROC-LIC-APER" }
  },
  "applicationData": { "applicationDate": "2024-03-14T09:00:00Z" },
  "recordData": {
    "createdAt": "2024-03-15T10:30:00Z",
    "lastModifiedAt": "2024-06-01T14:00:00Z",
    "state": { "stateCode": "IN_PROGRESS", "description": { "SPANISH": "En tramitación", "BASQUE": "Izapidetzen" } },
    "interestedParty": { "personId": "12345678A", "displayName": "Juan García" }
  },
  "descriptionByLanguage": { "SPANISH": "Licencia de apertura de actividad", "BASQUE": "Irekiera-lizentzia" },
  "urls": [
    { "url": "https://sede.miadmin.eus/expediente/EXP-2024-00123", "language": "SPANISH", "tags": ["default"] },
    { "url": "https://egoitza.miadmin.eus/espedientea/EXP-2024-00123", "language": "BASQUE", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Notificación (`ADMINISTRATIVE_NOTICE`)

```json
{
  "type": "administrativeNotice",
  "oid": "NOT-OID-001",
  "id": "NOT-2024-00456",
  "noticeType": "OFFICIAL_NOTICE",
  "issuedAt": "2024-05-20T09:00:00Z",
  "readAt": null,
  "state": {
    "stateCode": "PENDING_TO_BE_READ_BY_DESTINATION",
    "description": { "SPANISH": "Pendiente de lectura", "BASQUE": "Irakurtzeko zain" }
  },
  "subjectByLanguage": {
    "SPANISH": "Resolución de concesión de ayuda",
    "BASQUE": "Laguntza emateko ebazpena"
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00456", "language": "SPANISH", "tags": ["default"] },
    { "url": "https://egoitza.miadmin.eus/jakinarazpena/NOT-2024-00456", "language": "BASQUE", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Libro de Registro (`ADMINISTRATIVE_LEDGER`)

```json
{
  "type": "administrativeLedger",
  "oid": "LED-OID-001",
  "id": "LED-2024-00789",
  "registeredAt": "2024-04-10T08:30:00Z",
  "entryDirection": {
    "stateCode": "ENTRY",
    "description": { "SPANISH": "Entrada", "BASQUE": "Sarrera" }
  },
  "subjectByLanguage": {
    "SPANISH": "Solicitud de licencia de obras",
    "BASQUE": "Obra-lizentzia eskaera"
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/registro/LED-2024-00789", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Pago único (`PAYMENT_ONE_OFF_PAYMENT`)

```json
{
  "type": "oneOffPayment",
  "oid": "PAY-OID-001",
  "id": "PAY-2024-00321",
  "paymentType": "ONE_OFF_PAYMENT",
  "paymentSubjectByLanguage": {
    "SPANISH": "Tasa por licencia de actividad",
    "BASQUE": "Jarduera-lizentziaren tasa"
  },
  "amount": { "amount": 4550, "base": "CENTS", "currency": "EUR" },
  "amountIfSurcharged": { "amount": 5005, "base": "CENTS", "currency": "EUR" },
  "paymentDates": {
    "issuedAt": "2024-05-01T00:00:00Z",
    "dueDate": "2024-06-30T23:59:59Z",
    "surchargedAt": "2024-07-01T00:00:00Z",
    "surchargeDueDate": "2024-07-31T23:59:59Z",
    "paidAt": null
  },
  "data": {
    "code": "PENDING",
    "medium": null,
    "device": null,
    "paymentProcessorId": null,
    "paymentProcessorTransactionId": null,
    "messageByLang": { "SPANISH": "Pendiente de pago", "BASQUE": "Ordaintzeke" }
  },
  "urls": [
    { "url": "https://pago.miadmin.eus/pay/PAY-2024-00321", "language": "SPANISH", "tags": ["payment"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Domiciliación (`PAYMENT_DIRECT_DEBIT_PAYMENT`)

```json
{
  "type": "directDebitPayment",
  "oid": "DD-OID-001",
  "id": "DD-2024-00100",
  "paymentType": "DIRECT_DEBIT",
  "paymentSubjectByLanguage": {
    "SPANISH": "Cuota mensual guardería",
    "BASQUE": "Haur-eskolako hileko kuota"
  },
  "directDebitData": {
    "setAt": "2024-01-15T00:00:00Z",
    "expiresAt": null,
    "frequency": "MONTHLY",
    "medium": "ACCOUNT_TRANSFER",
    "mediumHint": "ES91 **** **** **** 1332"
  },
  "nextChargeAt": "2024-07-01T00:00:00Z",
  "nextChargeAmountInEuro": 120.00,
  "paymentStatus": "ACTIVE",
  "history": [
    { "at": "2024-06-01T00:00:00Z", "amountInEuro": 120.00 },
    { "at": "2024-05-01T00:00:00Z", "amountInEuro": 120.00 }
  ],
  "urls": [
    { "url": "https://sede.miadmin.eus/domiciliacion/DD-2024-00100", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Cita (`SCHEDULE`)

```json
{
  "type": "scheduleItem",
  "oid": "SCHED-OID-001",
  "id": "CITA-2024-00050",
  "year": 2024,
  "monthOfYear": 7,
  "dayOfMonth": 15,
  "hourOfDay": 10,
  "minuteOfHour": 30,
  "durationMinutes": 30,
  "priority": "MEDIUM",
  "subjectByLanguage": {
    "SPANISH": "Cita previa para renovación de DNI",
    "BASQUE": "NAN berritzeko aurretiko hitzordua"
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/cita/CITA-2024-00050", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Credencial (`PERSON_CREDENTIAL`)

```json
{
  "type": "personCredential",
  "oid": "CRED-OID-001",
  "id": "CRED-2024-00001",
  "subjectByLanguage": {
    "SPANISH": "Carné joven europeo",
    "BASQUE": "Gazte txartel europarra"
  },
  "issuedAt": "2024-01-15T00:00:00Z",
  "expiresAt": "2026-01-15T00:00:00Z",
  "state": {
    "stateCode": "VALID",
    "description": { "SPANISH": "Vigente", "BASQUE": "Indarrean" }
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/credencial/CRED-2024-00001", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Acreditación de euskera (`PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE`)

```json
{
  "type": "personCredentialForBasqueLanguageDegree",
  "oid": "EUS-OID-001",
  "id": "EUS-B2-2024-001",
  "subjectByLanguage": {
    "SPANISH": "Título B2 de euskera",
    "BASQUE": "Euskarako B2 titulua"
  },
  "issuedAt": "2024-06-01T00:00:00Z",
  "expiresAt": null,
  "state": {
    "stateCode": "VALID",
    "description": { "SPANISH": "Vigente", "BASQUE": "Indarrean" }
  },
  "CEFRTier": "B2",
  "certifyingEntityByLanguage": {
    "SPANISH": "HABE",
    "BASQUE": "HABE"
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/credencial/EUS-B2-2024-001", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Prestación económica (`ECONOMIC_BENEFIT`)

```json
{
  "type": "economicBenefit",
  "oid": "BEN-OID-001",
  "id": "BEN-PEV-2024-001",
  "amount": { "amount": 150000, "base": "CENTS", "currency": "EUR" },
  "paymentOrderedAt": "2024-05-01T00:00:00Z",
  "state": {
    "stateCode": "GRANTED",
    "description": { "SPANISH": "Concedida", "BASQUE": "Emandakoa" }
  },
  "medium": "ACCOUNT_TRANSFER",
  "mediumHint": "ES91 **** **** **** 1332",
  "urls": [
    { "url": "https://sede.miadmin.eus/prestacion/BEN-PEV-2024-001", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Inscripción en registro (`ADMINISTRATIVE_PERSON_REGISTER`)

```json
{
  "type": "administrativePersonRegister",
  "oid": "REG-OID-001",
  "id": "REG-2024-00001",
  "registeredAt": "2024-03-01T00:00:00Z",
  "expiresAt": "2025-03-01T00:00:00Z",
  "state": {
    "stateCode": "REGISTERED",
    "description": { "SPANISH": "Inscrito", "BASQUE": "Inskribatua" }
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/registro/REG-2024-00001", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Revisión PEV (`ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV`)

```json
{
  "type": "administrativePersonRegisterForRevisionPEV",
  "oid": "RPEV-OID-001",
  "id": "RPEV-2024-001",
  "registeredAt": "2024-01-15T00:00:00Z",
  "expiresAt": "2025-01-15T00:00:00Z",
  "state": {
    "stateCode": "ACTIVE",
    "description": { "SPANISH": "Activo", "BASQUE": "Aktibo" }
  },
  "startDate": "2024-06-01T00:00:00Z",
  "endDate": "2024-09-01T00:00:00Z",
  "urls": [],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Matrícula (`PERSON_ENROLLMENT`)

```json
{
  "type": "personEnrollment",
  "oid": "ENR-OID-001",
  "id": "ENR-2024-00001",
  "registeredAt": "2024-09-01T00:00:00Z",
  "subjectByLanguage": {
    "SPANISH": "Programación Java avanzada",
    "BASQUE": "Java programazio aurreratua"
  },
  "institutionNameByLanguage": {
    "SPANISH": "Diputación Foral de Bizkaia",
    "BASQUE": "Bizkaiko Foru Aldundia"
  },
  "state": {
    "stateCode": "ENROLLED",
    "description": { "SPANISH": "Matriculado/a", "BASQUE": "Matrikulatua" }
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/matricula/ENR-2024-00001", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Matrícula académica (`PERSON_ACADEMIC_ENROLLMENT`)

```json
{
  "type": "personAcademicEnrollment",
  "oid": "ACENR-OID-001",
  "id": "ACENR-2024-001",
  "registeredAt": "2024-09-15T00:00:00Z",
  "subjectByLanguage": {
    "SPANISH": "Grado en Ingeniería Informática",
    "BASQUE": "Informatika Ingeniaritzako Gradua"
  },
  "institutionNameByLanguage": {
    "SPANISH": "Universidad del País Vasco / EHU",
    "BASQUE": "Euskal Herriko Unibertsitatea / UPV"
  },
  "state": {
    "stateCode": "ENROLLED",
    "description": { "SPANISH": "Matriculado/a", "BASQUE": "Matrikulatua" }
  },
  "academicYear": 2024,
  "levelByLanguage": {
    "SPANISH": "3er curso",
    "BASQUE": "3. maila"
  },
  "urls": [
    { "url": "https://www.ehu.eus/matricula/ACENR-2024-001", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

### Datos médicos (`PERSON_MEDICAL_DATA`)

```json
{
  "type": "personMedicalData",
  "medicalPassId": "01234567890",
  "medicalInsuranceId": "OSAKIDETZA-2024-001",
  "doctorFullName": {
    "name": "María",
    "surname": "García López",
    "surname2": null
  },
  "urls": [
    { "url": "tel:+34600123456", "tags": ["phone"] },
    { "url": "mailto:paciente@osakidetza.eus", "tags": ["email"] },
    { "url": "https://osakidetza.eus/cita/12345678A", "language": "SPANISH", "tags": ["default"] }
  ],
  "originAdminRef": { "administrationId": "OSAKIDETZA", "dir3Code": "EA0017100" },
  "aboutPersonRef": { "personId": "12345678A" }
}
```

---

## Autenticación

Si la administración requiere OAuth2, recibirá la cabecera:

```
Authorization: Bearer <access_token>
```

El token se obtiene automáticamente mediante client credentials. Ver [autenticación](../../../autenticacion/core-dena-administracion/index.md).

---

## Códigos HTTP

| Código | Significado |
|--------|-------------|
| `200` | Datos devueltos correctamente (puede ser lista vacía) |
| `400` | Petición malformada o parámetros inválidos |
| `401` | No autorizado (token inválido o expirado) |
| `403` | Prohibido (sin permisos) |
| `404` | Persona no encontrada |
| `500` | Error interno |
| `503` | Servicio no disponible |

---

## Requisitos para la administración

1. Exponer un endpoint `POST` que acepte y devuelva `application/json`
2. Interpretar `context.subjectPerson.personId` para identificar a la persona
3. Interpretar `context.dataType.dataTypeId` para filtrar el tipo de datos
4. Devolver los objetos en el formato del modelo semántico (ver tabla de tipos)
5. Incluir textos multiidioma (castellano y euskera como mínimo)
6. Incluir URLs de acceso a la sede electrónica cuando sea posible
7. Respetar los códigos HTTP estándar
8. Devolver HTTP 200 con `dataItems: []` cuando no hay datos (no usar 404)
9. Responder en menos de 30 segundos
10. Usar `code: "OK"` en respuestas exitosas y `code: "CLIENT_ERR"` o `code: "SERVER_ERR"` en errores

---

## Documentación relacionada

| Documento | Contenido |
|-----------|----------|
| [campos-comunes.md](./data/campos-comunes.md) | Campos heredados por todos los objetos |
| [expediente.md](./data/expediente.md) | Expediente administrativo |
| [notificacion.md](./data/notificacion.md) | Notificación |
| [libro-registro.md](./data/libro-registro.md) | Libro de Registro (Ledger) |
| [pago.md](./data/pago.md) | Pago único y domiciliación |
| [cita.md](./data/cita.md) | Cita / agenda |
| [credencial.md](./data/credencial.md) | Credencial y acreditación de euskera |
| [prestacion-economica.md](./data/prestacion-economica.md) | Prestación económica |
| [inscripcion-registro.md](./data/inscripcion-registro.md) | Inscripción en registro |
| [matricula.md](./data/matricula.md) | Matrícula y matrícula académica |
| [datos-medicos.md](./data/datos-medicos.md) | Datos médicos |
| [validaciones.md](./validaciones.md) | Reglas de formato y validación |
| [errores-troubleshooting.md](./errores-troubleshooting.md) | Guía de errores comunes |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
