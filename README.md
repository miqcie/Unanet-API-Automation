# Unanet API Automation

## Description
This project provides tools and scripts to automate interactions with Unanet's API, enabling seamless data extraction, authentication, and report generation. Designed for teams looking to integrate Unanet with Microsoft 365, Pipedrive, and other business systems, this repository offers reusable Python and PowerShell scripts for automation.

## Overview
This repository provides tools and scripts to automate interactions with Unanet's API, including authentication, data retrieval, and report generation. The goal is to streamline operations for businesses using Unanet and improve integration with other systems like Microsoft 365 and Pipedrive.

## Features
- **API Authentication**: Secure token-based authentication to Unanet.
- **Automated Data Extraction**: Retrieve project data, time entries, and reports.
- **Report Generation**: Use Python and OpenAI to summarize timecard comments into structured Monthly Activity Reports.
- **Integration with Microsoft 365**: Automate file handling in SharePoint and OneDrive.
- **Error Handling & Logging**: Built-in retry logic for handling Unanet’s API inconsistencies.

## Getting Started
### Prerequisites
1. **Python 3.8+**
2. **Homebrew (for macOS users)**
3. **Required Python Libraries**
   ```bash
   pip install requests aiopenapi3 httpx pandas python-docx tenacity dotenv
   ```

### Authentication
To interact with Unanet’s API, you need to authenticate using an API token.

#### Retrieve API Token:
```python
import requests

username = 'your_username'
password = 'your_password'

response = requests.post('https://deepwaterpoint.unanet.biz/platform/rest/login', json={
    'username': username,
    'password': password
})

if response.status_code == 200:
    token = response.json().get('authenticationToken')
    print(f'Your API Token: {token}')
else:
    print(f'Authentication failed: {response.text}')
```

## API Automation Tools
### Using `aiopenapi3` to Explore Unanet's API
This script dynamically loads Unanet’s API spec and fetches available endpoints.

```python
import asyncio
from aiopenapi3 import OpenAPI

async def main():
    spec_url = "https://deepwaterpoint.unanet.biz/platform/swagger/"
    async with OpenAPI.load(spec_url) as api:
        print("Available API endpoints:")
        for path in api.paths.keys():
            print(path)

asyncio.run(main())
```

### OpenAPI Generator CLI (Generating SDK)
If Unanet's API is well-documented, you can generate an SDK using OpenAPI Generator CLI.

#### Install:
```bash
brew install openapi-generator-cli
```

#### Generate a Python Client SDK:
```bash
openapi-generator-cli generate -i https://deepwaterpoint.unanet.biz/platform/swagger/ -g python -o unanet_sdk
```

#### Use the Generated SDK:
```python
from unanet_sdk import ApiClient, Configuration

config = Configuration()
config.api_key['Authorization'] = "Bearer YOUR_ACCESS_TOKEN"

client = ApiClient(config)
response = client.call_api('/projects', 'GET')
print(response)
```

## Power Automate Integration
For users who want to automate file management in SharePoint, Power Automate flows can:
- **Move generated reports** to project lead folders.
- **Trigger scripts** when a new file is uploaded.

Example Power Automate Actions:
1. Watch a SharePoint folder for new CSV uploads.
2. Call an **HTTP Request to a Python API** to generate reports.
3. Save the output to SharePoint and notify users via Teams.

## Contributing
1. Fork this repository.
2. Create a feature branch (`git checkout -b feature-xyz`).
3. Commit your changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature-xyz`).
5. Open a Pull Request.

## Roadmap
- [ ] Improve API documentation parsing for Unanet
- [ ] Add real-time logging for API requests
- [ ] Enhance error-handling for API failures
- [ ] Power Automate templates for SharePoint integration
- [ ] Open-source a CLI tool for Unanet API users

## License
MIT License - Free to use and modify.

