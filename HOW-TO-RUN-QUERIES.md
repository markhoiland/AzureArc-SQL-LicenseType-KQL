# How to Run Azure Resource Graph Queries

This document describes the different methods for running the KQL (Kusto Query Language) queries provided in this repository.

## Overview

Azure Resource Graph queries can be executed using multiple methods, each suited for different scenarios and preferences.

## Methods for Running Queries

### 1. Azure Portal - Resource Graph Explorer

The Azure Portal provides a built-in query editor called Resource Graph Explorer.

**Steps:**
1. Sign in to the [Azure Portal](https://portal.azure.com)
2. Search for "Resource Graph Explorer" in the top search bar
3. Click on "Resource Graph Explorer" from the results
4. Copy and paste the KQL query from any of the `.kql` files in this repository
5. Click "Run query" to execute
6. View results in the table below the query editor
7. Optionally download results as CSV or JSON

**Advantages:**
- User-friendly graphical interface
- No additional tools required
- Easy visualization of results
- Ability to save and share queries

### 2. Azure CLI

The Azure Command-Line Interface (CLI) allows you to run queries from your terminal or command prompt.

**Prerequisites:**
- Install [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
- Sign in using `az login`

**Syntax:**
```bash
az graph query -q "<your-query-here>"
```

**Example:**
```bash
az graph query -q "resources | where type =~ 'microsoft.hybridcompute/machines/extensions' | where properties.type =~ 'WindowsSqlServerExtension' or properties.type =~ 'LinuxSqlServerExtension' | project name, location"
```

**For multi-line queries from files:**
```bash
az graph query -q "$(cat query1-arc-servers-with-sql-extensions.kql)"
```

**Advantages:**
- Automation-friendly
- Can be integrated into scripts
- Supports output formatting (JSON, table, YAML)

### 3. Azure PowerShell

Azure PowerShell provides cmdlets for querying Azure Resource Graph.

**Prerequisites:**
- Install [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)
- Connect using `Connect-AzAccount`

**Syntax:**
```powershell
Search-AzGraph -Query "<your-query-here>"
```

**Example:**
```powershell
$query = Get-Content -Path "query1-arc-servers-with-sql-extensions.kql" -Raw
Search-AzGraph -Query $query
```

**For better formatted output:**
```powershell
$results = Search-AzGraph -Query $query
$results | Format-Table
```

**Advantages:**
- Native PowerShell integration
- Object-based output for easy manipulation
- Ideal for Windows environments

### 4. REST API

You can also call the Azure Resource Graph REST API directly.

**Prerequisites:**
- Azure AD authentication token

**Endpoint:**
```
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resources?api-version=2021-03-01
```

**Request Body:**
```json
{
  "subscriptions": ["<subscription-id>"],
  "query": "<your-kql-query>"
}
```

**Advantages:**
- Language-agnostic
- Can be integrated into any application
- Full programmatic control

### 5. Azure SDKs

Azure provides SDKs for various programming languages (Python, .NET, Java, JavaScript, etc.)

**Example (Python):**
```python
from azure.identity import DefaultAzureCredential
from azure.mgmt.resourcegraph import ResourceGraphClient
from azure.mgmt.resourcegraph.models import QueryRequest

credential = DefaultAzureCredential()
client = ResourceGraphClient(credential)

query = QueryRequest(
    subscriptions=["<subscription-id>"],
    query="<your-kql-query>"
)

response = client.resources(query)
```

**Advantages:**
- Full application integration
- Type-safe (depending on language)
- Asynchronous support

## Query Scope

### Single Subscription
All methods support querying a single subscription by default or can be explicitly specified.

### Multiple Subscriptions
You can query across multiple subscriptions by specifying them in the request:

**Azure CLI:**
```bash
az graph query -q "<query>" --subscriptions <sub-id-1> <sub-id-2>
```

**PowerShell:**
```powershell
Search-AzGraph -Query "<query>" -Subscription @("sub-id-1", "sub-id-2")
```

### Management Groups
Query across all subscriptions in a management group:

**Azure CLI:**
```bash
az graph query -q "<query>" --management-groups <mg-id>
```

**PowerShell:**
```powershell
Search-AzGraph -Query "<query>" -ManagementGroup "mg-id"
```

## Output Formatting

### Azure CLI Output Formats
```bash
# Table format (default)
az graph query -q "<query>" -o table

# JSON format
az graph query -q "<query>" -o json

# YAML format
az graph query -q "<query>" -o yaml
```

### PowerShell Output Formats
```powershell
# Format as table
Search-AzGraph -Query "<query>" | Format-Table

# Format as list
Search-AzGraph -Query "<query>" | Format-List

# Export to CSV
Search-AzGraph -Query "<query>" | Export-Csv -Path "results.csv"
```

## Best Practices

1. **Start Small**: Test queries on a single subscription before expanding to management groups
2. **Use Variables**: Store complex queries in variables or files for reusability
3. **Pagination**: Be aware that Resource Graph returns results in pages (default 100 rows, max 1000)
4. **Error Handling**: Implement proper error handling in scripts and applications
5. **Query Optimization**: Use filters early in the query to improve performance
6. **Save Queries**: Save frequently used queries in the Azure Portal for quick access

## Additional Resources

- [Azure Resource Graph Documentation](https://docs.microsoft.com/azure/governance/resource-graph/)
- [KQL Quick Reference](https://docs.microsoft.com/azure/data-explorer/kql-quick-reference)
- [Azure Arc-enabled servers Documentation](https://docs.microsoft.com/azure/azure-arc/servers/)
- [SQL Server on Azure Arc-enabled servers](https://docs.microsoft.com/sql/sql-server/azure-arc/overview)
