
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>霓虹灯数字时钟</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.9.1/gsap.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
            background: #2f363e
        }

        .clock {
            position: relative;
            width: 280px;
            height: 280px;
            display: flex;
            justify-content: center;
            align-items: center;
            scale: 2;
            box-shadow: inset 5px 5px 25px rgba(0, 0, 0, 0.25);
            border-radius: 50%
        }

        #time {
            position: relative;
            width: 250px;
            height: 250px;
            display: flex;
            justify-content: center;
            align-items: center
        }

        #time .circle {
            position: absolute;
            display: flex;
            justify-content: center;
            align-items: center
        }

        #time .circle svg {
            position: relative;
            transform: rotate(270deg)
        }

        #time .circle:nth-child(1) svg {
            width: 250px;
            height: 250px
        }

        #time .circle:nth-child(2) svg {
            width: 210px;
            height: 210px
        }

        #time .circle:nth-child(3) svg {
            width: 170px;
            height: 170px
        }

        #time .circle svg circle {
            width: 100%;
            height: 100%;
            fill: transparent;
            stroke-width: 5;
            stroke: var(--clr);
            transform: translate(5px, 5px);
            opacity: 0.25
        }

        #time .circle:nth-child(1) svg circle {
            stroke-dasharray: 760;
            stroke-dashoffset: 760
        }

        #time .circle:nth-child(2) svg circle {
            stroke-dasharray: 630;
            stroke-dashoffset: 630
        }

        #time .circle:nth-child(3) svg circle {
            stroke-dasharray: 510;
            stroke-dashoffset: 510
        }

        .dots {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: flex-start;
            justify-content: center;
            z-index: 10
        }

        .dots::before {
            content: "";
            position: absolute;
            top: -3px;
            width: 15px;
            height: 15px;
            background: var(--clr);
            border-radius: 50%;
            box-shadow: 0 0 20px var(--clr), 0 0 40px var(--clr), 0 0 60px var(--clr), 0 0 80px var(--clr)
        }

        .niddles {
            position: absolute;
            width: 200px;
            height: 200px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            z-index: 10
        }

        .niddles i {
            position: absolute;
            width: 2px;
            background: var(--clr2);
            height: 50%;
            opacity: 0.75;
            border-radius: 6px;
            transform-origin: bottom;
            transform: scaleY(0.5)
        }

        .niddles.niddles2 {
            width: 170px;
            height: 170px;
            z-index: 9
        }

        .niddles.niddles2 i {
            width: 3px
        }

        .niddles.niddles3 {
            width: 140px;
            height: 140px;
            z-index: 8
        }

        .niddles.niddles3 i {
            width: 4px
        }

        #time span {
            position: absolute;
            inset: 55px;
            text-align: center;
            color: #999;
            font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
            transform: rotate(calc(30deg * var(--i)))
        }

        #time span b {
            font-size: 0.75em;
            font-weight: 500;
            display: inline-block;
            transform: rotate(calc(-30deg * var(--i)))
        }
    </style>
</head>

<body>
<div class="clock">
    <div id="time">
        <div class="circle" style="--clr:#ff2972">
            <div class="dots sec_dot"></div>
            <svg>
                <circle cx="120" cy="120" r="120" id="ss"></circle>
            </svg>
        </div>
        <div class="circle" style="--clr:#fee800">
            <div class="dots min_dot"></div>
            <svg>
                <circle cx="100" cy="100" r="100" id="mm"></circle>
            </svg>
        </div>
        <div class="circle" style="--clr:#04fc43">
            <div class="dots hr_dot"></div>
            <svg>
                <circle cx="80" cy="80" r="80" id="hh"></circle>
            </svg>
        </div>
        <div class="niddles" style="--clr2:#ff2972;" id="sc"><i></i></div>
        <div class="niddles niddles2" style="--clr2:#fee800;" id="mn"><i></i></div>
        <div class="niddles niddles3" style="--clr2:#04fc43;" id="hr"><i></i></div>

        <span style="--i:1;"><b>1</b></span>
        <span style="--i:2;"><b>2</b></span>
        <span style="--i:3;"><b>3</b></span>
        <span style="--i:4;"><b>4</b></span>
        <span style="--i:5;"><b>5</b></span>
        <span style="--i:6;"><b>6</b></span>
        <span style="--i:7;"><b>7</b></span>
        <span style="--i:8;"><b>8</b></span>
        <span style="--i:9;"><b>9</b></span>
        <span style="--i:10;"><b>10</b></span>
        <span style="--i:11;"><b>11</b></span>
        <span style="--i:12;"><b>12</b></span>
    </div>
</div>
</body>
<script>
    setInterval(() => {
        let hh = document.getElementById("hh");
        let mm = document.getElementById("mm");
        let ss = document.getElementById("ss");
        let sec_dot = document.querySelector(".sec_dot");
        let min_dot = document.querySelector(".min_dot");
        let hr_dot = document.querySelector(".hr_dot");
        let hr = document.getElementById("hr");
        let mn = document.getElementById("mn");
        let sc = document.getElementById("sc");
        let h = new Date().getHours();
        let m = new Date().getMinutes();
        let s = new Date().getSeconds();
        hh.style.strokeDashoffset = 510 - (510 * h) / 12;
        mm.style.strokeDashoffset = 630 - (630 * m) / 60;
        ss.style.strokeDashoffset = 760 - (760 * s) / 60;
        sec_dot.style.transform = `rotateZ(${s * 6}deg)`;
        min_dot.style.transform = `rotateZ(${m * 6}deg)`;
        hr_dot.style.transform = `rotateZ(${h * 30}deg)`;
        hr.style.transform = `rotateZ(${h * 30}deg)`;
        mn.style.transform = `rotateZ(${m * 6}deg)`;
        sc.style.transform = `rotateZ(${s * 6}deg)`
    });
</script>
</html>
