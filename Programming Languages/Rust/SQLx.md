**SQLx is an async, pure-Rust SQL toolkit with compile-time checked queries.**

## What It Is
- Async database library for Rust
- Works with `tokio`, `async-std`
- Not a traditional ORM — you write raw SQL

## Supported Databases
- PostgreSQL
- MySQL / MariaDB
- SQLite

## Key Features

- ✅ **Compile-time query checking** (via macros like `query!`)
- ✅ **Async & non-blocking**
- ✅ **Connection pooling**
- ✅ **Transactions support**
- ✅ **Prepared statements**
- ✅ **Type-safe row mapping**
- ✅ **Offline mode** (no DB needed at compile time after setup)
- ✅ Pure Rust (no C bindings required)

## Important Macros

- `query!()` → Compile-time checked query
- `query_as!()` → Map result into a struct
- `query()` → Runtime-checked query

## Why Use SQLx?

- High performance
- Strong type safety
- Direct SQL control
- Production-ready

**Summary:** SQLx is a lightweight, async, type-safe way to interact with SQL databases in Rust without using a heavy ORM.