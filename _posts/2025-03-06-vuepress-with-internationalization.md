---
layout: single
title: Vitepress with Internationalization
tags:
  - vitepress
  - vite
  - i18n
  - bun
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/02/ruby.png
  overlay_image: /assets/2025/02/ruby.png
excerpt: Setup vitepress with bun and internationalization
---
I want to setup vitepress with multiple languages. I'm using Bahasa Indonesia as default language and English as my second language.

# Setup vitepress

We need to setup manually using Bun.

## Init Bun

```bash
mkdir docsite && cd docsite

bun init
```

Select the blank. Should be like this:

![my vuepress setup](/assets/2025/03/bun-init.png)

Or, **alternatively,** we need can just run this:
```bash
bun add -D @types/bun
```

I personally like `bun init` with blank project because it creates additional files.

## Init Vitepress

```bash
bun add -D vitepress
bun vitepress init
```

I enable custom theme. here's the thing:
![my vuepress setup](/assets/2025/03/vitepress-setup.png)

A note from me, it's better to put vitepress documentation site in a dedicated directory. Given its young nature, some of its 3rd party plugin might scan the `node_modules` and other undesired directories.

## My Initial Setup

I would like to have each of the article in their own language directory.

Let's move the content to `id`.

```bash
mkdir -p docs/id
mv docs/* docs/id/
```

Clone `id` to `en`

```bash
cp -a docs/{id,en}
```

The results:
```text
├── bun.lock
├── docs
│   ├── en
│   │   ├── api-examples.md
│   │   ├── index.md
│   │   └── markdown-examples.md
│   ├── id
│   │   ├── api-examples.md
│   │   ├── index.md
│   │   └── markdown-examples.md
│   └── .vitepress
│       ├── config.ts
│       └── theme
│           ├── index.ts
│           └── style.css
├── .gitignore
├── index.ts
├── package.json
├── README.md
└── tsconfig.json
```
# Setup Sidebar

I love to use this plugin `vitepress-sidebar`.

## Install

```bash
bun add -D vitepress-sidebar 
bun add -D vitepress-i18n
```

# Config


**I'm doing this by trial and error!**

In `docs/.vitepress/config.ts` set this:

```js
import { defineConfig } from 'vitepress';
import { withSidebar } from 'vitepress-sidebar';

const rootLocale = 'id'
const supportedLocales = [rootLocale, 'en'];

const vitePressConfigs = {
  title: "jpmrblood",
  footer: {
    copyright: 'Copyright © 2025 Jan Peter Alexander'
  },
  locales: {
    root: {
      label: 'Bahasa Indonesia',
      lang: 'id',
      description: 'Situs dokumentasi pribadi',
      "themeConfig": {
        "docFooter": {
          "prev": "Sebelumnya",
          "next": "Selanjutnya"
        },
        "outline": {
          "label": "Di halaman ini"
        },
        "lastUpdated": {
          "text": "Terbaharui pada"
        },
        "returnToTopLabel": "Kembali ke atas",
        "sidebarMenuLabel": "Menu",
        "darkModeSwitchLabel": "Tampilan",
        "lightModeSwitchTitle": "Ganti jadi tema terang",
        "darkModeSwitchTitle": "Ganti jadi tema gelap"
      }
    },
    en: {
      label: 'English',
      lang: 'en',
      description: 'A documentation site for myself',
      "themeConfig": {
        "docFooter": {
          "prev": "Previous page",
          "next": "Next page"
        },
        "outline": {
          "label": "On this page"
        },
        "lastUpdated": {
          "text": "Last updated"
        },
        "langMenuLabel": "Change language",
        "returnToTopLabel": "Return to top",
        "sidebarMenuLabel": "Menu",
        "darkModeSwitchLabel": "Appearance",
        "lightModeSwitchTitle": "Switch to light theme",
        "darkModeSwitchTitle": "Switch to dark theme"
      }
    },
  },
  "themeConfig": {
    logo: '/logo.png',
    siteTitle: false,
    "search": {
      "provider": "local",
      "options": {
        "locales": {
          "root": {
            "translations": {
              "button": {
                "buttonText": "Cari",
                "buttonAriaLabel": "Cari"
              },
              "modal": {
                "displayDetails": "Tampilkan daftar lengkap",
                "resetButtonTitle": "Reset pencarian",
                "backButtonTitle": "Tutup pencarian",
                "noResultsText": "Tidak ada hasil untuk",
                "footer": {
                  "selectText": "memilih",
                  "selectKeyAriaLabel": "enter",
                  "navigateText": "menuju",
                  "navigateUpKeyAriaLabel": "up arrow",
                  "navigateDownKeyAriaLabel": "down arrow",
                  "closeText": "to close",
                  "closeKeyAriaLabel": "escape"
                }
              }
            }
          },
          "en": {
            "translations": {
              "button": {
                "buttonText": "Search",
                "buttonAriaLabel": "Search"
              },
              "modal": {
                "displayDetails": "Display detailed list",
                "resetButtonTitle": "Reset search",
                "backButtonTitle": "Close search",
                "noResultsText": "No results for",
                "footer": {
                  "selectText": "to select",
                  "selectKeyAriaLabel": "enter",
                  "navigateText": "to navigate",
                  "navigateUpKeyAriaLabel": "up arrow",
                  "navigateDownKeyAriaLabel": "down arrow",
                  "closeText": "to close",
                  "closeKeyAriaLabel": "escape"
                }
              }
            }
          }
        }
      }
    }
  },
  rewrites: {
    'id/:rest*': ':rest*'
  }
}

const commonSidebarConfigs = {
  useTitleFromFrontmatter: true
}

const vitePressSidebarConfigs = [
  ...supportedLocales.map((lang) => {
    return {
      ...commonSidebarConfigs,
      ...(rootLocale === lang ? {} : { basePath: `/${lang}/` }), // If using `rewrites` option
      documentRootPath: `/docs/${lang}`,
      resolvePath: rootLocale === lang ? '/' : `/${lang}/`,
    };
  })
]

export default defineConfig(withSidebar(vitePressConfigs, vitePressSidebarConfigs));

```

## Add a public directory and put logo there

I forgot to put logo.

```bash
mkdir docs/public
```

And put `logo.png` there. I put PNG because I don't know how to create a good SVG graph. Did try to create the SVG, but it so undistinguishable with the text title. LOL.

## Root config

Footer and text title still in the outside.

## Inside Locales

Inside of the `locales` should be the language and description. We also translate UI elements there.

## Inside Theme Config

- Logo configuration is there.
- search provider also configured there along with its localization.


## Rewrites

I'm using `vitepress` rewrite to make Bahasa Indonesia as the default language. It should be accessed with `/article-name` rather than `/id/article-name`. 

# Test it

Run locally:

```bash
bun docs:dev

```

Everytime you change the `config.ts`, you need to re-run the `bun`.

# TODO
- Integration with Tailwind 4
- Styling

This is my first exposure with VueJS as a developer. I want to try to create an SSR integration later for member-only section later.

# References

- [Install Tailwind CSS using Vite](https://tailwindcss.com/docs/installation/using-vite)