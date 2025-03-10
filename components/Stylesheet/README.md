# Astro Stylesheet Component

A simple component to help abstract the monotony of adding stylesheets to any Astro project.

To use simply:

```bash
npm i astro-stylesheet -D
```

Within your `./src/layouts/**.astro` layout file or `./src/pages/**.astro`, apply the following:

```astro
---
import Stylesheet from 'astro-stylesheet'

---
<html>
  <head>
  <!-- Example of Single use of component -->
    <Stylesheet 
        href = "./styles/global.css" 
        media = "screen"
        sanitize = 'all'
    />
    <!-- Example for managing multiple stylesheets -->
    <Stylesheet 
        list: Array<Object> = {
          [
            {
              href: "./styles/global.css", 
              media: "screen"
            },
            {
              href: Astro.resolve('./src/components/print.css'),
              media:  "print"
            },
            {
              href :  "npm:bootstrap@next/dist/css/bootstrap.min.css",
              media : "screen and (max-width:600px)"
            }
          ]
        }
       <!-- Apply sanitize.css stylesheets -->
        sanitize: SanitizeList =  "all" | "bare"| "forms"| "assets"| "typography"|
                            "reducedMotion"| "sysUI"| "modern" |"monoUI"
       <!-- Select Multiple sanitize.css stylesheets -->
        sanitize: SanitizeList = "forms , assets, typography , reducedMotion , sysUI , modern ,monoUI"
    />
 </head>
</html>

```

The `<Stylesheet/>` component would then populate all the relevant `<link rel='stylesheet' href='' type='text/css' media=''>` required in the `<head>` of the `page.astro` or `layout.astro` file.

This component allows you to either reference a single Stylesheet, or a group of stylesheets together into a `list` as demonstrated above.

## Props `:Props`

The `props` interface is a really straight forward to use. It follows these shapes,

```ts
type PropsAttributes = {
    href: "npm:" | string ,
    media?: string,
    preload?,
    alternative?,
    title?:string,
    cors?:'anonymous'|'use-credentials',
    sanitize?:SanitizedList
}

```

### `href:string`

The `href` attribute, captures the hyperlink reference to the CSS file. This could be stored either within the `public` directory i.e:`./public/styles.css` and referenced as `./styles.css`. Or alternatively use `Astro.resolve()` to resolve files located within the `./src/*` directory.

The `href` also allows to link to any `https://` or CDN to obtain the desired CSS file.

> ❗ All Files must be a `type='text/css'` file ending in `*.css`

The `href` can also source css files located within npm packages. By utilising [Skypack](https://www.skypack.dev/) to obtain the CSS files you can access stylesheets from other css frameworks to name but a few:

- [Bootstrap](https://www.skypack.dev/view/bootstrap) `npm:bootstrap/css/bootstrap.min.css`
- [Bulma](https://www.skypack.dev/view/bulma) `npm:bulma/css/bulma.css`
- [SakuraCSS](https://www.skypack.dev/view/sakura.css) `npm:sakuraCss/sakura.css`

### **⚠️ Caution**

This only works for addresses that route directly to a stylesheet, if your desired framework requires additional `<script>` tags to work, then this would not be supportive of your endeavours, sorry.

### `media?:string`

This media directive allows for stylesheets that are media query specific to be applied with this `<Stylesheet>` ComponentAPI.

Using the same syntax as one normally would to direct such things. For further information see [MDN Stylesheet.media](https://developer.mozilla.org/en-US/docs/Web/API/StyleSheet/media)

### `preload?`

This `preload` attribute allows you to stipulate if that particular stylesheet should be preloaded ahead of everything else. For further information on Preloading Stylesheet see [MDN Preload](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload)

```astro
<Stylesheet ... preload />
```

### `alternative?`

This allows you to designate alternative stylesheets to let uses see alternative versions of the page based on their needs or preferences. To find out more see [MDN Alternative Stylesheets](https://developer.mozilla.org/en-US/docs/Web/CSS/Alternative_style_sheets)

### `title? : string`

This accepts a named title to your `alternative` stylesheet. These two attributes must be used together.

```astro
<Stylesheet ... alternative title="Alternative Stylesheet"/>
```

### `cors? : 'anonymous' | 'use-credentials'`

This allows you to specify which type of CORS policy you wish to use for obtaining external resources.

## `sanitize? : SanitizedList`

The `<Stylesheet>` component is tightly coupled with the [`sanitizer.css`](https://csstools.github.io/sanitize.css/) project. This project alongside its sister project [`normalize.css`](https://github.com/csstools/normalize.css), helps to provide a consistent cross-browser CSS library. Helping to give developers a default styling experience.

To utilize Sanitize.css with this component, simply designate which of the Sanitize.css packages you wish to add to your site.

```ts
type SanitizeList =
        "all" |
        "bare"|
        "forms"|
        "assets"|
        "typography"|
        "reducedMotion"|
        "sysUI"|
        "modern"|
        "monoUI"

```

```astro
<Stylesheet
  href="/styles.css"
  <!-- apply all the sanitize.css sheets -->
  sanitize="all"
  <!-- apply multiple selective sanitizer sheets -->
  sanitize="modern,forms,assets"
/>
```

This would then apply the relevant set of Sanitizer links requested to the `<head>` of the document.

### ChangeLog

- Added [modern-css-reset](https://www.skypack.dev/view/modern-css-reset) as requested by [jasikpark](https://github.com/jasikpark)
- Refactored Codebase,
- Added better TypeSupport and Errors
- Added Single use of component provided by [Olyno](https://github.com/Olyno)

## Credits

This project was largely inspired and assisted by [jonathantneal](https://github.com/jonathantneal) from [csstools/sanitize.css](https://github.com/csstools/sanitize.css). Please look to support their project by giving them a star on github, it would really mean the world to them.

Special Thanks to Olyno for augmenting the component with the Single Use functionality.
