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
- Product listing page access rules ⚙️  
- Product URL extraction rules ⚙️  
- Product detail extraction rules (LLM-supported generation) 🧠  
- Unique identifier strategy ⚙️  
- Deduplication & processing code modules ⚙️  
- Validation metadata ⚙️  
- Stored rule version in DB ⚙️  

---

## 1.3. Pipeline Steps

### **Step 1: Discover Product Listing Page**
- Crawl landing pages 🌐  
- Heuristic URL detection (`/search`, `/category`, `/products`) ⚙️  
- DOM pattern analysis for repeated product tiles ⚙️  
- LLM suggestion on best candidate listing URL (optional) 🧠  
- Validate listing functionality ⚙️  
- Store access instructions ⚙️  

---

### **Step 2: Store Listing Page Access Instructions**
- URL templates with query placeholders ⚙️  
- Form/POST metadata ⚙️  
- JS-triggering workflows (browser automation) 🌐  
- Required headers, cookies, auth ⚙️  

---

### **Step 3: Collect Sample Product Pages**
- Extract sample product URLs from listing page ⚙️  
- Fetch rendered HTML using proxy/browser tools 🌐  

---

### **Step 4: Generate Product Detail Extraction Rules**
- DOM clustering across product pages ⚙️  
- Pattern identification for title, price, SKU ⚙️  
- **LLM-based rule generation**:  
  - Propose CSS/XPath selectors 🧠  
  - Propose regex for price/availability 🧠  
  - Suggest fallback extraction strategy 🧠  
- Human-verifiable rule output stored in DB ⚙️  

---

### **Step 5: Identify Unique Product Identifier**
- Detect candidate identifiers (SKU, GTIN, MPN) ⚙️  
- Validate across sample pages for uniqueness ⚙️  
- Store ID fallback chain ⚙️  

---

### **Step 6: Generate Deduplication & Processing Logic**
- Code generation for dedupe and update logic (optional LLM assistance) 🧠  
- Store generated logic in a version-controlled environment ⚙️  

---

### **Step 7: Rule-Set Validation**
- Test rules on multiple product pages ⚙️  
- Validate field completeness, consistency ⚙️  
- Coverage scoring ⚙️  
- Optional LLM repair suggestions if rules break 🧠  

---

# 2. Product Onboarding Pipeline (After Vendor Onboarding)

## 2.1. Input
- Vendor already onboarded  
- User provides queries for fetching product results

---

## 2.2. Pipeline Steps

### **Step 1: Listing Page Query Execution**
- Apply saved access instructions ⚙️  
- Construct query URLs or POST requests ⚙️  
- Render listing page using proxy/browser if needed 🌐  
- Validate listing presence ⚙️  

---

### **Step 2: Extract Product URLs from Listing Page**
- Use stored deterministic rules (CSS/XPath) ⚙️  
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
- Listing page validation ⚙️  
- Product page ingestion ⚙️  
- Rule-set validation ⚙️  
- Optional: LLM rule self-repair 🧠  

### **Product Onboarding Validation**
- Listing extraction validation ⚙️  
- **Rule validation (new step)** ⚙️ + 🧠  
- Product detail extraction validation ⚙️  
- Post-processing validation ⚙️  

---

# 4. Technologies & Tools

### **Scraping Layer (Non-LLM)**
- Oxylabs / Bright Data 🌐  
- Playwright / Puppeteer 🌐  
- Headless browser DOM extraction 🌐  

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
