graph LR
    subgraph PAYOR FLOW
        A["<b>FORM 2307</b><br>FILLED UP BY: PAYOR"] --> B["<b>QAP</b><br>ATTACHED TO: Form 1601-EQ"]
        B --> C["<b>Annual QAP</b><br>ATTACHED TO: Form 1604-E"]
        
        B -- "Submitted Via" --> D["<b>BIR eSubmission</b><br>(esubmission@bir.gov.ph)"]
        C -- "Submitted Via" --> E["<b>BIR eSubmission or eFPS portal</b>"]
    end
    
    style A fill:#bde0fe,stroke:#333,stroke-width:1px
    style B fill:#bde0fe,stroke:#333,stroke-width:1px
    style C fill:#bde0fe,stroke:#333,stroke-width:1px
    style D fill:#bde0fe,stroke:#333,stroke-width:1px
    style E fill:#bde0fe,stroke:#333,stroke-width:1px