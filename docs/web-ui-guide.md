# Web UI guide for Azure Static Web Apps

This guide explains how to use and validate the deployed web UI hosted on Azure Static Web Apps.

## Scope

Use this guide after `azd up` completes successfully.

Expected result:

- the web UI is reachable
- sign-in works
- chat requests reach the API and return responses

## Step 1: Get the web app URL

From your repository root:

```bash
azd env get-values | grep SERVICE_WEBAPP_URI
```

Copy the URL value and open it in your browser.

## Step 2: Confirm the page is served by Static Web Apps

On first load, verify:

- the main chat UI is visible
- static assets load without errors
- no browser certificate warnings

If the page does not load:

- confirm deployment finished without errors
- confirm the resource exists in Azure Portal

## Step 3: Sign in

1. Click the sign-in control in the UI.
2. Select an available identity provider.
3. Complete authentication.
4. Return to the app and confirm the signed-in state is shown.

If sign-in loops or fails:

- verify your auth provider configuration in Static Web Apps
- check redirect URI settings

## Step 4: Send a test prompt

Use a basic prompt first:

- `Explain this solution in three bullets.`

Expected behavior:

- your message appears in the chat thread
- a response returns from the backend and renders in the UI

## Step 5: Verify protected access

1. Open a private/incognito browser window.
2. Visit the same URL.
3. Confirm the app requests authentication before chat use.

This validates Easy Auth protection from the user perspective.

## Step 6: Optional checks for troubleshooting

If UI works but chat fails:

- confirm API service deployment succeeded
- confirm required environment values are present in `packages/api/.env` after postprovision
- check Azure Portal logs for the Function App

## UI architecture summary

- Hosting: Azure Static Web Apps
- Authentication: Static Web Apps Easy Auth
- Chat backend: Azure Functions linked API route
- Model access: backend uses Azure identity-based access to Azure OpenAI

## Workshop tip

For group sessions, run these steps as a checklist and capture screenshots after Step 3 and Step 4 as evidence of successful setup.
