---
name: medusa-mcp
description: "Manage Medusa e-commerce operations — products, orders, customers, carts, inventory, fulfillments, payments, promotions, and store settings — via 299 MCP tools. Use when the user asks about e-commerce workflows, product catalog management, order processing, customer management, or Medusa store administration."
version: 1.0.0
---

# medusa-mcp Skill

Interact with a Medusa commerce backend through MCP without loading all 299 tool definitions into context. Tools are invoked dynamically via an executor script.

## When to Use

Use this skill when the user needs to:

- **Storefront operations**: browse products, manage carts, handle checkout, process payments, manage customer accounts
- **Admin operations**: manage orders, fulfillments, inventory, pricing, promotions, returns, exchanges, claims
- **Store configuration**: regions, shipping options, tax rates, sales channels, stock locations, currencies

## Tool Categories

| Category | Store Tools | Admin Tools | Description |
|----------|------------|-------------|-------------|
| Auth | 7 | 7 | Customer/admin authentication, sessions, password resets |
| Carts | 12 | — | Create carts, add items, apply promotions, checkout |
| Products | 6 | 18 | Browse/manage products, variants, categories, tags, types |
| Orders | 7 | 20 | View/manage orders, fulfillments, edits, transfers |
| Customers | 4 | 8 | Customer accounts, addresses, groups |
| Payments | 1 | 8 | Payment providers, collections, captures, refunds |
| Inventory | — | 8 | Inventory items, stock levels, reservations |
| Promotions | — | 12 | Campaigns, promotions, rules |
| Returns | 3 | 16 | Returns, exchanges, claims |
| Fulfillment | 1 | 10 | Shipping options, fulfillment providers, shipments |
| Other | 6 | 45+ | Collections, regions, currencies, uploads, taxes, etc. |

For the full tool list with descriptions, see [TOOLS_REFERENCE.md](TOOLS_REFERENCE.md).

## Workflow

**Step 1: Identify the right tool**

Use the category table above to narrow down the area, then either check [TOOLS_REFERENCE.md](TOOLS_REFERENCE.md) or query tool details:

```bash
cd $SKILL_DIR
python executor.py --describe GetProducts
```

**Step 2: Generate a tool call** as JSON:

```json
{
  "tool": "tool_name",
  "arguments": {
    "param1": "value1"
  }
}
```

**Step 3: Execute via bash:**

```bash
cd $SKILL_DIR
python executor.py --call '{"tool": "tool_name", "arguments": {"param1": "value1"}}'
```

Replace `$SKILL_DIR` with the actual discovered path of this skill directory. All examples below assume you have `cd`'d into `$SKILL_DIR` first. Always check the response for errors before using returned IDs in follow-up calls — run `--describe <tool_name>` to verify required arguments if a call fails.

## Examples

### List products in a store

```bash
python executor.py --call '{"tool": "GetProducts", "arguments": {"limit": 10}}'
```

### Create a cart and add an item

```bash
python executor.py --call '{"tool": "PostCarts", "arguments": {"region_id": "reg_01ABC"}}'
```

Verify the response contains a cart `id` before proceeding:

```bash
python executor.py --call '{"tool": "PostCartsIdLineItems", "arguments": {"id": "cart_01XYZ", "variant_id": "variant_01DEF", "quantity": 1}}'
```

### Admin: Retrieve an order

```bash
python executor.py --call '{"tool": "AdminGetOrdersId", "arguments": {"id": "order_01GHI"}}'
```

### Get detailed tool schema

```bash
python executor.py --describe PostCartsIdComplete
```

## Error Handling

If the executor returns an error:

1. Verify the tool name matches one from [TOOLS_REFERENCE.md](TOOLS_REFERENCE.md)
2. Run `--describe <tool_name>` to check required arguments
3. Ensure the MCP server is accessible and environment variables are set (`MEDUSA_BACKEND_URL`, `PUBLISHABLE_KEY`)

---

*This skill was auto-generated from an MCP server configuration.*
*Generator: mcp_to_skill.py*
