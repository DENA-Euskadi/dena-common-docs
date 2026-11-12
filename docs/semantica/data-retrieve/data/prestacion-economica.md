# :material-currency-eur: Prestación Económica (Economic Benefit)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **Test:** [DN99DENATestMockObjFactoryForEconomicBenefit.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/economicbenefit/DN99DENATestMockObjFactoryForEconomicBenefit.java)
> - **Código:** [DN00EconomicBenefit.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIEconomicBenefitModelClasses/src/main/java/dena/api/data/model/economicbenefit/DN00EconomicBenefit.java)

## Descripción

Representa un beneficio económico o pago realizado a una persona: prestaciones PEV, ayudas, subvenciones...

> Ver también: [campos-comunes.md](./campos-comunes.md)

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00EconomicBenefit | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIEconomicBenefitModelClasses/src/main/java/dena/api/data/model/economicbenefit/DN00EconomicBenefit.java) |
| DN00EconomicBenefitState | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIEconomicBenefitModelClasses/src/main/java/dena/api/data/model/economicbenefit/DN00EconomicBenefitState.java) |
| DN00EconomicBenefitStateCode | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIEconomicBenefitModelClasses/src/main/java/dena/api/data/model/economicbenefit/DN00EconomicBenefitStateCode.java) |

---

## 🧪 Tests y ejemplos

| Mock Factory | Repositorio |
|--------------|-------------|
| DN99DENATestMockObjFactoryForEconomicBenefit | [Ver código]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/economicbenefit/DN99DENATestMockObjFactoryForEconomicBenefit.java) |

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"economicBenefit"` | Discriminador polimórfico |
| `oid` | `String` | ✅ | `"BEN-OID-001"` | Identificador técnico único |
| `id` | `String` | ✅ | `"BEN-2024-00001"` | Identificador de la prestación |
| `amount` | `Object` | ❌ | `{"amount":50000,"base":"CENTS","currency":"EUR"}` | Importe en euros (centésimas) |
| `paymentOrderedAt` | `String` (ISO 8601) | ❌ | `"2024-05-01T00:00:00Z"` | Fecha de orden de pago |
| `state` | `Object` | ❌ | `{"stateCode":"GRANTED"}` | Estado de la prestación |
| `state.stateCode` | `String` | ❌ | `"GRANTED"` | Código de estado |
| `medium` | `String` | ❌ | `"ACCOUNT_TRANSFER"` | Medio de pago |
| `record` | `Object` | ❌ | *(expediente vinculado)* | Expediente de referencia |
| `urls` | `Array` | ❌ | *(ver campos-comunes.md)* | URLs de acceso |

---

## Estados (`state.stateCode`)

| Código | Descripción |
|--------|-------------|
| `REQUESTED` | Solicitada |
| `GRANTED` | Concedida |
| `PAID` | Pagada |
| `REJECTED` | Rechazada |

---

## Ejemplo JSON

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
  }
}
```

!!! tip "Importe"
    El campo `amount` se expresa en centésimas de euro. Ejemplo: `150000` = 1.500,00 €

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
