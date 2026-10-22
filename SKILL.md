---
name: huahua-code-style
description: 【全局开发规范】 Apply the user's development, file-authorization, tool, art and UI-review conventions across projects, including strict code style when writing or modifying code; C# is the primary style reference.
metadata:
  short-description: Huahua's global code style and implementation conventions
---

# Huahua Code Style

Apply the collaboration and tool rules across all projects, and the code rules whenever code is written or modified. C# is the primary formatting reference. Enable Unity, Netcode, Blender and UI rules only for the corresponding work.

## General principles

- Inspect the existing code style before editing and avoid unrelated refactors.
- Keep changes scoped to the request and preserve user changes.
- The user leads development. A recommendation is not authorization to implement it.
- For next-step recommendations, first read the project's short overview and actual progress, then relevant full design sections and the real code/scenes. Explain the recommended part, rationale, dependencies and acceptance criteria; let the user decide.
- Ask the user when implementation format, responsibility boundaries, or a long-term rule is uncertain.
- Add a new rule only after explaining it and receiving the user's confirmation.
- If a new rule conflicts with an existing rule, point out the conflict and ask which rule to keep.

## File and project boundaries

- The user creates project folders, scripts, scenes, resources, configurations, and project structure.
- By default, modify only existing files explicitly authorized for the current task.
- Do not create, delete, move, copy, or rename project files without explicit authorization.
- If a new file is required, first provide its complete path, purpose, and reason, then wait for the user to create it unless the user explicitly authorizes creation.
- Before editing, list each complete path, its purpose, and the exact modification goal.
- After editing, report each actual path, its purpose, a change summary, and the verification result.
- Authorization is task-scoped. List new paths, purposes and scope together; do not repeatedly request approval for the same approved scope or treat one initialization as permanent permission to create files. Include generated companion files (such as Unity `.meta`) in the relevant asset scope.

## Tools and art production

- Do not use desktop or browser computer-control automation in any project. Use files, APIs, CLIs and MCP tools; provide preview files or a local page for the user to inspect and return feedback.
- For Unity work, read the applicable official `unity:*` plugin skills before implementation. Use available API/CLI/MCP capabilities and distinguish third-party Unity MCP servers from the official skill package.
- **Persistent MCP reconnection — user confirmed 2026-10-10:** If MCP disconnects, times out, or becomes temporarily unavailable during execution, preserve progress and continue diagnosis and automatic reconnection within existing configuration and authorization. Use increasing wait intervals with a capped delay; do not end the task merely after one failure or a fixed small retry count. After reconnection, use an actual MCP call to verify connectivity and the intended project/resource identity, reconcile the interrupted operation's actual outcome, and resume only unfinished work. Do not blindly replay successful writes or writes with unknown outcomes. For Unity development, a verified target-project MCP connection is required before starting or resuming; while disconnected, pause development operations but continue recovery, and do not bypass this requirement through offline code edits or CLI/API alone. Honor a user stop request. If recovery requires user action for permissions, configuration, or an external failure, explain attempted steps and the recovery point and request the necessary help instead of making futile retries. Reconnection does not expand authorization or authorize restoring disabled services.
- Resolve missing art within the authorized task. Inspect reusable existing assets first; prefer Blender MCP for missing models. If MCP is difficult to connect or unavailable, Blender CLI (background mode with Python scripts) is an authorized fallback without another tool-switch confirmation. Check applicable skills, installed plugins and the chosen execution route before modeling. Report actual tool status and results; file authorization and the ban on computer control still apply.
- For image generation/editing, use the installed `image-gen-pro` skill and its `gpt-image-2.5-sunburst` imagen workflow, including its diagnostics and artifact verification. Do not silently switch model/provider or treat a dry run as a generated image.
- Asset production, exports, previews and generated artifacts remain subject to file authorization. A tool preference does not authorize additional deliverables or an expanded task.

## UI design and iteration

1. Create a coherent visual concept and obtain the user's approval of the direction.
2. Break down the approved concept into asset requirements and generate each new UI image asset independently as a standalone output; organize reusable single assets within one shared visual system. The concept is a style/layout reference, not a production sprite sheet.
3. Assemble a web preview, defaulting to HTML/CSS, SVG and existing assets; another suitable non-computer-control method may be used. Iterate from user feedback.
4. For Unity projects, only after the user approves the preview, implement UI prefabs with the project's chosen UI stack; other projects implement in their target stack after the same approval gate.
5. Treat the user's adjusted result as the next iteration's reference. Propose reusable new rules and update this skill only after approval; do not silently turn every local adjustment into a global rule.

Keep fonts, colors, spacing and controls consistent. Approval at one stage does not automatically approve later stages or new files outside the agreed task.

**Standalone UI asset rule — user confirmed 2026-10-10:** Never crop production UI assets from a concept image, screenshot, or composite asset board. Do not generate a multi-asset sheet/atlas and then slice it into production assets. “Split” means split the requirements and generation jobs, not split the concept image. Each new UI image must be generated individually, with its own output and provenance, and inspected separately before integration. Reuse existing approved, independently generated standalone assets when suitable. Trimming transparent margins of a single generated asset, configuring nine-slice borders, and runtime atlas packing of compliant individual assets are allowed normalization/optimization, not exceptions to this rule. Keep labels, counts, and state-dependent text dynamic in the UI layer (uGUI/TextMeshPro for Unity), not baked into artwork. Preserve historical evidence; this rule does not authorize an unrelated retrospective asset replacement.

**Minimal text in UI concept images — user confirmed 2026-10-10:** Prioritize layout, colors, controls, and visual hierarchy. Include only text necessary to understand the interface or actions, such as an essential title, button label, key value, or state; these examples are not mandatory elements in every image. Do not fill concept images with meaningless placeholder text, redundant labels, lengthy explanations, or decorative copy. Keep design explanations outside the image. Omit nonessential text or replace it with appropriate visual cues while preserving necessary interface meaning.

## Class documentation and comments

- Every script must begin with an accurate XML class summary.
- The class summary must make the responsibility, usage context, and boundary obvious; include a short example when useful.
- Every method declaration must have one blank line immediately before its XML documentation comment and must include an XML `<summary>` describing its responsibility.
- Every method parameter must have a matching `<param name="...">`; every non-void method must have a meaningful `<returns>` description. Void methods do not need an empty `<returns>` tag.
- Keep ordinary method summaries to one line whenever possible.
- Add `param`, `returns`, or additional lines only for parameters, return values, complex flows, or important constraints.
- Use concise Chinese comments by default.
- Add a concise comment to every field, constant, property, event, method parameter, and local variable.
- Use XML `<param>` comments for method parameters and a nearby `//` comment for local variables.
- Divide method bodies into relatively independent functional sections rather than commenting every statement.
- When one method contains two or more independent functional sections, add one concise comment before each section.
- A functional section may contain conditions, assignments, method calls, and early returns; do not repeat comments for each statement inside it.
- Place the section comment before the first statement and describe the section's overall purpose, effect, or constraint.
- Typical functional sections include local/remote filtering, input reading, data conversion, state validation, network requests, component initialization, shared-state application, and failure handling.
- Add a separate comment to a condition only when the condition represents an independent behavior or important restriction.
- Ordinary null checks, loop control, and simple early returns do not require repeated standalone comments.
- The method summary describes the method's overall responsibility; internal section comments describe its distinct functional sections.
- A practical test is: if removing one block would leave another block as a separately describable behavior, treat them as separate functional sections.
- Explain necessary purpose, reason, and constraints; do not translate every line of code into a comment.
- Use `[Tooltip]` as the Inspector-facing comment for serialized Unity fields; do not duplicate the same text with a normal comment.
- Constants and nonserialized fields do not use `[Tooltip]`; document them with a concise nearby `//` or XML comment.
- Comments describe variables and meaningful logic blocks, not every individual statement.

## Script layout

Use this default order:

1. `using` directives.
2. XML class summary.
3. Class declaration.
4. Parameter fields.
5. A blank line.
6. Reference fields.
7. A blank line.
8. Constants, state fields, properties, and events.
9. Lifecycle methods.
10. Custom methods.

Use parameter and reference sections when they exist:

```csharp
[Header("参数")]

[Tooltip("移动速度"), SerializeField] private float moveSpeed = 5f;

[Space(10)]

[Header("引用")]

[Tooltip("角色控制器"), SerializeField] private CharacterController characterController;
```

- Leave blank lines between logical sections.
- Do not create empty sections when a script has no fields for that section.
- Place lifecycle methods in execution order, such as `Awake`, `OnEnable`, `Start`, `Update`, `LateUpdate`, `OnDisable`, and `OnDestroy`, using only methods the script needs.
- Place custom methods after lifecycle methods.
- Keep short attributes and field declarations on one line in the user's preferred style; split only when readability requires it.
- Treat 120 characters as the soft line-length limit; keep short declarations, calls, and assignments on one line and wrap longer or hard-to-read expressions.
- Put opening braces for classes, methods, conditions, loops, and other control-flow blocks on their own line.
- Always use braces for `if`, loops, and other control-flow blocks.

## Naming and using directives

- Types, methods, properties, events, and enums use `PascalCase`.
- Private fields, parameters, and local variables use `camelCase`.
- Constants use `PascalCase`.
- Follow the existing project namespace convention; do not introduce or rewrite namespaces just for uniformity.
- Group `using` directives as system, third-party, then project references; keep each group clear and stable.
- Do not add underscores to every script name for artificial consistency.
- Use object-name underscores only where the project already defines that convention, such as `Crate_Interaction`.

## References and resources

- Prefer runtime lookup for references that can be found reliably instead of assigning them in the Inspector.
- Prefer `GetComponent`, `GetComponentInChildren`, `transform.Find`, or equivalent runtime lookup for components.
- Prefer `Resources.Load` for resources that are appropriate to centralize there, but treat this as a preference rather than an absolute rule.
- Keep reference fields empty in the Inspector when runtime lookup is intended.
- Do not add null checks everywhere merely because a value could be null.
- If a missing reference blocks the current feature, expose the error so the actual configuration problem is easy to locate.
- If a missing reference affects only optional presentation or a non-critical branch, use a simple guard and continue the main flow.
- Normal business branches, such as a raycast not hitting an interactable object, are not configuration errors and should be handled normally.

## Responsibility and shared capabilities

- Keep each script focused on one clear responsibility.
- Interfaces define capabilities and data shared by all implementations.
- Shared scripts execute common detection, input, prompt composition, and invocation flows.
- Concrete object scripts provide only their object-specific wording, state, and behavior.
- Move stable common behavior into an interface or shared layer; do not prematurely abstract behavior that has not become a stable commonality.
- Keep fields private by default; expose read-only data through properties and state changes through explicit public methods.
- Do not let another script directly modify another script's fields; use a method or property with a clear responsibility instead.

For an interaction system, for example:

- The interface defines the unified interaction key, action-text contract, permission check, and interaction entry point.
- The player interaction script performs raycast detection, reads input, combines the key with the action text, and invokes the interaction.
- The crate script owns only crate-specific pickup, drop, and network behavior.

## Unity and multiplayer rules

Enable these rules only in Unity or Netcode code:

- Only the local player reads keyboard, mouse, and local interaction input.
- Remote players do not run local camera, cursor-lock, or input logic.
- Inherit from `NetworkBehaviour` only when the script actually needs RPCs, `NetworkVariable`, network-object state, or Netcode lifecycle callbacks.
- The server validates and decides shared multiplayer results.
- Clients send requests; they do not directly decide shared state.
- After changing Unity code, perform compilation, scene, and runtime checks proportional to the risk.

## Verification and reporting

- Check compilation errors after code changes.
- When Unity components, prefabs, or scenes are involved, check actual attachment and runtime behavior.
- For multiplayer logic, verify local-input isolation and server-authoritative shared state.
- Never describe a static check as a successful runtime test.
- Explicitly report anything that was not verified.
- Deliver a report containing the actual modified paths, summaries, and verification results.

## Rule iteration

- When the user proposes a new code-format, comment, structure, or naming preference, first list the proposed additions or changes.
- After user confirmation, update this skill and validate it.
- New rules apply to future code and code modified in future tasks; do not rewrite old code merely because the skill changed.
- The skill itself must follow the same file-authorization and reporting boundaries.
