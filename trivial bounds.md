# Trivial Bounds
Currently [trivial bounds](https://github.com/rust-lang/rust/issues/48214) are not available on stable.  
To get around this we can use a generic with a trait bound which will only match one type. With this free generic, we can plug it into anywhere that needs trivial bounds.

[Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=f2e7a77af988af81e710fee0c46955c4)
```rust
use std::marker::PhantomData;

/// Self is the same type as T.
/// Used to bypass trivial bounds.
trait Is<T> {}
impl<T> Is<T> for T {}

/// The type that we rely on using our trivial bounds.
struct Switch<T>(PhantomData<T>);

/// Some trait that we want.
trait Blah {}

// () will only implement Blah if Switch<()> implements Blah.
impl<T: Is<()>> Blah for T where Switch<T>: Blah {}
```
