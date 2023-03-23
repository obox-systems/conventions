# How to describe a demo

### Structure

- Name
- Сompetencies
- Abstract
- Screenshot
- Ho to run ( only in readme )

### Competencies

Crates/library, technologies, and skills used:

- Must be as descriptive as possible, even simple crates are welcome
- Explanation of why you chose them
- How you use them to solve the problem
- Preferably one sentence for each competency
- Example:
```
For this demo, I've used `serde`/`serde_json` crates for API messages serialization and deserialization,
the `actix-web` crate of `actix` framework for API server and routing,
and `tokio` for async runtime used by `actix`. 
```

### Abstract

Demo description: 5-10 sentences about the problem and how this demo solves it:

- What does it do 
- How does it work 
- Competencies involved

### How to run

The `Try it out!` section

- Steps to use the demo
- Every external dependency should be specified, preferably with steps to install them
- Descriptive information about each step
- Example for demos involving Rust:
```
      ### Try it out!

      1. Install [Rust](https://rustup.rs/)
      2. Set up MongoDB docker instance
      ```bash
      docker run --name db -p 27017:27017/tcp -d mongo:latest
      ```
      3. Run the app
      ```bash
      cargo run --release
      ```
```
  
### Screenshot 
 
Preferably to show screenshots.
 
- If your demo doesn't have any meaningful output, you can use a screenshot of code that describes your demo logic in the best way ( ex. API usage )

### Distribution

Duplicate and sync description in 3 places.

- In your google doc portfolio
- At your profile
- In a readme file of the repository
