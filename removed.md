``` [1-12|13-16|19-30]
error[E0382]: borrow of moved value: `paths`
   --> src/main.rs:8:34
    |
4   |     let paths: Vec<PathBuf> = vec![];
    |         ----- move occurs because `paths` has type `Vec<PathBuf>`,
                    which does not implement the `Copy` trait
5   |
6   |     paths.into_iter().map(|p| File::create(p));
    |           ----------- `paths` moved due to this method call
7   |
8   |     println!("Created {} files", paths.len());
    |                                  ^^^^^^^^^^^ value borrowed here after move
    |
note: `into_iter` takes ownership of the receiver `self`, which moves `paths`
   --> /.../src/rust/library/core/src/iter/traits/collect.rs:271:18
    |
271 |     fn into_iter(self) -> Self::IntoIter;
    |                  ^^^^
help: you can `clone` the value and consume it, but this might not be your desired behavior
    |
6   |     paths.clone().into_iter().map(|p| File::create(p));
    |           ++++++++
```
<!-- .element: style="font-size: 0.4em; height: 100%;" -->

--

<!-- .slide: data-auto-animate-->

```rust
fn main() {
    let paths: Vec<PathBuf> = vec![];

    paths.into_iter().map(|p| File::create(p));

    println!("Created {} files", paths.len());
}
```
<!-- .element: data-id=1 -->

--
<!-- .slide: data-auto-animate-->

```rust [4]
fn main() {
    let paths: Vec<PathBuf> = vec![];

    paths.iter().map(|p| File::create(p));

    println!("Created {} files", paths.len());
}
```
<!-- .element: data-id=1 -->


--

```rust
fn main() {
    let paths: Vec<PathBuf> = vec![];

    paths.into_iter().map(|p| File::create(p));

    println!("Created {} files", paths.len());
}
```


---

# 🧑‍🎨 Coding Style

- Tests
  - We like rstest for fixture and `#[case(…)]`
- Module structure and export management
- Error management
  - DIY, anyhow or eyre
  - Wrapping errors when bubbling up
  - Only print where the error is dismissed
- Logging
  - The log crate offers a simple façade for multiple loggers
  - Agree on what each level means
  - Which error formatter will you use?  `{} {:?} {:#}`