<picture>
    <source srcset="https://raw.githubusercontent.com/leptos-rs/leptos/main/docs/logos/Leptos_logo_Solid_White.svg" media="(prefers-color-scheme: dark)">
    <img src="https://raw.githubusercontent.com/leptos-rs/leptos/main/docs/logos/Leptos_logo_RGB.svg" alt="Leptos Logo">
</picture>

# Leptos Axum Tailwind DaisyUI demo

This is a demo for the [Leptos](https://github.com/leptos-rs/leptos) web framework and the [cargo-leptos](https://github.com/akesson/cargo-leptos) tool using [Axum](https://github.com/tokio-rs/axum), [Tailwind CSS](https://tailwindcss.com/), and [DaisyUI](https://daisyui.com/).

Note: This is not an official demo.

# Steps to reproduce:

1. ### Create project (Leptos + Axum template)
	1. `cargo leptos new --git https://github.com/leptos-rs/start-axum`
		1. details: https://github.com/leptos-rs/start-axum
	2. `cd` into new project then:
		1. `rustup toolchain install nightly --allow-downgrade`
		2. `rustup target add wasm32-unknown-unknown`
		3. `cargo install cargo-generate`
2. ### Add Tailwind
	1. Add the following to Cargo.toml
		```
		# The tailwind input file.
		#
		# Optional, Activates the tailwind build
		tailwind-input-file = "style/tailwind.css"
		```
		1. Parameter list here: https://github.com/leptos-rs/cargo-leptos
	2. Remove/comment this line in `Cargo.toml`
		```
		style-file = "style/main.scss"
		```
	3. Delete `style/main.scss`
	4. In `src/app.rs`, update the HomePage() component `view!` macro contents to:
		```
		<h1 class="m-8 text-4xl">"Welcome to Leptos!"</h1>  
		<button class="mx-8 h-auto p-4 border rounded-md bg-purple-400 hover:bg-purple-700" on:click=on_click>"Click Me: " {count}</button>
		```
    5. `cargo run watch` to test Tailwind CSS functionality
3. ### (Optional) Install daisyUI
	1. (details https://daisyui.com/docs/install/standalone/#2-get-daisyui-bundled-js-file)
		1. From project root run:
			```
			curl -sLO https://github.com/saadeghi/daisyui/releases/latest/download/daisyui.js
			curl -sLO https://github.com/saadeghi/daisyui/releases/latest/download/daisyui-theme.js
			```
		2. Update `style/tailwind.css`:
			```
			@import "tailwindcss" source(none);
			@plugin "./daisyui.js";
			
			/* Optional for custom themes – Docs: https://daisyui.com/docs/themes/#how-to-add-a-new-custom-theme */
			@plugin "./daisyui-theme.js"{
			  /* custom theme here */
			  themes: light --default, dark --prefersdark, cupcake;
			}
			```
		3. In `src/app.rs`, update the HomePage() component `view!` macro contents to:
		```
        <main class="my-0 mx-auto max-w-3xl text-center">
            <h2 class="p-6 text-4xl underline decoration-wavy">"Welcome to Leptos with Tailwind"</h2>
            <p class="px-10 pb-10 text-left">"Tailwind will scan your Rust files for Tailwind class names and compile them into a CSS file."</p>
            <button
                class="bg-sky-600 hover:bg-sky-700 px-5 py-3 text-white rounded-lg"
                on:click=move |_| *set_count.write() += 1
            >
                {move || if count.get() == 0 {
                    "Click me!".to_string()
                } else {
                    count.get().to_string()
                }}
            </button>
        </main>
		```
4. ### VSCode config:
	1. Add to VSCode `.vscode/settings.json`
	```
    "rust-analyzer.procMacro.ignored": {
        "leptos_macro": [
            // optional:
            // "component",
            "server"
        ],
    },
    // if code that is cfg-gated for the `ssr` feature is shown as inactive,
    // you may want to tell rust-analyzer to enable the `ssr` feature by default
    //
    // you can also use `rust-analyzer.cargo.allFeatures` to enable all features
    "rust-analyzer.cargo.features": ["ssr"],
	{
	    "emmet.includeLanguages": {
	        "rust": "html",
	        "*.rs": "html"
	    },
	    "tailwindCSS.includeLanguages": {
	        "rust": "html",
	        "*.rs": "html"
	    },
	    "files.associations": {
	        "*.rs": "rust"
	    },
	    "editor.quickSuggestions": {
	        "other": "on",
	        "comments": "on",
	        "strings": true
	    },
	    "css.validate": false
	}
	```
	2. Install [Tailwind CSS Intellisense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss).
5. ### Leptos DX
   1. `cargo install leptosfmt`
      1. default formatting from project root: `leptosfmt ./**/*.rs`
   2. [Use --cfg=erase_components during development](https://book.leptos.dev/getting_started/leptos_dx.html#4-use---cfgerase_components-during-development)