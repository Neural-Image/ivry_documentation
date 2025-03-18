# Ivry CLI Documentation

## Introduction

Ivry CLI is a powerful command-line interface tool designed to simplify the process of creating, managing, and deploying AI applications. It provides seamless integration with ComfyUI workflows and supports various AI model deployment scenarios.

### What is Ivry?

Ivry is a platform that allows AI creators to easily build, deploy, and share their AI applications. The Ivry CLI tool is the command-line interface that provides direct access to the Ivry platform, enabling you to:

- Initialize new AI projects
- Manage authentication with the Ivry platform
- Pull existing projects from the Ivry server
- Run and monitor your AI services
- Deploy your applications with secure tunneling

### Key Features

- **Simple Project Initialization**: Create new ComfyUI or custom model projects with a single command
- **Seamless Authentication**: Secure login with API tokens
- **Project Management**: List, pull, and manage your applications
- **Service Orchestration**: Start, stop, and monitor your AI services
- **Automatic Tunneling**: Integrated Cloudflare tunnel support for secure public access
- **Process Management**: Built-in PM2 integration for reliable process management
- **Comprehensive Logging**: Monitor your application's performance and troubleshoot issues

### Who Should Use Ivry CLI?

Ivry CLI is designed for:

- **AI Developers** who want to deploy their models for public consumption
- **ComfyUI Workflow Creators** who want to share their workflows as applications
- **Data Scientists** who need to deploy models in a production environment
- **AI Enthusiasts** who want to experiment with sharing their creations

## Getting Started

To get started with Ivry CLI, you'll need to:

1. [Install Ivry CLI](getting-started.md) on your system
2. Authenticate with your Ivry API key
3. Initialize your first project

## System Requirements

- **Python**: 3.9 or higher
- **OS**: Windows (with WSL2), macOS, or Linux
- **Dependencies**: 
  - Node.js and npm
  - PM2 (installed automatically)
  - Cloudflared (installed automatically on Linux)