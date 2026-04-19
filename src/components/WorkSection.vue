<template>
  <section class="work section-pad" id="work">

    <div class="section-label">Selected work</div>
    <h2 class="section-title">Things I've <em>built.</em></h2>

    <div class="work__grid">

      <!--
        v-for renders one project row per item in the "projects" array.

        :class applies "work-item--inactive" when project.inProgress is true
        (disables pointer and dims the row).

        :href dynamically sets the link destination from project.link.

        We use <a> for live projects and <div> for in-progress — see v-if below.
      -->
      <template v-for="project in projects" :key="project.id">

        <!--
          v-if renders this element ONLY when project.inProgress is falsy.
          renders as a real <a> link for completed projects.
        -->
        <a
          v-if="!project.inProgress"
          :href="project.link"
          class="work-item fade-up"
        >
          <WorkItem :project="project" />
          <!-- arrow icon for live projects -->
          <span class="work-item__arrow"><ArrowRight :size="20"/> </span>
        </a>

        <!--
          v-else renders when project.inProgress is true.
        -->
        <div v-else class="work-item work-item--inactive fade-up">
          <WorkItem :project="project" />
          <!-- Badge instead of arrow -->
          <span class="work-item__badge">In Progress</span>
        </div>

      </template>

    </div>
  </section>
</template>

<script lang="ts">
// import child component
// WorkItem handles the inner content of each row (number, title, tags).
// splitting it out keeps this file focused on the list logic.
import WorkItem from './WorkItem.vue'
import {ArrowRight} from "lucide-vue-next"
export default {
  name: 'WorkSection',

  // register components used in <template>
  components: { WorkItem ,ArrowRight},

  data() {
    return {
      // Project data — each object maps to one row in the list
      projects: [
        {
          id: 1,
          num: '01',
          title: 'Trecco',
          description: 'Full-stack Fintect application with wallet integration and ERP tools',
          tags: ['React Native','Expo', 'Node.js', 'PostgreSQL'],
          link: '#',
          inProgress: false,
        },
        {
          id: 2,
          num: '02',
          title: 'Negxux',
          description: 'Multi-vendor marketplace with Flutterwave Checkout, admin panel, and inventory management',
          tags: ['TypeScript', 'Next.js', 'React Native'],
          link: '#',
          inProgress: true,
        },
        {
          id: 3,
          num: '03',
          title: 'payflow',
          description: 'A centralised platform for collection payments from customers in an efficient manner.',
          tags: ['Golang', 'React Native', 'Docker'],
          link: '#',
          inProgress: true,
        },
        {
          id: 4,
          num: '04',
          title: 'TEL- A full nextjs website for an NGO.',
          description: 'The Essence League; fully managed web app for collecting donations, showcasing events and news',
          tags: ['Next.js', 'TailwindCss', 'REST API'],
          link: '#',
          inProgress: false,
        },
        // {
        //   id: 5,
        //   num: '05',
        //   title: 'LinkDrop',
        //   description: 'OAuth2 + JWT auth service with refresh tokens, rate limiting, and session management',
        //   tags: ['Node.js', 'PostgreSQL', 'Redis'],
        //   link: '#',
        //   inProgress: false,
        // },
        // {
        //   id: 6,
        //   num: '06',
        //   title: 'NeuralNotes — AI Note-Taking App',
        //   description: 'AI-powered knowledge base with embeddings, semantic search, and GPT-4 summaries',
        //   tags: ['React', 'Python', 'OpenAI'],
        //   link: '#',
        //   inProgress: true, // this project gets the "In Progress" badge
        // },
      ],
    }
  },

  mounted() {
    // ── Scroll-triggered fade-in for each project row ─────────────────────
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible')
            observer.unobserve(entry.target)
          }
        })
      },
      { threshold: 0.1 } // fire when just 10% is visible (rows are tall)
    )
    this.$el.querySelectorAll('.fade-up').forEach((el) => observer.observe(el))
  },
}
</script>

<style scoped>
/*  section background  */
.work { background: var(--surface); }

/* stacked list  */
.work__grid {
  display: flex;
  flex-direction: column;
  gap: 5px;         /* 1px gap creates a visible divider between rows */
  margin-top: 60px;
}

/* project row — shared styles */
.work-item {
  display: grid;
  grid-template-columns: 1fr auto; /* content left, arrow/badge right */
  align-items: center;
  gap: 24px;
  background: var(--bg);
  padding: 32px 40px;
  border: 1px solid transparent;
  transition: border-color 0.3s;
  text-decoration: none;
  color: var(--text);
  position: relative;
  overflow: hidden;
}

/* subtle green wash on hover */
.work-item::before {
  content: '';
  position: absolute; inset: 0;
  background: linear-gradient(90deg, var(--accent) 0%, transparent 60%);
  opacity: 0;
  transition: opacity 0.4s;
}
.work-item:hover { border-color: var(--accent); }
.work-item:hover::before { opacity: 0.04; }

/* in-progress state  */
.work-item--inactive {
  opacity: 0.5;
  pointer-events: none; /* prevents hover / click */
}

/* arrow  */
.work-item__arrow {
  font-size: 20px;
  color: var(--muted);
  transition: transform 0.3s, color 0.3s;
  flex-shrink: 0;
}
.work-item:hover .work-item__arrow {
  transform: translate(4px, -4px);
  color: var(--accent);
}

/* "In Progress" badge */
.work-item__badge {
  font-family: var(--font-mono);
  font-size: 10px;
  color: var(--accent2);
  border: 1px solid var(--accent2);
  padding: 3px 8px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  white-space: nowrap;
  flex-shrink: 0;
}

/* mobile */
@media (max-width: 900px) {
  .work-item { padding: 24px; }
}
</style>
