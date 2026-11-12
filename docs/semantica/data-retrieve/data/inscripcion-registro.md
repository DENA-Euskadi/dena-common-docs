# :material-clipboard-list: Inscripción en Registro (Person Register)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **Test:** [DN99DENATestMockObjFactoryForAdministrativePersonRegister.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/personregister/DN99DENATestMockObjFactoryForAdministrativePersonRegister.java)
> - **Código:** [DN00AdministrativePersonRegisterEntryBase.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/DN00AdministrativePersonRegisterEntryBase.java)

## Descripción

Representa la inscripción de una persona en un registro administrativo. Existen cuatro variantes:

| Tipo (`type`) | Descripción |
|---------------|-------------|
| `administrativePersonRegister` | Inscripción genérica |
| `administrativePersonRegisterForHousingPEV` | Inscripción en registro de vivienda PEV |
| `administrativePersonRegisterForLastWill` | Registro de últimas voluntades |
| `administrativePersonRegisterForRevisionPEV` | Revisión PEV (con ventana de fechas) |

> Ver también: [campos-comunes.md](./campos-comunes.md)

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00AdministrativePersonRegisterEntryBase | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/DN00AdministrativePersonRegisterEntryBase.java) |
| DN00AdministrativePersonRegisterForHousingPEV | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/datatypes/DN00AdministrativePersonRegisterForHousingPEV.java) |
| DN00AdministrativePersonRegisterForLastWill | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/datatypes/DN00AdministrativePersonRegisterForLastWill.java) |
| DN00AdministrativePersonRegisterForRevisionPEV | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/datatypes/DN00AdministrativePersonRegisterForRevisionPEV.java) |
| DN00AdministrativePersonRegisterStateCode | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/DN00AdministrativePersonRegisterStateCode.java) |

---

## 🧪 Tests y ejemplos

| Mock Factory | Repositorio |
|--------------|-------------|
| DN99DENATestMockObjFactoryForAdministrativePersonRegister | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/personregister/DN99DENATestMockObjFactoryForAdministrativePersonRegister.java) |
| DN99DENATestMockObjFactoryForAdministrativePersonRegisterForHousingPEV | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/personregister/DN99DENATestMockObjFactoryForAdministrativePersonRegisterForHousingPEV.java) |
| DN99DENATestMockObjFactoryForAdministrativePersonRegisterForLastWill | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/personregister/DN99DENATestMockObjFactoryForAdministrativePersonRegisterForLastWill.java) |
| DN99DENATestMockObjFactoryForAdministrativePersonRegisterForRevisionPEV | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/personregister/DN99DENATestMockObjFactoryForAdministrativePersonRegisterForRevisionPEV.java) |

---

## Atributos JSON — campos comunes a todas las variantes

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"administrativePersonRegister"` | Discriminador (ver tabla de tipos) |
| `oid` | `String` | ✅ | `"REG-OID-001"` | Identificador técnico único |
| `id` | `String` | ✅ | `"REG-2024-00001"` | Número de inscripción |
| `registeredAt` | `String` (ISO 8601) | ❌ | `"2024-03-01T00:00:00Z"` | Fecha de inscripción |
| `expiresAt` | `String` (ISO 8601) | ❌ | `"2025-03-01T00:00:00Z"` | Fecha de vencimiento |
| `state` | `Object` | ❌ | `{"stateCode":"REGISTERED"}` | Estado de la inscripción |
| `state.stateCode` | `String` | ❌ | `"REGISTERED"` | Código de estado |
| `urls` | `Array` | ❌ | *(ver campos-comunes.md)* | URLs de acceso |

## Campos adicionales — `administrativePersonRegisterForRevisionPEV`

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `startDate` | `String` (ISO 8601) | ❌ | `"2024-06-01T00:00:00Z"` | Inicio del periodo de revisión |
| `endDate` | `String` (ISO 8601) | ❌ | `"2024-09-01T00:00:00Z"` | Fin del periodo de revisión |

---

## Estados (`state.stateCode`)

| Código | Descripción |
|--------|-------------|
| `REGISTERED` | Inscrito |
| `ACTIVE` | Activo |
| `EXPIRED` | Vencido |
| `CANCELLED` | Cancelado |

---

## Ejemplos JSON

=== "Inscripción genérica"

    ```json
    {
      "type": "administrativePersonRegister",
      "oid": "REG-OID-001",
      "id": "REG-2024-00001",
      "registeredAt": "2024-03-01T00:00:00Z",
      "expiresAt": "2025-03-01T00:00:00Z",
      "state": { "stateCode": "REGISTERED" }
    }
    ```

=== "Revisión PEV"

    ```json
    {
      "type": "administrativePersonRegisterForRevisionPEV",
      "oid": "RPEV-OID-001",
      "id": "RPEV-2024-001",
      "registeredAt": "2024-01-15T00:00:00Z",
      "state": { "stateCode": "ACTIVE" },
      "startDate": "2024-06-01T00:00:00Z",
      "endDate": "2024-09-01T00:00:00Z"
    }
    ```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
