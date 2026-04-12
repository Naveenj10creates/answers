# answers

#---------------------------------------------
# EXPERIMENT 1: CORRELATION ANALYSIS
#---------------------------------------------

#Aim:Correlation Analysis using scatter diagram,Karl Pearson’s correlation coefficient and drawing inferences

# Load libraries
library(tidyverse)
library(corrplot)
library(PerformanceAnalytics)
library(ggcorrplot)

#---------------------------------------------
# 1) SCATTER PLOT (IRIS DATA)
#---------------------------------------------
data(iris)

ggplot(iris, aes(x = Sepal.Width, y = Sepal.Length)) +
  facet_wrap(~Species, scales = "free_x") +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  labs(
    x = "Sepal Width",
    y = "Sepal Length",
    title = "Sepal Length vs Sepal Width in Irises",
    subtitle = "Grouped by Species"
  )

#---------------------------------------------
# 2) KARL PEARSON CORRELATION TEST
#---------------------------------------------
data(mtcars)

cor_1 = cor.test(mtcars$wt, mtcars$mpg, method = "pearson")
cor_1

#---------------------------------------------
# 3) CORRELATION MATRIX
#---------------------------------------------
my_data = mtcars[, c(1, 3, 4, 5)]
head(my_data, 5)

cor_2 = round(cor(my_data, method = "pearson"), 4)
cor_2

#---------------------------------------------
# 4) VISUALIZATION USING CORRPLOT
#---------------------------------------------
corrplot(cor_2, type = "upper", order = "hclust",
         tl.col = "blue", tl.srt = 45)

#---------------------------------------------
# 5) CHART CORRELATION
#---------------------------------------------
chart.Correlation(my_data, histogram = TRUE, method = "pearson")

#---------------------------------------------
# 6) SYMBOLIC REPRESENTATION
#---------------------------------------------
cor_5 = cor(my_data)
cor_5
symnum(cor_5, abbr.colnames = FALSE)

#---------------------------------------------
# 7) GGCORRPLOT VISUALIZATION
#---------------------------------------------
cor_6 = round(cor(my_data), 4)
ggcorrplot(cor_6)
 
#-----------------------------------------------------------------------------------------------------

#---------------------------------------------------
# EXPERIMENT 2: SIMPLE LINEAR REGRESSION
#---------------------------------------------------

#Aim: Simple linear regression: model fitting, estimation of parameters, computing R and adjusted R and model interpretation

# Load dataset
data(cars)

# Summary of dataset
summary(cars)

# Extract variables
speed = cars$speed
dist = cars$dist

# Basic statistics
mean(speed)
mean(dist)
sd(speed)
sd(dist)

#---------------------------------------------------
# SCATTER PLOT
#---------------------------------------------------
plot(speed, dist,
     ylab = "Car Stopping Distance (ft)",
     xlab = "Speed (mph)",
     main = "Car Speed vs Stopping Distance")

#---------------------------------------------------
# MODEL FITTING
#---------------------------------------------------
SLR = lm(dist ~ speed)

# Add regression line
abline(SLR, col = "red")

#---------------------------------------------------
# RESIDUAL PLOT (Corrected)
#---------------------------------------------------
plot(SLR$fitted.values, SLR$residuals,
     xlab="Fitted Values", ylab="Residuals",
     main="Residual Plot")
abline(h = 0, col = "red")

#---------------------------------------------------
# MODEL SUMMARY
#---------------------------------------------------
summary(SLR)

#---------------------------------------------------
# ANOVA TABLE
#---------------------------------------------------
anova(SLR)

#---------------------------------------------------
# F-TEST (Corrected)
#---------------------------------------------------
F_value = summary(SLR)$fstatistic[1]
df1 = summary(SLR)$fstatistic[2]
df2 = summary(SLR)$fstatistic[3]

Ft = qf(0.95, df1, df2)
Ft

pv = 1 - pf(F_value, df1, df2)
pv

#---------------------------------------------------
# CONFIDENCE INTERVAL
#---------------------------------------------------
confint(SLR, level = 0.95)

#---------------------------------------------------
# PREDICTION
#---------------------------------------------------
newdata = data.frame(speed = 15)

# Confidence interval (mean response)
predict(SLR, newdata, interval = "confidence")

# Prediction interval (individual response)
predict(SLR, newdata, interval = "predict")

#--------------------------------------------------------------------------------------------------------

#---------------------------------------------------
# EXPERIMENT 3: RESIDUAL ANALYSIS & FORECASTING
#---------------------------------------------------

#Aim: Residual analysis and forecast accuracy for a given data set.

# Load libraries
library(UsingR)
library(broom)
library(ggplot2)
library(forecast)

#---------------------------------------------------
# 1) DIAMOND DATASET (RESIDUAL ANALYSIS)
#---------------------------------------------------
data("diamond")
mydata = diamond

# Scatter plot
plot(mydata$carat, mydata$price,
     ylab = "Price", xlab = "Carat")

# Linear model
fit = lm(price ~ carat, data = mydata)

# Regression line
abline(fit, col = "red")

# Model diagnostics using broom
model.diag.metrics = augment(fit)
head(model.diag.metrics)

# Residual visualization
ggplot(model.diag.metrics, aes(x = carat, y = price)) +
  geom_point() +
  stat_smooth(method = "lm", se = FALSE) +
  geom_segment(aes(xend = carat, yend = .fitted),
               color = "red", linewidth = 0.5)

# Diagnostic plots
plot(fit, 1)  # Residuals vs Fitted
plot(fit, 2)  # Normal Q-Q
plot(fit, 3)  # Scale-Location
plot(fit, 4)  # Cook's distance

#---------------------------------------------------
# 2) FORECASTING (AIRPASSENGERS DATA)
#---------------------------------------------------

data("AirPassengers")
series = AirPassengers

# Time series plot
plot(series, col = "darkblue",
     ylab = "Passengers", xlab = "Year")

# Seasonal boxplot
boxplot(split(series, cycle(series)),
        names = month.abb, col = "gold")

# Train-test split
train_set = window(series, start = 1949, end = c(1956, 12))
test_set = window(series, start = 1957, end = c(1960, 12))

# Plot training data
plot(train_set, main = "AirPassengers", ylab = "", xlab = "Month")

# Forecasting models
lines(meanf(train_set, h = 48)$mean, col = 4)
lines(rwf(train_set, h = 48)$mean, col = 2)
lines(rwf(train_set, drift = TRUE, h = 48)$mean, col = 3)
lines(snaive(train_set, h = 48)$mean, col = 5)

# Legend
legend("topleft", lty = 1,
       col = c(4, 2, 3, 5),
       legend = c("Mean", "Naive", "Drift", "Seasonal Naive"),
       bty = "n")

# Actual test data
lines(test_set, col = "red")

# Accuracy comparison
accuracy(meanf(train_set, h = 48), test_set)
accuracy(rwf(train_set, h = 48), test_set)
accuracy(rwf(train_set, drift = TRUE, h = 48), test_set)
accuracy(snaive(train_set, h = 48), test_set)

# Final comparison plot
plot(test_set, main = "Forecast vs Actual",
     ylab = "", xlab = "Month",
     ylim = c(200, 600))

lines(meanf(train_set, h = 48)$mean, col = 4)
lines(rwf(train_set, h = 48)$mean, col = 2)
lines(rwf(train_set, drift = TRUE, h = 48)$mean, col = 3)
lines(snaive(train_set, h = 48)$mean, col = 5)

legend("topleft", lty = 1,
       col = c(4, 2, 3, 5),
       legend = c("Mean", "Naive", "Drift", "Seasonal Naive"),
       bty = "n")

#------------------------------------------------------------------------------------------------------
#---------------------------------------------------
# EXPERIMENT 4: VALIDATION OF SLR (t, F, p tests)
#---------------------------------------------------

# Load dataset
data(cars)

# Boxplots for outliers
par(mfrow = c(1, 2))
boxplot(cars$speed, main = "Speed",
        sub = paste("Outliers:", boxplot.stats(cars$speed)$out))
boxplot(cars$dist, main = "Distance",
        sub = paste("Outliers:", boxplot.stats(cars$dist)$out))

# Correlation
cor(cars$speed, cars$dist)

# Scatter plot
scatter.smooth(x = cars$speed, y = cars$dist,
               main = "Distance vs Speed")

# Linear model
linear_model = lm(dist ~ speed, data = cars)
summary(linear_model)

#---------------------------------------------------
# t-TEST
#---------------------------------------------------

modelsummary = summary(linear_model)
modelcoeffs = modelsummary$coefficients

n = nrow(cars)
alpha = 0.05

beta_estimate = modelcoeffs["speed", "Estimate"]
std_error = modelcoeffs["speed", "Std. Error"]

t_value = beta_estimate / std_error

t_table = qt(1 - alpha/2, df = n - 2)

t_value
t_table

if (abs(t_value) > t_table) {
  print("Reject H0: Predictor coefficient is zero")
} else {
  print("Fail to reject H0")
}

# p-value
p_value = 2 * pt(-abs(t_value), df = n - 2)
p_value

if (p_value < alpha) {
  print("Reject H0 using p-value")
} else {
  print("Fail to reject H0")
}

#---------------------------------------------------
# F-TEST
#---------------------------------------------------

f_stat = summary(linear_model)$fstatistic

f_value = f_stat[1]
df1 = f_stat[2]
df2 = f_stat[3]

f_table = qf(0.95, df1, df2)

f_value
f_table

if (f_value > f_table) {
  print("Reject H0: Model is not significant")
} else {
  print("Fail to reject H0")
}

#---------------------------------------------------
# R-SQUARED
#---------------------------------------------------

summary(linear_model)$r.squared
summary(linear_model)$adj.r.squared

#---------------------------------------------------
# CONFIDENCE INTERVAL
#---------------------------------------------------

confint(linear_model)

#--------------------------------------------------------------------------------------

#---------------------------------------------------
# EXPERIMENT 5
# Confidence Interval & Testing (SLR + MLR)
#---------------------------------------------------

#========================
# PART A: SIMPLE LINEAR REGRESSION (cars)
#========================

data(cars)

# Model
lin_Mod = lm(dist ~ speed, data = cars)
lin_Mod

# New data
new_speeds = data.frame(speed = c(13, 20, 25, 10))

# Predictions
predict(lin_Mod, newdata = new_speeds)

# Confidence Interval
predict(lin_Mod, newdata = new_speeds, interval = "confidence")

# Prediction Interval
predict(lin_Mod, newdata = new_speeds, interval = "prediction")

# Visualization
pred_int = predict(lin_Mod, interval = "prediction")
mydata = cbind(cars, pred_int)

library(ggplot2)

ggplot(mydata, aes(x = speed, y = dist)) +
  geom_point() +
  stat_smooth(method = "lm") +
  geom_line(aes(y = lwr), color = "red", linetype = "dashed") +
  geom_line(aes(y = upr), color = "red", linetype = "dashed")


#========================
# PART B: MULTIPLE LINEAR REGRESSION (stackloss)
#========================

data(stackloss)

# Model
stackloss_lm = lm(stack.loss ~ Air.Flow + Water.Temp + Acid.Conc.,
                  data = stackloss)

stackloss_lm

# New data
newdata = data.frame(
  Air.Flow = c(72, 82, 69, 77),
  Water.Temp = c(20, 25, 18, 22),
  Acid.Conc. = c(85, 90, 88, 92)
)

# Confidence Interval
predict(stackloss_lm, newdata, interval = "confidence")

# Prediction Interval
predict(stackloss_lm, newdata, interval = "prediction")


#========================
# PART C: ASSESSMENT (marketing dataset)
#========================

# Install once if not installed
# install.packages("datarium")

library(datarium)
data("marketing")

#------------------------
# Simple Regression
#------------------------
model1 = lm(sales ~ youtube, data = marketing)
summary(model1)

newdata1 = data.frame(youtube = c(50, 100, 150))

# Confidence Interval
predict(model1, newdata1, interval = "confidence")

# Prediction Interval
predict(model1, newdata1, interval = "prediction")

# Visualization
pred = predict(model1, interval = "prediction")
data_plot = cbind(marketing, pred)

ggplot(data_plot, aes(x = youtube, y = sales)) +
  geom_point() +
  stat_smooth(method = "lm") +
  geom_line(aes(y = lwr), color = "red", linetype = "dashed") +
  geom_line(aes(y = upr), color = "red", linetype = "dashed")


#------------------------
# Multiple Regression
#------------------------
model2 = lm(sales ~ youtube + facebook, data = marketing)
summary(model2)

newdata2 = data.frame(
  youtube = c(50, 120, 200),
  facebook = c(30, 60, 90)
)

# Confidence Interval
predict(model2, newdata2, interval = "confidence")

# Prediction Interval
predict(model2, newdata2, interval = "prediction")

#--------------------------------------------------------------------------------------------

#---------------------------------------------------
# EXPERIMENT 6
# MULTIPLE REGRESSION ANALYSIS
#---------------------------------------------------

# Load dataset
data(mtcars)

head(mtcars)
names(mtcars)

#---------------------------------------------------
# FULL MODEL
#---------------------------------------------------
model = lm(mpg ~ cyl + disp + hp + drat + wt + qsec + vs + am + gear + carb,
           data = mtcars)

summary(model)

#---------------------------------------------------
# t-TEST FOR COEFFICIENT (Example: 3rd variable)
#---------------------------------------------------
teststat = coef(summary(model))[3,1] / coef(summary(model))[3,2]
teststat

# p-value (correct)
2 * pt(-abs(teststat), df = nrow(mtcars) - length(coef(model)))

#---------------------------------------------------
# TEST FOR INTERCEPT
#---------------------------------------------------
teststat1 = coef(summary(model))[1,1] / coef(summary(model))[1,2]
teststat1

2 * pt(-abs(teststat1), df = nrow(mtcars) - length(coef(model))
       
)

#---------------------------------------------------
# RESIDUAL ANALYSIS
#---------------------------------------------------

residuals = model$residuals

# Histogram
hist(residuals, main = "Histogram of Residuals")

# Normal Q-Q plot
qqnorm(residuals)
qqline(residuals, col = "red")

# Residuals vs Fitted (corrected)
plot(model$fitted.values, residuals,
     xlab = "Fitted Values", ylab = "Residuals",
     main = "Residuals vs Fitted")
abline(h = 0, col = "red")

# Diagnostic plots
plot(model)

#---------------------------------------------------
# MODEL TRANSFORMATION
#---------------------------------------------------
model1 = lm(mpg ~ cyl + log(disp) + log(hp) + drat + wt + qsec + vs + am + gear + carb,
            data = mtcars)

summary(model1)

# Compare models
AIC(model)
AIC(model1)

#---------------------------------------------------
# VARIABLE SELECTION (STEPWISE)
#---------------------------------------------------
library(MASS)

stepAIC(model)
stepAIC(model1)

#---------------------------------------------------
# REDUCED MODELS
#---------------------------------------------------
model2 = lm(mpg ~ qsec + wt + am, data = mtcars)
summary(model2)

model3 = lm(mpg ~ log(disp) + gear + carb, data = mtcars)
summary(model3)

model4 = lm(mpg ~ qsec + wt * am, data = mtcars)
summary(model4)

AIC(model4)

#---------------------------------------------------
# PREDICTION
#---------------------------------------------------
new_values = data.frame(wt = 2.92, qsec = 20.1, am = 1)

# Prediction interval
pred_y = predict(model2, new_values, interval = "predict")
pred_y

# Confidence interval
conf_y = predict(model2, new_values, interval = "confidence")
conf_y

#-----------------------------------------------------------------------------------------------------

#---------------------------------------------------
# EXPERIMENT 7
# MULTICOLLINEARITY & VIF
#---------------------------------------------------

# Load libraries
library(tidyverse)
library(corrplot)
library(olsrr)

#---------------------------------------------------
# DATA PREPARATION
#---------------------------------------------------
data(mtcars)

mydata = mtcars %>% select(mpg, cyl, disp, hp, wt)
head(mydata)

#---------------------------------------------------
# MODEL
#---------------------------------------------------
model = lm(mpg ~ ., data = mydata)
summary(model)

#---------------------------------------------------
# CORRELATION MATRIX
#---------------------------------------------------
cor_matrix = cor(mydata)
cor_matrix

corrplot(cor_matrix, method = "number")

#---------------------------------------------------
# VARIANCE INFLATION FACTOR (VIF)
#---------------------------------------------------
ols_vif_tol(model)

#---------------------------------------------------
# CONDITION INDEX (EIGEN VALUES)
#---------------------------------------------------
ols_eigen_cindex(model)

#--------------------------------------------------------------------------------------

#---------------------------------------------------
# EXPERIMENT 8
# AUTOCORRELATION & VARIABLE SELECTION METHODS
#---------------------------------------------------

# Load dataset
data(mtcars)
head(mtcars)

# Install only once (commented)
# install.packages("lmtest")
# install.packages("olsrr")

# Load libraries
library(lmtest)
library(olsrr)

#---------------------------------------------------
# DURBIN-WATSON TEST (AUTOCORRELATION)
#---------------------------------------------------
model1 = lm(mpg ~ disp + wt, data = mtcars)
summary(model1)

dwtest(model1)

#---------------------------------------------------
# FULL MODEL FOR VARIABLE SELECTION
#---------------------------------------------------
model = lm(mpg ~ disp + hp + wt + qsec, data = mtcars)
summary(model)

#---------------------------------------------------
# ALL POSSIBLE REGRESSIONS
#---------------------------------------------------
k1 = ols_step_all_possible(model)
k1
plot(k1)

#---------------------------------------------------
# BEST SUBSET SELECTION
#---------------------------------------------------
k2 = ols_step_best_subset(model)
k2

#---------------------------------------------------
# FORWARD SELECTION (p-value)
#---------------------------------------------------
k3 = ols_step_forward_p(model, details = TRUE)
k3
plot(k3)

#---------------------------------------------------
# BACKWARD ELIMINATION (p-value)
#---------------------------------------------------
k4 = ols_step_backward_p(model, details = TRUE)
k4
plot(k4)

#---------------------------------------------------
# STEPWISE SELECTION (p-value)
#---------------------------------------------------
k5 = ols_step_both_p(model, details = TRUE)
k5
plot(k5)

#---------------------------------------------------
# FORWARD SELECTION (AIC)
#---------------------------------------------------
k6 = ols_step_forward_aic(model, details = TRUE)
k6
plot(k6)

#---------------------------------------------------
# BACKWARD ELIMINATION (AIC)
#---------------------------------------------------
k7 = ols_step_backward_aic(model, details = TRUE)
k7
plot(k7)

#---------------------------------------------------
# STEPWISE SELECTION (AIC)
#---------------------------------------------------
k8 = ols_step_both_aic(model, details = TRUE)
k8
plot(k8)

#-----------------------------------------------------------------------------------------------------
#
#---------------------------------------------------
# EXPERIMENT 9
# AUTOCORRELATION & AUTO REGRESSIVE MODEL
#---------------------------------------------------

#-----------------------------------
# 1) BASIC AUTOCORRELATION
#-----------------------------------
x = c(22,24,25,25,28,29,34,37,40,44,51,48,47,50,51)
x

library(tseries)

# ACF without plot
acf(x, plot = FALSE)

# ACF with lag limit
acf(x, lag.max = 5, plot = FALSE)

# ACF plot
acf(x, main = "Autocorrelation by lag")

#-----------------------------------
# 2) AIRPASSENGERS TIME SERIES
#-----------------------------------
data("AirPassengers")

is.ts(AirPassengers)

print(AirPassengers)

start(AirPassengers)
end(AirPassengers)
frequency(AirPassengers)

# Time series plot
ts.plot(AirPassengers,
        xlab = "Year",
        ylab = "No of passengers",
        main = "Monthly AirPassengers Data")

# Add trend line
abline(lm(AirPassengers ~ time(AirPassengers)))

# ACF
acf(AirPassengers)

#-----------------------------------
# 3) AUTO REGRESSIVE MODEL (AR)
#-----------------------------------
AR = arima(AirPassengers, order = c(1,0,0))
AR

# Fitted values
AR_fit = AirPassengers - residuals(AR)

# Plot
ts.plot(AirPassengers, main = "AR Model Fit")
lines(AR_fit, col = "red", lty = 2)

# Prediction
predict_AR = predict(AR)
predict_AR$pred[1]

# Forecast next 10 values
AR_forecast = predict(AR, n.ahead = 10)$pred
AR_forecast_se = predict(AR, n.ahead = 10)$se

# Plot forecast
ts.plot(AirPassengers, xlim = c(1949, 1961),
        main = "AR Forecast")

lines(AR_forecast, col = "red")
lines(AR_forecast - 2 * AR_forecast_se, col = "red", lty = 2)
lines(AR_forecast + 2 * AR_forecast_se, col = "red", lty = 2)

#-----------------------------------
# 4) MOVING AVERAGE MODEL (MA)
#-----------------------------------
MA = arima(AirPassengers, order = c(0,0,1))
MA

# Fitted values
MA_fit = AirPassengers - residuals(MA)

# Plot
ts.plot(AirPassengers, main = "MA Model Fit")
lines(MA_fit, col = "blue", lty = 2)

# Prediction
predict_MA = predict(MA)
predict_MA$pred[1]

# Forecast
MA_forecasts = predict(MA, n.ahead = 10)$pred
MA_forecasts_se = predict(MA, n.ahead = 10)$se

# Plot forecast
ts.plot(AirPassengers, xlim = c(1949, 1961),
        main = "MA Forecast")

lines(MA_forecasts, col = "blue")
lines(MA_forecasts - 2 * MA_forecasts_se, col = "blue", lty = 2)
lines(MA_forecasts + 2 * MA_forecasts_se, col = "blue", lty = 2)

#-----------------------------------
# 5) MODEL COMPARISON
#-----------------------------------
cor(AR_fit, MA_fit)

AIC(AR)
AIC(MA)

BIC(AR)
BIC(MA)

#-------------------------------------------------------------------------------------------------
#
#---------------------------------------------------
# EXPERIMENT 10
# NON-LINEAR REGRESSION MODELS
#---------------------------------------------------

#-----------------------------------
# 1) POLYNOMIAL REGRESSION
#-----------------------------------

HWC = c(0,1,2,3,4,5,6,5,7,8,9,10,11,12,13,14,15)
TS  = c(15,20,24,26,30,34,38,40,42,47,53,52,53,49,43,27,22)

data_poly = data.frame(HWC, TS)
data_poly

# Scatter plot
plot(data_poly$HWC, data_poly$TS,
     xlab = "Hardwood Content",
     ylab = "Tensile Strength",
     col = "red", pch = 1, cex = 2)

# Quadratic model (method 1)
model1 = lm(TS ~ HWC + I(HWC^2), data = data_poly)
summary(model1)

# Prediction curve
HWC_VAL = seq(0, 20, 0.01)
TS_VAL1 = predict(model1, data.frame(HWC = HWC_VAL))
lines(HWC_VAL, TS_VAL1, col = "blue", lwd = 2)

# Quadratic model (method 2)
model2 = lm(TS ~ poly(HWC, 2, raw = TRUE), data = data_poly)
summary(model2)

TS_VAL2 = predict(model2, data.frame(HWC = HWC_VAL))
lines(HWC_VAL, TS_VAL2, col = "green", lwd = 2)


#-----------------------------------
# 2) NON-LINEAR MODEL (Puromycin)
#-----------------------------------

data("Puromycin")
dt1 = Puromycin

plot(dt1$conc, dt1$rate,
     xlab = "Concentration",
     ylab = "Rate")

# Michaelis-Menten model
model_mm = nls(rate ~ (a * conc) / (b + conc),
               data = dt1,
               start = list(a = 200, b = 0.1))

summary(model_mm)

# Curve
conc_val = seq(min(dt1$conc), max(dt1$conc), 0.001)
rate_val = predict(model_mm, data.frame(conc = conc_val))

lines(conc_val, rate_val, col = "blue", lwd = 2)


#-----------------------------------
# 3) NON-LINEAR GROWTH MODEL
#-----------------------------------

# install.packages("minpack.lm")  # run once
library(minpack.lm)

week = c(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16)
mass = c(4,8,12,13.5,15,18,19.5,19.8,21.5,22,23.5,24,24.3,24.8,25,25.5)

dt2 = data.frame(week, mass)

plot(dt2$week, dt2$mass,
     xlab = "Weeks",
     ylab = "Mass",
     col = "red", pch = 1, cex = 2)

# Exponential growth model
model_growth = nlsLM(mass ~ a * (1 - exp(-b * week)),
                     data = dt2,
                     start = list(a = max(dt2$mass), b = 0.2))

summary(model_growth)

# Curve
week_val = seq(1, 16, 0.1)
mass_val = predict(model_growth, data.frame(week = week_val))

lines(week_val, mass_val, col = "blue", lwd = 2)
