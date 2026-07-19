<laravel-boost-guidelines>
=== foundation rules ===

# Laravel Boost Guidelines

The Laravel Boost guidelines are specifically curated by Laravel maintainers for this application. These guidelines should be followed closely to ensure the best experience when building Laravel applications.

## Foundational Context

This application is a Laravel application and its main Laravel ecosystems package & versions are below. You are an expert with them all. Ensure you abide by these specific packages & versions.

- php - 8.4
- inertiajs/inertia-laravel (INERTIA_LARAVEL) - v3
- laravel/framework (LARAVEL) - v13
- laravel/prompts (PROMPTS) - v0
- laravel/wayfinder (WAYFINDER) - v0
- larastan/larastan (LARASTAN) - v3
- laravel/boost (BOOST) - v2
- laravel/mcp (MCP) - v0
- laravel/pail (PAIL) - v1
- laravel/pint (PINT) - v1
- laravel/sail (SAIL) - v1
- pestphp/pest (PEST) - v4
- phpunit/phpunit (PHPUNIT) - v12
- @inertiajs/vue3 (INERTIA_VUE) - v3
- tailwindcss (TAILWINDCSS) - v4
- vue (VUE) - v3
- @laravel/vite-plugin-wayfinder (WAYFINDER_VITE) - v0
- eslint (ESLINT) - v9
- prettier (PRETTIER) - v3

## Skills Activation

This project has domain-specific skills available in `**/skills/**`. You MUST activate the relevant skill whenever you work in that domain—don't wait until you're stuck.

## Conventions

- You must follow all existing code conventions used in this application. When creating or editing a file, check sibling files for the correct structure, approach, and naming.
- Use descriptive names for variables and methods. For example, `isRegisteredForDiscounts`, not `discount()`.
- Check for existing components to reuse before writing a new one.

## Verification Scripts

- Do not create verification scripts or tinker when tests cover that functionality and prove they work. Unit and feature tests are more important.

## Application Structure & Architecture

- Stick to existing directory structure; don't create new base folders without approval.
- Do not change the application's dependencies without approval.

## Frontend Bundling

- If the user doesn't see a frontend change reflected in the UI, it could mean they need to run `npm run build`, `npm run dev`, or `composer run dev`. Ask them.

## Documentation Files

- You must only create documentation files if explicitly requested by the user.

## Replies

- Be concise in your explanations - focus on what's important rather than explaining obvious details.

=== boost rules ===

# Laravel Boost

## Tools

- Laravel Boost is an MCP server with tools designed specifically for this application. Prefer Boost tools over manual alternatives like shell commands or file reads.
- Use `database-query` to run read-only queries against the database instead of writing raw SQL in tinker.
- Use `database-schema` to inspect table structure before writing migrations or models.
- Use `get-absolute-url` to resolve the correct scheme, domain, and port for project URLs. Always use this before sharing a URL with the user.
- Use `browser-logs` to read browser logs, errors, and exceptions. Only recent logs are useful, ignore old entries.

## Searching Documentation (IMPORTANT)

- Always use `search-docs` before making code changes. Do not skip this step. It returns version-specific docs based on installed packages automatically.
- Pass a `packages` array to scope results when you know which packages are relevant.
- Use multiple broad, topic-based queries: `['rate limiting', 'routing rate limiting', 'routing']`. Expect the most relevant results first.
- Do not add package names to queries because package info is already shared. Use `test resource table`, not `filament 4 test resource table`.

### Search Syntax

1. Use words for auto-stemmed AND logic: `rate limit` matches both "rate" AND "limit".
2. Use `"quoted phrases"` for exact position matching: `"infinite scroll"` requires adjacent words in order.
3. Combine words and phrases for mixed queries: `middleware "rate limit"`.
4. Use multiple queries for OR logic: `queries=["authentication", "middleware"]`.

## Artisan

- Run Artisan commands directly via the command line (e.g., `php artisan route:list`). Use `php artisan list` to discover available commands and `php artisan [command] --help` to check parameters.
- Inspect routes with `php artisan route:list`. Filter with: `--method=GET`, `--name=users`, `--path=api`, `--except-vendor`, `--only-vendor`.
- Read configuration values using dot notation: `php artisan config:show app.name`, `php artisan config:show database.default`. Or read config files directly from the `config/` directory.

## Tinker

- Execute PHP in app context for debugging and testing code. Do not create models without user approval, prefer tests with factories instead. Prefer existing Artisan commands over custom tinker code.
- Always use single quotes to prevent shell expansion: `php artisan tinker --execute 'Your::code();'`
  - Double quotes for PHP strings inside: `php artisan tinker --execute 'User::where("active", true)->count();'`

=== php rules ===

# PHP

- Always use curly braces for control structures, even for single-line bodies.
- Use PHP 8 constructor property promotion: `public function __construct(public GitHub $github) { }`. Do not leave empty zero-parameter `__construct()` methods unless the constructor is private.
- Use explicit return type declarations and type hints for all method parameters: `function isAccessible(User $user, ?string $path = null): bool`
- Use TitleCase for Enum keys: `FavoritePerson`, `BestLake`, `Monthly`.
- Prefer PHPDoc blocks over inline comments. Only add inline comments for exceptionally complex logic.
- Use array shape type definitions in PHPDoc blocks.

=== deployments rules ===

# Deployment

- Laravel can be deployed using [Laravel Cloud](https://cloud.laravel.com/), which is the fastest way to deploy and scale production Laravel applications.

=== herd rules ===

# Laravel Herd

- The application is served by Laravel Herd at `https?://[kebab-case-project-dir].test`. Use the `get-absolute-url` tool to generate valid URLs. Never run commands to serve the site. It is always available.
- Use the `herd` CLI to manage services, PHP versions, and sites (e.g. `herd sites`, `herd services:start <service>`, `herd php:list`). Run `herd list` to discover all available commands.

=== inertia-laravel/core rules ===

# Inertia

- Inertia creates fully client-side rendered SPAs without modern SPA complexity, leveraging existing server-side patterns.
- Components live in `resources/js/pages` (unless specified in `vite.config.js`). Use `Inertia::render()` for server-side routing instead of Blade views.
- ALWAYS use `search-docs` tool for version-specific Inertia documentation and updated code examples.
- IMPORTANT: Activate `inertia-vue-development` when working with Inertia Vue client-side patterns.

# Inertia v3

- Use all Inertia features from v1, v2, and v3. Check the documentation before making changes to ensure the correct approach.
- New v3 features: standalone HTTP requests (`useHttp` hook), optimistic updates with automatic rollback, layout props (`useLayoutProps` hook), instant visits, simplified SSR via `@inertiajs/vite` plugin, custom exception handling for error pages.
- Carried over from v2: deferred props, infinite scroll, merging props, polling, prefetching, once props, flash data.
- When using deferred props, add an empty state with a pulsing or animated skeleton.
- Axios has been removed. Use the built-in XHR client with interceptors, or install Axios separately if needed.
- `Inertia::lazy()` / `LazyProp` has been removed. Use `Inertia::optional()` instead.
- Prop types (`Inertia::optional()`, `Inertia::defer()`, `Inertia::merge()`) work inside nested arrays with dot-notation paths.
- SSR works automatically in Vite dev mode with `@inertiajs/vite` - no separate Node.js server needed during development.
- Event renames: `invalid` is now `httpException`, `exception` is now `networkError`.
- `router.cancel()` replaced by `router.cancelAll()`.
- The `future` configuration namespace has been removed - all v2 future options are now always enabled.

=== laravel/core rules ===

# Do Things the Laravel Way

- Use `php artisan make:` commands to create new files (i.e. migrations, controllers, models, etc.). You can list available Artisan commands using `php artisan list` and check their parameters with `php artisan [command] --help`.
- If you're creating a generic PHP class, use `php artisan make:class`.
- Pass `--no-interaction` to all Artisan commands to ensure they work without user input. You should also pass the correct `--options` to ensure correct behavior.

### Model Creation

- When creating new models, create useful factories and seeders for them too. Ask the user if they need any other things, using `php artisan make:model --help` to check the available options.

## APIs & Eloquent Resources

- For APIs, default to using Eloquent API Resources and API versioning unless existing API routes do not, then you should follow existing application convention.

## URL Generation

- When generating links to other pages, prefer named routes and the `route()` function.

## Testing

- When creating models for tests, use the factories for the models. Check if the factory has custom states that can be used before manually setting up the model.
- Faker: Use methods such as `$this->faker->word()` or `fake()->randomDigit()`. Follow existing conventions whether to use `$this->faker` or `fake()`.
- When creating tests, make use of `php artisan make:test [options] {name}` to create a feature test, and pass `--unit` to create a unit test. Most tests should be feature tests.

## Vite Error

- If you receive an "Illuminate\Foundation\ViteException: Unable to locate file in Vite manifest" error, you can run `npm run build` or ask the user to run `npm run dev` or `composer run dev`.

=== wayfinder/core rules ===

# Laravel Wayfinder

Use Wayfinder to generate TypeScript functions for Laravel routes. Import from `@/actions/` (controllers) or `@/routes/` (named routes).

=== pint/core rules ===

# Laravel Pint Code Formatter

- If you have modified any PHP files, you must run `vendor/bin/pint --dirty --format agent` before finalizing changes to ensure your code matches the project's expected style.
- Do not run `vendor/bin/pint --test --format agent`, simply run `vendor/bin/pint --format agent` to fix any formatting issues.

=== pest/core rules ===

## Pest

- This project uses Pest for testing. Create tests: `php artisan make:test --pest {name}`.
- The `{name}` argument should not include the test suite directory. Use `php artisan make:test --pest SomeFeatureTest` instead of `php artisan make:test --pest Feature/SomeFeatureTest`.
- Run tests: `php artisan test --compact` or filter: `php artisan test --compact --filter=testName`.
- Do NOT delete tests without approval.

=== inertia-vue/core rules ===

# Inertia + Vue

Vue components must have a single root element.
- IMPORTANT: Activate `inertia-vue-development` when working with Inertia Vue client-side patterns.

</laravel-boost-guidelines>

=== project rules ===

# Planning

For every implementation:

1. Inspect referenced files first.
2. Analyze existing patterns.
3. Explain the proposed solution.
4. Highlight risks and concerns.
5. List files to be modified.
6. Provide implementation plan.
7. Wait for approval before editing.

If the user asks for analysis, suggestions, review, planning, or explanation, do not modify files.

Only edit files when the user explicitly says:

- proceed
- implement
- apply
- edit
- modify

---

# Project Context

This application is called FieldOps ERP.

FieldOps ERP is a management system for a plant-installation company (photovoltaic, plumbing, HVAC).

This is a portfolio project. The evaluation criteria it must showcase, in order of priority:

1. Data integrity — real foreign keys, snapshots on fiscal documents, append-only stock ledger.
2. Concurrency correctness — fiscal numbering and stock operations must be safe under parallel requests.
3. Architectural clarity — thin controllers, explicit state machines, typed frontend.

When a shortcut conflicts with one of these, do not take the shortcut. Flag the conflict instead.

The primary business domain includes:

- leads and deals
- customers and contacts
- quotes, invoices, credit notes
- products and stock movements
- delivery notes (DDT)
- jobs and construction sites

---

# Phase 1 Scope

Phase 1 includes:

- authentication
- users, roles and permissions
- CRM: leads, deals, customers, contacts
- administration: quotes, invoices, credit notes with fiscal numbering
- warehouse: products, DDTs, stock movements

Phase 2 (do not implement unless explicitly requested):

- jobs, construction sites, work items

Do not introduce additional modules without approval.

---

# Preferred Packages

Prefer these packages unless explicitly instructed otherwise:

- Laravel Fortify for authentication
- Inertia.js + Vue 3 + TypeScript for UI
- Ziggy for named routes in the frontend
- Spatie Permission for roles and permissions
- Pest for testing
- Larastan for static analysis

Do not propose alternative third-party packages unless there is a strong technical reason.

Do not add packages without asking.

Laravel Boost generates framework-level guidelines for installed packages. This file covers only working method and domain rules; do not duplicate Boost guidelines here.

---

# Non-Negotiable Domain Rules

- Money is stored as integer cents. Never float. No `double` columns, no `round()` chains.
- A deal always has a `customer_id`. Leads never link to documents. `deals.lead_id` is provenance only.
- Document numbers are assigned only at issue time, via `document_counters` (per type, per year) with `SELECT ... FOR UPDATE` inside the issuing transaction. Never at creation.
- Issued documents (quotes, invoices, credit notes, DDTs) are immutable except for their status. Editing happens only in draft.
- Documents snapshot customer data, product description and unit price at issue time. An invoice never changes because a price list changed.
- Fiscal documents are never deleted. No soft deletes on fiscal documents. Cancellation is a state transition.
- State machines are explicit: transitions validated in a dedicated class per document type; illegal transitions throw.
- No `stock` column on products. Stock on hand = SUM of `stock_movements.quantity`. The ledger is append-only: movements are never updated or deleted; corrections are new adjustment rows.
- No polymorphic relations (`morphs`). This project deliberately uses real foreign keys.
- `database/schema.dbml` is the schema source of truth. If the implementation needs to diverge, update the DBML in the same commit and explain why.

---

# Architecture

- Prefer Form Requests.
- Prefer Policies.
- Prefer PHP Enums.
- Keep controllers thin: validate, call action, return Inertia response.
- Prefer Actions (single-purpose invokable classes) over fat services.
- Avoid Repository pattern unless explicitly requested.
- Do not introduce tenancy packages or tenant infrastructure.

---

# Existing Patterns

Follow existing project patterns before introducing new abstractions.

Prefer consistency with the current codebase over theoretical best practices.

If multiple solutions are valid, choose the one most consistent with surrounding code.

---

# Simplicity

Prefer the simplest solution that satisfies the requirement.

Do not introduce abstractions for anticipated future needs.

Avoid premature optimization.

Avoid overengineering.

---

# Domain Modeling

Always model the business domain before designing the database.

When requirements are ambiguous:

- ask for clarification
- avoid assumptions
- avoid premature abstractions

Do not introduce entities, relationships, workflows or statuses that are not part of the approved business domain.

---

# Roles

Initial application roles:

- superadmin
- office
- worker

These represent application permissions.

Do NOT confuse them with business roles on deals or construction sites (e.g. site manager, referent). Business roles belong to the domain and are independent from authentication.

Do not introduce new application roles unless explicitly requested.

---

# Authorization

Prefer Laravel Policies for business authorization.

Permissions determine what a user can do.

Assignments determine which business resources a user can access.

Current business assignment:

- Job (a worker only sees jobs assigned to them)

Always enforce authorization server-side.

Never rely only on frontend filtering.

Test authorization rules for all critical business operations.

---

# Inertia & Vue Component Design

- Inertia is not an API. No fetch/axios toward JSON endpoints: controllers return `Inertia::render()`, forms use `useForm()`.
- Always `<script setup lang="ts">`. No Options API in new code.
- `resources/js/Pages/` mirrors routes. `Components/` for reusable UI. `Composables/` for shared logic. `types/` for TypeScript interfaces.
- Every page defines typed props: `defineProps<{ ... }>()` with interfaces from `types/`.
- Shared data (auth user, flash, permissions) goes through `HandleInertiaRequests::share()` and is typed once as global `PageProps`.
- Use Ziggy's `route()` helper for URLs. Never hardcode URLs in components.
- After mutations prefer partial reloads: `router.reload({ only: [...] })`.
- The client displays money amounts but never computes them for persistence. The server computes.
- Keep components focused on a single responsibility. Extract child components when a component becomes too large.

---

# Eloquent Relationships

Prefer Eloquent relationships over manual foreign key queries.

Prefer expressive relationship names matching the domain.

Use singular names for belongsTo / hasOne: customer(), deal(), invoice().

Use plural names for hasMany / belongsToMany: deals(), invoiceLines(), stockMovements().

Prefer eager loading to avoid N+1 queries.

---

# Testing

New business logic should include tests.

Every migration ships with a model factory.

Concurrency-critical paths (fiscal numbering, stock movements, state transitions) get dedicated tests, written before the implementation.

Test authorization rules and business visibility rules.

Larastan must pass at the configured level.

---

# Naming & Localization

Always use English naming for code-level identifiers.

Never use Italian for:

- classes
- methods
- variables
- enums
- routes
- migrations
- database tables
- database columns
- translation keys

Domain terms with no clean English equivalent (e.g. DDT) keep their Italian acronym as an accepted domain word.

Treat the application as multilingual.

Never hardcode user-facing strings.

Always use translation files.

Default locale:

- it

Fallback locale:

- en

---

# Enum Conventions

Prefer PHP Enums over raw strings.

Enum backing values must use lowercase strings.

Map enums to VARCHAR columns with CHECK constraints, not MySQL ENUM.

Prefer enum casts in Eloquent models.

Business logic should rely on enum methods.

Never display enum backing values directly.

Always resolve labels through translations.

---

# UI Direction

There is no Figma for this project.

Target: clean, dense admin panel. Tailwind with semantic token names (brand, surface, muted, success, warning, danger). Avoid purely visual names.

Prefer reusable wrappers for recurring patterns (buttons, inputs, tables, badges, modals, empty states, page headers, pagination).

Keep wrappers thin unless the component is intentionally becoming a project-level primitive.

Avoid duplicating complex markup across pages.
