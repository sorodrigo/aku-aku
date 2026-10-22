---
name: design-roast
description: Critique UI against user needs, business goals and the app's brand using the frontend-design plugin. Return useful design findings with images that show possible improvements. Use when asked for a design roast or called by a review workflow.
---

# Design roast

Read and use the installed `frontend-design:frontend-design` skill. Locate its installed entrypoint rather than relying on a fixed cache version. If unavailable, report the missing dependency; do not claim the roast ran. Use its design guidance for critique. This task does not call for building or changing the app.

Gather the UI under review, the task's business goal, who uses it and what they need to achieve. Read nearby screens, shared components, styles and brand rules to learn the app's design patterns. Use supplied images or existing previews when available. State gaps in the evidence; do not treat guesses about the rendered UI as facts.

Judge function before form. Check whether people can understand the screen, find the main action, complete their task and recover from mistakes. Consider content, navigation, accessibility, small screens and relevant loading, empty, success and error states. A polished screen that misses the task's goals is a poor design.

Then judge visual quality and fit. Aim for an attractive, fun, modern startup feel that belongs in this app. Respect its brand, shared controls and familiar patterns. Do not propose a new visual identity just to make the screen stand out, or add decoration that makes the task harder.

Be candid about weak work and specific about its effect. For each useful finding, give the location, evidence, user or business cost, and smallest useful improvement. Separate clear harm from taste or uncertain benefits. Return no findings when the design already serves its purpose well; do not invent improvements to fill a review.

For each finding worth acting on, include an image that explains the proposed improvement in the app's visual language. Use the available image-generation skill or tool; an approximate concept is enough. Keep the image focused on the concern, with realistic content and enough surrounding UI to make it clear. Do not run or edit the app just to create it. Label generated images as concepts, not screenshots or proof of the current UI, and explain what each shows. If image generation is unavailable, return that limit with the finding.

Return findings and images to the caller. Leave code unchanged. The caller decides what to share and handles any posting; this skill does not post comments or make approval decisions.
