# Leaf Category Inference — Interview Problem

## About this interview

This is a real customer problem, not a leetcode exercise. It tests how you use AI to solve a difficult, open-ended customer issue, problem-solve in a dynamic environment, and work with unknown APIs. Please use AI and any tools you'd use on the job, and explain your thinking as you go — I won't ask about every line, but you should be able to explain what your code does at a high level and what problem it solves.

**Time:** ~1 hour, with a hard stop at 1:10 to discuss the role and answer your questions. We can extend a few minutes if things are going well.

---

## The customer's message

> Hi there!
>
> As mentioned earlier, we're dealing with mis-categorization on our e-commerce site — the leaf category assigned to some items is not accurate. We're sharing 4 groups of items with you for leaf-category inference: please infer the most accurate leaf category for each item **from its title**.
>
> Leaf categories differ across e-comm sites, so we're also giving you the leaf-category list for each relevant site: US (`site_id = 0`), UK (`site_id = 3`), Germany (`site_id = 77`), and Motors (`site_id = 100`). Each item's `item_site_id` tells you which site list to use — please select the most accurate category from that site's list.

---

## Data

All files are in the `data/` folder under the project root (the root is the folder containing `prompt/`). Paths below are relative to `data/`.

**Site category lists — use the one matching each item's `item_site_id`:**

| Site | site_id | File |
|---|---|---|
| US | 0 | `usa_leaf_categories_site_id_0.csv` |
| UK | 3 | `uk_leaf_categories_site_id_3.csv` |
| Germany | 77 | `germany_leaf_categories_site_id_77.csv` |
| Motors | 100 | `motors_leaf_categories_site_id_100.csv` |

**Item groups (~50 items each):**

- Group 1 — `g1_items_SAP_category_data_37_111_1219.csv`
- Group 2 — `g2_items_C2C_and_4or5_Seller_Seg_data_100_1219.csv`
- Group 3 — `g3_items_Big_3_Sites_Data_100.csv`
- Group 4 — `g4_items_Hang_data_100_1229.csv`

---

## Requirements

**1. Output — one category per item.**
For each item, infer exactly **one** leaf category: the single most accurate `leaf_categ_id` (with its category name) from the site list matching the item's `item_site_id` (0 → US, 3 → UK, 77 → Germany, 100 → Motors). This feeds a machine-learning data-labeling workflow, so return exactly one category per item — not a ranked list or multiple candidates.

**2. Compare against the existing label.**
Each item row already has a `leaf_categ_id` — the category currently assigned in our system, which is the value we suspect is sometimes wrong. Treat it as the existing label to **audit, not** as ground truth, and don't feed it to the model as a hint. Return the output as a comparison: the original (provided) `leaf_categ_id`, your inferred `leaf_categ_id` + name, and a match flag for whether they agree (i.e., whether the item looks mis-categorized).

**3. Scale — design for 30k+ options.**
Build the solution to handle every row in all of the CSVs. Some category files have 30,000+ options, so design a retrieval/matching approach that stays accurate at that scale instead of sending the entire list to the model. **Don't worry about how long a full run would take** — prioritize an accurate approach. When testing, only run **2 items per CSV** to keep it quick and cheap.

**4. Code & packaging.**
Put all of your code in the `code/` folder under the project root (Python preferred; a TypeScript or Golang SDK is available on request). Keep everything in that one folder and include a reproducible way to run it — `uv`, a `requirements.txt`, or similar package management.

**5. Platform.**
Use the Roe AI agent platform (app.roe-ai.com) for the LLM step — the multimodal agent (`data collection → multimodal extraction`) is recommended, but you're free to use anything. Aim for something you'd be comfortable sending to a customer.

---

## Bonus

- **LLM judge:** after the category has been computed (kept separate from the inference step so it doesn't bias the result), add an LLM judge that decides whether the new category is correct, including a 1–2 sentence reasoning on why.
- **Cleanup:** if your solution uses multiple agents, make sure they're cleaned up afterward.

---

## References

- **Roe SDK:** https://pypi.org/project/roe-ai/ (the SDK you'll need) · source: https://github.com/roe-ai/roe-python · docs: https://docs.roe-ai.com/
- **Credentials:** log in at https://app.roe-ai.com/ and select **"roe interview"** in the bottom left to get your API key, org id, and any other credentials you need.
- Use any tools you'd use on the job — ChatGPT, Gemini, Cursor, Claude Code, Codex, etc.
- Think critically and **verify your code's outputs** — be ready to explain your methodology and results.
- Questions? Ask your interviewer anytime. Thank you!
