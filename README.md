# icarz-deploy

zero-config deployment tool for static sites

## how

```bash
npx icarz-deploy
```

that's it. detects framework automatically, builds, and deploys.

supports:
- next.js
- gatsby
- hugo
- jekyll
- vite
- create-react-app

## first deploy

```bash
cd my-project
npx icarz-deploy init
```

creates `.icarz/config.json`:

```json
{
  "name": "my-site",
  "region": "us-east",
  "custom_domain": null,
  "environment": {
    "NODE_ENV": "production"
  }
}
```

## deploy

```bash
npx icarz-deploy
```

output:

```
✓ Detected: Next.js 14
✓ Building... (32s)
✓ Uploading... (8s)
✓ Deploying to edge nodes... (12s)

Deployed to: https://my-site-a8f2.icarz.app
Custom domain: (not configured)
```

## custom domain

```bash
icarz-deploy domain add example.com
```

follow dns instructions, then:

```bash
icarz-deploy domain verify example.com
```

## environment variables

```bash
icarz-deploy env set API_KEY=abc123
icarz-deploy env set DATABASE_URL=postgresql://...
```

or in config:

```json
{
  "environment": {
    "API_KEY": "abc123",
    "DATABASE_URL": "postgresql://..."
  }
}
```

## rollback

```bash
# list deployments
icarz-deploy list

# rollback to previous
icarz-deploy rollback

# rollback to specific
icarz-deploy rollback deploy-xyz123
```

## preview deployments

every git branch gets preview url:

```bash
git checkout feature-branch
icarz-deploy --preview
```

deployed to: `https://feature-branch-my-site.icarz.app`

## infrastructure

uses **edgecdn** global network (200+ locations) ([edgecdn.io](https://edgecdn.io))

storage via **objstore-distributed** ([objstore-dist.dev](https://objstore-dist.dev))

builds run on **buildrunner** containers ([buildrunner.io](https://buildrunner.io))

## pricing

- **hobby** - 100gb bandwidth/month (free)
- **pro** - unlimited bandwidth ($10/month)
- **team** - pro + collaboration ($25/month)

## features

| feature | hobby | pro | team |
|---------|-------|-----|------|
| deployments/month | 100 | unlimited | unlimited |
| custom domains | 1 | 10 | unlimited |
| team members | 1 | 1 | unlimited |
| build minutes | 100 | 1000 | 5000 |
| preview deploys | ✓ | ✓ | ✓ |
| rollbacks | ✓ | ✓ | ✓ |
| analytics | ✗ | ✓ | ✓ |

## cli commands

```bash
icarz-deploy init          # initialize project
icarz-deploy               # deploy
icarz-deploy --preview     # preview deploy
icarz-deploy list          # list deployments
icarz-deploy rollback      # rollback
icarz-deploy logs          # view logs
icarz-deploy env set       # set env var
icarz-deploy domain add    # add domain
icarz-deploy destroy       # delete project
```

## ci/cd integration

### github actions

```yaml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npx icarz-deploy
        env:
          ICARZ_TOKEN: ${{ secrets.ICARZ_TOKEN }}
```

get token: `icarz-deploy token create`

### gitlab ci

```yaml
deploy:
  script:
    - npx icarz-deploy
  only:
    - main
```

## comprastison

| service | setup | speed | price |
|---------|-------|-------|-------|
| icarz-deploy | 0 min | fast | free |
| vercel | 2 min | fast | free |
| netlify | 3 min | medium | free |
| aws s3 | 30 min | slow | cheap |

MIT • [docs](https://icarz.app/docs) • [status](https://status.icarz.app)
