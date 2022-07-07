# Dev Note - Next.js

## Initial Project

```bash
# Have a single command without worrying about setting or using the environment variable properly for the platform.
npm install cross-env --save
```

## Change port

```json
"scripts": {
  "dev": "cross-env port=3261 next dev",
}
```

## Tailwind CSS

### Install

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

tailwind.config.js

```javascript
module.exports = {
  content: [
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### Multi theme

npm install next-themes --save
