<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bali-Tech Intraday Trading Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        darkbg: '#080808',
                        cardbg: '#121212',
                        accent: '#10b981', // Hijau Neon Trading
                        mutedText: '#9ca3af'
                    }
                }
            }
        }
    </script>
    <style>
        body { background-color: #080808; color: #ffffff; }
        
        /* Efek Bayangan Ukiran / Ornamen Kas Bali Tipis di Background Card */
        .balinese-watermark {
            position: relative;
            overflow: hidden;
        }
        .balinese-watermark::before {
            content: "";
            position: absolute;
            bottom: -20px;
            right: -20px;
            width: 150px;
            height: 150px;
            /* Simulasi siluet ukiran tradisional halus menggunakan SVG/Gradient */
            background-image: radial-gradient(circle, rgba(16, 185, 129, 0.07) 10%, transparent 70%);
            border-radius: 50%;
            pointer-events: none;
            z-index: 0;
        }
        .card-content { position: relative; z-index: 1; }
    </style>
</head>
<body class="p-4 md:p-8 font-sans">

    <header class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4 border-b border-gray-800 pb-4">
        <div>
            <h1 class="text-2xl font-bold flex items-center gap-3">
                <span class="text-accent text-3xl"><i class="fas fa-chart-line"></i></span> 
                BaliTrader X - Pro Dashboard
            </h1>
            <p class="text-mutedText text-sm mt-1">Intraday XAUUSD Precision Tracking & Analytics</p>
        </div>
        <div class="flex gap-3 text-sm">
            <div class="bg-cardbg border border-gray-800 px-4 py-2 rounded-lg flex items-center gap-2">
                <i class="fas fa-user-circle text-accent"></i>
                <span class="font-medium">Komang (Pro Trader)</span>
            </div>
        </div>
    </header>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
        
        <div class="bg-cardbg border border-gray-800 p-5 rounded-xl balinese-watermark shadow-lg">
            <div class="card-content flex items-center gap-4">
                <div class="text-accent text-3xl bg-gray-900 p-3 rounded-lg"><i class="fas fa-wallet"></i></div>
                <div>
                    <p class="text-mutedText text-xs uppercase tracking-wider font-semibold">Net Profit Hari Ini</p>
                    <h3 class="text-2xl font-bold mt-1 text-white">$ 1,240.00</h3>
                    <p class="text-accent text-xs font-medium mt-1"><i class="fas fa-arrow-up"></i> +14.2% hari ini</p>
                </div>
            </div>
        </div>

        <div class="bg-cardbg border border-gray-800 p-5 rounded-xl balinese-watermark shadow-lg">
            <div class="card-content flex items-center gap-4">
                <div class="text-yellow-500 text-3xl bg-gray-900 p-3 rounded-lg"><i class="fas fa-bullseye"></i></div>
                <div>
                    <p class="text-mutedText text-xs uppercase tracking-wider font-semibold">Win Rate Akurasi</p>
                    <h3 class="text-2xl font-bold mt-1 text-white">74.5%</h3>
                    <p class="text-accent text-xs font-medium mt-1"><i class="fas fa-arrow-up"></i> Konsisten</p>
                </div>
            </div>
        </div>

        <div class="bg-cardbg border border-gray-800 p-5 rounded-xl balinese-watermark shadow-lg">
            <div class="card-content flex items-center gap-4">
                <div class="text-blue-500 text-3xl bg-gray-900 p-3 rounded-lg"><i class="fas fa-layer-group"></i></div>
                <div>
                    <p class="text-mutedText text-xs uppercase tracking-wider font-semibold">Total Lot Executed</p>
                    <h3 class="text-2xl font-bold mt-1 text-white">18.5 Lot</h3>
                    <p class="text-mutedText text-xs font-medium mt-1">XAUUSD / EURUSD</p>
                </div>
            </div>
        </div>

        <div class="bg-cardbg border border-gray-800 p-5 rounded-xl balinese-watermark shadow-lg">
            <div class="card-content flex items-center gap-4">
                <div class="text-purple-500 text-3xl bg-gray-900 p-3 rounded-lg"><i class="fas fa-balance-scale"></i></div>
                <div>
                    <p class="text-mutedText text-xs uppercase tracking-wider font-semibold">Risk / Reward Ratio</p>
                    <h3 class="text-2xl font-bold mt-1 text-white">1 : 3.2</h3>
                    <p class="text-accent text-xs font-medium mt-1"><i class="fas fa-check"></i> Optimal Setup</p>
                </div>
            </div>
        </div>

    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6">
        
        <div class="lg:col-span-2 bg-cardbg border border-gray-800 rounded-xl p-5 balinese-watermark">
            <div class="card-content">
                <div class="flex justify-between items-center mb-4">
                    <h3 class="text-base font-semibold text-gray-200"><i class="fas fa-chart-area text-accent mr-2"></i>Live Equity Curve (Intraday Growth)</h3>
                    <span class="bg-gray-900 border border-gray-800 text-xs px-3 py-1 rounded-full text-accent font-mono">Target Tercapai</span>
                </div>
                <div id="mainEquityChart" class="w-full"></div>
            </div>
        </div>

        <div class="bg-cardbg border border-gray-800 rounded-xl p-5 balinese-watermark flex flex-col">
            <div class="card-content flex-grow flex flex-col justify-between">
                <h3 class="text-base font-semibold text-gray-200 mb-2"><i class="fas fa-globe text-blue-400 mr-2"></i>Profit Berdasarkan Sesi</h3>
                <div id="sessionPieChart" class="w-full flex-grow flex items-center justify-center"></div>
            </div>
        </div>

    </div>

    <script>
        const chartOptions = {
            chart: { background: 'transparent', toolbar: { show: false } },
            theme: { mode: 'dark' },
            grid: { borderColor: '#1f2937', strokeDashArray: 3 }
        };

        // Equity Curve
        var equityConfig = {
            ...chartOptions,
            series: [{ name: 'Profit ($)', data: [0, 210, 180, 450, 400, 890, 820, 1240] }],
            chart: { type: 'area', height: 280, ...chartOptions.chart },
            colors: ['#10b981'],
            fill: {
                type: 'gradient',
                gradient: { shadeIntensity: 1, opacityFrom: 0.45, opacityTo: 0.0, stops: [0, 100] }
            },
            stroke: { curve: 'smooth', width: 3 },
            xaxis: {
                categories: ['08:00', '10:00', '12:00', '14:00', '16:00', '18:00', '20:00', '22:00'],
                labels: { style: { colors: '#9ca3af' } }
            },
            yaxis: { labels: { style: { colors: '#9ca3af' }, formatter: (v) => "$" + v } }
        };
        new ApexCharts(document.querySelector("#mainEquityChart"), equityConfig).render();

        // Sesi Skenario
        var sessionConfig = {
            ...chartOptions,
            series: [55, 30, 15],
            labels: ['New York Session', 'London Session', 'Tokyo Session'],
            chart: { type: 'donut', height: 240, ...chartOptions.chart },
            colors: ['#10b981', '#3b82f6', '#f59e0b'],
            plotOptions: {
                pie: { donut: { size: '70%', labels: { show: true, name: { color: '#9ca3af' }, value: { color: '#fff', fontSize: '20px' } } } }
            },
            legend: { position: 'bottom', labels: { colors: '#9ca3af' } }
        };
        new ApexCharts(document.querySelector("#sessionPieChart"), sessionConfig).render();
    </script>
</body>
</html>
