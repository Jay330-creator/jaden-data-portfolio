# Jaden Boothe — Data Portfolio

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

I'm a 3rd-year Data Science student at Penn State, and I like building analytics projects that go all the way through, from messy raw data to a finding someone could actually act on. Every project below starts with a real question, shows how I worked through the data, and ends somewhere I could defend in front of a stakeholder.

The parts I care most about are the unglamorous ones: cleaning data that doesn't want to be cleaned, figuring out what question is actually worth asking, and saying the answer in plain English so a non-technical person can use it.

---

## Table of Contents

* [About Me](#about-me)
* [Project 1: AI Sports Betting Agent + Live Performance Dashboard](#project-1-ai-sports-betting-agent--live-performance-dashboard)
* [Project 2: What Makes a Hit Song? Spotify Analytics](#project-2-what-makes-a-hit-song-spotify-analytics)
* [Project 3: NBA Game Outcome Prediction, A Machine Learning Project](#project-3-nba-game-outcome-prediction-a-machine-learning-project)
* [Project 4: NYC Permits & 311 Service Efficiency Analysis](#project-4-nyc-permits--311-service-efficiency-analysis)
* [Project 5: NYC Transit Accessibility Analysis](#project-5-nyc-transit-accessibility-analysis)
* [Project 6: Sports Betting Performance Dashboard (Prototype)](#project-6-sports-betting-performance-dashboard-prototype)
* [Project 7: Titanic Survival Analysis](#project-7-titanic-survival-analysis)
* [Project 8: Palmer Penguins Analysis](#project-8-palmer-penguins-analysis)
* [Project 9: ABS vs Replay Analysis](#project-9-abs-vs-replay-analysis)

---

## About Me

I'm a Data Science student at Penn State focused on data analysis, visualization, and sports analytics. Almost five years in customer-facing roles taught me how to take something complicated and explain it to someone who just wants the bottom line, which turns out to be most of the job in analytics. I care less about which tool I use than whether the analysis actually answers the question someone is paying me to answer.

---

## Project 1: AI Sports Betting Agent + Live Performance Dashboard

> **End-to-end case study.** This is the one I'd walk through in an interview.

### The Business Problem
If you're running any kind of automated decision system, like a betting agent, a trading bot, or a recommendation engine, two questions come up fast: is it actually working, and is it worth what it costs to run? Most tools answer one or the other. I wanted to build something that answers both in real time, where I can trace any single decision back to what it cost to make.

### The Question I Answered
**Is the agent's profit per dollar of AI spend good enough to justify keeping it running?**

To answer that I needed three things connected together: what the agent picked, whether it won, and what it cost to think about each pick.

### My Approach
* **Designed the data model first.** A 5-table Postgres schema in Supabase where every pick links back to the AI run that produced it, its cost row, and its grading result. I used Row Level Security so the public dashboard can read from the same tables the agent writes to without exposing anything it shouldn't.
* **Solved a timing problem.** The agent makes picks at 1 PM, but the cost data for that run doesn't land until about 30 seconds later. I built a two-stage flow where the producer stamps a run ID and the cost sync matches it up afterward within a 60-second window, so the two never have to finish at the same time.
* **Cut runtime cost by about 85%.** Originally every scheduled job ran through the LLM, even simple ones that just moved data around. I moved the deterministic jobs to macOS launchd with no LLM involved and kept only the judgment jobs on the AI runtime. Daily cost went from roughly $1.71 to $0.30.
* **Replaced a bad data source.** My first grading version scraped scores out of search results, and it kept reading game dates as final scores. I switched to ESPN's public API and wrote grading logic for moneylines, spreads, and totals.
* **Built the dashboard.** React and TypeScript, five pages, including a Costs page for the metric I cared about most: profit earned per dollar of AI spend.

### Key Findings
* The agent's higher-confidence "best" picks beat its "secondary" picks by 25 points (75% vs 50% hit rate), so its confidence signal actually means something.
* Rewriting one prompt dropped its cost by 79%. I hadn't thought of prompt wording as a budget item until I saw that.
* Over the initial sample, the agent returned about +4.47 units of profit per dollar of AI spend. Not a recommendation to bet, but a real number I can stand behind.

### Why It Matters
Every team putting AI into production is going to run into this question: what does an autonomous system actually cost per decision, and is it earning its keep? Most dashboards stop at win or loss. This one follows the cost all the way down to the individual pick, which is what makes the ROI honest.

### Tools
Python, JavaScript, React, TypeScript, Node.js, Supabase (Postgres), OpenAI API, Tailwind, Recharts, macOS launchd, ESPN public API.

### Project Preview
![Insights Betting Overview](https://raw.githubusercontent.com/Jay330-creator/insights-betting-dashboard/main/overview.png)
![Insights Betting Costs](https://raw.githubusercontent.com/Jay330-creator/insights-betting-dashboard/main/costs.png)

**Repository:** [Insights Betting Dashboard](https://github.com/Jay330-creator/insights-betting-dashboard)

---

## Project 2: What Makes a Hit Song? Spotify Analytics

### The Business Problem
Labels and streaming services place big bets on which songs to push and which artists to sign, and a lot of it rides on instinct about what feels like a hit. Instinct isn't very reliable. I approached this as if I were on an A&R or streaming team trying to figure out which audio traits, song lengths, and genres actually line up with commercial success.

### The Question I Answered
**What separates a hit (popularity 70 or higher) from everything else, and which common assumptions about hit-making actually survive contact with the data?**

### My Approach
* **Loaded 114,000 tracks into BigQuery** and wrote five SQL queries covering audio features, song length, genre hit rates, the effect of explicit content by genre, and the average profile of the top 5% of tracks.
* **Chased the weird result.** The explicit-content-by-genre query was where it got interesting, because the usual "explicit helps" assumption turned out to be genre-dependent and actually flips in country.
* **Stayed honest about sample sizes.** The dataset is balanced at 1,000 songs per genre, which keeps cross-genre comparisons fair. Where a subgroup was small (explicit country is only 30 songs), I flagged that the direction holds but the exact percentage is shaky.
* **Built a 4-page Power BI dashboard** as a walkthrough: an overview, then what features matter, then which genres win, then the overall recipe. Each page answers one question instead of just showing charts.

### Key Findings
* Only about 4.8% of tracks are hits. Out of 114,000 songs, just 5,472 clear the bar, which sets the stage for how rare success is.
* Hit songs are 31% less acoustic than non-hits, the biggest single gap of any feature. How produced a song is matters more than tempo, which barely moves (120 vs 122 BPM).
* The one that made me re-run my query: explicit content nearly doubles the hit rate in pop (29% to 59%) and gives R&B about a 9x bump, but in country it cuts the hit rate from 8.9% to 3.3%. Country is the only genre where it backfires, which goes against the assumption that explicit always helps.
* The top 5% of tracks average 120 BPM, 3:38 long, danceability 0.61, energy 0.67, low acousticness around 0.22, and loud mastering near -6.7 dB. Only 15.7% are explicit, so explicit isn't a hit-maker by itself, just a multiplier in certain genres.

### Why It Matters
For an A&R team picking what to promote, "songs that look like hits" stops being a vibe and becomes something measurable. The country reversal alone could change how a label markets a release, and the fact that production matters more than tempo would change which songs get pitched to producers.

### Tools
SQL (Google BigQuery), Power BI, Python (preprocessing). Dataset: Kaggle Spotify Tracks Dataset (114,000 tracks).

### Project Preview
![Spotify Executive Overview](https://raw.githubusercontent.com/Jay330-creator/spotify-hit-analysis/main/images/01_overview.png)
![Spotify The Recipe](https://raw.githubusercontent.com/Jay330-creator/spotify-hit-analysis/main/images/04_recipe.png)

**Repository:** [Spotify Hit Analysis](https://github.com/Jay330-creator/spotify-hit-analysis)

---

## Project 3: NBA Game Outcome Prediction, A Machine Learning Project

### The Business Problem
Sports analytics teams, broadcasters, and betting markets all spend real money forecasting NBA game outcomes, but most public prediction models either quietly leak future information into their features or never benchmark themselves against a real-world standard. I wanted to build a model that does neither. Strict leakage-free features, and an honest comparison against the most accurate predictor we know of: Vegas.

### The Question I Answered
**Using only information available before tip-off, how accurately can a machine learning model predict NBA game winners, and how does it stack up against actual Vegas betting favorites?**

### My Approach
* **Cleaned 8,600+ NBA games (2016 to 2022)** and engineered 16 leakage-free features: season win rate, last-10 form, conference seed, home/road splits, star-weighted injuries, travel distance, rest days, and head-to-head record.
* **Enforced a strict no-leakage rule.** Every feature is computed from each team's history coming INTO the game, never from the game itself. The pipeline records the feature first, then updates history with the result. I also used a time-based train/test split so the model never sees future data.
* **Trained three models plus an ensemble.** Logistic Regression, Random Forest, and Gradient Boosting, then combined them. All three converged near 62% accuracy, which told me the bottleneck was the data not the algorithm.
* **Benchmarked against real Vegas odds** on the same 1,665 test games using historical moneyline favorites.

### Key Findings
* The ensemble hit 62.0% accuracy on 1,718 held-out games the model never saw during training. That's about 6 points above the home-court baseline of 56.1%.
* Vegas got 66.8% on the same games. The model closes most of the gap to professional bookmakers using only public data.
* The remaining 5-point gap to Vegas is a real, measured quantity. It represents the value of the data Vegas has that public box scores don't: live injury severity, betting-market movement, and play-by-play lineup data.
* The strongest signals were conference seed difference and recent form (last-10 win rate). How good a team is RIGHT NOW matters more than their full-season record.

### Why It Matters
A lot of public NBA prediction projects quote inflated accuracy numbers because they accidentally use post-game stats as features. Doing the analysis honestly, with leakage-free features and a real Vegas benchmark, lands at a lower but believable number. The conclusion that gets you to 70% requires data access Vegas has and the public doesn't, which is itself a useful finding for anyone building in this space.

### Tools
Python, Pandas, scikit-learn, NumPy, Matplotlib, Jupyter Notebook.

### Project Preview
![Accuracy vs Baseline](https://raw.githubusercontent.com/Jay330-creator/nba-prediction-model/main/images/accuracy_comparison.png)
![Feature Importance](https://raw.githubusercontent.com/Jay330-creator/nba-prediction-model/main/images/feature_importance.png)

**Repository:** [NBA Prediction Model](https://github.com/Jay330-creator/nba-machine-learning-predictions

---

## Project 4: NYC Permits & 311 Service Efficiency Analysis

### The Business Problem
City agencies are always under pressure to respond to permits and 311 complaints faster, but "faster" is vague. Different complaint types have completely different baselines, and without breaking it down by category, leadership can't tell a real bottleneck from normal variation. I worked on it as if I were presenting to a city ops team deciding where to spend its process-improvement budget.

### The Question I Answered
**Which complaint and permit categories are dragging service efficiency down, and where would fixing things actually move the needle?**

### My Approach
* **Cleaned two messy public datasets.** NYC's open data has inconsistent timestamps, missing close dates, and category labels that change over time. I used Pandas to standardize the timestamps and line the categories up across both sources.
* **Aggregated with SQL** to get response-time distributions per category instead of just averages, since averages hide the long tail where the real problems are.
* **Built a Power BI dashboard** around that one question, so every chart answers something a city ops leader would actually ask.

### Key Findings
* Response time varies by several times over between the fastest and slowest categories, even after accounting for borough.
* Some categories are slow but steady, others swing wildly, and those two problems need different fixes.
* A handful of categories spike consistently, and those are where process improvement would pay off most.

### Why It Matters
With this in front of them, a city ops leader could decide where to put inspector hours next quarter or which agency handoffs need fixing. Without the category breakdown, those calls get made on gut feel.

### Tools
Python, Pandas, SQL, Power BI.

### Project Preview
![NYC Permits and 311 Dashboard](images/powerbis.png)

**Repository:** [NYC Permits & 311 Service Efficiency Analysis](https://github.com/Jay330-creator/nyc-permits-311-service-efficiency-analysis)

---

## Project 5: NYC Transit Accessibility Analysis

### The Business Problem
The MTA reports ADA compliance publicly, but compliance and real accessibility aren't the same thing. A station can be compliant on paper and still be useless to someone in a wheelchair the day the elevator is down. For an advocacy group or a city planner trying to find the real mobility gap, the headline compliance number can be misleading.

### The Question I Answered
**Where is subway accessibility actually failing, and how far does that diverge from the official ADA compliance rate?**

### My Approach
* **Joined three MTA datasets** (stations, entrances, elevators) in Python. Each had its own quirks in how stations were identified, so getting them to line up took some work.
* **Aggregated accessibility by borough with SQL** so I could compare access across the city instead of just looking at one citywide number.
* **Checked the correlation** between ADA compliance and real accessibility to see whether the official metric is a good stand-in.
* **Built a Power BI dashboard** so someone non-technical could explore the borough-by-borough picture on their own.

### Key Findings
* Only about one in three stations is fully accessible.
* Manhattan leads; the outer boroughs trail well behind.
* I found a -0.713 correlation between ADA compliance and real-world accessibility. Stations that look compliant on paper often aren't usable in practice, which means the headline number is probably overstating how much coverage there really is.

### Why It Matters
For an advocacy group or planner, this is the difference between "we're doing fine" and "our main metric is hiding the problem." That negative correlation is the kind of thing that would change how a stakeholder reports progress.

### Tools
Python, SQL, Power BI, Pandas.

### Project Preview
![NYC Transit Dashboard](https://raw.githubusercontent.com/Jay330-creator/nyc-transit-accessibility-analysis/main/images/dashboard-preview.png)

**Repository:** [NYC Transit Accessibility Analysis](https://github.com/Jay330-creator/nyc-transit-accessibility-analysis)

---

## Project 6: Sports Betting Performance Dashboard (Prototype)

> **Note:** This is the prototype that became [Project 1](#project-1-ai-sports-betting-agent--live-performance-dashboard). This one analyzed historical bets; the newer system generates picks on its own and tracks costs in real time.

### The Business Problem
Most bettors track wins and losses in a notebook and use that to decide whether to keep going. The trouble is that gut-feel pattern spotting isn't reliable, and a losing streak could be bad luck or a sign a strategy stopped working, and it's hard to tell which. I built this as a tool for a serious recreational bettor who wants to judge their own performance honestly.

### The Question I Answered
**Which sports and bet types are actually profitable over a real sample, and which ones just feel profitable until you account for sample size?**

### My Approach
* **Built a SQLite database** from raw bet history and used SQL to break it down by sport, bet type, and odds range, which are the cuts that actually matter for judging a strategy.
* **Measured real profitability** in units net rather than just win/loss count, so the numbers reflect money rather than how often a bet hit.
* **Built a Tableau dashboard** the way I'd actually analyze my own betting: pick a sport, drill into bet types, and check whether the sample is big enough to trust.

### Key Findings
* Profitability swings a lot by sport. Some that feel like winners lose money once measured, and a couple go the other way.
* Bet type drives more win-rate variance than the sport does. Some markets, like player props, are far more volatile than others.
* Without real tracking, bettors badly overrate themselves. The discipline of writing it all down is the biggest edge.

### Why It Matters
This is the kind of dashboard someone would use to stop themselves from repeating the same mistake. Project 1 takes the idea further by automating the whole thing, from generating picks to grading them to tracking cost per decision.

### Tools
SQL (SQLite), Tableau, Excel.

### Project Preview
![Sports Betting Dashboard](https://raw.githubusercontent.com/Jay330-creator/sports-betting-analytics/main/images/SportsTab.png)

**Repository:** [Sports Betting Analytics Dashboard](https://github.com/Jay330-creator/sports-betting-analytics)

---

## Project 7: Titanic Survival Analysis

### The Business Problem
Picture an insurance analyst trying to understand the risk profile of a historical disaster, or a researcher looking at how class shaped who lived and who didn't. The Titanic dataset is a small, complete record of one event, clean enough to learn on but with real patterns about how demographics and money affected survival.

### The Question I Answered
**Which mattered most for surviving, class, gender, age, or fare, and what does that say about how social structure played out in a crisis?**

### My Approach
* **Cleaned the passenger data first** (missing ages, messy cabin codes, fare outliers) in Pandas. A lot of beginner Titanic analyses skip this, but it changes the conclusions.
* **Tested ideas one at a time.** Did class matter on its own, separate from fare? Did the gender effect hold inside each class? I used grouping and crosstabs instead of jumping straight to a model.
* **Visualized survival across each variable** with Matplotlib and Seaborn so the patterns are readable without a stats background.

### Key Findings
* Class was the strongest single predictor. First-class passengers survived at far higher rates than third.
* Gender mattered a lot. Women survived at roughly 3x the rate of men, and it held even within the same class.
* Higher fare lined up with higher survival, but most of that is really just class. Fare on its own isn't doing the work.

### Why It Matters
The Titanic gets told as a tragedy of nature, but the data shows it was also a tragedy of social structure. Survival broke down hard along class and gender lines, which echoes how modern crises tend to hit people differently depending on their circumstances.

### Tools
Python, Pandas, Matplotlib, Seaborn, Jupyter Notebook.

### Project Preview
![Titanic Chart 1](images/titanic-chart-1.png)
![Titanic Chart 2](images/titanic-chart-2.png)

**Repository:** [Titanic Survival Analysis](https://github.com/Jay330-creator/titanic-survival-eda-python)

---

## Project 8: Palmer Penguins Analysis

### The Business Problem
Researchers studying penguins need a quick way to tell species apart in the field, especially when visual ID is hard because of distance, lighting, or hybrids. Physical measurements are easier to collect reliably, but only if a couple of them actually separate the species cleanly. I treated this as a quick reference for a field researcher who needs to know which two measurements to grab first.

### The Question I Answered
**Which measurements separate the penguin species most cleanly, and how confidently can you call a species from measurements alone?**

### My Approach
* **Cleaned the dataset** (dropped null rows, standardized units) so every comparison was fair.
* **Compared each pair of measurements** to find the combination that splits the species best visually.
* **Made scatter and pair plots** to show the clustering directly, since clean clusters from two measurements are basically the researcher's answer.

### Key Findings
* Flipper length and body mass together give the cleanest split. Most penguins can be classified from just those two.
* Some species cluster tightly while others overlap, so a few are easier to call confidently than others.
* The visuals turned up patterns that summary stats alone would have missed.

### Why It Matters
For someone in the field, this answers a practical question: if you can only grab two measurements quickly, which two? The two-measurement rule here would let them classify most penguins on the spot with good confidence.

### Tools
Python, Pandas, Matplotlib, Seaborn, Jupyter Notebook.

### Project Preview
![Penguins Chart 1](images/penguins-chart-1.png)
![Penguins Chart 2](images/penguins-chart-2.png)

**Repository:** [Palmer Penguins Analysis](https://github.com/Jay330-creator/penguins-data-analysis-python)

---

## Project 9: ABS vs Replay Analysis

### The Business Problem
Leagues are weighing whether to swap human review for automated systems like MLB's Automated Ball-Strike system. There's real money on it: broadcast pacing, fan retention, and umpire labor all hang on the answer. Officials and broadcasters need actual evidence on review speed and consistency before committing to a change.

### The Question I Answered
**Are automated reviews measurably faster and more consistent than human replay, and is the gap big enough to justify changing how officiating works?**

### My Approach
* **Pulled review-time data** across several leagues so I could compare automated and human review on the same measure.
* **Looked at the whole distribution, not just averages,** because a system that's faster on average but wildly inconsistent is actually worse for a broadcast than a slower, steadier one.
* **Visualized the timing** to show how the spread differs between automated and human review.

### Key Findings
* Automated review is clearly faster than human replay on the same kinds of plays.
* Human review is far more inconsistent. The worst-case times run many times the median, which is what wrecks broadcast flow.
* Speed and consistency together are what shape the fan experience, and automated review wins on both.

### Why It Matters
For a league making this call, the consistency finding might matter more than the speed. Broadcasters can plan around a slow but predictable review window; an unpredictable one is the real production headache. The data backs moving to automated review on operational grounds, not just because it's quicker.

### Tools
Python, Pandas, Matplotlib, Jupyter Notebook.

### Project Preview
![ABS Chart 1](images/abs-chart-1.png)

**Repository:** [ABS vs Replay Analysis](https://github.com/Jay330-creator/abs-replay-analysis)

---

## 📫 Contact

* Email: jaden.n.boothe@gmail.com  
* LinkedIn: https://www.linkedin.com/in/jaden-boothe-29b8873b9/  
* GitHub: https://github.com/Jay330-creator  

Open to data analyst internship conversations for Summer 2026.
