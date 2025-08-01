CatBoost Rust Package
======================

### Prerequisites

The minimal supported Rust version is 1.64.0 .

CatBoost Rust package uses `catboost-sys` crate inside that is a wrapper around [`libcatboostmodel` library](https://catboost.ai/docs/en/concepts/c-plus-plus-api_dynamic-c-pluplus-wrapper) with an exposed C API.
In order to build it some environment setup is necessary. Modern versions of CatBoost use CMake build system, build environment setup for CMake is described [here](https://catboost.ai/docs/en/installation/build-environment-setup-for-cmake), CatBoost versions before 1.2 used Ya Make build system, build environment setup for YaMake is described [here](https://catboost.ai/docs/en/installation/build-environment-setup-for-ya-make).

### Basic usage example

1. Add a dependency to your Cargo.toml:
```
[dependencies]
catboost = { git = "https://github.com/catboost/catboost" }
```

2. Now you can apply pretrained model in your code:
```rust
// Bring catboost module into the scope
use catboost;

fn main() {
    // Load the trained model
    let model = catboost::Model::load("tmp/model.bin").unwrap();

    println!("Number of cat features {}", model.get_cat_features_count());
    println!("Number of float features {}", model.get_float_features_count());

    // Apply the model
    let prediction = model
        .calc_model_prediction(
            vec![
                vec![-10.0, 5.0, 753.0],
                vec![30.0, 1.0, 760.0],
                vec![40.0, 0.1, 705.0],
            ],
            vec![
                vec![String::from("north")],
                vec![String::from("south")],
                vec![String::from("south")],
            ],
        )
        .unwrap();
    println!("Prediction {:?}", prediction);
}
```

### Documentation
Run `cargo doc --open` in `catboost/rust-package` directory.

### Tests

Run `cargo test` in `catboost/rust-package` directory.
