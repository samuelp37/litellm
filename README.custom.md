# LiteLLM fork adjustments

## Target use case

* This LiteLLM fork is used locally for personal use.
* LLM Models are used from providers Azure Foundry and AWS Bedrock

## Local setup

### Prerequisites

* Docker is installed (tested it with v29)

### Prepare LiteLLM repository

* Copy the `.env.custom.example` as `.env`.
* Fill the LiteLLM-related variables in the `.env`. See the [LiteLLM documentation](https://docs.litellm.ai/docs/proxy/docker_quick_start) for more info.

### Prepare Azure Foundry resource

* Create Azure Account
* Create a Resource Group and an Azure Foundry resource
* Within Azure Foundry, deploy the base models needed (make sure to update the `config.yaml` if any change)
* Copy the Microsoft Foundry endpoint in the `.env` file (should look like `https://<foundry-resource-name>.services.ai.azure.com`)
* Copy the API key

### Prepare AWS resources

* Create AWS account
* Setup AWS CLI and check access to AWS account (AWS SSO, access keys..etc)
* Reference these credentials inside a profile and copy the profile name in the `.env` file (`AWS_PROFILE_NAME`)
* Check access to AWS Bedrock and that all models listed in `config.yaml` are enabled. Note : Some models (ex. Anthropic) require agreeing extra EULA.

### Run and access UI

Run `docker compose up` and access `http://127.0.0.1:4000/ui`.

### Best practices before using

#### Create a virtual key in LiteLLM UI

Before using LiteLLM in coding agent setups, create a virtual key (security, budget) and eventually provide a defined budget for the key. This enables not to have to directly use the LiteLLM master key.

#### Managing AWS and Azure Budget

AWS and Azure budgets cannot be limited easily in terms of budget. Define a Budget on each provider to get an alarm if a specific consumption has been reached.

## Fork adjustments - summary

* Removing `config.yaml` from `.gitignore` : No secret should stored there anyway
* Sharing AWS credentials via volume mounting instead of explicitely exposing access keys (or creating a static Bedrock key). This enables as well to just use a AWS SSO based setup.
