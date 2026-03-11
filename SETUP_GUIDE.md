# QuickBooks MCP Server Setup Guide

## ✅ What's Already Done

1. ✓ Cloned official Intuit QuickBooks MCP server
2. ✓ Installed all npm dependencies  
3. ✓ Built the TypeScript project to `dist/`
4. ✓ Added to Claude Desktop config at: `~/Library/Application Support/Claude/claude_desktop_config.json`

## 🚀 Next Steps to Complete Setup

### Step 1: Get QuickBooks Developer Credentials

1. Go to [Intuit Developer Portal](https://developer.intuit.com/)
2. Sign in with your Intuit account
3. Click "Create an App" or select an existing app
4. In the app settings:
   - Note your **Client ID**
   - Note your **Client Secret**
   - Add this redirect URI: `http://localhost:8000/callback`
   - Choose "Accounting" scope

### Step 2: Choose Your Environment

**For Testing (Recommended First):**
- Use QuickBooks Sandbox environment
- Set `QUICKBOOKS_ENVIRONMENT=sandbox` in `.env`
- Use sandbox company credentials

**For Production:**
- Use live QuickBooks Online account
- Set `QUICKBOOKS_ENVIRONMENT=production` in `.env`

### Step 3: Get OAuth Tokens

You need a **Refresh Token** and **Realm ID** (Company ID). There are two ways:

#### Option A: Use OAuth Flow (Easiest)

The server includes built-in OAuth flow, but you need to manually authenticate:

1. Update `.env` with your Client ID and Secret:
   ```bash
   cd ~/mcp-servers/quickbooks
   nano .env
   ```

2. Add these values:
   ```env
   QUICKBOOKS_CLIENT_ID=your_actual_client_id
   QUICKBOOKS_CLIENT_SECRET=your_actual_client_secret
   QUICKBOOKS_ENVIRONMENT=sandbox
   QUICKBOOKS_REDIRECTURI=http://localhost:8000/callback
   ```

3. You'll need to create a simple OAuth flow script or use the QuickBooks OAuth Playground:
   - Visit [OAuth 2.0 Playground](https://developer.intuit.com/app/developer/playground)
   - Authorize your app
   - Get the refresh token and realm ID

#### Option B: Manual Token Entry

If you already have tokens:

1. Edit `.env`:
   ```bash
   nano ~/mcp-servers/quickbooks/.env
   ```

2. Add all credentials:
   ```env
   QUICKBOOKS_CLIENT_ID=your_client_id
   QUICKBOOKS_CLIENT_SECRET=your_client_secret
   QUICKBOOKS_REFRESH_TOKEN=your_refresh_token
   QUICKBOOKS_REALM_ID=your_company_id
   QUICKBOOKS_ENVIRONMENT=sandbox
   ```

### Step 4: Update Claude Desktop Config

The config has been pre-configured, but you need to add your actual credentials:

1. Open Claude Desktop config:
   ```bash
   nano ~/Library/Application\ Support/Claude/claude_desktop_config.json
   ```

2. Update the QuickBooks section with your actual values:
   ```json
   "quickbooks": {
     "command": "node",
     "args": [
       "/Users/kanini-ltp-889/mcp-servers/quickbooks/dist/index.js"
     ],
     "env": {
       "QUICKBOOKS_CLIENT_ID": "YOUR_ACTUAL_CLIENT_ID",
       "QUICKBOOKS_CLIENT_SECRET": "YOUR_ACTUAL_SECRET",
       "QUICKBOOKS_REFRESH_TOKEN": "YOUR_REFRESH_TOKEN",
       "QUICKBOOKS_REALM_ID": "YOUR_COMPANY_ID",
       "QUICKBOOKS_ENVIRONMENT": "sandbox"
     }
   }
   ```

### Step 5: Restart Claude Desktop

```bash
pkill -f "Claude"
open -a "Claude"
```

## 🎯 Available Tools

Once configured, you'll have access to these QuickBooks tools:

### Customer Management
- `create_customer` - Create new customers
- `get_customer` - Get customer details
- `update_customer` - Update customer info
- `delete_customer` - Delete customers
- `search_customers` - Search for customers

### Invoice Management
- `create_invoice` - Create invoices
- `read_invoice` - Read invoice details
- `update_invoice` - Update invoices
- `search_invoices` - Search invoices

### Bill Management
- `create_bill` - Create bills
- `get_bill` - Get bill details
- `update_bill` - Update bills
- `delete_bill` - Delete bills
- `search_bills` - Search bills

### Vendor Management
- `create_vendor` - Create vendors
- `get_vendor` - Get vendor details
- `update_vendor` - Update vendors
- `delete_vendor` - Delete vendors
- `search_vendors` - Search vendors

### Account Management
- `create_account` - Create accounts
- `update_account` - Update accounts
- `search_accounts` - Search accounts

### Item/Product Management
- `create_item` - Create items
- `read_item` - Read item details
- `update_item` - Update items
- `search_items` - Search items

### Estimate Management
- `create_estimate` - Create estimates
- `get_estimate` - Get estimate details
- `update_estimate` - Update estimates
- `delete_estimate` - Delete estimates
- `search_estimates` - Search estimates

### Employee Management
- `create_employee` - Create employees
- `get_employee` - Get employee details
- `update_employee` - Update employees
- `search_employees` - Search employees

### Journal Entry Management
- `create_journal_entry` - Create journal entries
- `get_journal_entry` - Get journal entry details

## 🧪 Testing

Test the connection in Claude Desktop:
```
"List all my QuickBooks customers"
"Create a new customer named Test Customer"
"Show me recent invoices"
```

## 📚 Resources

- [QuickBooks API Documentation](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/account)
- [Intuit Developer Portal](https://developer.intuit.com/)
- [OAuth 2.0 Playground](https://developer.intuit.com/app/developer/playground)
- [MCP Server GitHub Repo](https://github.com/intuit/quickbooks-online-mcp-server)

## 🔧 Troubleshooting

**Server won't connect:**
- Check that all environment variables are set correctly
- Verify tokens haven't expired
- Check Claude Desktop logs: `~/Library/Logs/Claude/`

**Authentication errors:**
- Ensure redirect URI matches exactly in your Intuit app settings
- Verify you're using the correct environment (sandbox vs production)
- Refresh tokens may expire - generate new ones if needed

**API errors:**
- Check your QuickBooks company/sandbox account is accessible
- Verify the realm ID (company ID) is correct
- Ensure you have proper permissions in QuickBooks

## 📝 Current Status

**Ready to configure:** You need to add your QuickBooks credentials to complete the setup.

**Location:** `/Users/kanini-ltp-889/mcp-servers/quickbooks/`

**Next immediate action:** Get your Client ID and Client Secret from the Intuit Developer Portal, then update the `.env` file and Claude Desktop config.
