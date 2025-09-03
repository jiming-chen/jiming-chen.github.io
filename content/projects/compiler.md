---
date: '2025-09-03T13:04:58-04:00'
title: 'TinyOCaml to WebAssembly Compiler'
draft: false
cover:
    image: "/projects/compiler/compiler.png"
    alt: ""
    relative: false # when usng page bundles set this to true
---

I've wanted to learn about compilers for a while, but it hasn't been offered at Cornell since 2023, so I decided to learn on my own by making this small compiler. It was written in TypeScript and compiles a subset of OCaml (which I'll call TinyOCaml) to WebAssembly (Wasm). Here is the {{< newtabref href="https://tinyocaml2wasm.vercel.app/" title="link to the demo" >}}.

For more details like the exact BNF grammar of the language, check out the {{< newtabref href="https://github.com/jjc256/tinyocaml2wasm" title="GitHub repo" >}}.

You can try your own code, and does parsing and type-checking (using algorithm W). You can then see the abstract syntax tree, intermediate representation, and Wasm code (and run it).

I also used the abstract syntax tree (AST) generated to make a JavaScript interpreter, which I compared against the WebAssembly in several benchmarks.

{{< newimgref src="/projects/compiler/benchmarks.png" alt="Benchmarks" width="50%" >}}
<figcaption>Fig. 1. Several programs and their speedup in Wasm compared to interpreted Javascript.</figcaption>

Interestingly, the speedup was dramatically reduced and sometimes reversed when I tried running the benchmarks from a mobile browser. I think this might have to do with less optimized pipelining when it comes to mobile execution of Wasm code.