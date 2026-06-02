# Building with Azure Skills | All things Azure
Source: https://devblogs.microsoft.com/all-things-azure/building-with-azure-skills/
Captured: 2026-05-30 | Action: read

## Summary
This post serves as a prompt cookbook for the Azure Skills Plugin, demonstrating how natural language commands trigger specific Azure workflows like deployment, cost optimization, and debugging. It emphasizes automating the full app lifecycle (prepare → validate → deploy) and provides actionable examples for real Azure resources.

## Key Points
- Natural language prompts activate skills (e.g., 'Deploy my Python FastAPI app to Azure Container Apps') without manual infrastructure coding.
- Core workflows automate full deployment pipelines, while skills like `azure-cost-optimization` and `azure-diagnostics` provide actionable insights.
- Prompt specificity (e.g., including resource names) improves accuracy, and chaining prompts builds complex workflows.

## Context & Related Topics
- Azure CLI and Bicep/Terraform for manual infrastructure management
- Azure Resource Graph for resource exploration
- Azure Cost Management for budget tracking
- Copilot for Azure documentation

## Action Items
- [ ] Install Azure Skills Plugin via aka.ms/azure-plugin
- [ ] Test 'Deploy my app to Azure' prompt on a real project
- [ ] Chain two prompts: 'Prepare for Azure' followed by 'Deploy'
