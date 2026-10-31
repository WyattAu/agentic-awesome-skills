> **IMPORTANT:** Only use the flags documented below. Do not invent or guess flags; this snapshot is not exhaustive. Verify missing or version-sensitive flags against the approved CLI's help and current official docs. The CLI auto-detects the package manager from the project's lockfile; there is no `--package-manager` flag.
### `init`: Initialize or create a project
| `--template <template>` | `-t`  | Template (next, start, vite, next-monorepo, react-router) |;       |
| `--preset [name]`       | `-p`  | Preset configuration (named, code, or URL)                |;       |
| `--name <name>`         | `-n`  | Name for new project                                      |;       |
| `--rtl`                 |       | Enable RTL support                                        |;       |
| `--monorepo`            |       | Scaffold a monorepo project                               |;       |
| `--no-monorepo`         |       | Skip the monorepo prompt                                  |;       |
### `add`: Add components
| `--path <path>` | `-p`  | Target path for the component                                                                                        |;       |
| `--diff [path]` |       | Show diffs. Without a path, shows the first 5 files. With a path, shows that file only (implies `--dry-run`)         |;       |
| `--view [path]` |       | Show file contents. Without a path, shows the first 5 files. With a path, shows that file only (implies `--dry-run`) |;       |
- When the user asks "what files will this add?" or "what will this change?"; use `--dry-run`.
- Before overwriting existing components; use `--diff` to preview the changes first.
- When the user wants to inspect component source code without installing; use `--view`.
- When checking what CSS changes would be made to `globals.css`; use `--diff globals.css`.
- When the user asks to review or audit third-party registry code before installing; use `--view` to inspect the source.
### `search`: Search registries
| `--query <query>`   | `-q`  | Search query           |;       |
### `view`: View item details
### `docs`: Get component documentation URLs
### `diff`: Check for updates
### `info`: Project information
| `base`               | `string`  | Primitive library (`radix` or `base`); determines component APIs and available props      |
| `tailwind.css`       | `string`  | Global CSS path; this is where custom CSS variables go                                    |
| `iconLibrary`        | `string`  | Icon library; determines icon import package (e.g. `lucide-react`, `@tabler/icons-react`) |
### `build`: Build a custom registry
> **IMPORTANT:** Never try to decode, fetch, or resolve preset codes manually. Preset codes are opaque; pass them directly to `npx shadcn@latest init --preset <code>` and let the CLI handle resolution.