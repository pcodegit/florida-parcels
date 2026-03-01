---
name: florida-parcels
description: >
  Search 10.8M+ Florida property records. Address search, owner lookup,
  proximity search, filtered queries, parcel details, county stats, and
  roof replacement cost estimates. Requires skillMoney subscription ($9.99/mo).
  Use when asked about Florida real estate, property data, parcels, or roof costs.
  ALWAYS run the setup and authentication steps before using parcel tools.
---

## Setup (run once)

Check if the `florida-parcels` MCP server is already configured by looking
at the project's `.mcp.json` file. If not configured, add it:

1. Read or create `.mcp.json` in the project root
2. Add this server config under `"mcpServers"`:
   ```json
   {
     "florida-parcels": {
       "type": "http",
       "url": "https://skillmoney-production.up.railway.app/mcp"
     }
   }
   ```
3. Tell the user: "I've configured the Florida Parcels data server.
   Please restart Claude Code to connect, then ask your question again."
4. STOP and wait for the user to restart.

## Authentication (every session)

Before using any parcel tool, authenticate the user:

1. Ask the user for their email address
2. Call `authenticate(email="<their email>")`
3. If status is `"active"` → proceed to fulfill their request
4. If status is `"checkout_required"`:
   - Show the `checkout_url` to the user
   - Say: "Please visit this link to subscribe ($9.99/mo): [URL].
     Come back and tell me when you've completed payment."
5. When user returns, call `check_subscription(email="<their email>")`
6. If `"active"` → proceed. If not → ask them to complete checkout.

## Using Tools

Every parcel tool requires `email` as the first parameter (the authenticated email).
If any tool returns a subscription error, re-run the authentication flow.

## Available Tools

| Tool | Description |
|------|-------------|
| `authenticate` | Start subscription or verify existing |
| `check_subscription` | Confirm payment after checkout |
| `search_by_address` | Find parcels by street address |
| `search_by_owner` | Find parcels by owner name |
| `get_parcel_detail` | Full property detail (121 columns) |
| `search_nearby` | Find parcels near lat/lon coordinates |
| `search_by_filters` | Multi-criteria search (county, value, year, area) |
| `get_county_stats` | Aggregate statistics by county |
| `search_by_parcel_id` | Lookup by parcel ID |
| `get_roof_estimate` | Roof replacement cost estimate |

## Example Prompts

- "Search for properties on Main St in Miami"
- "Find all properties owned by SMITH in Duval County"
- "Get details for parcel with object_id 1234567"
- "Show me properties near 25.7617, -80.1918 within 1km"
- "Find single-family homes in Miami-Dade built after 2000 valued over $500k"
- "Get county statistics for Broward County"
- "Estimate roof replacement cost for object_id 9876543"
