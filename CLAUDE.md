# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a website for "Complete Billing Solutions" (CBS), a medical billing service specializing in mental health providers in Worcester. The site promotes their high collection rate (96-98%) compared to industry standards (75%).

The project is built with:

- **Astro 5.0**: Static site generator
- **Tailwind CSS**: For styling
- **TypeScript**: For type safety
- **MDX**: For enhanced Markdown content

## Recent Changes

### 2025-05-13: Code Architecture Improvements

1. **Component Refactoring:**

   - Created a dedicated `ContactModal.astro` component to encapsulate the contact form modal
   - Created `CustomHeader.astro` component for consistent site-wide navigation
   - Updated page layouts to use these new components

2. **Content Updates:**

   - Added comprehensive Terms and Conditions page with proper legal content
   - Added detailed Privacy Policy page with proper legal content
   - Updated Formspree endpoints for form submissions

3. **Navigation Improvements:**
   - Standardized navigation across all pages using CustomHeader component
   - Modified navigation links on secondary pages to point back to homepage sections
   - Converted Contact Us links to buttons with modal trigger functionality

### 2025-05-08: Accessibility & Style Improvements

1. **Accessibility Improvements:**

   - Converted interactive links to proper `<button>` elements in the following locations:
     - Hero section CTA button (changed `href` to `type: 'button'`)
     - "Contact Us" button in CTA section
     - "Schedule Consultation" button in CTA section
     - Floating Action Button
   - Added appropriate `aria-label="Visit Tarek Aloch's homepage"` to the footer icon link

2. **Styling Consistency:**

   - Updated button styles for visual consistency
   - Applied gold accent color to CTA buttons on hover
   - Improved focus states for keyboard navigation
   - Standardized transition animations across interactive elements

3. **Modal Improvements:**

   - Refactored modal JavaScript to work with Astro's View Transitions
   - Implemented event delegation for more reliable event handling
   - Added proper TypeScript type annotations (HTMLElement | null, KeyboardEvent, MouseEvent)
   - Improved error handling for modal initialization
   - Added support for astro:page-load event to work with Astro's View Transitions

4. **Content Updates:**

   - Updated "Years in Business" statistic from "20+" to "25+"

5. **Footer Optimization:**
   - Removed unused column links from footer for cleaner appearance
   - Preserved detailed documentation in comments for future reference

### Earlier Changes

#### Header and Navigation

- Changed the sticky header background from `bg-primary-100` to `bg-white` in light mode to improve text readability
- Improved the theme toggle button with smoother animations (added CSS classes and transitions)

#### Contact Modal

- Added a contact modal system instead of navigating to sections
- All "Contact Us" and "Schedule a Free Consultation" buttons now open the modal instead of linking to page sections
- Modal includes proper form fields and animations
- Added JavaScript to handle modal interactions with multiple trigger buttons using class-based selectors
- Added CSS to ensure modal is properly centered with flex display
- Added smooth transitions with proper durations (500ms) for opening/closing animations

#### Attribution and Footer

- Moved the "Made by Tarek Aloch" credit from a separate Announcement component to the Footer component
- Simplified display by separating the text and icon
- Removed the GitHub link to focus only on the personal website link
- Removed social links section from the Footer component
- Fixed TypeScript errors by removing inline comments from HTML attributes

#### TypeScript Fixes

- Fixed TypeScript warning in `src/utils/images.ts` by properly typing the `_image` variable

## Development Commands

| Command                  | Description                              |
| ------------------------ | ---------------------------------------- |
| `npm install`            | Install dependencies                     |
| `npm run dev`            | Start dev server at `localhost:4321`     |
| `npm run build`          | Build production site to `./dist/`       |
| `npm run preview`        | Preview build locally                    |
| `npm run check`          | Run all checks (astro, eslint, prettier) |
| `npm run check:astro`    | Check Astro types                        |
| `npm run check:eslint`   | Run ESLint                               |
| `npm run check:prettier` | Check code formatting                    |
| `npm run fix`            | Fix all issues (eslint, prettier)        |
| `npm run fix:eslint`     | Fix ESLint issues                        |
| `npm run fix:prettier`   | Fix formatting issues                    |

## Project Architecture

### Core Structure

- **`src/`**: Main source code
  - **`assets/`**: Static assets (images, styles)
  - **`components/`**: Reusable Astro components
    - **`blog/`**: Blog-specific components
    - **`common/`**: Shared utility components like theme toggles
    - **`ui/`**: Basic UI elements
    - **`widgets/`**: Larger page sections
      - **`ContactModal.astro`**: Contact form modal component
      - **`CustomHeader.astro`**: Shared site navigation header
  - **`content/`**: Content configuration
  - **`data/`**: Blog posts and content (MDX/MD)
  - **`layouts/`**: Page layouts
    - **`Layout.astro`**: Base HTML structure with head elements
    - **`PageLayout.astro`**: Main layout with structured page sections
    - **`MarkdownLayout.astro`**: Layout for markdown content pages
  - **`pages/`**: Astro routes/pages
    - **`index.astro`**: Main homepage
    - **`terms.md`**: Terms and conditions page
    - **`privacy.md`**: Privacy policy page
  - **`utils/`**: Helper functions

### Key Configuration Files

- **`src/config.yaml`**: Main site configuration
- **`astro.config.ts`**: Astro build configuration
- **`tailwind.config.js`**: Tailwind CSS configuration
- **`src/components/CustomStyles.astro`**: Brand colors and typography

## Theming and Customization

### Brand Colors

```css
--aw-color-primary: #0057b8; /* Deep Blue */
--aw-color-secondary: #00a0df; /* Light Blue */
--aw-color-accent: #ffb81c; /* Gold - for CTAs */
```

### Typography

- **Headings**: Montserrat (font-heading)
- **Body**: Open Sans (font-body)

## Deployment

The site can be deployed on:

- **Vercel**: Configuration in `vercel.json`
- **Netlify**: Configuration in `netlify.toml`

Build output is static HTML (`output: 'static'` in astro.config.ts).

## Performance Optimizations

- **Image Optimization**: Using Astro's built-in image processing
- **CSS/JS Compression**: Via astro-compress
- **Icon Optimization**: Using astro-icon
- **Lazy Loading**: For images and components

## SEO Features

- **Metadata**: Configured in `src/config.yaml`
- **Sitemap**: Generated automatically
- **Open Graph**: Default images and metadata
- **Robots.txt**: Configured for proper indexing

## Current Work In Progress

### 2025-05-15: Cross-Page Component Integration Improvements

We've been working on two significant issues related to site-wide component integration and Astro's View Transitions:

1. **Duplicate theme toggle issue:** 
   - **Issue**: Duplicate sun/moon toggle appearing on the terms and privacy policy pages
   - **Root cause**: Multiple instances of the theme toggle functionality loading simultaneously - both the custom implementation in `CustomHeader.astro` and potentially the default `ToggleTheme.astro` component elsewhere
   - **Technical analysis**: 
     - Both toggles use the same data attribute `data-aw-toggle-color-scheme` 
     - The `BasicScripts.astro` component initializes all elements with this attribute
     - When both are present on the same page, both get initialized and are visible
   - **Attempted fix**: Added `transition:replace` directive to `CustomHeader` component in MarkdownLayout.astro to ensure proper replacement during View Transitions
   - **Status**: Issue still persists and requires further debugging

2. **Contact modal functionality:** A complex issue with multiple facets:
   - **Original issue**: Modal buttons with class `js-open-contact-modal` not working consistently when navigating between pages
   - **Behavioral analysis**: Detailed logs show that the problem occurs after specific navigation sequences (e.g., Terms → Home via logo link → click modal button)
   - **Root cause investigation**:
     - Using browser devtools confirms the modal HTML exists on all pages after our changes
     - Event listeners appear to be attached correctly according to console logs
     - Specific navigation patterns seem to create a disconnect between the delegated click listener and the modal element references
     - Likely related to how Astro's View Transitions preserves/replaces DOM elements
   - **Implemented changes**:
     - Changed "Contact Us" links in header to standard anchor links (`<a href="/#contact">`) instead of modal triggers
     - Added `id="contact"` attribute to the CTA section on the homepage for anchor link targets
     - Added `transition:replace` directive to header elements for proper View Transitions
     - Added form submission handler to handle AJAX submissions with proper feedback
     - Completely rewrote the ContactModal.astro JavaScript with a robust event delegation approach:
       - Created separate delegated listeners for open buttons, close button, overlay clicks, and ESC key
       - Used global window flags to ensure listeners are attached exactly once
       - Implemented detailed logging for debugging
       - Simplified `astro:page-load` handler to only update element references
       - Added TypeScript declarations for window properties
       - Added `findAndSetModalElements()` function that is called on every click to ensure fresh element references
   - **Status**: Some buttons still not working properly after certain navigation sequences
   
### Next Steps

1. **For theme toggle issue**:
   - Replace custom theme toggle implementation in CustomHeader.astro with standard ToggleTheme component
   - OR modify theme initialization to conditionally control which toggle is active
   - OR add a unique attribute to one implementation and modify BasicScripts.astro to target only one type

2. **For contact modal issue**:
   - Further debugging of element references and event binding after View Transitions
   - Consider using a more global approach like a custom element or Astro island with `client:only` directive
   - Ensure form submission handler is properly reinitialized after page transitions
   - Possible solution: Move modal to the Layout.astro base component to ensure it's always present
   - Alternative: Implement as a web component for better encapsulation across page transitions

Both issues relate to how components behave across Astro's View Transitions, particularly when identical or similar functionality exists in multiple components or when JavaScript needs to maintain state and event bindings across page navigation. The challenges highlight the complexity of managing stateful interactions in a hybrid static/dynamic site with client-side transitions.

**Key Files for Review:**
- `src/components/widgets/CustomHeader.astro`: Contains site-wide navigation with theme toggle implementation
- `src/components/common/ToggleTheme.astro`: Standard theme toggle component
- `src/components/common/BasicScripts.astro`: Contains the theme toggle initialization logic (line ~67)
- `src/components/widgets/ContactModal.astro`: Contains modal HTML, CSS and JavaScript with event delegation
- `src/layouts/MarkdownLayout.astro`: Layout for content pages (Terms & Privacy) that use CustomHeader
- `src/layouts/Layout.astro`: Base layout that includes BasicScripts.astro
- `src/pages/index.astro`: Homepage with contact section and custom header implementation

**File Changes Made:**
- Modified `src/components/widgets/CustomHeader.astro`: Changed Contact Us buttons to anchor links
- Modified `src/pages/index.astro`: Added id="contact" to CTA section
- Modified `src/layouts/MarkdownLayout.astro`: Added transition:replace to CustomHeader
- Modified `src/components/widgets/ContactModal.astro`: Complete rewrite of JavaScript with event delegation
