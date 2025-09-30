Basic Java Practice
-------------------

*-----Warm-up (Pure Java, no Spring)-----*

1. Movies – Lists and Filters
Goal: revisit collections, comparators, and streams.
Model: Movie{id, title, year, rating, genres: List<String>}.
Tasks: 
- Load 20 movies from a local JSON or CSV into a List<Movie>.
- Filters: by year, by genre, top-N by rating, substring search.
- Composite sorting (rating desc, year asc).
- Criteria: pure methods, tests with JUnit5.
- Extra: index by genre using Map<String, List<Movie>>.

2. Simple In-Memory Agenda
Goal: equals/hashCode, mutable collections, Optional.
Model: Contact{id, name, email, tags: Set<String>}.
Tasks: CRUD on a List<Contact>, avoid duplicates by email.
Criteria: edge tests (duplicate emails, empty lists).
Extra: export/import CSV.

*-----Spring Boot – Basics (REST + Validation + Lists)-----*

3. Book Catalog (in-memory CRUD)
Goal: first REST API with Spring Boot.
Deps: Web, Validation, Lombok.
Model: Book{id, title, author, year, price}.
Endpoints:
- POST /books (validation @NotBlank, @Positive)
- GET /books, GET /books/{id}
- PUT /books/{id}, DELETE /books/{id}
- Criteria: correct 400/404 responses, list kept in a @Service.
- Extra: filters by author/year via query params.

4. Tasks with Pagination and Sorting (no DB)
Goal: practice manual pagination and sorting with a List.
Model: Task{id, title, done, priority, dueDate}.
Endpoints: GET /tasks?page=&size=&sort=priority,desc
Criteria: return Page-like (total, page, size, content).
Extra: combined searches (by date range and status).

*-----Lightweight Persistence-----*

5. Notes with H2 in-memory
Goal: JPA/Hibernate + Spring Data repos.
Deps: Web, Validation, JPA, H2, Lombok.
Model: Note{id, title, body, createdAt}.
Endpoints: full CRUD.
Criteria: auto-generated schema, DTOs for requests/responses (don’t expose entity).
Extra: GET /notes?text=... with LIKE and pagination.

6. Inventory with Relationships (1-N)
Goal: JPA relations + DTO mapping.
Model: Category{id, name} - Product{id, name, price, Category category}
Endpoints: create categories, products, list products by category.
Criteria: avoid N+1 (use @EntityGraph or fetch join).
Extra: MapStruct for DTO mapping.

*-----Integration, Validation, and Layers-----*

7. User Registration with Strong Validation
Goal: advanced Bean Validation and error handling.
Model: User{id, name, email, password}.
Endpoints: POST /users with rules: valid email, password (length, numbers, uppercase)
Criteria: @ControllerAdvice for consistent error responses.
Extra: custom validation @StrongPassword.

8. Favorites with Cache
Goal: Spring Cache with “slow” data.
Model: Favorite{id, userId, label, url}.
Tasks: endpoint simulates being costly (sleep) and caches results by userId.
Criteria: @Cacheable, @CacheEvict, dev/prod profiles.
Extra: Caffeine as cache provider.

*-----Files, Scheduling, and Metrics-----*

9. File Upload/Download
Goal: MultipartFile, reading, listing.
Model: FileMeta{id, filename, size, uploadAt}.
Endpoints: POST /files, GET /files, GET /files/{id}/download
Criteria: validate size/mime, handle duplicate names.
Extra: store in file system with configurable path.

10. Scheduled Jobs and Metrics
Goal: @Scheduled + Actuator.
Tasks: a job cleans old records every X minutes and exposes metrics (deleted counter).
Criteria: Actuator endpoints enabled with basic security.
Extra: cron configurable via application.yml.

*-----Security and Tests-----*

11. API with Simple Login (homemade JWT)
Goal: Spring Security + filters.
Tasks:POST /auth/login returns JWT /me endpoints protected
Criteria: roles USER/ADMIN, correct 401/403.
Extra: token refresh or simple invalidation (in-memory blacklist).

12. Integration Tests with H2 and Seed Data
Goal: @SpringBootTest + TestRestTemplate or MockMvc.
Tasks: seed data with data.sql, test CRUD end-to-end.
Criteria: test profile with H2, error cases covered.
Extra: Testcontainers with PostgreSQL (for higher level).

*-----Working with External APIs and Real-Time-----*

13. External API Client (HTTP)
Goal: RestClient/WebClient + fault tolerance.
Tasks: consume a public API (e.g., quotes, countries) and expose via your own contract.
Criteria: timeouts, simple retries, clean DTO.
Extra: circuit breaker with Resilience4j.

14. Real-Time Notifications (SSE or WebSocket)
Goal: event streaming.
Tasks: publish “new note / new task” events to subscribed clients.
Criteria: GET /stream endpoint emits on insert.
Extra: channel notifications per user.

*-----One-Day Express Project (integrating multiple pieces)-----*

15. Personal “Mini Kanban”
Goal: CRUD + filters + light security + tests.
Model: Board, Column (TODO/DOING/DONE), Card{title, description, tags: List<String>, dueDate}
Key Endpoints:
- POST /boards, GET /boards/{id}
- POST /boards/{id}/cards, move card between columns
- Filters: by tags and dueDate
Criteria:
- Input validation, consistent errors
- Pagination for cards
- Integration tests for create/move/list
Extra: cache for GET /boards/{id}, SSE for real-time changes.

----------------------------------------------

How to Start Each Project (Quick Template)
Spring Initializr: Web, Validation, Lombok, JPA, H2 (depending on case), Security/Actuator/Cache if needed.
- Structure: domain (entities), dto, repository, service, web (controllers), config, exception

Best practices (express):
- Don’t expose JPA entities in the controller → use DTOs.
- @ControllerAdvice for errors.
- application.yml with dev and test profiles.
- Minimal tests per layer (at least 1 per controller and 1 per service).