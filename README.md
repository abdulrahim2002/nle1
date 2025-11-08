## Lab for Natural Language Engineering, Assignment 1

### Task 1 

is about obtaining structured data from unstructured data. We are
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
| --- | --- |
| paprika | 150 g |
| zucker | 0.014 liter |

Note that we must use some approximate measures for ambigous quantities
like `stück (slice), glas (glass), table spoon, tea spoon, etwas
(some)`. For example, if we have `etwas zucker` which means `some
sugar`, we must make an intelligent guess about its quantity in standard
units.

### Task 2
