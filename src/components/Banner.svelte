<script lang="ts">
  import { Runtime, Inspector } from "@observablehq/runtime";
import { onMount } from "svelte";
  export var title;

  function notebook(runtime, observer) {
    const main = runtime.module();
    main
      .variable(observer("animation"))
      .define("animation", ["DOM", "width", "mutable speed"], function* (
        DOM,
        width,
        $0
      ) {
        const height = 300;
        const min = 0;
        const max = 100;
        const max2 = Math.pow(max, 2);
        const context = DOM.context2d(width, height);
        const canvas = context.canvas;

        const n = 100;
        const particles = new Array(n);
        for (let i = 0; i < n; ++i) {
          particles[i] = {
            x: Math.random() * (width + max * 2) - max,
            y: Math.random() * (height + max * 2) - max,
            vx: 0,
            vy: 0,
          };
        }

        while (true) {
          for (let i = 0; i < n; ++i) {
            const p = particles[i];
            p.x += p.vx;
            if (p.x < -max) p.x += width + max;
            else if (p.x > width + max) p.x -= width + max;
            p.y += p.vy;
            if (p.y < -max) p.y += height + max;
            else if (p.y > height + max) p.y -= height + max;
            p.vx += $0.value * (Math.random() - 0.5) - 0.01 * p.vx;
            p.vy += $0.value * (Math.random() - 0.5) - 0.01 * p.vy;
          }

          for (let i = 0; i < n; ++i) {
            for (let j = i + 1; j < n; ++j) {
              const pi = particles[i];
              const pj = particles[j];
              const dist = Math.pow(pi.x - pj.x, 2) + Math.pow(pi.y - pj.y, 2);
              if (dist < max2) {
                context.beginPath();
                context.strokeStyle = `hsl(200,80%,${
                  100 - ((max2 - dist) / max2) * 25
                }%)`;
                context.moveTo(pi.x, pi.y);
                context.lineTo(pj.x, pj.y);
                context.stroke();
              }
            }
          }

          yield canvas;
        }
      });
    main.define("initial speed", function () {
      return 0.1;
    });
    main
      .variable(observer("mutable speed"))
      .define(
        "mutable speed",
        ["Mutable", "initial speed"],
        (M, _) => new M(_)
      );
    main
      .variable(observer("speed"))
      .define("speed", ["mutable speed"], (_) => _.generator);

    return main;
  }

  // reference to mutable speed
  let speed = { value: 0.1 };

  function draw(node) {
    const runtime = new Runtime();
    runtime.module(notebook, (name) => {
      if (name === "animation") {
        return new Inspector(node);
      }
      if (name === "mutable speed") {
        return {
          fulfilled: (value) => {
            speed = value;
          },
        };
      }
    });
  }

</script>

<style lang="scss">
  .banner-container {
    position: relative;
  }
  .banner-inner {
    overflow: hidden;
  }

  .banner-title {
    position: absolute;
    bottom: 2rem;
    left: 2rem;
  }
</style>

<div class="banner-container">
  <div class="banner-inner" use:draw />
  <h1 class="banner-title">{title}</h1>
</div>
