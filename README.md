# Where Is Inventory Actually Getting Stuck? Smart Inventory Analytics

An inventory and fulfillment performance analysis across four product categories and four
regions, investigating whether order delays are a sourcing problem or a fulfillment problem,
and whether regional cost differences are actually explained by inventory volume.

## Business Problem

A multi-category, multi-region distribution operation wants to understand two things: (1) where
order fulfillment is breaking down, whether it's supplier lead time or something inside the
warehouse/fulfillment process, and (2) whether regional transportation cost differences are
driven by how much inventory each region holds, or by something else (route, distance, shipment
frequency). Getting this wrong means optimizing the wrong lever, for example renegotiating
supplier contracts when the real problem is warehouse throughput.

## Dataset Source

Inventory and Supply Chain dataset, category-level and region-level records covering inventory
levels, lead time, order status, transportation cost, warehouse utilization, and units sold across
Clothing, Electronics, Furniture, and Accessories in the East, North, South, and West regions.
Public dataset, used here for independent analysis and modeling practice.

## Approach

- Profiled the flat source table in SQL first, checking row counts, nulls, and category/region
  cardinality before building any Power BI measures, to confirm the data was clean enough to
  trust at face value.
- Built KPI cards and breakdown visuals in Power BI: Warehouse Utilization Rate, Inventory
  Turnover Ratio, Time for Inventory Sales, lead time by category, order status distribution,
  transportation cost by region/category, and inventory level by category/region.
- Cross-validated the key findings below with standalone SQL queries (check sql_queries file)
  rather than relying on dashboard visuals alone.

## Key Findings

**1. Lead time barely varies by category, so fulfillment delays are more likely an internal
process issue than a supplier issue.** Average lead time ranges from 15.29 days (Clothing) to
16.60 days (Accessories), a spread of under 1.5 days across all four categories. If supplier-side
sourcing were driving delays, categories with more complex supply chains would show noticeably
longer lead times than simpler ones. They don't. That points investigation toward internal
fulfillment capacity (warehouse processing, pick/pack throughput, order prioritization) rather
than procurement.

**2. Roughly 30% of all orders are not landing in "Fulfilled" status, a bigger lever than any
single region or category.** Of all order records, 69.8% are Fulfilled, 20.7% Pending, and 9.5%
Canceled. Before optimizing transportation cost by region or inventory by category, the more
urgent priority is understanding why one in five orders sits in Pending. Fixing that would move
overall performance more than any single regional or category-level tweak.

**3. Regional transportation cost differences don't line up cleanly with regional inventory
volume, suggesting cost is driven more by route or shipment frequency than by stock levels.**
Inventory holdings are fairly evenly distributed across East, North, South, and West within each
category, with no single region consistently holding the most stock. Yet East and West show the
highest transportation costs. If cost were purely a function of how much inventory a region
carries, the highest-inventory region would also show the highest cost. That alignment isn't
present here, which is a reason to investigate route/logistics factors before assuming inventory
volume is the driver.

## Business Recommendations

- **Prioritize a fulfillment-process review over supplier renegotiation.** The flat lead time
  across categories suggests the roughly 30% of orders stuck in Pending/Canceled is more likely
  explained by internal capacity constraints than supplier delays. Start there.
- **Treat the Pending-order share as the primary KPI to move**, ahead of category- or
  region-specific optimizations, since it represents the largest single share of unresolved
  volume in the dataset.
- **Investigate route and shipment-frequency data for East and West** before assuming
  transportation cost is inventory-driven. Since inventory levels don't clearly explain the cost
  gap, the fix may be in logistics routing rather than warehouse stocking policy.

## Tools Used

Power BI (KPI dashboard, DAX measures), SQL (data profiling, validation queries)
