# Alu-Alu Command — the logic behind it

This is the "why" document. It explains every number the app computes, so you can trust it, argue with it, and change it knowingly. Written for you as a CA — formulas included, no hand-waving.

---

## 1. The files, and what each one does

- **index.html** — the whole app: the screens, the maths, the styling. Everything lives inside this one file. Open it in a browser and it runs. This is the only file that is strictly required.
- **manifest.json** — a small label that tells the phone the app's name ("Alu-Alu"), that it should launch full-screen (not inside a browser bar), and its colours. It's what makes "Add to Home Screen" produce something that *feels* like an app.
- **sw.js** — the *service worker*. It quietly saves a copy of the app on the phone the first time it loads, so afterwards it opens instantly and **works with no internet**. This is the file your strength app had too.

Only `index.html` does real work. The other two are polish: a name, full-screen launch, and offline. If you ever deleted them, the app would still run — it just wouldn't be offline or as cleanly named.

---

## 2. The core idea

This is a **pass-through business**. About 80% of your selling price is raw material that flows straight through you. You are really a converter who adds a thin layer of value on top of a lot of foil. That single fact drives every design decision: the app obsesses over the *spread* (price minus cost) and over the *cash* needed to push that much material through, because those are the two things that can kill the business.

The app answers one question in three views:

- **ROI** — on your own money, at four volumes. The verdict.
- **Enter data** — every input you can change.
- **Report** — a one-page sheet you can save as PDF.

---

## 3. The cost of one kilogram

### Material build-up
Each film layer has a weight per square metre called **GSM** (grams per square metre). The physics:

> **GSM = thickness (µm) × density (g/cm³)**

Example, the foil: 50 µm × 2.71 = 135.5 GSM. This is just "how heavy a 1 m² sheet of this layer is." Heavier layer = more kilograms = more cost.

Cost of that layer per square metre:

> **Cost/m² = (GSM ÷ 1000) × rate (Rs/kg)**

Add the layers' GSM together (OPA + foil + PVC + adhesive) to get the **total GSM**, e.g. ~252. Convert to kilograms per square metre — that's your **yield**:

> **Yield (kg/m²) = total GSM ÷ 1000** ≈ 0.252

Now flip cost-per-m² into cost-per-kg by dividing by the yield:

> **Cost/kg = Cost/m² ÷ Yield**

The total of all layers is your **ideal cost per kg** (~Rs 373).

### Adhesive
The adhesive isn't priced by GSM directly; it's a wet mix. You blend KUB130 : Hardener : Ethyl Acetate in a ratio (100:15:100), which gives a blended rate per kg. You then consume a fixed amount of that wet mix per tonne of output (50 kg/tonne). So:

> **Adhesive cost/kg of output = (kg of wet mix per tonne × blended rate) ÷ 1000**

### Wastage
Real production scraps some material. You add a wastage % on top of the ideal cost to get what you actually *consume*:

> **RM consumed/kg = Ideal cost/kg × (1 + wastage%)**

≈ Rs 373 × 1.06 ≈ **Rs 396/kg**. This is the honest raw-material cost of a saleable kilogram.

**Why it matters:** the foil alone is ~75% of this Rs 396. A Rs 30/kg swing in foil price moves your whole business more than any saving you could make elsewhere. That's why "Raw materials" is the first place the app sends you.

---

## 4. Conversion cost — the value you add

Everything that isn't raw material is **conversion cost**: salaries, electricity, rent, packaging, transport, etc. The app splits these the way a cost accountant does:

- **Fixed costs** — don't change much with volume (rent, supervisor, admin). Spread over more kilograms, they *shrink per kg*.
- **Variable costs** — rise roughly in step with volume (packaging, transport), so they stay flat *per kg*.

> **Conversion cost/kg = (total fixed ÷ kg) + (total variable ÷ kg)**

This is computed **bottom-up** from the actual line items you enter — not from a headline figure. (Important: your original sheet's fixed-cost lines added up to *more* than its stated total. The app trusts the lines you enter, so correct them and the conversion cost corrects itself.)

The key behaviour: **as you make more tonnes, conversion cost per kg falls** (fixed cost dilutes). That's why bigger volumes earn a fatter margin per kilogram.

> **Net margin/kg = Price − RM consumed − Conversion** (then less tax, if any)

This is the final spread — the number that actually feeds your return.

---

## 5. Working capital — the part I got wrong first, then fixed

This is the heart of the business, and the subtlest part. Working capital is the cash *frozen* inside the operating cycle at any moment. It is **not** an expense — you get it back — but you must *fund* it the whole time you run. It has four parts:

| Part | What it is | Valued at | Formula |
|---|---|---|---|
| **RM stock** | foil/film sitting in stores | material cost | annual RM ÷ 365 × stock-days |
| **Finished goods** | made but not yet shipped | production cost | annual COGS ÷ 365 × FG-days |
| **Debtors** | credit you give customers | **selling price** | annual sales ÷ 365 × credit-days |
| *less* **Creditors** | credit suppliers give you | material cost | annual RM ÷ 365 × supplier-days |

> **Net working capital = RM stock + Finished goods + Debtors − Creditors**

Two things to notice, because they're where intuition goes wrong:

1. **Debtors are valued at your *selling price*, not at cost.** When a customer owes you for a tonne, they owe the full invoice (Rs 490/kg), not your Rs 396 cost. My first version wrongly used cost — that understated the cash you're short by crores.
2. **Creditors are free working capital.** Every extra day your supplier waits for payment is a day you don't have to fund. Stretching supplier credit from 30 to 45 days is the *cheapest* working capital there is — it can lift a marginal volume over your 30% line without you spending a rupee.

At 50 tonnes/month with a 75-day customer credit and 1 month of foil stock, this nets to **~Rs 7.1 Cr** — far more than the crude "RM × 75 days" I first used.

---

## 6. The return — on your money

You told me the rule: **you fund with your own money, and you want 30% on the whole pile.** So the app defines:

> **Own money to deploy = Fixed assets + Net working capital**

That's the total capital the business locks up — the machines *and* the cash cycle. All of it is yours (no debt), so:

> **ROI on own money = Annual net profit ÷ Own money deployed**

The verdict is simply: is that ≥ 30%?

### Why ROI *rises* as you grow
Two forces pull in your favour as volume climbs:
- Net margin per kg **rises** (conversion cost dilutes).
- Profit grows **faster** than working capital does.

So the more you sell, the better the return — up to your machine's capacity. With proper costs, the line works out roughly:

| Volume | Net margin/kg | Own money | ROI |
|---|---|---|---|
| 25 T | ~Rs 23 | ~Rs 7.0 Cr | ~10% |
| 50 T | ~Rs 47 | ~Rs 10.6 Cr | ~27% |
| 75 T | ~Rs 54 | ~Rs 14.1 Cr | ~35% |
| 100 T | ~Rs 58 | ~Rs 17.7 Cr | ~40% |

The honest conclusion: **you clear 30% somewhere around 65–75 tonnes, not at 50.** Below that, fixed costs and the cash cycle weigh too heavily.

### "Max own money at 30%"
The last row of the matrix. Since ROI = profit ÷ capital, the *most* you could pour in and still earn exactly 30% is:

> **Max own money @ 30% = Annual net profit ÷ 0.30**

If the capital the business actually *needs* is below this number, you can fund it entirely yourself and stay above 30% — and the gap is how much *extra* of your own money you could safely deploy (bigger buffer, more stock) before the return slips. That's your answer to "how far can I keep pumping my own funds."

### When would you ever go outside?
By your own rule, only if funding more of it yourself would drop you under 30% — which, because ROI rises with scale, basically never happens in the range you'd run. The only real reason to take outside money would be to grow *beyond your own wallet*, and even then debt would *lift* your return, not spoil it. So staying self-funded is a sound choice; the app simply shows you the capital each volume demands so you can decide if you have it.

---

## 7. The three levers that matter most

1. **Foil price** — 75% of cost. Watch it like a hawk; everything else is rounding error by comparison.
2. **Volume** — the single biggest driver of ROI, because it dilutes fixed cost *and* fattens margin. Which loops straight back to demand: can you sell ~70 tonnes?
3. **Credit terms** — the days you give customers and take from suppliers. These don't touch your margin at all, but they swing how much cash you must freeze. The cheapest way to improve ROI without selling a single extra kilogram.

---

## 8. How to read the app, in one breath

Open **ROI**. The big number is your return on your own money at the focused volume — green means it beats 30%. The matrix below shows all four volumes so you can see where you cross the line. The working-capital build-up shows *why* the cash number is what it is. **Enter data** is where every assumption lives — change a foil price, a salary, a credit term, and the whole verdict re-decides. **Report** turns the focused volume into a one-page PDF.

That's the entire machine. Nothing is hidden; nothing is hard-coded except the formulas above.
