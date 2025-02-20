---
category: advanced
path: /advanced/pnpify
title: "PnPify"
---

```toc
# This code block gets replaced with the Table of Contents
```

## Motivation

Plug'n'Play is, by design, compatible with all projects that only make use of the `require` API - whether it's `require`, `require.resolve`, or `createRequireFromPath`. However, some rare projects prefer to reimplement the Node resolution themselves and as such aren't compatible by default with our environment (unless they integrate their resolvers with the [PnP API](/advanced/pnpapi)).

## PnPify

PnPify is a tool designed to workaround these compatibility issues. It's not perfect - it brings its own set of caveats and doesn't allow you to leverage all the features that PnP has to offer - but it's often good enough to unblock you until better solutions are implemented.

How it works is simple: when a non-PnP-compliant project tries to access the `node_modules` directories (for example through `readdir` or `readFile`), PnPify intercepts those calls and converts them into calls to the PnP API. Then, based on the result, it simulates the existence of a virtual `node_modules` folder that the underlying tool will then consume - still unaware that the files are extracted from a virtual filesystem.

## Usage

1. Add PnPify to your dependencies:

```bash
yarn add @yarnpkg/pnpify
```

2. Use pnpify to run the incompatible tool:

```bash
yarn pnpify tsc
```

## Caveat

- Due to how PnPify emulates the `node_modules` directory, some problems are to be expected, especially with tools that watch directories inside `node_modules`.

- PnPify isn't designed to be a long-term solution; its purpose is purely to help projects during their transition to the stricter Plug'n'Play module resolution scheme. Relying on PnPify doesn't allow you to take full advantage of everything Plug'n'Play has to offer, in particular perfect flattening and boundary checks.

## IDE Support

When using Plug'n'Play installs with your favorite text editors you will probably want to keep using your extensions, like ESLint or Prettier. To do so, you may need to use `yarn pnpify --sdk`. For more information, consult the detailed documentation in the [editor sdks](/advanced/editor-sdks) section.
