# Warehouse Lease Optimizer

A Streamlit web app that solves the Web Mercantile warehouse-leasing problem: find the cheapest combination of 1- to 5-month leases that covers the space required each month.

With the case data, the optimal total leasing cost is **$7,650,000**.

## Files

| File | Purpose |
|---|---|
| `app.py` | The web page: inputs, results, chart, and model explanation |
| `solver.py` | The linear-programming model, solved with SciPy's HiGHS solver |
| `requirements.txt` | Python packages Streamlit Community Cloud installs |

## Deploy it (GitHub + Streamlit Community Cloud, all in the browser)

1. **Create the repository.** On github.com, click **New** repository. Name it (for example `web-mercantile-optimizer`), set it to **Public**, and click **Create repository**.
2. **Upload the files.** On the new repo page, click **uploading an existing file**. Drag in `app.py`, `solver.py`, `requirements.txt`, and `README.md`, then click **Commit changes**. All four files should sit at the top level of the repo.
3. **Connect Streamlit.** Go to share.streamlit.io and sign in with your GitHub account. Authorize Streamlit when prompted.
4. **Create the app.** Click **Create app** in the upper-right corner, then choose **Yup, I have an app**.
5. **Fill in the form.**
   - Repository: `your-username/web-mercantile-optimizer`
   - Branch: `main`
   - Main file path: `app.py`
   - App URL: optional, for a short custom address
6. **Deploy.** Click **Deploy**. The first build takes a few minutes while the packages install. The app then gets a public `*.streamlit.app` URL.

Any later commit to the repo redeploys the app automatically.

## Run it on your own computer (optional)

```bash
pip install -r requirements.txt
streamlit run app.py
```

## Using the app

- **Sidebar:** set the planning horizon (1–12 months), the space required each month, and the cost per square foot for each lease length. Leave a cost blank if that lease length isn't offered.
- **Results:** the optimal total cost, compared against leasing month-to-month and leasing the peak amount for the whole horizon.
- **Chart:** stacked bars show which leases are in effect each month, and tick marks show the space required.
- **Tables:** the lease plan (when each lease starts and ends, its size, and its cost) and month-by-month coverage.
- **Reset to case data** restores the original Web Mercantile numbers.

## The model

Each decision variable is the square footage of one possible lease, set by its start month *s* and its length *L*. A lease must end by the last month of the horizon, which gives 15 lease options for the 5-month case.

- **Objective:** minimize the total of (cost per sq ft for an *L*-month lease) × (square feet leased), summed over all leases.
- **Constraints:** for each month, the square feet from all leases active that month must be at least that month's requirement.
- **Bounds:** square feet leased are whole numbers and never negative.
