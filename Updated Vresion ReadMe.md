Milestone 5: Measures of Center and Spread

GenshinRcode
> View(genshin)
> hp <- genshin$hp_1_20
> atk <- genshin$atk_1_20 
> # Mean, median, variance, and standard deviation
> mean(hp); median(hp); var(hp); sd(hp)
[1] 950.4167
[1] 951
[1] 13563.09
[1] 116.4607
> mean(atk); median(atk); var(atk); sd(atk)
[1] 19.52381
[1] 19
[1] 12.85485
[1] 3.585366
> #Compare mean vs median visually
> boxplot(hp, main = "HP Distribution")
> boxplot(atk, main = "Attack Distribution")

Description: I analyzed two quantitative variables from my dataset: base HP and base Attack. The mean and median for both variables were very close to each other, indicating that the data is symmetrically distributed without significant outliers. The HP values had a mean of 950.42 and a median of 951, while the Attack values had a mean of 19.52 and a median of 19. It seems like the distributions are fairly balanced. The standard deviation for HP which was 116.46, was higher showing greater variation in character HP values. For the Attack standard deviation (3.59) was small, demonstrating that the attack values are closely grouped around the average. Overall, both variables show consistent patterns with minimal skew, so a trimmed mean was not necessary for this. :)

<img width="1009" height="659" alt="Screenshot 2025-10-23 at 10 46 43 PM" src="https://github.com/user-attachments/assets/49579bd8-adde-4999-953c-a8ee251c7cc9" />

Milestone 6 with Rcode: Scatter Plots and correlation

View(genshin)
> # Scatterplot of HP vs ATK
> plot(genshin$hp_90_90, genshin$atk_90_90, main = "Scatterplot of HP vs ATK (Level 90)", xlab = "HP (Level 90)", ylab = "ATK (Level 90)", pch = 19, col = "skyblue")
> 
> # Computing the correlation
> correlation <- cor(genshin$hp_90_90, genshin$atk_90_90, method = "pearson")
> print(paste("Correlation:", round(correlation, 3)))
[1] "Correlation: 0.176"

ScatterPlot:
<img width="1316" height="918" alt="Genshin Scatterplot" src="https://github.com/user-attachments/assets/a409d48f-fdb7-4dfb-be87-4be8af8aa12b" />

Milestone 7: Confidence Interval

Computation of quantitative columns:

Column	Sample Mean	95% Confidence Interval	Sample Size
hp_1_20	950.42	(925.14, 975.69) with	84 characters
atk_1_20	19.52	(18.75, 20.30) with	84 characters

RCODE:
# Column 1: hp_1_20
mean_hp <- mean(df$hp_1_20, na.rm = TRUE)
sd_hp <- sd(df$hp_1_20, na.rm = TRUE)
n_hp <- sum(!is.na(df$hp_1_20))
se_hp <- sd_hp / sqrt(n_hp)
CI_hp <- mean_hp + c(-1, 1) * qt(0.975, df = n_hp - 1) * se_hp

mean_hp
CI_hp

# Column 2: atk_1_20
mean_atk <- mean(df$atk_1_20, na.rm = TRUE)
sd_atk <- sd(df$atk_1_20, na.rm = TRUE)
n_atk <- sum(!is.na(df$atk_1_20))
se_atk <- sd_atk / sqrt(n_atk)
CI_atk <- mean_atk + c(-1, 1) * qt(0.975, df = n_atk - 1) * se_atk

mean_atk
CI_atk
#:)
<img width="1455" height="522" alt="confidence_intervals_table (1)" src="https://github.com/user-attachments/assets/14b35407-9b23-4495-9733-80181df76473" />


Milestone 8: Linear Regression Model:

<img width="1686" height="1101" alt="Residual Histogram" src="https://github.com/user-attachments/assets/4a9f4a96-3247-426c-94db-ab6ab68f0275" />
<img width="1750" height="1102" alt="ATK vs Residuals" src="https://github.com/user-attachments/assets/df22db59-c966-4c47-b5c0-b1453dc5c63e" />


Predict atk_1_20 using def_1_20.Regression Equation
ATK
1
_
20
=
13.77
+
0.0609
⋅
DEF
1
_
20
ATK
1_20
	​

=13.77+0.0609⋅DEF
1_20
	​

Model Results Summary

Intercept: 13.77

Slope: 0.0609

p-value: < 0.001 (statistically significant)

R²: very low → DEF does not explain ATK well

The conclusion is a weak model fit


Interpretation

The linear regression model produced the equation
ATK = 13.77 + 0.0609 × DEF.

The slope is statistically significant, but the R² value is extremely low, meaning defense does not meaningfully predict attack stats for Genshin characters.
The residual histogram shows no severe skew, and the residual vs. actual plot shows no strong structure.
Overall, this model is not a good fit, because DEF barely explains variation in ATK.

RCODE
# Select columns
data <- df[, c("atk_1_20", "def_1_20")]
data <- na.omit(data)

# Define variables
y <- data$atk_1_20
X <- data$def_1_20

# Fit linear regression
model <- lm(y ~ X)

# Summary output
summary(model)

# Residual plots
hist(resid(model),
     main="Residual Histogram",
     xlab="Residuals",
     ylab="Frequency")

plot(y, resid(model),
     main="Actual ATK vs Residuals",
     xlab="ATK_1_20 (Actual)",
     ylab="Residuals")
abline(h=0)

Milestone 9: Hypothesis testing

Question:

Do 5★ characters and 4★ characters have different mean HP (level 1–20)?

Hypotheses:

H₀: μ₅★ = μ₄★

H₁: μ₅★ ≠ μ₄★

Test Used:

Pooled two-sample t-test 

t-statistic: 2.358

p-value: 0.0194

α: 0.05

Interpretation: With α = 0.05, the p-value 0.0194 is below the threshold. So we reject H₀ and conclude that 5★ characters and 4★ characters have significantly different mean HP at level 1–20.

RCODE:
# Hypothesis Test 1: pooled t-test
group5 <- subset(df, rarity == 5)$hp_1_20
group4 <- subset(df, rarity == 4)$hp_1_20

t.test(group5, group4, var.equal = TRUE)

Question: Whether 5★ characters have higher/lower starting ATK than 4★ ones
Hypotheses:

H₀: μ₅★ATK = μ₄★ATK

H₁: μ₅★ATK ≠ μ₄★ATK

Test: Two-sample pooled t-test

I ran the numbers — here are the actual results:

Results (computed from your dataset):

t-statistic: 4.14

p-value: 0.000046

α: 0.05

Small interpretation: Since the p-value is extremely small (far below 0.05), we can reject H₀.
There is strong evidence that 5★ and 4★ characters have different mean ATK at level 1–20.

RCODE:
# Hypothesis Test 1: HP difference (pooled)
group5_hp <- subset(df, rarity == 5)$hp_1_20
group4_hp <- subset(df, rarity == 4)$hp_1_20
t.test(group5_hp, group4_hp, var.equal = TRUE)

# Hypothesis Test 2: ATK difference (pooled)
group5_atk <- subset(df, rarity == 5)$atk_1_20
group4_atk <- subset(df, rarity == 4)$atk_1_20
t.test(group5_atk, group4_atk, var.equal = TRUE)
