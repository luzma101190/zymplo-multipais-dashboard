# 🌎 Zymplo · Dashboard Multi-país

> **Vista ejecutiva** del estado de facturación electrónica + Open Finance bancaria por país. Para detalle técnico (PRs, migrations, paridad estructural) ver `git log MULTIPAIS-DASHBOARD.md` · historial commits archivado.
>
> **Última actualización:** 2026-05-13 · **CO + EC sandbox · CL + PE estructural · UY E2E con Oracle real (APIs + sqlcl verify)** · 5 países con sample fiscal · CO [PDF](samples/colombia/factura-1-co-SETP5.pdf) · EC [PDF](samples/ecuador/factura-1-ec-001-001-000000001.pdf) · CL [PDF](samples/chile/dte-33-cl-folio-1.pdf) · PE [PDF](samples/peru/factura-01-pe-F001-00000001.pdf) · UY [PDF Oracle real](samples/uruguay/cfe-21-uy-real-oracle.pdf)

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
| 🇨🇴 **Colombia** | ✅ **E2E sandbox** | 🟡 Belvo CO | ✅ App · ✅ WA (#752 bot + #753 app + #748 público-view) | [facturacion-co-qa](https://facturacion-co-qa.zymplo.com/api-docs/) · [openfinance-co-qa](https://openfinance-co-qa.zymplo.com/api-docs/) · [PDF](samples/colombia/factura-1-co-SETP5.pdf) |
| 🇦🇷 **Argentina** | 🧪 AFIP sandbox | ✅ Prometeo sandbox **E2E real 2026-05-08** | ❌ pendiente | [openfinance-ar-qa](https://openfinance-ar-qa.zymplo.com/api-docs/) |
| 🇨🇱 **Chile** | 🟡 **E2E estructural** (cert dummy) | ✅ Belvo sandbox | ✅ App · ✅ WA | [facturacion-cl-qa](https://facturacion-cl-qa.zymplo.com/api-docs/) · [openfinance-cl-qa](https://openfinance-cl-qa.zymplo.com/api-docs/) · [PDF](samples/chile/dte-33-cl-folio-1.pdf) |
| 🇪🇨 **Ecuador** | ✅ **E2E sandbox** | ✅ Prometeo sandbox **E2E real 2026-05-08** | ✅ App · ✅ WA (#755 bot · scaffold #678/#679/#749 público-view) | [facturacion-ec-qa](https://facturacion-ec-qa.zymplo.com/api-docs/) · [openfinance-ec-qa](https://openfinance-ec-qa.zymplo.com/api-docs/) · [PDF](samples/ecuador/factura-1-ec-001-001-000000001.pdf) |
| 🇵🇪 **Perú** | 🟡 **E2E estructural** (cert dummy) | ✅ Prometeo PE **E2E real 2026-05-08** | ✅ App · ✅ WA (#764 bot + #767 v2 proxy + #768 mobile + #763 PDF+público-view) | [openfinance-pe-qa](https://openfinance-pe-qa.zymplo.com/api-docs/) · [PDF](samples/peru/factura-01-pe-F001-00000001.pdf) |
| 🇺🇾 **Uruguay** | ✅ **E2E mock + Oracle real 2026-05-13** | ✅ Prometeo UY **E2E real 2026-05-08** | ✅ App · ✅ WA (#759 bot + #760 app + #758 PDF+público-view) | [facturacion-uy-qa](https://facturacion-uy-qa.zymplo.com) · [openfinance-uy-qa](https://openfinance-uy-qa.zymplo.com/api-docs/) · [PDF](samples/uruguay/cfe-21-uy-real-oracle.pdf) |
| 🇨🇷 **Costa Rica** | 🚧 código clone de Perú · pendiente owner | ✅ OF QA healthy | ❌ pendiente | [openbanking-cr-qa](https://openbanking-cr-qa.zymplo.com) |
| 🇺🇸 **EEUU** | 🟡 UBL · FE-US QA healthy | ⏳ Akoya | ❌ pendiente | [facturacion-us-qa](https://facturacion-us-qa.zymplo.com) |
| 🇪🇸 **España** | 🧪 sin cert | 🧪 sandbox | ❌ pendiente | externo |

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
- **Open Finance**: ❌ **NO disponible** · ASFI no abrió Open Banking · ningún aggregator LatAm cubre Bolivia (Belvo · Prometeo · Pluggy · Salt Edge · todos verificados sin cobertura). La empresa BO usa solo facturación.
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
- **Open Finance**: 🟡 Belvo CO en monorepo · falta deploy QA público · [openfinance-co-qa](https://openfinance-co-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert real Camerfirma o Andes SCD del cliente · (2) deploy OF QA + smoke e2e · (3) primer emit real (estructural ya validado) · (4) screens F2+ (NC/ND, drafts, detail)

---

## 🇦🇷 Argentina

- **Facturación**: 🧪 AFIP sandbox público · **probado sin costo** · falta migrar al monorepo y conseguir cert real para prod
- **API docs**: facturación todavía no en monorepo · OF en [openfinance-ar-qa](https://openfinance-ar-qa.zymplo.com/api-docs/)
- **Open Finance**: ✅ Prometeo sandbox cubre AR · thin desplegado en QA
- **Para PROD**: (1) cert AFIP del cliente · (2) migrar zymplo-afip al monorepo · (3) plan Prometeo prod

---

## 🇨🇱 Chile

- **Facturación**: 🟡 **E2E ESTRUCTURAL completo 2026-05-12** (cert+CAF DUMMY · NO firmó+envió SII real) · DTE 33 Factura emitida con TED + XMLDSig + persistencia Oracle + PDF Res. Ex. 80/2014 con PDF417 timbre. Backend QA-ready (DTE F0-F4 · 11 tablas CL_DTE_*)
- **E2E validado (estructural · 2026-05-12)**: DTE id=81 · folio 1 · neto 100k + IVA 19% · total $119.000 CLP · Estado FIRMADO (no enviado a SII porque cert auto-signed)
  - PR #799 fix(cl): expose devMessage cuando SII_AMBIENTE != produccion
- **⚠️ Caveat crítico**: cert PFX auto-firmado + CAF generado localmente · SII rechazaría la firma del CAF y la cadena de trust del cert. PDF y XML estructuralmente SII-compliant pero NO válidos fiscalmente
- **Sample E2E**: [dte-33-cl-folio-1.pdf](samples/chile/dte-33-cl-folio-1.pdf) · [dte-33-cl-folio-1.xml](samples/chile/dte-33-cl-folio-1.xml)
- **API docs**: https://facturacion-cl-qa.zymplo.com/api-docs/
- **Open Finance**: ✅ Belvo sandbox cubre CL (BancoEstado · BCI · etc) · 5 links E2E validados en VM · [openfinance-cl-qa](https://openfinance-cl-qa.zymplo.com/api-docs/)
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
- **Open Finance**: ✅ Prometeo sandbox cubre EC (Pichincha · Intermatico · 5 providers) · [openfinance-ec-qa](https://openfinance-ec-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert BCE del cliente final (~30 USD · estructural ya validado) · (2) plan Prometeo prod

---

## 🇵🇪 Perú

- **Facturación**: 🟡 **E2E ESTRUCTURAL completo 2026-05-13** (cert+CAF DUMMY · NO firmó+envió SUNAT real) · Factura 01 F001-00000001 emitida con UBL 2.1 + XMLDSig + persistencia SQLite + PDF SUNAT 097-2012 con QR canónico. SUNAT beta recibió XML pero rechazó firma del cert auto-signed (esperado)
- **E2E validado (estructural · 2026-05-13)**: docId=1 · neto S/. 100.00 + IGV 18% · total S/. 118.00 · hash SHA-1 calculado · monto en letras "CIENTO DIECIOCHO Y 00/100 SOLES"
  - PR #809 feat(pe/sunat) PDF formato oficial SUNAT 097-2012 · empresa data + tabla detalle + breakdown subtotal/IGV + monto en letras + cleanup columnas
- **⚠️ Caveat crítico**: cert PFX auto-firmado · SUNAT al validar firma rechaza ("Unsupported Signature signer format"). PDF y XML estructuralmente SUNAT-compliant pero NO válidos fiscalmente
- **Sample E2E**: [factura-01-pe-F001-00000001.pdf](samples/peru/factura-01-pe-F001-00000001.pdf) · [factura-01-pe-F001-00000001.xml](samples/peru/factura-01-pe-F001-00000001.xml)
- **PDF formato oficial**: ✅ PdfFacturaService #763 + #809 · pdfkit + qrcode · QR SUNAT RTF 097-2012 · cubre 4 tipos doc (01 Factura · 03 Boleta · 07 NC · 08 ND) · IGV 18% · watermark "PENDIENTE SUNAT" cuando sin sunat_codigo_respuesta='0' · tabla detalle items · monto en letras
- **Público-view pattern**: ✅ aplicado #763 · `SUNAT_PUBLIC_VIEW_ENABLED=true` activa clickthrough HTML+PDF+XML+CDR desde dashboard
- **API docs**: peru/zymplo-sunat en monorepo · OF en [openfinance-pe-qa](https://openfinance-pe-qa.zymplo.com/api-docs/)
- **App + WhatsApp F0 completo (2026-05-12)**:
  - PaisCodi=33 canónico · en FISCAL_COUNTRIES (#764)
  - Bot WhatsApp #764 · `sunat_tools.py` + `sunat_client.py` + registry
  - v2 proxy hexagonal #767 · `zymplo-api/src/modules/sunat-pe/` (12 archivos TS · paralelo a cfe-uy/sri-ec/dian-co · endpoints `/api/v2/sunat-pe/facturas[/:id[/xml]]`)
  - Mobile #768 · hook `useSunatPe.ts` + screens `sunat-pe-emit.tsx` + `sunat-pe-history.tsx`
  - NC/ND (07/08), drafts, detail/setup, anulaciones, ICBPER quedan F2+
- **Open Finance**: 🟡 Belvo PE en monorepo · falta deploy QA + smoke e2e
- **Para PROD real**: (1) cert SUNAT del cliente final (estructural ya validado) · (2) v2 proxy + mobile hook + screens (F2) · (3) deploy QA público · (4) plan Belvo prod

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
- **Open Finance**: ✅ Prometeo sandbox cubre UY · thin en monorepo (#593)
- **Para PROD**: (1) cert DGI del cliente real · (2) crear user `ZMP_UY_QA` + grants (DevOps lo está coordinando, ver PR #789 Layer A) · (3) container QA correr en thick mode con LD_LIBRARY_PATH+libnnz para que el pool conecte real (ya validado en Oracle migrations VM) · (4) plan Prometeo prod

---

## 🇨🇷 Costa Rica

- **Facturación**: 🚧 **BLOQUEADO por código** · `costarica/zymplo-hacienda/` es un clone literal de `facturacion-peru` (package.json `"name": "facturacion-peru"`, src con endpoints SUNAT, .env.example con `SUNAT_BETA_BILLSERVICE`) · DevOps dejó `facturacion-cr-qa` retornando 503 maintenance hasta que owner haga rename + reescritura adapter MH-CR. **Pendiente**: (1) rename package + .env stub MH-CR, (2) reemplazar integraciones SUNAT por API Hacienda CR (XML + firma según MH).
- **API docs**: facturación bloqueada (503) · Open Banking en [openbanking-cr-qa](https://openbanking-cr-qa.zymplo.com)
- **Open Finance**: ✅ container `openbanking-cr-qa` healthy (DevOps fixó HEALTHCHECK 2026-05-08 · puerto 3003 `/api/health`) · pero el container heredó respuestas con `"service": "openbanking-peru"` · **deuda dev** para owner CR
- **Para PROD**: (1) **owner asignado para destrabe código** (rename + adapter MH-CR · ~1-2 sprints), (2) cert real Hacienda del cliente, (3) limpieza referencias `openbanking-peru` en respuestas OF

---

## 🇺🇸 EEUU

- **Facturación**: 🟡 UBL Fastify · **container `facturacion-us-qa` healthy** (DevOps · postgres compartido `postgres-multipais-qa` · healthcheck `/healthz` · owner Andrea) · no envía a regulador (US no tiene factura electrónica obligatoria) · pendiente migración fe-us al monorepo + Oracle ZMP si aplica
- **API docs**: [facturacion-us-qa](https://facturacion-us-qa.zymplo.com)
- **Open Finance**: ⏳ Akoya en proceso (sandbox disponible · cubre EEUU)
- **Para PROD**: caso de uso distinto · facturación es opcional · OF requiere acuerdo Akoya

---

## 🇪🇸 España

- **Facturación**: 🧪 código AEAT funcionando en repo origen · cert pendiente vía Jesús (gestión interna)
- **Open Finance**: 🧪 sandbox disponible
- **Para PROD**: (1) cert AEAT del cliente · (2) migrar al monorepo · (3) deploy QA

---

## 🎯 Próximos pasos prioritarios (cross-país)

| Prioridad | Acción | Países afectados |
|---|---|---|
| **Alta** | **Asignar owner CR-Hacienda** · rename `facturacion-peru` → `facturacion-costarica` + reescribir adapter SUNAT → MH-CR (XML + firma según MH) · destraba `facturacion-cr-qa` (hoy 503 maintenance) | 1 país (CR) |
| **Alta** | Replicar modelo BR (cliente colaborador presta cert) en CL · EC · PE · UY · CR · ES | 6 países desbloqueables |
| **Alta** | Negociar plan Belvo productivo (cubre MX · BR · CL · CO · PE en un solo contrato) | 5 países |
| **Media** | Negociar plan Prometeo productivo (cubre AR · UY · PY · EC) | 4 países |
| **Media** | DevOps · activar feature flags `OPENFINANCE_*_ENABLED` en zymplo-api proxy | EC · AR · UY · PE · CO |
| **Baja** | Migrar facturación AR/PY/UY/PE/EEUU/ES al monorepo (orden Wave 5) | 6 países |

---

## 🏦 Cobertura por aggregator

| Provider | Cubre | NO cubre | Estado integración |
|---|---|---|---|
| **Belvo** | MX · CO · CL · PE · BR · EC*  | BO · PY · UY · AR plenamente | ✅ core en monorepo |
| **Prometeo** | AR · UY · PY · EC · CL* · PE* | BO · MX · BR | ✅ core en monorepo |
| **Pluggy** | BR (alternativa) | resto LatAm | 🟡 scaffold (#667-#669) · no activado |
| **Akoya** | EEUU | resto | ❌ futuro |
| **Kushkipagos** | EC (alternativa) | resto | ❌ futuro |

`*` cobertura parcial · prefer Prometeo o Belvo según país

**Bolivia y Paraguay** son los únicos países sin path bancario viable (regulación local).

---

## 📦 Componentes compartidos

| Componente | Estado |
|---|---|
| `zymplo-api` proxy multi-país (cross-país gateway) | ✅ MX · BR · BO · CL · EC modules wired |
| `zymplo-mobile` (Expo) | ✅ MX/BO/CL/BR feature parity · EC hook scaffold (#678) |
| `zymplo-langgraph` (WhatsApp bot) | ✅ MX/BO/CL/BR + EC dispatch (#679) |
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
