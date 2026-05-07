# 🌎 Zymplo · Dashboard Multi-país

> **Propósito:** vista comparativa de los 13 países del ecosistema Zymplo · estado de facturación electrónica + integración bancaria · marcando avance de alineación al monorepo `zymplo/` y Oracle ZMP.
>
> **Última actualización:** 2026-05-07 madrugada · **🎯 PARIDAD ESTRUCTURAL 100% lograda · 4 países alineados (MX · BO · CL · BR)** · cadenas A/B/C cerradas (preview cross-country + Brasil catch-up + catálogos vendoreados) + Brasil normalize completo (B1-B3 + B-i/B-ii/B-iii/B-iv/B-v) + CL audit endpoint #631 + BR structural validator #632 + **🇧🇷 BR Open Finance stack cerrado** (zymplo-api openfinance-br #635 · core webhook HISTORICAL_UPDATE #639 · langgraph BR+CL OF tools #636 · mobile widget Belvo OFDA #638) + **🇧🇷 BR NFS-e Phase 1 mobile cerrado** (drafts + setup mobile #641 · A1 ZMP.BR_NFSE_* migrations aplicadas en dbautdesa02 2026-05-07) · **E2E facturación BO SIAT confirmado** (API → Service → Oracle ZMP trinity verificada 2026-05-07 03:30 UTC · CUF persistido en `zmp.bo_siat_factura` + audit en `zmp.zmp_audit_log`) · ver `git log MULTIPAIS-DASHBOARD.md` para historial.

> **`paisCodi` canonical** · ley = `SKN.COME_PAIS` (Oracle ATP · 30 filas con CHILE recién agregado). Mobile usa `constants/paisCodi.ts` · langgraph usa `src/utils/pais_codi.py`. NO hardcodear magic numbers · usar `PAIS_CODI.MEXICO` (= 25), `PAIS_CODI.BOLIVIA` (= 28), `PAIS_CODI.CHILE` (= 30).

## 📐 Convención

- ✅ **prod-ready** (alineado al monorepo + Oracle ZMP + tests QA verdes + cert/creds productivos + deploy prod)
- 🟢 **QA-ready** (alineado al monorepo + Oracle ZMP + tests QA verdes + smoke e2e en QA · **falta solo el último mile**: cert/creds productivos + deploy prod · usualmente bloqueado por acuerdo comercial con el provider o cert real del cliente)
- 🟡 partial (code escrito o test pasa, pero falta alguno de: monorepo / Oracle ZMP / certificación / deploy QA)
- 🧪 listo en test externo (funciona en repo del dueño, **falta migrar al monorepo zymplo + Oracle ZMP**)
- ⏳ en proceso
- ⏸️ pausado
- ❌ pending / no iniciado
- `?` requires audit

> **Nota importante:** "Listo" en el sheet legacy del equipo significa **🧪 listo en test externo** — el código funciona en su repo origen, pero todavía no está en el monorepo zymplo ni persiste en Oracle ZMP. La migración al monorepo es un trabajo separado por país.

---

## 📈 Vista global ejecutiva

| País | Dueño | Facturación E. (estado) | OF Bancaria (provider · estado) | Alineación monorepo |
|---|---|---|---|---|
| 🇧🇷 **Brasil**     | Martín Rolón / Luz E. | 🟢 NFS-e · `brasil/zymplo-nfse/` · paridad estructural full (B1 middleware #616 · B2 rebrand #617 · B3 DDL ZMP.BR_NFSE_* #619 · B-i provider split #624 · B-ii crons #625 · B-iii drafts + B-iv audit endpoint #626 · B-v structural validator #632) · cutover SKN→ZMP B4+ (DBA-coordinated) · falta cert ICP-Brasil prod | 🟢 thin Belvo OFDA stack completo · `brasil/zymplo-openfinance-br` (#618 + #620 · widget-token + persistence ZMP_OF_LINK BRA E2E validado 2026-05-06) + zymplo-api openfinance-br module (#635) + langgraph OF tools BR (#636) + mobile widget Belvo OFDA (#638) + core webhook HISTORICAL_UPDATE (#639) · falta DevOps deploy público + Belvo dashboard callback config | ✅ brasil/zymplo-openfinance-br + ✅ brasil/zymplo-nfse (paridad full · cutover ZMP cuando DBA aplique) |
| 🇵🇾 **Paraguay**   | Liz Villasanti   | 🧪 DNIT listo (Zymplo Fact. E.)      | ⏸️ En pausa (80% docu) · pending CO   | ❌ no en monorepo |
| 🇲🇽 **México**     | Luz Espínola     | 🟢 CFDI F0 + drafts + NC/ND mobile (#591) · F7.4b validators (#585 RFC homoclave · #587 structural · #594 wireup) · **#621 preview endpoint** · **#627 SAT catálogos vendoreados** (c_RegimenFiscal · c_UsoCFDI · c_FormaPago) · QA-ready · falta cert SAT prod + deploy prod | 🟢 Belvo · thin + core + sync + smoke e2e · falta acuerdo comercial Belvo prod + deploy prod | ✅ mexico/zymplo-cfdi + zymplo-openfinance (thin) |
| 🇺🇸 **EEUU**       | Andrea Amarilla  | 🧪 UBL listo (no envía a regulador)  | ⏳ Akoya en proceso                   | ❌ no en monorepo |
| 🇨🇴 **Colombia**   | Liz Villasanti   | 🧪 DIAN listo (falta cert real)      | 🟡 thin Belvo CO en monorepo (#589) · falta deploy QA + smoke e2e | ⏳ colombia/zymplo-openfinance-co (post #589) · DIAN aún externo |
| 🇪🇸 **España**     | Francisco Villalba | 🧪 listo (falta cert real)         | 🧪 listo                              | ❌ no en monorepo |
| 🇦🇷 **Argentina**  | Gadiel Muñoz     | 🧪 listo (falta cert real)           | 🟡 thin Prometeo AR en monorepo (#592) · falta deploy QA + smoke e2e | ⏳ argentina/zymplo-openfinance-ar (post #592) · AFIP aún externo |
| 🇵🇪 **Perú**       | Alberto Mendez   | 🧪 listo (falta cert real)           | 🟡 thin Belvo PE en monorepo (#590) · falta deploy QA + smoke e2e | ⏳ peru/zymplo-openfinance-pe (post #590) · SUNAT aún externo |
| 🇪🇨 **Ecuador**    | Orlando          | 🧪 listo (falta cert real)           | 🧪 kushkipagos listo                  | ❌ no en monorepo |
| 🇨🇱 **Chile**      | Martín Rolón / Luz E. | 🟢 DTE F0+F4 (#559/#562/#564/#566) + mobile chain (#580/#582/#584/#586) + langgraph dte_tools (#578) + **F7.1-F7.3b cross-country (#572/#576/#577/#583)** · F8.1 zymplo-api proxy (#581) · **#623 preview endpoint** · **#629 SII catálogos vendoreados** (Tipos DTE · Códigos Referencia) · **#631 audit endpoint** · paridad estructural full · QA-ready · falta cert SII prod (~$25 USD E-CertChile) | 🟢 Belvo · thin + paridad MX/BO (#573) + recon DTE↔Belvo (#569) + mobile screen (#586) · E2E VM validado · falta deploy QA público (Victor handoff #570) | ✅ chile/zymplo-dte + chile/zymplo-openfinance · Oracle ZMP (cl_dte_*, ZMP_OF_*, TRAN_DTE_ID) |
| 🇺🇾 **Uruguay**    | Orlando Dure     | 🧪 listo (falta cert real)           | 🟡 thin Prometeo UY en monorepo (#593) · falta deploy QA + smoke e2e | ⏳ uruguay/zymplo-openfinance-uy (post #593) · DGI aún externo |
| 🇧🇴 **Bolivia**    | Luz Espínola     | 🟢 SIAT F0 + F7.4b structural (#588) + #595 wireup · **#622 preview endpoint** · **#628 SIN catálogos vendoreados** (c_MetodoPago · c_TipoEmision) · **🎯 E2E confirmado 2026-05-07** (API → Service → Oracle: CUF persistido en `zmp.bo_siat_factura` row 21 + audit log entry) · QA-ready · falta cert ADSIB prod + deploy prod | 🟢 Prometeo · thin + core + auto-relogin + sync + smoke e2e · falta acuerdo comercial Prometeo prod + deploy prod | ✅ bolivia/zymplo-siat + zymplo-openfinance-bo (thin) |
| 🇨🇷 **Costa Rica** | Alberto Mendez   | ⏳ en proceso                        | ❌ no iniciado                        | ❌ no en monorepo |

| 🌐 **Componentes Compartidos** | Estado |
|---|---|
| `zymplo-api` proxy (cross-país gateway) | ✅ MX (cfdi + openfinance) + BR (nfse + **openfinance-br #635**) + BO (openfinance-bo + factura-bo) + **CL (DTE post #581)** · cierra paridad 4 países |
| `zymplo-mobile` (Expo · React Native) | ✅ MX/BO/CL/BR paridad OF (setup · emit · history · detail · drafts · NC/ND · OF) + BR widget Belvo OFDA mobile cerrado (#638 · `useOpenFinanceBr` + `OpenFinanceBrView` con `expo-web-browser` + `WebBrowser.openAuthSessionAsync`) · paridad mobile 4 países cerrada |
| `zymplo-langgraph` (WhatsApp bot) | ✅ MX (CFDI + OF Belvo) + BR (NFS-e + **OF Belvo OFDA #636**) + BO (SIAT 6 tools + OF Prometeo) + **CL (dte_tools 6 tools post #578 + OF Belvo #636)** · paridad 4 países OF cerrada (`_COUNTRY_TO_PROXY_PATH` 4 países · `_RECON_SUPPORTED_COUNTRIES` 3 países · BR sin recon hasta NFS-e ZMP) |
| Tablas Oracle ZMP genéricas (audit, notif, of) | ✅ creadas 2026-05-05 |
| Service core `zymplo-openfinance-belvo` | ✅ código + deploy infra · runtime DevOps pendiente (#58277) |
| Service core `zymplo-openfinance-prometeo` | ✅ código + deploy infra · runtime DevOps pendiente (#58277) |
| Provider abstraction · contract neutral entre cores | ✅ #493-#497 (2026-05-06) |
| Thins MX/CL/BO provider-agnostic vía `OF_CORE_BASE_URL` | ✅ #494-#495 (2026-05-06) |
| Service core `zymplo-openfinance-pluggy` | ❌ no aplica F1 · BR ahora usa core Belvo compartido (PR #618) · Pluggy queda como fallback futuro si Belvo no escala |
| Service core `zymplo-openfinance-akoya` | ❌ Fase 4+ (cuando entre EEUU) |
| Service core `zymplo-openfinance-kushkipagos` | ❌ Fase 4+ (cuando entre Ecuador) |
| Governance (arch-guard CI · CLAUDE.md por país · backlog) | ❌ Fase 4 |

## 🎯 Paridad mobile cross-país (post 2026-05-06)

| País | Setup | Emit | History | Detail | Drafts | Nota | OF |
|---|---|---|---|---|---|---|---|
| 🇲🇽 MX | ✅ | ✅ | ✅ | ✅ | ✅ #591 | ✅ #591 | ✅ |
| 🇧🇴 BO | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 🇨🇱 CL | ✅ | ✅ | ✅ | ✅ | ✅ #584 | ✅ #584 | ✅ #586 |
| 🇧🇷 BR | ✅ #641 | ✅ | ✅ | ✅ | ✅ #641 | ❌ Phase 2 | ✅ widget Belvo OFDA #638 (read-only views + WebBrowser.openAuthSessionAsync) |

3 países (MX/BO/CL) con feature parity completa: setup cert · emit factura · history paginado · drilldown · drafts (BORRADOR workflow) · NC/ND emission · OF banking. Brasil avanzó OF stack completo (8 PRs cerrados) + NFS-e drafts + setup mobile (#641) · falta solo Nota mobile (Phase 2 · backend Substituição/Cancelamento Receita Federal pendiente).

---

## 🎯 Paridad estructural cross-country · 4 países (post 2026-05-07)

Tabla de features estructurales cross-country alineadas en MX/BO/CL/BR. Cada ✓ corresponde a un PR específico mergeado.

| Capa | 🇲🇽 MX | 🇧🇴 BO | 🇨🇱 CL | 🇧🇷 BR |
|---|:-:|:-:|:-:|:-:|
| **Cross-country middleware** (correlationId · idempotencyKey · tenant · serviceAuth + ZMP_AUDIT_LOG + ZMP_IDEMPOTENCY) | ✓ | ✓ | ✓ #572 | ✓ #616 |
| **Provider abstraction** (Mock + Live facade) | ❌ (1 PAC) | ✓ | ✓ #576 | ✓ #624 |
| **Crons** (token refresh · estado poll · etc) | 1 (compliance) | 5 (cufd · contingencia · lotePoll · compliance · evento) | 2 (#577 token · estado) | 3 (#625 estado · retry · cert expiry) |
| **Drafts workflow** (BORRADOR + edit + emit-from-draft) | ✓ | ✓ | ✓ | ✓ #626 |
| **Audit log endpoint** (`GET /audit-log` · tenancy) | ✓ | ✓ | ✓ #631 | ✓ #626 |
| **Preview endpoint** (build XML + totales sin tocar provider · ahorra cuota) | ✓ #621 | ✓ #622 | ✓ #623 | ✓ (nativo) |
| **Structural validator** (Bag pattern · validations + length + coherence) | ✓ #587 + #594 wireup | ✓ #588 + #595 wireup | ✓ xsdValidator nativo | ✓ #632 |
| **Identifier validators** (RFC homoclave · NIT reservados · RUT módulo-11) | ✓ #585 #579 | ✓ #579 | ✓ rutValidator nativo | ✓ B-v |
| **Catálogos vendoreados** (códigos oficiales en JSON) | ✓ #627 (3 catálogos · 64 entries) | ✓ #628 (2 · 10 entries) | ✓ #629 (2 · 16 entries) | ✓ nativo (LC116 · NBS · MUNI · IBS/CBS · 5+ catálogos) |

(1) MX usa Facturama (1 PAC) · split formal NO necesario · arquitectura no requiere

**🎯 Paridad estructural 100% lograda** · cualquier desarrollo nuevo en cualquiera de los 4 países puede reusar templates idénticos.

---

## 🧪 E2E facturación · validación runtime

| País | Endpoint QA | E2E | Trinity API ↔ Service ↔ Oracle |
|---|---|---|---|
| 🇧🇴 BO SIAT | `https://facturacion-bolivia-qa.zymplo.com` | ✅ **2026-05-07 03:30 UTC** | CUF `0001023456789…92BDBFDF09A1A4EE7BFF206` · API response (HTTP 201) ↔ `ZMP.BO_SIAT_FACTURA.fac_id=21` ↔ `ZMP.ZMP_AUDIT_LOG` audit entry (`country=BOL · service=siat · action=FACTURA_EMIT`) · 3 facturas totales en mock mode ✓ |
| 🇲🇽 MX CFDI | `https://cfdi-mx-qa.zymplo.com` | ✅ histórico (Luz E. confirmado en sesiones previas) | `MX_CFDI_HISTORICO` persiste UUIDs de Facturama · audit en ZMP_AUDIT_LOG |
| 🇨🇱 CL DTE | TBD · falta deploy QA público | ✅ histórico (Martín R. confirmado en sesiones previas · VM testing) | `CL_DTE_HISTORICO` persiste · TED + folio CAF + envíos SII |
| 🇧🇷 BR NFS-e | TBD · falta deploy QA público (post B1-B5 + DBA cutover) | ✅ histórico (en repo origen · pre-monorepo) | Tablas legacy `SKN.FISC_NFSE_*` (cutover a `ZMP.BR_NFSE_*` post B3 #619 · DBA-coordinated) |

**Test pattern verificado** (BO SIAT · 2026-05-07): POST /factura/emit (mock mode) → API retorna `cuf` + `numeroFactura: 3` · `SELECT FROM ZMP.BO_SIAT_FACTURA` confirma row con mismo CUF + status AUTORIZADA + total 1130 · `SELECT FROM ZMP.ZMP_AUDIT_LOG` confirma audit entry con mismo entityId. **Trinity completa**.

---

## 🧾 Detalle · Facturación Electrónica

| País | Autoridad fiscal | Estado en repo origen | Repo/Stack origen actual | En monorepo zymplo | Tablas Oracle ZMP | Cert. real | Deploy QA | Deploy Prod |
|---|---|---|---|---|---|---|---|---|
| 🇧🇷 Brasil     | Receita Federal · NFS-e | 🟢 monorepo full · paridad B1-B5 + drafts mobile + setup mobile | `brasil/zymplo-nfse/` (en monorepo) | ✅ sí · 4 tablas `ZMP.BR_NFSE_*` aplicadas a `dbautdesa02_high` 2026-05-07 (cutover code B4+ pendiente DBA · service aún usa FISC_NFSE_*) | ✅ `BR_NFSE_EMPR/CERT/DOCU/EVEN` (vacías · post-migration) | ⏳ cert ICP-Brasil sandbox QA TBD | 🟡 backend QA-ready · DevOps subdomain pendiente | ❌ |
| 🇵🇾 Paraguay   | DNIT/SIFEN              | 🧪 listo en "Zymplo Facturación Electrónica" | externo al monorepo (otro producto) | ❌ | ❌ | 🧪 ya certif probable | ? | 🟡 productivo? |
| 🇲🇽 México     | SAT · CFDI 4.0          | 🟢 QA-ready  | `mexico/zymplo-cfdi/` (Facturama PAC) | ✅ sí · F0 cerrado | ✅ MX_CFDI_HISTORICO + MX_CSD_VAULT + MX_EMPR_FISCAL | ❌ falta cert prod | ✅ | ❌ |
| 🇺🇸 EEUU       | (UBL std · sin regulador) | 🧪 listo | externo al monorepo | ❌ | ❌ | N/A | ? | ❌ |
| 🇨🇴 Colombia   | DIAN                    | 🧪 listo | externo al monorepo | ❌ | ❌ | ❌ falta cert real | ? | ❌ |
| 🇪🇸 España     | AEAT                    | 🧪 listo | externo al monorepo | ❌ | ❌ | ❌ falta cert real | ? | ❌ |
| 🇦🇷 Argentina  | AFIP · FCE              | 🧪 listo | externo al monorepo | ❌ | ❌ | ❌ falta cert real | ? | ❌ |
| 🇵🇪 Perú       | SUNAT · CPE             | 🧪 listo | externo al monorepo | ❌ | ❌ | ❌ falta cert real | ? | ❌ |
| 🇪🇨 Ecuador    | SRI                     | 🧪 listo | externo al monorepo | ❌ | ❌ | ❌ falta cert real | ? | ❌ |
| 🇨🇱 Chile      | SII · DTE               | 🟢 QA-ready · F0+F4 chain cerrado (anular #559 · NC/ND #562 · drafts #564 · catálogos #566) | `chile/zymplo-dte/` (Oracle ZMP) | ✅ sí · cl_dte_* tables · paridad MX CFDI | ✅ Oracle ZMP · TRAN_DTE_ID | ❌ falta cert SII prod (~$25 USD E-CertChile · Martín) | ✅ smoke 41/41 OK | ❌ |
| 🇺🇾 Uruguay    | DGI                     | 🧪 listo | externo al monorepo | ❌ | ❌ | ❌ falta cert real | ? | ❌ |
| 🇧🇴 Bolivia    | SIN · SIAT              | 🟢 QA-ready · KUDE redesign · GET PDF fix | `bolivia/zymplo-siat/` | ✅ sí · F0 + features post-F2 | ✅ BO_SIAT_FACTURA + 4 más + ensanche cufd_codigo_control | ❌ falta cert ADSIB prod | ✅ | ❌ |
| 🇨🇷 Costa Rica | DGT                     | ⏳ en proceso | externo al monorepo | ❌ | ❌ | ❌ | ? | ❌ |

---

## 🏦 Detalle · Open Finance / Integración bancaria

| País | Provider | Cobertura del provider | Estado en repo origen | Service core en monorepo | Alineado a `ZMP_OF_*` | Falta para alineación |
|---|---|---|---|---|---|---|
| 🇧🇷 Brasil     | **Belvo OFDA**   | BR | 🟢 stack 8 PRs cerrado · thin (#618 + #620) + dev-mock + E2E (#630) + paridad Swagger (#633) + zymplo-api (#635) + langgraph (#636) + mobile widget (#638) + core webhook HISTORICAL_UPDATE (#639) | ✅ `zymplo-openfinance-belvo` (compartido · `_dev-mock-import` + HISTORICAL_UPDATE handler) · `brasil/zymplo-openfinance-br` (thin) · `zymplo-api/modules/openfinance-br/*` (gateway) · `zymplo-mobile/api/hooks/useOpenFinanceBr.ts` (mobile) · `zymplo-langgraph/src/utils/openfinance_client.py` (bot) | 🟢 E2E backend validado 2026-05-06 + paridad cross-pillar (api · mobile · bot · core webhook) cerrada 2026-05-07 | DevOps deploy público + Belvo dashboard callback config + reconciliation post-audit NFS-e (F3) + auto-upsert via webhook ZMP_OF_WIDGET_TOKEN_PENDING (F2) |
| 🇵🇾 Paraguay   | (pending CO)     | ?  | ⏸️ pausado · 80% docu         | depends on provider                     | ❌ | confirmar provider · arrancar |
| 🇲🇽 México     | **Belvo**        | LatAm + 6 países | 🟢 thin provider-agnostic + sync endpoint + smoke e2e (QA-ready) | ✅ `zymplo-openfinance-belvo` (deployed QA) | ✅ ZMP_OF_LINK/ACCOUNT/TX | acuerdo comercial Belvo prod + deploy prod |
| 🇺🇸 EEUU       | **Akoya**        | EEUU | ⏳ en proceso                | ❌ `zymplo-openfinance-akoya` (Fase 4+) | ❌ | finalizar test · service core futuro |
| 🇨🇴 Colombia   | **Belvo**        | sí (Belvo CO) | 🧪 listo (Docu+Swagger) | depends Fase 1                          | ❌ | thin CO post Fase 1 |
| 🇪🇸 España     | (no Belvo)       | ?  | 🧪 listo                      | ?                                       | ❌ | confirmar provider |
| 🇦🇷 Argentina  | (Belvo? local?)  | Belvo no cubre AR plenamente | 🧪 listo | ?                                       | ❌ | confirmar provider |
| 🇵🇪 Perú       | **Belvo**        | sí (Belvo PE) | 🧪 listo               | depends Fase 1                          | ❌ | thin PE post Fase 1 |
| 🇪🇨 Ecuador    | **kushkipagos**  | EC | 🧪 listo                       | ❌ `zymplo-openfinance-kushkipagos` (Fase 4+) | ❌ | service core futuro |
| 🇨🇱 Chile      | **Belvo**        | sí (Belvo CL) | 🟢 thin + paridad MX/BO endpoints (#573) + recon DTE↔Belvo (#569) + extraFields fix (#571) · E2E validado VM (5 links persistidos) | ✅ usa core Belvo compartido (mismas creds workspace que MX) | ✅ ZMP_OF_LINK/ACCOUNT/TX + TRAN_DTE_ID | deploy QA público (Victor handoff #570 · ~1h DevOps) · plan Belvo prod confirmar |
| 🇺🇾 Uruguay    | (no iniciado)    | ?  | ❌                            | ❌                                       | ❌ | arrancar |
| 🇧🇴 Bolivia    | **Prometeo**     | BO/PY/UY/AR/PE/CL | 🟢 thin + auto-relogin + cred vault AES + sync + smoke e2e (QA-ready) | ✅ `zymplo-openfinance-prometeo` (deployed QA) | ✅ ZMP_OF_LINK/ACCOUNT/TX/CRED_VAULT | API key Prometeo prod + acuerdo comercial + deploy prod |
| 🇨🇷 Costa Rica | (no iniciado)    | ?  | ❌                            | ❌                                       | ❌ | arrancar |

> **Nota providers cross-país:** Belvo cubre MX/CO/CL/PE/BR (sandbox cubre BR completo · prod a confirmar con Martín) · Prometeo cubre BO/PY/UY/AR/PE/CL · Pluggy queda como fallback BR si Belvo no escala (no en monorepo todavía) · Akoya cubre EEUU · kushkipagos cubre Ecuador. **Cada provider = un service core compartido** en raíz del monorepo.

---

## 🔁 Provider matrix · validación e2e cross-swap

Post serie provider-abstraction (#493-#498), cada thin país-específico puede apuntar a cualquier core (Belvo o Prometeo) cambiando solo `OF_CORE_BASE_URL` · sin tocar código. **Smoke tests local 2026-05-06**:

| Thin | Default | Cross-swap | E2e validado |
|---|---|---|---|
| 🇲🇽 MX (`mexico/zymplo-openfinance`) | → Belvo (`:3010`) | → Prometeo (`:3011`) | ✅ ambas direcciones · `country=MEX, provider={belvo,prometeo}` persistido |
| 🇧🇴 BO (`bolivia/zymplo-openfinance-bo`) | → Prometeo (`:3011`) | → Belvo (`:3010`) | ✅ ambas direcciones · `country=BOL, provider={prometeo,belvo}` persistido |
| 🇨🇱 CL (`chile/zymplo-openfinance`) | → Belvo (`:3010`) | → Prometeo (`:3011`) | ✅ E2E validado VM 2026-05-06 · 5 links persistidos en `zmp_of_link` con `country=CHL, provider=belvo` (incluido soft-delete · tested via #569 + #571 + #573) |

**Cómo cambiar de provider en runtime** (un solo file):

```bash
# Bolivia → Belvo en lugar de Prometeo (ej. si Prometeo está caído)
# bolivia/zymplo-openfinance-bo/.env
OF_PROVIDER=belvo
OF_CORE_BASE_URL=http://core-belvo:3010
OF_CORE_AUTH_TOKEN=<token-belvo-core>
```

**Caveats observados** en smoke con mock data:

- Catálogo de instituciones del provider mock no siempre cubre el país objetivo (ej. Belvo mock no tiene BOL · Prometeo mock no tiene MEX). En producción cada provider real tiene cobertura propia · ver columna "Cobertura del provider" arriba.
- POST /links + GET /links + GET /accounts funcionan correctamente en cross-swap · la persistencia ZMP_OF_LINK discrimina por `(country, provider)` correctamente.

**Webhooks NO son provider-agnostic** (intencional · `POST /webhooks/belvo` y `POST /webhooks/prometeo` son rutas separadas con firmas distintas). Cada provider tiene su propia ruta y secret HMAC.

---

## 📱 Integración con plataforma · Mobile + WhatsApp Bot

Estado de cada país en los productos cross-cutting que consumen las APIs país-específicas:

- **`zymplo-mobile`** · Expo / React Native · multi-tenant · screens por país-feature
- **`zymplo-langgraph`** · Python · LangGraph bot WhatsApp · tools que llaman al `zymplo-api` proxy

| País | Mobile App (screens) | WhatsApp Bot (tools) |
|---|---|---|
| 🇲🇽 **México**     | ✅ FULL · CFDI setup + emit + history + detail + drafts + NC/ND (PR-MMX1 #591) · OF connect/manual-match | ✅ `cfdi_tools.py` + `openfinance_tools.py` |
| 🇧🇷 **Brasil**     | ✅ NFS-e emit/history/detail | ✅ `nfse_tools.py` |
| 🇧🇴 **Bolivia**    | ✅ FULL · SIAT setup + emit + history + detail + PdV + NC/ND + Drafts (#534-#537) · OF Prometeo (#525) | ✅ `siat_tools.py` 6 tools (emit/buscar/listar/anular + NC/ND #526 #535) · `factura_tools.py` wrapper multi-país (#530) · OF dispatch multi-país (#521) |
| 🇨🇱 **Chile**      | ✅ FULL · DTE setup + emit + history + detail + drafts + NC/ND (PR-MCL1/2/3 #580/#582/#584) · OF Belvo screen (PR-MCL4 #586) | ✅ `dte_tools.py` 6 tools (paridad SIAT BO) + dispatch en `factura_tools.py` para CL (#578) |
| 🇵🇾 Paraguay   | ❌ no integrado | ❌ no integrado |
| 🇨🇴 Colombia   | ❌ no integrado | ❌ no integrado |
| 🇵🇪 Perú       | ❌ no integrado | ❌ no integrado |
| 🇪🇨 Ecuador    | ❌ no integrado | ❌ no integrado |
| 🇪🇸 España     | ❌ no integrado | ❌ no integrado |
| 🇦🇷 Argentina  | ❌ no integrado | ❌ no integrado |
| 🇺🇾 Uruguay    | ❌ no integrado | ❌ no integrado |
| 🇺🇸 EEUU       | ❌ no integrado | ❌ no integrado |
| 🇨🇷 Costa Rica | ❌ no integrado | ❌ no integrado |

### ✅ Resolved · langgraph OF migrado a multi-país (#521)

Las referencias a `MX_OF_LINK` en `openfinance_tools.py` y `openfinance_client.py` eran solo **comments doc desactualizados** (no queries SQL · el bot llama via httpx al proxy `zymplo-api`, no a Oracle directo). Comentarios actualizados a `zmp_of_link` post #521.

Bonus en #521: el bot ahora dispatch automático según `paisCodi` (canonical SKN.COME_PAIS post #550) · empresas BO (paisCodi=28) consultan `/api/v2/openfinance-bo/*` (Prometeo) · empresas MX (paisCodi=25) siguen `/api/v2/openfinance/*` (Belvo) · futuro CL (paisCodi=30) consultará `/api/v2/openfinance-cl/*` · transparente.

**TODO** (PRs futuros · prioridad alta):
- ~~Mobile · agregar screens SIAT BO~~ · ✅ cerrado #527 (emit/history/detail) + chain follow-ups #534-#537 (PdV/NC-ND/drafts/cert)
- ~~Mobile · openfinance multi-país BO~~ · ✅ cerrado #525 (sub-view BoView en openfinance.tsx con paisCodi dispatch)
- ~~Langgraph · agregar `siat_tools.py` para BO~~ · ✅ cerrado #526 (4 tools) + #535 (NC/ND · 6 tools total) · proxy `factura-bo` en zymplo-api ya existía
- ~~Langgraph · wrapper multi-país facturas~~ · ✅ cerrado #530 (`factura_tools.py · erp_emitir_comprobante` auto-dispatch BR/MX/BO)
- ~~Langgraph · agregar `dte_tools.py` para CL~~ · ✅ cerrado #578 (6 tools + dispatch en factura_tools.py)
- ~~Mobile · agregar screens dte-* + openfinance-cl~~ · ✅ cerrado chain mobile CL #580/#582/#584/#586 + paridad MX drafts/nota #591
- Future PR · proxy multipart en zymplo-api `/api/v2/dte-cl/cargar-cert` (mientras tanto · workaround directo al thin)
- Future PR · `EXPO_PUBLIC_DTE_CL_SERVICE_TOKEN` en Doppler para cert upload mobile en QA
- DevOps · deploy QA público thin Chile OF (URL `openfinance-cl-qa.zymplo.com` + nginx + Doppler) · ver `chile/zymplo-openfinance/deploy/CHECKLIST.md` · backend mergeado y E2E validado en VM (5 links persistidos)
- DevOps · `EXPO_PUBLIC_SIAT_BO_SERVICE_TOKEN` en Doppler para que mobile cert upload funcione en QA (#537)
- Future PR · endpoint multipart en proxy `zymplo-api/factura-bo` para destrabar mobile cert upload en producción (workaround actual: directo al thin con service-token)
- Future migration · agregar `emi_empresa_id` FK a `cl_dte_emisor` para que `RunAllReconciliationService` CL auto-resuelva mapping (hoy caller-side via `pairs` array)

---

## 🎯 Plan de alineación al monorepo (lo que falta para cerrar todo)

Para cada país que está 🧪 (listo en test externo), el trabajo es:

```
1. Crear estructura en monorepo:
   - <country>/zymplo-<servicio>/  (factura electrónica país-específica)
   - <country>/zymplo-openfinance/ (thin adapter cross-provider)

2. Migrar código del repo origen al monorepo siguiendo patrón:
   - mexico/zymplo-cfdi/ y bolivia/zymplo-siat/ son los referentes
   - chile/zymplo-dte/ después del refactor F1 también será referente

3. Convertir migrations DB a Oracle ZMP idempotentes:
   - <COUNTRY>_<SERVICE>_FACTURA / _CERT_VAULT / _etc en zmp.<>
   - NO PostgreSQL/MySQL aislado · NO replicar SKN

4. Wirear repo a Oracle ATP (con in-memory fallback)

5. Adoptar service core OF compartido por provider (Belvo/Prometeo/Pluggy/Akoya/kushkipagos)

6. Doppler + deploy QA workflow

7. Tests verdes
8. Certificación real (cuando aplique)
9. Deploy Prod
```

---

## 🌐 Componentes técnicos genéricos (zymplo monorepo)

### Tablas Oracle ZMP

| Tabla | Estado | Uso |
|---|---|---|
| `ZMP_TRAN` | ✅ existente · extendida 2026-05-05 (+3 cols: cuf, cfdi_uuid, belvo_tx_id) | cashflow universal · conciliación bidireccional |
| `ZMP_AUDIT_LOG` | ✅ creada 2026-05-05 | audit cross-país (country, service) |
| `ZMP_NOTIFICATION` | ✅ creada · ⚠️ 2 indexes secundarios pendientes (Oracle no soporta WHERE) | notifs cross-país (country, service) |
| `ZMP_OF_LINK` | ✅ creada 2026-05-05 | links OF cross-país, cross-provider |
| `ZMP_OF_ACCOUNT` | ✅ creada 2026-05-05 | cuentas OF cross-país, cross-provider |
| `ZMP_OF_TRANSACTION` | ✅ creada 2026-05-05 | transactions OF cross-país, cross-provider |
| `ZMP_OF_LINK_CRED_VAULT` | ✅ creada 2026-05-05 | creds AES-256-GCM (providers session-based) |
| `MX_CFDI_HISTORICO`, `MX_CSD_VAULT`, `MX_EMPR_FISCAL` | ✅ existentes | facturación MX |
| `BO_SIAT_FACTURA`, `BO_SIAT_PUNTO_VENTA`, `BO_SIAT_CUFD_CACHE`, `BO_SIAT_CERT_VAULT`, `BO_SIAT_EVENTO_SIG` | ✅ creadas 2026-05-05 | facturación BO |
| `SKN.CLIE_MAES`, `SKN.COME_EMPR`, `SKN.SEGU_USER` | ✅ legacy · read-only | clientes/empresas/usuarios cross-país |

### Microservicios compartidos

| Servicio | Provider | Países que lo usarán | Estado |
|---|---|---|---|
| `zymplo-openfinance-belvo` | Belvo | MX, CO, PE, CL, **BR (PRs #618 + #635 + #638 + #639)**, futuros | ✅ código + deploy infra + HISTORICAL_UPDATE webhook handler (#639 · BR OFDA observability) · DevOps pendiente |
| `zymplo-openfinance-prometeo` | Prometeo | BO, PY, UY, AR, PE, CL | ✅ código + deploy infra · DevOps pendiente |
| `zymplo-openfinance-pluggy` | Pluggy | BR (fallback) | ❌ no aplica F1 · BR usa Belvo (PR #618) · Pluggy queda como fallback si Belvo no escala |
| `zymplo-openfinance-akoya` | Akoya | EEUU | ❌ F4+ (cuando entre EEUU) |
| `zymplo-openfinance-kushkipagos` | kushkipagos | EC | ❌ F4+ (cuando entre Ecuador) |

---

## 🔗 API Docs (Swagger UI · entornos QA)

Cada thin/servicio país-específico expone su OpenAPI 3 en `/api-docs` con "Try it out" listo. **Solo en QA** (en prod los Swagger UI quedan deshabilitados por convención `OF_DEV_HEADERS_ENABLED=false` · ver memoria `feedback_dev_headers_security_prod`).

| País | Servicio | Standard | Swagger UI |
|---|---|---|---|
| 🇲🇽 México   | CFDI (Facturama PAC)        | CFDI 4.0 | https://cfdi-mx-qa.zymplo.com/api-docs/ |
| 🇲🇽 México   | Open Finance (Belvo · thin) | OF MX    | https://openfinance-mx-qa.zymplo.com/api-docs/ |
| 🇧🇴 Bolivia  | Facturación (SIAT/SIN)      | SIAT     | https://facturacion-bolivia-qa.zymplo.com/api-docs/ |
| 🇧🇴 Bolivia  | Open Finance (Prometeo · thin) | OF BO  | https://openfinance-bo-qa.zymplo.com/api-docs/ |
| 🇧🇷 Brasil   | NFS-e (Receita Federal)     | NFS-e    | TBD · `nfse-qa.zymplo.com` no expone Swagger todavía (audit Oracle ZMP pendiente) |
| 🇧🇷 Brasil   | Open Finance (Belvo OFDA · thin) | OF BR · widget    | TBD · `openfinance-br-qa.zymplo.com` falta deploy QA público + Belvo dashboard callback config (PRs #618 + #635 + #636 + #638 + #639 mergeados · DevOps via `brasil/zymplo-openfinance-br/deploy/CHECKLIST.md`) |
| 🇨🇱 Chile    | DTE SII                     | DTE      | TBD · falta deploy QA público (backend QA-ready post chain CL1-CL4 · Victor handoff) |
| 🇨🇱 Chile    | Open Finance (Belvo · thin) | OF CL    | TBD · falta deploy QA público (Victor handoff via `chile/zymplo-openfinance/deploy/CHECKLIST.md` · validado E2E en VM 2026-05-06) |

### Cores compartidos (multi-país · sin Swagger UI público)

Los service core (`zymplo-openfinance-belvo`, `zymplo-openfinance-prometeo`) son consumidos solo por los thins por service name interno · no exponen Swagger público. Para inspeccionar request/response del core, usar el thin del país correspondiente.

### Cómo testear desde Swagger UI · auth dev shortcut

Cada thin con `OF_DEV_HEADERS_ENABLED=true` (solo QA · jamás prod) expone:

```bash
GET /auth/dev/info
# devuelve x-of-service-token + x-empresa-id listos para pegar en
# el modal "Authorize" de Swagger UI · sin pedir nada a admin
```

Ejemplo BO:
```bash
curl https://openfinance-bo-qa.zymplo.com/auth/dev/info
# → { "headers": { "x-of-service-token": "...", "x-empresa-id": "1" }, ... }
```

---

## 🛣️ Roadmap por fases

| Fase | Qué | Cuándo | Estado |
|---|---|---|---|
| **F0** | Migrations Oracle ZMP base + Bolivia SIAT cerrado | 2026-05-05 | ✅ cerrado hoy |
| **F1** | Service core compartido OF Belvo + Prometeo + thin adapters MX/BO | esta semana / próxima | ⏳ |
| **F2** | Chile DTE Oracle ZMP + paridad MX/BO (anular/NC-ND/drafts/catálogos/recon Belvo) | 2026-05-06 | ✅ cerrado · chain #559/#562/#564/#566/#569/#570/#571/#573 |
| **F2.5** | Mobile + bot paridad full MX/BO/CL · drafts + NC/ND + OF cross-país | 2026-05-06 | ✅ cerrado · #578/#580/#582/#584/#586/#591 |
| **F2.6** | XSD validators MX CFDI 4.0 + BO SIAT estructural · pre-PAC failsafe | 2026-05-06 | ✅ cerrado · #587/#588 |
| **F3** | Brasil OF al monorepo · thin Belvo (no Pluggy) · paridad MX/CL · audit NFS-e Oracle ZMP pendiente para reconciliation | 2026-05-06 | 🟢 thin OF mergeado · PR #618 · NFS-e audit ⏳ |
| **F3.1** | Wave 5.1 OF · CO + PE thin adapters Belvo (post-CL pattern) | 2026-05-06 | ✅ thin code · #589 #590 · falta deploy QA + smoke + screens mobile |
| **F3.2** | Wave 5.2 OF · AR + UY thin adapters Prometeo | 2026-05-06 | ✅ thin code · #592 #593 · falta deploy QA + smoke + screens mobile |
| **F4** | Governance (arch-guard CI · CLAUDE.md por país · backlog) | paralelo | ⏳ |
| **F5** | **Migración masiva** · alinear los 🧪 al monorepo (CO/PE/AR/EC/UY/EEUU/ES/PY/CR) | ola por ola post F4 | ⏳ |
| **F6** | Sumar país nuevo no listado (template repetible) | cuando entren | N/A |
| **F7** | Chile DTE cross-country middleware + provider split + crons + cert vault wiring | 2026-05-06/07 | ✅ #572 #576 #577 #583 |
| **F7.4** | Cross-country validators (RFC homoclave + structural CFDI/SIAT + reserved NITs) | 2026-05-06/07 | ✅ #579 #585 #587 #588 #594 #595 |
| **F8** | zymplo-api DTE module proxy + wireup F8.2 | 2026-05-07 | ✅ #581 #594 #595 |
| **F9** | Brasil NFS-e normalize · cross-country middleware + ZMP DDL + provider split + crons + drafts + audit + structural validator | 2026-05-07 | ✅ #616 #617 #619 #624 #625 #626 #632 (B-v) |
| **F10** | **Cadena A** · cross-country preview endpoints (espejo Brasil nativo) MX+BO+CL | 2026-05-07 | ✅ #621 #622 #623 |
| **F10.1** | **Cadena C** · cross-country catálogos vendoreados (espejo Brasil nativo) MX+BO+CL | 2026-05-07 | ✅ #627 #628 #629 |
| **F11** | Cierre paridad 100% · CL audit endpoint + BR structural validator | 2026-05-07 | ✅ #631 #632 |
| **F12** | 🇧🇷 BR Open Finance stack completo · zymplo-api proxy + langgraph bot tools + mobile widget Belvo OFDA + core webhook HISTORICAL_UPDATE | 2026-05-07 | ✅ #635 #636 #638 #639 |
| **F13** | 🇧🇷 BR NFS-e Phase 1 · A1 ZMP migrations + B1 drafts mobile + B2 setup mobile | 2026-05-07 | ✅ #641 (A1 aplicado en dbautdesa02 · sin PR · DBA op directo) |

### Orden de migración masiva sugerido (Fase 5)

Priorización por:
1. **Provider en común** (los que usan Belvo se benefician primero del core)
2. **Cobertura de mercado**
3. **Estado del cert real** (más cerca de prod = más prioridad)

| Wave | Países | Provider OF | Razón |
|---|---|---|---|
| 5.1 | 🇨🇴 CO, 🇵🇪 PE, 🇨🇱 CL | Belvo | core ya extraído (F1) · solo agregar thin adapter |
| 5.2 | 🇦🇷 AR, 🇺🇾 UY | Prometeo (probable) | core ya extraído |
| 5.3 | 🇧🇷 BR | **Belvo OFDA** (#618 thin · #635 api · #636 bot · #638 mobile · #639 webhook) | stack 8 PRs cerrado · paridad cross-pillar (api/bot/mobile/core) · falta DevOps deploy + Belvo dashboard callback |
| 5.4 | 🇪🇸 ES, 🇪🇨 EC | varios (kushkipagos para EC) | crear cores (F4+) |
| 5.5 | 🇺🇸 EEUU, 🇵🇾 PY, 🇨🇷 CR | varios | misceláneos · uno a uno |

---

## 📋 Checklist · alinear país existente al monorepo

Cuando llegue el turno de migrar un país que está 🧪 al monorepo:

### Facturación electrónica
- [ ] Identificar repo origen donde vive el código actual
- [ ] Crear `<country>/zymplo-<servicio>/` en monorepo (ver convención naming según autoridad fiscal)
- [ ] Copiar/migrar código siguiendo patrón `mexico/zymplo-cfdi/` o `bolivia/zymplo-siat/`
- [ ] Reescribir migrations DB en `zymplo-api/migrations/<fecha>_<country>_<servicio>_*.sql` para Oracle ZMP
- [ ] Vault certificados con Fernet o AES-256-GCM (NO plain text)
- [ ] Wirear repos contra Oracle ATP (`db.withConnection`) con in-memory fallback
- [ ] Tests unit verdes (mismo nivel que MX/BO)
- [ ] CLAUDE.md del subproyecto (regla "datos van a Oracle ZMP")
- [ ] Doppler config QA + prod
- [ ] Deploy QA workflow + smoke test
- [ ] Marcar 🟡 → ✅ en este dashboard

### Open Finance
- [ ] Identificar provider del país (Belvo/Prometeo/Pluggy/Akoya/kushkipagos/etc)
- [ ] Si el service core del provider NO existe aún en monorepo → crear primero (F1/F3/F4+)
- [ ] Crear `<country>/zymplo-openfinance/` thin adapter
- [ ] Adapter invoca service core con `country=<XXX>` · provider implícito
- [ ] Wirear contra `ZMP_OF_*` (genérica · ya existe)
- [ ] Compliance regulador local (CNBV/SBIF/SBS/SFC/etc)
- [ ] Tests verdes
- [ ] Deploy QA + prod
- [ ] Marcar en dashboard

### Operacional / mobile
- [ ] Integración a la app mobile (mostrar facturas + cuentas bancarias)
- [ ] Integración WhatsApp bot (per Luz: pendiente para todos los países hoy)
- [ ] Documentación end-user (si aplica)

---

## 📝 Cómo se actualiza este dashboard

1. **Cambio de estado** (✅/🟡/🧪/⏳/❌): edit-this-md, PR a `develop`, descripción del avance
2. **Sumar país nuevo**: agregar fila en las tablas + entrada en provider mapping
3. **Migrar país de 🧪 a 🟡 a ✅**: actualizar las 3 columnas correspondientes (Estado / Tablas Oracle ZMP / monorepo)
4. **Cambio de fase del roadmap**: edit la sección de fases con nueva fecha y estado
5. **Cambio de dueño**: actualizar columna Dueño en vista global

**¿Por qué Markdown y no HTML interactivo?** Versionable en git · GitHub renderiza tablas y checkboxes nativamente · cualquier dev lo actualiza via PR · diff claro de qué cambió y cuándo. Si después se quiere visualización fancy, agregamos un script Python/Node que genere HTML desde este `.md`.
