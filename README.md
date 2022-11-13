# Compute-Validation-Score-
Compute Validation Score - [CV 563]
Validation Notebook for My Candidate ReRank Model
In this notebook we compute validation score for my other notebook here which submits an LB 0.573 solution. To compute validation, we just need to load parquet files from a different Kaggle dataset. Instead of loading the real train and real test data. We load the first 3 week of original train as "new train". And the last 1 week of original train as "new test". Then we train our model with "new train" and predict "new test". Finally we compute competition metric from our predictions. The data and code for validation comes from Radek here.

Notes
Below are notes about versions:

Version 2 CV 0.5630 is validation for Candidate Rerank notebook version 1 with LB 0.573
Version 3 CV 0.5633 is validation for Candidate Rerank notebook version 2 with LB ???
Introduction from My Candidate ReRank Notebook
In this notebook, we present a "candidate rerank" model using handcrafted rules. We can improve this model by engineering features, merging them unto items and users, and training a reranker model (such as XGB) to choose our final 20. Furthermore to tune and improve this notebook, we should build a local CV scheme to experiment new logic and/or models.

Note in this competition, a "session" actually means a unique "user". So our task is to predict what each of the 1,671,803 test "users" (i.e. "sessions") will do in the future. For each test "user" (i.e. "session") we must predict what they will click, cart, and order during the remainder of the week long test period.

Step 1 - Generate Candidates
For each test user, we generate possible choices, i.e. candidates. In this notebook, we generate candidates from 5 sources:

User history of clicks, carts, orders
Most popular 20 clicks, carts, orders during test week
Co-visitation matrix of click/cart/order to cart/order with type weighting
Co-visitation matrix of cart/order to cart/order called buy2buy
Co-visitation matrix of click/cart/order to clicks with time weighting
Step 2 - ReRank and Choose 20
Given the list of candidates, we must select 20 to be our predictions. In this notebook, we do this with a set of handcrafted rules. We can improve our predictions by training an XGBoost model to select for us. Our handcrafted rules give priority to:

Most recent previously visited items
Items previously visited multiple times
Items previously in cart or order
Co-visitation matrix of cart/order to cart/order
Current popular items
