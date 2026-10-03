# AI Declaration

I used Claude for initial idea feedback, dataset recommendations and writing improvement. I used Hermes Agent (asai/gpt-6-astra) to review feedback and rubric alignment, explore literature and data sources, refine methodology, and revise the proposal’s wording and structure.

# ![][image1]

#

#

#

# FIT5145 Assignment 1

## Predicting Late-Race Running Slowdown in HYROX

Name: Lim Excelyynx  
Student ID: 34476245

### 1.0 Introduction

HYROX alternates eight 1 km runs with eight functional stations, is one of the fastest-growing participation sports globally, seeing athletes rising from 570000 in 2024/25 season to 1.5 million in 2025/26 (SportsPro, 2026). This project asks whether information available immediately after Station 4 can predict subsequent running slowdown, rather than merely benchmark a completed race.

The analytical population are Individual Pro finishers from completed in the 2025/26 season with valid run and station splits time. One observation represents one athlete-race across all stations.

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

The primary source is the official results web-scrapped from the HYROX All Time Ranking Official Website with the use of Python web-scrapping script from an open-source tool (GitHub imterence/github_analysis repository). The web-scrapper will be used to scrap Hyrox Male Pro dataset and save as CSV. The raw dataset includes Name, City, Age Group, All workout times (Running 1-4, SkiErg, Sled Push/Pull, etc.), Total time and Ranking.

Initially, an R-based prototype will replay historical races, hiding all information after Station 4. A future service would receive timestamped splits from a licensed timing feed or consented manual input, calculate features and return a forecast corridor to a coach dashboard. Public results access does not establish live-feed availability. Race-day use requires latency testing, permission and prospective validation, the prototype promises none of these.

#### 3.2 Level of Analytics

Descriptive analytics will summarise run trajectories and separate station-relative distributions. Predictive analytics will compare a training-median baseline, running-only linear regression, and run-plus-station regression whereas a random forest is an optional nonlinear comparator.

Evaluation will hold out whole events, remove training records of identifiable test athletes and fit preprocessing on training data only. Report mean absolute error in slowdown percentage points and prediction-interval coverage. Final time, finishing rank and later splits cannot be predictors.

#### 3.3 Challenges

Missing or misassigned splits, penalties, transition timing and venue layouts can distort apparent slowing. Audit records, document exclusions and test sensitivity to anomalous runs. Missing outcomes are impossible to be imputed, hence can only be dropped off. Shared denominators can inflate apparent associations, so also evaluate predictions of absolute later running time.

### 4.0 Characterising and Analysing Data

#### 4.1 Potential Data Sources, Characteristics, Platforms and Tools

The preferred prototype source is the official HYROX Season 8 results, extracted to local CSV using an adapted Python scraper based on imterence's open-source repository (HYROX Results; imterence, 2025). The two existing files, `hyrox_season8_male_pro.csv`, contain respectively 12,400 rows and fields for eight runs, eight stations, total time, age group and an athlete-detail URL. Their _volume_ is manageable on a laptop; _variety_ is modest (tabular identifiers, categorical fields and time strings); _velocity_ is batch updates after races, not a verified live stream; and _veracity_ requires substantial checking of missing, duplicated and incorrectly labelled records. In fact, both current files contain male-coded URLs, share 12,399 identical rows, and lack an explicit event identifier. They must not be treated as independent male/female samples or as ready for event-held-out validation. Re-extraction with verified sex and event filters, followed by checking detail-page event IDs and distinct athlete-race records, is a prerequisite. The ranking view alone may not represent all individual race attempts. The original repository documents only Runs 1–4, so the adapted eight-run extraction must also be checked against source detail pages (imterence, 2025).

A secondary option is JGug's Kaggle HYROX results, described as covering Seasons 4–6 and including event identifiers and detailed splits (JGug, 2024). Its _volume_ is larger (the listed CSV is 25.31 MB); _variety_ adds event and division metadata; _velocity_ is periodic historical publication; and _veracity_ depends on coverage, missing splits, division harmonisation and whether the downloadable file actually supplies all eight run splits. It is useful for historical validation if these checks pass, but older seasons can differ from Season 8. The Kaggle listing's season description and file preview are not fully consistent, so coverage must be confirmed locally before pooling. A Kaggle download and RStudio suffice for an offline alternative.

For expansion, the third-party Hyrox Result API advertises versioned JSON event, athlete and split endpoints, with bearer authentication, subscription and rate limits (Hyrox Result API, 2025). Its _volume_ could scale across events; _variety_ includes nested JSON and event metadata; _velocity_ depends on provider updates rather than a guaranteed Station-4 live feed; and _veracity_ requires checking split completeness, latency and provider provenance. Its public sandbox exposes sample responses, not proof of real-time checkpoint delivery. A paid plan and permission check would precede production use; scheduled ingestion into a restricted relational store and automated schema/quality tests would replace repeated CSV downloads if access and timeliness prove adequate. For the present small, static files, VS Code/Git for scraper versioning, Python for extraction, and local CSV plus RStudio for cleaning, modelling and plots are proportionate; a cloud warehouse is unnecessary.

#### 4.2 Data Analysis Approach

The analysis unit is a verified Individual Pro athlete-race. After confirming source permission and provenance, retain division/sex, event and athlete-race keys; parse split strings into seconds; check positive durations, station order, missingness and duplicate URLs; and exclude records without the checkpoint or outcome rather than fabricate times. Profile selection and anomalous timing by event and sex before modelling. Missing event identity would block the proposed event-held-out test, requiring a better source rather than claiming generalisation from a random row split.

At the checkpoint immediately after Station 4, only Runs 1–4 and Stations 1–4 (through Burpee Broad Jumps) are available. Define early running time as the mean of Runs 1–4, later running time as the mean of Runs 5–8, and slowdown as 100 × (later − early)/early; negative values indicate faster later running. Describe distributions and trajectories by sex/event without interpreting station time as a direct physiological measure of fatigue. Derive early pace trend and station-relative times using station-specific training-set medians; consider station-time × subsequent-run-change features only for observed Runs 2–4. Exclude Runs 5–8, Stations 5–8, final time, rank and any post-checkpoint aggregates from predictors.

Compare a training-median slowdown baseline, a running-only linear model and the same model augmented with early station features; optionally test a random forest for nonlinearity. Fit imputation, scaling, station medians and tuning _within training folds_. Hold out whole events and prevent identifiable athletes in test events from appearing in training, then report test mean absolute error in percentage points, absolute later-run-time error and the station-feature ablation difference. Calibrate a prediction corridor on separate validation events and report empirical coverage and width on untouched test events, including sex/event strata where samples allow. The expected outcome is an auditable checkpoint forecast and evidence of whether stations add value; a null improvement, poor coverage or unusable event metadata would limit the proposed application rather than justify a race-day claim.

The high-level design here is distinct from Section 5's narrower R demonstration, which should use verified available data and report measured results rather than assume these validation outcomes.

### 5.0 Demonstration

#### 5.1 Chosen Dataset

Hyrox All Time Ranking Official Website (scrap hyrox_season8_male_pro.csv using python web-scrapper from GitHub - imterence/hyrox_analysis (2025))

- Use VSCode, clone repo, create and run scrape_male_pro.py to extract hyrox_season8_male_pro.csv.
- In RStudio, clean data, create geom_line / geom_point to show time degradation across each station, differing by gender.
- Using only first half of the race result, produce a model that predict the closest prediction to the next half race.
- Display the actual whole race and predictive model to see accuracy.

#### 5.2 Analysis Process

- from R

#### 5.3 Analysis Results

- from R

### 6.0 Standard for Data Science Process, Data Governance and Management

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

HYROX All Time Ranking Official Website
https://results.hyrox.com/season-8/?page=2695&event=HPRO_HYROXOVERALL&pid=list_overall&pidp=ranking_nav&search%5Bsex%5D=M

JGUG. (2024). Hyrox Race Results Dataset [hyrox_results.csv]. Kaggle.
https://www.kaggle.com/datasets/jgug05/hyrox-results

API, H. R. (2025). Hyrox Result API. Hyrox Result API. https://hyroxresultapi.com/
