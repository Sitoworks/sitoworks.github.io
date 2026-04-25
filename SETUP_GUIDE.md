# Sitoworks Portfolio - Complete Setup Guide

## 📋 Complete File Structure & Code

Copy and create each file exactly as shown below.

---

## 1️⃣ **package.json**
**Location:** `package.json` (root)

```json
{
  "name": "sitoworks-portfolio",
  "version": "1.0.0",
  "description": "Cloud Expert & Trainer Portfolio",
  "type": "module",
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview",
    "deploy": "npm run build && gh-pages -d dist"
  },
  "dependencies": {
    "astro": "^4.2.0",
    "astro-icon": "^1.1.0"
  },
  "devDependencies": {
    "gh-pages": "^6.1.0"
  }
}
```

---

## 2️⃣ **astro.config.mjs**
**Location:** `astro.config.mjs` (root)

```javascript
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://sitoworks.github.io',
  integrations: [],
  output: 'static',
  vite: {
    ssr: {
      external: ['svgo']
    }
  }
});
```

---

## 3️⃣ **tsconfig.json**
**Location:** `tsconfig.json` (root)

```json
{
  "extends": "astro/tsconfigs/strict"
}
```

---

## 4️⃣ **.gitignore**
**Location:** `.gitignore` (root)

```
# build output
dist/
.output/

# dependencies
node_modules/

# logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*

# environment variables
.env
.env.local
.env.*.local

# editor
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# IDE
.astro/
.cache/
```

---

## 5️⃣ **src/layouts/BaseLayout.astro**
**Location:** `src/layouts/BaseLayout.astro`

```astro
---
import ThemeToggle from '../components/ThemeToggle.astro';

interface Props {
  title: string;
  description?: string;
}

const { title, description = 'Cloud Expert & Trainer Portfolio' } = Astro.props;
---

<!doctype html>
<html lang="en" class="scroll-smooth">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <title>{title} | Sitoworks</title>
    <style is:global>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      :root {
        --color-bg: #ffffff;
        --color-bg-secondary: #f5f5f5;
        --color-text: #1a1a1a;
        --color-text-secondary: #666666;
        --color-accent: #8b5cf6;
        --color-accent-dark: #7c3aed;
        --color-border: #e5e5e5;
      }

      html.dark {
        --color-bg: #0f172a;
        --color-bg-secondary: #1e293b;
        --color-text: #f8fafc;
        --color-text-secondary: #cbd5e1;
        --color-accent: #a78bfa;
        --color-accent-dark: #c4b5fd;
        --color-border: #334155;
      }

      body {
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
        background-color: var(--color-bg);
        color: var(--color-text);
        transition: background-color 0.3s, color 0.3s;
        line-height: 1.6;
      }

      a {
        color: var(--color-accent);
        text-decoration: none;
        transition: color 0.3s;
      }

      a:hover {
        color: var(--color-accent-dark);
      }

      h1, h2, h3, h4, h5, h6 {
        font-weight: 700;
        margin-bottom: 0.5rem;
      }

      h1 {
        font-size: 3rem;
        line-height: 1.2;
      }

      h2 {
        font-size: 2rem;
      }

      p {
        margin-bottom: 1rem;
        color: var(--color-text-secondary);
      }

      .container {
        max-width: 1200px;
        margin: 0 auto;
        padding: 0 1.5rem;
      }
    </style>
  </head>
  <body>
    <ThemeToggle client:load />
    <slot />
    <script>
      if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
        document.documentElement.classList.add('dark');
      } else {
        document.documentElement.classList.remove('dark');
      }
    </script>
  </body>
</html>

<script>
  // Theme persistence
  document.addEventListener('theme-toggle', () => {
    const html = document.documentElement;
    if (html.classList.contains('dark')) {
      localStorage.theme = 'dark';
    } else {
      localStorage.theme = 'light';
    }
  });
</script>

<style>
  :global(body) {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }
</style>
```

---

## 6️⃣ **src/components/ThemeToggle.astro**
**Location:** `src/components/ThemeToggle.astro`

```astro
---
---

<button id="theme-toggle" class="theme-toggle" aria-label="Toggle dark mode">
  <svg class="sun-icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
    <circle cx="12" cy="12" r="5"></circle>
    <line x1="12" y1="1" x2="12" y2="3"></line>
    <line x1="12" y1="21" x2="12" y2="23"></line>
    <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"></line>
    <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"></line>
    <line x1="1" y1="12" x2="3" y2="12"></line>
    <line x1="21" y1="12" x2="23" y2="12"></line>
    <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"></line>
    <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"></line>
  </svg>
  <svg class="moon-icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
    <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path>
  </svg>
</button>

<style>
  .theme-toggle {
    position: fixed;
    top: 2rem;
    right: 2rem;
    z-index: 1000;
    background: var(--color-bg-secondary);
    border: 2px solid var(--color-border);
    border-radius: 50%;
    width: 50px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s;
    color: var(--color-accent);
  }

  .theme-toggle:hover {
    border-color: var(--color-accent);
    transform: scale(1.1);
  }

  .sun-icon {
    position: absolute;
    transition: opacity 0.3s, transform 0.3s;
  }

  .moon-icon {
    position: absolute;
    opacity: 0;
    transform: rotate(-180deg);
    transition: opacity 0.3s, transform 0.3s;
  }

  html.dark .sun-icon {
    opacity: 0;
    transform: rotate(180deg);
  }

  html.dark .moon-icon {
    opacity: 1;
    transform: rotate(0);
  }
</style>

<script>
  const toggle = document.getElementById('theme-toggle');
  const html = document.documentElement;

  toggle?.addEventListener('click', () => {
    html.classList.toggle('dark');
    document.dispatchEvent(new Event('theme-toggle'));
  });
</script>
```

---

## 7️⃣ **src/components/Navigation.astro**
**Location:** `src/components/Navigation.astro`

```astro
---
---

<nav class="navbar">
  <div class="container">
    <div class="nav-content">
      <a href="/" class="logo">
        <span class="logo-text">Sitoworks</span>
      </a>
      <ul class="nav-links">
        <li><a href="/#about">About</a></li>
        <li><a href="/#expertise">Expertise</a></li>
        <li><a href="/#contact">Contact</a></li>
      </ul>
    </div>
  </div>
</nav>

<style>
  .navbar {
    position: sticky;
    top: 0;
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    border-bottom: 1px solid var(--color-border);
    z-index: 100;
    transition: all 0.3s;
  }

  html.dark .navbar {
    background: rgba(15, 23, 42, 0.95);
  }

  .nav-content {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 70px;
  }

  .logo {
    font-size: 1.5rem;
    font-weight: 800;
    background: linear-gradient(135deg, #8b5cf6, #ec4899);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    transition: transform 0.3s;
  }

  .logo:hover {
    transform: scale(1.05);
  }

  .nav-links {
    display: flex;
    list-style: none;
    gap: 2.5rem;
  }

  .nav-links a {
    font-weight: 500;
    position: relative;
    transition: color 0.3s;
  }

  .nav-links a::after {
    content: '';
    position: absolute;
    bottom: -5px;
    left: 0;
    width: 0;
    height: 2px;
    background: var(--color-accent);
    transition: width 0.3s;
  }

  .nav-links a:hover::after {
    width: 100%;
  }

  @media (max-width: 768px) {
    .nav-links {
      gap: 1.5rem;
      font-size: 0.9rem;
    }

    .nav-content {
      height: 60px;
    }

    .logo {
      font-size: 1.2rem;
    }
  }
</style>
```

---

## 8️⃣ **src/components/Hero.astro**
**Location:** `src/components/Hero.astro`

```astro
---
---

<section class="hero">
  <div class="container">
    <div class="hero-content">
      <div class="hero-text">
        <h1 class="gradient-text">
          Cloud Expert & <span class="highlight">Training Leader</span>
        </h1>
        <p class="tagline">
          Helping teams architect, deploy, and master cloud infrastructure with cutting-edge strategies and hands-on training.
        </p>
        <div class="cta-buttons">
          <a href="#contact" class="btn btn-primary">Get in Touch</a>
          <a href="#expertise" class="btn btn-secondary">View Expertise</a>
        </div>
      </div>
      <div class="hero-visual">
        <div class="gradient-blob"></div>
        <div class="gradient-blob blob-2"></div>
        <svg class="icon" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
          <circle cx="50" cy="50" r="30" fill="none" stroke="currentColor" stroke-width="2"/>
          <circle cx="50" cy="50" r="20" fill="none" stroke="currentColor" stroke-width="1.5"/>
          <circle cx="50" cy="50" r="10" fill="none" stroke="currentColor" stroke-width="1"/>
          <circle cx="50" cy="20" r="3" fill="currentColor"/>
          <circle cx="50" cy="80" r="3" fill="currentColor"/>
          <circle cx="20" cy="50" r="3" fill="currentColor"/>
          <circle cx="80" cy="50" r="3" fill="currentColor"/>
        </svg>
      </div>
    </div>
  </div>
</section>

<style>
  .hero {
    padding: 6rem 0;
    min-height: 80vh;
    display: flex;
    align-items: center;
    position: relative;
    overflow: hidden;
  }

  .hero::before {
    content: '';
    position: absolute;
    top: -50%;
    right: -10%;
    width: 600px;
    height: 600px;
    background: radial-gradient(circle, rgba(139, 92, 246, 0.1) 0%, transparent 70%);
    border-radius: 50%;
    pointer-events: none;
  }

  .hero-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
    position: relative;
    z-index: 2;
  }

  .hero-text {
    animation: fadeInUp 0.8s ease-out;
  }

  .gradient-text {
    font-size: clamp(2.5rem, 8vw, 4rem);
    line-height: 1.2;
    margin-bottom: 1.5rem;
    color: var(--color-text);
  }

  .highlight {
    background: linear-gradient(135deg, #8b5cf6 0%, #ec4899 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    position: relative;
  }

  .tagline {
    font-size: 1.2rem;
    line-height: 1.8;
    color: var(--color-text-secondary);
    margin-bottom: 2rem;
  }

  .cta-buttons {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
  }

  .btn {
    padding: 0.75rem 1.5rem;
    border-radius: 8px;
    font-weight: 600;
    transition: all 0.3s;
    display: inline-block;
    border: 2px solid transparent;
    cursor: pointer;
  }

  .btn-primary {
    background: linear-gradient(135deg, #8b5cf6 0%, #7c3aed 100%);
    color: white;
  }

  .btn-primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 25px rgba(139, 92, 246, 0.3);
  }

  .btn-secondary {
    background: transparent;
    color: var(--color-accent);
    border-color: var(--color-accent);
  }

  .btn-secondary:hover {
    background: var(--color-accent);
    color: white;
  }

  .hero-visual {
    position: relative;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadeInRight 0.8s ease-out;
  }

  .gradient-blob {
    position: absolute;
    width: 300px;
    height: 300px;
    background: linear-gradient(135deg, rgba(139, 92, 246, 0.2), rgba(236, 72, 153, 0.1));
    border-radius: 40% 60% 70% 30% / 40% 50% 60% 50%;
    animation: blob 8s infinite;
  }

  .blob-2 {
    animation-delay: 2s;
    width: 250px;
    height: 250px;
    opacity: 0.7;
  }

  @keyframes blob {
    0%, 100% { transform: translate(0, 0) rotate(0deg); }
    50% { transform: translate(30px, -30px) rotate(180deg); }
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes fadeInRight {
    from {
      opacity: 0;
      transform: translateX(30px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

  .icon {
    width: 200px;
    height: 200px;
    color: var(--color-accent);
    position: relative;
    z-index: 2;
  }

  @media (max-width: 768px) {
    .hero {
      padding: 4rem 0;
      min-height: auto;
    }

    .hero-content {
      grid-template-columns: 1fr;
      gap: 2rem;
    }

    .hero-visual {
      height: 250px;
    }

    .gradient-blob {
      width: 200px;
      height: 200px;
    }

    .blob-2 {
      width: 150px;
      height: 150px;
    }

    .cta-buttons {
      flex-direction: column;
    }

    .btn {
      width: 100%;
      text-align: center;
    }
  }
</style>
```

---

## 9️⃣ **src/components/About.astro**
**Location:** `src/components/About.astro`

```astro
---
---

<section id="about" class="about">
  <div class="container">
    <h2>About Me</h2>
    <div class="about-content">
      <div class="about-image">
        <img src="/profile.jpg" alt="Sitoworks - Cloud Expert & Trainer" class="profile-img" />
      </div>
      <div class="about-text">
        <p>
          I'm a passionate cloud architect and technical trainer with 10+ years of experience helping organizations transform their infrastructure and optimize their cloud strategies.
        </p>
        <p>
          My journey began with a deep fascination for distributed systems, and it has evolved into a mission to demystify cloud technologies for teams of all levels. I believe that the best solutions come from understanding not just the "how" but the "why" behind every architectural decision.
        </p>
        <p>
          When I'm not architecting cloud solutions or delivering training, you'll find me contributing to open-source projects, speaking at conferences, or mentoring the next generation of cloud engineers.
        </p>
        <div class="social-links">
          <a href="https://github.com" target="_blank" rel="noopener noreferrer" class="social-link">GitHub</a>
          <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" class="social-link">LinkedIn</a>
          <a href="https://twitter.com" target="_blank" rel="noopener noreferrer" class="social-link">Twitter</a>
        </div>
      </div>
    </div>
    <div class="stats-grid">
      <div class="stat-card">
        <div class="stat-number">10+</div>
        <div class="stat-label">Years Experience</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">500+</div>
        <div class="stat-label">Engineers Trained</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">50+</div>
        <div class="stat-label">Successful Projects</div>
      </div>
      <div class="stat-card">
        <div class="stat-number">15+</div>
        <div class="stat-label">Talks & Conferences</div>
      </div>
    </div>
  </div>
</section>

<style>
  .about {
    padding: 6rem 0;
    background: var(--color-bg-secondary);
  }

  h2 {
    text-align: center;
    margin-bottom: 3rem;
  }

  .about-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
    margin-bottom: 4rem;
  }

  .about-image {
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
    animation: fadeInLeft 0.8s ease-out;
  }

  .profile-img {
    width: 100%;
    height: auto;
    display: block;
  }

  .about-text p {
    font-size: 1.05rem;
    line-height: 1.8;
    margin-bottom: 1.5rem;
  }

  .social-links {
    display: flex;
    gap: 1.5rem;
    margin-top: 2rem;
  }

  .social-link {
    display: inline-block;
    padding: 0.5rem 1.25rem;
    border: 2px solid var(--color-accent);
    border-radius: 6px;
    font-weight: 600;
    transition: all 0.3s;
    color: var(--color-accent);
  }

  .social-link:hover {
    background: var(--color-accent);
    color: white;
    transform: translateY(-2px);
  }

  .stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 2rem;
  }

  .stat-card {
    background: var(--color-bg);
    padding: 2rem;
    border-radius: 12px;
    border: 2px solid var(--color-border);
    text-align: center;
    transition: all 0.3s;
  }

  .stat-card:hover {
    border-color: var(--color-accent);
    transform: translateY(-5px);
  }

  .stat-number {
    font-size: 2.5rem;
    font-weight: 800;
    background: linear-gradient(135deg, #8b5cf6 0%, #ec4899 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 0.5rem;
  }

  .stat-label {
    font-size: 0.95rem;
    color: var(--color-text-secondary);
    margin: 0;
  }

  @keyframes fadeInLeft {
    from {
      opacity: 0;
      transform: translateX(-30px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

  @media (max-width: 768px) {
    .about {
      padding: 4rem 0;
    }

    .about-content {
      grid-template-columns: 1fr;
      gap: 2rem;
    }

    .social-links {
      flex-wrap: wrap;
    }

    .stats-grid {
      grid-template-columns: 1fr 1fr;
    }
  }
</style>
```

---

## 🔟 **src/components/Expertise.astro**
**Location:** `src/components/Expertise.astro`

```astro
---
---

<section id="expertise" class="expertise">
  <div class="container">
    <h2>Areas of Expertise</h2>
    <div class="expertise-grid">
      <div class="expertise-card">
        <div class="card-icon">☁️</div>
        <h3>Cloud Architecture</h3>
        <p>Designing scalable, secure, and cost-optimized cloud solutions on AWS, Azure, and GCP.</p>
        <ul class="tech-list">
          <li>Multi-cloud strategies</li>
          <li>Microservices architecture</li>
          <li>Serverless design</li>
          <li>Infrastructure as Code</li>
        </ul>
      </div>

      <div class="expertise-card">
        <div class="card-icon">🔐</div>
        <h3>Security & Compliance</h3>
        <p>Implementing robust security practices and ensuring compliance with industry standards.</p>
        <ul class="tech-list">
          <li>Zero-trust architecture</li>
          <li>Identity & Access Management</li>
          <li>Data protection strategies</li>
          <li>Compliance automation</li>
        </ul>
      </div>

      <div class="expertise-card">
        <div class="card-icon">⚙️</div>
        <h3>DevOps & Automation</h3>
        <p>Streamlining deployment pipelines and automating infrastructure management.</p>
        <ul class="tech-list">
          <li>CI/CD pipelines</li>
          <li>Container orchestration</li>
          <li>Infrastructure automation</li>
          <li>Monitoring & observability</li>
        </ul>
      </div>

      <div class="expertise-card">
        <div class="card-icon">🎓</div>
        <h3>Training & Mentoring</h3>
        <p>Delivering hands-on training programs and mentoring teams to become cloud experts.</p>
        <ul class="tech-list">
          <li>Certification prep courses</li>
          <li>Workshop facilitation</li>
          <li>Best practices guidance</li>
          <li>Career development</li>
        </ul>
      </div>

      <div class="expertise-card">
        <div class="card-icon">💰</div>
        <h3>Cost Optimization</h3>
        <p>Reducing cloud spending without compromising performance or reliability.</p>
        <ul class="tech-list">
          <li>Resource optimization</li>
          <li>Reserved instance strategy</li>
          <li>Cost analysis & reporting</li>
          <li>Budget management</li>
        </ul>
      </div>

      <div class="expertise-card">
        <div class="card-icon">📊</div>
        <h3>Data & Analytics</h3>
        <p>Building data pipelines and analytics solutions on modern cloud platforms.</p>
        <ul class="tech-list">
          <li>Data warehousing</li>
          <li>ETL/ELT pipelines</li>
          <li>Real-time analytics</li>
          <li>BI integration</li>
        </ul>
      </div>
    </div>
  </div>
</section>

<style>
  .expertise {
    padding: 6rem 0;
  }

  h2 {
    text-align: center;
    margin-bottom: 3rem;
  }

  .expertise-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 2rem;
  }

  .expertise-card {
    padding: 2.5rem;
    background: var(--color-bg-secondary);
    border: 2px solid var(--color-border);
    border-radius: 12px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }

  .expertise-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 4px;
    background: linear-gradient(90deg, #8b5cf6, #ec4899);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.3s;
  }

  .expertise-card:hover::before {
    transform: scaleX(1);
  }

  .expertise-card:hover {
    border-color: var(--color-accent);
    transform: translateY(-8px);
    box-shadow: 0 10px 30px rgba(139, 92, 246, 0.1);
  }

  .card-icon {
    font-size: 2.5rem;
    margin-bottom: 1rem;
  }

  .expertise-card h3 {
    margin-bottom: 1rem;
    color: var(--color-text);
  }

  .expertise-card p {
    margin-bottom: 1.5rem;
    font-size: 0.95rem;
  }

  .tech-list {
    list-style: none;
  }

  .tech-list li {
    padding: 0.4rem 0;
    padding-left: 1.5rem;
    position: relative;
    font-size: 0.9rem;
    color: var(--color-text-secondary);
  }

  .tech-list li::before {
    content: '→';
    position: absolute;
    left: 0;
    color: var(--color-accent);
    font-weight: bold;
  }

  @media (max-width: 768px) {
    .expertise {
      padding: 4rem 0;
    }

    .expertise-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
```

---

## 1️⃣1️⃣ **src/components/ContactForm.astro**
**Location:** `src/components/ContactForm.astro`

```astro
---
---

<section id="contact" class="contact">
  <div class="container">
    <h2>Get In Touch</h2>
    <p class="contact-intro">
      Have a project in mind? Let's discuss how I can help you succeed with your cloud journey.
    </p>
    <form class="contact-form" id="contact-form">
      <div class="form-group">
        <label for="name">Your Name</label>
        <input type="text" id="name" name="name" required />
      </div>

      <div class="form-group">
        <label for="email">Email Address</label>
        <input type="email" id="email" name="email" required />
      </div>

      <div class="form-group">
        <label for="subject">Subject</label>
        <input type="text" id="subject" name="subject" required />
      </div>

      <div class="form-group">
        <label for="message">Message</label>
        <textarea id="message" name="message" rows="6" required></textarea>
      </div>

      <button type="submit" class="btn btn-primary">Send Message</button>
      <div id="form-message" class="form-message"></div>
    </form>
  </div>
</section>

<style>
  .contact {
    padding: 6rem 0;
    background: var(--color-bg-secondary);
  }

  h2 {
    text-align: center;
    margin-bottom: 1rem;
  }

  .contact-intro {
    text-align: center;
    font-size: 1.1rem;
    margin-bottom: 3rem;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
  }

  .contact-form {
    max-width: 600px;
    margin: 0 auto;
    background: var(--color-bg);
    padding: 2.5rem;
    border-radius: 12px;
    border: 2px solid var(--color-border);
  }

  .form-group {
    margin-bottom: 1.5rem;
  }

  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 600;
    color: var(--color-text);
  }

  input,
  textarea {
    width: 100%;
    padding: 0.75rem 1rem;
    border: 2px solid var(--color-border);
    border-radius: 8px;
    background: var(--color-bg);
    color: var(--color-text);
    font-family: inherit;
    transition: all 0.3s;
    font-size: 1rem;
  }

  input:focus,
  textarea:focus {
    outline: none;
    border-color: var(--color-accent);
    box-shadow: 0 0 0 3px rgba(139, 92, 246, 0.1);
  }

  textarea {
    resize: vertical;
    min-height: 120px;
  }

  .btn {
    width: 100%;
    margin-top: 1rem;
    padding: 0.85rem 1.5rem;
  }

  .form-message {
    margin-top: 1rem;
    padding: 1rem;
    border-radius: 8px;
    text-align: center;
    font-weight: 600;
    display: none;
  }

  .form-message.success {
    display: block;
    background: rgba(34, 197, 94, 0.1);
    color: #22c55e;
    border: 1px solid #22c55e;
  }

  .form-message.error {
    display: block;
    background: rgba(239, 68, 68, 0.1);
    color: #ef4444;
    border: 1px solid #ef4444;
  }

  @media (max-width: 768px) {
    .contact {
      padding: 4rem 0;
    }

    .contact-form {
      padding: 1.5rem;
    }
  }
</style>

<script>
  const form = document.getElementById('contact-form');
  const messageDiv = document.getElementById('form-message');

  form?.addEventListener('submit', async (e) => {
    e.preventDefault();

    const formData = new FormData(form);
    const data = Object.fromEntries(formData);

    try {
      // Note: You'll need to set up a backend service to handle form submissions
      // For now, this is a placeholder that shows a success message
      messageDiv.textContent = 'Thank you for your message! I\'ll get back to you soon.';
      messageDiv.className = 'form-message success';
      form.reset();

      setTimeout(() => {
        messageDiv.style.display = 'none';
      }, 5000);
    } catch (error) {
      messageDiv.textContent = 'An error occurred. Please try again later.';
      messageDiv.className = 'form-message error';
    }
  });
</script>
```

---

## 1️⃣2️⃣ **src/components/Footer.astro**
**Location:** `src/components/Footer.astro`

```astro
---
---

<footer class="footer">
  <div class="container">
    <div class="footer-content">
      <div class="footer-section">
        <h4>Sitoworks</h4>
        <p>Cloud Expert & Trainer</p>
      </div>
      <div class="footer-section">
        <h4>Quick Links</h4>
        <ul>
          <li><a href="/#about">About</a></li>
          <li><a href="/#expertise">Expertise</a></li>
          <li><a href="/#contact">Contact</a></li>
        </ul>
      </div>
      <div class="footer-section">
        <h4>Connect</h4>
        <ul>
          <li><a href="https://github.com" target="_blank">GitHub</a></li>
          <li><a href="https://linkedin.com" target="_blank">LinkedIn</a></li>
          <li><a href="https://twitter.com" target="_blank">Twitter</a></li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">
      <p>&copy; 2026 Sitoworks. Built with Astro.</p>
    </div>
  </div>
</footer>

<style>
  .footer {
    background: var(--color-bg-secondary);
    border-top: 2px solid var(--color-border);
    padding: 4rem 0 2rem;
    margin-top: auto;
  }

  .footer-content {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 2rem;
    margin-bottom: 2rem;
  }

  .footer-section h4 {
    margin-bottom: 1rem;
    color: var(--color-text);
  }

  .footer-section p,
  .footer-section li {
    color: var(--color-text-secondary);
    margin-bottom: 0.5rem;
  }

  .footer-section ul {
    list-style: none;
  }

  .footer-section a {
    transition: color 0.3s;
  }

  .footer-section a:hover {
    color: var(--color-accent);
  }

  .footer-bottom {
    border-top: 1px solid var(--color-border);
    padding-top: 2rem;
    text-align: center;
    color: var(--color-text-secondary);
    font-size: 0.9rem;
  }

  @media (max-width: 768px) {
    .footer-content {
      grid-template-columns: 1fr;
    }
  }
</style>
```

---

## 1️⃣3️⃣ **src/pages/index.astro**
**Location:** `src/pages/index.astro`

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Navigation from '../components/Navigation.astro';
import Hero from '../components/Hero.astro';
import About from '../components/About.astro';
import Expertise from '../components/Expertise.astro';
import ContactForm from '../components/ContactForm.astro';
import Footer from '../components/Footer.astro';
---

<BaseLayout title="Home" description="Cloud Expert & Trainer Portfolio - Sitoworks">
  <Navigation />
  <main>
    <Hero />
    <About />
    <Expertise />
    <ContactForm />
  </main>
  <Footer />
</BaseLayout>

<style>
  main {
    flex: 1;
  }
</style>
```

---

## 1️⃣4️⃣ **.github/workflows/deploy.yml**
**Location:** `.github/workflows/deploy.yml`

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install
      - run: npm run build
      - uses: actions/upload-artifact@v3
        with:
          name: site
          path: dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - uses: actions/download-artifact@v3
        with:
          name: site
          path: dist
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

---

## 1️⃣5️⃣ **README.md**
**Location:** `README.md` (root)

```markdown
# Sitoworks Portfolio

A modern, creative, and responsive portfolio website for cloud experts and trainers.

## Features

✨ **Modern Design**
- Creative and bold visual design
- Smooth animations and transitions
- Fully responsive mobile-first approach

🌙 **Dark Mode**
- Toggle between light and dark themes
- Persistent theme preference (localStorage)
- Smooth transitions

📱 **Mobile Optimized**
- High priority responsive design
- Touch-friendly interface
- Fast loading on all devices

📧 **Contact Form**
- Clean, accessible form design
- Client-side validation
- Success/error messaging

⚡ **Performance**
- Built with Astro for optimal performance
- Zero JavaScript by default
- Fast static site generation

## Tech Stack

- **Astro** - Static site generator
- **CSS** - Modern CSS with custom properties and grid/flexbox
- **JavaScript** - Minimal vanilla JS for interactivity
- **GitHub Pages** - Hosting

## Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/Sitoworks/sitoworks.github.io.git
cd sitoworks.github.io

# Install dependencies
npm install
```

### Development

```bash
# Start the development server
npm run dev

# Open http://localhost:3000 in your browser
```

### Building

```bash
# Build for production
npm run build

# Preview the build
npm run preview
```

### Deployment

The GitHub Actions workflow automatically deploys to GitHub Pages on every push to main.

## Customization

### Update Content

1. **About Section** - Edit `src/components/About.astro`
2. **Expertise** - Edit `src/components/Expertise.astro`
3. **Social Links** - Update URLs in `src/components/About.astro` and `src/components/Footer.astro`
4. **Profile Picture** - Replace `public/profile.jpg` with your image

### Update Colors

Edit the CSS custom properties in `src/layouts/BaseLayout.astro`:

```css
:root {
  --color-bg: #ffffff;
  --color-text: #1a1a1a;
  --color-accent: #8b5cf6; /* Change this to your brand color */
  /* ... other colors ... */
}
```

## Form Integration

The contact form is currently a placeholder. To make it functional, integrate with:

1. **Formspree** (Recommended) - https://formspree.io
2. **EmailJS** - https://www.emailjs.com
3. **Netlify Forms** - If hosting on Netlify

Example with Formspree:
```html
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
  <!-- form fields -->
</form>
```

## Browser Support

- Chrome/Edge: Latest 2 versions
- Firefox: Latest 2 versions
- Safari: Latest 2 versions
- Mobile browsers: Latest versions

## License

MIT

## Support

For questions or issues, please open a GitHub issue.
```

---

## 📦 **Final Directory Structure**

```
sitoworks.github.io/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── src/
│   ├── components/
│   │   ├── About.astro
│   │   ├── ContactForm.astro
│   │   ├── Expertise.astro
│   │   ├── Footer.astro
│   │   ├── Hero.astro
│   │   ├── Navigation.astro
│   │   └── ThemeToggle.astro
│   ├── layouts/
│   │   └── BaseLayout.astro
│   └── pages/
│       └── index.astro
├── public/
│   └── profile.jpg (← Your profile picture)
├── .gitignore
├── astro.config.mjs
├── package.json
├── tsconfig.json
└── README.md
```

---

## 🚀 **Deployment Steps**

1. **Create all files** as shown above
2. **Add your profile picture** to `public/profile.jpg`
3. **Update social links** in About and Footer components
4. **Install dependencies**: `npm install`
5. **Test locally**: `npm run dev`
6. **Push to GitHub**: 
   ```bash
   git add .
   git commit -m "Initial portfolio setup with profile picture"
   git push origin main
   ```
7. **GitHub Actions will automatically deploy** to GitHub Pages
8. **Visit** `https://sitoworks.github.io` in 2-3 minutes

Done! 🎉
```

---

**✅ You now have the complete setup guide with all code ready to copy and paste!** 

Would you like me to create any additional files or modifications?
