<template>
  <section class="about section-pad" id="about">

    <!-- reusing the global utility classes from global.css -->
    <div class="section-label">About me</div>
    <h2 class="section-title">Engineer who <em>ships</em> real things.</h2>

    <div class="about__grid">

      <!-- left: bio text-->
      <div class="about__text">
        <p>
          I'm a <strong>Full Stack Developer</strong> who thrives across the entire
          stack — from architecting database schemas to crafting awesome UIs.
          I care deeply about writing code that is clean, maintainable, and built
          to scale under pressure.
        </p>
        <p>
          With 4+ years of experience, I've shipped production applications across
          <strong>fintech, oil and gas, non-governmental organisations, reality tv shows and SaaS</strong>;working with early-stage
          startups and established teams alike. I'm comfortable in ambiguity and
          thrive when given ownership.
        </p>
        <p>
          Currently open to <strong>full-time roles, contract work,</strong> and
          exciting collaborations. Let's build software that truly matters.
        </p>
      </div>

      <!-- right: highlight cards ── -->
      <div class="about__cards">
        <!--
          v-for renders one .about__card for each item in the "cards" array.
          We add "fade-up" so the IntersectionObserver in mounted() can animate them.
          :key gives Vue a stable identifier for each element.
        -->
        <div
          v-for="card in cards"
          :key="card.title"
          class="about__card fade-up"
        >
          <!-- card.icon is an emoji; card.title and card.body come from data() -->
          <div>
            <component :is="card.icon" />
            <h4>{{ card.title }}</h4>
          </div>
          <p>{{ card.body }}</p>
        </div>
      </div>

    </div>
  </section>
</template>

<script lang="ts">
import { Globe, Code, Cloud, Laptop } from 'lucide-vue-next'

export default {
  name: 'AboutSection',

  data() {
    return {
      // Each object here becomes one card in the right column
      cards: [
        {
          icon: Globe,
          title: 'Remote & Flexible',
          body: 'Available to work with global teams, async-first or real-time.',
        },
        {
          icon: Code,
          title: 'Full Stack, End-to-End',
          body: 'From database schema design to pixel-perfect UI implementation.',
        },
        {
          icon: Cloud,
          title: 'Shipping-First Mindset',
          body: 'I care about delivering value, not just writing code that looks clever.',
        },
        {
          icon: Laptop,
          title: 'Fast Learner',
          body: 'Picks up new stacks quickly and ships without hand-holding.',
        },
      ],
    }
  },

  mounted() {
    // Scroll-triggered fade-in
    // IntersectionObserver fires a callback when elements enter the viewport.
    // This is the modern, performant way to do scroll animations (no scroll events!).

    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          // entry.isIntersecting is true when the element is visible in the viewport
          if (entry.isIntersecting) {
            entry.target.classList.add('visible') // triggers the CSS transition
            observer.unobserve(entry.target)       // stop watching once animated
          }
        })
      },
      { threshold: 0.15 } // fire when 15% of the element is visible
    )

    // Observe all .fade-up elements inside this component
    this.$el.querySelectorAll('.fade-up').forEach((el:any) => observer.observe(el))
  },
}
</script>

<style scoped>
/* section background */
.about {
  background: var(--surface); /* slightly lighter than the page bg */
}

/* two-column grid */
.about__grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 80px;
  align-items: start;
  margin-top: 60px;
}

/* bio text */
.about__text p {
  font-size: 17px;
  line-height: 1.8;
  color: rgba(232, 228, 220, 0.75); /* slightly dimmed for readability */
  margin-bottom: 20px;
}
.about__text p strong {
  color: var(--text);
  font-weight: 600;
}

/* cards column */
.about__cards {
  display: flex;
  flex-direction: column;
  gap: 2px; /* 1px gap between cards to look like dividers */
}

/* individual card  */
.about__card {
  background: var(--surface2);
  padding: 28px 32px;
  border-left: 3px solid transparent; /* placeholder for hover accent */
  transition: border-color 0.3s, background 0.3s;
}
.about__card:hover {
  /*border-color: var(--accent);*/
  background: #1e1e1e;
}
.about__card h4 {
  font-size: 16px;
  font-weight: 700;
  margin-bottom: 6px;
}
.about__card p {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--muted);
  line-height: 1.6;
}

/* mobile  */
@media (max-width: 900px) {
  .about__grid {
    grid-template-columns: 1fr; /* stack columns on mobile */
    gap: 40px;
  }
}
</style>
