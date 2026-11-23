> [!NOTE]
> This is a fork of [RustForWeb/shadcn-ui](https://github.com/RustForWeb/shadcn-ui), continued here because the original project was unmaintained. The Leptos packages build on Leptos 0.8.23.

---

<p align="center">
    <a href="./logo.svg">
        <img src="./logo.svg" width="300" height="200" alt="Rust shadcn/ui Logo">
    </a>
</p>

<h1 align="center">Rust shadcn/ui</h1>

Rust port of [shadcn/ui](https://ui.shadcn.com/).

[shadcn/ui](https://ui.shadcn.com/) is a library of beautifully designed components that you can copy and paste into your apps.

The maintained components are Alert, Badge, Button, and Card. Each one has a `default` style and a `new_york` style. The component names are the same in both. Examples below use `default`.

Yew components are still in the tree. They are not part of the workspace build, because they depend on crates that are no longer published.

## Use

The crates are not published. Depend on them from this repository, in a Leptos 0.8 app:

```toml
[dependencies]
leptos = { version = "0.8.23", features = ["csr"] }
leptos-node-ref = "0.2"
leptos-struct-component = "0.2"
leptos-style = "0.2"
lucide-leptos = { version = "2", features = ["arrows", "development", "layout", "notifications"] }
tailwind_fuse = { version = "0.3", features = ["variant"] }

shadcn-ui-leptos-alert = { git = "https://github.com/nicholas-johnson/shadcn-ui", package = "shadcn-ui-leptos-alert" }
shadcn-ui-leptos-badge = { git = "https://github.com/nicholas-johnson/shadcn-ui", package = "shadcn-ui-leptos-badge" }
shadcn-ui-leptos-button = { git = "https://github.com/nicholas-johnson/shadcn-ui", package = "shadcn-ui-leptos-button" }
shadcn-ui-leptos-card = { git = "https://github.com/nicholas-johnson/shadcn-ui", package = "shadcn-ui-leptos-card" }
```

`leptos-struct-component` is only required by Button. `lucide-leptos` is only required when an example draws an icon. Icon modules are feature-gated: `Terminal` is `development`, `CircleAlert`, `BellRing`, and `Check` are `notifications`, `ChevronRight` is `arrows`, and `LoaderCircle` is `layout`.

Tailwind has to see the class strings, which live in the component source. A path dependency in this workspace can point `content` at `packages/leptos/*/src/*.rs`, which is what the book example does. A git dependency is checked out under `~/.cargo/git/checkouts/`, and that directory has to be on `content` too. Copying `packages/leptos/<name>/src/default.rs` into your crate avoids that, and `content` can stay `./src/**/*.rs`.

The book example is [book-examples/leptos/tailwind.config.js](./book-examples/leptos/tailwind.config.js) and [book-examples/leptos/style/tailwind.css](./book-examples/leptos/style/tailwind.css). Copy the CSS variables from that stylesheet, and extend the theme like this:

```js
module.exports = {
    darkMode: ["class"],
    content: ["./src/**/*.rs", "./index.html"],
    theme: {
        extend: {
            colors: {
                border: "hsl(var(--border))",
                input: "hsl(var(--input))",
                ring: "hsl(var(--ring))",
                background: "hsl(var(--background))",
                foreground: "hsl(var(--foreground))",
                primary: { DEFAULT: "hsl(var(--primary))", foreground: "hsl(var(--primary-foreground))" },
                secondary: { DEFAULT: "hsl(var(--secondary))", foreground: "hsl(var(--secondary-foreground))" },
                destructive: { DEFAULT: "hsl(var(--destructive))", foreground: "hsl(var(--destructive-foreground))" },
                muted: { DEFAULT: "hsl(var(--muted))", foreground: "hsl(var(--muted-foreground))" },
                accent: { DEFAULT: "hsl(var(--accent))", foreground: "hsl(var(--accent-foreground))" },
                card: { DEFAULT: "hsl(var(--card))", foreground: "hsl(var(--card-foreground))" },
            },
            borderRadius: {
                lg: "var(--radius)",
                md: "calc(var(--radius) - 2px)",
                sm: "calc(var(--radius) - 4px)",
            },
        },
    },
};
```

A path dependency inside this workspace can point `content` at `packages/leptos/*/src/*.rs` instead. The book example does that.

Import a style module and render the component. `class`, `id`, `style`, and `node_ref` are optional on every component.

```rust
use leptos::prelude::*;
use shadcn_ui_leptos_button::default::Button;

#[component]
fn App() -> impl IntoView {
    view! {
        <Button>"Save"</Button>
    }
}
```

For `new_york`, change `default` to `new_york` in the import. The working examples in this repo are under [book-examples/leptos](./book-examples/leptos).

## Button

`shadcn_ui_leptos_button::default::{Button, ButtonVariant, ButtonSize, ButtonChildProps}`

Variants are `Default`, `Destructive`, `Outline`, `Secondary`, `Ghost`, and `Link`. Sizes are `Default`, `Sm`, `Lg`, and `Icon`.

```rust
use leptos::prelude::*;
use lucide_leptos::{ChevronRight, LoaderCircle};
use shadcn_ui_leptos_button::default::{Button, ButtonChildProps, ButtonSize, ButtonVariant};

#[component]
fn Buttons() -> impl IntoView {
    view! {
        <Button>"Button"</Button>
        <Button variant={ButtonVariant::Destructive}>"Destructive"</Button>
        <Button variant={ButtonVariant::Outline}>"Outline"</Button>
        <Button variant={ButtonVariant::Secondary}>"Secondary"</Button>
        <Button variant={ButtonVariant::Ghost}>"Ghost"</Button>
        <Button variant={ButtonVariant::Link}>"Link"</Button>

        <Button size={ButtonSize::Sm}>"Small"</Button>
        <Button size={ButtonSize::Lg}>"Large"</Button>
        <Button variant={ButtonVariant::Outline} size={ButtonSize::Icon}>
            <ChevronRight attr:class="h-4 w-4" />
        </Button>

        <Button disabled=true>
            <LoaderCircle attr:class="mr-2 h-4 w-4 animate-spin" />
            "Please wait"
        </Button>

        <Button as_child={Callback::new(|ButtonChildProps { class, .. }| {
            view! { <a class={class} href="/login">"Login"</a> }.into_any()
        })} />
    }
}
```

## Badge

`shadcn_ui_leptos_badge::default::{Badge, BadgeVariant}`

Variants are `Default`, `Secondary`, `Destructive`, and `Outline`.

```rust
use leptos::prelude::*;
use shadcn_ui_leptos_badge::default::{Badge, BadgeVariant};

#[component]
fn Badges() -> impl IntoView {
    view! {
        <Badge>"Badge"</Badge>
        <Badge variant={BadgeVariant::Secondary}>"Secondary"</Badge>
        <Badge variant={BadgeVariant::Destructive}>"Destructive"</Badge>
        <Badge variant={BadgeVariant::Outline}>"Outline"</Badge>
    }
}
```

## Alert

`shadcn_ui_leptos_alert::default::{Alert, AlertTitle, AlertDescription, AlertVariant}`

Variants are `Default` and `Destructive`. Put an icon first, then `AlertTitle` and `AlertDescription`. The alert positions that icon.

```rust
use leptos::prelude::*;
use lucide_leptos::{CircleAlert, Terminal};
use shadcn_ui_leptos_alert::default::{Alert, AlertDescription, AlertTitle, AlertVariant};

#[component]
fn Alerts() -> impl IntoView {
    view! {
        <Alert>
            <Terminal attr:class="h-4 w-4" />
            <AlertTitle>"Heads up!"</AlertTitle>
            <AlertDescription>
                "You can add components to your app using the cli."
            </AlertDescription>
        </Alert>

        <Alert variant={AlertVariant::Destructive}>
            <CircleAlert attr:class="h-4 w-4" />
            <AlertTitle>"Error"</AlertTitle>
            <AlertDescription>
                "Your session has expired. Please log in again."
            </AlertDescription>
        </Alert>
    }
}
```

## Card

`shadcn_ui_leptos_card::default::{Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter}`

```rust
use leptos::prelude::*;
use lucide_leptos::{BellRing, Check};
use shadcn_ui_leptos_button::default::Button;
use shadcn_ui_leptos_card::default::{
    Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle,
};

#[component]
fn NotificationCard() -> impl IntoView {
    view! {
        <Card class="w-[380px]">
            <CardHeader>
                <CardTitle>"Notifications"</CardTitle>
                <CardDescription>"You have 3 unread messages."</CardDescription>
            </CardHeader>
            <CardContent class="grid gap-4">
                <div class="flex items-center space-x-4 rounded-md border p-4">
                    <BellRing />
                    <div class="flex-1 space-y-1">
                        <p class="text-sm font-medium leading-none">"Push Notifications"</p>
                        <p class="text-sm text-muted-foreground">"Send notifications to device."</p>
                    </div>
                </div>
            </CardContent>
            <CardFooter>
                <Button class="w-full">
                    <Check />
                    " Mark all as read"
                </Button>
            </CardFooter>
        </Card>
    }
}
```

## Documentation

See [the Rust shadcn/ui book](https://shadcn-ui.rustforweb.org).

## Credits

[Daniëlle Huisman](https://github.com/DanielleHuisman) created this project as part of [Rust for Web](https://github.com/RustForWeb).

The logo is a combination of the [shadcn/ui logo](https://github.com/shadcn-ui/ui/blob/main/apps/www/components/icons.tsx) and [Ferris the Rustacean](https://rustacean.net/).

## License

This project is available under the [MIT license](LICENSE.md).

## History

[Daniëlle Huisman](https://github.com/DanielleHuisman) created the port under [Rust for Web](https://github.com/RustForWeb). The original repository was marked unmaintained in February 2026. This fork exists so the Leptos packages can keep going.
