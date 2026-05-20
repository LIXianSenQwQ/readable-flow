---
name: readable-flow
description: Readability guardrail for Java Spring and backend work. Use when Codex designs, implements, reviews, or tests code that must stay human-readable, with one main entry method per use case, a linear top-to-bottom flow, clear step comments, and a readability check before extracting helper methods, interfaces, abstract classes, template methods, factories, strategies, managers, processors, or other over-encapsulation.
---

# Readable Flow

## Core Intent

Optimize for code that humans can read and maintain. This skill is not against encapsulation. It is against AI-generated over-encapsulation that scatters one business flow across many tiny methods, helpers, managers, processors, interfaces, or abstract layers just to look engineered.

Prefer one clear main flow over many shallow wrappers. A large method is acceptable when it reads from top to bottom as a business process and uses clear step comments. Do not split code only because a method looks long.

## Single Workflow

Follow this workflow in order:

1. Read local context.
2. Choose one main entry method for the use case.
3. Write the readable linear flow.
4. Run the readability check before any extraction or abstraction.
5. Test and review through observable behavior.

## 1. Read Local Context

Before designing, implementing, or reviewing, quickly inspect the project conventions that exist:

- `AGENTS.md` / `SKILL.md`
- `CONTEXT.md` / `CONTEXT-MAP.md`
- `docs/adr/`
- Nearby production code and tests

Use these files only to align with terminology, boundaries, style, and existing decisions. Do not create or edit `CONTEXT.md`, ADRs, or other documentation just because this skill is active.

## 2. Choose One Main Entry Method

For each business use case, prefer one main method that carries the complete readable flow. In Spring projects, this is often a service method called by a controller, job, listener, or application service.

The reader should be able to open the entry method and understand:

- what is validated
- what data is loaded
- what business decisions are made
- what state changes happen
- what is returned or published

Do not make the reader jump through many one-line private methods to reconstruct the use case.

## 3. Write A Readable Linear Flow

A method may be long if it stays readable. Structure the method as a top-to-bottom flow with step comments before meaningful business blocks.

For Chinese Java projects, prefer comments in this style:

```java
// 步骤1：校验输入参数，避免后续流程处理无效请求

// 步骤2：读取订单与用户信息，保证业务判断基于最新状态

// 步骤3：根据当前订单状态执行业务校验

// 步骤4：保存结果并返回调用方需要的响应
```

For English projects, use the equivalent:

```java
// Step 1: Validate input before the business flow starts

// Step 2: Load the data needed for business decisions

// Step 3: Apply the business rules in execution order

// Step 4: Persist the result and return the response
```

Avoid unreadable large methods. A large method fails this skill when it has no step structure, mixes unrelated responsibilities, hides important failure handling, or becomes a block of unmarked branching.

## 4. Run The Readability Check

Before extracting a helper method, interface, abstract class, abstract method, template method, factory, strategy, adapter, base class, manager, processor, handler, executor, registry, or callback mechanism, answer these questions:

1. Does the extraction make the main flow easier to read from top to bottom?
2. Is there real reuse now, not imagined future reuse?
3. Does the extracted name express a business step instead of a technical wrapper?
4. Would a maintainer understand the use case faster after this extraction?
5. Is the complexity already present in current requirements or nearby code?
6. Can observable behavior tests still describe the feature without depending on this internal shape?

Default decisions:

- One implementation: do not create an interface.
- One algorithm: do not create a strategy.
- One caller: do not create a framework or manager layer.
- One reusable block: keep it inline unless extraction clearly improves readability.
- One variable step: prefer clear inline branching or a small private method over a template method.
- Future-only extensibility: treat it as unnecessary.

If the check fails, keep the code inline or use the simplest local structure. Explain the removed complexity briefly, for example: "Kept this inline because extracting a private method would hide the payment flow without adding reuse."

## 5. Test And Review By Behavior

When business logic is involved, use behavior-first testing:

1. Write one test that observes a real outcome through a public interface.
2. Implement the smallest readable flow that passes.
3. Add the next behavior only after the first one is green.
4. Refactor only while tests are green, and only when readability improves.

Tests should describe what the system does for the caller. They should not lock in private helper names, class hierarchies, or temporary abstractions.

During review, prioritize these findings:

- The main flow is scattered across too many small methods.
- A long method has no clear step comments.
- Helper methods wrap one or two lines without adding business meaning.
- Interfaces, abstract classes, or strategies exist with only one real implementation.
- Names like `Manager`, `Processor`, `Handler`, `Helper`, or `Util` hide domain intent.
- The reader cannot understand the use case from the entry method.

Give concrete simplification guidance. Prefer: "Inline these two private methods and add step comments around the validation and persistence blocks." Avoid vague feedback like: "This might be overengineered."
