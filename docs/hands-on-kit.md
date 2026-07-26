# Hands-on kit guide

## Audience and outcomes

- Audience: developers, cloud engineers, and solution architects
- Skill level: intermediate (basic Azure and JavaScript familiarity)
- Duration: 90 to 150 minutes

By the end of the hands-on, participants will:

- deploy a secure end-to-end application with azd
- explain why this architecture uses private endpoints and managed identity
- run local and cloud validation checks
- implement one small extension and redeploy

## Prerequisites

- Node.js 20+
- npm 10+
- Azure Developer CLI (azd)
- Azure CLI
- Access to an Azure subscription that can deploy Azure OpenAI
- Permission to create role assignments and deploy resources

## Session flow

## Module 0: Environment setup (10 to 20 min)

1. Clone the repository and enter the folder.
2. Install dependencies.
3. Sign in to Azure.

```bash
git clone <repo-url>
cd openai-secure-ui-js
npm ci
azd auth login
```

Checkpoint:

- azd auth status succeeds
- npm ci finishes without errors

## Module 1: Deploy baseline (20 to 35 min)

1. Verify model availability for your target Azure region.
2. Optionally set model and version values.
3. Run deployment.

```bash
az cognitiveservices model list --location eastus2 --query "[?kind=='OpenAI'].{Name:model.name, Version:model.version, Format:model.format}" -o table
azd env set AZURE_OPENAI_API_MODEL gpt-5-mini
azd env set AZURE_OPENAI_API_MODEL_VERSION <available-version>
azd up
```

Checkpoint:

- deployment succeeds
- static web app URL is returned
- chat app can load and sign-in flow starts

## Module 2: Validate secure architecture (20 to 30 min)

1. Open the resource group in Azure Portal.
2. Validate network and identity controls:

- storage account public network access is disabled
- private endpoints exist for blob, queue, and table
- function app uses managed identity
- api can call Azure OpenAI without API keys in code

Checkpoint:

- team can describe how private DNS and private endpoints route storage access

## Module 3: Run locally and compare (15 to 25 min)

1. Start frontend and backend locally.
2. Test baseline chat behavior.

```bash
npm run start:webapp
```

Checkpoint:

- app responds locally
- participants can explain local vs cloud execution path

## Module 4: Make one extension (20 to 40 min)

Pick one extension and redeploy:

- switch web app framework in azure.yaml
- add a UI tweak in packages/ai-chat-components
- enable Microsoft Defender user context

Redeploy:

```bash
azd deploy
```

Checkpoint:

- extension works in deployed app
- participants can describe impacted files and services

## Deliverables

Each participant or team should produce:

- one deployed environment
- one architecture validation note
- one implemented extension with commit history
- one short retrospective: what was easy, what was risky

## Recommended facilitator checklist

- verify subscription quota and OpenAI access before session
- pre-test azd up in target region
- provide a fallback region in case of model unavailability
- reserve 15 minutes for troubleshooting buffer

## Cleanup

To avoid ongoing cost:

```bash
azd down --purge
```

## Reference material

- main project guide: ../README.md
- cost notes: ./cost.md
- troubleshooting: ./troubleshooting.md
- faq: ./faq.md
