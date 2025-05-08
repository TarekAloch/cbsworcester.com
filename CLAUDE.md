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

## Recent Project Improvements

### 2025-05-17 19:15:22: View Transitions Consistency Overhaul

We've completed a comprehensive overhaul of the site's component integration with Astro's View Transitions, fixing several critical issues:

1. **Header Component Standardization:** ✅ FIXED
   - **Original issue**: Homepage used a custom-built header while other pages used the `CustomHeader.astro` component
   - **Root cause**: The initial design started with inline header HTML in `index.astro` instead of using the reusable component
   - **Solution implemented**:
     - Replaced the entire custom-built header HTML in `index.astro` (60+ lines of code) with the standardized `<CustomHeader />` component
     - Added proper import statement: `import CustomHeader from '~/components/widgets/CustomHeader.astro';`
     - Correctly applied the `transition:replace` directive directly to the component: `<CustomHeader transition:replace />`
     - Removed misplaced `transition:replace` attribute from the parent div
   - **Impact**: 
     - Consistent header component usage across all pages
     - Proper View Transitions handling between pages
     - Eliminated TypeScript errors related to incorrect directive usage
     - Significantly reduced code duplication (60+ lines of HTML replaced with a single component)
     - Improved maintainability as header changes can now be made in a single component

### 2025-05-17 18:30:45: Theme Toggle Styling Fix Implemented

We've successfully resolved the theme toggle styling issue:

1. **Theme toggle styling issue:** ✅ FIXED
   - **Original issue**: Theme toggle button in `CustomHeader.astro` on Terms/Privacy pages was missing animation/styles
   - **Root cause**:
     - The theme toggle CSS was only defined in `index.astro` and not available globally
     - The animation styles for the sun/moon icons weren't applied on pages using the custom header
   - **Solution implemented**:
     - Moved all theme toggle CSS from `src/pages/index.astro` to `src/components/CustomStyles.astro`
     - This makes the styles globally available across all pages since CustomStyles is imported in the base Layout
     - Preserved all the detailed animations and transitions for a consistent experience
     - Fixed layout issue on the Terms and Privacy pages where the modal hydration directive was causing errors
   - **Impact**: Theme toggle now has consistent styling and animations across all pages in the site
   - **Technical details**:
     - Before the fix: The theme toggle worked functionally but without animations on Terms/Privacy pages
     - After the fix: The sun/moon toggle smoothly animates and has proper hover states throughout the site

### 2025-05-17 16:42:37: Contact Modal Fix Implemented

We've successfully resolved the contact modal functionality issue:

1. **Contact modal functionality:** ✅ FIXED
   - **Original issue**: Modal buttons with class `js-open-contact-modal` not working consistently when navigating between pages
   - **Solution implemented**:
     - Completely refactored the modal JavaScript in `ContactModal.astro` to use a centralized listener management approach:
       - Moved all event listener setup inside the `astro:page-load` event handler
       - Created a dedicated `setupAllModalListeners()` function to handle all listener management
       - Changed from attaching listeners to `body` to attaching them directly to `document`
       - Implemented proper listener cleanup and reattachment on each page load
       - Added explicit handler functions with proper type annotations
       - Removed global window flags in favor of local flag management
     - The refactoring ensures all listeners are properly established after each View Transition
     - Extensive testing confirms modal works reliably in all navigation scenarios

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
   - **Status**: ✅ FIXED on 2025-05-17 with the styling changes (see solution above)

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
   - **Status**: ✅ FIXED on 2025-05-17 (see solution above)

### Project Status

1. **All Major Issues Resolved** ✅
   - The site now functions properly with Astro's View Transitions
   - Theme toggle works consistently with animations across all pages
   - Contact modal buttons function reliably regardless of navigation sequence
   - Standardized components used throughout the site
   - CSS is properly organized for global access
   - No TypeScript errors or console warnings

2. **Future Enhancements** (Optional):
   - Consolidate theme toggle implementations (consider using only the standard `ToggleTheme.astro` component)
   - Further optimize View Transitions with transition:animate directives for smooth animations
   - Add loading states to the contact form
   - Consider implementing skeleton loading states for content during transitions

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
- Modified `src/pages/index.astro`: 
  - Added id="contact" to CTA section
  - Replaced custom header HTML with `<CustomHeader transition:replace />` component
  - Added proper import: `import CustomHeader from '~/components/widgets/CustomHeader.astro'`
  - Moved theme toggle CSS to CustomStyles.astro for global availability
- Modified `src/layouts/MarkdownLayout.astro`: 
  - Added transition:replace to CustomHeader
  - Fixed ContactModal hydration directive issue (removed client:idle)
- Modified `src/components/widgets/ContactModal.astro`: Complete rewrite of JavaScript with centralized event delegation
- Modified `src/components/CustomStyles.astro`: Added global theme toggle animation styles
