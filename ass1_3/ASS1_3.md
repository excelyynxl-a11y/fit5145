

# AI Declaration 

I used Claude for initial idea feedback, dataset recommendations and writing improvement. I used Hermes Agent (asai/gpt-6-astra) to review feedback and rubric alignment, explore literature and data sources, refine methodology, and revise the proposal’s wording and structure.

# ![][image1]

# 

# 

# 

# FIT5145 Assignment 1

## Predicting Late-Race Running Slowdown in HYROX Men’s Open 

Name: Lim Excelyynx   
Student ID: 34476245

### 1.0 Introduction

HYROX alternates eight 1 km runs with eight functional stations, is one of the fastest-growing participation sports globally, seeing athletes rising from 570000 in 2024/25 season to 1.5 million in 2025/26 (SportsPro, 2026). This project asks whether information available immediately after Station 4 can predict subsequent running slowdown, rather than merely benchmark a completed race.

The analytical population are Individual Open finishers from completed in the 2025/26 season with valid run and station splits time. One observation represents one athlete-race across all stations. 
 
The goals are to describe pacing variation, test whether early station information improves predictions beyond running splits alone and produce an uncertainty corridor for later running time that coaches can interpret.

### 2.0 Related Work

#### 2.1 Existing projects

In 2025, Brandt et al. studied 11 recreational athletes in a simulated HYROX, using Wilcoxon tests and Spearman correlations to characterise physiological demands and performance associations. This supports examining both runs and stations, but does not validate checkpoint-based predictions of subsequent slowdown.

In 2025, Miyazaki et al. used first-half marathon biomechanics and functional logistic regression to predict severe late-race slowing, achieving 73.9% accuracy on a held-out test subset of 665 selected cases. This establishes that early-warning prediction is not itself new. However, it requires wearable measurements, excludes intermediate slowdown cases, and predicts a binary outcome rather than a continuous magnitude.

In 2026, HyroxDataLab advertised segment percentiles and goal-time pacing tools. HyroxVault predicts finish times from fitness inputs, but explicitly identifies unmodelled back-half fading as a weakness. These published descriptions do not demonstrate a validated Station-4 forecast using the athlete's unfolding race. This is a bounded gap in the reviewed work, not a claim that no comparable system exists.


#### 2.2 Novelty

The proposed contribution is a split-only, cross-modal checkpoint forecast, leading to the question: does early station performances add information about later running slowdown beyond early running pace? Each station will be normalised against its own historical training-data distribution. Raw durations of different exercises cannot measure a common fatigue trajectory. Candidate interactions pair station-relative times with changes in the following run is available only through Run 4.

An ablation comparison will test running-only against run-plus-station models on unseen events. Predicting continuous slowdown and reporting uncertainty extends the reviewed binary and static tools without requiring specialist sensors. Novelty rests on this testable information contribution and decision-time design, not on using random forests or a different sport. No improvement would be an informative negative finding.


#### 2.3 Importance

In Brandt et al.'s simulation, median running time was 51.2 minutes versus 32.8 minutes at stations, with peak heart rate, lactate and perceived exertion occurring at the final station. These findings establish substantial running demands and late-race strain, but the small sample cannot establish population-wide slowdown prevalence.

For athletes, an accessible forecast could support realistic expectations and discussion of pacing with coaches. Undetected pacing collapse under fatigue within athletes leads to a 25 - 35% technique breakdown at heavy loaded stations (Bauerfeind). Having an early-warning signal contributes to athlete-safety, not limited to a performance one.

For gyms, replaying race trajectories could make debriefs more specific and reduce manual analysis. A free basic report could widen access beyond athletes purchasing specialist sensors. Benefits would be evaluated through prediction error, coach-reported usefulness and analysis time saved; improved retention, injury prevention and faster finishes remain unproven. This positively impacts members as a structured pace periodization is proven to reduce injury risk by 50-70% (Bauerfeind). Alerts must avoid false reassurance or pressure to maintain an unsafe pace.

### 3.0 Business Model

#### 3.1 Data Business Model

The proposed model is business-to-business analytics-as-a-service, transforming historical results into decision support rather than reselling raw athlete records. HYROX supplies the underlying results, coaches and affiliated gyms are prospective subscription customers, athletes receive interpreted forecasts. Revenue would fund data maintenance, validation and dashboard support. Demand and willingness to pay require testing.

The primary source is the official results web-scrapped from the HYROX All Time Ranking Official Website with the use of Python web-scrapping script from an open-source tool (GitHub imterence/github_analysis repository). The web-scrapper will be used to scrap Hyrox Male Open and Female Open dataset and save as CSV. The raw dataset includes Name, City, Age Group, All workout times (Running 1-4, SkiErg, Sled Push/Pull, etc.), Total time and Ranking.

Initially, an R-based prototype will replay historical races, hiding all information after Station 4. A future service would receive timestamped splits from a licensed timing feed or consented manual input, calculate features and return a forecast corridor to a coach dashboard. Public results access does not establish live-feed availability. Race-day use requires latency testing, permission and prospective validation, the prototype promises none of these.

#### 3.2 Level of Analytics

Descriptive analytics will summarise run trajectories and separate station-relative distributions. Predictive analytics will compare a training-median baseline, running-only linear regression, and run-plus-station regression whereas a random forest is an optional nonlinear comparator.

Evaluation will hold out whole events, remove training records of identifiable test athletes and fit preprocessing on training data only. Report mean absolute error in slowdown percentage points and prediction-interval coverage. Final time, finishing rank and later splits cannot be predictors. 


#### 3.3 Challenges

Missing or misassigned splits, penalties, transition timing and venue layouts can distort apparent slowing. Audit records, document exclusions and test sensitivity to anomalous runs. Missing outcomes are impossible to be imputed, hence can only be dropped off. Shared denominators can inflate apparent associations, so also evaluate predictions of absolute later running time.

### Reference
SportsPro. (2026). How Hyrox is turning fitness into the world's next mass participation sport.
https://www.sportspro.com/features/finance-investment/hyrox-business-model-mass-participation-private-equity-investment/

HyroxDataLab. (2026). Data-driven HYROX training & race analysis for all athletes.
https://hyroxdatalab.com/

Brandt et al. (2025). Acute physiological responses and performance determinants in Hyrox© – a new running-focused high intensity functional fitness trend
https://pmc.ncbi.nlm.nih.gov/articles/PMC11994925/

Miyazaki, Y., et al. (2025). Early marathon running metrics from inertial measurement units predict significant pace reduction. Frontiers in Sports and Active Living,
https://pmc.ncbi.nlm.nih.gov/articles/PMC12575221

GitHub - imterence/hyrox_analysis (2025). Extracted data from https://results.hyrox.com/ and analysed the podium finishers to the rest of their competition to see what makes them a winner.
https://github.com/imterence/hyrox_analysis

Ketzer et al. (2016). Injury epidemiology in HYROX athletes: an international cross-sectional survey.
https://www.medrxiv.org/content/10.64898/2026.08.09.26359590v1

Bauerfeind. HYROX injury | Common problems, causes & prevention.
https://www.bauerfeind-sports.com/hyrox-injuries/
