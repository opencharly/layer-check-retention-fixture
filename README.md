# check-retention-fixture

Retention-discriminator fixture layer for the `check-retention` R10 bed.

The `check-retention-fixture` candy drops `/etc/charly-retention-fixture`, a
single file stamped with a generation string, into any filesystem it is composed
into. It is the fixture the retention discriminator bed rebuilds: changing the
generation stamp yields a genuinely **distinct** image id, while rebuilding it
**unchanged** with a fresh `--tag` yields the **same** id wearing another tag.
Those two moves let the bed construct the one group shape on which the pre-fix
and post-fix `keep_images` rankings disagree — one image wearing several tags,
with distinct siblings behind it.

The generation stamp is the only content that varies, so every build after the
first is a cache hit down to this final layer: the fixture costs a layer, not an
image. It uses a dedicated marker path (not `check-group`'s or `check-local`'s)
so the beds never collide when fanned out concurrently.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `check-retention-fixture` |
| Packages | none |
| Artifact | `/etc/charly-retention-fixture` (generation-stamped marker, mode `0644`) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-bed:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-check-retention-fixture:v2026.239.1629'
```

The layer writes the marker at image build; the `plan:` checks assert the file
exists and records a generation stamp.

## Layout

- `charly.yml` — the `check-retention-fixture:` candy entity: the `write:` marker
  step and its `check:` assertions.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-check:check` — the check bed and `plan:` authoring
  reference (this repo declares no `skill:` entity; the gap is tracked in
  [`opencharly/opencharly#291`](https://github.com/opencharly/opencharly/issues/291))
- The fixture half of the `check-retention` retention-discriminator bed
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
