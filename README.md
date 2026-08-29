<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Trading Dashboard</title>
    <!-- Tailwind CSS untuk Layout -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome untuk Icon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- ApexCharts untuk Grafik Interaktif Canggih -->
    <script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        darkbg: '#0a0a0a',
                        cardbg: '#141414',
                        accent: '#22c55e', // Hijau Neon
                        accentDark: '#166534',
                        textMuted: '#9ca3af'
                    }
                }
            }
        }
    </script>
    <style>
        body { background-color: #0a0a0a; color: #ffffff; }
        /* Kustomisasi scrollbar agar makin elegan */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #141414; }
        ::-webkit-scrollbar-thumb { background: #374151; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #22c55e; }
    </style>
</head>
<body class="p-4 md:p-6 font-sans">

    <!-- HEADER -->
    <header class="flex flex-col md:flex-row justify-between items-start md:items-center mb-6 gap-4 border-b border-gray-800 pb-4">
        <div>
            <h1 class="text-2xl font-bold flex items-center gap-2">
                <span class="text-accent text-3xl"><i class="fas fa-chart-line"></i></span> 
                Intraday Performance Dashboard
            </h1>
            <p class="text-textMuted text-sm mt-1">Trading Overview & Advanced Insights (XAUUSD Focus)</p>
        </div>
        <div class="flex gap-3 text-sm">
            <div class="bg-cardbg border border-gray-800 px-4 py-2 rounded-md flex items-center gap-2">
                <i class="far fa-calendar-alt text-textMuted"></i>
                <span>1 Aug 2026 - 31 Aug 2026</span>
                <i class="fas fa-chevron-down text-xs text-textMuted ml-2"></i>
            </div>
            <div class="bg-cardbg border border-gray-800 px-4 py-2 rounded-md flex items-center gap-2">
                <i class="fas fa-filter text-textMuted"></i>
                <span>Pair: All</span>
                <i class="fas fa-chevron-down text-xs text-textMuted ml-2"></i>
            </div>
        </div>
    </header>

    <!-- KPI CARDS (Tampilan 4 Kolom) -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
        <!-- Card 1 -->
        <div class="bg-cardbg border border-gray-800 p-4 rounded-lg flex items-center gap-4">
            <div class="text-accent text-4xl"><i class="fas fa-sack-dollar"></i></div>
            <div>
                <p class="text-textMuted text-xs uppercase tracking-wider font-semibold">Net Profit</p>
                <h3 class="text-2xl font-bold mt-1">$ 4,830.50</h3>
                <p class="text-accent text-xs font-medium mt-1"><i class="fas fa-arrow-up"></i> 18.6% vs Last Month</p>
            </div>
        </div>
        <!-- Card 2 -->
        <div class="bg-cardbg border border-gray-800 p-4 rounded-lg flex items-center gap-4">
            <div class="text-yellow-500 text-4xl"><i class="fas fa-crosshairs"></i></div>
            <div>
                <p class="text-textMuted text-xs uppercase tracking-wider font-semibold">Win Rate</p>
                <h3 class="text-2xl font-bold mt-1">68.4%</h3>
                <p class="text-accent text-xs font-medium mt-1"><i class="fas fa-arrow-up"></i> 2.1% vs Last Month</p>
            </div>
        </div>
        <!-- Card 3 -->
        <div class="bg-cardbg border border-gray-800 p-4 rounded-lg flex items-center gap-4">
            <div class="text-blue-500 text-4xl"><i class="fas fa-exchange-alt"></i></div>
            <div>
                <p class="text-textMuted text-xs uppercase tracking-wider font-semibold">Total Trades</p>
                <h3 class="text-2xl font-bold mt-1">142</h3>
                <p class="text-red-500 text-xs font-medium mt-1"><i class="fas fa-arrow-down"></i> 5.4% vs Last Month</p>
            </div>
        </div>
        <!-- Card 4 -->
        <div class="bg-cardbg border border-gray-800 p-4 rounded-lg flex items-center gap-4">
            <div class="text-purple-500 text-4xl"><i class="fas fa-chart-pie"></i></div>
            <div>
                <p class="text-textMuted text-xs uppercase tracking-wider font-semibold">Profit Factor</p>
                <h3 class="text-2xl font-bold mt-1">2.14</h3>
                <p class="text-accent text-xs font-medium mt-1"><i class="fas fa-arrow-up"></i> 0.3 vs Last Month</p>
            </div>
        </div>
    </div>

    <!-- MAIN CHARTS AREA -->
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6">
        
        <!-- Equity Curve (Besar, Kiri) -->
        <div class="lg:col-span-2 bg-cardbg border border-gray-800 rounded-lg p-4">
            <div class="flex justify-between items-center mb-2">
                <h3 class="text-sm font-semibold text-gray-300"><i class="fas fa-chart-area text-accent mr-2"></i>Equity Curve (Over Time)</h3>
                <span class="bg-gray-800 border border-gray-700 text-xs px-2 py-1 rounded text-accent font-mono">High: $5,120</span>
            </div>
            <div id="equityChart" class="w-full"></div>
        </div>

        <!-- Sesi Trading & Pair (Kanan) -->
        <div class="bg-cardbg border border-gray-800 rounded-lg p-4 flex flex-col">
            <h3 class="text-sm font-semibold text-gray-300 mb-2"><i class="fas fa-clock text-yellow-500 mr-2"></i>Profit by Session</h3>
            <div id="sessionChart" class="w-full flex-grow flex items-center justify-center"></div>
        </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <!-- Top Trading Days (Diagram Batang Horizontal) -->
        <div class="lg:col-span-1 bg-cardbg border border-gray-800 rounded-lg p-4">
            <h3 class="text-sm font-semibold text-gray-300 mb-2"><i class="fas fa-calendar-day text-blue-500 mr-2"></i>Best Days (Net Profit)</h3>
            <div id="daysChart" class="w-full"></div>
        </div>

        <!-- Tabel Jurnal Canggih -->
        <div class="lg:col-span-2 bg-cardbg border border-gray-800 rounded-lg p-4">
            <h3 class="text-sm font-semibold text-gray-300 mb-4"><i class="fas fa-list text-purple-500 mr-2"></i>Recent Closed Trades</h3>
            <div class="overflow-x-auto">
                <table class="w-full text-left text-sm whitespace-nowrap">
                    <thead class="text-textMuted border-b border-gray-800">
                        <tr>
                            <th class="pb-2">Pair</th>
                            <th class="pb-2">Type</th>
                            <th class="pb-2">Lot</th>
                            <th class="pb-2">Open Price</th>
                            <th class="pb-2">Close Price</th>
                            <th class="pb-2 text-right">Net PnL</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y divide-gray-800 font-mono text-xs">
                        <tr>
                            <td class="py-3 text-white">XAUUSD</td>
                            <td class="py-3 text-accent">BUY</td>
                            <td class="py-3">1.50</td>
                            <td class="py-3">2345.50</td>
                            <td class="py-3">2350.10</td>
                            <td class="py-3 text-right text-accent font-bold">+$690.00</td>
                        </tr>
                        <tr>
                            <td class="py-3 text-white">XAUUSD</td>
                            <td class="py-3 text-red-500">SELL</td>
                            <td class="py-3">0.50</td>
                            <td class="py-3">2340.00</td>
                            <td class="py-3">2343.50</td>
                            <td class="py-3 text-right text-red-500 font-bold">-$175.00</td>
                        </tr>
                        <tr>
                            <td class="py-3 text-white">EURUSD</td>
                            <td class="py-3 text-accent">BUY</td>
                            <td class="py-3">2.00</td>
                            <td class="py-3">1.0850</td>
                            <td class="py-3">1.0875</td>
                            <td class="py-3 text-right text-accent font-bold">+$500.00</td>
                        </tr>
                        <tr>
                            <td class="py-3 text-white">XAUUSD</td>
                            <td class="py-3 text-accent">BUY</td>
                            <td class="py-3">1.00</td>
                            <td class="py-3">2360.20</td>
                            <td class="py-3">2364.00</td>
                            <td class="py-3 text-right text-accent font-bold">+$380.00</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- SCRIPT UNTUK MERENDER GRAFIK (APEXCHARTS) -->
    <script>
        // Set tema gelap untuk semua chart
        const commonOptions = {
            chart: { background: 'transparent', toolbar: { show: false } },
            theme: { mode: 'dark' },
            grid: { borderColor: '#1f2937', strokeDashArray: 4 },
            dataLabels: { enabled: false }
        };

        // 1. CHART EQUITY (Area Canggih dengan Gradasi Hijau)
        var equityOptions = {
            ...commonOptions,
            series: [{ name: 'Cumulative Profit ($)', data: [0, 500, 300, 1200, 1000, 2500, 2200, 3800, 4830] }],
            chart: { type: 'area', height: 280, ...commonOptions.chart },
            colors: ['#22c55e'], // Hijau Neon
            fill: {
                type: 'gradient',
                gradient: { shadeIntensity: 1, opacityFrom: 0.4, opacityTo: 0.05, stops: [0, 100] }
            },
            stroke: { curve: 'smooth', width: 3 },
            xaxis: {
                categories: ['1 Aug', '4 Aug', '8 Aug', '12 Aug', '16 Aug', '20 Aug', '24 Aug', '28 Aug', '31 Aug'],
                axisBorder: { show: false }, axisTicks: { show: false },
                labels: { style: { colors: '#9ca3af' } }
            },
            yaxis: { labels: { style: { colors: '#9ca3af' }, formatter: (val) => "$" + val } }
        };
        new ApexCharts(document.querySelector("#equityChart"), equityOptions).render();

        // 2. CHART SESI TRADING (Donut Chart)
        var sessionOptions = {
            ...commonOptions,
            series: [45, 35, 20],
            labels: ['New York', 'London', 'Tokyo'],
            chart: { type: 'donut', height: 260, ...commonOptions.chart },
            colors: ['#22c55e', '#3b82f6', '#eab308'],
            plotOptions: {
                pie: {
                    donut: { size: '75%', labels: { show: true, name: { color: '#9ca3af' }, value: { color: '#fff', fontSize: '24px' }, total: { show: true, label: 'Top Session', color: '#9ca3af', formatter: function (w) { return "New York" } } } }
                }
            },
            stroke: { show: true, colors: '#141414', width: 2 },
            legend: { position: 'bottom', labels: { colors: '#9ca3af' } }
        };
        new ApexCharts(document.querySelector("#sessionChart"), sessionOptions).render();

        // 3. CHART HARI TERBAIK (Horizontal Bar)
        var daysOptions = {
            ...commonOptions,
            series: [{ name: 'Profit ($)', data: [1200, 950, 800, 600, 450] }],
            chart: { type: 'bar', height: 220, ...commonOptions.chart },
            colors: ['#22c55e'],
            plotOptions: {
                bar: { horizontal: true, borderRadius: 4, barHeight: '50%' }
            },
            xaxis: {
                categories: ['Tuesday', 'Thursday', 'Wednesday', 'Monday', 'Friday'],
                labels: { show: false }, axisBorder: { show: false }, axisTicks: { show: false }
            },
            yaxis: { labels: { style: { colors: '#d1d5db', fontWeight: 'bold' } } },
            grid: { show: false }
        };
        new ApexCharts(document.querySelector("#daysChart"), daysOptions).render();
    </script>
</body>
</html>
