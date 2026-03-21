# PACHA MVP — TDD v1 (Test-Driven Development)

## Estado
- **Versión:** v1
- **Fecha:** 2026-03-21
- **Repositorio:** `sposso/10XBuilders`
- **Contexto base usado:** `Module1/Technical_Brief.md`, `Module1/review_template .md`, `Module2/mvp_priorities.md`

---

## 1) Objetivo del TDD

Definir y ejecutar un flujo **test-first** para el MVP PACHA priorizado en Module2:

**receive order → extract → normalize → review → save → send receipt**

Este TDD asegura que cada comportamiento de negocio crítico se traduzca primero en pruebas verificables, antes de implementar o modificar código productivo.

---

## 2) Alcance del MVP (incluido / excluido)

## Incluido (Phase 1)
1. Recepción de mensajes WhatsApp (texto/imagen)
2. Extracción de items de pedido
3. Normalización de productos/unidades
4. Flujo de revisión humana
5. Persistencia y estados de orden
6. Generación y envío de recibo

## Excluido (Phase 2)
- Supplier outreach agent
- Procurement optimization
- Route planning optimization avanzada
- Analytics y personalización

---

## 3) Convenciones de TDD

## Reglas obligatorias
1. **No se implementa código productivo sin test en RED previo**.
2. Cada caso de uso tiene mínimo:
   - 1 happy path
   - 1 edge case
   - 1 error/fallo (cuando aplique)
3. Nombres de tests orientados a comportamiento observable.
4. Para bugfixes: test de reproducción obligatorio antes del fix.
5. Al cerrar tarea: evidencia RED→GREEN + regresión + riesgos.

## Patrón de test
- `describe(<unidad de comportamiento>)`
- `it(<resultado observable>)`
- Given / When / Then dentro del test

---

## 4) Checklist de progreso TDD (enforcement)

## TDD Progress
- [ ] Paso 1: Aclarar casos de uso
- [ ] Paso 2: Convertir casos de uso en casos de prueba
- [ ] Paso 3: Escribir primero tests en falla (RED)
- [ ] Paso 4: Implementar codigo minimo (GREEN)
- [ ] Paso 5: Refactor seguro si aplica (REFACTOR)
- [ ] Paso 6: Verificar y reportar resultados

> Esta checklist debe copiarse en cada PR/entregable y actualizarse en tiempo real.

---

## 5) Casos de uso oficiales del MVP

## CU-01 — Recepción de orden por WhatsApp
- **Actor:** Restaurante
- **Entrada:** mensaje de texto o imagen
- **Salida:** orden creada con estado inicial `received`
- **Bordes:** payload parcial, mensaje vacío, tipo no soportado
- **Errores:** firma inválida / autenticación webhook fallida

## CU-02 — Extracción de ítems
- **Entrada:** texto directo u OCR de imagen
- **Salida:** lista estructurada de items (`product`, `quantity`, `unit`)
- **Bordes:** unidad faltante, cantidad faltante, typo
- **Errores:** OCR inservible o parseo inválido

## CU-03 — Normalización de productos y unidades
- **Entrada:** items extraídos con ruido semántico
- **Salida:** producto canónico + unidad estándar + flags de confianza
- **Bordes:** sinónimos múltiples, abreviaturas, tildes
- **Errores:** producto no reconocido → `needs_review=true`

## CU-04 — Revisión humana
- **Entrada:** borrador de orden extraída/normalizada
- **Salida:** orden corregida y marcada como `reviewed`/`approved`
- **Bordes:** edición rápida múltiple, recarga de sesión
- **Errores:** aprobación con datos incompletos

## CU-05 — Persistencia y máquina de estados
- **Entrada:** eventos del flujo de orden
- **Salida:** transición válida y persistencia consistente
- **Bordes:** reintentos, idempotencia de webhooks
- **Errores:** caída DB durante transición

## CU-06 — Generación y envío de recibo
- **Entrada:** entrega confirmada + precios/qty finales
- **Salida:** recibo generado y enviado por WhatsApp; registro almacenado
- **Bordes:** orden grande, redondeo, datos incompletos
- **Errores:** fallo PDF o fallo de envío

---

## 6) Matriz de pruebas (Use Case → Test Cases)

## CU-01 — WhatsApp Intake
- **Test 01 (happy):** `creates order in received state for valid text webhook`
- **Test 02 (edge):** `returns safe validation response for empty message body`
- **Test 03 (error):** `rejects webhook with invalid signature`

## CU-02 — Extraction
- **Test 04 (happy):** `extracts product quantity and unit from simple text line`
- **Test 05 (edge):** `flags line item when unit is missing`
- **Test 06 (error):** `returns extraction_failed for unreadable OCR text`

## CU-03 — Normalization
- **Test 07 (happy):** `normalizes limon and limón to canonical limón`
- **Test 08 (edge):** `normalizes kgs and kilo to kg`
- **Test 09 (error):** `marks unknown product as needs_review`

## CU-04 — Human Review
- **Test 10 (happy):** `persists manual quantity correction before approval`
- **Test 11 (edge):** `keeps edited values after refresh/reload`
- **Test 12 (error):** `prevents approval when required fields are missing`

## CU-05 — State + Persistence
- **Test 13 (happy):** `transitions order received extracted reviewed approved`
- **Test 14 (edge):** `ignores duplicate webhook event with idempotency key`
- **Test 15 (error):** `rolls back state transition when database write fails`

## CU-06 — Receipt
- **Test 16 (happy):** `generates receipt with correct subtotal and total`
- **Test 17 (edge):** `handles many line items without total mismatch`
- **Test 18 (error):** `marks receipt_failed when whatsapp send operation fails`

---

## 7) Contratos de datos mínimos para test

## 7.1 Orden (mínimo)
```json
{
  "order_id": "ORD-001",
  "restaurant_id": "R001",
  "source": "whatsapp",
  "status": "received",
  "items": [
    { "product_raw": "limon", "quantity_raw": "20", "unit_raw": "kg" }
  ],
  "needs_review": false,
  "created_at": "ISO8601"
}
```

## 7.2 Item normalizado (mínimo)
```json
{
  "product_canonical": "limón",
  "quantity": 20,
  "unit": "kg",
  "confidence": 0.96,
  "needs_review": false
}
```

## 7.3 Recibo (mínimo)
```json
{
  "receipt_id": "REC-001",
  "order_id": "ORD-001",
  "items": [
    { "product": "limón", "quantity": 20, "unit_price": 3200, "subtotal": 64000 }
  ],
  "currency": "COP",
  "total": 64000,
  "sent_via": "whatsapp",
  "status": "sent"
}
```

---

## 8) Estructura sugerida de tests (v1)

> Nota: Como aún no se confirmó stack ejecutable final del repo, esta estructura es neutral y orientativa.

- `tests/intake/whatsapp_intake.test.*`
- `tests/extraction/order_extraction.test.*`
- `tests/normalization/product_normalization.test.*`
- `tests/review/review_workflow.test.*`
- `tests/orders/order_state_machine.test.*`
- `tests/receipt/receipt_generation_and_delivery.test.*`

Si usan monorepo JS/TS:
- `app/src/.../*.test.tsx`
- `api/src/.../*.test.ts`
- `packages/shared/src/.../*.test.ts`

Si usan Python/FastAPI:
- `tests/unit/...`
- `tests/integration/...`

---

## 9) Estrategia RED → GREEN → REFACTOR

## Iteración 1 (mínima)
1. Escribir tests CU-01 (3 tests) → RED
2. Implementar intake mínimo → GREEN
3. Refactor liviano (si aplica)

## Iteración 2
1. Tests CU-02 y CU-03 → RED
2. Implementar extraction+normalization mínimo → GREEN
3. Refactor

## Iteración 3
1. Tests CU-04 y CU-05 → RED
2. Implementar revisión y máquina de estado → GREEN
3. Refactor

## Iteración 4
1. Tests CU-06 → RED
2. Implementar recibo + envío + retries básicos → GREEN
3. Refactor final

---

## 10) Criterios de aceptación (DoD de testing)

Una historia se considera completa solo si:
1. Todos sus tests definidos en la matriz existen.
2. Se evidenció al menos una corrida RED previa.
3. Tests pasan en GREEN.
4. No rompe regresión en suite relevante.
5. Riesgos remanentes están documentados.

---

## 11) Riesgos y gaps actuales (explícitos)

1. **No hay inventario de código ejecutable aún en el contexto compartido** (solo documentación).
2. **No hay confirmación de framework de test activo** (Vitest/Pytest).
3. **No hay contratos API reales** (payload exacto webhook WhatsApp, schemas DB).
4. **No hay definición final de reglas contables COP** (redondeos/impuestos).

Mitigación v1:
- Mantener tests contractuales con fixtures controlados.
- Aislar integraciones externas con mocks/fakes.
- Validar redondeos con casos explícitos en COP.

---

## 12) Plantilla obligatoria de reporte por entrega

```md
Reporte de verificacion
- Casos de uso cubiertos: <n>/<total>
- Tests agregados: <lista>
- Tests actualizados: <lista>
- Comandos ejecutados:
  - <cmd 1>
  - <cmd 2>
- RED observado:
  - <test>
  - <motivo breve>
- GREEN confirmado:
  - <test(s)>
- Resultado general: <pass/fail>
- Riesgos remanentes:
  - <riesgo 1>
  - <riesgo 2>
```

---

## 13) Política de calidad para cambios futuros

1. Cualquier feature nueva del flujo MVP debe iniciar con **actualización de casos de uso**.
2. Todo bug productivo requiere test de reproducción antes de corrección.
3. No mezclar cambios de comportamiento con refactors grandes en una misma iteración RED→GREEN.
4. Toda integración externa (WhatsApp/OCR/PDF/DB) debe tener:
   - test unitario con mock
   - al menos un test de integración controlada.

---

## 14) Próximo paso recomendado (inmediato)

Ejecutar **Sprint TDD-01 (CU-01)**:
- crear `whatsapp_intake.test.*` con 3 tests (happy/edge/error),
- correr RED,
- implementar intake mínimo,
- correr GREEN y reportar con plantilla oficial.
