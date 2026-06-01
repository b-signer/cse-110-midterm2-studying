# Vocab and Acronyms — Lessons 08–13

---

## The -ilities (Quality Attributes)

★ = ISO 25010 key -ility (the 8 we especially need to know)

| Term | Definition |
|------|-----------|
| **-ility** | Any non-functional quality attribute that describes *how* a system behaves, not *what* it does |
| **Functional Suitability** ★ | The system provides functions that meet stated and implied needs |
| **Performance Efficiency** ★ | Resource usage, throughput, and response time under load |
| **Compatibility** ★ | Works correctly across different browsers, devices, OS, or environments |
| **Usability** ★ | How effectively and intuitively users can interact with the system |
| **Reliability** ★ | System produces correct results consistently over time |
| **Security** ★ | Resistance to unauthorized access, manipulation, or data disclosure |
| **Maintainability** ★ | How easily the system can be changed, debugged, or extended |
| **Portability** ★ | Ability to operate across different environments or platforms |
| **Availability** | Ability to access the system and its functions when needed |
| **Robustness** | Ability to handle errors, edge cases, and adverse conditions gracefully |
| **Accessibility** | Ability to use the system's functions regardless of ability or disability |
| **Satisfiability** | Ability to enjoy or feel satisfied by the system's functions |
| **Utility** | The system provides the required functions at all |
| **Testability** | How readily behavior can be verified through tests |
| **Scalability** | Ability to handle growing load without degradation |
| **Modifiability** | Ease of making targeted changes without unintended side effects |
| **Interoperability** | Ability to work with other systems |
| **Flexibility** | Ability to adapt to changing requirements |
| **Reusability** | Degree to which components can be used in other contexts |
| **Durability** | Ability to persist and function correctly over long periods |
| **Installability** | Ease of installing and configuring the system |

### Remembering the 8 Key -ilities

**Sentence (ISO 25010 order):** *"For Perfect Code, Users Rely on Solid Maintained Platforms."

**Conceptual grouping (4 + 4):**

| User Experience (FURSec) | Technical Quality (PerCMaP) |
|------------------------|--------------------------|
| **F**unctional Suitability — does it do what I need? | **Per**formance Efficiency — is it fast/efficient? |
| **U**sability — is it easy to use? | **C**ompatibility — does it run on my device/browser? |
| **R**eliability — does it stay working? | **Ma**intainability — can devs change it? |
| **Sec**urity — is my data safe? | **P**ortability — can it move to a new environment? |

---

## Architectural Principles and Concepts

| Term | Definition |
|------|-----------|
| **Architecture** | The important, hard-to-change decisions about a system's structure, behavior, design, and trade-offs |
| **SRP** | Single Responsibility Principle — every module/class/function should have exactly one reason to change |
| **Separation of Concerns** | Dividing a system so that each part addresses a distinct, non-overlapping concern |
| **DRY** | Don't Repeat Yourself — every piece of knowledge should have a single, authoritative representation |
| **KISS** | Keep It Simple Stupid — prefer the simplest solution that works; fight unnecessary complexity |
| **YAGNI** | You Ain't Gonna Need It — don't build features or abstractions for hypothetical future needs |
| **Form Follows Function** | Design the system around what it needs to do before deciding how it looks or what tools it uses |
| **Abstraction** | Hiding implementation details and exposing only what is necessary to use a component |
| **Data Hiding / Encapsulation** | Keeping internal state private so it can only be modified through controlled interfaces |
| **Decomposition** | Breaking a system into appropriately-sized, coherent parts |
| **Cohesion** | How related and focused the responsibilities inside a single module are (want: high cohesion) |
| **Coupling** | How dependent one module is on another (want: loose coupling) |
| **Loose Coupling** | Components relate but are not deeply dependent; easy to swap or test in isolation |
| **High Coupling** | Components depend heavily on each other; changes ripple unpredictably |
| **High Cohesion** | Everything inside a module works toward one clear, unified purpose |
| **Low Cohesion** | A "grab bag" module where unrelated things are grouped together |
| **Dependency Injection (DI)** | Providing a component's dependencies from the outside rather than letting it create them |
| **Inversion of Control (IoC)** | The framework/caller controls when and how code runs, rather than the code controlling it |
| **Minimalism** | Use the fewest features, abstractions, and tools necessary to solve the problem |
| **Trade-off** | Accepting a cost in one -ility to gain in another; cannot optimize all -ilities simultaneously |
| **Tech Debt** | The accumulated future cost of shortcuts, rushed decisions, and deferred quality work |
| **Tech Investment** | Spending effort now (on architecture, cleanup, education) to gain future payoff |
| **Big Ball of Mud** | A system with no discernible architecture where everything is tangled together |
| **GETS** | Good Enough To Ship — a term for software that ships despite quality shortcuts |
| **Premature Optimization** | Tuning performance before profiling reveals the actual bottleneck |
| **Premature Abstraction** | Creating interfaces or base classes before variation has been proven necessary |
| **Premature Automation** | Automating a process before understanding it well enough to know what should be automated |
| **Lambo Problem** | Making architectural decisions for problems (scale, abstraction) you don't yet have |
| **Survivorship Bias** | Crediting an approach because it worked, while ignoring all the cases where it failed |
| **Architectural Astronaut** | An architect so distant from actual code that their decisions are disconnected from reality |
| **Technical Thermodynamics** | The natural tendency of systems toward entropy and disorder over time |
| **ADR** | Architectural Decision Record — a short document capturing a significant architectural decision and its rationale |
| **MADR** | Markdown Architectural Decision Record — a lightweight, plain-text ADR format |
| **Ubiquitous Language** | A shared vocabulary used consistently from stakeholder conversations all the way down to variable names |
| **System Model** | The internal, technical representation of how the software is organized |
| **User Model / Mental Model** | The conceptual map a user builds in their mind about how the system works |
| **Domain** | The real-world problem space a piece of software serves |
| **DDD** | Domain Driven Design — designing software so its structure mirrors the domain's concepts |
| **Solution-First Thinking** | Choosing a technology or pattern before understanding the problem — an anti-pattern |
| **Pace Layers** | The idea that different layers of technology change at radically different speeds |
| **Tech Fashion** | Popular technologies du jour; changes quickly; dangerous to build expertise around exclusively |

---

## Architectural Patterns

| Term | Definition |
|------|-----------|
| **Client-Server** | Fundamental web model: client makes requests, server fulfills them over a network |
| **Fat Client** | Architecture where the client does the bulk of computation; thin server |
| **Thin Client** | Architecture where the server does the bulk of computation; client is mostly display |
| **Localhost Effect** | Developer blind spot caused by working on a local machine with no real network latency or failures |
| **MPA** | Multi-Page Application — server renders and delivers a new HTML page for each request |
| **SPA** | Single-Page Application — one HTML shell; JS handles all routing and rendering client-side |
| **CRUD** | Create, Read, Update, Delete — the four fundamental operations on persistent data |
| **REST** | REpresentational State Transfer — URLs as nouns (resources), HTTP methods as verbs on those resources |
| **RESTful** | Following REST principles in spirit, if not with complete formal precision |
| **HTTP Methods** | GET (Read), POST (Create), PUT/PATCH (Update), DELETE (Delete) |
| **Endpoint** | A specific URL at which an API resource is accessible |
| **OpenAPI / Swagger** | Standard format for documenting REST APIs in a machine-readable way |
| **MVC** | Model-View-Controller — separates data/logic (Model), presentation (View), and input handling (Controller) |
| **MVP** | Model-View-Presenter — a MVC variant where the Presenter handles all view logic |
| **MVVM** | Model-View-ViewModel — a MVC variant using a data-binding ViewModel; common in reactive frameworks |
| **MV\*** | Umbrella term acknowledging that the Model-View separation is stable but the third component varies |
| **Monolith** | A single deployable unit containing all application logic |
| **Microservice** | A single-responsibility service deployed independently; communicates with others over a network |
| **Layered Architecture** | System organized in horizontal layers where each layer depends only on the layer below it |
| **Event-Driven** | System architecture where components communicate by emitting and reacting to events |
| **Pub-Sub** | Publisher-Subscriber pattern — publishers emit events; subscribers listen without direct coupling |
| **Event Delegation** | Attaching a single listener to a parent element to handle events from all child elements via bubbling |
| **Strategy Pattern** | Defining a family of interchangeable algorithms and selecting one at runtime |
| **Web Components** | Browser-native component model using Custom Elements, Shadow DOM, and HTML Templates |
| **Graceful Degradation** | Building for full capability first, then ensuring a reasonable experience when features are unavailable |
| **Progressive Enhancement** | Building a baseline functional experience first, then layering enhanced capabilities on top |
| **Separation of Concerns (layered web)** | HTML = structure, CSS = presentation, JS = behavior — each in its own layer |
| **GoF** | Gang of Four — authors of the influential *Design Patterns* book that codified software design patterns |

---

## Build, CI/CD, and Development Practices

| Term | Definition |
|------|-----------|
| **CI** | Continuous Integration — constantly integrating code changes into a shared, automatically tested build |
| **CD** | Continuous Delivery — automatically producing and delivering a passing build to production quickly |
| **CI/CD** | The combined practice of automating integration and delivery in a pipeline |
| **Build Pipeline** | An automated series of steps (lint, test, format, build, deploy) run on every code change |
| **Feature Flag / Feature Toggle** | A conditional that controls whether a feature is active for a given user or cohort |
| **A/B Testing** | Showing two variants of a feature to different user segments to measure which performs better |
| **Canary Release** | Releasing a change to a small subset of users before rolling it out broadly |
| **NoDUF** | No Design Up Front — avoiding all architectural planning before coding begins (anti-pattern) |
| **Design Engineering** | Doing enough upfront design to explore big ideas and obvious risks before committing |
| **Process Engineering** | Using iteration and experimentation to deal with uncertainty that remains after design |
| **Rocket Launch Commit** | A massive batch of changes pushed at once — high risk, hard to diagnose and roll back |
| **Linter / Linting** | Tool that statically analyzes code for style violations, bugs, or bad patterns (e.g., ESLint) |
| **Formatter** | Tool that automatically enforces code style by rewriting code to match a standard (e.g., Prettier) |
| **Pre-commit Hook** | A script that runs automatically before a git commit, used to enforce formatting or tests |
| **EditorConfig** | A file in the repo that configures editors to use consistent indentation and whitespace settings |
| **Husky** | A tool for adding git hooks (e.g., pre-commit formatters) to a Node.js project |
| **Style Guide** | A team's documented agreement on code formatting and naming conventions |
| **"Feel the Hate Before You Automate"** | The principle of doing a task manually first to understand it before deciding to automate |
| **Dogfooding** | Using your own product internally to find problems before users do |
| **Pull Request (PR)** | A mechanism to propose, review, and discuss code changes before merging them into the main branch |
| **Agile** | A development philosophy emphasizing iterative delivery, collaboration, and adaptability |
| **Sprint** | A fixed-length development cycle (typically 1–2 weeks) in Agile/Scrum |
| **Iteration** | One cycle of build-test-learn used to grow software incrementally |
| **Bikeshedding** | Disproportionate debate over trivial decisions (e.g., tabs vs. spaces) at the expense of important ones |

---

## Testing

| Term | Definition |
|------|-----------|
| **Test Pyramid** | Visual model: wide base of unit tests, middle layer of integration tests, narrow top of E2E tests |
| **Testing Trophy** | Alternative to the pyramid; wider integration test layer for frontend-heavy apps |
| **Testing Polygon** | The idea that the right test shape is domain-specific, not a universal pyramid |
| **Unit Testing** | Testing a single function or class in complete isolation, with all dependencies mocked |
| **Integration Testing** | Testing that two or more components work correctly together, without full mocking |
| **E2E Testing** | End-to-End Testing — testing the entire application from the user's perspective with all layers running |
| **Load Testing** | Simulating many concurrent users to find performance limits and failure modes |
| **Snapshot Testing / Pixel Testing** | Capturing a UI screenshot or DOM snapshot and comparing it to a saved baseline |
| **Compatibility Testing** | Verifying the application works across different browsers, OS, and devices |
| **UAT** | User Acceptance Testing — real users validate that the software actually solves their needs |
| **Code Coverage** | The percentage of source lines/branches executed when the test suite runs |
| **TDD** | Test Driven Development — Red (write failing test), Green (write minimal passing code), Refactor |
| **Red-Green-Refactor** | The three steps of TDD: failing test → passing code → clean code |
| **BDD** | Behavior Driven Development — writing test specs in natural language (Given-When-Then) from the user's perspective |
| **Given-When-Then** | The BDD syntax: Given (context), When (action), Then (expected outcome) |
| **Cucumber / Behave / SpecFlow** | BDD frameworks that parse natural-language scenarios and map them to executable test code |
| **Playwright / Cypress** | E2E testing frameworks that drive a real browser programmatically |
| **k6 / JMeter** | Load testing tools |
| **Percy / Chromatic** | Visual/snapshot regression testing tools |
| **axe-core** | Automated accessibility testing library |
| **BrowserStack** | Cross-browser and cross-device testing platform |
| **SRI** | Subresource Integrity — a browser mechanism that refuses to execute external resources whose content has been tampered with |
| **Golden Snapshot** | The saved baseline image that snapshot tests compare against |
| **False Positive** | A test that passes when the system is actually broken (or fails when it is actually fine) |
| **WCAG** | Web Content Accessibility Guidelines — the international standard for web accessibility |

---

## Operations, Monitoring, and DevOps

| Term | Definition |
|------|-----------|
| **DevOps** | A cultural movement that unifies software development and operations to eliminate silos |
| **Five Nines** | 99.999% availability — about 5.26 minutes of downtime per year |
| **SLA** | Service Level Agreement — a formal commitment about availability, performance, or support |
| **MTBF** | Mean Time Between Failures — average time a system runs without a failure |
| **Redundancy** | Duplicating critical components (servers, data centers, network paths) so no single failure causes downtime |
| **Chaos Engineering** | Deliberately injecting failures into a production system to test and improve resilience |
| **On-call** | A rotation where engineers are available to respond to production incidents outside business hours |
| **Incident Response** | The structured process of detecting, diagnosing, and resolving a production problem |
| **Remediation** | The act of fixing a problem discovered during monitoring or an incident |
| **Observability** | The ability to understand a system's internal state by examining its outputs (logs, metrics, traces) |
| **Logging** | Recording timestamped events and data from a running system for later analysis |
| **Log Rotation** | Periodically archiving or deleting old log files to manage storage |
| **PII** | Personally Identifiable Information — data that can identify a specific individual (must be protected in logs) |
| **Common Log Format** | A standard log format used by web servers |
| **RUM** | Real User Monitoring — capturing performance and behavior data from actual user sessions |
| **Synthetic Monitoring** | Testing system availability and behavior using automated bots/scripts rather than real users |
| **Navigation Timing API** | Browser API for measuring real page load performance in RUM |
| **KPI** | Key Performance Indicator — a metric used to measure progress toward a goal |
| **Technographics** | Basic usage stats — device type, browser, OS, geographic location |
| **Heatmap** | A visual overlay showing where users click, move, or look on a page |
| **Rage Click** | A signal in analytics where a user clicks the same element multiple times rapidly, indicating frustration |
| **Analytics** | Collection and analysis of user behavior data to inform product decisions |
| **Engagement Fallacy** | The incorrect assumption that high user activity metrics (clicks, time-on-page) always indicate user satisfaction |
| **Dark Pattern** | A UI deliberately designed to confuse or manipulate users into doing something they didn't intend |
| **Testing Effect** | The phenomenon where users behave differently because they know they are being observed or tested |
| **A/B Testing** | Showing two variants to different user segments to measure which produces better outcomes |
| **Beta Testing** | Pre-release testing with a selected group of real users |
| **Status Page** | A public page reporting system health; should be hosted independently from your main infrastructure |
| **ChatOps** | Using a team chat platform (e.g., Slack) as the command center for incident coordination |
| **Garden vs. Building** | Metaphor: software after launch is more like a garden (continuously tended) than a building (finished and static) |
| **Software Lifecycle** | The full arc of software from creation through operation, evolution, and eventual retirement |
| **SaaS** | Software as a Service — software delivered and operated over the internet on a subscription basis |

---

## Code Smells and Anti-Patterns

| Term | Definition |
|------|-----------|
| **Code Smell** | A surface indication in code that suggests a deeper design problem |
| **Magic Number** | A numeric literal in code with no named constant explaining what it represents |
| **God Function / God Class** | A function or class that does too many things, violating SRP |
| **Poor Naming** | Identifiers (variables, functions, files) that don't convey meaning or domain context |
| **Dead Code** | Code that is never executed or reachable |
| **Copy-Paste Coding** | Duplicating code instead of abstracting it — a DRY violation |
| **Spaghetti Code** | Code with no discernible structure and tangled control flow |
| **Breaking Abstraction** | Writing code that reaches past an abstraction's interface to access its internals |
| **Over-decomposition** | Breaking things into too many small pieces, increasing coupling and coordination overhead |
| **Under-decomposition** | Leaving too much in one place, creating a monolithic, hard-to-reason-about module |
| **Specificity War (CSS)** | When overly specific CSS selectors become impossible to override without even higher specificity |
| **Tight Specificity Chain** | A long CSS selector (e.g., `div#header ul li a`) that breaks when HTML structure changes |
| **XSS** | Cross-Site Scripting — injecting malicious scripts into a page via unsanitized user input |
| **MITM** | Man-in-the-Middle attack — intercepting network traffic to read or modify it |
| **innerHTML (misuse)** | Using `innerHTML` with unescaped user input — the primary vector for DOM-based XSS |
| **Client-Side Hashing (anti-pattern)** | Hashing passwords in the browser before sending; the hash becomes the password — insecure |
| **Scrap Heap Programming** | Copy-pasting together a working solution with no design consideration |
| **Vibe Understanding** | Accepting the general sense of something without actually understanding it — anti-engineering |

---

## Documentation

| Term | Definition |
|------|-----------|
| **ADR** | Architectural Decision Record — captures a decision, its context, alternatives, and rationale |
| **MADR** | Markdown ADR — a lightweight plain-text ADR format |
| **JSDoc** | A documentation format for JavaScript using `/** */` comment blocks with typed annotations |
| **OpenAPI / Swagger** | Standard for describing REST API contracts in a machine-readable YAML/JSON format |
| **Mermaid** | A Markdown-compatible diagram language that renders diagrams from text (diagram-as-code) |
| **PlantUML** | A text-based diagramming tool for sequence, class, and other UML diagrams |
| **Structurizr DSL** | A domain-specific language for describing software architecture as code |
| **Diagram-as-Code** | Storing diagrams as text files in version control so they are reviewed and updated alongside code |
| **Living Documentation** | Documentation that is co-located with code and updated in the same workflow, minimizing staleness |
| **Runbook** | A step-by-step guide for operating or troubleshooting a system |

---

## HTML / CSS / JS Specific Terms

| Term | Definition |
|------|-----------|
| **Semantic HTML** | Using HTML elements for their intended meaning (`<button>`, `<label>`, `<nav>`) rather than `<div>` for everything |
| **`<label for>`** | HTML element that associates a text label with a form input via the input's `id` — required for accessibility |
| **`type="tel"` / `type="email"` / `type="number"`** | Input types that trigger correct mobile keyboards, browser validation, and autofill behaviors |
| **`autocomplete`** | HTML attribute that enables browser autofill for form fields |
| **`required`** | HTML attribute that triggers browser-native validation requiring a field to be filled before submission |
| **`aria-required`** | ARIA attribute that signals a required field to assistive technology |
| **`aria-live`** | ARIA attribute that announces dynamic DOM changes to screen readers |
| **`defer` / `async`** | `<script>` attributes that prevent render-blocking: `defer` executes after HTML parse; `async` executes immediately when loaded |
| **`textContent`** | DOM property for inserting plain text — safe against XSS; does not parse HTML |
| **`innerHTML`** | DOM property for inserting HTML markup — dangerous with unsanitized input; enables XSS |
| **`dataset`** | Access to `data-*` custom attributes on HTML elements (`el.dataset.target`) |
| **`data-*` attributes** | Custom HTML attributes used to embed data in elements without misusing class or id |
| **CSS Custom Properties** | CSS variables defined with `--name: value` in `:root` and referenced with `var(--name)` |
| **Design Token** | A named value for a design decision (color, spacing, font-size) used as a CSS custom property |
| **CSS Specificity** | The algorithm determining which CSS rule applies when multiple rules match the same element |
| **`closest()`** | JS method that traverses up the DOM tree to find the nearest ancestor matching a selector |
| **`querySelectorAll()`** | Returns all elements matching a CSS selector as a NodeList |
| **`addEventListener()`** | Attaches an event handler to an element; used on a parent for event delegation |
| **Event Bubbling** | The mechanism by which events propagate up the DOM from the target element through its ancestors |
| **`fetch()`** | Browser API for making HTTP requests and receiving responses asynchronously |
| **`async` / `await`** | JavaScript syntax for writing asynchronous code in a synchronous style |
| **`localStorage`** | Browser storage for persisting key-value data across sessions (no expiry) |
| **`URLSearchParams`** | Browser API for parsing query string parameters from a URL |
| **SRI hash** | A cryptographic hash in a `<script>` or `<link>` tag's `integrity` attribute that the browser uses to verify the resource hasn't been tampered with |

---

## Quick Acronym Reference

| Acronym | Stands For |
|---------|-----------|
| **ADR** | Architectural Decision Record |
| **MADR** | Markdown Architectural Decision Record |
| **SRP** | Single Responsibility Principle |
| **DRY** | Don't Repeat Yourself |
| **KISS** | Keep It Simple Stupid |
| **YAGNI** | You Ain't Gonna Need It |
| **CI** | Continuous Integration |
| **CD** | Continuous Delivery |
| **CI/CD** | Continuous Integration / Continuous Delivery |
| **UAT** | User Acceptance Testing |
| **TDD** | Test Driven Development |
| **BDD** | Behavior Driven Development |
| **E2E** | End-to-End (testing) |
| **MVC** | Model-View-Controller |
| **MVP** | Model-View-Presenter |
| **MVVM** | Model-View-ViewModel |
| **SPA** | Single-Page Application |
| **MPA** | Multi-Page Application |
| **CRUD** | Create, Read, Update, Delete |
| **REST** | REpresentational State Transfer |
| **DI** | Dependency Injection |
| **IoC** | Inversion of Control |
| **DDD** | Domain Driven Design |
| **DevOps** | Development + Operations (cultural movement) |
| **RUM** | Real User Monitoring |
| **KPI** | Key Performance Indicator |
| **PII** | Personally Identifiable Information |
| **SaaS** | Software as a Service |
| **SRI** | Subresource Integrity |
| **XSS** | Cross-Site Scripting |
| **MITM** | Man-in-the-Middle (attack) |
| **GETS** | Good Enough To Ship |
| **WCAG** | Web Content Accessibility Guidelines |
| **GoF** | Gang of Four (Design Patterns authors) |
| **MTBF** | Mean Time Between Failures |
| **NoDUF** | No Design Up Front |
| **SRS** | Software Requirements Specification |
| **UCD** | User-Centered Design |
