<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Heart</title>
    <style>
        body { margin: 0; overflow: hidden; background: #000; cursor: pointer; }
        canvas { display: block; }
    </style>
</head>
<body>
    <canvas id="c"></canvas>
    <script>
        const canvas = document.getElementById('c');
        const ctx = canvas.getContext('2d');
        let w = canvas.width = window.innerWidth;
        let h = canvas.height = window.innerHeight;
        let particles = [];
        let formed = false;

        const getHeartPoint = (t) => {
            let x = 16 * Math.pow(Math.sin(t), 3);
            let y = -(13 * Math.cos(t) - 5 * Math.cos(2*t) - 2 * Math.cos(3*t) - Math.cos(4*t));
            return { x: x * 15 + w/2, y: y * 15 + h/2 };
        };

        for (let i = 0; i < 600; i++) {
            particles.push({
                x: Math.random() * w,
                y: Math.random() * h,
                tx: Math.random() * w,
                ty: Math.random() * h,
                s: Math.random() * 0.05 + 0.02,
                c: '#ff3366'
            });
        }

        function draw() {
            ctx.fillStyle = 'rgba(0,0,0,0.1)';
            ctx.fillRect(0,0,w,h);
            particles.forEach((p, i) => {
                if (formed) {
                    let pos = getHeartPoint((i/600) * Math.PI * 2);
                    let pulse = 1 + Math.sin(Date.now() * 0.005) * 0.1;
                    p.tx = (pos.x - w/2) * pulse + w/2;
                    p.ty = (pos.y - h/2) * pulse + h/2;
                }
                p.x += (p.tx - p.x) * p.s;
                p.y += (p.ty - p.y) * p.s;
                ctx.fillStyle = p.c;
                ctx.font = '12px Arial';
                ctx.fillText('I LOVE YOU', p.x, p.y);
            });
            requestAnimationFrame(draw);
        }

        window.onclick = () => {
            formed = !formed;
            if (!formed) {
                particles.forEach(p => {
                    p.tx = Math.random() * w;
                    p.ty = Math.random() * h;
                });
            }
        };

        draw();
    </script>
</body>
</html>
