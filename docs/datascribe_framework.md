# **Datascribe: Philosophy and Documentation Standards**
## Manifesto

LLMs open a new chapter for data documentation—for two key reasons:
1. They can generate far more documentation, much faster than humans.
2. They can use that documentation to write accurate text-to-SQL queries.

With this expanded capacity for both creation and consumption, documentation can be richer than ever. Imagine an essay detailing the caveats of using a particular column to calculate revenue—versus a traditional data dictionary entry: INT, Revenue in dollars, nullable.

This level of detail isn't just possible now—it's valuable. The more LLMs rely on documentation to interpret and query databases, the more critical that documentation becomes. Datascribe exists to generate data documentation for _accurate_ LLM-based data querying.

### A New Standard for Data Documentation
Data documentation is complete when an LLM could use it to answer any answerable question from a relational database.

## **Overview**

In real-world enterprise databases, **raw schemas** often fail to fully describe the true meaning, reliability, and correct use of the data. Column labels can be misleading, redundant, or innaccurate. Critical transformations can be undocumented. And necessary context for accurate calculations can only be known through human conversations.

Clarity about the data's correct interpretation is necessary for **LLM-based text-to-SQL tools** to work reliably.

**Datascribe** exists to create **rich documentation** that gives LLMs everything they need to understand real-world data. It does so by capturing not just the schema but the **provenance**, **limitations**, **relationships**, and **business meaning** of the data.

---

## **Documentation Source Types**
Datascribe distinguishes all pieces of information in the documentation into two types:
- Observed
- Reported

**Observed** information is a fact like the count of non-null records in a column. This information can be observed by the LLM application at any time simply by querying the database.
**Reported** is information that is told to the LLM by a human (or other textual source) _about_ the data. Example: "this table was migrated from a legacy version of the application; some of the records were copied and others are original"

## **How Datascribe Works**

All documentation is organized into a scalable file system:

```
datascribe/
├── _datasets.yml           
├── _<dataset_1>/           
│   ├── <table_1>.yml
│   └── ...
└── ...
```

### **1\. `_datasets.yml`**

* High-level summary of all datasets.

* Includes:

  * **Display name**

  * **Dataset ID**

  * **Brief description**

  * **Key notes** (e.g., migrations, major quality issues)

This file allows an LLM to **quickly shortlist** candidate datasets when trying to answer a user query.

**Example:**

```yaml  
datasets:  
  - id: products  
    name: Products  
    description: Primary product data for Commerce Hub Online's platform post-2021 database migration.  
    notes: "Migrated from legacy system; field inconsistencies noted."
```

---

### **2\. `_<dataset_name>.yml`**

* Full narrative documentation for a specific dataset.

* Structured to include:

  * **Dataset purpose**

  * **Business context**

  * **Known data issues**

  * **Relationships to other datasets**

  * **Field-by-field notes**

  * **Transformation history**

This information equips the LLM to **interpret the data correctly**, **adjust for quirks**, and **write correct SQL**.

**Example Structure:**

```yaml  
dataset:  
  id: products  
  name: Products  
  description: "Products sold through CommerceHubOnline's platform after 2021 migration."  
  business_context: "E-commerce platform core product catalog."  
  known_issues:  
    - "Legacy 'product_id' overlaps with pg_products."  
    - "Inconsistent timestamps for imports."  
  related_datasets:  
    - "pg_products" (legacy source)  
    - "analytics_product_summary" (reporting layer)  
  schema:  
    fields:  
      - name: product_id  
        type: STRING  
        notes: "Unique within table but not globally unique."  
      - name: created_at  
        type: TIMESTAMP  
        notes: "For legacy imports, indicates import time."  
  transformation_history:  
    - "Migrated from legacy version 2021-04."
```

## **Key Datascribe Principles**

* **Enable LLMs to succeed**: Every part of Datascribe exists to empower LLMs to write better queries, not just humans.

* **Narrative over technical**: Tell the *story* of the data — why it looks the way it does, where it came from, and where it is reliable or not.

* **Capture the hidden knowledge**: Anything a human would need to know to query safely should be included.

* **Record uncertainty**: Be explicit about known issues, gaps, and ambiguities.

* **Support memory-efficient reasoning**: Structure files so an LLM can selectively load only what it needs.

---

## **Intended LLM Behavior**

When answering a user's question:

1. **Scan `_datasets.yml`** to identify relevant datasets.

2. **Read specific `_<dataset>.yml` files** to gather necessary details.

3. **Ask clarifying questions** if gaps remain.

4. **Update the documentation** over time with any new discoveries.

Over time, this process creates a **self-improving, expert-level knowledge base** that makes **LLM-based text-to-SQL** tools dramatically more accurate and powerful.
