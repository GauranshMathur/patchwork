<p align="center">
  <img src="https://github.com/patched-codes/patchwork/assets/126385808/a7adcf24-b615-43a0-a244-45789d184f0a" width="160" alt="PatchWork Logo">
</p>

# PatchWork

An open-source framework for patching and managing code repositories using large language models. PatchWork allows you to automate workflows like PR reviews, bug fixing, security patching, and more using a self-hosted CLI agent and your preferred LLMs.

## Key Components

- **Steps**: A set of reusable atomic actions that define various operations.
- **Patchflows**: LLM-assisted automations such as PR reviews, code fixing, debugging.

Patchflows can be run locally in your CLI and IDE, or as part of your CI/CD pipeline.

## Installation

### Using Pip

PatchWork is available on PyPI and can be installed using pip:

```bash
pip install patchwork-cli --upgrade
```

### Using Poetry

PatchWork is built using Poetry, a dependency management and packaging tool for Python. To install PatchWork using Poetry, follow these steps:

1. Make sure you have Poetry installed. If you don't have it installed, you can install it by running:
   ```
   curl -sSL https://install.python-poetry.org | python3 -
   ```

2. Clone the PatchWork repository:
   ```
   git clone https://github.com/patched-codes/patchwork.git
   ```

3. Navigate to the project directory:
   ```
   cd patchwork
   ```

4. Activate a shell using virtual environment:
   ```
   poetry shell
   ```

5. Install the dependencies using Poetry:
   ```
   poetry install
   ```

## PatchWork CLI

The CLI runs Patchflows, as follows:

```
patchwork <Patchflow> <?Arguments>
```

Where
- **Arguments**: Allow for overriding default/optional attributes of the Patchflow in the format of `key=value`. If `key` does not have any value, it is considered a boolean `True` flag.

### Example

For an AutoFix patchflow which patches vulnerabilities based on a scan using Semgrep:

```bash
patchwork AutoFix openai_api_key=<YOUR_OPENAI_API_KEY> github_api_key=<YOUR_GITHUB_TOKEN>
```

The above command will default to patching code in the current directory, by running Semgrep to identify the vulnerabilities.

You can take a look at the `default.yml` [file](patchwork/patchflows/AutoFix/defaults.yml) for the list of configurations you can set to manage the AutoFix patchflow. 

The [patchwork-configs](https://github.com/patched-codes/patchwork-configs) repository contains the default configuration and prompts for all the patchflows. You can clone that repo and pass it as a flag to the CLI:

```bash
patchwork AutoFix --config /path/to/patchwork-configs/patchflows
```

## Patchflows

Patchwork comes with a set of predefined patchflows, and more will be added over time. Below is a sample list of patchflows:

- AutoFix: Generate and apply fixes to code vulnerabilities in a repository.
- DependencyUpgrade: Update your dependencies from vulnerable to fixed versions.
- PRReview: On PR creation, extract code diff, summarize changes, and comment on PR.
- GenerateREADME: Create a README.md file for a given folder, to add documentation to your repository.

## Prompt Templates

Prompt templates are used by patchflows and passed as queries to LLMs. Templates contain prompts with placeholder variables enclosed by {{}} which are replaced by the data from the steps or inputs on every run. 

Below is a sample prompt template:

```json
{
  "id": "diffreview_summary",
    "prompts": [
      {
        "role": "user",
        "content": "Summarize the following code change descriptions in 1 paragraph. {{diffreviews}}"
      }
    ]
}
```

Each patchflow comes with an optimized default prompt template. But you can specify your own using the `prompt_template_file=/path/to/prompt/template/file` option. 

## Contributing

To create a new patchflow, follow [these instructions](patchwork/patchflows/README.md).

To create a new step, follow [these instructions](patchwork/steps/README.md).
