# :material-hospital: Datos Médicos (Person Medical Data)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **Test:** [DN99DENATestMockObjFactoryForPersonMedicalData.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/persondata/medical/DN99DENATestMockObjFactoryForPersonMedicalData.java)
> - **Código:** [DN00PersonMedicalData.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonDataModelClasses/src/main/java/dena/api/data/model/persondata/medical/DN00PersonMedicalData.java)

## Descripción

Especialización de `personData` que añade información sanitaria: TIS (tarjeta individual sanitaria), aseguradora, médico de cabecera. Extiende `DN00PersonData` con los campos médicos.

> Ver también: [persona.md](./persona.md) para los campos base de datos de persona, y [campos-comunes.md](./campos-comunes.md)

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00PersonMedicalData | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonDataModelClasses/src/main/java/dena/api/data/model/persondata/medical/DN00PersonMedicalData.java) |
| DN00PersonMedicalIDs | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonDataModelClasses/src/main/java/dena/api/data/model/persondata/medical/DN00PersonMedicalIDs.java) |

---

## 🧪 Tests y ejemplos

| Mock Factory | Repositorio |
|--------------|-------------|
| DN99DENATestMockObjFactoryForPersonMedicalData | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/persondata/medical/DN99DENATestMockObjFactoryForPersonMedicalData.java) |

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"personMedicalData"` | Discriminador polimórfico |
| `medicalPassId` | `String` | ❌ | `"01234567890"` | Número TIS (Tarjeta Individual Sanitaria) |
| `medicalInsuranceId` | `String` | ❌ | `"MUTU-2024-001"` | Número de aseguradora o mutua |
| `doctorFullName` | `Object` | ❌ | `{"name":"García","surname":"López"}` | Médico/a de cabecera |
| `doctorFullName.name` | `String` | ❌ | `"María"` | Nombre |
| `doctorFullName.surname` | `String` | ❌ | `"García López"` | Apellido(s) |
| `urls` | `Array` | ❌ | *(ver campos-comunes.md)* | URLs (incluyendo `tel:` y `mailto:`) |

!!! info "URLs de contacto"
    Además de URLs estándar, este tipo puede incluir entradas `tel:+34600123456` (tag `phone`) y `mailto:persona@ejemplo.com` (tag `email`) en el array `urls`.

---

## Ejemplo JSON

```json
{
  "type": "personMedicalData",
  "medicalPassId": "01234567890",
  "medicalInsuranceId": "OSAKIDETZA-001",
  "doctorFullName": { "name": "María", "surname": "García López" },
  "urls": [
    { "url": "tel:+34600123456", "tags": ["phone"] },
    { "url": "mailto:paciente@ejemplo.com", "tags": ["email"] }
  ]
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
