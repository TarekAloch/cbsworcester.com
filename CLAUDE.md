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

We are currently addressing two issues related to the site-wide component integration:

1. **Duplicate theme toggle issue:** There appears to be a duplicate sun/moon toggle appearing on the terms and privacy policy pages, likely due to multiple instances of the theme toggle functionality being loaded.

2. **Contact modal functionality:** The "Contact Us" button in the CustomHeader component doesn't properly trigger the contact modal when clicked from pages other than the homepage. This is likely because:
   - The modal HTML might only exist on the homepage
   - Event listeners for the modal buttons may not be properly attached on all pages
   - JavaScript initialization might need to be moved to a global script

The next phase of work will focus on ensuring consistent theme toggling and making the contact modal work properly across all pages of the site.
