# Pennsylvania-Election-Analysis

**Introduction:**

Greetings,

Welcome to the ‘Pennsylvania Election Analysis repository!

At the time of writing this report, it is mid September 2024 and the election season surge is in full effect. Along with pumpkin spiced lattes, football season and Halloween around the corner, it’s safe to say the upcoming Presidential Election in November has brought with it a seemingly omnipresent buzz of political activity and media coverage. It seems as though there is always some sort of campaign advertisement within ear shot or on our screens that’s designed to both capture the viewer’s attention and more importantly attempt to inform their decision of who to cast their vote for in a few short months. Although components that contribute to an individual’s political preference are numerous and extremely complex, its clear that factors affecting individuals’ day-to-day lives, such as economic conditions, educational experiences, and social interactions play a significant role in shaping their political affiliations. By taking a closer look at these socioeconomic conditions and their metrics coupled with demographic elements, could it be possible to train a deep learning model capable of determining a specific geography/population’s election outcome?

**Scope & Process:**

In an effort to answer the above question, this project sources a rich array of county level attributes for the state of Pennsylvania collected from the U.S. Census Bureau. The county level was chosen as election, demographic and socioeconomic data is well documented and widely available for this grouping. Not only has the state of Pennsylvania voted correctly for the national winner in 47 out of 59 of our nation’s elections, it is of particular importance as it represents a key swing state in this cycle with the most electoral votes and a pivotal path to victory win for Republicans and Democrats alike (Pennsylvania: 19. North Carolina: 16, Georgia: 16, Michigan: 15, Arizona: 11, Wisconsin: 10 & Nevada: 6).

In total, 102 metrics for the 40 most populous counties in Pennsylvania were collected across previous elections years 2012, 2016 and 2020 (2019). It is important to note that the ‘American Community Survey’ (ACS) 1-Year estimates are published only for geographies with a population of 65,000 or more, resulting in data for 40 out of the 67 counties in Pennsylvania. Furthermore, as the COVID-19 Pandemic rendered 2020 data unavailable, 2019 data was utilized for 2020. County specific attributes were compiled from the following U.S. Census Bureau datasets:

“Demographic and Housing Estimates” (DP05)
- Total male and female percentage
- Male and female percentage over 18 years old (voting age)
- Race percentage: White, Black or African American, American Indian and Alaska Native, Asian

“Educational Attainment” (S1501)
- 18 to 24 years old
Percent High school graduate (including equivalency)
Percent Bachelor’s degree or higher
- 25 to 34 years old
Percent high school graduate or higher
Percent Bachelor’s degree or higher
- 35 to 44 years old
Percent high school graduate or higher
Percent Bachelor’s degree or higher
- 45 to 64 years old
Percent high school graduate or higher
Percent Bachelor’s degree or higher
- 65 & Older years old
Percent high school graduate or higher
Percent Bachelor’s degree or higher

“Income in the Past 12 Months (Survey Year Inflation_Adjusted Dollars” (S1901)
Households, Families, Married-Couple Families, Nonfamily Households
- Total count for each group
- Percentage of each group that falls into the income brackets: Less Than $10,000, $10,000-$14,999, $15,000-$24,999, $25,000-$34,999, $35,000-$49,999, $50,000-$74,999, $75,000-$99,999, $100,000-$149,999, $150,000-$199,999 & $200,000 or More
- Median income (dollars) for each group
- Mean income (dollars) for each group

“Veteran Status” (S2101)
Total Population, Veteran Population and Nonveteran Population
- Percentage of groups above in the following age brackets:
18 to 34, 35 to 54, 55 to 64, 65 to 74 and 75 & Over
- Labor force participation rate for each group
- Unemployment rate for each group
- Percentage of each group with income below poverty level in the past 12 months
- Percentage of each group with any disability

“Fertility” (S1301)
Women with births in the past 12 months
- Birth rate per 1,000 Women in the following age brackets: 15-19 Years Old, 20-34 Years Old, 35-50 Years Old
- Received public assistance income rate per 1,000 Women

Once county level attribute information was extracted for the mentioned years, the datasets were cleaned/transformed, joined and loaded accordingly with special consideration for the imputation/dropping of ’NaN’/null values. Unsupervised machine learning principal component analysis and subsequent segmentation/clustering was performed for dataset enrichment as well and county level election outcomes were included via a binary classifier column (Republican = 1 & Democrat = 0). This county election result data was obtained via the “Harvard Dataverse”, which is maintained by the MIT Election Data and Science Lab (MEDSL). It was transformed from it’s raw state to include year, county and a Republican or Democratic outcome for all 40 counties studied.

The cleaned data was then separated into a labeled/target outcome and an attributes array with 102 input dimensions. The dataset was subsequently split into training and testing sets, scaled, and then utilized to compile, train, and evaluate an initial neural network model. This model achieved a loss metric of 0.1034 and an impressive accuracy metric of 0.9666. Model optimization was conducted via a Keras-Tuner approach to determine the best activation function, number of layers, number of neurons/nodes per layer, number of epochs, etc. The optimized model returned an improved loss metric and accuracy metric of 0.0386 and 1.0 respectively. Feature importance for this model was also determined by the SHAP (SHapley Additive exPlanations) method, which was graphically represented as the top ten features with highest SHAP values meaning they are the most influential in driving the model’s predictions toward a (1) or Republican outcome and the bottom 10 features with the lowest SHAP values describing the least contributing features to the model’s predictions toward a (1) or Republican outcome. Please refer to the repository’s ‘Resource’ directory for feature importance visualizations. This optimized model and its associated scaler were saved/exported in order to generate predictions for the most recent available county attribute data from 2023. The algorithm’s predictions for the winning political party for each of the 40 Pennsylvania counties was added back into the dataset, which was then combined with the 2012/2016/2019(2020) dataset to create a complete county dataset for additional visualizations. The Model’s predictions classified 18 of the 40 2023 counties as Democrat or (0) and 22 counties as Republican or (1).

**Conclusions/Insights/Extensions:**

The optimized model demonstrates a meaningful predictive power as evidenced by a high accuracy metric of 1.0 and low loss metric of 0.0567. As such, although the county level attribute metrics collected/included are both granular and multitudinous, deriving an algorithm capable of determining a specific geography/population’s election outcome based on demographic and socioeconomic factors is not only within the realm of possibility, but has the potential to provide valuable insights into voting behaviors and trends, allowing stakeholders to make informed decisions and strategies for future electoral engagement.

The principle/methodology of this model could be quite effective, especially if given the opportunity to digest relevant data from all the counties in a particular state. For example, with 100% of a state’s county attribute data a reliable algorithm of this nature could potentially predict the political party outcome of each county and in turn, be incorporated into a congressional district map to determine the state’s ultimate political affiliation and which party receives its coveted electoral votes. However, given the subject domain it’s important to note election data is extremely complex, often times highly dimensional and shouldn’t be oversimplified. While a framework of this nature has the ability to generate useful information from the binary classifier labels (Republican = 1 & Democrat = 0) themselves, another dimension of this model’s value is uncovered when looking specifically at its predictions as probabilities prior to the standard threshold value of 0.5 being used for conversion. When looking at the results on a scale from 0 to 1, we can identify which counties are more polarized than others. In other words, by determining which counties have outcome predictions that lie closest to the 0.5 threshold (boundary or marginal points), we’re able to highlight which counties exhibit the most uncertainty regarding their voting preferences and thereby identify key areas for targeted campaign engagement. This strategy could be utilized to generate an effective means of outreach, resource allocation and more, allowing a more optimized return on investment (i.e., votes!). Furthermore, taking into consideration the most influential features as determined by the model, campaigns can tailor their messaging and initiatives to resonate more effectively with the specific demographics and socioeconomic factors that drive voter behavior in these key areas.

In an effort to provide more context surrounding our model’s ability to highlight key ‘battleground’ or boundary/marginal counties via its probability outcome, a Tableau workbook was created and hydrated with two tables: One for election years 2012, 2016 and 2020(2019) that contains attribute columns ‘Year’, ‘County’ and ‘winning_party D:0 R:1 and the other containing 2023 model predictions with attribute columns ‘Year’, ‘County’, ‘winning_party D:0 R:1’ and ‘prediction_strength’. The generated dashboard is split into two visualizations displaying Pennsylvania counties mapped geographically. The top map (generated from the 2012, 2016 and 2020(2019) table) shows a breakdown of the 40 counties studied and indicates whether they voted Republican or Democrat by their fill colors being red and blue respectively. This visualization can be filtered to show county political results in any or all the studied years. When all is selected, states that changed their party affiliation are filled with a purple color indicating a shift in political alignment over the years. The bottom map (generated from the 2023 table with model prediction probabilities) incorporates a custom filter option that allows the user to select targeted upper and lower bounds for inclusion criteria based on the model’s predicted probability outcome for counties in 2023. Choosing a lower bound of 0.4 and an upper bound of 0.6 for example, filters results to only show counties represented as the ‘battleground’ or boundary/marginal with probability outcomes closest to the chosen threshold value 0.5. With these counties identified, the top visualization can now be utilized further to toggle between the historical political outcomes within the counties, showing that they have indeed changed colors over the years and are displayed as purple when all (2012, 2016 & 2020(2019)) data is included. Not only does this dashboard interactivity contextualize another way of model validation, but it represents a powerful tool that can add value for political/campaign strategists alike. Just as counties with probabilities closest to the threshold value can easily be identified for targeted approaches to win ‘middle’ or ‘undecided’ voters, more polarized (Republican/Democrat, extremely close to 1 or 0) counties can be identified for more grassroots campaigning to energize, engage and motivate a base.

**Limitations:**
- Only 40 of the 67 total counties in Pennsylvania had available data via the U.S. Census Bureau (American Community Survey 1-Year estimates are published for geographies with a population of 65,000 or more).
- 2020 U.S. Census Bureau data for the studied tables was unavailable due to COVID-19 complications. As such, metrics derived from 2019 were utilized for 2020.
- The relationship between an individual’s political affiliation and county-level outcomes is highly complex. A dataset that accurately captures the numerous attributes influencing a county’s voting outcome would be extremely high dimensional and near impossible to accurately define. Coupled with the fact that historically, United State Presidential Elections are decided by razor thin margins, a model’s validity, repeatability and extrapolation should rigorously tested and validated across various contexts to ensure its robustness and reliability in predicting electoral outcomes.
- Utilizing a real-time model may prove challenging in practice, as reliably collecting the specific level of detail regarding input dimensions in real-time is often unrealistic. Therefore, unless this situation changes, we will be unable to effectively employ this algorithm for predictive insights into the 2024 Presidential Election.

**Acknowledgements:**
- County level attribute data for this analysis were obtained from the U.S. Census Bureau
- Historical county level election outcome data were obtained from The Harvard Dataverse, maintained by the MIT Election Data and Science Lab (MEDSL). (https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/VOQCHQ)

**Repository Structure:**
- Tableau Public Link:
  - “tableau_pa_election_analysis”
  - Tableau Workbook with dashboard with maps displaying relationship between historical election outcomes and 2023 model predictions for the counties studied in Pennsylvania
  - https://public.tableau.com/shared/G49X8SQYH?:display_count=n&:origin=viz_share_link

- “ETL_County_Attributes.ipynb”
  - Code for 2012, 2016 & 2020(2019) dataset cleaning/preprocessing, including Principal Component Analysis and Segmentation

- “ETL_Extrapolation_Data_2023.ipynb”
  - Code for 2012, 2016 & 2020(2019) dataset cleaning/preprocessing, including Principal Component Analysis and Segmentation

- “ETL_pres_results.ipynb”
  - Code for 2012, 2016 & 2020(2019) county election results (labeled/target outcome for binary classification) cleaning/preprocessing

- “ETL_Complete_County_Data.ipynb”
  - Code for the joining of all datasets (2012, 2016, 2020(2019) & 2023) with segments and outcomes

- “ETL_tableau_data_2023”
  - Code for the cleaning/preprocessing of datasets utilized for Tableau Workbook. Includes the incorporation of 2023 model predictions as raw probabilities

- “County_Election_Deep_Learning_Model.ipynb”
  - Code for the compilation, training and evaluation of the neural network model
  - Includes SHAP feature importance analysis and Keras-Tuner model optimization

- “2023_Model_Predictions.ipynb”
  - Code for 2023 county predictions via the optimized neural network model

- “Pennsylvania Election Analysis - Election 2024.pdf”
  - PDF Presentation

- ‘Resources’ Directory
  - (40) Excel spreadsheets containing U.S. Census Bureau sourced data for counties in Pennsylvania across the years 2012, 2016, 2020(2019) and 2023. (Demographics, Education, Income Households, Income Families, Income Married-Couple Families, Income Nonfamily Households, Total Auxiliary, Veterans Auxiliary, Nonveterans Auxiliary & Fertility)
  - (9) CSV files containing exported DataFrame results from cleaning/preprocessing and analysis
  - (3) Keras files for the initial model, optimized model, optimized model retrained and (1) PKL file for optimized model’s scaler
  - (4) PNG files displaying accuracy and loss metric visualizations for both the initial and optimized models
  - (4) PNG files displaying a graphical representation of the optimized model’s feature importance (Top 10 and Bottom 10 features - reformatted PNG files include also)
