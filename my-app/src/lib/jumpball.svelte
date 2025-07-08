<script>
    import { onMount } from "svelte";
  
    let canvas;
    let ctx;
    let width = 400;
    let height = 300;
  
    let ball = {
      x: width / 2,
      y: height - 20,
      radius: 15,
      color: "blue",
      vy: 0,
      gravity: 0.6,
      jumpStrength: -10
    };
  
    function jump() {
      if (ball.y >= height - ball.radius - 5) {
        ball.vy = ball.jumpStrength;
      }
    }
  
    function update() {
      ball.vy += ball.gravity;
      ball.y += ball.vy;
  
      // Prevent ball from falling through the ground
      if (ball.y > height - ball.radius) {
        ball.y = height - ball.radius;
        ball.vy = 0;
      }
    }
  
    function draw() {
      ctx.clearRect(0, 0, width, height);
  
      // Draw ground
      ctx.fillStyle = "#444";
      ctx.fillRect(0, height - 5, width, 5);
  
      // Draw ball
      ctx.beginPath();
      ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
      ctx.fillStyle = ball.color;
      ctx.fill();
      ctx.closePath();
    }
  
    function loop() {
      update();
      draw();
      requestAnimationFrame(loop);
    }
  
    onMount(() => {
      ctx = canvas.getContext("2d");
  
      // Start game loop
      loop();
  
      // Handle key and mouse/touch
      window.addEventListener("keydown", (e) => {
        if (e.code === "Space") jump();
      });
      window.addEventListener("click", jump);
      window.addEventListener("touchstart", jump);
    });
  </script>
  
  <canvas bind:this={canvas} width={width} height={height} class="border mx-auto block mt-4"></canvas>
  
  <style>
    canvas {
      background: #eee;
      border: 2px solid #333;
    }
  </style>