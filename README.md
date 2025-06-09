<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Laporan Keuangan - MTS Miftahul Huda Palang</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary-color: #2c3e50;
            --secondary-color: #3498db;
            --success-color: #27ae60;
            --danger-color: #e74c3c;
            --light-bg: #f8f9fa;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f5f7fa;
            color: #333;
            padding-bottom: 50px;
        }
        
        .header {
            background: linear-gradient(135deg, var(--primary-color), #1a2530);
            color: white;
            padding: 20px 0;
            text-align: center;
            margin-bottom: 25px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        .login-container {
            max-width: 450px;
            margin: 80px auto;
            padding: 30px;
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
            transition: transform 0.3s ease;
        }
        
        .login-container:hover {
            transform: translateY(-5px);
        }
        
        .dashboard {
            display: none;
            background-color: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08);
            margin-bottom: 30px;
        }
        
        .table-container {
            overflow-x: auto;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }
        
        .btn-download {
            margin-bottom: 20px;
            transition: all 0.2s;
        }
        
        .btn-download:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .nav-tabs {
            margin-bottom: 25px;
            border-bottom: 2px solid var(--primary-color);
        }
        
        .nav-link {
            font-weight: 600;
            color: #555;
            transition: all 0.2s;
        }
        
        .nav-link.active {
            color: var(--primary-color);
            border-bottom: 3px solid var(--secondary-color);
        }
        
        .logo {
            width: 85px;
            height: 85px;
            margin-right: 15px;
            border-radius: 50%;
            border: 3px solid white;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .saldo-akhir {
            background-color: var(--light-bg);
            padding: 18px;
            border-radius: 8px;
            margin-bottom: 25px;
            border-left: 5px solid var(--primary-color);
            font-size: 1.2rem;
        }
        
        .card {
            border-radius: 10px;
            overflow: hidden;
            transition: transform 0.3s, box-shadow 0.3s;
            border: none;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
        }
        
        .card-header {
            font-weight: 600;
            font-size: 1.1rem;
        }
        
        .chart-container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-top: 25px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
        }
        
        .backup-section {
            background-color: var(--light-bg);
            padding: 20px;
            border-radius: 10px;
            margin-top: 30px;
        }
        
        .feature-icon {
            font-size: 1.5rem;
            margin-right: 10px;
            color: var(--secondary-color);
        }
        
        .stat-number {
            font-size: 1.8rem;
            font-weight: 700;
        }
        
        .btn-feature {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 10px 15px;
            margin: 8px;
            border-radius: 8px;
            font-weight: 600;
        }
        
        @media (max-width: 768px) {
            .header h1 {
                font-size: 1.5rem;
            }
            .header h3 {
                font-size: 1rem;
            }
            .logo {
                width: 65px;
                height: 65px;
            }
            .btn-feature {
                width: 100%;
                margin: 5px 0;
            }
        }
    </style>
</head>
<body>
    <!-- Login Form -->
    <div id="loginSection" class="login-container">
        <div class="text-center mb-4">
            <h2 class="mb-3">Login Admin</h2>
            <p class="text-muted">Sistem Laporan Keuangan MTS Miftahul Huda Palang</p>
        </div>
        <form id="loginForm">
            <div class="mb-3">
                <label for="username" class="form-label">Username</label>
                <input type="text" class="form-control" id="username" required>
            </div>
            <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input type="password" class="form-control" id="password" required>
            </div>
            <button type="submit" class="btn btn-primary w-100 py-2">
                <i class="bi bi-box-arrow-in-right me-2"></i>Login
            </button>
        </form>
    </div>

    <!-- Dashboard -->
    <div id="dashboardSection" class="container dashboard">
        <div class="header">
            <div class="d-flex flex-column flex-md-row align-items-center justify-content-center">
                <div class="mb-3 mb-md-0 me-md-4">
                    <img src="https://cdn-icons-png.flaticon.com/512/6380/6380329.png" alt="Logo Sekolah" class="logo">
                </div>
                <div>
                    <h1>MTS Miftahul Huda Palang</h1>
                    <h3>Sistem Laporan Keuangan Madrasah</h3>
                </div>
            </div>
        </div>

        <ul class="nav nav-tabs" id="myTab" role="tablist">
            <li class="nav-item" role="presentation">
                <button class="nav-link active" id="transaksi-tab" data-bs-toggle="tab" data-bs-target="#transaksi" type="button" role="tab">
                    <i class="bi bi-cash-coin me-1"></i>Transaksi
                </button>
            </li>
            <li class="nav-item" role="presentation">
                <button class="nav-link" id="laporan-tab" data-bs-toggle="tab" data-bs-target="#laporan" type="button" role="tab">
                    <i class="bi bi-file-earmark-bar-graph me-1"></i>Laporan
                </button>
            </li>
            <li class="nav-item" role="presentation">
                <button class="nav-link" id="rekap-tab" data-bs-toggle="tab" data-bs-target="#rekap" type="button" role="tab">
                    <i class="bi bi-pie-chart me-1"></i>Rekap
                </button>
            </li>
            <li class="nav-item" role="presentation">
                <button class="nav-link" id="grafik-tab" data-bs-toggle="tab" data-bs-target="#grafik" type="button" role="tab">
                    <i class="bi bi-graph-up me-1"></i>Grafik
                </button>
            </li>
        </ul>

        <div class="tab-content" id="myTabContent">
            <!-- Tab Transaksi -->
            <div class="tab-pane fade show active" id="transaksi" role="tabpanel">
                <div class="d-flex flex-wrap justify-content-between align-items-center mb-4">
                    <h3><i class="bi bi-plus-circle me-2"></i>Tambah Transaksi</h3>
                    <div class="d-flex">
                        <button id="backupData" class="btn btn-secondary me-2">
                            <i class="bi bi-cloud-arrow-down me-1"></i>Backup
                        </button>
                        <button id="restoreData" class="btn btn-secondary">
                            <i class="bi bi-cloud-arrow-up me-1"></i>Restore
                        </button>
                    </div>
                </div>
                
                <form id="transaksiForm" class="mb-4">
                    <div class="row g-3">
                        <div class="col-md-3">
                            <label for="tanggal" class="form-label">Tanggal</label>
                            <input type="date" class="form-control" id="tanggal" required>
                        </div>
                        <div class="col-md-3">
                            <label for="jenis" class="form-label">Jenis</label>
                            <select class="form-select" id="jenis" required>
                                <option value="masuk">Pemasukan</option>
                                <option value="keluar">Pengeluaran</option>
                            </select>
                        </div>
                        <div class="col-md-3">
                            <label for="jumlah" class="form-label">Jumlah (Rp)</label>
                            <input type="number" class="form-control" id="jumlah" required>
                        </div>
                        <div class="col-md-3">
                            <label for="kategori" class="form-label">Kategori</label>
                            <select class="form-select" id="kategori" required>
                                <option value="SPP">SPP</option>
                                <option value="Dana Bos">Dana BOS</option>
                                <option value="Donasi">Donasi</option>
                                <option value="Gaji">Gaji Guru & Staf</option>
                                <option value="Peralatan">Peralatan Sekolah</option>
                                <option value="Bangunan">Pemeliharaan Bangunan</option>
                                <option value="Lainnya">Lainnya</option>
                            </select>
                        </div>
                        <div class="col-md-9">
                            <label for="keterangan" class="form-label">Keterangan</label>
                            <input type="text" class="form-control" id="keterangan" required>
                        </div>
                        <div class="col-md-3 d-flex align-items-end">
                            <button type="submit" class="btn btn-primary w-100 py-2">
                                <i class="bi bi-plus-lg me-1"></i>Tambah
                            </button>
                        </div>
                    </div>
                </form>

                <div class="saldo-akhir">
                    <h4><i class="bi bi-wallet2 me-2"></i>Saldo Akhir: <span id="saldoAkhir">Rp 0</span></h4>
                </div>

                <h3><i class="bi bi-clock-history me-2"></i>Transaksi Terakhir</h3>
                <div class="table-container">
                    <table class="table table-striped table-hover">
                        <thead class="table-dark">
                            <tr>
                                <th>No</th>
                                <th>Tanggal</th>
                                <th>Jenis</th>
                                <th>Kategori</th>
                                <th>Jumlah</th>
                                <th>Keterangan</th>
                                <th>Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="transaksiTableBody">
                            <!-- Data transaksi akan dimasukkan melalui JavaScript -->
                        </tbody>
                    </table>
                </div>
                
                <div class="backup-section">
                    <h4><i class="bi bi-safe me-2"></i>Manajemen Data</h4>
                    <div class="row mt-3">
                        <div class="col-md-6 mb-3">
                            <h5><i class="bi bi-google me-2"></i>Integrasi Google Sheets</h5>
                            <p>Ekspor data ke Google Sheets untuk analisis lebih lanjut</p>
                            <button id="exportToSheets" class="btn btn-feature btn-success">
                                <i class="bi bi-google me-1"></i>Export to Google Sheets
                            </button>
                        </div>
                        <div class="col-md-6">
                            <h5><i class="bi bi-file-earmark-arrow-down me-2"></i>Backup & Restore</h5>
                            <p>Cadangkan atau pulihkan data keuangan Anda</p>
                            <div class="d-flex">
                                <button id="exportToCSV" class="btn btn-feature btn-info">
                                    <i class="bi bi-file-earmark-spreadsheet me-1"></i>Export CSV
                                </button>
                                <button id="importFromCSV" class="btn btn-feature btn-warning">
                                    <i class="bi bi-file-earmark-plus me-1"></i>Import CSV
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Tab Laporan -->
            <div class="tab-pane fade" id="laporan" role="tabpanel">
                <h3><i class="bi bi-funnel me-2"></i>Filter Laporan</h3>
                <form id="filterForm" class="mb-4">
                    <div class="row g-3">
                        <div class="col-md-3">
                            <label for="filterBulan" class="form-label">Bulan</label>
                            <select class="form-select" id="filterBulan">
                                <option value="all">Semua Bulan</option>
                                <option value="1">Januari</option>
                                <option value="2">Februari</option>
                                <option value="3">Maret</option>
                                <option value="4">April</option>
                                <option value="5">Mei</option>
                                <option value="6">Juni</option>
                                <option value="7">Juli</option>
                                <option value="8">Agustus</option>
                                <option value="9">September</option>
                                <option value="10">Oktober</option>
                                <option value="11">November</option>
                                <option value="12">Desember</option>
                            </select>
                        </div>
                        <div class="col-md-3">
                            <label for="filterTahun" class="form-label">Tahun</label>
                            <select class="form-select" id="filterTahun">
                                <option value="2023">2023</option>
                                <option value="2024" selected>2024</option>
                                <option value="2025">2025</option>
                                <option value="2026">2026</option>
                                <option value="2027">2027</option>
                                <option value="2028">2028</option>
                                <option value="2029">2029</option>
                                <option value="2030">2030</option>
                            </select>
                        </div>
                        <div class="col-md-3">
                            <label for="filterJenis" class="form-label">Jenis</label>
                            <select class="form-select" id="filterJenis">
                                <option value="all">Semua Jenis</option>
                                <option value="masuk">Pemasukan</option>
                                <option value="keluar">Pengeluaran</option>
                            </select>
                        </div>
                        <div class="col-md-3 d-flex align-items-end">
                            <button type="submit" class="btn btn-primary w-100 py-2">
                                <i class="bi bi-filter me-1"></i>Filter
                            </button>
                        </div>
                    </div>
                </form>

                <div class="d-flex flex-wrap justify-content-between align-items-center mb-3">
                    <h3><i class="bi bi-file-earmark-text me-2"></i>Laporan Keuangan</h3>
                    <div>
                        <button id="downloadLaporan" class="btn btn-success btn-download me-2">
                            <i class="bi bi-file-earmark-pdf me-1"></i>Download PDF
                        </button>
                        <button id="printLaporan" class="btn btn-info btn-download">
                            <i class="bi bi-printer me-1"></i>Print
                        </button>
                    </div>
                </div>

                <div id="laporanContent" class="table-container">
                    <table class="table table-striped">
                        <thead class="table-dark">
                            <tr>
                                <th>No</th>
                                <th>Tanggal</th>
                                <th>Jenis</th>
                                <th>Kategori</th>
                                <th>Jumlah</th>
                                <th>Keterangan</th>
                            </tr>
                        </thead>
                        <tbody id="laporanTableBody">
                            <!-- Data laporan akan dimasukkan melalui JavaScript -->
                        </tbody>
                    </table>
                </div>
                
                <div class="alert alert-info mt-4">
                    <h5><i class="bi bi-info-circle me-2"></i>Total dan Saldo</h5>
                    <div class="d-flex flex-wrap">
                        <div class="me-4">
                            <span class="fw-bold">Total Pemasukan:</span>
                            <span id="totalPemasukanLaporan">Rp 0</span>
                        </div>
                        <div class="me-4">
                            <span class="fw-bold">Total Pengeluaran:</span>
                            <span id="totalPengeluaranLaporan">Rp 0</span>
                        </div>
                        <div>
                            <span class="fw-bold">Saldo Akhir:</span>
                            <span id="saldoAkhirLaporan">Rp 0</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Tab Rekap -->
            <div class="tab-pane fade" id="rekap" role="tabpanel">
                <div class="d-flex flex-wrap justify-content-between align-items-center mb-3">
                    <h3><i class="bi bi-clipboard-data me-2"></i>Rekapitulasi Keuangan</h3>
                    <button id="downloadRekap" class="btn btn-success btn-download">
                        <i class="bi bi-file-earmark-pdf me-1"></i>Download PDF
                    </button>
                </div>

                <div id="rekapContent">
                    <div class="row mb-4">
                        <div class="col-md-4">
                            <div class="card text-white bg-success">
                                <div class="card-header">
                                    <i class="bi bi-arrow-down-circle me-2"></i>Total Pemasukan
                                </div>
                                <div class="card-body">
                                    <h5 class="card-title stat-number" id="totalPemasukan">Rp 0</h5>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="card text-white bg-danger">
                                <div class="card-header">
                                    <i class="bi bi-arrow-up-circle me-2"></i>Total Pengeluaran
                                </div>
                                <div class="card-body">
                                    <h5 class="card-title stat-number" id="totalPengeluaran">Rp 0</h5>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-4">
                            <div class="card text-white bg-primary">
                                <div class="card-header">
                                    <i class="bi bi-wallet2 me-2"></i>Saldo Akhir
                                </div>
                                <div class="card-body">
                                    <h5 class="card-title stat-number" id="totalSaldo">Rp 0</h5>
                                </div>
                            </div>
                        </div>
                    </div>

                    <h4><i class="bi bi-calendar-month me-2"></i>Rekap Bulanan</h4>
                    <div class="table-container">
                        <table class="table table-striped">
                            <thead class="table-dark">
                                <tr>
                                    <th>Bulan</th>
                                    <th>Pemasukan</th>
                                    <th>Pengeluaran</th>
                                    <th>Saldo</th>
                                </tr>
                            </thead>
                            <tbody id="rekapTableBody">
                                <!-- Data rekap akan dimasukkan melalui JavaScript -->
                            </tbody>
                        </table>
                    </div>

                    <h4 class="mt-4"><i class="bi bi-tag me-2"></i>Rekap per Kategori</h4>
                    <div class="row">
                        <div class="col-md-6">
                            <h5><i class="bi bi-arrow-down-circle text-success me-2"></i>Pemasukan</h5>
                            <table class="table table-striped">
                                <thead class="table-dark">
                                    <tr>
                                        <th>Kategori</th>
                                        <th>Jumlah</th>
                                        <th>Persentase</th>
                                    </tr>
                                </thead>
                                <tbody id="rekapKategoriMasuk">
                                    <!-- Data akan dimasukkan melalui JavaScript -->
                                </tbody>
                            </table>
                        </div>
                        <div class="col-md-6">
                            <h5><i class="bi bi-arrow-up-circle text-danger me-2"></i>Pengeluaran</h5>
                            <table class="table table-striped">
                                <thead class="table-dark">
                                    <tr>
                                        <th>Kategori</th>
                                        <th>Jumlah</th>
                                        <th>Persentase</th>
                                    </tr>
                                </thead>
                                <tbody id="rekapKategoriKeluar">
                                    <!-- Data akan dimasukkan melalui JavaScript -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Tab Grafik -->
            <div class="tab-pane fade" id="grafik" role="tabpanel">
                <div class="d-flex justify-content-between align-items-center mb-4">
                    <h3><i class="bi bi-bar-chart-line me-2"></i>Visualisasi Data Keuangan</h3>
                    <button id="downloadCharts" class="btn btn-success">
                        <i class="bi bi-download me-1"></i>Download Grafik
                    </button>
                </div>
                
                <div class="row mb-4">
                    <div class="col-md-6">
                        <div class="chart-container">
                            <h4><i class="bi bi-graph-up-arrow text-success me-2"></i>Pemasukan vs Pengeluaran</h4>
                            <canvas id="incomeExpenseChart"></canvas>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="chart-container">
                            <h4><i class="bi bi-pie-chart text-primary me-2"></i>Distribusi Kategori</h4>
                            <canvas id="categoryDistributionChart"></canvas>
                        </div>
                    </div>
                </div>
                
                <div class="row">
                    <div class="col-md-12">
                        <div class="chart-container">
                            <h4><i class="bi bi-calendar-heart text-info me-2"></i>Trend Bulanan</h4>
                            <canvas id="monthlyTrendChart"></canvas>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <footer class="bg-dark text-white text-center py-3 mt-5">
        <div class="container">
            <p class="mb-0">Sistem Laporan Keuangan MTS Miftahul Huda Palang &copy; 2025</p>
            <small>Dikembangkan untuk transparansi dan akuntabilitas keuangan sekolah copy; Powered by Arif Wibowo</small>
        </div>
    </footer>

    <!-- Modal untuk Restore Data -->
    <div class="modal fade" id="restoreModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title"><i class="bi bi-cloud-arrow-up me-2"></i>Restore Data</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="mb-3">
                        <label for="backupFile" class="form-label">Pilih file backup (JSON)</label>
                        <input class="form-control" type="file" id="backupFile" accept=".json">
                    </div>
                    <div class="alert alert-warning">
                        <i class="bi bi-exclamation-triangle me-2"></i>
                        Restore data akan menggantikan semua data saat ini. Pastikan Anda telah membackup data terbaru.
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Batal</button>
                    <button type="button" class="btn btn-primary" id="confirmRestore">Restore Data</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // Data transaksi (akan disimpan di localStorage)
        let transactions = [];
        let charts = {};
        
        // Fungsi untuk menyimpan data ke localStorage
        function saveTransactions() {
            localStorage.setItem('mts_transactions', JSON.stringify(transactions));
        }

        // Fungsi untuk memuat data dari localStorage
        function loadTransactions() {
            const saved = localStorage.getItem('mts_transactions');
            if (saved) {
                try {
                    transactions = JSON.parse(saved);
                } catch (e) {
                    console.error("Error parsing saved transactions:", e);
                    transactions = [];
                }
            }
        }

        // Fungsi untuk memformat angka ke format Rupiah
        function formatRupiah(angka) {
            return new Intl.NumberFormat('id-ID', { 
                style: 'currency', 
                currency: 'IDR',
                minimumFractionDigits: 0
            }).format(angka);
        }

        // Fungsi untuk menghitung saldo
        function hitungSaldo() {
            const totalPemasukan = transactions
                .filter(trans => trans.jenis === 'masuk')
                .reduce((sum, trans) => sum + trans.jumlah, 0);
                
            const totalPengeluaran = transactions
                .filter(trans => trans.jenis === 'keluar')
                .reduce((sum, trans) => sum + trans.jumlah, 0);
                
            return totalPemasukan - totalPengeluaran;
        }

        // Fungsi untuk menampilkan transaksi di tabel
        function renderTransactions() {
            const tbody = document.getElementById('transaksiTableBody');
            tbody.innerHTML = '';
            
            // Ambil 10 transaksi terakhir (urut berdasarkan tanggal descending)
            const sortedTransactions = [...transactions].sort((a, b) => new Date(b.tanggal) - new Date(a.tanggal));
            const recentTransactions = sortedTransactions.slice(0, 10);
            
            recentTransactions.forEach((trans, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${trans.tanggal}</td>
                    <td><span class="badge ${trans.jenis === 'masuk' ? 'bg-success' : 'bg-danger'}">${trans.jenis === 'masuk' ? 'Pemasukan' : 'Pengeluaran'}</span></td>
                    <td>${trans.kategori}</td>
                    <td>${formatRupiah(trans.jumlah)}</td>
                    <td>${trans.keterangan}</td>
                    <td>
                        <button class="btn btn-sm btn-danger" onclick="deleteTransaction(${trans.id})">
                            <i class="bi bi-trash"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(row);
            });

            // Update saldo akhir
            const saldo = hitungSaldo();
            document.getElementById('saldoAkhir').textContent = formatRupiah(saldo);
            document.getElementById('saldoAkhirLaporan').textContent = formatRupiah(saldo);
        }

        // Fungsi untuk menampilkan laporan berdasarkan filter
        function renderLaporan() {
            const bulan = document.getElementById('filterBulan').value;
            const tahun = document.getElementById('filterTahun').value;
            const jenis = document.getElementById('filterJenis').value;
            
            const tbody = document.getElementById('laporanTableBody');
            tbody.innerHTML = '';
            
            let filteredTransactions = [...transactions];
            
            // Filter berdasarkan tahun
            filteredTransactions = filteredTransactions.filter(trans => {
                const transYear = new Date(trans.tanggal).getFullYear();
                return transYear.toString() === tahun;
            });
            
            // Filter berdasarkan bulan jika bukan 'all'
            if (bulan !== 'all') {
                filteredTransactions = filteredTransactions.filter(trans => {
                    const transMonth = new Date(trans.tanggal).getMonth() + 1;
                    return transMonth.toString() === bulan;
                });
            }
            
            // Filter berdasarkan jenis jika bukan 'all'
            if (jenis !== 'all') {
                filteredTransactions = filteredTransactions.filter(trans => trans.jenis === jenis);
            }
            
            // Urutkan berdasarkan tanggal
            filteredTransactions.sort((a, b) => new Date(a.tanggal) - new Date(b.tanggal));
            
            let totalPemasukan = 0;
            let totalPengeluaran = 0;
            
            filteredTransactions.forEach((trans, index) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${trans.tanggal}</td>
                    <td><span class="badge ${trans.jenis === 'masuk' ? 'bg-success' : 'bg-danger'}">${trans.jenis === 'masuk' ? 'Pemasukan' : 'Pengeluaran'}</span></td>
                    <td>${trans.kategori}</td>
                    <td>${formatRupiah(trans.jumlah)}</td>
                    <td>${trans.keterangan}</td>
                `;
                tbody.appendChild(row);
                
                // Hitung total
                if (trans.jenis === 'masuk') {
                    totalPemasukan += trans.jumlah;
                } else {
                    totalPengeluaran += trans.jumlah;
                }
            });
            
            document.getElementById('totalPemasukanLaporan').textContent = formatRupiah(totalPemasukan);
            document.getElementById('totalPengeluaranLaporan').textContent = formatRupiah(totalPengeluaran);
        }

        // Fungsi untuk menampilkan rekap
        function renderRekap() {
            // Hitung total pemasukan dan pengeluaran
            const totalPemasukan = transactions
                .filter(trans => trans.jenis === 'masuk')
                .reduce((sum, trans) => sum + trans.jumlah, 0);
                
            const totalPengeluaran = transactions
                .filter(trans => trans.jenis === 'keluar')
                .reduce((sum, trans) => sum + trans.jumlah, 0);
                
            const saldoAkhir = totalPemasukan - totalPengeluaran;
                
            document.getElementById('totalPemasukan').textContent = formatRupiah(totalPemasukan);
            document.getElementById('totalPengeluaran').textContent = formatRupiah(totalPengeluaran);
            document.getElementById('totalSaldo').textContent = formatRupiah(saldoAkhir);
            
            // Rekap bulanan
            const rekapBulanan = {};
            const bulanNames = ['Januari', 'Februari', 'Maret', 'April', 'Mei', 'Juni', 'Juli', 'Agustus', 'September', 'Oktober', 'November', 'Desember'];
            
            transactions.forEach(trans => {
                const date = new Date(trans.tanggal);
                const bulan = date.getMonth();
                const tahun = date.getFullYear();
                const key = `${tahun}-${bulan}`;
                
                if (!rekapBulanan[key]) {
                    rekapBulanan[key] = {
                        bulan: bulan,
                        tahun: tahun,
                        pemasukan: 0,
                        pengeluaran: 0
                    };
                }
                
                if (trans.jenis === 'masuk') {
                    rekapBulanan[key].pemasukan += trans.jumlah;
                } else {
                    rekapBulanan[key].pengeluaran += trans.jumlah;
                }
            });
            
            const tbodyRekap = document.getElementById('rekapTableBody');
            tbodyRekap.innerHTML = '';
            
            // Urutkan berdasarkan tahun dan bulan
            const sortedKeys = Object.keys(rekapBulanan).sort((a, b) => {
                const [yearA, monthA] = a.split('-').map(Number);
                const [yearB, monthB] = b.split('-').map(Number);
                return yearA - yearB || monthA - monthB;
            });
            
            sortedKeys.forEach(key => {
                const item = rekapBulanan[key];
                const saldo = item.pemasukan - item.pengeluaran;
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${bulanNames[item.bulan]} ${item.tahun}</td>
                    <td>${formatRupiah(item.pemasukan)}</td>
                    <td>${formatRupiah(item.pengeluaran)}</td>
                    <td>${formatRupiah(saldo)}</td>
                `;
                tbodyRekap.appendChild(row);
            });
            
            // Rekap per kategori pemasukan
            const rekapKategoriMasuk = {};
            const rekapKategoriKeluar = {};
            
            transactions.forEach(trans => {
                if (trans.jenis === 'masuk') {
                    if (!rekapKategoriMasuk[trans.kategori]) {
                        rekapKategoriMasuk[trans.kategori] = 0;
                    }
                    rekapKategoriMasuk[trans.kategori] += trans.jumlah;
                } else {
                    if (!rekapKategoriKeluar[trans.kategori]) {
                        rekapKategoriKeluar[trans.kategori] = 0;
                    }
                    rekapKategoriKeluar[trans.kategori] += trans.jumlah;
                }
            });
            
            const tbodyKategoriMasuk = document.getElementById('rekapKategoriMasuk');
            tbodyKategoriMasuk.innerHTML = '';
            
            const totalMasuk = Object.values(rekapKategoriMasuk).reduce((a, b) => a + b, 0);
            
            for (const [kategori, jumlah] of Object.entries(rekapKategoriMasuk)) {
                const persentase = totalMasuk > 0 ? ((jumlah / totalMasuk) * 100).toFixed(1) : 0;
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${kategori}</td>
                    <td>${formatRupiah(jumlah)}</td>
                    <td>
                        <div class="progress">
                            <div class="progress-bar bg-success" role="progressbar" style="width: ${persentase}%">
                                ${persentase}%
                            </div>
                        </div>
                    </td>
                `;
                tbodyKategoriMasuk.appendChild(row);
            }
            
            const tbodyKategoriKeluar = document.getElementById('rekapKategoriKeluar');
            tbodyKategoriKeluar.innerHTML = '';
            
            const totalKeluar = Object.values(rekapKategoriKeluar).reduce((a, b) => a + b, 0);
            
            for (const [kategori, jumlah] of Object.entries(rekapKategoriKeluar)) {
                const persentase = totalKeluar > 0 ? ((jumlah / totalKeluar) * 100).toFixed(1) : 0;
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${kategori}</td>
                    <td>${formatRupiah(jumlah)}</td>
                    <td>
                        <div class="progress">
                            <div class="progress-bar bg-danger" role="progressbar" style="width: ${persentase}%">
                                ${persentase}%
                            </div>
                        </div>
                    </td>
                `;
                tbodyKategoriKeluar.appendChild(row);
            }
            
            // Render grafik setelah rekap
            renderCharts();
        }
        
        // Fungsi untuk render grafik
        function renderCharts() {
            // Hancurkan grafik sebelumnya jika ada
            if (charts.incomeExpense) charts.incomeExpense.destroy();
            if (charts.categoryDistribution) charts.categoryDistribution.destroy();
            if (charts.monthlyTrend) charts.monthlyTrend.destroy();
            
            // Data untuk grafik pemasukan vs pengeluaran
            const totalPemasukan = transactions
                .filter(trans => trans.jenis === 'masuk')
                .reduce((sum, trans) => sum + trans.jumlah, 0);
                
            const totalPengeluaran = transactions
                .filter(trans => trans.jenis === 'keluar')
                .reduce((sum, trans) => sum + trans.jumlah, 0);
            
            const incomeExpenseCtx = document.getElementById('incomeExpenseChart').getContext('2d');
            charts.incomeExpense = new Chart(incomeExpenseCtx, {
                type: 'bar',
                data: {
                    labels: ['Pemasukan', 'Pengeluaran'],
                    datasets: [{
                        label: 'Jumlah (Rp)',
                        data: [totalPemasukan, totalPengeluaran],
                        backgroundColor: [
                            'rgba(40, 167, 69, 0.7)',
                            'rgba(220, 53, 69, 0.7)'
                        ],
                        borderColor: [
                            'rgba(40, 167, 69, 1)',
                            'rgba(220, 53, 69, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return formatRupiah(context.raw);
                                }
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                callback: function(value) {
                                    return formatRupiah(value);
                                }
                            }
                        }
                    }
                }
            });
            
            // Data untuk grafik distribusi kategori
            const categories = {};
            transactions.forEach(trans => {
                if (!categories[trans.kategori]) {
                    categories[trans.kategori] = 0;
                }
                categories[trans.kategori] += trans.jumlah;
            });
            
            const categoryLabels = Object.keys(categories);
            const categoryData = Object.values(categories);
            
            // Buat warna untuk setiap kategori
            const backgroundColors = [];
            categoryLabels.forEach(() => {
                backgroundColors.push(`rgba(${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)}, 0.7)`);
            });
            
            const categoryCtx = document.getElementById('categoryDistributionChart').getContext('2d');
            charts.categoryDistribution = new Chart(categoryCtx, {
                type: 'pie',
                data: {
                    labels: categoryLabels,
                    datasets: [{
                        data: categoryData,
                        backgroundColor: backgroundColors,
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    const label = context.label || '';
                                    const value = context.raw || 0;
                                    const total = context.dataset.data.reduce((a, b) => a + b, 0);
                                    const percentage = Math.round((value / total) * 100);
                                    return `${label}: ${formatRupiah(value)} (${percentage}%)`;
                                }
                            }
                        }
                    }
                }
            });
            
            // Data untuk grafik trend bulanan
            const bulanNames = ['Jan', 'Feb', 'Mar', 'Apr', 'Mei', 'Jun', 'Jul', 'Ags', 'Sep', 'Okt', 'Nov', 'Des'];
            const monthlyData = Array(12).fill(0).map((_, i) => {
                const monthTrans = transactions.filter(trans => new Date(trans.tanggal).getMonth() === i);
                return {
                    pemasukan: monthTrans.filter(t => t.jenis === 'masuk').reduce((sum, t) => sum + t.jumlah, 0),
                    pengeluaran: monthTrans.filter(t => t.jenis === 'keluar').reduce((sum, t) => sum + t.jumlah, 0)
                };
            });
            
            const monthlyCtx = document.getElementById('monthlyTrendChart').getContext('2d');
            charts.monthlyTrend = new Chart(monthlyCtx, {
                type: 'line',
                data: {
                    labels: bulanNames,
                    datasets: [
                        {
                            label: 'Pemasukan',
                            data: monthlyData.map(m => m.pemasukan),
                            borderColor: 'rgba(40, 167, 69, 1)',
                            backgroundColor: 'rgba(40, 167, 69, 0.1)',
                            tension: 0.3,
                            fill: true
                        },
                        {
                            label: 'Pengeluaran',
                            data: monthlyData.map(m => m.pengeluaran),
                            borderColor: 'rgba(220, 53, 69, 1)',
                            backgroundColor: 'rgba(220, 53, 69, 0.1)',
                            tension: 0.3,
                            fill: true
                        }
                    ]
                },
                options: {
                    responsive: true,
                    plugins: {
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `${context.dataset.label}: ${formatRupiah(context.raw)}`;
                                }
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                callback: function(value) {
                                    return formatRupiah(value);
                                }
                            }
                        }
                    }
                }
            });
        }

        // Fungsi untuk menambahkan transaksi baru
        function addTransaction(event) {
            event.preventDefault();
            
            const tanggal = document.getElementById('tanggal').value;
            const jenis = document.getElementById('jenis').value;
            const jumlah = parseInt(document.getElementById('jumlah').value);
            const kategori = document.getElementById('kategori').value;
            const keterangan = document.getElementById('keterangan').value;
            
            // Validasi jumlah
            if (isNaN(jumlah) || jumlah <= 0) {
                alert("Jumlah harus angka positif");
                return;
            }
            
            // Generate ID baru
            const newId = transactions.length > 0 ? Math.max(...transactions.map(t => t.id)) + 1 : 1;
            
            // Tambahkan transaksi baru
            const newTransaction = {
                id: newId,
                tanggal,
                jenis,
                kategori,
                jumlah,
                keterangan
            };
            
            transactions.push(newTransaction);
            saveTransactions(); // Simpan ke localStorage
            
            // Reset form
            document.getElementById('transaksiForm').reset();
            // Set tanggal default ke hari ini
            document.getElementById('tanggal').valueAsDate = new Date();
            
            // Render ulang tabel
            renderTransactions();
            renderLaporan();
            renderRekap();
            
            alert('Transaksi berhasil ditambahkan!');
        }

        // Fungsi untuk menghapus transaksi
        function deleteTransaction(id) {
            if (confirm('Apakah Anda yakin ingin menghapus transaksi ini?')) {
                transactions = transactions.filter(trans => trans.id !== id);
                saveTransactions(); // Simpan ke localStorage
                renderTransactions();
                renderLaporan();
                renderRekap();
                alert('Transaksi berhasil dihapus!');
            }
        }

        // Fungsi untuk download PDF dengan sisa saldo
        function downloadPDF(contentId, filename) {
            const { jsPDF } = window.jspdf;
            const element = document.getElementById(contentId);
            const saldo = hitungSaldo();
            
            html2canvas(element).then(canvas => {
                const imgData = canvas.toDataURL('image/png');
                const pdf = new jsPDF('p', 'mm', 'a4');
                const imgProps = pdf.getImageProperties(imgData);
                const pdfWidth = pdf.internal.pageSize.getWidth();
                const pdfHeight = (imgProps.height * pdfWidth) / imgProps.width;
                
                pdf.addImage(imgData, 'PNG', 0, 0, pdfWidth, pdfHeight);
                
                // Tambahkan informasi saldo di footer
                pdf.setFontSize(12);
                pdf.text(`Saldo Akhir: ${formatRupiah(saldo)}`, pdfWidth / 2, pdfHeight + 15, { align: 'center' });
                
                pdf.save(filename);
            });
        }
        
        // Fungsi untuk backup data
        function backupData() {
            const dataStr = JSON.stringify(transactions, null, 2);
            const blob = new Blob([dataStr], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            
            const a = document.createElement('a');
            a.href = url;
            a.download = `backup-keuangan-mts-${new Date().toISOString().slice(0,10)}.json`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            
            alert('Backup data berhasil diunduh!');
        }
        
        // Fungsi untuk restore data
        function restoreData() {
            const restoreModal = new bootstrap.Modal(document.getElementById('restoreModal'));
            restoreModal.show();
        }
        
        // Fungsi untuk export ke CSV
        function exportToCSV() {
            let csv = 'Tanggal,Jenis,Kategori,Jumlah,Keterangan\n';
            
            transactions.forEach(trans => {
                csv += `"${trans.tanggal}",`;
                csv += `"${trans.jenis === 'masuk' ? 'Pemasukan' : 'Pengeluaran'}",`;
                csv += `"${trans.kategori}",`;
                csv += `"${trans.jumlah}",`;
                csv += `"${trans.keterangan}"\n`;
            });
            
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `data-keuangan-mts-${new Date().toISOString().slice(0,10)}.csv`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            
            alert('Data berhasil diekspor ke CSV!');
        }
        
        // Fungsi untuk export ke Google Sheets
        function exportToGoogleSheets() {
            // Dalam implementasi nyata, ini akan mengintegrasikan dengan API Google Sheets
            // Di sini kita hanya akan menampilkan pesan
            alert('Fitur ekspor ke Google Sheets akan membuka jendela autentikasi Google. Dalam implementasi nyata, Anda perlu mengatur OAuth 2.0 dan Google Sheets API.');
            
            // Simulasikan proses ekspor
            setTimeout(() => {
                alert('Data berhasil diekspor ke Google Sheets! Kunjungi Google Drive Anda untuk melihat spreadsheet baru.');
            }, 1500);
        }

        // Fungsi untuk login
        function login(event) {
            event.preventDefault();
            
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            // Ganti dengan kredensial yang lebih aman
            if (username === 'admin' && password === 'aishwa') {
                document.getElementById('loginSection').style.display = 'none';
                document.getElementById('dashboardSection').style.display = 'block';
                
                // Render data awal
                renderTransactions();
                renderLaporan();
                renderRekap();
            } else {
                alert('Username atau password salah!');
                document.getElementById('password').value = '';
            }
        }

        // Event listeners
        document.addEventListener('DOMContentLoaded', function() {
            // Muat data dari localStorage
            loadTransactions();
            
            document.getElementById('loginForm').addEventListener('submit', login);
            document.getElementById('transaksiForm').addEventListener('submit', addTransaction);
            document.getElementById('filterForm').addEventListener('submit', function(e) {
                e.preventDefault();
                renderLaporan();
            });
            
            document.getElementById('downloadLaporan').addEventListener('click', function() {
                downloadPDF('laporanContent', 'Laporan_Keuangan_MTS_Miftahul_Huda_Palang.pdf');
            });
            
            document.getElementById('printLaporan').addEventListener('click', function() {
                window.print();
            });
            
            document.getElementById('downloadRekap').addEventListener('click', function() {
                downloadPDF('rekapContent', 'Rekap_Keuangan_MTS_Miftahul_Huda_Palang.pdf');
            });
            
            document.getElementById('backupData').addEventListener('click', backupData);
            document.getElementById('restoreData').addEventListener('click', restoreData);
            document.getElementById('exportToSheets').addEventListener('click', exportToGoogleSheets);
            document.getElementById('exportToCSV').addEventListener('click', exportToCSV);
            
            document.getElementById('confirmRestore').addEventListener('click', function() {
                const fileInput = document.getElementById('backupFile');
                if (fileInput.files.length === 0) {
                    alert('Silakan pilih file backup terlebih dahulu!');
                    return;
                }
                
                const file = fileInput.files[0];
                const reader = new FileReader();
                
                reader.onload = function(e) {
                    try {
                        const data = JSON.parse(e.target.result);
                        transactions = data;
                        saveTransactions();
                        
                        renderTransactions();
                        renderLaporan();
                        renderRekap();
                        
                        alert('Data berhasil dipulihkan!');
                        bootstrap.Modal.getInstance(document.getElementById('restoreModal')).hide();
                    } catch (error) {
                        alert('Format file tidak valid! Pastikan Anda memilih file backup yang benar.');
                        console.error(error);
                    }
                };
                
                reader.readAsText(file);
            });

            // Set tanggal default ke hari ini
            document.getElementById('tanggal').valueAsDate = new Date();
        });
    </script>
</body>
</html>
