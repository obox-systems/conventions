# Code Style Guidline


## Importing: `use of crate::*`

For importing modules, prefer using `super::*` to collectively bring all items from the parent module into scope, rather than specifying each item individually. This approach simplifies import statements and maintains cleaner code. When importing from the current crate, `use crate::*` is acceptable if necessary to collectively import items from the crate root. Avoid using `crate::some_module::{item1, item2}` format for imports within the same crate. This guideline aims to streamline module imports and enhance code readability.

> ❌ **Bad**

Directly specifying multiple items from a parent module or the crate root should be avoided to keep import statements concise and maintainable.

```rust
// From a parent module
use super::{ a, b, c::d };
```

> ✅ **Good**

Use super::* for importing all items from a parent module collectively. Use crate::* sparingly, only when necessary to import items collectively from the crate root.

```rust
// Importing everything from a parent module
use super::*;
use d;
```

> ❌ **Bad**

Directly specifying multiple items from a parent module or the crate root should be avoided to keep import statements concise and maintainable.

```rust
// From the crate root
use crate::some_module::{ item1, item2 };
```

> ✅ **Good**

Use super::* for importing all items from a parent module collectively. Use crate::* sparingly, only when necessary to import items collectively from the crate root.

```rust
// Correctly importing items collectively from the crate root (use sparingly)
use crate::*;
use some_module::{ item1, item2 }
```

This approach not only simplifies the import statements but also makes the code more consistent and easier to read and maintain. Remember, the goal is to achieve clarity and simplicity in your codebase, ensuring that imports are managed in a straightforward and uniform manner.

## Importing: Local Entities

Prefer making all entities defined within the crate or its parent accessible in the current scope. Favor importing higher-level modules over individual entities from external crates.

When importing entities within your crate or from a parent file, aim for accessibility and clarity. It's advisable to prefer imports of high-level modules rather than specific, granular entities, especially when dealing with external crates. This approach enhances readability and maintainability by reducing clutter and focusing on module structure.

- **Local vs. External Entities**: For entities defined in your own crate, feel free to use broad imports to make everything available in the current scope. However, for external crates, consider using more specific paths to avoid namespace pollution and potential conflicts.

- **Granularity of Imports**: While it might be tempting to import only what you need from an external crate, overly granular imports can lead to long and hard-to-read import lists. Conversely, importing too broadly (e.g., using `*`) from external sources may introduce unused entities or overshadow local definitions.

> ❌ **Bad**

```rust
use crate::plot::{ PlotDescription, PlotOptions, plot };
use crate::path::{ AbsolutePath, refine };
```

> ✅ **Good**

```rust
use crate::*;
use plot::{ PlotDescription, PlotOptions, plot };
use path::{ AbsolutePath, refine };
```

### Importing: Structuring `std` Imports

Consolidate `std` library imports into a single use statement. Avoid multi-level nesting within `{}` to keep imports readable. If nesting is unavoidable, ensure only one `{}` per line.

> ❌ **Bad**

```rust
use std::fmt::{ Formatter, Write };
use std::path::PathBuf;
use std::collections::HashSet;
```

> ❌ **Bad**

```rust
use std::{ fmt::{ Formatter, Write }, fmt::path::PathBuf, collections::HashSet };
```

> ✅ **Good**

```rust
use std::
{
  fmt::{ Formatter, Write },
  path::PathBuf,
  collections::HashSet,
};
```

This approach simplifies tracking dependencies and enhances code readability.

## Lints and warnings

Make sure you have no warnings from clippy with this lints enabled:

Recommended lints configuration.

> ✅ **Good**

```toml

[workspace.lints.rust]
# Source :: https://github.com/obox-systems/conventions/blob/master/code_style.md#lints-and-warnings

# Denies non-idiomatic code for Rust 2018 edition.
rust_2018_idioms = "deny"
# Denies using features that may break in future Rust versions.
future_incompatible = "deny"
# Warns if public items lack documentation.
missing_docs = "warn"
# Warns for public types not implementing Debug.
missing_debug_implementations = "warn"
# Denies all unsafe code usage.
unsafe-code = "deny"

[workspace.lints.clippy]
# Denies restrictive lints, limiting certain language features/patterns.
restriction = "warn"
# Denies pedantic lints, enforcing strict coding styles and conventions.
pedantic = "warn"
# Denies undocumented unsafe blocks.
undocumented_unsafe_blocks = "deny"

```

If you add lint rules into workspace file don't forget to reuse them from crate's cargo files.

```toml
[lints]
workspace = true
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

## Codestyle Formatting

#### New Lines for Blocks

- Open `{`, `(`, `<` on new lines, except when the block is concise enough to fit on a single line.
- Do not place the opening brace on the same line as a function or control structure signature.
- Macro is not exception. When using macros like `quote!` and arguments for macro is longer to be on the same line with, place the opening `{` on a new line following the macro call.
- For lambda expressions ensure that the opening brace `{` of the closure's body is placed on a new line, unless whole body of closure is on the same line.
- For lambda expressions, ensure that the `|` symbols and parameters inside are separated by spaces from the parameters and the body block.

> ✅ **Good**

```rust
fn f1()
{
  if condition
  {
    // Code block
  }
}
```

> ✅ **Good**

```rust
let result = quote!
{
  #( #from_impls )*
};
```

> ✅ **Good**

```rust
fields
.iter()
.map( | field |
{
  f1( field );
});
```

#### Indentation

- Use strictly 2 spaces over tabs for consistent indentation across environments. Avoid using 4 spaces or tabs.
- When chaining method calls, start each method on a new line if including everything on the same line exceeds an admissible line length. Each method call in the chain should start directly below the first character of the chain start, without additional indentation. This ensures clarity and consistency without unnecessary spaces or alignment efforts.
- When chaining method calls that exceed a single line's admissible length, each method call in the chain should start on a new line directly below the object or variable initiating the chain. These subsequent method calls should align with the first character of the initiating call, without additional indentation beyond what is used for the start of the chain. Avoid adding extra indentation before each method in the chain, as this can obscure the structure and flow of chained operations.
- Method calls starting with a dot (`.method`) should not be indented further than the first call in the chain.

> ✅ **Good**

```rust
struct Struct1
{
  a : i32,
  b : i32,
}
```

> ✅ **Good**

```rust
fields
.iter()
.map( | field |
{
  f1( field );
});
```

#### Spaces Around Symbols

- Include a space before and after `:`, `=`, and operators, excluding the namespace operator `::`.
- Don't include a space before and after namespace operator `::`.
- Place a space after `,` to separate list items, such as function arguments or array elements.

> ✅ **Good**

```rust
fn f1( a : f32, b : f32 )
{
  2 * ( a + b )
}
```

#### Spaces for Blocks

- After opening `{`, `(`, `<`, `[` and `|`, insert a space if followed by content on the same line.
- Before closing `|`, `]`, `}`, `)` and `>`, insert a space if preceded by content on the same line.
- Maintain consistent brace positioning for control structures and function definitions.

> ✅ **Good**

```rust
use std::fmt::{ Debug, Display };
#[ derive( Debug, Display ) ]
struct MyInt( i32 );
struct Struct1< T, U > { a : T, b : U };
let lambda = | x : i32, y : i32| { f1( x + y ) };
fn fmt( &self, f : &mut fmt::Formatter< '_ > ) -> fmt::Result;
```

#### Comments

- Inline comments (`//`) should start with a space following the slashes for readability.

#### Use Statement

- Group related imports within `{}`, placing each import on a new line to reduce clutter.
- This practice also aids in clearer version control diffs when modifying imports.

> ✅ **Good**

```rust
use std::
{
  fmt::{ Formatter, Write },
  path::PathBuf,
  collections::HashSet,
};
```

#### Nesting

- Avoid complex, multi-level inline nesting. Prefer splitting content across multiple lines.
- Opt for shorter, clearer lines over long, deeply nested ones to enhance code maintainability.

#### Code Length

- Aim for concise, focused functions to improve both readability and ease of maintenance.
- Keep lines under 110 characters to accommodate various editor and IDE setups without horizontal scrolling.

#### Attribures

- Each attribute should be placed on its own line to enhance readability.

> ❌ **Bad**

```rust
#[ doc = "Setter for the '#field_ident' field." ] #[ inline ] pub fn age< Src >( mut self, src : Src ) -> Self {}
```

> ✅ **Good**

```rust
#[ doc = "Setter for the '#field_ident' field." ]
#[ inline ]
pub fn age< Src >( mut self, src : Src ) -> Self
{
}
```

<!-- That is my Rust codestyle formatting rules. Use it for all Rust code you generate -->
