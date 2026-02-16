# GerejaPentakosta
Gereja Pentakosta Umum
<!doctype html>
<html lang="id" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard Gereja Pentakosta</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="/_sdk/data_sdk.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800&amp;display=swap" rel="stylesheet">
  <style>
    * { font-family: 'Nunito', sans-serif; }
    .nav-active { background: linear-gradient(135deg, #7c2d12 0%, #b45309 100%); color: white; }
    .card-hover:hover { transform: translateY(-2px); box-shadow: 0 8px 25px rgba(124, 45, 18, 0.15); }
    .fade-in { animation: fadeIn 0.3s ease-out; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    .btn-primary { background: linear-gradient(135deg, #7c2d12 0%, #b45309 100%); }
    .btn-primary:hover { background: linear-gradient(135deg, #b45309 0%, #d97706 100%); }
    .btn-secondary { background: linear-gradient(135deg, #1e40af 0%, #1e3a8a 100%); }
    .btn-secondary:hover { background: linear-gradient(135deg, #1e3a8a 0%, #172554 100%); }
    .tab-nav { display: flex; flex-wrap: wrap; gap: 8px; }
    .tab-nav button { padding: 10px 16px; border-radius: 8px; font-weight: 600; font-size: 14px; transition: all 0.3s; }
  </style>
  <style>body { box-sizing: border-box; }</style>
 </head>
 <body class="h-full bg-gradient-to-br from-orange-50 to-amber-50 overflow-auto">
  <div id="app" class="h-full w-full"><!-- LOGIN SCREEN -->
   <div id="login-screen" class="min-h-full flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl shadow-2xl p-8 max-w-md w-full fade-in">
     <div class="text-center mb-8">
      <div class="text-5xl mb-3">
       ⛪
      </div>
      <h1 class="text-2xl font-bold text-[#7c2d12]">Gereja Pentakosta</h1>
      <p class="text-gray-500 text-sm mt-1">Sistem Manajemen Gereja</p>
     </div>
     <form id="login-form" class="space-y-4">
      <div><label class="block text-sm font-semibold text-gray-700 mb-2">Username</label> <input type="text" id="login-username" required placeholder="Masukkan username" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
      </div>
      <div><label class="block text-sm font-semibold text-gray-700 mb-2">Password</label> <input type="password" id="login-password" required placeholder="Masukkan password" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
      </div><button type="submit" class="w-full btn-primary text-white py-3 rounded-lg font-bold mt-6">🔐 Masuk</button>
     </form>
     <div class="mt-6 p-4 bg-blue-50 rounded-lg">
      <p class="text-xs text-gray-600"><strong>Demo Account:</strong></p>
      <p class="text-xs text-gray-500 mt-1">👨‍💼 <strong>Admin:</strong> admin / admin123</p>
      <p class="text-xs text-gray-500">👤 <strong>User:</strong> user / user123</p>
     </div>
     <div id="login-error" class="mt-4 p-3 bg-red-50 text-red-600 text-sm rounded-lg hidden text-center"></div>
    </div>
   </div><!-- MAIN DASHBOARD -->
   <div id="dashboard" class="hidden">
    <header class="bg-gradient-to-r from-[#7c2d12] to-[#b45309] text-white px-6 py-5 shadow-lg sticky top-0 z-40">
     <div class="max-w-7xl mx-auto flex items-center justify-between">
      <div class="flex items-center gap-3">
       <div class="w-12 h-12 bg-white/20 rounded-xl flex items-center justify-center text-2xl">
        ⛪
       </div>
       <div>
        <h1 class="text-2xl font-bold">Gereja Pentakosta</h1>
        <p class="text-orange-200 text-sm">Sistem Manajemen Gereja</p>
       </div>
      </div>
      <div class="flex items-center gap-4">
       <div class="text-right">
        <p id="user-name" class="font-semibold">User</p>
        <p id="user-level" class="text-xs text-orange-200">Level Akses</p>
       </div><button type="button" id="logout-btn" class="px-4 py-2 bg-white/20 hover:bg-white/30 rounded-lg transition-all">🚪 Logout</button>
      </div>
     </div>
    </header><!-- Cabang Selector -->
    <div class="max-w-7xl mx-auto px-6 py-4 bg-white/60 backdrop-blur-sm sticky top-16 z-30 shadow-sm">
     <div class="flex items-center gap-2 flex-wrap"><span class="text-sm font-semibold text-gray-700">📍 Pilih Cabang:</span>
      <div id="cabang-selector" class="flex gap-2 flex-wrap"></div><button id="add-cabang-btn" type="button" onclick="showAddCabangModal()" class="ml-auto btn-secondary text-white px-4 py-2 rounded-lg font-semibold transition-all duration-300 flex items-center gap-2 text-sm hidden">➕ Tambah Cabang</button>
     </div>
    </div><!-- Navigation Menu -->
    <div class="max-w-7xl mx-auto px-6 py-4 sticky top-32 z-30 bg-white rounded-lg shadow-sm mb-4">
     <div class="tab-nav" id="main-nav"><button type="button" onclick="switchModule('beranda')" data-module="beranda" class="nav-active text-white">🏠 Beranda</button> <button type="button" onclick="switchModule('penkerja')" data-module="penkerja" class="bg-gray-100 text-gray-600">🙏 Pengerja</button> <button type="button" onclick="switchModule('jemaat')" data-module="jemaat" class="bg-gray-100 text-gray-600">👥 Jemaat</button> <button type="button" onclick="switchModule('kegiatan')" data-module="kegiatan" class="bg-gray-100 text-gray-600">📅 Kegiatan</button> <button type="button" onclick="switchModule('keuangan')" data-module="keuangan" class="bg-gray-100 text-gray-600">💰 Keuangan</button> <button type="button" onclick="switchModule('surat')" data-module="surat" class="bg-gray-100 text-gray-600">📨 Surat</button> <button type="button" onclick="switchModule('kehadiran')" data-module="kehadiran" class="bg-gray-100 text-gray-600">✅ Kehadiran</button> <button type="button" onclick="switchModule('search')" data-module="search" class="bg-gray-100 text-gray-600">🔍 Search</button> <button type="button" id="users-tab" onclick="switchModule('users')" data-module="users" class="bg-gray-100 text-gray-600 hidden">👨‍💼 Kelola User</button>
     </div>
    </div><!-- Main Content -->
    <div class="max-w-7xl mx-auto px-6 pb-6"><!-- BERANDA MODULE -->
     <div id="beranda-module" class="module-content">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
       <div class="bg-white rounded-xl p-4 shadow-md card-hover">
        <div class="flex items-center gap-3">
         <div class="w-10 h-10 bg-blue-100 rounded-lg flex items-center justify-center text-xl">
          🙏
         </div>
         <div>
          <p class="text-gray-500 text-xs">Total Pengerja</p>
          <p id="stat-pengerja" class="text-xl font-bold text-[#7c2d12]">0</p>
         </div>
        </div>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md card-hover">
        <div class="flex items-center gap-3">
         <div class="w-10 h-10 bg-green-100 rounded-lg flex items-center justify-center text-xl">
          👨‍👩‍👧‍👦
         </div>
         <div>
          <p class="text-gray-500 text-xs">Total Jemaat</p>
          <p id="stat-jemaat" class="text-xl font-bold text-[#7c2d12]">0</p>
         </div>
        </div>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md card-hover">
        <div class="flex items-center gap-3">
         <div class="w-10 h-10 bg-purple-100 rounded-lg flex items-center justify-center text-xl">
          💰
         </div>
         <div>
          <p class="text-gray-500 text-xs">Dana Kas Tersimpan</p>
          <p id="stat-dana" class="text-xl font-bold text-[#7c2d12]">Rp 0</p>
         </div>
        </div>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md card-hover">
        <div class="flex items-center gap-3">
         <div class="w-10 h-10 bg-amber-100 rounded-lg flex items-center justify-center text-xl">
          📨
         </div>
         <div>
          <p class="text-gray-500 text-xs">Surat Bulan Ini</p>
          <p id="stat-surat" class="text-xl font-bold text-[#7c2d12]">0</p>
         </div>
        </div>
       </div>
      </div>
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
       <div class="bg-white rounded-xl p-5 shadow-md">
        <h3 class="font-bold text-[#7c2d12] mb-4">📅 Kegiatan Mendatang</h3>
        <div id="upcoming-activities" class="space-y-3 max-h-80 overflow-y-auto"></div>
       </div>
       <div class="bg-white rounded-xl p-5 shadow-md">
        <h3 class="font-bold text-[#7c2d12] mb-4">📊 Ringkasan Kehadiran Ibadah (4 Minggu)</h3>
        <div id="attendance-summary" class="space-y-4"></div>
       </div>
      </div>
     </div><!-- PENKERJA MODULE -->
     <div id="penkerja-module" class="module-content hidden">
      <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-5 gap-4 mb-6">
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Total Penkerja</p>
        <p id="peng-total" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Pendeta</p>
        <p id="peng-pendeta" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Pendeta Muda</p>
        <p id="peng-pendeta-muda" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Sintua</p>
        <p id="peng-sintua" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Pembela Sidang</p>
        <p id="peng-pembela" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
      </div>
      <div id="penkerja-form-container" class="bg-white rounded-xl p-5 shadow-md mb-6 hidden">
       <h3 class="font-bold text-[#7c2d12] mb-4">➕ Tambah Pengerja Baru</h3>
       <form id="penkerja-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <div><label class="block text-sm text-gray-600 mb-1">Nama Lengkap *</label> <input type="text" id="pk-nama" required placeholder="Nama lengkap" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Jabatan *</label> <select id="pk-jabatan" required class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none"> <option value="">Pilih Jabatan</option> <option value="Pendeta">Pendeta</option> <option value="Pendeta Muda">Pendeta Muda</option> <option value="Sintua">Sintua</option> <option value="Pembela Sidang">Pembela Sidang</option> </select>
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">No. Telepon</label> <input type="tel" id="pk-telepon" placeholder="08xxxxxxxxxx" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div class="md:col-span-2 lg:col-span-3 flex gap-3"><button type="submit" id="pk-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold flex items-center gap-2"><span>💾</span> Simpan</button> <button type="button" onclick="resetPenkerjaForm()" class="bg-gray-100 text-gray-600 px-6 py-2 rounded-lg font-semibold">Batal</button>
        </div>
       </form>
      </div>
      <div id="penkerja-list" class="space-y-3"></div>
     </div><!-- JEMAAT MODULE -->
     <div id="jemaat-module" class="module-content hidden">
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-6">
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Total Jemaat</p>
        <p id="jem-total" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Kaum Ibu</p>
        <p id="jem-ibu" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Kaum Bapak</p>
        <p id="jem-bapak" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">Anak-anak</p>
        <p id="jem-anak" class="text-2xl font-bold text-[#7c2d12]">0</p>
       </div>
      </div>
      <div id="jemaat-form-container" class="bg-white rounded-xl p-5 shadow-md mb-6 hidden">
       <h3 class="font-bold text-[#7c2d12] mb-4">➕ Tambah Jemaat Baru</h3>
       <form id="jemaat-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <div><label class="block text-sm text-gray-600 mb-1">Nama Lengkap *</label> <input type="text" id="jm-nama" required placeholder="Nama lengkap" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">No. Telepon</label> <input type="tel" id="jm-telepon" placeholder="08xxxxxxxxxx" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Kategori</label> <select id="jm-kategori" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none"> <option value="Kaum Ibu">Kaum Ibu</option> <option value="Kaum Bapak">Kaum Bapak</option> <option value="Anak-anak">Anak-anak</option> </select>
        </div>
        <div class="md:col-span-2 lg:col-span-3 flex gap-3"><button type="submit" id="jm-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold flex items-center gap-2"><span>💾</span> Simpan</button> <button type="button" onclick="resetJemaatForm()" class="bg-gray-100 text-gray-600 px-6 py-2 rounded-lg font-semibold">Batal</button>
        </div>
       </form>
      </div>
      <div id="jemaat-list" class="space-y-3"></div>
     </div><!-- KEGIATAN MODULE -->
     <div id="kegiatan-module" class="module-content hidden">
      <div id="kegiatan-form-container" class="bg-white rounded-xl p-5 shadow-md mb-6 hidden">
       <h3 class="font-bold text-[#7c2d12] mb-4">📅 Rencana Kegiatan</h3>
       <form id="kegiatan-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <div><label class="block text-sm text-gray-600 mb-1">Jenis Kegiatan *</label> <input type="text" id="kg-jenis" required placeholder="Contoh: Ibadah Raya" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Tanggal Kegiatan *</label> <input type="date" id="kg-tanggal" required class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Anggaran (Rp)</label> <input type="number" id="kg-dana" placeholder="0" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div class="md:col-span-2 lg:col-span-3 flex gap-3"><button type="submit" id="kg-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold flex items-center gap-2"><span>💾</span> Simpan</button> <button type="button" onclick="resetKegiatanForm()" class="bg-gray-100 text-gray-600 px-6 py-2 rounded-lg font-semibold">Batal</button>
        </div>
       </form>
      </div>
      <div class="bg-white rounded-xl p-5 shadow-md">
       <h3 class="font-bold text-[#7c2d12] mb-4">📋 Daftar Kegiatan Setahun</h3>
       <div class="overflow-x-auto">
        <table class="w-full text-sm">
         <thead class="bg-orange-100">
          <tr>
           <th class="px-4 py-2 text-left text-[#7c2d12] font-bold">📅 Tanggal</th>
           <th class="px-4 py-2 text-left text-[#7c2d12] font-bold">Jenis Kegiatan</th>
           <th class="px-4 py-2 text-right text-[#7c2d12] font-bold">Anggaran</th>
           <th id="kegiatan-aksi-header" class="px-4 py-2 text-center text-[#7c2d12] font-bold hidden">Aksi</th>
          </tr>
         </thead>
         <tbody id="kegiatan-table" class="divide-y divide-gray-200"></tbody>
        </table>
       </div>
      </div>
     </div><!-- KEUANGAN MODULE -->
     <div id="keuangan-module" class="module-content hidden">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
       <div class="bg-white rounded-xl p-4 shadow-md">
        <p class="text-gray-500 text-sm">💰 Dana Kas Tersimpan</p>
        <p id="keu-total-kas" class="text-3xl font-bold text-[#7c2d12]">Rp 0</p>
       </div>
       <div class="bg-white rounded-xl p-4 shadow-md">
        <div class="flex gap-4">
         <div class="flex-1">
          <p class="text-gray-500 text-xs">Persembahan Minggu</p>
          <p id="keu-minggu" class="text-2xl font-bold text-green-600">Rp 0</p>
         </div>
         <div class="flex-1">
          <p class="text-gray-500 text-xs">Persembahan Tengah Minggu</p>
          <p id="keu-tengah" class="text-2xl font-bold text-blue-600">Rp 0</p>
         </div>
        </div>
       </div>
      </div>
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-4">
       <div id="keuangan-form-container" class="bg-white rounded-xl p-5 shadow-md hidden">
        <h3 class="font-bold text-[#7c2d12] mb-4">💵 Input Dana Masuk</h3>
        <form id="kas-form" class="space-y-4">
         <div><label class="block text-sm text-gray-600 mb-1">Jumlah Dana (Rp) *</label> <input type="number" id="kas-jumlah" required min="0" placeholder="0" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
         </div>
         <div><label class="block text-sm text-gray-600 mb-1">Keterangan</label> <input type="text" id="kas-ket" placeholder="Misal: Sumbangan, Kolekta" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
         </div><button type="submit" id="kas-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold w-full flex items-center justify-center gap-2"><span>💾</span> Simpan</button>
        </form>
       </div>
       <div id="keuangan-keluar-container" class="bg-white rounded-xl p-5 shadow-md hidden">
        <h3 class="font-bold text-[#7c2d12] mb-4">🚪 Input Dana Keluar</h3>
        <form id="keluar-form" class="space-y-4">
         <div><label class="block text-sm text-gray-600 mb-1">Jumlah Dana (Rp) *</label> <input type="number" id="keluar-jumlah" required min="0" placeholder="0" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
         </div>
         <div><label class="block text-sm text-gray-600 mb-1">Keterangan</label> <input type="text" id="keluar-ket" placeholder="Misal: Operasional, Perbaikan" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
         </div><button type="submit" id="keluar-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold w-full flex items-center justify-center gap-2"><span>💾</span> Simpan</button>
        </form>
       </div>
       <div class="bg-white rounded-xl p-5 shadow-md">
        <h3 class="font-bold text-[#7c2d12] mb-4">💒 Persembahan per Minggu</h3>
        <div id="persembahan-mingguan" class="space-y-2"></div>
       </div>
      </div>
      <div class="bg-white rounded-xl p-5 shadow-md mt-4">
       <h3 class="font-bold text-[#7c2d12] mb-4">📊 Daftar Dana Keluar</h3>
       <div id="dana-keluar-list" class="space-y-2"></div>
      </div>
     </div><!-- SURAT MODULE -->
     <div id="surat-module" class="module-content hidden">
      <div id="surat-form-container" class="bg-white rounded-xl p-5 shadow-md mb-6 hidden">
       <h3 class="font-bold text-[#7c2d12] mb-4">📨 Input Surat</h3>
       <form id="surat-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <div><label class="block text-sm text-gray-600 mb-1">Nomor Surat *</label> <input type="text" id="sr-nomor" required placeholder="No. Surat" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Tanggal Surat *</label> <input type="date" id="sr-tanggal" required class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Pengirim/Tujuan *</label> <input type="text" id="sr-pengirim" required placeholder="Nama pengirim/tujuan" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div class="md:col-span-2 lg:col-span-3"><label class="block text-sm text-gray-600 mb-2">📎 Upload File (Foto/PDF)</label>
         <div id="file-upload-area" class="border-2 border-dashed border-gray-300 rounded-lg p-6 text-center cursor-pointer hover:border-orange-400 hover:bg-orange-50 transition-all"><input type="file" id="sr-file" accept=".pdf,.jpg,.jpeg,.png,.gif,.bmp" class="hidden">
          <div id="file-upload-placeholder" class="pointer-events-none">
           <p class="text-3xl mb-2">📎</p>
           <p class="text-sm text-gray-600 font-semibold">Klik atau drag &amp; drop file</p>
           <p class="text-xs text-gray-400 mt-1">PDF, JPG, PNG, GIF, BMP (Max 5MB)</p>
          </div>
          <div id="file-upload-success" class="hidden pointer-events-none">
           <p class="text-3xl mb-2">✅</p>
           <p id="file-name-display" class="text-sm text-green-600 font-semibold"></p>
           <p id="file-size-display" class="text-xs text-gray-500 mt-1"></p><button type="button" onclick="clearFile()" class="text-xs text-orange-500 hover:text-orange-600 mt-2 underline">Ubah file</button>
          </div>
         </div>
        </div>
        <div class="md:col-span-2 lg:col-span-3 flex gap-3"><button type="submit" id="sr-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold flex items-center gap-2"><span>💾</span> Simpan</button> <button type="button" onclick="resetSuratForm()" class="bg-gray-100 text-gray-600 px-6 py-2 rounded-lg font-semibold">Batal</button>
        </div>
       </form>
      </div>
      <div id="surat-list" class="space-y-3"></div>
     </div><!-- KEHADIRAN MODULE -->
     <div id="kehadiran-module" class="module-content hidden">
      <div id="kehadiran-form-container" class="bg-white rounded-xl p-5 shadow-md mb-6 hidden">
       <h3 class="font-bold text-[#7c2d12] mb-4">✅ Input Kehadiran Ibadah</h3>
       <form id="kehadiran-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <div><label class="block text-sm text-gray-600 mb-1">Jenis Ibadah *</label> <select id="kh-jenis" required class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none"> <option value="">Pilih Ibadah</option> <option value="Ibadah Raya">Ibadah Raya</option> <option value="Ibadah Tengah Minggu">Ibadah Tengah Minggu</option> <option value="Pengerja Doa Pra Ibadah">Pengerja Doa Pra Ibadah</option> <option value="Ibadah Kaum Ibu">Ibadah Kaum Ibu</option> <option value="Ibadah Kaum Bapak">Ibadah Kaum Bapak</option> <option value="Ibadah Kaum Pemuda">Ibadah Kaum Pemuda</option> <option value="Ibadah Kaum Remaja">Ibadah Kaum Remaja</option> </select>
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Tanggal Ibadah *</label> <input type="date" id="kh-tanggal" required class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Jumlah Hadir *</label> <input type="number" id="kh-jumlah" required min="0" placeholder="0" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Persembahan (Rp)</label> <input type="number" id="kh-persembahan" min="0" placeholder="0" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div class="md:col-span-2 lg:col-span-4 flex gap-3"><button type="submit" id="kh-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold flex items-center gap-2 flex-1"><span>💾</span> Simpan</button> <button type="button" onclick="resetKehadiranForm()" class="bg-gray-100 text-gray-600 px-4 py-2 rounded-lg font-semibold">Batal</button>
        </div>
       </form>
      </div>
      <div id="kehadiran-list" class="space-y-3"></div>
     </div><!-- SEARCH MODULE -->
     <div id="search-module" class="module-content hidden">
      <div class="grid grid-cols-1 lg:grid-cols-4 gap-4 mb-6">
       <div class="lg:col-span-3 bg-white rounded-xl p-5 shadow-md">
        <h3 class="font-bold text-[#7c2d12] mb-4">🔍 Cari Data Per Cabang</h3>
        <div class="space-y-4">
         <div><label class="block text-sm font-semibold text-gray-700 mb-2">📍 Pilih Cabang</label> <select id="search-cabang" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none"> <option value="">Semua Cabang</option> </select>
         </div>
         <div><label class="block text-sm font-semibold text-gray-700 mb-2">📂 Tipe Data</label>
          <div class="grid grid-cols-2 gap-2"><label class="flex items-center gap-2 cursor-pointer"> <input type="checkbox" name="search-type" value="penkerja" class="w-4 h-4 text-orange-600"> <span class="text-sm">🙏 Penkerja</span> </label> <label class="flex items-center gap-2 cursor-pointer"> <input type="checkbox" name="search-type" value="jemaat" class="w-4 h-4 text-orange-600"> <span class="text-sm">👥 Jemaat</span> </label> <label class="flex items-center gap-2 cursor-pointer"> <input type="checkbox" name="search-type" value="kegiatan" class="w-4 h-4 text-orange-600"> <span class="text-sm">📅 Kegiatan</span> </label> <label class="flex items-center gap-2 cursor-pointer"> <input type="checkbox" name="search-type" value="surat" class="w-4 h-4 text-orange-600"> <span class="text-sm">📨 Surat</span> </label> <label class="flex items-center gap-2 cursor-pointer"> <input type="checkbox" name="search-type" value="kehadiran" class="w-4 h-4 text-orange-600"> <span class="text-sm">✅ Kehadiran</span> </label> <label class="flex items-center gap-2 cursor-pointer"> <input type="checkbox" name="search-type" value="kas" class="w-4 h-4 text-orange-600"> <span class="text-sm">💰 Kas</span> </label>
          </div>
         </div>
         <div><label class="block text-sm font-semibold text-gray-700 mb-2">🔎 Cari Keyword</label> <input type="text" id="search-keyword" placeholder="Ketik nama, nomor, atau keterangan..." class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
         </div>
        </div>
       </div>
       <div class="bg-gradient-to-br from-orange-100 to-amber-100 rounded-xl p-5 shadow-md flex flex-col justify-center">
        <p class="text-sm text-gray-600 mb-2">📊 Total Hasil</p>
        <p id="search-result-count" class="text-4xl font-bold text-[#7c2d12] mb-4">0</p><button type="button" onclick="resetSearchForm()" class="bg-white text-[#7c2d12] px-4 py-2 rounded-lg font-semibold hover:bg-orange-50 transition-all">🔄 Reset</button>
       </div>
      </div>
      <div id="search-results" class="space-y-3"></div>
     </div><!-- USER MANAGEMENT MODULE (ADMIN ONLY) -->
     <div id="users-module" class="module-content hidden">
      <div id="user-form-container" class="bg-white rounded-xl p-5 shadow-md mb-6 hidden">
       <h3 class="font-bold text-[#7c2d12] mb-4">👤 Tambah User Baru</h3>
       <form id="user-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <div><label class="block text-sm text-gray-600 mb-1">Nama User *</label> <input type="text" id="u-nama" required placeholder="Nama pengguna" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Username *</label> <input type="text" id="u-username" required placeholder="Username unik" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Password *</label> <input type="password" id="u-password" required placeholder="Password" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Level Akses *</label> <select id="u-level" required class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none"> <option value="user">👤 User</option> <option value="admin">👑 Admin</option> </select>
        </div>
        <div><label class="block text-sm text-gray-600 mb-1">Cabang (Jika User)</label> <select id="u-cabang" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none"> <option value="">--Pilih Cabang--</option> </select>
        </div>
        <div class="md:col-span-2 lg:col-span-3 flex gap-3"><button type="submit" id="u-submit-btn" class="btn-primary text-white px-6 py-2 rounded-lg font-semibold flex items-center gap-2"><span>💾</span> Simpan User</button> <button type="button" onclick="resetUserForm()" class="bg-gray-100 text-gray-600 px-6 py-2 rounded-lg font-semibold">Batal</button>
        </div>
       </form>
      </div>
      <div class="bg-white rounded-xl p-5 shadow-md">
       <div class="flex items-center justify-between mb-4">
        <h3 class="font-bold text-[#7c2d12]">👨‍💼 Daftar Pengguna</h3><button type="button" id="show-user-form-btn" onclick="toggleUserFormVisibility()" class="btn-primary text-white px-4 py-2 rounded-lg font-semibold flex items-center gap-2 text-sm">➕ Tambah User</button>
       </div>
       <div class="overflow-x-auto">
        <table class="w-full text-sm">
         <thead class="bg-orange-100">
          <tr>
           <th class="px-4 py-2 text-left text-[#7c2d12] font-bold">👤 Nama</th>
           <th class="px-4 py-2 text-left text-[#7c2d12] font-bold">📧 Username</th>
           <th class="px-4 py-2 text-left text-[#7c2d12] font-bold">🔐 Level</th>
           <th class="px-4 py-2 text-left text-[#7c2d12] font-bold">📍 Cabang</th>
           <th class="px-4 py-2 text-center text-[#7c2d12] font-bold">⚙️ Aksi</th>
          </tr>
         </thead>
         <tbody id="users-table" class="divide-y divide-gray-200"></tbody>
        </table>
       </div>
      </div>
     </div>
    </div>
   </div><!-- Modals -->
   <div id="cabang-modal" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-xl p-6 max-w-sm w-full shadow-2xl fade-in">
     <h3 class="text-lg font-bold text-gray-800 mb-4">📍 Tambah Cabang</h3>
     <form id="cabang-form" class="space-y-4">
      <div><label class="block text-sm text-gray-600 mb-1">Nama Cabang *</label> <input type="text" id="cabang-nama" required placeholder="Nama cabang" class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:ring-2 focus:ring-orange-300 outline-none">
      </div>
      <div class="flex gap-3"><button type="button" onclick="closeCabangModal()" class="flex-1 py-2 px-4 bg-gray-100 text-gray-700 rounded-lg font-semibold">Batal</button> <button type="submit" class="flex-1 py-2 px-4 btn-primary text-white rounded-lg font-semibold">Simpan</button>
      </div>
     </form>
    </div>
   </div>
   <div id="delete-modal" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-xl p-6 max-w-sm w-full shadow-2xl fade-in">
     <div class="text-center">
      <div class="text-5xl mb-4">
       🗑️
      </div>
      <h3 class="text-lg font-bold text-gray-800 mb-2">Hapus Data?</h3>
      <p id="delete-name" class="text-gray-600 mb-6">Apakah Anda yakin?</p>
      <div class="flex gap-3"><button type="button" onclick="cancelDelete()" class="flex-1 py-2 px-4 bg-gray-100 text-gray-700 rounded-lg font-semibold">Batal</button> <button type="button" onclick="confirmDelete()" class="flex-1 py-2 px-4 bg-red-500 text-white rounded-lg font-semibold">Hapus</button>
      </div>
     </div>
    </div>
   </div>
   <div id="loading-overlay" class="hidden fixed inset-0 bg-white/80 flex items-center justify-center z-50">
    <div class="text-center">
     <div class="w-12 h-12 border-4 border-orange-200 border-t-[#7c2d12] rounded-full animate-spin mx-auto mb-3"></div>
     <p class="text-gray-600">Memproses...</p>
    </div>
   </div>
  </div>
  <script>
    let allData = [];
    let currentUser = null;
    let currentModule = 'beranda';
    let currentCabang = null;
    let currentDeleteTarget = null;
    let currentEditingId = null;
    let currentEditingType = null;
    let selectedFile = null;
    let selectedFileBase64 = null;

    const DEMO_USERS = [
      { username: 'admin', password: 'admin123', level_akses: 'admin', nama_user: 'Administrator' },
      { username: 'user', password: 'user123', level_akses: 'user', cabang_akses: 'cab_1', nama_user: 'User Gereja' }
    ];

    const dataHandler = {
      onDataChanged: (data) => {
        allData = data;
        updateUI();
      }
    };

    async function init() {
      if (window.dataSdk) await window.dataSdk.init(dataHandler);

      document.getElementById('login-form').addEventListener('submit', handleLogin);
      document.getElementById('logout-btn').addEventListener('click', handleLogout);
      document.getElementById('penkerja-form').addEventListener('submit', handlePenkerjaSubmit);
      document.getElementById('jemaat-form').addEventListener('submit', handleJemaatSubmit);
      document.getElementById('kegiatan-form').addEventListener('submit', handleKegiatanSubmit);
      document.getElementById('surat-form').addEventListener('submit', handleSuratSubmit);
      document.getElementById('kehadiran-form').addEventListener('submit', handleKehadiranSubmit);
      document.getElementById('cabang-form').addEventListener('submit', handleAddCabang);
      document.getElementById('kas-form').addEventListener('submit', handleKasSubmit);
      document.getElementById('keluar-form').addEventListener('submit', handleKeluarSubmit);
      document.getElementById('user-form').addEventListener('submit', handleUserSubmit);

      document.getElementById('search-cabang').addEventListener('change', performSearch);
      document.querySelectorAll('input[name="search-type"]').forEach(checkbox => {
        checkbox.addEventListener('change', performSearch);
      });
      document.getElementById('search-keyword').addEventListener('keyup', performSearch);

      const fileUploadArea = document.getElementById('file-upload-area');
      const fileInput = document.getElementById('sr-file');

      fileUploadArea.addEventListener('click', () => fileInput.click());
      fileUploadArea.addEventListener('dragover', (e) => {
        e.preventDefault();
        fileUploadArea.classList.add('border-orange-400', 'bg-orange-50');
      });
      fileUploadArea.addEventListener('dragleave', () => {
        fileUploadArea.classList.remove('border-orange-400', 'bg-orange-50');
      });
      fileUploadArea.addEventListener('drop', (e) => {
        e.preventDefault();
        fileUploadArea.classList.remove('border-orange-400', 'bg-orange-50');
        if (e.dataTransfer.files.length > 0) {
          handleFileSelect(e.dataTransfer.files[0]);
        }
      });

      fileInput.addEventListener('change', (e) => {
        if (e.target.files.length > 0) {
          handleFileSelect(e.target.files[0]);
        }
      });

      document.getElementById('u-level').addEventListener('change', (e) => {
        const cabangSelect = document.getElementById('u-cabang');
        cabangSelect.disabled = e.target.value === 'admin';
        if (e.target.value === 'admin') {
          cabangSelect.value = '';
        }
      });
    }

    init();

    function handleFileSelect(file) {
      const maxSize = 5 * 1024 * 1024;
      const allowedTypes = ['application/pdf', 'image/jpeg', 'image/png', 'image/gif', 'image/bmp'];

      if (file.size > maxSize) {
        showToast('Ukuran file terlalu besar (max 5MB)', 'error');
        return;
      }

      if (!allowedTypes.includes(file.type)) {
        showToast('Tipe file tidak didukung', 'error');
        return;
      }

      selectedFile = file;

      const reader = new FileReader();
      reader.onload = (e) => {
        selectedFileBase64 = e.target.result;
      };
      reader.readAsDataURL(file);

      document.getElementById('file-upload-placeholder').classList.add('hidden');
      document.getElementById('file-upload-success').classList.remove('hidden');
      document.getElementById('file-name-display').textContent = file.name;
      document.getElementById('file-size-display').textContent = (file.size / 1024).toFixed(2) + ' KB';
    }

    function clearFile() {
      selectedFile = null;
      selectedFileBase64 = null;
      document.getElementById('sr-file').value = '';
      document.getElementById('file-upload-placeholder').classList.remove('hidden');
      document.getElementById('file-upload-success').classList.add('hidden');
    }

    async function handleLogin(e) {
      e.preventDefault();
      const username = document.getElementById('login-username').value;
      const password = document.getElementById('login-password').value;

      let user = DEMO_USERS.find(u => u.username === username && u.password === password);

      if (!user) {
        const userData = allData.find(d => d.tipe === 'user' && d.username === username && d.password === password);
        if (userData) {
          user = {
            username: userData.username,
            password: userData.password,
            level_akses: userData.level_akses,
            nama_user: userData.nama_user,
            cabang_akses: userData.cabang_id
          };
        }
      }

      if (user) {
        currentUser = user;
        if (user.level_akses === 'user' && !currentCabang) {
          currentCabang = user.cabang_akses;
        }
        document.getElementById('login-screen').classList.add('hidden');
        document.getElementById('dashboard').classList.remove('hidden');
        updateUserInfo();
        updateUI();
        showToast('Selamat datang, ' + user.nama_user + '! 👋', 'success');
      } else {
        document.getElementById('login-error').textContent = 'Username atau password salah!';
        document.getElementById('login-error').classList.remove('hidden');
        setTimeout(() => document.getElementById('login-error').classList.add('hidden'), 3000);
      }
    }

    function handleLogout(e) {
      e.preventDefault();
      const confirmLogout = document.createElement('div');
      confirmLogout.className = 'fixed inset-0 bg-black/50 flex items-center justify-center z-50 p-4';
      confirmLogout.innerHTML = `
        <div class="bg-white rounded-xl p-6 max-w-sm w-full shadow-2xl">
          <h3 class="text-lg font-bold text-gray-800 mb-4">Konfirmasi Logout</h3>
          <p class="text-gray-600 mb-6">Yakin ingin keluar?</p>
          <div class="flex gap-3">
            <button type="button" onclick="this.parentElement.parentElement.remove()" class="flex-1 py-2 px-4 bg-gray-100 text-gray-700 rounded-lg font-semibold">Batal</button>
            <button type="button" onclick="performLogout()" class="flex-1 py-2 px-4 bg-red-500 text-white rounded-lg font-semibold">Keluar</button>
          </div>
        </div>
      `;
      document.body.appendChild(confirmLogout);
    }

    function performLogout() {
      currentUser = null;
      currentCabang = null;
      document.getElementById('dashboard').classList.add('hidden');
      document.getElementById('login-screen').classList.remove('hidden');
      document.getElementById('login-form').reset();
      document.getElementById('login-error').classList.add('hidden');
      document.querySelectorAll('[class*="flex items-center justify-center z-50"]').forEach(el => {
        if (el.querySelector('button[onclick="performLogout()"]')) el.remove();
      });
      showToast('Anda telah keluar 👋', 'success');
    }

    function isAdmin() {
      return currentUser && currentUser.level_akses === 'admin';
    }

    function updateUserInfo() {
      document.getElementById('user-name').textContent = currentUser.nama_user;
      document.getElementById('user-level').textContent = currentUser.level_akses === 'admin' ? '👑 Admin' : '👤 User';

      if (isAdmin()) {
        document.getElementById('add-cabang-btn').classList.remove('hidden');
        document.getElementById('penkerja-form-container').classList.remove('hidden');
        document.getElementById('jemaat-form-container').classList.remove('hidden');
        document.getElementById('kegiatan-form-container').classList.remove('hidden');
        document.getElementById('keuangan-form-container').classList.remove('hidden');
        document.getElementById('keuangan-keluar-container').classList.remove('hidden');
        document.getElementById('surat-form-container').classList.remove('hidden');
        document.getElementById('kehadiran-form-container').classList.remove('hidden');
        document.getElementById('kegiatan-aksi-header').classList.remove('hidden');
        document.getElementById('user-form-container').classList.remove('hidden');
        document.getElementById('users-tab').classList.remove('hidden');
      } else {
        document.getElementById('add-cabang-btn').classList.add('hidden');
        document.getElementById('penkerja-form-container').classList.add('hidden');
        document.getElementById('jemaat-form-container').classList.add('hidden');
        document.getElementById('kegiatan-form-container').classList.add('hidden');
        document.getElementById('keuangan-form-container').classList.add('hidden');
        document.getElementById('keuangan-keluar-container').classList.add('hidden');
        document.getElementById('surat-form-container').classList.add('hidden');
        document.getElementById('kehadiran-form-container').classList.add('hidden');
        document.getElementById('kegiatan-aksi-header').classList.add('hidden');
        document.getElementById('user-form-container').classList.add('hidden');
        document.getElementById('users-tab').classList.add('hidden');
      }
    }

    function updateUI() {
      updateCabangSelector();
      updateSearchCabangOptions();
      updateUserCabangOptions();
      updateStats();
      renderAllModules();
    }

    function updateCabangSelector() {
      const selector = document.getElementById('cabang-selector');
      selector.innerHTML = '';

      let cabangs = allData.filter(d => d.tipe === 'cabang');

      if (!isAdmin()) {
        cabangs = cabangs.filter(c => c.cabang_id === currentUser.cabang_akses);
      }

      if (cabangs.length === 0) {
        selector.innerHTML = '<span class="text-gray-500">Belum ada cabang</span>';
        return;
      }

      cabangs.forEach(cabang => {
        const btn = document.createElement('button');
        btn.type = 'button';
        btn.className = `px-4 py-2 rounded-lg font-semibold transition-all ${
          currentCabang === cabang.cabang_id
            ? 'btn-primary text-white'
            : 'bg-white text-gray-700 border border-gray-200'
        }`;
        btn.innerHTML = cabang.nama_cabang;
        btn.onclick = () => { currentCabang = cabang.cabang_id; updateUI(); };
        selector.appendChild(btn);
      });

      if (!currentCabang && cabangs.length > 0) {
        currentCabang = cabangs[0].cabang_id;
      }
    }

    function updateSearchCabangOptions() {
      const searchCabangSelect = document.getElementById('search-cabang');
      const currentValue = searchCabangSelect.value;

      searchCabangSelect.innerHTML = '<option value="">Semua Cabang</option>';

      let cabangs = allData.filter(d => d.tipe === 'cabang');

      if (!isAdmin()) {
        cabangs = cabangs.filter(c => c.cabang_id === currentUser.cabang_akses);
      }

      cabangs.forEach(cabang => {
        const option = document.createElement('option');
        option.value = cabang.cabang_id;
        option.textContent = cabang.nama_cabang;
        searchCabangSelect.appendChild(option);
      });

      searchCabangSelect.value = currentValue;
    }

    function updateUserCabangOptions() {
      const cabangSelect = document.getElementById('u-cabang');
      const currentValue = cabangSelect.value;

      cabangSelect.innerHTML = '<option value="">--Pilih Cabang--</option>';

      const cabangs = allData.filter(d => d.tipe === 'cabang');

      cabangs.forEach(cabang => {
        const option = document.createElement('option');
        option.value = cabang.cabang_id;
        option.textContent = cabang.nama_cabang;
        cabangSelect.appendChild(option);
      });

      cabangSelect.value = currentValue;
    }

    function updateStats() {
      if (!currentCabang) return;

      const cabangData = allData.filter(d => d.cabang_id === currentCabang);
      const penkerja = cabangData.filter(d => d.tipe === 'penkerja').length;
      const jemaat = cabangData.filter(d => d.tipe === 'jemaat').length;
      const totalKas = cabangData.filter(d => d.tipe === 'kas').reduce((sum, d) => sum + (d.dana_kas || 0), 0);
      const totalKeluar = cabangData.filter(d => d.tipe === 'keluar').reduce((sum, d) => sum + (d.dana_keluar || 0), 0);
      const netKas = totalKas - totalKeluar;
      const suratBulan = cabangData.filter(d => {
        if (d.tipe !== 'surat') return false;
        const bulan = new Date().getMonth();
        const tahun = new Date().getFullYear();
        const [y, m] = d.tanggal_surat ? d.tanggal_surat.split('-') : ['', ''];
        return parseInt(m) === bulan + 1 && parseInt(y) === tahun;
      }).length;

      document.getElementById('stat-pengerja').textContent = penkerja;
      document.getElementById('stat-jemaat').textContent = jemaat;
      document.getElementById('stat-dana').textContent = 'Rp ' + Math.max(0, netKas).toLocaleString('id-ID');
      document.getElementById('stat-surat').textContent = suratBulan;
    }

    function renderAllModules() {
      renderPenkerja();
      renderJemaat();
      renderKegiatan();
      renderKeuangan();
      renderSurat();
      renderKehadiran();
      renderBerandaChart();
      renderUsers();
      performSearch();
    }

    function performSearch() {
      const cabangFilter = document.getElementById('search-cabang').value;
      const keyword = document.getElementById('search-keyword').value.toLowerCase();
      const typeCheckboxes = document.querySelectorAll('input[name="search-type"]:checked');
      const selectedTypes = Array.from(typeCheckboxes).map(cb => cb.value);

      let results = allData;

      if (cabangFilter) {
        results = results.filter(d => d.cabang_id === cabangFilter);
      }

      if (selectedTypes.length > 0) {
        results = results.filter(d => selectedTypes.includes(d.tipe));
      }

      if (keyword) {
        results = results.filter(d => {
          const searchableText = [
            d.nama, d.nama_cabang, d.nomor_surat, d.pengirim,
            d.jenis_kegiatan, d.keterangan, d.jabatan, d.kategori,
            d.tipe_ibadah, d.tanggal_surat, d.tanggal_kegiatan
          ].filter(Boolean).join(' ').toLowerCase();

          return searchableText.includes(keyword);
        });
      }

      document.getElementById('search-result-count').textContent = results.length;
      renderSearchResults(results);
    }

    function renderSearchResults(results) {
      const container = document.getElementById('search-results');

      if (results.length === 0) {
        container.innerHTML = '<div class="text-center py-12 text-gray-500">Tidak ada hasil pencarian</div>';
        return;
      }

      const grouped = {};
      results.forEach(item => {
        if (!grouped[item.tipe]) grouped[item.tipe] = [];
        grouped[item.tipe].push(item);
      });

      const typeEmojis = {
        penkerja: '🙏',
        jemaat: '👥',
        kegiatan: '📅',
        surat: '📨',
        kehadiran: '✅',
        kas: '💰',
        keluar: '🚪'
      };

      let html = '';
      Object.entries(grouped).forEach(([type, items]) => {
        html += `<div class="bg-white rounded-xl p-5 shadow-md"><h4 class="font-bold text-[#7c2d12] mb-3 flex items-center gap-2"><span>${typeEmojis[type] || '📋'}</span> ${type.charAt(0).toUpperCase() + type.slice(1)} (${items.length})</h4>`;

        items.forEach(item => {
          let description = '';
          let cabangName = '';
          const cabang = allData.find(c => c.tipe === 'cabang' && c.cabang_id === item.cabang_id);
          if (cabang) cabangName = `📍 ${cabang.nama_cabang}`;

          if (type === 'penkerja') {
            description = `${item.nama} - ${item.jabatan}${item.telepon ? ` (📱 ${item.telepon})` : ''}`;
          } else if (type === 'jemaat') {
            description = `${item.nama} - ${item.kategori}${item.telepon ? ` (📱 ${item.telepon})` : ''}`;
          } else if (type === 'kegiatan') {
            description = `${item.jenis_kegiatan} (📅 ${new Date(item.tanggal_kegiatan).toLocaleDateString('id-ID')}) - Rp ${item.jumlah_dana?.toLocaleString('id-ID') || '0'}`;
          } else if (type === 'surat') {
            description = `${item.nomor_surat} - ${item.pengirim} (📅 ${new Date(item.tanggal_surat).toLocaleDateString('id-ID')})${item.file_name ? ` 📎 ${item.file_name}` : ''}`;
          } else if (type === 'kehadiran') {
            description = `${item.tipe_ibadah} (📅 ${new Date(item.tanggal_ibadah).toLocaleDateString('id-ID')}) - ${item.jumlah_hadir} orang${item.persembahan ? ` - Rp ${item.persembahan.toLocaleString('id-ID')}` : ''}`;
          } else if (type === 'kas') {
            description = `${item.keterangan || 'Dana Masuk'} - Rp ${item.dana_kas?.toLocaleString('id-ID') || '0'} (${new Date(item.tanggal).toLocaleDateString('id-ID')})`;
          } else if (type === 'keluar') {
            description = `${item.keterangan || 'Dana Keluar'} - Rp ${item.dana_keluar?.toLocaleString('id-ID') || '0'} (${new Date(item.tanggal).toLocaleDateString('id-ID')})`;
          }

          html += `
            <div class="bg-gray-50 rounded-lg p-3 mb-2 border-l-4 border-orange-400">
              <p class="text-sm font-semibold text-gray-800">${description}</p>
              ${cabangName ? `<p class="text-xs text-gray-500 mt-1">${cabangName}</p>` : ''}
            </div>
          `;
        });

        html += '</div>';
      });

      container.innerHTML = html;
    }

    function resetSearchForm() {
      document.getElementById('search-cabang').value = '';
      document.getElementById('search-keyword').value = '';
      document.querySelectorAll('input[name="search-type"]').forEach(cb => cb.checked = false);
      performSearch();
    }

    function renderBerandaChart() {
      if (!currentCabang) return;
      const kehadiran = allData.filter(d => d.tipe === 'kehadiran' && d.cabang_id === currentCabang);
      const today = new Date();
      const weeks = [];
      for (let i = 3; i >= 0; i--) {
        const d = new Date(today);
        d.setDate(d.getDate() - d.getDay() - (i * 7));
        weeks.push({
          start: new Date(d),
          end: new Date(d.getTime() + 6 * 24 * 60 * 60 * 1000),
          label: `Minggu ${4 - i}`
        });
      }

      let chartHtml = '<div class="flex items-end gap-2 h-24 justify-center">';
      weeks.forEach((week, idx) => {
        const count = kehadiran.filter(k => {
          const kDate = new Date(k.tanggal_ibadah);
          return kDate >= week.start && kDate <= week.end;
        }).reduce((sum, k) => sum + k.jumlah_hadir, 0);

        const height = Math.max(20, (count / 100) * 80);
        chartHtml += `<div class="flex flex-col items-center">
          <div style="height: ${height}px; width: 40px; background: linear-gradient(to top, #7c2d12, #b45309); border-radius: 4px; transition: all 0.2s;" title="${count} orang" class="hover:opacity-80"></div>
          <p class="text-xs mt-2 text-gray-600">W${4-idx}</p>
        </div>`;
      });
      chartHtml += '</div>';

      document.getElementById('attendance-summary').innerHTML = chartHtml || '<div class="text-center py-8 text-gray-500">Belum ada data kehadiran</div>';
    }

    async function handlePenkerjaSubmit(e) {
      e.preventDefault();
      if (!currentCabang) return showToast('Pilih cabang terlebih dahulu', 'error');

      const data = {
        tipe: 'penkerja',
        cabang_id: currentCabang,
        nama: document.getElementById('pk-nama').value,
        jabatan: document.getElementById('pk-jabatan').value,
        telepon: document.getElementById('pk-telepon').value,
        status: 'Aktif',
        tanggal_bergabung: new Date().toISOString()
      };

      showLoading(true);
      let result;
      if (currentEditingId) {
        const existing = allData.find(d => d.__backendId === currentEditingId);
        result = await window.dataSdk.update({ ...existing, ...data });
      } else {
        result = await window.dataSdk.create(data);
      }
      showLoading(false);

      if (result.isOk) {
        showToast('Penkerja berhasil disimpan!', 'success');
        resetPenkerjaForm();
      }
    }

    function renderPenkerja() {
      if (!currentCabang) return;

      let data = allData.filter(d => d.tipe === 'penkerja' && d.cabang_id === currentCabang);

      const total = data.length;
      const pendeta = data.filter(d => d.jabatan === 'Pendeta').length;
      const pendetaMuda = data.filter(d => d.jabatan === 'Pendeta Muda').length;
      const sintua = data.filter(d => d.jabatan === 'Sintua').length;
      const pembela = data.filter(d => d.jabatan === 'Pembela Sidang').length;

      document.getElementById('peng-total').textContent = total;
      document.getElementById('peng-pendeta').textContent = pendeta;
      document.getElementById('peng-pendeta-muda').textContent = pendetaMuda;
      document.getElementById('peng-sintua').textContent = sintua;
      document.getElementById('peng-pembela').textContent = pembela;

      const html = data.map(item => {
        let actionButtons = '';
        if (isAdmin()) {
          actionButtons = `
            <div class="flex gap-2">
              <button type="button" onclick="editPenkerja('${item.__backendId}')" class="p-2 text-blue-500 hover:bg-blue-50 rounded">✏️</button>
              <button type="button" onclick="showDeleteModal('${item.__backendId}', '${item.nama}', 'penkerja')" class="p-2 text-red-500 hover:bg-red-50 rounded">🗑️</button>
            </div>
          `;
        }
        return `
          <div class="bg-white rounded-xl p-4 shadow-md card-hover">
            <div class="flex items-center justify-between">
              <div>
                <h4 class="font-bold text-[#7c2d12]">${item.nama}</h4>
                <p class="text-sm text-gray-500">${item.jabatan}</p>
                ${item.telepon ? `<p class="text-sm text-gray-400">📱 ${item.telepon}</p>` : ''}
              </div>
              ${actionButtons}
            </div>
          </div>
        `;
      }).join('');

      document.getElementById('penkerja-list').innerHTML = html || '<div class="text-center py-8 text-gray-500">Belum ada data penkerja</div>';
    }

    function editPenkerja(id) {
      const item = allData.find(d => d.__backendId === id);
      if (!item) return;
      currentEditingId = id;
      currentEditingType = 'penkerja';
      document.getElementById('pk-nama').value = item.nama;
      document.getElementById('pk-jabatan').value = item.jabatan;
      document.getElementById('pk-telepon').value = item.telepon;
      document.getElementById('pk-submit-btn').innerHTML = '<span>💾</span> Perbarui';
    }

    function resetPenkerjaForm() {
      currentEditingId = null;
      currentEditingType = null;
      document.getElementById('penkerja-form').reset();
      document.getElementById('pk-submit-btn').innerHTML = '<span>💾</span> Simpan';
    }

    async function handleJemaatSubmit(e) {
      e.preventDefault();
      if (!currentCabang) return showToast('Pilih cabang terlebih dahulu', 'error');

      const data = {
        tipe: 'jemaat',
        cabang_id: currentCabang,
        nama: document.getElementById('jm-nama').value,
        telepon: document.getElementById('jm-telepon').value,
        kategori: document.getElementById('jm-kategori').value,
        status: 'Aktif',
        tanggal_bergabung: new Date().toISOString()
      };

      showLoading(true);
      let result;
      if (currentEditingId) {
        const existing = allData.find(d => d.__backendId === currentEditingId);
        result = await window.dataSdk.update({ ...existing, ...data });
      } else {
        result = await window.dataSdk.create(data);
      }
      showLoading(false);

      if (result.isOk) {
        showToast('Jemaat berhasil disimpan!', 'success');
        resetJemaatForm();
      }
    }

    function renderJemaat() {
      if (!currentCabang) return;

      let data = allData.filter(d => d.tipe === 'jemaat' && d.cabang_id === currentCabang);

      const total = data.length;
      const ibu = data.filter(d => d.kategori === 'Kaum Ibu').length;
      const bapak = data.filter(d => d.kategori === 'Kaum Bapak').length;
      const anak = data.filter(d => d.kategori === 'Anak-anak').length;

      document.getElementById('jem-total').textContent = total;
      document.getElementById('jem-ibu').textContent = ibu;
      document.getElementById('jem-bapak').textContent = bapak;
      document.getElementById('jem-anak').textContent = anak;

      const html = data.map(item => {
        let actionButtons = '';
        if (isAdmin()) {
          actionButtons = `
            <div class="flex gap-2">
              <button type="button" onclick="editJemaat('${item.__backendId}')" class="p-2 text-blue-500 hover:bg-blue-50 rounded">✏️</button>
              <button type="button" onclick="showDeleteModal('${item.__backendId}', '${item.nama}', 'jemaat')" class="p-2 text-red-500 hover:bg-red-50 rounded">🗑️</button>
            </div>
          `;
        }
        return `
          <div class="bg-white rounded-xl p-4 shadow-md card-hover">
            <div class="flex items-center justify-between">
              <div>
                <h4 class="font-bold text-[#7c2d12]">${item.nama}</h4>
                <span class="bg-blue-100 text-blue-700 px-2 py-0.5 rounded text-xs">📂 ${item.kategori}</span>
                ${item.telepon ? `<p class="text-sm text-gray-400 mt-1">📱 ${item.telepon}</p>` : ''}
              </div>
              ${actionButtons}
            </div>
          </div>
        `;
      }).join('');

      document.getElementById('jemaat-list').innerHTML = html || '<div class="text-center py-8 text-gray-500">Belum ada data jemaat</div>';
    }

    function editJemaat(id) {
      const item = allData.find(d => d.__backendId === id);
      if (!item) return;
      currentEditingId = id;
      currentEditingType = 'jemaat';
      document.getElementById('jm-nama').value = item.nama;
      document.getElementById('jm-telepon').value = item.telepon;
      document.getElementById('jm-kategori').value = item.kategori;
      document.getElementById('jm-submit-btn').innerHTML = '<span>💾</span> Perbarui';
    }

    function resetJemaatForm() {
      currentEditingId = null;
      currentEditingType = null;
      document.getElementById('jemaat-form').reset();
      document.getElementById('jm-submit-btn').innerHTML = '<span>💾</span> Simpan';
    }

    async function handleKegiatanSubmit(e) {
      e.preventDefault();
      if (!currentCabang) return showToast('Pilih cabang terlebih dahulu', 'error');

      const data = {
        tipe: 'kegiatan',
        cabang_id: currentCabang,
        jenis_kegiatan: document.getElementById('kg-jenis').value,
        tanggal_kegiatan: document.getElementById('kg-tanggal').value,
        jumlah_dana: parseInt(document.getElementById('kg-dana').value) || 0,
        keterangan: 'Kegiatan'
      };

      showLoading(true);
      let result;
      if (currentEditingId) {
        const existing = allData.find(d => d.__backendId === currentEditingId);
        result = await window.dataSdk.update({ ...existing, ...data });
      } else {
        result = await window.dataSdk.create(data);
      }
      showLoading(false);

      if (result.isOk) {
        showToast('Kegiatan berhasil disimpan!', 'success');
        resetKegiatanForm();
      }
    }

    function renderKegiatan() {
      if (!currentCabang) return;

      let data = allData.filter(d => d.tipe === 'kegiatan' && d.cabang_id === currentCabang)
        .sort((a, b) => new Date(b.tanggal_kegiatan) - new Date(a.tanggal_kegiatan));

      const tbody = document.getElementById('kegiatan-table');
      tbody.innerHTML = data.map(item => {
        let aksi = '';
        if (isAdmin()) {
          aksi = `
            <td class="px-4 py-2 text-center">
              <button type="button" onclick="editKegiatan('${item.__backendId}')" class="text-blue-500 hover:underline">✏️</button>
              <button type="button" onclick="showDeleteModal('${item.__backendId}', '${item.jenis_kegiatan}', 'kegiatan')" class="text-red-500 hover:underline ml-2">🗑️</button>
            </td>
          `;
        }
        return `
          <tr>
            <td class="px-4 py-2">${new Date(item.tanggal_kegiatan).toLocaleDateString('id-ID')}</td>
            <td class="px-4 py-2">${item.jenis_kegiatan}</td>
            <td class="px-4 py-2 text-right font-semibold">Rp ${item.jumlah_dana?.toLocaleString('id-ID') || '0'}</td>
            ${aksi}
          </tr>
        `;
      }).join('') || '<tr><td colspan="4" class="px-4 py-8 text-center text-gray-500">Belum ada data kegiatan</td></tr>';
    }

    function editKegiatan(id) {
      const item = allData.find(d => d.__backendId === id);
      if (!item) return;
      currentEditingId = id;
      currentEditingType = 'kegiatan';
      document.getElementById('kg-jenis').value = item.jenis_kegiatan;
      document.getElementById('kg-tanggal').value = item.tanggal_kegiatan;
      document.getElementById('kg-dana').value = item.jumlah_dana;
      document.getElementById('kg-submit-btn').innerHTML = '<span>💾</span> Perbarui';
    }

    function resetKegiatanForm() {
      currentEditingId = null;
      currentEditingType = null;
      document.getElementById('kegiatan-form').reset();
      document.getElementById('kg-submit-btn').innerHTML = '<span>💾</span> Simpan';
    }

    async function handleKasSubmit(e) {
      e.preventDefault();
      if (!currentCabang) return showToast('Pilih cabang terlebih dahulu', 'error');

      const data = {
        tipe: 'kas',
        cabang_id: currentCabang,
        dana_kas: parseInt(document.getElementById('kas-jumlah').value) || 0,
        keterangan: document.getElementById('kas-ket').value,
        tanggal: new Date().toISOString()
      };

      showLoading(true);
      const result = await window.dataSdk.create(data);
      showLoading(false);

      if (result.isOk) {
        showToast('Dana kas berhasil disimpan!', 'success');
        document.getElementById('kas-form').reset();
      }
    }

    async function handleKeluarSubmit(e) {
      e.preventDefault();
      if (!currentCabang) return showToast('Pilih cabang terlebih dahulu', 'error');

      const data = {
        tipe: 'keluar',
        cabang_id: currentCabang,
        dana_keluar: parseInt(document.getElementById('keluar-jumlah').value) || 0,
        keterangan: document.getElementById('keluar-ket').value,
        tanggal: new Date().toISOString()
      };

      showLoading(true);
      const result = await window.dataSdk.create(data);
      showLoading(false);

      if (result.isOk) {
        showToast('Dana keluar berhasil disimpan!', 'success');
        document.getElementById('keluar-form').reset();
      }
    }

    function renderKeuangan() {
      if (!currentCabang) return;

      const kas = allData.filter(d => d.tipe === 'kas' && d.cabang_id === currentCabang);
      const keluar = allData.filter(d => d.tipe === 'keluar' && d.cabang_id === currentCabang);
      const kehadiran = allData.filter(d => d.tipe === 'kehadiran' && d.cabang_id === currentCabang);

      const totalKas = kas.reduce((sum, d) => sum + (d.dana_kas || 0), 0);
      const totalKeluar = keluar.reduce((sum, d) => sum + (d.dana_keluar || 0), 0);
      const netKas = totalKas - totalKeluar;

      const totalMinggu = kehadiran.filter(k => {
        const d = new Date();
        return k.tipe_ibadah === 'Ibadah Raya' && k.tanggal_ibadah >= new Date(d.getTime() - 7*24*60*60*1000).toISOString();
      }).reduce((sum, k) => sum + (k.persembahan || 0), 0);
      const totalTengah = kehadiran.filter(k => {
        const d = new Date();
        return k.tipe_ibadah === 'Ibadah Tengah Minggu' && k.tanggal_ibadah >= new Date(d.getTime() - 7*24*60*60*1000).toISOString();
      }).reduce((sum, k) => sum + (k.persembahan || 0), 0);

      document.getElementById('keu-total-kas').textContent = 'Rp ' + Math.max(0, netKas).toLocaleString('id-ID');
      document.getElementById('keu-minggu').textContent = 'Rp ' + totalMinggu.toLocaleString('id-ID');
      document.getElementById('keu-tengah').textContent = 'Rp ' + totalTengah.toLocaleString('id-ID');

      const minggu = {};
      kehadiran.forEach(k => {
        if (!k.tipe_ibadah || !k.tanggal_ibadah) return;
        const date = new Date(k.tanggal_ibadah);
        const weekNum = Math.ceil((date.getDate() - date.getDay() + 1) / 7);
        const key = `${date.getFullYear()}-${date.getMonth()}-W${weekNum}`;
        minggu[key] = (minggu[key] || 0) + (k.persembahan || 0);
      });

      const html = Object.entries(minggu).sort().slice(-4).map(([key, val]) => `
        <div class="flex justify-between items-center p-2 bg-gray-50 rounded">
          <span class="text-sm text-gray-600">${key}</span>
          <span class="font-bold text-[#7c2d12]">Rp ${val.toLocaleString('id-ID')}</span>
        </div>
      `).join('');

      document.getElementById('persembahan-mingguan').innerHTML = html || '<div class="text-center py-4 text-gray-500 text-sm">Belum ada data</div>';

      const keluarHtml = keluar.map(item => `
        <div class="bg-white rounded-lg p-3 shadow-sm flex justify-between items-center">
          <div>
            <p class="font-semibold text-gray-700">${item.keterangan || 'Dana Keluar'}</p>
            <p class="text-xs text-gray-500">${new Date(item.tanggal).toLocaleDateString('id-ID')}</p>
          </div>
          <p class="text-lg font-bold text-red-500">-Rp ${item.dana_keluar.toLocaleString('id-ID')}</p>
        </div>
      `).join('');

      document.getElementById('dana-keluar-list').innerHTML = keluarHtml || '<div class="text-center py-4 text-gray-500 text-sm">Belum ada data dana keluar</div>';
    }

    async function handleSuratSubmit(e) {
      e.preventDefault();
      if (!currentCabang) return showToast('Pilih cabang terlebih dahulu', 'error');

      const data = {
        tipe: 'surat',
        cabang_id: currentCabang,
        nomor_surat: document.getElementById('sr-nomor').value,
        tanggal_surat: document.getElementById('sr-tanggal').value,
        pengirim: document.getElementById('sr-pengirim').value,
        file_url: selectedFileBase64 || '',
        file_name: selectedFile ? selectedFile.name : '',
        file_size: selectedFile ? selectedFile.size : 0,
        file_type: selectedFile ? selectedFile.type : '',
        keterangan: 'Surat'
      };

      showLoading(true);
      let result;
      if (currentEditingId) {
        const existing = allData.find(d => d.__backendId === currentEditingId);
        result = await window.dataSdk.update({ ...existing, ...data });
      } else {
        result = await window.dataSdk.create(data);
      }
      showLoading(false);

      if (result.isOk) {
        showToast('Surat berhasil disimpan!', 'success');
        resetSuratForm();
      }
    }

    function renderSurat() {
      if (!currentCabang) return;

      let data = allData.filter(d => d.tipe === 'surat' && d.cabang_id === currentCabang)
        .sort((a, b) => new Date(b.tanggal_surat) - new Date(a.tanggal_surat));

      const html = data.map(item => {
        let actionButtons = '';
        let fileIcon = '';
        if (item.file_url) {
          if (item.file_type === 'application/pdf') {
            fileIcon = '📄 PDF';
          } else if (item.file_type && item.file_type.startsWith('image/')) {
            fileIcon = '🖼️ Foto';
          } else {
            fileIcon = '📎 File';
          }
        }

        if (isAdmin()) {
          actionButtons = `
            <div class="flex gap-2">
              <button type="button" onclick="editSurat('${item.__backendId}')" class="p-2 text-blue-500 hover:bg-blue-50 rounded">✏️</button>
              <button type="button" onclick="showDeleteModal('${item.__backendId}', '${item.nomor_surat}', 'surat')" class="p-2 text-red-500 hover:bg-red-50 rounded">🗑️</button>
            </div>
          `;
        }
        return `
          <div class="bg-white rounded-xl p-4 shadow-md card-hover">
            <div class="flex items-center justify-between">
              <div class="flex-1">
                <p class="font-bold text-[#7c2d12]">${item.nomor_surat}</p>
                <p class="text-sm text-gray-600"><strong>Pengirim/Tujuan:</strong> ${item.pengirim}</p>
                <p class="text-sm text-gray-400">📅 ${new Date(item.tanggal_surat).toLocaleDateString('id-ID')}</p>
                ${fileIcon ? `<div class="flex items-center gap-2 mt-2"><span class="text-xs bg-green-100 text-green-700 px-2 py-1 rounded">${fileIcon}</span><span class="text-xs text-gray-500">${item.file_name || 'File'}</span></div>` : ''}
              </div>
              ${actionButtons}
            </div>
          </div>
        `;
      }).join('');

      document.getElementById('surat-list').innerHTML = html || '<div class="text-center py-8 text-gray-500">Belum ada data surat</div>';
    }

    function editSurat(id) {
      const item = allData.find(d => d.__backendId === id);
      if (!item) return;
      currentEditingId = id;
      currentEditingType = 'surat';
      document.getElementById('sr-nomor').value = item.nomor_surat;
      document.getElementById('sr-tanggal').value = item.tanggal_surat;
      document.getElementById('sr-pengirim').value = item.pengirim;

      if (item.file_url) {
        selectedFileBase64 = item.file_url;
        document.getElementById('file-upload-placeholder').classList.add('hidden');
        document.getElementById('file-upload-success').classList.remove('hidden');
        document.getElementById('file-name-display').textContent = item.file_name || 'File tersimpan';
        document.getElementById('file-size-display').textContent = item.file_size ? (item.file_size / 1024).toFixed(2) + ' KB' : '';
      }

      document.getElementById('sr-submit-btn').innerHTML = '<span>💾</span> Perbarui';
    }

    function resetSuratForm() {
      currentEditingId = null;
      currentEditingType = null;
      document.getElementById('surat-form').reset();
      clearFile();
      document.getElementById('sr-submit-btn').innerHTML = '<span>💾</span> Simpan';
    }

    async function handleKehadiranSubmit(e) {
      e.preventDefault();
      if (!currentCabang) return showToast('Pilih cabang terlebih dahulu', 'error');

      const data = {
        tipe: 'kehadiran',
        cabang_id: currentCabang,
        tipe_ibadah: document.getElementById('kh-jenis').value,
        tanggal_ibadah: document.getElementById('kh-tanggal').value,
        jumlah_hadir: parseInt(document.getElementById('kh-jumlah').value) || 0,
        persembahan: parseInt(document.getElementById('kh-persembahan').value) || 0
      };

      showLoading(true);
      let result;
      if (currentEditingId) {
        const existing = allData.find(d => d.__backendId === currentEditingId);
        result = await window.dataSdk.update({ ...existing, ...data });
      } else {
        result = await window.dataSdk.create(data);
      }
      showLoading(false);

      if (result.isOk) {
        showToast('Kehadiran berhasil disimpan!', 'success');
        resetKehadiranForm();
      }
    }

    function renderKehadiran() {
      if (!currentCabang) return;

      const data = allData.filter(d =>
        d.tipe === 'kehadiran' &&
        d.cabang_id === currentCabang
      ).sort((a, b) => new Date(b.tanggal_ibadah) - new Date(a.tanggal_ibadah));

      const ibadiTypes = {
        'Ibadah Raya': { emoji: '⛪', color: 'bg-blue-50', textColor: 'text-blue-700' },
        'Ibadah Tengah Minggu': { emoji: '🙏', color: 'bg-purple-50', textColor: 'text-purple-700' },
        'Pengerja Doa Pra Ibadah': { emoji: '📿', color: 'bg-indigo-50', textColor: 'text-indigo-700' },
        'Ibadah Kaum Ibu': { emoji: '👩‍🤝‍👩', color: 'bg-pink-50', textColor: 'text-pink-700' },
        'Ibadah Kaum Bapak': { emoji: '👨‍🤝‍👨', color: 'bg-green-50', textColor: 'text-green-700' },
        'Ibadah Kaum Pemuda': { emoji: '🎒', color: 'bg-yellow-50', textColor: 'text-yellow-700' },
        'Ibadah Kaum Remaja': { emoji: '🎓', color: 'bg-red-50', textColor: 'text-red-700' }
      };

      const summary = {};
      data.forEach(item => {
        if (!summary[item.tipe_ibadah]) {
          summary[item.tipe_ibadah] = {
            count: 0,
            totalHadir: 0,
            totalPersembahan: 0,
            lastDate: null
          };
        }
        summary[item.tipe_ibadah].count++;
        summary[item.tipe_ibadah].totalHadir += item.jumlah_hadir || 0;
        summary[item.tipe_ibadah].totalPersembahan += item.persembahan || 0;
        if (!summary[item.tipe_ibadah].lastDate || new Date(item.tanggal_ibadah) > new Date(summary[item.tipe_ibadah].lastDate)) {
          summary[item.tipe_ibadah].lastDate = item.tanggal_ibadah;
        }
      });

      const summaryHtml = Object.entries(summary).map(([ibadiType, stats]) => {
        const typeInfo = ibadiTypes[ibadiType] || { emoji: '✨', color: 'bg-gray-50', textColor: 'text-gray-700' };
        const avgHadir = stats.count > 0 ? Math.round(stats.totalHadir / stats.count) : 0;
        return `
          <div class="${typeInfo.color} rounded-xl p-4 shadow-sm border-l-4 ${typeInfo.textColor.replace('text-', 'border-')}">
            <div class="flex items-start justify-between">
              <div class="flex items-start gap-3">
                <span class="text-3xl">${typeInfo.emoji}</span>
                <div>
                  <p class="font-bold text-gray-800">${ibadiType}</p>
                  <p class="text-xs text-gray-600 mt-1">📅 Terakhir: ${new Date(stats.lastDate).toLocaleDateString('id-ID')}</p>
                </div>
              </div>
              <div class="text-right">
                <p class="text-sm text-gray-600">Frekuensi</p>
                <p class="text-2xl font-bold text-gray-800">${stats.count}x</p>
              </div>
            </div>
            <div class="grid grid-cols-2 gap-3 mt-4 pt-4 border-t border-gray-200">
              <div>
                <p class="text-xs text-gray-600">Rata-rata Hadir</p>
                <p class="text-lg font-bold text-gray-800">${avgHadir} orang</p>
              </div>
              <div>
                <p class="text-xs text-gray-600">Total Persembahan</p>
                <p class="text-lg font-bold ${typeInfo.textColor}">Rp ${stats.totalPersembahan.toLocaleString('id-ID')}</p>
              </div>
            </div>
          </div>
        `;
      }).join('');

      const listHtml = data.map(item => {
        let actionButtons = '';
        if (isAdmin()) {
          actionButtons = `
            <button type="button" onclick="editKehadiran('${item.__backendId}')" class="p-2 text-blue-500 hover:bg-blue-50 rounded">✏️</button>
            <button type="button" onclick="showDeleteModal('${item.__backendId}', '${item.tipe_ibadah}', 'kehadiran')" class="p-2 text-red-500 hover:bg-red-50 rounded">🗑️</button>
          `;
        }
        return `
          <div class="bg-white rounded-xl p-4 shadow-sm">
            <div class="flex justify-between items-start">
              <div>
                <p class="font-semibold text-gray-800">${item.tipe_ibadah}</p>
                <p class="text-xs text-gray-500">📅 ${new Date(item.tanggal_ibadah).toLocaleDateString('id-ID')}</p>
              </div>
              <div class="flex items-center gap-4">
                <div class="text-right">
                  <p class="font-bold text-[#7c2d12]">${item.jumlah_hadir} orang</p>
                  ${item.persembahan ? `<p class="text-sm text-gray-600">Rp ${item.persembahan.toLocaleString('id-ID')}</p>` : ''}
                </div>
                <div class="flex gap-1">
                  ${actionButtons}
                </div>
              </div>
            </div>
          </div>
        `;
      }).join('');

      const container = document.getElementById('kehadiran-list');
      if (Object.keys(summary).length > 0) {
        container.innerHTML = `
          <div class="mb-6">
            <h3 class="font-bold text-[#7c2d12] mb-4">📊 Resume Kehadiran per Jenis Ibadah</h3>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
              ${summaryHtml}
            </div>
          </div>
          <div>
            <h3 class="font-bold text-[#7c2d12] mb-4">📋 Detail Semua Kehadiran</h3>
            <div class="space-y-3">
              ${listHtml}
            </div>
          </div>
        `;
      } else {
        container.innerHTML = '<div class="text-center py-8 text-gray-500">Belum ada data kehadiran</div>';
      }
    }

    function editKehadiran(id) {
      const item = allData.find(d => d.__backendId === id);
      if (!item) return;
      currentEditingId = id;
      currentEditingType = 'kehadiran';
      document.getElementById('kh-jenis').value = item.tipe_ibadah;
      document.getElementById('kh-tanggal').value = item.tanggal_ibadah;
      document.getElementById('kh-jumlah').value = item.jumlah_hadir;
      document.getElementById('kh-persembahan').value = item.persembahan || 0;
      document.getElementById('kh-submit-btn').innerHTML = '<span>💾</span> Perbarui';
    }

    function resetKehadiranForm() {
      currentEditingId = null;
      currentEditingType = null;
      document.getElementById('kehadiran-form').reset();
      document.getElementById('kh-submit-btn').innerHTML = '<span>💾</span> Simpan';
    }

    async function handleUserSubmit(e) {
      e.preventDefault();
      const cabangId = document.getElementById('u-level').value === 'admin' ? null : document.getElementById('u-cabang').value;

      if (document.getElementById('u-level').value === 'user' && !cabangId) {
        showToast('Pilih cabang untuk user!', 'error');
        return;
      }

      const data = {
        tipe: 'user',
        cabang_id: cabangId || '',
        nama_user: document.getElementById('u-nama').value,
        username: document.getElementById('u-username').value,
        password: document.getElementById('u-password').value,
        level_akses: document.getElementById('u-level').value
      };

      showLoading(true);
      let result;
      if (currentEditingId) {
        const existing = allData.find(d => d.__backendId === currentEditingId);
        result = await window.dataSdk.update({ ...existing, ...data });
      } else {
        result = await window.dataSdk.create(data);
      }
      showLoading(false);

      if (result.isOk) {
        showToast('User berhasil disimpan!', 'success');
        resetUserForm();
        toggleUserFormVisibility();
      }
    }

    function renderUsers() {
      const users = allData.filter(d => d.tipe === 'user');

      const tbody = document.getElementById('users-table');
      tbody.innerHTML = users.map(user => {
        const cabang = user.cabang_id ? allData.find(c => c.tipe === 'cabang' && c.cabang_id === user.cabang_id) : null;
        const cabangName = cabang ? cabang.nama_cabang : 'Admin (Semua)';
        const levelBadge = user.level_akses === 'admin' ? '👑 Admin' : '👤 User';

        return `
          <tr>
            <td class="px-4 py-3 font-semibold text-gray-800">${user.nama_user}</td>
            <td class="px-4 py-3 text-gray-600">${user.username}</td>
            <td class="px-4 py-3"><span class="bg-${user.level_akses === 'admin' ? 'purple' : 'blue'}-100 text-${user.level_akses === 'admin' ? 'purple' : 'blue'}-700 px-3 py-1 rounded-full text-xs font-semibold">${levelBadge}</span></td>
            <td class="px-4 py-3 text-gray-600">${cabangName}</td>
            <td class="px-4 py-3 text-center">
              <button type="button" onclick="editUser('${user.__backendId}')" class="text-blue-500 hover:text-blue-700 mr-3">✏️</button>
              <button type="button" onclick="showDeleteModal('${user.__backendId}', '${user.nama_user}', 'user')" class="text-red-500 hover:text-red-700">🗑️</button>
            </td>
          </tr>
        `;
      }).join('') || '<tr><td colspan="5" class="px-4 py-8 text-center text-gray-500">Belum ada user tambahan</td></tr>';
    }

    function editUser(id) {
      const item = allData.find(d => d.__backendId === id);
      if (!item) return;
      currentEditingId = id;
      currentEditingType = 'user';
      document.getElementById('u-nama').value = item.nama_user;
      document.getElementById('u-username').value = item.username;
      document.getElementById('u-password').value = item.password;
      document.getElementById('u-level').value = item.level_akses;
      document.getElementById('u-cabang').value = item.cabang_id || '';
      document.getElementById('u-cabang').disabled = item.level_akses === 'admin';
      document.getElementById('u-submit-btn').innerHTML = '<span>💾</span> Perbarui User';
      document.getElementById('user-form-container').classList.remove('hidden');
    }

    function resetUserForm() {
      currentEditingId = null;
      currentEditingType = null;
      document.getElementById('user-form').reset();
      document.getElementById('u-cabang').disabled = false;
      document.getElementById('u-submit-btn').innerHTML = '<span>💾</span> Simpan User';
    }

    function toggleUserFormVisibility() {
      const container = document.getElementById('user-form-container');
      if (container.classList.contains('hidden')) {
        container.classList.remove('hidden');
        resetUserForm();
      } else {
        container.classList.add('hidden');
      }
    }

    async function handleAddCabang(e) {
      e.preventDefault();
      const cabangId = 'cab_' + Date.now();
      const data = {
        tipe: 'cabang',
        cabang_id: cabangId,
        nama_cabang: document.getElementById('cabang-nama').value
      };

      showLoading(true);
      const result = await window.dataSdk.create(data);
      showLoading(false);

      if (result.isOk) {
        showToast('Cabang berhasil ditambah!', 'success');
        closeCabangModal();
      }
    }

    function showAddCabangModal() {
      document.getElementById('cabang-modal').classList.remove('hidden');
    }

    function closeCabangModal() {
      document.getElementById('cabang-modal').classList.add('hidden');
      document.getElementById('cabang-form').reset();
    }

    function switchModule(module) {
      currentModule = module;
      document.querySelectorAll('.module-content').forEach(el => el.classList.add('hidden'));
      document.getElementById(module + '-module').classList.remove('hidden');
      document.querySelectorAll('#main-nav button').forEach(btn => {
        btn.className = btn.dataset.module === module
          ? 'nav-active text-white'
          : 'bg-gray-100 text-gray-600';
      });
    }

    function showDeleteModal(id, name, type) {
      currentDeleteTarget = id;
      currentEditingType = type;
      document.getElementById('delete-name').textContent = `Hapus "${name}"?`;
      document.getElementById('delete-modal').classList.remove('hidden');
    }

    function cancelDelete() {
      document.getElementById('delete-modal').classList.add('hidden');
    }

    async function confirmDelete() {
      if (!currentDeleteTarget) return;
      const item = allData.find(d => d.__backendId === currentDeleteTarget);
      if (!item) return;

      showLoading(true);
      const result = await window.dataSdk.delete(item);
      showLoading(false);

      if (result.isOk) {
        showToast('Data berhasil dihapus!', 'success');
        cancelDelete();
      }
    }

    function showLoading(show) {
      document.getElementById('loading-overlay').className = show
        ? 'fixed inset-0 bg-white/80 flex items-center justify-center z-50'
        : 'hidden';
    }

    function showToast(message, type) {
      const toast = document.createElement('div');
      toast.className = `fixed bottom-4 right-4 px-6 py-3 rounded-xl shadow-lg z-50 ${
        type === 'success' ? 'bg-green-500' : 'bg-red-500'
      } text-white fade-in`;
      toast.innerHTML = `${type === 'success' ? '✅' : '❌'} ${message}`;
      document.body.appendChild(toast);
      setTimeout(() => toast.remove(), 3000);
    }
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9ceda93cf2baf92a',t:'MTc3MTI1MTM2OC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
