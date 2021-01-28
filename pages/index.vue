<template>
  <div>
    <canvas id="cvs"></canvas>
  </div>
</template>

<script>
export default {
    layout: 'matrix',
    data: () => {
      return {
        chars: "φ₪田由甲申ω甴电甶男甸甹町画甼甽甾甿畀畁畂ξ畃畄畅畆畇Θ畑ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890!@#$%^&*(Ω)_-/+.,`\"\\|{};'[]?=~:",
        fontSize: 18,
        drops: [],
      };
    },
    mounted() {
      let chars = this.chars;
      let fontSize = this.fontSize;

      let cvs = document.getElementById("cvs");
      let ctx = cvs.getContext("2d");

      cvs.height = window.innerHeight;
      cvs.width = window.innerWidth;

      chars = chars.split("");
      let columns = cvs.width / fontSize; // Number of columns for the rain.

      let drops = [];
      for (let x = 0; x < columns; x++)
        drops[x] = 1;

      function draw() {
        // Black background for the canvas.
        // Translucent background to show trail.
        ctx.fillStyle = "rgba(0, 0, 0, 0.08)";
        ctx.fillRect(0, 0, cvs.width, cvs.height);
        ctx.fillStyle = "#00FF00"; // The code rain text color.
        ctx.font = fontSize + "px monospace"; // The code rain text font.

        for (let i = 0; i < drops.length; i++) {
          let text = chars[Math.floor(Math.random() * chars.length)];

          ctx.fillText(text, i * fontSize, drops[i] * fontSize);

          // Send randomly the drop back to the top after it has crossed the screen.
          // Add randomness to the reset, in order to make the drops scattered on the Y axis.
          if (drops[i] * fontSize > cvs.height && Math.random() > 0.985)
            drops[i] = 0;

          // Increment Y coordinate.
          drops[i]++;
        }
      }

      setInterval(draw, 45);

    }
  }
</script>
<style scoped>
  * {
    margin: 0;
    padding: 0;
  }

  html {
    width: 100%;
    height: 100%;
    background: radial-gradient(circle, #FFFFFF 0%, #AAAAAA 100%) no-repeat;
    overflow-x: hidden;
    overflow-y: hidden;
  }

  body {
    text-align: center;
    display: table;
    background: black;
    width: 100%;
    height: 100%;
    overflow-x: hidden;
    overflow-y: hidden;
  }

  canvas {
    display: block;
  }

  #text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: #00B700;
    z-index: 1;
    text-shadow: -2px 2px 3px #22FF22, 2px -2px 3px #22FF22, 2px 2px 3px #22FF22, -2px -2px 3px #22FF22;
    box-sizing: border-box;
    font-weight: 700;
    font-family: monospace;
    font-size: 15em;
  }

  @keyframes cursor {
    0% {
      opacity: 0;
    }
    40% {
      opacity: 0;
    }
    50% {
      opacity: 1;
    }
    90% {
      opacity: 1;
    }
    100% {
      opacity: 0;
    }
  }
</style>
