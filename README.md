# next-supa-mcp

> 🤖 **MCP Server — Official AI Companion for [`next-supa-utils`](https://www.npmjs.com/package/next-supa-utils)**

An [MCP (Model Context Protocol)](https://modelcontextprotocol.io/) server that ensures AI assistants **always** generate code using [`next-supa-utils`](https://nextsupautils.vercel.app/) — instead of writing raw Supabase boilerplate.

---

## ✨ Why?

When AI assistants write Next.js + Supabase code, they tend to:
- ❌ Use raw `@supabase/ssr` with verbose cookie-chunking logic  
- ❌ Write manual `try/catch` in every Server Action  
- ❌ Copy-paste 40+ lines of middleware boilerplate  

**`next-supa-mcp` fixes this** by giving the AI tools that output code using `next-supa-utils` — a library that condenses all of that into clean, type-safe one-liners.

---

## 🛠️ Available Tools

| Tool | Description |
|------|-------------|
| `get_next_supa_utils_docs` | Returns the official cheatsheet with correct imports and API usage |
| `scaffold_auth_middleware` | Generates a `middleware.ts` using `withSupaAuth()` with RBAC support |
| `generate_crud_action` | Generates Server Actions wrapped with `createAction()` |
| `read_supabase_schema` | Reads the user's `database.types.ts` for accurate code generation |

---

## 🚀 Quick Setup

### 1. Clone & Build

```bash
git clone https://github.com/aryaintarann/next-supa-mcp.git
cd next-supa-mcp
npm install
npm run build
```

### 2. Configure Your AI Client

Add to your MCP client configuration:

<details>
<summary><b>Claude Desktop</b> — <code>claude_desktop_config.json</code></summary>

```json
{
  "mcpServers": {
    "next-supa-mcp": {
      "command": "node",
      "args": ["/absolute/path/to/next-supa-mcp/dist/index.js"]
    }
  }
}
```
</details>

<details>
<summary><b>Cursor</b> — <code>.cursor/mcp.json</code></summary>

```json
{
  "mcpServers": {
    "next-supa-mcp": {
      "command": "node",
      "args": ["/absolute/path/to/next-supa-mcp/dist/index.js"]
    }
  }
}
```
</details>

<details>
<summary><b>VS Code (Copilot)</b> — <code>.vscode/mcp.json</code></summary>

```json
{
  "servers": {
    "next-supa-mcp": {
      "type": "stdio",
      "command": "node",
      "args": ["/absolute/path/to/next-supa-mcp/dist/index.js"]
    }
  }
}
```
</details>

<details>
<summary><b>Windsurf</b> — <code>~/.codeium/windsurf/mcp_config.json</code></summary>

```json
{
  "mcpServers": {
    "next-supa-mcp": {
      "command": "node",
      "args": ["/absolute/path/to/next-supa-mcp/dist/index.js"]
    }
  }
}
```
</details>

> **Note:** Replace `/absolute/path/to/next-supa-mcp` with your actual path.

---

## 📖 Tool Examples

### `get_next_supa_utils_docs`

Returns a full cheatsheet covering all available imports:

```
Client Hooks:  useSupaUser, useSupaSession, useSupaUpload, useSupaRealtime, SupaProvider
Server Helpers: withSupaAuth, createAction, routeWrapper
```

### `scaffold_auth_middleware`

**Input:**
```json
{
  "routes": [
    { "path": "/dashboard" },
    { "path": "/admin/:path*", "allowedRoles": ["admin"] }
  ],
  "redirectTo": "/login"
}
```

**Output:** A complete `middleware.ts` using `withSupaAuth`:
```typescript
import { withSupaAuth } from 'next-supa-utils/server';

export default withSupaAuth({
  routes: [
    { path: '/dashboard' },
    { path: '/admin/:path*', allowedRoles: ['admin'] },
  ],
  redirectTo: '/login',
});

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico).*)'],
};
```

### `generate_crud_action`

**Input:**
```json
{ "tableName": "posts", "operation": "insert" }
```

**Output:** A Server Action using `createAction`:
```typescript
'use server';
import { createAction } from 'next-supa-utils/server';

export const createPosts = createAction(
  async (supabase, input: CreatePostsInput) => {
    const { data, error } = await supabase
      .from('posts')
      .insert(input)
      .select()
      .single();
    if (error) throw error;
    return data;
  }
);
```

### `read_supabase_schema`

Reads `database.types.ts` from the user's project so generated code uses the **exact** table and column names.

---

## 🧱 Tech Stack

- **TypeScript** — Strict mode
- **[@modelcontextprotocol/sdk](https://github.com/modelcontextprotocol/typescript-sdk)** — Official MCP SDK
- **[Zod](https://zod.dev/)** — Tool input validation
- **[tsup](https://tsup.egoist.dev/)** — Bundler

---

## 📚 Related

- **[next-supa-utils](https://www.npmjs.com/package/next-supa-utils)** — The library this MCP promotes
- **[Documentation](https://nextsupautils.vercel.app/)** — Full API reference
- **[MCP Specification](https://modelcontextprotocol.io/)** — Learn about MCP

---

## 📄 License

MIT
