# Auto Trait Specialisation
You have some trait, and want to implement it for most types.  
Some Types should be able to override that implementation and provide their own.  
To accomplish this, we implement the trait for all types that implement an auto trait.  
I suggest `Unpin`, as typically it has no impact on your code.  
Auto traits can be un-implemented by adding a `const REAL: bool = true` generic to the type, which don't worry, as it has a default you will not need to specify the generic when using the type.  
Then you can implement `Unpin` for your type with the generic set to `false` which then causes rust to no-longer implement `Unpin` for your type with its generic set to the default `true`.

[Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=c851f4e25816b28f2f9396c788c01c08)
```rust
// Your type.
struct Blah<const REAL: bool = true>;

// Your trait.
trait Something {}

//Blanket implementation.
impl<T: Unpin> Something for T {}

// Implementing Unpin for Blah<false> causes Blah to no longer be Unpin.
impl Unpin for Blah<false> {}
// Specialised implementation!
impl Something for Blah {}
```
