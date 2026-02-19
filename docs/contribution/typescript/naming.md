---
title: Naming
sidebar_position: 2
---

# Naming

## File & Folder Naming rules
Component tsx files: `UpperCamelCase`  
Custom Hooks: `lowerCamelCase` (i.e. useHookname)  
All other files and folders: `kebab-case` (i.e. util files)  

## Identifier Rules
:::note Identifier Naming Conventions
**UpperCamelCase identifiers:**
- Class  
- Interface  
- Type  
- Enum  
- Component functions in TSX  

**lowerCamelCase identifiers:**
- Variable  
- Parameter  
- Function  
- Method  
- Props  
:::

**Note:** since python will use snake_case, it is okay to use snake_case when referring to keys
directly from a request to the backend. However this is only if it directly used, if there is
some data transformation involved, always switch to lowerCamelCase.

## Abbreviations
Always treat abbreviations like words
```typescript
let loadHttpUrl = 'url here' // good!
let loadHTTPURL = 'url here' // bad
```
