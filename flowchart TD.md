flowchart TD
    %% Styling Definitions
    classDef system fill:#d4e6f1,stroke:#2874a6,stroke-width:2px;
    classDef payor fill:#d1f2eb,stroke:#117a65,stroke-width:2px;
    classDef payee fill:#fdebd0,stroke:#d68910,stroke-width:2px;
    classDef bir fill:#fadbd8,stroke:#cb4335,stroke-width:2px;
    classDef extra fill:#ebdef0,stroke:#6c3483,stroke-width:2px;
    classDef manual fill:#fcf3cf,stroke:#b7950b,stroke-width:1px,stroke-dasharray: 5 5;

    %% 1. DATA INPUT & AUTOMATION
    subgraph FullSuite_Processing ["1. FullSuite Core Automation"]
        direction TB
        FS_In["User Uploads Raw Data<br>(Standard CSV Files)"]:::system
        FS_Val{"System Validation<br>Errors in CSV?"}:::system
        FS_Err["Block Generation<br>Highlight Row Error"]:::system
        
        FS_Conv["Format Conversion Engine<br>(Translates to .dat format)"]:::system
        FS_2307["Form 2307 Generator<br>(Compiles to PDF)"]:::system
        
        FS_In --> FS_Val
        FS_Val -- "Yes" --> FS_Err
        FS_Err -. "Fix & Re-upload" .-> FS_In
        FS_Val -- "Clean Data" --> FS_Conv
        FS_Val -- "Clean Data" --> FS_2307
    end

    %% 2. 2307 LIFECYCLE (PAYOR/PAYEE LOOP)
    subgraph Doc_Review ["2. Form 2307 Lifecycle & Review Loop"]
        direction TB
        P1["Payor issues 2307 PDF<br>(Saves copy for Audit Trail)"]:::payor
        P2["Payee Accounting Team<br>Receives & Verifies 2307<br>(TIN, Name, Math, Signature)"]:::payee
        
        Check2307{"Are there errors?"}:::manual
        Reject["Payee Rejects Form<br>(Only Payor can fix)"]:::payee
        Accept["Payee Accepts & Holds<br>until Tax Season"]:::payee
        
        FS_2307 --> P1
        P1 --> P2
        P2 --> Check2307
        Check2307 -- "Yes" --> Reject
        Reject -. "Requests Fix / Cancels Old" .-> P1
        Check2307 -- "No, Math Matches" --> Accept
    end

    %% 3. QUARTERLY/ANNUAL SUBMISSIONS
    subgraph Submissions ["3. Tax Season Submissions to BIR"]
        direction LR
        %% Payor Side
        Sub_Payor_QAP["Payor Submits QAP (.dat)<br>Attached to 1601-EQ/1604-E"]:::payor
        Sub_Payor_Main["Payor Files Main Return<br>(Pays Grand Total)"]:::payor
        
        %% Payee Side
        Sub_Payee_SAWT["Payee Submits SAWT (.dat)<br>Attached to 1701Q/1702Q"]:::payee
        Sub_Payee_2307["Payee Scans 2307 PDFs<br>(Uploads to eAFS)"]:::payee
        
        FS_Conv -- "Payor Path" --> Sub_Payor_QAP
        Accept -- "Summarized into" --> Sub_Payee_SAWT
        Accept -- "Physical/Digital Copies" --> Sub_Payee_2307
    end

    %% 4. BIR VERIFICATION ENGINE
    subgraph BIR_Verification ["4. BIR Cross-Matching Engine"]
        direction TB
        BIR_QAP_In["Receives QAP via<br>esubmission@bir.gov.ph"]:::bir
        BIR_SAWT_In["Receives SAWT via<br>esubmission@bir.gov.ph"]:::bir
        
        CrossMatch{"BIR System<br>Cross-References Data"}:::bir
        Match_Pass["TIN & Amount Match perfectly<br>Approved: Payee gets Tax Discount"]:::bir
        Match_Fail["Discrepancy Found<br>Audit / Rejection"]:::bir
        
        Sub_Payor_QAP --> BIR_QAP_In
        Sub_Payee_SAWT --> BIR_SAWT_In
        
        BIR_QAP_In --> CrossMatch
        BIR_SAWT_In --> CrossMatch
        
        CrossMatch -- "Match" --> Match_Pass
        CrossMatch -- "Mismatch" --> Match_Fail
    end

    %% 5. PARALLEL STREAMS (PAYROLL & VAT)
    subgraph Parallel_Streams ["5. Parallel Tax Streams (VAT & Payroll)"]
        direction TB
        
        %% SLSP Track
        FS_Conv -- "VAT Track" --> VAT_SLSP["SLSP Engine (Tracks 12% VAT)<br>SLS (Sales) + SLP (Purchases)"]:::extra
        VAT_Sub["Submit SLSP (.dat) via<br>esubmission@bir.gov.ph<br>Attached to Form 2550Q"]:::extra
        VAT_SLSP --> VAT_Sub
        
        %% Payroll Track
        FS_Val -- "Payroll Track" --> Pay_Calc["1604-C Annualization Engine<br>Re-computes Jan-Nov Assumed Tax"]:::extra
        Pay_Eval{"December Evaluation"}:::manual
        Pay_Due["Tax Due: Automatically deduct<br>from December Salary"]:::extra
        Pay_Ref["Tax Refund: Automatically add<br>to December Salary"]:::extra
        Pay_Sub["Submit 1604-C + Alphalist<br>via eSubmission/eFPS"]:::extra
        
        Pay_Calc --> Pay_Eval
        Pay_Eval -- "Paid Too Little" --> Pay_Due
        Pay_Eval -- "Paid Too Much" --> Pay_Ref
        Pay_Due --> Pay_Sub
        Pay_Ref --> Pay_Sub
    end