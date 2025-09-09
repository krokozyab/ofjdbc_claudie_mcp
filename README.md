# ofjdbc_claudie_mcp
Claude Desktop MCP server supporting OFJDBC and OFARROW

Prerequisitions
Claudie desktop.
You already have OFJDBC (https://github.com/krokozyab/ofjdbc) installed and worked.

Installation
Windows
1. Download ofmcp.exe and db_worker_safe.exe from github release into designated folder on you local pc.

Open Claude desktop.

2. pic 1

3. pic 2

4. Edit claude_desktop_config.json

{
"mcpServers": {
"Fusion Metadata": {
"command": "C:\\Users\\<you_username>\\<folder from step 1>\\of_mcp\\ofmcp.exe",
"args": ["--db","C:\\Users\\<you_username>\\.oflight\\metadata.db","--mode","stdio"]
}
}
}

5. Save and restart Claude desktop (Click on 'burger' in top left corner then File -> Exit).

You should see.

pic 3









