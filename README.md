STATISTICAL INFERENCE LAB
EXPERIMENT-1
#-------------------- QUESTION 1 --------------------
# 95% Confidence Interval
x <- c(13.0,20.8,25.1,16.0,18.5,19.3,16.8,21.7,16.4,18.8,
       20.4,15.2,14.8,23.1,17.4,21.3,19.4,15.2,25.2,21.5,
       17.3,19.9,23.1,16.8,23.2,19.1,15.3,15.6,24.9,18.1,
       19.4,17.6)
t.test(x, conf.level = 0.95)

#-------------------- QUESTION 2 --------------------
# 90% Confidence Interval
x <- c(481,537,513,583,453,510,570,500,457,555,
       618,327,350,643,499,421,505,637,599,392)
t.test(x, conf.level = 0.90)

#-------------------- QUESTION 3 --------------------
# Two Sample Z-Test
install.packages("BSDA")   # run once
library(BSDA)

zsum.test(mean.x = 937.4, sigma.x = 28, n.x = 56,
          mean.y = 988.9, sigma.y = sqrt(627), n.y = 57,
          mu = 0, conf.level = 0.90)

#-------------------- QUESTION 4 (a) --------------------
# Two sample t-test (equal variance)
x <- c(3.26,2.26,2.62,2.62,2.36,3.00,2.62,2.40,2.30,2.40)
y <- c(1.80,1.46,1.54,1.42,1.32,1.56,1.36,1.64,2.00,1.54)
t.test(x, y, var.equal = TRUE, conf.level = 0.95)


#-------------------- QUESTION 4 (b) --------------------
# Boxplot
boxplot(x, y,
        names = c("With Wedge", "Without Wedge"),
        main = "Boxplot of Seal Removal Force",
        ylab = "Force",
        col = c("lightblue", "lightgreen"))

EXPERIMENT-2

#-------------------- BASIC DATA --------------------
data(cars)
summary(cars)
head(cars)

#-------------------- BARPLOT --------------------
x <- c("A","B","C","D","E","F")
y <- c(2,4,6,8,10,3)

barplot(y, names.arg = x, col = "blue")

#-------------------- BARPLOT (mtcars) --------------------
barplot(table(mtcars$cyl),
        main = "No. of Cylinders",
        col = "green")

#-------------------- BOXPLOT --------------------
boxplot(mtcars$mpg, col = "yellow")


#-------------------- GROUPED BOXPLOT --------------------
data(mtcars)
boxplot(mpg ~ gear, data = mtcars,
        col = c("lightgreen","green","darkgreen"),
        xlab = "Number of Gears",
        ylab = "Miles per Gallon")

#-------------------- HISTOGRAM --------------------
hist(mtcars$mpg, col = "yellow")

hist(mtcars$mpg,
     col = "green",
     breaks = 15,
     border = "blue")

#-------------------- DENSITY PLOT --------------------
den <- density(mtcars$mpg)
plot(den, type = "p", col = "red")

#-------------------- PIE CHART --------------------
pie(table(mtcars$gear),
    main = "Gears",
    radius = 1,
    col.main = "darkgreen")

#-------------------- SCATTER PLOT --------------------
plot(x = cars$speed,
     y = cars$dist,
     main = "Speed vs Stopping Distance",
     xlab = "Speed (mph)",
     ylab = "Stopping Distance (ft)",
     pch = 23,
     col = "blue")
#-------------------- IRIS DATA --------------------
summary(iris)
head(iris)

#-------------------- HISTOGRAM (IRIS) --------------------
hist(iris$Sepal.Length, col = "yellow")
hist(iris$Sepal.Width, col = "pink")
hist(iris$Petal.Length, col = "red")
hist(iris$Petal.Width, col = "blue")

#-------------------- BARPLOT (IRIS) --------------------
barplot(table(iris$Sepal.Length),
        main = "No of Sepal Length",
        col = "pink")

#-------------------- DENSITY (IRIS) --------------------
den <- density(iris$Sepal.Length)
plot(den, type = "s", col = "red")

#-------------------- BOXPLOTS (IRIS) --------------------
boxplot(iris$Sepal.Length, col = "yellow")
boxplot(iris$Sepal.Width, col = "pink")
boxplot(iris$Petal.Length, col = "red")
boxplot(iris$Petal.Width, col = "green")

boxplot(Sepal.Length ~ Petal.Length, data = iris,
        col = c("blue","green","red"),
        xlab = "Sepal Length",
        ylab = "Petal Length")


#-------------------- PIE (IRIS) --------------------
pie(table(iris$Species),
    main = "Iris Species",
    radius = 1,
    col.main = "pink")

#-------------------- SCATTER (IRIS) --------------------
plot(iris$Sepal.Length, iris$Sepal.Width,
     col = iris$Species,
     pch = 21,
     xlab = "Sepal Length",
     ylab = "Sepal Width",
     main = "Scatterplot of Sepal Dimensions")

plot(iris$Sepal.Length, iris$Petal.Length,
     main = "Petal Length vs Sepal Length",
     xlab = "Sepal Length",
     ylab = "Petal Length",
     pch = 15,
     col = "red")

EXPERIMENT-3
#-------------------- 1. SINGLE MEAN Z TEST --------------------
xbar <- 85
sigma <- 140
n <- 18
mu0 <- 20
alpha <- 0.05

z <- (xbar - mu0) / (sigma / sqrt(n))
z

z_crit <- qnorm(1 - alpha)

if (z > z_crit) {
  cat("Reject Null Hypothesis H0\n")
} else {
  cat("Accept Null Hypothesis H0\n")
}

# Plot
zvals <- seq(-4, 4, 0.01)
dens <- dnorm(zvals)

plot(zvals, dens, type = "l",
     main = "Single Mean Z Test",
     xlab = "Z value", ylab = "Density")

z_region <- seq(z_crit, 4, 0.01)
polygon(c(z_region, rev(z_region)),
        c(dnorm(z_region), rep(0, length(z_region))),
        col = "lightcoral", border = NA)

abline(v = z_crit, col = "red", lwd = 2)
abline(v = z, col = "blue", lwd = 2, lty = 2)


#-------------------- 2. TWO MEANS Z TEST --------------------
xbar1 <- 86; xbar2 <- 58
s1 <- 10; s2 <- 15
n1 <- 75; n2 <- 80
alpha <- 0.05

z <- (xbar1 - xbar2) / sqrt((s1^2/n1) + (s2^2/n2))
z

z_crit <- qnorm(1 - alpha/2)

if (abs(z) > z_crit) {
  cat("Reject Null Hypothesis H0\n")
} else {
  cat("Accept Null Hypothesis H0\n")
}

# Plot
zvals <- seq(-5, 5, 0.01)
plot(zvals, dnorm(zvals), type = "l",
     main = "Two Means Z Test",
     xlab = "Z value", ylab = "Density")

abline(v = c(-z_crit, z_crit), col = "red", lwd = 2)
abline(v = z, col = "blue", lwd = 2, lty = 2)


#-------------------- 3. SINGLE PROPORTION Z TEST --------------------
x <- 150
n <- 300
p0 <- 0.25
alpha <- 0.05

phat <- x / n
z <- (phat - p0) / sqrt((p0*(1-p0))/n)

z_crit <- qnorm(1 - alpha/2)

if (abs(z) > z_crit) {
  cat("Reject Null Hypothesis H0\n")
} else {
  cat("Accept Null Hypothesis H0\n")
}

# Plot
zvals <- seq(-4, 4, 0.01)
plot(zvals, dnorm(zvals), type = "l",
     main = "Single Proportion Z Test",
     xlab = "Z value", ylab = "Density")

abline(v = c(-z_crit, z_crit), col = "red", lwd = 2)
abline(v = z, col = "blue", lwd = 2, lty = 2)


#-------------------- 4. TWO PROPORTION Z TEST --------------------
x1 <- 75; n1 <- 400
x2 <- 60; n2 <- 280
alpha <- 0.05

p1 <- x1/n1
p2 <- x2/n2

p_pool <- (x1 + x2) / (n1 + n2)

z <- (p1 - p2) / sqrt(p_pool*(1-p_pool)*(1/n1 + 1/n2))

z_crit <- qnorm(1 - alpha/2)

if (abs(z) > z_crit) {
  cat("Reject Null Hypothesis H0\n")
} else {
  cat("Accept Null Hypothesis H0\n")
}

# Plot
zvals <- seq(-4, 4, 0.01)
plot(zvals, dnorm(zvals), type = "l",
     main = "Two Proportion Z Test",
     xlab = "Z value", ylab = "Density")

abline(v = c(-z_crit, z_crit), col = "red", lwd = 2)
abline(v = z, col = "blue", lwd = 2, lty = 2)
#############################################
# 🔹 LAB 4: F-TEST (ONE-TAILED & TWO-TAILED)
#############################################

# -------- ONE-TAILED F TEST --------
s1_sq <- 40; s2_sq <- 10     # sample variances
n1 <- 14; n2 <- 12           # sample sizes
alpha <- 0.05

F_cal <- s1_sq / s2_sq       # F statistic
df1 <- n1 - 1; df2 <- n2 - 1

F_critical <- qf(1 - alpha, df1, df2)  # critical value

# decision
if(F_cal > F_critical){
  print("Reject H0")
} else {
  print("Accept H0")
}

# -------- TWO-TAILED F TEST --------
s1_sq <- 30; s2_sq <- 27
n1 <- 10; n2 <- 12
alpha <- 0.05

F_cal <- s1_sq / s2_sq
df1 <- n1 - 1; df2 <- n2 - 1

F_upper <- qf(1 - alpha/2, df1, df2)
F_lower <- 1 / qf(1 - alpha/2, df2, df1)

# decision
if(F_cal < F_lower || F_cal > F_upper){
  print("Reject H0")
} else {
  print("Accept H0")
}
 

# 🔹 LAB 5: CHI-SQUARE (GOODNESS OF FIT)

# -------- ONE-TAILED --------
obs <- c(18,24,20,25,15)     # observed
exp <- c(20,20,20,20,20)     # expected
n <- length(obs)
alpha <- 0.05

chi_cal <- sum((obs - exp)^2 / exp)   # chi-square statistic
chi_critical <- qchisq(1 - alpha, n - 1)

# decision
if(chi_cal > chi_critical){
  print("Reject H0")
} else {
  print("Accept H0")
}

# -------- TWO-TAILED --------
obs <- c(20,30,25,25)
exp <- c(25,25,25,25)
n <- length(obs)

chi_cal <- sum((obs - exp)^2 / exp)

chi_upper <- qchisq(1 - alpha/2, n - 1)
chi_lower <- qchisq(alpha/2, n - 1)

# decision
if(chi_cal < chi_lower || chi_cal > chi_upper){
  print("Reject H0")
} else {
  print("Accept H0")
}


#############################################
# 🔹 LAB 6: CHI-SQUARE (INDEPENDENCE TEST)
#############################################

# -------- DATASET 1 --------
data1 <- matrix(c(40,75,50,80), nrow=2, byrow=TRUE)

# chi-square test
test1 <- chisq.test(data1, correct=FALSE)

# critical value method
df <- test1$parameter
critical <- qchisq(0.95, df)

if(test1$statistic > critical){
  print("Reject H0 (Dependent)")
} else {
  print("Accept H0 (Independent)")
}

# plots
barplot(data1, beside=TRUE, col=c("yellow","green"),
        legend=TRUE, main="Bar Plot")

mosaicplot(data1, color=TRUE, main="Mosaic Plot")

assocplot(data1, main="Association Plot")

# -------- DATASET 2 --------
data2 <- matrix(c(40,70,30,80), nrow=2, byrow=TRUE)

test2 <- chisq.test(data2, correct=FALSE)

df <- test2$parameter
critical <- qchisq(0.95, df)

if(test2$statistic > critical){
  print("Reject H0 (Dependent)")
} else {
  print("Accept H0 (Independent)")
}

# plots
barplot(data2, beside=TRUE, col=c("yellow","green"),
        legend=TRUE, main="Bar Plot")

mosaicplot(data2, color=TRUE, main="Mosaic Plot")

assocplot(data2, main="Association Plot")
##################################

EXP 7: SIGN TEST

Aim:
To test whether the population median differs from a hypothesized value.

Steps:
1. Define data and hypothesized median (M0)
2. Compute signs of (data - M0)
3. Remove zero values (ties)
4. Count positive signs (S)
5. Compute p-value using binomial distribution
6. Compare with alpha (0.05)

R Code:
data = c(...)
M0 = ...
alpha = 0.05

signs = sign(data - M0)
signs = signs[signs != 0]

S = sum(signs > 0)
n = length(signs)

p_val = 2 * pbinom(min(S, n-S), n, 0.5)

if(p_val < alpha){
  print("REJECT H0")
}else{
  print("FAIL TO REJECT H0")
}

Conclusion:
If p-value < 0.05 → Reject H0
Else → Fail to reject H0


WILCOXON SIGNED RANK TEST

Aim:
To compare two related samples using ranks.

R Code:
before = c(...)
after = c(...)

test = wilcox.test(after, before, paired=TRUE, exact=TRUE)
print(test)

if(test$p.value < 0.05){
  print("REJECT H0")
}else{
  print("FAIL TO REJECT H0")
}

Conclusion:
Checks if median difference between pairs is significant.


-----------------------------------------------------

EXP 8: MEDIAN TEST

Aim:
To compare medians of multiple groups.

Steps:
1. Combine all data
2. Compute overall median
3. Convert values into above/below median
4. Create contingency table
5. Apply Fisher’s Exact Test

R Code:
A = c(...)
B = c(...)
C = c(...)
D = c(...)

data = c(A,B,C,D)
group = factor(c(rep('A',10), rep('B',10), rep('C',10), rep('D',10)))

median_val = median(data)

tbl = table(group, data > median_val)

fisher.test(tbl)

Conclusion:
If p-value < 0.05 → Significant difference exists


-----------------------------------------------------
EXP 9: KOLMOGOROV-SMIRNOV TEST

ONE SAMPLE K-S TEST

Aim:
To compare sample with a theoretical distribution.

R Code:
x = c(...)

ks.test(x, 'pnorm', mean=..., sd=...)

Conclusion:
If p-value < 0.05 → Data does not follow distribution


TWO SAMPLE K-S TEST

Aim:
To compare two datasets.

R Code:
A = c(...)
B = c(...)

ks.test(A, B)

Conclusion:
If p-value < 0.05 → Distributions are different


-----------------------------------------------------

IMPORTANT VISUALIZATIONS (WRITE ANY 2-3)

Sign Test:
barplot(data)
plot(data)
hist(data)

Wilcoxon:
boxplot(before, after)
plot(before); lines(after)
barplot(after-before)

Median Test:
boxplot(A,B,C,D)
plot(data)
hist(A)

K-S Test:
plot(ecdf(x))
hist(x)
plot difference curves





