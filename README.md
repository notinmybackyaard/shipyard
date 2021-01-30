# Shipyard <!-- omit in toc -->

Shipyard is an Entity Component System focused on usability and speed.

[![Crates.io](https://img.shields.io/crates/v/shipyard)](https://crates.io/crates/shipyard)
[![Documentation](https://docs.rs/shipyard/badge.svg)](https://docs.rs/shipyard)
[![LICENSE](https://img.shields.io/crates/l/shipyard)](LICENSE-APACHE)

If you have any question or want to follow the development more closely <sub>[![Chat](https://img.shields.io/badge/join-Zulip-brightgreen.svg)](https://shipyard.zulipchat.com)</sub>.

Learning resources:
- [Guide](https://leudz.github.io/shipyard/guide)

## Basic Example <!-- omit in toc -->
```rust
use shipyard::{IntoIter, View, ViewMut, World};

struct Health(u32);
struct Position {
    _x: f32,
    _y: f32,
}

fn in_acid(positions: View<Position>, mut healths: ViewMut<Health>) {
    for (_, mut health) in (&positions, &mut healths)
        .iter()
        .filter(|(pos, _)| is_in_acid(pos))
    {
        health.0 -= 1;
    }
}

fn is_in_acid(_: &Position) -> bool {
    // it's wet season
    true
}

fn main() {
    let mut world = World::new();

    world.add_entity((Position { _x: 0.0, _y: 0.0 }, Health(1000)));

    world.run(in_acid).unwrap();
}

```

## Small Game Example <!-- omit in toc -->

Inspired by Erik Hazzard's [Rectangle Eater](http://erikhazzard.github.io/RectangleEater/).

[![Play](https://img.shields.io/badge/Play-Online-green)](https://leudz.github.io/shipyard/square_eater)
[![Source](https://img.shields.io/badge/View-Source-blue)](square_eater/src/main.rs)

## Table of Contents <!-- omit in toc -->
- [Origin of the name](#origin-of-the-name)
- [Motivation](#motivation)
- [Cargo Features](#cargo-features)
- [License](#license)
- [Contributing](#contributing)

## Origin of the name

Assembly lines take input, process it at each step, and output a result. You can have multiple lines working in parallel as long as they don't bother each other.

Shipyards such as the [Venetian Arsenal](https://en.wikipedia.org/wiki/Venetian_Arsenal) are some of the oldest examples of successful, large-scale, industrial assembly lines.  So successful that it could output a fully-finished ship _every day_.

*Shipyard* is a project you can use to build your own highly-parallel software processes.

## Motivation

I initially wanted to make an ECS to learn how it works. After a failed attempt and some research, I started working on Shipyard.

[Specs](https://github.com/amethyst/specs) was already well established as the go-to Rust ECS but I thought I could do better and went with [EnTT](https://github.com/skypjack/entt)'s core data-structure (`SparseSet`) and grouping model. A very flexible combo.

I wouldn't say Shipyard is better or worse than Specs, it's just different.

## Cargo Features

- **serde1** &mdash; adds (de)serialization support with [serde](https://github.com/serde-rs/serde)
- **std** *(default)* &mdash; lets Shipyard use the standard library
- **thread_local** &mdash; adds methods and types required to work with `!Send` and `!Sync` components

## License

Licensed under either of

 * Apache License, Version 2.0
   ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license
   ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.  

## Contributing

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
