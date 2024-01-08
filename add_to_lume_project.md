# Add this plugin to your Lume project

How to use the [Markdown Run Book](https://deno.land/x/mdrb) for automating the steps of adding `lume_langdata` to your Lume project:

1. If `mdrb` isn't already installed on your computer then run `deno install -Arfn mdrb https://deno.land/x/mdrb/mod.ts`.

1. Open the root directory of your Lume project in a terminal and run `mdrb https://deno.land/x/lume_langdata/add_to_lume_project.md`.

This will automate much of the installation process.

## Automated installation

<details data-mdrb>
<summary>Start the lume project configuration</summary>

<pre>
description = '''
Adding `lume_langdata` to this lume project will modify the `deno.json` and `_config.ts` files.
'''
</pre>
</details>

```ts
// Update deno.json
const deno = JSON.parse(await Deno.readTextFile('./deno.json'));
let fileChanged = false;
if(!deno.imports) deno.imports = {};
if(!deno.imports.lume_langdata) {
  deno.imports.lume_langdata = 'https://deno.land/x/lume_langdata/mod.ts';
  fileChanged = true;
}
if(!deno.compilerOptions) deno.compilerOptions = {};
if(!deno.compilerOptions.types) deno.compilerOptions.types = [];
if(!deno.compilerOptions.types.includes('lume/types.ts')){
  deno.compilerOptions.types.push('lume/types.ts');
  fileChanged = true;
}
if(fileChanged)await Deno.writeTextFile('./deno.json', JSON.stringify(deno, null, 2));

// Update _config.ts
const lume = await Deno.readTextFile('./_config.ts');
if(!lume.includes('lume_langdata')){
  await Deno.writeTextFile('./_config.ts', `import langdata from 'lume_langdata';\n${lume}`);
} 

// Manual step
const completed: boolean = await $.confirm('Last step: add `.use(langdata())` to _config.ts at a suitable place.'); 
if(completed) {
  await $`echo '✅ Project configuration complete.'`; 
}else {
  await $`echo '❌ Project configuration incomplete.'`;
}
```
