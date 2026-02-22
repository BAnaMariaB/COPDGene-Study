# COPDGene-Study

###  Introduction to the topic of COPD and motivation for the analysis
Chronic obstructive pulmonary disease (or COPD) is a progressive respiratory condition. Some common characteristics are airflow limitation and abnormal inflammation responses in the lungs. This dieseas affects more than 16 million Americans. It is also ranked fourth leading cause of death in the United States. To determin how sever COPD is in a patient, we use spirometry. From this assement we get some key measures such as forced expiratory volume in one second (FEV1) and forced vital capacity (FVC). <br>
The progression of COPD can span years. Thus understanding how baseline clinical, demographic and imaging variables can relate to lung function, in a follow-up assement is critical. Understanding these variables will allow us to identify patient risk stratification and modifiable factors.

###  Description of the purpose of the study/report
In continuation, in this study we'll examine the relationship between five-year follow-up FEV1 (or phase 2) and the COPDGene cohort. This study has multiple pusposes. Firstly we'll to seek to characterize missingness and distributional properties of key variables. Secondly we'll explore univariate associations between FEV1 phase2 and both categorical and continuous predictors. Lastly we'll perform inferential testing, this includes nonparametric bootstrapping (for categorical comparisons) and simple linear regression for continuous predictors.

### Data used to perform the analysis
For our analysis we'll draw upon three linked data sources collected by the COPDGene reseach group. This data included demographic information, spirometric measures, and quantitative imaging metrics.
* Spirometry measurements include FEV₁, FVC, FEV₁/FVC ratio, and the five-year follow-up FEV1 phase2.
* Imaging variables capture lung volumes and emphysema percentage at baseline.
* Demographic and clinical covariates captured age at visit, body mass index, average cigarettes smoked per day, history of asthma, smoking status (current and former) and physician-diagnosed COPD status.


### Methodology

1. **Data Wrangling & Descriptive Statistics:** <br><br>
   We started by combining three datasets from the COPDGene study: demographics (CSV), imaging (JSON), and spirometry (HTML). We loaded them and then merged into a single data frame. In continuation, we cleaned the data by changing the way some of the data was presented, such as adding height and weight  in inches and lbs and removing some of the redundency in the data.  We fooled up by creating a custom summary function to assess variables by type. For numeric variables, it computed the mean and standard deviation. And for categorical variables, it provided frequency counts. This function soon revealed ten missing entries in several spirometry and imaging variables. We, later on, identified 20 participants with at least one missing value. We removed those rows and only retained the cleaned dataset for the subsequent analyses.

2. **Exploratory Data Analysis (EDA):**<br><br>
   We generated histograms for all numeric and integer variables using the ggplot2 library. We also computed our own bin widths. This proved neccesary since the way some of the data was displayed didn't help with the analysis. We computed the bins width as one-thirtieth of the variable’s range. Several features such as emphysema percentage, lung volumes, average cigarettes/day or BMI showed skewedness in their distributions. Thus we applied log transformations to these variables, this helped to normalize them and enhance interpretability on the histograms.

3. **Categorical Comparisons:**<br><br>
    In continuation we explored how FEV₁ phase2 varied across five categorical variables. We used gender, race, smoking status (current vs. former), asthma history, and COPD diagnosis.To visualize the distribution we used boxplots. This highlighted group differences in median and variability. To further our inspection we applied a bootstrap-based inference method (with B = 1,000 resamples) to test for statistically significant differences in group means. For each variable, we have computed :  the observed difference in means, a 95% bootstrap confidence interval, and a two-sided p-value.

4. **Continuous Associations & Regression:** <br><br>
   Lastly we explored the relationships between FEV₁ phase2 and five numeric predictors. We used visit age, BMI, cigarettes per day, inspiratory lung volume, and emphysema percentage. To visualize the trend and shape of the data we used scatterplots with LOESS smoothing. These visualizations revealed a mix of linear and non-linear associations.To continue, we then fitted five separate simple linear regression models (one for each predictor) to test whether the slope of each variable was significantly different from zero. From this, for each model, we reported the estimated slope, its 95% confidence interval, and the p-value. From this analysis we can obtain statistical support for the observed trends in lung function at five-year follow-up.


### Results
After this analysis, our results are as follows :

* We found missing data. 20 participants were excluded, because either their spirometry or imaging was missing. The remaining data showed no further missingness.
* With the exception of few, most continuous variables were approximately symmetric. Those who weren't we used log-transforming to facilitate more reliable model fitting.
* The bootstrap comparisons yielded all five subgroup differences in mean FEV1 phase 2 proved statistically significant.
    * Gender: We are 95% confident that the true mean difference in five-year FEV1 between males and females lies between 0.16 L and 0.32 L(observed ≈ 0.24 L, p = 0.001).
    * Race: We are 95% confident that the true mean difference in five-year FEV1 between White and African-American participants lies between 0.085 L and 0.28 L (observed ≈ 0.18 L, p = 0.001).
    * Smoking Status: We are 95% confident that the true mean difference in five-year FEV1 between former and current smokers lies between 0.34 L and 0.50 L (observed ≈ 0.42 L, p = 0.001).
    * Asthma History: We are 95% confident that the true mean difference in five-year FEV1 between those without and with asthma lies between 0.48 L and 0.70 L (observed ≈ 0.59 L, p = 0.001).
    * COPD Diagnosis: We are 95% confident that the true mean difference in five-year FEV1 between participants without and with COPD lies between 0.72 L and 0.90 L (observed ≈ 0.81 L, p = 0.001).


#### **Regression Analyses:**
 * Age is negatively associated with five-year FEV1 (slope = –0.032 L/year; 95 % CI [–0.035, –0.029], p < 0.001), this reflects the expected decline in lung function with advancing age.
 * Inspiratory lung volume demonstrated a strong positive relationship (slope = 0.165 L per liter of inspiratory capacity; 95 % CI [0.139, 0.190], p < 0.001).
 * Emphysema percentage was inversely related (slope = –0.042 L per percent emphysema; 95 % CI [–0.045, –0.039], p < 0.001).
 * Average cigarettes per day showed a modest but significant negative slope (–0.0038 L per cigarette, 95 % CI [–0.0063, –0.0009], p = 0.016).
 * BMI was not significantly associated with FEV1 phase2 (slope = –0.0015 L/kg·m⁻²; 95 % CI [–0.0066, 0.0040], p = 0.54).

### Conclusion
In conclusion, baseline age, inspiratory lung volume, emphysema burden, and the categorical factors (gender, race, smoking status, asthma, COPD diagnosis) all showed significant associations with FEV1 at five-year follow-up. In contrast BMI has not show any statical significants. Quantitative imaging metrics—baseline inspiratory lung volume and emphysema percentage—emerged as the strongest predictors of lung function five years out. Age and smoking intensity also had significant, though smaller, impacts. BMI showed no meaningful linear effect. The clear FEV1 gaps across gender, race, smoking status, asthma history, and COPD diagnosis highlight the need for personalized risk stratification. Clinically, these results support early imaging‐based evaluation and targeted strategies to preserve lung volume and slow emphysema progression in COPD management.
