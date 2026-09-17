# Windows bindings

`src/bindings.rs` contains only the Windows APIs used by winreg, generated with
`windows-bindgen` 0.100.0 and linked through `windows-link` 0.100.0. The generator
is a separate developer tool; building winreg does not run it or depend on it.

To update the bindings, edit the filter in `src/main.rs` and run from the repository
root with Rust 1.95.0 and its rustfmt component, matching CI:

```console
cargo +1.95.0 run --manifest-path tools/bindgen/Cargo.toml
cargo +1.95.0 fmt --all
```

Commit the regenerated `src/bindings.rs` together with changes to the filter.
The `Check generated Windows bindings` CI job regenerates and formats the file,
then fails if it differs from the checked-in version.

Public flag constants in `src/enums.rs` re-export the generated types directly.
Registry permission parameters use the generated `REGSAM` type, re-exported from
`winreg::enums`, and option parameters use `u32`, matching the generated Windows
API signatures. Cast signed flag constants when passing them to these parameters.
