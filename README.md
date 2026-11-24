# Political Swings — Pipeline Execution Order

Run the scripts below in exact sequential order from your project root (`*BASE DIRECTORY*`)[cite: p13n-topic-2]. Raw inputs in `data/` are never modified; all intermediate files are written to `derived/`, and outputs are saved to `figures/` and `tables/`.

---

## Stage 1: Clean & Merge Political Data (Python)
1. **`clean_vdem.py`**
   - **Input:** `data/V-Dem-CY-Full+Others-v16.dta`
   - **Output:** `derived/vdem_clean.dta`
2. **`gen_electionsyears.py`**
   - **Input:** `derived/vdem_clean.dta`
   - **Output:** `derived/vdem_electionyears.dta`
3. **`clean_ideologies.py`** *(run alongside V-Dem cleaning before Step 4)*
   - **Input:** Raw Global Leader Ideologies dataset in `data/`
   - **Output:** `derived/ideologies_clean.dta`
4. **`merge_vdem_ideologies.py`**
   - **Inputs:** `derived/vdem_electionyears.dta`, `derived/ideologies_clean.dta`
   - **Output:** `derived/vdem_ideologies_merged.dta`
5. **`gen_swing_variables.py`**
   - **Input:** `derived/vdem_ideologies_merged.dta`
   - **Output:** `derived/vdem_ideologies_swings.dta`

---

## Stage 2: Clean & Merge Macroeconomic Outcomes (Python)
6. **`clean_outcomes.py`** *(run alongside Stage 1 before Step 7)*
   - **Inputs:** Raw IMF Global Debt, World Bank Inflation, and Penn World Table 11.0 datasets in `data/`
   - **Outputs:** `derived/IMFdebt_clean.dta`, `derived/WBinf_clean.dta`, `derived/PWTgdp_clean.dta`
7. **`merge_outcomes.py`**
   - **Inputs:** `derived/vdem_ideologies_swings.dta`, `derived/IMFdebt_clean.dta`, `derived/WBinf_clean.dta`, `derived/PWTgdp_clean.dta`
   - **Output:** `derived/master_panel.dta`

---

## Stage 3: Descriptive Figures & Summary Tables (Python & Stata)
*(Can be run in any order once `vdem_ideologies_swings.dta` and `master_panel.dta` are built)*

8. **Descriptive Heatmaps (Python)**
   - `plot_lac_elections_heatmap.py` $\rightarrow$ `figures/LAC_elections_heatmap.{png,pdf}`
   - `plot_lac_ideology_heatmap.py` $\rightarrow$ `figures/LAC_ideology_heatmap.{png,pdf}`
   - `plot_lac_major_swings_heatmap.py` $\rightarrow$ `figures/LAC_major_electionswing_heatmap.{png,pdf}`
9. **Election & Ideology Swing Summary Tables (Stata)**
   - `tab_global_swings.do` $\rightarrow$ `tables/global_swings.tex`
   - `tab_regional_swings.do` $\rightarrow$ `tables/regional_swings.tex`
   - `tab_swing_directions.do` $\rightarrow$ `tables/swing_directions.tex`
10. **Outcome Summary Table & Event-Study Figure (Stata)**
    - `tab_outcome_summary_by_ideo.do` $\rightarrow$ `tables/outcome_summary_by_ideology.tex`
    - `fig_gdp_around_major_swing.do` $\rightarrow$ `figures/gdp_around_major_swing.{png,pdf}`
