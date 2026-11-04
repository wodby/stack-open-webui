# Open WebUI application stack for Kubernetes on Wodby

Deploy Open WebUI, a self-hosted chat interface for AI models, on Kubernetes
with Wodby.

This repository defines the Wodby stack manifest and its default service
composition.

- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Open WebUI service](https://github.com/wodby/service-open-webui)
- [PostgreSQL service](https://github.com/wodby/service-postgres)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Open WebUI<br>`open-webui` | required; main service; links: `postgres` → `postgres`; 10 GB data volume |
| PostgreSQL<br>`postgres` | required; 10 GB data volume |

Open WebUI does not run models itself. It connects to model APIs.

## Deploy this stack

1. Set the **Admin email** and **Admin password** settings of the Open WebUI
   service. The administrator account is created on the first start, and
   public sign-up is turned off. Without it, the first visitor would become
   the administrator, so both settings are required.
2. Deploy, open the app's domain, and sign in with those credentials.
3. Add a model connection in **Admin Panel → Settings → Connections**: an API
   key for a hosted provider, or the address of a self-hosted
   OpenAI-compatible server.
4. Invite users in **Admin Panel → Users**, or turn sign-up back on there.

In a custom stack, the Open WebUI service's optional `openai` link connects it
to an OpenAI-compatible service such as [vLLM](https://github.com/wodby/service-vllm),
including its API key. The link takes effect only on the first start, because
Open WebUI keeps the connections it saved then; on an existing app, add the
connection in Open WebUI instead.

## Operational scope

This stack runs one Open WebUI replica. Several replicas would need pgvector,
Redis, and storage shared between replicas, which this stack does not set up.

Users, chats, and settings are in PostgreSQL. Uploaded files and the document
search index are on the Open WebUI volume. Back up both together.

## Maintain a custom version

1. Fork this repository.
2. Edit `stack.yml`.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target.
Stack-local names and referenced service names are distinct identifiers.

Validate the manifest with:

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

See the [stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)
and the [managed services index](https://github.com/wodby/services).
