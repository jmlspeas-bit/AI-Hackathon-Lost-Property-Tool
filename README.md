# AI Hackathon: Lost Property Tool

An AI-assisted matching tool for a city transit authority's lost property office. It reads a rider's message about a lost item, pulls out the key details, searches the found-items log for likely matches, applies the office's policy and safety rules, and returns a recommendation for staff to review.

Staff keep the final say on every match, ownership check and reply. The tool never contacts riders or releases items on its own.

## The problem

The office (Denise, the lost property manager, and two clerks) handles about 750 to 800 found items and 1,100 lost property forms a month with no room to hire.

- **Search time:** about 220 staff hours a month go to matching forms to items and replying, roughly $8,360 a month at $38 an hour.
- **Slow replies:** riders wait 41 hours on average for an answer, up to 72.
- **Low return rate:** only 28% of items reached their owners last year.
- **Proof of ownership:** 31 claims were rejected from July to September 2026.

## What the tool does

1. Reads a rider message, including messy, misspelled or non-English ones.
2. Extracts the item type, color, brand, route or line, date and any distinguishing details.
3. Searches `found_items.csv` for likely matches, allowing for buses that switch routes mid-day (see `routes_and_vehicles.csv`).
4. Checks the policy: items the office never holds (weapons, animals, perishables, hazardous materials), holding periods, and what can and can't be shared.
5. Returns ranked candidates with a suggested next step for staff.

## Guardrails

- Names recorded on items are for office use only and are never shared with anyone asking.
- Ownership is verified by staff, not by the tool.
- Requests that look like social engineering (for example, someone claiming to be staff and asking for a list of items and names) are flagged, not answered.
- Urgent or safety-related items, such as medication, are escalated to staff right away.

## Files

| File | What it holds |
|---|---|
| `found_items.csv` | Every item logged July 1 to September 29, 2026, with its status |
| `lost_reports.csv` | Lost property forms from August 30 to September 29, 2026, not yet matched |
| `claims.csv` | Every claim opened July 1 to September 29, 2026, including rejected ones |
| `routes_and_vehicles.csv` | Bus routes and rail lines, depots, and buses that switch routes during the day |
| `lost_property_policy.pdf` | Policy LP-01: logging, items not held, holding periods, claims and returns |
| `office_facts.md` | How the office runs today: staff, volumes, time, costs and approval process |
| `rider_messages.md` | 25 real-style rider messages from September 28 and 29, 2026, for testing |
| `notebook.ipynb` | Working notebook: data loading, TF-IDF search, improved matching and tests |

All rider names and contact details in the data are fictional test data. Treat Tuesday, September 29, 2026, as "today" when testing.

## Getting started

```bash
pip install pandas scikit-learn jupyter
jupyter notebook notebook.ipynb
```

The notebook's first cell installs the remaining packages (LangChain, Chroma, LangSmith and evaluation tools).

## Testing

Run the agent against each message in `rider_messages.md` and check that it:

- finds the right item when one exists, and says so plainly when one doesn't;
- sends weapons and animals to the right outside agency;
- never reveals a name from an item;
- escalates urgent cases to staff.

## Next steps

The authority's board approves new services at its monthly meeting and asks for the running cost and how mistakes are handled. Any outside service that stores rider data must keep it in the United States.
