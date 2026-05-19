# 🌎 Zymplo · Dashboard Multi-país

> **Vista ejecutiva** del estado de facturación electrónica + Open Finance bancaria por país. Para detalle técnico (PRs, migrations, paridad estructural) ver `git log MULTIPAIS-DASHBOARD.md` · historial commits archivado.
>
> **Última actualización:** 2026-05-19 · **🇦🇷 AR OF Oracle pool connected** post-#871/#872 (deploy script CL-leftover + smoke paths) · **🇺🇸 USA OF smoke E2E 11/11 verde** post-#865/#866/#869 (seed Akoya + LINK_STATUS bridge + LINK_EMPRESA_ID overflow fix) · **Encadenado OF multipaís cerrado**: PE #861 + UY #862 + mobile screens BO/CL/CO/EC #863 mergeados · OF integration pattern (proxy + mobile hook + bot dispatcher) ahora completo en 11 países (MX/BO/CL/BR/EC/AR/CO/UY/PE/USA + ES sin OF) · **Pendiente DevOps**: activar `OPENFINANCE_{CL,CO,UY,PE}_ENABLED=true` + tokens en runtime · **🚨 PE facturación NO finalizado · no alineado a Oracle (sólo SQLite)** · 5 países con E2E con persistencia Oracle real · CO [PDF](samples/colombia/factura-1-co-SETP5.pdf) · EC [PDF](samples/ecuador/factura-1-ec-001-001-000000001.pdf) · CL [PDF](samples/chile/dte-33-cl-folio-1.pdf) · UY [PDF Oracle real](samples/uruguay/cfe-21-uy-real-oracle.pdf) · USA [PDF Oracle real](samples/usa/invoice-000001-acme-corp.pdf) · PE [preview SQLite](samples/peru/factura-01-pe-F001-00000001.pdf)

## Convención

| Símbolo | Significado |
|---|---|
| ✅ | Cerrado end-to-end (firmado+enviado al regulador / sandbox validado) |
| 🟡 | Code-ready · falta cert real o deploy QA público |
| 🧪 | Listo en repo origen · falta migrar al monorepo |
| ⏸️ | Pausado |
| ❌ | No iniciado / no aplica |

---

## 📊 Snapshot ejecutivo

| País | Facturación | Open Finance | App + WhatsApp | API docs (QA) |
|---|---|---|---|---|
| 🇧🇷 **Brasil** | ✅ E2E confirmado | ✅ Belvo OFDA | ✅ App · ✅ WA | [facturacion-br-qa](https://facturacion-br-qa.zymplo.com/api-docs/) · [openfinance-br-qa](https://openfinance-br-qa.zymplo.com/api-docs/) |
| 🇲🇽 **México** | ✅ E2E confirmado | ✅ Belvo sandbox | ✅ App · ✅ WA | [facturacion-mx-qa](https://facturacion-mx-qa.zymplo.com/api-docs/) · [openfinance-mx-qa](https://openfinance-mx-qa.zymplo.com/api-docs/) |
| 🇧🇴 **Bolivia** | ✅ E2E confirmado | ❌ no aplica (ASFI) | ✅ App · ✅ WA | [facturacion-bo-qa](https://facturacion-bo-qa.zymplo.com/api-docs/) |
| 🇵🇾 **Paraguay** | ✅ E2E (DNIT externo) | ⏸️ pausado | ❌ pendiente | externo (sin monorepo) |
| 🇨🇴 **Colombia** | ✅ **E2E sandbox** (DIAN) | 🟡 Belvo CO · proxy listo (#847) · runtime pendiente DevOps env vars | ✅ App · ✅ WA (#752 DIAN + #848 OF hook + #847 OF proxy) | [facturacion-co-qa](https://facturacion-co-qa.zymplo.com/api-docs/) · [openfinance-co-qa](https://openfinance-co-qa.zymplo.com/api-docs/) · [PDF](samples/colombia/factura-1-co-SETP5.pdf) |
| 🇦🇷 **Argentina** | 🧪 AFIP sandbox (legacy externo · sin migrar) | ✅ Prometeo sandbox · thin Oracle **connected 2026-05-19** (post-#871 + #872 destrabar deploy script CL-leftover + smoke paths) · gating zymplo-staging activo | ✅ App · ✅ WA (PR #846 hook + #845 proxy + #858 screens · DevOps activó `OPENFINANCE_AR_ENABLED=true`) | [openfinance-ar-qa](https://openfinance-ar-qa.zymplo.com/api-docs/) · [runbook](argentina/zymplo-openfinance-ar/docs/RUNBOOK.md) · [smoke E2E](argentina/zymplo-openfinance-ar/scripts/smoke-e2e-qa.sh) |
| 🇨🇱 **Chile** | 🟡 **E2E estructural** (cert dummy) | ✅ Belvo sandbox · OF mobile screens dedicadas (#863) · runtime pendiente DevOps env vars | ✅ App · ✅ WA | [facturacion-cl-qa](https://facturacion-cl-qa.zymplo.com/api-docs/) · [openfinance-cl-qa](https://openfinance-cl-qa.zymplo.com/api-docs/) · [PDF](samples/chile/dte-33-cl-folio-1.pdf) |
| 🇪🇨 **Ecuador** | ✅ **E2E sandbox** | ✅ Prometeo sandbox **E2E real 2026-05-08** · OF mobile screens dedicadas (#863) | ✅ App · ✅ WA (#755 bot · scaffold #678/#679/#749 público-view) | [facturacion-ec-qa](https://facturacion-ec-qa.zymplo.com/api-docs/) · [openfinance-ec-qa](https://openfinance-ec-qa.zymplo.com/api-docs/) · [PDF](samples/ecuador/factura-1-ec-001-001-000000001.pdf) |
| 🇵🇪 **Perú** | ❌ **NO alineado a Oracle ZMP** · E2E NO finalizado · usa SQLite | ✅ Prometeo PE **E2E real 2026-05-08** · OF proxy + mobile hook + bot dispatcher (#861) · runtime pendiente DevOps env vars | ✅ App · ✅ WA (#764 bot + #767 v2 proxy + #768 mobile + #763 PDF+público-view + #861 OF integration) | [openfinance-pe-qa](https://openfinance-pe-qa.zymplo.com/api-docs/) · [PDF preview](samples/peru/factura-01-pe-F001-00000001.pdf) |
| 🇺🇾 **Uruguay** | ✅ **E2E mock + Oracle real 2026-05-13** | ✅ Prometeo UY **E2E real 2026-05-08** · OF proxy + mobile hook + bot dispatcher (#862) · runtime pendiente DevOps env vars | ✅ App · ✅ WA (#759 bot + #760 app + #758 PDF+público-view + #862 OF integration) | [facturacion-uy-qa](https://facturacion-uy-qa.zymplo.com) · [openfinance-uy-qa](https://openfinance-uy-qa.zymplo.com/api-docs/) · [PDF](samples/uruguay/cfe-21-uy-real-oracle.pdf) |
| 🇨🇷 **Costa Rica** | 🚧 código clone de Perú · pendiente owner | ✅ OF QA healthy | ❌ pendiente | [openbanking-cr-qa](https://openbanking-cr-qa.zymplo.com) |
| 🇺🇸 **EEUU** | ✅ **E2E con Oracle real 2026-05-18** | ✅ **E2E smoke 11/11 verde 2026-05-19** · Akoya/Mikomo sandbox · gating zymplo-staging activo (tenant zymplo-staging gating Luz) | ✅ App · ✅ WA (PR #835 hook + #844 proxy + 6 tools) | [facturacion-us-qa](https://facturacion-us-qa.zymplo.com) · [openfinance-us-qa](https://openfinance-us-qa.zymplo.com/healthz) · [PDF](samples/usa/invoice-000001-acme-corp.pdf) |
| 🇪🇸 **España** | 🟡 **AEAT port mergeado #843** · F0-F3 completo · 81 tests · 11 tablas ES_FE_* en Oracle · cert AEAT pendiente Jesús | ❌ no aplica (no aggregator EU integrado) | ✅ App · ✅ WA (PR #843 mobile hook + #849 bot tools + workflow + mount proxy) | externo + en monorepo `espana/zymplo-aeat/` · service deploy pendiente DevOps env runtime |

---

## 🇧🇷 Brasil

- **Facturación**: ✅ **E2E end-to-end confirmado** · firmamos+enviamos a SEFIN Nacional con cert real ICP-Brasil **proporcionado por el contador del cliente piloto** · row 168 persistido en Oracle ZMP con XML firmado + comprimido + envío mTLS exitoso
- **API docs**: https://facturacion-br-qa.zymplo.com/api-docs/
- **Endpoints visualización**: XML raw · HTML standalone (con QR) · PDF A4 (con QR · DANFSe oficial NT 008/2026) · todos por `docu_codi`
- **Sample E2E**: [doc 170 · HTML](https://facturacion-br-qa.zymplo.com/api/v1/nfse/170/visualizacao) · [PDF oficial](https://facturacion-br-qa.zymplo.com/api/v1/nfse/170/pdf?inline=true) (clickthrough QA público · ver [PATTERN-PUBLIC-VIEW.md](PATTERN-PUBLIC-VIEW.md))
- **Open Finance**: ✅ Belvo OFDA sandbox · [openfinance-br-qa](https://openfinance-br-qa.zymplo.com/api-docs/) · widget mobile cerrado
- **Para PROD**: (1) cert ICP-Brasil del cliente final · (2) **adesão CNC NFS-e** en la Prefeitura Fortaleza (trámite del cliente · típicamente bloqueante) · (3) plan Belvo prod negociado

---

## 🇲🇽 México

- **Facturación**: ✅ E2E COMPLETO sandbox · emisión + persistencia normalizada + visualización + PDF binario, todo via API · validado 2026-05-08 con UUID `46b60ce9-74a8-454a-96dd-7273032a2081` (total $23,200 MXN · 2 items · status AUTORIZADO)
- **Persistencia**: ✅ Oracle ZMP · `MX_CFDI_HISTORICO` (CFDI completo) + `MX_CFDI_ITEM` (items normalizados, post-migration `2026-05-06_mexico_cfdi_item.sql`)
- **PDF formato oficial**: ✅ CFDI 4.0 binario · `application/pdf` · render server-side con pdfkit + QR de validación SAT (`https://verificacfdi.facturaelectronica.sat.gob.mx`) · endpoint `/cfdi/{uuid}/pdf?inline=true` (PR #700 cierra deuda Facturama-prod-PDF para sandbox)
- **API docs**: https://facturacion-mx-qa.zymplo.com/api-docs/ · 38 paths · incluye `/cfdi/preview`, `/cfdi/validate`, `/cfdi/lote/*`, `/cfdi/draft/*` (PR #698 cerró gap Swagger)
- **Sample E2E (clickthrough)**: [CFDI 46b60ce9... · HTML](https://facturacion-mx-qa.zymplo.com/cfdi/46b60ce9-74a8-454a-96dd-7273032a2081/preview.html) · [PDF formato CFDI 4.0](https://facturacion-mx-qa.zymplo.com/cfdi/46b60ce9-74a8-454a-96dd-7273032a2081/pdf?inline=true) (clickthrough QA público · ver [PATTERN-PUBLIC-VIEW.md](PATTERN-PUBLIC-VIEW.md))
- **Open Finance**: ✅ Belvo sandbox · cubre MX (BBVA · Banamex · Santander · HSBC · Banorte) · [openfinance-mx-qa](https://openfinance-mx-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert SAT del cliente final (CSD productivo · gratis · trámite cliente) · (2) plan Facturama productivo (cobra por timbre · sandbox actual usa mock-pac · sellos placeholder) · (3) plan Belvo prod negociado

---

## 🇧🇴 Bolivia

- **Facturación**: ✅ E2E estructural confirmado · **certificado ADSIB de prueba conseguido** · CUF persistido en Oracle ZMP · audit log verificado 2026-05-07 · emit 2026-05-11 con KUDE oficial SIN renderizado
- **PDF formato oficial**: ✅ KUDE binario · `application/pdf` · render server-side con pdfkit + QR de validación SIN (`https://siat.impuestos.gob.bo/consulta/QR`) + Ley 453 · endpoint `/factura/{cuf}/pdf`
- **API docs**: https://facturacion-bo-qa.zymplo.com/api-docs/
- **Sample E2E (clickthrough)**: [CUF 0001023...3A302C3 · HTML](https://facturacion-bo-qa.zymplo.com/factura/00010234567892026051118153898000011101000000000400003ED663F1DC0DEEAC3A302C3/preview.html) · [PDF formato KUDE](https://facturacion-bo-qa.zymplo.com/factura/00010234567892026051118153898000011101000000000400003ED663F1DC0DEEAC3A302C3/pdf?inline=true) (clickthrough QA público · ver [PATTERN-PUBLIC-VIEW.md](PATTERN-PUBLIC-VIEW.md))
- **Estado envío al SIN**: ⚠️ `siatResponse=(mock)` · estructura XML firmada + CUF + CUFD + QR oficial SIN OK · pero **no llegó al endpoint real** `pilotosiat.impuestos.gob.bo`. Causa: `isMockMode()` ([bolivia/zymplo-siat/src/adapters/siat/SiatClient.js:26](bolivia/zymplo-siat/src/adapters/siat/SiatClient.js#L26)) requiere `SIAT_MODE='sandbox'` ✓ **AND** `SIAT_TOKEN_DELEGADO` no-vacío ❌. Hoy está vacío.
- **Bloqueo para E2E real real**: trámite externo del contribuyente ante SIN piloto · delegar facultades de emisión a Zymplo en el portal `pilotosiat.impuestos.gob.bo` · output del trámite es el `SIAT_TOKEN_DELEGADO` que se pega en Doppler. Sin gestión en curso al 2026-05-11. Mismo código emite real una vez conseguido el token · cero cambios de código.
- **Open Finance**: ❌ **NO disponible regulatoriamente** · ASFI no abrió Open Banking · ningún aggregator LatAm cubre Bolivia (Belvo · Prometeo · Pluggy · Salt Edge · todos verificados sin cobertura). La empresa BO usa solo facturación. **Nota**: el código tiene proxy `openfinance-bo` + mobile screens (#863 · `of-bo-link` + `of-bo-account` con `prometeoRef`) listos para activarse cuando/si ASFI abra el marco. Hoy quedan dormidos.
- **Para PROD**: cert ADSIB del cliente final (trámite cliente con ADSIB) + deploy prod

---

## 🇵🇾 Paraguay

- **Facturación**: ✅ DNIT funcionando en repo legacy externo (Zymplo Facturación Electrónica) · NO en monorepo Zymplo todavía
- **API docs**: externa · pendiente migración al monorepo
- **Open Finance**: ⏸️ pausado (80% docu) · sin marco regulatorio bancario abierto · misma situación que BO
- **Para PROD**: migrar facturación al monorepo + Oracle ZMP · OF queda en backlog

---

## 🇨🇴 Colombia

- **Facturación**: ✅ **E2E completo en sandbox 2026-05-12** · zymplo-dian alineado al monorepo (#665 · UBL 2.1 + CUFE SHA-384 · 11 tablas CO_DIAN_*) · live en facturacion-co-qa
- **E2E validado (2026-05-12 · 4 PRs)**: factura SETP5 · Oracle persiste HIS_ID=5 · CUFE c2c7d503... · IVA 19% · total $119.000 COP
  - PR #784 fix shape impuesto (tipo/porcentaje vs codigo/tarifa)
  - PR #785 fix tx visibility (findById post-commit)
  - PR #786 wire generarPdf entrypoint
  - PR #788 + #790 layout DIAN típico (tabla widths, watermark, CUFE compacto)
- **Sample E2E**: [factura-1-co-SETP5.pdf](samples/colombia/factura-1-co-SETP5.pdf) · [factura-1-co-SETP5.xml](samples/colombia/factura-1-co-SETP5.xml)
- **API docs**: https://facturacion-co-qa.zymplo.com/api-docs/
- **Público-view pattern**: ✅ aplicado #748 · `DIAN_PUBLIC_VIEW_ENABLED=true` activa clickthrough HTML+PDF desde dashboard
- **App + WhatsApp F0 MVP**: PaisCodi=32 + FISCAL_COUNTRIES update (#751) · `dian_client.py` + `dian_tools.py` bot WhatsApp (#752) · screens `dian-co-emit` + `dian-co-history` mobile (#753) · NC/ND, drafts, detail/setup pendientes F2+
- **Open Finance**: 🟡 Belvo CO en monorepo · OF mobile screens dedicadas (#863 · `of-co-link` + `of-co-account` con `belvoTxId`) · falta deploy QA público · [openfinance-co-qa](https://openfinance-co-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert real Camerfirma o Andes SCD del cliente · (2) deploy OF QA + smoke e2e · (3) primer emit real (estructural ya validado) · (4) screens F2+ (NC/ND, drafts, detail)

---

## 🇦🇷 Argentina

- **Facturación**: 🧪 AFIP sandbox público · **probado sin costo** · falta migrar al monorepo y conseguir cert real para prod
- **API docs**: facturación todavía no en monorepo · OF en [openfinance-ar-qa](https://openfinance-ar-qa.zymplo.com/api-docs/)
- **Open Finance**: ✅ Prometeo sandbox cubre AR · thin desplegado QA · **Oracle pool connected 2026-05-19** post-#871 (deploy script era copy-paste literal de CL · RUNTIME_DIR apuntaba a `openfinance-cl-service-qa/.env` y leía creds erróneas → mock-mode) + **#872** (smoke E2E script paths · `/institutions` → `/links/institutions`). Pendiente correr smoke completo end-to-end · espera `OPENFINANCE_AR_SERVICE_AUTH_TOKEN` de DevOps por canal seguro
- **Para PROD**: (1) cert AFIP del cliente · (2) migrar zymplo-afip al monorepo · (3) plan Prometeo prod

---

## 🇨🇱 Chile

- **Facturación**: 🟡 **E2E ESTRUCTURAL completo 2026-05-12** (cert+CAF DUMMY · NO firmó+envió SII real) · DTE 33 Factura emitida con TED + XMLDSig + persistencia Oracle + PDF Res. Ex. 80/2014 con PDF417 timbre. Backend QA-ready (DTE F0-F4 · 11 tablas CL_DTE_*)
- **E2E validado (estructural · 2026-05-12)**: DTE id=81 · folio 1 · neto 100k + IVA 19% · total $119.000 CLP · Estado FIRMADO (no enviado a SII porque cert auto-signed)
  - PR #799 fix(cl): expose devMessage cuando SII_AMBIENTE != produccion
- **⚠️ Caveat crítico**: cert PFX auto-firmado + CAF generado localmente · SII rechazaría la firma del CAF y la cadena de trust del cert. PDF y XML estructuralmente SII-compliant pero NO válidos fiscalmente
- **Sample E2E**: [dte-33-cl-folio-1.pdf](samples/chile/dte-33-cl-folio-1.pdf) · [dte-33-cl-folio-1.xml](samples/chile/dte-33-cl-folio-1.xml)
- **API docs**: https://facturacion-cl-qa.zymplo.com/api-docs/
- **Open Finance**: ✅ Belvo sandbox cubre CL (BancoEstado · BCI · etc) · 5 links E2E validados en VM · OF mobile screens dedicadas (#863 · `of-cl-link` + `of-cl-account` con `belvoTxId`) · [openfinance-cl-qa](https://openfinance-cl-qa.zymplo.com/api-docs/)
- **Para PROD real**: (1) **cliente chileno colaborador** que nos preste cert SII (modelo BR) · O · (2) gestionar RUT corporativo Zymplo Chile · (3) cert E-CertChile (~25 USD) + CAFs reales del SII · (4) plan Belvo prod (mismo del MX/BR)

---

## 🇪🇨 Ecuador

- **Facturación**: ✅ **E2E completo en sandbox 2026-05-12** · zymplo-sri F0-F4 · 11 tablas EC_SRI_* · Oracle ZMP wireup activo (post fix #785 OUT_FORMAT_OBJECT + #794 secuencial tx)
- **E2E validado (2026-05-12 · 5 PRs)**: factura `001-001-000000001` · Oracle persiste HIS_ID=1 · claveAcceso 49 dígitos · IVA 15% post-reforma · total $115 USD
  - PR #793 fix swap args validarIdentificacion (receptor route)
  - PR #794 fix SecuencialRepository auto-tx (FOR UPDATE atómico)
  - PR #795 fix factura XML/PDF routes respetan public-view
  - PR #796 fix RidePdfService adapter shape (nested vs flat)
  - PR #797 feat RIDE cleanup · watermark MOCK · status legible · subtotales por tarifa
- **Sample E2E**: [factura-1-ec-001-001-000000001.pdf](samples/ecuador/factura-1-ec-001-001-000000001.pdf) · [factura-1-ec-001-001-000000001.xml](samples/ecuador/factura-1-ec-001-001-000000001.xml)
- **API docs**: https://facturacion-ec-qa.zymplo.com/api-docs/
- **Público-view pattern**: ✅ aplicado #749 + #795 · `SRI_PUBLIC_VIEW_ENABLED=true` activa clickthrough HTML+PDF
- **App + WhatsApp F0**: PaisCodi=31 ya canónico · scaffolding mobile #678/#679 · bot WhatsApp #755 (`sri_tools.py` + registry) · NC/ND, retenciones, guías, liquidaciones F2+
- **Open Finance**: ✅ Prometeo sandbox cubre EC (Pichincha · Intermatico · 5 providers) · OF mobile screens dedicadas (#863 · `of-ec-link` + `of-ec-account` con `prometeoTxId`) · [openfinance-ec-qa](https://openfinance-ec-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert BCE del cliente final (~30 USD · estructural ya validado) · (2) plan Prometeo prod

---

## 🇵🇪 Perú

- **Facturación**: ❌ **E2E NO FINALIZADO** · servicio `peru/zymplo-sunat` **NO está alineado a Oracle ZMP** (usa `better-sqlite3` local · no `oracledb`). El sample que generé el 2026-05-13 persistió en SQLite local, NO en Oracle ZMP. **Pendiente migración Postgres-style → Oracle ZMP** (mismo trabajo que España PR #811 + issue #816)
- **Lo que falta para alinear** (replicar pattern UY/CO/EC/CL/US):
  1. Agregar `oracledb` a `package.json` (hoy solo `better-sqlite3`)
  2. Crear `src/db/connection.js` Oracle pool con thick/thin mode
  3. Reescribir `src/empresas/repository.js` + `documentos-repo.js` SQLite → oracledb
  4. SQL migration: crear `ZMP.PE_SUNAT_HISTORICO` + `PE_SUNAT_DETALLE` + etc. (8 tablas país-específicas como las otras)
  5. Reusar `ZMP_FISC_*` cross-país (operadores · emisores · usuarios · cert vault)
- **Preview estructural (2026-05-13 · sin Oracle)**: Factura 01 F001-00000001 con UBL 2.1 + XMLDSig + PDF SUNAT 097-2012 · persistencia SOLO en SQLite efímero · SUNAT beta rechazó firma del cert dummy (esperado)
  - PR #809 feat(pe/sunat) PDF formato oficial SUNAT 097-2012 (independiente de Oracle)
- **⚠️ Caveats**: (1) servicio NO persiste en Oracle ZMP, sólo SQLite (2) cert PFX auto-firmado · NO válido fiscalmente
- **Sample preview** (NO es E2E con persistencia real): [factura-01-pe-F001-00000001.pdf](samples/peru/factura-01-pe-F001-00000001.pdf) · [factura-01-pe-F001-00000001.xml](samples/peru/factura-01-pe-F001-00000001.xml)
- **PDF formato oficial**: ✅ PdfFacturaService #763 + #809 · pdfkit + qrcode · QR SUNAT RTF 097-2012 · cubre 4 tipos doc (01 Factura · 03 Boleta · 07 NC · 08 ND) · IGV 18% · watermark "PENDIENTE SUNAT" cuando sin sunat_codigo_respuesta='0' · tabla detalle items · monto en letras
- **Público-view pattern**: ✅ aplicado #763 · `SUNAT_PUBLIC_VIEW_ENABLED=true` activa clickthrough HTML+PDF+XML+CDR desde dashboard
- **API docs**: peru/zymplo-sunat en monorepo · OF en [openfinance-pe-qa](https://openfinance-pe-qa.zymplo.com/api-docs/)
- **App + WhatsApp F0 completo (2026-05-12)**:
  - PaisCodi=33 canónico · en FISCAL_COUNTRIES (#764)
  - Bot WhatsApp #764 · `sunat_tools.py` + `sunat_client.py` + registry
  - v2 proxy hexagonal #767 · `zymplo-api/src/modules/sunat-pe/` (12 archivos TS · paralelo a cfe-uy/sri-ec/dian-co · endpoints `/api/v2/sunat-pe/facturas[/:id[/xml]]`)
  - Mobile #768 · hook `useSunatPe.ts` + screens `sunat-pe-emit.tsx` + `sunat-pe-history.tsx`
  - NC/ND (07/08), drafts, detail/setup, anulaciones, ICBPER quedan F2+
- **Open Finance**: 🟡 Prometeo PE en monorepo · **integration pattern completo (#861)**: proxy v2 `openfinance-pe` (hexagonal · feature flag `OPENFINANCE_PE_ENABLED`) + mobile hook `useOpenFinancePe.ts` + bot dispatcher `openfinance_client.py` + `openfinance_tools.py` (PaisCodi.PERU=33) · falta DevOps activar env vars + smoke E2E
- **Para PROD**: (1) **ALINEAR A ORACLE ZMP** (5 items arriba · BLOQUEANTE para que sea verdadero E2E con persistencia, no solo preview) · (2) cert SUNAT del cliente final · (3) v2 proxy + mobile hook + screens (F2) · (4) deploy QA público · (5) plan Belvo prod

---

## 🇺🇾 Uruguay

- **Facturación**: ✅ **E2E con Oracle real validado 2026-05-13** · 7 pasos vía REST APIs · persistencia confirmada en cada paso con sqlcl (`ZMP_FISC_EMISOR` · `ZMP_FISC_USUARIO` · `UY_CFE_HISTORICO` · `UY_CFE_DETALLE`). Service corrió en Oracle migrations VM (thick mode + Instant Client + libnnz) contra `dbautdesa02_high` · provider mock (sin DGI real)
- **E2E real verificado (2026-05-13)**: CFE id=21 · tipo 101 e-Ticket · RUT 213540089545 · UYU 1100 (1000 base + 100 IVA Mín 10%) · estado ACCEPTED · `<eTck v1.43>` + XMLDSig mock
- **PDF formato oficial**: ✅ PdfCfeService #758 + rewrite #772 al layout DGI oficial · pdfkit + qrcode · QR DGI RG 145/2018 · cubre 14 tipos CFE (101 e-Ticket · 111 e-Factura · 121 Exp · 131 e-Remito · 141 e-Resguardo · 181 e-Boleta) · 3 cols header (emisor / caja CFE / QR) + tabla 7 cols zebra + caja totales DGI por tasa (Mín 10% + Bás 22% + No Gravado + Exento) · Ley Defensa Consumidor 17.250 · watermark "PENDIENTE DGI" cuando sin autorización
- **Sample E2E (2026-05-12 in-memory)**: CFE #1 · serie A-1 · tipo 101 e-Ticket · $1.650 UYU · [PDF](samples/uruguay/cfe-1-uy-eTicket-A1.pdf) · [XML](samples/uruguay/cfe-1-uy-eTicket-A1.xml)
- **Sample E2E (2026-05-13 Oracle real)**: CFE #21 · serie A-1 · tipo 101 e-Ticket · $1.100 UYU · persistencia verificada · [📄 PDF DGI layout](samples/uruguay/cfe-21-uy-real-oracle.pdf) · [📋 XML eTck v1.43](samples/uruguay/cfe-21-uy-real-oracle.xml)
- **Público-view pattern**: ✅ aplicado #758 · `CFE_PUBLIC_VIEW_ENABLED=true` activa clickthrough HTML+PDF+XML desde dashboard (⏳ pendiente flag en `.env` runtime nfe-s · cuando active permite URL pública del PDF en lugar del sample en repo)
- **API docs**: [facturacion-uy-qa](https://facturacion-uy-qa.zymplo.com) · [openfinance-uy-qa](https://openfinance-uy-qa.zymplo.com/api-docs/)
- **App + WhatsApp F0 (2026-05-12)**: PaisCodi=27 ya canónico · scaffolding mobile #760 (`cfe-uy-emit.tsx` + `cfe-uy-history.tsx` · tipoCfe 101/111 + tax codes 1-4 + moneda UYU/USD) · bot WhatsApp #759 (`cfe_tools.py` + `cfe_client.py` + registry) · NC/ND, e-Remito/Exp/Resguardo/Boleta, drafts, detail F2+
- **Open Finance**: ✅ Prometeo sandbox cubre UY · thin en monorepo (#593) · **integration pattern completo (#862)**: proxy v2 `openfinance-uy` (hexagonal · feature flag `OPENFINANCE_UY_ENABLED`) + mobile hook `useOpenFinanceUy.ts` + bot dispatcher (PaisCodi.URUGUAY=27 en `_COUNTRY_TO_PROXY_PATH` + tools) · falta DevOps activar env vars + smoke E2E
- **Para PROD**: (1) cert DGI del cliente real · (2) crear user `ZMP_UY_QA` + grants (DevOps lo está coordinando, ver PR #789 Layer A) · (3) container QA correr en thick mode con LD_LIBRARY_PATH+libnnz para que el pool conecte real (ya validado en Oracle migrations VM) · (4) plan Prometeo prod

---

## 🇨🇷 Costa Rica

- **Facturación**: 🚧 **BLOQUEADO por código** · `costarica/zymplo-hacienda/` es un clone literal de `facturacion-peru` (package.json `"name": "facturacion-peru"`, src con endpoints SUNAT, .env.example con `SUNAT_BETA_BILLSERVICE`) · DevOps dejó `facturacion-cr-qa` retornando 503 maintenance hasta que owner haga rename + reescritura adapter MH-CR. **Pendiente**: (1) rename package + .env stub MH-CR, (2) reemplazar integraciones SUNAT por API Hacienda CR (XML + firma según MH).
- **API docs**: facturación bloqueada (503) · Open Banking en [openbanking-cr-qa](https://openbanking-cr-qa.zymplo.com)
- **Open Finance**: ✅ container `openbanking-cr-qa` healthy (DevOps fixó HEALTHCHECK 2026-05-08 · puerto 3003 `/api/health`) · pero el container heredó respuestas con `"service": "openbanking-peru"` · **deuda dev** para owner CR
- **Para PROD**: (1) **owner asignado para destrabe código** (rename + adapter MH-CR · ~1-2 sprints), (2) cert real Hacienda del cliente, (3) limpieza referencias `openbanking-peru` en respuestas OF

---

## 🇺🇸 EEUU

- **Facturación**: ✅ **E2E con Oracle real validado 2026-05-18** · 7 pasos vía REST APIs (login → onboarding → buyer → invoice → PDF/UBL) · persistencia confirmada con sqlcl en cada paso (`ZMP_FISC_OPERADOR` · `ZMP_FISC_EMISOR` · `ZMP_FISC_USUARIO` · `US_FE_BUYER` · `US_FE_INVOICE` · `US_FE_INVOICE_LINE`). Service corre en nfe-s con thick mode + Instant Client (vars + bug volumes :!override fixeados DevOps 2026-05-14)
- **E2E real verificado (2026-05-18)**: Invoice `000001` · ACME CORP buyer · 2 líneas (CONS-001 x $1500 + LIC-PRO x $5000) · subtotal $8000 · tax NY 8.53% via state_lookup = $682.40 · **total $8682.40 USD** · status EMITTED
- **PDF + UBL**: ✅ pdfkit + Puppeteer/Nunjucks template (`invoice.html`) · **UBL 2.1 PEPPOL BIS Billing 3.0** + **EN 16931 compliant** (`urn:cen.eu:en16931:2017#compliant#urn:fdc:peppol.eu:2017:poacc:billing:3.0`)
- **Sample E2E (Oracle real)**: [`invoice-000001-acme-corp.pdf`](samples/usa/invoice-000001-acme-corp.pdf) (77.5KB) · [`invoice-000001-acme-corp.xml`](samples/usa/invoice-000001-acme-corp.xml) (5.5KB UBL)
- **API docs**: [facturacion-us-qa](https://facturacion-us-qa.zymplo.com) · OpenAPI v0.1.0 · 18 endpoints REST
- **Tax intelligence**: state_lookup tabla `US_FE_STATE_TAX` con NY/CA/TX/etc. precargados (state_rate + local_avg)
- **Open Finance**: ✅ **E2E smoke 11/11 verde 2026-05-19** · Akoya/Mikomo sandbox · service [openfinance-us-qa](https://openfinance-us-qa.zymplo.com) corre con `tmp_claude` (paridad fe-us-service-qa · NO requiere ZMP user dedicado) · gating activo en zymplo-staging vía `OPENFINANCE_US_ENABLED=true` + `OPENFINANCE_US_TENANT_API_KEY` (DevOps commit infra e4411ee). 3 PRs cerraron destrabes encadenados: **#865** (seed Akoya en `ZMP.US_OF_INSTITUTION`) · **#866** (bridge LINK_STATUS domain↔Oracle CK constraint) · **#869** (fix LINK_EMPRESA_ID overflow NUMBER(10) · 32-bit hash · caveat birthday-collision ~65k tenants → reemplazar por lookup real `SKN.COME_EMPRESA` en prod)
- **Stack OF**: FDX v6.5 · Akoya adapter (sandbox provider Mikomo) · service Fastify oracledb thick mode · puerto 3074 host / 8000 container · subdomain Cloudflare proxied
- **Schema Oracle**: 7 tablas país-específicas `ZMP.US_OF_*` (TENANT/END_USER/INSTITUTION/SYNC_RUN/OAUTH_STATE/TENANT_API_KEY/AUDIT_LOG · migración aplicada 2026-05-18) + reuso tablas compartidas `ZMP.ZMP_OF_LINK` / `_CRED_VAULT` / `_ACCOUNT` (mismas que Belvo/Prometeo cross-país)
- **Mobile + WhatsApp integration**: ✅ PR #835 + #844 + #850 · `useOpenFinanceUs.ts` (8 hooks React Query) + bot tools `fe_us_tools.py` (3 read) + `openfinance_us_tools.py` (3 read · `erp_consultar_saldo_us` · `erp_listar_bancos_conectados_us` · `erp_listar_movimientos_us`) · country gate `paisCodi=9` · brand voice inglés
- **Smoke E2E script**: `usa/zymplo-openfinance-us/scripts/smoke-e2e-qa.sh` · 11 pasos API + Oracle verify · ✅ corrió verde 2026-05-19 (tenant `b33e3a5b-286a-4c1c-8f4e-f320956aa441` · connection `c3c64b3c-c466-4c3b-bab5-79ce1c356349` · status UNCONFIRMED · audit ≥1 en `ZMP.US_OF_AUDIT_LOG`)
- **Marco regulatorio**: USA **NO tiene mandato federal** de formato PDF para B2B invoices (a diferencia de SAT/DIAN/DGI/SII). Standards de facto: PEPPOL BIS 3.0 (que generamos en UBL) + IRS bookkeeping + state sales tax. El PDF cumple convenciones B2B profesionales sin "compliance" vinculante
- **Para PROD**: (1) cert para PEPPOL gateway si se quiere certificar PEPPOL network · (2) Akoya prod creds (negociar plan) · (3) opcional: rotar logo/branding del PDF · facturación funcional ya · (4) **reemplazar `tenantToEmpresaId` por lookup real contra `SKN.COME_EMPRESA.empresa_id`** (shim de 32 bits OK para QA · birthday collision a ~65k tenants en prod)

---

## 🇪🇸 España

- **Facturación**: 🟡 **AEAT port mergeado 2026-05-18 (#843)** · F0-F3 completo en `espana/zymplo-aeat/` · 192 archivos · +10,176 LOC · 81 tests verdes (65 Jest + 16 vitest hex) · 11 tablas `ZMP.ES_FE_*` aplicadas en `dbautdesa02_high` · paisCodi=5 ES habilitado en SKN.COME_PAIS
- **Stack**: Facturae v3.2.2 + XAdES-EPES (firma local) · adapters para SII real-time + Verifactu hash chain + FACe (AAPP públicas) + PEPPOL Spain (red.es) · IVA 21/10/4/0 + zero-rate exenciones
- **Runbook cert AEAT handoff** (Jesús → DevOps): [docs/RUNBOOK-cert-AEAT-handoff.md](espana/zymplo-aeat/docs/RUNBOOK-cert-AEAT-handoff.md) · pasos 1-7 desde gestión FNMT hasta validación E2E real
- **Smoke E2E script**: [espana/zymplo-aeat/scripts/smoke-e2e-qa.sh](espana/zymplo-aeat/scripts/smoke-e2e-qa.sh) · 12 pasos API + Oracle persistence
- **Mobile screens dedicadas**: [aeat-list](zymplo-mobile/app/(tabs)/aeat-list/index.tsx) + [aeat-detail](zymplo-mobile/app/(tabs)/aeat-detail/[id].tsx) · gate `paisCodi=5`
- **Workflow + bot WA**: ✅ PR #849 · `deploy-qa-espana-aeat-selfhosted.yml` + bot tools `aeat_tools.py` (3 read · `erp_listar_facturas_es` · `erp_buscar_factura_es` · `erp_listar_facturas_pendientes_es`) + mount `fe-es` proxy en `/datos/fe-es/*` (Luz pattern ORDS-canonical · gated por `FE_ES_ENABLED`) · `FEATURE_COUNTRIES.fiscal` incluye ESPAÑA
- **Mobile**: ✅ `useFeEs.ts` (Martín #843)
- **Open Finance**: ❌ **no aplica** · ningún aggregator de la EU integrado en el monorepo (Tink confusión inicial DevOps · descartada · ver respuesta 2026-05-18 a checklist)
- **Estado runtime QA**: pendiente DevOps · template en `servers/nfe-s/facturacion-es-qa/` listo (commit infra 243ae48) · faltan (1) cert AEAT real (Jesús · gestión externa) · (2) activar `FE_ES_ENABLED=true` en zymplo-api `.env` · (3) confirmar DB user TMP_CLAUDE provisional aceptable
- **Subdomain target**: `facturacion-es-qa.zymplo.com` (Cloudflare proxied + wildcard cert)
- **Para PROD**: (1) cert AEAT productivo del cliente · (2) `BILLING_QA_APP` user dedicado post-cert · (3) deploy prod con FE_ES_PROVIDER real (SII + Verifactu + FACe activos)

---

## 🎯 Próximos pasos prioritarios (cross-país)

| Prioridad | Acción | Países afectados |
|---|---|---|
| ~~Alta~~ | ~~**DevOps · DBA reset password ZMP** en `dbautdesa02` · destraba smoke E2E USA OF (ORA-01017)~~ ✅ **Cerrado 2026-05-19** · sync de creds tmp_claude desde fe-us-service-qa + 3 PRs (#865/#866/#869) destrabaron smoke 11/11 verde · NO requería reset de password | USA |
| **Alta** | **DevOps · cargar env vars + activar flags** `OPENFINANCE_{CL,CO,UY,PE,AR}_ENABLED=true` + `FE_ES_ENABLED=true` + `OPENFINANCE_US_*` (post-ZMP password) + `OPENFINANCE_<XX>_SERVICE_AUTH_TOKEN` por país en zymplo-api `.env` runtime | USA · AR · CO · CL · UY · PE · ES |
| **Alta** | **Cert AEAT productivo España** (Jesús · gestión externa) · prerequisite para `BILLING_QA_APP` user dedicado y para emitir contra SII real | ES |
| **Alta** | **Asignar owner CR-Hacienda** · rename `facturacion-peru` → `facturacion-costarica` + reescribir adapter SUNAT → MH-CR (XML + firma según MH) · destraba `facturacion-cr-qa` (hoy 503 maintenance) | 1 país (CR) |
| **Alta** | Replicar modelo BR (cliente colaborador presta cert) en CL · EC · PE · UY · CR | 5 países desbloqueables |
| **Alta** | Negociar plan Belvo productivo (cubre MX · BR · CL · CO · PE en un solo contrato) | 5 países |
| **Media** | Negociar plan Prometeo productivo (cubre AR · UY · PY · EC) | 4 países |
| **Media** | Migrar facturación AR/PY/UY/PE al monorepo (orden Wave 5 · ES + USA ya completos) | 4 países |
| **Baja** | Perú facturación · refactor SQLite → Oracle ZMP (mismo patrón que USA OF #832) | 1 país (PE) |

---

## 🏦 Cobertura por aggregator

| Provider | Cubre | NO cubre | Estado integración |
|---|---|---|---|
| **Belvo** | MX · CO · CL · PE · BR · EC*  | BO · PY · UY · AR plenamente | ✅ core en monorepo |
| **Prometeo** | AR · UY · PY · EC · CL* · PE* | BO · MX · BR | ✅ core en monorepo |
| **Pluggy** | BR (alternativa) | resto LatAm | 🟡 scaffold (#667-#669) · no activado |
| **Akoya** | EEUU | resto | 🟡 core deployed sandbox 2026-05-18 · runtime smoke pendiente DBA |
| **Kushkipagos** | EC (alternativa) | resto | ❌ futuro |

`*` cobertura parcial · prefer Prometeo o Belvo según país

**Bolivia y Paraguay** son los únicos países sin path bancario viable (regulación local).

---

## 📦 Componentes compartidos

| Componente | Estado |
|---|---|
| `zymplo-api` proxy multi-país (cross-país gateway) | ✅ MX · BR · BO · CL · EC · AR · CO · UY · PE · USA · ES modules wired (post #845/#847/#860/#861/#862 OF + #844/#843 FE) |
| `zymplo-mobile` (Expo) | ✅ MX/BO/CL/BR/EC/AR/CO/UY/PE/USA/ES hooks · OF screens dedicadas BO/CL/CO/EC (#863) + AR/USA/ES previas |
| `zymplo-langgraph` (WhatsApp bot) | ✅ MX/BO/CL/BR/EC/AR/CO/UY/PE/USA/ES dispatch · `openfinance_client.py` + `openfinance_tools.py` cubren 9 países OF + `_COUNTRY_INFO` registry |
| Service core `zymplo-openfinance-belvo` | ✅ deployed `nfe-s:3010` |
| Service core `zymplo-openfinance-prometeo` | ✅ deployed `nfe-s:3011` |
| Tablas Oracle ZMP genéricas (audit · OF · idempotency) | ✅ aplicadas |

---

## 📝 Cómo se actualiza

- Cambio de estado por país: editar fila en "Snapshot ejecutivo" + bloque de detalle del país
- Sumar país nuevo: agregar bloque + entrada en snapshot + cobertura aggregator
- Cambio de provider/cobertura: actualizar tabla "Cobertura por aggregator"
- **Click-through samples E2E**: aplicar [`PATTERN-PUBLIC-VIEW.md`](PATTERN-PUBLIC-VIEW.md) cuando un país llegue a E2E firmado · habilita el `Sample E2E` clickeable desde el bloque del país

Para detalle técnico de PRs específicos (cuáles features cerraron, cuándo, qué tablas se crearon), ver `git log MULTIPAIS-DASHBOARD.md` o el commit history del PR correspondiente.
