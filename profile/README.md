<div align="center">

# napplet

**Sandboxed, single-purpose Nostr apps that run inside any host shell.**

[Website](https://napplet.run) · [Docs](https://napplet.run/docs) · [NIP-5D &amp; specs](https://github.com/napplet/naps) · [Reference runtime → Kehto](https://github.com/kehto/web)

</div>

---

A **napplet** is a small web app that runs in a locked-down iframe —
`sandbox="allow-scripts"`, no `allow-same-origin` — and delegates everything
privileged (signing, relays, storage, identity, media…) to its host **shell**
over a tiny JSON-envelope `postMessage` protocol: **NIP-5D**.

That boundary is the whole idea. A napplet bundles no client and holds no keys;
it just asks the shell. So **one app can host many napplets, and one napplet runs
in any conformant shell** — compose clients instead of rebuilding them.

napplet is the **protocol and the SDK** — not an app, not a runtime, not a vendor.
Shells like [Kehto](https://github.com/kehto/web) are pluggable implementations;
the wire format is what's standard.

### In this org

| Repo | What |
|------|------|
| [**naps**](https://github.com/napplet/naps) | NIP-5D transport + the NAP interface specs (relay, identity, storage, …) |
| [**web**](https://github.com/napplet/web) | `@napplet/*` SDK — framework-agnostic, ESM, zero runtime deps |
| [**boilerplate**](https://github.com/napplet/boilerplate) | `npx @napplet/boilerplate` — start a napplet in seconds |

### Build one

```bash
npx @napplet/boilerplate my-napplet
cd my-napplet && pnpm install
pnpm dev                 # develop against a shell
pnpm test:conformance    # prove it speaks the protocol before you ship
```

```ts
import '@napplet/shim';                  // installs window.napplet
import { relay, identity } from '@napplet/sdk';

const me = await identity.getPublicKey();
const notes = await relay.query({ kinds: [1], authors: [me], limit: 20 });
```

No signer in the sandbox. No relay pool in the app. The shell does it; the napplet
just asks.

---

<div align="center">
<sub><a href="https://napplet.run">napplet.run</a> · <a href="https://napplet.run/docs">docs</a> · built on <a href="https://github.com/nostr-protocol/nips">Nostr</a> · MIT</sub>
</div>
