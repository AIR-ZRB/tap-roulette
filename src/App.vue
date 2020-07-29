<template>
    <div id="app">
        <button
            class="start-button"
            @touchstart="(event) => random(event)"
        ></button>
        <div class="content">
            <!-- <p>sdfaf</p> -->
            <div
                v-for="(item, index) in touchPosition"
                :key="item.left + index"
                class="circle"
                :style="touchPosition[index]"
            >
                <span class="triangle" :style="touchPosition[index]"></span>
            
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            touchPosition: [],
            duration: 3,
            speed: 200,
            currentIndex: 0,
            currentTime: 0,
            timer: null,
            color: [
                "orange",
                "pink",
                "skyblue",
                "purple",
                "yellow",
                "blue",
                "green",
            ],
        };
    },
    methods: {
        random() {
            this.time = 0;

            if (this.touchPosition.length === 0) return;
            if (this.timer) clearTimeout(this.timer);

            this.timer = setTimeout(this.running, this.speed);
        },
        running() {
            this.time += this.speed / 1000;
            const stochastic = parseInt(
                Math.random() * this.touchPosition.length
            );

            this.touchPosition.forEach((item) => {
                item.opacity = 0.1;
            });

            this.touchPosition[stochastic].opacity = 1;

            if (this.time <= this.duration)
                this.timer = setTimeout(this.running, this.speed);
        },
        reset(event) {
            this.touchPosition = [];
            this.color = [
                "orange",
                "pink",
                "skyblue",
                "purple",
                "yellow",
                "blue",
                "green",
            ];
            event.touches.forEach((item) => {
                const random = parseInt(Math.random() * this.color.length);
                const domWidth = 140;

                this.touchPosition.push({
                    left: item.clientX - domWidth / 2 + "px",
                    top: item.clientY - domWidth / 2 + "px",
                    opacity: 1,
                    background: this.color[random],
                });
                console.log(this.color[random]);
                this.color.splice(random, 1);
            });
        },
    },

    mounted() {
        // 1. 可以通过event里的touches来获取当前正在touch坐标
        // 2. 通过对比touchstart/touchend里的touches就可以对比那只手指离开了
        const content = document.querySelector(".content");
        content.ontouchstart = (event) => {
            this.reset(event);
        };
        content.ontouchend = (event) => {
            this.reset(event);
        };
    },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
html,
body {
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #000;
}
#app {
    width: 100%;
    height: 100%;
    overflow: hidden;
}
.circle {
    width: 140px;
    height: 140px;
    border-radius: 50%;
    position: absolute;
    transition: all 0.5s;
    opacity: 1;
    z-index: 2;
    animation: rotation 2s linear infinite;
}

@keyframes rotation {
    to {
        transform: rotate(0deg);
    }
    from {
        transform: rotate(360deg);
    }
}

.triangle {
    display: block;
    width: 20%;
    height: 10%;
    transform: rotate(45deg);

    opacity: 1;
    z-index: -1;
}

.start-button {
    width: 45px;
    height: 45px;
    border-radius: 50%;
    border: 5px solid #fff;
    background-color: #fff;
    background-clip: content-box;
    padding: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -22px;
    margin-top: -22px;
    outline: none;
    z-index: 999;
}
.content {
    width: 100%;
    height: 100%;
}
</style>
