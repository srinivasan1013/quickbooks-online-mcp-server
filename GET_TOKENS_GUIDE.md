# QuickBooks OAuth Token Guide

## 🚀 Get Your Tokens Using OAuth Playground

**This method bypasses the redirect URI issue!**

### Step 1: Access the Playground

1. Go to: https://developer.intuit.com/app/developer/playground
2. Sign in with your Intuit Developer account

### Step 2: Select Your App

1. In the "Select an App" dropdown, choose your app
   - Client ID: `ABFKhPPRJku5LMQYVCwO9iJewZcqX0nLIp7kq4p6HsQNa4R2ay`
2. Click on **"Scopes"** section
3. Check **"Accounting"** checkbox
4. Environment: Select **"Sandbox"** (for testing)

### Step 3: Get Authorization Code

1. Click the blue button **"Get authorization code"**
2. A new window will open
3. Sign in to your QuickBooks **Sandbox** account
   - If you don't have one, click "Create a test company" 
   - Username/password for sandbox account
4. Click **"Authorize"** to allow access
5. The window will close automatically
6. You'll see "Authorization code received" ✓

### Step 4: Get OAuth Tokens

1. Click the blue button **"Get OAuth 2.0 tokens"**
2. The playground will exchange the code for tokens
3. You'll see:
   - ✅ **Access Token** (short-lived, ignore this)
   - ✅ **Refresh Token** ← **COPY THIS** (long string, 100+ characters)
   - ✅ **Realm ID** ← **COPY THIS** (number, e.g., 9341452745768901)

### Step 5: Copy Your Tokens

**Important:** Copy the ENTIRE tokens - they're very long!

**Refresh Token example:**
```
AB11728374839abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
```
(The real token is 100-200+ characters)

**Realm ID example:**
```
9341452745768901
```
(A long number, typically 16 digits)

### Step 6: Update Your .env File

Open `/Users/kanini-ltp-889/mcp-servers/quickbooks/.env` and update:

```env
QUICKBOOKS_CLIENT_ID=ABFKhPPRJku5LMQYVCwO9iJewZcqX0nLIp7kq4p6HsQNa4R2ay
QUICKBOOKS_CLIENT_SECRET=3Tue63EE1rTQsqp5fKUpSzD3fVVMkU1bYN6C6YEx
QUICKBOOKS_REFRESH_TOKEN=<PASTE_FULL_REFRESH_TOKEN_HERE>
QUICKBOOKS_REALM_ID=<PASTE_REALM_ID_HERE>
QUICKBOOKS_REDIRECTURI=http://localhost:8000/callback
QUICKBOOKS_ENVIRONMENT=sandbox
```

### Step 7: Update Claude Desktop Config

Also update the config at:
`~/Library/Application Support/Claude/claude_desktop_config.json`

Replace the tokens in the "quickbooks" section with the same values.

### Step 8: Restart Claude Desktop

```bash
pkill -f "Claude"
open -a "Claude"
```

### Step 9: Test

In Claude Desktop, try:
- "List my QuickBooks customers"
- "Show me my QuickBooks invoices"

---

## 📝 Troubleshooting

**"Invalid grant" error:**
- Refresh token expired (they expire after 100 days of inactivity)
- Go back to OAuth Playground and get new tokens

**"Unauthorized" error:**
- Check that realm ID matches your sandbox company
- Verify environment is set to "sandbox"

**"Company not found" error:**
- Wrong realm ID
- Verify you're connected to the correct QuickBooks company

---

## 🔄 Token Expiration

- **Access tokens:** Expire after 1 hour (MCP server renews automatically)
- **Refresh tokens:** Expire after 100 days of inactivity
- If refresh token expires, get new tokens from OAuth Playground

