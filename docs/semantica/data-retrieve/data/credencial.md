# :material-card-account-details: Credencial (Person Credential)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **Test:** [DN99DENATestMockObjFactoryForPersonCredential.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/credential/DN99DENATestMockObjFactoryForPersonCredential.java)
> - **Código:** [DN00PersonCredential.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonCredentialModelClasses/src/main/java/dena/api/data/model/credential/DN00PersonCredential.java)

## Descripción

Representa una credencial o carné emitido a una persona: carné joven, licencias, titulaciones...

Existe una especialización para acreditaciones de euskera: `personCredentialForBasqueLanguageDegree` (tipo `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE`).

> Ver también: [campos-comunes.md](./campos-comunes.md)

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00PersonCredential | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonCredentialModelClasses/src/main/java/dena/api/data/model/credential/DN00PersonCredential.java) |
| DN00PersonCredentialForBasqueLanguageDegree | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonCredentialModelClasses/src/main/java/dena/api/data/model/credential/language/DN00PersonCredentialForBasqueLanguageDegree.java) |
| DN00PersonCredentialState | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonCredentialModelClasses/src/main/java/dena/api/data/model/credential/DN00PersonCredentialState.java) |
| DN00PersonCredentialStateCode | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonCredentialModelClasses/src/main/java/dena/api/data/model/credential/DN00PersonCredentialStateCode.java) |
| DN00LanguageDegreeCEFRTier | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonCredentialModelClasses/src/main/java/dena/api/data/model/credential/language/DN00LanguageDegreeCEFRTier.java) |

---

## 🧪 Tests y ejemplos

| Mock Factory | Repositorio |
|--------------|-------------|
| DN99DENATestMockObjFactoryForPersonCredential | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/credential/DN99DENATestMockObjFactoryForPersonCredential.java) |
| DN99DENATestMockObjFactoryForPersonCredentialForBasqueLanguageDegree | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/credential/DN99DENATestMockObjFactoryForPersonCredentialForBasqueLanguageDegree.java) |

---

## Atributos JSON — `personCredential`

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"personCredential"` | Discriminador polimórfico |
| `oid` | `String` | ✅ | `"CRED-OID-001"` | Identificador técnico único |
| `id` | `String` | ✅ | `"CRED-2024-00001"` | Número de credencial/carné |
| `subjectByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"Carné joven"}` | Descripción de la credencial |
| `issuedAt` | `String` (ISO 8601) | ❌ | `"2024-01-15T00:00:00Z"` | Fecha de emisión |
| `expiresAt` | `String` (ISO 8601) | ❌ | `"2026-01-15T00:00:00Z"` | Fecha de caducidad |
| `state` | `Object` | ❌ | `{"stateCode":"VALID"}` | Estado de la credencial |
| `state.stateCode` | `String` | ❌ | `"VALID"` | Código de estado |
| `record` | `Object` | ❌ | *(expediente vinculado)* | Expediente de referencia (si aplica) |
| `urls` | `Array` | ❌ | *(ver campos-comunes.md)* | URLs de acceso |

## Campos adicionales — `personCredentialForBasqueLanguageDegree`

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"personCredentialForBasqueLanguageDegree"` | Discriminador |
| `CEFRTier` | `String` | ✅ | `"B2"` | Nivel MCER del título de euskera |
| `certifyingEntityByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"HABE","BASQUE":"HABE"}` | Entidad certificadora |

---

## Estados (`state.stateCode`)

| Código | Descripción |
|--------|-------------|
| `VALID` | Vigente |
| `EXPIRED` | Caducada |

## Niveles CEFR (`CEFRTier`)

| Código | Descripción |
|--------|-------------|
| `A1` | Nivel A1 |
| `A2` | Nivel A2 |
| `B1` | Nivel B1 |
| `B2` | Nivel B2 |
| `C1` | Nivel C1 |
| `C2` | Nivel C2 |

---

## Ejemplos JSON

=== "Credencial genérica"

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
      "state": { "stateCode": "VALID" }
    }
    ```

=== "Acreditación de euskera"

    ```json
    {
      "type": "personCredentialForBasqueLanguageDegree",
      "oid": "EUS-OID-001",
      "id": "EUS-B2-2024-001",
      "subjectByLanguage": { "SPANISH": "Título B2 de euskera", "BASQUE": "Euskarako B2 titulua" },
      "issuedAt": "2024-06-01T00:00:00Z",
      "state": { "stateCode": "VALID" },
      "CEFRTier": "B2",
      "certifyingEntityByLanguage": { "SPANISH": "HABE", "BASQUE": "HABE" }
    }
    ```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
