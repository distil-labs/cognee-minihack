# AI Hack Night
Before you start building, **complete the environment setup in `SETUP.md`**.
Once setup is finished, you’ll have access to Cognee’s search interface, backed by a prebuilt knowledge graph generated from synthetic invoice and transaction data.
Your job is to build anything that uses this QA capability in a meaningful way.
## What You Work With
- Query Cognee using natural-language questions (see how `completion` is generated in the `solution_q_and_a.py`).
- Receive structured or free-text answers.
- Use those answers however you like in your project.
## Constraints
- The local model must remain functional; online LLM use is optional.
- The raw data included in the `data` folder is there for reference and should not be used directly.
## What You Can Build
Any tool, workflow, interface, or feature that benefits from QA over vendor, product, payment, or order information.
## Deliverables
- Create a folder named `submission` on your USB stick and place your entire project inside it.
- Your project must include code that demonstrates successful queries to Cognee.
- Hand in the USB at the deadline and be ready to give a short demo.
## Notes
- You do not have to add new files modify or enrich the graph. In case you want to, there is some additional data in the `optional_data_for_enrichment` folder.