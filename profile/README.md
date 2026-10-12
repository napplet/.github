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
| **Web** | iframe + `postMessage` → `window.napplet.*` · [NIP-5D](https://github.com/nostr-protocol/nips/pull/2303) | **Live** |
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
| [**web**](https://github.com/napplet/web) | `@napplet/*` SDK — framework-agnostic ESM packages |
| [**boilerplate**](https://github.com/napplet/boilerplate) | `npx @napplet/boilerplate` — start a napplet in seconds |
| [**cvm**](https://github.com/napplet/cvm) | Generation, capability review, and verified toolchain promotion |
| [**nappagochi**](https://github.com/napplet/nappagochi) | Activity-driven pet napplet |
| [**migrate**](https://github.com/napplet/migrate) | Manifest migration tool |
| [**.github**](https://github.com/napplet/.github) | Organization guidance |

### Build a (web) napplet

```bash
npx @napplet/boilerplate my-napplet
cd my-napplet && pnpm install
pnpm dev                 # develop against a host shell
pnpm test:conformance    # prove it speaks the protocol before you ship
```

```ts
// The runtime injects window.napplet before app code runs.
import { outbox, identity } from '@napplet/sdk';

const me = await identity.getPublicKey();
const notes = await outbox.query([{ kinds: [1], authors: [me], limit: 20 }]);
```

No signer in the sandbox. No relay pool in the app. The host does it; the napplet
just asks — same contract, whatever the runtime.

---

<div align="center">
<sub><a href="https://napplet.run">napplet.run</a> · <a href="https://napplet.run/docs">docs</a> · web binding rides <a href="https://github.com/nostr-protocol/nips">Nostr</a> · MIT</sub>
</div>

### Controller input

The proposed [NAP-GAMEPAD](https://github.com/napplet/naps/pull/108) supplies optional controller snapshots through a host-owned interface. The SDK, runtime, conformance tooling, and starter guidance track that proposal. Apps keep keyboard or pointer fallbacks and respect frame focus; native controller access belongs to the host. This is non-normative guidance; the living proposal defines the API and messages.
