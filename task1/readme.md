## Task 1: Implementing Normalization

Implement a Python function that normalizes variants for units to either

- grams
- liters
- pieces (stück)

Find approximative values for tablespoon, glas, etc. based on
observations in the data file!



### Description

the task is about obtaining structured data from unstructured data. We are
provided with `ingridient` and `quantity`. We need to normalize all
quantities in standard units (i.e. liters, grams, pieces)

For example, we have:

| ingredient    | amount    |
|------         | -----     |
| paprika       | 1 stück   |
| zucker        | 0.5 tl    |


> Note:  tl = table spoon

For this data, we need to find the normalized quantities.

| ingredient | normalized amount |
| ---        | ---               |
| paprika    | 150 g             |
| zucker     | 0.014 liter       |

Note that we must use some approximate measures for ambigous quantities
like `stück (slice), glas (glass), table spoon, tea spoon, etwas
(some)`. For example, if we have `etwas zucker` which means `some
sugar`, we must make an intelligent guess about its quantity in standard
units.

### Possible approaches

As mentioned in this lecture: https://elearning.uni-regensburg.de/mod/url/view.php?id=3009031

We can take a couple of approaches to solve this problem.

Currently, we infer the ingredients and amounts from annotated data provided in `annotated_gemma.csv`
using rules/dictionaries/regular expression.

If later in the pipeline, we find that our approach is not good enough, then we reprompt the
llm to identify the amounts and units ourself
