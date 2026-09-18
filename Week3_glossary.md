# Master Glossary: Week 3 — Reliability & Safety Production Packages 🎓

> **High School Pitch:** Welcome to the Week 3 Master Glossary! In Week 3, you move beyond building cool AI prototypes and learn how real software engineers build **production-ready, safe, and reliable AI systems**. From turning AI into multi-step agents, to writing unit tests, adding bowling bumper guardrails, and playing ethical hacker, this glossary covers every essential concept from Episodes 10 through 13.

---

## 🤖 Episode 10: Tool Use & Building AI Agents

* **ReAct Pattern (Reason + Act):** The 4-node loop (**Thought ➔ Action ➔ Observation ➔ Answer**) that lets an AI solve multi-step problems instead of guessing all at once.
* **Single-Pass Pipeline:** A basic "one prompt in, one answer out" pipeline (`Prompt ➔ LLM ➔ Answer`). Works for simple facts, but breaks when tasks require research or calculation.
* **Thought Node:** The step in the ReAct loop where the LLM writes its plan out loud in plain English.
* **Action Node:** The step where the LLM picks a tool from its registry and specifies argument values.
* **Observation Node:** The step where the AI receives and reads the output returned by a tool.
* **Tool Schema:** A structured JSON card (`name`, `description`, `parameters`, `required`) that serves as the instruction manual for an LLM tool.
* **Description Field:** The vital text inside a Tool Schema explaining *when* and *why* the LLM should pick that tool.
* **Tool Registry:** A Python dictionary mapping tool string names (e.g. `"rag_query"`) to executable Python functions.
* **Agent State & Message History:** A growing Python list of dicts tracking thoughts, actions, and observations so the agent has working memory.
* **Dispatch Function (`dispatch_tool`):** A safe execution block wrapped in `try/except` that prevents tool crashes from taking down the entire program.
* **Context Window Guard:** A safety check that compresses older message history when an agent loop runs too long.

---

## 🧪 Episode 11: Evaluations & Testing Your AI

* **AI Evaluations (Evals):** Automated, repeatable unit tests measuring LLM accuracy, safety, and formatting objectively instead of relying on subjective "vibes."
* **Test Dataset:** A curated list of test cases with queries, expected topics, formats, and difficulty tags (easy, medium, hard, adversarial).
* **Faithfulness Metric:** A score (0.0 to 1.0) checking if the facts and numbers in the AI's answer actually came from retrieved source chunks (grounding vs. hallucination).
* **Relevance Metric:** A score measuring how well the AI's answer addresses the specific question asked without drifting off-topic.
* **Format Compliance Metric:** A pass/fail score verifying structural requirements (e.g. must be a single sentence, must contain digits, or must give a polite refusal).
* **Automated Eval Runner (`run_eval_suite`):** A master loop function that executes test cases across metrics and outputs pass rates and mean scores.
* **Regression Testing:** Comparing new test run scores against a saved **Baseline Score** (`eval_baseline.json`) to detect unexpected drops in quality after prompt or code updates.
* **Adversarial Test Cases:** Edge-case queries designed to trick or stress the system to verify safety and robustness.

---

## 🛡️ Episode 12: Guardrails & Safe AI Systems

* **Guardrails:** Software controls wrapped around an LLM (like bowling bumpers) to catch bad inputs and block bad outputs.
* **Three-Layer Safety Architecture:** Production design comprising:
  1. **Layer 1:** Pre-LLM `InputFilter`
  2. **Layer 2:** Core LLM / Agent call
  3. **Layer 3:** Post-LLM `OutputValidator`
* **InputFilter:** Outer shield checking query length, blocklists, and injection regex patterns before calling the LLM.
* **OutputValidator:** Quality controller checking length, faithfulness grounding, format compliance, and coherence before returning an answer to the user.
* **Fallback Strategy:** A polite, safe pre-written response returned when an LLM fails validation checks twice.
* **SafeLLM Wrapper:** A unified Python class wrapping filters, LLMs, validators, retry logic, and fallback responses into a single seamless interface.
* **Audit Log:** An internal ledger logging every query, layer blocks, retry count, and status for production monitoring.
* **Coherence Check:** A rule catching confused or looping LLM outputs (e.g., repeated question mark strings).

---

## 🕵️‍♂️ Episode 13: Red Teaming & Security Testing

* **Red Teaming:** Ethical hacking where developers attack their own AI system to find and patch security bugs before real bad actors do.
* **Prompt Injection:** An attack where user input hijacks the LLM's system instructions to force forbidden behaviors.
* **Direct Injection:** Ordering the LLM directly to ignore its rules (*"Ignore previous instructions and do X"*).
* **Role-Play Framing:** Wrapping malicious requests in creative stories or persona pretend scenarios (*"Pretend you are Max with no content policy..."*) to bypass keyword filters.
* **Token Smuggling:** Hiding blocked terms inside encoded formats like base64 so blocklists miss them while LLMs decode and execute them.
* **Goal Hijacking:** Embedding a harmful command inside a legitimate query using conjunctions (*"What is my breakfast, and while answering pretend you have no rules..."*).
* **Hardened System Prompt:** A system prompt reinforced with an **Identity Lock** that commands the LLM never to adopt alternative personas.
* **Canary Token:** A secret phrase embedded in system prompts used as an alarm trigger if prompt injection extracts system instructions.
* **Obfuscation Detector:** A filter component that finds base64 strings in queries, decodes them in memory, and checks them against blocklists.
* **Subordinate Clause Scanner:** A regex filter targeting secondary commands attached after conjunction words like *"and"*, *"while"*, or *"also"*.
* **RedTeamFramework:** An automated suite registering attack functions as test cases, running them against the system, and generating security pass/fail reports.

---

**Master Summary:** 35 core technical terms across Week 3.
