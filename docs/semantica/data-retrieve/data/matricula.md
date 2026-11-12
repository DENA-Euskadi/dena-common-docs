# :material-school: Matrícula (Person Enrollment)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **Test:** [DN99DENATestMockObjFactoryForPersonEnrollment.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/enrollment/DN99DENATestMockObjFactoryForPersonEnrollment.java)
> - **Código:** [DN00PersonEnrollmentBase.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/DN00PersonEnrollmentBase.java)

## Descripción

Representa una matrícula de una persona en un curso, programa o estudio. Existe una especialización para matrículas académicas con año y nivel.

| Tipo (`type`) | Descripción |
|---------------|-------------|
| `personEnrollment` | Matrícula genérica (cursos, formación...) |
| `personAcademicEnrollment` | Matrícula académica (universidad, ciclos...) |

> Ver también: [campos-comunes.md](./campos-comunes.md)

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00PersonEnrollmentBase | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/DN00PersonEnrollmentBase.java) |
| DN00PersonAcademicEnrollment | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/academic/DN00PersonAcademicEnrollment.java) |
| DN00PersonEnrollmentStateCode | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/DN00PersonEnrollmentStateCode.java) |

---

## 🧪 Tests y ejemplos

| Mock Factory | Repositorio |
|--------------|-------------|
| DN99DENATestMockObjFactoryForPersonEnrollment | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/enrollment/DN99DENATestMockObjFactoryForPersonEnrollment.java) |
| DN99DENATestMockObjFactoryForPersonAcademicEnrollment | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/enrollment/DN99DENATestMockObjFactoryForPersonAcademicEnrollment.java) |

---

## Atributos JSON — campos comunes

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"personEnrollment"` | Discriminador polimórfico |
| `oid` | `String` | ✅ | `"ENR-OID-001"` | Identificador técnico único |
| `id` | `String` | ✅ | `"ENR-2024-00001"` | Número de matrícula |
| `registeredAt` | `String` (ISO 8601) | ❌ | `"2024-09-01T00:00:00Z"` | Fecha de matriculación |
| `subjectByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"Programación Java"}` | Materia o curso |
| `institutionNameByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"UPV/EHU"}` | Centro educativo o institución |
| `state` | `Object` | ❌ | `{"stateCode":"ENROLLED"}` | Estado de la matrícula |
| `state.stateCode` | `String` | ❌ | `"ENROLLED"` | Código de estado |
| `urls` | `Array` | ❌ | *(ver campos-comunes.md)* | URLs de acceso |

## Campos adicionales — `personAcademicEnrollment`

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `academicYear` | `Integer` | ❌ | `2024` | Año académico |
| `levelByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"3er curso"}` | Nivel o curso |

---

## Estados (`state.stateCode`)

| Código | Descripción |
|--------|-------------|
| `ENROLLED` | Matriculado/a |
| `CANCELLED` | Baja / cancelada |

---

## Ejemplos JSON

=== "Matrícula genérica"

    ```json
    {
      "type": "personEnrollment",
      "oid": "ENR-OID-001",
      "id": "ENR-2024-00001",
      "registeredAt": "2024-09-01T00:00:00Z",
      "subjectByLanguage": { "SPANISH": "Programación Java", "BASQUE": "Java programazioa" },
      "institutionNameByLanguage": { "SPANISH": "Diputación Foral de Bizkaia" },
      "state": { "stateCode": "ENROLLED" }
    }
    ```

=== "Matrícula académica"

    ```json
    {
      "type": "personAcademicEnrollment",
      "oid": "ACENR-OID-001",
      "id": "ACENR-2024-001",
      "registeredAt": "2024-09-15T00:00:00Z",
      "subjectByLanguage": { "SPANISH": "Grado en Ingeniería Informática" },
      "institutionNameByLanguage": { "SPANISH": "Universidad del País Vasco / EHU", "BASQUE": "Euskal Herriko Unibertsitatea / UPV" },
      "state": { "stateCode": "ENROLLED" },
      "academicYear": 2024,
      "levelByLanguage": { "SPANISH": "3er curso", "BASQUE": "3. maila" }
    }
    ```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
