# etf-global-crsp-compustat-ownership
Merges ETF Global holdings with CRSP and Compustat to compute stock-level ETF ownership for 10 actively managed ETFs (JEPI, DFAC, JEPQ, DYNF, CGDV, AVUV, DFUS, CGGR, DFIV, AVEM) over Jan 1–31, 2024. Produces firm-month ownership (Ben-David, Franzoni &amp; Moussawi, 2018), validation logs, and analysis tables.

# ETF Global Data Merge — ETF Ownership (Jan 2024)

This project merges **ETF Global** holdings with **CRSP** and **Compustat** to compute **ETF ownership** for constituents of 10 actively managed ETFs — JEPI, DFAC, JEPQ, DYNF, CGDV, AVUV, DFUS, CGGR, DFIV, AVEM — over **Jan 1–31, 2024**.

## Objectives
1) Ingest ETF Global holdings snapshots for Jan 2024.
2) Link ETF holdings to CRSP/Compustat identifiers via CCM.
3) Compute firm-month ETF ownership following **Ben-David, Franzoni & Moussawi (2018)**.
4) Output validation logs, summary stats, and final firm-month panel.

## Data Sources
- **ETF Global:** holdings (ETF ticker, holding identifier, shares held, report date).
- **CRSP:** stock data & shares outstanding (SHROUT), PERMNO/CUSIP/TICKER.
- **Compustat (Fundamentals / Security):** GVKEY, name history, security mapping.
- **CRSP–Compustat Merged (CCM):** PERMNO–GVKEY link table for stable crosswalks.

## Method (SAS)
- Clean identifiers; prefer **CUSIP/ISIN**; fall back to TICKER with issuer checks.
- Use CCM to map holdings → PERMNO/GVKEY; restrict to **live, best links** within Jan 2024.
- Align dates to **month-end 2024-01-31** (or nearest available within Jan).
- **Ownership measure (Ben-David et al., 2018):**  
  For each stock *i* in month *t*,  
  `ETF_Ownership_{i,t} = ( Σ_ETFs SharesHeld_{ETF,i,t} ) / SharesOutstanding_{i,t}`  
  Aggregate across the 10 ETFs; also compute market-value weights if needed.
- Handle duplicates (multiple share classes), stock splits, and missing SHROUT.
- Produce diagnostics: matched/unmatched counts, extreme outlier flags.

## Key Outputs
- `results/firm_month_ownership.csv` with:
  - `permno, gvkey, cusip, ticker, month, shrs_out, shrs_etf, etf_own, etf_count`
- `results/summary_stats.csv`: mean/median/std/min/max of `etf_own`
- `results/validation_logs.txt`: link quality, missing IDs, winsorization notes

## Reproducibility
- Language: **SAS**
- Script: `code/sas/etf_ownership_merge_jan2024.sas`
- Period: **2024-01-01 to 2024-01-31**
- ETFs: JEPI, DFAC, JEPQ, DYNF, CGDV, AVUV, DFUS, CGGR, DFIV, AVEM

## Reference
Ben-David, I., Franzoni, F., & Moussawi, R. (2018). *Do ETFs Increase Volatility?* J. Finance.

## License
MIT
