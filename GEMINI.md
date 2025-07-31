# Gemini Development Guide: Prompts Tauri

This document guides the development of the Prompts Tauri application.

## Project Overview

The Prompts Tauri application is a desktop application for managing prompts for large language models (LLMs). It is built using the Tauri framework, with a web-based frontend and a Rust backend.

## Core Tenets

- **Tauri for Cross-Platform:** The application will be built with Tauri to ensure cross-platform compatibility (Windows, macOS, and Linux).
- **Web-based Frontend:** The frontend will be a single-page application (SPA) built with a modern web framework like React or Vue.
- **Rust for Backend:** The backend will be written in Rust and will be responsible for all the core prompt management logic.

## Development Workflow

1.  **Test First:** For any new feature, write a failing test that clearly defines the desired behavior.
2.  **Implement:** Write the minimum amount of code required to make the test pass.
3.  **Refactor:** Refactor the code to improve its design, readability, and performance, ensuring all tests still pass.
4.  **Repeat:** Repeat the cycle for the next feature.

## Key Technologies

- **Tauri:** The primary framework for building the application.
- **Rust:** For the backend logic.
- **React/Vue/Svelte:** For the frontend UI.
- **prompts-cli:** The core library for prompt management.

## Initial Setup

1.  **Initialize Tauri Project:** Set up a new Tauri project.
2.  **Add Dependencies:** Add `prompts-cli` to the Rust backend.
3.  **Write First Test:** Write a simple test to ensure that the basic application rendering is working correctly.
