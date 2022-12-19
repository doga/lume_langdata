# lume_langdata

A [Lume](https://lume.land) plugin for developing multi-language websites.
Lume is a static-site generator for the [Deno](https://deno.land) JavaScript/TypeScript runtime.

lume_langdata automates the creation of language-related [shared data for Lume projects](https://lume.land/docs/creating-pages/shared-data/#the-_data-directories).

## Usage

Call lume_langdata from your [Lume project's configuration file](https://lume.land/docs/configuration/config-file/):

```ts
// _config.ts

import lume from 'lume/mod.ts';
import lume_langdata from 'https://deno.land/x/lume_langdata@v1.0.4/mod.ts';

export default
lume({
  location: new URL('https://site.example'),
  src     : './src',
  dest    : './build',
})
.use(lume_langdata());
```

## Lume project directory structure

lume_langdata assumes that your [Lume project's source directory](https://lume.land/docs/configuration/config-file/#src) contains one directory for each language. The directory name must be a [ISO 639-1 language code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes). lume_langdata will ignore directories with a non-conforming name.

For example, if your source directory contains the following directories and files:

- `en`
- `tr`
- `assets`
- `index.html`

then lume_langdata will ignore the `assets` directory and the `index.html` file. Note that the name of the language directories must be lower-cased.

## Which shared data is generated?

Given the source directory structure shown above, lume_langdata will generate the following data files. Note that the generated data files will modify your source directory.

### List of site languages

In this example, a file named `languages.json` will be generated in the source directory, containing:

```json
["en","tr"]
```

so that the source directory will then look as follows:

- `languages.json`
- `en`
- `tr`
- `assets`
- `index.html`

The main use case for `languages.json` is redirecting the user to his/her preferred language. For example, `index.html` could contain JavaScript code such as:

```js
fetch('/languages.json')
.then((response) => response.json())
.then((siteLanguages) => {
  let lang = siteLanguages[0];

  // Fine-tune lang here ...

  window.location.assign(`/${lang}/`);
}
```

### Shared data for each language directory

lume_langdata will generate the following files:

- `en/_data/lang.yaml`
- `tr/_data/lang.yaml`

For example, `en/_data/lang.yaml` will contain:

```yaml
code: en
```

The main use case for this shared data is the localization of [Lume layouts](https://lume.land/docs/getting-started/create-a-layout/):

```html
<!DOCTYPE html>
<html lang="{{ lang.code }}">
  <!-- ... -->
</html>
```

## License

lume_langdata is released under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).
