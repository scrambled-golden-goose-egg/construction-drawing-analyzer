# How to Build Your Excel Takeoff Template

The construction-takeoff skill populates your Excel template. You build the template once and reuse it on every project.

A ready-built version is at `templates/takeoff-template.xlsx`. Use it as is, or rebuild it to your firm's format using the structure below and save over that file.

---

## Column structure

The skill writes to these columns in this order. Do not change the column positions.

| Col | Header | What Claude writes | Who fills it |
|-----|--------|--------------------|-------------|
| A | Item No | Sequential 1, 2, 3... | Claude |
| B | Element | Element type (Pad Footing, UC Column, etc.) | Claude |
| C | Description | Full spec from drawing | Claude |
| D | Source Sheet | Drawing number | Claude |
| E | Source Method | Tag count / Schedule / Visual / Dimension | Claude |
| F | Qty | Number only | Claude |
| G | Unit | EA / m / m2 / m3 / LM / kg | Claude |
| H | Confidence | HIGH / MEDIUM / LOW | Claude |
| I | Excavation (m3) | Calculated | Claude |
| J | Concrete (m3) | Calculated | Claude |
| K | Formwork (m2) | Calculated | Claude |
| L | Reinforcement (kg) | Calculated | Claude |
| M | Rate | Leave blank | Your estimator |
| N | Total | Add =F*M formula | Your estimator |
| O | Notes | Manual check flags | Claude |

---

## Row structure

```
Row 1:  Title row — Project name and drawing reference
Row 2:  Column headers (use the headers from the table above)
Row 3:  FOUNDATIONS  (bold, no data — section header)
Row 4:  Pad Footings  (bold, no data — sub-header)
Rows 5+: [Claude writes data rows here]
...
Next section header: Strip Footings
...
Next section header: COLUMNS
...
Next section header: BEAMS
...
Next section header: SLABS
...
Next section header: REINFORCEMENT SUMMARY
...
Final row: TOTALS  (SUM formulas for each column)
```

---

## Formulas to add

In column N (Total), add this formula and copy down:
```
=IF(F5="","",F5*M5)
```

In each section Subtotal row and the TOTALS row, use SUBTOTAL rather than SUM for the secondary quantity and cost columns (I, J, K, L, N), covering the full range above:
```
=SUBTOTAL(9,I3:I193)   for Excavation total
=SUBTOTAL(9,J3:J193)   for Concrete total
=SUBTOTAL(9,K3:K193)   for Formwork total
=SUBTOTAL(9,L3:L193)   for Reinforcement total
=SUBTOTAL(9,N3:N193)   for Cost total
```

SUBTOTAL skips other SUBTOTAL cells, so the grand total does not double count the section subtotals. A plain SUM would.

---

## Formatting tips

- Bold and shade section header rows (light grey works well)
- Freeze row 2 (the header row) so it stays visible when scrolling
- Format columns I, J, K, L to 2 decimal places
- Format column M and N as currency
- Add conditional formatting to column H: green for HIGH, yellow for MEDIUM, red for LOW

---

## After Claude populates it

1. Open `outputs/markups/` and check the marked-up drawing images — they show what was counted and where
2. Verify every LOW confidence item manually against the original drawing
3. Fill in your rates in column M
4. Review secondary quantity totals against your experience on similar projects
5. Adjust any quantities Claude flagged as needing manual check
