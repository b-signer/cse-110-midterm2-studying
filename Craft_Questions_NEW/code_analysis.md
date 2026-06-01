# Review and Analysis Questions — Code Analysis and Refactoring

---

## Code Smells: Magic Numbers, Poor Naming, and Missing Abstraction

**Q: Review the following JavaScript. Identify every code smell you can find, name which -ilities are most at risk, and refactor it.**

```javascript
function calc(x, y, z) {
    if (z === 1) {
        return x * y * 0.9;
    } else if (z === 2) {
        return x * y * 0.85;
    } else if (z === 3) {
        return x * y * 0.75;
    }
    return x * y;
}

function process(items) {
    let t = 0;
    for (let i = 0; i < items.length; i++) {
        t += calc(items[i].p, items[i].q, items[i].type);
    }
    return t;
}
```

**Code smells present:**

- **Magic numbers:** `0.9`, `0.85`, `0.75`, `1`, `2`, `3` all appear with no explanation of what they represent. A reader cannot know that `0.85` means a 15% discount for the silver tier without reading surrounding context that doesn't exist.
- **Poor naming throughout:** `calc`, `x`, `y`, `z`, `t`, `p`, `q` are all single-letter or meaningless identifiers. These names carry zero domain information — there is no hint that this computes a discounted order total.
- **No documentation:** No JSDoc, no comments explaining what the tiers mean, what the function expects, or what it returns.
- **No abstraction for the tier system:** The discount rates are baked directly into conditionals rather than expressed as a named, centralized data structure. Adding a fourth tier requires editing the conditional chain and knowing which numbers are "correct."

**-ilities most at risk:**

- **Maintainability:** A new developer (or the original developer six months later) cannot modify this safely without understanding the entire implicit model. Changing a discount rate requires knowing which magic number to update.
- **Testability:** You cannot write a meaningful test that communicates intent — `calc(100, 2, 2)` in a test is as opaque as the production code.
- **Modifiability:** Adding a new tier or changing a discount rate requires touching the function body directly, with no guardrail against accidentally breaking another tier's logic.

**Refactored:**

```javascript
// Single source of truth for all tier discounts.
// To add a new tier or change a rate, edit exactly one line here — nowhere else.
const TIER_DISCOUNTS = {
    bronze: 0.90,  // 10% discount
    silver: 0.85,  // 15% discount
    gold:   0.75,  // 25% discount
};

/**
 * Calculates the discounted subtotal for a single line item.
 * @param {number} unitPrice - Price per unit in dollars
 * @param {number} quantity - Number of units
 * @param {string} tier - Customer tier: 'bronze' | 'silver' | 'gold'
 * @returns {number} Discounted line total
 */
function calculateLineTotal(unitPrice, quantity, tier) {
    // ?? 1 = nullish coalescing: if tier is unrecognized, fall back to no discount (multiplier of 1)
    const discountMultiplier = TIER_DISCOUNTS[tier] ?? 1;
    return unitPrice * quantity * discountMultiplier;
}

/**
 * Calculates the total cost of an order across all line items.
 * @param {Array<{unitPrice: number, quantity: number, tier: string}>} items
 * @returns {number} Order total in dollars
 */
function calculateOrderTotal(items) {
    return items.reduce(
        (total, item) => total + calculateLineTotal(item.unitPrice, item.quantity, item.tier),
        0  // initial accumulator value — starts the running total at zero
    );
}
```

The `TIER_DISCOUNTS` object is now the single source of truth for discount rates — adding or changing a tier requires editing one named entry. The JSDoc makes both functions independently understandable and testable. A test can now read `calculateLineTotal(100, 2, 'silver')` and immediately convey its intent.

---

## Semantic HTML and the Accessibility -ility

**Q: The following HTML is for a contact form. Identify every -ility violation, explain the specific problem each causes, and rewrite the HTML to fix them.**

```html
<div onclick="submitForm()">Submit</div>
<div class="red-text">* Required fields</div>

<div class="input-box">
    <div>Name:</div>
    <input type="text" id="n1">
</div>
<div class="input-box">
    <div>Email:</div>
    <input type="text" id="e1">
</div>
<div class="input-box">
    <div>Age:</div>
    <input type="text" id="a1">
</div>
```

**-ility violations:**

- **Usability / Accessibility — `<div onclick>` as a button:** A `<div>` is not a native interactive element. It cannot receive keyboard focus via Tab, cannot be activated with Enter or Space, and is not announced as a button to screen readers. Any user who doesn't use a mouse — including keyboard-only users and screen reader users — cannot submit this form. Fix: use `<button type="submit">`.

- **Usability / Accessibility — no `<label>` elements:** The text "Name:", "Email:", and "Age:" are plain `<div>` tags with no programmatic association to the inputs below them. A screen reader landing on `#n1` hears "text field" with no label — the user has no idea what to type. Fix: use `<label for="...">` tied to each input's `id`.

- **Usability — wrong `type` attributes:** `type="text"` on the email field misses the built-in email format validation, the `@` keyboard shortcut on mobile, and browser autofill for email addresses. `type="text"` on the age field misses the numeric keyboard on mobile and numeric input semantics. Fix: `type="email"` and `type="number"` respectively.

- **Maintainability — presentational class name:** `class="red-text"` encodes a visual detail (red color) as the class name. If the design changes and required fields are now indicated in orange, the class name becomes a lie. Fix: name by purpose — `class="required-notice"` — and control the color in CSS.

- **Usability / Robustness — no `<form>` element:** Without a `<form>`, native browser behaviors (Enter-to-submit, built-in validation UI, browser password manager integration) are unavailable. Fix: wrap in `<form>`.

- **Robustness — no `required` attributes:** The server must validate regardless, but the absence of `required` removes the browser's first-line validation feedback, worsening the UX.

- **System/User Model — cryptic IDs:** `n1`, `e1`, `a1` are the developer's internal shorthand, not domain language. Fix: `id="name"`, `id="email"`, `id="age"`.

**Refactored:**

```html
<!-- <form> restores native behavior: Enter-to-submit, built-in validation UI,
     and browser password manager / autofill integration -->
<form id="contact-form">
    <!-- Class name describes PURPOSE ("required-notice"), not appearance ("red-text").
         If the color changes to orange, the class name stays accurate. -->
    <p class="required-notice">* Required fields</p>

    <div class="form-field">
        <!-- for="name" programmatically associates this label with the input whose id="name".
             Screen readers announce "Name" when the input is focused. -->
        <label for="name">Name: *</label>
        <!-- autocomplete="name" lets the browser prefill from saved contact profiles -->
        <input type="text" id="name" name="name" autocomplete="name" required>
    </div>

    <div class="form-field">
        <label for="email">Email: *</label>
        <!-- type="email" triggers the @ keyboard on mobile and enables browser format validation -->
        <input type="email" id="email" name="email" autocomplete="email" required>
    </div>

    <div class="form-field">
        <label for="age">Age:</label>
        <!-- type="number" triggers the numeric keypad on mobile.
             min/max give the browser bounds for its built-in validation. -->
        <input type="number" id="age" name="age" min="0" max="120">
    </div>

    <!-- <button type="submit"> is keyboard-focusable, activatable with Enter or Space,
         and announced as "Submit button" by screen readers — a <div> provides none of this -->
    <button type="submit">Submit</button>
</form>
```

---

## SRP Violation: The God Function

**Q: The following function handles user login. Identify every Single Responsibility Principle (SRP) violation, explain how each violation damages testability, and describe what a refactored structure would look like. Also identify the security issue hidden in the implementation.**

```javascript
async function handleUserLogin(username, password) {
    if (!username || username.length < 3) {
        document.getElementById('error-msg').textContent = 'Username must be at least 3 characters.';
        document.getElementById('error-msg').style.display = 'block';
        return;
    }
    if (!password || password.length < 8) {
        document.getElementById('error-msg').textContent = 'Password must be at least 8 characters.';
        document.getElementById('error-msg').style.display = 'block';
        return;
    }

    const encoder = new TextEncoder();
    const encoded = encoder.encode(password);
    const hashBuffer = await crypto.subtle.digest('SHA-256', encoded);
    const hash = Array.from(new Uint8Array(hashBuffer))
        .map(b => b.toString(16).padStart(2, '0')).join('');

    const response = await fetch('/api/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password: hash }),
    });

    const result = await response.json();
    if (result.success) {
        localStorage.setItem('user_token', result.token);
        localStorage.setItem('user_id', result.userId);
        localStorage.setItem('user_name', result.username);
        window.location.href = '/dashboard';
    } else {
        document.getElementById('error-msg').textContent = result.message;
        document.getElementById('error-msg').style.display = 'block';
    }
}
```

**SRP violations — this function has at least five responsibilities:**

1. **Input validation** — checking username/password length rules
2. **DOM manipulation / error display** — directly touching `document.getElementById` to show error messages
3. **Cryptographic hashing** — running `crypto.subtle.digest`
4. **API communication** — `fetch('/api/login', ...)`
5. **Session management** — writing to `localStorage`
6. **Navigation** — `window.location.href = '/dashboard'`

**How each violation damages testability:** To unit test the validation logic alone, you need a DOM environment with an element whose id is `error-msg`, or the function throws. To test the API call, you must let the hash run first. To test session storage, you must mock the entire network layer. There is no way to test any one responsibility in isolation — the function is one giant integration point, not a composable unit.

**The hidden security issue:** Client-side hashing with SHA-256 before sending to the server is a security anti-pattern. The server receives the hash and treats it as the credential. If the server's credential store is ever breached and an attacker obtains `SHA-256(password)`, they can log in by sending that hash directly — they do not need to crack it. The hash effectively becomes the password. Password hashing must be performed server-side with a slow, salted algorithm (bcrypt, Argon2) so that even a DB breach does not yield usable credentials.

**Refactored structure (decomposed):**

```javascript
// Responsibility 1: pure validation logic — no DOM, no network, no side effects.
// Can be unit tested with a single function call and zero browser mocking.
function validateLoginInputs(username, password) {
    if (!username || username.length < 3) return 'Username must be at least 3 characters.';
    if (!password || password.length < 8)  return 'Password must be at least 8 characters.';
    return null; // null = inputs are valid
}

// Responsibility 2: the ONLY function that touches the DOM.
// Isolating DOM manipulation here means all other functions are DOM-free and fully testable in Node.
function showError(message) {
    const el = document.getElementById('error-msg');
    el.textContent = message; // textContent, not innerHTML — safe against XSS
    el.style.display = 'block';
}

// Responsibility 3: network call only — can be mocked independently in tests.
// Password sent as plaintext over HTTPS; the SERVER hashes it with bcrypt/Argon2.
// Client-side hashing would make the hash itself the credential — a security anti-pattern
// because anyone who steals the hash can log in by replaying it directly.
async function postLoginCredentials(username, password) {
    const response = await fetch('/api/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, password }),
    });
    return response.json();
}

// Responsibility 4: session persistence only.
// Testable by checking localStorage state after calling this function in isolation.
function saveSession(token, userId, username) {
    localStorage.setItem('user_token', token);
    localStorage.setItem('user_id', userId);
    localStorage.setItem('user_name', username);
}

// Orchestrator: thin coordinator that sequences the above steps.
// Each step is independently testable; this function is testable as an integration.
async function handleUserLogin(username, password) {
    const validationError = validateLoginInputs(username, password);
    if (validationError) { showError(validationError); return; }

    const result = await postLoginCredentials(username, password);

    if (result.success) {
        saveSession(result.token, result.userId, result.username);
        window.location.href = '/dashboard'; // navigation is the final side effect — state is saved first
    } else {
        showError(result.message);
    }
}
```

Now `validateLoginInputs` can be unit tested with zero DOM dependency. `postLoginCredentials` can be tested by mocking `fetch`. `saveSession` can be tested by checking `localStorage` state in isolation. The DOM coupling (`showError`) is isolated to one named function.

---

## CSS: DRY Violations and Overly Specific Selectors

**Q: Review the following CSS. Identify every DRY violation and code smell, explain which -ilities are most at risk, and rewrite it to address the problems.**

```css
div#header div.nav ul li a {
    color: #3a86ff;
    font-size: 14px;
    padding: 8px 16px;
}

div#header div.nav ul li a:hover {
    color: #2563eb;
    background: rgba(58, 134, 255, 0.1);
}

.footer a {
    color: #3a86ff;
    font-size: 14px;
}

.sidebar a {
    color: #3a86ff;
}

#contact-form input {
    border: 1px solid #ccc;
    padding: 8px;
    font-size: 14px;
    width: 280px;
}

#contact-form textarea {
    border: 1px solid #ccc;
    padding: 8px;
    font-size: 14px;
    width: 280px;
    height: 120px;
}
```

**DRY violations and code smells:**

- **`#3a86ff` appears 3 times** (nav link, footer link, sidebar link) — this is a design token (the brand link color) with no single source of truth. Changing the brand color requires finding and updating three selectors, with no guarantee all instances are caught.
- **`font-size: 14px` appears 4 times** — a base font size with no central definition.
- **`padding: 8px` appears 3 times** — a spacing value with no central definition.
- **`border: 1px solid #ccc` appears twice** — the form field border style duplicated across `input` and `textarea`.
- **`width: 280px` appears twice** — a magic number with no name describing what it represents (the form field width).
- **`div#header div.nav ul li a`** — a five-level specificity chain. This selector breaks the moment anyone restructures the HTML (removes the `<ul>`, flattens the nav). The `div` type selectors add nothing meaningful and inflate specificity, making the rule harder to override. It also tightly couples the CSS to a specific DOM structure — a **maintainability** and **modifiability** risk.

**-ilities most at risk:**

- **Maintainability:** A designer changing the brand color must hunt through the stylesheet; a developer restructuring the HTML may silently break the nav styling without realizing the selector's fragility.
- **Modifiability:** Every new component that needs standard spacing, border, or color must duplicate the raw values rather than referencing a named token.

**Refactored:**

```css
/* ── Design Tokens ──────────────────────────────────────────────────────────
   All raw values live here and nowhere else (DRY).
   To change the brand color, update --color-link once — every rule that uses
   var(--color-link) updates automatically. No grep, no missed instances.
   ────────────────────────────────────────────────────────────────────────── */
:root {
    --color-link:          #3a86ff;              /* primary brand link color */
    --color-link-hover:    #2563eb;              /* darker shade on hover */
    --color-link-bg-hover: rgba(58, 134, 255, 0.1); /* same hue as link at low opacity */
    --color-border:        #ccc;                 /* standard input border */
    --font-size-base:      14px;                 /* base text size across all components */
    --spacing-sm:          8px;                  /* small spacing unit */
    --spacing-md:          16px;                 /* medium spacing unit (2× small) */
    --form-field-width:    280px;                /* named — clear what it is; easy to change */
}

/* Flat class selector — not coupled to any HTML structure.
   The original "div#header div.nav ul li a" breaks if the HTML is ever reorganized.
   This class works wherever .nav-link appears, regardless of nesting. */
.nav-link {
    color: var(--color-link);
    font-size: var(--font-size-base);
    padding: var(--spacing-sm) var(--spacing-md);
}

.nav-link:hover {
    color: var(--color-link-hover);
    background: var(--color-link-bg-hover);
}

/* Grouped selector — both share identical declarations, so we write them once.
   If the shared style needs to change, one edit covers both. */
.footer a,
.sidebar a {
    color: var(--color-link);
    font-size: var(--font-size-base);
}

/* Base style shared by both input and textarea.
   Apply this class in HTML to any form field that should match this style. */
.form-field {
    border: 1px solid var(--color-border);
    padding: var(--spacing-sm);
    font-size: var(--font-size-base);
    width: var(--form-field-width);
}

/* textarea extends .form-field — only the ONE property that differs is added here.
   This is the correct pattern: inherit the base, override the exception. */
#contact-form textarea.form-field {
    height: 120px;
}
```

The color, spacing, and size values are now defined once in `:root` as named custom properties. Changing the brand color is a single-line edit. The nav selector is a flat class rather than a fragile DOM-path chain.

---

## Security -ility: XSS and External Resource Risks

**Q: The following HTML/JS snippet is from a landing page. Identify every security and reliability -ility risk, explain the specific attack or failure mode each enables, and describe how to fix each.**

```html
<div id="welcome"></div>

<img src="https://cdn.third-party.com/hero.jpg">
<link rel="stylesheet" href="http://fonts.external.com/typography.css">
<script src="http://analytics.vendor.com/tracker.js"></script>

<script>
    const params = new URLSearchParams(window.location.search);
    const name = params.get('name');
    document.getElementById('welcome').innerHTML = 'Hello, ' + name + '!';
</script>
```

**Risk 1 — Cross-Site Scripting (XSS) via `innerHTML`:**
The URL query parameter `name` is read directly from `window.location.search` and written into the DOM via `innerHTML` without any sanitization. An attacker can craft a link such as `?name=<img src=x onerror=alert(document.cookie)>` and trick a user into clicking it. The browser parses the injected HTML, executes the `onerror` handler, and the attacker's script runs in the victim's browser with full access to cookies, session tokens, and the page's DOM. This is a stored-less reflected XSS attack. **Fix:** Use `textContent` instead of `innerHTML`. `textContent` treats the value as a plain string — no HTML is parsed, no scripts execute.

```javascript
// textContent treats the value as a plain string — the browser NEVER parses it as HTML.
// innerHTML would let an attacker inject: ?name=<img src=x onerror=alert(document.cookie)>
// and the browser would execute the onerror handler as a script.
document.getElementById('welcome').textContent = 'Hello, ' + name + '!';
```

**Risk 2 — MITM injection via `http://` resources:**
Both the stylesheet and the analytics script are loaded over unencrypted `http://`. Any network intermediary (a compromised router, a coffee shop access point, an ISP) can intercept the HTTP response and replace the stylesheet with CSS that exfiltrates form data via `background: url(...)` requests, or replace the script with arbitrary JavaScript that takes over the page entirely. **Fix:** All third-party resources must use `https://`. Additionally, add **Subresource Integrity (SRI)** hashes so the browser refuses to execute a resource whose content has been tampered with, even over HTTPS:

```html
<!-- integrity= provides a cryptographic hash the browser checks before executing the file.
     If the file's content doesn't match the hash — even over HTTPS — the browser refuses to run it.
     This defeats supply-chain attacks where a CDN or domain is compromised. -->
<!-- crossorigin="anonymous" is REQUIRED for SRI to work on cross-origin resources.
     Without it the browser can't read the response headers needed for the integrity check. -->
<script src="https://analytics.vendor.com/tracker.js"
        integrity="sha384-[hash]"
        crossorigin="anonymous"></script>
```

**Risk 3 — Third-party CDN reliability dependency:**
The hero image, stylesheet, and analytics script all depend on third-party domains staying available, correctly configured, and not hijacked. If `cdn.third-party.com` goes down, the hero image is broken. If `fonts.external.com` is slow, the page blocks rendering (stylesheets are render-blocking). If `analytics.vendor.com`'s domain registration lapses and is acquired by an attacker, every page that loads this script has now handed an attacker arbitrary JS execution access. **Fix:** Self-host any asset that is critical to page function (fonts, stylesheets). For truly third-party services (analytics), evaluate whether the functionality is worth the reliability and security dependency — and if kept, use SRI and `async`/`defer` so the script is not render-blocking and a loading failure does not break the page.

---

## DRY and Event Delegation

**Q: The following JavaScript registers click handlers for a four-tab navigation. Identify the DRY violations and coupling issues, explain why event delegation is the appropriate fix, and refactor it using event delegation along with the corresponding HTML changes.**

```javascript
document.getElementById('btn-home').addEventListener('click', function () {
    document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
    document.getElementById('btn-home').classList.add('active');
    document.getElementById('section-home').style.display = 'block';
    document.getElementById('section-about').style.display = 'none';
    document.getElementById('section-contact').style.display = 'none';
    document.getElementById('section-portfolio').style.display = 'none';
});

document.getElementById('btn-about').addEventListener('click', function () {
    document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
    document.getElementById('btn-about').classList.add('active');
    document.getElementById('section-home').style.display = 'none';
    document.getElementById('section-about').style.display = 'block';
    document.getElementById('section-contact').style.display = 'none';
    document.getElementById('section-portfolio').style.display = 'none';
});

document.getElementById('btn-contact').addEventListener('click', function () {
    document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
    document.getElementById('btn-contact').classList.add('active');
    document.getElementById('section-home').style.display = 'none';
    document.getElementById('section-about').style.display = 'none';
    document.getElementById('section-contact').style.display = 'block';
    document.getElementById('section-portfolio').style.display = 'none';
});

document.getElementById('btn-portfolio').addEventListener('click', function () {
    document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
    document.getElementById('btn-portfolio').classList.add('active');
    document.getElementById('section-home').style.display = 'none';
    document.getElementById('section-about').style.display = 'none';
    document.getElementById('section-contact').style.display = 'none';
    document.getElementById('section-portfolio').style.display = 'block';
});
```

**DRY violations and coupling issues:**

The identical pattern (deactivate all buttons, activate the clicked one, hide all sections, show the target section) is copy-pasted four times with only the target name changing. Adding a fifth tab requires: (1) writing a fifth handler, (2) adding a fifth `style.display = 'none'` line to every existing handler. Adding a sixth tab requires the same change across five handlers. The number of edits required grows as O(n²) — this is the clearest possible sign of a DRY violation that will accumulate tech debt with every new tab.

The code is also **tightly coupled to the DOM structure**: each handler has hardcoded references to every section ID. If a section is renamed, the code breaks in four places.

**Why event delegation is the correct fix:**

Event delegation means attaching a single listener to a parent element and letting browser event bubbling deliver clicks from child elements up to that listener. The listener then inspects `event.target` to determine which child was clicked. This produces one handler instead of N handlers, and that handler contains the logic once rather than N times. Adding a new tab requires only a new HTML element — no JavaScript changes at all, because the handler discovers the available tabs and sections from the DOM at runtime rather than having them hardcoded.

**Refactored HTML:**

```html
<nav id="main-nav">
    <!-- data-target stores which section this button reveals.
         The JS reads this at click time — the button is self-describing.
         Adding a 5th tab = add one <button> here. Zero JS changes required. -->
    <button class="nav-btn" data-target="home">Home</button>
    <button class="nav-btn" data-target="about">About</button>
    <button class="nav-btn" data-target="contact">Contact</button>
    <button class="nav-btn" data-target="portfolio">Portfolio</button>
</nav>

<main>
    <!-- Convention: id must be "section-" + the data-target value on the matching button.
         Adding a 5th section = add one <section> here. Zero JS changes required. -->
    <section id="section-home">...</section>
    <section id="section-about">...</section>
    <section id="section-contact">...</section>
    <section id="section-portfolio">...</section>
</main>
```

**Refactored JS:**

```javascript
// Cache DOM references once at startup — not inside the handler on every click
const nav      = document.getElementById('main-nav');
const buttons  = document.querySelectorAll('.nav-btn');
const sections = document.querySelectorAll('main > section');

// Single listener on the PARENT nav element.
// Clicks on any child button bubble up to here — this is event delegation.
nav.addEventListener('click', (e) => {
    // closest() walks up from e.target to find the nearest .nav-btn ancestor.
    // This handles clicks on any child element inside a button (e.g. an icon span)
    // that would otherwise set e.target to the icon, not the button.
    const btn = e.target.closest('.nav-btn');
    if (!btn) return; // click landed on the <nav> background, not a button — ignore it

    // Read which section to show from the button's own data attribute.
    // No hardcoded list of section names — the button tells us what to do.
    const target = btn.dataset.target;

    // Clear active state from all buttons, then mark only the clicked one
    buttons.forEach(b => b.classList.remove('active'));
    btn.classList.add('active');

    // Show the matching section; hide all others.
    // Template literal builds the expected id: "section-" + target (e.g. "section-about")
    sections.forEach(s => {
        s.style.display = s.id === `section-${target}` ? 'block' : 'none';
    });
});
```

One listener. One place to update logic. Adding a fifth tab is now zero-JS work — add a `<button data-target="faq">` and a `<section id="section-faq">` to the HTML and it works automatically.

---

## System Model vs. User Model: Domain Language in HTML and API Design

**Q: The following code is a complete feature — a form and its submission handler. Analyze it from the perspective of system model vs. user model alignment. Identify every place where the developer's internal model has leaked into the user-facing interface, and rewrite it assuming this is a "Support Ticket Submission" form.**

```html
<form id="frm1">
    <label for="f1">Field 1:</label>
    <input type="text" id="f1" name="f1">

    <label for="f2">Field 2:</label>
    <input type="text" id="f2" name="f2">

    <label for="f3">Field 3:</label>
    <select id="f3" name="f3">
        <option value="t1">Type 1</option>
        <option value="t2">Type 2</option>
        <option value="t3">Type 3</option>
    </select>

    <button type="submit">Execute Operation</button>
</form>

<script>
document.getElementById('frm1').addEventListener('submit', async (e) => {
    e.preventDefault();
    const payload = {
        f1: document.getElementById('f1').value,
        f2: document.getElementById('f2').value,
        f3: document.getElementById('f3').value,
    };
    await fetch('/api/v1/execute-operation', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload),
    });
});
</script>
```

**System model vs. user model analysis:**

The system model (the developer's internal implementation view) has been exposed wholesale to the user. Every identifier reflects how the developer stored or thought about the data internally, with zero translation into the user's conceptual world:

- `frm1` — the developer's variable name for the first form on the page, not a description of what the form does
- `f1`, `f2`, `f3` — positional field identifiers; "Field 1" tells a user nothing about what to type
- `t1`, `t2`, `t3` — internal option codes with no meaning to a user reading a dropdown
- `"Execute Operation"` — developer jargon for a submit action; no user thinks of filling out a form as "executing an operation"
- `/api/v1/execute-operation` — a verb-based endpoint that violates REST principles (REST endpoints should be nouns representing resources, with HTTP methods as verbs); additionally it exposes the API version in a way that will create consumer pain when v2 ships
- The `payload` object keys (`f1`, `f2`, `f3`) mean the server receives data named after field positions rather than domain concepts — downstream code must then guess what `f1` represents

**Misalignment consequence:** A user looking at "Field 1" with a text input has no idea what to type. A screen reader user hears "field one, text field" — no label, no context. Even a developer reviewing this form cannot understand its purpose without reading surrounding code. The system model is opaque to everyone, including the developers maintaining it.

**Refactored (as a Support Ticket Submission form):**

```html
<!-- id uses domain language: "support-ticket-form", not "frm1" -->
<form id="support-ticket-form">
    <div class="form-field">
        <label for="subject">Subject: *</label>
        <!-- placeholder gives a concrete example — reduces ambiguity about expected content -->
        <input type="text" id="subject" name="subject"
               placeholder="Brief description of your issue" required>
    </div>

    <div class="form-field">
        <label for="contact-email">Your Email: *</label>
        <!-- type="email" enables the @ keyboard on mobile and format validation in the browser -->
        <!-- autocomplete="email" lets the browser prefill from saved profiles -->
        <input type="email" id="contact-email" name="contactEmail"
               autocomplete="email" required>
    </div>

    <div class="form-field">
        <label for="issue-type">Issue Type: *</label>
        <select id="issue-type" name="issueType" required>
            <!-- Empty default option with no value forces the user to make an active choice.
                 required + value="" means the browser blocks submit if this is still selected. -->
            <option value="">-- Select a category --</option>
            <!-- option values are domain slugs ("billing"), not internal codes ("t2") -->
            <option value="billing">Billing</option>
            <option value="technical">Technical Problem</option>
            <option value="account">Account Access</option>
        </select>
    </div>

    <!-- Button label is a user action in plain language, not developer jargon -->
    <button type="submit">Submit Ticket</button>
    <!-- aria-live="polite" tells screen readers to announce any text changes
         to this element without interrupting whatever they are currently reading -->
    <p id="submission-status" aria-live="polite"></p>
</form>

<script>
document.getElementById('support-ticket-form').addEventListener('submit', async (e) => {
    e.preventDefault(); // stop the browser's default full-page form submission

    const status = document.getElementById('submission-status');

    // Payload keys use domain vocabulary — the server receives "subject", "contactEmail",
    // "issueType" instead of positional codes like "f1", "f2", "f3" that require
    // a silent translation layer on the server side.
    const ticket = {
        subject:      document.getElementById('subject').value,
        contactEmail: document.getElementById('contact-email').value,
        issueType:    document.getElementById('issue-type').value,
    };

    // POST to a REST noun endpoint: /api/tickets = the ticket resource collection.
    // HTTP POST means "create a new resource" — no verb needed in the URL itself.
    // Contrast with the original: /api/v1/execute-operation (verb-based, not RESTful).
    const response = await fetch('/api/tickets', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(ticket),
    });

    // response.ok is true for any 2xx status code — no need to parse the body just to check success.
    // Writing to #submission-status triggers the aria-live announcement for screen readers.
    status.textContent = response.ok
        ? 'Your ticket has been submitted. We\'ll be in touch shortly.'
        : 'Something went wrong. Please try again.';
});
</script>
```

Every label now speaks the user's language. The API endpoint `/api/tickets` is a REST noun (a ticket is a resource; `POST` creates one). The payload keys (`subject`, `contactEmail`, `issueType`) match domain concepts that both the UI and the server can reason about without translation. The `aria-live="polite"` region on the status paragraph announces the submission result to screen reader users. The user model and system model are now aligned.
