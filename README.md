<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Absensi Harian — Ruang Kerja</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700&family=Instrument+Serif:ital@0;1&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #f6f4ef;
      --surface: #fffdf9;
      --surface-alt: #efeae0;
      --ink: #1c1b18;
      --muted: #6c685f;
      --line: rgba(28, 27, 24, 0.12);
      --accent: #1e4d3b;
      --accent-ink: #ffffff;
      --accent-soft: rgba(30, 77, 59, 0.1);
      --warn: #9a5b13;
      --warn-soft: rgba(154, 91, 19, 0.12);
      --danger: #9e2a2b;
      --danger-soft: rgba(158, 42, 43, 0.1);
      --radius: 16px;
    }

    *,
    *::before,
    *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Plus Jakarta Sans', -apple-system, BlinkMacSystemFont, sans-serif;
      background-color: var(--bg);
      background-image: radial-gradient(circle at 12% 8%, rgba(30, 77, 59, 0.06), transparent 42%),
                        radial-gradient(circle at 88% 90%, rgba(154, 91, 19, 0.06), transparent 45%);
      color: var(--ink);
      min-height: 100vh;
      line-height: 1.5;
      padding: 28px 18px 64px;
    }

    .wrap {
      max-width: 780px;
      margin: 0 auto;
    }

    /* Header */
    header {
      display: flex;
      flex-wrap: wrap;
      align-items: flex-end;
      justify-content: space-between;
      gap: 16px;
      padding-bottom: 20px;
      border-bottom: 1px solid var(--line);
      margin-bottom: 28px;
    }

    .brand-tag {
      font-size: 12px;
      font-weight: 600;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 4px;
    }

    h1 {
      font-family: 'Instrument Serif', Georgia, serif;
      font-weight: 400;
      font-size: clamp(2rem, 4.5vw, 2.75rem);
      line-height: 1.08;
      letter-spacing: -0.01em;
    }

    h1 em {
      font-style: italic;
      color: var(--accent);
    }

    .live-clock {
      text-align: right;
      font-size: 13px;
      color: var(--muted);
    }

    .live-clock strong {
      display: block;
      font-size: 15px;
      font-weight: 600;
      color: var(--ink);
      font-variant-numeric: tabular-nums;
    }

    /* Card */
    .card {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      padding: 26px;
      box-shadow: 0 10px 30px -18px rgba(28, 27, 24, 0.25);
    }

    .card-head {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      margin-bottom: 20px;
    }

    .card-head h2 {
      font-size: 17px;
      font-weight: 700;
    }

    .card-head p {
      font-size: 13px;
      color: var(--muted);
    }

    /* Tampilan Awal / Welcome Screen */
    .welcome-card {
      position: relative;
      overflow: hidden;
      padding: 34px 28px;
    }

    .welcome-card::after {
      content: "";
      position: absolute;
      right: -48px;
      top: -48px;
      width: 180px;
      height: 180px;
      border-radius: 999px;
      background: radial-gradient(circle, rgba(30, 77, 59, 0.1), transparent 70%);
      pointer-events: none;
    }

    .welcome-eyebrow {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 5px 12px;
      border-radius: 999px;
      background: var(--accent-soft);
      color: var(--accent);
      font-size: 12px;
      font-weight: 600;
      margin-bottom: 14px;
    }

    .welcome-title {
      font-family: 'Instrument Serif', Georgia, serif;
      font-weight: 400;
      font-size: clamp(1.9rem, 4vw, 2.5rem);
      line-height: 1.12;
      margin-bottom: 10px;
      max-width: 22ch;
    }

    .welcome-title em {
      font-style: italic;
      color: var(--accent);
    }

    .welcome-desc {
      font-size: 14px;
      color: var(--muted);
      max-width: 54ch;
      margin-bottom: 24px;
    }

    @media (max-width: 600px) {
      .welcome-card {
        padding: 24px 20px;
      }
      .live-clock {
        text-align: left;
      }
    }

    /* Form */
    #absensi-page {
      display: none;
    }

    #absensi-page.active {
      display: block;
      animation: fadeIn 0.25s ease;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
    }

    @media (max-width: 600px) {
      .form-grid {
        grid-template-columns: 1fr;
      }
    }

    .field {
      display: flex;
      flex-direction: column;
      gap: 7px;
    }

    .field.full {
      grid-column: 1 / -1;
    }

    label {
      font-size: 13px;
      font-weight: 600;
      color: var(--ink);
    }

    input[type="text"],
    select,
    textarea {
      width: 100%;
      font-family: inherit;
      font-size: 14px;
      color: var(--ink);
      background: var(--bg);
      border: 1px solid var(--line);
      border-radius: 10px;
      padding: 11px 13px;
      transition: border-color 0.15s, background 0.15s;
    }

    input[type="text"]:focus,
    select:focus,
    textarea:focus {
      outline: none;
      border-color: var(--accent);
      background: #fff;
    }

    textarea {
      resize: vertical;
      min-height: 76px;
    }

    /* Status pill selector */
    .status-group {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
    }

    .status-option input {
      position: absolute;
      opacity: 0;
      pointer-events: none;
    }

    .status-pill {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 7px;
      padding: 10px 12px;
      border-radius: 10px;
      border: 1px solid var(--line);
      background: var(--bg);
      font-size: 13.5px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.15s ease;
      user-select: none;
    }

    .status-option input:checked + .status-pill {
      border-color: var(--accent);
      background: var(--accent-soft);
      color: var(--accent);
    }

    /* Buttons */
    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      font-family: inherit;
      font-size: 14px;
      font-weight: 600;
      border: none;
      border-radius: 11px;
      padding: 12px 20px;
      cursor: pointer;
      transition: transform 0.1s, opacity 0.15s, background 0.15s;
    }

    .btn:active {
      transform: scale(0.99);
    }

    .btn-primary {
      background: var(--accent);
      color: var(--accent-ink);
      width: 100%;
      margin-top: 6px;
    }

    .btn-primary:hover {
      opacity: 0.92;
    }

    .btn-ghost {
      background: transparent;
      color: var(--muted);
      border: 1px solid var(--line);
      padding: 8px 13px;
      font-size: 12.5px;
      border-radius: 9px;
    }

    .btn-ghost:hover {
      color: var(--ink);
      border-color: rgba(28, 27, 24, 0.28);
    }

    .btn-danger-soft {
      background: var(--danger-soft);
      color: var(--danger);
      padding: 7px 12px;
      font-size: 12px;
      border-radius: 8px;
    }

    /* Tampilan setelah kirim absensi */
    .submitted-view {
      display: none;
      text-align: center;
      padding: 28px 16px;
      border-radius: 14px;
      background: var(--accent-soft);
      border: 1px solid rgba(30, 77, 59, 0.22);
    }

    .submitted-view.show {
      display: block;
      animation: fadeIn 0.25s ease;
    }

    .submitted-message {
      font-size: 20px;
      font-weight: 700;
      color: var(--accent);
      margin-bottom: 8px;
    }

    .receipt-meta {
      font-size: 13.5px;
      color: var(--ink);
    }

    .receipt-note {
      margin-top: 10px;
      font-size: 12px;
      color: var(--muted);
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(4px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* Owner Footer Bar */
    .owner-bar {
      margin-top: 28px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 12px;
      font-size: 12.5px;
      color: var(--muted);
    }

    /* Owner Panel */
    #owner-panel {
      display: none;
      margin-top: 22px;
    }

    #owner-panel.open {
      display: block;
      animation: fadeIn 0.25s ease;
    }

    .stats-row {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      margin-bottom: 18px;
    }

    @media (max-width: 600px) {
      .stats-row {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    .stat-box {
      background: var(--surface-alt);
      border-radius: 12px;
      padding: 12px 14px;
    }

    .stat-box span {
      display: block;
      font-size: 11.5px;
      color: var(--muted);
    }

    .stat-box strong {
      font-size: 20px;
      font-weight: 700;
      font-variant-numeric: tabular-nums;
    }

    .toolbar {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 14px;
    }

    .toolbar input,
    .toolbar select {
      width: auto;
      padding: 8px 11px;
      font-size: 13px;
    }

    .toolbar-left {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .table-wrap {
      overflow-x: auto;
      border: 1px solid var(--line);
      border-radius: 12px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 13.5px;
      text-align: left;
    }

    th {
      background: var(--surface-alt);
      font-size: 11.5px;
      text-transform: uppercase;
      letter-spacing: 0.04em;
      color: var(--muted);
      padding: 11px 14px;
      border-bottom: 1px solid var(--line);
      white-space: nowrap;
    }

    td {
      padding: 12px 14px;
      border-bottom: 1px solid var(--line);
      vertical-align: top;
    }

    tr:last-child td {
      border-bottom: none;
    }

    .badge {
      display: inline-block;
      padding: 3px 9px;
      border-radius: 999px;
      font-size: 11.5px;
      font-weight: 600;
    }

    .badge-hadir {
      background: var(--accent-soft);
      color: var(--accent);
    }

    .badge-izin {
      background: var(--warn-soft);
      color: var(--warn);
    }

    .badge-sakit {
      background: var(--danger-soft);
      color: var(--danger);
    }

    .empty-state {
      text-align: center;
      padding: 32px 16px;
      color: var(--muted);
      font-size: 13.5px;
    }

    /* Modal PIN */
    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(28, 27, 24, 0.45);
      backdrop-filter: blur(3px);
      display: none;
      align-items: center;
      justify-content: center;
      padding: 16px;
      z-index: 50;
    }

    .modal-backdrop.open {
      display: flex;
    }

    .modal {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      width: 100%;
      max-width: 360px;
      padding: 24px;
      box-shadow: 0 20px 50px rgba(0,0,0,0.22);
    }

    .modal h3 {
      font-size: 17px;
      margin-bottom: 6px;
    }

    .modal p {
      font-size: 13px;
      color: var(--muted);
      margin-bottom: 14px;
    }

    .modal input[type="password"] {
      width: 100%;
      font-family: inherit;
      font-size: 16px;
      letter-spacing: 0.25em;
      text-align: center;
      padding: 10px 12px;
      border-radius: 10px;
      border: 1px solid var(--line);
      background: var(--bg);
      margin-bottom: 10px;
    }

    .modal-error {
      color: var(--danger);
      font-size: 12px;
      min-height: 18px;
      margin-bottom: 8px;
    }

    .modal-actions {
      display: flex;
      gap: 8px;
      justify-content: flex-end;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div>
        <div class="brand-tag">Catatan Kerinduan</div>
        <h1>Form Absensi <em>Harian</em></h1>
      </div>
      <div class="live-clock">
        <strong id="clock-time">--:--:--</strong>
        <span id="clock-date">Memuat tanggal...</span>
      </div>
    </header>

    <!-- Tampilan Awal Sebelum Masuk ke Halaman Absensi -->
    <section id="welcome-screen" class="card welcome-card">
      <div class="welcome-eyebrow">Milik Abimanyu</div>
      <h2 class="welcome-title">Selamat datang kamu , silakan catat <em>kehadiran hari ini yaww!</em></h2>
      <p class="welcome-desc">
        Lengkapi nama, divisi, serta status kehadiran Anda secara singkat. Setiap kiriman tercatat privat untuk kepentingan privasi.
      </p>

      <button type="button" class="btn btn-primary" id="btn-enter-form">Mulai Isi Absensi</button>
    </section>

    <!-- Halaman Pengguna -->
    <main id="absensi-page" class="card">
      <div id="form-section">
        <div class="card-head">
          <div>
            <h2>Isi Kehadiran Anda</h2>
            <p>Data yang dikirim langsung tersimpan privat untuk pengelola.</p>
          </div>
        </div>

        <form id="absen-form" autocomplete="off">
          <div class="form-grid">
            <div class="field">
              <label for="nama">Nama Lengkap *</label>
              <input type="text" id="nama" name="nama" placeholder="Contoh: Raka Pratama" required />
            </div>

            <div class="field">
              <label for="divisi">Divisi / Bagian</label>
              <input type="text" id="divisi" name="divisi" placeholder="Contoh: Operasional / Desain" />
            </div>

            <div class="field full">
              <label>Status Kehadiran *</label>
              <div class="status-group">
                <label class="status-option">
                  <input type="radio" name="status" value="Hadir" checked />
                  <span class="status-pill">Hadir</span>
                </label>
                <label class="status-option">
                  <input type="radio" name="status" value="Izin" />
                  <span class="status-pill">Izin</span>
                </label>
                <label class="status-option">
                  <input type="radio" name="status" value="Sakit" />
                  <span class="status-pill">Sakit</span>
                </label>
              </div>
            </div>

            <div class="field full">
              <label for="keterangan">Catatan / Keterangan (opsional)</label>
              <textarea id="keterangan" name="keterangan" placeholder="Tulis keterangan singkat jika diperlukan..."></textarea>
            </div>

            <div class="field full">
              <button type="submit" class="btn btn-primary">Kirim Absensi</button>
            </div>
          </div>
        </form>
      </div>

      <div id="submitted-view" class="submitted-view" role="status" aria-live="polite">
        <div class="submitted-message">terimaksih telah mengisi</div>
        <div class="receipt-meta" id="receipt-detail"></div>
        <div class="receipt-note">Demi privasi, daftar kehadiran pengguna lain tidak ditampilkan di halaman ini.</div>
      </div>
    </main>

    <!-- Akses khusus pemilik situs -->
    <div class="owner-bar">
      <span>Rekap kehadiran hanya dapat diakses oleh pemilik situs.</span>
      <button type="button" class="btn btn-ghost" id="btn-owner-toggle">Akses Pemilik</button>
    </div>

    <!-- Panel Pemilik (Tersembunyi hingga PIN valid) -->
    <section id="owner-panel" class="card">
      <div class="card-head">
        <div>
          <h2>Rekap Absensi Masuk</h2>
          <p>Hanya tampil untuk pemilik situs</p>
        </div>
        <button type="button" class="btn btn-ghost" id="btn-lock">Kunci Panel</button>
      </div>

      <div class="stats-row">
        <div class="stat-box">
          <span>Total Masuk</span>
          <strong id="stat-total">0</strong>
        </div>
        <div class="stat-box">
          <span>Hadir</span>
          <strong id="stat-hadir">0</strong>
        </div>
        <div class="stat-box">
          <span>Izin</span>
          <strong id="stat-izin">0</strong>
        </div>
        <div class="stat-box">
          <span>Sakit</span>
          <strong id="stat-sakit">0</strong>
        </div>
      </div>

      <div class="toolbar">
        <div class="toolbar-left">
          <input type="text" id="filter-search" placeholder="Cari nama atau divisi..." />
          <select id="filter-status">
            <option value="ALL">Semua Status</option>
            <option value="Hadir">Hadir</option>
            <option value="Izin">Izin</option>
            <option value="Sakit">Sakit</option>
          </select>
        </div>
        <div class="toolbar-left">
          <button type="button" class="btn btn-ghost" id="btn-export">Unduh CSV</button>
          <button type="button" class="btn btn-danger-soft" id="btn-clear">Hapus Semua</button>
        </div>
      </div>

      <div class="table-wrap">
        <table>
          <thead>
            <tr>
              <th>Waktu</th>
              <th>Nama</th>
              <th>Divisi</th>
              <th>Status</th>
              <th>Keterangan</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody id="rekap-body"></tbody>
        </table>
      </div>
    </section>
  </div>

  <!-- Modal PIN Pemilik -->
  <div class="modal-backdrop" id="pin-modal" role="dialog" aria-modal="true" aria-labelledby="pin-title">
    <div class="modal">
      <h3 id="pin-title">Verifikasi Pemilik</h3>
      <p>Masukkan PIN pemilik untuk membuka rekap absensi. (Default: <code>1234</code>)</p>
      <form id="pin-form">
        <input type="password" id="pin-input" inputmode="numeric" maxlength="12" placeholder="••••" required />
        <div class="modal-error" id="pin-error"></div>
        <div class="modal-actions">
          <button type="button" class="btn btn-ghost" id="pin-cancel">Batal</button>
          <button type="submit" class="btn btn-primary" style="width:auto;margin-top:0;">Masuk</button>
        </div>
      </form>
    </div>
  </div>

  <script>
    const STORAGE_KEY = 'data_absensi_pemilik_v1';
    const OWNER_PIN = '1234'; // Ganti PIN sesuai kebutuhan Anda

    const welcomeScreen = document.getElementById('welcome-screen');
    const btnEnterForm = document.getElementById('btn-enter-form');
    const absensiPage = document.getElementById('absensi-page');

    const formSection = document.getElementById('form-section');
    const form = document.getElementById('absen-form');
    const submittedView = document.getElementById('submitted-view');
    const receiptDetail = document.getElementById('receipt-detail');

    const btnOwnerToggle = document.getElementById('btn-owner-toggle');
    const btnLock = document.getElementById('btn-lock');
    const ownerPanel = document.getElementById('owner-panel');

    const pinModal = document.getElementById('pin-modal');
    const pinForm = document.getElementById('pin-form');
    const pinInput = document.getElementById('pin-input');
    const pinError = document.getElementById('pin-error');
    const pinCancel = document.getElementById('pin-cancel');

    const filterSearch = document.getElementById('filter-search');
    const filterStatus = document.getElementById('filter-status');
    const rekapBody = document.getElementById('rekap-body');
    const btnExport = document.getElementById('btn-export');
    const btnClear = document.getElementById('btn-clear');

    let isOwnerUnlocked = false;

    // Jam & tanggal real-time
    function updateClock() {
      const now = new Date();
      document.getElementById('clock-time').textContent = now.toLocaleTimeString('id-ID', {
        hour: '2-digit',
