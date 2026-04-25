# 🏏 ICC Men's T20 Cricket World Cup 2022 — Data Analytics Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Web Scraping](https://img.shields.io/badge/Web%20Scraping-BeautifulSoup-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

## 📌 Project Overview

This project is a comprehensive **Player Performance Analytics Dashboard** built on **ICC Men's T20 Cricket World Cup 2022** data. Using **Python** for web scraping and data preprocessing, and **Power BI** for visualization, the dashboard allows users to:

- 📊 **Analyse & compare** individual player performances across all World Cup matches
- 🧠 **Pick the best Final 11** from the entire tournament pool based on data-driven selection criteria
- 🔍 **Explore players by role** — Power Hitters, Anchors, Finishers, All-Rounders, and Specialist Fast Bowlers

> 💡 **To interact with the dashboard**, download the `.pbix` file from this repository and open it in **Power BI Desktop**.

---

## 📋 Table of Contents

- [Problem Statement](#-problem-statement)
- [Dataset Information](#-dataset-information)
- [Project Workflow](#-project-workflow)
- [Data Collection](#-data-collection)
- [Data Transformation](#-data-transformation)
- [Data Modelling](#-data-modelling)
- [DAX Measures](#-dax-measures)
- [Dashboard Screenshots](#-dashboard-screenshots)
- [Tools & Technologies](#-tools--technologies)

---

## ❓ Problem Statement

Cricket coaches, analysts, and fans often struggle to objectively compare player performances across different teams and conditions. This project solves that by building an **interactive Power BI dashboard** that:

- Reviews and compares **all player performances** from the T20 World Cup 2022
- Enables users to **build their dream Final 11** by selecting players from the full tournament pool
- Evaluates players based on **clearly defined, role-specific selection criteria** (batting average, strike rate, economy, etc.)

---

## 📂 Dataset Information

| Detail | Info |
|--------|------|
| **Source** | [ESPN Cricinfo](https://www.espncricinfo.com/) |
| **Scraping Tool** | Bright Data + BeautifulSoup |
| **Tournament** | ICC Men's T20 World Cup 2022 (Australia) |
| **Coverage** | Qualifier Stage + Super 12 |
| **Data Types** | Match results, batting summaries, bowling summaries, player info |

**Key files used:**
- `t20_batting_summary` — innings-level batting stats per player per match
- `t20_bowling_summary` — innings-level bowling stats per player per match
- `t20_players_info` — player metadata (team, batting style, bowling style, playing role)
- `t20_match_results` — match-level information and match IDs

---

## 🔄 Project Workflow

```
Requirement Scoping
      ↓
Web Scraping (ESPN Cricinfo via Bright Data + BeautifulSoup)
      ↓
Data Cleaning & Preprocessing (Python + Pandas)
      ↓
Data Transformation (Power Query in Power BI)
      ↓
Data Modelling & DAX (Power BI)
      ↓
Dashboard Building (Power BI)
```

---

## 🌐 Data Collection

- All match and player data was scraped from **[ESPN Cricinfo](https://www.espncricinfo.com/)** using **Bright Data** as the proxy/scraping infrastructure and **Beautiful Soup** as the HTML parser.
- Raw scraped data was in **JSON format**, which was converted into **Pandas DataFrames** using **Jupyter Notebook**, and then exported as **CSV files** for further processing in Power BI.

---

## 🧹 Data Transformation

Performed **two stages** of data transformation:

**Stage 1 — Python (Pandas):**
- Corrected player name inconsistencies
- Handled missing values
- Linked match IDs across batting and bowling tables

**Stage 2 — Power Query (Power BI):**
- Final data shaping and column renaming
- Merging datasets for model-ready tables
- Creating conditional columns for role-based filtering

---

## 🗃️ Data Modelling

All tables were connected using defined **primary keys**:
- `matchID` — links batting summary, bowling summary, and match results
- `team` — links player info to match data

Additional **calculated columns** and **parameters** were created to enable dynamic player selection and role-based filtering directly in the dashboard.

---

## ⚙️ DAX Measures

Below are the key DAX measures powering the dashboard:

### 🏏 Batting Measures

```dax
Total Runs = SUM(t20_batting_summary[runs])

Total Innings Batted = COUNT(t20_batting_summary[matchID])

Total Innings Dismissed = SUM(t20_batting_summary[Out])

Batting Avg = DIVIDE([Total Runs], [Total Innings Dismissed], 0)

Total Balls Faced = SUM(t20_batting_summary[balls])

Strike Rate = DIVIDE([Total Runs], [Total Balls Faced], 0) * 100

Batting Position = ROUNDUP(AVERAGE(t20_batting_summary[battingPos]), 0)

Boundary % = DIVIDE(SUM(t20_batting_summary[Boundary runs]), [Total Runs], 0) * 100

Avg. Balls Faced = AVERAGE(t20_batting_summary[balls])

Boundary Runs Batting = t20_batting_summary[fours] * 4 + t20_batting_summary[sixes] * 6
```

### 🎯 Bowling Measures

```dax
Wickets = SUM(t20_bowling_summary[wickets])

Balls Bowled = SUM(t20_bowling_summary[balls])

Runs Conceded = SUM(t20_bowling_summary[runs])

Economy = DIVIDE([Runs Conceded], ([Balls Bowled] / 6), 0)

Bowling Strike Rate = DIVIDE([Balls Bowled], [Wickets], 0)

Bowling Average = DIVIDE([Runs Conceded], [Wickets], 0)

Total Innings Bowled = DISTINCTCOUNT(t20_bowling_summary[matchID])

Dot Ball % = DIVIDE(SUM(t20_bowling_summary[zeros]), SUM(t20_bowling_summary[balls]), 0)

Boundary Runs Bowling = t20_bowling_summary[fours] * 4 + t20_bowling_summary[sixes] * 6
```

### 🧩 Dynamic Selection Measures

```dax
Player Selection = IF(ISFILTERED(t20_players_info[name]), "1", "0")

Display Text = IF([Player Selection] = "1", " ", "Select Player(s) by clicking the player's name to see their individual or combined strength")

Color Callout Value = IF([Player Selection] = "0", "#E8D166", "#1D1D2E")
```

---

## 📊 Dashboard Screenshots

### 🔢 Final 11 — Player Selection View
> Select any players from the full tournament pool on the left panel. The dashboard dynamically shows combined team performance metrics at the bottom.

![Final 11](final-11.jpg)


---

### 💥 Power Hitters / Openers
> Analysis of top-order batters evaluated on Runs, Strike Rate, Batting Average, Balls Faced, and Boundary %.

![Power Hitters](power-hitters.jpg)

---

### ⚓ Anchors / Middle Order
> Middle-order batters assessed on consistency — Batting Average, Balls Faced, and Boundary contribution.

![Anchors](anchors.jpg)

---

### 🏁 Finishers / Lower Order Anchors
> Players who can finish games evaluated on Strike Rate under pressure, Bowling contribution, and Average Balls Faced.

![Finishers](finishers.jpg)

---

### 🔄 All-Rounders / Lower Middle Order
> Dual-threat players ranked by a combination of batting Strike Rate and bowling Economy.

![All Rounders](all-rounders.jpg)

---

### 🎳 Specialist Fast Bowlers / Tail End
> Pure bowlers evaluated on Wickets, Economy, Bowling Strike Rate, and Dot Ball %.

![Specialist Fast Bowlers](specialist-fast-bowlers.jpg)

---

## 🛠️ Tools & Technologies

| Tool / Library | Purpose |
|----------------|---------|
| **Python** | Core scripting language |
| **Beautiful Soup** | HTML parsing and web scraping |
| **Bright Data** | Web scraping infrastructure |
| **Pandas** | Data cleaning and preprocessing |
| **Jupyter Notebook** | JSON-to-DataFrame conversion and EDA |
| **Microsoft Power BI Desktop** | Data transformation, modelling, DAX & dashboard |
| **Power Query** | Final data shaping inside Power BI |
| **DAX** | Measures and calculated columns |
| **ESPN Cricinfo** | Primary data source |

---

## 📎 References

- 📌 [ESPN Cricinfo — T20 World Cup 2022](https://www.espncricinfo.com/series/icc-men-s-t20-world-cup-2022-1298134)
- 📌 [Bright Data — Web Scraping Platform](https://brightdata.com/)
- 📌 [Microsoft Power BI Documentation](https://learn.microsoft.com/en-us/power-bi/)

---

## 🙌 Acknowledgements

Special thanks to the open cricket data community and ESPN Cricinfo for making match and player data accessible for analytical projects.

---

> ⭐ If you found this project useful, please give it a **star** on GitHub!