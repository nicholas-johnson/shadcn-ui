> [!NOTE]
> This fork of [RustForWeb/shadcn-ui](https://github.com/RustForWeb/shadcn-ui) is maintained. The Leptos packages build on Leptos 0.8.23.

---

<p align="center">
    <a href="./logo.svg">
        <img src="./logo.svg" width="300" height="200" alt="Rust shadcn/ui Logo">
    </a>
</p>

<h1 align="center">Rust shadcn/ui</h1>

Rust port of [shadcn/ui](https://ui.shadcn.com/).

[shadcn/ui](https://ui.shadcn.com/) is a library of beautifully designed components that you can copy and paste into your apps.

## Frameworks

The maintained target is [Leptos](./packages/leptos). Alert, badge, button, card, and the book example compile for `wasm32-unknown-unknown` on Leptos 0.8.23.

Yew components are still in the tree. They are not part of the workspace build, because they depend on crates that are no longer published.

## Documentation

See [the Rust shadcn/ui book](https://shadcn-ui.rustforweb.org).

## Credits

The logo is a combination of the [shadcn/ui logo](https://github.com/shadcn-ui/ui/blob/main/apps/www/components/icons.tsx) and [Ferris the Rustacean](https://rustacean.net/).

## License

This project is available under the [MIT license](LICENSE.md).

## History

This repository started as part of [Rust for Web](https://github.com/RustForWeb). That project marked the port unmaintained in February 2026. This fork continues the Leptos packages.
