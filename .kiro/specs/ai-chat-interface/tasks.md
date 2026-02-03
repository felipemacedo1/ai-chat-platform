# Implementation Plan

## Backend (Spring Boot)

- [x] 1. Setup do projeto backend Spring Boot




  - [x] 1.1 Inicializar projeto Spring Boot com Maven/Gradle
    - Criar projeto com Spring Initializr (Web, Security, JPA, Validation, Lombok)
    - Configurar Java 8
    - _Requirements: 6.1_

  - [x] 1.2 Configurar PostgreSQL e JPA





    - Adicionar dependência PostgreSQL driver
    - Criar entidades User, Conversation, Message com JPA annotations
    - Configurar application.yml com datasource
    - _Requirements: 5.1, 5.2_

  - [x] 1.3 Configurar variáveis de ambiente
    - Criar application.yml e application-dev.yml
    - Configurar DATABASE_URL, JWT_SECRET via environment variables
    - _Requirements: 6.1_



- [x] 2. Implementar módulo de autenticação


  - [x] 2.1 Criar UserService e UserRepository




    - Implementar UserRepository com Spring Data JPA
    - Criar UserService com create, findByEmail, findById

    - Usar BCryptPasswordEncoder para hash de senhas
    - _Requirements: 1.1, 1.2_
  - [x] 2.2 Write property test for user registration


    - **Property 1: Valid registration creates user**


    - **Validates: Requirements 1.1**
  - [x] 2.3 Write property test for invalid registration




    - **Property 2: Invalid registration returns field-specific errors**
    - **Validates: Requirements 1.2**
  - [x] 2.4 Configurar Spring Security com JWT





    - Criar JwtTokenProvider para gerar/validar tokens
    - Criar JwtAuthenticationFilter
    - Configurar SecurityFilterChain
    - Implementar AuthService com register, login, validateUser
    - _Requirements: 1.3, 1.4, 1.5_
  - [x] 2.5 Write property test for JWT authentication





    - **Property 3: Valid credentials return JWT**
    - **Property 4: Invalid credentials return generic error**
    - **Validates: Requirements 1.3, 1.4**
  - [x] 2.6 Criar AuthController com endpoints




    - POST /api/auth/register
    - POST /api/auth/login
    - POST /api/auth/logout
    - _Requirements: 1.1, 1.3, 1.5_
  - [x] 2.7 Write property test for logout





    - **Property 5: Logout invalidates token**
    - **Validates: Requirements 1.5**

- [x] 3. Checkpoint - Garantir que todos os testes passam





  - Ensure all tests pass, ask the user if questions arise.

- [x] 4. Implementar módulo de conversas




  - [x] 4.1 Criar ConversationService e ConversationRepository


    - Implementar ConversationRepository com Spring Data JPA
    - Criar ConversationService com create, findAll, findOne, update, delete
    - Adicionar paginação com Pageable
    - _Requirements: 3.1, 3.2, 3.3, 3.4_

  - [x] 4.2 Write property test for conversation creation

    - **Property 8: New conversation is empty**
    - **Validates: Requirements 3.1**

  - [x] 4.3 Write property test for conversation deletion

    - **Property 9: Conversation deletion cascades to messages**
    - **Validates: Requirements 3.3**
  - [x] 4.4 Write property test for conversation rename


    - **Property 10: Conversation rename persists**
    - **Validates: Requirements 3.4**
  - [x] 4.5 Criar ConversationController


    - GET /api/conversations (com paginação)
    - POST /api/conversations
    - GET /api/conversations/{id}
    - PATCH /api/conversations/{id}
    - DELETE /api/conversations/{id}
    - _Requirements: 3.1, 3.2, 3.3, 3.4, 6.4_
  - [x] 4.6 Write property test for pagination


    - **Property 15: Pagination metadata is accurate**
    - **Validates: Requirements 6.4**

- [x] 5. Implementar módulo de mensagens




  - [x] 5.1 Criar MessageService e MessageRepository


    - Implementar MessageRepository com Spring Data JPA
    - Criar MessageService com create, findByConversation
    - Ordenar mensagens por createdAt
    - _Requirements: 5.1, 5.3_

  - [x] 5.2 Write property test for message ordering

    - **Property 12: Messages retrieved in chronological order**
    - **Validates: Requirements 5.3**


  - [x] 5.3 Write property test for message serialization


    - **Property 11: Message persistence round-trip**


    - **Validates: Requirements 5.4, 5.5**
  - [x] 5.4 Criar MessageController
    - GET /api/conversations/{conversationId}/messages
    - POST /api/conversations/{conversationId}/messages
    - _Requirements: 2.1, 5.1, 5.3_
  - [x] 5.5 Write property test for whitespace validation
    - **Property 6: Whitespace-only messages are rejected**
    - **Validates: Requirements 2.2**

- [ ] 6. Implementar integração com AI Provider
  - [ ] 6.1 Criar AiService
    - Implementar interface para AI provider (OpenAI/Claude)
    - Criar método sendMessage que recebe contexto e retorna resposta
    - Implementar tratamento de erros com retry
    - _Requirements: 2.1, 2.5_
  - [ ] 6.2 Write property test for AI error handling
    - **Property 7: AI errors are handled gracefully**
    - **Validates: Requirements 2.5**
  - [ ] 6.3 Integrar AiService com MessageController
    - Chamar AI após salvar mensagem do usuário
    - Salvar resposta da AI como nova mensagem
    - Auto-gerar título na primeira resposta
    - _Requirements: 2.1, 3.5_

- [x] 7. Implementar validação e error handling global
  - [x] 7.1 Criar DTOs com Bean Validation
    - RegisterRequest, LoginRequest, CreateConversationRequest, CreateMessageRequest
    - Usar @Valid, @NotBlank, @Email, @Size annotations
    - _Requirements: 6.3_
  - [ ] 7.2 Write property test for request validation
    - **Property 14: Request validation rejects invalid schemas**
    - **Validates: Requirements 6.3**
  - [x] 7.3 Criar GlobalExceptionHandler
    - @RestControllerAdvice para tratamento centralizado
    - Logar erros internos
    - Retornar respostas sanitizadas
    - _Requirements: 6.5_
  - [ ] 7.4 Write property test for JWT guard
    - **Property 13: JWT validation guards endpoints**
    - **Validates: Requirements 6.1, 6.2**

- [ ] 8. Checkpoint - Garantir que todos os testes do backend passam
  - Ensure all tests pass, ask the user if questions arise.

## Frontend (Next.js - Feature-Based Architecture)

- [ ] 9. Setup do projeto frontend Next.js
  - [ ] 9.1 Inicializar projeto Next.js com TypeScript
    - Criar projeto com `create-next-app`
    - Configurar App Router
    - _Requirements: 4.1_
  - [ ] 9.2 Configurar Flowbite React e Tailwind CSS
    - Instalar flowbite-react e tailwindcss
    - Configurar tailwind.config.js com Flowbite plugin
    - _Requirements: 4.1, 4.3_
  - [ ] 9.3 Criar estrutura base de features
    - Criar diretórios: features/auth, features/conversations, features/messages, features/chat
    - Criar shared/lib/httpClient.ts com Axios configurado
    - Criar config/env.ts para variáveis de ambiente
    - _Requirements: 6.1, 6.2_

- [ ] 10. Implementar feature de autenticação (features/auth)
  - [ ] 10.1 Criar DTOs de auth
    - features/auth/dto/request.ts: LoginRequest, RegisterRequest
    - features/auth/dto/response.ts: User, AuthResponse
    - _Requirements: 1.1, 1.3_
  - [ ] 10.2 Criar authService
    - features/auth/services/authService.ts
    - Métodos: login, register, logout (1:1 com AuthController)
    - _Requirements: 1.1, 1.3, 1.5_
  - [ ] 10.3 Criar AuthContext
    - features/auth/context/AuthContext.tsx
    - Estado global: user, token, isAuthenticated
    - Persistir token no localStorage
    - _Requirements: 1.3, 1.5_
  - [ ] 10.4 Criar useAuth hook (Controller)
    - features/auth/hooks/useAuth.ts
    - Orquestra: handleLogin, handleRegister, handleLogout
    - Gerencia loading e erros
    - _Requirements: 1.1, 1.3, 1.5_
  - [ ] 10.5 Criar componentes de auth
    - features/auth/components/LoginForm.tsx
    - features/auth/components/RegisterForm.tsx
    - Usar componentes Flowbite (Card, TextInput, Button)
    - Exibir erros de validação
    - _Requirements: 1.1, 1.2, 1.3, 1.4_
  - [ ] 10.6 Criar páginas de auth
    - app/(auth)/login/page.tsx - compõe LoginForm
    - app/(auth)/register/page.tsx - compõe RegisterForm
    - Implementar proteção de rotas
    - _Requirements: 1.3_

- [ ] 11. Implementar feature de conversas (features/conversations)
  - [ ] 11.1 Criar DTOs de conversations
    - features/conversations/dto/request.ts: CreateConversationRequest, UpdateConversationRequest
    - features/conversations/dto/response.ts: ConversationResponse, PaginatedResponse
    - _Requirements: 3.1, 3.4, 6.4_
  - [ ] 11.2 Criar conversationService
    - features/conversations/services/conversationService.ts
    - Métodos: getAll, getById, create, update, delete (1:1 com ConversationController)
    - _Requirements: 3.1, 3.2, 3.3, 3.4_
  - [ ] 11.3 Criar useConversations hook (Controller)
    - features/conversations/hooks/useConversations.ts
    - Orquestra: fetchConversations, createConversation, renameConversation, deleteConversation
    - Gerencia loading, erro e lista de conversas
    - _Requirements: 3.1, 3.2, 3.3, 3.4, 5.2_
  - [ ] 11.4 Criar componentes de conversations
    - features/conversations/components/ChatSidebar.tsx
    - features/conversations/components/ConversationList.tsx
    - features/conversations/components/ConversationItem.tsx
    - Layout responsivo (collapsible em mobile)
    - _Requirements: 3.1, 3.2, 3.3, 3.4, 4.1_

- [ ] 12. Implementar feature de mensagens (features/messages)
  - [ ] 12.1 Criar DTOs de messages
    - features/messages/dto/request.ts: CreateMessageRequest
    - features/messages/dto/response.ts: MessageResponse, MessageRole
    - _Requirements: 5.4, 5.5_
  - [ ] 12.2 Criar messageService
    - features/messages/services/messageService.ts
    - Métodos: getByConversation, create (1:1 com MessageController)
    - _Requirements: 2.1, 5.1_
  - [ ] 12.3 Criar useMessages hook (Controller)
    - features/messages/hooks/useMessages.ts
    - Orquestra: fetchMessages, sendMessage
    - Validação de mensagem vazia
    - Gerencia loading, erro e lista de mensagens
    - _Requirements: 2.1, 2.2, 5.1, 5.3_
  - [ ] 12.4 Criar componentes de messages
    - features/messages/components/ChatMessages.tsx - lista de mensagens com auto-scroll
    - features/messages/components/MessageBubble.tsx - estilização user/assistant, markdown
    - features/messages/components/ChatInput.tsx - textarea auto-resize, Enter/Shift+Enter
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 4.2, 4.3, 4.4, 4.5_

- [ ] 13. Implementar feature de chat (features/chat)
  - [ ] 13.1 Criar useChat hook (Controller)
    - features/chat/hooks/useChat.ts
    - Orquestra conversa ativa, integra useConversations e useMessages
    - _Requirements: 3.2_
  - [ ] 13.2 Criar ChatLayout component
    - features/chat/components/ChatLayout.tsx
    - Integra Sidebar, Messages e Input
    - _Requirements: 3.2, 4.1_
  - [ ] 13.3 Criar página principal de chat
    - app/(chat)/page.tsx - compõe ChatLayout
    - Proteção de rota (requer autenticação)
    - _Requirements: 2.1, 2.2, 2.5, 3.2_

- [ ] 14. Checkpoint Final - Garantir que tudo funciona
  - Ensure all tests pass, ask the user if questions arise.
