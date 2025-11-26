Milestone 8: Linear Regression Model

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
