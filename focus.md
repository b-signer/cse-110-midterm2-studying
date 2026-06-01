# Midterm 2 Spring 2026 Review

The second midterm will continue the first in the sense that our goal is to think about and apply the SWE concepts presented in
situation or to critically think about those ideas and reason about likely outcomes.
The lectures channel contains all the content you need to study which starts basically from May 8th until May 28th. This includes ALL
content except the data about team participation standings from Helena, that means the patterns code, the PDF screens and captures,
and any links. However, do note the details of the linked Martin Fowler article aren't important just the sense of that article which is
captured in slides or Craft document when it talks about A-B testing, feature toggles, etc.
As code will be seen you likely are very worried about writing code, that is not where you should focus. You are not going to be asked
to write code from scratch, but you will be asked to rework, comment upon, review, or pick between code ideas. Assume we are
considering you are evaluating code of others, from an LLM, etc. This means you must understand code and concepts. You also may
need to re-arrange or refactor what is provided, but the constructs should generally be included in the snippet.
Concepts you must make sure to understand
The -ilities and how they drive engineering and architectural decisions. This may go all the way up to testing and monitoring
thoughts. Think about how the iron triangle was applied in this way and you'll see questions you might ask in context. Often
questions that have you weigh implementation trade-offs and explain the pros and cons of approaching a problem a particular way
can be found on the second midterm.
•
High level Architectural Concepts - Cohesion, Coupling, Single Responsible Principle (SRP), Separation of Concerns, DRY (Don't
Repeat Yourself), Encapsulation, Abstraction, Dependency Injection, Minimalism
•
High level Architectural patterns and philosophies applied and conceptually - client-server, layered models (particularly the web
one we are using with HTML, CSS, JS), graceful degradation and progressive enhancement, MPA, SPA, CRUD, REST, MVC, the use
of components (particularly with web components), pub-sub, event delegation, dependency injection, and the strategy pattern.
•
Common Code Smells - applied examples in HTML, CSS and JS with magic numbers, poor commenting, breaking abstractions,
poor naming, etc. may be mixed in here as well
•
How Agile and SWE thoughts relate to the tooling we actually use or practices we perform - for example why is CI/CD such a big
deal with Agile? why do we do pull requests? what are the typical steps in a build pipeline?
•
• Testing forms and the testing pyramid.
• TDD and BDD - what they are, how they can help, why they might not be used as much, how we might use them going forward
The idea of software evolving over time and have a birth to death lifecycle. Understanding how our choices might effect the health
of the software over time and what things we do that might hurt or could help it.
•
• The importance of documentation and the common forms we have tried to use from ADRs to JSDoc comments.
Likely some reasoning questions related to how GenAI affects some of the concepts here, particularly in terms of tech debt,
preservation of complexity, shifting the difficulties in software engineering steps, etc.
•
• Questions about your team or project (should be easy points if you've been involved)