# Installing KovaPDF in Cline

KovaPDF is a remote MCP server. Nothing needs to be installed or built, and no API key is needed.

1. Open the MCP settings file (`cline_mcp_settings.json`) and add:

   ```json
   {
     "mcpServers": {
       "kovapdf": {
         "type": "streamableHttp",
         "url": "https://mcp.kovapdf.com"
       }
     }
   }
   ```

2. The first tool call opens the KovaPDF sign in page in the browser. Sign in to a KovaPDF account and click Allow.

3. Test it: ask for a KovaPDF tool on a PDF link, for example "Compress https://www.kovapdf.com/press/kovapdf-fact-sheet.pdf with KovaPDF".
