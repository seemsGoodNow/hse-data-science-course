# HW2 — Predict a business outcome and explain it

**Out:** Sep 30 (Session 5) | **Due:** Oct 21, 18:00 | **Weight:** 40% of the final grade | Individual.

## The task

A business asks a yes/no question: will this client default, will this customer open a deposit, will this guest cancel? You get a real table with the answer for past cases. Your job:

1. explore the data and find what goes with a "yes";
2. build a model you can read, then a stronger one, and compare them fairly;
3. choose how the business should use the model when each kind of mistake has a price;
4. explain it all to the person who makes the decision.

## Your dataset

You get one of five datasets in a direct message. Each is one table with a yes/no target. Read its documentation page **before** you trust a column.

| folder | business question | rows | target (share of "yes") | cost ratio |
|---|---|---|---|---|
| [`credit_card_default/`](data/credit_card_default/) | will a credit card client default next month? | 30,000 | `default payment next month` (22%) | 4 : 1 |
| [`bank_marketing/`](data/bank_marketing/) | will a client open a term deposit after a sales call? | 41,188 | `y` (11%) | 6 : 1 |
| [`home_equity/`](data/home_equity/) | will a home-equity loan go bad? | 5,960 | `BAD` (20%) | 4 : 1 |
| [`lending_club/`](data/lending_club/) | will a peer-to-peer loan default? | 40,000 (sample) | `Default` (20%) | 4 : 1 |
| [`hotel_bookings/`](data/hotel_bookings/) | will a hotel booking be cancelled? | 119,390 | `is_canceled` (37%) | 0.5 : 1 |

**Cost ratio** is the price of a missed "yes", counted in false alarms. 4 : 1 means that one missed "yes" costs as much as four false alarms. Part 5 uses it.

| dataset | a missed "yes" | a false alarm |
|---|---|---|
| `credit_card_default` | the unpaid balance is lost | a good client's limit is cut, and the interest they would pay is lost |
| `bank_marketing` | the deposit and its margin go to another bank | an operator spends a call on a client who says no |
| `home_equity` | the loss on the loan after the house is sold | a good borrower is turned away, and their interest is lost |
| `lending_club` | most of the lent money is lost | the interest on a good loan is lost |
| `hotel_bookings` | the room stays empty for the night | the room was sold again and the guest arrives: overbooking compensation and a lost customer |

In `hotel_bookings` the ratio is below 1 on purpose: there a false alarm is the dearer mistake.

**Default of credit card clients.** Credit card holders of a Taiwanese bank, 2005: credit limit, age, sex, education, marital status, and six months of repayment status, bills and payments. File: `credit_card_default.csv`, converted from the original spreadsheet without other changes.
Documentation: [UCI Machine Learning Repository, dataset 350](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients). Paper: Yeh and Lien (2009), *The comparisons of data mining techniques for the predictive accuracy of probability of default of credit card clients*, Expert Systems with Applications 36(2) ([doi](https://doi.org/10.1016/j.eswa.2007.12.020)). License: CC BY 4.0.

**Bank marketing.** Phone campaigns of a Portuguese bank, 2008–2010: client profile, the contact itself, earlier campaigns and five economic indicators of the month. File: `bank_marketing.csv`, the `bank-additional-full` version with commas instead of the original semicolons.
Documentation: [UCI Machine Learning Repository, dataset 222](https://archive.ics.uci.edu/dataset/222/bank+marketing): read the note on every column there, one of them matters a lot. Paper: Moro, Cortez and Rita (2014), *A data-driven approach to predict the success of bank telemarketing*, Decision Support Systems 62 ([doi](https://doi.org/10.1016/j.dss.2014.03.001)). License: CC BY 4.0.

**Home equity loans (HMEQ).** Loans secured by the borrower's home equity: loan size, mortgage due, property value, job, years on the job, credit history and debt-to-income ratio. File: `hmeq.csv`, as published.
Documentation: [Credit Risk Analytics, datasets page](http://www.creditriskanalytics.net/datasets-private2.html) (variable list). Source: Baesens, Roesch and Scheule (2016), *Credit Risk Analytics: Measurement Techniques, Applications, and Examples in SAS*, Wiley; the authors ask to cite the book when using the data.

**Lending Club (peer-to-peer loans).** Loans issued by the Lending Club platform in 2007–2018, reduced by the curators to the variables known **at the moment of the application**: income, loan amount, debt-to-income ratio, FICO score, employment, purpose, home ownership, state. File: `lending_club_sample.csv`, a random sample of 40,000 loans (seed 2026) without the long free-text column `desc`. This is the working file.
Documentation: [Zenodo record](https://doi.org/10.5281/zenodo.11295916), curated by Ariza-Garzón, Sanz-Guerrero and Arroyo Gallardo (2024). License: CC BY 4.0. **Optional:** the full file with all 1,347,681 loans (167 MB) is on the same Zenodo page; you don't need it for the homework.

**Hotel bookings.** Bookings of two hotels in Portugal (a resort and a city hotel) due to arrive between July 2015 and August 2017: lead time, length of stay, guests, meal, market segment, channel, deposit, special requests and more. File: `hotel_bookings.csv`, both hotels in one table (the TidyTuesday copy).
Documentation: Antonio, Almeida and Nunes (2019), *Hotel booking demand datasets*, Data in Brief 22 ([doi](https://doi.org/10.1016/j.dib.2018.11.126)): the paper describes every column and how the data was collected. CSV: [TidyTuesday, 2020-02-11](https://github.com/rfordatascience/tidytuesday/tree/master/data/2020/2020-02-11). Published open access under CC BY.

**Finding data like this yourself:** see Session 5, Demo 2, section 1.

## What you hand in

One notebook (`.ipynb` + exported `.html`), sent to the instructor in a direct message. Start from `hw2_template.ipynb` in this folder: it loads your dataset and holds the part titles. Write it as a researcher's story: say what you expect before the code, say what you see after it, and end with conclusions in words. The export steps are in the [setup guide](../../lectures/00-precourse/00-setup.md#exporting-a-notebook-to-html).

## Before you start: your data is not Taiwan

The bankruptcy table from Session 5 was all numbers, with no gaps. Yours has text columns, gaps, or both. Session code copied as is will stop with an error, or run and print wrong numbers.

- **The target as 1 and 0.** Models and the session counters expect 1 and 0. A text target doesn't always crash: a counter that compares with 1 just prints zeros. Convert it first, for example `(records["your_target"] == "yes").astype(int)` with your target column's name.
- **Text columns.** Logistic regression needs numbers (Part 3 shows how). CatBoost reads text itself, but only in the columns you name: `.fit(X_train, y_train, cat_features=text_columns)`, and the same list in `Pool(...)` for SHAP. If an assistant writes this code for you, tell it which columns are text.
- **Gaps.** Logistic regression stops at any missing value, with `Input X contains NaN` or `exog contains inf or nans`. The second message also appears when you divide a constant column by its std of zero. CatBoost accepts gaps in number columns; fill the gaps in text columns with a word, for example `.fillna("missing")`.

## Structure

### Part 0 — Clean the data
Goal: a table you can trust, and a short list of every change you made to it.

Check four things, using the documentation page:

1. **Leakage.** When does each group of columns become known: before the outcome or after it? A column known only after the outcome must go, however good it makes the model look (Session 6).
2. **Codes.** Values the documentation doesn't explain, and numbers that are really category codes.
3. **Gaps.** Which columns have them, and whether rows with a gap say "yes" more often than rows without.
4. **Duplicates**, and columns that almost never change.

Show: the list "what I drop or fix, and why", one line per decision.

### Part 1 — Explore: what goes with "yes"?
Goal: candidate drivers of the target, found with charts before any model.

1. The share of "yes", and the accuracy of a "model" that always says "no".
2. For 4 to 6 number columns you expect to matter: the "yes" rows and the "no" rows as two histograms on one chart, as in Session 3.
3. For 2 to 4 text columns: the share of "yes" in each category, with the number of rows behind it.
4. One more chart of your own choice that taught you something.

Every chart gets a title that says what is drawn, axis labels with units, and one sentence underneath on what you see.

Show: 3 to 6 hypotheses of the form "more X goes with more 'yes', because …". Part 3 tests them.

### Part 2 — Class weights: on or off?
Goal: a conscious choice about the rare class, made before any model.

Session 5 showed one cure for a rare "yes": class weights, where each "yes" row counts for more. In scikit-learn it is `class_weight="balanced"`, in CatBoost `auto_class_weights="Balanced"`. Decide whether you use them and write why. On a dataset that is barely imbalanced, "off" is a fair choice.

Use the same choice for every model you compare in Part 4. Part 5 checks whether it mattered.

Show: the choice and your reason, before any model.

### Part 3 — A baseline you can read
Goal: a logistic regression you can explain line by line.

1. Take 3 to 6 features from your Part 1 hypotheses, each with the direction you expect.
2. Split into train and test once, and keep this split for all later parts.
3. A text feature becomes 0/1 columns: `pd.get_dummies(table, columns=[...], drop_first=True, dtype=float)`. `drop_first` removes one category, because together they always add up to 1: an exact copy of the constant, the "exact twins" of Session 5. Each remaining weight then reads "compared with the dropped category".
4. Standardize the number features with the training mean and std, fit `sm.Logit`, and read the coefficients and their p-values. Read significance without class weights, even if you chose them in Part 2: statsmodels has no weights, and that is fine for reading directions. With tens of thousands of rows almost every p-value is tiny, so read the size of each coefficient too (Session 5).

Show: which directions match your hypotheses, which don't, and why.

If a weight is near zero, a p-value is near 1, or statsmodels says it did not converge, look at the share of "yes" for each value of that feature. A value that always means "yes", or always "no", breaks logistic regression, and it is a finding in itself.

### Part 4 — Can boosting beat it?
Goal: find out whether a model you can't read aloud is worth it on your data.

1. Train CatBoost on all the features you kept in Part 0, on the same split. Its opponent is your baseline refitted in scikit-learn (`LogisticRegression`, as in Session 5), and both get the same Part 2 choice. A logistic regression on more features is a fair opponent too.
2. On the test part, compare the models by caught, missed and false alarms, then accuracy, recall and precision. Put the train numbers next to them: a big gap between train and test means the model memorised.

Show: does boosting win on your data, by how much, and is the gain worth losing a model you can read? On some datasets it wins clearly, on others it doesn't. Both are valid findings.

### Part 5 — The threshold that costs least
Goal: the cut-off the business should use, given your cost ratio.

1. Take the model you would actually use, and say which.
2. Explain why accuracy misleads on your data, or why it misleads less. Show the confusion matrix at 0.5.
3. For thresholds from 0.01 to 0.99, compute the expected cost: missed × cost ratio + false alarms (Session 6). Draw one chart of cost against threshold, with the minimum and 0.5 marked.
4. At the chosen threshold, report recall, precision and the share of rows flagged.
5. Did your Part 2 choice matter? Fit the same model with the opposite choice (weights on instead of off, or the other way round), find its best threshold, and compare the two thresholds and the two costs.

Show: the threshold, what it costs compared with 0.5, what the business does with a flagged row, and whether your Part 2 choice mattered. Choosing the threshold on the test part, as in Session 6, is accepted; cross-validation on the training part is stronger. Say which you did.

### Part 6 — What drives the target
Goal: two views of "what matters", and two cases explained.

1. Put your Part 3 coefficients next to the boosting model's SHAP values: an importance ranking (built-in or mean |SHAP|, say which) and the beeswarm, as in Session 5. Do they agree on the main drivers and their direction? Where they don't, why: twins, a non-linear effect, gaps? Compare with your Part 1 charts too.
2. The beeswarm colours only mean something for number columns. For a text column, average its SHAP values by category, with the number of rows in each.
3. Explain two rows, one the model flags and one it clears, in words a manager would accept. Before each sentence, check the feature values behind the bars: a bar says which feature pushed, the value says why.

### Part 7 — Memo to the decision maker
At most 10 sentences, no code, for the person who owns the decision in your business (a credit committee, a head of sales, a revenue manager): what the model can and cannot do, which threshold you recommend and what it costs, the top-3 drivers, and what you'd want next (data, checks, monitoring).

## Rubric

| Criterion | Weight | What it means |
|---|---|---|
| Correctness of analysis | 40% | No leakage; honest split; imbalance handled consciously; metrics and threshold math correct |
| Research logic | 25% | Hypotheses from Part 1 tested in Part 3; class weights choice argued and tested; model comparison fair; claims follow evidence |
| Business conclusions | 20% | The memo is specific, quantified, and usable by a non-analyst |
| Artifact quality | 15% | Notebook reads top to bottom; every chart titled, labelled and read in a sentence; no dead cells |

A complete, correct submission on all four criteria scores **up to 8 of 10**. The last two points are for **one extra move**, done well:

- an **AI-generated HTML one-pager** of your memo for the decision maker;
- **cross-validation** from Session 6 instead of a single split when you compare the models in Part 4, with the spread across folds reported and read;
- **resampling** as a third answer to Part 2: after the split, copy the "yes" rows of the training part until the classes are closer in size (`pd.concat` and `.sample(replace=True)`), then find its best threshold in Part 5 next to the other two. Never before the split: copies of one row would land in both parts, and the model would be graded on rows it has memorised.

Exactly one extra counts, whichever is strongest; three half-done extras earn nothing. The extras are not required for 8.

## Rules

- **AI is allowed** under the [course AI policy](../../README.md#ai-policy): you must be able to explain every line you submit. For 2–3 students I ask a follow-up question in a direct message, announced practice, not suspicion.
- **Late work** loses 10% of the earned score per started day, up to three days; after that it is not accepted. Tell me *before* the deadline if something is wrong.

## Questions

Course chat. Underspecified bits are intentional: assume, write the assumption down, proceed.
