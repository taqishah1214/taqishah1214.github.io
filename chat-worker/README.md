# The robot's AI chat

The robot on taqishah.dev answers from its own rules first. When the rules have nothing (general questions, odd
phrasings), the site asks this small Cloudflare Worker, which asks Claude. Claude only gets the facts in
`facts.txt` (built from `data.js`) and is told never to invent anything about Taqi. If the Worker is slow, down or
over its limit, the robot gives its normal rule answer, so the site never breaks.

## One-time setup (about 10 minutes)

1. **API key.** Sign in at https://console.anthropic.com, add billing, then create a key under API keys.
   Under Limits, set a monthly spend limit (for example $10) so a busy day can never surprise you.
2. **Cloudflare.** Sign up free at https://dash.cloudflare.com. Go to Workers & Pages → Create →
   Import a repository, connect GitHub and pick `taqishah1214.github.io`. Under the build settings set the
   **root directory / path** to `chat-worker`, then Deploy.
3. **Add the key.** Open the new Worker → Settings → Variables and Secrets → Add. Type: Secret,
   name `ANTHROPIC_API_KEY`, value: the key from step 1. Save.
4. Copy the Worker's address (it looks like `https://taqishah-chat.<your-name>.workers.dev`) and put it in
   `data.js` as `aiChat`, then rebuild and publish the site.

## Settings (wrangler.toml)

- `MODEL`: `claude-opus-5-5` (best answers) or `claude-haiku-5-5` (much cheaper).
- `ALLOWED_ORIGINS`: the pages allowed to call the chat.
- `FACTS_URL`: where the facts live. After any change to `data.js`, rebuild `facts.txt`
  (`node out/bot/facts.js > facts.txt` in the source folder) and publish it next to `index.html`.
- `[[ratelimits]]`: 10 questions a minute per visitor.

## Cost

Each AI answer sends about 4,000 tokens of facts (cached between visitors) and gets a short reply. At list prices
that is roughly 1 to 3 US cents per AI answer on `claude-opus-5-5`, and a small fraction of a cent on
`claude-haiku-5-5` (an estimate; check the Usage page in the console after a few days). Most questions never reach
the AI, because the rules answer them first.
