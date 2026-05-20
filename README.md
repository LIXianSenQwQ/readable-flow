# Readable Flow

Readable Flow is a Codex skill for Java Spring and backend work. It helps keep AI-generated code human-readable by favoring one clear entry method, a linear top-to-bottom flow, and explicit step comments.

The skill is not against encapsulation. It is against over-encapsulation that scatters one business use case across too many tiny helpers, managers, processors, interfaces, or abstract layers.

## Use It When

- You want Java Spring code that is easy to read from the main service method.
- AI is likely to split simple business logic into too many small methods.
- You want a large method to stay acceptable when it has clear step structure.
- You want every extraction or abstraction to justify how it improves readability.

## Core Rule

Prefer one readable flow first. Extract methods or abstractions only when they make the business process easier to understand.

## Files

- `SKILL.md` - default skill instructions.
- `agents/openai.yaml` - UI metadata for Codex.
