# SQL-finetune: Text-to-SQL on a custom exoplanets database
<div style="text-align: center;">
    <img src="assets/tau-bootis-b.jpg" alt="Tau Boötis b" width="600" />
    <p><em>Tau Boötis b, Source: European Southern Observatory, CC BY 2.0 image</em></p>
</div>

- The goal is to fine tune smaller LLMs on a  database to get the models used to domain-specific language, adhere better to SQLite standards, and generate SQL that would answer questions about the database with increased accuracy.  A database about exoplanets was chosen, but the process generalizes to text-to-SQL fine tuning on an arbitrary database.
  
- A small custom dataset of 50 training examples and 10 validation examples was created based on https://www.kaggle.com/datasets/adityamishraml/nasaexoplanets/data. SQLite table exoplanets was made from the data, along with a reference_planets table made by inserting (name, mass) VALUES ('Jupiter', 1.898e27) and (name, mass) VALUES ('Earth', 5.972e24). The mass_wrt column in exoplanets maps to the mass column in reference_planets, allowing for more complex queries involving joins.  The data is now available on Hugging Face Datasets at dpv/exoplanets-sql.  See exo_to_sql.ipynb for some of the test functions developed: the goal is to test the queries by running them against the database and i.) recording the execution rate and ii.) obtaining the result accuracy for queries in the validation data set.
  
- Fine tuning premai-io/prem-1B-SQL: execution rate is 70-80% (up from 50%) and accuracy is around 30% (up from 10%) with premsql within 10 epochs/2 minutes of fine tuning all of the model weights in full precision.  The big unknown is exact the prompt format used by premsql, which I could not exactly determine.  Perhaps fixing this could lead to much better results. See notebook Fine_tune_premsql.ipynb for details.
  
- Fine tuning meta-llama/Llama-3.2-3B-Instruct with LoRA: execution rate starts at 80-90% percent for the original model, but the results are not accurate (no full matches, but did not investigate close matches). Fine tuning on the exoplanets data set took less than 2 minutes and still results 90% execution rate but now also a result accuracy of 70% (with affordances granted to differences in column names/multiple ways to interpret one of the questions).  In addition, the original model with weights in full precision took over 6.5 minutes to generate 10 queries whereas the fine tuned LoRA version took 12 seconds per 10 queries.  See notebook Fine_tune_llama3.ipynb for details.
