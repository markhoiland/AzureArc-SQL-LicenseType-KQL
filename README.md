# Azure Arc SQL Server License Type - KQL Queries

This repository contains Kusto Query Language (KQL) queries for Azure Resource Graph to help manage and monitor SQL Server extensions deployed on Azure Arc-enabled servers, with a focus on license type configurations.

## Overview

Azure Arc-enabled servers allow you to manage SQL Server instances running on-premises or in other clouds using Azure management tools. One important aspect of SQL Server management is tracking license types (PAYG vs. Bring Your Own License). This repository provides ready-to-use KQL queries to help you:

- Identify which Azure Arc servers have SQL Server extensions deployed
- Monitor license type configurations across your SQL Server fleet
- Find servers using specific license types (e.g., PAYG)

## Files in This Repository

### Query Files

- **query1-arc-servers-with-sql-extensions.kql**
  - Lists all Azure Arc-enabled servers that have SQL Server extensions (Windows or Linux) deployed
  - Returns machine name, machine ID, extension name, extension type, provisioning state, location, subscription, and resource group
  - Useful for getting an inventory of all Arc servers running SQL Server

- **query2-sql-extensions-with-license-type.kql**
  - Lists all SQL Server extensions with their license type settings
  - Returns machine name, extension details, license type, provisioning state, and location information
  - Useful for auditing license configurations across your environment

- **query3-sql-extensions-payg-license.kql**
  - Lists only SQL Server extensions configured with PAYG (Pay-As-You-Go) license type
  - Filters results to show only PAYG-licensed instances
  - Useful for identifying servers using consumption-based licensing

### Documentation Files

- **HOW-TO-RUN-QUERIES.md**
  - Comprehensive guide on different methods to execute the KQL queries
  - Covers Azure Portal (Resource Graph Explorer), Azure CLI, Azure PowerShell, REST API, and Azure SDKs
  - Includes examples, best practices, and tips for working with Azure Resource Graph queries

- **LICENSE**
  - License information for this repository

- **README.md** (this file)
  - Overview and description of all files in the repository

## Quick Start

### Using Azure Portal

1. Sign in to the [Azure Portal](https://portal.azure.com)
2. Search for "Resource Graph Explorer"
3. Copy the contents of any `.kql` file from this repository
4. Paste into the query editor
5. Click "Run query"

### Using Azure CLI

```bash
# Example: List all Arc servers with SQL extensions
az graph query -q "$(cat query1-arc-servers-with-sql-extensions.kql)"
```

### Using Azure PowerShell

```powershell
# Example: List SQL extensions with license type
$query = Get-Content -Path "query2-sql-extensions-with-license-type.kql" -Raw
Search-AzGraph -Query $query
```

## License Types Explained

Azure Arc-enabled SQL Server supports different license types:

- **PAYG (Pay-As-You-Go)**: Consumption-based licensing where you pay for what you use
- **Paid**: Bring Your Own License (BYOL) - use existing SQL Server licenses with Software Assurance
- **LicenseOnly**: License-only instances for development/test scenarios

## Prerequisites

To run these queries, you need:

- An Azure subscription with Azure Arc-enabled servers
- SQL Server extensions deployed on your Arc servers
- Appropriate permissions to read resources (e.g., Reader role or higher)
- One of the following tools:
  - Access to Azure Portal
  - Azure CLI installed and configured
  - Azure PowerShell installed and configured

## Additional Resources

- [Azure Resource Graph Documentation](https://docs.microsoft.com/azure/governance/resource-graph/)
- [Azure Arc-enabled servers Overview](https://docs.microsoft.com/azure/azure-arc/servers/overview)
- [SQL Server on Azure Arc-enabled servers](https://docs.microsoft.com/sql/sql-server/azure-arc/overview)
- [Manage SQL Server license type](https://docs.microsoft.com/sql/sql-server/azure-arc/manage-license-type)

## Contributing

Contributions are welcome! If you have additional queries or improvements, please feel free to submit a pull request.

## Support

For issues or questions:
- Review the [HOW-TO-RUN-QUERIES.md](HOW-TO-RUN-QUERIES.md) documentation
- Check Azure Arc and Resource Graph documentation
- Open an issue in this repository