# SignNow SmolAgents Tools

A collection of tools and agents for integrating with the [SignNow](https://www.signnow.com/) API using the [SmolAgents](https://github.com/smol-ai/smol-agents) framework.

## Features

This package provides:

- **Agents with authorization** using login/password and app key
- **Tool collection** (`ToolCollection`) for interacting with SignNow’s core API:
  - Upload local documents
  - Retrieve templates
  - Create documents from templates
  - Get detailed document information
  - Fetch contacts
  - Send invites for signing

You can use:
- A full agent (`SignNowSmolagentsCodeAgent` or `SignNowSmolagentsToolCallingAgent`)
- Just the tool collection (`SignNowSmolagentsToolset`)
- Or individual tools separately

## Installation

```bash
pip install -r requirements.txt
```

## Usage Example

### Get Access Token

```python
from sn_smolagent_tools_demo import access_token_from_login_password

token = access_token_from_login_password("your_login", "your_password", "your_app_key")
```

### Using the Agent

```python
from sn_smolagent_tools_demo import SignNowSmolagentsCodeAgent

agent = SignNowSmolagentsCodeAgent(access_token=token)
result = agent.run("Upload the file 'contract.pdf' and send it for signature to John Smith")
```

### Using the Tool Collection

```python
from sn_smolagent_tools_demo import SignNowSmolagentsToolset

tools = SignNowSmolagentsToolset(access_token=token).tools
for tool in tools:
    print(tool.name)
```

### Creating a Custom Agent with Individual Tools

```python
from sn_smolagent_tools_demo import (
    GetTemplateListTool,
    UploadLocalFileTool,
    GetContactsTool,
    CreateDocumentFromTemplateTool,
    GetDocumentInfoTool,
    SendInviteTool
)
from smolagents import CodeAgent

# Initialize tools individually
get_templates_list = GetTemplateListTool(access_token=token)
upload_document = UploadLocalFileTool(access_token=token)
get_contacts = GetContactsTool(access_token=token)
create_document_from_template = CreateDocumentFromTemplateTool(access_token=token)
get_document_info = GetDocumentInfoTool(access_token=token)
send_invite = SendInviteTool(access_token=token)

# Create custom agent with selected tools
manager_agent = CodeAgent(
    model=model,
    tools=[
        upload_document,
        get_contacts,
        get_document_info,
        send_invite,
        create_document_from_template,
        get_templates_list,
    ],
    name='signnow_agent',
    description=(
        'This agent integrates with the SignNow API to handle electronic signatures.'
        ' It can send documents for signing, retrieve their signing status, manage templates, and coordinate contacts—all within a streamlined workflow.'
    ),
    managed_agents=[],
    planning_interval=10,
    verbosity_level=2,
    max_steps=20,
)
```

### Integrating the SignNow Agent as a Managed Agent

You can include the SignNow agent in your workflow as a managed agent to seamlessly add e-signature functionality to any agent you build:

```python
from sn_smolagent_tools_demo import SignNowSmolagentsCodeAgent
from smolagents import CodeAgent

sn_agent = SignNowSmolagentsCodeAgent(access_token=token)

your_agent = CodeAgent(
    model=model,
    tools=[
        your_tool_1,  # replace with your own tools
        your_tool_2,
    ],
    name='your_agent',
    managed_agents=[sn_agent],  # integrate SignNow capabilities
    planning_interval=10,
    verbosity_level=2,
    max_steps=20,
)
```

## Available Tools

- `get_template_list`
- `upload_local_file_to_signnow`
- `get_contacts_from_signnow`
- `create_document_from_template`
- `get_document_info`
- `send_invite`

## TODO

- Add wrappers for more SignNow APIs
- Add logging and debugging features
- Improve error handling and retries

## License

MIT License

