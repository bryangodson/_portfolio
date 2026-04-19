<template>
  <!--
    These two divs are positioned fixed (they stay on screen while scrolling).
    Their x/y position is updated every frame via JavaScript below.
    "ref" connects these HTML elements to script variables.
  -->
  <div class="cursor"      ref="cursorEl"  />
  <div class="cursor-ring" ref="cursorRingEl" />
</template>

<script lang="ts">
// Vue 3 Composition API imports
// ref()         - creates a reactive reference to a value (or DOM element)
// onMounted()   - runs code after the component is added to the page
// onUnmounted() - runs code when the component is removed (cleanup)
import { ref, onMounted, onUnmounted } from 'vue'

export default {
  name: 'AppCursor',

  setup() {
    // Template refs (links to DOM elements)
    // When ref="" matches a variable name here, Vue sets it to the DOM element
    const cursorEl     = ref(null)
    const cursorRingEl = ref(null)

    // Tracking variables
    // Mouse position — updated instantly on every mousemove event
    let mouseX = 0, mouseY = 0
    // Ring position — interpolated  toward mouse for a lag effect
    let ringX  = 0, ringY  = 0
    // Store the animation frame ID so we can cancel it on unmount
    let animFrameId

    // Mouse move handler
    // Every time the mouse moves, store the new coordinates
    function onMouseMove(e) {
      mouseX = e.clientX
      mouseY = e.clientY
    }

    // Hover handlers
    // When hovering interactive elements, grow the cursor for feedback
    function addHover()    {
      cursorEl.value?.classList.add('hovered')
      cursorRingEl.value?.classList.add('hovered')
    }
    function removeHover() {
      cursorEl.value?.classList.remove('hovered')
      cursorRingEl.value?.classList.remove('hovered')
    }

    //  Animation loop
    // requestAnimationFrame runs ~60 times per second, keeping movement smooth.
    // The dot jumps straight to the mouse. The ring lerps (linear interpolation)
    // toward the mouse — creating a smooth trailing effect.
    function animate() {
      // Move dot cursor exactly to mouse position
      if (cursorEl.value) {
        cursorEl.value.style.left = mouseX + 'px'
        cursorEl.value.style.top  = mouseY + 'px'
      }

      // Lerp: ringX moves 12% closer to mouseX each frame
      // Formula: current + (target - current) * speed
      ringX += (mouseX - ringX) * 0.12
      ringY += (mouseY - ringY) * 0.12

      if (cursorRingEl.value) {
        cursorRingEl.value.style.left = ringX + 'px'
        cursorRingEl.value.style.top  = ringY + 'px'
      }

      // Schedule the next frame — this creates the continuous loop
      animFrameId = requestAnimationFrame(animate)
    }

    // onMounted — runs once after the component renders
      onMounted(() => {
      // Start listening for mouse movement globally
      document.addEventListener('mousemove', onMouseMove)

      // Attach hover listeners to all interactive elements
      // querySelectorAll returns a NodeList (not an Array), so we spread it
      const hoverTargets = document.querySelectorAll('a, button, .work-item, .about-card')
      hoverTargets.forEach(el => {
        el.addEventListener('mouseenter', addHover)
        el.addEventListener('mouseleave', removeHover)
      })

      // Kick off the animation loop
      animate()
    })

    // onUnmounted — cleanup when component is destroyed
    // Always remove event listeners you added! Forgetting this causes memory leaks.
    onUnmounted(() => {
      document.removeEventListener('mousemove', onMouseMove)
      cancelAnimationFrame(animFrameId) // stop the loop
    })

    // Expose template refs to <template> so ref="" can bind to them
    return { cursorEl, cursorRingEl }
  }
}
</script>

<style scoped>
/*
  "scoped" means these styles ONLY apply to elements inside this component.
  Vue adds a unique data attribute (e.g. data-v-xxxxxx) to scope them.
*/

.cursor {
  position: fixed;             /* stays on screen regardless of scroll */
  width: 10px;
  height: 10px;
  background: var(--accent);
  border-radius: 50%;          /* makes it a circle */
  pointer-events: none;        /* DON'T block clicks on elements beneath */
  z-index: 9999;               /* above everything */
  transform: translate(-50%, -50%); /* center it on the actual mouse point */
  transition: width 0.2s, height 0.2s, background 0.2s;
  mix-blend-mode: difference;  /* inverts colours under the cursor — cool effect */
}

/* Ring cursor  */
.cursor-ring {
  position: fixed;
  width: 36px;
  height: 36px;
  border: 1px solid rgba(200, 240, 85, 0.5);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9998;
  transform: translate(-50%, -50%);
  transition: width 0.25s, height 0.25s,
  border-color 0.2s;
}

/* Hover state — both elements grow  */
.cursor.hovered {
  width: 20px;
  height: 20px;
}
.cursor-ring.hovered {
  width: 60px;
  height: 60px;
  border-color: var(--accent);
}
</style>
