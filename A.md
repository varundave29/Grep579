Kubernetes Deployment Management

Ultra-Modern Frontend + Deployment Lifecycle Management

Build a new generic Kubernetes Deployment Management job inside the existing application.

This is a flagship frontend screen. Treat the UI/UX quality as a first-class engineering requirement, not as cosmetic polish. The objective is to demonstrate advanced frontend engineering, modern visualization, animation, interaction design, information hierarchy, real-time state handling, and operational UX.

Do not create a generic CRUD/table interface.

The final experience should feel like a modern, premium Kubernetes operations console with a futuristic dark visual language, 3D topology visualization, glass/crystal surfaces, animated infrastructure connections, smooth state transitions, and highly polished operational workflows.

The functionality must integrate with the existing application without breaking existing jobs, RBAC, authentication, job execution, Pod Metrics, or other functionality.

⸻

1. IMPORTANT IMPLEMENTATION RULE

Before changing code:

1. Inspect the existing repository thoroughly.
2. Identify:
    * frontend framework
    * component architecture
    * theme implementation
    * existing design system
    * CSS strategy
    * existing animation libraries
    * existing icon library
    * existing WebSocket/live-update implementation
    * job framework
    * RBAC implementation
    * user/group management
    * job-start YAML/application configuration
    * Pod Metrics discovery/status implementation
    * existing job history/audit implementation
3. Reuse existing infrastructure wherever practical.
4. Do not duplicate existing functionality unnecessarily.
5. Do not break existing jobs.
6. Do not replace the entire frontend stack simply to build this screen.
7. If the existing stack supports it, use modern libraries such as:
    * Framer Motion / Motion
    * React Three Fiber
    * Three.js
    * Drei
    * React Flow where appropriate
    * a maintained chart/visualization library
    * a maintained icon library
8. Introduce a new dependency only when it materially improves the required experience and is compatible with the existing application.

The final result must feel like one cohesive product, while this screen can have a significantly more advanced visual treatment than older screens.

⸻

2. PRODUCT CONCEPT

The feature is a generic Kubernetes Deployment Management console.

The primary object is:

Deployment

Not Pod.

The UI hierarchy is:

Namespace
    ↓
Deployments
    ↓
Pods
    ↓
Containers

The user first selects/discovers a namespace.

The application discovers all accessible Deployments in that namespace.

The user can visually inspect the namespace and its Deployments.

The user can then open an individual Deployment to inspect its Pods and operational state.

Authorized users can perform:

* Restart
* Stop
* Start
* Scale

For this implementation, Start/Stop/Restart share the same lifecycle permission. There is no need for separate user permissions for each action.

⸻

3. SCREEN ARCHITECTURE

There are three functional screens, with the first screen having two visualization modes.

Screen 1A — Deployment Topology

Default visualization.

Purpose:

Understand the health and structure of the selected namespace at a glance.

Screen 1B — Deployment Hex Tile View

Alternative visualization.

Purpose:

View the same Deployment information in a highly visual, searchable hexagonal layout.

Screen 2 — Deployment Details

Purpose:

Inspect one Deployment, its Pods, containers, health, events, metrics, and operational details.

Screen 3 — Deployment Action / Progress

Purpose:

Perform and observe Start / Stop / Restart operations with a highly polished real-time progress experience.

Topology must be the default Screen 1 mode.

Hex Tile View must be the only alternate visualization.

Do not add additional visualization modes.

⸻

4. GLOBAL VISUAL DESIGN LANGUAGE

The entire feature must use one coherent visual system.

Overall aesthetic

Use:

* futuristic
* premium
* dark
* glass/crystal
* high-tech infrastructure console
* subtle 3D depth
* restrained neon accents
* elegant motion
* excellent information hierarchy

Avoid:

* generic Bootstrap appearance
* ordinary enterprise tables
* flat cards everywhere
* excessive gradients
* excessive neon
* childish/glowing gaming UI
* excessive glass blur that hurts readability
* excessive animation
* visual clutter

The goal is:

premium infrastructure visualization, not a gaming dashboard.

⸻

5. COLOR SYSTEM

Use a very dark navy/blue-black base instead of pure black.

Suggested base colors:

Background:
#050914
#07101D
#0A1220
Primary Surface:
rgba(14, 25, 42, 0.70)
Secondary Surface:
rgba(20, 34, 55, 0.55)
Glass Border:
rgba(120, 190, 255, 0.16)
Primary Accent:
#38BDF8
Bright Accent:
#67E8F9
Secondary Accent:
#818CF8
Healthy:
#34D399
Healthy Bright:
#6EE7B7
Updating:
#60A5FA
Warning:
#FBBF24
Failure:
#F87171
Failure Bright:
#FB7185
Stopped:
#64748B
Primary Text:
#E6F1FF
Secondary Text:
#94A9C4
Muted Text:
#64748B

These values are a starting design system. Consolidate them into theme tokens rather than scattering raw values throughout components.

Use semantic colors consistently.

Healthy

Green/cyan glow.

Updating

Electric blue glow.

Warning

Amber glow.

Failed / Not Ready

Red/pink glow.

Stopped

Muted gray/blue with no active energy animation.

⸻

6. GLASS / CRYSTAL VISUAL LANGUAGE

Panels and topology objects should have a layered glass/crystal appearance.

Use:

* translucent dark surfaces
* backdrop blur where supported
* subtle inner gradients
* thin luminous borders
* very subtle outer glow
* layered shadows
* slight depth
* highlights along selected edges

Example conceptual style:

background:
  linear-gradient(
    135deg,
    rgba(...),
    rgba(...)
  );
border:
  1px solid rgba(120, 190, 255, 0.16);
box-shadow:
  0 0 30px rgba(..., 0.08);
backdrop-filter:
  blur(...);

Do not make every surface equally bright.

Use visual hierarchy:

Background
    ↓
Glass panel
    ↓
Glass object
    ↓
Selected object
    ↓
Active operation

Each level should progressively become more visually prominent.

⸻

7. TYPOGRAPHY

Use a modern clean sans-serif already compatible with the project.

Prioritize:

* readability
* compact operational information
* strong hierarchy

Recommended hierarchy:

Page title:
24–28px
Deployment title:
18–22px
Section heading:
14–16px
Primary operational value:
18–24px
Metadata:
12–13px
Secondary metadata:
11–12px

Use medium/semibold weights for important operational values.

Do not use huge headings that waste operational screen space.

⸻

8. SCREEN 1A — 3D DEPLOYMENT TOPOLOGY

This is the signature screen.

The default view after Discovery must be a 3D-style namespace topology.

The selected namespace is represented by a central Kubernetes object.

⸻

8.1 Central Kubernetes Object

Place a central translucent 3D cube/crystal object in the visualization.

The cube represents the Kubernetes namespace/control center.

It should have:

* transparent/glass surface
* subtle internal blue/cyan glow
* thin luminous edges
* slight 3D rotation/parallax
* subtle breathing/pulse
* Kubernetes icon or recognizable Kubernetes visual
* namespace name
* Deployment count
* Pod count

Example:

KUBERNETES
dev
15 Deployments
42 Pods

The object must visually feel like the source/control center of the namespace.

Do not make it enormous.

It should be the visual anchor while leaving sufficient room for Deployment nodes.

⸻

9. DEPLOYMENT NODES

Each Deployment should be represented as a 3D/glass node surrounding the central Kubernetes object.

The layout must be generated dynamically.

Never hardcode positions.

For example:

                  [api-service]
        [frontend]          [worker]
                 [ KUBERNETES ]
       [scheduler]          [proxy]
                  [backend]

This is only conceptual.

The actual visualization should use a dynamic layout algorithm.

⸻

10. SCALABILITY OF TOPOLOGY

The namespace may contain:

* 3 Deployments
* 5 Deployments
* 10 Deployments
* 15 Deployments
* 20+ Deployments

The visualization must remain usable.

Use dynamic radial/orbital positioning.

For small numbers:

Use one ring.

For larger numbers:

Use multiple rings/orbits around the center.

Example:

          Ring 1
       5–8 nodes
          Ring 2
       remaining nodes

Maintain minimum spacing between nodes.

Prevent overlapping labels.

Use camera zoom or automatic fitting.

Provide:

* Zoom In
* Zoom Out
* Reset View
* Fit All
* Fullscreen

Do not allow 15–20 Deployments to collapse into an unreadable cluster.

⸻

11. DEPLOYMENT NODE CONTENT

Each node should show only high-value information.

Example:

api-service
● Running
2 / 2
Pods Ready

Optionally include a compact replica indicator:

● ●

Do not put:

* image
* full container details
* node
* events
* timestamps
* YAML

on the topology node.

Those belong to Screen 2.

⸻

12. TOPOLOGY CONNECTIONS

This is a critical visual feature.

Every Deployment should have a visual connection to the central Kubernetes object.

Do not use static plain lines.

Use luminous energy/data-flow connections.

Conceptually:

             Deployment
                  ↑
             ✦ → ✦ → ✦
                  ↑
             Kubernetes

The particles must appear to travel from the central Kubernetes object toward the Deployment.

The effect should resemble a controlled flow of energy/data.

⸻

13. ENERGY FLOW ANIMATION

Implement animated particles travelling along each connection.

Requirements:

* continuous but subtle
* multiple particles at staggered intervals
* variable phase offsets
* smooth easing
* no abrupt restart
* no flashing
* no distracting strobing
* particles fade in/out smoothly
* flow direction is visually obvious

Healthy Deployment:

cyan/green particles

Updating Deployment:

blue particles
slightly faster flow

Warning:

amber flow

Failed:

red/pink flow

Stopped:

no active flow
muted connection

The flow should visually communicate Deployment state.

⸻

14. NODE STATUS EFFECTS

Healthy:

* subtle green/cyan edge glow
* normal particle flow
* very subtle breathing animation

Updating:

* blue edge glow
* slightly stronger pulse
* active connection flow

Failed:

* red edge glow
* controlled pulse
* red connection

Stopped:

* dim surface
* reduced opacity
* no particle flow

Do not continuously pulse everything.

Animation must communicate meaning.

⸻

15. HOVER INTERACTION

When the user hovers over a Deployment:

1. Node scales up slightly.
2. Node becomes brighter.
3. Border becomes more visible.
4. Connection becomes brighter.
5. Other nodes subtly reduce prominence.
6. Tooltip/detail preview appears.

Example:

api-service
● Running
2 / 2 Ready
2 Pods
0 Restarts
Click to inspect

Use smooth transitions around 180–300ms.

Do not instantly snap between states.

⸻

16. CLICK INTERACTION

Clicking a Deployment should:

1. Highlight the selected node.
2. Smoothly move/focus the camera toward it.
3. Preserve context.
4. Transition to Deployment Details.

Do not use a harsh page replacement.

Use a polished transition.

⸻

17. TOPOLOGY SEARCH

Provide a compact search control.

Example:

⌕ Search deployments...

When typing:

api

matching Deployments remain prominent.

Nonmatching nodes:

* fade
* reduce opacity
* reduce glow
* remain spatially stable where possible

Do not completely destroy and recreate the entire topology on every keystroke.

Use animated filtering.

⸻

18. TOPOLOGY FILTERS

Provide compact status filters:

All
Running
Updating
Warning
Failed
Stopped

Selecting a filter should smoothly emphasize matching nodes.

Avoid large filter panels.

⸻

19. TOPOLOGY SUMMARY

At the top of the screen provide a compact summary.

Example:

DEV
15 Deployments
42 Pods
38 Ready
2 Updating
2 Not Ready

Use small visual counters with semantic indicators.

These are informational only.

Do not put lifecycle controls here.

⸻

20. SCREEN 1B — HEXAGON TILE VIEW

The only alternative visualization to topology is the Hex Tile View.

Do NOT use rectangular cards.

Each Deployment must be represented by a hexagonal visual tile.

The geometry should be a proper hexagon or visually convincing hexagonal crystal tile.

Use:

* glass/crystal surface
* subtle gradient
* luminous border
* internal depth
* status glow
* small deployment icon
* deployment name
* replica state
* compact health information

Example conceptual content:

       ╱────────╲
      /          \
     | api-service |
     |             |
     |   ● ●  2/2  |
      \            /
       ╲──────────╱

The real UI should be significantly more polished than this conceptual representation.

⸻

21. HEX TILE STATES

Healthy:

* cyan/green border
* subtle glow

Updating:

* animated blue border
* subtle shimmer

Failed:

* red border
* controlled pulse

Stopped:

* muted gray/blue
* no active glow

Hover:

* slight scale
* stronger border
* brighter internal glow
* subtle elevation

Selected:

* persistent accent outline

⸻

22. HEX TILE LAYOUT

Tiles must automatically arrange themselves.

Support:

* 5 deployments
* 10 deployments
* 15 deployments
* 20+

Use responsive wrapping / grid logic.

Maintain consistent spacing.

Do not allow tiles to overlap.

The layout should animate smoothly when:

* filtering
* searching
* resizing
* deployment state changes

⸻

23. SCREEN 2 — DEPLOYMENT DETAILS

When the user clicks a Deployment, open the Deployment Details experience.

The screen should feel like moving from:

macro infrastructure view

to:

deep operational inspection.

⸻

24. DEPLOYMENT HEADER

Show:

‹ Deployments
api-service
● Running
Namespace:
dev
Replicas:
2 / 2
Strategy:
RollingUpdate
Age:
8 days

Actions appear only if the user has namespace lifecycle permission:

[ Restart ]
[ Scale ]
[ Stop ]

Users without lifecycle permission should not see these controls.

They can still inspect everything.

⸻

25. HEALTH SUMMARY

Create a compact visual health section.

Show:

Healthy
100%
Running       2
Ready         2
Available     2
Updating      0
Unavailable   0

Use subtle progress indicators.

Avoid giant dashboard gauges.

⸻

26. REPLICA VISUALIZATION

Show desired versus actual state.

Example:

Desired       ● ●
Ready         ● ●
Available     ● ●

If one is unavailable:

Desired       ● ●
Ready         ● ○
Available     ● ○

The transition between states must animate.

⸻

27. PODS SECTION

Display all Pods belonging to the Deployment.

Each Pod should be represented in a modern compact operational panel.

Show:

* Pod name
* Status
* Ready state
* Node
* Image
* Age
* Created time
* Restart count
* Pod IP where useful
* Container count
* container health

Example:

api-service-7d89f5c4
● Running       Ready 1/1
Node
worker-node-01
Image
registry/api:2.4.1
Age
8d
Created
18 Sep 2026 10:21
Restarts
0
IP
10.x.x.x
Containers
1

⸻

28. CONTAINER DETAILS

If the Deployment has sidecars, show them clearly.

Example:

Containers
api
● Running
registry/api:2.4.1
envoy
● Running
envoy:1.31

Do not flood the initial view with every environment variable or configuration value.

Those can be available in deeper inspection.

⸻

29. POD ACTIONS

Provide Pod-level actions where applicable:

Restart
Stop
Start

Again:

* only render actions if authorized
* backend must enforce authorization independently
* action behavior must respect the Deployment relationship

Do not treat a Deployment-managed Pod as an independent server behind the scenes.

⸻

30. DETAIL TABS

Use:

Overview
Pods
Events
Metrics
YAML

Overview

Health and deployment summary.

Pods

Detailed Pod inspection.

Events

Human-readable Kubernetes Events timeline.

Metrics

Reuse existing Pod Metrics functionality where appropriate.

YAML

Advanced inspection.

Do not expose YAML on the primary screen.

⸻

31. EVENTS

Render Kubernetes Events as a timeline rather than raw JSON.

Example:

10:24:22
✓ Pulled image
10:24:24
✓ Created container
10:24:25
✓ Started container
10:24:28
⚠ Readiness probe failed
10:24:31
✓ Readiness probe succeeded

Use semantic icons and colors.

⸻

32. SCREEN 3 — DEPLOYMENT ACTION WORKFLOW

This is the operational showcase screen.

It must be visually impressive.

When the user performs:

* Restart
* Start
* Stop
* Scale

transition into an operation-focused experience.

⸻

33. ACTION HEADER

Example:

Restarting Deployment
api-service
dev
2 replicas
RollingUpdate

Show the action prominently.

For example:

RESTARTING

Do not use generic:

⸻

34. OPERATION PIPELINE

Create a large horizontal or responsive pipeline:

●────────●────────●────────●────────○
Initiated
   ↓
Updating
   ↓
Replacing Pods
   ↓
Waiting for Ready
   ↓
Completed

Completed stages:

* bright
* solid
* checkmark

Current stage:

* animated
* glowing
* pulsing subtly

Future stages:

* subdued

The active connection between stages should animate.

⸻

35. PIPELINE ANIMATION

Use smooth transitions.

When moving from:

Updating

to:

Replacing Pods

animate the active indicator rather than instantly switching.

Use:

* progress line animation
* glowing active node
* subtle particle movement
* checkmark animation
* state transition

Avoid excessive motion.

Motion must communicate progress.

⸻

36. LIVE POD PROGRESS

Below the pipeline show actual Pods.

Example:

Pod Progress
✓ api-service-abc123
  Replaced
  Running
  Ready
● api-service-def456
  Running
  Waiting for readiness

Use animated state transitions.

If two replicas exist:

1 / 2 Pods Ready

animate the percentage/progress.

⸻

37. PROGRESS INDICATOR

Show:

1 / 2 Pods Ready
████████████░░░░░░░░
50%

The progress bar must animate smoothly.

Do not fake progress based purely on time.

Progress must be derived from actual Kubernetes state.

⸻

38. OPERATION TIMELINE

Show a compact operational timeline:

10:24:10   ✓ Restart request accepted
10:24:12   ✓ Restart triggered
10:24:15   ✓ New ReplicaSet detected
10:24:18   ✓ Pod replacement started
10:24:22   ✓ Pod created
10:24:25   ● Pod running
10:24:26   ● Waiting for readiness

This is not a raw log viewer.

It is a translated operational timeline.

⸻

39. SUCCESS STATE

On completion:

✓ Deployment restarted successfully
api-service
2 / 2 Ready
Duration
34 seconds

Use a controlled success animation.

For example:

* pipeline completes
* final node activates
* subtle glow expands outward
* checkmark appears
* success state settles

Do not use a giant celebratory animation.

Keep it professional.

⸻

40. FAILURE STATE

If the operation fails:

⚠ Deployment restart did not complete
api-service
dev
Reason
Readiness probe failed
Pod
api-service-abc123
Details
HTTP probe returned 503

Then:

Kubernetes Events
Readiness probe failed
Back-off restarting failed container

Actions:

[ View Events ]
[ View Recent Logs ]
[ Return to Deployment ]

⸻

41. FAILURE CLASSIFICATION

Use Kubernetes state and Events to classify failures.

Recognize at minimum:

* CrashLoopBackOff
* ImagePullBackOff
* ErrImagePull
* OOMKilled
* readiness probe failure
* liveness probe failure
* startup probe failure
* FailedScheduling
* FailedMount
* ContainerCreating timeout
* container initialization failure
* Pending timeout
* rollout timeout
* unknown failure

Translate them into concise user-facing explanations.

Do not rely exclusively on application logs.

⸻

42. START / STOP SEMANTICS

STOP

Before scaling to zero:

current desired replicas = N

Persist:

namespace
deployment
previousReplicaCount
user
timestamp
operationId

Then scale:

N → 0

Do not rely on Kubernetes to automatically provide a recoverable “previous replica count” for this application behavior.

START

Retrieve the stored previous replica count.

Restore:

0 → previousReplicaCount

If there is no valid stored replica state, fail safely and explain why.

Never blindly assume 1, 2, or 3.

⸻

43. RESTART SEMANTICS

Do NOT implement Deployment Restart as:

replicas → 0
replicas → original

Use a proper Deployment rolling restart mechanism.

The goal is for Kubernetes to recreate the Deployment Pods while preserving the Deployment’s desired state and respecting its rollout configuration.

Monitor the actual rollout until the Deployment becomes healthy again.

⸻

44. ASYNCHRONOUS EXECUTION

All lifecycle operations must remain asynchronous.

Conceptually:

UI
 ↓
Create operation
 ↓
Return operation ID
 ↓
Background job
 ↓
Kubernetes operation
 ↓
Watch state
 ↓
Push live state
 ↓
UI animation/state transition

Reuse existing asynchronous job infrastructure.

If WebSockets already exist, use them for live operation updates.

Do not implement inefficient polling if the existing architecture already supports event-driven updates.

⸻

45. RBAC

Add a reusable namespace-scoped permission layer on top of the existing RBAC.

New job permission concept:

Deployment Management

The exact final job name should be professional and generic.

Do not call it something overly narrow such as:

Restart Pod

because it manages Deployments.

Recommended working name:

Kubernetes Deployment Management

Use this consistently unless the existing product naming conventions suggest a better equivalent.

⸻

46. NAMESPACE AUTHORIZATION

Namespace configuration already exists in job-start YAML/application configuration.

Use that existing source.

Do not hardcode:

dev
qa
test
prod

in frontend code.

As applications and namespaces grow, discovery must automatically reflect them.

Users may have:

Developer
→ dev
Tester
→ qa
DevOps
→ dev + qa + prod

Users can inspect namespaces/deployments according to existing discovery rules.

Lifecycle controls require namespace permission.

⸻

47. CRITICAL RBAC UI RULE

Users without lifecycle permission:

MUST still see:

* deployments
* Pods
* containers
* health
* events
* metrics
* inspection information

But MUST NOT see:

* Restart
* Stop
* Start
* Scale

Do not merely disable the buttons.

Do not show disabled controls unless there is a strong UX reason.

Prefer completely omitting unauthorized actions.

Backend must independently enforce authorization.

⸻

48. AUDIT

Every operation must record:

user
timestamp
namespace
deployment
operation
previous replica count
result
failure reason
operation ID

Use existing job history/audit infrastructure if possible.

Do not create unnecessary parallel storage.

Audit visibility:

* Admin
* DevOps / authorized operational roles

Normal developers/testers should not automatically see the global audit history.

⸻

49. AUDIT UI

For authorized users, provide an operational history view or integrate with existing job history.

Example:

09:42
John Doe
Restart
dev / api-service
SUCCESS
34s

Failure:

09:31
Jane Doe
Restart
qa / worker
FAILED
Readiness probe failed

Use compact visual status indicators.

⸻

50. RESPONSIVENESS

The design must work across:

* desktop
* laptop
* smaller browser windows

The topology should adapt.

At smaller widths:

* reduce node size
* reduce spacing
* use controlled camera zoom
* maintain readability
* avoid overlapping labels

The Hex Tile View must also reflow naturally.

⸻

51. ANIMATION PRINCIPLES

Animation is important, but it must have purpose.

Use animation for:

* topology energy flow
* node state changes
* hover
* selection
* filtering
* search
* camera transitions
* pipeline progress
* progress bars
* pod state transitions
* success/failure transitions
* panel transitions

Avoid animation for:

* every piece of text
* every table row continuously
* unnecessary decorative movement

The experience should feel alive, not noisy.

⸻

52. ANIMATION TIMING

Use consistent motion tokens.

Suggested:

Micro interaction:
120–180ms
Hover:
180–220ms
Panel transition:
220–300ms
Layout transition:
300–500ms
Camera transition:
500–800ms
Major state transition:
500–1000ms
Topology ambient animation:
continuous, low intensity

Use appropriate easing curves.

Prefer smooth spring/eased motion over linear movement.

⸻

53. ACCESSIBILITY

Despite the visual complexity:

* maintain sufficient text contrast
* provide keyboard navigation
* provide visible focus states
* do not rely solely on color
* use icons + text/status labels
* support reduced-motion preferences where practical
* ensure important status information remains readable without animation

If reduced motion is enabled, disable/reduce:

* particle movement
* camera movement
* pulsing
* excessive transitions

⸻

54. PERFORMANCE

This screen may contain 20+ Deployments and many Pods.

Do not create an expensive render loop for every DOM element unnecessarily.

For 3D:

* optimize geometry
* reuse materials
* reuse particle systems where possible
* avoid excessive post-processing
* avoid unnecessary React re-renders
* use memoization where appropriate
* use virtualization where appropriate
* dynamically reduce visual complexity if object count becomes high

The UI must remain responsive.

Animation must not noticeably degrade interaction performance.

⸻

55. REAL-TIME DATA

Do not fake operational status.

Topology state must come from actual Kubernetes state.

When the Deployment changes:

2/2 → 1/2

the UI should animate the transition.

When:

1/2 → 2/2

the second indicator should transition into Ready.

When an operation completes, the topology/details state should update automatically.

⸻

56. DATA DISPLAY PRINCIPLE

Show enough information to be operationally useful.

Do not expose every Kubernetes field by default.

Primary UI:

* status
* replicas
* Pods
* readiness
* restarts
* node
* image
* age
* created time
* events
* rollout state

Advanced information belongs behind:

* detail sections
* tabs
* drawers
* inspection views

The user should understand the state without understanding Kubernetes internals.

⸻

57. NO HARD-CODED SAMPLE DATA

Sample/mock data is acceptable only in automated tests or isolated development fixtures.

The production UI must use dynamically discovered:

* namespaces
* Deployments
* Pods
* containers
* status
* events
* metrics

Never hardcode deployment names or namespace names.

⸻

58. TESTING

Add/update tests for:

Backend

* namespace discovery
* deployment discovery
* Pod discovery
* namespace authorization
* authorized lifecycle operation
* unauthorized lifecycle operation
* restart
* stop
* start
* replica preservation
* rollout monitoring
* timeout
* failure classification
* event retrieval
* audit records

Frontend

Test:

* topology rendering
* dynamic number of deployments
* topology layout
* topology filtering
* search
* topology → deployment transition
* hex tile rendering
* hex tile filtering
* unauthorized users not seeing lifecycle controls
* authorized users seeing lifecycle controls
* action workflow
* live operation state
* success state
* failure state
* responsive behavior

⸻

59. VISUAL QUALITY BAR

Do not stop when the screen is merely functional.

Perform a visual refinement pass after functionality works.

Check:

* spacing
* typography
* alignment
* hierarchy
* glow intensity
* border opacity
* glass transparency
* contrast
* animation smoothness
* transition timing
* topology spacing
* node collision
* label readability
* hover behavior
* loading states
* empty states
* error states
* responsive behavior

The final UI should look deliberately designed.

⸻

60. IMPORTANT: DO NOT RECREATE THE EXISTING SCREEN WITH A NEW COLOR

This is not a simple theme refresh.

The existing Pod Metrics screen can provide useful data/discovery logic, but this feature must introduce a genuinely new interaction model.

The user should immediately recognize:

This is the flagship Kubernetes operations experience.

⸻

61. FINAL USER JOURNEY

The intended experience is:

Open Deployment Management
        ↓
Select namespace
        ↓
Discover
        ↓
3D topology appears
        ↓
Central Kubernetes crystal appears
        ↓
Deployment nodes animate into position
        ↓
Energy flows from Kubernetes → Deployments
        ↓
User hovers / searches / filters
        ↓
User clicks Deployment
        ↓
Smooth transition
        ↓
Deployment Details
        ↓
Inspect replicas / Pods / containers / events / metrics
        ↓
Authorized user selects Restart / Start / Stop / Scale
        ↓
Action workflow opens
        ↓
Animated operational pipeline
        ↓
Live Pod progress
        ↓
Kubernetes state translated into human-readable status
        ↓
Success or intelligent failure explanation
        ↓
Return to Deployment Details / Topology

⸻

62. FINAL IMPLEMENTATION EXPECTATION

The finished implementation should demonstrate:

* advanced frontend architecture
* modern component design
* 3D visualization
* high-quality animation
* real-time state synchronization
* thoughtful information architecture
* strong operational UX
* responsive design
* accessible interactions
* namespace-scoped authorization
* reliable asynchronous Kubernetes operations
* auditability
* production-quality error handling

The most important requirement is the visual experience.

Do not implement a basic table first and consider the visual design finished later.

Build the architecture so that the visual experience is a first-class part of the feature.

The result should feel like a purpose-built modern Kubernetes operations product rather than an internal CRUD page.

Before finalizing, run the complete existing test suite, frontend lint/type/build checks, relevant backend tests, and manually inspect all four states:

1. Topology View
2. Hex Tile View
3. Deployment Details
4. Action/Progress Workflow

Verify both authorized and unauthorized users.

Do not break existing jobs, existing RBAC, Pod Metrics, authentication, or existing application functionality.
