# Estimating-NBA-Player-Box-Plus-Minus-using-Linear-Regression
A regression project that aims to the predict an NBA player's box-plus minus given the player's per-game stats.

Note this project was my final project of the course Methods of Data Analysis (course code: STA302) at the Unviersity of Toronto.



# Introduction

Measuring a player’s all-around impact on their basketball team is a difficult but important task. Fundamentally, basketball games are won by one team outscoring the other team throughout the duration of the game. Research has shown that teams with high points differential tend to be more successful. Therefore, a reasonable measure of a player’s impact is box-plus-minus (BPM), which is a per-game statistic that measures the points differential of the player’s team and the opposing team when the player is playing in the game. Previous research has concluded that BPM is a useful metric for evaluating a player’s value (Terner 2021). The goal of this research project is to study how a player’s individual game performance contributes to overall team success. In this research project, a linear regression model will be created to predict a player’s average BPM based on their per-game stats and understand which factors influence the statistic. By being able to understand what factors influence a player’s BPM, team managers can construct a team and allocate their resources in a way that optimizes team success. Furthermore, by being able to accurately predict a player’s BPM, team managers will have a rough idea of how impactful future prospects will be for their team.

# Methods

Initially in the analysis, the dataset is equally split into two parts, a training dataset and test dataset. The training dataset will be used to build and decide on a final model. Statistical tools used to find a final model are anova F-test, variance-inflation factor (vif), adjusted R-squared values, all possible subsets selection method and stepwise selection method. The anova F-test is used to verify whether at-least one predictor in a given model is significant to the response (BPM). If a model passes the anova F-test it should provide a good starting point for constructing a final model. The vif is used to detect possible multicollinearity in the predictors which should be done before constructing a model but after checking assumptions of a full model. Vif is used to decide which predictors in model have multicollinearity. Based on severity of the vif a decision will be made for whether or not those predictors should be removed from the model. The all-subsets selection method is used to find the best model for each number of predictors based on adjusted R-squared values. Then the best models of each size can be compared using their adjusted R-squared values to decide on model that has a balance of explaining a substantial amount of variation in the response as well as simplicity for comprehension. The stepwise selection method is one in which a model is chosen by adding or removing one predictor at a time based on a penalty term such as AIC or BIC until predictors cannot be added nor removed. This will result in a model that accounts for the conditional nature of regression and can be compared to the all-possible subsets method to help decide on a final model. After each model is created, conditions 1 and 2 are checked to ensure that residual plots of the model can be interpreted correctly. The residual plots will then be checked for any violations of the linearity, non-constant variance, uncorrelated errors, and normality assumptions.

If the linearity, non-constant variance, or normality assumptions are violated a transformation can be used to attempt to correct the assumption violation. However, if the uncorrelated errors assumption is violated it should be discussed as a limitation of the model. Additionally, problematic observations such leverage, outliers, and influential points should be checked for each model and should be used to compare different models. If problematic observations are found they should be discussed as limitations of the model and can be used as justification as to why one model is superior to another. Once decided on a final or between a few models, the test dataset will be used to validate the model(s) to conclude whether a model is a good model or has been overfitted on the training dataset. Assumptions and problematic observations should once again be checked when doing model diagnostics on models using the test dataset.

# Results

## Data Summary

<img width="313" alt="summary-statistics" src="https://user-images.githubusercontent.com/43526001/235789813-6d338b90-8739-4499-9183-99e2169a5841.png">


From looking at Table 1 we see that the training and test dataset are very similar in relation to the
summary of each variable.


<img width="542" alt="bpm-histogram" src="https://user-images.githubusercontent.com/43526001/235789869-e129138f-5808-494e-8419-faa93ef41501.png">

We can see from the histogram of the response (BPM) that it is normally distributed.

## Analysis Process and Results

I began constructing a final model by creating a full model which includes every
predictor. I used the anova F-test to determine whether the full model has at least one significant
predictor and it passed. I then checked the conditions as well as assumptions for the model and
found that that all were held. I proceeded by checking for multicollinearity in the model using
the vif of each predictor. I found that some predictors had extremely high vif values causing a
multicollinearity problem.

<img width="509" alt="VIF" src="https://user-images.githubusercontent.com/43526001/235789944-ef4ae468-6813-4e48-8bb4-44f694af6032.png">

To reduce the multicollinearity present in the model, I attempted to remove predictors
that were mostly summarized by other predictors. For example, “X3P.” (three-point percentage)
summarizes “X3P” (three pointers made) and “X3PA” (three pointer attempts), while I feel that
taking “X3PA” into account along with the “X3P.” is crucial, by removing those predictors we
get significantly less multicollinearity in our model. Similar reasoning was used to justify
removing “FG”, “FGA”, “FG.”, “FT”, and “FTA”. Furthermore, “TRB” was removed as “TRB”
= “ORB” + “DRB”, therefore “TRB” can be removed to reduce multicollinearity while not
losing any additional information. Additional, “eFG.” (effective field goal percentage) is the
number of shots made divided by the number of shot attempts, however “X2P” has a weight of 1
and “X3P” have weight of 1.5. Therefore, “eFG.” summarizes FG. (field goal percentage) and
gives back some information that was lost when removing X3P and X3PA. While there are some
predictors that still have concerning VIF values to worry about such as MP, AST, TOV, and
PTS, all these predictors should be highly related to the response and reflect a player's impact on
team success. After removing some predictors, multicollinearity in the model significantly
reduced.

<img width="494" alt="vif-multicollinearity" src="https://user-images.githubusercontent.com/43526001/235789916-cb733636-41bf-4816-ba4b-7264d701499e.png">



Therefore, moving forward I considered only the predictors above for the final model to
significantly reduce multicollinearity. Then I used the all-possible subsets method for insight on
what predictors should be included in the final model. I found that models with 10 or greater
predictors had very negligible adjusted R-squared values and decided that the model with 10
predictors had the best balance of simplicity and high adjusted R-squared value. For comparison,
I decided to create another model using the stepwise selection method. I found that both methods
were largely agreeing with the same predictors. Both models had no assumptions violations other
than a minor normality violation.


<img width="401" alt="model-summary" src="https://user-images.githubusercontent.com/43526001/235789997-9dd20c3d-d107-49e0-bebd-9cd2085b3a18.png">


Both models performed very similarly with the test dataset. However, Model 1 provides a
very comparable adjusted R-value while also having less a few less predictors. For this reason, I
decided to choose Model 1 as my final model.

# Discussion

From the final model we see that our analysis has concluded that MP, eFG., FT., DRB,
AST, STL, BLK, TOV, PF, PTS, and Tm are a player’s per-game stats that most influence their
BPM. It makes sense to include these predictors as they reflect what actions the player did to
contribute to their team. I find it fascinating how much larger the coefficient of eFG. than every
other predictor. The coefficient of eFG. in our final model using the training dataset tells us on
average a player’s box-plus-minus increases by 23.34 when that player’s effective field goal
percentage increases by 1% and all other predictors are held constant. While this does seem quite
high, it seems reasonable that as a player shoots better on average their team’s points differential
will be higher. This model can be used to evaluate player contribution and understand which
skills in basketball are most important for team success. Furthermore, scouts can use this model
to predict the value a future prospect brings to their team.

## Limitations

A limitation of the model is that basketball is a team sport and therefore much of a
player’s BPM is influenced by their teammates. For example, a poorly skilled player that plays
on a team with very good players will likely have a higher BPM simply because they’re team is
very good and outscores their opponents while that player is playing. To account for this a team
predictor was included in the final model. Another limitation is that many per-game stats are
related to each other. Therefore, some multicollinearity will always exist in the model due to the
nature of the predictors. Additionally, there were a few problematic observations which may
influence the accuracy of the model.

# References

Grassetti, Bellio, R., Di Gaspero, L., Fonseca, G., & Vidoni, P. (2021). An extended regularized
adjusted plus-minus analysis for lineup management in basketball using play-by-play data. IMA
Journal of Management Mathematics, 32(4), 385-409. https://doi.org/10.1093/imaman/dpaa022

Deshpande, & Jensen, S. T. (2016). Estimating an NBA player’s impact on his team’s chances of
winning. Journal of Quantitative Analysis in Sports, 12(2), 51-72. https://doi.org/10.1515/jqas2015-0027

Terner, & Franks, A. (2021). Modeling Player and Team Performance in Basketball. Annual
Review of Statistics and Its Application, 8(1), 1-23. https://doi.org/10.1146/annurev-statistics040720-015536

