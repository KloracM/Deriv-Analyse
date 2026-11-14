# Deriv-Analyse
Advanced Deriv Synthetic Indices analysis platform
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Deriv Analyser</title>

<script src="https://unpkg.com/lightweight-charts@4.2.2/dist/lightweight-charts.standalone.production.js"></script>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    background: #080d18;
    color: white;
    font-family: Arial, sans-serif;
}

header {
    background: #111a2b;
    padding: 20px;
    border-bottom: 1px solid #26344a;
}

header h1 {
    color: #00d68f;
    font-size: 23px;
}

header p {
    color: #8c9bb2;
    margin-top: 6px;
}

.container {
    padding: 16px;
    max-width: 1200px;
    margin: auto;
}

.demo {
    background: #332b15;
    color: #ffd166;
    padding: 12px;
    border-radius: 8px;
    margin-bottom: 18px;
    font-size: 13px;
}

.card {
    background: #111a2b;
    padding: 17px;
    border-radius: 12px;
    margin-bottom: 16px;
    border: 1px solid #26344a;
}

label {
    display: block;
    color: #9eacc0;
    margin-bottom: 9px;
}

select {
    width: 100%;
    padding: 13px;
    background: #1c2940;
    color: white;
    border: 1px solid #344763;
    border-radius: 7px;
    font-size: 15px;
}

.price {
    font-size: 25px;
    color: #00d68f;
    margin-top: 10px;
}

#chart {
    width: 100%;
    height: 350px;
}

.indicators {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
}

.indicator {
    background: #1c2940;
    padding: 15px;
    border-radius: 8px;
}

.indicator p {
    color: #9eacc0;
    font-size: 13px;
}

.indicator h3 {
    margin-top: 9px;
    color: #00d68f;
}

.signal {
    text-align: center;
    padding: 25px;
}

.signal h2 {
    color: #ffd166;
    margin-bottom: 10px;
}

footer {
    text-align: center;
    color: #718096;
    padding: 25px;
}
</style>
</head>

<body>

<header>
    <h1>DERIV ANALYSER</h1>
    <p>Smart Synthetic Indices Analysis</p>
</header>

<div class="container">

    <div class="demo">
        ⚠ DEMO MODE — Prices and charts are sample data,
        not live market information.
    </div>

    <div class="card">
        <label>Select Market</label>

        <select id="market">
            <option value="10">Volatility 10</option>
            <option value="25">Volatility 25</option>
            <option value="50">Volatility 50</option>
            <option value="75">Volatility 75</option>
            <option value="100">Volatility 100</option>
        </select>

        <div class="price" id="price">1000.00</div>
        <p style="color:#9eacc0">Sample market price</p>
    </div>

    <div class="card">
        <label>Select Timeframe</label>

        <select id="timeframe">
            <option value="60">1 Minute</option>
            <option value="300">5 Minutes</option>
            <option value="900">15 Minutes</option>
            <option value="1800">30 Minutes</option>
            <option value="3600">1 Hour</option>
        </select>
    </div>

    <div class="card">
        <h3 style="margin-bottom:15px">Market Chart</h3>
        <div id="chart"></div>
    </div>

    <div class="card">
        <h3 style="margin-bottom:15px">Technical Indicators</h3>

        <div class="indicators">

            <div class="indicator">
                <p>EMA 20</p>
                <h3 id="ema20">--</h3>
            </div>

            <div class="indicator">
                <p>EMA 50</p>
                <h3 id="ema50">--</h3>
            </div>

            <div class="indicator">
                <p>RSI</p>
                <h3 id="rsi">--</h3>
            </div>

            <div class="indicator">
                <p>MACD</p>
                <h3 id="macd">--</h3>
            </div>

        </div>
    </div>

    <div class="card signal">
        <h2>TRADE SIGNAL</h2>
        <h1 style="color:#ffd166">WAIT</h1>
        <p style="color:#9eacc0">
            Live signal analysis not yet connected.
        </p>
    </div>

</div>

<footer>
    DERIV ANALYSER © 2026
    <br><br>
    Independent market analysis platform.
</footer>

<script>

const chartElement = document.getElementById("chart");

const chart = LightweightCharts.createChart(chartElement, {
    width: chartElement.clientWidth,
    height: 350,

    layout: {
        background: { color: "#111a2b" },
        textColor: "#d1d5db"
    },

    grid: {
        vertLines: { color: "#26344a" },
        horzLines: { color: "#26344a" }
    },

    timeScale: {
        timeVisible: true,
        secondsVisible: false
    }
});

const candles = chart.addCandlestickSeries({
    upColor: "#00d68f",
    downColor: "#ff4d6d",
    borderUpColor: "#00d68f",
    borderDownColor: "#ff4d6d",
    wickUpColor: "#00d68f",
    wickDownColor: "#ff4d6d"
});

function generateData() {

    const market = document.getElementById("market").value;
    const timeframe = Number(
        document.getElementById("timeframe").value
    );

    let base = 1000 + Number(market) * 10;

    let data = [];

    let currentTime = Math.floor(Date.now() / 1000);

    let currentPrice = base;

    for (let i = 99; i >= 0; i--) {

        let open = currentPrice;

        let change = (Math.random() - 0.48) * 15;

        let close = open + change;

        let high = Math.max(open, close) + Math.random() * 5;

        let low = Math.min(open, close) - Math.random() * 5;

        data.push({
            time: currentTime - i * timeframe,
            open: Number(open.toFixed(2)),
            high: Number(high.toFixed(2)),
            low: Number(low.toFixed(2)),
            close: Number(close.toFixed(2))
        });

        currentPrice = close;
    }

    candles.setData(data);

    document.getElementById("price").textContent =
        currentPrice.toFixed(2);

    document.getElementById("ema20").textContent = "--";
    document.getElementById("ema50").textContent = "--";
    document.getElementById("rsi").textContent = "--";
    document.getElementById("macd").textContent = "--";

    chart.timeScale().fitContent();
}

document.getElementById("market")
    .addEventListener("change", generateData);

document.getElementById("timeframe")
    .addEventListener("change", generateData);

new ResizeObserver(() => {
    chart.applyOptions({
        width: chartElement.clientWidth
    });
}).observe(chartElement);

generateData();

</script>

</body>
</html>
