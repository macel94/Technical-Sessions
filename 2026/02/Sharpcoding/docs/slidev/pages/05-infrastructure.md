---
layout: two-cols
class: text-sm
transition: slide-left
---

# IaC with Bicep

- Event Grid **system topic** sourced from Key Vault
- Delivery tuned for reliability: 3 attempts, 24h TTL

```bicep {*}{maxHeight:'280px'}
param includedEventTypes array = [
	'Microsoft.KeyVault.SecretNewVersionCreated'
	'Microsoft.KeyVault.SecretNearExpiry'
	'Microsoft.KeyVault.SecretExpired'
]

properties: {
	eventDeliverySchema: 'CloudEventSchemaV1_0'
	retryPolicy: {
		maxDeliveryAttempts: 3
		eventTimeToLiveInMinutes: 1440
	}
}
```

::right::

<!-- meme image here -->

---
layout: two-cols
class: text-sm
transition: slide-left
---

# CI/CD pipeline

- Onboarding workflow generates parameter files and opens a PR per client
- **What-if** validates infra changes before any deployment
- **Deploy** wires Event Grid subscriptions only after what-if passes

```yaml {*}{maxHeight:'300px'}
what_if:
  needs: create_pr
  environment: production
  steps:
    - uses: azure/login@v2
      with:
        client-id: ${{ secrets.AZURE_CLIENT_ID }}
        tenant-id: ${{ secrets.AZURE_TENANT_ID }}
        subscription-id: ${{ inputs.subscriptionId }}
    - name: What-if deployment
      run: |
        az deployment sub what-if \
          --subscription "${{ inputs.subscriptionId }}" \
          --location "${{ inputs.location }}" \
          --template-file infra/main.subscription.bicep \
          --parameters @infra/parameters/kv-eg-sub-${{ needs.create_pr.outputs.short_sub }}.parameters.json

deploy:
  needs: [create_pr, what_if]
  environment: production
  steps:
    - uses: azure/login@v2
      with:
        client-id: ${{ secrets.AZURE_CLIENT_ID }}
        tenant-id: ${{ secrets.AZURE_TENANT_ID }}
        subscription-id: ${{ inputs.subscriptionId }}
    - name: Deploy Event Grid system topic
      run: |
        az deployment sub create \
          --subscription "${{ inputs.subscriptionId }}" \
          --location "${{ inputs.location }}" \
          --template-file infra/main.subscription.bicep \
          --parameters @infra/parameters/kv-eg-sub-${{ needs.create_pr.outputs.short_sub }}.parameters.json
```

::right::

<!-- meme image here -->

---
layout: two-cols
---

# GitHub as force multiplier

### Delivery and platform workflows

- GitHub Actions standardizes build, test, publish, and deployment gates
- Onboarding workflows generate repeatable infra changes per client
- Pull requests become a shared operating model across teams

### Dependency hygiene

- Dependabot keeps NuGet and action versions current
- Security and maintenance updates are visible, reviewable, and trackable

::right::

# GitHub Copilot impact

- Speeds up implementation drafts and refactors
- Helps produce clearer docs and runbooks while coding
- Accelerates this Slidev presentation creation and iteration
- Frees engineering time for architecture and reliability decisions
