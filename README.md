# rust-tilejson

[![Build Status](https://travis-ci.org/stepankuzmin/rust-tilejson.svg)](https://travis-ci.org/stepankuzmin/rust-tilejson)
[![tilejson on Crates.io](https://meritbadge.herokuapp.com/tilejson)](https://crates.io/crates/tilejson)

[Documentation](https://docs.rs/tilejson/)

Library for serializing the [TileJSON](https://github.com/mapbox/tilejson-spec) file format

## Examples

### Reading

```rust
extern crate tilejson;
extern crate serde_json;

use tilejson::TileJSON;

fn main() {
    let tilejson_str = r#"{
        "tilejson": "2.2.0",
        "name": "compositing",
        "scheme": "tms",
        "tiles": [
            "http://localhost:8888/admin/1.0.0/world-light,broadband/{z}/{x}/{y}.png"
        ]
    }"#;

    let tilejson: TileJSON = serde_json::from_str(&tilejson_str).unwrap();
    println!("{:?}", tilejson);
}
```

### Writing

Using builder pattern

```rust
extern crate tilejson;
extern crate serde_json;

use tilejson::TileJSONBuilder;

fn main() {
    let mut tilejson_builder = TileJSONBuilder::new();

    tilejson_builder.name("tileset name");
    tilejson_builder.description("some description");

    let tiles = vec!["http://localhost:8888/admin/1.0.0/world-light,broadband/{z}/{x}/{y}.png"];
    tilejson_builder.tiles(tiles);

    let tilejson = tilejson_builder.finalize();
    let serialized_tilejson = serde_json::to_string(&tilejson).unwrap();

    println!("{}", serialized_tilejson);
}
```