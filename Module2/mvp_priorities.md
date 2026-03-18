# PACHA MVP Priorities

## Goal
Build the smallest PACHA system that creates real operational value:

**receive order → extract → normalize → review → save → send receipt**

This MVP should focus on reducing manual work in the daily order workflow before adding supplier automation or route optimization.

---

## 1. Essential Skills

Prioritize these skills first:

### 1. WhatsApp webhook handling
Receive text and image messages from restaurants and send replies.

### 2. Order extraction
Parse text orders and extract structured items from handwritten photos.

### 3. Product normalization
Map messy names such as `limon`, `limón`, or abbreviations into one internal product name.

### 4. Backend orchestration
Move each order through clear states:

`received → extracted → reviewed → approved`

### 5. Human review workflow
Allow manual correction before the order is finalized.

### 6. Database modeling
Store restaurants, orders, items, statuses, and logs.

### 7. Receipt generation
Generate a simple PDF or structured receipt after delivery.

---

## 2. Essential MCPs

Only implement the minimum integrations needed for the MVP:

### 1. WhatsApp MCP
Most important integration.

Use it to:
- receive messages
- fetch image/media files
- send confirmations
- send receipts

### 2. Database MCP
Use it to:
- read and write restaurants
- store orders and items
- update order status
- save receipt records

### 3. OCR / File Ingestion MCP
Use it to:
- process handwritten order photos
- extract text from images
- return structured candidates for parsing

### 4. PDF / Document MCP
Use it to:
- generate receipts
- store receipt files
- attach receipts to outgoing WhatsApp messages

---

## 3. Essential Subagents

For v1, keep the agent system small.

### 1. Order Intake Agent
Responsible for:
- detecting whether the message is an order, question, or correction
- routing text vs image inputs correctly

### 2. Extraction Agent
Responsible for:
- converting text or photo input into structured line items
- identifying product, quantity, and unit

### 3. Normalization Agent
Responsible for:
- cleaning product names
- standardizing units
- flagging unclear values

### 4. Review Assistant Agent
Responsible for:
- preparing draft orders for approval
- highlighting low-confidence items
- supporting manual correction

### 5. Receipt Agent
Responsible for:
- generating the final receipt
- formatting totals
- sending the receipt back to the client

---

## 4. Do Not Prioritize Yet

These are valuable, but should come after the MVP is already reliable:

- Supplier outreach agent
- Procurement optimization agent
- Route planning agent
- Analytics agent
- Memory or personalization agent

These belong in **Phase 2**, not the initial MVP.

---

## 5. Recommended MVP Build Order

Build in this sequence:

1. WhatsApp intake
2. Text and image extraction
3. Product normalization
4. Review dashboard or review workflow
5. Save approved order
6. Generate and send receipt

---

## 6. Simplest MVP Stack

Recommended minimum stack:

- **Python**
- **FastAPI**
- **PostgreSQL** or **Firestore**
- **WhatsApp Cloud API**
- **OCR or multimodal model**
- **Basic PDF generator**

---

## 7. One-Sentence Summary

For the PACHA MVP, prioritize the systems that let you reliably:

**receive orders, extract them, normalize them, review them, store them, and send a receipt.**
