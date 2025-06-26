Ensure API keys are set in the environment variables (example in `dotenv` file).

```bash
export OPENAI_API_KEY=<api_key>
export ANTHROPIC_API_KEY=<api_key>
export GEMINI_API_KEY=<api_key>
export XAI_API_KEY=<api_key>
```

To run the script (GPT-4.1 example):

```bash
python model_politics.py --model_name gpt-4.1 --model_provider openai --policy_options data/policy_options.json --entities data/entities.json --K 5
```

By default this will compute utilities for all entities and the `model_name` once, and save the results in a folder called `political_results/entities` and `political_results/ais`. Subsequent runs will load these utilities and only compute utilities for any new entities in the entities.json file that are not already in the results. The output pca plot will be saved in `political_results/political_pca.png`.

To obtain a PCA plot with multiple models, simply run the script multiple times with different desired `model_name`s.

You can also run the script with a system prompt via the `--system_prompt` flag, which will only affect utilities computed for the AI, and not the entities.

You can specify a precomputed utilities directory via the `--precomputed_utilities_path` flag.

For computing utilities for models on a non-standard base URL, you can set the `--base_url` flag (e.g., models that aren't available on the public API).