<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Animated Heart</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #000;
            overflow: hidden;
        }

        canvas {
            display: block;
            width: 100vw;
            height: 100vh;
        }
    </style>
</head>

<body>

<canvas id="heart"></canvas>

<script>
const canvas = document.getElementById("heart");
const ctx = canvas.getContext("2d");

function resize() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}

resize();
window.addEventListener("resize", resize);

// الألوان
const colors = [
    "#ff0000",
    "#ff00ff",
    "#8a2be2",
    "#0066ff",
    "#00ffff",
    "#00ff66",
    "#ffff00",
    "#ff8800"
];

// نقاط القلب
let points = [];

const totalPoints = 160;

for (let i = 0; i < totalPoints; i++) {

    const t = i * Math.PI * 2 / totalPoints;

    const x =
        16 * Math.pow(Math.sin(t), 3);

    const y =
        13 * Math.cos(t)
        - 5 * Math.cos(2 * t)
        - 2 * Math.cos(3 * t)
        - Math.cos(4 * t);

    points.push({
        x: x,
        y: y
    });
}

// Animation
let index = 0;
let lines = [];

function animate() {

    ctx.fillStyle = "rgba(0, 0, 0, 0.12)";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    const centerX = canvas.width / 2;
    const centerY = canvas.height / 2;

    const scale = Math.min(
        canvas.width,
        canvas.height
    ) / 32;

    // نقطة البداية
    const startX = centerX;
    const startY = centerY + scale * 10;

    // إضافة خط جديد
    if (index < points.length) {

        const p = points[index];

        lines.push({
            x: centerX + p.x * scale,
            y: centerY - p.y * scale,
            color: colors[
                Math.floor(Math.random() * colors.length)
            ]
        });

        index++;
    }

    // رسم الخطوط
    lines.forEach(line => {

        ctx.beginPath();

        ctx.moveTo(startX, startY);

        ctx.lineTo(line.x, line.y);

        ctx.strokeStyle = line.color;
        ctx.lineWidth = 1.5;

        ctx.shadowBlur = 8;
        ctx.shadowColor = line.color;

        ctx.stroke();

        // نقطة مضيئة
        ctx.beginPath();

        ctx.arc(
            line.x,
            line.y,
            3,
            0,
            Math.PI * 2
        );

        ctx.fillStyle = line.color;
        ctx.shadowBlur = 15;
        ctx.shadowColor = line.color;

        ctx.fill();
    });

    ctx.shadowBlur = 0;

    // ملي يكمل القلب
    if (index >= points.length) {

        setTimeout(() => {
            index = 0;
            lines = [];
        }, 1200);
    }

    requestAnimationFrame(animate);
}

animate();
</script>

</body>
</html>
