**`mod.rs` is a Rust module file used to define a folder as a module.**
In older Rust (before 2018), when you created a folder module, you had to put a `mod.rs` file inside it. That file acted as the **main file for that module**.

Example:

```
src/  
└── utils/       
├── mod.rs   ← defines the `utils` module       
└── math.rs``
```

Inside `mod.rs`, you would declare submodules like:

`pub mod math;`

In modern Rust (2018+), you don’t need `mod.rs` anymore — you can use `utils.rs` instead.