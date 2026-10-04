# Technical Specification

> EDITING DIRECTIVE: DEVELOPER AND AGENT EDIT THIS FILE COLLABORATIVELY. THE DEVELOPER MUST REVIEW AND APPROVE ITS CONTENT.

Purpose of this file: Turn the approved research, project brief, and hand-drawn screen designs into testable requirements.

## Instructions for the Developer

Make and approve the product decisions, draw every proposed screen, provide the drawings to the Agent, and keep this file current as the intended result changes.

To begin, open the project repository in a fresh chat and enter:

`Read ./spec.md and help me begin the Project 3 specification.`

## Instructions for the Agent

Read `AGENTS.md`, `brief.md`, `research.md`, and this file. Review the screen drawings the Developer provides. Ask one focused question at a time, surface gaps and trade-offs without inventing requirements, and keep the specification concise and testable.

## Goal

State what the app should help its Users accomplish and name the user story or stories that define that need.

Help college students quickly decide what to wear each day by turning the current or forecasted weather for their chosen US location into a character-driven outfit recommendation, with helpful reminders (umbrella, sunscreen, hydration) when conditions call for them.

Grounded in the user story from `research.md`: "As a college student checking the weather each morning, I want to quickly see what to wear based on the day's forecast, so that I can get dressed appropriately without having to interpret raw weather data myself."

## Screen designs

Draw every proposed screen by hand, in both phone and laptop layouts, on paper, a tablet, a whiteboard, or another hand-drawing surface. Save photos or exports in `reference/`, provide them to the Agent, and link them here. Use the drawings to define layout, hierarchy, controls, navigation, and important interaction states.

**Main screen — phone:** [IMG_0205.jpeg](reference/IMG_0205.jpeg). Portrait, single column: Location/Date controls at top, Temp/Condition, character illustration (fills most of the screen), written clothing recommendation below it, a Reminder callout, and an (i) info button at the bottom for one-handed thumb reach.

**Main screen — laptop:** [IMG_0204.jpeg](reference/IMG_0204.jpeg). Landscape, uses the wider screen: same top bar and character illustration, but the Reminder is placed beside the character instead of stacked below it, and a 7-day forecast strip (tap a date to preview) runs alongside — content the phone layout doesn't show. Info button in the corner.

**Info screen — phone and laptop:** [IMG_0206.jpeg](reference/IMG_0206.jpeg). One layout reused for both sizes (simple scrollable text stack, no responsive restructuring needed): Back button and "Info" label at top, then creator credit, weather-data source (Open-Meteo), a brief explanation of how recommendations work (temp bands, UV, PoP, heat index) with a link to the methodology, a privacy statement (location saved on-device only), and art credits (AI-generated).

## Requirements

Translate every fixed brief requirement and the selected research-driven feature into a testable requirement. Define the chosen behavior, content, controls, current and forecast data, responsive layout, accessibility, error handling, privacy, credits, and deployment. The main screen should make clear the location, date, units, data source, and whether conditions are current or forecast. Include an acceptance check for each requirement.

**Location & date**
1. User can enter a US location manually (city/zip) or use device geolocation (requested only on explicit user action, never on page load).
2. Only the most recently used location is saved (on-device, e.g. localStorage) — no server storage, no history of past locations.
3. User can select "today" or any forecast date up to 7 days out; dates beyond that are not selectable.
*Acceptance: entering a location and reloading the page shows that same location pre-filled; selecting a date 8+ days out is not possible in the UI.*

**Recommendation state**
4. For a given location + date, one recommendation state (derived from Open-Meteo's temp, UV index, precipitation probability, and heat index) determines the character's outfit, the weather icon, the written recommendation text, and any reminders — all four always agree with each other.
*Acceptance: changing the date changes all four together; none can show a mismatched combination (e.g., winter coat with a "hot" written recommendation).*

**Content variation**
5. Each of the 4 temperature categories (Cold/Cool/Mild/Hot) has 3 outfit-art variations and 3 written-recommendation wordings, chosen independently at random.
6. Each reminder (umbrella/sunscreen/hydration) has 3 wording variations, chosen independently at random, and reminders only appear when their threshold is met (PoP ≥ 40%, UV ≥ 3, heat index ≥ 90°F).
7. Returning to a previously selected date (same session or after reload) shows the exact same outfit/wording/reminder variations as before — the random choice is seeded/stored per date+location, not re-rolled.
*Acceptance: selecting the same date twice (including after a page reload) never shows a different variation than the first time.*

**Info screen**
8. Info screen shows: creator name, weather-data source (Open-Meteo, with link), a plain-language explanation of the recommendation method (temp bands/UV/PoP/heat index) with a link to the cited sources in research.md, a privacy statement (location stored on-device only, never sent to a server), and art credits (AI-generated, tool disclosed).

**Error/loading/permission handling**
9. Loading state shown while fetching weather data.
10. If the weather API fails or returns no data, show a clear error message distinct from the loading state, with a retry option.
11. If geolocation permission is denied, show a message explaining manual entry is still available, and fall back to the manual-entry control — never a silent failure.

**Responsive & accessibility**
12. Phone layout matches IMG_0205 (one-handed, bottom-reachable controls); laptop layout matches IMG_0204 (uses extra width for the 7-day strip + side-by-side reminder).
13. All interactive elements are at least 44×44px with at least 3:1 contrast against their background (per WCAG guidance in research.md).

**Additional feature**
14. Double-tapping the character on the main screen reveals a hidden Halloween outfit variation, replacing the normal outfit art until the character is tapped again or the screen is reloaded.

**Deployment**
15. App is deployed to a public HTTPS URL via GitHub Pages, matching this approved spec.

## Recommendation state and data flow

Define the weather inputs, recommendation categories, coded rules, and shared state. Weather values must come from the provider, and rules must follow the weather guidance cited in `research.md`. The selected location, date, and live weather data must produce one recommendation state that drives every visual and written output.

## Content variation

Define at least three outfit variations for each recommendation category and at least three wording variations for each reminder type. Define how outfit and reminder variations are chosen independently at random, and how a previously selected date keeps the same variations, including whether they persist after the page reloads.

## Assets

List every art and graphical asset: the character, each outfit variation, icons, and any other visuals. For each, note where it appears, its format, and whether it will be created, generated, or licensed, with its credit or license.

## Out of scope

Record features intentionally excluded from this project.

## Revisions

After implementation or testing, record requirement changes and the evidence that prompted them. Update the screen drawings when a material layout or interaction changes.

## Approval

The Developer reviews and explicitly approves this specification and its screen designs before planning begins.

## Saving the transcript

After the Developer approves the specification, ask them to enter `save transcript`. When directed, save the complete conversation as `transcripts/spec-YYYY-MM-DD_HHMMSS.md`, label chat messages `Developer` and `Agent`, and confirm the saved path.
