# Chat AI

Full-stack AI chat application with Spring Boot backend and Next.js frontend.

## Project Structure

This is a monorepo using Git submodules:

- `backend/` - Spring Boot REST API (Java 8)
- `frontend/` - Next.js web application (TypeScript)

## Getting Started

### Clone with submodules

```bash
git clone --recurse-submodules <repository-url>
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

### Backend

```bash
cd backend
./mvnw spring-boot:run
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

## Documentation

See `.kiro/specs/ai-chat-interface/` for detailed requirements and design documents.
