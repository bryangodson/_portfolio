<template>
  <footer class="footer">

    <!-- Left: copyright with dynamic year from computed property -->
    <p class="footer__copy">
      &copy; {{ currentYear }} Bryan Godson - <span class="footer__accent">bryan.godson</span>
    </p>

    <!-- Right: fun tagline -->
    <p class="footer__tagline">No frameworks harmed, but I 'Vued'</p>

  </footer>
</template>

<script>
export default {
  name: 'AppFooter',

  // computed
  // computed properties are like data(), but they're derived/calculated values.
  // They're cached — Vue only re-runs them when their dependencies change.
  // Use computed when you need a value that depends on other reactive data.
  computed: {
    currentYear() {
      return new Date().getFullYear()
    },
  },
}
</script>

<style scoped>
/* Footer layout */
.footer {
  border-top: 1px solid var(--border);
  padding: 32px 48px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/*  Text styles  */
.footer__copy,
.footer__tagline {
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--muted);
  letter-spacing: 0.05em;
}

/* Accent highlight on the domain name */
.footer__accent { color: var(--accent); }

/* Mobil*/
@media (max-width: 600px) {
  .footer {
    flex-direction: column; /* stack vertically */
    gap: 8px;
    text-align: center;
    padding: 24px;
  }
}
</style>
