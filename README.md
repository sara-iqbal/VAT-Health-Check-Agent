# VAT Health-Check

**A robot checker that reads every receipt a business has and flags the ones that look wrong.**

[Live dashboard](https://YOUR-USERNAME.github.io/vat-health-check/) · [Run the notebook in Colab](notebooks/VAT_Health_Check.ipynb)

> All data is made up. This is a learning project, not tax advice.

## A real-life example

Imagine you own a small building firm. Every month you buy materials, pay suppliers and send invoices to customers. Most of these carry **VAT**, a sales tax added on top of the price. You collect it from customers, pay it to the government (HMRC in the UK), and can claim back the VAT you paid on business costs.

With thousands of invoices a year, mistakes creep in:

- A supplier charged 20% VAT when the item should have been 0%.
- The same invoice was typed in twice, so you claimed the VAT back twice.
- You claimed VAT back on a client dinner, which isn't allowed.
- A supplier's VAT number on the invoice is missing or fake.
- The invoice total simply doesn't add up.
- A builder charged VAT when, under the "reverse charge" rule, you should have accounted for it yourself.

Nobody has time to check every invoice, so accountants usually check a small sample. Mistakes outside the sample are found later, by the tax office, with fines and interest.

## What this project does

It checks **every** invoice using three helpers:

1. **The rule-checker** knows the VAT rulebook and catches known mistakes, without flagging honest cases like small suppliers who correctly charge no VAT.
2. **The "that looks odd" spotter** learns what a normal invoice looks like and points at unusual ones, even when no rule covers them.
3. **The explainer** writes a short plain-English note for each flagged invoice: what looks wrong, which rule applies, and what to do next.

Two safety rules apply throughout. **The AI never does the maths**: every number comes from ordinary code. And **a person always makes the final decision**. The project also double-checks that any amount in an AI-written note really came from the data.

## What it found (on practice data)

I created 2,026 pretend invoices and secretly hid 156 mistakes in them, worth about **£43,500** of VAT. The checker found all of them without wrongly flagging a single honest invoice. Correcting them would have changed the VAT paid over the year by about **£38,000**.

Those perfect scores need a caution. I wrote the checks to match the mistakes I planted, so the result proves the system works, not that it will be perfect on real, messy accounts. Testing on messier data is the next step.

## See it

- **Dashboard:** `docs/index.html` shows the flagged invoices, the money at stake and the effect on the quarterly VAT return.
- **Notebook:** `notebooks/VAT_Health_Check.ipynb` runs everything from start to finish.

## Run it yourself

1. Open the notebook in Google Colab.
2. Run all cells and upload `data/ledger.csv` and `data/ground_truth.csv` when asked.
3. Optional: add a Colab secret called `ANTHROPIC_API_KEY` to have an AI write the explanations. Without it, a template is used.

To create new practice data: `pip install -r requirements.txt`, then `python -m vat_health_check.generate_data`.

## What's in the box

| Folder | Contents |
|---|---|
| `vat_health_check/` | VAT rules and the practice-data maker |
| `notebooks/` | The full checker, step by step |
| `docs/` | The dashboard web page |
| `data/` | Practice invoices and the answer sheet |
| `tests/` | Automatic checks that the code works |

## Limits

- Covers a simplified set of UK VAT rules (not partial exemption, flat-rate scheme, imports and exports).
- Checks a VAT number's format, not whether it is really registered.
- Rules change. Confirm against current GOV.UK guidance.
- The AI-written notes are tested only for safe fallback without a key, so try them with your own key first.

## Roadmap

- [x] Practice data with hidden mistakes
- [x] Rule-checker, spotter, explainer, draft VAT return
- [x] Dashboard page
- [ ] Messier practice data and scoring on unseen data
- [ ] Interactive review app where a person approves or rejects each flag
