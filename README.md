<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Bosque VR SBS</title>
    <style>
      body {
        margin: 0;
        display: flex;
        overflow: hidden;
      }
      canvas {
        width: 50vw;
        height: 100vh;
      }
    </style>
  </head>
  <body>
    <canvas id="leftEye"></canvas>
    <canvas id="rightEye"></canvas>

    <script>
      function createScene(ctx, offsetX) {
        // Cielo
        ctx.fillStyle = "#87CEEB";
        ctx.fillRect(0, 0, ctx.canvas.width, ctx.canvas.height);

        // Suelo
        ctx.fillStyle = "#228B22";
        ctx.fillRect(0, ctx.canvas.height / 2, ctx.canvas.width, ctx.canvas.height / 2);

        // Árboles con perspectiva
        for (let i = 0; i < 20; i++) {
          let x = Math.random() * ctx.canvas.width;
          let y = ctx.canvas.height / 2 - Math.random() * 50;
          let height = 80 + Math.random() * 50;

          // Tronco
          ctx.fillStyle = "#8B4513";
          ctx.fillRect(x + offsetX, y + 40, 10, height);

          // Copa
          ctx.beginPath();
          ctx.fillStyle = "#006400";
          ctx.arc(x + offsetX + 5, y + 40, 30, 0, 2 * Math.PI);
          ctx.fill();
        }
      }

      const leftCanvas = document.getElementById('leftEye');
      const rightCanvas = document.getElementById('rightEye');

      [leftCanvas, rightCanvas].forEach(canvas => {
        canvas.width = window.innerWidth / 2;
        canvas.height = window.innerHeight;
      });

      const leftCtx = leftCanvas.getContext('2d');
      const rightCtx = rightCanvas.getContext('2d');

      createScene(leftCtx, -10);  // Simula el ojo izquierdo
      createScene(rightCtx, 10);  // Simula el ojo derecho
    </script>
  </body>
</html>
