# OFJDBC Claude MCP Server

A Claude Desktop MCP server that provides Oracle Fusion metadata context through OFJDBC and OFARROW integration.

## Overview

This server transforms Claude AI from a syntax helper into a documentation-grounded Oracle expert — it retrieves verified Oracle Fusion metadata from your local DuckDB cache, enabling Claude to reason using the same structural context and reference logic as the actual Fusion application, not just correct SQL syntax.

## Security & Privacy

🔒 **Complete Security Isolation**
- **No Database Access**: This MCP server has **zero** access to your Oracle Fusion database
- **No SQL Results**: Cannot see or access any actual data from your SQL query results
- **No Credentials Required**: Does not need or store any Oracle Fusion login credentials
- **Metadata Only**: Works exclusively with locally cached table/column/index metadata
- **Offline Operation**: Functions independently - DBeaver and OFJDBC don't need to be running

Your sensitive data remains completely secure as Claude only sees database structure information, never the actual content.

### How It Works

1. **Effortless Metadata Capture:** As you browse tables or run SELECTs in DBeaver, OFJDBC seamlessly collects rich metadata on every table and view you touch—no extra steps required.
2. **Private, Local Storage:** All metadata is securely stored in your `.ofjdbc/metadata.db` DuckDB file, right on your machine. No cloud. No outside access. Your privacy is fully protected.
3. **Smart Context Injection:** This MCP server injects that metadata directly into Claude, giving it real-time, schema-aware insights into your unique Oracle Fusion environment.
4. **Continuous, Organic Learning:** The more you explore, the deeper Claude’s knowledge grows—metadata expands automatically as you work, powering ever-smarter assistance.

💡 **Want to skip the warm-up?**  
Message me on [LinkedIn](https://www.linkedin.com/in/sergeyrudenko) — I can share a prebuilt metadata database containing the full Oracle Fusion schema to get you started instantly.

### Important Notes

- **Initial Setup**: Only table names and descriptions are saved when you first use OFJDBC in DBeaver
- **Column Information**: Table columns are saved only when you:
   - Write SELECT statements using those tables
   - Manually open tables in the database explorer (left pane)
- **Index Information**: Index details are saved only when you open them in the database explorer
- **Scope**: Claude AI can only access locally stored metadata

### See It In Action

🎥 **Quick Demo**: Watch a [short video demonstration](https://www.youtube.com/watch?v=acgyR3kB6s8) to see the OFJDBC Claude MCP server in action.



🎥 **Longer Demo**: Watch a [video demonstration](https://youtu.be/pALBDmEnCm4?si=8wlEohutvBmzNbLG) to see the OFJDBC Claude MCP server in action.

### Simplified version in OpenAI ChatGPT [Oracle Fusion Technical Consultant](https://chatgpt.com/g/g-68cbf632f2288191a3b97833626b792e-oracle-fusion-technical-consultant)

## Prerequisites

- **Claude Desktop**: Installed and running
- **OFJDBC**: Already installed and working ([GitHub Repository](https://github.com/krokozyab/ofjdbc))

## Installation

### Installation Guides

- **Windows:** See [docs/install_windows.md](docs/install_windows.md)
- **macOS:** See [docs/install_macos.md](docs/install_macos.md)

## Metadata Database Structure

The local DuckDB database contains three main tables that store your Oracle Fusion metadata:

### CACHED_TABLES
Stores basic information about database tables and views:
- **Schema and Table Information**: Schema name, table name, and table type
- **Documentation**: Table remarks and descriptions
- **Categorization**: Table catalog and type information
- **Unique Identification**: Combination of schema and table name serves as the primary key

### CACHED_COLUMNS
Contains detailed column information for each table:
- **Column Details**: Column name, data type, size, and precision information
- **Position and Constraints**: Ordinal position within the table and nullable constraints
- **Documentation**: Column-level remarks and descriptions
- **Relationships**: Links to parent tables via schema and table name

### CACHED_INDEXES
Stores index information for performance optimization:
- **Index Properties**: Index name, type, and uniqueness information
- **Column Mapping**: Which columns are included in each index and their order
- **Performance Metadata**: Cardinality and page information for query optimization
- **Relationships**: Links indexes to their corresponding tables and columns

### Working with Metadata

When working with Claude, you can reference these table structures to:
- Ask specific questions about table schemas or column types
- Request queries that leverage existing indexes
- Understand relationships between different database objects
- Get help with performance optimization based on available indexes

## Usage

Once installed, Claude AI will have access to your Oracle Fusion metadata and can assist with:

- Writing and optimizing SQL queries
- Understanding table relationships
- Exploring database schema
- Providing context-aware database assistance

The more you use OFJDBC in DBeaver, the more comprehensive Claude's knowledge of your database becomes.

## 🎯 Optimal Prompting Guide

This guide shows you how to effectively use each MCP tool with Claude to get the most accurate and helpful responses.

### 📋 Available Tools Overview
You rarely need to call these tools manually—describe your intent in natural language (“show me tables for invoices”, “why does this SQL fail?”) and Claude will invoke the appropriate tool automatically. Direct tool calls remain available for scripted workflows or debugging.

| Tool                      | What It Does                                                                 | Use It When...                                                                 |
|---------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| `list_tables`             | Lists tables/views with schema, type, and remarks                            | You need a quick inventory of objects in a schema                             |
| `list_columns`            | Returns ordered column metadata (type, nullable, remarks)                    | Designing queries or assessing table shape                                    |
| `index_info`              | Groups index definitions with column order and uniqueness                     | Planning joins/filters or validating performance assumptions                  |
| `table_overview`          | Summarises a table (module context, key columns, inferred relationships)      | Getting a one-shot briefing on an unfamiliar table                            |
| `module_summary`          | Highlights module-prefixed tables, joins, notable fields, sample SQL          | Scoping work around `AP_`, `GL_`, or other functional prefixes                |
| `list_business_processes` | Lists built-in and custom process codes with friendly titles                  | Advertising available workflows to Claude / picking the right process code    |
| `business_process_map`    | Breaks a process into stages, key tables, and reports                         | Following procure-to-pay/order-to-cash-style workflows                        |
| `search_identifiers`      | Fuzzy identifier search with scoring and size-aware limits                    | Hunting tables/columns when names are partial or typo-prone                   |
| `semantic_search`         | Business-term search with synonyms, module/process awareness                  | Looking for tables via layperson terms ("invoice aging", "hire-to-retire")     |
| `search_descriptions`     | Full-text search over table/column remarks with phrase support                | Exploring documentation or business definitions stored in metadata            |
| `raw_select`              | Executes read-only SQL (enforces SELECT + limits) with branded result footer  | Running ad-hoc metadata queries (`SELECT * FROM CACHED_COLUMNS WHERE ...`)     |
| `lint_sql`                | Performs read-only analysis and returns diagnostics (no automatic changes)    | Validating SQL snippets before executing them elsewhere                       |
| `fix_sql`                 | Applies limited auto-fixes (e.g., rewrites `LIMIT` to Oracle syntax)          | Quickly patching common Oracle syntax mismatches after linting                |
| `suggest_sql`             | Provides context-aware skeleton queries and next-action hints                 | Accelerating reporting queries or exploring a dataset                         |

---

### 🔍 Discovery & Exploration

#### Finding Relevant Tables

**❌ Ineffective prompting:**
> "Show me all tables"

**✅ Better prompting:**
> "I need to analyze customer data. Can you list tables that might contain customer information? Look for tables with 'CUSTOMER', 'CLIENT', or 'USER' in their names, and show me the first 10 with their descriptions."

**✅ Even better - specific business context:**
> "I'm working on a sales report. Please find tables related to sales transactions, customer orders, or revenue. Focus on tables from the FUSION schema and limit to 15 results."

#### Understanding Table Structure

**❌ Ineffective:**
> "What columns does this table have?"

**✅ Better:**
> "I need to understand the PO_HEADERS_ALL table structure. Show me all columns with their data types, and highlight any that seem to be foreign keys or dates."

**✅ Contextual approach:**
> "For building a purchase order analysis, show me the columns in PO_HEADERS_ALL. I'm particularly interested in date fields, amount fields, and any status indicators."

---

### 🔎 Targeted Search Strategies

#### Using search_identifiers Effectively

**❌ Too broad:**
> "Find anything with 'order'"

**✅ Strategic search:**
> "Search for database objects containing 'invoice' - I need to find all invoice-related tables and columns for an accounts receivable analysis."

**✅ Multi-step discovery:**
> "First, search for 'GL_' objects to find General Ledger tables, then search for 'ACCOUNT' to find accounting-related objects. I'm building a financial reporting solution."

#### Pattern-Based Discovery

**✅ Effective patterns:**
```
"Search for identifiers matching these patterns:
- 'AP_' for Accounts Payable objects
- 'AR_' for Accounts Receivable objects
- 'INVOICE' for invoice-related tables
Limit results to 20 most relevant matches."
```

---

### 📊 Data Analysis & Queries

#### Planning Queries with Metadata

**✅ Comprehensive approach:**
> "I want to analyze customer purchase patterns. First, help me find customer and order tables, then show me their relationships through foreign keys. Finally, suggest a query to get top customers by purchase volume."

#### Smart Query Building

**❌ Direct query without context:**
> "Write a query for sales data"

**✅ Metadata-driven approach:**
> "I need sales data for Q4 2024. Can you:
> 1. Find tables containing sales/order information
> 2. Show me the date and amount columns available
> 3. Check for any indexes on date fields
> 4. Then write an optimized query for Q4 sales totals by month"

#### Query Validation Workflow

**✅ Best practice sequence:**
```
1. "Here's my query: [SQL]. Please lint it to check for syntax issues."
2. "If there are issues, use fix_sql to correct them."
3. "Finally, suggest any optimizations based on the available indexes."
```

---

### 🔧 SQL Quality & Validation

#### SQL Linting

**✅ Proactive validation:**
> "Before I run this complex query, can you lint it to check for any syntax errors or potential issues: [YOUR_SQL_QUERY]"

**✅ Learning-focused approach:**
> "I'm learning Oracle SQL syntax. Please lint this query and explain any issues you find: SELECT * FROM po_headers WHERE creation_date > '2024-01-01'"

#### SQL Fixing

**✅ Auto-correction with explanation:**
> "This query is giving me syntax errors: [BROKEN_SQL]. Please use fix_sql to correct it and explain what was wrong."

**✅ Best practices enhancement:**
> "Fix this query and suggest improvements for better performance: SELECT * FROM large_table WHERE some_column LIKE '%search%'"

#### SQL Suggestions

**✅ Completion assistance:**
> "I'm writing a query to find customers with overdue invoices. I have 'SELECT c.customer_name FROM hz_customers c WHERE'. Can you suggest how to complete this with proper joins and conditions?"

**✅ Optimization guidance:**
> "Suggest improvements for this query to make it more efficient and follow Oracle best practices: [YOUR_QUERY]"

---

### 🎯 Schema-Specific Prompting

#### Oracle Fusion Context

**✅ Leverage business knowledge:**
> "I'm analyzing the Order-to-Cash process in Oracle Fusion. Find tables related to:
> - Sales orders (OM_ prefix)
> - Accounts receivable (RA_, AR_ prefixes)
> - Customer data (HZ_ prefix)
    > Show relationships between these table groups."

#### Multi-Schema Analysis

**✅ Organized exploration:**
> "Compare the table structures between FUSION and APPS schemas for supplier data. Look for tables with 'SUPPLIER' or 'VENDOR' in their names, and highlight any differences in column structures."

---

### 🚀 Performance-Focused Prompting

#### Index-Aware Query Design

**✅ Performance-conscious approach:**
> "I need to query large transaction tables. First show me the indexes available on AP_INVOICES_ALL, then suggest an optimized query to find invoices from the last 30 days, using the best available index."

#### Efficient Data Exploration

**✅ Size-aware discovery:**
> "Find the largest tables in the database (by estimated row count from metadata), then show me their key columns and indexes. I need to design efficient queries for reporting."

---

### 🔧 Advanced Techniques

#### Combining Multiple Tools

**✅ Workflow-based prompting:**
> "I'm designing a financial dashboard. Please:
> 1. Search for GL (General Ledger) related tables
> 2. Show detailed columns for the 3 most relevant GL tables
> 3. Check available indexes for query optimization
> 4. Suggest efficient queries for account balance summaries
> 5. Lint and optimize the final queries"

#### Business Process Mapping

**✅ Process-oriented discovery:**
> "Map the Procure-to-Pay process tables:
> 1. Find purchase requisition tables (PO_REQUISITION%)
> 2. Find purchase order tables (PO_HEADERS%, PO_LINES%)
> 3. Find invoice tables (AP_INVOICES%, AP_INVOICE_LINES%)
> 4. Find payment tables (AP_CHECKS%, IBY_PAYMENTS%)
     > Show the relationships between these table groups."

---

### 💡 Pro Tips for Better Results

#### 1. **Be Specific About Your Goal**
Instead of: *"Show me customer data"*
Use: *"I need customer data for churn analysis - find tables with customer demographics, transaction history, and account status information."*

#### 2. **Leverage Business Context**
Instead of: *"Find order tables"*
Use: *"I'm analyzing the Order-to-Cash cycle - find tables for sales orders, fulfillment, invoicing, and collections."*

#### 3. **Use Iterative Discovery**
```
Step 1: "Search for 'CUSTOMER' objects to find customer-related tables"
Step 2: "Show me detailed columns for HZ_CUSTOMERS - focus on date fields and status indicators"
Step 3: "What indexes are available on HZ_CUSTOMERS for customer_id and creation_date?"
Step 4: "Write an optimized query to find customers created in the last quarter"
```

#### 4. **Combine Schema Knowledge with Tools**
> "I know Oracle Fusion uses HZ_ prefix for customer data, RA_ for receivables, and PO_ for purchasing. Find the main tables for each of these modules and show their relationships."

#### 5. **Ask for Explanations**
> "After showing me the table structure, explain what each key column represents in the business context and suggest what kinds of analyses would be most valuable."

---

### 🎨 Prompting Templates

#### Discovery Template
```
"I'm working on [BUSINESS_OBJECTIVE].
Please find tables related to [FUNCTIONAL_AREA] by:
1. Searching for objects containing '[KEY_TERMS]'
2. Showing detailed structure of the top [N] most relevant tables
3. Highlighting [SPECIFIC_COLUMN_TYPES] columns
4. Suggesting relationships between these tables"
```

#### Analysis Template
```
"For [ANALYSIS_TYPE], I need to:
1. Find tables containing [DATA_TYPES]
2. Check available indexes for performance
3. Build an optimized query for [SPECIFIC_REQUIREMENT]
4. Validate and fix any SQL issues
```

#### Exploration Template
```
"Help me understand the [BUSINESS_PROCESS] in this database:
1. Map the main tables involved in this process
2. Show the data flow between these tables
3. Identify key metrics I can extract
4. Suggest efficient queries for common reporting needs"
```


## Support

For issues related to:
- **OFJDBC**: Visit the [OFJDBC GitHub repository](https://github.com/krokozyab/ofjdbc)
- **This MCP Server**: Create an issue in this repository
