# Chatflare

A web based chat interface built on [Cloudflare Pages](https://page.cloudflare.com) that allows for exploring [Text Generation models](https://developers.cloudflare.com/workers-ai/models/#text-generation) on [Cloudflare Workers AI](https://developers.cloudflare.com/workers-ai/). Design is built using [tailwind](https://tailwindcss.com/).

This demo makes use of [LocalStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) to maintain state. We have better solutions available (moar coming soon).

This is, like all of us, a Work in Progress.

## Installation

```bash
npm install
# If this is your first time here
npx wrangler login
```

## Develop

This uses the local Vite server for rapid development

```bash
npm run dev
```

### Preview

This builds and runs in Wrangler your site locally, just as it will run on production

```bash
npm run preview
```

## Deploy

This hosts your site on [Cloudflare Pages](https://pages.cloudflare.com)

```bash
npm run deploy
```

###  Debug

```bash
npx wrangler pages deployment tail
```

## Advanced

If you are on a Mac, you can generate the list of models in [script.js](./public/static/script.js) by running the following commands:

```bash
# If this is your first time here. You won't regret it.
brew install jq
```

```bash
# Filter all Text Generation models into "ga" and "beta"
npx wrangler ai models --json | jq ' 
  reduce .[] as $item (
    {beta: [], ga: []};
    if ($item.task.name == "Text Generation") then
      if ($item.properties | any(.property_id == "beta" and .value == "true")) then
        .beta += [$item.name]
      else
        .ga += [$item.name]
      end
    else
      .
    end
  ) |
  .beta |= sort |
  .ga |= sort
'
```

## TODOs

- [x] Add help area
- [x] Change settings to be "advanced" mode.
- [ ] Get access in front of it
- [ ] Use D1 for Session state