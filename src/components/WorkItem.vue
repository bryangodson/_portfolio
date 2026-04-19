<template>
  <div class="work-item__inner">

    <!-- project number (e.g. "01") -->
    <span class="work-item__num">{{ project.num }}</span>

    <!-- title + description block -->
    <div class="work-item__info">
      <h3 class="work-item__title">{{ project.title }}</h3>
      <p class="work-item__desc">{{ project.description }}</p>
    </div>

    <!-- tech tags — v-for over the project's tags array -->
    <div class="work-item__tags">
      <span
        v-for="tag in project.tags"
        :key="tag"
        class="tag"
      >
        {{ tag }}
      </span>
    </div>

  </div>
</template>

<script>
export default {
  name: 'WorkItem',

  // props definition
  // props declares what data this component EXPECTS to receive from its parent.
  // Here we expect a single "project" prop which must be an Object.
  // Vue will warn in the console if the wrong type is passed.
  props: {
    project: {
      type: Object,
      required: true, // this prop is mandatory — parent MUST provide it
    },
  },

  // No data() here — this component is purely driven by props.
  // No methods either — it just displays what it's given.
}
</script>

<style scoped>
/* inner layout: num | info | tags */
.work-item__inner {
  display: grid;
  grid-template-columns: 80px 1fr auto;
  align-items: center;
  gap: 40px;
  width: 100%;
}

/* number */
.work-item__num {
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--accent);
  letter-spacing: 0.1em;
}

/* info */
.work-item__title {
  font-size: 20px;
  font-weight: 700;
  margin-bottom: 4px;
}
.work-item__desc {
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--muted);
  letter-spacing: 0.03em;
}

/* tags  */
.work-item__tags {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}
.tag {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--muted);
  border: 1px solid var(--border);
  padding: 4px 10px;
}

/* mobile — hide tags to save space*/
@media (max-width: 900px) {
  .work-item__inner { grid-template-columns: 60px 1fr; gap: 16px; }
  .work-item__tags  { display: none; }
}
</style>
