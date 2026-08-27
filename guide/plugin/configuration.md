# Configuration

`ossScanPlugin` takes a single options object. Every field is optional.

## Options

| Option             | Type           | Default                                         |
| ------------------ | -------------- | ----------------------------------------------- |
| `selfModules`      | `string[]`     | `[]` — the registered module is always excluded |
| `includeOhpmCache` | `boolean`      | `false`                                         |
| `outputFile`       | `string`       | `src/main/resources/rawfile/osslibraries.<ext>` |
| `format`           | `OutputFormat` | `OutputFormat.JSON`                             |

## selfModules

Modules that belong to the project and should not appear in the license list. The module the plugin is registered on is always excluded automatically; add every other local module here.

```ts
plugins: [ossScanPlugin({ selfModules: ["mylibrary", "3rdlibrary"] })];
```

## includeOhpmCache

Also scan `oh_modules/.ohpm`, the internal package store where OHPM keeps every physical package copy at `.ohpm/<name>@<version>/oh_modules/<name>/`, mounted at `oh_modules/<name>` through symlinks. Transitive dependency versions that are not hoisted to a top-level symlink live only inside the store:

```ts
plugins: [ossScanPlugin({ includeOhpmCache: true })];
```

Declare this option when you need complete coverage: every installed version of a dependency appears in the license data, including conflicting transitive versions listed side by side.

## outputFile

A relative path from the module directory. Set it to write the output somewhere other than the default rawfile location:

```ts
plugins: [ossScanPlugin({ outputFile: "src/main/resources/rawfile/licenses.json" })];
```

## format

The output format: JSON or MessagePack. Prefer the enum members over raw strings for safe renames and autocompletion:

```ts
import { ossScanPlugin, OutputFormat } from "osslibraries-hvigor-plugin";

plugins: [ossScanPlugin({ format: OutputFormat.MessagePack })];
```

Raw strings are accepted too: `'json'` or `'message-pack'`.

The output file extension follows the format unless `outputFile` is set. See [Output Formats](/reference/plugin/output-formats) for the difference between the two.
