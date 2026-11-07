# FLAGS site + backend

public/                 the website (index.html)
netlify/functions/      claim.mjs (save a claim) and stats.mjs (country totals)
netlify/lib/            the rules (core.mjs) and country list
test/                   backend tests (npm test)

## Deploy (about 15 minutes)

1. Make a free GitHub account and a new PRIVATE repo (for example "flags-site").
   Upload everything in this folder (drag and drop works on github.com: "Add file > Upload files").
2. On netlify.com: Add new site > Import an existing project > GitHub > pick the repo.
   Build command: leave empty. Publish directory: public. Deploy.
   (Netlify reads netlify.toml and installs the packages itself.)
3. Site configuration > Change site name > flagsonchain
4. Site configuration > Environment variables, add:
   TOKEN_ADDRESS = the token contract address (0x...) after you launch on Pons
   TOKEN_NAME    = FLAGS   (must match NAME in public/index.html)
   Optional: MIN_HOLD (default 1), MATURE_HOURS (24), CHANGE_HOURS (24),
             IP_DAILY_LIMIT (5), MIN_COUNTRY_HOLDERS (3)
5. In public/index.html set  TOKEN: '0x...'  (same address) and commit.
   Netlify redeploys on every commit.
6. Test with 2-3 of your own wallets before posting on X.

## Rules built in
- Claim = free signature, must be fresh (10 min), match the wallet, and the wallet must hold MIN_HOLD tokens.
- A wallet counts only after MATURE_HOURS, and only while it still holds the minimum.
- Changing country restarts the clock and is limited to once per CHANGE_HOURS.
- Max IP_DAILY_LIMIT claims per network per day (IP stored only as a short hash).
- A country needs MIN_COUNTRY_HOLDERS holders before it earns points.
- Totals refresh every 5 minutes (balances re-checked on-chain).

## Not covered
- Nothing verifies where someone really lives. It is an honor-system game.
- One person with many wallets can still game small countries. The rules above make it slower, not impossible.
