# student reviewer notes

## Architecture

This is a minimal React 19 single-page application built with Vite and TypeScript. `src/main.tsx` bootstraps the app under `StrictMode`; `src/App.tsx` contains the complete landing page, while `App.css` and `index.css` provide styling. There is no router, backend, state-management library, or API integration in the sampled code.

## Conventions

- Use functional React components and hooks; `App` uses `useState`, and the reusable `Icon` component is defined in `src/App.tsx`.
- Keep TypeScript types close to their usage. For example, `IconName` restricts valid icon names and `Icon` accepts `{ name: IconName }`.
- Prefer typed DOM event handlers, as shown by `handleSubmit(event: FormEvent<HTMLFormElement>)` in `src/App.tsx`.
- Repeated display content is represented as data and rendered with `.map()`. The `features` tuple array is mapped into `.feature-card` articles using `title` as the React key.
- Icons are local inline SVGs selected through the `paths: Record<IconName, string>` map; avoid adding an icon dependency for simple icons without a clear need.
- Use semantic and accessible HTML: navigation has `aria-label`, the decorative SVG has `aria-hidden`, the form uses associated labels, and submission feedback uses `role="status"` (`src/App.tsx`).
- Files use ES modules and omit semicolons. The project is configured with `"type": "module"` in `package.json`; Vite configuration and ESLint configuration use `import` syntax.
- Run project checks through the scripts in `package.json`: `npm run lint` for ESLint and `npm run build` for TypeScript plus the Vite production build.

## Intentional non-standard choices

- The page is intentionally implemented as one large `App` component with inline JSX rather than split into many files; the current sample is a small static marketing template.
- The contact form intentionally performs no network request. `handleSubmit` prevents the browser submission and only displays a local success message after native constraint validation passes (`src/App.tsx`).
- Anchor links (`#home`, `#about`, `#features`, `#contact`) are used instead of client-side routing because this is a single-page landing page.

## Watch out for

- Do not add feature entries with icon names outside the `IconName` union; this will break the typed icon lookup in `src/App.tsx`.
- Preserve stable, unique React keys when changing the `features.map()` rendering; the current implementation keys by `title`.
- Avoid removing native form attributes (`required`, `type="email"`) or `event.currentTarget.checkValidity()`, since they provide the current validation behavior.
- New browser globals or non-TS files may not receive the same ESLint coverage: the flat config targets `**/*.{ts,tsx}` and only declares browser globals (`eslint.config.js`).
- Changes to the contact form should not imply messages are actually delivered unless a backend or API integration is added; current behavior is only visual feedback.