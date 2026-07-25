# Deno does not merge compilerOptions between `deno.json(c)` and `tsconfig.json` in a workspace

This repo is a reproduction of an issue in Deno 2.9.4 where workspace and member `compilerOptions` are only merged when both configuration files are the same type. If they are not the same type, deno only uses the `compilerOptions` in the `deno.json(c)` file and ignores the `tsconfig.json`

Each case uses these `compilerOptions`

Workspace:
```json
{
    "lib": [
        "dom"
    ]
}
```

Package:
```json
{
    "rootDirs": [".", "../../../types"]
}
```

And the difference is if it's defined in a `deno.json` or a `tsconfig.json` file

| Workspace     | Member        | Pass `deno check`?            |
|---------------|---------------|-------------------------------|
| tsconfig.json | tsconfig.json | ✅                            |
| tsconfig.json | deno.jsonc    | ❌, `lib` is not applied      |
| deno.jsonc    | tsconfig.json | ❌, `rootDirs` is not applied |
| deno.jsonc    | deno.jsonc    | ✅                            |

<img width="274" height="130" alt="image" src="https://github.com/user-attachments/assets/1a5d9295-970f-42f9-b25c-51c6feac182a" />
