# Sharkitect Card Template

Master template for custom digital business cards. This repo is marked as a **GitHub Template Repository** — create new client cards from it via `gh repo create --template sharkitect-cards/_template`.

## What's in here

- `index.html` — the master card HTML with `{{VARIABLES}}` placeholders
- Every variable is documented in the build guide at `resources/digital-card/template/BUILD-GUIDE.md` in the Workforce HQ workspace

## How to spawn a new card

Use the automation tool in the Skill Management Hub workspace (once built): `tools/new-card.py --slug acme --name "Acme Inc" --email contact@acme.com ...`

The tool will:
1. Spawn a new repo from this template: `sharkitect-cards/{slug}`
2. Fill in all `{{VARIABLES}}` with the client's values
3. Generate a QR code SVG pointing to the card's live URL
4. Build the vCard (.vcf) file with embedded contact photo
5. Enable GitHub Pages
6. Return the live URL: `https://cards.sharkitectdigital.com/{slug}/`

## Manual spawn (if the tool isn't available)

```bash
gh repo create sharkitect-cards/{slug} --template sharkitect-cards/_template --public
git clone https://github.com/sharkitect-cards/{slug}.git
cd {slug}
# Edit index.html, replace all {{VARIABLES}}
# Add logo.png, qr-code.svg, {slug}.vcf
git add -A && git commit -m "Build {slug} card" && git push
gh api repos/sharkitect-cards/{slug}/pages -X POST --input - <<'EOF'
{"source":{"branch":"main","path":"/"},"build_type":"legacy"}
EOF
```

Live URL will be `https://cards.sharkitectdigital.com/{slug}/` within a few minutes.

## Do not serve this repo directly

The template is not meant to be viewed as a card — it still has `{{VARIABLES}}` placeholders. Pages is NOT enabled on this repo for that reason. Spawn a new repo from it, fill the variables, enable Pages on the spawned repo.
