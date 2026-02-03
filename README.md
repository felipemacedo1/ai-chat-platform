# AI Chat Platform

Full-stack AI chat application with Spring Boot backend and Next.js frontend.

## Project Structure

This is a monorepo using Git submodules:

- `backend/` - [ai-chat-api](https://github.com/felipemacedo1/ai-chat-api) - Spring Boot REST API (Java 8)
- `frontend/` - [ai-chat-ui](https://github.com/felipemacedo1/ai-chat-ui) - Next.js web application (TypeScript)

## Getting Started

### Clone with submodules

```bash
git clone --recurse-submodules https://github.com/felipemacedo1/ai-chat-platform.git
```

### Or initialize submodules after clone

```bash
git submodule init
git submodule update
```

### Update submodules

```bash
git submodule update --remote
```

## Development

### Backend (ai-chat-api)

```bash
cd backend
./mvnw spring-boot:run
```

### Frontend (ai-chat-ui)

```bash
cd frontend
npm install
npm run dev
```

## Documentation

See `.kiro/specs/ai-chat-interface/` for detailed requirements and design documents.
