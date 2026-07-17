# Backoffice Page-Level PRDs Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Produce 22 detailed, independently reviewable page-level PRDs and one linked index for the commercial office leasing platform backoffice.

**Architecture:** The output is organized by the existing seven first-level navigation groups. A shared index fixes terminology, roles, public rules, statuses, and screenshot-derived capability mappings; every page PRD then applies the same 16-section contract while defining module-specific fields, flows, permissions, exceptions, data rules, and acceptance criteria.

**Tech Stack:** Markdown, POSIX shell validation, `rg`, `find`, `wc`, Git.

## Global Constraints

- Preserve the existing seven first-level and 22 second-level navigation items.
- Create exactly one PRD per second-level navigation page; child detail, edit, review, drawer, and modal views remain inside the owning PRD.
- Keep the existing master PRD and combined second-level PRD unchanged.
- Use the four screenshots in `payload-office-platform/public/images/` as evidence for front-office capabilities.
- Do not copy competitor branding, advertising modules, visual styling, or unsupported transaction capabilities.
- Every PRD must contain all 16 standard sections; an inapplicable section must explain why.
- Use Beijing time; store prices with currency, period, and unit; use square meters for area; use immutable internal IDs.
- Export must inherit filters and data scope, mask sensitive data by role, and create an audit log.
- Deletion is logical by default; linked business records cannot be physically deleted.
- Do not add contract, payment, e-signature, settlement, or transaction-closing scope.

## File Map

- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/README.md` — document index, shared terminology, roles, common rules, screenshot traceability.
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/01_工作台/*.md` — two workbench PRDs.
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/02_楼盘管理/*.md` — two building-management PRDs.
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理/*.md` — three inventory-management PRDs.
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索/*.md` — four lead-management PRDs.
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/05_商户经纪/*.md` — three merchant/broker PRDs.
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/06_数据看板/*.md` — three analytics PRDs.
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置/*.md` — five system-setting PRDs.
- Reference only: `payload-office-platform/public/prd/商办租赁平台后台管理系统_MVP_PRD.md` — authoritative information architecture and MVP boundary.
- Reference only: `payload-office-platform/public/prd/商办租赁平台后台管理系统_MVP_二级页面PRD.md` — prior field and acceptance inventory to deepen.
- Reference only: `docs/superpowers/specs/2026-07-17-backoffice-page-prd-split-design.md` — approved structure and quality contract.

---

### Task 1: Establish Shared Index and Page Contract

**Files:**
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/README.md`

**Interfaces:**
- Consumes: Existing navigation, roles, scope, and approved design specification.
- Produces: Canonical terminology, role names, shared status definitions, common product rules, screenshot traceability, and relative links that all 22 PRDs must use.

- [ ] **Step 1: Extract the authoritative navigation and shared terms**

Run:

```bash
rg -n '^## 3\.|^### 3\.|角色|权限|MVP|楼盘|房源|线索|商户|经纪人' \
  payload-office-platform/public/prd/商办租赁平台后台管理系统_MVP_PRD.md \
  payload-office-platform/public/prd/商办租赁平台后台管理系统_MVP_二级页面PRD.md
```

Expected: Output identifies all seven first-level groups, 22 second-level pages, core roles, and MVP boundaries.

- [ ] **Step 2: Create the index with exact shared definitions**

Write `README.md` with these sections: document purpose and version; 22-page linked directory; role and data-scope matrix; canonical definitions for building, listing, merchant, broker, customer, lead, rental, sale, direct listing, publish status, and review status; shared time/price/area/ID/delete/export/batch/concurrency rules; four-screenshot capability matrix; excluded scope; reading and review guidance.

- [ ] **Step 3: Verify the index contains 22 Markdown links and no placeholders**

Run:

```bash
test "$(rg -o '\]\([^)]*_PRD\.md\)' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/README.md | wc -l | tr -d ' ')" = "22"
! rg -n 'TBD|TODO|待定|稍后补充|另行确认' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/README.md
```

Expected: Both commands exit 0.

- [ ] **Step 4: Commit the shared contract**

```bash
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/README.md
git commit -m "docs: add backoffice page PRD index"
```

### Task 2: Building and Listing Management PRDs

**Files:**
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/02_楼盘管理/01_楼盘列表_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/02_楼盘管理/02_商圈配置_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理/01_房源列表_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理/02_房源审核_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理/03_房源举报_PRD.md`

**Interfaces:**
- Consumes: Canonical terms and rules from `README.md`; four screenshots; existing building and listing fields.
- Produces: Stable building/listing data definitions, publish/review flows, front-office display mapping, and location/tag dictionaries used by leads, dashboards, and settings.

- [ ] **Step 1: Record screenshot-derived building and listing requirements**

Cover: district/business-area/subway location; rental/sale type; building type; area and price ranges; decoration; photos and ordered galleries; address and coordinates; rentable/saleable counts; registration/direct/verified tags; rental unit and total price; consultant ownership; introduction; amenities; nearby POIs; recommended buildings; listing sorting and publishing.

- [ ] **Step 2: Write the two building PRDs with the 16-section contract**

`01_楼盘列表_PRD.md` must include list, create/edit/detail child views, duplicate detection, gallery management, map coordinates, front-office preview, publish controls, consultant ownership, aggregate listing values, permissions, concurrency, import/export, and testable acceptance criteria.

`02_商圈配置_PRD.md` must include city/district/business-area/subway hierarchy, map boundary or center point, front-office visibility, sorting, enable/disable effects, linked-building safeguards, and audit requirements.

- [ ] **Step 3: Write the three listing PRDs with the 16-section contract**

`01_房源列表_PRD.md` must distinguish building from concrete rentable/saleable units and define rent/sale type, area, unit price, total monthly rent or sale price, decoration, capacity, registration, direct listing, images, floor, availability, owner, publish state, and batch operations.

`02_房源审核_PRD.md` must define submissions, before/after comparison, sensitive-field checks, duplicate/media/price validation, approve/reject/request-change transitions, reason templates, notifications, SLA, and immutable review history.

`03_房源举报_PRD.md` must define report source/type/evidence, triage, duplicate merging, listing suspension, investigation, valid/invalid closure, restoration, reporter privacy, notification, and audit trail.

- [ ] **Step 4: Verify all five files contain every standard section**

Run:

```bash
for file in \
  payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/02_楼盘管理/*_PRD.md \
  payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理/*_PRD.md; do
  count=$(rg '^## ([1-9]|1[0-6])\.' "$file" | wc -l | tr -d ' ')
  test "$count" = "16" || { echo "$file: $count sections"; exit 1; }
done
! rg -n 'TBD|TODO|待定|稍后补充|另行确认' \
  payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/02_楼盘管理 \
  payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理
```

Expected: Both checks exit 0 with no output.

- [ ] **Step 5: Commit the core inventory PRDs**

```bash
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/02_楼盘管理 \
        payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理
git commit -m "docs: add building and listing page PRDs"
```

### Task 3: Lead Management PRDs

**Files:**
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索/01_线索池_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索/02_我的客户_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索/03_公海客户_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索/04_跟进记录_PRD.md`

**Interfaces:**
- Consumes: Front-office inquiry sources, building/listing IDs, consultant ownership, shared privacy and export rules.
- Produces: Lead/customer lifecycle, assignment and recovery rules, follow-up model, and conversion definitions used by workbench and analytics.

- [ ] **Step 1: Define one canonical lead lifecycle across all four documents**

Use: new → pending assignment → following → valid opportunity → viewing → negotiating → converted/lost; allow public-pool transfer from eligible non-terminal states. Define deduplication by normalized phone plus configurable time window, with manual merge preserving sources and history.

- [ ] **Step 2: Write the lead pool and customer ownership PRDs**

`01_线索池_PRD.md` covers appointment, entrusted search, phone consultation, and other source intake; deduplication; assignment; rejection; bulk assignment; SLA; masking; building/listing attribution; and source parameters.

`02_我的客户_PRD.md` covers customer profile, requirements, preferred location/area/budget, related buildings/listings, follow-up timeline, viewing records, stage changes, next action, transfer, lost reason, and conversion.

- [ ] **Step 3: Write the public pool and follow-up PRDs**

`03_公海客户_PRD.md` covers entry triggers, protection period, claim eligibility, claim limits, conflict resolution, manager assignment, recovery, and full history retention.

`04_跟进记录_PRD.md` covers call/WeChat/visit/viewing/other channels, content and attachments, next-follow-up time, immutable author/time, correction workflow, privacy, filters, exports, and linked customer/building/listing.

- [ ] **Step 4: Cross-check status and metric consistency**

Run:

```bash
rg -n '待分配|跟进中|有效商机|带看|谈判|已转化|已流失|公海' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索
```

Expected: Each state is defined consistently; no document invents a conflicting terminal or active state.

- [ ] **Step 5: Commit the lead PRDs**

```bash
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索
git commit -m "docs: add lead management page PRDs"
```

### Task 4: Merchant, Broker, and Team PRDs

**Files:**
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/05_商户经纪/01_商户管理_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/05_商户经纪/02_经纪人管理_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/05_商户经纪/03_团队管理_PRD.md`

**Interfaces:**
- Consumes: Consultant ownership and front-office contact display requirements; lead assignment and data-scope rules.
- Produces: Merchant/broker/team hierarchy, verification and enable states, display eligibility, and assignment capacity used by listings, leads, and role permissions.

- [ ] **Step 1: Define merchant, broker, and team boundaries**

Merchant is the contracted organization; broker is a verified person under one merchant; team is an internal assignment and data-scope grouping. A front-office consultant must be an enabled, verified broker with contact-display permission and an active building/listing relationship.

- [ ] **Step 2: Write all three PRDs using the 16-section contract**

Merchant management covers qualification files, verification, service cities, status, linked brokers/listings, suspension effects, and audit.

Broker management covers identity and employment verification, contact masking, specialties, service areas, online/display state, capacity, transfer on disable, and QR/contact-display controls.

Team management covers hierarchy, leader, members, service scope, lead allocation, data scope, member transfer, deletion safeguards, and aggregate performance links.

- [ ] **Step 3: Verify relationship and disable-effect coverage**

Run:

```bash
rg -n '停用影响|关联楼盘|关联房源|线索分配|前台展示|数据权限|转移' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/05_商户经纪
```

Expected: All three PRDs explicitly define relationship changes and downstream effects.

- [ ] **Step 4: Commit the merchant and broker PRDs**

```bash
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/05_商户经纪
git commit -m "docs: add merchant and broker page PRDs"
```

### Task 5: Workbench PRDs

**Files:**
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/01_工作台/01_我的待办_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/01_工作台/02_数据概览_PRD.md`

**Interfaces:**
- Consumes: Review, report, lead SLA, follow-up, merchant qualification, and publish states from Tasks 2–4.
- Produces: Role-aware task aggregation and summary definitions.

- [ ] **Step 1: Write My Tasks PRD**

Define task sources, assignee, priority, due time, overdue rule, unread status, deduplication, list filters, deep links, processing result, transfer, batch handling limits, permission filtering, refresh, and acceptance cases for stale/deleted targets.

- [ ] **Step 2: Write Data Overview PRD**

Define role-specific cards for buildings, listings, pending reviews, reports, new leads, following customers, upcoming follow-ups, overdue tasks, and conversion; include time range, comparison period, formulas, unique-count keys, refresh frequency, drill-down filters, empty/no-permission states, and card visibility.

- [ ] **Step 3: Verify every task and card maps to an authoritative source**

Run:

```bash
rg -n '来源|口径|去重|刷新|下钻|超时|失效|无权限' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/01_工作台
```

Expected: Both PRDs include all listed concepts with concrete rules.

- [ ] **Step 4: Commit the workbench PRDs**

```bash
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/01_工作台
git commit -m "docs: add workbench page PRDs"
```

### Task 6: Analytics PRDs

**Files:**
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/06_数据看板/01_经营概览_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/06_数据看板/02_房源分析_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/06_数据看板/03_线索分析_PRD.md`

**Interfaces:**
- Consumes: Canonical entity IDs, listing states, lead lifecycle, merchant/broker/team hierarchy, and conversion events.
- Produces: Stable KPI formulas, dimensions, refresh behavior, and drill-down contracts.

- [ ] **Step 1: Create a metric dictionary inside each PRD**

For every metric specify name, business definition, formula numerator/denominator, unique key, included/excluded statuses, event time field, timezone, dimensions, refresh frequency, and drill-down target.

- [ ] **Step 2: Write the three analytics PRDs**

Operating overview covers supply, active inventory, new/valid leads, viewings, conversions, conversion rate, merchant/broker activity, trends, comparison, ranking, and permission scope.

Listing analysis covers building/listing supply, publish rate, review pass rate, available area, price distribution, decoration/type/tag/location distributions, stale inventory, and front-office exposure/inquiry attribution if event data exists.

Lead analysis covers sources, assignment SLA, validity rate, stage funnel, follow-up timeliness, viewing rate, conversion rate, loss reasons, broker/team performance, and source-to-building/listing attribution.

- [ ] **Step 3: Verify metric definition completeness**

Run:

```bash
for file in payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/06_数据看板/*_PRD.md; do
  for field in 业务定义 计算公式 去重键 时间字段 刷新频率 下钻; do
    rg -q "$field" "$file" || { echo "$file: missing $field"; exit 1; }
  done
done
```

Expected: Exit 0 with no output; manual review confirms every metric row uses all six fields and Beijing time.

- [ ] **Step 4: Commit the analytics PRDs**

```bash
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/06_数据看板
git commit -m "docs: add analytics page PRDs"
```

### Task 7: System Setting PRDs

**Files:**
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置/01_账号管理_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置/02_角色权限_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置/03_城市区域_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置/04_字典配置_PRD.md`
- Create: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置/05_操作日志_PRD.md`

**Interfaces:**
- Consumes: Roles, data scopes, location hierarchy, dictionaries, sensitive operations, and audit events used in all prior PRDs.
- Produces: Administrative controls that make prior permissions, enums, regions, and logs executable.

- [ ] **Step 1: Write account and permission PRDs**

Account management covers invite/create, identity fields, merchant/team binding, role assignment, city scope, enable/disable, password reset, session invalidation, sensitive-field access, lockout, and disable effects.

Role permissions covers system and custom roles, menu/action/field/data permissions, scope inheritance, conflict resolution, permission preview, copy, enable/disable, affected-user count, and high-risk confirmation.

- [ ] **Step 2: Write region and dictionary PRDs**

City/region covers city, district, business area, subway line/station, hierarchy, codes, coordinates, sorting, front-office visibility, linked-data safeguards, and disable effects.

Dictionary configuration covers building type, decoration, listing tags, registration, direct/verified labels, lead source/stage/loss reason, follow-up type, report type/result, and rejection reason; define immutable code, editable label, sorting, visibility, disable safeguards, and usage count.

- [ ] **Step 3: Write operation log PRD**

Cover actor, role, organization/team, action, object type/ID, before/after values with masking, source IP/device, request ID, result, failure reason, Beijing timestamp, filters, detail, export, retention, immutability, access permission, and links to valid business objects.

- [ ] **Step 4: Verify every shared dependency has an owning settings page**

Run:

```bash
rg -n '菜单权限|操作权限|字段权限|数据权限|城市|行政区|商圈|地铁|字典编码|使用数量|变更前|变更后|请求ID' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置
```

Expected: All shared control concepts have explicit ownership and rules.

- [ ] **Step 5: Commit the system-setting PRDs**

```bash
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/07_系统设置
git commit -m "docs: add system setting page PRDs"
```

### Task 8: Full Quality and Traceability Verification

**Files:**
- Modify: `payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/README.md` only if links or shared definitions need correction.
- Modify: Any page PRD that fails the checks below.

**Interfaces:**
- Consumes: All 23 generated Markdown files.
- Produces: Verified final document set with complete links, consistent terminology, screenshot coverage, and no unsupported scope.

- [ ] **Step 1: Verify exact file count and inventory**

Run:

```bash
test "$(find payload-office-platform/public/prd/后台管理系统_MVP_页面PRD -type f -name '*.md' | wc -l | tr -d ' ')" = "23"
test "$(find payload-office-platform/public/prd/后台管理系统_MVP_页面PRD -type f -name '*_PRD.md' | wc -l | tr -d ' ')" = "22"
```

Expected: Both commands exit 0.

- [ ] **Step 2: Verify all 22 PRDs contain all standard sections**

Run:

```bash
for file in $(find payload-office-platform/public/prd/后台管理系统_MVP_页面PRD -type f -name '*_PRD.md'); do
  count=$(rg '^## ([1-9]|1[0-6])\.' "$file" | wc -l | tr -d ' ')
  test "$count" = "16" || { echo "$file: $count sections"; exit 1; }
done
```

Expected: Exit 0 with no output.

- [ ] **Step 3: Verify index links resolve**

Run from the index directory:

```bash
cd payload-office-platform/public/prd/后台管理系统_MVP_页面PRD
for target in $(sed -n 's/.*](\([^)]*_PRD\.md\)).*/\1/p' README.md); do
  test -f "$target" || { echo "broken link: $target"; exit 1; }
done
```

Expected: Exit 0 with no output.

- [ ] **Step 4: Scan placeholders, terminology conflicts, and forbidden scope**

Run:

```bash
! rg -n 'TBD|TODO|待定|稍后补充|另行确认|视情况而定' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD
! rg -n '支持在线签约|提供在线支付|接入电子签章|执行佣金结算|执行交易结算' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD
```

Expected: Both commands exit 0. Excluded capabilities may appear only as explicit non-goals.

- [ ] **Step 5: Verify screenshot capability traceability**

Run:

```bash
rg -l '阿里商办列表页|阿里商办详情页|58 商办列表页|58 商办详情页' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD | wc -l
rg -n '区域|商圈|地铁|面积|单价|装修|租赁|出售|可注册|直租|相册|顾问|周边配套|推荐楼盘|预约看房|委托找房' payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/README.md payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/02_楼盘管理 payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/03_房源管理 payload-office-platform/public/prd/后台管理系统_MVP_页面PRD/04_客户线索
```

Expected: The first command returns at least `5`; the second shows each capability in the index and at least one owning PRD.

- [ ] **Step 6: Inspect working tree and commit verification corrections**

```bash
git diff --check
git status --short -- payload-office-platform/public/prd/后台管理系统_MVP_页面PRD
git add payload-office-platform/public/prd/后台管理系统_MVP_页面PRD
git commit -m "docs: verify backoffice page PRD set"
```

Expected: `git diff --check` exits 0; commit contains only final PRD corrections or index updates.
