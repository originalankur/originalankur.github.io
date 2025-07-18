---
layout: post
title: >
    Circuit Breakers and Quotas for LLM Workloads
tags: [llms, technology, finops, cost]
description: >
    how to ensure LLM API usage are in check without overengineering a solution.
---

## Circuit Breakers and Quotas for LLM Workloads

LLM APIs can rack up costs fast—especially in background jobs, serverless functions, or cron-based pipelines running as part of CI/CD or due to programmatic errors doing retries. Here's how to keep usage in check without overengineering a solution.

#### Set Budget Alerts and Quotas

Start with basic alert guardrails:

- **AWS**: [Budgets & Alerts](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-create.html)
- **Azure**: Cost Management + action groups
- **GCP**: Budgets + export to BigQuery for custom tracking

These won’t stop usage, but they give early warning.

#### Use Separate Keys for Staging vs Production

Create different API keys with distinct limits:

- **Staging**: Throttled, low-cap keys
- **Production**: Monitored with alerts

This prevents accidental overuse during testing and helps in bifurcation.

#### Create a Billing APIs that can be called programmatically

Use cloud billing APIs to track LLM spend:

- **AWS**: [Cost Explorer](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetCostAndUsage.html)
- **Azure**: [Usage Details](https://learn.microsoft.com/en-us/rest/api/consumption/)
- **GCP**: [BigQuery Billing Export](https://cloud.google.com/billing/docs/how-to/export-data-bigquery)

Against service name once get granular cost usage and return boolean stating if LLM api calls can be made or not.

#### Add Circuit Breakers in Code by calling above API

Check usage every Nth requests or every Nth min elapsed based of your worker being stateless or stateful. Stop LLM usage if api returns false.

LLM calls often run outside of web requests. Add usage checks in:

- **Cold start**: Exit if api returns false aka usage exceeds threshold
- **Every N calls**: API check before triggering the LLM

With these effort, you can avoid budget surprises while keeping your LLM-powered systems running smoothly.