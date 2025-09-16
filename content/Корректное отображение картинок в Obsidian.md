---
url: https://phil-kat.github.io/public-notes/Корректное-отображение-картинок-в-Obsidian
tags:
---
Чтобы отображались картинки в Quartz, вам нужно внести несколько изменений в конфигурацию:

## 1. Проверьте структуру папок
Убедитесь, что картинки находятся в правильной папке:
- Создайте папку `content` в корне проекта
- Поместите картинки в папку `content` или её подпапки

## 2. Обновите конфигурацию

Замените эту строку:
```javascript
ignorePatterns: ["private", "templates", ".obsidian"],
```

На:
```javascript
ignorePatterns: ["private", "templates"],
```

Или добавьте исключение для папки с изображениями, если она у вас есть.

## 3. Проверьте синтаксис Markdown
Убедитесь, что в ваших Markdown-файлах картинки добавлены правильно:
```markdown
![Описание изображения](./путь/к/изображению.jpg)
```

## 4. Добавьте обработку ресурсов
Убедитесь, что плагин `Assets()` включен в emitters (у вас он уже есть):

```javascript
emitters: [
  // ... другие плагины
  Plugin.Assets(), // Этот плагин обрабатывает изображения
  Plugin.Static(),
  // ... остальные плагины
],
```

## 5. Полная проверенная конфигурация

```javascript
import { QuartzConfig } from "./quartz/cfg"
import * as Plugin from "./quartz/plugins"

const config: QuartzConfig = {
  configuration: {
    pageTitle: "Дом, в котором",
    pageTitleSuffix: "",
    enableSPA: true,
    enablePopovers: true,
    analytics: {
      provider: "plausible",
    },
    locale: "ru-RU", // Изменил на русскую локаль
    baseUrl: "quartz.jzhao.xyz",
    ignorePatterns: ["private", "templates"], // Убрал .obsidian
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
      Plugin.Assets(), // Обрабатывает изображения
      Plugin.Static(),
      Plugin.Favicon(),
      Plugin.NotFoundPage(),
      Plugin.CustomOgImages(),
    ],
  },
}

export default config
```

## 6. Дополнительные проверки:

1. **Пути к изображениям**: Убедитесь, что пути относительные и правильные
2. **Форматы изображений**: Поддерживаются JPG, PNG, GIF, SVG, WebP
3. **Сборка**: После изменений пересоберите проект:
   ```bash
   npx quartz build
   ```

Если проблема persists, проверьте консоль браузера на ошибки загрузки ресурсов.