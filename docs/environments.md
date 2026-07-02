# 🏦 CFBL Credit Summary — Environments & Quick-Links Hub

> **One place for every link the team needs** — UI, Swagger, and observability (Splunk, Grafana,
> AppDynamics) across all environments. Bookmark this page. ⭐

> 📌 **Owner:** _<team / DL>_  ·  🗓️ **Last updated:** _<YYYY-MM-DD>_  ·  ✏️ Keep it current — see [Maintaining this page](#-maintaining-this-page).

---

## 🗺️ Environment map

| Tier | Code | Purpose | App links | Status |
|:---|:---:|:---|:---:|:---|
| 🟢 **DEV** | `CX` | Development | ✅ | Active |
| 🔵 **TE1** | `I4` | Test / SIT | ✅ | Active |
| 🟡 **TE2** | `R0` | UAT | ✅ | Active |
| 🔴 **PROD** | `P0` | Production | 🔧 | **Links pending — to be updated** |

<sub>Naming: **Tier** is our label (DEV/TE1/TE2/PROD); **Code** is the platform environment code (CX/I4/R0/P0).</sub>

---

## 🔗 Application links

Quick-open matrix — click **Open ↗** to launch. Backend services (Presentation, Data Collection,
Business) live on the same `*-credit-summary-bs` host per environment; **Gen AI** is the
`credit-summary` Gen AI service.

| Service | 🟢 DEV · CX | 🔵 TE1 · I4 | 🟡 TE2 · R0 | 🔴 PROD · P0 |
|:---|:---:|:---:|:---:|:---:|
| 🖥️ **UI Dashboard** | [Open ↗](https://cx-whs-shared.ubsdev.net/app/KUA/credit-summary-dashboard/) | [Open ↗](https://i4-whs-shared.ubstest.net/app/KUA/credit-summary-dashboard/) | [Open ↗](https://i4-whs-shared.ubstest.net/app/KUA/credit-summary-dashboard/) | 🔧 _TBD_ |
| 🤖 **Gen AI Swagger** | [Open ↗](https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/K16/1/credit-summary/docs) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/K16/1/credit-summary/docs) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/K16/1/credit-summary/docs) | 🔧 _TBD_ |
| 📤 **Presentation Swagger** | [Open ↗](https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/presentation/swagger-ui/index.html) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/presentation/swagger-ui/index.html) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/presentation/swagger-ui/index.html) | 🔧 _TBD_ |
| 📥 **Data Collection Swagger** | [Open ↗](https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/data-collection/swagger-ui/index.html) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/data-collection/swagger-ui/index.html) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/data-collection/swagger-ui/index.html) | 🔧 _TBD_ |
| ⚙️ **Business Service Swagger** | [Open ↗](https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/business/swagger-ui/index.html) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/business/swagger-ui/index.html) | [Open ↗](https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/business/swagger-ui/index.html) | 🔧 _TBD_ |

> ⚠️ **Verify TE2 (R0):** in the source data the TE2 UI and Swagger links are **identical to TE1 (I4)**
> (`i4-whs-shared` / `at60723-test`). R0 almost certainly has its own hostnames — please confirm and
> update this column. (Splunk/Grafana below correctly reference R0 resources: `at60724-preprod`, index `…-r0`.)

---

## 📊 Observability

### 🔴 Splunk

| Env | HEC endpoint | Index |
|:---|:---|:---|
| 🟢 DEV · CX | `https://hec.wmch-weu-dev.splunk.azure.ubs.net:8088` | `wmpc-aa50102` |
| 🔵 TE1 · I4 | `https://hec.wmch-weu-sit.splunk.azure.ubs.net:8088` | `wmpc-aa50102` |
| 🟡 TE2 · R0 | `https://hec.wmch-nch-uat.splunk.azure.ubs.net:8088` | `wmpc-aa50102-r0` |
| 🔴 PROD · P0 | `https://hec.wmch-nch-prd.splunk.azure.ubs.net:8088` | `wmpc-aa50102-p0` |

#### 🔍 Sample search — template

Paste into the Splunk search bar and swap the `<placeholders>`:

```spl
index="<INDEX>"                         ⟵ pick from the table above (env)
  namespace="<NAMESPACE>"               ⟵ see Namespaces reference below
  service="<SERVICE>"                   ⟵ presentation | data-collection | credit-summary | business
  level="<LEVEL>"                       ⟵ optional: ERROR | WARN | INFO
  "<free-text / correlationId>"         ⟵ optional keyword, e.g. an account id or trace id
earliest=-1h latest=now
| sort - _time
```

> ⚠️ **Placeholder query — confirm field names.** The field names above (`service`, `namespace`,
> `level`) are the expected shape; adjust them to match how CFBL logs are actually indexed in your
> `sourcetype`. If unsure, start with a bare keyword search: `index="wmpc-aa50102" "presentation"`.

#### 🎛️ Parameters you can pass

| Parameter | What it filters | Example values |
|:---|:---|:---|
| `index` | **Environment** (required) | `wmpc-aa50102` (DEV/TE1) · `wmpc-aa50102-r0` (TE2) · `wmpc-aa50102-p0` (PROD) |
| `service` | **Which microservice** | `presentation` · `data-collection` · `credit-summary` (Gen AI) · `business` |
| `namespace` | Kubernetes namespace (per env) | see [Namespaces reference](#-reference--kubernetes-namespaces) |
| `level` | Log severity | `ERROR` · `WARN` · `INFO` |
| `earliest` / `latest` | Time window | `-15m` · `-1h` · `-24h@h` · `now` |
| `correlationId` / `requestId` | Trace a single request end-to-end | `<uuid>` |

#### 📋 Ready-to-use examples

```spl
# Presentation service — errors in the last hour (DEV)
index="wmpc-aa50102" service="presentation" level="ERROR" earliest=-1h | sort - _time

# Data Collection service — all logs (TE2 / UAT)
index="wmpc-aa50102-r0" service="data-collection" earliest=-1h

# Gen AI service — follow one request by correlation id (PROD)
index="wmpc-aa50102-p0" service="credit-summary" "<correlationId>" earliest=-24h
```

### 📈 Grafana (UK8S)

| Env | Dashboard |
|:---|:---:|
| 🟢 DEV / SIT | [Open ↗](https://cirruspl-uk8score-h2hrakffeuh4gecb.neu.grafana.azure.com/d/uk8s_namespace/uk8s-namespace-view?var-datasource=cirruspl-uk8score&var-cluster=kd416753357neu01&var-namespace=at61078-dev-credit-summary-bs&orgId=1&from=now-1h&to=now&timezone=utc&var-resolution=5m&refresh=30s) |
| 🟡 TE2 / UAT | [Open ↗](https://cirruspl-uk8s-nch-andafgagf7cpaacc.nch.grafana.azure.com/d/uk8s_namespace/uk8s-namespace-view?orgId=1&from=now-1h&to=now&timezone=browser&var-datasource=cirruspl-nch002-rt201-int-uk8score-prep&var-cluster=uk8s-tsshared-nch-rt201-int-u002&var-namespace=at60724-preprod-credit-summary-bs&var-resolution=5m&refresh=30s) |
| 🔴 PROD | [Open ↗](https://cirruspl-uk8s-nch-andafgagf7cpaacc.nch.grafana.azure.com/d/uk8s_namespace/uk8s-namespace-view?orgId=1&from=now-1h&to=now&timezone=browser&var-datasource=cirruspl-nch002-rt201-int-uk8score-prod&var-cluster=$_all&var-namespace=at61187-prod-sore-data-collection&var-resolution=5m&refresh=30s) |
| 🧭 **Dashboard Locator** (Power BI) | [Open ↗](https://app.powerbi.com/groups/me/reports/b981ea2d-4c36-4245-a2ff-5d70f3daafdf/ReportSectionb0be8e449989e4?experience=power-bi) |

### 🟣 AppDynamics

| Env | Controller |
|:---|:---:|
| 🟢🔵 DEV + TE1 (CX, I4) | [Open ↗](https://appdynamics-wmpc-tel-nch-a-azure.ubstest.net/controller) |
| 🟡 TE2 (R0) | [Open ↗](https://appdynamics-wmpc-te2-nch-a-azure.ubstest.net/controller) |
| 🔴 PROD (P0) | [Open ↗](https://appdynamics-wmpc-prod-nch-a-azure.ubs.net/controller) |

---

## 🧭 Reference — Kubernetes namespaces

Used in Splunk `namespace=` filters and Grafana `var-namespace`.

| Env | Namespace |
|:---|:---|
| 🟢 DEV · CX | `at61078-dev-credit-summary-bs` |
| 🔵 TE1 · I4 | `at60723-test-credit-summary-bs` |
| 🟡 TE2 · R0 | `at60724-preprod-credit-summary-bs` |
| 🔴 PROD · P0 | _TBC_ — Grafana references `at61187-prod-sore-data-collection`; confirm the correct prod namespace |

---

## 📎 Appendix — raw URLs (copy-paste)

<details>
<summary>🟢 <b>DEV · CX</b></summary>

```text
UI                     https://cx-whs-shared.ubsdev.net/app/KUA/credit-summary-dashboard/
Gen AI Swagger         https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/K16/1/credit-summary/docs
Presentation Swagger   https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/presentation/swagger-ui/index.html#
Data Collection        https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/data-collection/swagger-ui/index.html
Business Service       https://at61078-dev-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/business/swagger-ui/index.html
```
</details>

<details>
<summary>🔵 <b>TE1 · I4</b></summary>

```text
UI                     https://i4-whs-shared.ubstest.net/app/KUA/credit-summary-dashboard/
Gen AI Swagger         https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/K16/1/credit-summary/docs
Presentation Swagger   https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/presentation/swagger-ui/index.html#
Data Collection        https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/data-collection/swagger-ui/index.html
Business Service       https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/business/swagger-ui/index.html
```
</details>

<details>
<summary>🟡 <b>TE2 · R0</b> — ⚠️ mirrors TE1, verify</summary>

```text
UI                     https://i4-whs-shared.ubstest.net/app/KUA/credit-summary-dashboard/
Gen AI Swagger         https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/K16/1/credit-summary/docs
Presentation Swagger   https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/presentation/swagger-ui/index.html#
Data Collection        https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/data-collection/swagger-ui/index.html
Business Service       https://at60723-test-credit-summary-bs-app-v1.uk8s-aks-neu-dev.azpriv-cloud.ubs.net/app/KXT/1/business/swagger-ui/index.html
```
</details>

<details>
<summary>🔴 <b>PROD · P0</b> — 🔧 to be updated</summary>

```text
UI                     TBD
Gen AI Swagger         TBD
Presentation Swagger   TBD
Data Collection        TBD
Business Service       TBD
```
</details>

---

## 🛠️ Maintaining this page

- 🔧 **Fill in PROD (P0)** app links once the environment is live.
- ⚠️ **Confirm TE2 (R0)** UI/Swagger hostnames — they currently duplicate TE1.
- ✅ When you add a service, add a **row** to the Application links matrix and a line to each
  Appendix block.
- 🗓️ Update the **Last updated** date at the top on every change.
- 🔐 These are internal UBS endpoints — keep this page in an access-controlled repo/wiki.
