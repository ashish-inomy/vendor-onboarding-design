# 📘 Brief Summary of Requirements

This system enables automated onboarding of e-commerce vendor websites and scalable extraction of product data using a hybrid architecture of **deterministic rules** and **LLM-assisted logic**.

---

## ✅ Vendor Onboarding Pipeline (High-Level Summary)

1. **Input**  
   - User provides the root URL of a shopping site (e.g., `https://xyz.com`).

2. **Discover Product Listing/Search Page**  
   - System analyzes site structure and identifies how products can be queried.  
   - Stores URL templates, query parameters, pagination rules.

3. **Generate Product Extraction Rules**  
   - Collects sample product pages.  
   - Creates CSS/XPath/regex selectors for product fields (title, price, availability, ID).  
   - Uses LLM only where beneficial (selector generation + rule repair).

4. **Define Unique Identifier Strategy**  
   - Chooses primary unique ID (SKU/GTIN/MPN) and fallback logic.

5. **Build Processing Modules**  
   - Deduplication logic  
   - Price-update detection  
   - Attribute change detection

6. **Validate & Version Rule Set**  
   - Ensures rules work across multiple product pages.

---

## ✅ Product Onboarding Pipeline (High-Level Summary)

1. **Execute Search Query**  
   - Uses stored instructions to load the product listing with a specific user query.

2. **Extract Product URLs**  
   - Applies stored selectors to extract all product page links.

3. **Rule Validation Stage (Pre-Extraction)**  
   - Validates rules against sample product pages.  
   - Auto-repairs rules using LLM if needed.

4. **Extract Product Details**  
   - Scrapes each product page using validated selectors.

5. **Deduplication & Final Processing**  
   - Removes duplicates based on ID strategy.  
   - Applies price/attribute update logic.  
   - Stores cleaned product entities.

---

## 🔗 Documentations
For a full detailed specification, refer to:

- [**Detailed Requirement Document →**](https://github.com/ashish-inomy/vendor-onboarding-design/blob/main/requirements.md)
- [**Architecture Document →**](https://github.com/ashish-inomy/vendor-onboarding-design/blob/main/architecture.md)

---
