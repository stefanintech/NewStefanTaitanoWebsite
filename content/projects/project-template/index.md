---
date: '2025-07-21'
draft: true
title: "Project Title"
cover:
    image: https://placecats.com/300/200
    alt: "New alt"
    caption: "New caption"
    hidden: false
    hiddenInSingle: false
summary: "A one-sentence summary of the outcome. e.g., 'Automated instance administration tasks with a custom CLI, reducing manual effort by 50%.'" # Make this results-oriented
ShowReadingTime: false
tags: ["ServiceNow", "REST API", "Python", "Automation"] # Add relevant tech tags
---

### The Objective (The "Problem")
*What was the business problem or personal goal you were trying to solve?*

**Example for your "ServiceNow CLI":**
> As a ServiceNow developer, I frequently needed to perform repetitive administrative tasks like clearing caches, looking up `sys_id`s, and running background scripts. Performing these actions through the native UI was time-consuming and inefficient, requiring multiple clicks and page loads. The goal was to create a tool to streamline my own development workflow.

### The Solution & Technical Details
*How did you solve the problem? What specific technologies and ServiceNow modules did you use?*

**Example:**
> I developed a command-line interface (CLI) using Python and the `requests` library to interact with the ServiceNow Table API and Scripted REST APIs. Key functionalities include:
>
> *   **Authentication:** Securely manages credentials for multiple instances.
> *   **Table API Interaction:** Implemented functions to `get`, `query`, and `update` records from any table.
> *   **Script Execution:** Created a dedicated Scripted REST API endpoint on the ServiceNow side to safely execute server-side scripts passed from the CLI.
> *   **Instance Administration:** Built commands for common actions like `cache.do` and user lookups.

### Code Snippet Showcase
*Include a small, elegant, or interesting block of code that demonstrates your skill.*

**Example:**
> This Python function showcases how I structured the API calls to be reusable for any GET request to the Table API:

```python
def get_record(instance, table, sys_id):
    """Fetches a single record from a specified table."""
    url = f"https://{instance}.service-now.com/api/now/table/{table}/{sys_id}"
    headers = {
        "Accept": "application/json",
        "Authorization": f"Basic {get_auth()}" # get_auth() retrieves credentials
    }
    response = requests.get(url, headers=headers)
    response.raise_for_status() # Raises an exception for bad status codes
    return response.json()