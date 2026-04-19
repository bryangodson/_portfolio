<template>
  <section class="skills section-pad" id="skills">

    <div class="section-label">Tech Stack</div>
    <h2 class="section-title">What I <em>work with.</em></h2>

    <!--
      outer grid: three columns, one per skill group.
      the thin border + background trick creates 1px dividers between cells.
    -->
    <div class="skills__grid">

      <!--
        outer v-for: iterates over the "groups" array — Frontend, Backend, Tools.
        each group becomes one column.
      -->
      <div
        v-for="group in groups"
        :key="group.category"
        class="skills__group"
        ref="skillGroups"
      >
        <!-- category heading e.g. "Frontend" -->
        <h3 class="skills__category">{{ group.category }}</h3>

        <!--
          inner v-for: iterates over skill objects inside each group.
          this is "nested v-for" - a loop inside a loop.
        -->
        <div
          v-for="skill in group.skills"
          :key="skill.name"
          class="skill-item"
        >
          <!-- skill name -->
          <span class="skill-item__name">{{ skill.name }}</span>

          <!-- progress bar wrapper -->
          <div class="skill-item__bar-wrap">
            <!--
              :style is Vue's dynamic inline style binding.
              we set the width using the skill's "pct" value (e.g. "95%").
              initially 0% — animated to full width by the IntersectionObserver below.
            -->
            <div
              class="skill-item__bar"
              :style="{ width: animatedBars ? skill.pct : '0%' }"
            ></div>
          </div>

          <!-- text label e.g. "Expert" -->
          <span class="skill-item__level">{{ skill.level }}</span>
        </div>

      </div>
    </div>

  </section>
</template>

<script lang="ts">
export default {
  name: 'SkillsSection',

  data() {
    return {
      // controls whether bars animate to their target width.
      // starts false (bars are 0-width), flips to true when section is visible.
      animatedBars: false,

      // skill data — grouped by category.
      // each skill: name (display), pct (bar fill), level (label text)
      groups: [
        {
          category: 'Frontend',
          skills: [
            {
              name: 'React/React Native',
              pct: '95%',
              level: 'Adv.'
            },
            {
              name: 'TypeScript',
              pct: '95%',
              level: 'Expert'
            },
            {
              name: 'Next.js',
              pct: '88%',
              level: 'Expert'
            },
            {
              name: 'HTML / CSS',
              pct: '98%',
              level: 'Master'
            },
            {
              name:'Vue',
              pct:'50%',
              level:'Inter.'
            }
          ],
        },
        {
          category: 'Backend',
          skills: [
            {
              name: 'Node.js',
              pct: '92%',
              level: 'Expert'
            },
            {
              name: 'PostgreSQL',
              pct: '88%',
              level: 'Adv.'
            },
            {
              name: 'REST APIs',
              pct: '95%',
              level: 'Expert'
            },
            {
              name: 'Express.js',
              pct: '78%',
              level: 'Solid'
            },
            {
              name:'Golang',
              pct:'50%',
              level:'Inter.'
            }
          ],
        },
        {
          category: 'Tools & DevOps',
          skills: [
            {
              name: 'Docker',
              pct: '85%',
              level: 'Adv.'
            },
            {
              name: 'AWS',
              pct: '50%',
              level: 'Inter.'
            },
            {
              name: 'Git / CI',
              pct: '92%',
              level: 'Expert'
            },
            {
              name: 'Redis',
              pct: '75%',
              level: 'Solid'
            },
            {
              name:'Postman',
              pct:'90%',
              level:'Expert'
            }
          ],
        },
      ],
    }
  },

  mounted() {
    // animate skill bars on scroll
    // we watch the whole section. when it becomes visible, we flip
    // animatedBars to true — which triggers the CSS transition on each bar.

    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            // $nextTick waits for Vue to process any pending DOM updates
            // before we change animatedBars. This ensures bars start at 0%
            // in the DOM before we animate them to their target width.
            this.$nextTick(() => {
              this.animatedBars = true
            })
            observer.unobserve(entry.target) // only animate once
          }
        })
      },
      { threshold: 0.3 } // fire when 30% of the section is visible
    )

    // Observe this component's root element (the <section>)
    observer.observe(this.$el)
  },
}
</script>

<style scoped>
/*  grid layout — three equal columns */
.skills__grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1px;                /* tiny gap to act as a divider line */
  background: var(--border); /* background shows through the gap = border effect */
  margin-top: 60px;
}

/*  individual column */
.skills__group {
  background: var(--bg); /* fills over the --border background, creating column bg */
  padding: 40px;
}

/*  category heading  */
.skills__category {
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 28px;
}

/* skill row  */
.skill-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 0;
  border-bottom: 1px solid var(--border);
}
.skill-item:last-child { border-bottom: none; }

.skill-item__name {
  font-size: 15px;
  font-weight: 600;
  flex: 1; /* take up remaining space */
}

/* progress bar */
.skill-item__bar-wrap {

  width: 80px; height: 3px;
  background: var(--border);
  position: relative;
  overflow: hidden;
  margin: 0 12px;
}
.skill-item__bar {
  position: absolute;
  top: 0; left: 0; height: 100%;
  background: var(--accent);
  /* smooth transition when width changes from 0% to target value */
  transition: width 1.2s cubic-bezier(0.16, 1, 0.3, 1);
}

/* level label  */
.skill-item__level {
  font-family: var(--font-mono);
  font-size: 10px;
  color: var(--muted);
  width: 50px;
  text-align: right;
}

/* mobile  */
@media (max-width: 900px) {
  .skills__grid {
    grid-template-columns: 1fr; /* stack on mobile */
  }
}
</style>
