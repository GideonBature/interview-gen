# interview-gen

An AI-powered interview question generator that produces 3 thoughtful, role-specific interview questions from any job title. Built as a single HTML file with no dependencies, no build step, and no backend.

**Live demo:** [gideonbature.github.io/interview-gen](https://gideonbature.github.io/interview-gen)

---

## What it does

Enter any job title — for example, _Customer Success Manager_ — and the app calls the Google Gemini API to generate 3 interview questions tailored specifically to that role:

- **Behavioral** — draws out past experience relevant to the role
- **Situational** — presents a realistic scenario the candidate would face on the job
- **Competency-based** — tests a core skill the role demands

The questions are role-specific by design. The prompt explicitly rules out generic filler questions like "tell me about yourself" or "where do you see yourself in 5 years."

---

## Tech decisions

### Why a single HTML file?

The assignment called for a simple, hosted, interactive page. A single `index.html` with no framework, no bundler, and no build step is the right tool for that scope. It is easier to read, easier to deploy, and easier to explain. Complexity should be earned, not assumed.

### Why Google Gemini?

Most AI APIs — including OpenAI, Anthropic, and DeepSeek — block direct browser requests due to CORS policy. Gemini's REST API accepts the key as a URL query parameter, which sidesteps the CORS preflight check and makes it the correct choice for a purely client-side static app. Using a backend proxy would have added infrastructure complexity with no meaningful benefit for this use case.

### Why Gemini 2.5 Flash specifically?

It is fast, capable, and available on the free tier with no credit card required. For generating structured interview questions, it produces output that is indistinguishable from larger, more expensive models.

### Why does the user enter their own API key?

Hardcoding an API key in a public repository is a security risk. Embedding it in client-side JavaScript exposes it to anyone who views the page source. Requiring the user to supply their own key at runtime means no credentials ever touch the codebase. In a production scenario, the key would live in a backend environment variable — but for a static hosted demo, this is the right tradeoff.

### Why structured JSON output?

The prompt instructs the model to return a strict JSON array with typed question objects. This makes the response easy to parse and render consistently, and gives each question a visible label in the UI (Behavioral / Situational / Competency). It also makes the output predictable — easier to test, easier to extend.

---

## How to run it locally

No installation required.

1. Clone the repository:
   ```bash
   git clone https://github.com/gideonbature/interview-gen.git
   cd interview-gen
   ```

2. Open `index.html` directly in your browser:
   ```bash
   # macOS
   open index.html

   # Linux
   xdg-open index.html

   # Windows
   start index.html
   ```

3. Get a free Gemini API key from [aistudio.google.com](https://aistudio.google.com/app/apikey) — no credit card required.

4. Paste the key into the API key field, enter a job title, and click **Generate Questions**.

---

## How to deploy to GitHub Pages

1. Push the repository to GitHub.
2. Go to your repository **Settings → Pages**.
3. Under **Source**, select the `main` branch and root `/` folder.
4. Click **Save**. GitHub will provide a live URL within a minute.

---

## Project structure

```
interview-gen/
└── index.html      # Entire application — HTML, CSS, and JavaScript in one file
└── README.md       # This file
```

---

## Prompt design

The system prompt is the core of what makes this app useful rather than generic. The key decisions:

- **Persona assignment:** The model is told it is an expert hiring manager with 15+ years of experience. This anchors the tone and depth of the output.
- **Strict question types:** Each question type (behavioral, situational, competency) is defined with a starting phrase pattern to ensure consistency and variety.
- **Explicit exclusions:** The prompt bans generic questions by name. Without this, models default to surface-level output.
- **Structured output format:** The model is instructed to return only a JSON array with no markdown fences or preamble. This makes parsing reliable.

```
You are an expert hiring manager and talent acquisition specialist
with 15+ years of experience conducting interviews across industries.

When given a job title, generate exactly 3 thoughtful, role-specific
interview questions...

Return ONLY a valid JSON array in this exact format:
[
  { "type": "Behavioral", "question": "..." },
  { "type": "Situational", "question": "..." },
  { "type": "Competency", "question": "..." }
]
```

---

## What I would improve with more time

- **Question categories toggle** — let users choose between behavioral, technical, cultural fit, or leadership questions depending on the interview stage
- **Seniority level selector** — junior, mid, senior, and executive roles warrant different question depth; this could be an additional input that feeds into the prompt
- **Copy to clipboard** — a small button on each card to copy individual questions
- **Export as PDF** — useful for interviewers who want a printed question sheet before a call
- **History** — store previous searches in localStorage so users can revisit past question sets

---

## AI usage

This project was built with AI assistance. Claude (Anthropic) was used for code generation, debugging, and prompt refinement. Per the assessment brief.

---

## Author

**Gideon Bature**  
[github.com/GideonBature](https://github.com/GideonBature)