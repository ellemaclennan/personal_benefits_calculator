# Health Plan Sensitivity & Cost Analysis

This Jupyter notebook analyzes different employee healthcare plans (Platinum PPO, Gold PPO, Silver HDHP) to determine which plan minimizes total yearly cost based on medical usage patterns.

It generates heatmaps that visualize which plan is most cost-effective under varying levels of specialist visits, prescription fills, and major medical events.

## Features
**Three Plan Cost Models:**

- Implements cost formulas for each Serán plan:
  - **Platinum PPO** — higher premiums, low copays  
  - **Gold PPO** — low premiums, moderate copays  
  - **Silver HDHP (HSA)** — low premiums, full cost until deductible, with employer HSA contribution  
- Incorporates:
  - Deductibles, out-of-pocket limits, copays, and HSA credits
  - In-network cost assumptions
  - Single Employee

**Personalized best plan calculator:**

- Uses your predicted healthcare events to calculate your total yearly costs accross all three plans

- Prints the lowest cost plan based on these parameters

    My results (based on my prescriptions and provider visits)
    ```
    The costs for my specific scenario across different plans are:
    Platinum: $4,439.52
    Gold: $2,400.00
    Silver: $3,330.00

    Therefore, the best healthcare plan for my specific scenario is Gold at $2,400.00
    ```

**Sensitivity Matrix -  Calculates total yearly cost across combinations of:**

- Specialist visits per year

- Prescription (Rx) fills per year

    ### Data Structure
    The sensitivity analysis generates a DataFrame with these columns:
    - `Specialist_Visits`: Number of specialist visits
    - `Rx_Fills`: Number of prescription fills (Tier 2 approximation for grid)
    - `MajorEvent`: Major medical event cost
    - `Platinum`: Total cost under Platinum plan
    - `Gold`: Total cost under Gold plan
    - `Silver_NetHSA`: Total cost under Silver HDHP (net of HSA contribution)
    - `BestPlan`: Which plan has the lowest cost for this scenario


**Heatmaps:**

- Best Plan (categorical) — shows which plan is cheapest for each usage combination.

- Total Cost Gradient — numeric heatmap of yearly cost for a single plan (Gold by default).

- Major Event Simulation — recalculates all plans assuming a one-time $9000 medical event. Also included a medium event simulation at $4500.

## Key Assumptions

### Sensitivity Grid (Heatmaps)
- Uses **Tier 2 copays/costs as approximation** for prescription medications
- Assumes average specialist visit cost of $400
- Assumes average Tier 2 prescription cost of $120
- Major event costs tested: $0, $4,500, $9,000
### Important Notes
- **Platinum and Gold:** Copays apply from day one (before deductible)
- **Silver HDHP:** You pay full cost until deductible is met
- **Major events:** For Platinum/Gold, you pay the deductible first, then coinsurance percentage
- **Out-of-pocket max:** All plans cap your total annual out-of-pocket costs

## Interpreting Results

### When Silver HDHP Wins
- Low annual usage (few visits, few prescriptions)
- The $1,400 HSA contribution offsets costs
- Best for healthy individuals with minimal healthcare needs
- Best in Major healthcare event scenario

### When Gold PPO Wins
- Moderate to high usage
- Lower premium than Platinum
- Copays are reasonable ($30 specialist, $30 Tier 2 Rx)
- Best for individuals with routine healthcare visits and expensive prescription medications

### When Platinum PPO Wins
- Extremely high usage (many visits + many prescriptions)
- The $2,700 premium difference needs to be offset by copay savings
- Rarely optimal unless usage is very high or major event occurs

## Customizing the Analysis

### Change Usage Ranges
Modify these ranges to explore different scenarios:

```python
specialist_visits = list(range(0, 36, 3))  # 0, 3, 6, 9... 24 visits
rx_fills = list(range(0, 48, 3))            # 0, 3, 6, 9... 24 fills
major_event_costs = [0, 4500, 9000]         # Different event cost scenarios
```

### Adjust Plan Parameters
If your employer's plan details differ, update these dictionaries:

```python
premiums = {"Platinum": 3179.52, "Gold": 480.0, "Silver": 480.0}
deductibles = {"Platinum": 500, "Gold": 2500, "Silver": 4250}
oop_max = {"Platinum": 4000, "Gold": 7500, "Silver": 4250}
# ... etc.
```

### Modify Cost Assumptions
Update these to your OOP rx costs:

```python
office_visit_cost = 400      # Average cost per specialist visit
tier_1_cost = 128
tier_2_cost = 1080
tier_3_cost = 0          
```
Update these to your yearly number of visits/rx fills/major event assumptions
```python
# CHANGE THIS to your predicted # of visits for the year 
visits = 7 + 3 

# CHANGE THIS to your predicted # of prescription fills for each tier
tier_1_rx = 12 
tier_2_rx = 10
tier_3_rx = 0

# CHANGE THIS to your expected amount
major_event_cost = 1200 #gave myself a $1200 budget for a small accident
```

## Limitations

1. **Tier 2 Approximation:** The sensitivity grid uses only Tier 2 costs for simplicity. For precise calculations, use the personal cost calculator with your actual tier distribution.

2. **Average Costs:** Specialist visits and prescription costs are averaged. Actual costs may vary.

3. **Static Analysis:** Doesn't account for drastic health changes or variable monthly usage.

4. **Network Restrictions:** Assumes all providers are in-network.

5. **Other Services:** Doesn't model primary care visits, lab tests, imaging, or other services separately.
   
6. **HSA Use:** Assumes entirety of HSA goes towards healthcare costs. Investment accounting is not modeled. 

## Installation & Setup
1. Clone Repository.
   
2. **Install Dependencies**

Make sure you have Python ≥3.10 and these packages installed:
```bash
pip install pandas numpy matplotlib seaborn jupyter
```

3. Open the Notebook

From a terminal or VS Code:
```
jupyter notebook health_plan_heatmaps.ipynb
```
4. Change the variables to your specific healthcare needs. Refer to ``Customizing the Analysis`` section.

5. Click Run -> Run all Cells

## License

This is a personal analysis tool. Modify freely for your own healthcare planning needs.

## Disclaimer

This analysis is for informational purposes only and should not be considered financial or medical advice. Always review your actual plan documents and consult with HR or a benefits advisor before making enrollment decisions.

## Author
Elle MacLennan

---


**Last Updated:** November 2025  



