```mermaid
---
config:
  layout: fixed
---
flowchart LR
 subgraph subGraph0["SELLER FLOW"]
        A["<b>FORM 2307</b><br>RECEIVED FROM: PAYOR"]
        B["<b>SAWT</b><br>ATTACHED TO: Forms 1701Q / 1702Q / 1701 / 1702"]
        C["<b>FORM 2307</b><br>ATTACHED TO: eAFS System"]
        D["<b>BIR eSubmission</b><br>(esubmission@bir.gov.ph)"]
        E["<b>Electronic Audited Financial Statements</b><br>(eAFS System)"]
  end
    A --> B
    B --> C
    B -- Submitted Via --> D
    C -- Submitted Via --> E

    style A fill:#bde0fe,stroke:#333,stroke-width:1px
    style B fill:#bde0fe,stroke:#333,stroke-width:1px
    style C fill:#bde0fe,stroke:#333,stroke-width:1px
    style D fill:#bde0fe,stroke:#333,stroke-width:1px
    style E fill:#bde0fe,stroke:#333,stroke-width:1px
    ```