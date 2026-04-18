<div align="center">
# 🎬 Item-Based Collaborative Filtering

A Comparative Analysis of Similarity Metrics on MovieLens
Show Image
Show Image
Show Image
Show Image
Predicting what users want to watch next, using the movies they've already loved.

</div>
📖 Overview
This project implements an Item-Based Collaborative Filtering recommendation system on the MovieLens Small dataset. Three similarity metrics — Cosine, Adjusted Cosine, and Pearson Correlation — are compared head-to-head on prediction accuracy, coverage, and runtime to determine which approach best captures inter-movie relationships in a highly sparse rating matrix.
<br>
📊 Dataset at a Glance
MetricValueTotal Ratings100,836Users610Movies9,724Rating Scale0.5 – 5.0Matrix Sparsity98.30%Train / Test Split80% / 20%Test Sample2,000 ratings

The extreme sparsity (98.30%) means that only 1.70% of all possible user–movie pairs have a recorded rating, making the prediction task both challenging and realistic.

<br>
⚙️ Pipeline
┌──────────┐    ┌──────────┐    ┌──────────────┐    ┌──────────────┐    ┌───────────┐    ┌──────────┐
│ Load     │ →  │ Split    │ →  │ Build Matrix │ →  │ Compute      │ →  │ Predict   │ →  │ Evaluate │
│ Data     │    │ 80 / 20  │    │ 610 × 8,983  │    │ Similarities │    │ Top-30 KNN│    │ RMSE/MAE │
└──────────┘    └──────────┘    └──────────────┘    └──────────────┘    └───────────┘    └──────────┘
<br>
🔬 Similarity Metrics Compared
1️⃣ Cosine Similarity
Measures the cosine of the angle between two item rating vectors. Treats unrated entries as zero. Simple, fast, and effective on sparse data.
cos(A, B) = (A · B) / (‖A‖ × ‖B‖)
2️⃣ Adjusted Cosine Similarity
Subtracts each user's mean rating before computing cosine similarity. Normalises for the fact that different users rate on different personal scales (e.g., a "generous" rater vs. a "strict" rater).
Adjusted: subtract user_mean from each rating → then compute cosine
3️⃣ Pearson Correlation
Centres each item vector by its own mean, then computes cosine. This is equivalent to the Pearson correlation coefficient and captures linear correlation between item rating patterns.
Pearson: center each item by item_mean → then compute cosine
For each test rating, the algorithm identifies the top-30 most similar items that the user has already rated, then computes a weighted average of those ratings (weighted by absolute similarity) to produce the predicted score, clipped to [0.5, 5.0].
<br>
📈 Results
Comparison Summary
MethodRMSE ↓MAE ↓CoverageTimeCosine Similarity0.8520 ⭐0.6522 ⭐96.2%0.2sAdjusted Cosine2.36782.066696.2%0.2sPearson Correlation0.86440.663296.3%0.1s
Metric Comparison Chart
Show Image
Similarity Heatmap — Top 10 Popular Movies
Show Image
<br>
💡 Key Findings

Cosine Similarity wins with the lowest RMSE (0.8520) and MAE (0.6522).

Why Cosine outperforms the others on this dataset:

Cosine vs. Pearson — Cosine edges out Pearson by a small margin (~1.5%). Both handle the sparse matrix well, but Pearson's item-mean centering introduces slight noise when most entries are zero.
Cosine vs. Adjusted Cosine — Adjusted Cosine performs significantly worse (RMSE 2.37) due to the extreme sparsity. When 98.3% of entries are missing and replaced with zero after subtracting user means, the resulting vectors are dominated by artifacts from the zero-fill, severely distorting the true similarity signal.
Coverage — All three methods achieve nearly identical coverage (~96%), confirming that the differences lie purely in prediction quality, not in the ability to generate predictions.

Heatmap Insights
ClusterMoviesSimilarityDramaForrest Gump, Shawshank Redemption, Pulp Fiction0.58ActionJurassic Park, Terminator 2, Braveheart0.56 – 0.60Sci-Fi BridgeThe Matrix, Star Wars IV0.40 – 0.51Family OutlierToy Story0.37 – 0.46
The heatmap reveals intuitive genre-based clustering: 90s dramas group together, action blockbusters form their own neighbourhood, and Toy Story stands apart as the sole animated/family film among adult-oriented titles.
<br>
🎯 Sample Recommendations
Top-10 predicted movies for User 1 using Cosine Similarity (k=30):
RankMoviePredicted Rating1Black Butler: Book of the Atlantic (2017)5.0002Fireworks, Should We See It from the Side or the Bottom? (2017)5.0003Unedited Footage of a Bear (2014)5.0004No Game No Life: Zero (2017)5.0005Kizumonogatari III: Cold Blood (2017)5.0006The Stanford Prison Experiment (2015)5.0007Five Senses, The (1999)5.0008A Perfect Day (2015)4.8889Codependent Lesbian Space Alien Seeks Same (2011)4.88810Watching the Detectives (2007)4.888
<br>
🚀 Quick Start
Prerequisites
bashpip install pandas numpy scikit-learn matplotlib
Run

Place archive.zip in the same directory as the notebook
Open item_based_cf.ipynb in Jupyter
Run cells 1–7 in order

python# Cell 1 auto-extracts the dataset
import zipfile, os
if not os.path.exists('ml-latest-small'):
    with zipfile.ZipFile('archive.zip', 'r') as z:
        z.extractall('.')
<br>
📁 Project Structure
📦 item-based-collaborative-filtering/
├── 📓 item_based_cf.ipynb        # Main notebook (7 cells)
├── 📦 archive.zip                # MovieLens Small dataset
│   └── ml-latest-small/
│       ├── ratings.csv           # 100,836 user-movie ratings
│       ├── movies.csv            # 9,724 movie titles + genres
│       ├── links.csv             # IMDb / TMDb links
│       ├── tags.csv              # User-generated tags
│       └── README.txt            # Dataset documentation
├── 📊 comparison_chart.png       # RMSE / MAE / Coverage bar chart
├── 📊 similarity_heatmap.png     # Top-10 movie similarity heatmap
└── 📄 README.md                  # This file
<br>
🛠️ Built With
ToolPurposePython 3.xCore languagePandasData manipulationNumPyNumerical computationscikit-learnSimilarity computation, metrics, train-test splitMatplotlibVisualisationJupyter NotebookInteractive development
<br>

<div align="center">
Data source: MovieLens Small by GroupLens Research · University of Minnesota
</div>
