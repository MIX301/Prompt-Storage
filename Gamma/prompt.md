Presentation Structure for A-EYE Pitch (10 min)

1. Group Introduction (30 sec)

State your group name.

Quick tagline: “We are building A-EYE, an AI-powered assistant for people who need help identifying items in their environment.”



2. Problem Statement & Motivation (2 min)

Hook / story: Personal anecdote (Germany / olive oil shelf without contact lenses).

Broaden to general problem:

Challenges for visually impaired people in navigating stores and environments.

Challenges for foreigners who cannot read local language labels.

Why it matters: accessibility, independence, inclusion.

Possible stat or emphasis: visually impaired and language barriers affect millions globally.



3. Prototype Concept & Scope (2–3 min)

Introduce A-EYE app:

Simple interface, powered by AI.

User workflow:

Ask a question by voice/text (“Where is the gluten-free bread?”).

Take a photo of the shelf.

AI responds with precise, location-based instructions (“third shelf from the top, top right corner”).

Examples: gluten-free bread, olive oil, cooking ingredients.

Scope for prototype:

Focus on grocery store use case first.

Support both voice input (Whisper) and text input.

Clear, actionable spatial descriptions as output.



4. Planned Interactive/Functional Elements (2 min)

Beyond static mockups:

Working prototype with photo capture.

Live voice-to-text transcription (Whisper).

Backend AI reasoning with OpenAI Vision API.

Interactive feedback loop: user can refine question (e.g., “I meant sunflower oil”).

UI/UX highlights:

Minimal interface for accessibility.

Voice-first approach for ease of use.



5. Planned Role of AI in the Process/System (2 min)

Computer vision: OpenAI Vision API to analyze images.

Language understanding: interpret natural language queries (text or voice).

Speech-to-text: Whisper for accessible interaction.

Output reasoning: AI generates human-friendly spatial descriptions.

Future potential:

Object detection fine-tuned for accessibility.

Multi-language support.

Real-time AR overlays (later scope).



6. Tech Stack & Development Plan (1 min)

Frontend: React Native + Expo.

Backend: FastAPI (Python).

AI services: OpenAI Vision API + Whisper.

Tools: Cursor for dev, Claude Code for big context tasks.

Repo structure: dual (frontend + backend).



7. Closing (30 sec)

Reinforce the value proposition:

Independence for visually impaired and travelers.

Practical, simple, accessible AI application.

Tagline: “With A-EYE, finding what you need is just one question away.