# :material-book-open-page-variant: Registro Oficial → Libro de Registro

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}

!!! warning "Tipo eliminado en v0.5.20"
    El tipo `ADMINISTRATIVE_OFFICIAL_REGISTER` ha sido **eliminado** en la versión 0.5.20.
    Ha sido reemplazado por el tipo **`ADMINISTRATIVE_LEDGER`** (Libro de Registro).

    Consulta la documentación actualizada en: [Libro de Registro (Ledger)](./libro-registro.md)

## Migración

| v0.3.x | v0.5.x |
|--------|--------|
| `administrativeOfficialRegisterRecord` | `administrativeLedger` |
| `DN00AdministrativeOfficialRegisterRecord` | `DN00AdministrativeLedger` |
| `denaCommonDataAPIAdministrativeOfficialRegisterModelClasses` | `denaCommonDataAPIAdministrativeLedgerModelClasses` |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
