# rust-nightly

A Docker image that:

- Installs Rust nightly in a very slim image with minimal deps
- Copies your `Cargo.toml` and `Cargo.lock` first and builds it
  so that dependencies are cached and then re-used
- Finally copies the rest of your source code and builds it

Hosted on Docker Hub as [zeithq/rust-nightly](https://hub.docker.com/r/zeithq/rust-nightly/)

## How to use

Create a new directory with

```bash
cargo new --bin my-project
```

And then ensure the following files more or less match:

**Dockerfile**

```
FROM zeithq/rust-nightly
EXPOSE 80
CMD ROCKET_ENV=production cargo run --release
```

**Cargo.toml**

```toml
[package]
name = "my-project"
version = "0.1.0"

[dependencies]
rocket = "0.1.4"
rocket_codegen = "0.1.4"
```

**src/main.rs**

```rust
#![feature(plugin)]
#![plugin(rocket_codegen)]

extern crate rocket;

fn main() {
    rocket::ignite().mount("/", routes![index]).launch()
}

#[get("/")]
fn index() -> &'static str {
    "▲ Hello Rocket.rs!"
}
```

Then, [install now](https://zeit.co/install) and deploy with one command:

```bash
now
```

The `Dockerfile` is synced and built on your behalf!
