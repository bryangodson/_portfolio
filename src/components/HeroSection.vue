<template>
  <section class="hero" id="home">

    <!--divides the hero without the stat and the image -->
    <div class="hero_with_image">
    <!-- left column: text content -->
    <div class="hero__left">

      <!-- "Available" badge with animated green dot -->
      <div class="hero__badge">Available for opportunities</div>

      <!-- Main headline -->
      <h1 class="hero__title">
        <!--
          Each line is wrapped in a <span class="line"> so we can animate
          each line individually (overflow: hidden clips the slide-up).
        -->
        <span class="line">Full Stack</span>
        <!--
          "serif" class applies the italic Instrument Serif font.
          The outline (stroke) effect is done in CSS with -webkit-text-stroke.
        -->
        <span class="line serif">Engineer.</span>
      </h1>

      <!-- Short bio paragraph -->
      <p class="hero__subtitle">
        I build complete web products — clean REST APIs, scalable backends,
        and responsive interfaces. Based in Kumasi,Ghana. Working remotely worldwide.
      </p>

      <!-- CTA buttons row -->
      <div class="hero__actions">
        <a href="#work"    class="btn-primary">View my work </a>
        <a href="#contact" class="btn-ghost">Get in touch</a>
      </div>
    </div>

    <!--  right column: profile photo  -->
    <div class="hero__right">
      <div class="hero__frame">
        <img
          src="../assets/images/bryan.JPG"
          alt="Bryan Godson — Full Stack Developer"
        />
        <!-- decorative offset border behind the photo -->
        <div class="hero__frame-border"></div>
        <!-- small location caption below the frame -->
        <div class="hero__frame-label">Kumasi,Ghana — {{currentYear}}</div>
      </div>
    </div>
    </div>
    <!-- ── Stats row — bottom left ── -->
    <div class="hero__stats">
      <!--
        v-for over the stats array defined in data().
        :key uses the label as a unique identifier.
      -->
      <div
        v-for="stat in stats"
        :key="stat.label"
        class="hero__stat"
      >
        <!-- num can contain HTML (the <span> for the + suffix), so we use v-html -->
        <div class="hero__stat-num" v-html="stat.num"></div>
        <div class="hero__stat-label">{{ stat.label }}</div>
      </div>
    </div>

    <!-- ── Scroll indicator — bottom right ── -->
    <div class="hero__scroll-hint">
      Scroll
      <div class="scroll-line"></div>
    </div>

  </section>
</template>

<script lang="ts">
export default {
  name: 'HeroSection',

  data() {
    return {
      // Stats displayed in the bottom row of the hero
      stats: [
        { num: '4<span>+</span>',  label: 'Years exp.'     },
        { num: '10<span>+</span>', label: 'Apps built'     },
        { num: '12<span>+</span>', label: 'Happy clients'  },
      ],
    }
  },
  computed:{
    currentYear(){
      return new Date().getFullYear()
    }
  },
  mounted() {
    // Hero entrance animation
    // We grab all the hero elements and stagger their fade-in on page load.
    // Note: this.$el refers to the root element of THIS component's template.

    const animItems = this.$el.querySelectorAll(
      '.hero__badge, .line, .hero__subtitle, .hero__actions, .hero__stats'
    )

    animItems.forEach((el, index) => {
      // Start invisible and shifted down
      el.style.opacity   = '0'
      el.style.transform = 'translateY(24px)'
      el.style.transition = `opacity 0.8s ease ${index * 0.12}s, transform 0.8s ease ${index * 0.12}s`

      // requestAnimationFrame waits one paint cycle so the "invisible" state
      // is actually rendered before we trigger the transition to visible.
      requestAnimationFrame(() => {
        el.style.opacity   = '1'
        el.style.transform = 'translateY(0)'
      })
    })
  },
}
</script>

<style scoped>
/* layout  */
.hero {
  min-height: 100vh;
  display: grid;
  grid-template-columns: 1fr ; /* two equal columns */
  position: relative;
  overflow: hidden;
}
.hero_with_image{
  display:grid;
  grid-template-columns: 1fr 1fr;
}

/* left column  */
.hero__left {
  padding: 160px 48px 80px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  z-index: 2;
}
/* Right column — photo */
.hero__right {
  display: flex;
  align-items: center;
  justify-content: center;
}
/* "Available" badge */
.hero__badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--accent);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  margin-bottom: 40px;
}
/* pulsing dot before the badge text */
.hero__badge::before {
  content: '';
  width: 6px; height: 6px;
  background: var(--accent);
  border-radius: 50%;
  animation: pulse 2s infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1;
    transform: scale(1);
  }
  50% {
    opacity: 0.4;
    transform: scale(0.8);
  }
}

/*  headline  */
.hero__title {
  font-size: clamp(56px, 6vw, 88px);
  font-weight: 800;
  line-height: 0.95;
  letter-spacing: -0.03em;
  margin-bottom: 8px;
}
/* each word on its own line */
.hero__title .line {
  display: block;
  overflow: hidden;
}
/* outline/stroke treatment for the italic line */
.hero__title .serif {
  font-family: var(--font-serif);
  font-weight: 400;
  font-style: italic;
  color: transparent;
  -webkit-text-stroke: 1px var(--accent);
}

/* Subtitle  */
.hero__subtitle {
  font-family: var(--font-mono);
  font-size: 13px;
  color: var(--muted);
  line-height: 1.8;
  margin: 32px 0 48px;
  max-width: 420px;
  border-left: 1px solid var(--border);
  padding-left: 20px;
}

/* Buttons  */
.hero__actions {
  display: flex;
  gap: 16px;
  align-items: center;
  margin-bottom: 100px;
}

.btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: var(--accent);
  color: var(--bg);
  font-family: var(--font-mono);
  font-size: 12px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 14px 28px;
  text-decoration: none;
  transition: background 0.2s,
  transform 0.2s;
}
.btn-primary:hover {
  background: var(--white);
  transform: translateY(-2px);
}

.btn-ghost {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  border: 1px solid var(--border);
  color: var(--muted);
  font-family: var(--font-mono);
  font-size: 12px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 14px 28px;
  text-decoration: none;
  transition: all 0.2s;
}
.btn-ghost:hover {
  border-color: var(--text);
  color: var(--text);
}


.hero__frame {
  position: relative;
  width: 380px; height: 480px;
  margin-top: 120px;
}
.hero__frame img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top;
  filter: grayscale(20%);
  position: relative;
  z-index: 2;
  display: block;
}
/* Decorative border offset behind the photo */
.hero__frame-border {
  position: absolute;
  top: 16px; left: 16px;
  right: -16px;
  bottom: -16px;
  border: 1px solid var(--accent);
  z-index: 1;
  pointer-events: none;
}
.hero__frame-label {
  position: absolute;
  bottom: -40px;
  right: 0;
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--muted);
  letter-spacing: 0.1em;
}

/* Stats  */
.hero__stats {
  position: absolute;
  bottom: 80px; left: 48px;
  display: flex; gap: 48px;
}
.hero__stat {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.hero__stat-num {
  font-size: 36px;
  font-weight: 800;
  color: var(--text);
  letter-spacing: -0.04em;
  line-height: 1;
}
/* the "+" suffix inside stat-num */
:deep(.hero__stat-num span) {
  color: var(--accent);
}
.hero__stat-label {
  font-family: var(--font-mono);
  font-size: 10px;
  color: var(--muted);
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

/* ── Scroll hint*/
.hero__scroll-hint {
  position: absolute;
  right: 48px;
  bottom: 80px;
  display: flex;
  flex-direction: column;
  align-items: center; gap: 12px;
  font-family: var(--font-mono);
  font-size: 10px;
  color: var(--muted);
  letter-spacing: 0.15em;
  text-transform: uppercase;
  writing-mode: vertical-rl;
}
.scroll-line {
  width: 1px; height: 60px;
  background: var(--border);
  position: relative; overflow: hidden;
}
/* the animated line that moves downward */
.scroll-line::after {
  content: '';
  position: absolute;
  top: -100%;
  width: 100%;
  height: 100%;
  background: var(--accent);
  animation: scrollLine 2s ease-in-out infinite;
}
@keyframes scrollLine {
  0%   {
    top: -100%;
  }
  100% {
    top: 100%;
  }
}

/* Mobile */
@media (max-width: 900px) {
  .hero {
    grid-template-columns: 1fr;
    padding-top: 100px;
  }
.hero_with_image{
  grid-template-rows: 1fr 1fr;
  grid-template-columns: 1fr ;

}
  .hero__frame {
    margin-top: -160px;
  }
  .hero__left  { padding: 120px 24px 0px 24px ; }
  .hero__stats {
    left: 24px;
    gap: 32px;
    bottom: 10px;
  }
  .hero__scroll-hint { display: none; }
}
</style>
