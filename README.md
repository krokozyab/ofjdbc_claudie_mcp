# ofjdbc_claudie_mcp
Claude Desktop MCP server supporting OFJDBC and OFARROW

Prerequisitions
Claudie desktop.
You already have OFJDBC (https://github.com/krokozyab/ofjdbc) installed and worked.

Installation
Windows
1. Download ofmcp.exe and db_worker_safe.exe from github Windows release into designated folder on you local pc.

2. Open Claude desktop.

3. <img src="pics/w_setup_1.png" width="800" alt=""/>

4. <img src="pics/w_setup_2.png" width="800" alt=""/>

5. Edit claude_desktop_config.json

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

6. <img src="pics/w_setup_3.png" width="800" alt=""/>

Mac OS

1. Download ofmcp, ofmcp.sh and libduckdb.dylib from github Mac OS release into designated folder on you local mac.

2. IMPORTANT! make ofmcp and ofmcp.sh executable, in terminal run commands - chmod +x ofmcp, chmod +x ofmcp.sh

3. Open Claude desktop.

4. <img src="pics/m_setup_1.png" width="800" alt=""/>

5. <img src="pics/m_setup_2.png" width="800" alt=""/>

6. Edit claude_desktop_config.json

{
"mcpServers": {
"fusion-metadata": {
"command": "/Users/<you_username>/<folder from step 1>/ofmcp.sh",
"args": ["--db", "/Users/<you_username>/.ofjdbc/metadata.db", "--mode", "stdio"]
}
}
}

You should see.

7. <img src="pics/m_setup_3.png" width="800" alt=""/>








