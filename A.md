# Deployment Manager — React 19.3 Modernization + UI Reconstruction

The application has already been upgraded to React 19 / React DOM 19.

Do NOT downgrade React.

The goal now is to bring the frontend dependency stack up to current stable, React-19-compatible versions and then completely correct the Deployment Manager UI according to the existing Deployment Manager design specification and the issues identified in the previous implementation.

## 1. Dependency modernization

First inspect the current package.json and lockfile.

Upgrade the frontend stack to current stable versions that are mutually compatible with React 19.3.

At minimum verify and modernize:

- react
- react-dom
- @types/react
- @types/react-dom
- three
- @types/three
- @react-three/fiber
- @react-three/drei
- animation libraries
- relevant UI/icon libraries
- relevant build tooling
- relevant TypeScript tooling

Use stable releases, not alpha, beta, canary, or experimental versions unless there is a compelling compatibility requirement.

For the 3D stack, use the current React-19-compatible generation of:

- Three.js
- @react-three/fiber
- @react-three/drei

Do NOT use @react-three/fiber v8.

Do NOT retain obsolete React-18 compatibility solely to preserve the previous implementation.

For animation, evaluate the current Motion ecosystem and use the modern React integration where appropriate rather than introducing additional overlapping animation libraries.

Do not blindly upgrade unrelated packages if doing so creates unnecessary application-wide breakage. Resolve dependency conflicts properly and keep the dependency graph coherent.

After dependency changes:

- remove obsolete packages
- remove duplicate animation libraries where they are no longer required
- regenerate the lockfile correctly
- run npm dedupe if appropriate
- verify npm ls has no invalid or conflicting React dependencies
- verify there is only one effective React major version
- verify there is only one effective React DOM major version

## 2. Do not treat the previous UI implementation as the design

The previous implementation is technically functional but visually incomplete.

It looks like a skeleton/prototype and incorrectly reuses existing Deployment Manager UI components.

Do not preserve existing visual components simply because they already exist.

The new Deployment Manager UI must be treated as a deliberate redesign.

Existing backend APIs, RBAC, authentication, discovery logic, audit logic, and working deployment-management functionality should be preserved unless changes are required to support the new UI.

The UI must be rebuilt around the finalized Deployment Manager specification.

---

# SCREEN 1 — DEPLOYMENT MANAGER OVERVIEW

This is the main landing screen after discovering a namespace.

The namespace selector remains.

The screen must contain:

## Header

Deployment Manager title and concise description.

Breadcrumb/navigation infrastructure must be available.

Example:

Deployment Manager

When entering a deployment:

Deployment Manager / <Deployment Name>

When entering an operation:

Deployment Manager / <Deployment Name> / Restart Operation

Breadcrumb items must be clickable for valid previous navigation states.

Do not use the old side drawer navigation pattern.

Do not open deployment details in the existing popup/drawer.

Deployment details must be a dedicated page.

## Search

Retain deployment search.

The search field must contain an integrated clear X button on the right side whenever text is entered.

Clicking X must:

- clear the search text
- immediately restore the full deployment set
- restore the appropriate topology/tile layout
- preserve the selected namespace
- preserve the current view mode

Do not add a separate clear button outside the search field.

## Summary metrics

Replace the current Degraded / Unknown / Updating / Stopped / Healthy card arrangement.

Use the finalized summary model:

- Total Deployments
- Total Pods
- Running Pods
- Not Ready Pods
- Updating Pods

These should be visually prominent but compact.

Each metric should be an interactive filter.

Clicking a metric filters the deployments/topology according to that status.

Do not create a second row of All / Healthy / Updating / Degraded / Stopped / Unknown filter pills.

There must be ONE filtering mechanism.

## View switcher

Retain:

- Topology
- Hex Tiles

Remove the old List option from the primary UI.

Topology is the default view.

Hex Tiles is the alternative view.

The switcher itself must visually belong to the new design system.

Use a polished segmented control rather than the current plain text controls.

The active state must be obvious through:

- filled background
- subtle glow
- smooth transition
- icon
- active indicator

## Refresh

Refresh must be a filled control consistent with the visual language.

Do not use the current plain-text refresh treatment.

Refresh should have:

- icon
- hover state
- pressed state
- loading/spinning state while discovery is running
- subtle transition

---

# SCREEN 1A — 3D TOPOLOGY

This is the flagship visual element.

The current topology implementation is NOT sufficient.

The current implementation resembles a floating rotating ball with basic nodes.

That is not the target design.

Rebuild/refine the topology so it feels like a finished enterprise-grade 3D Kubernetes control surface.

## Central Kubernetes object

The center must be a clearly recognizable Kubernetes cluster representation.

Use a polished translucent/glass/crystal 3D object.

It should visually feel like:

- crystalline
- translucent
- glass-like
- internally illuminated
- premium
- futuristic
- technically sophisticated

It must NOT look like:

- a generic sphere
- a simple wireframe ball
- an unfinished Three.js demo
- a rotating globe

Use actual geometric depth.

The central object should have:

- translucent material
- subtle internal illumination
- edge highlights
- controlled bloom/glow
- glass-like refraction/transparency where performance permits
- subtle surface detail
- restrained animation

Do not constantly rotate the central object.

The object may have extremely subtle ambient motion, but it should feel anchored.

## Deployment nodes

Each deployment should be represented as a physical 3D deployment cube/block.

Do NOT use floating text labels as the primary representation.

The deployment must feel like an actual object in the topology.

Each deployment object should have:

- 3D depth
- beveled/chamfered edges
- glass/crystal material
- internal status glow
- subtle shadow/depth
- readable deployment name
- replica readiness indicator

Examples:

1/1 Ready
2/2 Ready
1/2 Ready
0/1 Ready

The visual state must be immediately understandable.

## Status colors

Use a restrained enterprise palette.

Healthy:

- emerald/green
- subtle cyan-green glow

Updating:

- amber/orange
- animated restrained glow

Degraded:

- red/coral
- stronger warning glow

Stopped:

- neutral/dim
- low-energy appearance

Unknown:

- muted violet/gray

Do not make every object brightly saturated.

The overall topology must remain elegant.

## Energy flow

This is one of the most important visual elements.

The connection between the central Kubernetes cluster and each deployment should contain a continuous energy-flow effect.

The visual metaphor:

Kubernetes Cluster
→ energy pulse
→ deployment

The energy should continuously travel outward from the center toward deployment nodes.

Use:

- glowing particles
- small energy pulses
- directional motion
- subtle trails
- controlled bloom

The particles must clearly move FROM the Kubernetes cluster TO the deployment.

Do not make the entire scene rotate to create the animation.

The topology remains spatially stable.

Only the energy moves.

The animation should feel alive without becoming distracting.

## Connection lines

Connections should be thin and elegant.

Use:

- translucent lines
- subtle glow
- status-aware tint
- animated energy overlay

Do not use thick bright lines.

## Deployment placement

Do not hardcode deployment positions.

Generate deterministic layout positions from the discovered deployment set.

The topology must work with:

- 1 deployment
- 3 deployments
- 5 deployments
- 10 deployments
- 15 deployments
- 20+ deployments

Avoid excessive overlap.

Maintain readable spacing.

Use a deterministic layout algorithm rather than random positions.

The layout should remain stable between refreshes when the deployment set has not changed.

## Interaction

Hover:

- slightly increase glow
- subtly scale the deployment
- brighten its connection
- show concise deployment information

Click:

Navigate to the dedicated Deployment Details page.

Do NOT open the existing popup/drawer.

Selected deployment should remain visually highlighted.

## Camera

Allow restrained:

- orbit
- zoom
- pan

Provide compact camera controls.

Do not allow the user to accidentally destroy the composition.

Provide a reset-camera control.

The default camera position must be intentionally composed.

Do not start zoomed too far out.

## Background

The topology area should have a dark premium environment.

Use:

- deep near-black/navy background
- subtle atmospheric gradient
- very subtle star/particle field if performance permits
- restrained volumetric feeling

Do not turn it into a gaming UI.

This is an enterprise DevOps console.

The visual language should be futuristic but professional.

---

# SCREEN 1B — HEX TILE VIEW

This is the alternative to topology.

Do not use cards.

Do not use rectangular tiles.

Use the finalized hexagonal deployment tiles.

Each deployment should appear as a visually polished hexagonal object with:

- depth
- glass effect
- subtle border
- status indicator
- deployment name
- replica readiness

Example:

gb-svc-cc-sci-recon

1/1 Ready

Use the same visual design language as the topology:

- status colors
- glass effect
- glow
- typography
- animation
- hover behavior

The hex tiles should animate into position smoothly.

Hover should provide a subtle elevation/glow effect.

Click should navigate to the dedicated Deployment Details page.

No drawer.

No popup.

---

# SCREEN 2 — DEPLOYMENT DETAILS

Clicking a deployment from either topology or Hex Tiles must navigate to a dedicated page.

Do NOT reuse the existing Deployment Details popup/drawer.

Do NOT open a terminal-like interface.

This must be a completely new page.

Breadcrumb:

Deployment Manager / <Deployment Name>

## Header

Display:

- Deployment name
- Namespace
- current status
- desired replicas
- ready replicas
- available replicas
- updated replicas

Include a compact status indicator.

## Primary actions

Only display actions permitted by RBAC.

Possible actions:

- Start
- Stop
- Restart
- Scale

Do not expose action controls to users who do not have namespace-level permission for that operation.

Discovery and inspection remain available even when mutation privileges are absent.

## Deployment overview

Display useful Kubernetes information such as:

- namespace
- deployment name
- desired replicas
- ready replicas
- available replicas
- unavailable replicas
- strategy
- selector
- creation time
- age
- image
- image tag/digest where available
- container count
- restart information where available

Avoid dumping raw Kubernetes YAML into the primary UI.

Use structured information.

## Pods

Show each pod belonging to the deployment.

For every pod show:

- pod name
- status
- readiness
- node
- IP where available
- image
- container count
- age
- creation time
- restart count
- readiness state

Pods should be visually distinguishable.

Use compact expandable pod sections rather than a giant raw table.

## Container information

For each container, expose relevant information:

- container name
- image
- image ID where available
- ready
- restart count
- state
- started time
- ports where available

## Probes

Show:

- liveness probe
- readiness probe
- startup probe

Clearly indicate:

Configured
Not Configured
Passing
Failing

Do not fabricate probe results if Kubernetes does not expose them directly.

## Events

Show relevant recent Kubernetes events.

Examples:

- Pulling
- Pulled
- Created
- Started
- Killing
- Unhealthy
- BackOff
- Failed
- Scheduled

Events should be visually prioritized by severity.

## Failure intelligence

When a deployment/pod is unhealthy, provide a concise explanation derived from actual Kubernetes state.

Examples:

CrashLoopBackOff
ImagePullBackOff
Readiness probe failed
Liveness probe failed
Container terminated
Pending scheduling
Insufficient resources

Do not invent explanations.

If relevant, provide a compact link/action to inspect logs.

Do not dump the entire log stream into this page.

---

# SCREEN 3 — ACTION / RESTART PIPELINE

Starting, stopping, restarting, or scaling a deployment must navigate to the dedicated operation page.

Do NOT use the existing side drawer.

Do NOT open the existing terminal UI.

Do NOT reuse the current action-progress popup.

Build a completely new operation screen.

Breadcrumb:

Deployment Manager / <Deployment Name> / Restart Operation

## Pipeline

Use the finalized horizontal pipeline concept:

Initiated
→ Updating
→ Pods Replacing
→ Waiting for Ready
→ Completing
→ Finished

Each stage should have:

- status icon
- label
- active animation
- completed state
- failed state
- timestamp where available

The active stage should have a restrained animated glow.

Completed stages should visually remain completed.

Future stages should be subdued.

## Deployment state

Show:

- deployment name
- namespace
- operation
- previous replica count
- target replica count
- current ready replicas
- desired replicas
- available replicas

## Pods

Show the affected pods and their live state.

Example:

Pod
gb-svc-cc-sci-recon-xxxxx

State
Terminating / Pending / Running / Ready / Failed

Readiness:

0/1
1/1

Do not display a raw terminal.

## Progress

Progress must be derived from actual Kubernetes state.

Do not create fake percentage progress.

For restart:

Initiated
→ Deployment update observed
→ Old pods terminating
→ New pods created
→ New pods running
→ New pods ready
→ Deployment complete

Only mark Finished when the Kubernetes deployment actually satisfies the required readiness condition.

## Failure handling

If the operation fails or times out:

Show:

- failed stage
- concise reason
- affected pod
- Kubernetes event reason
- relevant condition
- recommended inspection target

Examples:

Readiness probe failed

CrashLoopBackOff

ImagePullBackOff

Pod scheduling failed

Deployment exceeded progress deadline

Do not dump huge logs.

Provide a compact expandable diagnostic section.

## Completion

Successful completion should show a clear finished state.

Include:

- operation
- deployment
- namespace
- start time
- completion time
- duration
- final replica state
- affected pods

Provide navigation back to Deployment Details.

---

# RBAC

Do not break existing RBAC.

Deployment Manager introduces an additional namespace-level authorization layer.

Existing application-level/job-level authorization remains intact.

The new job-level permission controls mutation actions.

Discovery and inspection remain available where existing access allows it.

Mutation controls must be evaluated against namespace-level permission.

Example:

Developer:

dev namespace → Start/Stop/Restart/Scale permitted

QA:

qa namespace → permitted according to configured namespace access

Developer:

qa namespace → actions hidden/disabled according to authorization

DevOps/Admin:

authorized namespaces according to existing policy

Do not infer permissions from frontend state alone.

Backend authorization remains authoritative.

Frontend visibility is only a UX representation of backend authorization.

---

# AUDIT

Every mutation operation must be auditable.

Capture:

- user
- timestamp
- namespace
- deployment
- operation
- previous replica count
- target replica count
- result
- duration
- affected pods
- failure reason if applicable

Audit data must not be exposed to ordinary users unless their existing authorization permits it.

Admin/DevOps users should have access to the audit view.

Do not expose sensitive implementation details unnecessarily.

---

# PERFORMANCE

The 3D topology must be lazy-loaded.

Do not load Three.js for users who remain in Hex Tiles or other views.

Use React lazy loading / Suspense appropriately.

Avoid unnecessary re-renders.

Avoid recreating the Three.js scene on every Kubernetes refresh.

Keep topology layout deterministic.

Do not animate React state every frame.

Use the Three.js render loop for 3D animation.

Use GPU-friendly materials and effects.

Avoid excessive bloom, post-processing, particle counts, or geometry complexity.

20+ deployments must remain usable.

Provide a graceful fallback when WebGL is unavailable.

The fallback must be visually polished, not a broken empty canvas.

---

# REACT 19.3 FEATURES

Since the application is now on React 19.3, evaluate appropriate use of modern React capabilities where they genuinely improve the UI.

In particular evaluate:

- View Transitions
- Activity
- Suspense
- lazy loading
- transitions
- modern React DOM APIs

Do not introduce APIs merely because they are new.

Use them where they improve navigation, loading, or perceived performance.

---

# VISUAL QUALITY BAR

The implementation must NOT look like:

- a skeleton
- a prototype
- a generic admin dashboard
- a collection of existing components
- a Three.js demo
- a terminal
- a drawer-based CRUD application

It must look like a deliberate, premium DevOps control surface.

The visual language should be consistent across all three screens:

Topology
→ Deployment Details
→ Action Pipeline

Use the same:

- typography
- spacing
- border treatment
- glass effect
- status colors
- iconography
- animation curves
- hover behavior
- transitions
- button treatment
- background treatment

The 3D topology can be visually richer than the other screens, but it must still belong to the same product.

---

# VALIDATION

After implementation run:

1. TypeScript check
2. ESLint
3. Unit tests
4. Existing frontend tests
5. Production build
6. npm dependency validation
7. Verify no React 18 dependency remains
8. Verify React 19.3 compatibility
9. Verify @react-three/fiber React 19 compatibility
10. Verify topology lazy loading
11. Verify Hex Tile view without loading Three.js
12. Verify WebGL fallback
13. Verify 1, 5, 10, 15 and 20+ deployments
14. Verify search + clear button
15. Verify metric filtering
16. Verify topology → deployment navigation
17. Verify Hex Tiles → deployment navigation
18. Verify deployment → action navigation
19. Verify breadcrumbs
20. Verify RBAC hides unauthorized mutation actions
21. Verify audit records
22. Verify existing Deployment Manager functionality has not regressed

Do not declare completion merely because the application builds.

The visual implementation must be inspected against the specification.

If a component is technically functional but visually incomplete, continue refining it.

The final result should be production-quality, not a functional prototype.
