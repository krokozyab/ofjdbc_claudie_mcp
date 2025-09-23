# Privacy Policy for Oracle Fusion Metadata GPT

_Last updated: September 2025_

## Overview
This Custom GPT (“Oracle Fusion Metadata GPT”) provides read-only access to **metadata** from Oracle Fusion Applications via the open-source [ofjdbc](https://github.com/krokozyab/ofjdbc) and related projects.  
It is designed for **development, metadata exploration, and AI assistant integration**.

## Data Handling
- **No Personal Data Collected**: The GPT does not request, collect, or store personal information about users.
- **Metadata Only**: The system exposes **metadata (tables, columns, indexes, relationships)** only. No transactional or confidential business data is accessed.
- **No Persistence**: Queries are handled in memory against cached metadata. Results are not logged, stored, or shared.
- **No Third-Party Sharing**: Data from user queries is not sold, rented, or shared with third parties.

## Security
- The underlying server (`ofmcp_http` or similar) requires an API key and runs on user-controlled infrastructure.
- Users remain in full control of their own data sources and environments.
- The project is provided **as-is**, without warranties, and intended for safe experimentation with Oracle Fusion metadata.

## Contact
For questions, issues, or concerns about this Privacy Policy, please contact:  
👉 [GitHub Issues – krokozyab/ofjdbc](https://github.com/krokozyab/ofjdbc/issues)

---

By using this GPT, you acknowledge and agree that it is a **metadata-only tool** and does not process or retain personal or sensitive information.