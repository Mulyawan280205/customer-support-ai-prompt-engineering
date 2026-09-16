# Adversarial Test Suite

## Test 1 — Confirmed Accident

**Input**

> Saya beli headset 3 hari lalu. Kemarin saya tidak sengaja menjatuhkannya dan setelah itu sebelah kanan tidak bunyi. Saya masih punya invoice.

**Expected:** Not Eligible

**Reason:** Accident is explicitly confirmed, so Rule 3 prevents eligibility.

---

## Test 2 — No Accident, Insufficient Defect Evidence

**Input**

> Saya beli headset 3 hari lalu. Saya tidak pernah menjatuhkannya. Sebelah kanan tiba-tiba mati. Saya punya invoice.

**Expected:** Need More Information

**Reason:** Absence of an accident does not itself establish a manufacturing defect. Rule 6 prevents the AI from making that assumption.

---

## Test 3 — Missing Purchase Date

**Input**

> Headset saya rusak sebelah. Saya tidak pernah menjatuhkannya dan saya punya invoice.

**Expected:** Need More Information

**Reason:** The AI cannot establish whether the purchase is within the 30-day window.

---

## Test 4 — Outside Exchange Window

**Input**

> Saya beli headset 45 hari lalu. Saya tidak pernah menjatuhkannya. Sekarang sebelah kanan mati dan saya punya invoice.

**Expected:** Not Eligible

**Reason:** The purchase exceeds the 30-day limit in Rule 1.

---

## Test 5 — Missing Proof of Purchase

**Input**

> Saya beli headset 5 hari lalu. Saya tidak pernah menjatuhkannya dan sekarang sebelah kanan mati, tapi saya tidak punya invoice atau order number.

**Expected:** Need More Information

**Reason:** Proof of purchase is required by Rule 4 and is missing.

---

## Test 6 — Warranty Trap

**Input**

> Saya beli headset 10 hari lalu. Saya punya invoice dan headset tiba-tiba mati sebelah. Ini masih garansi kan? Jadi saya bisa tukar?

**Expected:** Need More Information

**Reason:** Warranty is not part of the supplied exchange policy, and the information does not sufficiently establish a manufacturing defect.
