# Lensoplus AI — AI-Powered E-commerce Sales Consultation

**An n8n-based conversational sales assistant for a photography and content-creation equipment store.**

**Status:** Functional local prototype, demonstrated with a multi-turn shopping scenario. Public production deployment and business-impact measurements are not claimed.

## Demo

[Watch the 84-second demo](assets/demo.mp4)

![Actual n8n consultation workflow](assets/workflow.png)

## The challenge

Customers shopping for production equipment often need more than a keyword search. They want recommendations that account for their use case, budget, existing devices, and the compatibility of multiple items. A generic chatbot may invent product details or fail to update a shopping bundle when requirements change.

## The solution

Lensoplus AI collects customer requirements in Persian, uses an AI agent to search a structured PostgreSQL product catalogue, and presents recommendations as product cards containing verified product links, prices, and availability information from the store's data. Follow-up requests can add or remove products while preserving the customer's stated priorities and budget.

## Demonstrated capabilities

- **Multi-turn consultation:** The assistant remembers the context of the ongoing conversation.
- **Product discovery:** Search via PostgreSQL using customer needs, product categories, and budget.
- **Bundle recommendations:** Suggest combinations of equipment for real scenarios.
- **Iterative bundle updates:** Add or remove items without restarting the consultation.
- **Deterministic budget calculations:** Recompute totals and remaining budget in JavaScript.
- **Structured responses:** Format answers and product cards through a structured-output pipeline.
- **Product URL verification:** Restore exact product URLs from the database before responding.
- **Input validation:** Check incoming requests and return explicit errors for invalid inputs.
- **Compatibility caveats:** Flag missing specifications rather than claiming unverified device compatibility.

## Technology

| Layer | Technology |
| --- | --- |
| Workflow orchestration | n8n (self-hosted) |
| Language model | OpenAI API |
| Product retrieval | PostgreSQL |
| Store integration | WooCommerce product catalogue synchronization |
| Business logic | JavaScript |
| Demo frontend | HTML, CSS, JavaScript |

## Consultation architecture

```text
Customer web chat
    ↓
Webhook — Receive Consultation
    ↓
Code — Validate Consultation Request
    ↓
IF — Consultation Request Valid
    ├── False → Respond — Invalid Consultation
    └── True  → Code — Prepare Search Inputs
                 ↓
               AI Agent
                 ├── OpenAI Chat Model
                 ├── Execute a SQL query in Postgres (tool)
                 └── Structured Output Parser
                 ↓
               Postgres — Restore Exact Product URLs
                 ↓
               Code — Merge Exact Product URLs
                 ↓
               Respond to Webhook — Return Answer
```

A separate workflow synchronizes product catalogue data into PostgreSQL. The consultation workflow reads that data; the demo does not represent a completed order or payment integration.

## Demo scenario

A clothing-store owner asks for equipment to create mobile-first Instagram content inside their shop.

1. The assistant identifies lighting, audio, and camera-support needs, and asks about budget and phone model.
2. Given a **60,000,000 toman** budget, it recommends a lighting kit and wireless microphone: **34,250,000 toman** total.
3. The customer adds a smartphone gimbal; the package becomes **52,950,000 toman**.
4. The customer removes the microphone; the package is recalculated to **44,500,000 toman**, leaving **15,500,000 toman**.
5. The assistant explains lighting limitations and what to check before purchasing a smartphone gimbal.

Prices and stock shown in the demo represent the store data at the time of testing, not live price guarantees.

## Development scope and limitations

- Tested locally with realistic customer conversations.
- Device compatibility is flagged when the exact phone model is unknown.
- Security hardening, rate limiting, hosting, monitoring, and broader production testing are future deployment work.
- No sales uplift, conversion-rate improvement, or deployment to public production has been measured or claimed.
- Credentials, customer records, and internal database exports are not included.

## My contribution

**Rasoul Dashti Nejad — AI Automation & Agent Developer**

Workflow architecture, n8n orchestration, conversational agent behavior, PostgreSQL product retrieval, structured output handling, validation, deterministic pricing logic, and integration with the demo frontend.

## Note

This repository is a **portfolio case study**, not a complete public release of the production workflow. It contains the demo and architectural documentation, without private credentials or proprietary store data.
