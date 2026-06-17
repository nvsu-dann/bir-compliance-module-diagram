# BIR Compliance Module - Workflow Diagrams

This repository contains the Mermaid.js source codes for the various BIR tax compliance workflows. 

It serves as a central hub to securely store diagram codes (bypassing the Mermaid Live 3-project limit). These codes will also be utilized for future technical specifications to be fed to AI models like Claude.

## 📂 Workflows Included

All workflow codes are located in the `workflows/` directory:

* **[Payor Flow](./workflows/payor-flow.md)**: Illustrates how payors process Form 2307, attach QAP to Form 1601-EQ, and the Annual QAP to Form 1604-E.
* **[Seller Flow](./workflows/seller-flow.md)**: Details how sellers route received Form 2307s either to SAWT (for Forms 1701Q/1702Q/1701/1702) or directly to the eAFS system.
* **[Other Streams / Employees](./workflows/form-1601-C-flow.md)**: Outlines the employee track connecting Form 1601-C and the Annual Alphalist of Employees.
* **[SLSP / VAT Track](./workflows/SLSP-flow.md)**: Maps the VAT process consolidating Sales and Purchase data into the SLSP, attached to Form 2550Q.

## 🛠️ How to Use

1. Navigate to the specific `.md` file in the `workflows/` folder.
2. Copy the Mermaid.js code block.
3. Paste it directly into the [Mermaid Live Editor](https://mermaid.live/) to view, edit, or export the updated visual diagram.
4. Alternatively, copy the code directly into Claude or other LLMs when writing or updating the module's technical specifications.