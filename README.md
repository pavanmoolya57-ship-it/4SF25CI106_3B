# 4SF25CI106_3B

This project analyzes a Spotify artist dataset with Python and Pandas. The
notebook demonstrates dataset loading, data understanding, conditional data
transformation, missing-value handling with mean imputation, and grouped
insight generation.

## Project files

The analysis files are located under:

```text
project.ipynb/
└── projectt1.ipynb/
    └── cie/
        ├── cie.ipynb
        └── Spotify.csv
```

- `cie.ipynb` is the executable analysis notebook.
- `Spotify.csv` is the input dataset used by the notebook.

## Requirements

- Python 3.10 or newer
- Jupyter Notebook, JupyterLab, or Visual Studio Code with the Jupyter
  extension
- Pandas
- NumPy

Install the Python dependencies in a virtual environment:

```bash
python -m venv .venv
```

Activate the environment:

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Windows Command Prompt:

```bat
.venv\Scripts\activate.bat
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Install the required packages:

```bash
python -m pip install --upgrade pip
python -m pip install pandas numpy jupyter
```

## Running the notebook

The notebook reads `Spotify.csv` with a relative path:

```python
spotify_df = pd.read_csv("Spotify.csv")
```

Therefore, open or run the notebook with
`project.ipynb/projectt1.ipynb/cie` as the working directory. In a terminal:

```bash
cd project.ipynb/projectt1.ipynb/cie
jupyter notebook cie.ipynb
```

In Visual Studio Code, open the repository folder, open
`project.ipynb/projectt1.ipynb/cie/cie.ipynb`, select the Python environment
containing Pandas and NumPy, and run the cells from top to bottom.

## Notebook walkthrough

### Initial setup and Task 1: Dataset selection

The notebook imports Pandas and NumPy, then loads `Spotify.csv` into the
`spotify_df` DataFrame. It prints the dataset shape and displays the first five
rows so that the input can be checked before analysis.

The dataset contains 37,000 artist records and these columns:

| Column | Description |
| --- | --- |
| `ids` | Spotify artist identifier |
| `names` | Artist name |
| `popularity` | Artist popularity score |
| `followers` | Number of followers |
| `genres` | Artist genre |
| `first_release` | First release year |
| `last_release` | Most recent release year |
| `num_releases` | Number of releases |
| `num_tracks` | Number of tracks |
| `monthly_listeners` | Monthly listener count |

### Task 2: Data understanding

The notebook uses:

- `spotify_df.describe()` for statistical summaries of numeric columns.
- `spotify_df.info()` for row counts, data types, and memory usage.
- `spotify_df.isnull().sum()` to identify missing values in each column.

The `genres` field contains missing values, and `monthly_listeners` has a
small number of missing numeric values.

### Task 3: Data transformation

The transformation cell checks whether optional columns named `explicit` and
`energy` exist before using them:

- If `explicit` exists, it creates an integer `is_explicit_flag` column.
- If `energy` exists, it creates low, medium, and high energy categories using
  quantile bins.
- With the supplied artist dataset, those optional columns are not present, so
  the notebook reports that the transformations were skipped instead of
  raising a `KeyError`.

The cell then displays a sample of the main identifying and performance
columns.

### Task 4: Null-value handling

The notebook identifies numeric columns containing null values and fills each
one with that column's mean:

```python
spotify_df[column] = spotify_df[column].fillna(spotify_df[column].mean())
```

It prints missing-value counts before and after imputation. Text columns such
as `genres` are not mean-imputed because a numeric mean is not meaningful for
them.

### Task 5: Insight generation

Task 5 uses the actual column names from `Spotify.csv`.

#### Genre-level analysis

Missing genres are labeled `Unknown`. The data is grouped by `genres`, and the
notebook calculates:

- Average popularity
- Total followers
- Total tracks
- Total monthly listeners

The resulting table is sorted by total followers in descending order.

#### Artist-level analysis

The data is grouped by `names`, and the notebook calculates the same
performance measures for each artist. The resulting table is sorted by total
followers to show the highest-performing artists first.

> The notebook intentionally uses `genres`, `names`, `popularity`, `followers`,
> `num_tracks`, and `monthly_listeners`. Column names such as `genre`,
> `artist_name`, `stream_count`, `popularity_score`, and `track_id` belong to a
> different Spotify track-level schema and should not be used with this CSV.

## Troubleshooting

### `FileNotFoundError: Spotify.csv`

Run the notebook with `project.ipynb/projectt1.ipynb/cie` as the working
directory, or change the loading code to use the correct relative path.

### `KeyError` for `genre` or `artist_name`

Use the dataset's actual names: `genres` for genre and `names` for artist.
Also use the numeric fields listed in the dataset schema above.

### Missing `monthly_listeners` values after loading

Run the Task 4 cell before Task 5. Task 4 fills missing numeric values using
the mean of each affected column.

### Notebook cells show stale output

Restart the kernel, run the cells in order, and confirm that the selected
Python interpreter has Pandas and NumPy installed.

## Reproducibility

Run all cells from top to bottom in a fresh kernel. The analysis does not
require network access or API credentials; it uses only the included CSV file.
