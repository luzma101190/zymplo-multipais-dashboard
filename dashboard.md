# 🌎 Zymplo · Dashboard Multi-país

> **Vista ejecutiva** del estado de facturación electrónica + Open Finance bancaria por país. Para detalle técnico (PRs, migrations, paridad estructural) ver `git log MULTIPAIS-DASHBOARD.md` · historial commits archivado.
>
> **Última actualización:** 2026-05-20

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
| 🇧🇷 **Brasil** | ✅ E2E confirmado | ✅ Belvo OFDA · ✅ Pluggy sandbox | ✅ App · ✅ WA | [facturacion-br-qa](https://facturacion-br-qa.zymplo.com/api-docs/) · [openfinance-br-qa](https://openfinance-br-qa.zymplo.com/api-docs/) · [openfinance-pluggy-qa](https://openfinance-pluggy-qa.zymplo.com/health) |
| 🇲🇽 **México** | ✅ E2E confirmado | ✅ Belvo sandbox | ✅ App · ✅ WA | [facturacion-mx-qa](https://facturacion-mx-qa.zymplo.com/api-docs/) · [openfinance-mx-qa](https://openfinance-mx-qa.zymplo.com/api-docs/) |
| 🇧🇴 **Bolivia** | ✅ E2E confirmado | ❌ no aplica (ASFI) | ✅ App · ✅ WA | [facturacion-bo-qa](https://facturacion-bo-qa.zymplo.com/api-docs/) |
| 🇵🇾 **Paraguay** | ✅ E2E SIFEN (externo) | ⏸️ pausado | ✅ App · ✅ WA | [facturacion-py-qa](https://sifen-qa.alarmas.com.py/docs/api-reference/) |
| 🇨🇴 **Colombia** | ✅ E2E sandbox (DIAN) | 🟡 Belvo sandbox | ✅ App · ✅ WA | [facturacion-co-qa](https://facturacion-co-qa.zymplo.com/api-docs/) · [openfinance-co-qa](https://openfinance-co-qa.zymplo.com/api-docs/) |
| 🇦🇷 **Argentina** | ✅ E2E Oracle real (AFIP · cert pendiente) | ✅ Prometeo sandbox | ✅ App · ✅ WA | [facturacion-ar-qa](https://facturacion-ar-qa.zymplo.com/api-docs/) · [openfinance-ar-qa](https://openfinance-ar-qa.zymplo.com/api-docs/) · [PDF AFIP oficial](samples/argentina/ar-arca-factura-A-4-cae-88015788975331-rg1415.pdf) |
| 🇨🇱 **Chile** | 🟡 E2E estructural (cert dummy) | ✅ Belvo sandbox | ✅ App · ✅ WA | [facturacion-cl-qa](https://facturacion-cl-qa.zymplo.com/api-docs/) · [openfinance-cl-qa](https://openfinance-cl-qa.zymplo.com/api-docs/) |
| 🇪🇨 **Ecuador** | ✅ E2E sandbox | ✅ Prometeo sandbox | ✅ App · ✅ WA | [facturacion-ec-qa](https://facturacion-ec-qa.zymplo.com/api-docs/) · [openfinance-ec-qa](https://openfinance-ec-qa.zymplo.com/api-docs/) |
| 🇵🇪 **Perú** | ✅ E2E sandbox · Oracle real | ✅ Belvo sandbox | ✅ App · ✅ WA | [facturacion-pe-qa](https://facturacion-pe-qa.zymplo.com/docs/) · [openfinance-pe-qa](https://openfinance-pe-qa.zymplo.com/api-docs/) |
| 🇺🇾 **Uruguay** | ✅ E2E mock + Oracle real | ✅ Prometeo sandbox | ✅ App · ✅ WA | [facturacion-uy-qa](https://facturacion-uy-qa.zymplo.com/api-docs/) · [openfinance-uy-qa](https://openfinance-uy-qa.zymplo.com/api-docs/) |
| 🇨🇷 **Costa Rica** | ✅ E2E estructural Oracle real (Hacienda · cert pendiente) | ✅ Belvo sandbox | ✅ App · ✅ WA | [facturacion-cr-qa](https://facturacion-cr-qa.zymplo.com/docs/) · [openbanking-cr-qa](https://openbanking-cr-qa.zymplo.com/api-docs/) |
| 🇺🇸 **EEUU** | ✅ E2E Oracle real | ✅ Akoya sandbox | ✅ App · ✅ WA | [facturacion-us-qa](https://facturacion-us-qa.zymplo.com/api-docs/) · [openfinance-us-qa](https://openfinance-us-qa.zymplo.com/docs) |
| 🇪🇸 **España** | ✅ E2E QA público (cert AEAT pendiente) | ✅ Tink sandbox | ✅ App · ✅ WA | [facturacion-es-qa](https://facturacion-es-qa.zymplo.com/api-docs/) · [openfinance-es-qa](https://openfinance-es-qa.zymplo.com/api-docs/) |
