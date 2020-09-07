# SimpsonsParadox: Automatic Simpson's Paradox Detector

## Description
This function automatically detects Simpson’s Pairs in a dataset using regression. Simpson's Pairs are pairs of independent variables where one of those variables reverses relationship with the dependent variable when conditioned on the second independent variable.

This function applies the following data pre-processing steps:
1. Excluding user-defined variables from the analysis 
2. Encoding any non-numeric variables in the data set 
3. Standardizing any continuous variables in the data
4. Binning large conditioning variables in the data

This function outputs the following Simpson's Pairs details:
1. The pair of variables with Simpson's Paradox (i.e. independent and conditioning independent variables)
2. Some of the summary statistics from the model generated for the aggregated data and disaggregated data
3. A simple plot of the regression lines for the aggregated data and each subgroup of the disaggregated data

More details about the pre-processing steps, outputs, and terminology can be found in the [Wiki](https://github.com/ehart-altair/SimpsonsParadox/wiki).

## Usage
1.	Download and unzip ``SimpsonsParadox-master.zip`` or use Git Bash to clone the repository.
2.	Open Anaconda Prompt, navigate to this project's directory, and run the following commands:
    * `conda env create -f environment.yml`
    * `conda activate simpsons-paradox`
    * `jupyter lab` or `jupyter notebook`
3. Load your own data, or use the examples provided in ```simpsons_paradox.ipynb```
4. Modify the function parameters depending on your use case. Here is an example:
```python
kwargs = {
    "df": df, # The pandas dataframe with the desired data set
    "dv": 'class', # The dependent variable to run analysis for
    "ignore_columns": ['user_id'], # A list of columns to ignore
    "bin_columns": ['timestamp'], # A list of the columns to bin
    "output_plots": True # Displays plots and summary statistics
}

simpsons_pairs = SimpsonsParadox(**kwargs).get_simpsons_pairs()
```
This outputs a list of Simpson's Pairs and displays plots and summary statistics for each Simpson's pair.

It's possible to modify other parameters, which all have defaults outlined in this [Wiki](https://github.com/ehart-altair/SimpsonsParadox/wiki/Arguments).

Here are some of the parameters:
```python
self.model = 'logistic'
self.weighting = True # Excludes weak cases of Simpson's Paradox
self.max_pvalue = 0.05 # Filters out pairs with large p-values
self.min_corr = 0.01 # Filters out pairs with no correlation
self.quiet = True # Silences all warnings and verbosity
```

### Trouble-shooting example
In some cases, the function will run into issues if the appropriate arguments aren't passed. 

For example, if you pass these arguments on this sample data set from the notebook:
```python
kwargs = {
    "df": pd.read_csv('data/conposcovidloc.csv'),
    "dv": 'Outcome1',
    "ignore_columns": ['Row_ID']
}

simpsons_pairs = SimpsonsParadox(**kwargs).get_simpsons_pairs()

```
You'll get the following error:
```
ValueError: You have a non-binary DV. Pass a value to the target_category in the function or re-bin your DV prior to using the function.
```
To fix this issue, add an additional argument to set a target category for one-versus-all logistic regression as follows:
```python 
kwargs = {
    "df": pd.read_csv('data/conposcovidloc.csv'),
    "dv": 'Outcome1',
    "ignore_columns": ['Row_ID']
    "target_category": 1
}

simpsons_pairs = SimpsonsParadox(**kwargs).get_simpsons_pairs()
```
Now the function should be able to run successfully.

See the [Wiki](https://github.com/ehart-altair/SimpsonsParadox/wiki) for more details about troubleshooting and unit tests.

### Command-line usage
You can also call this function from the command line.
1.	Download and unzip ``SimpsonsParadox-master.zip`` or clone the repo
2.	Open a command line and navigate to the directory of this project
3.  See the [Wiki](https://github.com/ehart-altair/SimpsonsParadox/wiki) for sample commands.

See the [Wiki](https://github.com/ehart-altair/SimpsonsParadox/wiki) for more details about the arguments and their defaults.

## References
Some existing tools and resources that we reference in this project:
* Simpsons R Package: https://rdrr.io/cran/Simpsons/man/Simpsons.html
* Can you Trust the Trend: Discovering Simpson's Paradoxes in Social Data: https://arxiv.org/abs/1801.04385

## Acknowledgements
This function was created as part of a summer internship project at [Altair Engineering](https://altair.com/).

Please let us know if you have any feedback and/or suggestions by starting an issue or [reaching out](mailto:walaamar@outlook.com).
