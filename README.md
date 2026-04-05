# PhoneRAG

PhoneRAG is a simple Android proof of concept for fully on-device AI using `llama.cpp`.

It demonstrates how to:
- run a GGUF LLM locally on Android
- load models from device storage
- perform inference without any cloud API
- build a Kotlin Android app around local inference
- experiment with lightweight retrieval for source-grounded responses

## Demo

This project is part of a short demo on fully local AI on Android.

- Blog: `add-your-blog-link-here`
- LinkedIn post: `add-your-linkedin-post-link-here`

## Why this project

Most AI app demos rely on cloud inference. The goal here was different:

- keep prompts and data on the device
- avoid server dependency
- understand how to wire native `llama.cpp` inference into an Android app
- test whether lightweight RAG-style behavior is practical on mobile

## What it does

The app lets a user:

1. Pick a `.gguf` model file from device storage
2. Import and load the model locally
3. Send prompts to the model on-device
4. View responses in a simple Android UI

I also used this project to test retrieval-style prompting with a small local knowledge base and source display.

## Core stack

- `llama.cpp`
- Android Studio
- Kotlin
- GGUF models
- Android document picker
- App-private storage for local model loading
- `AiChat` / `InferenceEngine` integration pattern from the official Android sample

## Project direction

This repo started as a custom Android integration attempt, then evolved toward a more stable approach by reusing the official `llama.android` integration pattern and customizing the application layer on top.

That made the native setup more reliable and let me focus on:
- the app flow
- prompt handling
- lightweight retrieval experiments
- debugging model behavior on-device

## App flow

The working inference path is:

1. User selects a GGUF model
2. App parses GGUF metadata
3. App copies the model into app-private storage
4. The inference engine loads the model from the local path
5. User sends a prompt
6. Tokens stream back and are rendered in the UI

## Retrieval experiment

To move toward a RAG-like experience, I tested lightweight retrieval approaches inside the app.

The goal was to:
- match a user query to small local context chunks
- pass the selected context to the model
- ask grounded questions with visible sources

This was useful for exploring:
- query-to-context matching
- prompt design for grounded QA
- how much model quality affects source-aware answers

## Problems faced during build

This project involved several practical Android-native issues:

- CMake path and project structure issues
- Gradle and plugin mismatches
- AndroidX and minSdk mismatches
- ABI packaging problems
- backend-loading/runtime failures in native inference
- weaker GGUF models failing to follow grounded QA prompts well

A major lesson was that on-device AI is not only about choosing a model. Native packaging and runtime setup matter just as much as the UI.

## Lessons learned

- The official Android sample is the safest base for `llama.cpp` on Android
- Native integration issues can dominate development time
- Model quality strongly affects grounded QA performance
- Retrieval may work correctly even when the model answers poorly
- Kotlin is fully capable of handling the app-side orchestration for local AI

## How to run

### Requirements

- Android Studio
- Android SDK / NDK configured
- A compatible Android device or emulator
- A GGUF model file stored locally

### Steps

1. Open the project in Android Studio
2. Sync Gradle
3. Build and run the app
4. Pick a `.gguf` model from storage
5. Wait for the model to be imported and loaded
6. Start chatting locally

## Debugging notes

During development, the most useful debugging tool was Logcat.

Things worth checking:
- current device ABI
- model import path
- GGUF metadata parsing
- model load success/failure
- backend-loading logs
- prompt text being sent
- retrieval hits and source scores
- generated token stream

Typical failure categories:
- model path issues
- ABI mismatch
- backend loading failure
- weak prompt-following by the model

## Repository purpose

This repository is mainly a learning and engineering log:
- how to get local LLM inference working on Android
- how Kotlin can be used around `llama.cpp`
- what broke during integration
- what worked in the final app
- how a lightweight retrieval experiment can be layered on top

## Credits

This work builds on:
- `llama.cpp`
- the official Android integration pattern from `examples/llama.android`

## Status

This is a proof of concept, not a production-ready app.

It is intended to document the build process, the integration decisions, and the practical lessons from getting fully local Android AI running.
