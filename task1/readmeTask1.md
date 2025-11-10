## Task 1: Implementing Normalization

Implement a Python function that normalizes variants for units to either

- grams
- liters
- pieces (stück)

Find approximate values for tablespoon, glas, etc. based on
observations in the data file!

> Our main final output for task 1 in "gemma_annotation_normalized.csv" and other files within task 1 folder are for in-depth dataset analysis

### Description

the task is about obtaining structured data from unstructured data. We are
provided with `ingredient` and `amount`. We need to normalize all
quantities in standard units (i.e. liters, grams or g, pieces or stück in german)

For example, we have:

| ingredient | amount    |
|------------|-----------|
| paprika    | 1 stück   |
| milch      | 0.5 liter |


Note that we must use some approximate measures for ambiguous quantities
like `stück (slice), glas (glass), table spoon, tea spoon, etwas
(some)`. For example, if we have `etwas zucker` which means `some
sugar`, we must make an intelligent guess about its quantity in standard
units.

### Possible approaches

As mentioned in this lecture: https://elearning.uni-regensburg.de/mod/url/view.php?id=3009031

We can take different approaches to solve this problem.

Currently, we infer the ingredients and amounts from annotated data provided in `annotated_gemma.csv`
using logical rules/dictionaries/regular expressions etc

If later in the pipeline in task 3, we find that our approach is not good enough preprocessing for API calls, then we prompt the
llm to process the dataset further to identify the missing ingredients with right nutritional calculations (iterative process) 
