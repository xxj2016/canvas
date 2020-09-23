<template>
    <div class="cnavasInfo">
        <canvas id="canvas" width="800" height="500"></canvas>
    </div>
</template>

<script>
export default {
    name: "CanvasMove",
    data() {
        return {
            canvas: null,
            ctx: null,
            arcObj: {},
        };
    },
    mounted() {
        this.canvas = document.getElementById("canvas");
        this.ctx = this.canvas.getContext("2d");
        // this.move(); // 矩形的边缘碰撞函数
        // this.moveArc(); // 绘制碰撞圆形，对象形式
        this.moveRect();
    },
    methods: {
        move() {
            let x = 0;
            let y = 0;
            let width = 100;
            let height = 100;
            let speedX = 2;
            let speedY = 2;
            let ctx = this.ctx;
            ctx.fillStyle = "red";
            ctx.fillRect(x, y, width, height);
            setInterval(() => {
                ctx.clearRect(x, y, this.canvas.width, this.canvas.height);
                x += speedX;
                if (x > this.canvas.width - width) {
                    speedX *= -1;
                } else if (x < 0) {
                    speedX *= -1;
                }
                y += speedY;
                if (y > this.canvas.height - height) {
                    speedY *= -1;
                } else if (y < 0) {
                    speedY *= -1;
                }
                ctx.fillRect(x, y, width, height);
            }, 10);
            // this.requestmove(x,y,width,height,ctx,speedX,speedY); // 请求帧的动画过程
        },
        requestmove(x, y, width, height, ctx, speedX, speedY) {
            ctx.clearRect(x, y, this.canvas.width, this.canvas.height);
            x += speedX;
            if (x > this.canvas.width - width) {
                speedX *= -1;
            } else if (x < 0) {
                speedX *= -1;
            }
            y += speedY;
            if (y > this.canvas.height - height) {
                speedY *= -1;
            } else if (y < 0) {
                speedY *= -1;
            }
            ctx.fillRect(x, y, width, height);
            window.requestAnimationFrame(
                this.requestmove(x, y, width, height, ctx, speedX, speedY)
            );
        },
        moveArc(x, y, r, speedX, speedY) {
            this.x = x;
            this.y = y;
            this.r = r;
            this.speedX = speedX;
            this.speedY = speedY;
            this.moveUpdata = function () {
                this.x += this.speedX;
                if (this.x > this.canvas.width - this.r) {
                    this.speedX *= -1;
                } else if (this.x < 0) {
                    this.speedX *= -1;
                }
                this.y += this.speedY;
                if (this.y > this.canvas.height - this.r) {
                    this.speedY *= -1;
                } else if (this.y < 0) {
                    this.speedY *= -1;
                }
            };
        },
        moveRect() {
            // 面向对象编程
            function Rect(
                x,
                y,
                width,
                height,
                color,
                speedX,
                speedY,
                ctx,
                canvas
            ) {
                this.x = x;
                this.y = y;
                this.width = width;
                this.height = height;
                this.color = color;
                this.speedX = speedX;
                this.speedY = speedY;
                this.ctxRect = ctx;
                this.canvas = canvas;
            }
            Rect.prototype.draw = function () {
                this.ctxRect.beginPath();
                this.ctxRect.fillStyle = this.color;
                this.ctxRect.fillRect(this.x, this.y, this.width, this.height);
                this.ctxRect.closePath();
            };
            Rect.prototype.move = function () {
                this.x += this.speedX;
                if (this.x > this.canvas.width - this.width) {
                    this.speedX *= -1;
                } else if (this.x < 0) {
                    this.speedX *= -1;
                }
                this.y += this.speedY;
                if (this.y > this.canvas.height - this.height) {
                    this.speedY *= -1;
                } else if (this.y < 0) {
                    this.speedY *= -1;
                }
            };
            let rect1 = new Rect(
                0,
                100,
                100,
                100,
                "red",
                2,
                2,
                this.ctx,
                this.canvas
            );
            let rect2 = new Rect(
                300,
                100,
                100,
                100,
                "blue",
                -2,
                -2,
                this.ctx,
                this.canvas
            );
            // rect1.draw();
            // rect2.draw()
            let animate = () => {
                this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
                rect1.draw();
                rect1.move();
                rect2.draw();
                rect2.move();
                let rect1Min = rect1.x;
                let rect1Max = rect1.x + rect1.width;
                let rect2Min = rect2.x;
                let rect2Max = rect2.x + rect2.width;
                let min = Math.max(rect1Min, rect2Min);
                let max = Math.min(rect1Max, rect2Max);
                if (min < max) {
                    rect1.speedX *= -1;
                    rect1.speedY *= 1;
                    rect2.speedX *= -1;
                    rect2.speedY *= 1;
                }
                window.requestAnimationFrame(animate);
            };
            animate();
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#canvas {
    border: 1px solid black;
}
</style>
