<div align="center">

# napplet

**Sandboxed, single-purpose apps that delegate everything privileged to their host — everywhere.**

[Website](https://napplet.run) · [Docs](https://napplet.run/docs/) · [NIP-5D](https://github.com/nostr-protocol/nips/pull/2303) · [NAPs track](https://github.com/napplet/naps) · [Reference runtime → Kehto](https://github.com/kehto/web)

</div>

---

A **napplet** owns its UI and nothing else. Everything privileged — signing, storage, network, identity, media… — it asks its **host** for, over a small message protocol. It holds no keys and bundles no client.

That makes a napplet **portable by construction**. The same pattern *projects* onto any runtime that can sandbox code and pass messages:

| Projection | Binding | Status |
|------------|---------|--------|
| **Web** | sandboxed iframe + `postMessage` → `window.napplet.*` · [NIP-5D](https://github.com/nostr-protocol/nips/pull/2303) | **Live** |
| **Native** | OS process + IPC / FFI | Possible |
| **WASM** | host imports | Possible |

**Web first — not web only.** Web napplets prove the protocol on the web *by choice*; the napplet / NAP model itself is transport-agnostic.

It's an **app store, not an app**: one host runs many napplets, and one napplet runs in any conformant host — compose clients instead of rebuilding them. napplet is the *protocol and SDK*, not a product; runtimes like [Kehto](https://github.com/kehto/web) are pluggable.

> [!WARNING]
> Everything here is **alpha**. NIP-5D and most NAPs are open proposals and will change. The specs are canonical; code in these repos is non-normative and defers to them.

### In this org

**Protocol**

| Repo | What |
|------|------|
| [**naps**](https://github.com/napplet/naps) | The NAP track — capability contracts (`relay`, `storage`, `identity`, …) between a napplet and its runtime, plus the projection registry and governance |

**SDK & tooling**

| Repo | What |
|------|------|
| [**web**](https://github.com/napplet/web) | `@napplet/*` SDK, Vite plugin, `napplet` CLI, conformance tooling, agent skills, and the [napplet.run](https://napplet.run) site |
| [**boilerplate**](https://github.com/napplet/boilerplate) | Vite + TypeScript starter behind `napplet create` |
| [**mcp**](https://github.com/napplet/mcp) | MCP server that tells agents which NAPs exist, which revision to implement, and what to ignore |
| [**migrate**](https://github.com/napplet/migrate) | In-browser tool to move napplet manifests to the current NIP-5D event schema — [migrate.napplet.run](https://migrate.napplet.run) |

**Services**

| Repo | What |
|------|------|
| [**napshot**](https://github.com/napplet/napshot) | Captures missing napplet screenshots in a sandboxed shell, mirrors them to Blossom, and publishes NIP-32 labels shells can discover |
| [**cvm**](https://github.com/napplet/cvm) | Turns a text idea into a verified napplet over ContextVM *(in development)* |

**Napplets**

| Repo | What |
|------|------|
| [**evil**](https://github.com/napplet/evil) | Red team — deliberately hostile, spec-shaped napplets that probe runtimes for sandbox and protocol gaps (demo-only by default) |
| [**nappagochi**](https://github.com/napplet/nappagochi) | A desktop pet whose life follows your Nostr posts |

### Build a (web) napplet

```bash
curl -fsSL https://napplet.run/install.sh | sh   # standalone CLI (Windows: irm https://napplet.run/install.ps1 | iex)

napplet create my-napplet && cd my-napplet
napplet init                    # name, relays, Blossom servers
npx skills add napplet/web      # optional: napplet-* agent skills
pnpm install && pnpm verify     # build + conformance
napplet deploy
```

```ts
import { outbox, identity } from '@napplet/sdk';

const me = await identity.getPublicKey();
const { events } = await outbox.query([{ kinds: [1], authors: [me], limit: 20 }]);
```

No signer in the sandbox. No relay pool in the app. The runtime injects `window.napplet`, the host does the work, and the napplet just asks — same contract, whatever the runtime.

---

<div align="center">
<sub><a href="https://napplet.run">napplet.run</a> · <a href="https://napplet.run/docs/">docs</a> · web binding rides <a href="https://github.com/nostr-protocol/nips">Nostr</a></sub>
</div>
