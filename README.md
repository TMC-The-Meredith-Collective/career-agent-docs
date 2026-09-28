# Career Agent public documentation

Public documentation only. The private application code and career data are maintained separately.

- Canonical site: https://career-agent.tmcsolutions-org.net/
- Canonical privacy policy: https://career-agent.tmcsolutions-org.net/privacy/
- Terms of service: https://career-agent.tmcsolutions-org.net/terms/
- Policy source of truth: `privacy.md`
- Publishing: Cloudflare Pages from `main` (settings below). The previous GitHub Pages copy at https://tmc-the-meredith-collective.github.io/career-agent-docs/ stays up until every registration (Google OAuth consent screen, LinkedIn app if any) points at the canonical domain; then it is retired.

## Cloudflare Pages build settings

| Setting | Value |
|---|---|
| Production branch | `main` |
| Build command | `bundle exec jekyll build --config _config.yml,_config.cloudflare.yml` |
| Build output directory | `_site` |
| Environment variable | `RUBY_VERSION` = `3.2.2` |
| Custom domain | `career-agent.tmcsolutions-org.net` |

Do not enable Cloudflare Web Analytics on this project. The privacy policy states that this site adds no analytics scripts.

## Maintenance

1. Review actual data flows before changing the policy. Describe unimplemented requirements as pending, not completed controls.
2. Obtain owner approval for material changes. Update the effective date when publishing a materially revised policy.
3. Edit `privacy.md`; consumers should link to the canonical URL rather than maintain duplicate policy text.
4. Review every staged file for secrets and private records before committing. Keep this repository documentation-only.
5. After pushing, verify the Pages build succeeds and fetch the policy URL without authentication. Confirm the expected date, contact, and content appear.
6. Confirm application links and provider registrations still use the canonical URL. Publication does not deploy the application or complete OAuth configuration.

Privacy contact: administrative-agent@tmcsolutions-org.net.
