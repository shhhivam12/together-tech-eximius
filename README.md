# Together
**Tech Eximius 2.0 · Team codehackers**

Together helps a resident track a request for flood relief. In the exercise, a family at a school asks for drinking water. The relief desk sees the request, acknowledges it, and updates the status on the resident’s phone. When connectivity drops, the phone keeps an unsent draft for the resident to retry.

[Open the working prototype](https://together-tech-eximius.shiiim.chatgpt.site/demo) · [Watch the narrated demo](https://together-tech-eximius.shiiim.chatgpt.site/walkthrough.mp4) · [Download the six-slide presentation](submission/Together-Tech-Eximius.pptx)

## Try the exercise

1. Open the [resident app](https://together-tech-eximius.shiiim.chatgpt.site/). Choose English or Hindi in settings and select **School relief hub**.
2. Select **Request help**. Choose water, enter four people and a fictional landmark, then send. The request changes to **received** once the server accepts it.
3. Open the [coordinator console](https://together-tech-eximius.shiiim.chatgpt.site/node) in another tab or on another device. Select the same hub, open **Requests**, and acknowledge the request you just created.
4. Return to the resident app. The status updates after the next refresh. The coordinator can then assign help and mark the request complete.
5. For an offline exercise, use **Guides → Save for offline use** while connected. Disconnect, draft a request and read a saved guide. Reconnect and sync the outgoing queue. The retry uses the original request ID.

**Use fictional information.** Everyone visiting the exercise can see shared records. The app does not dispatch emergency services.

## How the handoff works

| Component | Role |
| --- | --- |
| Resident PWA | React and TypeScript provide a phone interface. A service worker caches prepared guides and the local map. The device stores pending requests with stable IDs. |
| Shared request API | A Cloudflare Worker validates requests. Cloudflare D1 keeps records for each hub and deduplicates retries. |
| Coordinator console | The console reads the same records and advances a request through received, acknowledged, assigned and completed. Both views poll for updates. |
| Optional services | The original source includes OpenAI, Cerebras and Exa adapters. This public exercise uses the fallback paths and runs without paid AI credentials. |

The prototype combines a device-local outbox with a status that comes back from a human coordinator. A resident can distinguish **saved on this phone** from **received at the desk**. The 3D village and LoRa hardware bench illustrate possible future deployments. Physical radios, field performance and real-world response improvements have yet to be tested.

## Evidence and next drill

The narrated walkthrough follows one fictional water request across the public resident app and coordinator console. All 17 repository tests passed for this submission build. The next field drill would measure time to acknowledgement, duplicate records after retries, and whether residents can correctly identify a request’s current status.

- [Network API, storage and retry behavior](docs/network.md)
- [Request implementation](app/api/network/route.ts) and [database schema](db/schema.ts)
- [Original project evaluation and limitations](docs/evaluation.md)
- [Original technical README and local setup](ORIGINAL-README.md)

## Run locally

Use Node.js 22.18 or newer and run these commands from the repository root.

```sh
npm ci
npm test
npm run build
npx wrangler d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0000_dizzy_lenny_balinger.sql
npm start
```

Apply the migration once to a fresh local database. Open the address printed by Wrangler. The resident app is at `/`, the desk is at `/node` and the project tour is at `/demo`. Provider keys are optional.

## Project history

[Team Sisyphus originally built Together for DevDay Exchange New Delhi](https://github.com/shhhivam12/openai-devday-delhi). This repository retains that source and its commit history. Team codehackers’ Tech Eximius submission adds an isolated judge deployment, a recorded walkthrough and the presentation. The public exercise has its own database.
