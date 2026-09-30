# PCA Formative Assignment — Africa Economic, Banking and Systemic Crisis Data

## What this project is about

For this assignment we had to implement Principal Component Analysis (PCA) completely from scratch, using only numpy, no shortcuts like sklearn. The goal of PCA is to take a dataset with a lot of columns and compress it down into a much smaller number of columns while still keeping most of the important information (the variance) in the data.

## The dataset

We used the **Africa Economic, Banking and Systemic Crisis Data** dataset (originally sourced from Kaggle, credited to the user Chirin). It tracks economic and crisis-related indicators for African countries over time, including things like:

- Country and year
- Exchange rate to USD
- Annual inflation (CPI)
- Whether the country had a systemic crisis, banking crisis, currency crisis, or inflation crisis
- Sovereign and domestic debt default indicators
- GDP-weighted default

This dataset has 11+ numeric columns plus text columns like `country`, `cc3`, and `banking_crisis`, which satisfied the assignment's requirement of having at least 7 columns and at least one non-numeric column.

### Handling missing values

When we first checked the data, every column had zero missing values, which didn't meet the assignment's requirement of working with a dataset that has actual gaps in it. Rather than search for a different dataset, we intentionally introduced missing values ourselves: we randomly removed about 5% of the values from three numeric columns (`exch_usd`, `inflation_annual_cpi`, and `gdp_weighted_default`). This let us demonstrate the actual skill the assignment was testing — detecting and properly handling missing data — rather than relying on a dataset that happened to already have gaps.

We then filled those missing values back in using the mean of each column, which is a simple and defensible way to impute numeric data without distorting the overall distribution too much.

## Step by step, what we did and why

**1. Loaded the data.** We used pandas just to read the CSV into a table so we could inspect it — this part isn't PCA itself, just getting the data ready.

**2. Checked for missing values and data types.** We needed to know which columns were numeric and which were text, and confirm where the gaps in the data were.

**3. Separated numeric columns from text columns.** PCA only works on numbers, so we set aside `country`, `cc3`, and `banking_crisis` and kept the rest for the actual analysis.

**4. Standardized the data.** Our columns were on wildly different scales — `year` is in the thousands, while `exch_usd` is a tiny decimal. If we skipped this step, columns with naturally larger numbers would dominate the analysis just because of their scale, not because they're actually more important. Standardizing rescales every column to have a mean of 0 and a standard deviation of 1, putting them all on equal footing.

**5. Calculated the covariance matrix.** This is a grid that shows how every pair of columns relates to each other — for example, whether inflation and currency crises tend to rise and fall together. We needed this for two main reasons: first, it reveals which columns carry overlapping information, and second, it's the direct mathematical input for the next step, since eigenvalues and eigenvectors are calculated from it.

**6. Performed eigen decomposition.** Breaking the covariance matrix down gave us eigenvalues (numbers showing how much variance each new direction captures) and eigenvectors (the actual "recipes" for combining our original columns into new, compressed ones). The bigger the eigenvalue, the more important that direction is.

**7. Sorted the components by eigenvalue**, so the most important directions come first.

**8. Projected the data onto the top principal components**, reducing our 11 numeric columns down to just 2, while keeping as much of the meaningful variation between countries as possible.

**9. Visualized the data before and after PCA**, to see the compression in action.

## Why this matters for our dataset

Since our data is about African countries' economic activity and crisis history, compressing it down means we lose some of the fine-grained detail — for instance, we can no longer tell exactly which specific factor (inflation vs. debt default vs. exchange rate) is driving a country's position on the chart, only that it's driven by some combination of them. That's the tradeoff: a simpler, more visual picture of how countries compare, at the cost of some precision about which exact indicator matters most.

## What's in this repository

- `Template_PCA_Formative_1.ipynb` — the notebook containing all our code and written answers
- `data.csv` — the Africa Economic, Banking and Systemic Crisis Data dataset we used
- `README.md` — this file

## How we ran it

We built and ran this entire notebook in **Google Colab** (colab.research.google.com), not on a local machine, so that's the easiest way to reproduce it. Here's how, step by step:

1. Go to Google Colab and sign in with a Google account.
2. Choose **File → Upload notebook**, and select `Template_PCA_Formative_1.ipynb` from this repository.
3. In the notebook itself, click the folder icon on the left sidebar, then the upload button, and upload `data.csv` so it sits in the same Colab session as the notebook. Note: Colab session storage is temporary, so this file needs to be re-uploaded each time a fresh session is started (it does not persist automatically just because it's in this GitHub repo).
4. Run each cell from top to bottom, either one at a time using the play button beside each cell, or all at once using **Runtime → Run all**. The cells depend on each other in order (for example, standardizing the data depends on the data being loaded first), so they need to run sequentially rather than out of order.
5. Only two libraries are used throughout: `pandas` (just to load the CSV into a table) and `numpy` (for every actual PCA calculation — standardization, covariance, eigen decomposition, sorting, and projection). No `sklearn` or other machine learning libraries were used anywhere, as required by the assignment.

## Why we built it this way

We deliberately avoided any built-in PCA function so that every step — standardizing, computing covariance, decomposing into eigenvalues and eigenvectors, sorting by importance, and projecting the data down — is done manually with basic numpy operations. This was the whole point of the assignment: to actually understand what PCA is doing mathematically, rather than just calling a library function that hides all of that.
