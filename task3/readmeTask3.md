### Task 3: Improving Prompts

In the following, you find the currently used prompts to process raw
data (see columns *amount* and *ingredient*) with an LLM (llama3.3:70b)

> Please kindly find our solution explanation for this task down below at **Processing Flow Overview** 

#### For Ingredient Extraction

```
Your task is to identify cooking ingredients in texts and output them in the nominative singular.
If characteristics such as color, condition, processing, quality, or origin are mentioned for an ingredient, output these characteristics in the German basic form as well.

1. Please generate a JSON array for the output.
2. Always use `zutat` as the key for the ingredient and `eigenschaft`
   for all properties. If none is mentioned, do not use this key.
3. If an ingredient has more than one property, still only name the key
   `zutat` once.

**Example:**
**Input:** `garnelen frisch groß`
**Output:**

{
"zutat": "Garnele",
"eigenschaft": ["frisch", "groß"]
}

Now identify the ingredients in this text: `{ingr}`
```

#### For Amount Extraction

```
Your task is to identify quantities, weights, and volumes in cooking
ingredients in texts.

1. Please generate a JSON array for the output.
2. Always use `menge` as the key for quantities, `gewicht` for weights,
   and `volumen` for volumes.
3. Always specify the unit found, e.g., `l`, `g`, `Stück`, `Prise`,
   `msp`, or `Esslöffel`.
4. Use `einheit` as the key for this. Do not include any other
   information in the output.

Now identify quantities, weights, and volumes in this text: `{amount}`

```

---


- Analyze the log file from Task 2 to identify patterns in raw input
  that are not processed correctly with the current prompts.
- Optimize the prompts by adding constraints for the LLM output and/or
  by providing examples (one-/few-shot learning).
- Iterate your solution to Task 2 with the modified prompts until your
  log file only contains cases that you consider difficult to solve or
  even not solvable by human experts (e.g., severe misspellings).

For this task, you can access the Ollama Server on
`hcai.uni-regenburg.de` and use the `llama3` model.  Python code for
accessing the API is provided on GRIPS.

### **Processing Flow Overview** 

The full workflow for extracting and enriching nutritional data proceeds in multiple refinement stages using "task3.ipynb" for LLM processing and "testing.ipynb" for API testing:

1. **LLM-Based Ingredient Normalization – Prompt 1**

   * Input: `failed_extracts_initial.csv`
   * Processed using **Prompt 1**  
   * Output: `gemma_annotated_prompt1.csv`

2. **Re-Evaluation of Prompt 1 Results**

   * The annotated ingredients from `gemma_annotated_prompt1.csv` are tested again via the UR Nährwertrechner API.
   * Output:

     *  Remaining errors are saved as `failed_extracts_1.csv`

3. **LLM-Based Quantity & Unit Refinement – Prompt 2**

   * Input: `failed_extracts_1.csv`
   * Processed using **Prompt 2** 
   * Output: `gemma_annotated_prompt2.csv`

4. **Final Nutrition Extraction Attempt**

   * The LLM-refined dataset `gemma_annotated_prompt2.csv` is reprocessed through the API.
   * Output:

     *  Remaining unresolved rows saved as `failed_extracts_final.csv`
