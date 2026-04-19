<template>
  <section class="contact section-pad" id="contact">

    <div class="contact__inner">

      <!-- centered section label -->
      <div class="section-label contact__label">Let's connect</div>

      <!-- large headline with italic serif contrast -->
      <h2 class="contact__title">
        Let's build<br /><em>something great.</em>
      </h2>

      <!-- short descriptor paragraph -->
      <p class="contact__sub">
        Open to full-time and part time roles, contract work, and collaborations.
      </p>

      <!-- contact link buttons rendered from data -->
      <div class="contact__links">
        <!--
          v-for over the "contactLinks" array.
          :href binds the link's URL from the data.
          :key uses the label for uniqueness.
        -->
        <a
          v-for="link in contactLinks"
          :key="link.label"
          :href="link.href"
          class="contact-link"
        >
          <!-- icon and label rendered from data -->
          <component class="contact-link__icon" :is="link.icon" />
          {{ link.label }}
        </a>
      </div>

    </div>
  </section>
</template>

<script lang="ts">
import {Globe,Mail,Twitter,Linkedin,Github,FileUser} from "lucide-vue-next"
export default {
  name: 'ContactSection',

  data() {
    return {
      // Each object becomes one button in the contact grid
      contactLinks: [
        {
          icon: Mail,
          label: 'Email me',
          href: 'mailto:ppdnquh@gmail.com'
        },
        {
          icon: Twitter,
          label: 'Twitter / X',
          href: 'https://twitter.com/iamdanquah'
        },
        {
          icon: Linkedin,
          label: 'LinkedIn',
          href: 'https://linkedin.com'
        },
        {
          icon: Github,
          label: 'GitHub',
          href: 'https://github.com/bryangodson'
        },
        {
          icon: FileUser,
          label: 'Resume',
          href: '/docs/bryan_cv.pdf'
        },
      ],
    }
  },
}
</script>

<style scoped>
/*Center everything*/
.contact__inner {
  max-width: 900px;
  margin: 0 auto;
  text-align: center;
}

/* Centered section label overrides the left-aligned default */
.contact__label { justify-content: center; }

/* Big headline  */
.contact__title {
  font-size: clamp(48px, 6vw, 96px);
  font-weight: 800;
  line-height: 0.95;
  letter-spacing: -0.04em;
  margin: 40px 0 32px;
}
/* Outline / stroke treatment for the italic em */
.contact__title em {
  font-family: var(--font-serif);
  font-style: italic;
  font-weight: 400;
  color: transparent;
  -webkit-text-stroke: 1px var(--text);
}

/* Sub-paragraph */
.contact__sub {
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--muted);
  max-width: 460px;
  margin: 0 auto;
  line-height: 1.8;
}

/* Contact buttons row  */
.contact__links {
  display: flex;
  flex-wrap: wrap; /* wraps onto next line on narrow screens */
  gap: 12px;
  justify-content: center;
  margin-top: 48px;
}

/*Individual contact button */
.contact-link {
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: var(--font-mono);
  font-size: 12px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--muted);
  border: 1px solid var(--border);
  padding: 14px 24px;
  text-decoration: none;
  transition: color 0.25s, background 0.25s, border-color 0.25s;
}
.contact-link:hover {
  color: var(--bg);
  background: var(--accent);
  border-color: var(--accent);
}

.contact-link__icon { font-size: 16px; }

/* Mobile  */
@media (max-width: 600px) {
  .contact-link { padding: 12px 18px; font-size: 11px; }
}
</style>
