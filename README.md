<h1 align="center">Laravel With Tailwindcss Config</h1>

<p align="center">
  <i>Step by step setting up laravel App with Tailwind CSS</i>
  <br>
</p>

<hr>

## Table of Contents
1. [Creating Project](#1-creating-project)
    * [Create new laravel Application](#create-new-laravel-application)
    * [Install Laravel’s front-end dependencies using `npm`](#install-laravels-front-end-dependencies-using-npm)
2. [Setting up Tailwind CSS](#2-setting-up-tailwind-css)
    * [Install Tailwind and its peer-dependencies using `npm`](#install-tailwind-and-its-peer-dependencies-using-npm)
    * [Generate `tailwind.config.js` file](#generate-tailwindconfigjs-file)
    * [Configure Tailwind to remove unused styles in production](#configure-tailwind-to-remove-unused-styles-in-production)
    * [Configure Tailwind with Laravel Mix](#configure-tailwind-with-laravel-mix)
    * [Include Tailwind in CSS](#include-tailwind-in-css)
3. [Finishing](#3-finishing)

<hr>

## 1. Creating Project

### Create new laravel Application
```bash
laravel new my-project
```
```bash
cd my-project
```

### Install Laravel’s front-end dependencies using `npm`
```bash
npm install
```

## 2. Setting up Tailwind CSS

*Tailwind CSS requires Node.js 12.13.0 or higher.*

### Install Tailwind and its peer-dependencies using `npm`
```bash
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

### Generate `tailwind.config.js` file
```bash
npx tailwindcss init
```

This will create a minimal tailwind.config.js file at the root project
```javascript
// tailwind.config.js
module.exports = {
    purge: [],
    darkMode: false, // or 'media' or 'class'
    theme: {
        extend: {},
    },
    variants: {
        extend: {},
    },
    plugins: [],
}
```

### Configure Tailwind to remove unused styles in production
In `tailwind.config.js` file, configure the `purge` option with the paths to all of your Blade templates and JavaScript components so Tailwind can tree-shake unused styles in production builds:
```javascript
// tailwind.config.js
module.exports = {
    // ...
    purge: [
        './resources/**/*.blade.php',
        './resources/**/*.js',
        './resources/**/*.vue',
    ],
    // ...
}
```

### Configure Tailwind with Laravel Mix
In `webpack.mix.js`, add `tailwindcss` as a PostCSS plugin
```javascript
// webpack.mix.js
mix.js("resources/js/app.js", "public/js")
    .postCss("resources/css/app.css", "public/css", [
        require("tailwindcss"),
]);
```

### Include Tailwind in CSS
Open the `./resources/css/app.css` file that Laravel generates for you by default and use the `@tailwind` directive to include Tailwind’s `base`, `components`, and `utilities` styles, replacing the original file contents
```css
/* ./resources/css/app.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Import stylesheet in main Blade layout (commonly `resources/views/layouts/app.blade.php` or similar) and add the responsive viewport meta tag if it’s not already present
```html
<!doctype html>
<head>
    <!-- ... --->
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link href="{{ asset('css/app.css') }}" rel="stylesheet">
    <!-- ... --->
</head>
```

## 3. Finishing
Now when you run `npm run watch` or `npm run dev` or `npm run prod`, Tailwind CSS will be ready to use in Laravel Mix project.
