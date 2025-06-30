# AI Political Utilities

This repository contains a script for computing political utilities of LLMs. The script evaluates how LLMs and political entities (politicians/platforms) would prefer different policy options, then visualizes the results using PCA to show political alignment in a 2D space, e.g.,

![image](https://github.com/user-attachments/assets/14657ea4-38b3-4394-88c5-f7acdba7b4b0)

## Overview

The script works by:
1. Presenting pairwise policy comparisons to AI models and asking them to choose preferences
2. Using the same approach to model how political entities would respond
3. Fitting a Thurstonian model to obtain AIs'/entities' utilities for each policy option
4. Creating a PCA visualization showing the political positioning of AI models relative to known political entities

## Setup

### Environment Variables

Set the appropriate API keys for the model providers you plan to use:

```bash
export OPENAI_API_KEY=<your_openai_api_key>
export ANTHROPIC_API_KEY=<your_anthropic_api_key>
export GEMINI_API_KEY=<your_gemini_api_key>
export XAI_API_KEY=<your_xai_api_key>
```

### Dependencies

Install required Python packages:
```bash
pip install openai numpy torch scikit-learn matplotlib seaborn tqdm adjustText
```

## Usage

### Basic Command

```bash
python model_politics.py --model_name gpt-4.1 --model_provider openai --policy_options data/policy_options.json --entities data/entities.json
```

### Arguments

#### Required Arguments
- `--model_name`: Name of the AI model to evaluate (e.g., "gpt-4.1", "claude-3-sonnet")
- `--policy_options`: Path to JSON file containing policy options organized by category
- `--entities`: Path to JSON file containing politicians and political platforms to compare against

#### Model Config
- `--model_provider`: API provider for the main model (choices: openai, anthropic, google, xai; default: openai)
- `--entity_model_name`: Model to use for entity evaluations (default: gpt-4.1)
- `--entity_model_provider`: API provider for entity model (choices: openai, anthropic, google, xai; default: openai)
- `--base_url`: Override the default API base URL (useful for custom endpoints)

#### Output and Caching
- `--output_dir`: Directory to save results (default: political_results)
- `--precomputed_utilities_path`: Directory containing existing utility files to avoid recomputation (Only needed if results were explicitly moved to another location that is different from the default/set output directory)

#### Advanced Options
- `--system_prompt`: Custom system prompt for the AI model evaluation (affects only AI utilities, not entities)
- `--K`: Number of completions per prompt for robustness (default: 5)
- `--temperature`: Sampling temperature for model queries (default: 1.0)
- `--max_tokens`: Maximum tokens to generate per completion (default: 100)
- `--concurrency_limit`: Maximum concurrent LLM requests (default: 30)

### Examples

**Basic usage with GPT-4:**
```bash
python model_politics.py --model_name gpt-4.1 --model_provider openai --policy_options data/policy_options.json --entities data/entities.json
```

**Using Claude with custom system prompt:**
```bash
python model_politics.py --model_name claude-3-sonnet --model_provider anthropic --policy_options data/policy_options.json --entities data/entities.json --system_prompt "You are a thoughtful policy analyst."
```

**Using a custom API endpoint:**
```bash
python model_politics.py --model_name custom-model --model_provider openai --policy_options data/policy_options.json --entities data/entities.json --base_url "https://your-custom-endpoint.com/v1"
```

## Output

The script generates:

1. **Utility files**: JSON files containing computed political utilities
   - `political_results/ais/`: AI model utilities
   - `political_results/entities/`: Political entity utilities

2. **PCA visualization**: `political_results/political_pca.png`
   - Shows AI models and political entities positioned in 2D political space
   - AI models are highlighted in blue/red, entities in gray

## Caching and Reuse

The script automatically caches computed utilities. Subsequent runs will:
- Load existing utilities for previously computed models/entities
- Only compute utilities for new entities or models
- Generate updated PCA plots including all available models

To compare multiple AI models, simply run the script multiple times with different `--model_name` values.
