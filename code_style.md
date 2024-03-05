# Code Style Rules


## Use::*

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

## Lints and warnings

Make sure you have no warnings from clippy with this lints enabled:

Recommended lints configuration.

> ✅ **Good**

```toml

[workspace.lints.rust]
# Warns if public items lack documentation.
missing_docs = "warn"
# Warns for public types not implementing Debug.
missing_debug_implementations = "warn"
# Denies non-idiomatic code for Rust 2018 edition.
rust_2018_idioms = "deny"
# Denies using features that may break in future Rust versions.
future_incompatible = "deny"
# Denies all unsafe code usage.
unsafe-code = "deny"
# Denies undocumented unsafe blocks.
undocumented_unsafe_blocks = "deny"

[workspace.lints.clippy]
# Denies restrictive lints, limiting certain language features/patterns.
restriction = "deny"
# Denies pedantic lints, enforcing strict coding styles and conventions.
pedantic = "deny"

```

## Prefer workspace lints over entry file lints

Workspace-level lint settings ensure a consistent code quality standard across the entire project, rather than configuring lints file-by-file. This approach not only simplifies maintenance but also ensures that all parts of your workspace adhere to the same quality checks, promoting uniformity and reducing the risk of overlooking lint warnings in individual crates or modules.

> ❌ **Bad**

Defining lint rules at the beginning of each entry file can lead to inconsistencies and overlooked issues, as it depends on manually adding these to every file:

```rust
#![ deny( unsafe_code ) ]
```

Defining lint rules in the workspace configuration applies them uniformly across all crates and modules, ensuring consistent code quality and reducing manual oversight:

> ✅ **Good**

```toml
[workspace.lints.rust]
unsafe-code = "deny"
```

By centralizing lint configurations at the workspace level, you enhance code quality across your entire project, streamline the review process, and make it easier to enforce coding standards and best practices.

## Avoid Using Attributes for Documentation, Use Doc Comments

For documenting code, prefer using ordinary doc comments `//!` over attributes like `#![doc = ""]`. Doc comments are more conventional and readable, aligning with Rust's idiomatic documentation practices. This approach ensures consistency in how documentation is written and maintained across the codebase.

> ❌ **Bad**

Using the `doc` attribute for documentation can disrupt the visual flow and consistency of source code documentation.

```rust
#![ doc = "Description of file." ]

#[ doc = "Implements a new type of secure connection." ]
mod secure_connection
{
  #[ doc = "Establishes a secure link." ]
  pub fn establish()
  {
  }
}
```

> ✅ **Good**

Ordinary doc comments `//!` and `///` provide a clearer, more idiomatic way to document modules and functions, enhancing readability.

```rust
//! Description of file.

/// Implements a new type of secure connection.
mod secure_connection
{
  /// Establishes a secure link.
  pub fn establish()
  {
  }
}
```

By adhering to the use of doc comments, you maintain a uniform documentation style throughout your project, making it easier for developers to read and understand the codebase. This practice also aligns with Rust's community standards and tooling, which are optimized for parsing and displaying doc comments.
