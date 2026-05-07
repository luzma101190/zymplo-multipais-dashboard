# 🌎 Zymplo · Dashboard Multi-país

> **Propósito:** vista comparativa de los 13 países del ecosistema Zymplo · estado de facturación electrónica + integración bancaria · marcando avance de alineación al monorepo `zymplo/` y Oracle ZMP.
>
> **Última actualización:** 2026-05-06 noche · **paridad full mobile cross-país MX=BO=CL** (drafts + NC/ND + OF Belvo via thin) · expansion OF a CO/PE (#589 #590 Belvo) + AR/UY (#592 #593 Prometeo) · CFDI/SIAT XSD validators wireup (#587 #588 #594) · ver `git log MULTIPAIS-DASHBOARD.md` para historial · cualquier dev puede actualizar via PR a `develop`.

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
| 🇧🇷 **Brasil**     | Martín Rolón     | 🧪 NFS-e listo                       | 🧪 Pluggy listo (Docu+Swagger)        | 🟡 zymplo-nfse en monorepo · audit |
| 🇵🇾 **Paraguay**   | Liz Villasanti   | 🧪 DNIT listo (Zymplo Fact. E.)      | ⏸️ En pausa (80% docu) · pending CO   | ❌ no en monorepo |
| 🇲🇽 **México**     | Luz Espínola     | 🟢 CFDI F0 + drafts + NC/ND mobile (PR-MMX1 #591) · F7.4b XSD validators (#587) + RFC homoclave (#585) · QA-ready · falta cert SAT prod + deploy prod | 🟢 Belvo · thin + core + sync + smoke e2e · falta acuerdo comercial Belvo prod + deploy prod | ✅ mexico/zymplo-cfdi + zymplo-openfinance (thin) |
| 🇺🇸 **EEUU**       | Andrea Amarilla  | 🧪 UBL listo (no envía a regulador)  | ⏳ Akoya en proceso                   | ❌ no en monorepo |
| 🇨🇴 **Colombia**   | Liz Villasanti   | 🧪 DIAN listo (falta cert real)      | 🟡 thin Belvo CO en monorepo (#589) · falta deploy QA + smoke e2e | ⏳ colombia/zymplo-openfinance-co (post #589) · DIAN aún externo |
| 🇪🇸 **España**     | Francisco Villalba | 🧪 listo (falta cert real)         | 🧪 listo                              | ❌ no en monorepo |
| 🇦🇷 **Argentina**  | Gadiel Muñoz     | 🧪 listo (falta cert real)           | 🟡 thin Prometeo AR en monorepo (#592) · falta deploy QA + smoke e2e | ⏳ argentina/zymplo-openfinance-ar (post #592) · AFIP aún externo |
| 🇵🇪 **Perú**       | Alberto Mendez   | 🧪 listo (falta cert real)           | 🟡 thin Belvo PE en monorepo (#590) · falta deploy QA + smoke e2e | ⏳ peru/zymplo-openfinance-pe (post #590) · SUNAT aún externo |
| 🇪🇨 **Ecuador**    | Orlando          | 🧪 listo (falta cert real)           | 🧪 kushkipagos listo                  | ❌ no en monorepo |
| 🇨🇱 **Chile**      | Martín Rolón     | 🟢 DTE F0+F4 chain cerrado (#559/#562/#564/#566) + mobile chain (PR-MCL1/2/3/4 #580/#582/#584/#586) + bot langgraph dte_tools (#578) · QA-ready · falta cert SII prod (~$25 USD E-CertChile) | 🟢 Belvo · thin + paridad MX/BO endpoints (#573) + recon DTE↔Belvo (#569) + mobile screen openfinance-cl (#586) · E2E validado en VM · falta deploy QA público (Victor #570 handoff) | ✅ chile/zymplo-dte + chile/zymplo-openfinance · Oracle ZMP (cl_dte_*, ZMP_OF_*, TRAN_DTE_ID) |
| 🇺🇾 **Uruguay**    | Orlando Dure     | 🧪 listo (falta cert real)           | 🟡 thin Prometeo UY en monorepo (#593) · falta deploy QA + smoke e2e | ⏳ uruguay/zymplo-openfinance-uy (post #593) · DGI aún externo |
| 🇧🇴 **Bolivia**    | Luz Espínola     | 🟢 SIAT F0 cerrado 2026-05-05 + F7.4b XSD validator (#588) · QA-ready · falta cert ADSIB prod + deploy prod | 🟢 Prometeo · thin + core + auto-relogin + sync + smoke e2e · falta acuerdo comercial Prometeo prod + deploy prod | ✅ bolivia/zymplo-siat + zymplo-openfinance-bo (thin) |
| 🇨🇷 **Costa Rica** | Alberto Mendez   | ⏳ en proceso                        | ❌ no iniciado                        | ❌ no en monorepo |

| 🌐 **Componentes Compartidos** | Estado |
|---|---|
| `zymplo-api` proxy (cross-país gateway) | ✅ MX (cfdi + openfinance) + BR (nfse) + BO (openfinance-bo) · falta SIAT + DTE |
| `zymplo-mobile` (Expo · React Native) | ✅ MX (CFDI completo) + BR (NFS-e) + BO (SIAT completo · setup/emit/history/detail/PdV/NC-ND/drafts) + OF multi-país MX/BO · falta DTE CL |
| `zymplo-langgraph` (WhatsApp bot) | ✅ MX (CFDI + OF Belvo) + BR (NFS-e) + BO (SIAT 6 tools incluido NC/ND + OF Prometeo) · falta `dte_tools` CL |
| Tablas Oracle ZMP genéricas (audit, notif, of) | ✅ creadas 2026-05-05 |
| Service core `zymplo-openfinance-belvo` | ✅ código + deploy infra · runtime DevOps pendiente (#58277) |
| Service core `zymplo-openfinance-prometeo` | ✅ código + deploy infra · runtime DevOps pendiente (#58277) |
| Provider abstraction · contract neutral entre cores | ✅ #493-#497 (2026-05-06) |
| Thins MX/CL/BO provider-agnostic vía `OF_CORE_BASE_URL` | ✅ #494-#495 (2026-05-06) |
| Service core `zymplo-openfinance-pluggy` | ❌ Fase 3 (cuando entre BR al monorepo) |
| Service core `zymplo-openfinance-akoya` | ❌ Fase 4+ (cuando entre EEUU) |
| Service core `zymplo-openfinance-kushkipagos` | ❌ Fase 4+ (cuando entre Ecuador) |
| Governance (arch-guard CI · CLAUDE.md por país · backlog) | ❌ Fase 4 |

## 🎯 Paridad mobile cross-país (post 2026-05-06)

| País | Setup | Emit | History | Detail | Drafts | Nota | OF |
|---|---|---|---|---|---|---|---|
| 🇲🇽 MX | ✅ | ✅ | ✅ | ✅ | ✅ #591 | ✅ #591 | ✅ |
| 🇧🇴 BO | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 🇨🇱 CL | ✅ | ✅ | ✅ | ✅ | ✅ #584 | ✅ #584 | ✅ #586 |
| 🇧🇷 BR | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | N/A |

3 países (MX/BO/CL) con feature parity completa: setup cert · emit factura · history paginado · drilldown · drafts (BORRADOR workflow) · NC/ND emission · OF banking. Brasil pendiente migración cuando NFS-e entre al monorepo.

---

## 🧾 Detalle · Facturación Electrónica

| País | Autoridad fiscal | Estado en repo origen | Repo/Stack origen actual | En monorepo zymplo | Tablas Oracle ZMP | Cert. real | Deploy QA | Deploy Prod |
|---|---|---|---|---|---|---|---|---|
| 🇧🇷 Brasil     | Receita Federal · NFS-e | 🧪 listo | `brasil/zymplo-nfse/` (en monorepo) | 🟡 sí · falta audit Oracle | ? audit | ❌ | ? | ❌ |
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
| 🇧🇷 Brasil     | **Pluggy**       | BR | 🧪 listo (Docu+Swagger)       | ❌ `zymplo-openfinance-pluggy` (Fase 3) | ❌ | crear core + thin BR |
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

> **Nota providers cross-país:** Belvo cubre MX/CO/CL/PE/BR (parcial) · Prometeo cubre BO/PY/UY/AR/PE/CL · Pluggy cubre BR (más amplio) · Akoya cubre EEUU · kushkipagos cubre Ecuador. **Cada provider = un service core compartido** en raíz del monorepo.

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
| `zymplo-openfinance-belvo` | Belvo | MX, CO, PE, CL, (BR partial), futuros | ✅ código + deploy infra · DevOps pendiente |
| `zymplo-openfinance-prometeo` | Prometeo | BO, PY, UY, AR, PE, CL | ✅ código + deploy infra · DevOps pendiente |
| `zymplo-openfinance-pluggy` | Pluggy | BR | ❌ F3 (cuando entre BR al monorepo) |
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
| 🇧🇷 Brasil   | NFS-e (Receita Federal)     | NFS-e    | TBD · `nfse-qa.zymplo.com` no expone Swagger todavía (servicio pre-monorepo) |
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
| **F3** | Brasil al monorepo · service core Pluggy · alineación NFS-e a Oracle | post-F2 | ⏳ |
| **F3.1** | Wave 5.1 OF · CO + PE thin adapters Belvo (post-CL pattern) | 2026-05-06 | ✅ thin code · #589 #590 · falta deploy QA + smoke + screens mobile |
| **F3.2** | Wave 5.2 OF · AR + UY thin adapters Prometeo | 2026-05-06 | ✅ thin code · #592 #593 · falta deploy QA + smoke + screens mobile |
| **F4** | Governance (arch-guard CI · CLAUDE.md por país · backlog) | paralelo | ⏳ |
| **F5** | **Migración masiva** · alinear los 🧪 al monorepo (CO/PE/AR/EC/UY/EEUU/ES/PY/CR) | ola por ola post F4 | ⏳ |
| **F6** | Sumar país nuevo no listado (template repetible) | cuando entren | N/A |

### Orden de migración masiva sugerido (Fase 5)

Priorización por:
1. **Provider en común** (los que usan Belvo se benefician primero del core)
2. **Cobertura de mercado**
3. **Estado del cert real** (más cerca de prod = más prioridad)

| Wave | Países | Provider OF | Razón |
|---|---|---|---|
| 5.1 | 🇨🇴 CO, 🇵🇪 PE, 🇨🇱 CL | Belvo | core ya extraído (F1) · solo agregar thin adapter |
| 5.2 | 🇦🇷 AR, 🇺🇾 UY | Prometeo (probable) | core ya extraído |
| 5.3 | 🇧🇷 BR | Pluggy | crear core (F3) |
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
