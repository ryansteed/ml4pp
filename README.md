# Ryan Steed's ML For Public Policy Homework

## Data Collection and ETL

Using a Jupyter script for ease of data management. No reason to run a script 20 times and spam the API. Also no reason to unnecessarily cache things. Downside is extensibility, but this assignment is narrowly scoped.

### Installation

Use a self-contained environment (`conda`, `virtualenv`, etc.) to avoid chaos. Python 3.8 recommended. Requirements can be downloaded from `requirements.txt`.

Also, get a Census API key from [here]("https://api.census.gov/data/key_signup.html") and store it as an environmental variable. I use Conda:

```bash
conda env config vars set census_key=<YOUR_KEY>
conda env config vars set ml4pp_pass=<PASSWORD>
```

