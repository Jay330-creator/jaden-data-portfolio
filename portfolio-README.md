# Jaden Boothe - Data Portfolio

## Tech Stack

<p align="left">
  <img src="https://skillicons.dev/icons?i=python,git,github,vscode,mysql,html,css" />
</p>

<p align="left">
  <img src="https://go-skill-icons.vercel.app/api/icons?i=pandas,numpy,matplotlib,jupyter" />
</p>

<p align="left">
  <img src="https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white" />
  <img src="https://img.shields.io/badge/Tableau-Learning-E97627?style=for-the-badge&logo=tableau&logoColor=white" />
  <img src="https://img.shields.io/badge/SQL-Learning-336791?style=for-the-badge&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Data%20Visualization-1F77B4?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Sports%20Analytics-6A5ACD?style=for-the-badge" />
</p>

Welcome to my Data Science Portfolio! This repository showcases my projects in data analysis, visualization, and sports analytics. Here, you will find a collection of work that demonstrates my ability to clean data, analyze trends, and communicate insights clearly.

---

## Table of Contents

* [About Me](#about-me)
* [Project 1: AI Sports Betting Agent + Live Performance Dashboard](#project-1-ai-sports-betting-agent--live-performance-dashboard)
* [Project 2: NYC Permits & 311 Service Efficiency Analysis](#project-2-nyc-permits--311-service-efficiency-analysis)
* [Project 3: NYC Transit Accessibility Analysis](#project-3-nyc-transit-accessibility-analysis)
* [Project 4: Sports Betting Performance Dashboard (Prototype)](#project-4-sports-betting-performance-dashboard-prototype)
* [Project 5: Titanic Survival Analysis](#project-5-titanic-survival-analysis)
* [Project 6: Palmer Penguins Analysis](#project-6-palmer-penguins-analysis)
* [Project 7: ABS vs Replay Analysis](#project-7-abs-vs-replay-analysis)

---

## About Me

I'm a Data Science student at Penn State University with a strong interest in data analysis, visualization, and sports analytics. My background in customer-facing roles has helped me build strong communication, problem-solving, and decision-making skills that I bring into technical projects. I enjoy using data to uncover trends, tell clear stories, and support smarter decisions.

---

## Project 1: AI Sports Betting Agent + Live Performance Dashboard

**Tools Used:** Python, JavaScript, React, TypeScript, Node.js, Supabase (Postgres), OpenAI API, Tailwind, Recharts

### Problem
Sports bettors and AI agent operators face two recurring questions: how do you objectively evaluate an automated picking system, and how do you control the cost of running it? Most tracking solutions handle one or the other — not both. This project builds an end-to-end pipeline where an AI agent generates daily picks, posts them to Discord, grades them automatically, and surfaces real-time analytics including per-pick AI compute costs.

### Approach
- Built an autonomous agent that previews 2 plays daily (best + secondary) using OpenAI GPT-5.2 with hard-capped tool budgets to control cost
- Designed a 5-table Postgres schema (Supabase) with idempotent sync logic, foreign-key relationships, and Row Level Security
- Engineered a hybrid scheduler: AI-judgment jobs run via OpenClaw cron, deterministic ETL jobs run via macOS launchd, cutting daily API spend by ~85%
- Implemented automated grading using ESPN's public API to fetch real game scores, with multi-bet-type grading logic (moneyline / spread / totals)
- Built a React + TypeScript dashboard with 5 pages including cumulative units tracking, win rate breakdowns by sport / bet type / confidence tier, and a dedicated "Costs" page showing per-pick AI compute attribution

### Key Insights
- Higher-confidence picks ("best") outperformed secondary picks (75% vs 50% hit rate over the initial sample)
- Prompt engineering can deliver dramatic cost wins: a single prompt rewrite (web_fetch-first, max-8-tool-calls) cut preview generation cost by 79%
- Per-pick cost tracking enables a powerful new metric: ROI on compute (units of profit per dollar spent on AI), giving the agent's economics a clear measure
- Splitting LLM-driven jobs from deterministic ETL is the single highest-leverage architectural decision for a hobbyist AI agent

### Project Preview
![Insights Betting Overview](https://raw.githubusercontent.com/Jay330-creator/insights-betting-dashboard/main/overview.png)
![Insights Betting Costs](https://raw.githubusercontent.com/Jay330-creator/insights-betting-dashboard/main/costs.png)

**Repository Link:** [Insights Betting Dashboard](https://github.com/Jay330-creator/insights-betting-dashboard)

---

## Project 2: NYC Permits & 311 Service Efficiency Analysis

**Tools Used:** Python, Pandas, SQL, Power BI

### Problem
City service systems generate large volumes of permit requests and 311 complaints, but it is often difficult to measure response efficiency, identify delays, and understand how service performance varies across categories. This project analyzes NYC permits and 311 service data to evaluate operational efficiency and uncover patterns in response times.

### Approach
- Cleaned and prepared permit and 311 service request datasets using Python and Pandas  
- Used SQL to organize and aggregate service performance metrics  
- Analyzed response time patterns across complaint and permit categories  
- Built a Power BI dashboard to visualize efficiency trends and service bottlenecks  

### Key Insights
- Certain complaint and permit categories experience significantly longer response times than others  
- Service efficiency varies across request type, location, and agency workload  
- Data visualization makes it easier to identify bottlenecks and improvement opportunities  
- Operational metrics can help support faster, more informed city service decisions  

### Project Preview
![NYC Permits and 311 Dashboard](images/powerbis.png)

**Repository Link:** [NYC Permits & 311 Service Efficiency Analysis](https://github.com/Jay330-creator/nyc-permits-311-service-efficiency-analysis)

---

## Project 3: NYC Transit Accessibility Analysis

**Tools Used:** Python, SQL, Power BI, Pandas

### Problem
Public transit accessibility is critical for mobility, but there is no simple way to compare how accessible NYC subway stations are across boroughs. This project evaluates accessibility coverage and identifies gaps between ADA compliance and real-world usability.

### Approach
- Cleaned and transformed raw station, entrance, and elevator datasets using Python  
- Built SQL queries to aggregate borough-level accessibility metrics  
- Developed a Power BI dashboard to visualize accessibility patterns  

### Key Insights
- Only ~33% of NYC subway stations are fully accessible  
- Manhattan has the highest accessibility rate, while other boroughs lag behind  
- ADA-compliant infrastructure does not guarantee full accessibility  
- Found a **-0.713 correlation** between ADA compliance and accessibility  

### Project Preview
![NYC Transit Dashboard](https://raw.githubusercontent.com/Jay330-creator/nyc-transit-accessibility-analysis/main/images/dashboard-preview.png)

**Repository Link:** [NYC Transit Accessibility Analysis](https://github.com/Jay330-creator/nyc-transit-accessibility-analysis)

---

## Project 4: Sports Betting Performance Dashboard (Prototype)

**Tools Used:** SQL (SQLite), Tableau, Excel

> **Note:** This project was the prototype that evolved into the full-stack [AI Sports Betting Agent + Live Performance Dashboard](#project-1-ai-sports-betting-agent--live-performance-dashboard) (Project 1). The original Tableau dashboard analyzed historical betting data; the new system generates picks autonomously and tracks performance in real time.

### Problem
Sports bettors often track performance manually without clear insights into profitability, risk, and strategy effectiveness. This project aims to analyze betting performance using structured data.

### Approach
- Used SQL to transform raw betting data into aggregated tables  
- Analyzed performance by sport, bet type, and odds range  
- Built a Tableau dashboard to visualize trends and profitability  

### Key Insights
- Certain sports consistently outperform others in profitability  
- Bet type significantly impacts win rate and risk exposure  
- Data-driven tracking provides a clear edge over unstructured betting  

### Project Preview
![Sports Betting Dashboard](https://raw.githubusercontent.com/Jay330-creator/sports-betting-analytics/main/images/SportsTab.png)

**Repository Link:** [Sports Betting Analytics Dashboard](https://github.com/Jay330-creator/sports-betting-analytics)

---

## Project 5: Titanic Survival Analysis

**Tools Used:** Python, Pandas, Matplotlib, Seaborn, Jupyter Notebook

### Problem
The Titanic disaster provides a classic dataset for understanding survival patterns. This project investigates which factors most influenced survival outcomes.

### Approach
- Cleaned and analyzed passenger data  
- Explored relationships between class, age, gender, and fare  
- Built visualizations to identify survival trends  

### Key Insights
- Passenger class had a major impact on survival rates  
- Females had significantly higher survival rates than males  
- Higher ticket fares were associated with increased survival likelihood  

### Project Preview
![Titanic Chart 1](images/titanic-chart-1.png)
![Titanic Chart 2](images/titanic-chart-2.png)

**Repository Link:** [Titanic Survival Analysis](https://github.com/Jay330-creator/titanic-survival-eda-python)

---

## Project 6: Palmer Penguins Analysis

**Tools Used:** Python, Pandas, Matplotlib, Seaborn, Jupyter Notebook

### Problem
Understanding differences between penguin species requires analyzing multiple biological measurements. This project explores how physical characteristics vary across species and what patterns exist in the data.

### Approach
- Cleaned and prepared biological measurement data  
- Performed exploratory data analysis on species features  
- Built visualizations to compare physical traits  

### Key Insights
- Clear separation between species based on flipper length and body mass  
- Certain species show strong clustering across multiple measurements  
- Visualizations reveal patterns not obvious from raw data alone  

### Project Preview
![Penguins Chart 1](images/penguins-chart-1.png)
![Penguins Chart 2](images/penguins-chart-2.png)

**Repository Link:** [Palmer Penguins Analysis](https://github.com/Jay330-creator/penguins-data-analysis-python)

---

## Project 7: ABS vs Replay Analysis

**Tools Used:** Python, Pandas, Matplotlib, Jupyter Notebook

### Problem
Sports leagues rely on replay systems to ensure accuracy, but many fans and players criticize these systems for being slow and overly subjective. This project evaluates whether automated systems like MLB's ABS provide a better experience compared to traditional replay systems.

### Approach
- Collected and compared review time data across multiple sports leagues  
- Analyzed differences between automated and human-based decision systems  
- Built visualizations to compare efficiency and consistency  

### Key Insights
- Automated systems like ABS are significantly faster than traditional replay systems  
- Subjective review systems introduce delays and inconsistency  
- Speed and objectivity are key drivers of positive fan experience  

### Project Preview
![ABS Chart 1](images/abs-chart-1.png)

**Repository Link:** [ABS vs Replay Analysis](https://github.com/Jay330-creator/abs-replay-analysis)

---

## 📫 Contact

* Email: jaden.n.boothe@gmail.com  
* LinkedIn: https://www.linkedin.com/in/jaden-boothe-29b8873b9/  
* GitHub: https://github.com/Jay330-creator  

Feel free to reach out for opportunities, collaborations, or questions!
