# Business 360 — Business Requirements Specification

**Project:** Business 360 — Retail & E-commerce Analytics  
**Version:** 1.0  
**Status:** Phase 1 finalized  
**Project level:** Advanced  
**Dataset:** Brazilian E-Commerce Public Dataset by Olist  
**Technology stack:** Python, Pandas, MySQL, Power BI, GitHub  
**Automation:** Scheduled ETL to be implemented after the core project is complete

---

## 1. Executive Summary

Business 360 is an end-to-end business intelligence and analytics project designed to transform historical e-commerce data into actionable business insights.

The project will integrate sales, customers, products, sellers, payments, delivery operations, and customer reviews into a consistent analytical solution.

Python will be used for data profiling, validation, cleaning, and exploratory analysis. MySQL will provide structured storage and advanced SQL analytics. Power BI will deliver interactive dashboards for executive management and operational stakeholders. GitHub will host the project code, documentation, tests, and portfolio deliverables.

The project will prioritize analytical accuracy, reproducibility, appropriate data modeling, transparent KPI definitions, and practical business recommendations.

## 2. Business Problem Statement

E-commerce management needs a unified understanding of historical business performance across multiple operational areas.

Analyzing sales, customer behavior, seller performance, payments, delivery outcomes, and customer reviews independently can obscure relationships and lead to inconsistent reporting.

The project will establish a reliable analytical foundation that enables stakeholders to:

- Evaluate sales performance and historical trends.
- Understand customer purchasing and repeat-order behavior.
- Identify high-performing and underperforming products and sellers.
- Examine regional sales distribution.
- Monitor delivery performance and operational delays.
- Understand payment method usage and installment patterns.
- Investigate relationships between delivery outcomes and customer satisfaction.
- Translate analytical findings into measurable business recommendations.

## 3. Business Objectives

| ID | Objective | Expected outcome |
|---|---|---|
| BO-01 | Analyze sales performance | Reliable sales, order, and growth metrics |
| BO-02 | Understand customer behavior | Customer activity and repeat-purchase analysis |
| BO-03 | Evaluate product performance | Category and product-level comparisons |
| BO-04 | Analyze seller performance | Seller contribution and operational comparisons |
| BO-05 | Evaluate delivery operations | Delivery duration, delays, and order outcomes |
| BO-06 | Analyze payments | Payment methods, values, and installment patterns |
| BO-07 | Understand customer satisfaction | Review-score distributions and associated factors |
| BO-08 | Improve decision-making | Evidence-based recommendations supported by analysis |

## 4. Stakeholders and Business Questions

### Executive management
- How is sales performance changing over time?
- Which categories and regions contribute most to sales?
- Which operational indicators need management attention?

### Sales and category management
- Which product categories generate the highest sales value?
- How concentrated is sales value across products and sellers?
- Which regions show promising sales patterns?

### Customer and marketing teams
- How many distinct customers purchase from the marketplace?
- What percentage of customers place repeat orders?
- How does observed customer purchasing behavior vary over time?

### Seller management
- Which sellers contribute most to sales value and order volume?
- How do seller-level delivery and review indicators differ?
- Are results affected by seller order volume or geographic mix?

### Operations
- What is the typical time between purchase and delivery?
- How often are orders delivered after their estimated dates?
- Which regions or order segments have longer delivery durations?

### Customer experience
- How are review scores distributed?
- Are late deliveries associated with lower review scores?
- Which combinations of operational outcomes and review scores merit further investigation?

### Payments
- Which payment methods are most frequently used?
- How do payment values and installment counts vary?
- How should payment records be reconciled with order and item totals?

## 5. Project Scope

### 5.1 Included

1. Sales performance and trends.
2. Customer purchasing behavior and repeat purchasing.
3. Product and category performance.
4. Seller contribution and performance.
5. Geographic analysis using available location fields.
6. Order status and delivery operations.
7. Payment methods, values, and installments.
8. Customer review scores and their relationship with operational outcomes.
9. Data-quality assessment and validation.
10. Relational database design and analytical SQL.
11. Power BI dimensional modeling, DAX, and interactive reporting.
12. Documentation, testing, business recommendations, and GitHub publication.
13. Final-stage ETL scheduling and execution monitoring.

### 5.2 Excluded from the initial release

- Verified net profit, contribution margin, or product profitability without reliable cost data.
- Advertising attribution, campaign ROI, and customer acquisition cost.
- Live inventory and real-time order tracking.
- Real-time business monitoring.
- Forecasts presented as certain outcomes.
- Causal claims unsupported by an appropriate research design.
- Production-grade enterprise infrastructure unless separately justified.

These exclusions may be reconsidered if suitable additional data becomes available.

## 6. Dataset Requirements

The project will use the Brazilian E-Commerce Public Dataset by Olist, obtained from its published Kaggle dataset page.

Dataset source: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

The published dataset contains approximately 100,000 orders from 2016–2018 and related CSV files.

### Expected subject areas

- Orders
- Order items
- Customers
- Products
- Sellers
- Payments
- Reviews
- Geographic reference data, where available

The exact filenames, row counts, schemas, date ranges, and key relationships must be verified during Phase 2. The published dataset description is not a substitute for inspecting the actual downloaded files.

### Data handling requirements

- Preserve the original downloaded files unchanged.
- Store source files separately from processed data.
- Document data types, primary identifiers, relationships, and missing values.
- Validate duplicate keys, orphan records, date consistency, and financial reconciliations.
- Record exclusions and transformations.
- Avoid publishing sensitive credentials or unnecessary personal data.

## 7. KPI Specification

The following are the approved initial KPI definitions. Final implementation details must be confirmed through data profiling and documented before the metrics are released.

### 7.1 Sales KPIs

**KPI-01: Item Sales Value**

Definition: Sum of the `price` field in the order-items dataset for the selected population.

Rule: Do not multiply item prices by payment rows or review rows. Define treatment of canceled and unavailable orders explicitly in the analytical layer.

**KPI-02: Freight Value**

Definition: Sum of the item-level `freight_value` field for the selected population.

Rule: Report separately from item sales value.

**KPI-03: Total Order Value Proxy**

Definition: Item sales value plus freight value.

Rule: Label this metric as an order-value proxy based on available fields, not as verified customer payment receipts or recognized accounting revenue.

**KPI-04: Total Orders**

Definition: Count of distinct qualifying `order_id` values.

Rule: Publish the qualifying order-status policy and use it consistently across comparisons.

**KPI-05: Average Order Value**

Definition: Item sales value divided by the number of distinct qualifying orders.

Rule: Use the same order population for numerator and denominator. State whether canceled or unavailable orders are excluded.

**KPI-06: Monthly Sales Growth**

Definition: `(Current month item sales value - Previous month item sales value) / Previous month item sales value × 100`.

Rule: Use a consistent purchase-date calendar. Return blank or not applicable when the prior month's sales value is zero or unavailable.

### 7.2 Customer KPIs

**KPI-07: Unique Customers**

Definition: Count of distinct `customer_unique_id` values among the qualifying customer population.

Rule: Distinguish a unique customer identity from `customer_id`, which identifies a customer record associated with an order.

**KPI-08: Repeat-Customer Rate**

Definition: Number of unique customers with more than one qualifying distinct order divided by the number of unique customers with at least one qualifying order, multiplied by 100.

Rule: Count distinct orders, define order-status exclusions, and calculate repeat behavior over the observed dataset period. Do not interpret the result as a lifetime retention rate.

**KPI-09: Customer Sales Value**

Definition: Sum of item sales value attributed to a customer across qualifying orders.

Rule: Treat this as observed historical sales value, not profit or predictive lifetime value. Document the attribution key and observation period.

### 7.3 Product and Seller KPIs

**KPI-10: Category Sales Value**

Definition: Sum of item sales value grouped by product category.

Rule: Document how missing category values are handled. Keep product-level sales at the order-item grain.

**KPI-11: Seller Sales Value**

Definition: Sum of item sales value attributed to each seller through the order-items relationship.

Rule: Avoid counting a multi-item order as a single seller transaction when its items belong to different sellers.

**KPI-12: Seller Order Count**

Definition: Number of distinct qualifying orders associated with a seller.

Rule: This is a seller-order relationship count. Summing it across sellers may exceed the marketplace's total order count because one order can involve multiple sellers.

### 7.4 Operations KPIs

**KPI-13: Average Delivery Duration**

Definition: Average elapsed time between purchase timestamp and customer delivery timestamp for valid delivered orders.

Rule: Exclude missing or invalid timestamps, define the unit of measurement, and investigate impossible or negative durations.

**KPI-14: Late-Delivery Rate**

Definition: Number of delivered orders whose actual delivery timestamp is later than the estimated delivery timestamp divided by delivered orders with valid actual and estimated delivery timestamps, multiplied by 100.

Rule: Use strict timestamp comparison and exclude orders without sufficient dates from both numerator and denominator. Report excluded-date counts separately.

**KPI-15: Order Status Distribution**

Definition: Count and percentage of distinct orders grouped by the observed order-status field.

Rule: Preserve source statuses and document any grouping into higher-level categories.

### 7.5 Payments KPIs

**KPI-16: Payment Value by Method**

Definition: Sum of payment-record values grouped by payment type.

Rule: The payment table may contain multiple records for one order. Use payment records for payment analysis and validate payment totals against item and freight values before making reconciliation claims.

**KPI-17: Average Installments**

Definition: Average installment count across the defined payment-record population.

Rule: Document the population and treatment of unusual or missing values. Do not confuse installment count with payment value.

### 7.6 Customer Satisfaction KPIs

**KPI-18: Average Review Score**

Definition: Arithmetic mean of valid numeric review scores within the selected review population.

Rule: Document duplicate review records, missing scores, and whether the unit of analysis is a review or an order.

**KPI-19: Review Score Distribution**

Definition: Count and percentage of valid reviews by score.

Rule: Use the observed score scale, retain valid individual scores, and document missing or invalid values.

**KPI-20: Delivery and Review Association**

Definition: Compare review-score distributions or averages across clearly defined delivery-performance groups.

Rule: This is descriptive association analysis. It must not be presented as proof that delivery performance causes a particular review score.

## 8. Data Modeling and Integrity Requirements

The solution must preserve the correct grain of each business entity.

| Entity | Expected grain |
|---|---|
| Orders | One row per order |
| Order items | One row per order-item record |
| Customers | One row per customer record or documented unique-customer dimension |
| Sellers | One row per seller |
| Products | One row per product |
| Payments | One row per payment record |
| Reviews | One row per review record, subject to source-key validation |

The relational model and Power BI model must account for one-to-many relationships.

### Required safeguards

- Do not join order items, payments, and reviews in a way that multiplies financial amounts.
- Use distinct order counts where required.
- Validate key uniqueness and referential integrity.
- Document the treatment of missing dimension values.
- Reconcile totals across source, staging, and reporting layers.
- Establish an appropriate date dimension and consistent date relationships in Power BI.
- Document any bridge tables or alternate relationship paths required by the source structure.

## 9. Technical Requirements

### Python
- Profile source schemas and data quality.
- Validate types, keys, nulls, duplicates, dates, and numeric fields.
- Implement reproducible cleaning and transformation logic.
- Conduct exploratory analysis and record findings.
- Add automated data-quality tests.

### MySQL
- Create a documented database schema.
- Separate staging and analytical layers where appropriate.
- Implement primary and foreign key constraints where valid.
- Develop advanced analytical SQL using joins, CTEs, window functions, and aggregations.
- Validate query results against independently calculated control totals.

### Power BI
The report will contain multiple pages:

1. **Executive Overview:** sales value, orders, average order value, trends, and high-level performance.
2. **Sales Analysis:** monthly trends, growth, and geographic performance.
3. **Customer Analysis:** unique customers, repeat-customer rate, and observed customer sales value.
4. **Product Analysis:** category and product performance.
5. **Seller Analysis:** seller contribution and comparative indicators.
6. **Operations Analysis:** order status, delivery duration, and late-delivery rate.
7. **Payments Analysis:** payment types, payment values, and installments.
8. **Customer Satisfaction:** review distributions and delivery-review associations.

All pages must use consistent definitions, meaningful filters, readable visuals, and documented DAX measures. The exact page design may evolve after the data model is validated.

### GitHub
The repository will include:
- Project README.
- Business requirements specification.
- Data dictionary.
- Python scripts and notebooks.
- SQL scripts.
- Data-quality tests.
- Power BI screenshots and documentation.
- Business findings and recommendations.
- Dependency and setup instructions.
- `.gitignore` rules for local data, credentials, logs, and generated artifacts that should not be committed.

### Final-stage automation
A scheduled ETL job will be implemented only after the core pipeline has been built and tested. It must include execution logging, failure reporting, data validation, and safe rerun behavior.

## 10. Assumptions and Constraints

1. The published Olist dataset is historical and may not represent current marketplace performance.
2. Source records may contain missing values, duplicate keys, incomplete relationships, or inconsistent timestamps.
3. The dataset is not a complete accounting ledger.
4. Item sales value is not equivalent to net profit.
5. Payment records, order records, and order-item records have different grains.
6. Customer identities and customer records are not interchangeable.
7. The observed period limits repeat-purchase interpretation.
8. Review records may not map one-to-one to orders without further validation.
9. Delivery comparisons require valid and comparable timestamps.
10. The analysis is descriptive unless a suitable causal method and supporting data are introduced.
11. Data availability, machine resources, database version, and Power BI licensing may affect implementation choices.
12. Dataset usage, attribution, and redistribution must follow the applicable published terms.

## 11. Acceptance Criteria for Phase 1

Phase 1 is complete when all of the following conditions are met.

- [ ] Business problem and project objectives are documented.
- [ ] Stakeholders and their analytical questions are defined.
- [ ] Included and excluded scope is explicit.
- [ ] The dataset source is identified and documented.
- [ ] Initial KPI formulas and population rules are documented.
- [ ] Data-grain and join-multiplication risks are documented.
- [ ] Business assumptions and limitations are recorded.
- [ ] Required technology stack and deliverables are specified.
- [ ] Power BI report pages are defined at a high level.
- [ ] Data-quality and reconciliation requirements are specified.
- [ ] Final-stage automation is included in the roadmap, not treated as a prerequisite.
- [ ] The requirements document is saved in `docs/business_requirements.md`.

## 12. Definition of Done for the Overall Project

The complete Business 360 project will be considered ready for portfolio presentation when:

- Data transformations are reproducible and tested.
- Critical data-quality checks pass or documented exceptions are accepted.
- Analytical tables and relationships are documented.
- SQL outputs reconcile with Python control totals.
- Power BI KPIs match the approved definitions.
- All report pages have been validated for filters and totals.
- Business recommendations are supported by calculated evidence.
- Setup and execution instructions are documented.
- The GitHub repository is organized and contains no exposed credentials.
- The scheduled pipeline has been tested for success, failure, and safe reruns.

## 13. Phase 1 Sign-Off

**Business scenario:** Retail / E-commerce  
**Analytical scope:** Full Business 360  
**Project approach:** Advanced, portfolio-oriented, quality-first  
**Primary tools:** Python, MySQL, Power BI, GitHub  
**Dataset:** Brazilian E-Commerce Public Dataset by Olist  
**Next phase:** Dataset acquisition verification, profiling, schema inspection, and data-quality assessment.

**Approval status:** Requirements baseline finalized. Actual data characteristics and implementation details remain subject to validation in Phase 2.
