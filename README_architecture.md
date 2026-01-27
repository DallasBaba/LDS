# Medallion Architecture (Bronze → Silver → Gold)

This repository documents a **Medallion (Bronze/Silver/Gold)** architecture for ingesting CSV files into OneLake, transforming with Dataflow Gen2, and serving analytics via a modeled warehouse and Power BI semantic model.

✅ The diagram below is written in **Mermaid** and will render automatically on GitHub.

---

## Architecture Diagram (Mermaid)

```mermaid
flowchart TD

  %% ===================================================================
  %% BRONZE
  %% ===================================================================
  A[📥 CSV Files<br/>Uploaded from PC] --> B[🥉 Bronze_LandingStage_LH<br/>Raw Delta in OneLake]

  %% ===================================================================
  %% SILVER
  %% ===================================================================
  B --> C[🥈 Silver_dfgen2<br/>Dataflow Gen2<br/>Cleaning • Typing • Dedupe • Star Schema Prep]

  C --> D1[🥈 Silver_LH (Warehouse)<br/>Curated Delta Tables<br/>Accessible in Warehouse]
  C --> D2[🥈 Silver_LH (Notebook)<br/>Curated Delta Tables<br/>Accessible in Notebook]

  %% Teams / Workspaces
  D1 --> E1[TeamA_ws<br/>BI Team]
  D2 --> E2[TeamB_ws<br/>Data Science Team]

  %% ===================================================================
  %% GOLD
  %% ===================================================================
  D1 --> F[Pipeline CopyJob → 🏆 Gold_DWH<br/>Dim/Fact Modeled Tables<br/>SQL Interface]

  %% BI Consumption
  F --> G[📊 Power BI<br/>Enterprise Semantic Model<br/>ERD • KPIs • DAX]

  %% -------------------------------------------------------------------
  %% Quick overview section
  %% -------------------------------------------------------------------
  subgraph Overview[Quick Overview]
    B2[🥉 Bronze_LandingStage_LH]
    C2[🥈 Silver_dfgen2 → Silver_LH]
    F2[🏆 Gold_DWH]
  end

  B2 --> C2 --> F2
```

---

## What Each Layer Does

- **Bronze (Landing)**: Raw CSV landing zone stored as Delta in OneLake for traceability and replay.
- **Silver (Curated)**: Cleaned, typed, deduplicated, conformed structures prepared for analytics (including star-schema prep).
- **Gold (Serving)**: Modeled dimensional warehouse (Dim/Fact) optimized for SQL consumption and enterprise BI.

---

## How to Use

1. Copy this `README.md` into your GitHub repository root (or use the file included here).
2. GitHub will automatically render the Mermaid diagram.
3. Optionally, extend the repo with:
   - `docs/` for data dictionary and standards
   - `sql/` for DDL/views
   - `etl/` for pipelines/notebooks
