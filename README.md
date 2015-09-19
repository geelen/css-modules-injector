# CSS Modules Injector

A simple HTML parser & CSS Modules injector. Working with the following HTML:

```html
<html>
  <head>
    <link css-modules-inject>
    /* or <style css-modules-inject></style> if you want to inline the output */
  </head>
  <body module="main.css" styleName="body">
    <header module="header.css" styleName="outer">
      <h1 styleName="first">CSS Modules</h1>
      <h2 styleName="second">Statically compiled!</h2>
    </header>
    <section module="copy.css" styleName="outer">
      <p styleName="p first">Welcome to the future of CSS</p>
      <p styleName="p">We've got fun and games</p>
    </section>
  </body>
</html>
```

It compiles the CSS Modules referenced and injects their generated class names:

```html
<html>
  <head>
    <link href="styles.css"> /* Has the compiled output */
  </head>
  <body class="main_body_abcd1234">
    <header class="header_outer_bcde2345">
      <h1 class="header_first_cdef3456 type_h1_defa4567">CSS Modules</h1>
      <h2 class="header_second_efab5678 type_h2_fabc6789">Statically compiled!</h2>
    </header>
    <section class="copy_outer_abcd7890 layout_p1_bcde8901">
      <p class="copy_p_cdef9012 copy_first_defa0123 layout_mb1_efab1234">Welcome to the future of CSS</p>
      <p class="copy_p_cdef9012 layout_mb1_efab1234">We've got fun and games</p>
    </section>
  </body>
</html>
```

### Usage

Use the command-line API:
 
```
css-modules-injector --moduleDir "styles/modules" --outDir dist index.html
```

Or the Node API:

```
import fs from "fs"
import CMInjector from "css-modules-injector"

let injector = new CMInjector({ moduleDir: __dirName + "/styles/modules", cssFileName: "styles.css" })
let html = fs.readFileSync(__dirName + "/index.html")

injector.inject(html).then(output => {
  let { processedHTML, compiledCSS } = output
  fs.writeFileSync(processedHTML, __dirName + "/dist/index.html")
  fs.writeFileSync(compiledCSS, __dirName + "/dist/styles.css")
})
```

### Nested Modules

By default, all `styleName` properties look to the nearest parent (or themselves) that define a `module`. This encourages full encapsulation by default — a tag should only be referencing styles from a single module, and you should use Composition for style reuse.

However, if you do want to reference a style in an *outer* module, the following syntax is supported:

```html
<body module-main="main.css" styleName="main.body">
  <header module="header.css" styleName="main.bannerText outer">
    <h1 styleName="main.font-replace first">CSS Modules</h1>
    <h2 styleName="second">Statically compiled!</h2>
  </header>
  ...
</body>
```

### Todo

- Everything but the readme.
