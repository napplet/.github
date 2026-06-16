<div align="center">

# napplet

**Sandboxed, single-purpose apps that delegate everything privileged to their host — everywhere.**

[Website](https://napplet.run) · [Docs](https://napplet.run/docs) · [Specs (NIP-5D + NAPs)](https://github.com/napplet/naps) · [Reference runtime → Kehto](https://github.com/kehto/web)

</div>

---

A **napplet** owns its UI and nothing else. Everything privileged — signing,
storage, network, identity, media… — it asks its **host** for, over a small
message protocol. It holds no keys and bundles no client.

That makes a napplet **portable by construction**. The same pattern *projects* onto
any runtime that can sandbox code and pass messages:

| Projection | Binding | Status |
|------------|---------|--------|
| **Web** | iframe + `postMessage` → `window.napplet.*` · [NIP-5D](https://github.com/napplet/naps) | **Live** |
| **Native** | OS process + IPC / FFI | Possible |
| **WASM** | host imports | Possible |

**Web first — not web only.** Web napplets prove the protocol on the web *by choice*;
the napplet / NAP model itself is transport-agnostic.

It's an **app store, not an app**: one host runs many napplets, and one napplet runs
in any conformant host — compose clients instead of rebuilding them. napplet is the
*protocol and SDK*, not a product; runtimes like [Kehto](https://github.com/kehto/web)
are pluggable.

### In this org

| Repo | What |
|------|------|
| [**naps**](https://github.com/napplet/naps) | The protocol — NIP-5D (web binding) + the NAP interface specs and projection registry |
| [**web**](https://github.com/napplet/web) | `@napplet/*` SDK — framework-agnostic, ESM, zero runtime deps |
| [**boilerplate**](https://github.com/napplet/boilerplate) | `npx @napplet/boilerplate` — start a napplet in seconds |

### Build a (web) napplet

```bash
npx @napplet/boilerplate my-napplet
cd my-napplet && pnpm install
pnpm dev                 # develop against a host shell
pnpm test:conformance    # prove it speaks the protocol before you ship
```

```ts
import '@napplet/shim';                  // installs window.napplet
import { relay, identity } from '@napplet/sdk';

const me = await identity.getPublicKey();
const notes = await relay.query({ kinds: [1], authors: [me], limit: 20 });
```

No signer in the sandbox. No relay pool in the app. The host does it; the napplet
just asks — same contract, whatever the runtime.

---

<div align="center">
<sub><a href="https://napplet.run">napplet.run</a> · <a href="https://napplet.run/docs">docs</a> · web binding rides <a href="https://github.com/nostr-protocol/nips">Nostr</a> · MIT</sub>
</div>
