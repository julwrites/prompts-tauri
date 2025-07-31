# Agent Instructions for `prompts-tauri`

This document provides guidance for LLM agents working on the `prompts-tauri` repository.

## Project Overview

This repository contains the Tauri desktop application for the Prompts project. It is a Rust-based application with a web-based frontend. The application is a frontend for the `prompts-cli` core library, which is the sole dependency for this project.

## Core Tenets

- **Dependabot is Authoritative:** The `prompts-cli` dependency is managed by Dependabot. Do not manually update this dependency.
- **Focus on the UI:** The primary focus of this repository is the user interface. The core logic is handled by the `prompts-cli` library.
- **Web-based Frontend:** The frontend is a web-based application built with a modern web framework (e.g., React, Vue, Svelte).

## Development Workflow

1.  **Understand the `prompts-cli` API:** Before making any changes, familiarize yourself with the public API of the `prompts-cli` library.
2.  **Write Unit Tests:** For any new frontend code, write a unit test using a testing framework like Jest or Vitest.
3.  **Write Component Tests:** For any new UI component, write a test that renders the component and asserts on the rendered output.
4.  **Write E2E Tests:** For any new feature, write an E2E test that runs the application and simulates user input.
5.  **Run All Tests:** Ensure that all tests, including unit, component, and E2E tests, pass.

## Key Commands

- **Run tests:** `npm test`
- **Build the project:** `npm run build`
