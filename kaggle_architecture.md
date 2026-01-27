# Medallion Architecture (Bronze → Silver → Gold)

This notebook documents a **Medallion (Bronze/Silver/Gold)** architecture for ingesting CSV files into OneLake, transforming with Dataflow Gen2, and serving analytics via a modeled warehouse and Power BI semantic model.

---

## Architecture Diagram (Mermaid)

```mermaid
flowchart TD

  %% BRONZE
  A[📥 CSV Files<br/>Uploaded from PC] --> B[🥉 Bronze_LandingStage_LH<br/>Raw Delta in OneLake]

  %% SILVER
  B --> C[🥈 Silver_dfgen2<br/>Dataflow Gen2<br/>Cleaning • Typing • Dedupe • Star Schema Prep]

  C --> D1[🥈 Silver_LH (Warehouse)<br/>Curated Delta Tables<br/>Accessible in Warehouse]
  C --> D2[🥈 Silver_LH (Notebook)<br/>Curated Delta Tables<br/>Accessible in Notebook]

  D1 --> E1[TeamA_ws<br/>BI Team]
  D2 --> E2[TeamB_ws<br/>Data Science Team]

  %% GOLD
  D1 --> F[Pipeline CopyJob → 🏆 Gold_DWH<br/>Dim/Fact Modeled Tables<br/>SQL Interface]

  %% BI
  F --> G[📊 Power BI<br/>Enterprise Semantic Model<br/>ERD • KPIs • DAX]

  %% Quick Overview
  subgraph Overview[Quick Overview]
    B2[🥉 Bronze_LandingStage_LH]
    C2[🥈 Silver_dfgen2 → Silver_LH]
    F2[🏆 Gold_DWH]
  end

  B2 --> C2 --> F2
```

---

## Notes

- Use `<br/>` for line breaks inside Mermaid node labels (more reliable than `\n` across renderers).
- If Mermaid does not render in your Kaggle view, paste the same code into a GitHub README (GitHub renders Mermaid by default).
