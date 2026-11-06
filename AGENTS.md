# AGENTS.md :: Instruction Manual for the Digital Grimoire

> You are an AI agent working on the **Digital Grimoire**. Read this whole file before you change anything.
> These rules are strict. If a user request conflicts with a rule here, follow this file and tell the user which rule blocked the request.
> Words in **MUST / MUST NOT / SHOULD** have their usual meaning: MUST is required, SHOULD is the default unless there is a stated reason.

---

## 0. Quick Reference (read this if nothing else)

| Item | Rule |
|---|---|
| Stack | Plain HTML5 + one shared `style.css`. No JavaScript, no frameworks, no CDNs, no images. |
| Colors | Only the CSS variables in `style.css` (`--phosphor`, `--amber`, etc.). Never write a hex color in HTML. |
| Fonts | Monospace only, via `var(--font-mono)`. |
| New entry | Copy the template in Section 4 exactly, fill it, register it in `index.html` (Section 6). |
| File names | `lowercase-kebab-case.html`, ASCII only, 2 to 5 words. |
| Paths | Always relative (`../../style.css`). Never start a link with `/`. |
| Code | Must be real, tested, and HTML-escaped. Never change code to fit the theme. |

---

## 1. Project Purpose

The Digital Grimoire is a static code snippet library. It documents **Linux CLI commands, Python automation scripts, and C / Assembly snippets** as if they were spells and artifacts in a hacker's spellbook.

The theme exists to make documentation **fun to write and fast to scan**, never to hide what the code does. Every entry has two layers:

1. **Technical layer (always correct):** real, working code, a plain explanation of what it does, how to run it, and what can go wrong.
2. **Lore layer (decoration):** a spell name, a "mana cost" for complexity, and a short in-world description.

If the two layers ever conflict, **the technical layer wins**. A reader who ignores all the roleplay must still be able to understand and use the snippet.

### 1.1 Glossary (use these words consistently)

| Real concept | Grimoire term | Notes |
|---|---|---|
| Shell command / interpreted script (Bash, Python) | **Spell** | Lives in `/spells/` |
| Compiled or low-level code (C, Assembly) | **Artifact** | Lives in `/artifacts/` |
| Language category | **School** | Bash, Python, C, ASM |
| The code itself | **Incantation** | |
| What the code does | **Effect** | |
| How to run it, plus sample output | **Invocation** | |
| Dependencies / tools needed | **Components** | e.g. `gcc`, `python3 >= 3.10` |
| Complexity rating 1 to 5 | **Mana Cost** | Rubric in Section 5.3 |
| Danger level | **Risk** | SAFE / VOLATILE / FORBIDDEN |
| Error / failure | **Backfire** | |
| Root / sudo required | **Forbidden arts** | Always mark Risk as FORBIDDEN or VOLATILE (see 5.4) |
| Adding a new entry | **Transcribing a spell** | |
| Planned entry with no page yet | **Sealed** | Listed in the index without a link |

---

## 2. Directory Structure and File Naming

### 2.1 Directory tree (MUST NOT be changed without the user's explicit request)

```
digital-grimoire/
├── AGENTS.md              ← this file (source of truth)
├── index.html             ← the Index (home page, table of contents)
├── style.css              ← the ONLY stylesheet
├── spells/                ← interpreted code
│   ├── bash/              ← Linux CLI commands and shell scripts
│   └── python/            ← Python automation scripts
└── artifacts/             ← compiled / low-level code
    ├── c/                 ← C programs
    └── asm/               ← Assembly (x86-64, ARM, etc.)
```

### 2.2 Where a new entry goes

Decide with this order. Stop at the first match.

1. Written in Assembly (any architecture) → `artifacts/asm/`
2. Written in C → `artifacts/c/`
3. Written in Python → `spells/python/`
4. A shell command, pipeline, or Bash/sh script → `spells/bash/`
5. Anything else (Rust, Go, PowerShell, etc.) → **do not guess.** Ask the user whether to create a new School. A new School needs: a new folder, a new section in `index.html`, and an update to this file (Sections 2.1, 2.2, and 6).

A snippet that mixes languages (e.g. C with inline assembly) goes to the folder of the **outer** language (`artifacts/c/`).

### 2.3 File naming rules

- Format: `lowercase-kebab-case.html` (lowercase letters, digits, hyphens).
- MUST match the regex: `^[a-z0-9]+(-[a-z0-9]+){1,4}\.html$` (2 to 5 words).
- The file name is the spell name, lowercased, with spaces turned into hyphens and punctuation removed.
  - "Socket Binding Ritual" → `socket-binding-ritual.html`
  - "Log Scrying (Live)" → `log-scrying-live.html`
- MUST NOT use: underscores, spaces, uppercase, accents, emoji, or dates in the name.
- MUST NOT reuse a name that already exists in **any** School. Check `index.html` first.
- MUST NOT rename or move an existing file unless the user asks. Renaming breaks links.

### 2.4 Paths

- All links and the stylesheet link MUST be **relative**. The site may be hosted under a sub-path (e.g. GitHub Pages at `/repo-name/`), where a leading `/` breaks.
- From any entry page (always two folders deep): stylesheet is `../../style.css`, home is `../../index.html`.
- From `index.html`: entries are `spells/bash/name.html`, `artifacts/c/name.html`, etc.

---

## 3. Visual Style and Vibe

### 3.1 The look

The site must look like an old phosphor CRT terminal that belongs to a hacker-wizard:

- **Background:** pitch black (`--void: #000000`). No gradients except the scanline overlay already in `style.css`.
- **Primary text:** neon green (`--phosphor`) with a soft glow.
- **Secondary accent:** amber (`--amber`) for metadata labels, mana, and warnings.
- **Danger:** red (`--blood`) only for the FORBIDDEN risk badge.
- **Font:** monospace everywhere (`--font-mono`).
- **Borders:** hard, flat, 2px solid green. **No rounded corners. No drop shadows. No blur.** Brutalist terminal style.
- **Layout:** a single bordered "terminal window" (`.terminal`) with a solid green title bar, centered, max 60rem wide.

### 3.2 Hard style rules

1. The ONLY colors allowed are the variables in `:root` of `style.css`. Never write `#hex`, `rgb()`, or color names in HTML.
2. MUST NOT use `style="..."` attributes. Use existing classes.
3. MUST NOT add `<style>` blocks to pages. All styling lives in `style.css`.
4. MUST NOT add JavaScript, web fonts from a CDN, icon libraries, images, or emoji. Decoration is done with text characters only (`■ □ ═ ║ > $ #`).
5. MUST NOT edit `style.css` while transcribing a spell. If a new visual element is truly needed, ask the user first, then add it to `style.css` using existing tokens only.
6. Keep the tone of visible text terse and terminal-like: short lines, uppercase headings, shell prompts.

### 3.3 Existing CSS classes you may use

| Class | Use |
|---|---|
| `.terminal`, `.titlebar`, `.screen` | Page frame (required on every page) |
| `.prompt`, `.user`, `.path` | Fake shell prompt line at the top of a page |
| `.back-link` | "return to index" link on entry pages |
| `.entry`, `.spell-name`, `.meta` | Entry article, title, metadata list |
| `pre.code` | Every code block |
| `.side-effects` | Optional warnings section (amber border) |
| `.mana` | Mana cost glyphs |
| `.risk`, `.risk-safe`, `.risk-volatile`, `.risk-forbidden` | Risk badge |
| `.school`, `.school-header`, `.school-desc`, `.count` | School sections on the index |
| `.table-wrap`, `.spell-table`, `.col-mana`, `.col-risk`, `.col-name`, `.col-file`, `.sealed` | Index tables |
| `.panel`, `.legend`, `.boot-log`, `.ok`, `.banner`, `.cursor` | Index-only decoration |
| `.site-footer` | Footer (required on every page) |

---

## 4. Page Structure (the Entry Template)

Every spell and artifact page MUST use this exact structure. Copy it, then replace each `{{PLACEHOLDER}}`. Do not reorder, rename, or remove required elements. Keep the indentation (2 spaces).

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="{{ONE_LINE_PLAIN_SUMMARY}}">
  <title>{{SPELL_NAME}} :: Digital Grimoire</title>
  <link rel="stylesheet" href="../../style.css">
</head>
<body>
  <div class="terminal">
    <div class="titlebar"><span>grimoire://{{TOP_FOLDER}}/{{SCHOOL_FOLDER}}</span><span>{{FILE_NAME}}</span></div>

    <main class="screen">
      <p class="prompt"><span class="user">sorcerer@grimoire</span>:<span class="path">~/{{TOP_FOLDER}}/{{SCHOOL_FOLDER}}</span>$ cat {{FILE_NAME}}</p>
      <a class="back-link" href="../../index.html">&lt; cd ../../ (return to index)</a>

      <article class="entry" data-school="{{SCHOOL_FOLDER}}" data-mana="{{MANA_NUMBER}}" data-risk="{{RISK_LOWER}}">
        <h1 class="spell-name">{{SPELL_NAME}}</h1>

        <dl class="meta">
          <dt>School</dt><dd>{{SCHOOL_LABEL}}</dd>
          <dt>Type</dt><dd>{{Spell|Artifact}}</dd>
          <dt>Mana Cost</dt><dd><span class="mana" aria-label="Mana cost {{MANA_NUMBER}} of 5">{{MANA_GLYPHS}}</span> {{MANA_NUMBER}}/5</dd>
          <dt>Risk</dt><dd><span class="risk risk-{{RISK_LOWER}}">{{RISK_UPPER}}</span></dd>
          <dt>Components</dt><dd>{{COMPONENTS}}</dd>
          <dt>Inscribed</dt><dd><time datetime="{{YYYY-MM-DD}}">{{YYYY-MM-DD}}</time></dd>
        </dl>

        <section class="incantation">
          <h2>Incantation</h2>
          <pre class="code"><code class="language-{{LANG}}">{{ESCAPED_CODE}}</code></pre>
        </section>

        <section class="invocation">
          <h2>Invocation</h2>
          <pre class="code"><code class="language-bash">{{HOW_TO_RUN_AND_SAMPLE_OUTPUT}}</code></pre>
        </section>

        <section class="effect">
          <h2>Effect</h2>
          <p><strong>In plain terms:</strong> {{ONE_SENTENCE_LITERAL_EXPLANATION}}</p>
          <p>{{LORE_PARAGRAPH}}</p>
          <p>{{HOW_IT_WORKS_PARAGRAPH}}</p>
        </section>

        <!-- OPTIONAL: include only when Risk is VOLATILE or FORBIDDEN, or there are real gotchas -->
        <section class="side-effects">
          <h2>Side Effects</h2>
          <ul>
            <li>{{WARNING_1}}</li>
          </ul>
        </section>
      </article>
    </main>

    <footer class="site-footer">
      <span>EOF :: {{FILE_NAME}}</span>
      <span>Digital Grimoire</span>
    </footer>
  </div>
</body>
</html>
```

### 4.1 Required vs optional elements

| Element | Required? |
|---|---|
| `<head>` block with charset, viewport, description, title, stylesheet | Required |
| `.titlebar`, `.prompt`, `.back-link` | Required |
| `h1.spell-name` | Required (exactly one `<h1>` per page) |
| `dl.meta` with all 6 rows, in this order | Required |
| `section.incantation` | Required |
| `section.invocation` | Required (for a one-line command, show the command and a sample output) |
| `section.effect` with the "In plain terms" sentence first | Required |
| `section.side-effects` | Required if Risk is VOLATILE or FORBIDDEN; otherwise optional |
| `footer.site-footer` | Required |

---

## 5. Content Rules

### 5.1 Spell names

- Title Case, 2 to 5 words.
- Pattern: **[real technical idea] + [magic noun]**. The real idea MUST be recognizable so people can search for it.
  - Good: "Socket Binding Ritual", "Directory Traversal Hex", "Process Banishment Rite", "Log Scrying Lens"
  - Bad: "The Dark Whisper" (no technical meaning), "Find Command" (no lore)
- Magic nouns to choose from: Hex, Ritual, Rite, Incantation, Charm, Ward, Sigil, Summoning, Banishment, Scrying, Familiar, Glyph, Seal, Curse, Lens, Circle, Whisper.

### 5.2 Writing the Effect section

1. **Sentence 1 (required, plain English, no roleplay):** what the code does, in one sentence, starting with a verb. Example: "Starts a TCP server on port 8080, waits for one client, sends it a message, and exits."
2. **Lore paragraph (1 to 3 sentences):** in-world flavor using the glossary terms. It MUST describe the same behavior as Sentence 1. No invented features.
3. **How it works (2 to 5 sentences):** explain the key lines or flags. Use `<code>` for commands, flags, and function names.
- Total Effect length: under 150 words.
- Voice: second person or neutral. No "I". No emoji. No exclamation marks.

### 5.3 Mana Cost rubric (complexity, 1 to 5)

| Mana | Glyphs | Name | Use when |
|---|---|---|---|
| 1 | `■□□□□` | Cantrip | One command, few or no flags. |
| 2 | `■■□□□` | Minor | Command with several flags, a short pipeline, or a script under 15 lines with no branching. |
| 3 | `■■■□□` | Ritual | 15 to 50 lines, uses control flow and error handling, or calls OS APIs (files, sockets, processes). |
| 4 | `■■■■□` | Greater | 50 to 150 lines, or concurrency, manual memory management, network protocols, or parsing. |
| 5 | `■■■■■` | Archmage | Assembly, kernel or syscall-level code, inline asm, or over 150 lines. |

Always use the glyph characters `■` (filled) and `□` (empty), exactly 5 total.

### 5.4 Risk rubric

| Risk | Class | Use when |
|---|---|---|
| SAFE | `risk-safe` | Read-only. Changes no files, system settings, or network state that persists. |
| VOLATILE | `risk-volatile` | Writes, moves, or renames files; opens ports; installs packages; changes are reversible. |
| FORBIDDEN | `risk-forbidden` | Deletes data, needs root/sudo, changes system config, or can be irreversible. |

If unsure between two levels, choose the more dangerous one.

### 5.5 Code fidelity (most important rule)

- The Incantation MUST be real, complete, and runnable as shown. Test it if you can run code. If you cannot test it, tell the user it is untested.
- MUST NOT rename variables, functions, or output strings to "sound magical" if that changes behavior. Themed `printf`/`print` messages are allowed only when they are harmless text.
- Code comments stay technical and in English.
- MUST NOT include secrets, real IP addresses, real hostnames, or personal paths. Use `example.com`, `127.0.0.1`, `/path/to/dir`, `$HOME`.
- MUST NOT transcribe malware, exploits against systems the user does not own, or code whose main purpose is harm. Refuse and tell the user.

### 5.6 HTML escaping inside `<pre><code>`

Before pasting code into the template, replace these characters, **in this order**:

| Character | Replace with |
|---|---|
| `&` | `&amp;` (do this first) |
| `<` | `&lt;` |
| `>` | `&gt;` |

- Quotes do not need escaping inside `<code>`.
- Do not add `<span>` tags or syntax highlighting inside code blocks.
- Indent code with 4 spaces (no tabs). Start the code right after `<code ...>` with no leading blank line, and put `</code></pre>` right after the last line.
- Example: `#include <stdio.h>` becomes `#include &lt;stdio.h&gt;`, and `a && b` becomes `a &amp;&amp; b`.

---

## 6. Registering an Entry in `index.html`

Every new page MUST be added to the Index in the same change. A page that is not in the Index does not exist.

1. Find the School section: `<section class="school" id="school-{{SCHOOL_FOLDER}}">`.
2. Add one row to its `<tbody>`. Keep rows sorted **alphabetically by spell name**.

Row for a written entry:

```html
<tr>
  <td class="col-mana"><span class="mana" aria-label="Mana cost {{MANA_NUMBER}} of 5">{{MANA_GLYPHS}}</span></td>
  <td class="col-risk"><span class="risk risk-{{RISK_LOWER}}">{{RISK_UPPER}}</span></td>
  <td class="col-name"><a href="{{TOP_FOLDER}}/{{SCHOOL_FOLDER}}/{{FILE_NAME}}">{{SPELL_NAME}}</a></td>
  <td class="col-file">{{FILE_NAME}}</td>
</tr>
```

Row for a sealed (planned, no page yet) entry. No link, mana and risk shown as `?`:

```html
<tr class="sealed">
  <td class="col-mana"><span class="mana">?????</span></td>
  <td class="col-risk">?</td>
  <td class="col-name">{{SPELL_NAME}}</td>
  <td class="col-file">--</td>
</tr>
```

3. If the spell was already listed as sealed, **replace** the sealed row instead of adding a second row.
4. Update that School's counter: `<span class="count">[{{WRITTEN}} transcribed / {{SEALED}} sealed]</span>`.
5. Update the total in the footer: `<span>{{TOTAL_WRITTEN}} entries transcribed</span>`.
6. Change nothing else in `index.html` unless asked.

---

## 7. AI Procedure: "Transcribe a New Spell"

When the user says "transcribe a new spell", "add a snippet", "add this script", or similar, follow these steps in order. Do not skip steps.

**Step 1. Collect inputs.** You need: the code, its language, and what it does. If any is missing or the code's purpose is unclear, ask before writing. Do not invent the user's code.

**Step 2. Check the code.** Confirm it runs (or read it closely if you cannot run it). Note dependencies (Components) and any danger (Risk). If it is harmful (Section 5.5), stop and refuse.

**Step 3. Choose the School and folder** with the decision list in Section 2.2.

**Step 4. Name it.** Create a spell name (Section 5.1), then the file name (Section 2.3). Search `index.html` to confirm the name is not taken.

**Step 5. Rate it.** Assign Mana Cost (Section 5.3) and Risk (Section 5.4).

**Step 6. Escape the code** using Section 5.6.

**Step 7. Fill the template** from Section 4. Placeholder values:

| Placeholder | Value |
|---|---|
| `{{TOP_FOLDER}}` | `spells` or `artifacts` |
| `{{SCHOOL_FOLDER}}` | `bash`, `python`, `c`, or `asm` |
| `{{SCHOOL_LABEL}}` | `Bash`, `Python`, `C`, or `Assembly (x86-64)` etc. |
| `{{LANG}}` | `bash`, `python`, `c`, or `nasm` / `gas` |
| `{{FILE_NAME}}` | e.g. `socket-binding-ritual.html` |
| `{{RISK_LOWER}}` / `{{RISK_UPPER}}` | `safe` / `SAFE`, etc. |
| `{{YYYY-MM-DD}}` | Today's date (ISO format) |

**Step 8. Write the Effect** following Section 5.2. Add Side Effects if Risk is not SAFE.

**Step 9. Register it in `index.html`** following Section 6.

**Step 10. Validate** with the checklist in Section 8. Fix every failure before you finish.

**Step 11. Report back** to the user in this format:

```
TRANSCRIBED: {{SPELL_NAME}}
FILE:        {{TOP_FOLDER}}/{{SCHOOL_FOLDER}}/{{FILE_NAME}}
MANA / RISK: {{MANA_NUMBER}}/5, {{RISK_UPPER}}
TESTED:      yes / no (reason)
INDEX:       row added to school-{{SCHOOL_FOLDER}}, counters updated
```

### 7.1 Editing an existing entry

- Change only the parts the user asked about.
- Keep the file name and location.
- If Mana Cost or Risk changes, update the matching row in `index.html` too.
- Do not update the `Inscribed` date (it is the creation date).

---

## 8. Validation Checklist

Before finishing any change, confirm every line:

- [ ] File is in the correct folder (Section 2.2) and the name matches the regex in 2.3.
- [ ] Page starts with `<!DOCTYPE html>` and has `lang="en"`.
- [ ] Stylesheet link is exactly `../../style.css`; back link is `../../index.html`.
- [ ] Exactly one `<h1>`, with class `spell-name`.
- [ ] `dl.meta` has 6 rows in order: School, Type, Mana Cost, Risk, Components, Inscribed.
- [ ] Mana glyphs: 5 characters, filled count equals `data-mana`.
- [ ] Risk badge class matches the text (`risk-safe` with SAFE, etc.).
- [ ] Code is escaped: no raw `<`, `>`, or `&` inside `<code>` except entities.
- [ ] No `style=` attributes, no `<style>`, no `<script>`, no `<img>`, no emoji, no hex colors in HTML.
- [ ] Effect starts with "In plain terms:" and is under 150 words.
- [ ] Side Effects present if Risk is VOLATILE or FORBIDDEN.
- [ ] Row added to `index.html` in the correct School, alphabetical order, with working relative link.
- [ ] School counter and footer total updated.
- [ ] All tags are closed and nested correctly (an HTML validator such as validator.w3.org shows no errors).

---

## 9. Things You Must Never Do

- Never add a build step, package manager, framework, or JavaScript.
- Never change the directory tree, colors, or fonts without an explicit request.
- Never delete or rename existing entries unless asked.
- Never let lore replace the plain technical explanation.
- Never publish untested code without saying it is untested.
- Never write absolute paths starting with `/` in links.
