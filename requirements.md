# Requirements for Vendor Onboarding & Product Onboarding Pipelines

This document defines a technically precise and production-ready set of requirements for building a scalable, rule-driven, validated product-scraping system that works across arbitrary e-commerce websites.

Legend:  
- 🧠 **LLM-Driven**  
- ⚙️ **Rule-Based / Deterministic**  
- 🌐 **Scraping / Browser**  

---

# 1. Vendor Onboarding Pipeline

## 1.1. Input
- User provides a base URL of a shopping website (e.g., `xyz.com`).

## 1.2. Output
- Product search page access rules ⚙️  
- Product URL extraction rules (LLM-supported generation) 🧠  
- Product detail extraction rules (LLM-supported generation) 🧠  
- Unique identifier strategy ⚙️  
- Deduplication & processing code modules ⚙️  
- Stored rule version in DB ⚙️  

---

## 1.3. Pipeline Steps

### **Step 1: Discover Product Search Page**
- Crawl landing pages 🌐
- LLM suggestion on best candidate for search URL 🧠  
- Heuristic URL detection (`/search`, `/category`, `/products`) ⚙️  
- DOM pattern analysis for repeated product tiles ⚙️  
- Validate search functionality ⚙️  
- Store access instructions ⚙️  

---

### **Step 2: Store Search Page Access Instructions**

- Save **URL templates** with dynamic query placeholders (e.g., `?q={search_query}&page={page}`) ⚙️  
- Capture **Form/POST request metadata**, including:  
  - request method  
  - payload structure  
  - pagination parameters  
  - filter/sort parameters ⚙️  
- Record any required **JavaScript-triggered workflows**, such as:  
  - AJAX/XHR endpoints  
  - infinite scroll data feeds  
  - JS-rendered search results  
  - event-driven navigation steps 🌐  
- Store all **required headers, cookies, and authentication artifacts**  
  needed for consistent access during product ingestion ⚙️  

---

### **Step 3: Store Search Page Parsing Logic**

- Detect the **product container selector** used for each product card in the search page ⚙️
- Extract the details from each product card like URL, title, description.
- Generate **CSS/XPath selectors** or DOM-extractor rules to reliably parse product card fields ⚙️  
- Validate parsing rules across multiple sample search pages to ensure:  
  - consistent selectors  
  - pagination compatibility  
  - accurate URL extraction  
  - no phantom entries or empty records ⚙️  
- Store final rule set as **search_rules.json**, including:  
  - selectors  
  - pagination logic  
  - normalization rules  
  - post-processing transformations  
  - fallback mechanisms ⚙️
  
---

### **Step 4: Generate Product Detail Extraction Rules**
- DOM clustering across product pages ⚙️  
- **LLM-based rule generation**:  
  - Propose CSS/XPath selectors 🧠  
  - Propose regex for price/availability 🧠  
  - Suggest fallback extraction strategy 🧠  
- Human-verifiable rule output stored in DB ⚙️
- **Rule-Set Validation**
  - Test rules on multiple product pages ⚙️  
  - Validate field completeness, consistency using  ⚙️  
  - Coverage scoring ⚙️  
  - LLM repair suggestions if rules break 🧠
  
---

### **Step 5: Identify Vendor-Specific Product Identifier**
- Extract all potential identifier fields from the product page (e.g., SKU, GTINs, MPN, internal product IDs) ⚙️
- Evaluate each candidate across multiple sample product pages to determine stability, consistency, and uniqueness ⚙️
- If deterministic extraction fails, use an LLM to infer the most likely primary identifier based on page semantics and field usage patterns 🧠
- Generate and store a prioritized identifier fallback hierarchy (e.g., SKU → GTIN → MPN → InternalID) for use during product ingestion and deduplication ⚙️

---

### **Step 6: Generate Deduplication & Processing Logic**
- Code generation for dedupe and update logic (optional LLM assistance) 🧠  
- Store generated logic in a version-controlled environment ⚙️  

---

# 2. Product Onboarding Pipeline (After Vendor Onboarding)

## 2.1. Input
- Vendor already onboarded  
- User provides queries for fetching product results

---

## 2.2. Pipeline Steps

### **Step 1: Search Page Query Execution**
- Apply saved access instructions ⚙️  
- Construct query URLs or POST requests ⚙️  
- Render search page using proxy/browser if needed 🌐  
- Validate search presence ⚙️  

---

### **Step 2: Extract Product URLs from Search Page**
- Use stored deterministic rules for parsing search pages (CSS/XPath) ⚙️  
- Normalize and dedupe URLs ⚙️  
- Validate list size > threshold ⚙️  

---

### **Step 3: Rule Validation Stage (NEW — PRE-EXTRACTION STEP)**

**Purpose:** Ensure extraction rules haven’t broken since onboarding.

Process:  
1. Select sample product URLs ⚙️  
2. Fetch rendered HTML 🌐  
3. Apply stored rules ⚙️  
4. Schema validation ⚙️  
5. LLM-assisted auto-repair if rules break 🧠  
6. Re-validate updated rule set ⚙️  
7. Version bump stored rules ⚙️  

This stage prevents failure due to DOM changes.

---

### **Step 4: Full Product Detail Extraction**
- Fetch product pages using proxy/browser 🌐  
- Apply validated rules ⚙️  
- Data normalization ⚙️  
- Product schema validation ⚙️  

---

### **Step 5: Deduplication & Post-Processing**
- Apply deduplication logic generated during vendor onboarding ⚙️  
- Detect price changes ⚙️  
- Detect attribute changes ⚙️  
- Merge vs create-new decisions ⚙️  
- Store cleaned product entity ⚙️  

---

# 3. Validation Stages (LLM vs Non-LLM)

### **Vendor Onboarding Validation**
- Search page validation ⚙️  
- Product page ingestion ⚙️  
- Rule-set validation ⚙️  
- Optional: LLM rule self-repair 🧠  

### **Product Onboarding Validation**
- Search extraction validation ⚙️  
- **Rule validation (new step)** ⚙️ + 🧠  
- Product detail extraction validation ⚙️  
- Post-processing validation ⚙️  

---

# 4. Technologies & Tools

### **Scraping Layer (Non-LLM)**
- Oxylabs / Bright Data 🌐  
- Playwright / Puppeteer 🌐 - Not needed, we can use oxlabs/render html
- Headless browser DOM extraction 🌐 - Not needed

### **LLM-Driven Components**
- Selector generation 🧠  
- Fallback rule suggestion 🧠  
- Rule repair/regeneration 🧠  
- Optional: code generation for dedupe logic 🧠  

### **Deterministic Components**
- CSS/XPath rule engine ⚙️  
- Schema validator ⚙️  
- Rule versioning ⚙️  
- Deduplication engine ⚙️  
- Product processing logic ⚙️  
- Data normalization modules ⚙️  
