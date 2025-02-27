---
# You can also start simply with 'default'
theme: "default"
fonts:
  sans: "Epilogue"
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: "assets/main-cover.webp"
# some information about your slides (markdown enabled)
title: nantes.rb | inertia x rails = 🔥
info: |
  ## Basics of inertia.js used in a rails app
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# inertia x rails = 🔥

<div class="white">
  How to build modern monolithic SPAs 🤯
</div>

<div @click="$slidev.nav.next" class="mt-12 text-sm py-2 px-2 gap-1 rounded-md inline-flex items-center hover:bg-orange-500 hover:text-black hover:cursor-pointer">
  Let's start our exploration <carbon:arrow-right class="size-4"/>
</div>

<div class="abs-br m-6 inline-flex items-center gap-4">
  <div class="white">
    <a href="https://www.meetup.com/nantes-rb/events/305995476" class="text-orange-500 hover:text-orange-600!">nantes.rb</a> | 27/02/2025
  </div>

  <a href="https://www.stellaire.studio" target="_blank" class="border-b-0!">
    <img src="./assets/stellaire-logo.png" class="size-8">
  </a>
</div>

---
transition: fade-out
---

# Why is inertia.js great?
<div>
  <span v-click>It allows you to build </span>
  <span v-click class="font-bold">fully client-side rendered SPA,</span><span v-click class="text-orange-500 font-bold"> without the complexity.</span>
</div>

<div v-click class="mt-2">
  By leveraging existing server-side patterns that you already know & love.
</div>

<v-clicks class="mt-8">

  - No need to build REST/GraphQL APIs
  - No client-side routing required
  - No client-side state management headaches
  - No separate authentication/authorization system
  - No new deployment strategy
  - Write react/vue/svelte components while sticking to rails conventions

</v-clicks>

<div v-click class="mt-8">
  Think of inertia.js as the glue between your Rails backend and the JS frontend of your choice (react, vue, svelte..).
</div>

<div v-click class="mt-8">
  Allowing you to leverage the <span class="font-bold text-orange-500">JS native ecosystem</span> rails is <span class="font-bold text-orange-500">still missing out on</span>.
</div>

---
transition: fade-out
---

# What is inertia.js?

<div v-click class="mb-4">
  A new approach to building classic server-driven web apps
</div>

<v-clicks depth="2" every="1">

1. You build your app using Rails' existing patterns
  - Routes, controllers, middleware, auth, etc.
  - Everything stays the same except the view layer

</v-clicks>

<span class="mt-4"></span>

<v-clicks depth="2" every="1">

2. Instead of ERB templates, your views are JS components
  - React, Vue, or Svelte components
  - Full power of modern frontend frameworks

</v-clicks>

<span class="mt-4"></span>

<v-clicks depth="2" every="1">

3. The magic: client-side regular routing
  - Click an inertia powered `<Link>` or submit a form with inertia `<Router>` → intercepted, made via XHR
  - Server returns JSON (component name + props) instead of HTML
  - inertia swaps the page component & updates browser history

</v-clicks>

<span class="mt-4"></span>

<span v-click class="absolute rotate--8 top-48 right-8 bg-orange-500 text-white px-4 py-2 rounded-md">
  Smooth SPA experience 🎉
</span>

---
transition: fade-out
---

# Under the hood - First Visit
<div v-click class="mb-4">
  Regular <span class="font-bold text-orange-500">request</span> / response cycle rendering HTML
</div>

```http {hide|1|all}{lines:true, maxHeight: '20%'}
REQUEST
GET: http://showcase.stellaire.studio/startups
Accept: text/html, application/xhtml+xml
```

---
transition: fade-out
---

# Under the hood - First Visit
<div v-click class="mb-4">
  Regular request / <span class="font-bold text-orange-500">response</span> cycle rendering HTML
</div>

```http {hide|1|all}{lines:true, maxHeight: '20%'}
RESPONSE
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
```

```html {hide|2-6|8-31}{lines:true, wrap:true, maxHeight: '70%'}
<html>
  <head>
      <title>Startups showcase</title>
      <link href="/css/app.css" rel="stylesheet">
      <script src="/js/app.js" defer></script>
  </head>
  <body>
      <div id="app" data-page='{
        "component": "Startups/Index",
        "props": {
          "featured_startups": [
            {
              "id": 1,
              "name": "Astreinte Vitale",
              "description": "Because each second counts.",
              "preview_image_url": "/images/startups/1.jpg"
            },
            {
              "id": 2,
              "name": "kwali",
              "description": "The best after sale service you will ever give.",
              "preview_image_url": "/images/startups/2.jpg"
            }
          ],
          "filters": {
            "sort": "newest"
          }
        },
        "url": "/startups",
        "version": "c32b8e4965f418ad16eaebba1d4e960f"
      }'></div>
  </body>
</html>
```

<div class="mt-4 absolute top-20 right-14" v-click="'7'">
  <a href="https://inertia-rails.dev/guide/the-protocol#html-responses" class="text-orange-500 hover:text-orange-600!" target="_blank">Documentation</a>
</div>

---
transition: fade-out
---

# Under the hood - Subsequent Visits

<div v-click class="mb-4">
  inertia intercepts the request and returns a <span class="font-bold text-orange-500">JSON</span> response
</div>

```http {hide|1|all}{lines:true, maxHeight: '20%'}
REQUEST
GET: /startups/1
X-Inertia: true
X-Inertia-Version: c32b8e...
```

```http {hide|1|all}{lines:true, maxHeight: '20%'}
RESPONSE
HTTP/1.1 200 OK
Content-Type: application/json
```

```json {hide|all}{lines:true, maxHeight: '50%'}
{
  "component": "Startup",
  "props": {
    "startup": {
      "id": 1,
      "name": "Astreinte Vitale",
      "description": "Because each second counts.",
      "preview_image_url": "/images/startups/1.jpg",
      "team_members": [...]
    }
  },
  "url": "/startups/1",
  "version": "c32b8e..."
}
```

<div class="mt-4 absolute top-20 right-14" v-click="'7'">
  <a href="https://inertia-rails.dev/guide/the-protocol#inertia-responses" class="text-orange-500 hover:text-orange-600!" target="_blank">Documentation</a>
</div>

---
transition: fade-out
---

### Under the hood - Protocol flow

<br>

```mermaid {scale: 0.8, look: 'handDrawn', class: 'flex items-center justify-center'}
sequenceDiagram
  autonumber
    participant Browser
    participant Inertia
    participant Controller

    %% Initial Visit
    Browser->>Controller: GET /startups
    Controller-->>Browser: HTML + data-page

    %% Client-side Setup
    Note over Browser,Inertia: Inertia boots up<br/>frontend SPA mounts

    %% Subsequent Navigation
    Browser->>Inertia: Click <Link to="/startups/1">
    Inertia->>Controller: GET /startups/1
    Note over Inertia,Controller: X-Inertia: true
    Controller-->>Inertia: JSON response
    Inertia->>Browser: Swap component<br/>Update history
```

---
transition: fade-out
---

# Setting up inertia in rails

<v-clicks>

### 1. Server-side setup

```ruby
# Gemfile
gem 'inertia_rails'
gem 'vite_rails'
```

```bash
bin/rails generate inertia:install
```

This will:
- Set up vite.js for asset bundling
- Configure TypeScript (optional)
- Install Tailwind CSS (optional)
- Setup the rails app to work with inertia
- Set up example route, controller & view component

</v-clicks>

---
transition: fade-out
---

# Setting up inertia in rails

<v-clicks>

### 2. Client-side setup
```bash
# Install Inertia's React adapter
yarn add @inertiajs/react react react-dom
```

### 3. Initialize the inertia app
```js {hide|all|6-9|10-12|all}{lines:true, maxHeight: '100%'}
// app/frontend/entrypoints/application.js
import { createInertiaApp } from '@inertiajs/react'
import { createRoot } from 'react-dom/client'

createInertiaApp({
  resolve: (name) => {
    const pages = import.meta.glob('../pages/**/*.jsx', { eager: true })
    return pages[`../pages/${name}.jsx`]
  },
  setup({ el, App, props }) {
    createRoot(el).render(<App {...props} />)
  },
})
```

</v-clicks>

---
transition: fade-out
---

# Layout & controllers

<div class="grid grid-cols-2 gap-4">
<div>

```erb {hide|all}{lines:true, maxHeight: '100%'}
<%# app/views/layouts/application.html.erb %>
<!DOCTYPE html>
<html>
  <head>
    <title>Startups showcase</title>
    <%# If you want to use React add
        `vite_react_refresh_tag` %>
    <%= vite_client_tag %>
    <%= vite_javascript_tag 'application' %>
  </head>
  <body>
    <%= yield %>
  </body>
</html>
```

</div>
<div>

```ruby {hide|all|2-13|3-5|7-12|15-26|16-18|20-25|all}{lines:true, maxHeight: '90%'}
class StartupsController < ApplicationController
  def index
    startups = Startup.
      featured.
      with_attached_preview_image

    render inertia: 'Startups/Index', props: {
      featured_startups: startups.as_json(
        include: [:id, :name, :description],
        methods: [:preview_image_url]
      )
    }
  end

  def show
    startup = Startup.
      with_attached_preview_image.
      find(params[:id])

    render inertia: 'Startups/Show', props: {
      startup: startup.as_json(
        include: [:team_members],
        methods: [:preview_image_url]
      )
    }
  end
end
```

</div>
</div>

---
transition: fade-out
---

# Frontend components

<div v-click class="text-sm">
  w/ react
</div>

```jsx {hide|all|1|2|4|7|8-29|12,27|13-17,26|18-25|all}{lines:true, maxHeight: '90%'}
// app/frontend/Pages/Startups/Index.jsx
import { Head, Link } from '@inertiajs/react'

export default function Index({ featured_startups }) {
  return (
    <>
      <Head title="Our startups" />
      <div>
        <h1>Our startups</h1>

        <div className="grid ...">
          {featured_startups.map(startup => (
            <Link
              key={startup.id}
              href={`/startups/${startup.id}`}
            >
              <div>
                <img
                  src={startup.preview_image_url}
                  alt={startup.title}

                />
              </div>
              <h2>{startup.title}</h2>
            </Link>
          ))}
        </div>
      </div>
    </>
  )
}
```

---
transition: fade-out
---

# Batteries included


<div v-click class="font-bold mb-4">
  Smart <span class="text-orange-500">asset versioning</span>
</div>

<v-clicks>

```http
REQUEST:
GET: /startups/1
X-Inertia: true
X-Inertia-Version: [old-version]

RESPONSE:
409 Conflict
X-Inertia-Location: /startups/1
```

<a href="https://inertia-rails.dev/guide/the-protocol#asset-versioning" class="text-orange-500 hover:text-orange-600!" target="_blank">Documentation</a>

</v-clicks>

---
transition: fade-out
---

# Batteries included

<div v-click class="font-bold mb-4">
  Performance: <span class="text-orange-500">partial reloads</span>
</div>

<v-clicks>
```http
REQUEST
GET: /startups
X-Inertia: true
X-Inertia-Partial-Data: startups
X-Inertia-Partial-Component: Startups
```

```http
RESPONSE
Content-Type: application/json
```

```json
{
  "component": "Startups",
  "props": {
    "filters": [ ... ],  // NOT included
    "startups": [ ... ]  // included
  }
}
```

<a href="https://inertia-rails.dev/guide/the-protocol#partial-reloads" class="text-orange-500 hover:text-orange-600!" target="_blank">Documentation</a>

</v-clicks>

---
transition: fade-out
---

# Batteries included

<div v-click class="font-bold mb-4">
  More <span class="text-orange-500">goodies</span>
</div>

<v-clicks>

- Shared data across components
- Deferred props loading
- Link prefetching
- Classic rails html.erb views still work (perfect for devise out of the box setup)

</v-clicks>

---
transition: fade-out
---

# Additionnal batteries

<div v-click class="font-bold mb-4">
  Remember this <span class="text-orange-500">ugly thing..?</span>
</div>

```jsx {hide|all|1|7}{lines:true, maxHeight: '90%'}
// app/frontend/Pages/Startups/Index.jsx
// ...
<div className="grid ...">
  {featured_startups.map(startup => (
    <Link
      key={startup.id}
      href={`/startups/${startup.id}`}
    >
      <div>
        <img
          src={startup.preview_image_url}
          alt={startup.title}

        />
      </div>
      <h2>{startup.title}</h2>
    </Link>
  ))}
</div>
// ...
```

<span v-click class="text-4xl absolute top-60 left-100">
  🙊
</span>

---
transition: fade-out
---

# Additionnal batteries

<div v-click class="inline-block font-bold mb-4 me-2">
  There's a neat solution for this:
</div>
<a v-click href="https://js-from-routes.netlify.app/" target="_blank" class="text-orange-500 hover:text-orange-600!">JS from routes</a>

```jsx {hide|all|2|7-10|9}{lines:true, maxHeight: '90%'}
// app/frontend/Pages/Startups/Index.jsx
import { startups } from "@/path_helpers";

// ...
<div className="grid ...">
  {featured_startups.map(startup => (
    <Link
      key={startup.id}
      href={startups.show.path(startup)}
    >
      <div>
        <img
          src={startup.preview_image_url}
          alt={startup.title}

        />
      </div>
      <h2>{startup.title}</h2>
    </Link>
  ))}
</div>
// ...
```

<span v-click class="text-base absolute top-73 left-102">
  We're home now 🤗
</span>

---
transition: fade-out
---

# Start living <span class="text-orange-500 font-bold">your best frontend life</span>

<div v-click class="font-bold mb-4">
  Now you can focus on building great UIs:
</div>

<v-clicks>

- Leverage <a href="https://ui.shadcn.com/docs" target="_blank" class="text-orange-500 hover:text-orange-600!">shadcn/ui</a> slick & modern components
- Setup complex frontend interactions (slidesheet, drawer, etc)
- Never write boilerplate JS yourself ever again

</v-clicks>

---
transition: fade-out
---

# Start building apps <span class="text-orange-500 font-bold">at record speed</span>


<div class="space-y-1">
  <p class="text-white" v-click>Focus on what matters most.</p>
  <p v-click class="font-bold">Building your product features,</p>
  <p v-click>leveraging the full <span class="text-orange-500 font-bold">frontend native DX & UX,</span></p>
  <p v-click>right there within the <span class="text-orange-500 font-bold">comfy & efficient</span> monolith 🚀</p>
</div>

<div v-click class="mt-32 text-2xl text-center">
  Now, let's go and <span class="text-orange-500 font-bold">build something great</span> 🚀
</div>

---
transition: fade-out
---

# Noteworthy resources

## <span v-click class="font-bold">inertia</span>

<v-clicks>

- <a href="https://inertia-rails.dev/" target="_blank" class="text-orange-500 hover:text-orange-600!">Official docs - inertia-rails.dev</a>
- <a href="https://avohq.io/blog/inertia-js-with-rails" target="_blank" class="text-orange-500 hover:text-orange-600!">Blog AvoHQ - Inertia.js with Rails</a>
- <a href="https://evilmartians.com/chronicles/inertiajs-in-rails-a-new-era-of-effortless-integration" target="_blank" class="text-orange-500 hover:text-orange-600!">Blog Evil Martians - A new era of effortless integration</a>

</v-clicks>

## <span v-click class="font-bold">Other interesting avenues for JS frontend in rails</span>

<v-clicks>

- <a href="https://evilmartians.com/chronicles/the-art-of-turbo-mount-hotwire-meets-modern-js-frameworks" target="_blank" class="text-orange-500 hover:text-orange-600!">Blog Evil Martians - The art of turbo-mount/hotwire meets modern JS frameworks</a>

</v-clicks>

---
transition: fade-out
---

# About the speaker

<div v-click>
  I’ve been a programmer by trade for nearly 10 years, but above all <span class="font-bold text-orange-500">I’m a tech & product builder</span>
</div>

<div v-click>
  As such, when I build new things, I always aim for <span class="font-bold text-orange-500">speed & efficiency</span>.
</div>

<div v-click class="mt-4">
  Because winning with a new thing on the market always depends on how fast and efficiently the team behind it can ship it to users.
</div>

<div v-click class="font-bold my-4">
  I'm currently splitting my time between:
</div>

<v-clicks>

- Building <a href="https://www.stellaire.studio" target="_blank" class="text-orange-500 hover:text-orange-600!">stellaire.studio</a>, a startup studio specialized in building SaaS products
- Working as a freelance Product Developer / Founding Engineer

</v-clicks>

<div v-click class="mt-4">
  And btw, I’m currently on the lookout for a new part-time long-term freelancing mission 👋
</div>

<div v-click class="mt-8 text-center flex items-center justify-center gap-4">
  <img src="./assets/nfilzi.jpg" class="size-8 rounded-full">

  <a href="https://about.nfilzi.com" target="_blank" class="text-orange-500 hover:text-orange-600!">
    about.nfilzi.com
  </a>
</div>

---
transition: fade-out
---

# Questions?
