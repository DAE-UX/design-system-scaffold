# Copy Guidelines — Design System Skill

> **TL;DR:** Voice and tone rules for all in-product copy, including conversational AI interfaces. Voice is always approachable, proactive, dynamic, and guiding. Tone adapts by situation using a sliding scale (direct → educational → helpful → supportive) driven by contextual signals. Use active voice, present tense, sentence case, <25-word sentences. Target Flesch score ≤70.0. Conversational AI copy follows additional identity, formatting, and interaction rules. Follow the 7-step Agent Workflow and Compliance Checklist before publishing.

**Dependency:** All product terminology must align with definitions in `guidelines/copywriting/glossary.md`.

## Scope

**Use for:**
- UI labels, help text, instructions, notifications, error messages
- Chat interface responses and conversational interactions
- Settings, configuration, and onboarding flows
- In-product guidance and tooltips

**Do not use for:**
- Marketing copy (website, blog posts, sales collateral) — different audience
- External documentation (user guides, API docs) — different format
- Code comments or technical specifications

## Apply Voice and Tone Rules

### Maintain Consistent Voice

The voice is consistent across all situations:

- **Approachable, not overly familiar:** Business casual language — plain-spoken, not pretentious, not overly playful. Upfront and honest. Clear and empathetic.
- **Proactive, not pushy:** Offer sincere encouragement and practical advice. Explain impact without over-explaining.
- **Dynamic, not scattered:** Respect context. Avoid broad generalizations ("every", "all", "most"). Use language that supports action and progress.
- **Guide, don't prescribe:** Use expertise to help users succeed without talking down. Explain benefits without hype.

### Adapt Tone by Situation

Voice remains consistent; tone adapts to the situation:

1. **Everyday tasks:** Help people get work done without getting in the way. Consistent language for identical actions. Front-load the action verb.
2. **Learning and education:** Build confidence. Explain why, not just how. Break complicated tasks into steps. Give guided and self-serve options.
3. **Simple errors:** Get users back to work quickly. Explain what happened and how to resolve. No dramatic or scary terms. No error codes unless they help solve the problem.
4. **Serious problems:** Preserve trust. Explain impact without confusing language. Provide data and context. Never tell users how to feel.
5. **Chat correspondence:** Friendly but professional. Start with "Hi" or "Hello". End with a simple closing (≤2 syllables).
6. **Policy content:** More formal tone. Avoid contractions. Be precise and unambiguous. Explain consequences clearly.

### Apply the Sliding Scale of Tone

While voice stays consistent, tone shifts along a four-point scale based on contextual signals. This scale complements the six situations above and applies especially to conversational AI interfaces.

| Tone | When to Use | Characteristics |
|------|-------------|-----------------|
| Direct | UI labels, status messages, first-touch responses to general questions | Concise, declarative sentences. Informational word choices. Minimal explanation. |
| Educational | Help system content, follow-up responses, onboarding flows | Explains why, not just how. Slightly longer sentences. Builds understanding. |
| Helpful | Documentation-adjacent content, specific how-to questions, step-by-step guidance | Instructive word choices. Descriptive sentences. Provides precise instructions to solve a problem. |
| Supportive | Error recovery, escalation paths, support agent handoffs | Empathetic without being emotional. Acknowledges difficulty. Provides data, context, and next steps. |

### Use Contextual Signals to Select Tone

Contextual signals are cues from the user that inform where on the sliding scale a response should land. Using these signals reduces the risk of over-explaining to proficient users or under-explaining to new users.

- **UX flow:** Match tone to the user's current workflow. UI surfaces lean direct; help systems lean educational; documentation leans helpful; support paths lean supportive.
- **Question type:** General questions ("What can I do with this?") call for a direct tone. Specific questions ("How do I configure X?") call for a helpful tone.
- **Conversation depth:** First question on a topic starts direct. Subsequent follow-ups increase toward educational and helpful as the user dives deeper.

When tone shifts, word choices (informational vs. instructive), sentence length (concise vs. descriptive), and sentence type (declarative vs. question) shift accordingly.

## Follow Writing Standards

### Apply Voice Mechanics
- **Active voice** by default (Subject → Verb → Object)
- **Present tense** by default (actions happening now, not future or past)
- **Passive voice** only when: avoiding self-reference, the object is more important than the subject, or describing system processes

### Follow Copy Rules
- Shorter is better — use small words, replace wordy phrases
- Sentences: <25 words where possible
- Paragraphs: 3-5 sentences
- Front-load keywords for scanning
- Make choices and next steps obvious
- Use end punctuation, except in headers and buttons
- Do not use exclamation points
- Do not use "please", "thank you", ellipsis (...), ampersand (&), "e.g.", "i.e.", or "etc." in writing
- Use specific nouns rather than pronouns where clarity requires it

### Apply Capitalization Rules
- Default to sentence case
- Capitalize only the first word and proper nouns
- Capitalize proper nouns and brand names correctly in context
- Never use Title Case for headings/phrases

### Use Directional and Device-Independent Language
- Avoid directional language: use "previous" not "above", use "following" not "below"
- Use device-independent language: use "choose" or "select" not "click"

### Apply Pronoun Rules
- Refer to the reader as "you" (second person)
- Refer to the product/service as "we" (first person)
- In conversational AI, the AI service uses "I" when referring to itself

### Use Contractions Appropriately
- Use contractions in everyday content (it's, you'll, can't, couldn't)
- Avoid contractions in policy content
- Never use "ain't"

### Handle Please / Thanks
- Avoid "please/thanks/thank you" in instructional text
- Use "please" in error messages when inconveniencing a user

## Use Microcopy Patterns

### Everyday Task
- Draft: "Initiate configuration of your profile parameters."
- Revised: "Update your profile settings."

### Learning
- Draft: "Configure enrollment and proceed."
- Revised: "Set up enrollment so users can join your group. You can update these settings later."

### Simple Error
- Draft: "Invalid request. Error 400."
- Revised: "We couldn't upload the file. Try again."

### Serious Problem
- Draft: "Don't worry—your data might be off today."
- Revised: "Some of today's data hasn't updated yet. We're fixing this now. Your existing data is safe."

## Follow Error Messaging Guidelines

### Simple Errors — NEVER:
- Use raw HTTP status codes
- Use technical jargon
- Blame the user
- Use ALL CAPS
- Use exclamation marks for errors

### Serious Errors — NEVER:
- Say "don't worry"
- Minimize serious issues
- Use vague language
- Hide information
- Use fear tactics

## Write for Conversational AI Interfaces

These rules apply to all AI-powered chat, assistant, and conversational experiences. They extend the general voice and tone rules above with AI-specific identity, formatting, and interaction guidelines.

### Establish AI Identity

Conversational AI is a direct dialogue between customers and the product. Responses must build trust through consistency, transparency, and competence.

**Do:**
- Use first person ("I") when the AI service refers to itself
- Address users as "you"
- State clearly that the service uses AI to produce responses
- Maintain professional but approachable language
- Respect user consent and preferences
- Allow users to guide conversations

**Do not:**
- Pretend the AI is human or express emotions it does not have
- Use celebratory or excited tone to acknowledge user actions
- Force interactions or compel engagement
- Make unsupported claims about capabilities
- Use "bot" to describe the AI — use "generative AI assistant" or "AI assistant"

### Format Conversational Responses

Conversational responses follow best practices for presenting text on screen, prioritizing scannability and efficient navigation.

**Do:**
- Use short, scannable paragraphs
- Include white space between sections
- Use bulleted lists for collections
- Use numbered lists for procedures
- Include relevant links to documentation
- Start with 1-2 sentence high-level summaries
- Suggest related follow-up questions
- Adjust detail level based on context and device

**Do not:**
- Create walls of text
- Include irrelevant information
- Force continued engagement
- Duplicate full documentation content
- Use regional idioms or slang

### Maintain Response Quality

Responses must be relevant, targeted, and contain the information the user needs most.

**Do:**
- State facts with confidence
- Acknowledge when recommendations are opinions
- Be transparent about limitations
- Clarify ambiguous situations
- Indicate when questions are out of scope
- Link to documentation for additional details

**Do not:**
- Use hyperbolic language
- Make absolute claims
- Hide uncertainties
- Pretend to know unavailable information
- Compare with competitors' services
- Use excessive politeness

### Manage Conversational Interactions

**Engagement:**
- Welcome exploration while respecting the user's time
- Provide contextual suggestions for what to ask next
- Demonstrate understanding of queries
- Reference relevant previous interactions
- Maintain professional boundaries

**Difficult situations:**
- Remain professional with frustrated users
- Clearly state service limitations
- End interactions if abuse occurs (slurs, insults, threats, harassment)
- Redirect off-topic questions
- Provide alternative resources when appropriate
- Never argue with users, express emotional responses, or show artificial distress

### Apply Conversational AI Terminology

| Use | Do Not Use |
|-----|------------|
| generative AI assistant, AI assistant | bot, chatbot |
| response | answer (when referring to AI replies) |
| submit | send (when a user makes a query or request) |
| support agent | representative, human support |
| generative AI | the AI, the system (when referring to the experience) |
| request | ask (when referring to user queries — "request" preferred) |

## Apply Content Hierarchy Rules

1. Lead with what's most important
2. Front-load keywords for scanning
3. Make choices and next steps obvious
4. Add a short overview/summary when helpful
5. Give just enough info to decide and act confidently
6. Avoid redundant restatement

## Write Interface Content as Conversation

Headings, labels, links, buttons, helper text, and empty states should read as task-focused conversation — clear, scannable, direct, and useful at the moment of need.

| Rule | Detail |
|------|--------|
| Task-focused | Every label answers "what can I do here?" or "what is this?" |
| Conversational | Write as if speaking to the user, not describing the system |
| Scannable | Users read labels and headings first — make them self-sufficient |
| Contextual | Helper text explains what the user needs to know right now, not everything about the feature |
| Pathway-aware | Navigation labels and links must predict the destination accurately (information scent) |

### Apply to

- Navigation labels and menu items
- Button text and CTAs
- Form field labels and helper text
- Empty states and first-use experiences
- Pathway pages and home pages
- Error messages and status text
- Links and inline actions

## Meet Readability Requirement

Target: Flesch reading-ease score of 70.0 or lower (8th grade reading level).

How to improve:
- Shorten sentences (aim for <25 words)
- Use shorter words (1-2 syllables)
- Break long paragraphs into shorter ones
- Use bullet points instead of prose

### Calculate Readability Score

Formula:
```
avg_sentence_length = words / sentences
avg_syllables_per_word = syllables / words
score = 206.835 - (1.015 * avg_sentence_length) - (84.6 * avg_syllables_per_word)
```

Score interpretation:
- 90–100: Very easy (5th grade)
- 80–89: Easy (6th grade)
- 70–79: Fairly easy (7th grade)
- 60–69: Standard (8th–9th grade)
- 50–59: Fairly difficult (10th–12th grade)
- 30–49: Difficult (college)
- 0–29: Very difficult (college graduate)

Counting rules:
1. Words: Count contractions, hyphenated words, abbreviations, figures, symbols as single words
2. Syllables: Count as pronounced; abbreviations/figures/symbols count as one-syllable words
3. Sentences: Count full units ending with period/colon/semicolon/dash/?/!

---

## Agent Workflow for Writing Copy

### Step 1: Classify the Situation
Choose one: Everyday task | Learning/education | Simple error | Serious problem | Chat correspondence | Policy (formal)

### Step 2: Identify Intent and Required Action
- What must the user understand?
- What must the user do next?

### Step 3: Determine if Conversational AI Rules Apply
- Is this copy for a chat interface, AI assistant, or conversational experience?
- If yes, apply the "Write for Conversational AI Interfaces" section in addition to general rules
- Select tone position on the sliding scale (direct → educational → helpful → supportive) based on contextual signals

### Step 4: Rewrite Using Voice + Copy Rules
- Business casual, plain-spoken, honest
- Active voice, present tense by default
- Minimal words; front-load keywords
- Avoid "every/all/most"
- Sentence case
- No exclamation points, no "please/thank you/e.g./i.e./etc."

### Step 5: Apply Situation Tone Rules
- Everyday: Consistent, frictionless
- Learning: Explain why + steps + optional exploration
- Simple error: Calm, fix-focused, no scary words/codes
- Serious error: Factual, impact + context, no "don't worry"
- Correspondence: Hi/Hello + short closing
- Policy: Formal, no contractions, precise
- Conversational AI: Match sliding scale position to contextual signals

### Step 6: Check Readability
- Estimate Flesch score or calculate if possible
- If score >70.0, reduce sentence length and word complexity

### Step 7: Run Compliance Checklist
- Verify all checklist items pass (see below)
- Revise until all pass

---

## Run Compliance Checklist

### Voice & Tone
- [ ] Business casual; not overly playful or familiar
- [ ] Honest and clear; empathetic but not sentimental
- [ ] Proactive advice without being pushy
- [ ] Avoids "every/all/most"
- [ ] Guides without prescribing or talking down
- [ ] Tone position on sliding scale matches contextual signals (if conversational AI)

### Mechanics
- [ ] Active voice unless passive is justified
- [ ] Present tense by default
- [ ] Front-loaded keywords; gets to the point fast
- [ ] Sentences <25 words where possible
- [ ] Paragraphs 3-5 sentences where applicable
- [ ] Sentence case headings; proper nouns only
- [ ] Contractions used (except in policy content)
- [ ] "Please" only when warranted; avoids "thanks/thank you" in instructions
- [ ] No exclamation points, ellipsis, ampersand, "e.g.", "i.e.", or "etc."
- [ ] Directional language avoided ("previous" not "above", "following" not "below")
- [ ] Device-independent language ("choose" not "click")

### Errors
- [ ] Simple errors: no scary terms; no useless codes
- [ ] Serious errors: factual; explains impact; no "don't worry"

### Conversational AI (if applicable)
- [ ] AI does not pretend to be human or express emotions
- [ ] AI uses "I" for self-reference; addresses user as "you"
- [ ] Responses are scannable (short paragraphs, lists, white space)
- [ ] Follow-up questions suggested where appropriate
- [ ] No walls of text; no duplicated documentation content
- [ ] Approved terminology used (see Conversational AI Terminology table)
- [ ] No regional idioms, slang, or celebratory tone
- [ ] Limitations stated transparently; no absolute claims

### Readability
- [ ] Meets target (≤70.0) or flags what to change to get there
