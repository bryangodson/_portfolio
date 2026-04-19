<template>
  <nav :class="['nav', { 'nav--scrolled': isScrolled }]">
    <!-- logo — left side -->
    <div class="nav-logo">
      bryan<span class="nav-logo__dot">.</span>godson
    </div>

    <!-- nav links — center -->
    <ul class="nav-links">
      <li v-for="link in links" :key="link.href">
        <a :href="link.href">{{ link.label }}</a>
      </li>
    </ul>

    <!-- CTA button — right side -->
    <a href="mailto:ppdnquh@gmail.com" class="nav-cta">Hire me →</a>
  </nav>
</template>

<script lang="ts">
export default {
  name: 'AppNav',

  data() {
    return {
      // controls whether the "scrolled" style class is active
      isScrolled: false,

      // the nav links array — v-for iterates over this to render <li> elements.
      links: [
        { href: '#about',   label: 'About'   },
        { href: '#skills',  label: 'Stack'   },
        { href: '#work',    label: 'Work'    },
        { href: '#contact', label: 'Contact' },
      ],
    }
  },

  //vue lifecycle hook to attach listeners
  mounted() {
    // listen for window scroll to toggle the nav background
    window.addEventListener('scroll', this.handleScroll)
  },

  // beforeUnmount()
  // beforeUnmount() runs just before the component is destroyed.
  // Always clean up event listeners to prevent memory leaks!
  beforeUnmount() {
    window.removeEventListener('scroll', this.handleScroll)
  },

  // ── methods
  //  contains functions you want to use in the template or call
  // from other methods. Inside methods, "this" refers to the component instance,
  // so this.isScrolled accesses the data property above.
  methods: {
    handleScroll() {
      // If a user has scrolled more than 50px, set isScrolled to true.
      // Vue will then apply the "nav--scrolled" class to the <nav> element.
      this.isScrolled = window.scrollY > 50
    },
  },
}
</script>

<style scoped>
/* base nav styles */
.nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 28px 48px;
  /* Fade from dark to transparent below — acts as a soft gradient overlay */
  background: linear-gradient(to bottom, rgba(10,10,10,0.95) 0%, transparent 100%);
  transition: background 0.4s, padding 0.3s;
}

/* Applied when isScrolled === true — tightens padding and adds blur */
.nav--scrolled {
  padding: 18px 48px;
  background: rgba(10,10,10,0.96);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
}

/* ── Logo ────────────────────────────────────────────────────────────────────── */
.nav-logo {
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--muted);
  letter-spacing: 0.05em;
}
.nav-logo__dot { color: var(--accent); }

/* ── Nav links ───────────────────────────────────────────────────────────────── */
.nav-links {
  display: flex;
  gap: 36px;
  list-style: none; /* removes bullet points */
}
.nav-links a {
  font-family: var(--font-mono);
  font-size: 12px;
  letter-spacing: 0.08em;
  color: var(--muted);
  text-decoration: none;
  text-transform: uppercase;
  transition: color 0.2s;
  position: relative;
}
/* Animated underline using a pseudo-element */
.nav-links a::after {
  content: '';
  position: absolute;
  bottom: -2px; left: 0;
  width: 0; height: 1px;
  background: var(--accent);
  transition: width 0.3s ease;
}
.nav-links a:hover { color: var(--text); }
.nav-links a:hover::after { width: 100%; } /* expand underline on hover */

/* ── CTA button ──────────────────────────────────────────────────────────────── */
.nav-cta {
  font-family: var(--font-mono);
  font-size: 12px;
  letter-spacing: 0.08em;
  color: var(--bg);
  background: var(--accent);
  padding: 10px 20px;
  text-decoration: none;
  text-transform: uppercase;
  transition: background 0.2s, transform 0.2s;
}
.nav-cta:hover {
  background: var(--white);
  transform: translateY(-1px);
}

/* ── Mobile  */
@media (max-width: 900px) {
  .nav { padding: 20px 24px; }
  .nav-links { display: none; } /* hide links on small screens */
}
</style>
