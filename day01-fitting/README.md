Day 1 - Fitting a MLR Model
================

In this repository/directory you should see two items:

- `README.md` - this document.
- `activity03.Rmd` - the file you will complete in RStudio for this
  week.

## Task 1: Open the RMarkdown document

Read these directions first, then work through them.

- In the **Files** pane of RStudio, locate and click on the
  `activity03.Rmd` file to open it.
- This file is essentially a blank document with only a `title` and
  `output` option (to produce a GitHub friendly Markdown file). You will
  follow the tasks in this `README` file and do the work (coding,
  responding, etc.) in RStudio.

![check-in](../README-img/noun-magnifying-glass.png) **Check in**

Review your `activity02-day01.Rmd` file (the one from last week) and
take note of: - What you like about its organization. - What do you want
to do differently this time?

Consider my `README` documents and how they are organized. I do not
think these are the “best” way, but they do follow typical standards
(headers, sections, code formatted text, labeled R chunks, etc.)

As you work through this activity, be descriptive in your response to
questions and even leave comments in your code to help you understand
what you are doing. These are your notes to yourself. How can you make
it easier for *future* your to remember what *current* you is
thinking/doing?

## Task 2: Load the necessary packages

We will be using two packages from Posit (formerly
[RStudio](https://posit.co/)): `{tidyverse}` and `{tidymodels}`.
Remember that [Emil Hvitfeldt](https://www.emilhvitfeldt.com/) (of
Posit) has put together a [complementary online
text](https://emilhvitfeldt.github.io/ISLR-tidymodels-labs/index.html)
for the labs in the *ISLR* text that utilize `{tidyverse}` and
`{tidymodels}` instead of base R.

- In the **Packages** pane of RStudio, check if `{tidyverse}` and
  `{tidymodels}` are installed. Be sure to check both your **User
  Library** and **System Library**.

- If either of these are not currently listed, type the following in
  your **Console** pane, replacing `package_name` with the appropriate
  name, and press Enter/Return afterwards.

  ``` r
  install.packages("package_name")
  ```

- Once you have verified that both `{tidyverse}` and `{tidymodels}` are
  installed, load these packages in the R chunk titled `setup`. Press
  Enter/Return after line 7 to add more code lines, then type the
  following:

  ``` r
  library(tidyverse)
  library(tidymodels)
  ```

- Run the `setup` code chunk or **knit**
  <img src="../README-img/knit-icon.png" alt="knit" width = "20"/> icon
  your Rmd document to verify that no errors occur.

Since we will be looking at many relationships graphically, it will be
nice to not have to code each of these individually. `{GGally}` is an
extension to `{ggplot2}` that reduces some of the complexities when
combining multiple plots. For example,
[`GGally::ggpairs`](http://ggobi.github.io/ggally/articles/ggpairs.html)
is very handy for pairwise comparisons of multiple variables.

- In the **Packages** pane of RStudio, check if `{GGally}` is already
  installed. Be sure to check both your **User Library** and **System
  Library**.

- If either of these are not currently listed, type the following in
  your **Console** pane and press Enter/Return afterwards.

  ``` r
  install.packages("GGally")
  ```

- Once you have verified that `{GGally}` is installed, load it in the R
  chunk titled `setup`. Add another code line in this chunk, then type
  the following:

  ``` r
  library(GGally)
  ```

- Run the `setup` code chunk or **knit**
  <img src="../README-img/knit-icon.png" alt="knit" width = "20"/> icon
  your Rmd document to verify that no errors occur.

Remember to organize your RMarkdown document using your Markdown skills.

## Task 3: Load the data and

The data we’re working with is from the OpenIntro site:
`https://www.openintro.org/data/csv/hfi.csv`

- Create a new R code chunk to read in the linked CSV file.
- Rather than downloading this file, uploading to RStudio, then reading
  it in, explore how to load this file directly from the provided URL
  with `readr::read_csv` (`{readr}` is part of `{tidyverse}` so you do
  not need to load/`library` it separately).
- Assign this data set into a data frame named `hfi` (short for “Human
  Freedom Index”).

Review the [Human Freedom
Index](https://www.openintro.org/data/index.php?data=hfi) description
page. If you still have questions, review the **Source** at the bottom
of the description page. After doing this, answer the following
questions:

1.  Is this an observational study or an experiment?

You will need to create appropriate *univariate* graphs to help in
answering this:

2.  Describe the distribution of `pf_score` Is the distribution skewed?
    Are there any other interesting/odd features (outliers, multiple
    peaks, etc.)? What does that tell you about countries’ personal
    freedoms? Is this what you expected to see? Why, or why not?

3.  Excluding `pf_score`, select two other numeric variables (hint: look
    for `<dbl>` or `<int>` designations) and describe their relationship
    with each other using an appropriate visualization.

## Task 4: Pairwise relationships

In Activity 2 you explored simple linear regression models.
Specifically, you fit and assessed:

$$
\texttt{pf\\_score} = \beta_0 + \beta_1 \times \texttt{pf\\_expression\\_control} + \varepsilon
$$

![check-in](../README-img/noun-magnifying-glass.png) **Check in**

Review how you described this model in Activity 2. - What were your
parameter estimates (i.e., the $\beta$s)? How did you interpret these
and what did they imply for this scenario? - How good of a fit was this
model? What did you use to assess this?

For this activity, we will begin using the two other *scores* variables
(i.e., `hf_score` and `ef_score`) to describe the patterns in
`pf_score`. Take a moment to think about what this previous sentence
means:

- What does this mean from a statistical point of view?
- What does this mean from a “real world” point of view?

Now, we will obtain graphical and numerical summaries to describe the
pairwise relationships.

- Create a new R code chunk and type the following, then run your code
  chunk or knit your document.

  ``` r
  hfi %>% 
    select(ends_with("_score")) %>% 
    ggpairs()
  ```

Note that a warning message (really a list of warning messages) will
display in your **Console** and likely under your R code chunk when you
knit this report. To suppress warning messages from displaying after
this specific R code chunk, add the follow inside the curly brackets
(`{r }`) at the top of your R code chunk (notice the preceding comma):
`, warning=FALSE`.

After doing this, answer the following questions:

4.  For each pair of variables, how would you describe the relationship
    graphically? Do any of the relationships look linear? Are there any
    interesting/odd features (outliers, non-linear patterns, etc.)?

5.  For each pair of variables, how would you describe the relationship
    numerically?

Aside, if we use both `hf_score` and `ef_score` to describe the patterns
in `pf_score`, hopefully you noticed that `hf_score` and `ef_score` are
collinear (correlated). Essentially, this means that adding more than
one of these variables to the model would not add much value to the
model. We will talk more on this issue in Activity 4 (other
considerations in regression models). For the time being, we will simply
continue ahead.

## Task 5: The multiple linear regression model

You will now fit the following model:

$$
\texttt{pf\\_score} = \beta_0 + \beta_1 \times \texttt{hf\\_score} + \beta_2 \times \texttt{ef\\_score} + \varepsilon
$$

- Create a new R code chunk and type the following, then run your code
  chunk or knit your document.

  ``` r
  m_hr_ef <- lm(pf_score ~ hf_score + ef_score, data = hfi)
  tidy(m_hr_ef)
  ```

After doing this, answer the following questions:

6.  Using your output, write the complete estimated equation for this
    model. Remember in Activity 2 that this looked like:

$$
\hat{y} = 4.28 + 0.542 \times \texttt{pf\\_expression\\_control}
$$

Your model here will be slightly different.

7.  For each of the estimated parameters (the *y*-intercept, and slope
    associated with each explanatory variable - three total), interpret
    these values in the context of this problem. That is, what do they
    mean for a “non-data” person?

## Challenge: 3-D plots

In *ISL*, the authors provided a 3-D scatterplot with a plane that
represents the estimated model. Do some internet sluething to minimally
produce a 3-D scatterplot (you do not need to include the plane).
Ideally, this would be something that plays nicely with (looks simlilar
to) `{ggplot2}`.

- Create a new R code chunk and add your code to create this plot.

After doing this, respond to the following prompt:

7.  Compare your 3-D scatterplot and the `GGally::ggpairs` output.
    Comment on the strengths and weaknesses of these two visualizations.
    Do both display on GitHub when you push your work there?

## What is next?

We will continue exploring MLR models and assess their fit using
`{tidymodels}`.
