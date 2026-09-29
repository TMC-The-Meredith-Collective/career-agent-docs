# Career Agent public documentation

Public documentation only. The private application code and career data are maintained separately.

- Canonical site: https://career-agent.tmcsolutions-org.net/
- Canonical privacy policy: https://career-agent.tmcsolutions-org.net/privacy/
- Terms of service: https://career-agent.tmcsolutions-org.net/terms/
- Policy source of truth: `privacy.md`
- Publishing: Cloudflare Workers (static assets) from `main`, built by Workers Builds (settings below). The previous GitHub Pages copy at https://tmc-the-meredith-collective.github.io/career-agent-docs/ stays up until every registration (Google OAuth consent screen, LinkedIn app if any) points at the canonical domain; then it is retired.

## Cloudflare Workers build settings

The Worker is defined by `wrangler.jsonc` (name, static assets directory `_site`). Connect the repository under **Workers & Pages** > **Create** > **Continue with GitHub** and use:

| Setting | Value |
|---|---|
| Worker name | `career-agent-docs` (must match `name` in `wrangler.jsonc`) |
| Production branch | `main` |
| Build command | `bundle install && bundle exec jekyll build --config _config.yml,_config.cloudflare.yml` |
| Deploy command | `npx wrangler deploy` (default) |
| Preview command | `npx wrangler preview` (default) |
| Build variable | `LC_ALL` = `C.UTF-8` (without a UTF-8 locale Ruby defaults to US-ASCII and Sass fails on the theme stylesheet). Do not set `RUBY_VERSION`; the image's preinstalled Ruby 3.4 is used and the Gemfile adds the gems Jekyll 3 needs there. |
| Custom domain | `career-agent.tmcsolutions-org.net` (Worker **Settings** > **Domains & Routes**) |

`_headers` is copied into `_site` by `_config.cloudflare.yml` and supplies the security headers on every response.

Do not enable Cloudflare Web Analytics on this project. The privacy policy states that this site adds no analytics scripts.

## Design

The site uses the dark JARVIS look shared with the Career Command Center: `_layouts/default.html` plus `assets/css/site.css`. Long pages get a table of contents built from their `##` headings, and the bold-labelled lines under each title (effective date, operator, contact) render as a metadata strip, so page text never needs layout markup.

- Fonts are self-hosted in `assets/fonts/` (Space Grotesk and DM Mono, Latin subsets, SIL Open Font License; license texts sit beside the files). Pages make no third-party requests, and `_headers` allows styles and fonts from this origin only (`style-src 'self'; font-src 'self'`). Do not add inline `<style>` blocks or `style` attributes; the policy blocks them.
- The site loads no scripts.
- Do not add the JARVIS agent portraits here. They are game art and stay in the private application only.

## Maintenance

1. Review actual data flows before changing the policy. Describe unimplemented requirements as pending, not completed controls.
2. Obtain owner approval for material changes. Update the effective date when publishing a materially revised policy.
3. Edit `privacy.md`; consumers should link to the canonical URL rather than maintain duplicate policy text.
4. Review every staged file for secrets and private records before committing. Keep this repository documentation-only.
5. After pushing, verify the Workers build succeeds and fetch the policy URL without authentication. Confirm the expected date, contact, and content appear.
6. Confirm application links and provider registrations still use the canonical URL. Publication does not deploy the application or complete OAuth configuration.

Privacy contact: administrative-agent@tmcsolutions-org.net.
