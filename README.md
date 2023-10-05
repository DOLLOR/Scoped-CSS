# Scoped-CSS
An easy way to create scoped CSS
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scoped CSS</title>
  <script>
    /**
     * @param {HTMLStyleElement} styleEl
     */
    const setCSSScope = (styleEl) => {
      styleEl = styleEl || document.all[document.all.length - 2];
      const styleSheet = styleEl.sheet;

      /** @type {HTMLDivElement} */
      const parentEl = styleEl.parentNode;

      setCSSScope.count++;
      const scopeName = setCSSScope.count;

      parentEl.dataset.cssScope = scopeName;
      Array.from(styleSheet.cssRules).forEach((rule) => {
        if (rule.selectorText) {
          /** @type {string} */
          const selectorText = rule.selectorText;
          const newSelector = selectorText.split(',').map(x => `[data-css-scope='${scopeName}'] ${x.trim()}`);
          rule.selectorText = newSelector.join(',');
        }
      });
    };
    setCSSScope.count = 0;
  </script>
</head>

<body>
  <div>
    <!-- this style is available in the div only -->
    <style>
      .red {
        color: red;
      }

      .big,
      .large {
        font-size: 32px;
      }
    </style>
    <script>setCSSScope()</script>

    <div class="red">
      red
      <span class="big">big</span>
    </div>
    <div class="large">large</div>
  </div>

  <div class="red">
    red
    <span class="big">big</span>
  </div>
  <div class="large">large</div>
</body>

</html>
```
