# Project 02 Proposal: Anmol Virdi

**Team members:** N/A

See the [Project 02 instructions](https://data385.github.io/project/proj02_wholegame.html) for what goes in each section. Bullet points are great. Put any images in an `img/` folder.

## 1. Problem statement

_Who has the problem, what decision or need it involves, why it matters, and what you expect to find._

County health departments have small budgets and need to decide which health behaviors to look into to reduce chronic disease. Cancer affects a large number of adults and requires long term care, so knowing which community level factors go along with higher cancer prevalence can help departments prioritize prevention programs. I expect counties with higher smoking, obesity, and/or physical inactivity rates to have higher cancer prevalence, but the link may be weaker than expected because cancer prevalence counts survivors, not new cases. 

## 2. Research question

_A clear association between a response and at least one predictor variable._

Is a county's cancer prevalence associated with its smoking, obesity, and physical inactivity rates? 

## 3. Data acquisition

_In 1-2 sentences, describe the data you intend to collect and how you will get it._

I will use the CDC PLACES county level dataset, 2025 version. It gives estimates of health outcomes and behaviors for every U.S. county. I will download it directly from the CDC website.

### a. Confirm viability

_Scraping: summarize the "Before You Scrape" checklist. Downloading: citation, link to the documentation, and how the data were originally collected. Collecting it yourself: how you are collecting it and where you are storing it._

- **Open Source**: The dataset is published by the CDC and is in a public domain
- **Citation**: Centers for Disease Control and Prevention. PLACES: County Data (GIS Friendly Format), 2025 release. data.cdc.gov. <https://data.cdc.gov/d/i46a-9kgh>
- **Documentation:** [PLACES methodology](https://www.cdc.gov/places/methodology/) and [measure definitions](https://www.cdc.gov/places/measure-definitions/).
- **How it was collected**: The estimates come from a small area estimation method that starts with BRFSS survey data, a national health survey for adults, and uses a model to estimate county level values. They are only model based estimates and not direct counts. 

### b. Variables of interest

_If you do not have the data yet, list the variables you plan to collect and the exact question text. Include 1) the variable name, 2) data type 3) description. Use bullet points._

- **CountyName**: county name (categorical, used for labeling)
- **StateAbbr**: state abbreviation (categorical)
- **TotalPopulation**: county population (quantitative)
- **CANCER_AdjPrev**: age-adjusted % of adults who have ever had cancer (not including skin cancer) or melanoma (quantitative, response)
- **CSMOKING_AdjPrev**: age-adjusted % of adults who currently smoke cigarettes (quantitative, predictor)
- **OBESITY_AdjPrev**: age-adjusted % of adults with obesity (quantitative, predictor)
- **LPA_AdjPrev**: age-adjusted % of adults with no leisure-time physical activity (quantitative, predictor)
- **CountyFIPS**: county ID code (identifier, not used as an analysis variable)
- **Region**: U.S. Census region made from StateAbbr (categorical, control variable)

## 4. Data example

_Scraping: a URL and an annotated screenshot. Downloading: a link to your rendered `data_management.pdf` with `glimpse()`. Collecting: a screenshot of one example row._


## 5. EDA

_The techniques and functions you plan to use to import, wrangle, visualize, and explore the data, and why each is needed._

- **Import**: I’ll load the CSV with read_csv() and tidy the column names with clean_names() so they’re all lowercase and easy to type.
- **Inspect**: glimpse() and summary() will show what each column is and whether the numbers look reasonable.
- **Wrangle**: Population has commas in it (like “14,369”), so I’ll use parse_number() to turn it into a real number. I’ll use case_when() to make a Region column from the state.
- **Missing** **data**: Kentucky and Pennsylvania have no cancer estimates. I’ll use vis_miss() to see how much is missing, then filter() those rows out and say so in the report.
- **One variable at a time**: I’ll make histograms with ggplot() for each number variable, a bar chart of Region, and a tbl_summary() table to see what the data looks like.
- **Comparing variables**: I’ll make scatterplots of cancer against each predictor with a trend line, boxplots of cancer by Region, and run cor() to check whether the predictors overlap.

## 6. Modeling

_The models you plan to use, your response and predictor variables, and why the models fit your research question._

I’ll use multiple linear regression: lm(CANCER_AdjPrev ~ CSMOKING_AdjPrev + OBESITY_AdjPrev + LPA_AdjPrev + Region).

Cancer prevalence is the response. Smoking, obesity, and physical inactivity are the predictors, and Region is there so I’m not just picking up differences between parts of the country. Linear regression fits because cancer prevalence is a number, and I want to see how much each predictor goes with cancer while the others stay fixed.

To check how well it works, I’ll look at R-squared, residual plots, and the prediction error (RMSE) on a test set I hold out. Smoking, obesity, and inactivity probably rise together, so I’ll check that with vif() too.

## 7. Communicate

_How you plan to present your results (website, poster, or short slide presentation)._

I will present my results as a short slide presentation.

## 8. LLM disclaimer

_Whether and how you will use AI tools: the specific tool, its use, and your data protection methods. If you won't use any, say so._

I will use Claude to help me plan out the project, help me with debugging, and grammar check my writing. The data is public and doesn't have private information so nothing private will be shared. I will be writing and understanding my own analysis and code with suggestions on how to go about it. 
