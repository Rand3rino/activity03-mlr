Day 2 - Assessing a MLR Model
================

In this repository/directory you should see two items:

- `README.md` - this document.

You will continue working in your `day01-fitting/activity03.Rmd` file
that you started on [Day 1](../day01-fitting).

## Task 1: Pull your changes from GitHub into RStudio

You successfully updated your GitHub repo from the main
[`README`](../README), but we still need to update your RStudio files.
Read these directions first, then work through them.

- In the **Git** pane of RStudio (upper right-hand area), locate and
  click on the
  <img src="../README-img/pull-icon.png" alt="knit" width = "20"/>
  **Pull** icon.
- After a few moments (you might need to provide your GitHub username
  and PAT), your **Files** pane will be updated with the new items.

Since you are likely looking at the `README` (this page) on GitHub,
nothing of importance for your work today was brought in. However, it is
always a good habit to **pull** from GitHub before starting to work
after taking a break. For example, if you were collaborating on a
project with others, they might do work at different times than you (or
even at the same time). It is always good to solve problems on your end
before **push**ing to GitHub and causing more problems.

## Some Important Questions

From [Section 3.2.2](https://hastie.su.domains/ISLR2/ISLRv2_website.pdf)
of *ISL* (p. 75), you read of a series important questions that we
should consider when performing multiple linear regression (copied here
for ease of access):

1.  Is at least one of the $p$ predictors $X_1$, $X_2$, $\ldots$, $X_p$
    useful in predicting the response $Y$?
2.  Do all the predictors help to explain $Y$, or is only a subset of
    the predictors useful?
3.  How well does the model fit the data?
4.  Given a set of predictor values, what response value should we
    predict and how accurate is our prediction?

We will explore these questions using the Human Freedom Index dataset.

## Task 2: Overall model - is at least one predictor useful?

### Fitting the overall model

**I forgot to include this in Day 1: Like we did with SLR (Activity 2),
we will only focus on data from 2016.** **Update your code when you read
in the dataset to then create `hfi_2016` which contains all observations
from 2016.**

We will explore a different model on this day, and I encourage you to go
back through Day 1 materials using this new model - when you have time.
Note (mostly a note to myself) that future iterations of this course
will only use this model instead of what was done on Day 1.

$$
\texttt{pf\\_score} = \beta_0 + \beta_1 \times \texttt{pf\\_expression\\_influence} + \beta_2 \times \texttt{pf\\_expression\\_control} + \varepsilon
$$

- Open your `day01-fitting/activity03.Rmd` file and
  <img src="../README-img/knit-icon.png" alt="knit" width = "20"/>
  **knit** it to run the work you completed during Day 1 of this
  activity.

- Create a new R code chunk and type the following, then run your code
  chunk or knit your document.

  ``` r
  # review any visual patterns
  hfi %>% 
    select(pf_score, pf_expression_influence, pf_expression_control) %>% 
    ggpairs()

  #fit the mlr model
  m_pf <- lm(pf_score ~ pf_expression_influence + pf_expression_control, data = hfi)
  tidy(m_pf)
  ```

You should obtain the following estimated model:

$$
\hat{y} = 4.71 + 0.188 \times \texttt{pf\\_expression\\_influence} + 0.288 \times \texttt{pf\\_expression\\_control}
$$

Interpretation of these parameter estimates (i.e., the *y*-intercept and
two slopes) are now slight different than how we interpreted the
parameter estimates for an SLR model. This is because we now have more
information in our model so we need to include that in our
interpretations:

> For countries with a `pf_expression_influence` of 0 (those with the
> largest amount of laws/regulations on media content) and
> `pf_expression_control` of 0 (those with the largest amount of
> political pressure on media content), we expect their mean personal
> freedom score to be 4.71.

> For a given `pf_expression_control` value (i.e., a given level of
> laws/regulations on media content), a 1 unit increase in
> `pf_expression_influence` is associated with a 0.188 unit increase of
> a country’s mean personal freedom score.

> For a given `pf_expression_influence` value (i.e., a given level of
> political pressure on media content), a 1 unit increase in
> `pf_expression_control` is associated with a 0.288 unit increase of a
> country’s mean personal freedom score.

Look back at the `ggpairs` output from above. Does the relationship
appear linear, between $Y$ and each of $X_1$ and $X_2$? If so, we can
quantify the strength of the relationship with the correlation
coefficient ($r$; these are displayed in the upper right-hand triangle
of the `ggpairs` output as “Corr: …”).

Note that this also provided a visual and numerical summary of the
relationship between $X_1$ and $X_2$. We will circle back to this in
Activity 4, but for now will only focus on the potential relationships
between the response variable and each predictor variable.

Answer the following questions:

1.  What do these (two) correlation values mean in the context of this
    model?

## Is there a relationship between the response and predictors?

We visually assessed if there appeared to be a linear relationship
between the response and each predictor variable (along with providing a
measurement of the strength and direction of that relationship if it is
linear). Now we would like to assess if this relationship is “note
worthy”. That is, is there overwhelming evidence that at least one of
the slopes associated with this MLR model exists (or is not zero). If a
slope is zero (e.g., if $\beta_1 = 0$), that would indicate no
relationship.

We can do this by testing the overall model. The statistical hypotheses
that we are testing are:

$$
\begin{align}
H_0&: \beta_1 = \beta_2 = 0 \\
H_a&: \text{At least one }\beta_j\text{ is not zero, for }j = 1, 2
\end{align}
$$

From your reading, this hypothesis test is performed using teh
$F$-statistic which has two degrees of freedom associated with it
($df_{model} = p; df_{error} = n - p - 1$).

- Create a new R code chunk and type the following code.

  ``` r
  summary(m_pf)
  ```

There is a lot of information that gets produced here and I am always
cautious of using functions that produce a lot without directly asking
for things - looking at you, SAS. However, the information we need to
assess the overall model is the very last line of the output:

> `F-statistic:  1308 on 2 and 1375 DF,  p-value: < 2.2e-16`

This provides us with the $F$-statistic, the corresponding degrees of
freedom (model and error, respectively) and the corresponding $p$-value
under the assumption that none of explanatory variables are useful in
our model.

$$
\begin{align}
F &= 1308 \\
df_{model} &= 2; df_{error} = 1308 \\
p\text{-value} &< 0.0001
\end{align}
$$

Answer the following questions:

2.  Using a significance level of $\alpha = 0.05$, make a decision with
    respect to the hypotheses we are testing.
3.  Based on the decision you made in (2), what does this mean? That is,
    interpret the decision in the context of these data.

## Task 3: Deciding on important variables

Since we have a small number of predictors that we are considering
($p = 2$), we can “easily” explore all four potential models: the model
containing no predictors, the (two) models containing each individual
predictor, and the model containing both predictors. The text goes into
three *classical* methods for automating this tasks, but I want to
caution you for using “black box” methods that are very data-hungry and
actually make our analyses less powerful (from the statistical
definition of
[**power**](https://en.wikipedia.org/wiki/Power_of_a_test)). We will
explore alternative methods to these classical approaches and I want to
encourage you to avoid these automated variable selection processes.

Answer the following questions:

4.  Looking back at your `tidy` output, you hopefully noticed columns
    named: `std.error`, `statistic`, and `p.value`. The values in the
    `statistic` column are calculated by taking the value in `estimate`
    column and dividing it by the value in the `std.error` column.
    Verify that the `statistic` values for `pf_expression_influence` and
    `pf_expression_control` are correct.
5.  Do you know what type of `statistic` these values are? Hint: you
    obtained an $F$-statistic in Task 2, these are not $F$-statistics.
6.  These `statistic` values also have a corresponding degrees of
    freedom associated with them in order to obtain the values in the
    `p.value` column. Each `statistic` value has the degrees of freedom
    corresponding to the error of the overall model. What are the
    degrees of freedom for both of these statistics (label and value)?
7.  Since the `statistic` values are on the same scale (they are from
    the same distribution with the error degrees of freedome), we can
    compare them to one another. Does one seem more important than the
    other using your knowledge of this distributions shape (might need
    to Google what this distribution from (5) looks like)? Explain your
    reasoning.

We will explore ways to assess if any predictors are more “important”
than others later in this course.

## Task 4: Model fit

To assess the overall model fit, remember that for SLR models we
explored the proportion of variability in the response variable that is
explained by the linear relationship with the explanatory variables. We
can get a similar measurement for MLR models.

- Create a new R code chunk and type the following code.

  ``` r
  glance(m1)
  ```

After doing this and running the code, answer the following questions:

8.  What is the value of $R^2$ for this model?
9.  What does this value mean in the context of this model?
10. This model is similar to the model you fit in Activity 2 except with
    one additional explanatory variable. Compare the $R^2$ value from
    your SLR model to this $R^2$ value. What do you notice? That is, is
    one “better” than the other? Explain your reasoning.

Looking back at your `summary(m_pf)` output, you may also have noticed
that the RSE (residual standard error) is also provided:

> `Residual standard error: 0.8077 on 1375 degrees of freedom`
> `(80 observations deleted due to missingness)`

I often use this value (and the $R^2$ value) to compare between
“candidate” or potential models. When we cover methods to compare
between models, we will discuss how to use these values.

Now we will assess the residuals of our MLR model fit.

**Linearity**: You already checked if the relationship between
`pf_score` and `pf_expression_influence` and between `pf_score` and
`pf_expression_control` is linear using. We should also verify this
condition with a plot of the residuals vs. fitted (predicted) values
once we have fit a MLR model.

- Create a new R code chunk and type the following code.

  ``` r
  # obtain fitted values and residuals
  m_pf_aug <- augment(m_pf)

  # plot fitted values and residuals
  ggplot(data = m_pf_aug, aes(x = .fitted, y = .resid)) +
    geom_point() +
    geom_hline(yintercept = 0, linetype = "dashed", color = "red") +
    xlab("Fitted values") +
    ylab("Residuals")
  ```

After doing this and running the code, answer the following question:

11. Is there any apparent pattern in the residuals plot? What does this
    indicate about the linearity of the relationship between the two
    variables?

**Nearly normal residuals**: To check this condition, we can look at a
histogram of the residuals.

- Create a new R code chunk and type the following code.

  ``` r
  ggplot(data = m_pf_aug, aes(x = .resid)) +
    geom_histogram(binwidth = 0.25) +
    xlab("Residuals")
  ```

After doing this and running the code, answer the following question:

9.  Based on the histogram, does the nearly normal residuals condition
    appear to be violated? Why or why not?

**Constant variability**:

10. Based on the residuals vs. fitted plot, does the constant
    variability condition appear to be violated? Why or why not?

## Task 5: Prediction

Similar to predicting values for a SLR model, we can identify
(appropriate - i.e., avoiding *extrapolation*) values of our explanatory
values to obtain an estimate of the response. The text (pp. 81–82)
describes three sorts of uncertainty associated with predictions and we
will see this in action later this semester. For the time being, let’s
keep things focused and we will obtain a confidence interval and a
prediction interval for a given country’s personal freedom score.

The values for our variables of interest associated with the United
States in 2016 are:

``` r
hfi %>% 
  filter(countries == "United States" & year == 2016) %>% 
  select(pf_score, pf_expression_influence, pf_expression_control)
```

    ## # A tibble: 1 × 3
    ##   pf_score pf_expression_influence pf_expression_control
    ##      <dbl>                   <dbl>                 <dbl>
    ## 1     8.75                       8                     7

We are going to do some “fancy” programming here and will see
alternative ways to do this once we get more familiar with modeling in
general (i.e., using more of `{tidymodels}` capabilities). Essentially
what this code is doing is pulling out the $X$ values we are interested
in predicting for an existing observation (`hfi %>% filter(...)`), then
passing those observations as the second argument of the `predict`
function using the `.` method.

- Create a new R code chunk and type the following code.

  ``` r
  hfi %>% 
    filter(countries == "United States" & year == 2016) %>% 
    predict(m_pf, .)
  ```

The `predict` function takes a model object (i.e., `m_pf`) and applies
it to a list/dataframe/tibble of values. Notice that I am not
focusing/`select`ing specific columns here - the `m_pf` object knows
what information to for!

After doing this and running the code, complete the following items:

11. Using your estimated equation, calculate the predicted value to
    verify that your code matches your math.
12. The actual personal freedom score for the United States in 2016 was
    8.74731. Calculate the residual by hand. Look at the `m_pf`.
13. **Challenge**: Using the `m_pf_aug` object from before and your
    advance `{dplyr}` skills (I can think of a couple of different ways
    to go about this), verify your answers in (11) and (12) by finding
    the corresponding row for the United States in 2016. While this
    object does not have information on countries or year, the rows line
    up with the rows in `hfi`.

## What is next?

We will look at ways to include non-quantitative features as well as
discuss other details to be aware of and consider when building MLR
models.
