# PACHA Automation System — Technical Brief

## 1. Task Title  
**Purpose:** Clearly and concisely name the task so AI models immediately understand the scope and intent.  
Use a short, action-oriented title that reflects the main engineering objective.

**Task Title:**  
Automation of PACHA Operational Workflow Using a Multi-Agent System (Restaurants → Review → Suppliers → Pickup Planning → Receipt Generation)

---

## 2. Context  
**Purpose:** Provide background information necessary to understand why this task exists and how it fits into PACHA’s broader operations.

PACHA is a produce procurement platform that connects restaurants in Pereira  with the main food distriution hub. Currently, the workflow is mostly manual and relies heavily on WhatsApp communication, which makes it time-consuming, error-prone, and difficult to scale.

The operational workflow consists of four main stages:

### Stage 1 — Restaurant Orders  
Restaurants send daily orders via WhatsApp using:

- text messages  
- photos of handwritten orders

The goal is to develop a chatbot-like system (digital secretary) that:

- interacts directly with restaurant clients  
- receives orders automatically  
- extracts structured product lists  
- tabulates all orders into a clean, organized system  

---

### Stage 2 — Internal Review  
After aggregation, orders must be reviewed manually to:

- correct errors  
- confirm quantities  
- approve final consolidated demand  

The system should allow structured review and editing before supplier interaction.

---

### Stage 3 — Supplier Coordination  

Because:

- supplier prices fluctuate daily  
- inventory varies across vendors  

The system must use agents to:

- contact suppliers via WhatsApp  
- request daily price and stock availability  
- compare options automatically  
- generate optimal procurement decisions  

Finally, the system must generate a **daily pickup route plan** for logistics execution.

---

### Stage 4 — Client Receipt Generation (Post-Delivery)

After delivery is completed, the system must automatically generate and send a receipt to the client using the same WhatsApp chatbot that manages orders.

The receipt must include:

- final confirmed product list  
- delivered quantities  
- final unit prices  
- total cost  
- delivery date  
- PACHA business information  
- receipt or invoice number  

The system should ensure the receipt complies with applicable legal and accounting requirements in Colombia.

---

## 3. Technical Requirements  
**Purpose:** Define exactly what must be implemented from a technical perspective.  
This section should be precise, structured, and testable.

---

### 3.1 Language(s)  

Specify programming languages, frameworks, and tools required.

**Proposed Stack:**

- Python (primary backend and orchestration)  
- FastAPI (API layer)  
- LangChain / CrewAI (agent orchestration)  
- OpenAI or multimodal LLM APIs  
- WhatsApp Cloud API (Meta)

Optional:

- PostgreSQL / Firestore (data storage)  
- Redis (queueing)  
- Google Maps API (routing optimization)

---

### 3.2 Inputs  

Clearly define all expected inputs.

#### Stage 1 — Restaurant Orders  

Expected inputs:

**Text Example**

- 20 kg limon  
- 10 kg fresa  
- 80 kg papa  

**Image Example**

- handwritten or typed order lists  
- requires handwritten text recognition  

---

#### Stage 2 — Internal Review  

Inputs:

- aggregated daily order table  
- manual corrections from dashboard interface  

---

#### Stage 3 — Supplier Interaction  

Inputs:

- consolidated demand list  
- supplier contact registry  

**Agent message example:**  

> Buenos días. ¿Precio y disponibilidad hoy para limon, fresa y papa?

---

#### Stage 4 — Client Receipt Generation  

Inputs:

- finalized delivery order (post-review and execution)  
- confirmed delivered quantities  
- final negotiated prices  
- client billing information  

---

## 3.3 Outputs  
**Purpose:** Clearly define expected outputs and formats.

---

### Stage 1 Outputs  

- structured daily order dataset  
- clean tabulated dashboard view

```json
{
  "restaurant_id": "R001",
  "timestamp": "",
  "items": [
    {"product": "limon", "quantity": 20, "unit": "kg"},
    {"product": "fresa", "quantity": 10, "unit": "kg"},
    {"product": "papa", "quantity": 80, "unit": "kg"}
  ]
}
```

---

### Stage 2 Outputs  

- approved consolidated demand dataset
---

### Stage 3 Outputs  

Supplier comparison table (example format):

| Supplier | Product | Price | Stock | Selected |
|----------|---------|-------|-------|----------|

- optimized procurement decision report  

---

### Stage 4 Outputs  

The chatbot sends a structured receipt via WhatsApp.

Formats:

- PDF receipt (primary format)  
- optional text summary in message body  

Example WhatsApp message:

> Hola  
> Te compartimos el comprobante de tu pedido de hoy.  

---

### Logistics Outputs  

- optimized pickup route plan  
- daily logistics execution sheet  

---

## 4. Constraints  
**Purpose:** Document technical, operational, and business limitations affecting implementation.

---

### Operational Constraints  

- must operate directly through WhatsApp  
- must support Spanish conversational input  

---

### Technical Constraints  

- handwritten text recognition pipeline must work with noisy photos  
- message response latency must remain under ~10 seconds  

---

### Business Constraints  

- daily price fluctuations  
- inconsistent supplier responses  

---

### Scalability Constraints  

- support ≥10 restaurants/day  
- support ≥5 suppliers/day  

---

### Receipt Constraints  

- must follow Colombian receipt/invoicing requirements  
- must generate documents automatically without manual intervention  
- must send within minutes after delivery confirmation  

---

## 5. Definition of Done (DoD)  
**Purpose:** Provide clear acceptance criteria to determine when the task is complete.

---

### Stage 1 Completion Criteria  

- chatbot successfully deployed  
- processes both text and image inputs  
- achieves ≥95% extraction accuracy  
- automatically tabulates orders  

---

### Stage 2 Completion Criteria  

- dashboard supports manual review  
- manual edits are saved reliably and reflected in downstream stages  

---

### Stage 3 Completion Criteria  

- supplier agents operate successfully  
- pricing and stock parsed correctly  
- optimal supplier automatically selected  

---

### Stage 4 Completion Criteria  

- receipt generated automatically after delivery completion  
- totals calculated correctly  
- PDF formatted correctly  
- receipt sent successfully via WhatsApp  
- stored in system records  

---

### Logistics Completion Criteria  

- pickup routes generated automatically  

---

### System-Level Completion Criteria  

- end-to-end workflow operates daily  
- manual workload reduced significantly  
- validated with real PACHA operations  
- system documented clearly  
