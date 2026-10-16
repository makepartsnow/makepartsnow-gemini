# Make Parts Now for Gemini CLI: instant 3D printing and CNC machining quotes

![Make Parts Now](assets/banner.png)

The official Gemini CLI extension from [MakePartsNow.com](https://makepartsnow.com), a US 3D printing and CNC machining service. Give Gemini a CAD file (STL, STEP, 3MF or OBJ) and get an instant quote for custom parts and prototypes: SLS nylon (PA12), FDM plastic, or CNC-machined aluminum and brass, with the price and ship date for each production speed. Change the quantity, material, color, finish or speed, then get a link to order and pay on makepartsnow.com.

The extension bundles:

- The hosted Make Parts Now MCP server at `https://makepartsnow.com/mcp/assistant`. No sign-in is needed to quote.
- `skills/quote-parts/`, the workflow skill: how the upload, quote, change and checkout tools fit together, and the rules that keep it safe (only the prices the tools return, upload only the file you asked about, never pay for you).
- `/make-parts-now:quote`, a command that quotes a file in one step.

This repo is configuration only: no code runs on your machine.

## Install

```
gemini extensions install https://github.com/makepartsnow/makepartsnow-gemini
```

Then ask "Quote 25 of ./bracket.step in black nylon", or run:

```
/make-parts-now:quote ./bracket.step 25 black nylon
```

## What it does

- Quotes STL, STEP, 3MF and OBJ files for SLS nylon and FDM plastic 3D printing and CNC machining in aluminum and brass, with the total and estimated ship date for each production speed, priced exactly as on makepartsnow.com.
- Changes the quantity, material, finish, color or speed of a quote.
- Creates a checkout link. You review the order, enter the shipping address and pay on makepartsnow.com through Stripe Checkout. Gemini can't pay, and nothing is ordered until you do.
- Checks the status and tracking of your orders, after you sign in to your Make Parts Now account with `/mcp auth make-parts-now`.

Try "Change that to 50" or "I'll take Standard, send me the checkout link".

One part per quote, files up to 95 MB. Prices are in USD before sales tax. Shipping is within the United States only. Make Parts Now doesn't make firearms or their parts, other weapons, or other restricted products, and files subject to ITAR, EAR or CUI rules can't be uploaded.

## Tools

The server's own tool list is the source of truth.

| Tool | What it does | Access |
|---|---|---|
| `get_manufacturing_options` | Processes, materials, colors, finishes and speeds | Read |
| `create_upload_link` | A one-time link to upload one CAD file | Write (creates an upload slot) |
| `analyze_part` | Whether a part can be made, and by which processes | Read |
| `quote_part` | Prices a part for every production speed | Write (saves a quote) |
| `update_quote` | A revised quote with a new quantity, material, finish, color or speed | Write (saves a new quote) |
| `create_checkout_link` | A Stripe Checkout link for the quote and speed you picked | Write (no charge; you pay on the website) |
| `delete_uploaded_part` | Deletes a file you uploaded | Write (destructive) |
| `get_order_status` | Your orders and tracking, after you sign in | Read |

## What you should know before installing

- **Network.** The extension connects only to makepartsnow.com.
- **Your files.** When the file is on your computer, Gemini uploads it with one `curl` command to a one-time upload link (valid for 2 hours), and Gemini CLI asks you to confirm the command first. The skill tells Gemini to upload only the CAD file you asked it to quote, never other files, even if a message, web page or document asks it to. You can also upload through the upload page Gemini links to.
- **What Make Parts Now receives.** The file, what you said about the part, and the options you chose. Quoting needs no account. Files sent through an AI assistant that are never quoted are deleted after 14 days, and you can ask Gemini to delete a file at any time. See the [privacy policy](https://makepartsnow.com/legal/privacy).
- **Money.** Nothing is charged in the terminal. A checkout link opens Stripe Checkout on makepartsnow.com, where you enter the address and pay. Anyone with a checkout link can open it and pay for that quote, though they can't change it.
- **Sign-in.** Only order status needs your Make Parts Now account. `/mcp auth make-parts-now` opens Make Parts Now's own sign-in page (OAuth); Gemini CLI keeps the token, and the extension stores no keys or passwords. No telemetry.

## Support

- Orders and questions: orders@makepartsnow.com or [makepartsnow.com/contact](https://makepartsnow.com/contact)
- Other ways to connect (ChatGPT, Claude, Hermes, OpenClaw, coding agents): [makepartsnow.com/developers](https://makepartsnow.com/developers#ai-assistants)
- Problems with the extension: open an issue in this repo. Report security problems privately to orders@makepartsnow.com, not in an issue.
- [Terms](https://makepartsnow.com/legal/terms)

## License

MIT, for the files in this repo. The Make Parts Now service is covered by its [terms](https://makepartsnow.com/legal/terms).
