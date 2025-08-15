# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a comprehensive software development notes repository containing learning materials, project documentation, and practical examples in Traditional Chinese. It serves as a learning knowledge base covering frontend, backend, and full-stack development concepts and implementations.

## Repository Structure

- **Frontend/**: Vue.js ecosystem, TypeScript syntax, HTML5/CSS fundamentals
- **Backend/**: NestJS framework documentation and testing strategies
- **Database/**: MSSQL operations and database management notes
- **DevOps-Architecture/**: Microservices architecture patterns and shared library implementation
- **JavaScript-Utilities/**: Performance optimization utilities (debounce functions)
- **Projects/PinPin/**: Documentation for a full-stack travel planning web application
- **Reference/**: Web development terminology dictionary in Chinese
- **Examples/**: Practical code snippets and demonstrations

## Key Project: PinPin Travel Platform

The main practical project documented here is a travel planning platform. **Note: The actual codebase is located elsewhere** - this repository contains only the documentation and planning materials.

### Architecture Overview
```
pinpin/
├── pinpin_backend/     # NestJS backend with modular architecture
├── pinpin_frontend/    # Vue.js frontend with Vuetify UI framework
└── pinpin_library/     # Shared TypeScript library for type definitions
```

### Technology Stack
- **Frontend**: Vue.js, Vuetify UI, TypeScript, Vite, Vitest for testing
- **Backend**: NestJS, TypeScript, ESM modules, JWT authentication
- **Shared Library**: Local npm package pattern for type definitions and constants
- **External APIs**: Google Places API, Google Maps API, OpenWeather API, ExchangeRate API
- **Development Tools**: ESLint configuration, TypeScript strict mode

### Core Features (Documented)
1. **Group Travel Planning**: Multi-user trip planning with role-based permissions
2. **Wishlist Management**: Personal favorite locations with categorization
3. **Itinerary Planning**: Day-by-day scheduling with Google Maps integration
4. **Expense Splitting**: Multi-currency expense tracking and automatic splitting

## Shared Library Pattern

The project uses a local npm package pattern for sharing types between frontend and backend:

### Commands for Shared Library Development
```bash
# In pinpin_library/ directory
npm run build        # Compile TypeScript to dist/
npm run watch        # Watch mode for development

# In frontend/backend projects after library changes
npm install ../pinpin_library    # Reinstall updated library
```

### Library Structure
- Uses CommonJS module format for Node.js compatibility
- Generates `.d.ts` declaration files for TypeScript support
- Exports shared types, constants, and interfaces
- Maintains type safety across the full-stack application

## Development Practices

- **Language**: All documentation is in Traditional Chinese
- **Code Style**: ESM modules, TypeScript strict mode, ESLint configuration
- **API Integration**: Implements caching and rate limiting for third-party APIs
- **Authentication**: JWT-based authentication with role-based access control
- **Testing**: Frontend uses Vitest, backend follows NestJS testing patterns

## Project Context

This is a learning and documentation repository. The actual PinPin application code exists in a separate location. When working with this repository:
- Focus on documentation accuracy and clarity
- Maintain Traditional Chinese language consistency
- Preserve the educational context and detailed explanations
- Follow the established file organization patterns