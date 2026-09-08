# Data Analysis and Machine Learning for Business

An introductory Data Science course for the Master's programme **Strategic Corporate Finance** (HSE University). Module 1, 7 sessions, Wednesdays 18:10–21:00. Everything you need is in this repository.

## Communication

**Course Telegram chat** — announcements, recaps, and questions. Ask there first: a question one of you has, five of you have.

- **Environment broken?** Check the [error table in the setup guide](lectures/00-precourse/00-setup.md#when-something-breaks), then ask your AI assistant with the full error text pasted in, then write in Telegram Chat.

## First steps

Nothing here is graded, but the rest of the course assumes it.

1. **Set up your tools** with the [setup guide](lectures/00-precourse/00-setup.md). [Google Colab](https://colab.research.google.com) takes five minutes and installs nothing; Python plus VS Code on your own laptop takes about twenty. **Colab always counts in this course.**
2. **Work through the [Python primer](lectures/00-precourse/python-primer.ipynb).** One to two hours, self-paced, solutions inside. We speed-run it in class; you walk it slowly at home.
3. **Get an AI assistant account.** [DeepSeek](https://chat.deepseek.com), [Qwen](https://chat.qwen.ai) and [GLM](https://chat.z.ai) are free and handle everything at this level. [ChatGPT](https://chatgpt.com) and [Claude](https://claude.ai) are the ones I demo in class.
4. **Fill in the pre-course survey** if you have not yet ([survey](https://forms.gle/JchJimKWVagzocye8)).

And bring a laptop to every session.

## Schedule

Wednesdays, 18:10–21:00: two 80-minute parts with a break. First session **September 2**.

| # | Date   | Topic                                                        | Materials | Milestone            |
|---|--------|--------------------------------------------------------------|-----------|----------------------|
| 1 | Sep 2  | Intro: where data comes from, tools setup, Python quick start | [01-intro](lectures/01-intro/) |            |
| 2 | Sep 9  | Working with tables: pandas + SQL                            | [02-tables](lectures/02-tables/)         |                      |
| 3 | Sep 16 | Descriptive statistics and visualization                     | —         | **HW1 out**          |
| 4 | Sep 23 | Hypothesis testing                                           | —         |                      |
| 5 | Sep 30 | Machine learning on tabular data                             | —         | **HW1 due 18:00, HW2 out** |
| 6 | Oct 7  | Evaluating models                                            | —         | **Quiz, first 25 min** |
| 7 | Oct 14 | Clustering and communicating results                         | —         | **HW2 due 18:00**    |

The Materials column fills in as we go: notebooks are pushed the same evening as the session, slides right after.

## Grading

**40% HW1 + 40% HW2 + 20% quiz.** No exam, no final project.

| Work | Out | Due | Weight |
|---|---|---|---|
| [**HW1**](homeworks/hw1/) — one warehouse, and what you can prove about it | Sep 16 | Sep 30, 18:00 | 40% |
| [**HW2**](homeworks/hw2/) — corporate bankruptcy: predict it and explain it | Sep 30 | Oct 14, 18:00 | 40% |
| **Quiz** — in class, start of Session 6 | | Oct 7, 18:10 | 20% |

Both homeworks are individual: in HW1 you are assigned one whole warehouse out of four, in HW2 your own subsample of the bankruptcy data and your own cost of a missed default. The format is a *researcher's story*: a notebook where hypotheses are stated **before** the code, conclusions follow the evidence, and a short business summary closes the work. You submit the notebook plus an exported HTML by direct message. A detailed rubric is published with each assignment.

The **quiz** is 15 questions, about 25 minutes, closed book. It tests understanding, not memory: read a chart, spot the bug in a pandas snippet, interpret a p-value.

### AI policy

AI assistants are allowed and encouraged; learning to work with them is part of the course. Two rules:

1. **Think first, then ask, then verify.** Try it yourself, formulate a good request, and check whatever you get before using it.
2. **You should be able to explain every block of code you submit.** I may ask.

## What is in this repository

```
lectures/00-precourse/   setup guide + Python primer — do this before Session 1
lectures/NN-topic/       notebooks and materials for each session, slides added after it
lectures/extras/         optional deep-dives, added over time
homeworks/hwN/           the assignment, its rubric, and the data it runs on
data/                    course datasets and the data dictionary (data/README.md)
```

The warehouse data sits in `data/` from day one; homework data ships inside each assignment folder.

## About the course

**The idea.** Data Science has changed. With modern AI tools you don't need to memorize every function parameter. You need to understand the ideas, know what result you want, and be able to verify what you get. This course teaches you to solve real business problems on lifelike data: you do the analysis yourself, and use AI as a reference and an accelerator for boilerplate code, quick prototypes, and polished HTML reports that beat any spreadsheet.

**By the end of the course**, given an unfamiliar set of tables, you will be able to:

- reconstruct the business process behind the data and check your understanding against the data itself;
- explore, clean, join and aggregate tables with pandas (and read basic SQL);
- test hypotheses with statistics and interpret the results honestly;
- train and interpret ML models (regressions, gradient boosting, SHAP) without drowning in math;
- package findings into artifacts a business person will actually read, including AI-generated HTML reports.

**The data.** Sessions 1 to 4 and HW1 run on **warehouse operations logs** (picking, stock, warehouse topology), closely modelled on the live processes of a real e-commerce site. Sessions 5 and 6 and HW2 move to your home turf: **corporate bankruptcy data**, the financial ratios of ~6,800 real companies with a bankruptcy label. Session 7 adds **consumer complaints about financial products**, ten thousand pieces of real text to cluster.

## What's inside each session

<details>
<summary><b>1. Intro: where data comes from, tools, Python quick start</b></summary>

- Data is exhaust from business processes: transactions, orders, clicks, warehouse scans. To read a table, understand the process that fills it
- The three levels of a business question: what happened, why, and what will happen
- Roles in a data team (engineers, analysts, data scientists) and where you fit in
- The toolbox: from Excel to pandas/SQL, and why code beats clicks once tables get big
- Setup: Python, VS Code, Jupyter; Google Colab as the zero-install alternative ([step-by-step guide](lectures/00-precourse/00-setup.md))
- Python crash course, supported by the self-paced [primer](lectures/00-precourse/python-primer.ipynb)
- First look at 312 thousand warehouse events, and the first-contact routine for any unknown table: head → shape → dtypes → describe → value_counts → one chart
- The AI loop, live on a real question: think → ask well → verify
</details>

<details>
<summary><b>2. Working with tables: pandas + SQL</b></summary>

- Select, filter, sort; new columns; groupby and aggregation
- Joining tables, and the three ways a join breaks without raising an error: rows lost, rows multiplied, the wrong key
- The row-count check that catches all three
- The same in SQL on sqlite: read and write basic queries, spot the analogies
- Opening a pickle: what a saved Python object is and how to get your data out of one
- The first-contact routine, second pass: missing values, duplicates, types
- The habit worth stealing: not sure what a function does? Build a five-row table where you know the answer and look
- Micro-cases on warehouse data
</details>

<details>
<summary><b>3. Descriptive statistics and visualization</b></summary>

- Mean vs median, quantiles, spread — what to report and when
- Key charts and what they are for: histogram/boxplot, line/scatter, bar
- Making a chart readable for a business person, and spotting the charts that mislead you
- Two warehouses side by side: the same metrics, two different stories
- AI for advanced visuals: quick HTML/interactive charts from a prompt
</details>

<details>
<summary><b>4. Hypothesis testing</b></summary>

- Null and alternative hypotheses: the intuition, with pictures
- What a p-value actually says (and what it doesn't)
- Choosing a test: a practical decision guide
- Pitfalls: peeking, multiple comparisons, "significant but tiny"
- Case: does factor X really change warehouse performance?
</details>

<details>
<summary><b>5. Machine learning on tabular data</b></summary>

- What ML solves: prediction vs explanation, regression vs classification
- Linear and logistic regression as ideas; overfitting
- Gradient boosting, the workhorse of tabular ML
- Which features matter: SHAP, by analogy with regression coefficients
- "Train and test a model in minutes" with an AI assistant, and what to check afterwards
- Case: corporate bankruptcy, from Altman's Z-score (1968) to gradient boosting
</details>

<details>
<summary><b>6. Evaluating models</b></summary>

- Quiz (first 25 minutes)
- Loss is not the metric; the accuracy trap
- Confusion matrix, precision/recall; picking a threshold when errors have costs
- Train/test splits, leakage, cross-validation, and why models lie to you
- Case continued: choosing the operating point of the bankruptcy model when errors have costs
</details>

<details>
<summary><b>7. Clustering and communicating results</b></summary>

- Unsupervised learning: k-means intuition, hands on
- Clustering real text: encoding thousands of real consumer complaints on a laptop CPU
- Dimensionality reduction at a glance (PCA and t-SNE, read as a map rather than as math)
- The finale: a full research from scratch, turned into an AI-assisted HTML report for a business customer
</details>
