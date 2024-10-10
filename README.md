# sql-finetune
- Fine tuning premai-io/prem-1B-SQL: brought execution rate up from 50% to 80% within 10 epochs/2 minutes of training.  Results don't yield the same answers with neither the original nor the fine tuned model, though!  Could be due to lack of knowledge about the prompt structure for premsql model, or perhaps the questions in the validation set need to be made clearer.
- Fine tuning meta-llama/Llama-3.2-3B-Instruct: execution rate starts at 80-90% percent for the original model, but the results are not accurate (no full matches, but did not investigate close matches). After fine tuning on the exoplanets data set, which took less than 2 minutes, got 90% execution rate and 70% result accuracy (with affordances granted to difference in column names/multiple ways to interpret one of the questions).
