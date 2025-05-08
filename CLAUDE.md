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

### Header and Navigation

- Changed the sticky header background from `bg-primary-100` to `bg-white` in light mode to improve text readability
- Improved the theme toggle button with smoother animations (added CSS classes and transitions)

### Contact Modal

- Added a contact modal system instead of navigating to sections
- All "Contact Us" and "Schedule a Free Consultation" buttons now open the modal instead of linking to page sections
- Modal includes proper form fields and animations
- Added JavaScript to handle modal interactions with multiple trigger buttons using class-based selectors
- Added CSS to ensure modal is properly centered with flex display
- Added smooth transitions with proper durations (500ms) for opening/closing animations

### Attribution and Footer

- Moved the "Made by Tarek Aloch" credit from a separate Announcement component to the Footer component
- Simplified display by separating the text and icon
- Removed the GitHub link to focus only on the personal website link
- Removed social links section from the Footer component
- Fixed TypeScript errors by removing inline comments from HTML attributes

### TypeScript Fixes

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
    - **`common/`**: Shared utility components
    - **`ui/`**: Basic UI elements
    - **`widgets/`**: Larger page sections
  - **`content/`**: Content configuration
  - **`data/`**: Blog posts and content (MDX/MD)
  - **`layouts/`**: Page layouts
  - **`pages/`**: Astro routes/pages
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
