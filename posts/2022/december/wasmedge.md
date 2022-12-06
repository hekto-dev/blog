---
title: "WasmEdge Server Runtime"
date: 2022-12-06T10:00:00
author: Sebastian Korfmann
tags:
  - wasm
---

[WebAssembly](https://webassembly.org/), or WASM, is a low-level bytecode language that is designed to be efficient and fast. It has become increasingly popular for use in web development, as it allows developers to run code in the browser that would normally be too slow or complex to execute in JavaScript.

There are several options for running WebAssembly on the server, each with its own strengths and weaknesses. In this blog post, we will briefly look at WasmEdge. In following blog posts we'll look at Wasmtime and Wasmer as well.

<!--more-->

## WasmEdge

[WasmEdge](https://wasmedge.org/), an [open source] project by [SecondState](https://www.secondstate.io/), is a CNCF [sandbox project](https://www.cncf.io/projects/wasmedge-runtime/). It's a WASM runtime build primarily in C++.

They are targeting non-browser environments. For example, Wasmedge provides a [quickjs](https://bellard.org/quickjs/) based [Javascript guest](https://wasmedge.org/book/en/write_wasm/js.html) environment with a Nodejs compatibility layer build on top of it (not full coverage yet). Here's a [recent talk](https://www.youtube.com/watch?v=UogNdp-0Bgs&ab_channel=TheLinuxFoundation) with more background about it.

There are [more directly](https://wasmedge.org/book/en/write_wasm.html) supported ways to write guest applications though. At the time of this writing it's mentioning: C, Rust, JavaScript, Go, Swift, AssemblyScript, Kotlin, Grain and Python.

[SDKs](https://wasmedge.org/book/en/sdk.html) for providing a host are present in C, Go, Node.js, Rust and Python.

The documentation examples are quite good, here's one example for Nodejs

Unfortunately, I haven't really found public examples of production usage for Wasmedge. However, here's a recent Hackernews [thread](https://news.ycombinator.com/item?id=33792322) talking about. Since it's Hackernews, this has to be taken with a grain of salt though. There has been some criticism, that the default settings for embedding a guest are unsafe and need explicit configuration to lock it down.

