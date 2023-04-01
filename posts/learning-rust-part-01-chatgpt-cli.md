---
title: "I'm Learning Rust Part 1: ChatGPT CLI App"
description: Getting started learning Rust with a simple CLI app.
aside: false
date: 2023-03-31
tags:
  - Rust
  - ChatGPT
---

Quick intro, I'm learning Rust and I'll be doing it on here. My code will be ugly, and I'll try and share as much of the ugly stuff as I can as we go. I love having a simple, useful project to build as part of getting started with any new language, so I thought I'd make a CLI app to interact with the ChatGPT API and return its response to the terminal.

## Setup.

The first thing we will need is Rust. Head over to [Rustup](https://rustup.rs/) for instructions for your specific system. I'm writing this on OSX, so I need ro run the following command to get things going...

```bash
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Once you have been through the simple setup prompts, ensure you either refresh your terminal session or use this command to set your environment path:

```bash
$ source "$HOME/.cargo/env"
```

We should now be able to check everything is installed correctly by querying the version of Cargo the Rustup installation script has installed:

```bash
$ cargo --version
cargo 1.68.2 (6feb7c9cf 2023-03-26)
```

Excellent, now we can use Cargo to create a starter project.

```bash
$ cargo new my-chatgpt-app
     Created binary (application) `my-chatgpt-app` package
```

Cargo should have created a Rust starter project for us with the following structure:

```bash
.
├── Cargo.toml
└── src
    └── main.rs
```

Lets test our project and environment have been created correctly.

```bash
$ cargo run
Hello, World!
```

## Dependencies.

Next, we'll need to configure some crates our app will need. Lets edit the Cargo.toml file and add these creates as dependencies. 

```toml
# Cargo.toml

[dependencies]
clap = { version = "4.0", features = ["derive"] }
chatgpt_rs = "1.1.0"
tokio = { version = "1.27.0", features = ["rt-multi-thread"] }
```

Clap is a widely used Command Line Argument Parser (CLAP) that our app will use to accept an API key, and the message we are forwarding to ChatGPT. chatgpt_rs is the crate I've chosen as our ChatGPT client, and chatgpt_rs uses tokio to enable non-blocking asyc calls to the API. Be mindful of the versions I've used here as they may well have different implementations in future.

## Code.

Now we have our dependancies configured, we can start building our app in the 'src/main.rs' file.

Firstly we'll import the module components we're going to need.

```rust
use chatgpt::prelude::*;
use chatgpt::types::CompletionResponse;
use clap::Parser;
```

Now we'll create a struct to handle the arguements we want to supply to our app. Keeping things simple we'll just take the ChatGPT API key and the message we are sending for now. This struct will make use of some of the attributes of the clap module, which we can assign using an attribute macro. Attribute macros (as I understand so far) are a shorthand way of applying traits to structs, enums, etc. from external libraries. 

```rust
use chatgpt::prelude::*;
use chatgpt::types::CompletionResponse;
use clap::Parser;

#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    #[arg(short, long)]
    key: String,

    #[arg(short, long)]
    message: String,
}
```

This will allow us to make use of some of the features of Clap, such as returning basic usage information to the user about what types of arguments our app accepts, or establishing that the struct properties are Clap command line arguments with both a short and long name.

Now for the main function of our app, we'll parse the values defined in the Args struct and use the key to instantiate a new ChatGPT client. We can then Query the client using the 'message' argument and output the response to the console.

```rust
use chatgpt::prelude::*;
use chatgpt::types::CompletionResponse;
use clap::Parser;

#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    #[arg(short, long)]
    key: String,

    #[arg(short, long)]
    message: String,
}

#[tokio::main]
async fn main() -> Result<()> {

    // Parsing the argument values 
    let args = Args::parse();

    // Getting the API key
    let key = args.key;

    // Getting the message
    let message = args.message;

    // Creating a new ChatGPT client
    let client = ChatGPT::new(key)?;

    // Sending a message and getting the completion
    let response: CompletionResponse = client
        .send_message(message)
        .await?;

    println!("Response: {}", response.message().content);
    Ok(())
}
```

Thats all we need for now. Lets test everything is working.

```bash
$ cargo run -- --key {{ YOUR_CHATGPT_API_KEY }} --message "describe the Rust programming language in five words"
Error: BackendError { message: "You exceeded your current quota, please check your plan and billing details.", error_type: "insufficient_quota" }
```

Perfect. Now once we've upgraded to the paid version of the API we should be able to call it using our app. Thats it for now. I've enjoyed getting to grips with Rust so far, the syntax is a little alien to me at the moment but the compiler is possibly the best I've ever used with very clear feedback.

Check out the whole project on [GitHub](https://github.com/AlexDarby/gpt).

From here we might want to consider how we could extend our project to securely store the API key locally. More to follow.