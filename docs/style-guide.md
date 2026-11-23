# HostCircle — Style Guide

> **When:** Week 2, alongside the Spec · **Owner:** Matthew McCreary — Delivery Lead (whole team approves) · **Done when:** every item in the SVC Self-Check at the end is true.
>
> **Version:** 1.0 (Draft)
>
> **Approved on:** Pending **by:** Delivery Lead Matthew McCreary · Product Owner Jacob · Architecture Lead Kenneth Elmore
>
> **Filled-in Example:** `docs/examples/style-guide-example.md`

The Style Guide explains how HostCircle should look and function throughout the application. The goal is to create a consistent, easy-to-use design that follows WCAG 2.2 AA accessibility guidelines.

---

## 1. Brand & Voice

- **Brand principles:** HostCircle should feel friendly, welcoming, and organized. Since the application helps friends plan gatherings and share hosting responsibilities, the design should be simple enough for anyone to use without instructions. We want to avoid complicated menus, unnecessary features, and a formal business-like appearance.
- **Tone:** Casual, friendly, and straightforward. Instructions and messages should be short and easy to understand.
- **Button example:** Use "Create Gathering" instead of "Submit Event Information." Use "Add Contribution" instead of "Register Item."
- **Error example:** Use "Please select a date for your gathering" instead of "Invalid Input Error."
- **Logo:** None planned for the initial version. The HostCircle name will appear as text in the navigation area. Do not use different fonts or inconsistent colors for the application name.

---

## 2. Colors

HostCircle will use a simple green, white, and gray color scheme. Green represents a welcoming and positive environment, while neutral colors keep information readable and organized.

| **Name** | **Hex** | **Use** | **Contrast vs. background** |
|---|---|---|---|
| Primary | `#166534` | Main brand color, primary buttons, active links | 7.13:1 |
| Text | `#1F2937` | Body text and headings | 14.68:1 |
| Background | `#FFFFFF` | Main page background | — |
| Secondary Background | `#F1F5F9` | Cards and grouped content areas | 1.10:1 (background only) |
| Error | `#B91C1C` | Error messages and invalid fields | 6.47:1 |
| Border | `#64748B` | Borders of form fields, buttons, and cards | 4.76:1 |

All ratios are calculated against the white page background.

The primary, text, error, and border colors meet the required contrast levels. The secondary background is only used for background surfaces, not text or important control boundaries.

White text on primary green buttons provides a 7.13:1 contrast ratio.

Colors should be used consistently. Important information should also include words or labels rather than relying on color alone.

---

## 3. Type & Spacing

HostCircle will use Arial with a sans-serif fallback font. This is a common font that is easy to read on computers, tablets, and phones.

| **Role** | **Font** | **Size** | **Weight** |
|---|---|---|---|
| Heading 1 | Arial | 28px | Bold (700) |
| Heading 2 | Arial | 22px | Bold (700) |
| Body | Arial | 16px | Regular (400) |

- **Spacing scale:** 8 / 16 / 24 / 32 / 48px, using an 8px base unit.
- **Max content width:** 1200px.
- **Text alignment:** Left-aligned for most text to make reading easier.
- **Line spacing:** Body text will use a line height of 1.5 for readability.

The application will use the same spacing scale throughout. Smaller spaces will separate related items, while larger spaces will separate different sections.

On smaller screens, content should move into a single-column layout so users do not have to scroll horizontally.

---

## 4. Components

HostCircle will use reusable components so that all pages look and behave consistently.

Every button, link, and form field must be accessible using a keyboard and show a visible focus outline. Interactive elements will use a 3px dark green focus outline with a 2px offset.

| **Component** | **Rules** |
|---|---|
| **Button (primary)** | Dark green (`#166534`) background with white text. Used for main actions such as "Create Gathering" or "Save Changes." Buttons have rounded 8px corners and a minimum height of 44px. On hover, the button becomes slightly darker. Keyboard focus shows a visible outline. Disabled buttons use a light gray background and are not clickable. If an action fails, an error message appears near the button. |
| **Button (secondary)** | White background, dark green text, and a dark green border. Used for actions such as "Cancel" or "Back." On hover, the background changes to light gray. Keyboard focus uses the same visible outline. Disabled buttons are visibly inactive. If an action fails, a text error message explains the problem. |
| **Card** | Cards display gatherings, group information, or member contributions. They use a light gray (`#F1F5F9`) background, a 1px gray border, 8px rounded corners, and 16px internal spacing. Interactive cards show a stronger border on hover and a visible focus outline. Disabled cards display an "Unavailable" label. Cards containing errors show a clear explanation in red text. Noninteractive cards do not change on hover or receive keyboard focus. |
| **Form field** | Fields have white backgrounds, gray borders, and clearly visible labels above each input. Fields use 16px text and a minimum height of 44px. On hover, the border becomes dark green. On focus, a visible outline appears. Disabled fields use light gray backgrounds and cannot be edited. Invalid fields receive a red border and an error message explaining what needs to be corrected. |
| **Navigation** | Navigation remains consistent across pages and includes Dashboard, Groups, Gatherings, and Profile. Links use dark gray text, with the active page identified using dark green text and an underline. On hover, links become underlined. Keyboard focus uses the same visible outline. Unavailable pages are not clickable and are labeled "Coming Soon" if shown. Navigation errors display a readable message without removing access to other pages. On smaller screens, navigation adjusts to fit without horizontal scrolling. |

**Additional Component Rules**

- Buttons must use clear action words so users understand what will happen when selecting them.
- Forms must display confirmation messages when information is successfully saved.
- Required fields must be clearly labeled.
- Error messages must explain what went wrong and how the user can correct it.
- Disabled controls should remain readable and visibly different from available controls.
- The same component styles should be used throughout the application.

---

## SVC Self-Check

- [x] **Simple** — 3–7 colors, no more than 2 font families, one spacing scale.
- [x] **Valuable** — Every rule would change how a screen gets built; the brand principles actually rule things out.
- [ ] **Complete** — Every section is filled or marked "N/A because…"; every color meets its contrast target; components include hover, focus, disabled, and error states; the whole team has approved it.

**Pending:** Final review and approval from the Product Owner, Architecture Lead, and Delivery Lead.
