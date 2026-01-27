# Medallion Architecture (Bronze -> Silver -> Gold)

This README uses a **high-compatibility Mermaid diagram** (no emojis, no special arrows, no HTML line breaks) so it renders reliably on GitHub.

---

## Architecture Diagram (Mermaid)

flowchart TD

  %% ==========================================================
  %% BRONZE
  %% ==========================================================
  A[CSV Files<br/>Uploaded from PC] --> B[Bronze_LandingStage_LH<br/>Raw Delta in OneLake]

  %% ==========================================================
  %% SILVER
  %% ==========================================================
  B --> C[Silver_dfgen2<br/>Dataflow Gen2<br/>Cleaning • Typing • Dedupe • Star Schema Prep]

  C --> D1[Silver_LH (Warehouse)<br/>Curated Delta Tables<br/>Accessible in Warehouse]
  C --> D2[Silver_LH (Notebook)<br/>Curated Delta Tables<br/>Accessible in Notebook]

  %% Teams / Workspaces
  D1 --> E1[TeamA_ws<br/>BI Team]
  D2 --> E2[TeamB_ws<br/>Data Science Team]

  %% ==========================================================
  %% GOLD
  %% ==========================================================
  D1 --> F[Pipeline CopyJob → Gold_DWH<br/>Dim/Fact Modeled Tables<br/>SQL Interface]

  %% BI Consumption
  F --> G[Power BI<br/>Enterprise Semantic Model<br/>ERD • KPIs • DAX]

  %% ----------------------------------------------------------
  %% Quick overview section
  %% ----------------------------------------------------------
  subgraph Overview[Quick Overview]
    B2[Bronze_LandingStage_LH]
    C2[Silver_dfgen2 → Silver_LH]
    F2[Gold_DWH]
  end

  B2 --> C2 --> F2

```mermaid
flowchart TD
  A[CSV Files uploaded from PC] --> B[Bronze_LandingStage_LH: Raw Delta in OneLake]
  B --> C[Silver_dfgen2: Dataflow Gen2 transforms]
  C --> D1[Silver_LH Warehouse: Curated Delta tables]
  C --> D2[Silver_LH Notebook: Curated Delta tables]
  D1 --> E1[TeamA_ws: BI Team]
  D2 --> E2[TeamB_ws: Data Science Team]
  D1 --> F[CopyJob Pipeline -> Gold_DWH: Dim/Fact tables (SQL)]
  F --> G[Power BI: Semantic model (ERD, KPIs, DAX)]

  subgraph Overview[Quick Overview]
    O1[Bronze]
    O2[Silver]
    O3[Gold]
  end

  O1 --> O2 --> O3
```

---

## Layer Summary

- **Bronze**: Raw landing in OneLake (Delta) for traceability and replay.
- **Silver**: Clean/typed/deduped curated tables for analytics readiness.
- **Gold**: Modeled Dim/Fact warehouse for SQL + BI consumption.
