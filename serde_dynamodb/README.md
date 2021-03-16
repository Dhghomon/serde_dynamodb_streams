#  serde_dynamodb [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Build Status](https://travis-ci.org/james-tru/serde_dynamodb_streams.svg?branch=master)](https://travis-ci.org/james-tru/serde_dynamodb_streams) [![Build status](https://ci.appveyor.com/api/projects/status/jnckw7ll01cberis?svg=true)](https://ci.appveyor.com/project/mockersf/serde-dynamodb) [![Coverage Status](https://coveralls.io/repos/github/james-tru/serde_dynamodb_streams/badge.svg?branch=master)](https://coveralls.io/github/james-tru/serde_dynamodb_streams?branch=master) [![Realease Doc](https://docs.rs/serde_dynamodb/badge.svg)](https://docs.rs/serde_dynamodb) [![Crate](https://img.shields.io/crates/v/serde_dynamodb.svg)](https://crates.io/crates/serde_dynamodb)

Library to de/serialize an object to an `HashMap` of `AttributeValue`s used by [rusoto_dynamodb](https://crates.io/crates/rusoto_dynamodb) to manipulate objects saved in dynamodb using [serde](https://serde.rs)

This is just a fork of https://github.com/mockersf/serde_dynamodb with the dynamodb bits taken out, so that streams can handle rustls

```toml
[dependencies]
serde_dynamodb_streams = "0.2.1"
```

## Example

```rust
#[derive(Serialize, Deserialize)]
struct Todo {
    id: uuid::Uuid,
    title: &'static str,
    done: bool,
}

let todo = Todo {
    id: uuid::Uuid::new_v4(),
    title: "publish crate",
    done: false,
};

let put_item = PutItemInput {
    item: serde_dynamodb::to_hashmap(&todo).unwrap(),
    table_name: "todos".to_string(),
    ..Default::default()
};

let client = DynamoDbClient::simple(Region::UsEast1);
client.put_item(&put_item).unwrap();
```

