# Product Requirements Document (PRD)

## AI Translation Practice Web App (Language-Agnostic MVP)

---

## 1. Overview

This product is a responsive web application that helps users practice translating from their native language (initially English) into a target language (initially Japanese) using large language models (LLMs).

The core learning loop is:

1. The system generates a source sentence.
2. The user translates it into the target language.
3. The LLM evaluates the translation.
4. The LLM provides concise feedback and an improved model translation.
5. After several sentences, the system reviews the same prompts to test retention.
6. The session ends and the user may restart with the same topic or select a new one.

Although Japanese is the MVP target language, the architecture must be language-agnostic so that additional target languages can be added without major redesign.

---

## 2. Goals

### Primary Goals

- Provide realistic translation practice with unlimited valid answers.
- Deliver immediate, high-quality feedback.
- Generate diverse sentences within a selected topic.
- Support learners across skill levels.
- Operate efficiently using minimal LLM context.
- Remain language-agnostic.
- Work well on both desktop and mobile browsers.

### MVP Goals

- Topic-based sentence generation.
- Configurable sentence count (default: 3).
- Difficulty selection.
- Translation evaluation with feedback.
- Clarification questions.
- Review cycle.
- Script display options.
- Romaji input support.
- Provider-agnostic LLM integration.

---

## 3. Target Users

Language learners who want:

- Open-ended translation practice.
- Natural language feedback.
- Topic-based exercises.
- Quick interaction cycles.
- Flexible input methods.

Supported proficiency levels in MVP:

- Elementary
- Middle School
- High School

These labels are passed directly to the LLM as difficulty guidance.

---

## 4. Core User Workflow

1. User selects:
   - Source language (default: English)
   - Target language (default: Japanese)
   - Topic
   - Difficulty
   - Number of sentences (default: 3)
   - Script display preference:
     - Native script (kana/kanji)
     - Romanized only

2. Backend creates a session plan:
   - Randomly selects one subtopic per sentence.
   - Randomly selects per sentence:
     - SpeechAct
     - Complexity
     - Perspective
     - TimeReference

3. Entire session plan is sent to the LLM at session start.

4. LLM generates all source sentences in structured JSON.

5. The app presents one sentence at a time.

6. For each sentence:
   - User asks clarifying questions and/or submits a translation.
   - LLM either answers the clarification or evaluates the translation.
   - LLM returns concise feedback and a suggested translation.

7. After all initial sentences:
   - Review begins.
   - Original source sentences are presented again verbatim.
   - User retranslates each sentence.
   - LLM provides normal evaluation.

8. Session ends.
9. User chooses:
   - Practice same topic again.
   - Select a new topic.

10. Session state is discarded.

---

## 5. Topics and Subtopics

Topics are predefined and each contains a fixed set of subtopics.

Examples include:

- travel
- work
- education
- food
- health
- commerce
- social
- media
- technology
- home
- administration
- emergency
- leisure

For each generated sentence, one subtopic is randomly selected and hidden from the user.

---

## 6. Randomization Fields

Each sentence is generated using:

- `topic`
- `subtopic`
- `difficulty`
- `speechAct`
- `complexity`
- `perspective`
- `timeReference`

The specific possible values for these fields will be included in src/lib/json/sentence-guidance.json

---

## 7. Functional Requirements

### Sentence Generation

- Generate all source sentences at session start.
- Use the full session plan as structured input.
- Ensure each sentence aligns with its assigned parameters.
- Output only the source sentence text plus optional metadata.

### Clarification Handling

- Users may ask unlimited questions before submitting a translation.
- Responses should help understanding without directly giving away the answer unless explicitly requested.

### Translation Evaluation

The LLM must:

- Accept multiple valid translations.
- Recognize alternative wording.
- Evaluate grammar, vocabulary, naturalness, and contextual politeness.
- Be tolerant of romaji spelling differences.
- Recognize both script and romanized input.
- Suggest idiomatic alternatives when appropriate.
- Immediately provide an improved or recommended translation.
- If the user's answer is already excellent, present alternatives as optional rather than superior.

### Review Phase

- Reuse the exact original source sentences.
- Present them in original order.
- Mark the first review sentence with a clear review indicator.
- Provide normal evaluation.
- Optionally compare performance across attempts.

### Session Reset

- Clear all conversation context after session completion.
- Prompt the user to restart with the same topic or select a new one.

---

## 8. Script and Input Modes

### Accepted Input

- Native script
- Romaji

### Output Modes

- Romanized only (default for Japanese)
- Native script

When romanized-only mode is selected:

- All target-language output must be transliterated using standard conventions.
- Minor user spelling variations must be accepted without penalty.

---

## 9. Feedback Requirements

Feedback should be concise and actionable.

Typical structure:

1. Brief evaluation.
2. Key corrections.
3. Naturalness notes.
4. Suggested translation.
5. Optional alternative expressions.

Politeness commentary should only be emphasized when context makes register especially relevant.

---

## 10. Non-Functional Requirements

### Performance

- Initial session generation: under 5 seconds target.
- Translation feedback: under 3 seconds target.

### Responsiveness

- Mobile-first responsive design.
- Fully usable in phone browsers.

### Reliability

- Structured JSON responses with schema validation.
- Retry logic for malformed outputs.

### Provider Independence

- Abstract LLM calls behind a provider interface.

---

## 11. Technical Architecture

### Frontend

- [Next.js](https://nextjs.org?utm_source=chatgpt.com) (App Router)
- [React](https://react.dev?utm_source=chatgpt.com)
- [TypeScript](https://www.typescriptlang.org?utm_source=chatgpt.com)
- [Tailwind CSS](https://tailwindcss.com?utm_source=chatgpt.com)

### Backend

- Next.js API routes or server actions.

### LLM Layer

- Provider abstraction interface.
- Initial provider: [Google Gemini API](https://ai.google.dev?utm_source=chatgpt.com)
- Future support for [OpenAI](https://openai.com?utm_source=chatgpt.com) and [Anthropic](https://www.anthropic.com?utm_source=chatgpt.com)

### Validation

- [Zod](https://zod.dev?utm_source=chatgpt.com) for schema enforcement.

### State Management

- Client-side session state only.
- No authentication.
- No persistent user storage.

---

## 12. Data Model

### Session Configuration

```ts
interface SessionConfig {
  sourceLanguage: string;
  targetLanguage: string;
  topic: string;
  difficulty: "elementary" | "middle_school" | "high_school";
  sentenceCount: number;
  scriptMode: "native" | "romanized";
}
```
