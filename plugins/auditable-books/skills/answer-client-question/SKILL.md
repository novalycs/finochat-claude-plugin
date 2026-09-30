---
name: answer-client-question
description: Use whenever the user asks anything about a client's books, finances, invoices, bills, revenue, expenses, payroll, GST/HST, aging, or a QuickBooks Online company — or asks to draft a reply to a client's accounting question. Answers come from the Auditable Books MCP tools (list_clients, answer_client_question), never from memory.
---

# Answer a client's question from their books

You are helping an accountant respond to a question about one of their clients. Auditable Books answers from that client's **actual QuickBooks Online books** and returns the **evidence** behind every figure. Your job is to route the question, then present the answer and its evidence so the accountant can sign their name to it.

## Steps

1. **Identify the client.** If the user named a client, call `answer_client_question` with that name; the server matches by name or clientId. If no client is named, or the server reports the name is ambiguous or unknown, call `list_clients` and ask the user which one. Never guess between two clients.
2. **Ask the books.** Call `answer_client_question` with the client and the question as the user (or their client) phrased it. Use `conciseness: "concise"` when the user wants a quick figure; leave the default for anything they will paste into an email.
3. **Present the answer, then the evidence.** Show the `answer` first. Then a short *Evidence* section: which ledger tools ran (from `evidence.toolCalls`) and the Intuit trace ids (`evidence.intuitTraceIds`) if any. If the answer includes `clarification`, put that question to the user instead of guessing.
4. **If asked to draft a reply to the client,** write it in the accountant's voice, keep every number exactly as returned, and end with a line noting the figures come from the client's books as of today. The accountant sends it; you never do.

## Rules

- **Never invent, round differently, or "correct" a figure.** If the tool did not return it, say the books did not show it.
- **One client per question.** For a cross-client question, run one call per client and say so.
- **Read-only.** These tools never change the books. If the user asks to post, edit or delete anything in QuickBooks, say that has to be done in Auditable Books or QuickBooks with a human approving it.
- **Credit.** If a call returns `credit_exhausted`, tell the user to top up in Auditable Books → Settings → Plan. Do not retry.
- **Reconnect.** If a client is reported as needing reconnection, tell the user to reconnect that client in Auditable Books; do not try another client silently.
- Do not paste raw JSON at the user. Tables in `answer.contentData` render well as Markdown tables.
