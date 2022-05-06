# emergencycallsth
Analysis of the ambulance calls prediction in the city of The Hague (NL) with the introduction of demographic and geographical features

# Introduction

Local policy-makers have the responsibility to provide fast and reliable
emergency medical services to every citizen in their region, and to do
so adequate planning is necessary to guarantee high service levels by
deploying ambulances where and when they are needed. To reach these
goals, policy-makers must first understand which are the demographic and
geographical factors influencing emergency calls.

Scientific literature proposes several methods to forecast emergency
calls, however most models proposed base their estimates on time series
and fail to identify the demographic variables underlying ambulance
calls. This is a crucial downside in an increasingly urbanized world in
which cities and their neighbourhoods change steadily because of
phenomenons like immigration and gentrification, and this is why an
hybrid model, including both demographic and geographical features, is
proposed to explain emergency calls.

In this study, an analysis has been performed on the city of The Hague
(NL), in which a regression model is used to identify a mix of both
demographical and geographical variables that represent statistically
significant predictors for the number of ambulance calls in a grid of
500 x 500 meters.

# Literature review

A literature review has been executed on predicting ambulance calls
using the search engines [*Scopus*][] and [*Google Scholar*][]. The
keywords used to identify relevant literature are shown in the table below, under
the concept they pinpoint.

<div class="center">

<div id="table:concepts">

| *Ambulance calls*          | *Inclusion of spatial features* | *Prediction attempt*    |
|:---------------------------|:--------------------------------|:------------------------|
| Ambulance                  | Spacial                         | Predicting              |
| Ambulance calls            | Spatio-temporal                 | Forecasting             |
| Emergency                  | Geospatial                      | Modeling                |
| Emergency calls            | Space-related                   | Model                   |
| EMS                        | Geographic                      | Pattern                 |
| Hospital                   | Geographical                    | Estimating              |
| Emergency medical services |                                 | AI                      |
|                            |                                 | Artificial Intelligence |
|                            |                                 |                         |

</div>

</div>

Setzler, Saydam, and Park (2009) used artificial neural network (ANN) following the forward feeding multilayer perceptron model to predict emergency calls in a one-hour and three-hours range in 2x2 and 4x4 miles grid over Mecklenburg Country, MC. This model uses as inputs the number of ambulance calls, their time and location information and provides as output the number of calls for each cell in the grid for each hour range. The researchers found that for 2x2 miles grid and one-hour time granularity, the traditionally employed system (based on a moving average) outperformed consistently the ANN model, while for other levels of spacial/temporal granularity the ANN system outperforms the moving average model, however it is worth noticing that this model tends to systematically underpredict ambulance calls, which can strongly affect the efficacy of the ambulance service. This study shows how difficult it is to accurately forecast at finer spatial and temporal granularity and does not include any demographic variable (population, age, employment,...).

Zhou, Matteson, Woodard, Henderson, and Micheas (2015) estimated emergency calls in a two-hours interval in Toronto (Canada) by employing time-varying Gaussian mixture models in which the component distributions are common through time, while the mixture weights change over time. The input are number of calls, timestamp and location they were made, and the output is an ambulance demand density. The prediction accuracy of this model outperforms the standard model, however this model to be applied requires point process to be time invariant, and/or data to be sparse at the desired temporal granularity to describe spatial structures accurately. Other limitations of this study are the lacking demographic features and the strict boundaries of analysis (the only city of Toronto) which do not allow to adequately analyse the peak of density near the borders of the city.

Zhou et al. (2015) hypothesised that ambulance calls should not be treated as random events because some medical events (e.g. cardiac arrests) have definite time-geographic distribution patterns related to the underlying population demographics. To analyse ambulance calls in Singapore, the data collected were: number of calls, including time of the call and specific address, patient characteristics (age, sex, race). These data were plotted using GIS and correlated to the time of day and day of the week, call occurrence was then compared with the census district population demographic in the district the call was made. The results showed that call occurrence follows the pattern of how circadian cycles affect some diseases, and that call occurrence was higher in Easter and Southern suburban areas, roughly corresponding to population density. Despite collecting some of these data, the study did not correlate age and social economic status to ambulance calls, and this represents a clear gap in identifying the non-random distribution pattern of emergency calls.

Martin, Mousavi, and Saydam (2021) analyzed the emergency calls of an unspecified metropolitan (country) area in the USA and used ARIMA, Holt-Winter’s (HW) exponential smoothing method, and a multilayer perceptron (MLP) model to produce daily call volume forecasts, the predictions were then compared to the traditional moving average employed traditionally. The features used for the prediction are: volume of calls, their position, year, season, month, week, day, day of week, hour of each call. The study finds that when producing spatially distributed call volume forecasts, MLP models significantly outperform traditional time series methods across the different spatial clustering levels evaluated. However, this study is missing features explaining population characteristics and suggests that these might be added together with weather data (e.g. temperature, rain) to enhance the performances of the prediction.

Sasaki, Comber, Suzuki, and Brunsdon (2010) used demographic factors and population changes to compare the number of ambulances needed in the future and their location. To do so, data about ambulance calls and their location, together with data about age groups, gender, number of firms employing more than five people, and number of employees were collected for every census area in the prefectural capital city of Niigata. Then, a multivariable regression analysis was used as a prediction model to estimate future demand for ambulance calls. The results show that age is a significant feature when predicting ambulance calls, as the age group of 65 years old and older accounted for almost half the total calls, while for the age group 15 to 54 years old the number of calls were comparable. Also the number of companies with more than five employees were statistically associated with the number of calls. Using this information, the researchers suggest the use of demographic variables to predict emergency calls and to locate ambulance, as these data are “convenient”, as they are the most collected variables, and can be easily integrated with other data (like other socio-economic variables). Considerations about income and future city planning as missing, and therefore the model might miss crucial information on where the people will move in the future (care homes for the elderly, new business centers), these represent limitations of the study.

Summarizing, the literature review shows that: currently there is no standard in the choice of a prediction model for ambulance calls; there are a wide set of valid approaches to the problem, each with its own setbacks; there is an ongoing debate about which features are needed to predict emergency calls, most researchers agree that this prediction field should include both demographic and geographical variables to better understand the uniqueness of the districts in the cities and their different health needs; modern prediction models for ambulance calls are not sufficiently explored by policy makers and instead it should be an important discussion point to guarantee a high service level to every citizen.

# Methodology

## Data used

The data used were retrieved from 112 Nederland and three datasets were used: ambulance calls, containing information about when and where an ambulance was requested; neighbourhood information, containing demographic and geographical information about every neighbourhood in The Hague; and grid information, containing demographic and geographical information about the city of The Hague in cells of 500 meters x 500 meters. All the datasets used refer to the year 2018.
Based on the literature analysed and stated hypothesis, the feature selected for the analysis are:

- n grid: unique code identifying each cell in the grid.
- BU CODE: unique code identifying each neighbourhood.
- pop: number of inhabitants in the cell on the 1st January 2018.
This feature has been selected as literature shows that population density is one of the main predictors of ambulance calls (Peacock & Peacock, 2006) (Zhou et al., 2015).
- per for: percentage of residents of whom at least one parent was born abroad in one of the countries of Africa, Latin America or Asia (excluding Indonesia and Japan) or in Turkey.
This feature has been used because several studies propose that foreigners might use emergency care more intensively than local population (Mahmoud & Hou, 2012) and underutilize primary care services (Yeo, 2004) due to language and cultural barriers.
- dist road: average distance of all inhabitants in an area to the nearest entrance to a national or provincial road, calculated over the road.
This feature has been included on the hypothesis that the closeness to a national/provincial road indicates a higher level of calls caused by car accidents.
- unemp: percentage of residents who receive unemployment benefits, a social assistance or assistance-related benefit or an occupational disability benefit. This concerns benefits to residents up to the state pension age. A person who receives several types of benefits is counted once.
This feature has been included on the hypothesis that unemployed citizens will be at home, therefore in “their cell”, more often than employed citizens, and this will drive up the number of calls inside the cell.
- p0 14: percentage of inhabitants under 15 years old.
This feature has been included based on the literature review (Sasaki et al., 2010).
- p65 PL: percentage of inhabitants over 64 years old.
This feature has been included based on the literature review (Sasaki et al., 2010).
- med inc: standardized median income, classified as: Low, Mid, or High.
This categorical variable has been included under the hypothesis that wealthy people are, on average, more healthy than the average population, and therefore their need for an ambulance will be lower.

A 500 meters x 500 meters grid has been used instead of the official neighbourhoods of the city because this, smaller and standard, level of granularity helps to remove the effect of varying size of neighbourhoods and gives more precise information on where ambulances are requested, other than controlling for historical and political factors that gave birth to neighbourhoods.

## Data pre-processing

Initially, the data went through a translation process, with the selected features translated from Dutch to English. Then, they went through a cleansing process. Since these datasets contain sensitive information, in every cell with less than 50 inhabitants some information (age groups, income,...) were hidden and shown as outliers, these cells were removed from the analysis.

The features age groups and unemployment have been transformed into percentages of the cell population to remove correlation with the population density variable.

The distributions of the data were then normalized first by checking the skewness of the distribution and ensuring it felt in the normality range (in a range of ±0.8), if this condition was not met then a square root transformation was executed. The square root transformation has been preferred to the log transformation since the data presented several values equal to zero. Then a standardization (Z-Score transformation) was applied to the features.

These transformations were applied to increase the comparability of the features and to bring all the features on a equal unit of measurement. This is an important step to avoid subsequential prediction models giving too much importance to some features just because of the high values they assume.
The datasets emergency calls and grid have been merged based on their geographical feature to assign every emergency call to the cell in which it was made. Then, the number of calls for each cell was calculated to obtain a final dataframe in which the number of ambulance calls was added as a feature in the grid dataframe.

This dataframe was merged again based on the geographical feature with the neighbourhood dataframe to add the neighbourhood feature to each cell of the grid. Since each cell can partially belong to multiple neighborhoods, the area that each cell shared with each neighbourhood was calculated, and each cell has been assigned to the neighborhood with whom it shares the greatest area. The resulting dataframe has been used to build the prediction model.

## Method selection

A multiple regression analysis (MR) has been chosen to analyse emergency calls because of its flexibility and the possibility to include multiple independent variables, both quantitative and categorical in nature. Moreover, a multiple regression model is suitable both for prediction and explanatory research (Venter & Maxwell, 2000), crucial steps in understanding and forecasting ambulance calls.

First, a linear regression has been performed using quantitative features to: identify the statistical significance of the features; the prediction power of the model (adjusted R2); and the multicollinearity among independent variables, to assess this effect the Variance inflation factor (VIF) has been used with 10 as a threshold value for the suitability of the model (Alin, 2010).

After, the error in the prediction of the number of calls in a cell with the above defined model has been plotted against the error in the prediction of the number of calls in the closest cell as shown in Figure 7. The results show that indeed errors tend to cluster (e.g. high positive error in a cell corresponds to high positive error in the closest cell), this suggests that the number of ambulance calls has an underlying geographical feature, to model this feature the closeness among the cells will be included in the model to find out if this improves the prediction power.

A new linear regression model has been created to includes the feature neighbourhood (“BU CODE”) as a categorical variable. As stated above, this is done to model the closeness of the cells and how this influences ambulance calls. Also, including this new categorical variables in the model helps it control for other geographical effects unidentified by demographic features.

The linear regression model works under the assumption that: there is indeed a linear relationship between the dependent variable and independent variables or between transformations of such; the model is correctly identified, as omitting relevant features will result in insufficient estimation power of the model; the errors of the model are normally distributed.

In this analysis, a linear relationship has been implied among the square root transformation of the dependent and independent variables based on how the distributions plot against each other. The scope of the research is to correctly identify the model, whether the goal was reached is discussed below.

# Results

The summary of the first model is shown in appendix A.1. Population, percentage of foreigners, distance from the closest provincial/national road, percentage of unemployed, percentage of inhabitants who are less than 15 years old, and percentage of inhabitants who are more than 65 years old are used to predict ambulance calls. The constant term and all the variables included (excluding unemployment) appear to be statistically significant (p-value < 5%) when predicting ambulance calls. In magnitude, population is shown to be the first feature influencing, positively, the number of ambulance calls, followed by the distance from the closest provincial/national road. The adjusted R2 of the model is equal to 57%, meaning that the independent variables used explain more than half the variance of emergency calls. The level of multicollinearity among the independent variables is 10, considered on the verge for the acceptance of the model.

This model shows that population density, percentage of foreigners, distance from the closest provincial/national road, and percentage of inhabitants who are 65 years old or more are positively correlated with the number of ambulance calls. On the other end, an increasing percentage of inhabitants who are younger than 15 years old is shown to be negatively correlated with the number of emergency calls.

The summary of the second mode is shown in appendix A.2. After including the categorical variables neighbourhood and median income, the distance from the closest provincial/national road is no longer significant. The opposite effect shows for the p-value of unemployment, which is still not significant but is now down to 18% from 47%.

In this model, the main features predicting ambulance calls are now the neighbourhood levels, with magnitude level significantly higher than the demographic features. The prediction power of the model increased, with the adjusted R2 up to 67.3% and R2 up to 80%. When including categorical variables, the p-value specifies whether the coefficient estimated for the level is significantly different from the one of the reference level, so this model points out that for some neighborhoods the coefficient is not significantly different than the one of the reference level, while for other neighbourhoods it is significantly different (reference level + coefficient of the categorical variable). At the same time the levels of median income are not statistically significant, indicating that, when controlling for neighbourhood belonging, it is not statistically significant to include income groups.

# Discussion

Predicting ambulance calls is a crucial responsibility for policy-makers to identify when and where ambulances are to be deployed in the city. To build an adequate prediction model, it is crucial to start by identifying which characteristics in the population/city actually influence the number of emergency calls.

This study proposes the identification of the relevant features with the use of a multiple regression analysis (MR). The results show that it is feasible to build a predicting model based on demographic variables alone, confirming the suggestions of Sasaki et al. (2010), but to improve the prediction power a new hybrid model is proposed that includes additional features to control for geographical characteristics of the city.

The hybrid model contains both demographic and geographical characteristics. Analyzing the summary of this model, the geographical feature “neighbourhood belonging” is significant for some neighbourhoods but not for everyone, meaning that belonging to some neighbourhoods in the city has a significant effect on the number of calls not otherwise explained by demographic variables. Considering only the neighbourhoods in which the feature is significant, the summary shows that the coefficients of these neighbourhoods are always negative (Figure 9), so in these areas the number of ambulance calls is always lower than the one predicted by the demographic variables only. These neighbourhoods mostly correspond to the parks in the city (e.g. Oostduinpark, Madestein, Haagse Bos), showing that the neighbourhood feature is indeed a good feature to correctly control for geographical features like parks.

The demographic variables population, percentage of foreigners and age groups 0-14 and 65 and over, are still significant in the hybrid model, showing that, coherent with the literature, these variables are fundamental to explain ambulance calls.
When the geographical features are added, the distance from provincial/national roads does no longer explain the number of ambulance calls, this could happen because in the previous model this variable was also explaining some geographical characteristics (e.g. how people cluster in a city based on their income level) that are now being controlled by the new model.

The same reasoning can be used to explain what happens to the unemployment feature, but this time the effect is opposite and the feature is significant at a significance level of 20%. It is worth noting that the strength of this correlation is negative, counter to the hypothesis that higher unemployment would lead to higher ambulance calls, further studies could try to include additional features to analyze whether this variable (and the relationship with ambulance calls) becomes statistically significant.

It is also important to notice that a low median income has a positive coefficient and would be significant with a significance level of 10%, coherent with the stated hypothesis that a lower median income might imply higher ambulance calls.

From these considerations, in this work a set of features mixing demographic and geographical features is proposed as a tool for policy-makers to improve the forecast of ambulance calls in The Hague. This mix tries to close the gap in the scientific literature where geographical or demographic data are used independently to predict ambulance calls, moreover these features represent a convenient set of data collected by most national statistical offices worldwide.

# Conclusion

This study started with the question: in the city of The Hague, which demographic and geographical features represent feasible features to predict ambulance calls?

In this research, based on literature review a group of demographic variables have been chosen and used to predict ambulance calls in the city of The Hague (NL) using the geo-scale 500 meters x 500 meters. To improve the prediction power of the model, a geographical feature has been added to the model: neighbourhood belonging.

The results show that the features population, percentage of foreigners, percentage of inhabitant in the age groups 0-14 and 65 and over, median income and neighbourhood belonging represent a valid set of features to explain the number of ambulance calls in the city of The Hague (NL) using a geo-scale of 500 meters x 500 meters.

## Limitations and further work

Given the limited availability of data, only a few demographic and geographical variables have been used in the research, further studies could include more features to help the model control for more and more geographical and demographic characteristics. An example would be to gather data about ethnicity to model how ethic groups cluster in a city and raise considerations about how different ethnic groups/cultures perceive ambulance calls.

Also, the model assumed a linear relationship between the independent variables and the dependent variable, even if the predictive power of the model is quite high and we have no indication that the relationships amongst the variables correlated with emergency calls are not linear (Sasaki et al., 2010), this relationship can be questioned and further studies could propose a different relation.

In this study the potential correlation between the two categorical variables (neighbourhoods and median income) has not been analysed, further studies could take into account how these variables are spatially related to see if they are correlated and so one feature can be used to explain the other (e.g. similar income groups live in the same cell/neighbourhood).

Starting from the findings of this study, a prediction model can be built to predict ambulance calls on a small time granularity (e.g. hourly/daily/weekly), this can be done by employing several tools, like moving averages and artificial neural networks.

  [*Scopus*]: https://www.scopus.com/search/form.uri?display=basic#basic
  [*Google Scholar*]: https://scholar.google.com
  
