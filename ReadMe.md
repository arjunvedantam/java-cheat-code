🧩 Naming Conventions (Stick to These)

File names: kebab-case

Headings: Sentence case

Code blocks: Language tagged

Rules: Bold “Rule:” lines

No emojis (keeps it professional & printable)


🔁 How ChatGPT Fits into This Workflow

Your future workflow becomes:

You say:

“Add a section on Stream short-circuiting to Java Cheat Code”

I respond with:

A ready-to-commit Markdown section

Correct headings, rules, examples

You:

Paste into the correct docs/*.md

Commit

I become your editor + reviewer, Git becomes your memory.





👉 **Do NOT hard-code page numbers per section**
We’ll handle page numbers at print time.

---

## 🖨️ Printing & Page Numbers (Correct Way)

### Never manually manage page numbers in Markdown
Instead:
- Use **headings + horizontal rules**
- Let the PDF engine handle pagination

### Best tools (pick one)

#### Option A: Pandoc (best)
```bash
pandoc print/java-cheat-code.md \
  -o java-cheat-code.pdf \
  --pdf-engine=xelatex


----------------------------------------------------------------------------------
Structure
----------------------------------------------------------------------------------

Java-cheat-code/
│
├── README.md
│
├── docs/
│   ├── 00-preface.md
│   ├── 01-method-references.md
│   ├── 02-lambdas.md
│   ├── 03-streams.md
│   ├── 04-optional.md
│   └── ...
│
├── print/
│   └── java-cheat-code.md
│
└── assets/
    └── diagrams/