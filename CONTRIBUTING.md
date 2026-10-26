# Listing a plugin on the evo-agents market

This repository is an index. Listing a plugin means adding one entry to `market.json` by pull request; the code, the
bundle and its signature stay in your plugin's repository. Hubs that read this market show the entry under Market
within six hours of the merge.

## Before you open the pull request

Your plugin is listed only when all of these hold:

1. **A signed release.** The plugin's GitHub repository has a release tagged `vX.Y.Z` whose assets are the bundle
   `evo-agents plugin pack` writes, `ID-X.Y.Z.tar.gz`, and its signature beside it, `ID-X.Y.Z.tar.gz.sigstore.json`
   (keyless Sigstore from a GitHub Actions workflow) or `ID-X.Y.Z.tar.gz.sig.json` (Ed25519). A repository made from
   [evo-plugin-template](https://github.com/maycuatroi1/evo-plugin-template) does this on every tag: its CI tests,
   packs, signs and releases.
2. **The entry matches the release.** The bundle's manifest has the entry's `id`, its `publisher` is the entry's
   `publisher.id`, and the identity that signed the bundle is the entry's `publisher.signer`, such as
   `sigstore:OWNER/REPO/.github/workflows/ci.yml`. CI downloads your latest release and refuses the pull request
   when any of this differs.
3. **Least permissions.** The manifest asks only for the permissions the plugin uses. A hub shows each one to the
   admin before installing, and a reviewer asks what each is for. A plugin that asks for more than its description
   needs is not listed.
4. **A new id.** `id` is not taken by another entry or by a core plugin of evo-agents (browser, builder, canvas, srs,
   tasks, crm, pm, marketing, telegram, chats), and `publisher.id` is yours: a publisher id already listed belongs to
   whoever listed it first.
5. **Isolated tier.** A plugin from a market runs isolated, outside the hub's processes. Core plugins ship in the
   hub's image and are listed here by the maintainers only.
6. **A paid plugin goes through a seller.** A plugin sold by subscription names `price.kind: subscription`, its USD
   price per hub and month, and a seller this index lists (`omelet`, https://agents.omelet.tech, for now). You
   publish the listing and upload the signed bundle on the seller's hub (`publish-seller` in the template's CI);
   the bundle of a paid plugin is never a public release asset. The seller collects payment, issues the licences
   and pays the publisher 80% of each month's revenue.

## The pull request

Add your entry at the end of `plugins` in `market.json`:

```json
{
  "id": "lumen-notes",
  "name": "Lumen notes",
  "description": "Meeting notes for a project, with action items as tasks.",
  "industries": ["education"],
  "categories": ["documents"],
  "repo": "lumen-labs/lumen-notes",
  "publisher": {"id": "lumen", "name": "Lumen Labs", "signer": "sigstore:lumen-labs/lumen-notes/.github/workflows/ci.yml"},
  "tier": "isolated",
  "price": {"kind": "free"},
  "homepage": "https://github.com/lumen-labs/lumen-notes",
  "license": "Apache-2.0"
}
```

`industries` and `categories` are short lowercase slugs; reuse the ones already in the index where they fit. The
template's `market-entry.json` is a ready entry for a plugin made from it. Then check it as CI will, with evo-ak
installed (`pip install evo-ak`):

```sh
evo-agents plugin market-check market.json --releases
```

In the pull request, say what the plugin does, who it is for and why it needs each permission. A maintainer reviews
the entry and the permissions of the release, then merges.

New releases need no pull request: a hub reads the latest release of each plugin when it reads the index. Change the
entry only when its description, signer, price or repository changes.

## Removing a plugin

The publisher, or a maintainer, removes a plugin by a pull request that deletes its entry. A maintainer also removes
one without asking when its latest release fails the CI check, it asks for permissions it does not use, or it harms
the hubs that install it. Removing an entry hides the plugin from the Market of every hub; hubs that already
installed it keep it until their admin uninstalls it, and its paid licences run until they end.

## Licence

By contributing to this repository you agree that your contribution is licensed under Apache-2.0 ([LICENSE](LICENSE)).
Your plugin keeps its own licence, which `license` in its entry names.
