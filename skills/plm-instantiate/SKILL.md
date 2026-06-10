---
name: plm-instantiate
description: "Create a new PLM for a product line, bind it to a repo, and optionally delegate commit tokens to other agents. Use when spinning up a new product line."
---

# PLM Instantiate

Create a new Product Line Manager (PLM) for a product line, bind it to a GitHub repo, and manage token delegation for agents that need to commit to that repo.

## Architecture

Each product line gets:
- A **PLM role** owned by the CSO (reports up: PLM → CSO → CEO)
- A **GitHub repo** tied exclusively to that PLM
- A **repo auth token** owned by the PLM (GitHub PAT scoped to the repo)
- Optionally, **delegated tokens** for agents that need to commit (COO, Engineering, etc.)

## Data File

Store PLM instances in `<instance>/plm-instances.json`:

```json
{
  "product_lines": [
    {
      "name": "<Product>",
      "plm_role": "PLM-<Product>",
      "CSO": "CSO",
      "repo_url": "https://github.com/<org>/<repo>",
      "repo_name": "<repo>",
      "created_date": "<date>",
      "token_delegations": [
        {
          "agent_role": "Engineering",
          "access": "read-write",
          "delegated_by": "PLM-<Product>",
          "delegated_date": "<date>"
        }
      ]
    }
  ]
}
```

## Workflow — Create New PLM

### 1. Collect Input
Ask the user for:
- **Product line name**
- **Repo URL**
- **Delegations**: which roles need commit access and at what level (read-only, read-write, admin)

### 2. Create PLM Record
1. Read `<instance>/plm-instances.json` (create if missing)
2. Check that the product line name is unique
3. Create the PLM entry

### 3. Write PLM Workflow File
Create `<instance>/workflows/plm/<product>/workflow.md` using the PLM template at `abe/workflows/cos/cso/plm-template.md`, customized for the product line.

### 4. Create PLM Token Record
Instruct the user to generate a GitHub PAT scoped to the repo with `contents`, `pull-requests`, `workflows` scopes.

### 5. Create Delegated Tokens (if requested)
For each delegation, create a fine-grained GitHub PAT with appropriate scopes.

### 6. Update Org Structure
- Add the new PLM to the instance's workflow directory
- Update the instance's `all_agents/README.md` and `agent-roster.md`

### 7. Confirm
Summarize what was created.

## Workflow — Request Token (Agent)

When a non-PLM agent needs to commit to a product line repo:
1. Agent identifies the product line
2. Look up the PLM in `plm-instances.json`
3. Check if a delegation already exists for the requesting role
4. If yes: provide the existing delegated token location
5. If no: request that the PLM create a new delegation (requires CSO approval)

## Gitignore

Ensure `<instance>/.plm-tokens/` is in `.gitignore`:
```
# PLM tokens — never commit
.plm-tokens/
```
