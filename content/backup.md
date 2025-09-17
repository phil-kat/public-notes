```
import { QuartzConfig } from "./quartz/cfg"
import * as Plugin from "./quartz/plugins"

/**
 * Quartz 4 Configuration
 *
 * See https://quartz.jzhao.xyz/configuration for more information.
 */
const config: QuartzConfig = {
  configuration: {
    pageTitle: "Дом, в котором",
    pageTitleSuffix: "",
    enableSPA: true,
    enablePopovers: true,
    analytics: {
      provider: "plausible",
    },
    locale: "en-US",
    baseUrl: "quartz.jzhao.xyz",
    ignorePatterns: ["private", "templates"],
    defaultDateType: "modified",
    theme: {
      fontOrigin: "googleFonts",
      cdnCaching: true,
      typography: {
        header: "Schibsted Grotesk",
        body: "Source Sans Pro",
        code: "IBM Plex Mono",
      },
      colors: {
        lightMode: {
          light: "#faf8f8",
          lightgray: "#e5e5e5",
          gray: "#b8b8b8",
          darkgray: "#4e4e4e",
          dark: "#2b2b2b",
          secondary: "#284b63",
          tertiary: "#84a59d",
          highlight: "rgba(143, 159, 169, 0.15)",
          textHighlight: "#fff23688",
        },
        darkMode: {
          light: "#161618",
          lightgray: "#393639",
          gray: "#646464",
          darkgray: "#d4d4d4",
          dark: "#ebebec",
          secondary: "#7b97aa",
          tertiary: "#84a59d",
          highlight: "rgba(143, 159, 169, 0.15)",
          textHighlight: "#b3aa0288",
        },
      },
    },
  },
  plugins: {
    transformers: [
      Plugin.FrontMatter(),
      Plugin.CreatedModifiedDate({
        priority: ["frontmatter", "git", "filesystem"],
      }),
      Plugin.SyntaxHighlighting({
        theme: {
          light: "github-light",
          dark: "github-dark",
        },
        keepBackground: false,
      }),
      Plugin.ObsidianFlavoredMarkdown({ enableInHtmlEmbed: true }),
      Plugin.GitHubFlavoredMarkdown(),
      Plugin.TableOfContents(),
      Plugin.CrawlLinks({ markdownLinkResolution: "shortest" }),
      Plugin.Description(),
      Plugin.Latex({ renderEngine: "katex" }),
    ],
    filters: [Plugin.RemoveDrafts()],
    emitters: [
      Plugin.AliasRedirects(),
      Plugin.ComponentResources(),
      Plugin.ContentPage(),
      Plugin.FolderPage(),
      Plugin.TagPage(),
      Plugin.ContentIndex({
        enableSiteMap: true,
        enableRSS: true,
      }),
      Plugin.Assets(),
      Plugin.Static(),
      Plugin.Favicon(),
      Plugin.NotFoundPage(),
      // Comment out CustomOgImages to speed up build time
      Plugin.CustomOgImages(),
    ],
  },
}

export default config

```

...
quartz/styles/custom.scss
```
@use "./base.scss";

// ===============================
// Базовые размеры для больших экранов (≥1000px)
// ===============================
:root {
  --font-size-body: 21px;

  --font-size-header: 2.2rem;
  --font-size-h2:     1.8rem;
  --font-size-h3:     1.5rem;
  --font-size-h4:     1.3rem;
  --font-size-h5:     1.2rem;
  --font-size-h6:     1.1rem;

  --font-size-small:  0.9rem;
  --font-size-code:   0.95em;

  --line-height-body: 1.3;
}

// ===============================
// Базовые стили
// ===============================
body {
  font-size: var(--font-size-body);
  line-height: var(--line-height-body);
}

h1 {
  font-size: var(--font-size-header);
  line-height: 1.3;
}

h2 {
  font-size: var(--font-size-h2);
  line-height: 1.3;
}

h3 {
  font-size: var(--font-size-h3);
  line-height: 1.4;
}

h4 {
  font-size: var(--font-size-h4);
}

h5 {
  font-size: var(--font-size-h5);
}

h6 {
  font-size: var(--font-size-h6);
}

small {
  font-size: var(--font-size-small);
}

code {
  font-size: var(--font-size-code);
}

// ===============================
// Для экранов меньше 1000px
// ===============================
@media (max-width: 960px) {
  :root {
    --font-size-body: 18px;

    --font-size-header: 2rem;
    --font-size-h2:     1.7rem;
    --font-size-h3:     1.4rem;
    --font-size-h4:     1.2rem;
    --font-size-h5:     1.1rem;
    --font-size-h6:     1rem;

    --font-size-small:  0.85rem;

    --line-height-body: 1.6;
  }
}

// ===============================
// Для очень маленьких экранов (<480px)
// ===============================
@media (max-width: 480px) {
  :root {
    --font-size-body: 16px;

    --font-size-header: 1.8rem;
    --font-size-h2:     1.5rem;
    --font-size-h3:     1.3rem;
    --font-size-h4:     1.1rem;
    --font-size-h5:     1rem;
    --font-size-h6:     0.95rem;

    --font-size-small:  0.8rem;

    --line-height-body: 1.0;
  }

  body {
    padding-inline: 0.75rem; // более современный способ задать паддинги
  }
}

// ===============================
// Custom CSS
// ===============================
// put your custom CSS here!
```

6 