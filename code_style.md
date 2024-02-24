# Code Style Rules


#### Use::*

For importing modules, prefer using `super::*` to collectively bring all items from the parent module into scope, rather than specifying each item individually. This approach simplifies import statements and maintains cleaner code. When importing from the current crate, `use crate::*` is acceptable if necessary to collectively import items from the crate root. Avoid using `crate::some_module::{item1, item2}` format for imports within the same crate. This guideline aims to streamline module imports and enhance code readability.

> ❌ **Bad**

Directly specifying multiple items from a parent module or the crate root should be avoided to keep import statements concise and maintainable.

```rust
// From a parent module
use super::{ a, b, c };

// From the crate root
use crate::some_module::{ item1, item2 };
```

> ✅ **Good**

Use super::* for importing all items from a parent module collectively. Use crate::* sparingly, only when necessary to import items collectively from the crate root.

```rust
// Importing everything from a parent module
use super::*;
// Correctly importing items collectively from the crate root (use sparingly)
use crate::*;

use { a, b, c };
use some_module::{ item1, item2 }
```

This approach not only simplifies the import statements but also makes the code more consistent and easier to read and maintain. Remember, the goal is to achieve clarity and simplicity in your codebase, ensuring that imports are managed in a straightforward and uniform manner.

#### Lints and warnings

Make sure you have no warnings from clippy with this lints enabled:

```rust
# Ensures code follows Rust 2018 edition best practices.
// Requires documentation for all public items.
#![ warn( missing_debug_implementations ) ]
// Ensures public types implement Debug.
#![ warn( missing_docs ) ]
// Ensures code follows Rust 2018 edition best practices.
#![ deny( rust_2018_idioms ) ]
// Prevents use of features that may break in future Rust versions.
#![ deny( future_incompatible ) ]
// Disallows unsafe code, promoting safety.
#![ deny( unsafe_code ) ]
```

> ✅ **Good**

```toml
[workspace.lints.rust]
# Requires documentation for all public items.
missing_docs = "warn"
# Ensures public types implement Debug.
missing_debug_implementations = "warn"
# Ensures code follows Rust 2018 edition best practices.
rust_2018_idioms = "deny"
# Prevents use of features that may break in future Rust versions.
future_incompatible = "deny"
# Disallows unsafe code, promoting safety.
unsafe-code = "deny"
```

#### Prefer workspace lints over entry file lints

Make sure you have no warnings from clippy with this lints enabled:

> ❌ **Bad**

```rust
# Ensures code follows Rust 2018 edition best practices.
#![ warn( missing_debug_implementations ) ]
#![ warn( missing_docs ) ]
#![ deny( rust_2018_idioms ) ]
#![ deny( future_incompatible ) ]
#![ deny( unsafe_code ) ]
```

> ✅ **Good**

```toml
[workspace.lints.rust]
missing_docs = "warn"
missing_debug_implementations = "warn"
rust_2018_idioms = "deny"
future_incompatible = "deny"
unsafe-code = "deny"
```
