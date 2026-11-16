<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>E-Voting Pilkades Desa Sukatani 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
    <style>
        body {
            background: linear-gradient(180deg, #dc2626 0%, #dc2626 42%, #ffffff 42%, #f3f4f6 100%);
            min-height: 100dvh;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation;
        }
        .fade-in { animation: fadeIn 0.25s ease-out; }
        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.98); }
            to { opacity: 1; transform: scale(1); }
        }
        #reader { width: 100% !important; border: none !important; }
        #reader video { width: 100% !important; height: 240px !important; border-radius: 0.75rem; object-fit: cover; }
    </style>
</head>
<body class="flex flex-col items-center justify-between p-2.5 sm:p-4 md:p-6">

    <!-- ================= HEADER RESMI ================= -->
    <header class="relative w-full max-w-6xl bg-white/95 backdrop-blur shadow-xl rounded-2xl p-2.5 sm:p-4 border-t-8 border-red-600 flex items-center justify-between gap-3">
        <!-- Tombol Pengaturan Panitia (Pojok Kanan Atas) -->
        <button onclick="bukaPengaturan()" title="Atur Foto & Nama Calon" class="absolute -top-3 right-3 bg-gray-800 hover:bg-black text-white text-[11px] font-bold px-3 py-1 rounded-full shadow-md flex items-center gap-1 z-30">
            ⚙️ Atur Foto/Calon
        </button>

        <!-- Logo Samping Kiri -->
        <div class="flex-shrink-0">
            <img id="img-logo-kiri" src="" alt="Logo Subang" class="w-12 h-12 sm:w-16 sm:h-16 md:w-20 md:h-20 object-contain rounded-lg">
        </div>

        <!-- Judul Surat Suara Besar -->
        <div class="text-center flex-grow">
            <h1 class="text-base sm:text-2xl md:text-3xl font-extrabold text-gray-900 tracking-wide leading-tight uppercase">
                SURAT SUARA <span class="text-red-600">PEMILIHAN KEPALA DESA</span><br>
                DESA SUKATANI KECAMATAN COMPRENG TAHUN 2026
            </h1>
        </div>

        <!-- Logo Samping Kanan -->
        <div class="flex-shrink-0">
            <img id="img-logo-kanan" src="" alt="Logo Sukatani" class="w-12 h-12 sm:w-16 sm:h-16 md:w-20 md:h-20 object-contain rounded-lg">
        </div>
    </header>

    <!-- ================= KONTEN UTAMA ================= -->
    <main class="w-full max-w-6xl my-auto py-2 sm:py-4 flex-grow flex flex-col justify-center">

        <!-- TAMPILAN 1: LANGSUNG KAMERA SCANNER QR CODE -->
        <section id="view-login" class="fade-in w-full max-w-md mx-auto bg-white rounded-2xl shadow-2xl p-5 sm:p-6 text-center border border-gray-200">
            <div class="inline-flex items-center gap-2 bg-red-100 text-red-700 text-xs font-bold px-3.5 py-1 rounded-full mb-2">
                <span class="w-2.5 h-2.5 bg-red-600 rounded-full animate-ping"></span>
                <span id="badge-cam-text">KAMERA SCANNER AKTIF</span>
            </div>
            <h2 class="text-xl sm:text-2xl font-extrabold text-gray-800 mb-1">Scan Barcode / QR Code</h2>
            <p class="text-gray-600 text-xs sm:text-sm mb-3">Arahkan QR Code ke kamera untuk membuka Surat Suara.</p>
            
            <!-- Kotak Kamera Live Langsung Menyala -->
            <div class="relative mx-auto w-full max-w-xs bg-gray-900 border-4 border-red-600 rounded-2xl overflow-hidden shadow-inner min-h-[240px] flex flex-col items-center justify-center">
                <div id="reader" class="w-full"></div>
                <p id="camera-status" class="text-white text-xs p-3 leading-relaxed">Menyalakan kamera...</p>
                
                <button id="btn-start-cam" onclick="mulaiKameraOtomatis()" class="hidden mb-3 bg-green-600 hover:bg-green-700 text-white text-xs font-bold py-2.5 px-4 rounded-xl shadow">
                    📷 Izinkan & Nyalakan Kamera
                </button>
            </div>

            <!-- Tombol Putar Kamera (Depan/Belakang) -->
            <div class="mt-3 flex gap-2 justify-center">
                <button id="btn-switch-cam" onclick="gantiKamera()" class="hidden flex-1 bg-gray-100 hover:bg-gray-200 text-gray-800 text-xs font-bold py-2.5 px-3 rounded-xl transition">
                    🔄 Putar Kamera
                </button>
            </div>
        </section>

        <!-- TAMPILAN 2: SURAT SUARA (4 KANDIDAT LANDSCAPE SEJAJAR) -->
        <section id="view-ballot" class="hidden fade-in w-full bg-white rounded-2xl shadow-2xl p-3 sm:p-5 border border-gray-200">
            <div class="text-center mb-3">
                <span class="bg-red-100 text-red-700 text-xs sm:text-sm font-bold px-4 py-1 rounded-full">
                    Silakan tekan foto kandidat pilihan Anda
                </span>
            </div>

            <!-- Grid 4 Kolom Landscape (Selalu 4 kolom berjajar ke samping) -->
            <div id="candidates-container" class="grid grid-cols-4 gap-2 sm:gap-4 md:gap-5">
                <!-- Kartu kandidat dibuat otomatis oleh JavaScript -->
            </div>

            <!-- Tombol Biru "PILIH KANDIDAT INI" di Bawah Ke-4 Kandidat -->
            <div class="mt-4 sm:mt-6 text-center">
                <button id="btn-submit-choice" 
                        disabled
                        onclick="bukaKonfirmasi()"
                        class="bg-blue-600 hover:bg-blue-700 disabled:bg-gray-300 disabled:cursor-not-allowed text-white text-base sm:text-xl font-extrabold py-3 sm:py-3.5 px-10 sm:px-14 rounded-xl shadow-lg transition transform active:scale-95">
                    PILIH KANDIDAT INI
                </button>
                <p id="helper-text" class="text-xs text-gray-500 mt-1.5">
                    *Tekan salah satu foto kandidat di atas.
                </p>
            </div>
        </section>

        <!-- TAMPILAN 4: SELESAI (SUARA TERSIMPAN & OTOMATIS KEMBALI KE KAMERA) -->
        <section id="view-finish" class="hidden fade-in max-w-md mx-auto bg-white rounded-2xl shadow-2xl p-8 text-center border-t-8 border-green-600">
            <div class="w-16 h-16 bg-green-100 text-green-600 rounded-full flex items-center justify-center mx-auto mb-4">
                <svg class="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7"></path>
                </svg>
            </div>
            <h2 class="text-2xl sm:text-3xl font-extrabold text-gray-900 mb-2">Suara Tersimpan!</h2>
            <p class="text-gray-600 text-sm sm:text-base mb-4">
                Terima kasih, pilihan Anda telah berhasil direkam secara rahasia.
            </p>
            <div class="inline-block bg-gray-100 text-gray-700 text-xs font-bold px-4 py-2 rounded-full">
                Kembali ke kamera scanner dalam <span id="countdown-text" class="text-red-600">3</span> detik...
            </div>
        </section>

    </main>

    <!-- ================= TAMPILAN 3: MODAL KONFIRMASI ================= -->
    <div id="modal-confirm" class="hidden fixed inset-0 bg-black/75 backdrop-blur-sm flex items-center justify-center p-4 z-50">
        <div class="bg-white rounded-2xl max-w-md w-full p-5 sm:p-6 text-center shadow-2xl fade-in border-t-8 border-blue-600">
            <h3 class="text-lg sm:text-xl font-bold text-gray-800 mb-3">
                Apakah Anda yakin memilih kandidat ini?
            </h3>

            <!-- Foto, Nomor Urut, dan Nama Calon yang Dipilih -->
            <div class="bg-gray-50 border-2 border-gray-200 rounded-xl p-3 mb-5">
                <span id="confirm-candidate-number" class="inline-block bg-red-600 text-white text-xs sm:text-sm font-extrabold px-4 py-1 rounded-full mb-2">
                    NOMOR URUT -
                </span>
                <img id="confirm-candidate-photo" src="" alt="Foto Kandidat Terpilih" class="w-36 h-40 sm:w-40 sm:h-44 object-cover mx-auto rounded-xl shadow-md border-2 border-white mb-2">
                <p id="confirm-candidate-name" class="text-lg sm:text-xl font-extrabold text-gray-900 uppercase"></p>
            </div>

            <!-- Tombol Ulangi (Merah) dan Tombol Pilih (Biru) -->
            <div class="flex gap-4 justify-center">
                <button onclick="tutupKonfirmasi()" class="flex-1 bg-red-600 hover:bg-red-700 text-white text-base sm:text-lg font-bold py-3 px-4 rounded-xl shadow-md transition active:scale-95">
                    Ulangi
                </button>
                <button onclick="selesaikanPemilihan()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white text-base sm:text-lg font-bold py-3 px-4 rounded-xl shadow-md transition active:scale-95">
                    Pilih
                </button>
            </div>
        </div>
    </div>

    <!-- ================= MODAL PENGATURAN FOTO & CALON (PANITIA) ================= -->
    <div id="modal-settings" class="hidden fixed inset-0 bg-black/80 backdrop-blur-sm flex items-center justify-center p-3 z-50 overflow-y-auto">
        <div class="bg-white rounded-2xl max-w-lg w-full p-5 sm:p-6 shadow-2xl my-auto max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b pb-3 mb-4">
                <h3 class="text-lg font-extrabold text-gray-900">⚙️ Pengaturan Foto & Nama Calon</h3>
                <button onclick="tutupPengaturan()" class="text-gray-500 hover:text-red-600 font-bold text-xl px-2">&times;</button>
            </div>
            <p class="text-xs text-gray-600 mb-4">Pilih foto dari Galeri perangkat. Foto akan otomatis dikompresi dan tersimpan permanen.</p>

            <div class="grid grid-cols-2 gap-3 mb-4 bg-gray-50 p-3 rounded-xl border">
                <div>
                    <label class="block text-xs font-bold text-gray-700 mb-1">Logo Kiri (Subang)</label>
                    <input type="file" accept="image/*" onchange="ubahGambar('logoKiri', event)" class="text-xs w-full">
                </div>
                <div>
                    <label class="block text-xs font-bold text-gray-700 mb-1">Logo Kanan (Sukatani)</label>
                    <input type="file" accept="image/*" onchange="ubahGambar('logoKanan', event)" class="text-xs w-full">
                </div>
            </div>

            <div id="settings-candidates-list" class="space-y-3 mb-5"></div>

            <div class="flex gap-2">
                <button onclick="resetDataBawaan()" class="bg-gray-200 hover:bg-gray-300 text-gray-800 text-xs font-bold py-3 px-4 rounded-xl">
                    Reset Bawaan
                </button>
                <button onclick="simpanPengaturan()" class="flex-1 bg-green-600 hover:bg-green-700 text-white text-sm font-bold py-3 px-4 rounded-xl shadow">
                    💾 Simpan & Terapkan
                </button>
            </div>
        </div>
    </div>

    <footer class="text-center text-gray-500 text-[11px] py-1">
        Panitia Pemilihan Kepala Desa Sukatani - Kecamatan Compreng &copy; 2026
    </footer>

    <!-- ================= LOGIKA JAVASCRIPT ================= -->
    <script>
        // Token QR Code yang diizinkan masuk (bisa dikosongkan "" jika semua QR Code boleh masuk)
        const KODE_QR_VALID = "SUKATANI-2026-SAH";

        function buatAvatarSVG(teks, warnaBg) {
            const svg = `<svg xmlns="http://www.w3.org/2000/svg" width="300" height="300" viewBox="0 0 300 300">
                <rect width="300" height="300" fill="${warnaBg}"/>
                <circle cx="150" cy="110" r="55" fill="#ffffff" opacity="0.85"/>
                <path d="M60 270 C60 200, 240 200, 240 270" fill="#ffffff" opacity="0.85"/>
                <text x="150" y="290" font-family="sans-serif" font-size="22" font-weight="bold" fill="#ffffff" text-anchor="middle">${teks}</text>
            </svg>`;
            return "data:image/svg+xml;charset=utf-8," + encodeURIComponent(svg);
        }

        const dataDefault = {
            logoKiri: buatAvatarSVG("KAB. SUBANG", "#dc2626"),
            logoKanan: buatAvatarSVG("DS. SUKATANI", "#1e40af"),
            kandidat: [
                { nomor: 1, nama: "NAMA CALON 1", foto: buatAvatarSVG("CALON 01", "#334155") },
                { nomor: 2, nama: "MUFIDIN",      foto: buatAvatarSVG("CALON 02", "#0f766e") },
                { nomor: 3, nama: "NAMA CALON 3", foto: buatAvatarSVG("CALON 03", "#1d4ed8") },
                { nomor: 4, nama: "NAMA CALON 4", foto: buatAvatarSVG("CALON 04", "#b45309") }
            ]
        };

        let appData = JSON.parse(localStorage.getItem('pilkades_sukatani_2026')) || JSON.parse(JSON.stringify(dataDefault));
        let kandidatTerpilih = null;
        let html5QrCode = null;
        let kameraSedangBerjalan = false;
        let modeKamera = "environment";
        let timerKembali = null;

        function muatLogoHeader() {
            document.getElementById('img-logo-kiri').src = appData.logoKiri;
            document.getElementById('img-logo-kanan').src = appData.logoKanan;
        }

        // --- 1. KAMERA LANGSUNG JALAN SAAT AWAL MASUK ---
        async function hentikanKameraBersih() {
            if (html5QrCode && kameraSedangBerjalan) {
                try {
                    await html5QrCode.stop();
                    await html5QrCode.clear();
                } catch (e) {}
                kameraSedangBerjalan = false;
            }
        }

        async function mulaiKameraOtomatis() {
            const statusText = document.getElementById('camera-status');
            const btnStart = document.getElementById('btn-start-cam');
            const btnSwitch = document.getElementById('btn-switch-cam');
            const badgeText = document.getElementById('badge-cam-text');

            statusText.classList.remove('hidden');
            statusText.textContent = "Menyalakan kamera...";
            btnStart.classList.add('hidden');

            if (!html5QrCode) {
                html5QrCode = new Html5Qrcode("reader");
            }

            await hentikanKameraBersih();

            try {
                await html5QrCode.start(
                    { facingMode: modeKamera },
                    { fps: 15, qrbox: { width: 190, height: 190 } },
                    (decodedText) => {
                        // Cek apakah QR Code valid (jika KODE_QR_VALID diisi)
                        if (!KODE_QR_VALID || decodedText.trim().toUpperCase() === KODE_QR_VALID) {
                            if (navigator.vibrate) navigator.vibrate(150);
                            hentikanKameraDanMasuk();
                        } else {
                            statusText.classList.remove('hidden');
                            statusText.textContent = "QR Code tidak terdaftar! Gunakan QR resmi.";
                        }
                    },
                    () => {}
                );
                kameraSedangBerjalan = true;
                statusText.classList.add('hidden');
                btnSwitch.classList.remove('hidden');
                badgeText.textContent = "KAMERA LIVE AKTIF";
            } catch (err) {
                kameraSedangBerjalan = false;
                badgeText.textContent = "KAMERA BUTUH IZIN";
                statusText.textContent = "Pastikan dibuka lewat link HTTPS dan izinkan akses kamera pada browser.";
                btnStart.classList.remove('hidden');
            }
        }

        async function gantiKamera() {
            modeKamera = (modeKamera === "environment") ? "user" : "environment";
            await mulaiKameraOtomatis();
        }

        async function hentikanKameraDanMasuk() {
            await hentikanKameraBersih();
            masukKeSuratSuara();
        }

        // --- 2. SURAT SUARA LANDSCAPE (4 KANDIDAT SEJAJAR) ---
        function renderKandidat() {
            const container = document.getElementById('candidates-container');
            container.innerHTML = '';

            appData.kandidat.forEach((calon) => {
                const isSelected = kandidatTerpilih && kandidatTerpilih.nomor === calon.nomor;

                const card = document.createElement('div');
                card.onclick = () => pilihKartuKandidat(calon);
                card.className = `relative cursor-pointer rounded-2xl border-4 transition-all duration-150 overflow-hidden bg-gray-50 flex flex-col justify-between shadow-md active:scale-95 ${
                    isSelected ? 'border-green-500 ring-4 ring-green-200 scale-[1.02]' : 'border-gray-200 hover:border-blue-300'
                }`;

                card.innerHTML = `
                    <div class="bg-red-600 text-white text-center py-1 sm:py-1.5 font-extrabold text-lg sm:text-2xl">
                        ${calon.nomor}
                    </div>
                    <div class="relative p-1.5 sm:p-2.5 bg-white flex-grow flex items-center justify-center">
                        <img src="${calon.foto}" alt="${calon.nama}" class="w-full h-32 sm:h-44 md:h-48 object-cover rounded-xl border border-gray-200">
                        <!-- Simbol Centang Hijau di Atas Samping Foto -->
                        <div class="${isSelected ? 'flex' : 'hidden'} absolute top-2.5 right-2.5 sm:top-4 sm:right-4 w-8 h-8 sm:w-11 sm:h-11 bg-green-500 text-white rounded-full items-center justify-center shadow-lg border-2 border-white">
                            <svg class="w-5 h-5 sm:w-7 sm:h-7" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3.5" d="M5 13l4 4L19 7"></path>
                            </svg>
                        </div>
                    </div>
                    <div class="p-2 sm:p-3 text-center bg-gray-100 border-t border-gray-200">
                        <h3 class="text-xs sm:text-base md:text-lg font-extrabold text-gray-900 uppercase leading-tight truncate">${calon.nama}</h3>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function pilihKartuKandidat(calon) {
            kandidatTerpilih = calon;
            renderKandidat();

            const btnSubmit = document.getElementById('btn-submit-choice');
            const helperText = document.getElementById('helper-text');
            btnSubmit.disabled = false;
            helperText.textContent = `Terpilih: Nomor Urut ${calon.nomor} (${calon.nama}). Tekan tombol biru untuk melanjutkan.`;
            helperText.classList.replace('text-gray-500', 'text-blue-600');
            helperText.classList.add('font-bold');
        }

        function masukKeSuratSuara() {
            document.getElementById('view-login').classList.add('hidden');
            document.getElementById('view-ballot').classList.remove('hidden');
            renderKandidat();
        }

        // --- 3. KONFIRMASI & SIMPAN SUARA ---
        function bukaKonfirmasi() {
            if (!kandidatTerpilih) return;
            document.getElementById('confirm-candidate-number').textContent = `NOMOR URUT ${kandidatTerpilih.nomor}`;
            document.getElementById('confirm-candidate-photo').src = kandidatTerpilih.foto;
            document.getElementById('confirm-candidate-name').textContent = kandidatTerpilih.nama;
            document.getElementById('modal-confirm').classList.remove('hidden');
        }

        function tutupKonfirmasi() {
            document.getElementById('modal-confirm').classList.add('hidden');
        }

        function selesaikanPemilihan() {
            // Simpan suara secara rahasia ke dalam array di localStorage (tidak ditampilkan ke layar)
            const logSuara = JSON.parse(localStorage.getItem('rekam_suara_rahasia_2026')) || [];
            logSuara.push({
                nomorUrut: kandidatTerpilih.nomor,
                namaKandidat: kandidatTerpilih.nama,
                waktu: new Date().toISOString()
            });
            localStorage.setItem('rekam_suara_rahasia_2026', JSON.stringify(logSuara));

            document.getElementById('modal-confirm').classList.add('hidden');
            document.getElementById('view-ballot').classList.add('hidden');
            document.getElementById('view-finish').classList.remove('hidden');

            // Hitung mundur 3 detik lalu otomatis kembali ke tampilan Kamera Scan QR
            let sisaDetik = 3;
            document.getElementById('countdown-text').textContent = sisaDetik;
            clearInterval(timerKembali);
            timerKembali = setInterval(() => {
                sisaDetik--;
                document.getElementById('countdown-text').textContent = sisaDetik;
                if (sisaDetik <= 0) {
                    clearInterval(timerKembali);
                    resetAplikasi();
                }
            }, 1000);
        }

        // --- 4. OTOMATIS KEMBALI KE KAMERA UNTUK PEMILIH BERIKUTNYA ---
        function resetAplikasi() {
            kandidatTerpilih = null;
            document.getElementById('btn-submit-choice').disabled = true;
            const helperText = document.getElementById('helper-text');
            helperText.textContent = "*Tekan salah satu foto kandidat di atas.";
            helperText.classList.replace('text-blue-600', 'text-gray-500');
            helperText.classList.remove('font-bold');

            document.getElementById('view-finish').classList.add('hidden');
            document.getElementById('view-login').classList.remove('hidden');
            mulaiKameraOtomatis();
        }

        // --- PENGATURAN FOTO & NAMA CALON ---
        function bukaPengaturan() {
            const list = document.getElementById('settings-candidates-list');
            list.innerHTML = '';
            appData.kandidat.forEach((calon, idx) => {
                list.innerHTML += `
                    <div class="p-3 border rounded-xl bg-gray-50 flex flex-col gap-2">
                        <div class="flex items-center gap-2">
                            <span class="bg-red-600 text-white font-bold text-xs px-2.5 py-1 rounded-lg">No. ${calon.nomor}</span>
                            <input type="text" id="input-nama-${idx}" value="${calon.nama}" class="flex-1 border rounded-lg px-2.5 py-1.5 text-sm font-bold uppercase">
                        </div>
                        <input type="file" accept="image/*" onchange="ubahFotoKandidat(${idx}, event)" class="text-xs">
                    </div>
                `;
            });
            document.getElementById('modal-settings').classList.remove('hidden');
        }

        function tutupPengaturan() {
            document.getElementById('modal-settings').classList.add('hidden');
        }

        function prosesKompresiGambar(file, callback) {
            const reader = new FileReader();
            reader.onload = function(e) {
                const img = new Image();
                img.onload = function() {
                    const canvas = document.createElement('canvas');
                    const maxW = 400;
                    const scale = maxW / img.width;
                    canvas.width = maxW;
                    canvas.height = img.height * scale;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                    callback(canvas.toDataURL('image/jpeg', 0.8));
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        }

        function ubahGambar(tipe, event) {
            const file = event.target.files[0];
            if (!file) return;
            prosesKompresiGambar(file, (base64) => { appData[tipe] = base64; });
        }

        function ubahFotoKandidat(index, event) {
            const file = event.target.files[0];
            if (!file) return;
            prosesKompresiGambar(file, (base64) => { appData.kandidat[index].foto = base64; });
        }

        function simpanPengaturan() {
            appData.kandidat.forEach((calon, idx) => {
                const inputNama = document.getElementById(`input-nama-${idx}`).value.trim();
                if (inputNama) calon.nama = inputNama;
            });
            localStorage.setItem('pilkades_sukatani_2026', JSON.stringify(appData));
            muatLogoHeader();
            renderKandidat();
            tutupPengaturan();
            alert("Pengaturan foto & nama kandidat berhasil disimpan!");
        }

        function resetDataBawaan() {
            if (confirm("Kembalikan semua foto dan nama calon ke pengaturan awal?")) {
                localStorage.removeItem('pilkades_sukatani_2026');
                appData = JSON.parse(JSON.stringify(dataDefault));
                muatLogoHeader();
                renderKandidat();
                tutupPengaturan();
            }
        }

        // Langsung nyalakan kamera begitu halaman dibuka
        window.addEventListener('DOMContentLoaded', () => {
            muatLogoHeader();
            mulaiKameraOtomatis();
        });
    </script>
</body>
</html>
