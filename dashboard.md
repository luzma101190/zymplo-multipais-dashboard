# 🌎 Zymplo · Dashboard Multi-país

> **Vista ejecutiva** del estado de facturación electrónica + Open Finance bancaria por país. Para detalle técnico (PRs, migrations, paridad estructural) ver `git log MULTIPAIS-DASHBOARD.md` · historial commits archivado.
>
> **Última actualización:** 2026-05-08 · 13 países · 4 con E2E facturación confirmado · 4 con OF bancaria sandbox-validado.

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

| País | Facturación | Open Finance | API docs (QA) |
|---|---|---|---|
| 🇧🇷 **Brasil** | ✅ E2E confirmado | ✅ Belvo OFDA | [facturacion-br-qa](https://facturacion-br-qa.zymplo.com/api-docs/) · [openfinance-br-qa](https://openfinance-br-qa.zymplo.com/api-docs/) |
| 🇲🇽 **México** | ✅ E2E confirmado | ✅ Belvo sandbox | [facturacion-mx-qa](https://facturacion-mx-qa.zymplo.com/api-docs/) · [openfinance-mx-qa](https://openfinance-mx-qa.zymplo.com/api-docs/) |
| 🇧🇴 **Bolivia** | ✅ E2E confirmado | ❌ no aplica (ASFI) | [facturacion-bo-qa](https://facturacion-bo-qa.zymplo.com/api-docs/) |
| 🇵🇾 **Paraguay** | ✅ E2E (DNIT externo) | ⏸️ pausado | externo (sin monorepo) |
| 🇨🇴 **Colombia** | 🟡 sandbox DIAN | 🟡 Belvo CO | [facturacion-co-qa](https://facturacion-co-qa.zymplo.com/api-docs/) · [openfinance-co-qa](https://openfinance-co-qa.zymplo.com/api-docs/) |
| 🇦🇷 **Argentina** | 🧪 AFIP sandbox | ✅ Prometeo sandbox | [openfinance-ar-qa](https://openfinance-ar-qa.zymplo.com/api-docs/) |
| 🇨🇱 **Chile** | 🟡 sin cert prueba | ✅ Belvo sandbox | [facturacion-cl-qa](https://facturacion-cl-qa.zymplo.com/api-docs/) · [openfinance-cl-qa](https://openfinance-cl-qa.zymplo.com/api-docs/) |
| 🇪🇨 **Ecuador** | 🟡 sin cert prueba | ✅ Prometeo sandbox | [facturacion-ec-qa](https://facturacion-ec-qa.zymplo.com/api-docs/) · [openfinance-ec-qa](https://openfinance-ec-qa.zymplo.com/api-docs/) |
| 🇵🇪 **Perú** | 🟡 sin cert prueba | 🟡 Belvo PE | [openfinance-pe-qa](https://openfinance-pe-qa.zymplo.com/api-docs/) |
| 🇺🇾 **Uruguay** | 🧪 sin cert prueba | 🟡 Prometeo UY | [openfinance-uy-qa](https://openfinance-uy-qa.zymplo.com/api-docs/) |
| 🇨🇷 **Costa Rica** | ⏳ en proceso | 🟡 sandbox | [openbanking-cr-qa](https://openbanking-cr-qa.zymplo.com) |
| 🇺🇸 **EEUU** | 🧪 UBL externo | ⏳ Akoya | externo |
| 🇪🇸 **España** | 🧪 sin cert | 🧪 sandbox | externo |

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

- **Facturación**: ✅ E2E sandbox · **Facturama PAC provee certificado de prueba** + cert real del cliente (CSD gratis emitido por SAT) · firmado+timbrado validado en Facturama sandbox
- **API docs**: https://facturacion-mx-qa.zymplo.com/api-docs/
- **Sample E2E**: pendiente (aplicar [PATTERN-PUBLIC-VIEW.md](PATTERN-PUBLIC-VIEW.md) sobre `zymplo-cfdi`)
- **Open Finance**: ✅ Belvo sandbox · cubre MX (BBVA · Banamex · Santander · HSBC · Banorte) · [openfinance-mx-qa](https://openfinance-mx-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert SAT del cliente final (CSD productivo · gratis · trámite cliente) · (2) plan Facturama productivo (cobra por timbre) · (3) plan Belvo prod negociado

---

## 🇧🇴 Bolivia

- **Facturación**: ✅ E2E confirmado · **certificado ADSIB de prueba conseguido** · CUF persistido en Oracle ZMP · audit log verificado 2026-05-07
- **API docs**: https://facturacion-bo-qa.zymplo.com/api-docs/
- **Sample E2E**: pendiente (aplicar [PATTERN-PUBLIC-VIEW.md](PATTERN-PUBLIC-VIEW.md) sobre `zymplo-siat`)
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

- **Facturación**: 🟡 DIAN ofrece sandbox público (no requiere cert real) · **probado en sandbox sin costo** · zymplo-dian alineado al monorepo (#665 · UBL 2.1 + CUFE SHA-384 · 11 tablas CO_DIAN_*)
- **API docs**: https://facturacion-co-qa.zymplo.com/api-docs/
- **Open Finance**: 🟡 Belvo CO en monorepo · falta deploy QA público · [openfinance-co-qa](https://openfinance-co-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert real Camerfirma o Andes SCD del cliente · (2) deploy OF QA + smoke e2e

---

## 🇦🇷 Argentina

- **Facturación**: 🧪 AFIP sandbox público · **probado sin costo** · falta migrar al monorepo y conseguir cert real para prod
- **API docs**: facturación todavía no en monorepo · OF en [openfinance-ar-qa](https://openfinance-ar-qa.zymplo.com/api-docs/)
- **Open Finance**: ✅ Prometeo sandbox cubre AR · thin desplegado en QA
- **Para PROD**: (1) cert AFIP del cliente · (2) migrar zymplo-afip al monorepo · (3) plan Prometeo prod

---

## 🇨🇱 Chile

- **Facturación**: 🟡 NO pudimos probar end-to-end · **no tenemos cert prueba y para tramitarlo se requiere RUT chileno** (replicar modelo Brasil: cliente colaborador con RUT que nos preste cert). Backend QA-ready (DTE F0-F4 · audit endpoint · catálogos vendoreados · ~25 USD E-CertChile)
- **API docs**: https://facturacion-cl-qa.zymplo.com/api-docs/
- **Open Finance**: ✅ Belvo sandbox cubre CL (BancoEstado · BCI · etc) · 5 links E2E validados en VM · [openfinance-cl-qa](https://openfinance-cl-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) **cliente chileno colaborador** que nos preste cert SII (modelo BR) · O · (2) gestionar RUT corporativo Zymplo Chile · (3) cert E-CertChile (~25 USD) · (4) plan Belvo prod (mismo del MX/BR)

---

## 🇪🇨 Ecuador

- **Facturación**: 🟡 NO pudimos probar end-to-end · **no tenemos cert prueba** (mismo bloqueante que CL/PE/UY/CR · cert real BCE ~30 USD del cliente). Backend QA-ready (zymplo-sri F0-F4 · 33 tests verdes · 11 tablas EC_SRI_*)
- **API docs**: https://facturacion-ec-qa.zymplo.com/api-docs/
- **Open Finance**: ✅ Prometeo sandbox cubre EC (Pichincha · Intermatico · 5 providers) · [openfinance-ec-qa](https://openfinance-ec-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert BCE del cliente final (~30 USD) · (2) deploy facturación QA público · (3) plan Prometeo prod

---

## 🇵🇪 Perú

- **Facturación**: 🟡 NO pudimos probar · sin cert prueba (igual que CL/EC) · backend código en repo origen · falta migración monorepo
- **API docs**: facturación todavía no en monorepo · OF en [openfinance-pe-qa](https://openfinance-pe-qa.zymplo.com/api-docs/)
- **Open Finance**: 🟡 Belvo PE en monorepo · falta deploy QA + smoke e2e
- **Para PROD**: (1) cert SUNAT del cliente · (2) migrar zymplo-sunat al monorepo · (3) plan Belvo prod

---

## 🇺🇾 Uruguay

- **Facturación**: 🧪 código DGI funcionando en repo origen · sin cert prueba · sin migrar al monorepo
- **API docs**: facturación externa · OF en [openfinance-uy-qa](https://openfinance-uy-qa.zymplo.com/api-docs/)
- **Open Finance**: ✅ Prometeo sandbox cubre UY · thin en monorepo (#593) · [openfinance-uy-qa](https://openfinance-uy-qa.zymplo.com/api-docs/)
- **Para PROD**: (1) cert DGI del cliente · (2) migrar zymplo-dgi al monorepo · (3) plan Prometeo prod

---

## 🇨🇷 Costa Rica

- **Facturación**: ⏳ en proceso (Hacienda · zymplo-hacienda iniciado en monorepo) · sin cert prueba todavía
- **API docs**: facturación TBD · Open Banking en [openbanking-cr-qa](https://openbanking-cr-qa.zymplo.com)
- **Open Finance**: 🟡 sandbox disponible · servicio thin operativo en QA
- **Para PROD**: (1) cert real Hacienda del cliente · (2) terminar implementación zymplo-hacienda

---

## 🇺🇸 EEUU

- **Facturación**: 🧪 UBL standard funciona en repo origen · no envía a regulador (US no tiene factura electrónica obligatoria) · sin migrar al monorepo
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
