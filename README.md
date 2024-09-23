<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>自由贪吃蛇</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.9.1/gsap.min.js"></script>
  <style>
    body,html{padding:0;margin:0;overscroll-behavior:none;overflow:hidden}.links{position:fixed;bottom:10px;right:10px;font-size:18px;font-family:sans-serif;background-color:white;padding:10px}a{text-decoration:none;color:black;margin-left:1em}a:hover{text-decoration:underline}a img.icon{display:inline-block;height:1em;margin:0 0 -0.1em 0.3em}
  </style>
</head>

<body>
  <canvas></canvas>
  <div class="links">
    <a href="https://dev.to/uuuuuulala/coding-an-interactive-and-damn-satisfying-cursor-7-simple-steps-2kb-of-code-1c8b"
      target="_blank">
  </div>
</body>
<script>
  const canvas = document.querySelector("canvas");
  const ctx = canvas.getContext('2d');
  let mouseMoved = false;

  const pointer = {
    x: .5 * window.innerWidth,
    y: .5 * window.innerHeight,
  }
  const params = {
    pointsNumber: 40,
    widthFactor: .3,
    mouseThreshold: .6,
    spring: .4,
    friction: .5
  };

  const trail = new Array(params.pointsNumber);
  for (let i = 0; i < params.pointsNumber; i++) {
    trail[i] = {
      x: pointer.x,
      y: pointer.y,
      dx: 0,
      dy: 0,
    }
  }

  window.addEventListener("click", e => {
    updateMousePosition(e.pageX, e.pageY);
  });
  window.addEventListener("mousemove", e => {
    mouseMoved = true;
    updateMousePosition(e.pageX, e.pageY);
  });
  window.addEventListener("touchmove", e => {
    mouseMoved = true;
    updateMousePosition(e.targetTouches[0].pageX, e.targetTouches[0].pageY);
  });

  function updateMousePosition(eX, eY) {
    pointer.x = eX;
    pointer.y = eY;
  }

  setupCanvas();
  update(0);
  window.addEventListener("resize", setupCanvas);


  function update(t) {

    if (!mouseMoved) {
      pointer.x = (.5 + .3 * Math.cos(.002 * t) * (Math.sin(.005 * t))) * window.innerWidth;
      pointer.y = (.5 + .2 * (Math.cos(.005 * t)) + .1 * Math.cos(.01 * t)) * window.innerHeight;
    }

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    trail.forEach((p, pIdx) => {
      const prev = pIdx === 0 ? pointer : trail[pIdx - 1];
      const spring = pIdx === 0 ? .4 * params.spring : params.spring;
      p.dx += (prev.x - p.x) * spring;
      p.dy += (prev.y - p.y) * spring;
      p.dx *= params.friction;
      p.dy *= params.friction;
      p.x += p.dx;
      p.y += p.dy;
    });

    ctx.lineCap = "round";
    ctx.beginPath();
    ctx.moveTo(trail[0].x, trail[0].y);

    for (let i = 1; i < trail.length - 1; i++) {
      const xc = .5 * (trail[i].x + trail[i + 1].x);
      const yc = .5 * (trail[i].y + trail[i + 1].y);
      ctx.quadraticCurveTo(trail[i].x, trail[i].y, xc, yc);
      ctx.lineWidth = params.widthFactor * (params.pointsNumber - i);
      ctx.stroke();
    }
    ctx.lineTo(trail[trail.length - 1].x, trail[trail.length - 1].y);
    ctx.stroke();

    window.requestAnimationFrame(update);
  }

  function setupCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
</script>
</html>
