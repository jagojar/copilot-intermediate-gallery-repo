# Photo Gallery App Documentation

This document explains how the application under `/tmp/workspace/jagojar/copilot-intermediate-gallery-repo/src` is organized so contributors can quickly understand where features live and how the app is assembled.

## Framework stack

The app is built with:

- **Next.js 15** using the **App Router**
- **React 19** for component-driven UI
- **TypeScript** for typed pages, components, and data modules
- **Tailwind CSS 4** for styling and reusable design utilities

The app also uses supporting libraries that show up across the UI, including `lucide-react` for icons, `framer-motion` for animations, and `react-dropzone` for the upload flow.

## `/src` folder structure

```text
src/
├── app/         # Route segments, root layout, and page entrypoints
├── components/  # Reusable UI building blocks and feature components
│   ├── gallery/ # Gallery-specific presentation and interactions
│   ├── ui/      # Shared layout, card, and stats primitives
│   └── upload/  # Upload-specific drag-and-drop experience
└── lib/         # Mock data modules and supporting app data
```

## How routing works in `src/app`

`src/app` follows the Next.js App Router convention where folders define URL segments and `page.tsx` files define the UI for each route:

- `src/app/page.tsx` renders the home page at `/`
- `src/app/gallery/page.tsx` renders the gallery browsing experience at `/gallery`
- `src/app/upload/page.tsx` renders the upload workflow at `/upload`
- `src/app/admin/page.tsx` renders the admin dashboard at `/admin`

`src/app/layout.tsx` is the shared root layout. It wraps every route with global metadata, fonts, navigation, and shared page chrome, so individual pages can focus on feature composition instead of repeating shell markup.

## Shared UI components in `src/components/ui`

`src/components/ui` contains the reusable presentation primitives that keep route files small and consistent:

- `Hero` renders the large page heading and description used across pages
- `SectionContainer` standardizes spacing and section width
- `SectionTitle` provides section headings and optional “View All” links
- `FeatureCard` displays icon + title + description cards for feature summaries
- `StatsGrid` renders dashboard statistics in a shared card layout

These components are exported through `src/components/ui/index.ts`, which lets pages import multiple shared building blocks from one place.

## Feature-specific components

Feature folders keep interactive or domain-specific UI separate from the shared primitives:

- `src/components/gallery/GalleryGrid.tsx` is the main gallery experience. It reads mock photo data, applies search/tag filtering, handles simple pagination, and renders the photo card grid and modal placeholder.
- `src/components/upload/UploadZone.tsx` owns the drag-and-drop upload interaction. It uses `react-dropzone`, tracks local upload progress, and shows preview cards for selected files.

Pages in `src/app` compose these feature components with shared UI wrappers. For example, the home page combines `Hero`, `FeatureCard`, `UploadZone`, and `GalleryGrid`, while the dedicated gallery and upload routes expand those feature areas into full-page workflows.

## Purpose of `src/lib`

`src/lib` is currently a **mock-data-driven** layer for the demo app. Instead of calling a live backend, route and feature components import seeded data directly from modules such as:

- `mock-photo-data.ts` for gallery items
- `mock-feature-card-data.ts` for homepage feature cards
- `mock-admin-data.ts` for dashboard stats and recent galleries
- `mock-tag-data.ts` for filter tags

This approach keeps the demo self-contained and makes it easy to prototype UI states, filtering, and composition patterns without introducing API or database dependencies.

## Overall architecture

The app uses a simple composition-first architecture:

1. **App Router pages** in `src/app` define route-level screens.
2. **Shared UI primitives** in `src/components/ui` provide consistent layout and presentation.
3. **Feature components** in folders like `gallery` and `upload` implement interactive page sections.
4. **Mock data modules** in `src/lib` feed those components with predictable demo content.

In practice, pages act as assembly points. They import shared sections and feature components, pass in any route-specific props, and rely on `src/lib` data to populate the UI. That keeps routing concerns, reusable presentation, feature behavior, and sample content separated while still making the app easy to understand for contributors.
