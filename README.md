# 🌍 United States Greenhouse‑Gas Emissions Analysis (2023)

## 📝 Project Overview

This project analyzes county‑level greenhouse gas (GHG) emissions in the United States using the **United States Emissions 2023** dataset provided by the Environmental Protection Agency.  The dataset contains 3,142 county records with fields for state and county identifiers, population, employment counts, Department of Energy climate zone classifications, electricity and natural‑gas consumption, vehicle‑miles travelled, and GHG emissions measured in *million tons of CO₂‑equivalent (mtons CO₂e)*.  National averages across these counties include a total population of **318,558,162 people**, an average of **15,532 vehicle miles travelled per person** (population‑weighted ~10 084 miles per person), average county emissions of **179,012 mtons CO₂e**, and an average climate zone number of **4.28**, indicating that the typical county falls in a mixed or cold climate.

The analysis replicates SQL queries written in a Databricks notebook, summarizes the results as charts and tables, and supplements them with insights from authoritative sources such as the EPA, EIA, IEA, and C2ES.  Business‑oriented observations and recommendations are provided in the conclusion.

---

## 🎯 Business Objective

Understanding where and why emissions occur is critical for designing effective decarbonisation strategies.  This analysis helps:

* Identify **states and counties with the highest total emissions**, concentrating efforts where reductions will have the greatest impact.
* Examine **emissions intensity per person** and **electricity consumption per person** to reveal states whose generation mix or consumption patterns drive high per‑capita emissions.
* Explore how **climate zones** influence emissions and inform building and infrastructure strategies.
* Provide **actionable recommendations** for businesses and policy‑makers to prioritise investments in renewable energy, efficiency, and transport electrification based on empirical evidence.

---

## 🧰 Tools and Resources

This project was developed in **Databricks** using Spark SQL for data preparation and analysis.  Visualisations and interpretation were exported to a **PDF dashboard** for distribution.  External research from the EPA, EIA, IEA, and C2ES was used to validate findings and provide a broader context.

---

## 📦 Dataset Description

The dataset comprises one row per U.S. county with the following key features:

* **State & county identifiers** – FIPS codes used to aggregate data at the state and county levels.
* **Population & employment counts** – 2023 estimates used to compute per‑capita metrics.
* **DOE climate zone (1‑8)** – a numerical classification describing a county’s climate regime (hot‑humid, mixed‑humid, cold, very cold, etc.).
* **Electricity & natural‑gas consumption** – energy use measured in MWh and therms.
* **Vehicle‑miles travelled (VMT)** – average miles driven per person.
* **GHG emissions** – total emissions in *million tons CO₂e*.

---

## 🧹 Data Preparation

1. **Schema creation and ingestion:** The raw EPA file was ingested into Databricks, and schema definitions were applied to normalize numeric fields and date columns.  County‑level emissions were aggregated to the state level using `GROUP BY` queries.
2. **Join lookup tables:** Population and DOE climate zone lookup tables were joined to compute per‑capita emissions and assign climate‑zone descriptions.
3. **Calculated columns:** Emissions per person (mtons CO₂e / population) and electricity consumption per person (MWh / population) were derived to enable fair comparisons across states.
4. **External data integration:** National and sectoral emissions trends were integrated from the EIA and IEA to contextualise the 2023 county data.

---

## 📊 Analysis & Insights

### 🌐 Top States by Total Emissions

Summing emissions by state reveals that total GHG emissions are **extremely concentrated**.  Texas and Florida alone account for roughly a quarter of total emissions.  The table below lists the top-emitting states and their drivers.

<img width="810" height="481" alt="10 State Emissions" src="https://github.com/user-attachments/assets/b3e53294-f14a-4314-91d3-473a7eee1377" />

| Rank | State | Total GHG emissions (mtons CO₂e) | Discussion |
|---|---|---|---|
| **1** | **Texas** | **59.0 million** | Texas’s dominance reflects its large population and energy‑intensive industrial base, particularly petrochemical and refining operations. The state’s electricity mix still has significant natural‑gas and coal generation. |
| **2** | **Florida** | **47.1 million** | Emissions stem mainly from transportation and electricity demand for cooling due to its hot‑humid climate. Rapid population growth drives energy consumption. |
| **3** | **Ohio** | **27.8 million** | Relies heavily on coal‑fired power and heavy industry; per‑capita emissions are high because of manufacturing and modest renewable penetration. |
| **4** | **Illinois** | **26.1 million** | The Chicago metropolitan area contributes substantial emissions. The power sector is transitioning from coal to natural gas and wind, but industrial emissions remain significant. |
| **5** | **Georgia** | **24.5 million** | Rapid economic and population growth, plus a warm climate that drives cooling demand. |
| **6–10** | **Missouri, California, Tennessee, Pennsylvania, North Carolina** | **19–21 million each** | These states round out the top ten; California appears lower here than in EIA rankings because the county‑level dataset omits some large point sources. |

📌 **Insight:** National decarbonisation strategies must pay particular attention to *Texas*, *Florida*, *Ohio*, *Illinois* and *Georgia*.  Drivers differ – industrial processes dominate in Texas and Ohio, whereas residential cooling and transport are key in Florida, implying that mitigation strategies should be tailored.

### 🏙️ Counties with the Highest Emissions

County‑level analysis highlights specific hotspots.  Four of the top ten counties are in Texas, and two are in Florida.

<img width="1124" height="420" alt="Top Counties" src="https://github.com/user-attachments/assets/26eacee1-ccc9-43f8-bdb4-e103fb95ca00" />


| Rank | County (State) | Population (millions) | Emissions (mtons CO₂e) | Possible drivers |
|---|---|---|---|---|
| **1** | **Maricopa County, AZ** | 4.09 | **9.81 m** | Large population centred in Phoenix; significant urban sprawl increases vehicle‑miles travelled and electricity use for cooling. |
| **2** | **Harris County, TX** | 4.43 | **9.68 m** | Home to Houston’s petrochemical complex and shipping industry. |
| **3** | **Cook County, IL** | 5.23 | **8.11 m** | Chicago area; high industrial output and dense population. |
| **4** | **Miami‑Dade County, FL** | 2.66 | **5.62 m** | Transportation and tourism drive emissions; demand for air‑conditioning is high. |
| **5** | **Dallas County, TX** | 2.51 | **5.43 m** | Urban transportation and electricity demand. |
| **6–10** | **Los Angeles (CA), Tarrant (TX), Broward (FL), Clark (NV), Bexar (TX)** | 1.85–10.06 | **3.8–5.0 m** | Major metropolitan areas with high energy use, transportation activity, and industrial processes. |

📌 **Insight:** Urban counties with large populations and heavy industrial or petrochemical activity contribute disproportionately to emissions.  Targeted urban sustainability measures—such as electrified public transit, renewable electricity procurement, and stringent building efficiency standards—can deliver substantial reductions.

### 🌡️ Emissions by Climate Zone

<img width="810" height="481" alt="Climate Zone Emissions" src="https://github.com/user-attachments/assets/356f85e2-2d6f-4cf8-a15f-e8ddf8d584d0" />

Each county is assigned a Department of Energy (DOE) climate zone.  **Zone 2 (hot‑humid)** counties in Florida and Texas produce the highest total emissions, followed by **Zone 3 (mixed‑humid/hot‑dry)** counties in Texas and Ohio.  Warm climate zones exhibit high electricity demand for cooling and rely on fossil‑fuel generation, whereas cold and very‑cold zones emit less but have smaller populations and more renewables.  

🔧 **Strategy:** In hot‑humid zones, investments in high‑efficiency air‑conditioning, reflective roofing, and insulation can reduce electricity demand.  In cold zones, electrified heating and building envelope upgrades are essential.

### ⚡ Emissions and Electricity Consumption per Person

The scatter plot of state‑level emissions per person versus electricity consumption per person shows a positive correlation—states that consume more electricity per person tend to emit more.  Outliers such as Tennessee and West Virginia consume less electricity per person than Alaska or Louisiana, but still have high emissions because their generation mixes are dominated by coal.

<img width="809" height="479" alt="State Emissions" src="https://github.com/user-attachments/assets/d5144e3b-80f5-4470-a447-fe0dfa00bbb3" />

#### States with High Per‑Capita Emissions

| Rank (per capita) | State | Emissions per person (mtons CO₂e) | Avg electricity use per person (MWh) | Observations |
|---|---|---|---|---|
| **1** | **Missouri (MO)** | **3.51** | **4.90** | Missouri relies heavily on coal‑fired power; renewable penetration is low.  EIA per‑capita rankings show that coal‑heavy states such as Wyoming and West Virginia top the list. |
| **2** | **Tennessee (TN)** | **3.13** | **5.78** | High reliance on coal and natural gas; consumption is above average due to industrial load. |
| **3** | **West Virginia (WV)** | **2.96** | **5.21** | Dominated by coal mining and coal‑fired power; small population yields high per‑capita emissions. |
| **4** | **Kentucky (KY)** | **2.91** | **5.35** | Coal remains a major energy source; industrial emissions are significant. |
| **5** | **Oklahoma (OK)** | **2.90** | **5.10** | Oil and gas production drives industrial emissions; electricity generation includes coal and natural gas. |

States with lower per‑capita emissions (not shown) include **California**, **New York**, and **Maryland**, which have aggressive renewable portfolios and energy‑efficiency policies.

📌 **Insight:** Per‑capita emissions depend on both consumption and the **carbon intensity** of electricity.  States such as Washington consume a lot of electricity but use abundant hydropower, resulting in low emissions per person.

### 🚗 Vehicle Miles Travelled & Transportation Emissions

The dataset shows an unweighted average of **15,532 vehicle miles per person**, but when weighted by population, the national average falls to about **10,084 miles per person**, aligning closely with external estimates of **9,745 miles per person in 2023**.  According to the EPA, **transportation is the largest direct source of U.S. GHG emissions**.  The EIA reports that **total energy‑related CO₂ emissions declined by 3 % in 2023**, largely due to reductions in electric‑power emissions; emissions from transportation and industry were essentially flat.

🔧 **Strategy:** Businesses with large transport fleets should reduce vehicle‑miles travelled and switch to electric vehicles (EVs).  Federal policies are accelerating this transition—EVs represented about **9.5 % of light‑duty vehicle sales in 2023**.  Tax credits and incentives for clean vehicles can support fleet electrification and charging infrastructure investments.

### 📉 National Emissions Trends & Policy Context

External sources provide critical context for interpreting the 2023 data:

* **Declining national emissions:** U.S. net GHG emissions fell **17.4 % between 2005 and 2023**; energy‑related CO₂ emissions are **7 % below pre‑pandemic levels**, and the electric‑power sector’s emissions have dropped **41 %** due to shifts from coal to natural gas and renewables.
* **Sectoral composition:** Transportation, electric power, industry, commercial/residential, and agriculture contributed roughly **30 %, 24 %, 23 %, 13 %, and 10 %** of total U.S. GHG emissions, respectively.
* **Policy landscape:** The Inflation Reduction Act and subsequent legislation offer generous tax credits for renewable generation, energy storage, industrial decarbonisation, and EVs.  Renewable energy’s share of U.S. electricity generation was **22 % in 2023** and is projected to reach **34 % by 2028**.

---

## 💡 Recommendations

1. **Prioritise high‑impact regions and sectors:** Focus sustainability initiatives on the top emitting states and counties.  Businesses operating in Texas, Florida, Ohio, and Illinois should evaluate their energy supply mix and consider procuring renewable electricity, on‑site generation (solar, wind, or battery storage), and demand‑side management.
2. **Improve building efficiency:** Tailor building strategies to climate zones.  In hot‑humid areas (e.g., Florida, Texas), invest in high‑efficiency cooling, reflective roofing, and insulation; in cold zones, prioritise efficient heating and building envelopes.
3. **Electrify transport and logistics:** Plan for fleet electrification and optimise logistics to reduce miles travelled.  Take advantage of federal incentives for clean vehicles and charging infrastructure.
4. **Monitor policy incentives and market trends:** Align corporate energy strategies with emerging incentives under the Inflation Reduction Act and related legislation.  Renewable energy is expected to grow rapidly, and moving early can lower long‑term costs and hedge against carbon‑pricing risks.
5. **Report and benchmark emissions transparently:** Publicly report emissions performance relative to peers and national averages.  Use per‑capita and per‑unit production metrics for fair comparisons and leverage EPA tools like the Facility Level Information on Greenhouse Gases (FLIGHT) for benchmarking.

---

## 🧾 Conclusion

The 2023 U.S. emissions dataset illustrates both the **concentration of emissions** in a handful of states and counties and the **diversity of drivers** across sectors and climates.  National emissions are declining thanks to shifts away from coal and improvements in energy efficiency, yet transportation and industrial sectors remain stubbornly high.  Business stakeholders can use these insights to prioritise investments in renewable energy, efficiency, and electrification, thereby aligning corporate strategies with national decarbonisation pathways and regulatory incentives.

---

## 🖼 Dashboard Preview

![Emissions Dashboard Using Databricks_page-0002](https://github.com/user-attachments/assets/ec7aa187-a0c1-4645-9206-c4ef7a776ca8)

---

## 📚 References

**Choose Energy.** *U.S. Carbon Emissions Report*. Available at: https://www.chooseenergy.com/data-center/carbon-dioxide-by-state/. Accessed January 26, 2026.

**Department of Energy.** *Climate Zones*. U.S. Department of Energy, Energy Efficiency and Renewable Energy. Available at: https://www.energy.gov/eere/buildings/climate-zones. Accessed January 26, 2026.

**Eno Center for Transportation.** *VMT Back to Pre-COVID Level in 2023, but Still Lags Per Capita*. Available at: https://enotrans.org/article/vmt-back-to-pre-covid-level-in-2023-but-still-lags-per-capita/. Accessed January 24, 2026.

**U.S. Environmental Protection Agency.** *Sources of Greenhouse Gas Emissions*. Available at: https://www.epa.gov/ghgemissions/sources-greenhouse-gas-emissions. Accessed January 24, 2026.

**U.S. Energy Information Administration.** *U.S. energy‑related CO₂ emissions decreased by 3 % in 2023*. Available at: https://www.eia.gov/todayinenergy/detail.php?id=61928. Accessed January 25, 2026.

**International Energy Agency.** *United States 2024 – Executive Summary*. Available at: https://www.iea.org/reports/united-states-2024/executive-summary. Accessed January 25, 2026.

**Center for Climate and Energy Solutions (C2ES).** *U.S. Emissions*. Available at: https://www.c2es.org/content/u-s-emissions/. Accessed January 26, 2026.
