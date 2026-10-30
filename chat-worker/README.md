# The robot's AI chat

The robot on taqishah.dev answers from its own rules first. When the rules have nothing (general questions, odd
phrasings), the site asks this small Cloudflare Worker, which asks an AI model. The model only gets the facts in
`facts.txt` (built from `data.js`) and is told never to invent anything about Taqi. If the Worker is slow, down or
out of free allowance, the robot gives its normal rule answer, so the site never breaks.

It runs free by default on Cloudflare Workers AI (`gpt-oss-120b`, 10,000 free Neurons a day, about 50 AI answers).
On the free Cloudflare plan nothing is ever billed: past the allowance the AI just pauses until 00:00 UTC.
Adding an Anthropic API key switches it to Claude, which follows the facts more closely but costs money.

## Setup, free (about 5 minutes)

1. Sign up free at https://dash.cloudflare.com. Go to Workers & Pages → Create → Import a repository, connect
   GitHub and pick `taqishah1214.github.io`. Under the build settings set the **root directory / path** to
   `chat-worker`, then Deploy.
2. Copy the Worker's address (it looks like `https://taqishah-chat.<your-name>.workers.dev`) and put it in
   `data.js` as `aiChat`, then rebuild and publish the site.

## Optional: switch to Claude

1. Sign in at https://console.anthropic.com, add billing, create a key under API keys, and set a monthly spend
   limit (for example $10) under Limits.
2. Open the Worker → Settings → Variables and Secrets → Add. Type: Secret, name `ANTHROPIC_API_KEY`, value: the key.
   Delete the secret to go back to the free model.

## Settings (wrangler.toml)

- `AI_MODEL`: the free Workers AI model (`@cf/openai/gpt-oss-120b`, or `@cf/meta/llama-3.3-70b-instruct-fp8-fast`).
- `MODEL`: the Claude model once a key is added: `claude-opus-5-5` (best answers) or `claude-haiku-5-5` (much cheaper).
- `ALLOWED_ORIGINS`: the pages allowed to call the chat.
- `FACTS_URL`: where the facts live. After any change to `data.js`, rebuild `facts.txt`
  (`node out/bot/facts.js > facts.txt` in the source folder) and publish it next to `index.html`.
- `[[ratelimits]]`: 10 questions a minute per visitor.

## Cost

Free on Workers AI. With Claude, each AI answer sends about 4,000 tokens of facts (cached between visitors) and gets a short reply. At list prices
that is roughly 1 to 3 US cents per AI answer on `claude-opus-5-5`, and a small fraction of a cent on
`claude-haiku-5-5` (an estimate; check the Usage page in the console after a few days). Most questions never reach
the AI, because the rules answer them first.
