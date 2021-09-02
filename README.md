# Ryan Steed's ML For Public Policy Homework

Author: Ryan Steed

## Data Collection and ETL Assignment - 09/14/2021

Code for this assignment is located in `acs`.

Using a Jupyter script for ease of data management. No reason to run a script 20 times during development and spam the API. Also no need to unnecessarily cache things. Downside is extensibility & modularity, but this is a small task and this code is written to be adaptable.

### Installation

Use a self-contained environment (`conda`, `virtualenv`, etc.) to avoid chaos. Python 3.8 recommended. Requirements can be downloaded from `requirements.txt`.

Also, get a Census API key from [here]("https://api.census.gov/data/key_signup.html") and store it as an environmental variable. Also set the database env variables as below. I use Conda for this, but there are other more lightweight methods:

```bash
conda env config vars set census_key=<YOUR_KEY>
conda env config vars set ACS_HOST=<HOST>
conda env config vars set ACS_PORT=<PORT>
conda env config vars set ACS_USER=<USER>
conda env config vars set ACS_PASS=<PASSWORD>
conda env config vars set ACS_DATABASE=<DATABASE>
```

### Design Choices

In general, I tried to do as little, as simply as possible. (Since I don't have an intended use for this data yet, I thought it best to keep things general and neat. Specific optimizations should be added later).

I did have a vague use case in mind - demographic impact assessment. I collected the 5-year (most reliable) race variables (counts of members identifying with different racial groups) in Pennsylvania. These data could be used to correlate a set of geo-outcomes (e.g. an environmental impact) with minority groups and other racial populations at the block group level. Linked geographically with other data, this data could help answer questions like, "How are the impacts of X distributed across racial groups?"

More details about my design choices can be found in the Jupyter notebook. A quick summary here:
- Used Jupyter notebook instead of a script to reduce API pings during development and enhance transparency, readability, visualization - this task isn't complicated or long enough for a full library
- Used `census` library as my API wrapper - so easy with an API key (instructions included)
- ACS variables are selected with a hard-coded list, including readable labels - simple to change or parameterize
- `pandas` makes storage easy - but didn't bother with an index at this stage
- Did very little transformation other than typing - e.g. later users will have to translate the county, state codes if they want string descriptors or geo data - the goal is to create a "usable format for downstream applications," which to me means generalizability and adaptability
- Used `sqlalchemy` and `pandas` for data loading
- Used composite primary key (`county`, `tract`, `block_group`) which I'm 90% sure is always unique
- Security: all DB info (including password) and API key are stored as environmental variables and loaded programmatically
- Dependencies: all pip-based, listed in `requirements.txt`; I used `conda`, but any virtual env will work

Limitations:
- Relies on many small-time libraries - e.g. `census` API wrapper
- Hard-coded elements (table name, variables downloaded) - but could be easily parameterized
- Expects an `acs` schema - if it doesn't exist already, fails
- I only picked integer variables (population counts)
- `pd.to_sql` is great, but it's quick and dirty with the schema - e.g there are no primary keys or indices - but could be easily created with a little extra raw SQL
- No bells and whistles in SQL - because downstream use unknown
