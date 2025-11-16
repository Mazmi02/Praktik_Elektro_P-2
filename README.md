<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Ground Fault (Gangguan Tanah)?", "id": "Arus Listrik Bocor Langsung Ke Tanah." },
  { "en": "Apa Itu Arus Gangguan (Fault Current)?", "id": "Arus Sangat Tinggi Saat Hubung Singkat." },
  { "en": "Apa Itu Kapasitas Pemutusan (Breaking Capacity)?", "id": "Arus Maksimum Yang Dapat Diputus MCB/Sekring." },
  { "en": "Apa Itu Sensor Hall Effect (Efek Hall)?", "id": "Sensor Yang Mendeteksi Keberadaan Medan Magnet." },
  { "en": "Apa Aplikasi Sensor Hall Effect?", "id": "Mendeteksi Posisi, Kecepatan Putaran, Dan Arus Listrik." },
  { "en": "Apa Itu Sensor Piezoelektrik?", "id": "Sensor Menghasilkan Tegangan Saat Ditekan (Mekanis)." },
  { "en": "Apa Aplikasi Sensor Piezoelektrik?", "id": "Sensor Getaran, Sensor Tekanan, Mikrofon." },
  { "en": "Apa Itu Sensor Pirani (Pirani Gauge)?", "id": "Sensor Pengukur Tekanan Vakum (Hampa Udara)." },
  { "en": "Apa Itu Sensor Tekanan (Pressure Transducer)?", "id": "Mengubah Tekanan Fluida Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Sel Beban (Load Cell)?", "id": "Sensor Mengubah Gaya (Berat) Menjadi Sinyal Listrik." },
  { "en": "Apa Prinsip Kerja Sel Beban (Load Cell)?", "id": "Menggunakan Strain Gauge (Jembatan Wheatstone)." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor Pengukur Percepatan Dan Getaran." },
  { "en": "Apa Fungsi Akselerometer Dalam Gawai (Gadget)?", "id": "Mendeteksi Orientasi Layar (Rotasi Otomatis)." },
  { "en": "Apa Itu Giroskop (Gyroscope)?", "id": "Sensor Pengukur Kecepatan Sudut (Rotasi)." },
  { "en": "Apa Fungsi Giroskop (Gyroscope)?", "id": "Stabilisasi Gambar (Kamera) Atau Navigasi." },
  { "en": "Apa Kepanjangan IMU (Inertial Measurement Unit)?", "id": "Unit Pengukuran Inersia." },
  { "en": "Apa Isi IMU (Inertial Measurement Unit)?", "id": "Gabungan Akselerometer Dan Giroskop." },
  { "en": "Apa Itu Termostat (Thermostat)?", "id": "Saklar Otomatis Berdasarkan Suhu (Contoh: AC)." },
  { "en": "Apa Fungsi Termostat (Thermostat)?", "id": "Menjaga Suhu Tetap Stabil Sesuai Setelan." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Tinggi Berbasis Efek Seebeck." },
  { "en": "Apa Prinsip Kerja Termokopel?", "id": "Dua Logam Berbeda Menghasilkan Tegangan (Efek Seebeck)." },
  { "en": "Apa Kepanjangan RTD (Resistance Temperature Detector)?", "id": "Detektor Suhu Berbasis Resistansi." },
  { "en": "Apa Bahan Umum RTD (Resistance Temperature Detector)?", "id": "Platina (Platinum), Contohnya PT100." },
  { "en": "Beda Utama RTD Dan Termokopel?", "id": "RTD (Resistansi), Termokopel (Tegangan)." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Yang Nilainya Peka Terhadap Suhu." },
  { "en": "Apa Itu NTC (Negative Temperature Coefficient)?", "id": "Suhu Naik, Resistansi Turun." },
  { "en": "Apa Itu PTC (Positive Temperature Coefficient)?", "id": "Suhu Naik, Resistansi Naik." },
  { "en": "Apa Aplikasi Termistor NTC?", "id": "Sensor Suhu Presisi (Termometer Digital)." },
  { "en": "Apa Aplikasi Termistor PTC?", "id": "Sekring Dapat Disetel Ulang (Resettable Fuse)." },
  { "en": "Apa Itu Dioda Foto (Photodiode)?", "id": "Sensor Cahaya (Mengubah Cahaya Menjadi Arus)." },
  { "en": "Apa Itu Transistor Foto (Phototransistor)?", "id": "Sensor Cahaya (Gabungan Dioda Foto Dan Transistor)." },
  { "en": "Apa Beda Dioda Foto Dan Transistor Foto?", "id": "Transistor Foto Lebih Sensitif (Ada Penguatan)." },
  { "en": "Apa Itu Optoisolator (Optocoupler)?", "id": "Gabungan LED Dan Transistor Foto (Isolasi)." },
  { "en": "Apa Fungsi Utama Optoisolator?", "id": "Isolasi Galvani (Memisahkan Dua Rangkaian Listrik)." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relai Elektronik (Tanpa Komponen Mekanis Bergerak)." },
  { "en": "Apa Keunggulan SSR (Solid State Relay)?", "id": "Cepat, Senyap, Tahan Lama (Awet)." },
  { "en": "Apa Kekurangan SSR (Solid State Relay)?", "id": "Menghasilkan Panas, Rentan Lonjakan Tegangan." },
  { "en": "Apa Itu Relai (Relay) Elektromekanis?", "id": "Saklar Yang Diaktifkan Oleh Kumparan Elektromagnetik." },
  { "en": "Apa Itu Koil (Coil) Relai?", "id": "Kumparan Elektromagnetik Penggerak Saklar Relai." },
  { "en": "Apa Itu Kontak NO (Normally Open) Relai?", "id": "Kontak Terbuka Saat Relai Mati (Tidak Aktif)." },
  { "en": "Apa Itu Kontak NC (Normally Closed) Relai?", "id": "Kontak Tertutup Saat Relai Mati (Tidak Aktif)." },
  { "en": "Apa Itu Kontak COM (Common) Relai?", "id": "Kaki Pusat (Umum) Saklar Relai." },
  { "en": "Apa Itu Dioda Flyback (Flyback Diode)?", "id": "Dioda Pelindung Lonjakan Tegangan Induktif Balik." },
  { "en": "Dimana Dioda Flyback Dipasang?", "id": "Paralel Terbalik Dengan Beban Induktif (Koil Relai)." },
  { "en": "Apa Fungsi Dioda Flyback Pada Relai?", "id": "Melindungi Transistor Pengontrol Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Kontaktor (Contactor)?", "id": "Relai Berkapasitas Besar (Daya Tinggi, Motor)." },
  { "en": "Apa Beda Relai Dan Kontaktor?", "id": "Kontaktor Untuk Arus Sangat Besar (Motor Listrik)." },
  { "en": "Apa Itu Kontak Bantu (Auxiliary Contact) Kontaktor?", "id": "Kontak Tambahan (NO/NC) Untuk Rangkaian Kontrol." },
  { "en": "Apa Itu Thermal Overload Relay (TOR)?", "id": "Relai Proteksi Beban Lebih Termal (Panas)." },
  { "en": "Apa Fungsi Thermal Overload Relay (TOR)?", "id": "Melindungi Motor Listrik Dari Beban Berlebih (Overload)." },
  { "en": "Bagaimana TOR (Thermal Overload Relay) Bekerja?", "id": "Menggunakan Bimetal (Melengkung Jika Arus Berlebih)." },
  { "en": "Apa Itu Motor DC Sikat (Brushed DC Motor)?", "id": "Motor DC Menggunakan Sikat (Brush) Karbon." },
  { "en": "Apa Itu Komutator (Commutator) Motor DC?", "id": "Segmen Logam Pembalik Arah Arus Di Rotor." },
  { "en": "Apa Fungsi Sikat (Brush) Motor DC?", "id": "Menyalurkan Listrik Dari Luar Ke Komutator." },
  { "en": "Apa Itu Motor DC Tanpa Sikat (BLDC)?", "id": "Motor DC Tanpa Komutator Dan Sikat Mekanis." },
  { "en": "Apa Kepanjangan BLDC (Brushless DC Motor)?", "id": "Motor DC Tanpa Sikat." },
  { "en": "Apa Keunggulan Motor BLDC (Brushless DC Motor)?", "id": "Efisien, Awet (Tanpa Perawatan Sikat), Senyap." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Loop Tertutup (Kontrol Posisi Akurat)." },
  { "en": "Apa Komponen Utama Motor Servo?", "id": "Motor DC, Gearbox, Sensor Posisi (Potensiometer/Encoder)." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Yang Bergerak Per Langkah (Step) Presisi." },
  { "en": "Apa Itu Mode Full Step Motor Stepper?", "id": "Mode Langkah Penuh (Akurasi Standar)." },
  { "en": "Apa Itu Mode Half Step Motor Stepper?", "id": "Mode Setengah Langkah (Akurasi Dua Kali Lipat)." },
  { "en": "Apa Itu Mode Microstepping Motor Stepper?", "id": "Mode Gerakan Sangat Halus (Resolusi Tinggi)." },
  { "en": "Apa Keunggulan Motor Stepper?", "id": "Kontrol Posisi Loop Terbuka Yang Akurat." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Motor AC Asinkron (Rotor Berputar Lebih Lambat)." },
  { "en": "Apa Itu Motor Sinkron (Synchronous Motor)?", "id": "Motor AC (Rotor Berputar Sama Cepat Medan Stator)." },
  { "en": "Apa Itu Rotor Sangkar Tupai (Squirrel Cage)?", "id": "Jenis Rotor Motor Induksi Paling Umum." },
  { "en": "Apa Itu Slip (Selip) Motor Induksi?", "id": "Perbedaan Putaran Rotor Dan Medan Magnet Stator." },
  { "en": "Apa Itu Motor Universal (Universal Motor)?", "id": "Motor Dapat Beroperasi Menggunakan Listrik AC Atau DC." },
  { "en": "Di Mana Motor Universal Digunakan?", "id": "Alat Rumah Tangga (Bor Tangan, Blender)." },
  { "en": "Apa Itu Turbin Uap (Steam Turbine)?", "id": "Mesin Putar Diaktifkan Oleh Energi Uap Panas." },
  { "en": "Apa Itu Turbin Gas (Gas Turbine)?", "id": "Mesin Putar Diaktifkan Oleh Gas Hasil Pembakaran." },
  { "en": "Apa Itu Turbin Air (Water Turbine)?", "id": "Mesin Putar Diaktifkan Oleh Energi Potensial Air." },
  { "en": "Apa Itu Boiler (Ketel Uap)?", "id": "Peralatan Menghasilkan Uap (Untuk Turbin Uap)." },
  { "en": "Apa Itu Kondensor (Condenser) Pembangkit Listrik?", "id": "Mengubah Uap Sisa Turbin Kembali Menjadi Air." },
  { "en": "Apa Itu Generator Sinkron (Synchronous Generator)?", "id": "Generator Menghasilkan Listrik AC (Frekuensi Sinkron Putaran)." },
  { "en": "Apa Nama Lain Generator Sinkron?", "id": "Alternator (Alternating Current Generator)." },
  { "en": "Apa Itu Sistem Eksitasi (Excitation System) Generator?", "id": "Sistem Pembangkit Medan Magnet Pada Rotor Generator." },
  { "en": "Apa Fungsi AVR (Automatic Voltage Regulator) Generator?", "id": "Menjaga Tegangan Output Generator Tetap Stabil." },
  { "en": "Apa Fungsi Governor (Pengatur Putaran) Generator?", "id": "Menjaga Kecepatan (Frekuensi) Generator Tetap Stabil." },
  { "en": "Apa Itu Sinkronisasi (Paralel) Generator?", "id": "Proses Menggabungkan Dua Generator Ke Jaringan Bersama." },
  { "en": "Apa Syarat Sinkronisasi Generator?", "id": "Tegangan Sama, Frekuensi Sama, Fasa Sama." },
  { "en": "Apa Itu Jaringan Listrik (Power Grid)?", "id": "Sistem Interkoneksi Pembangkit, Transmisi, Dan Distribusi." },
  { "en": "Apa Itu Frekuensi Jaringan Listrik Indonesia?", "id": "Lima Puluh Hertz (50 Hz)." },
  { "en": "Apa Itu Transmisi Listrik?", "id": "Penyaluran Listrik Tegangan Tinggi Jarak Jauh." },
  { "en": "Apa Itu Distribusi Listrik?", "id": "Penyaluran Listrik (Tegangan Rendah) Ke Pelanggan." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Tempat Transformasi Tegangan (Naik Atau Turun)." },
  { "en": "Apa Itu Isolator (Insulator) Jaringan Listrik?", "id": "Komponen Pencegah Aliran Listrik (Ke Tiang)." },
  { "en": "Apa Bahan Isolator Jaringan Listrik?", "id": "Keramik, Kaca, Atau Polimer." },
  { "en": "Apa Itu Lightning Arrester (Penangkal Petir)?", "id": "Pelindung Jaringan Listrik Dari Sambaran Petir." },
  { "en": "Apa Fungsi Lightning Arrester?", "id": "Membuang Tegangan Lebih Petir Ke Tanah." },
  { "en": "Apa Itu Kawat Tanah (Ground Wire) Transmisi?", "id": "Kawat Pelindung Petir Di Puncak SUTET." },
  { "en": "Di Mana Posisi Kawat Tanah Transmisi?", "id": "Di Bagian Paling Atas Menara Transmisi." },
  { "en": "Apa Itu PMT (Pemutus Tenaga)?", "id": "Memutus Arus Saat Berbeban Atau Gangguan (Korsleting)." },
  { "en": "Apa Itu PMS (Pemisah)?", "id": "Memutus Rangkaian Saat Tidak Berbeban (Pengaman Perawatan)." },
  { "en": "Kapan Boleh Membuka PMS (Pemisah)?", "id": "Hanya Saat Rangkaian Sudah Tidak Berbeban." },
  { "en": "Apa Itu MOV (Metal Oxide Varistor)?", "id": "Komponen Pelindung Lonjakan Tegangan (Surge)." },
  { "en": "Apa Fungsi MOV (Metal Oxide Varistor)?", "id": "Memotong (Clamp) Tegangan Berlebih Sesaat." },
  { "en": "Apa Itu GDT (Gas Discharge Tube)?", "id": "Tabung Gas Pelindung Lonjakan Tegangan (Surge)." },
  { "en": "Apa Fungsi GDT (Gas Discharge Tube)?", "id": "Melindungi Saluran Telekomunikasi Dari Lonjakan Petir." }, 
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppressor)?", "id": "Dioda Pelindung Lonjakan Tegangan (Respon Sangat Cepat)." },
  { "en": "Apa Beda TVS Dan MOV?", "id": "TVS Respon Lebih Cepat, MOV Kapasitas Energi Lebih Besar." },
  { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage) Dioda?", "id": "Tegangan Balik (Reverse) Menyebabkan Dioda Menghantar Arus." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Dioda Yang Sengaja Dibuat Bekerja Di Area Breakdown." },
  { "en": "Apa Fungsi Utama Dioda Zener?", "id": "Sebagai Penstabil Atau Regulator Tegangan." },
  { "en": "Apa Itu Tegangan Zener (Vz)?", "id": "Tegangan Breakdown Stabil Pada Dioda Zener." },
  { "en": "Apa Itu Rangkaian Penstabil Zener (Zener Regulator)?", "id": "Rangkaian Pembagi Tegangan Menggunakan Resistor Dan Zener." },
  { "en": "Apa Itu Dioda Penyearah (Rectifier Diode)?", "id": "Mengubah Tegangan AC Menjadi Tegangan DC." },
  { "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave Rectifier)?", "id": "Rangkaian Penyearah Menggunakan Satu Dioda." },
  { "en": "Apa Itu Penyearah Gelombang Penuh (Full-Wave Rectifier)?", "id": "Menyearahkan Kedua Siklus (Positif Dan Negatif) AC." },
  { "en": "Apa Itu Penyearah Jembatan (Bridge Rectifier)?", "id": "Penyearah Gelombang Penuh Menggunakan Empat Dioda." },
  { "en": "Apa Keuntungan Penyearah Jembatan?", "id": "Lebih Efisien, Riak (Ripple) Lebih Kecil." },
  { "en": "Apa Itu Riak (Ripple) Tegangan DC?", "id": "Sisa Tegangan AC Yang Belum Rata Pada DC." },
  { "en": "Apa Fungsi Kapasitor Filter (Filtering Capacitor)?", "id": "Memperhalus (Meratakan) Tegangan DC Hasil Penyearah." },
  { "en": "Apa Itu Dioda Schottky (Schottky Diode)?", "id": "Dioda Sambungan Logam-Semikonduktor (Tegangan Maju Rendah)." },
  { "en": "Apa Keunggulan Dioda Schottky?", "id": "Tegangan Maju (Vf) Rendah, Waktu Switching Cepat." },
  { "en": "Di Mana Dioda Schottky Digunakan?", "id": "Catu Daya Switching (SMPS), Proteksi Polaritas Terbalik." },
  { "en": "Apa Itu Dioda Pemancar Cahaya (LED)?", "id": "Dioda Yang Mengubah Listrik Menjadi Cahaya." },
  { "en": "Apa Kepanjangan LED (Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya." },
  { "en": "Apa Fungsi Resistor Pembatas Arus (Current Limiting Resistor)?", "id": "Resistor Seri Untuk Membatasi Arus LED." },
  { "en": "Mengapa LED Memerlukan Resistor Pembatas Arus?", "id": "Mencegah LED Terbakar Akibat Arus Berlebih." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage/Vf) LED?", "id": "Tegangan Yang Dibutuhkan LED Untuk Menyala." },
  { "en": "Apa Itu LED Inframerah (Infrared LED)?", "id": "LED Pemancar Cahaya Inframerah (Tak Terlihat)." },
  { "en": "Di Mana LED Inframerah Digunakan?", "id": "Remote Control (TV, AC), Sensor Halangan." },
  { "en": "Apa Itu LED Ultraviolet (UV LED)?", "id": "LED Pemancar Cahaya Ultraviolet (Sterilisasi, Deteksi Uang)." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Transistor Yang Dikendalikan Oleh Arus Basis (Base)." },
  { "en": "Sebutkan Tiga Kaki Transistor BJT?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Sebutkan Dua Tipe Transistor BJT?", "id": "Tipe NPN Dan Tipe PNP." },
  { "en": "Apa Fungsi Transistor BJT?", "id": "Sebagai Saklar (Switch) Dan Penguat (Amplifier)." },
  { "en": "Apa Itu Daerah Cut-Off (Cut-Off) Transistor?", "id": "Kondisi Transistor Mati (Off), Arus Kolektor Nol." },
  { "en": "Apa Itu Daerah Saturasi (Saturation) Transistor?", "id": "Kondisi Transistor Hidup Penuh (On), Seperti Saklar Tertutup." },
  { "en": "Apa Itu Daerah Aktif (Active Region) Transistor?", "id": "Kondisi Transistor Sebagai Penguat Sinyal (Amplifier)." },
  { "en": "Apa Itu Penguatan Arus (Hfe Atau Beta) BJT?", "id": "Rasio Arus Kolektor (Ic) Terhadap Arus Basis (Ib)." },
  { "en": "Apa Itu Transistor FET (Field-Effect Transistor)?", "id": "Transistor Yang Dikendalikan Oleh Tegangan Gerbang (Gate)." },
  { "en": "Sebutkan Tiga Kaki Transistor FET?", "id": "Gerbang (Gate), Sumber (Source), Kuras (Drain)." },
  { "en": "Apa Keunggulan FET Dibanding BJT?", "id": "Impedansi Input Sangat Tinggi (Hampir Tidak Butuh Arus Gate)." },
  { "en": "Apa Kepanjangan JFET (Junction Field-Effect Transistor)?", "id": "Transistor Efek Medan Sambungan." },
  { "en": "Apa Kepanjangan MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET Oksida Logam Semikonduktor." },
  { "en": "Sebutkan Dua Tipe MOSFET?", "id": "Tipe N-Channel Dan Tipe P-Channel." },
  { "en": "Sebutkan Dua Mode MOSFET?", "id": "Mode Penipisan (Depletion) Dan Mode Peningkatan (Enhancement)." },
  { "en": "Apa Itu MOSFET Mode Peningkatan (Enhancement Mode)?", "id": "MOSFET Paling Umum (Mati Saat Gate Nol Volt)." },
  { "en": "Apa Kelemahan MOSFET?", "id": "Sangat Rentan Terhadap Kerusakan Akibat Listrik Statis (ESD)." },
  { "en": "Apa Kepanjangan ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis." },
  { "en": "Bagaimana Melindungi Komponen Dari ESD?", "id": "Menggunakan Gelang Anti-Statis (ESD Wrist Strap)." },
  { "en": "Apa Kepanjangan CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Semikonduktor Oksida Logam Komplementer." },
  { "en": "Apa Itu CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Logika Digital Menggunakan Pasangan MOSFET (N-Channel, P-Channel)." },
  { "en": "Apa Keunggulan Logika CMOS?", "id": "Konsumsi Daya Statis Sangat Rendah (Hampir Nol)." },
  { "en": "Apa Kepanjangan TTL (Transistor-Transistor Logic)?", "id": "Logika Transistor-Transistor." },
  { "en": "Apa Itu Logika TTL (Transistor-Transistor Logic)?", "id": "Logika Digital Berbasis Transistor BJT." },
  { "en": "Berapa Tegangan Kerja Standar Logika TTL?", "id": "Lima (5) Volt DC." },
  { "en": "Berapa Tegangan Kerja Umum Logika CMOS Modern?", "id": "Tiga Koma Tiga (3.3) Volt DC." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Itu Gerbang AND (DAN)?", "id": "Output 1 Jika SEMUA Input 1." },
  { "en": "Apa Itu Gerbang OR (ATAU)?", "id": "Output 1 Jika SALAH SATU Input 1." },
  { "en": "Apa Itu Gerbang NOT (TIDAK/Inverter)?", "id": "Membalik Input (0 Jadi 1, 1 Jadi 0)." },
  { "en": "Apa Itu Gerbang NAND (BUKAN DAN)?", "id": "Kebalikan Dari Gerbang AND." },
  { "en": "Apa Itu Gerbang NOR (BUKAN ATAU)?", "id": "Kebalikan Dari Gerbang OR." },
  { "en": "Apa Itu Gerbang XOR (ATAU Eksklusif)?", "id": "Output 1 Jika Input BERBEDA (Ganjil)." },
  { "en": "Apa Itu Gerbang XNOR (BUKAN XOR)?", "id": "Output 1 Jika Input SAMA (Genap)." },
  { "en": "Apa Itu Tabel Kebenaran (Truth Table)?", "id": "Tabel Yang Menampilkan Output Gerbang Logika." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Untuk Menganalisis Sirkuit Logika Digital." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map/K-Map)?", "id": "Metode Grafis Menyederhanakan Ekspresi Logika Boolean." },
  { "en": "Apa Itu Rangkaian Kombinasional (Combinational Logic)?", "id": "Output Hanya Tergantung Pada Input Saat Ini." },
  { "en": "Apa Contoh Rangkaian Kombinasional?", "id": "Encoder, Decoder, Multiplexer, Demultiplexer." },
  { "en": "Apa Itu Rangkaian Sekuensial (Sequential Logic)?", "id": "Output Tergantung Input Saat Ini Dan Keadaan (Memori)." },
  { "en": "Apa Contoh Rangkaian Sekuensial?", "id": "Flip-Flop, Register, Pencacah (Counter)." },
  { "en": "Apa Itu Flip-Flop?", "id": "Rangkaian Dasar Penyimpan Satu (1) Bit Data." },
  { "en": "Apa Itu Sinyal Jam (Clock Signal)?", "id": "Sinyal Denyut (Pulsa) Untuk Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Flip-Flop Tipe D (Data/Delay)?", "id": "Menyimpan Nilai Input D Saat Tepi Jam Aktif." },
  { "en": "Apa Itu Flip-Flop Tipe T (Toggle)?", "id": "Membalik (Toggle) Nilai Output Setiap Tepi Jam." },
  { "en": "Apa Itu Flip-Flop Tipe JK?", "id": "Flip-Flop Universal (Bisa Jadi D, T, SR)." },
  { "en": "Apa Itu Register?", "id": "Kumpulan Flip-Flop Untuk Menyimpan Data (Word)." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Register Yang Dapat Menggeser Data (Bit) Kiri/Kanan." },
  { "en": "Apa Itu Pencacah (Counter)?", "id": "Rangkaian Sekuensial Penghitung Jumlah Pulsa Jam." },
  { "en": "Apa Itu Pencacah Asinkron (Asynchronous Counter/Ripple)?", "id": "Output Satu Flip-Flop Memicu Jam Flip-Flop Berikutnya." },
  { "en": "Apa Itu Pencacah Sinkron (Synchronous Counter)?", "id": "Semua Flip-Flop Diberi Sinyal Jam Bersamaan." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Memilih Satu Dari Banyak Input Ke Satu Output." },
  { "en": "Apa Nama Lain Multiplexer (MUX)?", "id": "Selektor Data (Data Selector)." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Mengirim Satu Input Ke Salah Satu Dari Banyak Output." },
  { "en": "Apa Nama Lain Demultiplexer (DEMUX)?", "id": "Distributor Data (Data Distributor)." },
  { "en": "Apa Itu Encoder (Penyandi)?", "id": "Mengubah Input (Desimal) Menjadi Kode Biner." },
  { "en": "Apa Itu Decoder (Pengurai Sandi)?", "id": "Mengubah Kode Biner Menjadi Output (Desimal)." },
  { "en": "Apa Itu Decoder BCD Ke 7-Segmen?", "id": "Decoder Mengubah Biner (BCD) Menjadi Tampilan 7-Segmen." },
  { "en": "Apa Kepanjangan BCD (Binary Coded Decimal)?", "id": "Desimal Yang Dikodekan Biner." },
  { "en": "Apa Itu Tampilan 7-Segmen (Seven-Segment Display)?", "id": "Tampilan Angka Menggunakan Tujuh Segmen LED." },
  { "en": "Apa Itu 7-Segmen Common Anode (Anoda Bersama)?", "id": "Semua Kaki Anoda (+) LED Terhubung Bersama." },
  { "en": "Apa Itu 7-Segmen Common Cathode (Katoda Bersama)?", "id": "Semua Kaki Katoda (-) LED Terhubung Bersama." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit)?", "id": "Sirkuit Terpadu (Chip)." },
  { "en": "Apa Itu IC Linear (Analog)?", "id": "IC Pengolah Sinyal Analog (Contoh: Op-Amp, 555)." },
  { "en": "Apa Itu IC Digital?", "id": "IC Pengolah Sinyal Digital (Contoh: Gerbang Logika, CPU)." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Penguat Diferensial Tegangan Sangat Tinggi." },
  { "en": "Apa Kepanjangan Op-Amp (Operational Amplifier)?", "id": "Penguat Operasional." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Penguat Op-Amp Yang Membalik Fasa Sinyal Output." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Penguat Op-Amp Yang Tidak Membalik Fasa Sinyal." },
  { "en": "Apa Itu Voltage Follower (Pengikut Tegangan)?", "id": "Penguat Op-Amp Dengan Penguatan (Gain) Satu (1)." },
  { "en": "Apa Fungsi Voltage Follower?", "id": "Sebagai Penyangga (Buffer) Impedansi." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Rangkaian Membandingkan Dua Tegangan Input." },
  { "en": "Apa Itu IC Pewaktu 555 (555 Timer)?", "id": "IC Serbaguna Untuk Pewaktu Dan Osilator." },
  { "en": "Apa Itu Mode Astable (Astabil) 555?", "id": "Mode Osilator (Penghasil Gelombang Kotak)." },
  { "en": "Apa Itu Mode Monostable (Monostabil) 555?", "id": "Mode Pewaktu Sekali Tembak (One-Shot Timer)." },
  { "en": "Apa Itu Regulator Tegangan (Voltage Regulator)?", "id": "IC Menjaga Tegangan Output Tetap Konstan." },
  { "en": "Apa Contoh IC Regulator Tegangan Linear?", "id": "Seri 78xx (Positif) Dan 79xx (Negatif)." },
  { "en": "Apa Arti Kode 7805?", "id": "Regulator Tegangan Output Positif Lima (5) Volt." },
  { "en": "Apa Arti Kode 7912?", "id": "Regulator Tegangan Output Negatif Dua Belas (12) Volt." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil (CPU, Memori, I/O) Dalam Satu Chip." },
  { "en": "Apa Beda Mikrokontroler Dan Mikroprosesor?", "id": "Mikrokontroler Punya RAM/ROM Internal, Mikroprosesor Tidak." },
  { "en": "Apa Kepanjangan CPU (Central Processing Unit)?", "id": "Unit Pemrosesan Pusat (Otak Mikrokontroler)." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memisahkan Memori Program (ROM) Dan Memori Data (RAM)." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Menggabungkan Memori Program Dan Data Dalam Satu Bus." },
  { "en": "Apa Kepanjangan GPIO (General Purpose Input/Output)?", "id": "Pin Input Atau Output Serbaguna." },
  { "en": "Apa Fungsi Register Arah Data (DDR)?", "id": "Mengatur Pin GPIO Sebagai Input Atau Output." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Resistor Menarik Pin Ke Tegangan Tinggi (VCC)." },
  { "en": "Apa Fungsi Resistor Pull-Up?", "id": "Memastikan Kondisi Logika Tinggi (High) Saat Saklar Terbuka." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Resistor Menarik Pin Ke Tegangan Rendah (Ground)." },
  { "en": "Apa Fungsi Resistor Pull-Down?", "id": "Memastikan Kondisi Logika Rendah (Low) Saat Saklar Terbuka." },
  { "en": "Apa Itu Resistor Pull-Up Internal?", "id": "Resistor Pull-Up Yang Sudah Ada Di Dalam Chip." },
  { "en": "Bagaimana Mengaktifkan Pull-Up Internal?", "id": "Mengatur Bit Register Khusus Di Mikrokontroler." },
  { "en": "Apa Itu Interupsi Eksternal (External Interrupt)?", "id": "Interupsi Yang Dipicu Oleh Perubahan Sinyal Pin Eksternal." },
  { "en": "Apa Kepanjangan ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Fungsi ADC (Analog-to-Digital Converter) Di Mikrokontroler?", "id": "Membaca Nilai Sensor Analog (Tegangan Variabel)." },
  { "en": "Apa Itu Resolusi ADC (ADC Resolution)?", "id": "Ukuran Detail Hasil Konversi (Contoh: 10-Bit)." },
  { "en": "Apa Arti ADC Resolusi 10-Bit?", "id": "Hasil Konversi Berupa Angka 0 Hingga 1023." },
  { "en": "Apa Itu Tegangan Referensi (Vref) ADC?", "id": "Tegangan Maksimum Yang Menjadi Acuan Konversi ADC." },
  { "en": "Apa Kepanjangan DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Menjadi Analog." },
  { "en": "Apa Fungsi DAC (Digital-to-Analog Converter)?", "id": "Menghasilkan Output Tegangan Analog Variabel." },
  { "en": "Apa Itu Timer/Counter?", "id": "Modul Perangkat Keras Penghitung Pulsa Jam Internal." },
  { "en": "Apa Fungsi Timer/Counter?", "id": "Menghitung Waktu, Membangkitkan Interupsi Periodik, PWM." },
  { "en": "Apa Itu Mode Overflow (Limpahan) Timer?", "id": "Timer Kembali Ke Nol Setelah Mencapai Nilai Maksimum." },
  { "en": "Apa Kepanjangan PWM (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa." },
  { "en": "Apa Fungsi Sinyal PWM?", "id": "Mengontrol Kecerahan LED Atau Kecepatan Motor DC." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle) PWM?", "id": "Persentase Waktu Sinyal Dalam Kondisi Tinggi (On)." },
  { "en": "Apa Arti Siklus Kerja 100 Persen?", "id": "Sinyal Tinggi (On) Sepenuhnya (Tegangan Penuh)." },
  { "en": "Apa Arti Siklus Kerja 0 Persen?", "id": "Sinyal Rendah (Off) Sepenuhnya (Tegangan Nol)." },
  { "en": "Apa Arti Siklus Kerja 50 Persen?", "id": "Waktu Sinyal Tinggi (On) Dan Rendah (Off) Sama." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Timer Pengawas Mencegah Sistem Macet (Hang)." },
  { "en": "Apa Kepanjangan WDT (Watchdog Timer)?", "id": "Pewaktu Anjing Penjaga." },
  { "en": "Bagaimana Watchdog Timer (WDT) Bekerja?", "id": "Program Utama Harus Mereset (Kick) WDT Periodik." },
  { "en": "Apa Yang Terjadi Jika WDT Tidak Direset?", "id": "WDT Akan Mereset (Restart) Paksa Mikrokontroler." },
  { "en": "Apa Itu Mode Tidur (Sleep Mode) Mikrokontroler?", "id": "Mode Hemat Daya (CPU Berhenti Sementara)." },
  { "en": "Bagaimana Membangunkan (Wake-Up) Mikrokontroler?", "id": "Melalui Interupsi Eksternal Atau Timer Internal." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Memori Non-Volatile (Penyimpan Program/Firmware)." },
  { "en": "Apa Itu Memori SRAM (Static RAM)?", "id": "Memori Volatile (Penyimpan Variabel Data Saat Jalan)." },
  { "en": "Apa Itu Memori EEPROM (Electrically Erasable PROM)?", "id": "Memori Non-Volatile (Penyimpan Konfigurasi, Data Pengguna)." },
  { "en": "Apa Beda Flash Dan EEPROM?", "id": "Flash (Blok Besar, Program), EEPROM (Byte Kecil, Data)." },
  { "en": "Apa Kepanjangan UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron." },
  { "en": "Sebutkan Dua Kabel Utama UART?", "id": "TX (Transmit/Mengirim) Dan RX (Receive/Menerima)." },
  { "en": "Apa Itu Laju Baud (Baud Rate)?", "id": "Kecepatan Transfer Data Serial (Bit Per Detik)." },
  { "en": "Contoh Laju Baud (Baud Rate) Umum?", "id": "Sembilan Ribu Enam Ratus (9600) Bps." },
  { "en": "Apa Itu Bit Awal (Start Bit)?", "id": "Sinyal Transisi (High Ke Low) Memulai Komunikasi UART." },
  { "en": "Apa Itu Bit Akhir (Stop Bit)?", "id": "Sinyal (Selalu High/1) Mengakhiri Komunikasi UART." },
  { "en": "Apa Itu Bit Paritas (Parity Bit)?", "id": "Bit Tambahan Untuk Pengecekan Kesalahan Sederhana." },
  { "en": "Apa Itu Komunikasi Asinkron (Asynchronous)?", "id": "Komunikasi Tanpa Sinyal Jam (Clock) Bersama." },
  { "en": "Apa Itu Komunikasi Sinkron (Synchronous)?", "id": "Komunikasi Menggunakan Sinyal Jam (Clock) Bersama." },
  { "en": "Apa Kepanjangan SPI (Serial Peripheral Interface)?", "id": "Antarmuka Periferal Serial." },
  { "en": "Apakah SPI Sinkron Atau Asinkron?", "id": "Sinkron (Menggunakan Sinyal Jam SCK)." },
  { "en": "Sebutkan Empat Kabel SPI?", "id": "MISO, MOSI, SCK, Dan SS." },
  { "en": "Apa Kepanjangan MOSI (Master Out Slave In)?", "id": "Data Keluar Dari Master, Masuk Ke Slave." },
  { "en": "Apa Kepanjangan MISO (Master In Slave Out)?", "id": "Data Keluar Dari Slave, Masuk Ke Master." },
  { "en": "Apa Fungsi Kabel SCK (Serial Clock) SPI?", "id": "Sinyal Jam Untuk Sinkronisasi Transfer Data." },
  { "en": "Apa Fungsi Kabel SS (Slave Select) SPI?", "id": "Memilih Perangkat Slave Yang Akan Diajak Bicara." },
  { "en": "Apa Kepanjangan I2C (Inter-Integrated Circuit)?", "id": "Sirkuit Antar-Terpadu." },
  { "en": "Apakah I2C Sinkron Atau Asinkron?", "id": "Sinkron (Menggunakan Sinyal Jam SCL)." },
  { "en": "Sebutkan Dua Kabel I2C?", "id": "Serial Data (SDA) Dan Serial Clock (SCL)." },
  { "en": "Apa Keunggulan I2C?", "id": "Hanya Butuh Dua Kabel Untuk Banyak Perangkat." },
  { "en": "Apa Itu Alamat I2C (I2C Address)?", "id": "Alamat Unik (7-Bit) Setiap Perangkat Slave I2C." },
  { "en": "Bagaimana Banyak Perangkat I2C Terhubung?", "id": "Paralel Di Jalur SDA Dan SCL Yang Sama." },
  { "en": "Apa Itu Kondisi Awal (Start Condition) I2C?", "id": "Transisi SDA (High Ke Low) Saat SCL High." },
  { "en": "Apa Itu Kondisi Akhir (Stop Condition) I2C?", "id": "Transisi SDA (Low Ke High) Saat SCL High." },
  { "en": "Apa Itu Bit ACK (Acknowledge) I2C?", "id": "Slave Menarik SDA Ke Low (Konfirmasi Menerima Data)." },
  { "en": "Apa Itu Bit NACK (Not Acknowledge) I2C?", "id": "Slave Membiarkan SDA High (Menolak Data/Selesai)." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Alat Merekam Dan Menganalisis Banyak Sinyal Digital." },
  { "en": "Kapan Penganalisis Logika Digunakan?", "id": "Untuk Debugging Protokol Serial (UART, I2C, SPI)." },
  { "en": "Apa Beda Osiloskop Dan Penganalisis Logika?", "id": "Osiloskop (Analog/Bentuk Gelombang), Logic Analyzer (Digital/Logika)." },
  { "en": "Apa Itu Lingkungan Pengembangan Terpadu (IDE)?", "id": "Perangkat Lunak Terpadu Untuk Menulis Kode (Editor, Compiler)." },
  { "en": "Apa Kepanjangan IDE (Integrated Development Environment)?", "id": "Lingkungan Pengembangan Terpadu." },
  { "en": "Apa Itu Debugger Perangkat Keras (Hardware Debugger)?", "id": "Alat Fisik Penghubung IDE Ke Mikrokontroler (JTAG/SWD)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Antarmuka Standar Untuk Pengujian Dan Debugging." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka Debugging Dua Kabel (Umum Di ARM)." },
  { "en": "Apa Itu Breakpoint (Titik Henti)?", "id": "Tanda Dalam Kode Untuk Menjeda (Pause) Eksekusi." },
  { "en": "Apa Fungsi Breakpoint (Titik Henti)?", "id": "Memeriksa Nilai Variabel Saat Program Berhenti." },
  { "en": "Apa Itu Peninjauan Kode (Code Review)?", "id": "Proses Memeriksa Kode Yang Ditulis Rekan Setim." },
  { "en": "Apa Itu Sistem Kontrol Versi (VCS)?", "id": "Perangkat Lunak Pelacak Perubahan Kode (Contoh: Git)." },
  { "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi Yang Populer." },
  { "en": "Apa Itu Repositori (Repository) Git?", "id": "Folder Proyek Yang Dilacak Oleh Git." },
  { "en": "Apa Itu Komit (Commit) Git?", "id": "Menyimpan Perubahan (Snapshot) Kode Ke Repositori Lokal." },
  { "en": "Apa Itu Cabang (Branch) Git?", "id": "Garis Pengembangan Paralel (Untuk Fitur Baru/Perbaikan)." },
  { "en": "Apa Itu Penggabungan (Merge) Git?", "id": "Menggabungkan Perubahan Dari Satu Cabang Ke Cabang Lain." },
  { "en": "Apa Itu Pointer (Penunjuk) Dalam C?", "id": "Variabel Yang Menyimpan Alamat Memori." },
  { "en": "Apa Fungsi Operator Asterisk (*) Pada Pointer?", "id": "Dereferensi (Mengakses Nilai Di Alamat Yang Ditunjuk)." },
  { "en": "Apa Fungsi Operator Ampersand (&) Dalam C?", "id": "Mengambil Alamat Memori Suatu Variabel." },
  { "en": "Apa Itu Pointer NULL (Nol)?", "id": "Pointer Yang Tidak Menunjuk Ke Alamat Manapun." },
  { "en": "Apa Itu Struktur (Struct) Dalam C?", "id": "Kumpulan Variabel Berbeda Tipe Dalam Satu Nama." },
  { "en": "Apa Itu Union Dalam C?", "id": "Beberapa Variabel Berbagi Lokasi Memori Yang Sama." },
  { "en": "Apa Itu Alokasi Memori Dinamis?", "id": "Meminta Memori Saat Program Berjalan (Fungsi Malloc)." },
  { "en": "Apa Itu Memori Heap?", "id": "Area Memori Tempat Alokasi Dinamis (Malloc) Terjadi." },
  { "en": "Apa Itu Memori Stack?", "id": "Area Memori Variabel Lokal Dan Panggilan Fungsi." },
  { "en": "Apa Itu Stack Overflow (Limpahan Stack)?", "id": "Penggunaan Memori Stack Melebihi Batas (Program Crash)." },
  { "en": "Apa Itu Kebocoran Memori (Memory Leak)?", "id": "Memori (Heap) Yang Dialokasikan (Malloc) Tapi Tidak Dibebaskan (Free)." },
  { "en": "Apa Itu Kata Kunci Volatile Dalam C?", "id": "Memberitahu Kompilator Nilai Variabel Bisa Berubah Tiba-Tiba." },
  { "en": "Kapan Menggunakan Volatile?", "id": "Variabel Yang Diubah Interupsi Atau Register Hardware." },
  { "en": "Apa Itu Rekursi (Recursion)?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Kasus Dasar (Base Case) Rekursi?", "id": "Kondisi Berhenti Agar Rekursi Tidak Berlanjut Selamanya." },
  { "en": "Apa Itu Pustaka (Library) Perangkat Lunak?", "id": "Kumpulan Kode (Fungsi) Yang Sudah Jadi." },
  { "en": "Apa Itu Pustaka Statis (Static Library)?", "id": "Kode Pustaka Disalin Ke Dalam Program Saat Kompilasi." },
  { "en": "Apa Itu Pustaka Dinamis (Dynamic Library/DLL/SO)?", "id": "Kode Pustaka Dimuat Saat Program Berjalan." },
  { "en": "Apa Itu Lisensi Perangkat Lunak (Software License)?", "id": "Aturan Hukum Penggunaan Dan Distribusi Perangkat Lunak." },
  { "en": "Apa Itu Lisensi GPL (GNU General Public License)?", "id": "Lisensi Perangkat Lunak Bebas (Copyleft)." },
  { "en": "Apa Itu Lisensi MIT (MIT License)?", "id": "Lisensi Perangkat Lunak Bebas Yang Sangat Permisif." },
  { "en": "Apa Itu Tes Tiga Titik (Live-Dead-Live)?", "id": "Prosedur Verifikasi Rangkaian Benar-Benar Mati (Aman)." },
  { "en": "Mengapa Tes Tiga Titik Penting?", "id": "Memastikan Alat Ukur (Voltmeter) Berfungsi Dengan Benar." },
  { "en": "Apa Langkah Pertama Tes Tiga Titik?", "id": "Tes Alat Ukur Pada Sumber Yang Pasti Hidup (Live)." },
  { "en": "Apa Langkah Kedua Tes Tiga Titik?", "id": "Tes Rangkaian Yang Akan Dikerjakan (Harus Mati/Dead)." },
  { "en": "Apa Langkah Ketiga Tes Tiga Titik?", "id": "Tes Ulang Alat Ukur Ke Sumber Hidup (Live)." },
  { "en": "Apa Itu Batas Arc Flash (Arc Flash Boundary)?", "id": "Jarak Aman Minimal Dari Bahaya Ledakan Busur Api." },
  { "en": "Apa Itu Batas Terbatas (Limited Approach Boundary)?", "id": "Batas Hanya Untuk Pekerja Listrik Yang Memenuhi Syarat." },
  { "en": "Apa Itu Batas Terlarang (Restricted Approach Boundary)?", "id": "Batas Sangat Dekat (Risiko Tinggi, Perlu APD Lengkap)." },
  { "en": "Apa Itu Kategori APD (PPE Category) Arc Flash?", "id": "Tingkatan Level Proteksi Pakaian Tahan Api." },
  { "en": "Apa Itu Energi Insiden (Incident Energy)?", "id": "Energi Panas (Kalori/Cm²) Yang Dilepaskan Saat Arc Flash." },
  { "en": "Apa Itu Peringkat Kalori (Calorie Rating) Baju?", "id": "Kemampuan Baju Menahan Energi Panas (Contoh: 8 Cal/Cm²)." },
  { "en": "Apa Itu Ufer Ground (Grounding Ufer)?", "id": "Sistem Pembumian Menggunakan Beton Fondasi Bangunan." },
  { "en": "Apa Nama Lain Ufer Ground?", "id": "Elektroda Terbungkus Beton (Concrete-Encased Electrode)." },
  { "en": "Apa Itu Ikatan (Bonding) Listrik?", "id": "Menghubungkan Bagian Logam (Non-Listrik) Menyamakan Potensial." },
  { "en": "Apa Tujuan Ikatan (Bonding) Listrik?", "id": "Mencegah Perbedaan Tegangan (Tegangan Sentuh) Antar Logam." },
  { "en": "Apa Beda Grounding Dan Bonding?", "id": "Grounding (Ke Bumi), Bonding (Antar Logam)." },
  { "en": "Apa Itu Kuncian Kabel (Cord Grip/Strain Relief)?", "id": "Mencegah Kabel Tertarik Keluar Dari Terminal." },
  { "en": "Apa Itu Paket TO-220?", "id": "Kemasan Komponen (Transistor) Tiga Kaki Tembus (Through-Hole)." },
  { "en": "Apa Itu Paket SOIC (Small Outline IC)?", "id": "Kemasan IC Pemasangan Permukaan (SMD) Seperti Sayap." },
  { "en": "Apa Itu Paket QFP (Quad Flat Package)?", "id": "Kemasan IC SMD Kaki Di Keempat Sisi." },
  { "en": "Apa Itu Paket QFN (Quad Flat No-Lead)?", "id": "Kemasan IC SMD Tanpa Kaki (Pad Di Bawah)." },
  { "en": "Apa Itu Paket SOT-23?", "id": "Kemasan SMD Kecil Umum (Tiga Atau Lima Kaki)." },
  { "en": "Bagaimana Membaca Kode Resistor SMD 103?", "id": "Sepuluh (10) Diikuti Tiga (3) Nol, (10.000 Ohm/10k)." },
  { "en": "Bagaimana Membaca Kode Resistor SMD 472?", "id": "Empat Puluh Tujuh (47) Diikuti Dua (2) Nol, (4.700 Ohm/4.7k)." },
  { "en": "Bagaimana Membaca Kode Resistor SMD 4R7?", "id": "Huruf 'R' Adalah Koma Desimal, (4.7 Ohm)." },
  { "en": "Bagaimana Membaca Kode Resistor SMD 0R10?", "id": "Huruf 'R' Adalah Koma Desimal, (0.10 Ohm)." },
  { "en": "Bagaimana Membaca Kode Resistor SMD 0 (Nol)?", "id": "Resistor Jumper (Penghubung) Nol Ohm." },
  { "en": "Apa Itu Resistor Jumper Nol Ohm?", "id": "Resistor SMD Berfungsi Sebagai Penghubung (Jumper)." },
  { "en": "Apa Itu Flux Tipe R (Rosin)?", "id": "Flux Paling Lemah (Non-Aktif), Bahan Alami Rosin." },
  { "en": "Apa Itu Flux Tipe RMA (Rosin Mildly Activated)?", "id": "Flux Rosin (Aktivasi Rendah), Umum Digunakan." },
  { "en": "Apa Itu Flux Tipe RA (Rosin Activated)?", "id": "Flux Rosin Sangat Aktif (Agresif), Harus Dibersihkan." },
  { "en": "Apa Itu Flux No-Clean (Tanpa Bersih)?", "id": "Flux Yang Residu-Nya Aman (Tidak Korosif)." },
  { "en": "Apa Itu Flux Larut Air (Water Soluble)?", "id": "Flux Sangat Aktif (Harus Dicuci Bersih Air)." },
  { "en": "Apa Itu De-Wetting (Solder)?", "id": "Timah Mundur (Tidak Menempel Rata) Dari Permukaan." },
  { "en": "Apa Itu Non-Wetting (Solder)?", "id": "Timah Gagal Menempel (Membentuk Bola) Di Permukaan." },
  { "en": "Apa Itu Ikat Rambut (Fillet) Solder?", "id": "Bentuk Solderan Ideal (Cekung Mengkilap) Di Sambungan." },
  { "en": "Apa Penyebab Solderan Dingin (Cold Joint)?", "id": "Panas Tidak Cukup Atau Pergerakan Saat Pendinginan." },
  { "en": "Apa Itu Jembatan Solder (Solder Bridge)?", "id": "Timah Menghubungkan Singkat Dua Pin Yang Berdekatan." },
  { "en": "Apa Itu Lubang Tiup (Blow Hole) Solder?", "id": "Lubang Kecil Akibat Gas Terjebak Saat Solder THT." },
  { "en": "Apa Itu Uji DLRO (Digital Low Resistance Ohmmeter)?", "id": "Mengukur Resistansi Sangat Rendah (Mikro Ohm)." },
  { "en": "Apa Fungsi Uji DLRO (Digital Low Resistance Ohmmeter)?", "id": "Memeriksa Kualitas Sambungan Busbar Atau Kontak Breaker." },
  { "en": "Mengapa Uji DLRO Menggunakan Empat Kawat (Kelvin)?", "id": "Menghilangkan Error Resistansi Kabel Uji Itu Sendiri." },
  { "en": "Apa Itu Tes Rasio Putaran Trafo (TTR)?", "id": "Memverifikasi Rasio Lilitan Trafo Sesuai Spesifikasi." },
  { "en": "Apa Kepanjangan TTR (Transformer Turns Ratio)?", "id": "Rasio Putaran Transformator." },
  { "en": "Apa Itu Uji Faktor Daya (Power Factor Test) Isolasi?", "id": "Mengukur Kualitas (Kebocoran) Isolasi Trafo Atau Kabel." },
  { "en": "Apa Itu Uji SFRA (Sweep Frequency Response Analysis)?", "id": "Mendeteksi Kerusakan Mekanis Internal Trafo (Pergeseran Lilitan)." },
  { "en": "Apa Itu Uji Resistansi Belitan (Winding Resistance)?", "id": "Mengukur Resistansi DC Lilitan Trafo Atau Motor." },
  { "en": "Apa Itu Uji DGA (Dissolved Gas Analysis) Minyak Trafo?", "id": "Menganalisis Gas Terlarut Mendeteksi Gangguan Internal." },
  { "en": "Apa Itu Uji Kekuatan Dielektrik Minyak Trafo?", "id": "Menguji Tegangan Tembus (Breakdown) Minyak Isolasi." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Sinyal Noise Kelipatan Frekuensi Dasar (50/60 Hz)." },
  { "en": "Apa Penyebab Harmonisa?", "id": "Beban Non-Linear (SMPS, VFD, Lampu LED)." },
  { "en": "Apa Akibat Buruk Harmonisa?", "id": "Memanaskan Kabel Netral, Merusak Peralatan Sensitif." },
  { "en": "Apa Itu Harmonisa Orde Ketiga (Triplen Harmonics)?", "id": "Harmonisa (150 Hz) Paling Berbahaya Bagi Netral." },
  { "en": "Mengapa Harmonisa Orde Ketiga Berbahaya?", "id": "Arus Harmonisa Tiga Fasa Dijumlahkan Di Kabel Netral." },
  { "en": "Apa Itu Filter Harmonisa (Harmonic Filter)?", "id": "Peralatan Untuk Mengurangi Atau Menghilangkan Harmonisa." },
  { "en": "Apa Itu Filter Pasif Harmonisa?", "id": "Rangkaian Resonansi (LC) Menyerap Frekuensi Harmonisa." },
  { "en": "Apa Itu Filter Aktif Harmonisa?", "id": "Elektronika Daya Menyuntikkan Arus Anti-Harmonisa." },
  { "en": "Apa Itu K-Factor Trafo?", "id": "Peringkat Trafo Tahan Panas Akibat Beban Harmonisa." },
  { "en": "Apa Kepanjangan THD (Total Harmonic Distortion)?", "id": "Total Distorsi Harmonik." },
  { "en": "Apa Itu THD-V (Total Harmonic Distortion Voltage)?", "id": "Distorsi Harmonik Pada Bentuk Gelombang Tegangan." },
  { "en": "Apa Itu THD-I (Total Harmonic Distortion Current)?", "id": "Distorsi Harmonik Pada Bentuk Gelombang Arus." },
  { "en": "Apa Itu Penganalisis Kualitas Daya (Power Quality Analyzer)?", "id": "Alat Merekam Gangguan Listrik (Sag, Swell, Harmonisa)." },
  { "en": "Apa Itu Penyesuai Daya (Power Conditioner)?", "id": "Alat Memperbaiki Kualitas Daya (Filter Noise, Stabilkan Voltase)." },
  { "en": "Apa Itu Pengatur Tegangan (Voltage Regulator/Stabilizer)?", "id": "Menjaga Tegangan Output Tetap Stabil (Konstan)." },
  { "en": "Apa Itu Inverter?", "id": "Mengubah Listrik DC Menjadi Listrik AC." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Listrik AC Menjadi Listrik DC." },
  { "en": "Apa Itu Konverter (Converter)?", "id": "Mengubah Parameter Listrik (AC-DC, DC-DC, DC-AC)." },
  { "en": "Apa Kepanjangan UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Tak Terputus (Baterai Cadangan)." },
  { "en": "Apa Fungsi Utama UPS (Uninterruptible Power Supply)?", "id": "Memberi Daya Darurat Sesaat Saat Listrik Padam." },
  { "en": "Apa Itu UPS Tipe Standby (Offline)?", "id": "UPS Pindah Ke Baterai Saat Listrik Padam (Ada Jeda)." },
  { "en": "Apa Itu UPS Tipe Line-Interactive?", "id": "UPS Standby Dengan Tambahan Stabilizer (AVR)." },
  { "en": "Apa Itu UPS Tipe Online (Double-Conversion)?", "id": "UPS Terus Menerus Mengubah (AC-DC-AC), Tanpa Jeda." },
  { "en": "Apa Keunggulan UPS Online?", "id": "Proteksi Paling Total, Output Selalu Bersih, Nol Jeda." },
  { "en": "Apa Itu Gelombang Sinus Murni (Pure Sine Wave) UPS?", "id": "Output AC Ideal, Sama Seperti PLN (Terbaik)." },
  { "en": "Apa Itu Gelombang Kotak (Square Wave) UPS?", "id": "Output AC Berbentuk Kotak (Paling Murah, Buruk)." },
  { "en": "Apa Itu Gelombang Sinus Modifikasi (Modified Sine Wave)?", "id": "Output AC (Bentuk Tangga), Di Antara Kotak Dan Sinus." },
  { "en": "Perangkat Apa Yang Butuh Sinus Murni (Pure Sine Wave)?", "id": "Motor (Kipas, Pompa), Trafo, Peralatan Audio Sensitif." },
  { "en": "Apa Satuan Kapasitas UPS?", "id": "Volt-Ampere (VA) Dan Watt (W)." },
  { "en": "Apa Beda VA Dan Watt Pada UPS?", "id": "VA (Daya Semu), Watt (Daya Aktif/Nyata)." },
  { "en": "Apa Itu Baterai VRLA (Valve Regulated Lead Acid)?", "id": "Baterai Kering (Aki Kering) Umum Untuk UPS." },
  { "en": "Apa Kepanjangan VRLA (Valve Regulated Lead Acid)?", "id": "Asam Timbal Yang Diatur Katup." },
  { "en": "Apa Tipe Baterai VRLA?", "id": "Tipe AGM (Absorbed Glass Mat) Dan Tipe Gel." },
  { "en": "Berapa Umur Baterai UPS (VRLA)?", "id": "Rata-Rata Tiga Sampai Lima Tahun." },
  { "en": "Apa Itu Generator Set (Genset)?", "id": "Mesin Pembangkit Listrik Darurat (Diesel/Bensin)." },
  { "en": "Apa Beda UPS Dan Genset?", "id": "UPS (Instan, Jangka Pendek), Genset (Butuh Waktu, Jangka Panjang)." },
  { "en": "Apa Itu Panel ATS (Automatic Transfer Switch)?", "id": "Panel Otomatis Memindah Beban Antara PLN Dan Genset." },
  { "en": "Apa Itu Panel AMF (Automatic Main Failure)?", "id": "Panel Otomatis Menghidupkan Genset Saat PLN Padam." },
  { "en": "Apa Itu Panel Sinkronisasi (Synchronizing Panel)?", "id": "Panel Untuk Menggabungkan (Paralel) Dua Genset Atau Lebih." },
  { "en": "Apa Itu Pemanas Jaket (Jacket Heater) Genset?", "id": "Pemanas Air Pendingin Mesin Diesel (Agar Siaga)." },
  { "en": "Mengapa Genset Perlu Pemanas Jaket (Jacket Heater)?", "id": "Agar Mesin Mudah Hidup (Start) Saat Dibutuhkan." },
  { "en": "Apa Itu Latihan Beban (Load Test) Genset?", "id": "Menguji Genset Dengan Beban (Dummy Load) Periodik." },
  { "en": "Mengapa Genset Perlu Latihan Beban (Load Test)?", "id": "Mencegah Penumpukan Karbon (Wet Stacking) Di Mesin." },
  { "en": "Apa Itu Wet Stacking (Penumpukan Basah)?", "id": "Kondisi Mesin Diesel Beroperasi Beban Rendah (BBM Tidak Terbakar)." },
  { "en": "Apa Itu AVR (Automatic Voltage Regulator) Genset?", "id": "Mengatur Tegangan Output Alternator (Generator) Tetap Stabil." },
  { "en": "Apa Itu Governor (Gubernur) Genset?", "id": "Mengatur Putaran Mesin (RPM) Agar Frekuensi Stabil." },
  { "en": "Berapa RPM (Rotations Per Minute) Genset 50 Hz?", "id": "Biasanya Seribu Lima Ratus (1500) RPM." },
  { "en": "Apa Itu Kunci Inggris (Adjustable Wrench)?", "id": "Kunci Pas Yang Ukuran Rahangnya Dapat Diatur." },
  { "en": "Apa Itu Kunci Pas (Open-End Wrench)?", "id": "Kunci Dengan Ujung Terbuka (Bentuk U)." },
  { "en": "Apa Itu Kunci Ring (Box-End Wrench)?", "id": "Kunci Dengan Ujung Tertutup (Bentuk Lingkaran/Heksagon)." },
  { "en": "Apa Itu Kunci Kombinasi (Combination Wrench)?", "id": "Gabungan Kunci Pas Dan Kunci Ring." },
  { "en": "Apa Itu Kunci L (Allen Key/Hex Key)?", "id": "Kunci Berbentuk L Untuk Baut Heksagon Dalam." },
  { "en": "Apa Itu Kunci Soket (Socket Wrench)?", "id": "Kunci (Mata Sok) Dengan Gagang Pemutar (Rachet)." },
  { "en": "Apa Itu Kunci Torsi (Torque Wrench)?", "id": "Kunci Pengencang Baut Dengan Ukuran Torsi Presisi." },
  { "en": "Mengapa Baut Panel Listrik Perlu Ditorsi?", "id": "Mencegah Koneksi Longgar (Panas) Atau Baut Patah." },
  { "en": "Apa Itu Tang Kupas Kabel (Wire Stripper)?", "id": "Alat Mengupas Isolasi Kabel Tanpa Memotong Kawat." },
  { "en": "Apa Itu Tang Potong (Cutting Pliers/Diagonal Cutters)?", "id": "Alat Memotong Kawat Dan Kabel Listrik." },
  { "en": "Apa Itu Tang Lancip (Needle-Nose Pliers)?", "id": "Alat Menjepit Benda Kecil Di Ruang Sempit." },
  { "en": "Apa Itu Tang Kombinasi (Combination Pliers)?", "id": "Gabungan Fungsi Tang Penjepit, Pemotong, Dan Pengunci." },
  { "en": "Apa Itu Tang Buaya (Locking Pliers/Vise-Grip)?", "id": "Tang Penjepit Yang Dapat Dikunci Pada Posisi Tertentu." },
  { "en": "Apa Itu Tang Skun (Crimping Tool)?", "id": "Alat Menjepit Skun (Konektor) Ke Ujung Kabel." },
  { "en": "Apa Itu Skun Kabel (Cable Lug)?", "id": "Konektor (Terminal) Di Ujung Kabel Untuk Sambungan." },
  { "en": "Apa Itu Ferrule (Cable Ferrule)?", "id": "Selongsong Logam Di Ujung Kabel Serabut." },
  { "en": "Apa Fungsi Ferrule (Cable Ferrule)?", "id": "Meratakan Ujung Kabel Serabut Agar Koneksi Sempurna." },
  { "en": "Apa Itu Tang Press Hidrolik (Hydraulic Crimper)?", "id": "Tang Skun Berukuran Besar Menggunakan Tenaga Hidrolik." },
  { "en": "Apa Itu Obeng Plus (Phillips Screwdriver)?", "id": "Obeng Untuk Sekrup Beralur Silang (+)." },
  { "en": "Apa Itu Obeng Minus (Flat/Slotted Screwdriver)?", "id": "Obeng Untuk Sekrup Beralur Lurus (-)." },
  { "en": "Apa Itu Obeng Tumbuk (Impact Driver)?", "id": "Obeng Membuka Sekrup Keras (Dipukul Martil)." },
  { "en": "Apa Itu Bor Tangan (Hand Drill)?", "id": "Alat Membuat Lubang Menggunakan Tenaga Listrik (Baterai/Kabel)." },
  { "en": "Apa Itu Mata Bor (Drill Bit)?", "id": "Ujung Tajam Pada Bor Untuk Melubangi Material." },
  { "en": "Apa Itu Gerinda Tangan (Angle Grinder)?", "id": "Alat Memotong Atau Mengikis Material (Logam, Keramik)." },
  { "en": "Apa Itu Martil (Hammer)?", "id": "Alat Pemukul (Palu) Untuk Memaku Atau Menumbuk." },
  { "en": "Apa Itu Gergaji Besi (Hacksaw)?", "id": "Gergaji Manual Untuk Memotong Benda Logam." },
  { "en": "Apa Itu Gunting Kabel (Cable Cutter)?", "id": "Gunting Besar Khusus Memotong Kabel Ukuran Besar." },
  { "en": "Apa Itu Pisau Kabel (Cable Knife)?", "id": "Pisau Khusus Mengupas Isolasi Kabel (Bukan Cutter)." },
  { "en": "Apa Itu Solder Uap (Hot Air/Blower)?", "id": "Alat Melepas Komponen SMD Menggunakan Udara Panas." },
  { "en": "Apa Itu Atraktor Solder (Solder Sucker/Desoldering Pump)?", "id": "Alat Menyedot Timah Solder Cair (Vakum)." },
  { "en": "Apa Itu Sumbu Solder (Solder Wick/Braid)?", "id": "Jalinan Tembaga Menyerap Sisa Timah Solder." },
  { "en": "Apa Itu Pasta Solder (Flux Paste)?", "id": "Membantu Timah Menempel Sempurna (Membersihkan Oksida)." },
  { "en": "Apa Itu Timah Solder (Soldering Lead)?", "id": "Logam Paduan (Timah, Timbal) Untuk Menyambung Komponen." },
  { "en": "Apa Beda Timah 60/40 Dan 63/37?", "id": "Rasio Campuran Timah (Sn) Dan Timbal (Pb)." },
  { "en": "Apa Itu Timah Bebas Timbal (Lead-Free Solder)?", "id": "Timah Solder Tanpa Kandungan Timbal (Pb)." },
  { "en": "Mengapa Titik Leleh Lead-Free Lebih Tinggi?", "id": "Karena Tidak Menggunakan Timbal (Pb) Sebagai Campuran." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Cetak (Tempat Komponen Elektronik)." },
  { "en": "Apa Itu Komponen THT (Through-Hole Technology)?", "id": "Komponen Dengan Kaki Yang Menembus Lubang PCB." },
  { "en": "Apa Itu Komponen SMD (Surface-Mount Device)?", "id": "Komponen Yang Dipasang Di Permukaan PCB (Tanpa Lubang)." },
  { "en": "Apa Kepanjangan THT (Through-Hole Technology)?", "id": "Teknologi Tembus Lubang." },
  { "en": "Apa Kepanjangan SMD (Surface-Mount Device)?", "id": "Perangkat Pemasangan Permukaan." },
  { "en": "Apa Keunggulan Komponen SMD?", "id": "Ukuran Sangat Kecil, Perakitan Otomatis Cepat." },
  { "en": "Apa Itu PCB Satu Sisi (Single-Sided)?", "id": "PCB Dengan Jalur Tembaga Hanya Di Satu Sisi." },
  { "en": "Apa Itu PCB Dua Sisi (Double-Sided)?", "id": "PCB Dengan Jalur Tembaga Di Kedua Sisi." },
  { "en": "Apa Itu PCB Multilayer (Multi-Lapisan)?", "id": "PCB Dengan Banyak Lapisan Jalur Tembaga (Lebih 2)." },
  { "en": "Apa Itu Via (Via) Pada PCB?", "id": "Lubang Penghubung Jalur Antar Lapisan PCB." },
  { "en": "Apa Itu Jalur (Trace) PCB?", "id": "Jalur Tembaga Konduktif Penghubung Antar Komponen." },
  { "en": "Apa Itu Pad (Pad) PCB?", "id": "Titik Logam Tempat Kaki Komponen Disolder." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung (Hijau) Mencegah Korsleting Solder." },
  { "en": "Apa Itu Silkscreen (Sablon) PCB?", "id": "Tinta Tanda (Label) Komponen Di Atas Solder Mask." },
  { "en": "Apa Itu Etsa (Etching) PCB?", "id": "Proses Melarutkan Tembaga Yang Tidak Diinginkan." },
  { "en": "Apa Bahan Etsa (Etching) PCB?", "id": "Ferri Klorida (FeCl3) Atau Larutan Asam Kuat." },
  { "en": "Apa Itu File Gerber?", "id": "Format File Standar Untuk Manufaktur (Cetak) PCB." },
  { "en": "Apa Itu Desain Skematik (Schematic Design)?", "id": "Gambar Diagram Rangkaian Elektronik (Simbol)." },
  { "en": "Apa Itu Desain Tata Letak (Layout Design) PCB?", "id": "Desain Posisi Komponen Dan Jalur Fisik PCB." },
  { "en": "Apa Itu Perangkat Lunak Desain PCB (PCB Design Software)?", "id": "Contoh: Altium Designer, Eagle, KiCad." },
  { "en": "Apa Itu Jejak Kaki (Footprint) Komponen?", "id": "Pola Pad (Ukuran Fisik) Komponen Di Desain PCB." },
  { "en": "Apa Itu Pustaka (Library) PCB?", "id": "Kumpulan Simbol Skematik Dan Jejak Kaki (Footprint)." },
  { "en": "Apa Itu Pengecekan Aturan Desain (DRC)?", "id": "Pengecekan Otomatis (Software) Aturan Desain PCB." },
  { "en": "Apa Kepanjangan DRC (Design Rule Check)?", "id": "Pengecekan Aturan Desain." },
  { "en": "Apa Itu Jarak Bebas (Clearance) PCB?", "id": "Jarak Minimum Antar Jalur Atau Antar Pad." },
  { "en": "Apa Itu Lebar Jalur (Trace Width) PCB?", "id": "Lebar Jalur Tembaga (Menentukan Arus Maksimum)." },
  { "en": "Semakin Besar Arus, Jalur Harus Semakin?", "id": "Lebar (Wide)." },
  { "en": "Apa Itu Panelisasi (Panelization) PCB?", "id": "Menggabungkan Banyak PCB Kecil Dalam Satu Papan Besar." },
  { "en": "Apa Itu V-Cut (V-Score)?", "id": "Potongan Alur 'V' Untuk Memisahkan PCB Panel." },
  { "en": "Apa Itu Mouse Bites (Gigitan Tikus)?", "id": "Lubang-Lubang Bor Kecil Untuk Memisahkan PCB Panel." },
  { "en": "Apa Itu Tenting (Tenting) Via?", "id": "Menutup Lubang Via Sepenuhnya Dengan Solder Mask." },
  { "en": "Apa Itu Cincin Anular (Annular Ring)?", "id": "Area Tembaga Sisa Di Sekitar Lubang Via." },
  { "en": "Apa Itu Finishing Permukaan (Surface Finish) PCB?", "id": "Lapisan Pelindung Pad (HASL, ENIG, OSP)." },
  { "en": "Apa Kepanjangan HASL (Hot Air Solder Leveling)?", "id": "Perataan Solder Udara Panas (Finishing Timah)." },
  { "en": "Apa Kepanjangan ENIG (Electroless Nickel Immersion Gold)?", "id": "Nikel Tanpa Listrik Celup Emas (Finishing Emas)." },
  { "en": "Apa Kepanjangan OSP (Organic Solderability Preservatives)?", "id": "Pengawet Solderabilitas Organik (Lapisan Organik Tipis)." },
  { "en": "Apa Keunggulan ENIG (Electroless Nickel Immersion Gold)?", "id": "Permukaan Sangat Datar (Rata), Baik Untuk BGA." },
  { "en": "Apa Itu Stensil SMT (SMT Stencil)?", "id": "Cetakan Logam Tipis Berlubang Untuk Pasta Solder." },
  { "en": "Apa Itu Pasta Solder (Solder Paste)?", "id": "Campuran Bubuk Timah Dan Flux (Seperti Krim)." },
  { "en": "Apa Itu Mesin Pick-And-Place?", "id": "Mesin Otomatis Memasang Komponen SMD Ke PCB." },
  { "en": "Apa Itu Oven Reflow (Reflow Oven)?", "id": "Oven Pemanas Untuk Melelehkan Pasta Solder." },
  { "en": "Apa Itu Profil Reflow (Reflow Profile)?", "id": "Kurva Suhu (Pemanasan, Leleh, Pendinginan) Oven Reflow." },
  { "en": "Apa Itu Solder Gelombang (Wave Soldering)?", "id": "Metode Solder Komponen THT (PCB Dilewatkan Gelombang Timah)." },
  { "en": "Apa Itu Solder Selektif (Selective Soldering)?", "id": "Menyolder Titik THT Tertentu Secara Otomatis (Nosel)." },
  { "en": "Apa Itu AOI (Automatic Optical Inspection)?", "id": "Inspeksi Visual PCB Otomatis Menggunakan Kamera." },
  { "en": "Apa Itu AXI (Automatic X-ray Inspection)?", "id": "Inspeksi Sinar-X (Melihat Sambungan Di Bawah BGA)." },
  { "en": "Apa Kepanjangan BGA (Ball Grid Array)?", "id": "Kemasan IC Dengan Bola Timah Di Bawahnya." },
  { "en": "Apa Itu Uji In-Circuit (ICT)?", "id": "Pengujian PCB Menggunakan Jarum Uji (Bed-of-Nails)." },
  { "en": "Apa Itu Uji Flying Probe (FPT)?", "id": "Pengujian PCB Menggunakan Probe Jarum Bergerak (Robotik)." },
  { "en": "Apa Itu Uji Fungsional (FCT)?", "id": "Pengujian Fungsi Akhir Papan Sirkuit (PCBA)." },
  { "en": "Apa Itu Titik Uji (Test Point)?", "id": "Titik Pad Khusus Di PCB Untuk Pengujian (ICT/FPT)." },
  { "en": "Apa Itu Pekerjaan Ulang (Rework) PCB?", "id": "Proses Perbaikan Manual (Melepas, Memasang Ulang) Komponen." },
  { "en": "Apa Itu Conformal Coating (Pelapisan Konformal)?", "id": "Lapisan Pelindung Tipis (Transparan) Pada PCB." },
  { "en": "Apa Fungsi Conformal Coating?", "id": "Melindungi PCB Dari Kelembaban, Debu, Dan Korosi." },
  { "en": "Apa Itu Potting (Pengecoran) Elektronik?", "id": "Mengisi Seluruh Casing Elektronik Dengan Resin Epoksi." },
  { "en": "Apa Fungsi Potting (Pengecoran)?", "id": "Perlindungan Total (Tahan Air, Getaran, Rahasia Desain)." },
  { "en": "Apa Itu Pendingin (Heat Sink)?", "id": "Logam (Aluminium) Penyerap Dan Pembuang Panas Komponen." },
  { "en": "Apa Fungsi Pendingin (Heat Sink)?", "id": "Menjaga Suhu Komponen (Transistor, CPU) Tetap Dingin." },
  { "en": "Apa Itu Pasta Termal (Thermal Paste)?", "id": "Bahan Penghantar Panas (Antara Chip Dan Pendingin)." },
  { "en": "Apa Fungsi Pasta Termal (Thermal Paste)?", "id": "Menutup Celah Udara Mikro, Transfer Panas Maksimal." },
  { "en": "Apa Itu Bantalan Termal (Thermal Pad)?", "id": "Bantalan Silikon Penghantar Panas (Pengganti Pasta)." },
  { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Pipa Tembaga (Isi Cairan) Transfer Panas Efisien." },
  { "en": "Apa Itu Pendingin Cair (Liquid Cooling)?", "id": "Sistem Pendingin Menggunakan Sirkulasi Cairan (Air)." },
  { "en": "Apa Itu Elemen Peltier (Peltier Cooler)?", "id": "Pendingin Termoelektrik (Memompa Panas Saat Dialiri Listrik)." },
  { "en": "Apa Itu Desain Untuk Manufaktur (DFM)?", "id": "Desain Produk Agar Mudah Diproduksi Massal." },
  { "en": "Apa Itu Desain Untuk Perakitan (DFA)?", "id": "Desain Produk Agar Mudah Dirakit (Manual/Otomatis)." },
  { "en": "Apa Itu Desain Untuk Pengujian (DFT)?", "id": "Desain Produk Agar Mudah Diuji (Test Point)." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Tegangan Maksimum Yang Dapat Ditahan Isolator." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant)?", "id": "Kemampuan Bahan Menyimpan Energi Listrik (Kapasitor)." },
  { "en": "Apa Itu Jarak Rambat (Creepage)?", "id": "Jarak Terpendek Antar Konduktor Sepanjang Permukaan Isolator." },
  { "en": "Apa Itu Jarak Bebas (Clearance)?", "id": "Jarak Terpendek Antar Konduktor Melalui Udara." },
  { "en": "Apa Itu Helm Pengaman (Safety Helmet)?", "id": "Pelindung Kepala Dari Benturan Benda Keras." },
  { "en": "Apa Itu Pelindung Wajah (Face Shield)?", "id": "Melindungi Wajah Dari Percikan Api Atau Cairan." },
  { "en": "Apa Itu Pelindung Telinga (Ear Protection)?", "id": "Mengurangi Intensitas Kebisingan Suara Tinggi." },
  { "en": "Apa Itu Ear Muff (Penutup Telinga)?", "id": "Tipe Pelindung Telinga Menutupi Seluruh Telinga." },
  { "en": "Apa Itu Ear Plug (Sumbat Telinga)?", "id": "Tipe Pelindung Telinga Dimasukkan Saluran Telinga." },
  { "en": "Apa Itu Ujung Baja (Steel Toe) Sepatu?", "id": "Bagian Logam Keras Pelindung Jari Kaki." },
  { "en": "Apa Itu Kacamata Pengaman (Safety Goggles)?", "id": "Melindungi Mata Dari Debu Dan Percikan Cairan." },
  { "en": "Apa Itu Sarung Tangan Isolasi (Insulating Gloves)?", "id": "Sarung Tangan Karet Khusus Tahan Tegangan Listrik." },
  { "en": "Apa Itu Pelindung Kulit (Leather Protector) Sarung Tangan?", "id": "Dipakai Di Luar Sarung Tangan Isolasi (Proteksi Karet)." },
  { "en": "Kapan Sarung Tangan Isolasi Harus Diuji?", "id": "Secara Periodik (Contoh: Setiap Enam Bulan Sekali)." },
  { "en": "Apa Itu Simulasi Rangkaian (Circuit Simulation)?", "id": "Menguji Rangkaian Elektronik Menggunakan Perangkat Lunak." },
  { "en": "Apa Kepanjangan SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Program Simulasi Dengan Penekanan Sirkuit Terpadu." },
  { "en": "Apa Itu LTspice?", "id": "Perangkat Lunak SPICE Gratis Dari Analog Devices." },
  { "en": "Apa Itu Proteus Design Suite?", "id": "Perangkat Lunak Desain (Skematik, PCB, Simulasi MCU)." },
  { "en": "Apa Kepanjangan MATLAB (Matrix Laboratory)?", "id": "Laboratorium Matriks (Perangkat Lunak Komputasi Teknis)." },
  { "en": "Apa Itu Simulink?", "id": "Lingkungan Simulasi Grafis (Blok Diagram) Di MATLAB." },
  { "en": "Apa Itu LabVIEW (Laboratory Virtual Instrument Engineering Workbench)?", "id": "Perangkat Lunak Desain Grafis (Visual) Instrumentasi." },
  { "en": "Apa Itu Pemrograman Grafis (Graphical Programming)?", "id": "Menyusun Blok Diagram Fungsional (Bukan Teks Kode)." },
  { "en": "Apa Itu AutoCAD Electrical?", "id": "Perangkat Lunak CAD Untuk Desain Skematik Kontrol Listrik." },
  { "en": "Apa Itu EPLAN Electric P8?", "id": "Perangkat Lunak CAE (Computer-Aided Engineering) Desain Proyek Listrik." },
  { "en": "Apa Itu LCR Meter?", "id": "Alat Ukur Induktansi (L), Kapasitansi (C), Resistansi (R)." },
  { "en": "Apa Beda LCR Meter Dan Multimeter?", "id": "LCR Meter Jauh Lebih Akurat Untuk Komponen Pasif." },
  { "en": "Apa Itu Frekuensi Uji (Test Frequency) LCR Meter?", "id": "Frekuensi AC Yang Digunakan Saat Pengukuran." },
  { "en": "Mengapa Frekuensi Uji LCR Meter Penting?", "id": "Nilai Komponen (L, C) Berubah Terhadap Frekuensi." },
  { "en": "Apa Itu Faktor Disipasi (Dissipation Factor/DF) Kapasitor?", "id": "Ukuran Kerugian (Ketidakidealan) Kapasitor (Diukur LCR)." },
  { "en": "Apa Itu Faktor Kualitas (Quality Factor/Q) Induktor?", "id": "Ukuran Efisiensi (Ideal) Induktor (Diukur LCR)." },
  { "en": "Apa Itu Pengukuran Empat Titik (Four-Wire Kelvin)?", "id": "Metode Pengukuran Resistansi Rendah (Menghilangkan Error Kabel)." },
  { "en": "Apa Kepanjangan SMU (Source Measure Unit)?", "id": "Unit Sumber Pengukur." },
  { "en": "Apa Fungsi SMU (Source Measure Unit)?", "id": "Sumber Dan Pengukur Presisi (Karakterisasi Komponen)." },
  { "en": "Apa Itu Kurva I-V (Arus-Tegangan)?", "id": "Grafik Hubungan Arus Terhadap Tegangan Suatu Komponen." },
  { "en": "Apa Itu Muatan Listrik (Electric Charge)?", "id": "Sifat Fisik Dasar Materi (Positif Atau Negatif)." },
  { "en": "Apa Satuan Muatan Listrik?", "id": "Coulomb (C)." },
  { "en": "Apa Itu Elektron (Electron)?", "id": "Partikel Subatom Pembawa Muatan Listrik Negatif." },
  { "en": "Apa Itu Proton (Proton)?", "id": "Partikel Subatom Pembawa Muatan Listrik Positif." },
  { "en": "Apa Itu Hukum Coulomb (Coulomb's Law)?", "id": "Gaya Tarik Atau Tolak Antara Dua Muatan Listrik." },
  { "en": "Apa Itu Medan Listrik (Electric Field)?", "id": "Area Di Sekitar Muatan (Mempengaruhi Muatan Lain)." },
  { "en": "Apa Satuan Medan Listrik?", "id": "Volt Per Meter (V/m) Atau Newton Per Coulomb." },
  { "en": "Apa Itu Potensial Listrik (Electric Potential)?", "id": "Energi Potensial Per Satuan Muatan (Tegangan)." },
  { "en": "Apa Itu Kapasitansi (Capacitance)?", "id": "Kemampuan Menyimpan Muatan Listrik (Dalam Medan Listrik)." },
  { "en": "Apa Itu Dielektrik (Dielectric)?", "id": "Bahan Isolator Di Antara Pelat Kapasitor." },
  { "en": "Apa Fungsi Dielektrik (Dielectric)?", "id": "Meningkatkan Nilai Kapasitansi Kapasitor." },
  { "en": "Apa Itu Permitivitas (Permittivity)?", "id": "Ukuran Kemampuan Dielektrik Menahan Medan Listrik." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field)?", "id": "Medan Gaya Akibat Gerakan Muatan (Arus Listrik)." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Perubahan Medan Magnet Menghasilkan Tegangan Listrik." },
  { "en": "Siapa Penemu Induksi Elektromagnetik?", "id": "Michael Faraday (Hukum Faraday)." },
  { "en": "Apa Itu Hukum Lenz (Lenz's Law)?", "id": "Arus Induksi Melawan Arah Perubahan Penyebabnya." },
  { "en": "Apa Itu Induktansi (Inductance)?", "id": "Kemampuan Komponen Menghasilkan Tegangan (Akibat Perubahan Arus)." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik?", "id": "Kemampuan Bahan Mendukung Pembentukan Medan Magnet." },
  { "en": "Apa Itu Arus Searah (Direct Current/DC)?", "id": "Arus Listrik Mengalir Dalam Satu Arah Tetap." },
  { "en": "Apa Sumber Listrik DC (Direct Current)?", "id": "Baterai, Panel Surya, Catu Daya DC." },
  { "en": "Apa Itu Arus Bolak-Balik (Alternating Current/AC)?", "id": "Arus Listrik Arahnya Berubah Bolak-Balik (Periodik)." },
  { "en": "Apa Bentuk Gelombang AC Standar?", "id": "Gelombang Sinusoidal (Sinus Wave)." },
  { "en": "Apa Itu Periode (Period) Gelombang AC?", "id": "Waktu Untuk Menyelesaikan Satu Siklus Penuh." },
  { "en": "Apa Itu Frekuensi (Frequency) Gelombang AC?", "id": "Jumlah Siklus Lengkap Per Detik (Satuan Hertz)." },
  { "en": "Apa Itu Amplitudo (Amplitude) Gelombang AC?", "id": "Nilai Puncak (Maksimum) Dari Gelombang Sinus." },
  { "en": "Apa Itu Nilai Puncak-Ke-Puncak (Peak-to-Peak) AC?", "id": "Nilai Dari Puncak Positif Hingga Puncak Negatif." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif AC (Setara Dengan Nilai DC)." },
  { "en": "Apa Hubungan RMS Dan Puncak (Peak) Sinus?", "id": "Nilai RMS Adalah Puncak Dibagi Akar Dua." },
  { "en": "Apa Itu Fasa (Phase) Gelombang AC?", "id": "Pergeseran Waktu Relatif Gelombang (Dalam Derajat)." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Murni Terhadap Aliran Arus (Menjadi Panas)." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Akibat Kapasitor Atau Induktor (Menyimpan Energi)." },
  { "en": "Apa Itu Reaktansi Induktif (XL)?", "id": "Hambatan Induktor (Naik Seiring Kenaikan Frekuensi)." },
  { "en": "Apa Itu Reaktansi Kapasitif (XC)?", "id": "Hambatan Kapasitor (Turun Seiring Kenaikan Frekuensi)." },
  { "en": "Apa Itu Impedansi (Impedance/Z)?", "id": "Hambatan Total Rangkaian AC (Gabungan R, XL, XC)." },
  { "en": "Apa Satuan Impedansi, Resistansi, Reaktansi?", "id": "Semuanya Menggunakan Satuan Ohm (Ω)." },
  { "en": "Apa Itu Rangkaian Resonansi (Resonant Circuit)?", "id": "Kondisi Dimana Reaktansi Induktif Sama Dengan Kapasitif (XL=XC)." },
  { "en": "Apa Itu Frekuensi Resonansi (Resonant Frequency)?", "id": "Frekuensi Unik Dimana Terjadi Resonansi (XL=XC)." },
  { "en": "Apa Itu Filter Lolos Bawah (Low Pass Filter)?", "id": "Meneruskan Frekuensi Rendah, Menghambat Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Lolos Atas (High Pass Filter)?", "id": "Meneruskan Frekuensi Tinggi, Menghambat Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Pita (Band Pass Filter)?", "id": "Hanya Meneruskan Rentang Frekuensi Tertentu (Pita)." },
  { "en": "Apa Itu Filter Tolak Pita (Band Stop/Notch Filter)?", "id": "Menghambat Rentang Frekuensi Tertentu (Pita)." },
  { "en": "Apa Itu Rangkaian RC (Resistor-Capacitor)?", "id": "Rangkaian Kombinasi Resistor Dan Kapasitor (Filter, Waktu)." },
  { "en": "Apa Itu Rangkaian RL (Resistor-Inductor)?", "id": "Rangkaian Kombinasi Resistor Dan Induktor (Filter)." },
  { "en": "Apa Itu Rangkaian RLC (Resistor-Inductor-Capacitor)?", "id": "Rangkaian Kombinasi Resistor, Induktor, Kapasitor (Resonansi)." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RC?", "id": "Waktu Pengisian Atau Pengosongan Kapasitor (Tau = R x C)." },
  { "en": "Apa Itu Energi Listrik (Electrical Energy)?", "id": "Daya Listrik Yang Digunakan Selama Waktu Tertentu." },
  { "en": "Apa Satuan Energi Listrik (Konsumsi)?", "id": "Watt-Jam (Watt-Hour) Atau KiloWatt-Jam (kWh)." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Yang Mudah Menghantarkan Arus Listrik (Elektron Bebas)." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Yang Sulit Menghantarkan Arus Listrik (Dielektrik)." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor Dan Isolator (Silikon)." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Menambah Zat Asing (Impuritas) Mengubah Konduktivitas." },
  { "en": "Apa Itu Semikonduktor Tipe-N (N-Type)?", "id": "Kelebihan Elektron (Donor, Muatan Negatif)." },
  { "en": "Apa Itu Semikonduktor Tipe-P (P-Type)?", "id": "Kekurangan Elektron (Akseptor, Muatan Positif/Hole)." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Pertemuan Semikonduktor Tipe P Dan Tipe N (Dioda)." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Kosong (Tanpa Muatan) Di Sekitar Sambungan P-N." },
  { "en": "Apa Itu Tegangan Maju (Forward Bias) Dioda?", "id": "Tegangan Positif Ke P, Negatif Ke N (Dioda Menghantar)." },
  { "en": "Apa Itu Tegangan Mundur (Reverse Bias) Dioda?", "id": "Tegangan Negatif Ke P, Positif Ke N (Dioda Menghambat)." },
  { "en": "Apa Itu Arus Bocor (Leakage Current) Dioda?", "id": "Arus Sangat Kecil Saat Dioda Diberi Tegangan Mundur." },
  { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage) Dioda?", "id": "Tegangan Mundur Terlalu Tinggi (Dioda Rusak/Menghantar)." },
  { "en": "Apa Itu Gelang Anti-Statis (ESD Wrist Strap)?", "id": "Alat Keselamatan Mencegah Kerusakan Komponen Akibat ESD." },
  { "en": "Apa Itu Matras Anti-Statis (ESD Mat)?", "id": "Alas Meja Kerja Mencegah Listrik Statis." },
  { "en": "Apa Itu Kemasan Anti-Statis (ESD Bag)?", "id": "Kantong Khusus (Perak/Pink) Melindungi Komponen Sensitif." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Ruangan Produksi Sangat Bersih (Pembuatan Chip/IC)." },
  { "en": "Apa Itu Wafer Silikon (Silicon Wafer)?", "id": "Kepingan Tipis Silikon Murni (Bahan Dasar IC)." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit Ke Wafer (Sinar UV)." },
  { "en": "Apa Itu Dicing (Pemotongan) Wafer?", "id": "Proses Memotong Wafer Menjadi Chip (Die) Individual." },
  { "en": "Apa Itu Ikatan Kawat (Wire Bonding)?", "id": "Menyambung Kawat Emas Halus Dari Chip Ke Kemasan." },
  { "en": "Apa Itu Prosedur Lockout/Tagout (LOTO)?", "id": "Prosedur Penguncian Dan Pelabelan Sumber Energi." },
  { "en": "Apa Tujuan Utama LOTO (Lockout/Tagout)?", "id": "Mencegah Energi Lepas (Kecelakaan) Saat Perawatan." },
  { "en": "Apa Itu Gembok (Padlock) LOTO?", "id": "Gembok Khusus (Biasanya Merah) Untuk Mengunci Isolasi." },
  { "en": "Apa Itu Tag (Label) LOTO?", "id": "Label Peringatan (Jangan Operasikan) Terpasang Di Gembok." },
  { "en": "Siapa Yang Boleh Melepas Gembok LOTO?", "id": "Hanya Orang Yang Memasang Gembok Tersebut." },
  { "en": "Apa Itu Lockout Hasp (Pengait LOTO)?", "id": "Alat Memungkinkan Banyak Gembok Di Satu Titik Isolasi." },
  { "en": "Apa Itu Titik Isolasi (Isolation Point)?", "id": "Perangkat Pemutus Energi (Contoh: MCB, Valve)." },
  { "en": "Apa Itu Zero Energy State (Kondisi Energi Nol)?", "id": "Kondisi Dimana Semua Energi (Listrik, Mekanis) Hilang." },
  { "en": "Kapan LOTO Harus Dilakukan?", "id": "Setiap Kali Melakukan Perawatan Atau Perbaikan Mesin." },
  { "en": "Apa Itu Energi Tersimpan (Stored Energy)?", "id": "Energi Sisa (Kapasitor, Pegas) Setelah Isolasi." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Proses Membandingkan Alat Ukur Dengan Standar." },
  { "en": "Mengapa Kalibrasi Penting?", "id": "Menjamin Keakuratan Dan Keandalan Hasil Pengukuran." },
  { "en": "Apa Itu Standar Kalibrasi (Calibration Standard)?", "id": "Alat Referensi Yang Diketahui Sangat Akurat." },
  { "en": "Apa Itu Ketertelusuran (Traceability) Kalibrasi?", "id": "Alat Ukur Terhubung Ke Standar Nasional/Internasional." },
  { "en": "Apa Itu Sertifikat Kalibrasi (Calibration Certificate)?", "id": "Dokumen Resmi Hasil Dan Status Kalibrasi." },
  { "en": "Apa Itu Rentang (Range) Alat Ukur?", "id": "Batas Minimum Dan Maksimum Pengukuran Alat." },
  { "en": "Apa Itu Akurasi (Accuracy) Alat Ukur?", "id": "Seberapa Dekat Hasil Ukur Dengan Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision) Alat Ukur?", "id": "Seberapa Dekat Hasil Ukur Yang Berulang (Konsisten)." },
  { "en": "Apa Itu Resolusi (Resolution) Alat Ukur?", "id": "Perubahan Terkecil Yang Dapat Dideteksi Alat Ukur." },
  { "en": "Apa Itu Zero Adjustment (Penyetelan Nol)?", "id": "Mengatur Ulang Alat Ukur Ke Titik Nol (0)." },
  { "en": "Apa Itu Span Adjustment (Penyetelan Rentang)?", "id": "Mengatur Ulang Skala Penuh Alat Ukur (Nilai Maksimal)." },
  { "en": "Apa Itu Kesalahan (Error) Paralaks?", "id": "Kesalahan Membaca Akibat Sudut Pandang (Meter Analog)." },
  { "en": "Apa Itu Kalibrator Loop (Loop Calibrator)?", "id": "Alat Uji (Simulasi, Ukur) Sinyal Loop 4-20 MA." },
  { "en": "Apa Itu Kalibrator Tekanan (Pressure Calibrator)?", "id": "Alat Menghasilkan Tekanan Akurat Untuk Uji Sensor." },
  { "en": "Apa Itu Kalibrator Suhu (Temperature Calibrator/Dry Block)?", "id": "Sumber Suhu Presisi Untuk Uji Termokopel/RTD." },
  { "en": "Apa Itu Sel Surya (Solar Cell/Photovoltaic Cell)?", "id": "Mengubah Energi Cahaya Matahari Menjadi Listrik DC." },
  { "en": "Apa Bahan Dasar Sel Surya (Solar Cell)?", "id": "Silikon (Si) Tipe Monokristalin Atau Polikristalin." },
  { "en": "Apa Itu Panel Surya (Solar Panel/Module)?", "id": "Kumpulan Sel Surya Yang Dirangkai Seri/Paralel." },
  { "en": "Apa Itu Array Surya (Solar Array)?", "id": "Kumpulan Panel Surya Yang Digabung (Instalasi Besar)." },
  { "en": "Apa Itu Tegangan Rangkaian Terbuka (Voc) Panel Surya?", "id": "Tegangan Maksimum Panel Surya Saat Tanpa Beban." },
  { "en": "Apa Itu Arus Hubung Singkat (Isc) Panel Surya?", "id": "Arus Maksimum Panel Surya Saat Hubung Singkat." },
  { "en": "Apa Itu Titik Daya Maksimum (MPP) Panel Surya?", "id": "Titik Operasi (V x I) Daya Output Maksimal." },
  { "en": "Apa Kepanjangan MPPT (Maximum Power Point Tracking)?", "id": "Pelacakan Titik Daya Maksimum." },
  { "en": "Apa Fungsi MPPT (Maximum Power Point Tracking) Charge Controller?", "id": "Menjaga Panel Surya Bekerja Di Titik Efisiensi Tertinggi." },
  { "en": "Apa Beda MPPT Dan PWM (Pulse Width Modulation) Charge Controller?", "id": "MPPT Jauh Lebih Efisien Dibanding PWM." },
  { "en": "Apa Itu Inverter Surya (Solar Inverter)?", "id": "Mengubah Listrik DC Panel Surya Menjadi AC (PLN)." },
  { "en": "Apa Itu Sistem On-Grid (Grid-Tied)?", "id": "Sistem Panel Surya Terhubung Ke Jaringan Listrik PLN." },
  { "en": "Apa Itu Sistem Off-Grid (Stand-Alone)?", "id": "Sistem Panel Surya Mandiri (Menggunakan Baterai)." },
  { "en": "Apa Itu Sistem Hibrida (Hybrid System) Surya?", "id": "Gabungan On-Grid Dan Off-Grid (PLN + Baterai)." },
  { "en": "Apa Itu Net Metering (Metering Ekspor-Impor)?", "id": "Sistem Menghitung Kelebihan Listrik Surya (Ekspor Ke PLN)." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mengubah Energi Kinetik Angin Menjadi Listrik AC/DC." },
  { "en": "Apa Itu Bilah (Blade) Turbin Angin?", "id": "Baling-Baling Yang Menangkap Energi Angin." },
  { "en": "Apa Itu Nacelle (Nacelle) Turbin Angin?", "id": "Rumah (Casing) Generator Dan Gearbox Di Atas Menara." },
  { "en": "Apa Itu Generator (Generator) Turbin Angin?", "id": "Mengubah Putaran Mekanis Menjadi Energi Listrik." },
  { "en": "Apa Itu Gearbox (Gearbox) Turbin Angin?", "id": "Meningkatkan Kecepatan Putaran Lambat Bilah Ke Generator." },
  { "en": "Apa Itu Kontrol Pitch (Pitch Control) Bilah?", "id": "Mengatur Sudut Kemiringan Bilah (Kontrol Daya/Kecepatan)." },
  { "en": "Apa Itu Kontrol Yaw (Yaw Control) Turbin?", "id": "Menggerakkan Nacelle Menghadap Arah Angin." },
  { "en": "Apa Itu Anemometer (Anemometer)?", "id": "Alat Pengukur Kecepatan Angin (Data Kontrol Turbin)." },
  { "en": "Apa Itu Kecepatan Cut-In (Cut-In Speed) Turbin?", "id": "Kecepatan Angin Minimum Mulai Menghasilkan Listrik." },
  { "en": "Apa Itu Kecepatan Cut-Out (Cut-Out Speed) Turbin?", "id": "Kecepatan Angin Maksimum (Turbin Berhenti/Aman)." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Semikonduktor (Dioda, MOSFET) Mengontrol Daya Listrik." },
  { "en": "Apa Itu Konverter AC Ke DC (Penyearah)?", "id": "Mengubah AC Menjadi DC (Contoh: Adaptor Laptop)." },
  { "en": "Apa Itu Konverter DC Ke AC (Inverter)?", "id": "Mengubah DC (Baterai) Menjadi AC (Jala-Jala)." },
  { "en": "Apa Itu Konverter DC Ke DC (DC-DC Converter)?", "id": "Mengubah Tegangan DC Ke Tegangan DC Lain." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Konverter DC-DC Penurun Tegangan (Step-Down)." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Konverter DC-DC Penaik Tegangan (Step-Up)." },
  { "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?", "id": "Konverter DC-DC (Tegangan Output Bisa Naik/Turun)." },
  { "en": "Apa Itu Konverter Cuk (Cuk Converter)?", "id": "Konverter DC-DC (Output Polaritas Terbalik/Negatif)." },
  { "en": "Apa Itu Siklotronverter (Cycloconverter)?", "id": "Mengubah Frekuensi AC Ke Frekuensi AC Lain (Rendah)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Membuat Beban Elektronik (SMPS) Terlihat Resistif Oleh PLN." },
  { "en": "Apa Kepanjangan PFC (Power Factor Correction)?", "id": "Koreksi Faktor Daya." },
  { "en": "Mengapa SMPS (Switched-Mode Power Supply) Membutuhkan PFC?", "id": "Karena Penyearah SMPS Menarik Arus Non-Linear (Harmonisa)." },
  { "en": "Apa Itu PFC Aktif (Active PFC)?", "id": "Menggunakan Konverter (Boost) Mengatur Arus Input Sefasa." },
  { "en": "Apa Itu PFC Pasif (Passive PFC)?", "id": "Menggunakan Induktor Besar (Filter) Memperbaiki Faktor Daya." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Informasi Mengubah Amplitudo Sinyal Pembawa (Carrier)." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Informasi Mengubah Frekuensi Sinyal Pembawa (Carrier)." },
  { "en": "Apa Itu Modulasi Fasa (PM)?", "id": "Informasi Mengubah Fasa Sinyal Pembawa (Carrier)." },
  { "en": "Apa Keunggulan FM Dibanding AM?", "id": "Lebih Tahan Noise (Gangguan), Kualitas Audio Lebih Baik." },
  { "en": "Apa Itu Modulasi Digital (Digital Modulation)?", "id": "Sinyal Informasi Berupa Data Digital (Bit 0 Dan 1)." },
  { "en": "Apa Itu ASK (Amplitude Shift Keying)?", "id": "Modulasi Digital Mengubah Amplitudo (Mirip AM)." },
  { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital Mengubah Frekuensi (Mirip FM)." },
  { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital Mengubah Fasa (Mirip PM)." },
  { "en": "Apa Itu QPSK (Quadrature Phase Shift Keying)?", "id": "PSK Menggunakan Empat Titik Fasa (Bawa 2 Bit)." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Gabungan Modulasi Amplitudo (ASK) Dan Fasa (PSK)." },
  { "en": "Apa Itu Diagram Konstelasi (Constellation Diagram)?", "id": "Grafik Visualisasi Titik-Titik Sinyal Modulasi Digital (QAM/PSK)." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Mengubah Sinyal Listrik Menjadi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Antena Isotropik (Isotropic Antenna)?", "id": "Antena Ideal (Teoritis) Memancar Sama Ke Segala Arah." },
  { "en": "Apa Itu Antena Omnidireksional (Omnidirectional)?", "id": "Memancar Kuat Ke Satu Bidang (Horizontal), Lemah Atas/Bawah." },
  { "en": "Apa Contoh Antena Omnidireksional?", "id": "Antena Tongkat (Whip) Pada Router Wi-Fi." },
  { "en": "Apa Itu Antena Direksional (Directional)?", "id": "Antena Memfokuskan Pancaran Kuat Ke Satu Arah Tertentu." },
  { "en": "Apa Contoh Antena Direksional?", "id": "Antena Yagi (TV), Antena Parabola (Satelit)." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Seberapa Fokus Pancaran Antena (Satuan: DBI/DBD)." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Grafik 3D Arah Dan Kekuatan Pancaran Antena." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Kecocokan Impedansi (Matching) Antena Dan Kabel." },
  { "en": "Berapa Nilai VSWR Ideal?", "id": "Satu Banding Satu (1:1), Berarti Tidak Ada Pantulan." },
  { "en": "Apa Itu Kabel Koaksial (Coaxial Cable)?", "id": "Kabel Transmisi RF (Konduktor Dalam, Isolator, Pelindung Luar)." },
  { "en": "Apa Itu Impedansi Karakteristik Kabel Koaksial?", "id": "Contoh: Lima Puluh (50) Ohm Atau Tujuh Puluh Lima (75) Ohm." },
  { "en": "Apa Itu Atenuasi (Attenuation) Kabel?", "id": "Pelemahan Kekuatan Sinyal Sepanjang Kabel (Rugi-Rugi)." },
  { "en": "Apa Itu Konektor BNC (Bayonet Neill-Concelman)?", "id": "Konektor Koaksial RF (Putar-Kunci) Untuk Osiloskop." },
  { "en": "Apa Itu Konektor SMA (SubMiniature Version A)?", "id": "Konektor Koaksial RF Ulir Kecil (Wi-Fi, GPS)." },
  { "en": "Apa Itu Konektor Tipe N (N Type)?", "id": "Konektor Koaksial RF Ulir Besar (Tahan Cuaca, Daya Tinggi)." },
  { "en": "Apa Itu Beban Leker (Dummy Load) RF?", "id": "Resistor Murni (50 Ohm) Pengganti Antena (Uji Coba)." },
  { "en": "Apa Itu Filter RF (RF Filter)?", "id": "Rangkaian Meneruskan Atau Menghambat Pita Frekuensi Tertentu." },
  { "en": "Apa Itu Penguat RF (RF Amplifier)?", "id": "Rangkaian Menguatkan Sinyal Listrik Frekuensi Radio." },
  { "en": "Apa Itu LNA (Low Noise Amplifier)?", "id": "Penguat RF Paling Depan (Gain Tinggi, Noise Rendah)." },
  { "en": "Apa Itu PA (Power Amplifier)?", "id": "Penguat RF Paling Akhir (Output Daya Besar) Sisi Pemancar." },
  { "en": "Apa Itu Mixer RF (RF Mixer)?", "id": "Mengalikan Dua Sinyal (Menggeser Frekuensi Naik/Turun)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Frekuensi Stabil (Carrier)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sirkuit Umpan Balik Penstabil Frekuensi Osilator (VCO)." },
  { "en": "Apa Itu Spektrum Elektromagnetik (Electromagnetic Spectrum)?", "id": "Rentang Semua Frekuensi Gelombang Elektromagnetik (Radio, Cahaya)." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Merancang, Membangun, Dan Mengoperasikan Robot." },
  { "en": "Apa Itu Lengan Robot (Robotic Arm)?", "id": "Manipulator Mekanis (Mirip Lengan Manusia) Untuk Tugas." },
  { "en": "Apa Itu Sendi (Joint) Robot?", "id": "Titik Sambungan Bergerak Pada Lengan Robot." },
  { "en": "Apa Itu Sendi Berputar (Revolute Joint)?", "id": "Sendi Yang Bergerak Memutar (Rotasi)." },
  { "en": "Apa Itu Sendi Prisma (Prismatic Joint)?", "id": "Sendi Yang Bergerak Lurus (Translasi/Linear)." },
  { "en": "Apa Itu Tautan (Link) Robot?", "id": "Bagian Kaku Yang Menghubungkan Antar Sendi Robot." },
  { "en": "Apa Itu Efektor Akhir (End Effector)?", "id": "Alat Kerja Di Ujung Lengan Robot (Gripper/Welder)." },
  { "en": "Apa Itu Pencengkeram (Gripper)?", "id": "Efektor Akhir (Tangan Robot) Untuk Memegang Objek." },
  { "en": "Apa Itu Pencengkeram Pneumatik (Pneumatic Gripper)?", "id": "Pencengkeram Yang Bergerak Menggunakan Tekanan Udara." },
  { "en": "Apa Itu Pencengkeram Vakum (Vacuum Gripper)?", "id": "Pencengkeram Menggunakan Isapan (Suction Cup) Vakum." },
  { "en": "Apa Itu Derajat Kebebasan (DOF)?", "id": "Jumlah Sumbu Gerakan Independen Yang Dimiliki Robot." },
  { "en": "Apa Kepanjangan DOF (Degrees of Freedom)?", "id": "Derajat Kebebasan." },
  { "en": "Apa Itu Ruang Kerja (Workspace) Robot?", "id": "Area Jangkauan Maksimum Yang Dapat Dicapai Ujung Robot." },
  { "en": "Apa Itu Kinematika Maju (Forward Kinematics)?", "id": "Menghitung Posisi Ujung (Dari Sudut Sendi)." },
  { "en": "Apa Itu Kinematika Terbalik (Inverse Kinematics)?", "id": "Menghitung Sudut Sendi (Dari Posisi Ujung Target)." },
  { "en": "Apa Itu Robot SCARA (Selective Compliance Assembly Robot Arm)?", "id": "Robot Cepat Untuk Perakitan (Gerakan Horizontal)." },
  { "en": "Apa Itu Robot Kartesian (Cartesian Robot)?", "id": "Robot Bergerak Lurus (Sumbu X, Y, Z)." },
  { "en": "Apa Itu Robot Kolaboratif (Cobot)?", "id": "Robot Didesain Aman Bekerja Berdampingan Dengan Manusia." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Komputer Menganalisis Dan Memahami Gambar/Video." },
  { "en": "Apa Itu LiDAR (Light Detection and Ranging)?", "id": "Sensor Jarak Menggunakan Pantulan Sinar Laser." },
  { "en": "Apa Itu SLAM (Simultaneous Localization and Mapping)?", "id": "Robot Membangun Peta Sambil Menentukan Posisinya." },
  { "en": "Apa Itu Avionik (Avionics)?", "id": "Sistem Elektronik Yang Digunakan Pada Pesawat Terbang." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Sistem Deteksi Objek Menggunakan Gelombang Radio." },
  { "en": "Apa Itu Transponder (Transmitter-Responder)?", "id": "Perangkat Menerima Sinyal Dan Membalasnya (Identifikasi Pesawat)." },
  { "en": "Apa Itu Sistem Pendaratan Instrumen (ILS)?", "id": "Bantuan Navigasi Radio Memandu Pesawat Mendarat." },
  { "en": "Apa Kepanjangan ILS (Instrument Landing System)?", "id": "Sistem Pendaratan Instrumen." },
  { "en": "Apa Itu Kotak Hitam (Black Box)?", "id": "Perekam Data Penerbangan (FDR) Dan Suara Kokpit (CVR)." },
  { "en": "Apa Kepanjangan FDR (Flight Data Recorder)?", "id": "Perekam Data Penerbangan." },
  { "en": "Apa Kepanjangan CVR (Cockpit Voice Recorder)?", "id": "Perekam Suara Kokpit." },
  { "en": "Apa Itu Autopilot (Pilot Otomatis)?", "id": "Sistem Mengendalikan Pesawat Secara Otomatis." },
  { "en": "Apa Itu Sistem Fly-By-Wire (FBW)?", "id": "Kontrol Pesawat Elektronik (Menggantikan Kontrol Mekanis/Hidrolik)." },
  { "en": "Apa Itu Pneumatik (Pneumatics)?", "id": "Sistem Yang Bekerja Menggunakan Tekanan Udara Kompresi." },
  { "en": "Apa Itu Hidrolik (Hydraulics)?", "id": "Sistem Yang Bekerja Menggunakan Tekanan Cairan (Oli)." },
  { "en": "Apa Itu Kompresor (Compressor)?", "id": "Mesin Pemampat (Kompresi) Udara." },
  { "en": "Apa Itu Silinder Pneumatik (Pneumatic Cylinder)?", "id": "Aktuator Menghasilkan Gerakan Lurus (Maju/Mundur)." },
  { "en": "Apa Itu Katup Solenoid (Solenoid Valve)?", "id": "Keran (Katup) Yang Dikendalikan Oleh Arus Listrik." },
  { "en": "Apa Itu Katup 3/2 (Tiga Per Dua)?", "id": "Katup Pneumatik (Tiga Lubang, Dua Posisi)." },
  { "en": "Apa Itu Katup 5/2 (Lima Per Dua)?", "id": "Katup Pneumatik (Lima Lubang, Dua Posisi)." },
  { "en": "Apa Itu Silinder Aksi Tunggal (Single-Acting)?", "id": "Silinder Bergerak Satu Arah (Pakai Pegas Balik)." },
  { "en": "Apa Itu Silinder Aksi Ganda (Double-Acting)?", "id": "Silinder Bergerak Dua Arah (Maju/Mundur Pakai Udara)." },
  { "en": "Apa Itu FRL (Filter, Regulator, Lubricator)?", "id": "Unit Persiapan Udara (Membersihkan, Mengatur Tekanan, Melumasi)." },
  { "en": "Apa Itu Manometer (Manometer/Pressure Gauge)?", "id": "Alat Pengukur Tekanan Udara Atau Cairan." },
  { "en": "Apa Itu Akustika (Acoustics)?", "id": "Ilmu Tentang Suara (Getaran Mekanis)." },
  { "en": "Apa Itu Frekuensi Audio (Audio Frequency)?", "id": "Rentang Frekuensi Suara (20 Hz Hingga 20.000 Hz)." },
  { "en": "Apa Itu Desibel (Decibel/DB)?", "id": "Satuan Logaritmik Pengukur Intensitas Suara (Kebisingan)." },
  { "en": "Apa Itu Mikrofon Dinamis (Dynamic Microphone)?", "id": "Mikrofon Berbasis Kumparan Bergerak (Induksi Magnetik)." },
  { "en": "Apa Itu Mikrofon Kondensor (Condenser Microphone)?", "id": "Mikrofon Berbasis Kapasitor (Perlu Daya Fantom)." },
  { "en": "Apa Itu Daya Fantom (Phantom Power)?", "id": "Daya DC (Biasanya +48V) Untuk Mikrofon Kondensor." },
  { "en": "Apa Itu Pola Kutub (Polar Pattern) Mikrofon?", "id": "Sensitivitas Arah Tangkapan Suara Mikrofon." },
  { "en": "Apa Itu Pola Kardioid (Cardioid)?", "id": "Pola Mikrofon (Menangkap Dari Depan, Menolak Belakang)." },
  { "en": "Apa Itu Pola Omnidireksional (Omnidirectional)?", "id": "Pola Mikrofon (Menangkap Suara Dari Segala Arah)." },
  { "en": "Apa Itu Crossover (Pemisah Frekuensi) Audio?", "id": "Memisahkan Sinyal Audio Ke Speaker (Woofer, Tweeter)." },
  { "en": "Apa Itu Woofer (Woofer)?", "id": "Speaker Untuk Frekuensi Rendah (Suara Bass)." },
  { "en": "Apa Itu Tweeter (Tweeter)?", "id": "Speaker Untuk Frekuensi Tinggi (Suara Treble/Cis)." },
  { "en": "Apa Itu Impedansi Speaker (Speaker Impedance)?", "id": "Hambatan Total Speaker (Contoh: 8 Ohm)." },
  { "en": "Apa Itu Penguat Kelas A (Class A Amplifier)?", "id": "Penguat Linearitas Tinggi, Efisiensi Sangat Rendah (Panas)." },
  { "en": "Apa Itu Penguat Kelas B (Class B Amplifier)?", "id": "Penguat Push-Pull (Menyebabkan Distorsi Crossover)." },
  { "en": "Apa Itu Penguat Kelas AB (Class AB Amplifier)?", "id": "Gabungan Kelas A Dan B (Paling Umum Audio)." },
  { "en": "Apa Itu Penguat Kelas D (Class D Amplifier)?", "id": "Penguat Switching (PWM), Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu Distorsi Harmonik Total (THD) Audio?", "id": "Ukuran Seberapa Banyak Distorsi (Noise) Dihasilkan Penguat." },
  { "en": "Apa Itu Respons Frekuensi (Frequency Response)?", "id": "Grafik Kemampuan Perangkat Merekam/Memutar Frekuensi." },
  { "en": "Apa Itu Kabel Audio Seimbang (Balanced)?", "id": "Kabel Tiga Konduktor (Mengurangi Noise Jarak Jauh)." },
  { "en": "Apa Itu Konektor XLR?", "id": "Konektor Tiga Pin (Kanon) Umum Untuk Kabel Balanced." },
  { "en": "Apa Itu Kabel Audio Tidak Seimbang (Unbalanced)?", "id": "Kabel Dua Konduktor (Jarak Pendek, Rentan Noise)." },
  { "en": "Apa Itu Konektor RCA (Cinch)?", "id": "Konektor (Merah, Putih) Umum Untuk Audio Unbalanced." },
  { "en": "Apa Itu Substrat PCB (PCB Substrate)?", "id": "Bahan Dasar Papan PCB (Isolator)." },
  { "en": "Apa Itu FR-4?", "id": "Bahan Substrat PCB Paling Umum (Fiberglass Epoksi)." },
  { "en": "Apa Itu PCB Aluminium (Metal Core PCB/MCPCB)?", "id": "PCB Dengan Inti Aluminium (Pendingin LED Daya Tinggi)." },
  { "en": "Apa Itu PCB Fleksibel (Flex PCB)?", "id": "PCB Yang Dapat Ditekuk (Bahan Polimida)." },
  { "en": "Apa Itu PCB Kaku-Fleksibel (Rigid-Flex PCB)?", "id": "Gabungan Antara Bagian PCB Kaku Dan Fleksibel." },
  { "en": "Apa Itu Teknik Tegangan Tinggi (High Voltage Engineering)?", "id": "Teknik Berkaitan Tegangan Di Atas 1000 Volt (1kV)." },
  { "en": "Apa Itu Pelepasan Korona (Corona Discharge)?", "id": "Pelepasan Listrik Sebagian Di Dekat Konduktor (Suara Desis)." },
  { "en": "Apa Itu Flashover (Loncatan Api)?", "id": "Loncatan Busur Api (Arc) Sepanjang Permukaan Isolator." },
  { "en": "Apa Itu Tusukan (Puncture) Isolator?", "id": "Kegagalan Isolator (Tembus) Akibat Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Bushing (Bushing) Tegangan Tinggi?", "id": "Isolator Berbentuk Silinder (Melewatkan Konduktor Lewat Dinding)." },
  { "en": "Apa Itu Isolator Rantai (String Insulator)?", "id": "Rangkaian Piringan Isolator (Keramik/Kaca) Digantung." },
  { "en": "Apa Fungsi Cincin Korona (Corona Ring)?", "id": "Membulatkan Medan Listrik, Mencegah Korona Di Ujung Isolator." },
  { "en": "Apa Itu Minyak Isolasi (Insulating Oil)?", "id": "Minyak Khusus (Pendingin Dan Isolasi) Di Dalam Trafo." },
  { "en": "Apa Itu Gas SF6 (Sulfur Hexafluoride)?", "id": "Gas Isolasi Sangat Kuat (Digunakan Di Pemutus Tenaga/CB)." },
  { "en": "Apa Itu Generator Impuls (Impulse Generator)?", "id": "Menghasilkan Tegangan Kejut Sangat Tinggi (Simulasi Petir)." },
  { "en": "Apa Itu Bilik Uji (Test Chamber) Tegangan Tinggi?", "id": "Ruangan Laboratorium Khusus Pengujian Tegangan Tinggi." },
  { "en": "Apa Itu Prostetik (Prosthetics) Bionik?", "id": "Anggota Tubuh Buatan (Tangan/Kaki) Yang Dikontrol Elektronik." },
  { "en": "Apa Itu Sinyal Miolistrik (Myoelectric Signal)?", "id": "Sinyal Listrik (EMG) Dari Kontraksi Otot." },
  { "en": "Bagaimana Prostetik Bionik Bekerja?", "id": "Membaca Sinyal Miolistrik (EMG) Otot Sisa Gerakan." },
  { "en": "Apa Itu Pencitraan Resonansi Magnetik (MRI)?", "id": "Pencitraan Medis Menggunakan Medan Magnet Kuat." },
  { "en": "Apa Kepanjangan MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Resonansi Magnetik." },
  { "en": "Apa Itu Pemindai CT (Computed Tomography)?", "id": "Pencitraan Medis Menggunakan Sinar-X (Rotasi 3D)." },
  { "en": "Apa Itu Mesin Ultrasonografi (USG)?", "id": "Pencitraan Medis Menggunakan Gelombang Suara Ultrasonik." },
  { "en": "Apa Itu Mesin EKG (Elektrokardiogram)?", "id": "Merekam Sinyal Listrik Aktivitas Jantung." },
  { "en": "Apa Itu Mesin EEG (Elektroensefalogram)?", "id": "Merekam Sinyal Listrik Aktivitas Otak." },
  { "en": "Apa Itu Oksimeter Denyut (Pulse Oximeter)?", "id": "Mengukur Saturasi Oksigen Dalam Darah (SpO2)." },
  { "en": "Apa Itu Pompa Infus (Infusion Pump)?", "id": "Alat Elektronik Mengontrol Aliran Cairan (Obat) Ke Pasien." },
  { "en": "Apa Itu Elektronika Yang Dapat Ditanam (Implantable Electronics)?", "id": "Perangkat Elektronik Ditanam Dalam Tubuh (Pacu Jantung)." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Implan Elektronik Menjaga Irama Detak Jantung." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Alat Memberi Kejut Listrik Mengatasi Aritmia Jantung." },
  { "en": "Apa Itu Implan Koklea (Cochlear Implant)?", "id": "Implan Elektronik Membantu Pendengaran (Tuli Saraf)." },
  { "en": "Apa Itu Telemetri (Telemetry) Medis?", "id": "Mengirim Data Pasien (EKG) Secara Nirkabel." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Praktik Melindungi Sistem Jaringan Dari Serangan Digital." },
  { "en": "Apa Itu Malware (Perangkat Lunak Berbahaya)?", "id": "Perangkat Lunak Didesain Untuk Merusak (Virus, Ransomware)." },
  { "en": "Apa Itu Virus (Virus) Komputer?", "id": "Malware Yang Menempel Program Lain Dan Mereplikasi Diri." },
  { "en": "Apa Itu Worm (Cacing) Komputer?", "id": "Malware Mandiri Yang Menyebar Sendiri Antar Jaringan." },
  { "en": "Apa Itu Trojan (Kuda Troya)?", "id": "Malware Menyamar Sebagai Perangkat Lunak Legal/Sah." },
  { "en": "Apa Itu Ransomware (Perangkat Lunak Tebusan)?", "id": "Malware Mengenkripsi Data Dan Meminta Tebusan Uang." },
  { "en": "Apa Itu Spyware (Perangkat Lunak Mata-Mata)?", "id": "Malware Mencuri Informasi Pengguna Secara Diam-Diam." },
  { "en": "Apa Itu Phishing (Pengelabuan)?", "id": "Upaya Menipu (Email, Situs Palsu) Mendapatkan Data Sensitif." },
  { "en": "Apa Itu Spear Phishing?", "id": "Phishing Yang Ditargetkan Khusus Ke Individu/Organisasi." },
  { "en": "Apa Itu Serangan DDoS (Distributed Denial-of-Service)?", "id": "Membanjiri Server Dengan Lalu Lintas Palsu (Server Down)." },
  { "en": "Apa Itu Firewall (Dinding Api)?", "id": "Sistem Keamanan Jaringan (Menyaring Lalu Lintas Masuk/Keluar)." },
  { "en": "Apa Itu Antivirus?", "id": "Perangkat Lunak Mendeteksi Dan Menghapus Malware." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Membuat Koneksi Jaringan Aman Dan Terenkripsi (Privat)." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Proses Mengubah Data Menjadi Kode Rahasia (Ciphertext)." },
  { "en": "Apa Itu Dekripsi (Decryption)?", "id": "Proses Mengembalikan Kode Rahasia Menjadi Data Asli." },
  { "en": "Apa Itu Kunci Simetris (Symmetric Key) Enkripsi?", "id": "Satu Kunci Sama Untuk Enkripsi Dan Dekripsi." },
  { "en": "Apa Itu Kunci Asimetris (Asymmetric Key) Enkripsi?", "id": "Menggunakan Dua Kunci (Publik Dan Privat)." },
  { "en": "Apa Itu Kunci Publik (Public Key)?", "id": "Kunci (Asimetris) Yang Boleh Dibagikan (Untuk Enkripsi)." },
  { "en": "Apa Itu Kunci Privat (Private Key)?", "id": "Kunci (Asimetris) Yang Harus Dirahasiakan (Untuk Dekripsi)." },
  { "en": "Apa Kepanjangan SSL (Secure Sockets Layer)?", "id": "Protokol Keamanan (Versi Lama TLS)." },
  { "en": "Apa Kepanjangan TLS (Transport Layer Security)?", "id": "Protokol Keamanan Enkripsi Web (HTTPS)." },
  { "en": "Apa Itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Versi Aman (Terenkripsi) Dari HTTP." },
  { "en": "Apa Itu Sertifikat Digital (Digital Certificate)?", "id": "Membuktikan Identitas Situs Web (Untuk HTTPS)." },
  { "en": "Apa Itu Otoritas Sertifikat (Certificate Authority/CA)?", "id": "Lembaga Tepercaya Yang Menerbitkan Sertifikat Digital." },
  { "en": "Apa Itu Otentikasi Dua Faktor (2FA)?", "id": "Metode Keamanan (Password Ditambah Kode Tambahan/OTP)." },
  { "en": "Apa Kepanjangan OTP (One-Time Password)?", "id": "Kata Sandi Sekali Pakai." },
  { "en": "Apa Itu Patch (Tambalan) Perangkat Lunak?", "id": "Pembaruan Perangkat Lunak Untuk Memperbaiki Bug/Keamanan." },
  { "en": "Apa Itu Zero-Day Vulnerability (Kerentanan Hari Nol)?", "id": "Celah Keamanan Yang Belum Diketahui/Belum Ada Patch." },
  { "en": "Apa Itu Penetrasi Tes (Penetration Testing/Pentest)?", "id": "Simulasi Serangan Siber Resmi Untuk Mencari Celah." },
  { "en": "Apa Itu Peretas Topi Putih (White Hat Hacker)?", "id": "Peretas Etis (Membantu Meningkatkan Keamanan)." },
  { "en": "Apa Itu Peretas Topi Hitam (Black Hat Hacker)?", "id": "Peretas Jahat (Mencuri Data, Merusak Sistem)." },
  { "en": "Apa Itu Rekayasa Sosial (Social Engineering)?", "id": "Memanipulasi Psikologis Orang Untuk Mendapat Informasi." },
  { "en": "Apa Itu Pusat Data (Data Center)?", "id": "Fasilitas Fisik Tempat Penyimpanan Server Dan Data." },
  { "en": "Apa Itu Rak Server (Server Rack)?", "id": "Lemari Logam Standar Tempat Memasang Server." },
  { "en": "Apa Itu Satuan Rak (Rack Unit/U)?", "id": "Satuan Tinggi Standar Peralatan Rak (1U = 1.75 Inci)." },
  { "en": "Apa Itu Server 1U?", "id": "Server Dengan Tinggi Satu (1) Satuan Rak." },
  { "en": "Apa Itu Server Blade (Blade Server)?", "id": "Server Tipis (Modular) Dalam Satu Sasis Hemat Ruang." },
  { "en": "Apa Itu PDU (Power Distribution Unit) Rak?", "id": "Stop Kontak Panjang Distribusi Listrik Dalam Rak." },
  { "en": "Apa Itu Catu Daya Redundan (Redundant Power Supply)?", "id": "Server Memiliki Dua Catu Daya (Jika Satu Gagal)." },
  { "en": "Apa Itu Lantai Yang Ditinggikan (Raised Floor)?", "id": "Lantai Ganda Di Pusat Data (Manajemen Kabel/Pendingin)." },
  { "en": "Apa Fungsi Raised Floor (Lantai Ditinggikan)?", "id": "Jalur Distribusi Udara Dingin Dan Kabel Listrik/Data." },
  { "en": "Apa Itu Pendingin CRAC (Computer Room Air Conditioner)?", "id": "AC Presisi Khusus Untuk Mendinginkan Pusat Data." },
  { "en": "Apa Kepanjangan CRAC (Computer Room Air Conditioner)?", "id": "Pendingin Udara Ruang Komputer." },
  { "en": "Apa Itu Lorong Panas/Lorong Dingin (Hot Aisle/Cold Aisle)?", "id": "Tata Letak Rak (Memisahkan Udara Panas/Dingin)." },
  { "en": "Apa Itu Penahanan (Containment) Lorong?", "id": "Menutup (Mengurung) Lorong Panas Atau Dingin." },
  { "en": "Apa Itu PUE (Power Usage Effectiveness)?", "id": "Ukuran Efisiensi Energi Pusat Data." },
  { "en": "Berapa Nilai PUE (Power Usage Effectiveness) Ideal?", "id": "Mendekati Satu (1.0), (Sangat Efisien)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply) Pusat Data?", "id": "UPS Skala Sangat Besar (Melindungi Seluruh Server)." },
  { "en": "Apa Itu Sistem Pemadam Api (Fire Suppression) Pusat Data?", "id": "Pemadam Api Aman (Gas Bersih) Untuk Elektronik." },
  { "en": "Apa Contoh Gas Bersih (Clean Agent)?", "id": "Novec 1230 Atau FM-200 (Tidak Merusak Server)." },
  { "en": "Apa Itu Redundansi Tier (Tier Redundancy) Pusat Data?", "id": "Tingkatan (Tier 1-4) Keandalan Infrastruktur Data Center." },
  { "en": "Apa Itu Tier 4 Pusat Data?", "id": "Keandalan Tertinggi (Sepenuhnya Redundan, Fault Tolerant)." },
  { "en": "Apa Itu Kolokasi (Colocation) Pusat Data?", "id": "Menyewa Ruang Rak Di Fasilitas Pusat Data Milik Orang." },
  { "en": "Apa Itu Cloud (Awan)?", "id": "Layanan Pusat Data (Penyimpanan, Komputasi) Lewat Internet." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?", "id": "Cabang AI (Kecerdasan Buatan) (Sistem Belajar Dari Data)." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Simulasi Kecerdasan Manusia Oleh Mesin." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Matematika (ML) Terinspirasi Otak Manusia." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning)?", "id": "Cabang ML (Menggunakan Jaringan Saraf Multi-Lapisan)." },
  { "en": "Apa Itu GPU (Graphics Processing Unit)?", "id": "Prosesor Grafis (Sangat Cepat Untuk Komputasi AI/ML)." },
  { "en": "Apa Kepanjangan GPU (Graphics Processing Unit)?", "id": "Unit Pemrosesan Grafis." },
  { "en": "Apa Itu TPU (Tensor Processing Unit)?", "id": "Chip (ASIC) Khusus Dibuat Google Untuk AI." },
  { "en": "Apa Itu Mahadata (Big Data)?", "id": "Kumpulan Data Sangat Besar, Kompleks, Cepat (Sulit Diproses)." },
  { "en": "Apa Itu Tiga V (V) Mahadata (Big Data)?", "id": "Volume (Ukuran), Velocity (Kecepatan), Variety (Ragam)." },
  { "en": "Apa Itu Gudang Data (Data Warehouse)?", "id": "Sistem Penyimpanan Data Terpusat Untuk Analisis Bisnis." },
  { "en": "Apa Itu Danau Data (Data Lake)?", "id": "Penyimpanan Data Mentah (Terstruktur/Tidak) Dalam Format Asli." },
  { "en": "Apa Itu ETL (Extract, Transform, Load)?", "id": "Proses Memindahkan Data (Ekstrak, Ubah, Muat)." },
  { "en": "Apa Itu Analitika Data (Data Analytics)?", "id": "Proses Memeriksa Data Untuk Mendapat Kesimpulan." },
  { "en": "Apa Itu Visualisasi Data (Data Visualization)?", "id": "Menampilkan Data Dalam Bentuk Grafis (Grafik, Peta)." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Kueri Standar Untuk Mengakses Basis Data Relasional." },
  { "en": "Apa Itu Basis Data Relasional (Relational Database)?", "id": "Basis Data Berbasis Tabel (Baris Dan Kolom)." },
  { "en": "Apa Itu Basis Data NoSQL (Not Only SQL)?", "id": "Basis Data Non-Relasional (Fleksibel, Dokumen, Graf)." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Teks Ringan Pertukaran Data (Populer)." },
  { "en": "Apa Itu XML (Extensible Markup Language)?", "id": "Bahasa Markup (Seperti HTML) Pertukaran Data Terstruktur." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Perangkat Lunak Berbeda." },
  { "en": "Apa Itu REST (Representational State Transfer) API?", "id": "Gaya Arsitektur API Populer (Menggunakan Metode HTTP)." },
  { "en": "Apa Itu Metode HTTP GET?", "id": "Metode API (REST) Untuk Meminta (Membaca) Data." },
  { "en": "Apa Itu Metode HTTP POST?", "id": "Metode API (REST) Untuk Mengirim (Membuat) Data Baru." },
  { "en": "Apa Itu Metode HTTP PUT?", "id": "Metode API (REST) Untuk Memperbarui Data Yang Sudah Ada." },
  { "en": "Apa Itu Metode HTTP DELETE?", "id": "Metode API (REST) Untuk Menghapus Data." },
  { "en": "Apa Itu Kode Status 200 (OK)?", "id": "Kode Respon API (Permintaan Berhasil)." },
  { "en": "Apa Itu Kode Status 404 (Not Found)?", "id": "Kode Respon API (Sumber Daya Tidak Ditemukan)." },
  { "en": "Apa Itu Kode Status 500 (Internal Server Error)?", "id": "Kode Respon API (Terjadi Kesalahan Di Server)." },
  { "en": "Apa Itu Mikrolayanan (Microservices)?", "id": "Arsitektur Memecah Aplikasi Besar Menjadi Layanan Kecil." },
  { "en": "Apa Itu Kontainer (Container) Perangkat Lunak?", "id": "Paket Perangkat Lunak Ringan (Termasuk Dependensi)." },
  { "en": "Apa Itu Docker?", "id": "Platform Populer Untuk Membangun Dan Menjalankan Kontainer." },
  { "en": "Apa Itu Kubernetes (K8s)?", "id": "Sistem Orkestrasi Kontainer (Manajemen Kontainer Otomatis)." },
  { "en": "Apa Itu DevOps (Development Operations)?", "id": "Kultur Menggabungkan Pengembangan (Dev) Dan Operasi (Ops)." },
  { "en": "Apa Itu CI/CD (Continuous Integration/Continuous Delivery)?", "id": "Praktik DevOps (Otomatisasi Build, Tes, Deploy)." },
  { "en": "Apa Itu Integrasi Berkelanjutan (Continuous Integration)?", "id": "Penggabungan Kode Otomatis Dan Pengujian (Build)." },
  { "en": "Apa Itu Pengiriman Berkelanjutan (Continuous Delivery)?", "id": "Otomatisasi Rilis Kode Ke Lingkungan Produksi." },
  { "en": "Apa Itu Pengujian Unit (Unit Test)?", "id": "Pengujian Otomatis Bagian Terkecil Kode (Fungsi)." },
  { "en": "Apa Itu Pengujian Integrasi (Integration Test)?", "id": "Pengujian Gabungan Antar Modul/Layanan." },
  { "en": "Apa Itu Pengujian E2E (End-to-End)?", "id": "Pengujian Alur Kerja Aplikasi Penuh (Dari Awal Akhir)." },
  { "en": "Apa Itu Utilitas Baris Perintah (Command Line Interface/CLI)?", "id": "Antarmuka Berbasis Teks (Terminal)." },
  { "en": "Apa Itu Antarmuka Grafis Pengguna (GUI)?", "id": "Antarmuka Berbasis Visual (Tombol, Jendela, Ikon)." },
  { "en": "Apa Kepanjangan GUI (Graphical User Interface)?", "id": "Antarmuka Grafis Pengguna." },
  { "en": "Apa Itu Linux?", "id": "Sistem Operasi Open-Source (Banyak Digunakan Server, Embedded)." },
  { "en": "Apa Itu Kernel (Kernel) Linux?", "id": "Inti Utama Dari Sistem Operasi Linux." },
  { "en": "Apa Itu Terminal (Terminal) Linux?", "id": "Antarmuka Baris Perintah (CLI) Untuk Perintah Teks." },
  { "en": "Apa Itu Shell (Shell) Linux?", "id": "Program Penerjemah Perintah Pengguna (Contoh: Bash)." },
  { "en": "Apa Perintah 'Ls' (Ls Command) Di Linux?", "id": "Menampilkan Daftar (List) File Dan Direktori." },
  { "en": "Apa Perintah 'Cd' (Cd Command) Di Linux?", "id": "Mengubah Direktori (Change Directory) Saat Ini." },
  { "en": "Apa Perintah 'Pwd' (Pwd Command) Di Linux?", "id": "Menampilkan Direktori Kerja Saat Ini (Print Working Directory)." },
  { "en": "Apa Perintah 'Mkdir' (Mkdir Command) Di Linux?", "id": "Membuat Direktori (Make Directory) Baru." },
  { "en": "Apa Perintah 'Rm' (Rm Command) Di Linux?", "id": "Menghapus (Remove) File Atau Direktori." },
  { "en": "Apa Perintah 'Cp' (Cp Command) Di Linux?", "id": "Menyalin (Copy) File Atau Direktori." },
  { "en": "Apa Perintah 'Mv' (Mv Command) Di Linux?", "id": "Memindahkan (Move) Atau Mengganti Nama (Rename) File." },
  { "en": "Apa Perintah 'Sudo' (Sudo Command) Di Linux?", "id": "Menjalankan Perintah Sebagai Super User (Root)." },
  { "en": "Apa Itu Root (Root) Di Linux?", "id": "Pengguna Dengan Hak Akses Tertinggi (Administrator)." },
  { "en": "Apa Itu Izin File (File Permission) Linux?", "id": "Mengatur Hak Aksa (Baca, Tulis, Eksekusi)." },
  { "en": "Apa Perintah 'Chmod' (Chmod Command)?", "id": "Mengubah Izin (Change Mode) File." },
  { "en": "Apa Perintah 'Chown' (Chown Command)?", "id": "Mengubah Kepemilikan (Change Owner) File." },
  { "en": "Apa Itu Skrip Shell (Shell Script)?", "id": "File Berisi Kumpulan Perintah Linux (Otomatisasi)." },
  { "en": "Apa Itu Cron (Cron Job)?", "id": "Penjadwal Tugas Otomatis (Scheduler) Di Linux." },
  { "en": "Apa Itu SSH (Secure Shell)?", "id": "Protokol Koneksi Remote (Terminal) Yang Aman (Terenkripsi)." },
  { "en": "Apa Itu SCP (Secure Copy Protocol)?", "id": "Menyalin File Antar Komputer Melalui SSH." },
  { "en": "Apa Itu RDP (Remote Desktop Protocol)?", "id": "Protokol Remote Desktop Grafis (GUI) Milik Microsoft." },
  { "en": "Apa Itu VNC (Virtual Network Computing)?", "id": "Sistem Remote Desktop Grafis (GUI) Lintas Platform." },
  { "en": "Apa Itu Server Web (Web Server)?", "id": "Perangkat Lunak Penyaji Halaman Web (Contoh: Apache, Nginx)." },
  { "en": "Apa Itu Server Basis Data (Database Server)?", "id": "Perangkat Lunak Pengelola Basis Data (Contoh: MySQL, PostgreSQL)." },
  { "en": "Apa Itu Server Aplikasi (Application Server)?", "id": "Menjalankan Logika Bisnis Aplikasi Web." },
  { "en": "Apa Itu Virtualisasi (Virtualization)?", "id": "Membuat Versi Virtual (Mesin, Jaringan, Penyimpanan)." },
  { "en": "Apa Itu Mesin Virtual (Virtual Machine/VM)?", "id": "Emulasi Sistem Komputer (Menjalankan OS Di Dalam OS)." },
  { "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Yang Membuat Dan Menjalankan Mesin Virtual." },
  { "en": "Apa Itu Tipe 1 Hypervisor (Bare-Metal)?", "id": "Hypervisor Berjalan Langsung Di Atas Hardware Fisik." },
  { "en": "Apa Itu Tipe 2 Hypervisor (Hosted)?", "id": "Hypervisor Berjalan Di Atas Sistem Operasi Lain." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Layanan Komputasi (Server, Storage) Melalui Internet." },
  { "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Menyewa Infrastruktur Virtual (VM, Jaringan) Di Cloud." },
  { "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Menyewa Platform (Basis Data, Server Web) Di Cloud." },
  { "en": "Apa Itu SaaS (Software as a Service)?", "id": "Menyewa Perangkat Lunak Jadi (Email, CRM) Di Cloud." },
  { "en": "Sebutkan Penyedia Cloud Publik Utama?", "id": "AWS (Amazon), Azure (Microsoft), GCP (Google)." },
  { "en": "Apa Itu Awan Privat (Private Cloud)?", "id": "Infrastruktur Cloud Yang Didedikasikan Untuk Satu Organisasi." },
  { "en": "Apa Itu Awan Hibrida (Hybrid Cloud)?", "id": "Gabungan Penggunaan Awan Publik Dan Awan Privat." },
  { "en": "Apa Itu Skalabilitas (Scalability) Cloud?", "id": "Kemampuan Menambah (Scale-Out) Kapasitas Dengan Mudah." },
  { "en": "Apa Itu Elastisitas (Elasticity) Cloud?", "id": "Kemampuan Menambah/Mengurangi Kapasitas Secara Otomatis." },
  { "en": "Apa Itu Cadangan (Backup) Data?", "id": "Proses Menyalin Data Untuk Pemulihan Bencana." },
  { "en": "Apa Itu Cadangan Penuh (Full Backup)?", "id": "Menyalin Semua Data Setiap Kali Backup." },
  { "en": "Apa Itu Cadangan Inkremental (Incremental Backup)?", "id": "Hanya Menyalin Data Yang Berubah Sejak Backup Terakhir." },
  { "en": "Apa Itu Cadangan Diferensial (Differential Backup)?", "id": "Menyalin Data Yang Berubah Sejak Backup Penuh Terakhir." },
  { "en": "Apa Itu Pemulihan Bencana (Disaster Recovery/DR)?", "id": "Rencana Mengembalikan Sistem IT Setelah Bencana." },
  { "en": "Apa Itu RPO (Recovery Point Objective)?", "id": "Batas Toleransi Kehilangan Data Maksimal (Waktu)." },
  { "en": "Apa Itu RTO (Recovery Time Objective)?", "id": "Batas Waktu Maksimal Sistem Boleh Mati (Offline)." },
  { "en": "Apa Itu Ketersediaan Tinggi (High Availability/HA)?", "id": "Desain Sistem Mengurangi Waktu Henti (Downtime) Minimal." },
  { "en": "Apa Itu Failover?", "id": "Proses Perpindahan Otomatis Ke Sistem Cadangan (Standby)." },
  { "en": "Apa Itu Penyeimbang Beban (Load Balancer)?", "id": "Membagi Lalu Lintas Jaringan Ke Beberapa Server." },
  { "en": "Mengapa Perlu Penyeimbang Beban (Load Balancer)?", "id": "Meningkatkan Kinerja, Skalabilitas, Dan Ketersediaan (HA)." },
  { "en": "Apa Itu Penyimpanan Terpasang Jaringan (NAS)?", "id": "Penyimpanan Data Terhubung Langsung Ke Jaringan (LAN)." },
  { "en": "Apa Kepanjangan NAS (Network Attached Storage)?", "id": "Penyimpanan Terpasang Jaringan." },
  { "en": "Apa Itu Jaringan Area Penyimpanan (SAN)?", "id": "Jaringan Khusus (Kecepatan Tinggi) Untuk Penyimpanan Data." },
  { "en": "Apa Kepanjangan SAN (Storage Area Network)?", "id": "Jaringan Area Penyimpanan." },
  { "en": "Apa Beda NAS Dan SAN?", "id": "NAS (File-Level, LAN), SAN (Block-Level, Jaringan Khusus)." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks)?", "id": "Menggabungkan Beberapa Hard Disk (Kinerja/Redundansi)." },
  { "en": "Apa Itu RAID 0 (Striping)?", "id": "Data Dibagi (Kinerja Cepat), Tidak Ada Redundansi." },
  { "en": "Apa Itu RAID 1 (Mirroring)?", "id": "Data Disalin (Duplikat) Ke Disk Lain (Redundansi Penuh)." },
  { "en": "Apa Itu RAID 5 (Striping with Parity)?", "id": "Data Dibagi Dengan Paritas (Tahan Satu Disk Gagal)." },
  { "en": "Apa Itu RAID 6 (Striping with Double Parity)?", "id": "Data Dibagi Paritas Ganda (Tahan Dua Disk Gagal)." },
  { "en": "Apa Itu RAID 10 (RAID 1+0)?", "id": "Gabungan Mirror (RAID 1) Dan Striping (RAID 0)." },
  { "en": "Apa Itu Hot Spare (Cadangan Panas) RAID?", "id": "Disk Cadangan Siaga (Menggantikan Disk Gagal Otomatis)." },
  { "en": "Apa Itu Hard Disk Drive (HDD)?", "id": "Penyimpanan Data Mekanis (Menggunakan Piringan Magnetik Berputar)." },
  { "en": "Apa Itu Solid State Drive (SSD)?", "id": "Penyimpanan Data Elektronik (Menggunakan Memori Flash/NAND)." },
  { "en": "Apa Keunggulan SSD Dibanding HDD?", "id": "Jauh Lebih Cepat, Tahan Guncangan, Tanpa Suara." },
  { "en": "Apa Itu SSD NVMe (Non-Volatile Memory Express)?", "id": "Antarmuka SSD Sangat Cepat (Melalui Jalur PCIe)." },
  { "en": "Apa Itu Keausan (Wear Leveling) SSD?", "id": "Teknik Meratakan Penulisan Data Di Sel Memori Flash." },
  { "en": "Apa Itu Sel Surya Perovskite?", "id": "Tipe Sel Surya Baru (Potensi Efisiensi Tinggi, Murah)." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Menghasilkan Listrik Dari Reaksi Kimia (Hidrogen, Oksigen)." },
  { "en": "Apa Emisi (Buangan) Sel Bahan Bakar Hidrogen?", "id": "Hanya Air Murni (H2O) Dan Panas." },
  { "en": "Apa Itu Elektrolisis (Electrolysis)?", "id": "Proses Memecah Air (H2O) Menjadi Hidrogen (H2) Dan Oksigen (O2)." },
  { "en": "Apa Itu Hidrogen Hijau (Green Hydrogen)?", "id": "Hidrogen Dihasilkan Elektrolisis (Menggunakan Listrik Terbarukan)." },
  { "en": "Apa Itu Hidrogen Biru (Blue Hydrogen)?", "id": "Hidrogen Dihasilkan Dari Gas Alam (Karbon Ditangkap/CCS)." },
  { "en": "Apa Itu Hidrogen Abu-Abu (Grey Hydrogen)?", "id": "Hidrogen Dihasilkan Dari Gas Alam (Karbon Dilepas)." },
  { "en": "Apa Kepanjangan CCS (Carbon Capture and Storage)?", "id": "Penangkapan Dan Penyimpanan Karbon." },
  { "en": "Apa Itu Geotermal (Panas Bumi)?", "id": "Energi Panas Yang Berasal Dari Inti Bumi." },
  { "en": "Apa Itu PLTP (Pembangkit Listrik Tenaga Panas Bumi)?", "id": "Menggunakan Uap Panas Bumi Memutar Turbin Generator." },
  { "en": "Apa Itu Energi Biomassa (Biomass)?", "id": "Energi Berasal Dari Bahan Organik (Kayu, Limbah Pertanian)." },
  { "en": "Apa Itu Biogas (Biogas)?", "id": "Gas Metana Hasil Penguraian Bahan Organik (Anaerobik)." },
  { "en": "Apa Itu Energi Pasang Surut (Tidal Energy)?", "id": "Energi Dihasilkan Dari Gerakan Naik Turun Air Laut." },
  { "en": "Apa Itu Energi Gelombang (Wave Energy)?", "id": "Energi Dihasilkan Dari Gerakan Gelombang Laut." },
  { "en": "Apa Itu OTEC (Ocean Thermal Energy Conversion)?", "id": "Energi Perbedaan Suhu Laut (Permukaan Hangat, Dalam Dingin)." },
  { "en": "Apa Itu Sistem Kontrol Terdistribusi (DCS)?", "id": "Sistem Kontrol Pabrik (Banyak Kontroler Terdistribusi)." },
  { "en": "Apa Kepanjangan DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi." },
  { "en": "Apa Beda PLC Dan DCS?", "id": "PLC (Cepat, Diskrit), DCS (Proses Besar, Analog, Terintegrasi)." },
  { "en": "Apa Itu Sistem Interlock Keselamatan (SIS)?", "id": "Sistem Kontrol Terpisah Khusus Untuk Keadaan Darurat." },
  { "en": "Apa Kepanjangan SIS (Safety Instrumented System)?", "id": "Sistem Berinstrumen Keselamatan." },
  { "en": "Apa Itu Tingkat Integritas Keselamatan (SIL)?", "id": "Ukuran Keandalan (Tingkat Keamanan) Sistem SIS." },
  { "en": "Apa Kepanjangan SIL (Safety Integrity Level)?", "id": "Tingkat Integritas Keselamatan (SIL 1 Sampai 4)." },
  { "en": "Apa Itu Diagram P&ID (Piping and Instrumentation Diagram)?", "id": "Gambar Teknik Detail (Pipa, Instrumen) Proses Pabrik." },
  { "en": "Apa Itu Katup Kontrol (Control Valve)?", "id": "Katup Mengatur Laju Aliran Fluida (Bukaan Variabel)." },
  { "en": "Apa Itu Posisioner (Positioner) Katup?", "id": "Alat Mengatur Posisi Bukaan Katup Kontrol Presisi." },
  { "en": "Apa Itu Aktuator (Actuator) Katup?", "id": "Penggerak (Pneumatik/Listrik) Yang Membuka/Menutup Katup." },
  { "en": "Apa Itu Transmitter (Pemancar) Sinyal 4-20 MA?", "id": "Sensor Mengirim Sinyal Arus Analog Standar Industri." },
  { "en": "Mengapa Menggunakan Sinyal Arus (4-20 MA)?", "id": "Tahan Noise, Tidak Terpengaruh Resistansi Kabel." },
  { "en": "Mengapa Dimulai Dari 4 MA (Bukan 0 MA)?", "id": "Membedakan Nilai Nol (4 MA) Dan Kabel Putus (0 MA)." },
  { "en": "Apa Itu Protokol HART (Highway Addressable Remote Transducer)?", "id": "Komunikasi Digital (Di Atas Sinyal 4-20 MA)." },
  { "en": "Apa Itu Fieldbus (Bus Lapangan)?", "id": "Jaringan Komunikasi Digital (Pengganti 4-20 MA)." },
  { "en": "Apa Itu Profibus (Process Field Bus)?", "id": "Standar Fieldbus Populer Di Eropa (Siemens)." },
  { "en": "Apa Itu Foundation Fieldbus?", "id": "Standar Fieldbus Populer Di Amerika (Proses Kontrol)." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Material Menghasilkan Tegangan Saat Diberi Tekanan Mekanis." },
  { "en": "Apa Itu Efek Piezoelektrik Terbalik (Reverse)?", "id": "Material Berubah Bentuk (Getar) Saat Diberi Tegangan." },
  { "en": "Apa Aplikasi Efek Piezoelektrik?", "id": "Sensor Tekanan, Sensor Getaran, Mikrofon, Buzzer." },
  { "en": "Apa Aplikasi Efek Piezoelektrik Terbalik?", "id": "Buzzer (Penghasil Suara), Aktuator Presisi." },
  { "en": "Apa Itu Bahan Piezoelektrik Umum?", "id": "Kristal Kuarsa (Quartz), Keramik PZT (Lead Zirconate Titanate)." },
  { "en": "Apa Itu Osilator Kristal (Crystal Oscillator)?", "id": "Osilator Stabil (Jam) Menggunakan Resonansi Kristal Piezoelektrik." },
  { "en": "Apa Itu Jaringan Seluler (Cellular Network)?", "id": "Jaringan Komunikasi Nirkabel (Handphone) Terbagi Dalam Sel." },
  { "en": "Apa Itu Stasiun Pangkalan (Base Station/BTS)?", "id": "Menara Pemancar/Penerima Sinyal Dalam Satu Sel." },
  { "en": "Apa Kepanjangan BTS (Base Transceiver Station)?", "id": "Stasiun Pemancar Penerima Pangkalan." },
  { "en": "Apa Itu Handoff (Handover) Seluler?", "id": "Proses Perpindahan Koneksi (Panggilan) Antar Sel (BTS)." },
  { "en": "Apa Itu 1G (Generasi Pertama)?", "id": "Teknologi Seluler Analog (Hanya Suara)." },
  { "en": "Apa Itu 2G (Generasi Kedua)?", "id": "Teknologi Seluler Digital (Suara, SMS, GPRS/EDGE)." },
  { "en": "Apa Kepanjangan GSM (Global System for Mobile Communications)?", "id": "Sistem Global Untuk Komunikasi Bergerak (Standar 2G)." },
  { "en": "Apa Itu 3G (Generasi Ketiga)?", "id": "Teknologi Seluler (Data Lebih Cepat, Internet Mobile)." },
  { "en": "Apa Itu 4G/LTE (Generasi Keempat)?", "id": "Teknologi Seluler (Data Sangat Cepat, Streaming Video)." },
  { "en": "Apa Kepanjangan LTE (Long-Term Evolution)?", "id": "Evolusi Jangka Panjang (Standar Utama 4G)." },
  { "en": "Apa Itu 5G (Generasi Kelima)?", "id": "Teknologi Seluler Terbaru (Sangat Cepat, Latensi Rendah)." },
  { "en": "Apa Keunggulan Utama 5G?", "id": "Kecepatan Sangat Tinggi, Latensi Sangat Rendah, Kapasitas Besar." },
  { "en": "Apa Itu Latensi (Latency) Jaringan?", "id": "Waktu Tunda (Delay) Pengiriman Paket Data." },
  { "en": "Apa Itu MmWave (Milimeter Wave) 5G?", "id": "Frekuensi Sangat Tinggi 5G (Super Cepat, Jarak Pendek)." },
  { "en": "Apa Itu Kartu SIM (Subscriber Identity Module)?", "id": "Kartu Pintar Menyimpan Identitas Pelanggan Seluler." },
  { "en": "Apa Itu IMEI (International Mobile Equipment Identity)?", "id": "Nomor Identitas Unik Perangkat Handphone (Hardware)." },
  { "en": "Apa Itu Bluetooth?", "id": "Teknologi Nirkabel Jarak Pendek (Koneksi Perangkat, Audio)." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Teknologi Jaringan Area Lokal Nirkabel (WLAN)." },
  { "en": "Apa Itu Router Wi-Fi?", "id": "Perangkat Memancarkan Sinyal Wi-Fi (Koneksi Internet)." },
  { "en": "Apa Itu Frekuensi Wi-Fi 2.4 GHz?", "id": "Frekuensi Wi-Fi (Jangkauan Jauh, Kecepatan Lebih Lambat)." },
  { "en": "Apa Itu Frekuensi Wi-Fi 5 GHz?", "id": "Frekuensi Wi-Fi (Jangkauan Pendek, Kecepatan Sangat Cepat)." },
  { "en": "Apa Kepanjangan NFC (Near Field Communication)?", "id": "Komunikasi Medan Dekat (Jarak Sangat Dekat)." },
  { "en": "Apa Aplikasi NFC (Near Field Communication)?", "id": "Pembayaran Nirkontak (E-Money), Pemasangan Perangkat." },
  { "en": "Apa Kepanjangan GPS (Global Positioning System)?", "id": "Sistem Pemosisi Global (Penentu Lokasi/Koordinat)." },
  { "en": "Bagaimana GPS (Global Positioning System) Bekerja?", "id": "Menerima Sinyal Waktu Dari Beberapa Satelit." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification)?", "id": "Identifikasi Frekuensi Radio (Tag Pasif, Pembaca Aktif)." },
  { "en": "Apa Aplikasi RFID (Radio-Frequency Identification)?", "id": "Tag Barang (Inventaris), Kartu Akses Kunci Pintu." },
  { "en": "Apa Itu Serat Optik (Fiber Optic)?", "id": "Kabel Transmisi Data Menggunakan Sinyal Cahaya." },
  { "en": "Apa Bahan Inti (Core) Serat Optik?", "id": "Kaca Silika Murni Atau Plastik." },
  { "en": "Apa Itu Pelapis (Cladding) Serat Optik?", "id": "Lapisan Kaca (Indeks Bias Rendah) Mengelilingi Inti." },
  { "en": "Bagaimana Cahaya Terjebak Dalam Serat Optik?", "id": "Melalui Pantulan Internal Total (Total Internal Reflection)." },
  { "en": "Apa Keunggulan Serat Optik?", "id": "Kecepatan Sangat Tinggi, Tahan Interferensi EMI." },
  { "en": "Apa Itu Serat Optik Mode Tunggal (Single-Mode)?", "id": "Inti Sangat Kecil (Jarak Sangat Jauh, Laser)." },
  { "en": "Apa Itu Serat Optik Mode Jamak (Multi-Mode)?", "id": "Inti Lebih Besar (Jarak Pendek, LED/VCSEL)." },
  { "en": "Apa Itu Laser (Laser)?", "id": "Sumber Cahaya Koheren Dan Monokromatik." },
  { "en": "Apa Kepanjangan Laser (Light Amplification by Stimulated Emission of Radiation)?", "id": "Penguatan Cahaya Oleh Emisi Radiasi Terstimulasi." },
  { "en": "Apa Itu Cahaya Koheren (Coherent)?", "id": "Semua Gelombang Cahaya Sefasa (Teratur)." },
  { "en": "Apa Itu Cahaya Monokromatik (Monochromatic)?", "id": "Cahaya Terdiri Dari Satu Panjang Gelombang (Warna)." },
  { "en": "Apa Itu Dioda Laser (Laser Diode)?", "id": "Semikonduktor (LED) Yang Menghasilkan Cahaya Laser." },
  { "en": "Apa Itu OTDR (Optical Time-Domain Reflectometer)?", "id": "Alat Menguji (Mencari Putus) Kabel Serat Optik." },
  { "en": "Bagaimana OTDR (Optical Time-Domain Reflectometer) Bekerja?", "id": "Mengirim Pulsa Cahaya, Menganalisis Sinyal Pantulan Balik." },
  { "en": "Apa Itu Penyambungan Fusi (Fusion Splicing)?", "id": "Menyambung Dua Ujung Serat Optik (Dilelehkan/Dilas)." },
  { "en": "Apa Itu Konektor LC (Lucent Connector) Serat Optik?", "id": "Konektor Serat Optik Kecil (Umum Digunakan)." },
  { "en": "Apa Itu Konektor SC (Subscriber Connector) Serat Optik?", "id": "Konektor Serat Optik (Bentuk Kotak, Dorong-Tarik)." },
  { "en": "Apa Itu OPM (Optical Power Meter)?", "id": "Alat Mengukur Kekuatan Sinyal (Daya) Cahaya Optik." },
  { "en": "Apa Itu OLS (Optical Light Source)?", "id": "Sumber Cahaya Laser (Uji) Untuk Mengukur Redaman." },
  { "en": "Apa Itu Redaman (Attenuation) Serat Optik?", "id": "Pelemahan Kekuatan Sinyal Cahaya Sepanjang Kabel." },
  { "en": "Apa Satuan Redaman Serat Optik?", "id": "Desibel Per Kilometer (DB/Km)." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Penyebaran Pulsa Cahaya (Menyebabkan Error Data)." },
  { "en": "Apa Kepanjangan WDM (Wavelength Division Multiplexing)?", "id": "Multiplexing Pembagian Panjang Gelombang." },
  { "en": "Apa Fungsi WDM (Wavelength Division Multiplexing)?", "id": "Mengirim Banyak Sinyal (Beda Warna) Dalam Satu Serat." },
  { "en": "Apa Itu Manajemen Proyek (Project Management)?", "id": "Merencanakan, Melaksanakan, Dan Mengendalikan Proyek." },
  { "en": "Apa Itu Lingkup (Scope) Proyek?", "id": "Batasan Pekerjaan Yang Harus Diselesaikan." },
  { "en": "Apa Itu Jadwal (Schedule) Proyek?", "id": "Garis Waktu (Timeline) Pengerjaan Proyek." },
  { "en": "Apa Itu Anggaran (Budget) Proyek?", "id": "Total Biaya Yang Dialokasikan Untuk Proyek." },
  { "en": "Apa Itu Tiga Kendala (Triple Constraint) Proyek?", "id": "Lingkup (Scope), Waktu (Time), Biaya (Cost)." },
  { "en": "Apa Itu Bagan Gantt (Gantt Chart)?", "id": "Bagan Batang Visualisasi Jadwal (Timeline) Proyek." },
  { "en": "Apa Itu Jalur Kritis (Critical Path) Proyek?", "id": "Rangkaian Tugas Terpanjang (Menentukan Durasi Total Proyek)." },
  { "en": "Apa Itu WBS (Work Breakdown Structure)?", "id": "Pemecahan Proyek Menjadi Tugas-Tugas Kecil Terkelola." },
  { "en": "Apa Itu Tonggak (Milestone) Proyek?", "id": "Titik Penting (Pencapaian) Dalam Jadwal Proyek." },
  { "en": "Apa Itu Pemangku Kepentingan (Stakeholder) Proyek?", "id": "Siapapun Yang Tertarik Atau Terdampak Oleh Proyek." },
  { "en": "Apa Itu Manajemen Risiko (Risk Management) Proyek?", "id": "Mengidentifikasi Dan Merespon Potensi Masalah Proyek." },
  { "en": "Apa Itu Metodologi Waterfall (Air Terjun)?", "id": "Metodologi Proyek Linear (Satu Tahap Selesai Pindah)." },
  { "en": "Apa Itu Metodologi Agile (Tangkas)?", "id": "Metodologi Proyek Iteratif Dan Fleksibel (Sprint)." },
  { "en": "Apa Itu Scrum (Scrum)?", "id": "Kerangka Kerja (Framework) Manajemen Proyek Agile." },
  { "en": "Apa Itu Sprint (Sprint) Dalam Scrum?", "id": "Siklus Kerja Waktu Tetap (Contoh: 2 Minggu)." },
  { "en": "Apa Itu Standar ISO 9001?", "id": "Standar Internasional Untuk Sistem Manajemen Mutu (Kualitas)." },
  { "en": "Apa Itu Standar ISO 14001?", "id": "Standar Internasional Untuk Sistem Manajemen Lingkungan." },
  { "en": "Apa Itu Standar ISO 45001?", "id": "Standar Internasional Untuk Sistem Manajemen K3 (OH&S)." },
  { "en": "Apa Itu Standar ISO/IEC 27001?", "id": "Standar Internasional Sistem Manajemen Keamanan Informasi (ISMS)." },
  { "en": "Apa Itu Standar ISO 50001?", "id": "Standar Internasional Sistem Manajemen Energi." },
  { "en": "Apa Kepanjangan IEC (International Electrotechnical Commission)?", "id": "Komisi Elektroteknik Internasional." },
  { "en": "Apa Fokus Standar IEC?", "id": "Standar Global Untuk Teknologi Kelistrikan Dan Elektronika." },
  { "en": "Apa Kepanjangan IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Institut Insinyur Listrik Dan Elektronik." },
  { "en": "Apa Standar IEEE 802.3?", "id": "Standar Untuk Jaringan Kabel Ethernet." },
  { "en": "Apa Standar IEEE 802.11?", "id": "Standar Untuk Jaringan Nirkabel (Wi-Fi)." },
  { "en": "Apa Kepanjangan ANSI (American National Standards Institute)?", "id": "Institut Standar Nasional Amerika." },
  { "en": "Apa Kepanjangan NEMA (National Electrical Manufacturers Association)?", "id": "Asosiasi Produsen Listrik Nasional (Standar Casing)." },
  { "en": "Apa Kepanjangan NFPA (National Fire Protection Association)?", "id": "Asosiasi Perlindungan Kebakaran Nasional." },
  { "en": "Apa Itu NFPA 70 (NEC)?", "id": "Standar Kode Kelistrikan Nasional Amerika (Instalasi)." },
  { "en": "Apa Itu NFPA 70E?", "id": "Standar Keselamatan Listrik Tempat Kerja (Arc Flash)." },
  { "en": "Apa Kepanjangan UL (Underwriters Laboratories)?", "id": "Organisasi Sertifikasi Keselamatan Produk (Amerika)." },
  { "en": "Apa Itu Tanda CE (CE Marking)?", "id": "Tanda Kesesuaian Produk Dengan Standar Uni Eropa." },
  { "en": "Apa Itu Arahan RoHS (Restriction of Hazardous Substances)?", "id": "Membatasi Bahan Berbahaya (Timbal, Merkuri) Di Elektronik." },
  { "en": "Apa Itu Arahan WEEE (Waste Electrical and Electronic Equipment)?", "id": "Manajemen Limbah (Sampah) Peralatan Elektronik." },
  { "en": "Apa Itu Manajemen Rantai Pasokan (SCM)?", "id": "Pengelolaan Alur Barang (Bahan Baku Hingga Konsumen)." },
  { "en": "Apa Itu Logistik (Logistics)?", "id": "Proses Penyimpanan Dan Pengiriman Barang." },
  { "en": "Apa Itu Inventaris (Inventory)?", "id": "Stok Barang (Bahan Baku, Barang Jadi)." },
  { "en": "Apa Itu Just-In-Time (JIT)?", "id": "Metode Inventaris (Stok Minimal, Tepat Waktu)." },
  { "en": "Apa Itu Waktu Tunggu (Lead Time)?", "id": "Waktu Antara Pemesanan Dan Penerimaan Barang." },
  { "en": "Apa Itu Kekurangan Chip (Chip Shortage)?", "id": "Krisis Pasokan Global Semikonduktor (Chip)." },
  { "en": "Apa Itu Manajemen Keusangan (Obsolescence Management)?", "id": "Mengelola Komponen Yang Akan Berhenti Produksi." },
  { "en": "Apa Itu Pemasok Tunggal (Single-Source Supplier)?", "id":"Hanya Ada Satu Pemasok Untuk Komponen Itu." },
  { "en": "Apa Itu Pemasok Ganda (Dual-Source Supplier)?", "id": "Memiliki Pemasok Alternatif (Cadangan) Untuk Komponen." },
  { "en": "Apa Itu Komponen Pasif (Passive Component)?", "id": "Komponen Tidak Membutuhkan Daya Eksternal (R, C, L)." },
  { "en": "Apa Itu Komponen Aktif (Active Component)?", "id": "Komponen Membutuhkan Daya EksternAL (Transistor, IC)." },
  { "en": "Apa Itu Resistor (Resistor)?", "id": "Menghambat Aliran Arus Listrik (Satuan Ohm)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Menyimpan Energi Listrik Dalam Medan Listrik (Farad)." },
  { "en": "Apa Itu Induktor (Inductor)?", "id": "Menyimpan Energi Listrik Dalam Medan Magnet (Henry)." },
  { "en": "Apa Itu Toleransi (Tolerance) Resistor?", "id": "Batas Persentase Penyimpangan Nilai (Contoh: 5 Persen)." },
  { "en": "Apa Itu Kapasitor Polar (Polarized Capacitor)?", "id": "Kapasitor Punya Kutub Positif Dan Negatif (Elco)." },
  { "en": "Apa Itu Kapasitor Non-Polar (Non-Polarized Capacitor)?", "id": "Kapasitor Tidak Punya Kutub (Keramik, Film)." },
  { "en": "Apa Itu Kapasitor Elektrolit (Elco)?", "id": "Kapasitor Polar Nilai Sangat Besar (Untuk Filter)." },
  { "en": "Apa Itu Kapasitor Keramik (Ceramic Capacitor)?", "id": "Kapasitor Non-Polar Kecil (Frekuensi Tinggi, Bypass)." },
  { "en": "Apa Itu Kapasitor Tantalum (Tantalum Capacitor)?", "id": "Kapasitor Polar (Ukuran Kecil, Performa Stabil)." },
  { "en": "Apa Itu Kapasitor Variabel (Variable Capacitor/Varco)?", "id": "Kapasitor Yang Nilai Kapasitansinya Dapat Diubah." },
  { "en": "Di Mana Kapasitor Variabel Digunakan?", "id": "Rangkaian Penala (Tuner) Frekuensi Radio (Radio Analog)." },
  { "en": "Apa Itu Potensiometer (Potentiometer)?", "id": "Resistor Variabel (Nilai Hambatannya Dapat Diatur)." },
  { "en": "Apa Fungsi Potensiometer?", "id": "Mengatur Volume Suara Atau Kecerahan Lampu (Dimmer)." },
  { "en": "Apa Itu Trimpot (Trimmer Potentiometer)?", "id": "Potensiometer Kecil (Ditanam Di PCB, Diatur Obeng)." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Cahaya (Nilai Hambatan Berubah)." },
  { "en": "Bagaimana Sifat LDR (Light Dependent Resistor)?", "id": "Semakin Terang Cahaya, Resistansi Semakin Kecil." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Peka Suhu (Nilai Hambatan Berubah)." },
  { "en": "Apa Itu NTC (Negative Temperature Coefficient) Termistor?", "id": "Suhu Naik, Resistansi Turun." },
  { "en": "Apa Itu PTC (Positive Temperature Coefficient) Termistor?", "id": "Suhu Naik, Resistansi Naik." },
  { "en": "Apa Itu Varistor (VDR - Voltage Dependent Resistor)?", "id": "Resistor Peka Tegangan (Melindungi Lonjakan Tegangan)." },
  { "en": "Apa Itu Rangkaian Seri (Series)?", "id": "Komponen Terhubung Berurutan (Satu Jalur Arus)." },
  { "en": "Apa Itu Rangkaian Paralel (Parallel)?", "id": "Komponen Terhubung Berjajar (Banyak Jalur Arus)." },
  { "en": "Bagaimana Resistansi Total Rangkaian Seri?", "id": "Dijumlahkan (RTotal = R1 + R2 + ...)." },
  { "en": "Bagaimana Resistansi Total Rangkaian Paralel?", "id": "Satu Per Dijumlahkan (1/RTotal = 1/R1 + 1/R2)." },
  { "en": "Bagaimana Kapasitansi Total Rangkaian Seri?", "id": "Satu Per Dijumlahkan (1/CTotal = 1/C1 + 1/C2)." },
  { "en": "Bagaimana Kapasitansi Total Rangkaian Paralel?", "id": "Dijumlahkan (CTotal = C1 + C2 + ...)." },
  { "en": "Bagaimana Induktansi Total Rangkaian Seri?", "id": "Dijumlahkan (LTotal = L1 + L2 + ...)." },
  { "en": "Bagaimana Induktansi Total Rangkaian Paralel?", "id": "Satu Per Dijumlahkan (1/LTotal = 1/L1 + 1/L2)." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "Tegangan = Arus Dikali Hambatan (V = I x R)." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Total Arus Masuk Titik Cabang = Total Arus Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Total Tegangan Dalam Satu Loop Tertutup = Nol." },
  { "en": "Apa Itu Daya Listrik (Electrical Power)?", "id": "Energi Per Satuan Waktu (Satuan: Watt)." },
  { "en": "Apa Rumus Daya Listrik (P)?", "id": "Daya = Tegangan Dikali Arus (P = V x I)." },
  { "en": "Apa Rumus Daya Listrik Lainnya?", "id": "Daya = Arus Kuadrat Dikali Resistansi (P = I² x R)." },
  { "en": "Apa Itu Efek Joule (Joule Heating)?", "id": "Energi Listrik Berubah Menjadi Panas Di Resistor." },
  { "en": "Apa Itu Pembagi Tegangan (Voltage Divider)?", "id": "Rangkaian Resistor Seri Membagi Tegangan Sumber." },
  { "en": "Apa Itu Pembagi Arus (Current Divider)?", "id": "Rangkaian Resistor Paralel Membagi Arus Sumber." },
  { "en": "Apa Itu Jembatan Wheatstone (Wheatstone Bridge)?", "id": "Rangkaian Jembatan Mengukur Resistansi Tidak Diketahui." },
  { "en": "Kapan Jembatan Wheatstone Seimbang?", "id": "Saat Voltmeter (Galvanometer) Menunjukkan Nol Volt." },
  { "en": "Apa Aplikasi Jembatan Wheatstone?", "id": "Membaca Sensor Resistif (Strain Gauge, RTD)." },
  { "en": "Apa Itu Rangkaian Terbuka (Open Circuit)?", "id": "Koneksi Terputus (Hambatan Tak Terhingga, Arus Nol)." },
  { "en": "Apa Itu Hubungan Singkat (Short Circuit)?", "id": "Koneksi Langsung (Hambatan Nol, Arus Maksimal)." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan Dan Resistor Seri." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus Dan Resistor Paralel." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Analisis Rangkaian Linear (Hitung Efek Tiap Sumber Terpisah)." },
  { "en": "Apa Itu Transfer Daya Maksimum?", "id": "Daya Maksimal Pindah Saat Impedansi Beban = Impedansi Sumber." },
  { "en": "Apa Itu Rangkaian Penyearah (Rectifier)?", "id": "Mengubah Sinyal AC Menjadi Sinyal DC." },
  { "en": "Apa Itu Rangkaian Filter (Filter Circuit)?", "id": "Meloloskan Frekuensi Tertentu, Menghambat Frekuensi Lain." },
  { "en": "Apa Itu Rangkaian Penguat (Amplifier)?", "id": "Meningkatkan Amplitudo (Kekuatan) Sinyal Listrik." },
  { "en": "Apa Itu Penguatan (Gain) Penguat?", "id": "Rasio Sinyal Output Terhadap Sinyal Input." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik (Gelombang) Sendiri." },
  { "en": "Apa Itu Rangkaian Tangki LC (LC Tank Circuit)?", "id": "Resonator Paralel (Induktor Dan Kapasitor) Untuk Osilator." },
  { "en": "Apa Itu Rangkaian Penjepit (Clamper Circuit)?", "id": "Menggeser Level DC Sinyal AC (Tanpa Mengubah Bentuk)." },
  { "en": "Apa Itu Rangkaian Pemotong (Clipper Circuit)?", "id": "Memotong (Membatasi) Amplitudo Sinyal Di Level Tertentu." },
  { "en": "Apa Itu Rangkaian Integrator (Integrator)?", "id": "Rangkaian Output-Nya Adalah Integral Sinyal Input (Op-Amp)." },
  { "en": "Apa Itu Rangkaian Diferensiator (Differentiator)?", "id": "Rangkaian Output-Nya Adalah Turunan Sinyal Input (Op-Amp)." },
  { "en": "Apa Itu Multivibrator (Multivibrator)?", "id": "Rangkaian Osilator Penghasil Gelombang Non-Sinusoidal (Kotak)." },
  { "en": "Apa Itu Multivibrator Astabil (Astable)?", "id": "Multivibrator Tidak Stabil (Osilator Gelombang Kotak)." },
  { "en": "Apa Itu Multivibrator Monostabil (Monostable)?", "id": "Satu Keadaan Stabil (Timer Sekali Tembak/One-Shot)." },
  { "en": "Apa Itu Multivibrator Bistabil (Bistable)?", "id": "Dua Keadaan Stabil (Flip-Flop, Penyimpan Memori)." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis (Dua Ambang Batas)." },
  { "en": "Apa Fungsi Histeresis (Hysteresis) Schmitt Trigger?", "id": "Mencegah Transisi Palsu (Noise) Dekat Titik Ambang." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Jutaan Komponen (Transistor) Dalam Satu Chip Silikon." },
  { "en": "Apa Itu Skala Integrasi (Scale of Integration)?", "id": "Ukuran Kepadatan Komponen (Transistor) Dalam IC." },
  { "en": "Apa Itu SSI (Small-Scale Integration)?", "id": "Integrasi Skala Kecil (Puluhan Transistor, Gerbang Logika)." },
  { "en": "Apa Itu MSI (Medium-Scale Integration)?", "id": "Integrasi Skala Menengah (Ratusan Transistor, Counter)." },
  { "en": "Apa Itu LSI (Large-Scale Integration)?", "id": "Integrasi Skala Besar (Ribuan Transistor, CPU Awal)." },
  { "en": "Apa Itu VLSI (Very Large-Scale Integration)?", "id": "Integrasi Skala Sangat Besar (Jutaan Transistor, CPU Modern)." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Cakram Tipis Silikon Murni (Bahan Dasar Pembuatan IC)." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Ruang Produksi IC (Sangat Bersih, Bebas Debu)." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Menambahkan Impuritas Ke Silikon (Tipe P Atau N)." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola (Masker) Ke Wafer (Sinar UV)." },
  { "en": "Apa Itu Etsa (Etching)?", "id": "Proses Kimia (Basah/Kering) Mengikis Material Wafer." },
  { "en": "Apa Itu Deposisi (Deposition)?", "id": "Proses Menambahkan Lapisan Material Baru Ke Wafer." },
  { "en": "Apa Itu Dicing (Pemotongan) Wafer?", "id": "Memotong Wafer Menjadi Chip (Die) Individual." },
  { "en": "Apa Itu Die (Mata Dadu) IC?", "id": "Satu Keping Chip Silikon (Inti) Sebelum Dikemas." },
  { "en": "Apa Itu Kemasan (Packaging) IC?", "id": "Proses Membungkus Die (Chip) Dan Memberi Kaki (Pin)." },
  { "en": "Apa Itu Ikatan Kawat (Wire Bonding)?", "id": "Menyambung Pad Die Ke Kaki Kemasan (Kawat Emas)." },
  { "en": "Apa Itu Kemasan DIP (Dual In-Line Package)?", "id": "Kemasan IC THT (Dua Baris Kaki Paralel)." },
  { "en": "Apa Itu Kemasan SOIC (Small Outline IC)?", "id": "Kemasan IC SMD (Bentuk Mirip DIP Tapi Kecil)." },
  { "en": "Apa Itu Kemasan QFP (Quad Flat Package)?", "id": "Kemasan IC SMD (Kaki Di Keempat Sisi)." },
  { "en": "Apa Itu Kemasan BGA (Ball Grid Array)?", "id": "Kemasan IC SMD (Koneksi Bola Timah Di Bawah)." },
  { "en": "Apa Itu Hukum Moore (Moore's Law)?", "id": "Jumlah Transistor Berlipat Ganda Setiap Dua Tahun." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Otak Komputer (Melakukan Perhitungan Dan Kontrol)." },
  { "en": "Apa Itu GPU (Graphics Processing Unit)?", "id": "Prosesor Khusus Pengolahan Grafis (Gambar, Video, AI)." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Penyimpanan Data Sementara (Volatile, Cepat)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Penyimpanan Permanen (Non-Volatile, BIOS/Firmware)." },
  { "en": "Apa Itu Hard Disk Drive (HDD)?", "id": "Penyimpanan Magnetik (Piringan Berputar, Mekanis)." },
  { "en": "Apa Itu Solid State Drive (SSD)?", "id": "Penyimpanan Flash (Elektronik, Tanpa Bagian Bergerak)." },
  { "en": "Apa Itu Papan Induk (Motherboard)?", "id": "Papan Sirkuit Utama Penghubung Semua Komponen Komputer." },
  { "en": "Apa Itu Bus (Bus) Komputer?", "id": "Jalur Komunikasi Data Antar Komponen (Alamat, Data, Kontrol)." },
  { "en": "Apa Itu BIOS (Basic Input/Output System)?", "id": "Firmware Pengecekan Hardware Awal Saat Komputer Booting." },
  { "en": "Apa Itu UEFI (Unified Extensible Firmware Interface)?", "id": "Pengganti BIOS Modern (Antarmuka Grafis, Cepat)." },
  { "en": "Apa Itu Sistem Operasi (Operating System/OS)?", "id": "Perangkat Lunak Pengelola Hardware Dan Software (Windows, Linux)." },
  { "en": "Apa Itu Perangkat Input (Input Device)?", "id": "Perangkat Memasukkan Data Ke Komputer (Keyboard, Mouse)." },
  { "en": "Apa Itu Perangkat Output (Output Device)?", "id": "Perangkat Menampilkan Hasil (Monitor, Printer, Speaker)." },
  { "en": "Apa Itu Pixel (Pixel)?", "id": "Titik Terkecil Pembentuk Gambar Digital Di Layar." },
  { "en": "Apa Itu Resolusi (Resolution) Layar?", "id": "Jumlah Pixel (Lebar X Tinggi) Pada Layar." },
  { "en": "Apa Itu Layar LCD (Liquid Crystal Display)?", "id": "Layar Tampilan Menggunakan Kristal Cair Dan Lampu Latar." },
  { "en": "Apa Itu Lampu Latar (Backlight) LCD?", "id": "Sumber Cahaya Di Belakang Panel Kristal Cair (CCFL/LED)." },
  { "en": "Apa Itu Layar TFT (Thin-Film Transistor)?", "id": "Tipe LCD (Tiap Piksel Punya Transistor Sendiri)." },
  { "en": "Apa Itu Layar OLED (Organic Light Emitting Diode)?", "id": "Layar Tampilan (Piksel Memancarkan Cahaya Sendiri)." },
  { "en": "Apa Keunggulan OLED Dibanding LCD?", "id": "Warna Hitam Sempurna, Kontras Tinggi, Lebih Tipis." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen)?", "id": "Layar Yang Berfungsi Sebagai Perangkat Input (Sentuhan)." },
  { "en": "Apa Itu Layar Sentuh Resistif (Resistive)?", "id": "Layar Sentuh Bekerja Berdasarkan Tekanan (Dua Lapisan)." },
  { "en": "Apa Itu Layar Sentuh Kapasitif (Capacitive)?", "id": "Layar Sentuh Bekerja Berbasis Listrik Statis Tubuh (Jari)." },
  { "en": "Apa Itu Internet?", "id": "Jaringan Komputer Global Yang Saling Terhubung (Worldwide)." },
  { "en": "Apa Itu World Wide Web (WWW)?", "id": "Layanan (Informasi, Halaman) Di Atas Jaringan Internet." },
  { "en": "Apa Itu Alamat IP (Internet Protocol)?", "id": "Alamat Numerik Unik Perangkat Di Jaringan Internet." },
  { "en": "Apa Itu IPv4 (Internet Protocol Version 4)?", "id": "Format Alamat IP 32-Bit (Contoh: 192.168.1.1)." },
  { "en": "Apa Itu IPv6 (Internet Protocol Version 6)?", "id": "Format Alamat IP 128-Bit (Menggantikan IPv4)." },
  { "en": "Apa Itu Alamat MAC (Media Access Control)?", "id": "Alamat Fisik Unik Perangkat Keras Jaringan (NIC)." },
  { "en": "Apa Kepanjangan NIC (Network Interface Card)?", "id": "Kartu Antarmuka Jaringan (Kartu LAN/Wi-Fi)." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan (Meneruskan Paket IP)." },
  { "en": "Apa Itu Switch (Switch) Jaringan?", "id": "Perangkat Penghubung Perangkat Dalam Satu Jaringan Lokal (LAN)." },
  { "en": "Apa Beda Router Dan Switch?", "id": "Router (Antar Jaringan, IP), Switch (Dalam Jaringan, MAC)." },
  { "en": "Apa Itu Hub (Hub) Jaringan?", "id": "Penghubung Jaringan Pasif (Mengulang Sinyal Ke Semua Port)." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Referensi Jaringan Tujuh (7) Lapisan." },
  { "en": "Apa Itu Model TCP/IP (Transmission Control Protocol/Internet Protocol)?", "id": "Model Referensi Jaringan Empat (4) Lapisan (Praktis)." },
  { "en": "Apa Itu Lapisan Fisik (Physical Layer)?", "id": "Lapisan OSI 1 (Kabel, Sinyal Listrik, Bit)." },
  { "en": "Apa Itu Lapisan Taut Data (Data Link Layer)?", "id": "Lapisan OSI 2 (Alamat MAC, Switch, Frame)." },
  { "en": "Apa Itu Lapisan Jaringan (Network Layer)?", "id": "Lapisan OSI 3 (Alamat IP, Router, Paket)." },
  { "en": "Apa Itu Lapisan Transport (Transport Layer)?", "id": "Lapisan OSI 4 (TCP, UDP, Port, Segmen)." },
  { "en": "Apa Itu Lapisan Aplikasi (Application Layer)?", "id": "Lapisan OSI 7 (HTTP, FTP, SMTP, DNS)." },
  { "en": "Apa Kepanjangan TCP (Transmission Control Protocol)?", "id": "Protokol Kontrol Transmisi (Andal, Terurut)." },
  { "en": "Apa Kepanjangan UDP (User Datagram Protocol)?", "id": "Protokol Datagram Pengguna (Cepat, Tidak Andal)." },
  { "en": "Kapan Menggunakan TCP (Transmission Control Protocol)?", "id": "Data Penting, Harus Utuh (Web, Email, File)." },
  { "en": "Kapan Menggunakan UDP (User Datagram Protocol)?", "id": "Data Cepat, Toleran Hilang (Streaming, Game, VoIP)." },
  { "en": "Apa Itu Nomor Port (Port Number)?", "id": "Nomor Identifikasi Layanan/Aplikasi Di Jaringan (Transport)." },
  { "en": "Berapa Nomor Port Untuk HTTP (Web)?", "id": "Port Delapan Puluh (80)." },
  { "en": "Berapa Nomor Port Untuk HTTPS (Web Aman)?", "id": "Port Empat Ratus Empat Puluh Tiga (443)." },
  { "en": "Berapa Nomor Port Untuk FTP (File Transfer)?", "id": "Port Dua Puluh Satu (21) Dan Dua Puluh (20)." },
  { "en": "Berapa Nomor Port Untuk SSH (Secure Shell)?", "id": "Port Dua Puluh Dua (22)." },
  { "en": "Apa Kepanjangan DNS (Domain Name System)?", "id": "Sistem Penamaan Domain." },
  { "en": "Apa Fungsi DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain (Google.com) Menjadi Alamat IP." },
  { "en": "Apa Kepanjangan DHCP (Dynamic Host Configuration Protocol)?", "id": "Protokol Konfigurasi Host Dinamis." },
  { "en": "Apa Fungsi DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberikan Alamat IP Secara Otomatis Ke Klien." },
  { "en": "Apa Itu NAT (Network Address Translation)?", "id": "Menyembunyikan Banyak Alamat IP Privat Dibalik Satu IP Publik." },
  { "en": "Apa Itu Alamat IP Privat?", "id": "Alamat IP Internal Jaringan (Contoh: 192.168.x.x)." },
  { "en": "Apa Itu Alamat IP Publik?", "id": "Alamat IP Unik (Rute Global) Di Internet." },
  { "en": "Apa Itu Firewall (Dinding Api)?", "id": "Sistem Keamanan Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Jaringan Pribadi Virtual (Koneksi Aman Lewat Internet)." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Jaringan Ethernet (Pasangan Terpilin Tanpa Pelindung)." },
  { "en": "Apa Itu Kabel STP (Shielded Twisted Pair)?", "id": "Kabel Jaringan (Pasangan Terpilin Dengan Pelindung Foil)." },
  { "en": "Apa Beda UTP Dan STP?", "id": "STP (Shielded) Lebih Tahan Interferensi (Noise) EMI." },
  { "en": "Apa Itu Konektor RJ45 (Registered Jack 45)?", "id": "Konektor Standar Delapan (8) Pin Kabel Ethernet." },
  { "en": "Apa Itu Urutan Kabel Straight-Through (Lurus)?", "id": "Urutan Pin Ujung Ke Ujung Sama (PC Ke Switch)." },
  { "en": "Apa Itu Urutan Kabel Crossover (Silang)?", "id": "Urutan Pin Menyilang (PC Ke PC, Switch Ke Switch)." },
  { "en": "Apa Itu Standar T568A Dan T568B?", "id": "Standar Urutan Warna Kabel UTP/RJ45." },
  { "en": "Apa Itu LAN Tester (Penguji LAN)?", "id": "Alat Memeriksa Koneksi Dan Urutan Kabel RJ45." },
  { "en": "Apa Itu PoE (Power over Ethernet)?", "id": "Menyalurkan Daya Listrik Melalui Kabel Ethernet (LAN)." },
  { "en": "Apa Aplikasi PoE (Power over Ethernet)?", "id": "Kamera IP, Telepon IP, Titik Akses (AP) Wi-Fi." },
  { "en": "Apa Itu Injektor PoE (PoE Injector)?", "id": "Alat Menambahkan Daya (PoE) Ke Kabel LAN." },
  { "en": "Apa Itu Switch PoE (PoE Switch)?", "id": "Switch Jaringan Yang Langsung Menyediakan Daya PoE." },
  { "en": "Apa Itu Ping (Ping)?", "id": "Perintah Menguji Konektivitas Jaringan (Mengirim Paket ICMP)." },
  { "en": "Apa Itu Tracert/Traceroute?", "id": "Perintah Melacak Rute (Lompatan Router) Paket Jaringan." },
  { "en": "Apa Itu Ipconfig/Ifconfig?", "id": "Perintah Menampilkan Konfigurasi Alamat IP Komputer." },
  { "en": "Apa Itu Subnet Mask?", "id": "Menentukan Bagian Jaringan (Network) Dan Host Alamat IP." },
  { "en": "Apa Itu Gateway (Gerbang) Default?", "id": "Alamat Router (Jalan Keluar) Jaringan Lokal." },
  { "en": "Apa Itu Server (Server)?", "id": "Komputer Penyedia Layanan (Data, Web) Di Jaringan." },
  { "en": "Apa Itu Klien (Client)?", "id": "Komputer Pengguna (Penerima) Layanan Dari Server." },
  { "en": "Apa Itu Domain (Domain)?", "id": "Nama Unik Identifikasi Server/Jaringan (Contoh: Google.com)." },
  { "en": "Apa Itu Hosting (Hosting) Web?", "id": "Layanan Penyimpanan File Situs Web Di Server." },
  { "en": "Apa Itu HTML (Hypertext Markup Language)?", "id": "Bahasa Markup Standar Membuat Struktur Halaman Web." },
  { "en": "Apa Itu CSS (Cascading Style Sheets)?", "id": "Bahasa Mengatur Tampilan (Gaya, Warna, Tata Letak) HTML." },
  { "en": "Apa Itu JavaScript?", "id": "Bahasa Pemrograman Membuat Halaman Web Interaktif (Dinamis)." },
  { "en": "Apa Itu Browser (Peramban) Web?", "id": "Perangkat Lunak Menampilkan Halaman Web (Chrome, Firefox)." },
  { "en": "Apa Itu URL (Uniform Resource Locator)?", "id": "Alamat Lengkap Sumber Daya Di Internet (Http://...)." },
  { "en": "Apa Itu Hyperlink (Tautan Hiper)?", "id": "Teks Atau Gambar Yang Dapat Diklik (Menuju URL Lain)." },
  { "en": "Apa Itu Cookie (Kuki) Web?", "id": "Data Kecil Disimpan Browser (Mengingat Sesi, Preferensi)." },
  { "en": "Apa Itu Cache (Cache) Browser?", "id": "Penyimpanan Sementara Data Web (Mempercepat Pemuatan Ulang)." },
  { "en": "Apa Itu IoT (Internet of Things)?", "id": "Objek Fisik Yang Dilengkapi Sensor Dan Koneksi Internet." },
  { "en": "Apa Itu Perangkat Embedded (Embedded System)?", "id": "Sistem Komputer Khusus (Tertanam Dalam Perangkat)." },
  { "en": "Apa Itu Arduino?", "id": "Platform Mikrokontroler Open-Source (Hardware Dan Software)." },
  { "en": "Apa Itu Raspberry Pi?", "id": "Komputer Papan Tunggal (Single-Board Computer) Berbasis Linux." },
  { "en": "Apa Beda Arduino Dan Raspberry Pi?", "id": "Arduino (Mikrokontroler, Real-Time), Raspberry Pi (Mikroprosesor, OS)." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Mengubah Besaran Fisik Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Mengubah Sinyal Listrik Menjadi Gerakan Fisik." },
  { "en": "Apa Contoh Sensor?", "id": "Sensor Suhu, Cahaya, Kelembaban, Jarak, Gerak." },
  { "en": "Apa Contoh Aktuator?", "id": "Motor DC, Motor Servo, LED, Relay, Solenoid." },
  { "en": "Apa Itu Protokol MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Komunikasi IoT Ringan (Model Publish-Subscribe)." },
  { "en": "Apa Itu Model Publish-Subscribe (Pub/Sub)?", "id": "Pola Pesan (Penerbit Mengirim Ke Topik, Pelanggan Menerima)." },
  { "en": "Apa Itu Broker (Broker) MQTT?", "id": "Server Pusat Yang Menerima Dan Mendistribusikan Pesan MQTT." },
  { "en": "Apa Itu Topik (Topic) MQTT?", "id": "Label (Saluran) Kategori Pesan MQTT (Contoh: 'Rumah/Suhu')." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Pemrosesan Data Dilakukan Dekat Perangkat IoT (Lokal)." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Pemrosesan Data Dilakukan Di Server Pusat (Internet)." },
  { "en": "Apa Keuntungan Komputasi Tepi (Edge Computing)?", "id": "Respon Cepat (Latensi Rendah), Privasi, Hemat Bandwidth." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka (Jembatan) Komunikasi Antar Perangkat Lunak." },
  { "en": "Apa Itu REST API (Representational State Transfer)?", "id": "Gaya Arsitektur API Populer (Menggunakan Metode HTTP)." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Teks Pertukaran Data Ringan (Mudah Dibaca)." },
  { "en": "Apa Itu Firmware Over-The-Air (FOTA)?", "id": "Memperbarui Perangkat Lunak (Firmware) Perangkat IoT Nirkabel." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Versi Bluetooth Hemat Daya (Untuk Perangkat IoT/Baterai)." },
  { "en": "Apa Itu Zigbee?", "id": "Protokol Nirkabel (Daya Rendah, Jaringan Mesh) Untuk IoT." },
  { "en": "Apa Itu Z-Wave?", "id": "Protokol Nirkabel (Daya Rendah) Populer Untuk Otomasi Rumah." },
  { "en": "Apa Itu LoRaWAN (Long Range Wide Area Network)?", "id": "Protokol Jaringan Nirkabel (Jarak Sangat Jauh, Daya Rendah)." },
  { "en": "Apa Itu Tes TDR (Time-Domain Reflectometer)?", "id": "Menguji Lokasi Kerusakan (Putus) Kabel." },
  { "en": "Apa Itu Tes Hipot (High Potential)?", "id": "Menguji Ketahanan Isolasi Kabel (Tegangan Tinggi)." },
  { "en": "Apa Itu Uji Resistansi Isolasi (Megger)?", "id": "Mengukur Nilai Resistansi Isolasi (Mega Ohm)." },
  { "en": "Apa Itu Pengujian Tan Delta (Tan Delta Test)?", "id": "Mengukur Kualitas (Faktor Disipasi) Isolasi." },
  { "en": "Apa Itu Pengujian Pelepasan Sebagian (Partial Discharge)?", "id": "Mendeteksi Pelepasan Listrik Kecil Di Isolasi." },
  { "en": "Apa Itu Uji Impedansi Baterai?", "id": "Mengukur Kesehatan Internal Baterai (Impedansi Internal)." },
  { "en": "Apa Itu Sekring HRC (High Rupturing Capacity)?", "id": "Sekring Kapasitas Pemutusan Arus Sangat Tinggi." },
  { "en": "Apa Itu Sistem Busbar Trunking (Busduct)?", "id": "Sistem Distribusi Daya (Busbar Dalam Selubung Logam)." },
  { "en": "Apa Itu Baki Kabel (Cable Tray)?", "id": "Struktur Jalur Penopang (Instalasi) Kabel Listrik." },
  { "en": "Apa Itu Baki Kabel Tipe Tangga (Ladder)?", "id": "Baki Kabel Berbentuk Tangga (Untuk Kabel Berat)." },
  { "en": "Apa Itu Baki Kabel Tipe Berlubang (Perforated)?", "id": "Baki Kabel (Pelat Berlubang) Untuk Ventilasi." },
  { "en": "Apa Itu Baki Kabel Tipe Solid (Solid Bottom)?", "id": "Baki Kabel (Pelat Utuh) Melindungi Kabel Sensitif." },
  { "en": "Apa Itu Busbar Pembumian (Grounding Busbar)?", "id": "Batang Tembaga Titik Kumpul Koneksi Ground." },
  { "en": "Apa Itu Ikatan Netral-Tanah (Neutral-Ground Bond)?", "id": "Sambungan Tunggal Antara Netral Dan Ground (Di Panel)." },
  { "en": "Mengapa Netral Dan Ground Diikat (Bonding)?", "id": "Memberi Jalur Balik Arus Gangguan (Trip Breaker)." },
  { "en": "Apa Itu Sistem TN-S (Terra Neutral-Separated)?", "id": "Sistem (Netral Dan Ground Terpisah Sepenuhnya)." },
  { "en": "Apa Itu Sistem TN-C (Terra Neutral-Combined)?", "id": "Sistem (Netral Dan Ground Digabung Menjadi PEN)." },
  { "en": "Apa Itu Sistem TT (Terra-Terra)?", "id": "Sistem (Ground Sumber Dan Beban Terpisah)." },
  { "en": "Apa Itu Sistem IT (Isolated Terra)?", "id": "Sistem (Tidak Ada Ground Langsung Di Sumber)." },
  { "en": "Apa Itu Pakaian Anti-Statis (ESD Smock)?", "id": "Jubah Pelindung Mencegah Listrik Statis Tubuh." },
  { "en": "Apa Itu Sepatu Anti-Statis (ESD Shoes)?", "id": "Sepatu Menyalurkan Muatan Statis Tubuh Ke Lantai." },
  { "en": "Apa Itu Lantai ESD (ESD Floor)?", "id": "Lantai Konduktif/Disipatif Menyalurkan Muatan Statis." },
  { "en": "Apa Itu Gelang Anti-Statis (ESD Wrist Strap)?", "id": "Gelang Menyalurkan Muatan Statis (Terhubung Ke Ground)." },
  { "en": "Apa Itu Ionizer (Ion Blower)?", "id": "Menetralkan Muatan Statis Di Udara (Bahan Isolator)." },
  { "en": "Apa Itu Panci Solder (Solder Pot)?", "id": "Wadah Pemanas Timah Cair (Mencelup Kabel)." },
  { "en": "Apa Itu Pemanas Awal (Pre-Heater) PCB?", "id": "Pemanas PCB Dari Bawah (Mencegah Kejut Termal)." },
  { "en": "Apa Itu Kejut Termal (Thermal Shock) PCB?", "id": "Kerusakan PCB Akibat Perubahan Suhu Mendadak." },
  { "en": "Apa Itu Pembungkus Kawat (Wire Wrap)?", "id": "Teknik Sambungan Kawat (Melilit Di Tiang)." },
  { "en": "Apa Itu Alat Wire Wrap?", "id": "Alat Khusus Melilitkan Kawat Ke Tiang (Post)." },
  { "en": "Apa Itu Alat Un-Wrap (Pembuka Lilitan)?", "id": "Alat Melepas Lilitan Kawat (Wire Wrap)." },
  { "en": "Apa Itu Penanda Kabel (Cable Marker)?", "id": "Label (Angka/Huruf) Identifikasi Kabel." },
  { "en": "Apa Itu Pita Isolasi (Electrical Tape)?", "id": "Pita Perekat (Vinyl) Mengisolasi Sambungan Listrik." },
  { "en": "Apa Itu Isolasi Cair (Liquid Electrical Tape)?", "id": "Cairan Isolasi (Mengeras Menjadi Karet)." },
  { "en": "Apa Itu Gemuk Dielektrik (Dielectric Grease)?", "id": "Gemuk Non-Konduktif (Pelindung Korosi Konektor)." },
  { "en": "Apa Itu Gemuk Konduktif (Conductive Grease)?", "id": "Gemuk Menghantar Listrik (Sambungan Busbar Aluminium)." },
  { "en": "Apa Itu Penyambung Kabel (Wire Connector/Nut)?", "id": "Konektor Putar (Mur) Menyambung Ujung Kabel." },
  { "en": "Apa Itu Konektor Tusuk (Push-In Connector)?", "id": "Konektor Cepat (Kabel Hanya Ditusuk Masuk)." },
  { "en": "Apa Itu Konektor Wago (Wago Connector)?", "id": "Merek Populer Konektor Tusuk Atau Tuas (Lever)." },
  { "en": "Apa Itu Konektor Scotchlok (Scotchlok Connector)?", "id": "Konektor Penjepit (IDC) Menyambung Kabel Telepon/Data." },
  { "en": "Apa Itu Konektor Sekop (Spade Connector)?", "id": "Konektor Terminal Berbentuk Garpu (Sekop)." },
  { "en": "Apa Itu Konektor Cincin (Ring Connector)?", "id": "Konektor Terminal Berbentuk Cincin (Untuk Baut)." },
  { "en": "Apa Itu Konektor Peluru (Bullet Connector)?", "id": "Konektor (Jantan/Betina) Berbentuk Peluru (Mudah Dilepas)." },
  { "en": "Apa Itu Konektor Quick Disconnect?", "id": "Konektor Pipih (Jantan/Betina) Mudah Dilepas Pasang." },
  { "en": "Apa Itu Konektor Anderson (Anderson Powerpole)?", "id": "Konektor Modular (Netral Gender) Arus DC Besar." },
  { "en": "Apa Itu Konektor XT60?", "id": "Konektor (Kuning) Populer Arus DC Besar (Baterai RC/Drone)." },
  { "en": "Apa Itu Konektor JST (Japan Solderless Terminal)?", "id": "Konektor Putih Kecil (Umum Di PCB Elektronik)." },
  { "en": "Apa Itu Konektor Molex?", "id": "Konektor (Umumnya Putih) Catu Daya Internal Komputer." },
  { "en": "Apa Itu Konektor SATA (Serial AT Attachment)?", "id": "Konektor Standar (Data Dan Daya) Hard Drive." },
  { "en": "Apa Itu Konektor D-Sub (D-Subminiature)?", "id": "Konektor (Bentuk D) Pin Banyak (VGA, RS-232)." },
  { "en": "Apa Itu Konektor VGA (Video Graphics Array)?", "id": "Konektor D-Sub 15-Pin (Standar Video Analog)." },
  { "en": "Apa Itu Konektor RS-232 (Recommended Standard 232)?", "id": "Konektor D-Sub 9-Pin (Komunikasi Serial Lama)." },
  { "en": "Apa Itu Konektor Centronics?", "id": "Konektor Besar (Jepit) Standar Printer Paralel Lama." },
  { "en": "Apa Itu Konektor USB (Universal Serial Bus)?", "id": "Konektor Standar Universal (Data Dan Daya)." },
  { "en": "Apa Itu USB Tipe A (Type-A)?", "id": "Konektor USB Standar (Persegi Panjang) Di Host (PC)." },
  { "en": "Apa Itu USB Tipe B (Type-B)?", "id": "Konektor USB (Persegi) Di Perangkat (Printer, Arduino)." },
  { "en": "Apa Itu USB Mini (Mini-USB)?", "id": "Konektor USB Kecil (Versi Lama, Kamera Digital)." },
  { "en": "Apa Itu USB Mikro (Micro-USB)?", "id": "Konektor USB Sangat Kecil (Handphone Android Lama)." },
  { "en": "Apa Itu USB Tipe C (Type-C)?", "id": "Konektor USB Kecil Reversibel (Bisa Dibalik)." },
  { "en": "Apa Itu USB OTG (On-The-Go)?", "id": "Fitur Handphone Membaca Perangkat USB (Flash Drive)." },
  { "en": "Apa Itu Konektor HDMI (High-Definition Multimedia Interface)?", "id": "Konektor Standar Transmisi Audio Video Digital." },
  { "en": "Apa Itu DisplayPort?", "id": "Konektor Standar Audio Video Digital (Alternatif HDMI)." },
  { "en": "Apa Itu Konektor RJ11 (Registered Jack 11)?", "id": "Konektor Kecil (4/6 Pin) Untuk Saluran Telepon." },
  { "en": "Apa Itu Konektor RJ45 (Registered Jack 45)?", "id": "Konektor Besar (8 Pin) Untuk Jaringan Ethernet (LAN)." },
  { "en": "Apa Itu Konektor Audio 3.5 Mm (TRS)?", "id": "Konektor Jack (Colokan) Umum Headphone/Earphone." },
  { "en": "Apa Kepanjangan TRS (Tip-Ring-Sleeve)?", "id": "Tip (Ujung), Ring (Cincin), Sleeve (Selubung)." },
  { "en": "Apa Itu Konektor TS (Tip-Sleeve)?", "id": "Konektor Jack Mono (Contoh: Jack Gitar Listrik)." },
  { "en": "Apa Itu Konektor TRRS (Tip-Ring-Ring-Sleeve)?", "id": "Konektor Jack (Empat Bagian) Headset (Termasuk Mikrofon)." },
  { "en": "Apa Itu Konektor RCA (Cinch)?", "id": "Konektor Audio/Video Analog (Kuning, Merah, Putih)." },
  { "en": "Apa Itu Konektor F (F-Type)?", "id": "Konektor Ulir (Drat) Untuk Kabel Antena TV Koaksial." },
  { "en": "Apa Itu Konektor BNC (Bayonet Neill-Concelman)?", "id": "Konektor Koaksial RF (Putar-Kunci) Untuk Osiloskop/CCTV." },
  { "en": "Apa Itu Konektor SMA (SubMiniature Version A)?", "id": "Konektor Koaksial RF Ulir Kecil (Antena Wi-Fi/GPS)." },
  { "en": "Apa Itu Konektor Tipe N (N-Type)?", "id": "Konektor Koaksial RF Ulir Besar (Daya Tinggi)." },
  { "en": "Apa Itu Konektor XLR (Cannon X)?", "id": "Konektor Tiga Pin (Audio Profesional Seimbang/Balanced)." },
  { "en": "Apa Itu Konektor Speakon (Speakon)?", "id": "Konektor Pengunci (Twist-Lock) Daya Tinggi (Speaker Profesional)." },
  { "en": "Apa Itu Blok Terminal PCB (PCB Terminal Block)?", "id": "Konektor Sekrup Hijau/Biru Di Papan PCB." },
  { "en": "Apa Itu Pin Header (Header Pin)?", "id": "Barisan Pin Jantan (Tusuk) Di PCB (Arduino)." },
  { "en": "Apa Itu Soket IC (IC Socket)?", "id": "Dudukan (Rumah) IC Tipe DIP Di PCB." },
  { "en": "Apa Fungsi Soket IC (IC Socket)?", "id": "Memudahkan Melepas/Mengganti IC Tanpa Menyolder Ulang." },
  { "en": "Apa Itu Soket ZIF (Zero Insertion Force)?", "id": "Soket (Tuas Pengunci) Memasang IC Tanpa Tekanan." },
  { "en": "Di Mana Soket ZIF (Zero Insertion Force) Digunakan?", "id": "Alat Pemrogram IC (EPROM Programmer), CPU Lama." },
  { "en": "Apa Itu Konektor Tepi (Edge Connector)?", "id": "Konektor (Jalur Emas) Di Tepi PCB (RAM)." },
  { "en": "Apa Itu Slot Ekspansi (Expansion Slot)?", "id": "Soket Di Motherboard (Tempat Kartu Grafis/PCIe)." },
  { "en": "Apa Itu Kabel Pita (Ribbon Cable)?", "id": "Kabel Pipih (Banyak Jalur Paralel)." },
  { "en": "Apa Itu Konektor IDC (Insulation Displacement Connector)?", "id": "Konektor Menjepit (Menusuk Isolasi) Kabel Pita." },
  { "en": "Apa Itu Stand-Off (Spacer) PCB?", "id": "Penyangga (Plastik/Logam) Memberi Jarak PCB Ke Casing." },
  { "en": "Apa Itu Relai Buluh (Reed Relay)?", "id": "Relai Menggunakan Saklar Buluh (Reed Switch) Mini." },
  { "en": "Apa Itu Saklar Buluh (Reed Switch)?", "id": "Saklar Kaca Mini (Kontak Tertutup Oleh Magnet)." },
  { "en": "Apa Itu Relai Merkuri (Mercury Relay)?", "id": "Relai Menggunakan Merkuri (Air Raksa) Sebagai Kontak." },
  { "en": "Apa Itu Relai Latching (Latching Relay)?", "id": "Relai Tetap Di Posisinya (Tanpa Daya Koil)." },
  { "en": "Bagaimana Relai Latching Bekerja?", "id": "Butuh Pulsa (Set) Untuk Aktif, Pulsa (Reset) Untuk Mati." },
  { "en": "Apa Itu Relai Waktu (Time Delay Relay/TDR)?", "id": "Relai Dengan Tundaan Waktu (Timer) Internal." },
  { "en": "Apa Itu Relai On-Delay?", "id": "Relai Tundaan Waktu Saat Diberi Daya (On)." },
  { "en": "Apa Itu Relai Off-Delay?", "id": "Relai Tundaan Waktu Saat Daya Dimatikan (Off)." },
  { "en": "Apa Itu Kontak Kering (Dry Contact)?", "id": "Kontak Relai (Saklar) Tanpa Sumber Tegangan Internal." },
  { "en": "Apa Itu Kontak Basah (Wet Contact)?", "id": "Kontak Relai (Saklar) Yang Menyediakan Sumber Tegangan." },
  { "en": "Apa Itu Gelang Kaki ESD (ESD Heel Strap)?", "id": "Dipakai Di Sepatu (Terhubung Ke Lantai ESD)." },
  { "en": "Apa Itu Isolator Rantai (String Insulator)?", "id": "Rangkaian Piringan Isolator (Keramik/Kaca) Gantung." },
  { "en": "Apa Itu Isolator Pasak (Pin Insulator)?", "id": "Isolator (Duduk Di Atas) Tiang Distribusi." },
  { "en": "Apa Itu Isolator Pos (Post Insulator)?", "id": "Isolator Kaku (Berdiri) Untuk Busbar Gardu Induk." },
  { "en": "Apa Itu Bushing (Bushing) Trafo?", "id": "Isolator Melewatkan Konduktor Keluar Tangki Trafo." },
  { "en": "Apa Itu Cincin Korona (Corona Ring)?", "id": "Cincin Logam (Mencegah Korona) Di Isolator HV." },
  { "en": "Apa Itu Pelepasan Korona (Corona Discharge)?", "id": "Pelepasan Listrik Sebagian (Berdesis) Di Udara." },
  { "en": "Apa Itu Jarak Rambat (Creepage Distance) Isolator?", "id": "Jarak Terpendek Sepanjang Permukaan Isolator." },
  { "en": "Apa Itu Jarak Loncatan (Flashover Distance) Isolator?", "id": "Jarak Terpendek Lewat Udara (Loncatan Api)." },
  { "en": "Apa Itu Menara Transmisi (Transmission Tower)?", "id": "Struktur Tiang Penyangga Kabel Transmisi Tegangan Tinggi." },
  { "en": "Apa Itu Lengan (Cross-Arm) Menara Transmisi?", "id": "Lengan Horizontal Tempat Menggantung Isolator Rantai." },
  { "en": "Apa Itu Kawat Tanah (Ground Wire/Shield Wire)?", "id": "Kawat Paling Atas (Pelindung Petir) Transmisi." },
  { "en": "Apa Itu Peredam Getaran (Vibration Damper) Kabel?", "id": "Alat Meredam Getaran (Aeolian) Pada Kabel Transmisi." },
  { "en": "Apa Itu Spacer (Spacer) Kabel Transmisi?", "id": "Alat Menjaga Jarak Antar Kabel Fasa (Bundel)." },
  { "en": "Apa Itu Kabel Bundel (Bundled Conductor)?", "id": "Dua (Atau Lebih) Konduktor Per Fasa (Mengurangi Korona)." },
  { "en": "Apa Kepanjangan SUTET (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Saluran Udara Tegangan Ekstra Tinggi (Contoh: 500 KV)." },
  { "en": "Apa Kepanjangan SUTT (Saluran Udara Tegangan Tinggi)?", "id": "Saluran Udara Tegangan Tinggi (Contoh: 150 KV)." },
  { "en": "Apa Kepanjangan SUTM (Saluran Udara Tegangan Menengah)?", "id": "Saluran Udara Tegangan Menengah (Contoh: 20 KV)." },
  { "en": "Apa Kepanjangan SUTR (Saluran Udara Tegangan Rendah)?", "id": "Saluran Udara Tegangan Rendah (Contoh: 220 V)." },
  { "en": "Apa Itu Jaringan Distribusi Radial?", "id": "Sistem Distribusi (Satu Arah) Menuju Pelanggan." },
  { "en": "Apa Itu Jaringan Distribusi Cincin (Ring Main)?", "id": "Sistem Distribusi (Loop) Lebih Andal (Dua Arah)." },
  { "en": "Apa Itu Recloser (Penutup Balik Otomatis)?", "id": "Pemutus (CB) Mencoba Menyambung Ulang (Otomatis)." },
  { "en": "Apa Fungsi Recloser?", "id": "Mengatasi Gangguan Sesaat (Pohon Tumbang) Di Jaringan Distribusi." },
  { "en": "Apa Itu Sectionalizer?", "id": "Saklar Otomatis (Mengisolasi Bagian Jaringan Yang Gangguan)." },
  { "en": "Apa Itu Kapasitor Seri (Series Capacitor) Transmisi?", "id": "Mengurangi Reaktansi Induktif Total Saluran (Jarak Jauh)." },
  { "en": "Apa Itu Reaktor Shunt (Shunt Reactor) Transmisi?", "id": "Menyerap Daya Reaktif Berlebih (Efek Ferranti)." },
  { "en": "Apa Itu Efek Ferranti (Ferranti Effect)?", "id": "Tegangan Ujung Penerima Lebih Tinggi Dari Pengirim." },
  { "en": "Kapan Efek Ferranti Terjadi?", "id": "Saluran Transmisi Panjang Saat Beban Sangat Ringan." },
  { "en": "Apa Itu Transposisi (Transposition) Saluran Transmisi?", "id": "Menukar Posisi Fisik Kabel Fasa (R-S-T)." },
  { "en": "Mengapa Saluran Transmisi Perlu Transposisi?", "id": "Menyeimbangkan Nilai Induktansi Dan Kapasitansi Antar Fasa." },
  { "en": "Apa Itu Generator Impuls (Impulse Generator)?", "id": "Pembangkit Tegangan Kejut Tinggi (Simulasi Petir)." },
  { "en": "Apa Itu Uji Impuls (Impulse Test)?", "id": "Menguji Ketahanan Isolasi Terhadap Tegangan Kejut Petir." },
  { "en": "Apa Itu Uji Tegangan Tembus (Breakdown Test)?", "id": "Menguji Tegangan Maksimum Isolator (Sampai Tembus/Rusak)." },
  { "en": "Apa Itu Pita Valensi (Valence Band)?", "id": "Pita Energi (Orbit) Elektron Terluar." },
  { "en": "Apa Itu Pita Konduksi (Conduction Band)?", "id": "Pita Energi (Orbit) Elektron Bebas (Bisa Bergerak)." },
  { "en": "Apa Itu Celah Energi (Energy Gap/Band Gap)?", "id": "Celah Antara Pita Valensi Dan Pita Konduksi." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Konduktor?", "id": "Tidak Ada (Pita Tumpang Tindih), Elektron Bebas." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Isolator?", "id": "Sangat Lebar, Elektron Sulit Melompat (Tidak Menghantar)." },
  { "en": "Bagaimana Celah Energi (Energy Gap) Semikonduktor?", "id": "Sempit, Elektron Bisa Melompat (Jika Ada Energi)." },
  { "en": "Apa Itu Elektron (Electron)?", "id": "Pembawa Muatan Negatif." },
  { "en": "Apa Itu Lubang (Hole)?", "id": "Kekosongan Elektron (Dianggap Pembawa Muatan Positif)." },
  { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni (Silikon Murni, Tanpa Doping)." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Yang Diberi Doping (Impuritas)." },
  { "en": "Apa Itu Doping Tipe-N (Donor)?", "id": "Menambah Impuritas (Pentavalen) Menghasilkan Kelebihan Elektron." },
  { "en": "Apa Itu Doping Tipe-P (Akseptor)?", "id": "Menambah Impuritas (Trivalen) Menghasilkan Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu Pembawa Muatan Mayoritas Tipe-N?", "id": "Elektron (Electron)." },
  { "en": "Apa Itu Pembawa Muatan Mayoritas Tipe-P?", "id": "Lubang (Hole)." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Gabungan Material Tipe P Dan Tipe N." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Netral (Tanpa Pembawa Muatan) Di Sambungan P-N." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Tegangan Positif Ke P, Negatif Ke N (Arus Mengalir)." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Tegangan Positif Ke N, Negatif Ke P (Arus Dihambat)." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir (PLTN)?", "id": "Pembangkit Uap (PLTU) Menggunakan Panas Reaksi Nuklir." },
  { "en": "Apa Itu Reaksi Fisi (Fission) Nuklir?", "id": "Pemecahan Inti Atom Berat (Uranium) Melepas Energi." },
  { "en": "Apa Itu Reaksi Fusi (Fusion) Nuklir?", "id": "Penggabungan Inti Atom Ringan (Hidrogen) Melepas Energi." },
  { "en": "Apa Sumber Energi Matahari?", "id": "Reaksi Fusi (Fusion) Nuklir." },
  { "en": "Apa Itu Batang Kendali (Control Rods) Reaktor?", "id": "Menyerap Neutron (Mengendalikan Laju Reaksi Fisi)." },
  { "en": "Apa Itu Moderator (Moderator) Reaktor?", "id": "Memperlambat Neutron (Agar Efisien Membelah Uranium)." },
  { "en": "Apa Itu Pendingin (Coolant) Reaktor?", "id": "Cairan (Air/Gas) Mengambil Panas Dari Inti Reaktor." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Mengubah Energi Potensial Air Menjadi Listrik." },
  { "en": "Apa Itu Bendungan (Dam) PLTA?", "id": "Membendung Air (Menciptakan Waduk/Reservoir)." },
  { "en": "Apa Itu Pipa Pesat (Penstock) PLTA?", "id": "Pipa Besar Menyalurkan Air (Tekanan Tinggi) Ke Turbin." },
  { "en": "Apa Itu Turbin Air (Water Turbine)?", "id": "Kincir (Roda) Diputar Oleh Aliran Air." },
  { "en": "Apa Itu Turbin Kaplan?", "id": "Turbin Air (Baling-Baling) Untuk Aliran Besar, Jatuh Rendah." },
  { "en": "Apa Itu Turbin Francis?", "id": "Turbin Air Paling Umum (Jatuh Dan Aliran Sedang)." },
  { "en": "Apa Itu Turbin Pelton?", "id": "Turbin Air (Mangkuk) Untuk Tekanan Tinggi, Jatuh Tinggi." },
  { "en": "Apa Itu PLTA Tipe Waduk (Reservoir)?", "id": "PLTA Menggunakan Bendungan Besar (Menyimpan Air)." },
  { "en": "Apa Itu PLTA Tipe Run-Of-River?", "id": "PLTA Mengandalkan Aliran Sungai Langsung (Tanpa Waduk Besar)." },
  { "en": "Apa Itu PLTA Pompa (Pumped Storage)?", "id": "Memompa Air Ke Atas (Beban Rendah), Melepas (Beban Puncak)." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gelombang (Wave Power)?", "id": "Menghasilkan Listrik Dari Gerakan Naik Turun Gelombang Laut." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Pasang Surut (Tidal Power)?", "id": "Menghasilkan Listrik Dari Arus Pasang Surut Air Laut." },
  { "en": "Apa Itu Energi Biomassa (Biomass)?", "id": "Energi Dari Bahan Organik (Kayu, Cangkang Sawit, Sampah)." },
  { "en": "Apa Itu Biogas (Biogas)?", "id": "Gas Metana (CH4) Hasil Fermentasi Anaerobik Limbah Organik." },
  { "en": "Apa Itu Biodiesel (Biodiesel)?", "id": "Bahan Bakar Diesel Dibuat Dari Minyak Nabati/Hewani." },
  { "en": "Apa Itu Bioetanol (Bioethanol)?", "id": "Alkohol (Etanol) Dibuat Dari Fermentasi Tanaman (Tebu, Jagung)." },
  { "en": "Apa Itu Keamanan Fungsional (Functional Safety)?", "id": "Sistem Keselamatan Yang Bergantung Pada Fungsi Benar." },
  { "en": "Apa Itu Tingkat Integritas Keselamatan (SIL)?", "id": "Ukuran Kinerja (Keandalan) Fungsi Keselamatan (SIS)." },
  { "en": "Apa Kepanjangan SIL (Safety Integrity Level)?", "id": "Tingkat Integritas Keselamatan (SIL 1 Hingga 4)." },
  { "en": "Apa Itu Probabilitas Kegagalan (Probability of Failure on Demand/PFD)?", "id": "Probabilitas Sistem Keselamatan Gagal Saat Dibutuhkan." },
  { "en": "Apa Itu Standar IEC 61508?", "id": "Standar Internasional Induk Untuk Keamanan Fungsional." },
  { "en": "Apa Itu Standar IEC 61511?", "id": "Standar Keamanan Fungsional (SIS) Untuk Industri Proses." },
  { "en": "Apa Itu Standar ISO 26262?", "id": "Standar Keamanan Fungsional Untuk Industri Otomotif (Mobil)." },
  { "en": "Apa Kepanjangan ASIL (Automotive Safety Integrity Level)?", "id": "Tingkat Integritas Keselamatan Otomotif (A Hingga D)." },
  { "en": "Apa Itu Analisis Bahaya Dan Risiko (HARA)?", "id": "Proses Identifikasi Bahaya (Penentuan ASIL) Di Otomotif." },
  { "en": "Apa Itu Protokol CAN (Controller Area Network)?", "id": "Protokol Jaringan Kuat (Robust) Untuk Kendaraan (Otomotif)." },
  { "en": "Apa Itu Protokol LIN (Local Interconnect Network)?", "id": "Protokol Jaringan Sederhana (Murah) Di Otomotif." },
  { "en": "Apa Aplikasi Protokol LIN?", "id": "Saklar Pintu, Sensor Sederhana, Kontrol Iklim (AC)." },
  { "en": "Apa Itu Protokol FlexRay?", "id": "Protokol Jaringan Otomotif Cepat, Deterministik (Rem, Kemudi)." },
  { "en": "Apa Itu Protokol MOST (Media Oriented Systems Transport)?", "id": "Protokol Jaringan Serat Optik (Multimedia/Infotainment) Mobil." },
  { "en": "Apa Itu Ethernet Otomotif (Automotive Ethernet)?", "id": "Jaringan Ethernet (Berbasis UTP) Untuk Aplikasi Mobil Modern." },
  { "en": "Apa Kepanjangan ECU (Electronic Control Unit)?", "id": "Unit Kontrol Elektronik (Komputer) Di Mobil." },
  { "en": "Apa Kepanjangan OBD-II (On-Board Diagnostics II)?", "id": "Port Standar Diagnostik (Scan) Masalah Mobil." },
  { "en": "Apa Itu DTC (Diagnostic Trouble Code)?", "id": "Kode Kesalahan Yang Disimpan Oleh ECU (OBD-II)." },
  { "en": "Apa Itu Sistem Rem ABS (Anti-Lock Braking System)?", "id": "Mencegah Roda Terkunci Saat Pengereman Mendadak." },
  { "en": "Apa Itu Kontrol Traksi (Traction Control System/TCS)?", "id": "Mencegah Roda Selip Saat Akselerasi (Berputar)." },
  { "en": "Apa Itu Kontrol Stabilitas Elektronik (ESC/ESP)?", "id": "Membantu Menjaga Stabilitas Mobil (Mencegah Tergelincir)." },
  { "en": "Apa Itu Sistem Drive-By-Wire?", "id": "Kontrol (Gas, Rem) Elektronik (Tanpa Kabel Mekanis)." },
  { "en": "Apa Itu Sistem Throttle-By-Wire?", "id": "Kontrol Katup Gas (Throttle) Secara Elektronik." },
  { "en": "Apa Itu Pengujian HIL (Hardware-in-the-Loop)?", "id": "Menguji ECU (Hardware) Dengan Simulasi Lingkungan Virtual." },
  { "en": "Apa Itu Pengujian SIL (Software-in-the-Loop)?", "id": "Menguji Algoritma (Software) Dalam Lingkungan Simulasi Murni." },
  { "en": "Apa Itu Pengujian MIL (Model-in-the-Loop)?", "id": "Menguji Model (Simulink) Dalam Lingkungan Simulasi." },
  { "en": "Apa Itu Kalibrasi ECU?", "id": "Proses Penyetelan (Tuning) Parameter Peta (Map) ECU." },
  { "en": "Apa Itu Peta (Map) ECU?", "id": "Tabel Data (Lookup Table) Parameter Operasi Mesin." },
  { "en": "Apa Itu Korelasi Silang (Cross-Correlation)?", "id": "Mengukur Kemiripan Dua Sinyal Berbeda Waktu." },
  { "en": "Apa Itu Korelasi Otomatis (Auto-Correlation)?", "id": "Mengukur Kemiripan Sinyal Dengan Versi Tundanya." },
  { "en": "Apa Itu Sinyal Deterministik (Deterministic Signal)?", "id": "Sinyal Yang Dapat Diprediksi (Rumus Matematika)." },
  { "en": "Apa Itu Sinyal Acak (Stochastic Signal)?", "id": "Sinyal Yang Tidak Dapat Diprediksi (Noise)." },
  { "en": "Apa Itu Sinyal Stasioner (Stationary Signal)?", "id": "Sinyal Acak Yang Sifat Statistiknya Konstan." },
  { "en": "Apa Itu Sinyal Ergodik (Ergodic Signal)?", "id": "Sinyal Stasioner (Rata-Rata Waktu = Rata-Rata Ensemble)." },
  { "en": "Apa Itu Kepadatan Spektral Daya (PSD)?", "id": "Distribusi Kekuatan Sinyal Pada Domain Frekuensi." },
  { "en": "Apa Kepanjangan PSD (Power Spectral Density)?", "id": "Kepadatan Spektral Daya." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform)?", "id": "Generalisasi Transformasi Fourier (Analisis Sistem Kontrol)." },
  { "en": "Apa Fungsi Transformasi Laplace Di Rangkaian?", "id": "Mengubah Persamaan Diferensial Menjadi Aljabar." },
  { "en": "Apa Itu Domain-S (S-Domain)?", "id": "Domain Frekuensi Kompleks (Hasil Transformasi Laplace)." },
  { "en": "Apa Itu Kutub (Pole) Fungsi Alih?", "id": "Nilai 'S' Yang Membuat Fungsi Alih Tak Terhingga." },
  { "en": "Apa Itu Nol (Zero) Fungsi Alih?", "id": "Nilai 'S' Yang Membuat Fungsi Alih Nol." },
  { "en": "Apa Pengaruh Kutub (Pole) Pada Stabilitas?", "id": "Kutub Di Kiri (Stabil), Kanan (Tidak Stabil)." },
  { "en": "Apa Itu Filter Butterworth (Butterworth Filter)?", "id": "Filter Dengan Respon Paling Datar (Flat) Di Passband." },
  { "en": "Apa Ciri Filter Butterworth?", "id": "Respon Passband Sangat Datar, Roll-Off Sedang." },
  { "en": "Apa Itu Filter Chebyshev (Chebyshev Filter)?", "id": "Filter Dengan Roll-Off Tajam (Tepi Curam)." },
  { "en": "Apa Ciri Filter Chebyshev?", "id": "Roll-Off Tajam, Tetapi Memiliki Riak (Ripple) Passband." },
  { "en": "Apa Itu Filter Bessel (Bessel Filter)?", "id": "Filter Dengan Respon Fasa Linear (Tunda Grup Konstan)." },
  { "en": "Apa Ciri Filter Bessel?", "id": "Menjaga Bentuk Gelombang Sinyal (Tunda Grup Konstan)." },
  { "en": "Apa Itu Filter Elliptic (Elliptic/Cauer Filter)?", "id": "Filter Dengan Roll-Off Paling Tajam (Sangat Curam)." },
  { "en": "Apa Ciri Filter Elliptic?", "id": "Roll-Off Sangat Tajam, Riak Di Passband Dan Stopband." },
  { "en": "Apa Itu Filter Adaptif (Adaptive Filter)?", "id": "Filter Yang Dapat Mengubah Parameternya Sendiri." },
  { "en": "Apa Itu Algoritma LMS (Least Mean Squares)?", "id": "Algoritma Umum Untuk Filter Adaptif." },
  { "en": "Apa Itu Filter Kalman (Kalman Filter)?", "id": "Algoritma Optimal (Memprediksi Status, Menghilangkan Noise)." },
  { "en": "Apa Fungsi Filter Kalman?", "id": "Estimasi Status Sistem (Navigasi, Pelacakan Objek)." },
  { "en": "Apa Itu Transformasi Wavelet (Wavelet Transform)?", "id": "Analisis Sinyal (Representasi Waktu Dan Frekuensi Lokal)." },
  { "en": "Apa Beda Wavelet Dan Fourier?", "id": "Fourier (Frekuensi Global), Wavelet (Frekuensi Lokal Waktu)." },
  { "en": "Apa Itu Foton (Photon)?", "id": "Partikel Kuantum Dasar Cahaya (Energi Elektromagnetik)." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel (Wave-Particle Duality)?", "id": "Cahaya (Foton) Bersifat Gelombang Dan Partikel." },
  { "en": "Apa Itu Cahaya Tampak (Visible Light)?", "id": "Spektrum Elektromagnetik Yang Terlihat Mata Manusia (Pelangi)." },
  { "en": "Apa Itu Sinar Inframerah (Infrared/IR)?", "id": "Gelombang Elektromagnetik Di Bawah Frekuensi Merah (Panas)." },
  { "en": "Apa Itu Sinar Ultraviolet (Ultraviolet/UV)?", "id": "Gelombang Elektromagnetik Di Atas Frekuensi Ungu." },
  { "en": "Apa Itu Sinar-X (X-Ray)?", "id": "Gelombang Elektromagnetik Frekuensi Sangat Tinggi (Medis, Tembus)." },
  { "en": "Apa Itu Sinar Gamma (Gamma Ray)?", "id": "Gelombang Elektromagnetik Paling Energetik (Dari Radioaktif)." },
  { "en": "Apa Itu Emisi Spontan (Spontaneous Emission)?", "id": "Atom Melepas Foton Secara Acak (Cahaya Normal/LED)." },
  { "en": "Apa Itu Emisi Terstimulasi (Stimulated Emission)?", "id": "Foton Datang Memicu Atom Melepas Foton Identik (Laser)." },
  { "en": "Apa Itu Inversi Populasi (Population Inversion)?", "id": "Kondisi (Atom Siap Melepas Foton) Untuk Membuat Laser." },
  { "en": "Apa Itu Media Penguatan (Gain Medium) Laser?", "id": "Material (Gas, Kristal) Tempat Terjadinya Emisi Terstimulasi." },
  { "en": "Apa Itu Resonator Optik (Optical Resonator)?", "id": "Dua Cermin (Memantulkan Foton) Membangun Cahaya Laser." },
  { "en": "Apa Itu Laser Gas (Gas Laser)?", "id": "Laser Menggunakan Media Penguatan Gas (Contoh: Helium-Neon/HeNe)." },
  { "en": "Apa Itu Laser Zat Padat (Solid-State Laser)?", "id": "Laser Media Penguatan Kristal/Kaca (Contoh: Nd:YAG)." },
  { "en": "Apa Itu Laser Serat (Fiber Laser)?", "id": "Laser Media Penguatan Serat Optik (Daya Tinggi)." },
  { "en": "Apa Itu Mode Terkunci (Mode-Locking) Laser?", "id": "Teknik Menghasilkan Pulsa Cahaya Sangat Pendek (Ultrafast)." },
  { "en": "Apa Itu Pulsa Ultrafast (Ultrafast Pulse) Laser?", "id": "Pulsa Cahaya Sangat Singkat (Femtoseconds/Picoseconds)." },
  { "en": "Apa Itu Lensa (Lens)?", "id": "Material Optik Transparan (Membelokkan Cahaya)." },
  { "en": "Apa Itu Lensa Konveks (Cembung)?", "id": "Lensa Mengumpulkan (Memfokuskan) Cahaya." },
  { "en": "Apa Itu Lensa Konkaf (Cekung)?", "id": "Lensa Menyebarkan Cahaya." },
  { "en": "Apa Itu Jarak Fokus (Focal Length) Lensa?", "id": "Jarak Lensa Ke Titik Fokus (Api)." },
  { "en": "Apa Itu Cermin (Mirror)?", "id": "Permukaan Reflektif (Memantulkan) Cahaya." },
  { "en": "Apa Itu Refleksi (Refleksi/Pantulan)?", "id": "Cahaya Memantul Dari Permukaan." },
  { "en": "Apa Itu Refraksi (Refraksi/Pembiasan)?", "id": "Cahaya Berbelok Saat Melewati Medium Berbeda." },
  { "en": "Apa Itu Indeks Bias (Refractive Index)?", "id": "Ukuran Seberapa Cepat Cahaya Lewat Medium (Pembiasan)." },
  { "en": "Apa Itu Polarisasi (Polarization) Cahaya?", "id": "Orientasi (Arah Getar) Gelombang Medan Listrik Cahaya." },
  { "en": "Apa Itu Filter Polarisasi (Polarizer)?", "id": "Filter Hanya Meneruskan Cahaya Dengan Polarisasi Tertentu." },
  { "en": "Apa Itu Interferensi (Interference) Cahaya?", "id": "Dua Gelombang Cahaya Bertemu (Menguatkan Atau Melemahkan)." },
  { "en": "Apa Itu Difraksi (Diffraction) Cahaya?", "id": "Cahaya Menyebar Saat Melewati Celah Sempit." },
  { "en": "Apa Itu Kisi Difraksi (Diffraction Grating)?", "id": "Komponen Optik (Banyak Celah) Memisahkan Warna (Spektrum)." },
  { "en": "Apa Itu Spektrometer (Spectrometer)?", "id": "Alat Mengukur Intensitas Cahaya Pada Tiap Warna (Spektrum)." },
  { "en": "Apa Itu Monokromator (Monochromator)?", "id": "Alat Memilih Satu Warna (Panjang Gelombang) Cahaya." },
  { "en": "Apa Itu Kriptografi (Cryptography)?", "id": "Ilmu Teknik Enkripsi (Menyembunyikan Pesan)." },
  { "en": "Apa Itu Teks Biasa (Plaintext)?", "id": "Pesan Asli Yang Dapat Dibaca." },
  { "en": "Apa Itu Teks Sandi (Ciphertext)?", "id": "Pesan Terenkripsi (Tidak Dapat Dibaca)." },
  { "en": "Apa Itu Algoritma Simetris (Symmetric Algorithm)?", "id": "Menggunakan Satu Kunci (Sama) Untuk Enkripsi/Dekripsi." },
  { "en": "Apa Contoh Algoritma Simetris?", "id": "AES (Advanced Encryption Standard), DES (Data Encryption Standard)." },
  { "en": "Apa Kepanjangan AES (Advanced Encryption Standard)?", "id": "Standar Enkripsi Lanjutan." },
  { "en": "Apa Contoh Algoritma Asimetris?", "id": "RSA (Rivest-Shamir-Adleman), ECC (Elliptic Curve Cryptography)." },
  { "en": "Apa Itu Fungsi Hash (Hash Function)?", "id": "Algoritma Satu Arah (Mengubah Data Menjadi Sidik Jari)." },
  { "en": "Apa Sifat Fungsi Hash?", "id": "Satu Arah (Tidak Bisa Dibalik), Ukuran Tetap." },
  { "en": "Apa Contoh Algoritma Hash?", "id": "SHA-256 (Secure Hash Algorithm 256-Bit), MD5 (Message Digest 5)." },
  { "en": "Apa Itu Tanda Tangan Digital (Digital Signature)?", "id": "Membuktikan Keaslian (Otentikasi) Pengirim Pesan (Data)." },
  { "en": "Apa Itu Infrastruktur Kunci Publik (PKI)?", "id": "Sistem (CA, Sertifikat) Mengelola Kunci Publik Asimetris." },
  { "en": "Apa Kepanjangan PKI (Public Key Infrastructure)?", "id": "Infrastruktur Kunci Publik." },
  { "en": "Apa Itu Serangan Brute Force (Brute Force Attack)?", "id": "Serangan Menebak Semua Kemungkinan Kata Sandi (Password)." },
  { "en": "Apa Itu Serangan Man-In-The-Middle (MITM)?", "id": "Serangan (Penyadapan) Di Tengah Jalur Komunikasi." },
  { "en": "Apa Kepanjangan MITM (Man-In-The-Middle)?", "id": "Pria (Orang) Di Tengah." },
  { "en": "Apa Itu Modul Keamanan Perangkat Keras (HSM)?", "id": "Perangkat Keras Khusus (Aman) Mengelola Kunci Kriptografi." },
  { "en": "Apa Kepanjangan HSM (Hardware Security Module)?", "id": "Modul Keamanan Perangkat Keras." },
  { "en": "Apa Fungsi HSM (Hardware Security Module)?", "id": "Penyimpanan Kunci Aman, Akselerasi Kriptografi." },
  { "en": "Apa Itu Trusted Platform Module (TPM)?", "id": "Chip Keamanan (Kriptografi) Di Motherboard Komputer." },
  { "en": "Apa Kepanjangan TPM (Trusted Platform Module)?", "id": "Modul Platform Tepercaya." },
  { "en": "Apa Fungsi TPM (Trusted Platform Module)?", "id": "Menyimpan Kunci Enkripsi (BitLocker), Booting Aman." },
  { "en": "Apa Itu Secure Boot (Booting Aman)?", "id": "Memverifikasi Perangkat Lunak (OS) Sah Saat Booting." },
  { "en": "Apa Itu Secure Element (SE)?", "id": "Chip Aman (Kecil) Di Handphone Atau Kartu Kredit." },
  { "en": "Apa Fungsi Secure Element (SE)?", "id": "Menyimpan Data Sangat Sensitif (Kunci Pembayaran NFC)." },
  { "en": "Apa Itu Saluran Samping (Side-Channel Attack)?", "id": "Serangan (Menganalisis Konsumsi Daya, Waktu) Perangkat Keras." },
  { "en": "Apa Itu Analisis Daya (Power Analysis Attack)?", "id": "Serangan Saluran Samping (Menganalisis Pola Konsumsi Daya)." },
  { "en": "Apa Itu Analisis Waktu (Timing Attack)?", "id": "Serangan Saluran Samping (Menganalisis Waktu Eksekusi Kriptografi)." },
  { "en": "Apa Itu PUF (Physical Unclonable Function)?", "id": "Fungsi Fisik Tak Dapat Klon (Sidik Jari Hardware)." },
  { "en": "Apa Fungsi PUF (Physical Unclonable Function)?", "id": "Menghasilkan Kunci Kriptografi Unik (Anti-Kloning Chip)." },
  { "en": "Apa Itu Generator Angka Acak (RNG)?", "id": "Menghasilkan Angka Acak (Penting Untuk Kunci Kriptografi)." },
  { "en": "Apa Kepanjangan RNG (Random Number Generator)?", "id": "Generator Angka Acak." },
  { "en": "Apa Itu PRNG (Pseudorandom Number Generator)?", "id": "Generator Angka Acak Semu (Berbasis Algoritma/Software)." },
  { "en": "Apa Itu TRNG (True Random Number Generator)?", "id": "Generator Angka Acak Sejati (Berbasis Fenomena Fisik/Hardware)." },
  { "en": "Apa Itu Avionik (Avionics)?", "id": "Sistem Elektronik Di Pesawat Terbang (Navigasi, Komunikasi)." },
  { "en": "Apa Itu Radar (Radar)?", "id": "Deteksi Objek Menggunakan Pantulan Gelombang Radio." },
  { "en": "Apa Kepanjangan Radar (Radio Detection and Ranging)?", "id": "Deteksi Dan Penjangkauan Radio." },
  { "en": "Apa Itu Transponder (Transponder) Pesawat?", "id": "Pemancar Otomatis (Meretransmisi Sinyal) Identitas Pesawat." },
  { "en": "Apa Itu Sistem Fly-By-Wire (FBW)?", "id": "Sistem Kontrol Pesawat Elektronik (Tanpa Mekanis)." },
  { "en": "Apa Itu Autopilot (Autopilot)?", "id": "Sistem Mengendalikan Jalur Terbang Pesawat Secara Otomatis." },
  { "en": "Apa Itu TCAS (Traffic Collision Avoidance System)?", "id": "Sistem Peringatan (Pencegah) Tabrakan Antar Pesawat." },
  { "en": "Apa Kepanjangan ILS (Instrument Landing System)?", "id": "Sistem Pendaratan Instrumen (Bantuan Navigasi Radio)." },
  { "en": "Apa Itu Localizer (Localizer) ILS?", "id": "Sinyal Radio Panduan Horizontal (Kiri/Kanan) Landasan Pacu." },
  { "en": "Apa Itu Glide Slope (Glide Slope) ILS?", "id": "Sinyal Radio Panduan Vertikal (Sudut Turun) Landasan Pacu." },
  { "en": "Apa Itu VOR (VHF Omnidirectional Range)?", "id": "Sistem Navigasi Radio (Arah/Azimuth) Stasiun Darat." },
  { "en": "Apa Itu DME (Distance Measuring Equipment)?", "id": "Peralatan Pengukur Jarak (Pesawat Ke Stasiun Darat)." },
  { "en": "Apa Itu ADC (Air Data Computer)?", "id": "Komputer (Mengolah Data Udara) Kecepatan, Ketinggian." },
  { "en": "Apa Itu Tabung Pitot (Pitot Tube)?", "id": "Sensor Tekanan Udara (Mengukur Kecepatan Udara/Airspeed)." },
  { "en": "Apa Itu Altimeter (Altimeter)?", "id": "Instrumen Pengukur Ketinggian Pesawat (Dari Tekanan Udara)." },
  { "en": "Apa Itu Indikator Kecepatan Vertikal (VSI)?", "id": "Instrumen Mengukur Laju Naik Atau Turun Pesawat." },
  { "en": "Apa Itu Cakrawala Buatan (Artificial Horizon)?", "id": "Instrumen Menampilkan Posisi (Angguk/Guling) Pesawat." },
  { "en": "Apa Itu Tampilan Kokpit Kaca (Glass Cockpit)?", "id": "Tampilan Kokpit Digital (Menggunakan Layar LCD/CRT)." },
  { "en": "Apa Kepanjangan EFIS (Electronic Flight Instrument System)?", "id": "Sistem Instrumen Penerbangan Elektronik." },
  { "en": "Apa Itu Satelit (Satellite)?", "id": "Benda Buatan Mengorbit Bumi (Komunikasi, Navigasi)." },
  { "en": "Apa Itu Orbit Geostasioner (GEO)?", "id": "Orbit (Satelit Tampak Diam) Di Atas Ekuator." },
  { "en": "Apa Itu Orbit Bumi Rendah (LEO)?", "id": "Orbit Rendah (Cepat) Untuk Internet (Contoh: Starlink)." },
  { "en": "Apa Itu Orbit Bumi Menengah (MEO)?", "id": "Orbit Antara LEO Dan GEO (Contoh: GPS)." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Satelit Navigasi Penentu Lokasi Global." },
  { "en": "Bagaimana GPS (Global Positioning System) Bekerja?", "id": "Menerima Sinyal Waktu Dari Minimal Empat Satelit." },
  { "en": "Apa Itu Transponder (Transponder) Satelit?", "id": "Menerima Sinyal (Uplink), Menguatkan, Mengirim (Downlink)." },
  { "en": "Apa Itu Uplink (Uplink) Satelit?", "id": "Sinyal Dikirim Dari Bumi Ke Satelit." },
  { "en": "Apa Itu Downlink (Downlink) Satelit?", "id": "Sinyal Dikirim Dari Satelit Kembali Ke Bumi." },
  { "en": "Apa Itu Stasiun Bumi (Earth Station)?", "id": "Antena (Parabola) Besar Di Bumi (Komunikasi Satelit)." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Direksional (Bentuk Piringan) Fokus Sinyal." },
  { "en": "Apa Itu LNB (Low-Noise Block) Parabola?", "id": "Menguatkan Sinyal Lemah, Menurunkan Frekuensi Satelit." },
  { "en": "Apa Itu Pita-C (C-Band) Satelit?", "id": "Frekuensi Satelit (4-8 GHz), Tahan Cuaca Hujan." },
  { "en": "Apa Itu Pita-Ku (Ku-Band) Satelit?", "id": "Frekuensi Satelit (12-18 GHz), Rentan Hujan (Rain Fade)." },
  { "en": "Apa Itu Pita-Ka (Ka-Band) Satelit?", "id": "Frekuensi Satelit (26-40 GHz), Kapasitas Sangat Besar." },
  { "en": "Apa Itu Rain Fade (Pudaran Hujan)?", "id": "Pelemahan Sinyal Satelit (Ku/Ka-Band) Akibat Hujan." },
  { "en": "Apa Itu Manufaktur Aditif (Additive Manufacturing)?", "id": "Proses Produksi Mencetak Lapis Demi Lapis (3D Print)." },
  { "en": "Apa Itu Pencetakan 3D (3D Printing)?", "id": "Nama Populer Untuk Manufaktur Aditif." },
  { "en": "Apa Itu FDM (Fused Deposition Modeling)?", "id": "Metode Cetak 3D Melelehkan Filamen Plastik (Umum)." },
  { "en": "Apa Itu Filamen (Filament) Cetak 3D?", "id": "Bahan Plastik (Gulungan Benang) Untuk Printer FDM." },
  { "en": "Apa Itu PLA (Polylactic Acid)?", "id": "Filamen Cetak 3D Paling Umum (Bahan Nabati)." },
  { "en": "Apa Itu ABS (Acrylonitrile Butadiene Styrene)?", "id": "Filamen Cetak 3D Kuat, Tahan Panas (Bau Menyengat)." },
  { "en": "Apa Itu SLA (Stereolithography) Cetak 3D?", "id": "Metode Cetak 3D Menggunakan Resin Cair Dan Laser UV." },
  { "en": "Apa Itu Resin (Resin) Cetak 3D?", "id": "Cairan Fotopolimer (Mengeras Jika Terkena Sinar UV)." },
  { "en": "Apa Keunggulan Cetak 3D SLA?", "id": "Hasil Cetak Sangat Halus Dan Detail (Presisi Tinggi)." },
  { "en": "Apa Itu SLS (Selective Laser Sintering)?", "id": "Metode Cetak 3D (Bubuk Material Dilelehkan Laser)." },
  { "en": "Apa Itu Manufaktur Subtraktif (Subtractive Manufacturing)?", "id": "Proses Produksi Membuang Material (Bor, Bubut)." },
  { "en": "Apa Itu Mesin CNC (Computer Numerical Control)?", "id": "Mesin Pabrik (Bubut/Milling) Dikontrol Komputer." },
  { "en": "Apa Itu Mesin Bubut (Lathe) CNC?", "id": "Mesin Membentuk Benda Silindris (Benda Kerja Berputar)." },
  { "en": "Apa Itu Mesin Milling (Milling Machine) CNC?", "id": "Mesin Membentuk Benda (Mata Bor Bergerak X-Y-Z)." },
  { "en": "Apa Itu G-Code (Kode G)?", "id": "Bahasa Pemrograman Standar Untuk Mengontrol Mesin CNC." },
  { "en": "Apa Itu Cetak Injeksi (Injection Molding)?", "id": "Proses Mencetak Plastik (Leleh Diinjeksi Ke Cetakan)." },
  { "en": "Apa Itu Cetakan (Mold/Die)?", "id": "Cetakan Logam (Baja) Untuk Cetak Injeksi." },
  { "en": "Apa Keunggulan Cetak Injeksi?", "id": "Produksi Massal Sangat Cepat Dan Murah (Per Unit)." },
  { "en": "Apa Itu Ekstrusi (Extrusion)?", "id": "Proses Mendorong Material Lewat Cetakan (Profil Panjang)." },
  { "en": "Apa Itu Pembentukan Termal (Thermoforming)?", "id": "Membentuk Lembaran Plastik Panas Di Atas Cetakan." },
  { "en": "Apa Itu Die Casting (Pengecoran Cetak Tekan)?", "id": "Proses Pengecoran Logam (Cair Ditekan Ke Cetakan)." },
  { "en": "Apa Itu Pengelasan (Welding)?", "id": "Proses Menyambung Logam (Melelehkan Bahan Induk)." },
  { "en": "Apa Itu Pematrian (Brazing)?", "id": "Menyambung Logam (Bahan Pengisi Meleleh, Induk Tidak)." },
  { "en": "Apa Itu Penyolderan (Soldering)?", "id": "Menyambung Logam (Titik Leleh Bahan Pengisi Rendah)." },
  { "en": "Apa Beda Welding Dan Soldering?", "id": "Welding (Induk Meleleh), Soldering (Pengisi Meleleh)." },
  { "en": "Apa Itu Las SMAW (Shielded Metal Arc Welding)?", "id": "Las Listrik Menggunakan Elektroda Terbungkus (Las Tongkat)." },
  { "en": "Apa Itu Las MIG (Metal Inert Gas)?", "id": "Las Menggunakan Kawat Gulungan Dan Gas Pelindung Inert." },
  { "en": "Apa Itu Las TIG (Tungsten Inert Gas)?", "id": "Las Menggunakan Elektroda Tungsten (Hasil Sangat Rapi)." },
  { "en": "Apa Itu Las Titik (Spot Welding)?", "id": "Las Menjepit Dua Lembar Logam (Arus Tinggi Sesaat)." },
  { "en": "Apa Itu Pengecoran (Casting)?", "id": "Menuang Logam Cair Ke Dalam Cetakan (Pasir/Logam)." },
  { "en": "Apa Itu Penempaan (Forging)?", "id": "Membentuk Logam Padat Dengan Pukulan (Tekanan) Keras." },
  { "en": "Apa Itu Pengerolan (Rolling) Logam?", "id": "Menipiskan Logam (Melewatkan Di Antara Dua Rol)." },
  { "en": "Apa Itu Perlakuan Panas (Heat Treatment) Logam?", "id": "Pemanasan/Pendinginan Terkontrol (Mengubah Sifat Logam)." },
  { "en": "Apa Itu Annealing (Anil)?", "id": "Perlakuan Panas (Melunakkan Logam, Menghilangkan Stres Internal)." },
  { "en": "Apa Itu Quenching (Pendinginan Cepat)?", "id": "Pendinginan Sangat Cepat (Mengeraskan Logam, Baja)." },
  { "en": "Apa Itu Tempering (Temper)?", "id": "Pemanasan Ulang (Mengurangi Kerapuhan Setelah Quenching)." },
  { "en": "Apa Itu Korosi (Corrosion)?", "id": "Kerusakan Logam Akibat Reaksi Kimia (Karat)." },
  { "en": "Apa Itu Karat (Rust)?", "id": "Korosi Oksidasi Pada Besi Dan Baja." },
  { "en": "Bagaimana Mencegah Korosi?", "id": "Pengecatan, Pelapisan (Coating), Proteksi Katodik." },
  { "en": "Apa Itu Pelapisan Galvanis (Galvanizing)?", "id": "Melapisi Besi/Baja Dengan Seng (Zinc)." },
  { "en": "Apa Itu Pelapisan Krom (Chrome Plating)?", "id": "Melapisi Logam Dengan Kromium (Tahan Karat, Mengkilap)." },
  { "en": "Apa Itu Anodisasi (Anodizing)?", "id": "Proses Oksidasi (Perlindungan Korosi) Untuk Aluminium." },
  { "en": "Apa Itu Proteksi Katodik (Cathodic Protection)?", "id": "Melindungi Logam (Pipa) Dengan Anoda Korban." },
  { "en": "Apa Itu Anoda Korban (Sacrificial Anode)?", "id": "Logam Lebih Reaktif (Seng) Dikorbankan (Karat)." },
  { "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam (Contoh: Baja, Kuningan, Perunggu)." },
  { "en": "Apa Itu Baja (Steel)?", "id": "Paduan Besi (Iron) Dan Karbon (Carbon)." },
  { "en": "Apa Itu Baja Tahan Karat (Stainless Steel)?", "id": "Baja Dengan Tambahan Kromium (Minimal 10.5 Persen)." },
  { "en": "Apa Itu Kuningan (Brass)?", "id": "Paduan Tembaga (Copper) Dan Seng (Zinc)." },
  { "en": "Apa Itu Perunggu (Bronze)?", "id": "Paduan Tembaga (Copper) Dan Timah (Tin)." },
  { "en": "Apa Itu Aluminium (Aluminum)?", "id": "Logam Ringan, Tahan Karat (Konduktor Baik)." },
  { "en": "Apa Itu Tembaga (Copper)?", "id": "Logam Konduktor Listrik Sangat Baik (Kabel)." },
  { "en": "Apa Itu Perak (Silver)?", "id": "Logam Konduktor Listrik Terbaik (Mahal)." },
  { "en": "Apa Itu Emas (Gold)?", "id": "Logam Mulia, Tahan Karat (Kontak Elektronik)." },
  { "en": "Apa Itu Titanium (Titanium)?", "id": "Logam Sangat Kuat, Ringan, Tahan Karat (Implan)." },
  { "en": "Apa Itu Tungsten (Tungsten/Wolfram)?", "id": "Logam Titik Leleh Sangat Tinggi (Filamen Bohlam)." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Material Rantai Molekul Panjang (Plastik, Karet)." },
  { "en": "Apa Itu Termoplastik (Thermoplastic)?", "id": "Plastik Meleleh Saat Dipanaskan (Bisa Didaur Ulang)." },
  { "en": "Apa Itu Termoset (Thermoset)?", "id": "Plastik Mengeras Permanen Saat Dipanaskan (Tidak Meleleh)." },
  { "en": "Apa Contoh Termoplastik?", "id": "PET (Botol Minum), PVC (Pipa), Polikarbonat." },
  { "en": "Apa Contoh Termoset?", "id": "Epoksi (Resin PCB), Bakelit (Pegangan Panci)." },
  { "en": "Apa Itu Elastomer (Elastomer)?", "id": "Polimer Elastis (Contoh: Karet Alam, Silikon)." },
  { "en": "Apa Itu Komposit (Composite)?", "id": "Gabungan Dua Material (Matriks Dan Penguat)." },
  { "en": "Apa Contoh Komposit?", "id": "Serat Karbon (Carbon Fiber), Serat Kaca (Fiberglass)." },
  { "en": "Apa Itu Keramik (Ceramics)?", "id": "Material Non-Logam (Padat, Keras, Rapuh, Tahan Panas)." },
  { "en": "Apa Contoh Keramik Teknik?", "id": "Alumina (Isolator), Silikon Nitrida (Bearing)." },
  { "en": "Apa Kepanjangan SEPIC (Single-Ended Primary-Inductor Converter)?", "id": "Konverter Induktor Primer Ujung Tunggal." },
  { "en": "Apa Itu Konverter SEPIC (SEPIC Converter)?", "id": "Konverter DC-DC (Bisa Naik/Turun, Output Positif)." },
  { "en": "Apa Itu Konverter Cuk (Cuk Converter)?", "id": "Konverter DC-DC (Naik/Turun, Output Polaritas Terbalik)." },
  { "en": "Apa Itu Kontrol Mode Histeresis (Hysteretic)?", "id": "Kontrol SMPS (Respon Sangat Cepat, Frekuensi Bervariasi)." },
  { "en": "Apa Itu Rangkaian Snubber (Snubber Circuit)?", "id": "Rangkaian RC (Melindungi Saklar Dari Lonjakan Induktif)." },
  { "en": "Apa Itu Suhu Sambungan (Junction Temperature/Tj)?", "id": "Suhu Internal (Inti) Semikonduktor (Chip)." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance/Rth)?", "id": "Ukuran Hambatan Aliran Panas (Chip Ke Pendingin)." },
  { "en": "Apa Kepanjangan SOA (Safe Operating Area)?", "id": "Area Operasi Aman." },
  { "en": "Apa Itu SOA (Safe Operating Area)?", "id": "Grafik Batas Aman (Arus, Tegangan) Operasi Transistor." },
  { "en": "Apa Itu Kegagalan Longsor (Avalanche Breakdown)?", "id": "Kerusakan Dioda/Transistor Akibat Energi Mundur Berlebih." },
  { "en": "Apa Kepanjangan HAWT (Horizontal-Axis Wind Turbine)?", "id": "Turbin Angin Sumbu Horizontal (Baling-Baling Umum)." },
  { "en": "Apa Kepanjangan VAWT (Vertical-Axis Wind Turbine)?", "id": "Turbin Angin Sumbu Vertikal (Tegak Lurus)." },
  { "en": "Apa Itu Kontrol Pitch (Pitch Control) Turbin?", "id": "Mengatur Sudut Bilah (Blade) Mengontrol Kecepatan." },
  { "en": "Apa Itu Kontrol Stall (Stall Control) Turbin?", "id": "Desain Bilah (Blade) Otomatis Melambat Di Angin Kencang." },
  { "en": "Apa Kepanjangan BMS (Battery Management System)?", "id": "Sistem Manajemen Baterai." },
  { "en": "Apa Fungsi Utama BMS (Battery Management System)?", "id": "Melindungi Sel Baterai (Overcharge, Overdischarge)." },
  { "en": "Apa Itu Penyeimbangan Sel (Cell Balancing) Baterai?", "id": "Proses Menyamakan Tegangan Sel Dalam Satu Paket." },
  { "en": "Apa Itu Penyeimbangan Pasif (Passive Balancing)?", "id": "Membuang Energi Sel Penuh (Menjadi Panas Resistor)." },
  { "en": "Apa Itu Penyeimbangan Aktif (Active Balancing)?", "id": "Memindahkan Energi (Sel Penuh Ke Sel Rendah)." },
  { "en": "Apa Kepanjangan SOC (State of Charge) Baterai?", "id": "Status Keterisian Daya (Persentase Sisa Baterai)." },
  { "en": "Apa Kepanjangan SOH (State of Health) Baterai?", "id": "Status Kesehatan (Ukuran Degradasi) Baterai." },
  { "en": "Apa Itu Inverter Rangkai (String Inverter) Surya?", "id": "Satu Inverter Besar Untuk Satu Rangkaian Panel." },
  { "en": "Apa Itu Inverter Mikro (Microinverter) Surya?", "id": "Satu Inverter Kecil Terpasang Di Setiap Panel Surya." },
  { "en": "Apa Keuntungan Inverter Mikro (Microinverter)?", "id": "Optimal Per Panel, Tahan Terhadap Bayangan (Shade)." },
  { "en": "Apa Itu Proteksi Anti-Pulau (Anti-Islanding)?", "id": "Inverter Surya Mati Otomatis Saat Listrik PLN Padam." },
  { "en": "Mengapa Anti-Pulau (Anti-Islanding) Penting?", "id": "Keselamatan Pekerja (Mencegah Setrum Balik Ke Jaringan)." },
  { "en": "Apa Itu Kurva I-V (I-V Curve) Panel Surya?", "id": "Grafik Hubungan Arus (I) Dan Tegangan (V)." },
  { "en": "Apa Itu Kurva P-V (P-V Curve) Panel Surya?", "id": "Grafik Hubungan Daya (P) Dan Tegangan (V)." },
  { "en": "Apa Itu Titik Daya Maksimum (MPP)?", "id": "Titik Puncak (Daya Output Tertinggi) Di Kurva P-V." },
  { "en": "Apa Itu VMP (Voltage At Maximum Power)?", "id": "Tegangan Panel Surya Saat Di Titik MPP." },
  { "en": "Apa Itu IMP (Current At Maximum Power)?", "id": "Arus Panel Surya Saat Di Titik MPP." },
  { "en": "Apa Itu VOC (Open Circuit Voltage)?", "id": "Tegangan Maksimum Panel Surya (Saat Tanpa Beban)." },
  { "en": "Apa Itu ISC (Short Circuit Current)?", "id": "Arus Maksimum Panel Surya (Saat Hubung Singkat)." },
  { "en": "Apa Itu Koefisien Suhu (Temperature Coefficient) Panel Surya?", "id": "Ukuran Penurunan Kinerja Panel Akibat Panas." },
  { "en": "Apa Itu Dioda Bypass (Bypass Diode) Panel Surya?", "id": "Dioda Paralel (Melindungi Sel Jika Terkena Bayangan)." },
  { "en": "Apa Itu Dioda Blokir (Blocking Diode) Surya?", "id": "Dioda Seri (Mencegah Arus Balik Ke Panel Malam Hari)." },
  { "en": "Apa Kepanjangan EVSE (Electric Vehicle Supply Equipment)?", "id": "Peralatan Pasokan Kendaraan Listrik (Stasiun Pengisian)." },
  { "en": "Apa Itu Pengisian AC Level 1 (Level 1)?", "id": "Pengisian EV (Sangat Lambat) Dari Stop Kontak Biasa." },
  { "en": "Apa Itu Pengisian AC Level 2 (Level 2)?", "id": "Pengisian EV (Sedang) Dari Wallbox Khusus (240V)." },
  { "en": "Apa Itu Pengisian DC Level 3 (Level 3)?", "id": "Pengisian EV Sangat Cepat (Fast Charging DC)." },
  { "en": "Apa Kepanjangan OBC (On-Board Charger)?", "id": "Pengisi Daya Di Dalam Mobil (Konverter AC Ke DC)." },
  { "en": "Apa Itu Konektor Tipe 1 (Type 1/J1772)?", "id": "Konektor Pengisian AC (Standar Amerika/Jepang)." },
  { "en": "Apa Itu Konektor Tipe 2 (Type 2/Mennekes)?", "id": "Konektor Pengisian AC (Standar Eropa/Indonesia)." },
  { "en": "Apa Kepanjangan CCS (Combined Charging System)?", "id": "Sistem Pengisian Gabungan (Konektor DC Cepat Eropa/Amerika)." },
  { "en": "Apa Itu Konektor CHAdeMO?", "id": "Konektor Pengisian DC Cepat (Standar Jepang)." },
  { "en": "Apa Itu Konektor GBT (GB/T)?", "id": "Konektor Pengisian EV (Standar Tiongkok/China)." },
  { "en": "Apa Kepanjangan V2G (Vehicle-to-Grid)?", "id": "Kendaraan Ke Jaringan (Mobil Menyuplai Listrik Ke Grid)." },
  { "en": "Apa Kepanjangan V2L (Vehicle-to-Load)?", "id": "Kendaraan Ke Beban (Mobil Menjadi Stop Kontak Portabel)." },
  { "en": "Apa Kepanjangan V2H (Vehicle-to-Home)?", "id": "Kendaraan Ke Rumah (Mobil Menjadi Catu Daya Rumah)." },
  { "en": "Apa Itu Fluks Cahaya (Luminous Flux)?", "id": "Ukuran Total Kecerahan Cahaya (Satuan: Lumen)." },
  { "en": "Apa Itu Iluminansi (Illuminance)?", "id": "Ukuran Kecerahan Cahaya Jatuh Di Permukaan (Satuan: Lux)." },
  { "en": "Apa Itu Lux (Lux)?", "id": "Satu (1) Lumen Per Meter Persegi (1 Lm/M²)." },
  { "en": "Apa Itu Intensitas Cahaya (Luminous Intensity)?", "id": "Kecerahan Cahaya Ke Arah Tertentu (Satuan: Candela)." },
  { "en": "Apa Itu Candela (Candela/Cd)?", "id": "Satuan Dasar SI Untuk Intensitas Cahaya." },
  { "en": "Apa Itu Luminansi (Luminance)?", "id": "Kecerahan Cahaya Yang Dipantulkan/Dipancarkan Permukaan (Silau)." },
  { "en": "Apa Kepanjangan CRI (Color Rendering Index)?", "id": "Indeks Renderasi Warna (Kemiripan Warna Asli)." },
  { "en": "Berapa Nilai CRI (Color Rendering Index) Ideal?", "id": "Di Atas Sembilan Puluh (90), Sangat Baik." },
  { "en": "Apa Kepanjangan CCT (Correlated Color Temperature)?", "id": "Suhu Warna Terkorelasi (Nuansa Putih)." },
  { "en": "Apa Satuan CCT (Correlated Color Temperature)?", "id": "Kelvin (K)." },
  { "en": "Apa CCT Untuk Warm White (Putih Hangat)?", "id": "Sekitar 2700 Kelvin Hingga 3000 Kelvin (Kuning)." },
  { "en": "Apa CCT Untuk Cool White (Putih Dingin)?", "id": "Di Atas 5000 Kelvin Hingga 6500 Kelvin (Kebiruan)." },
  { "en": "Apa CCT Untuk Natural White (Putih Netral)?", "id": "Sekitar 4000 Kelvin (Mirip Cahaya Siang)." },
  { "en": "Apa Itu Efikasi Luminus (Luminous Efficacy)?", "id": "Efisiensi Lampu (Seberapa Terang Per Watt)." },
  { "en": "Apa Satuan Efikasi Luminus?", "id": "Lumen Per Watt (Lm/W)." },
  { "en": "Apa Itu Peredupan (Dimming) PWM LED?", "id": "Meredupkan LED (Menyalakan-Matikan Sangat Cepat)." },
  { "en": "Apa Itu Peredupan (Dimming) CCR LED?", "id": "Meredupkan LED (Mengurangi Arus DC Konstan)." },
  { "en": "Apa Kepanjangan CCR (Constant Current Reduction)?", "id": "Pengurangan Arus Konstan." },
  { "en": "Apa Kepanjangan DALI (Digital Addressable Lighting Interface)?", "id": "Antarmuka Pencahayaan Digital Beralamat." },
  { "en": "Apa Itu Protokol DALI?", "id": "Protokol Standar (Dua Arah) Untuk Kontrol Pencahayaan Cerdas." },
  { "en": "Apa Itu Arbitrase (Arbitration) CAN Bus?", "id": "Proses Menentukan Prioritas Pesan (ID Terendah Menang)." },
  { "en": "Apa Itu Frame Kesalahan (Error Frame) CAN Bus?", "id": "Sinyal Khusus (Mengindikasikan Kesalahan) Di Jaringan CAN." },
  { "en": "Apa Itu Master (Master) LIN Bus?", "id": "Node Utama (Mengatur Jadwal Komunikasi) Di Jaringan LIN." },
  { "en": "Apa Itu Slave (Slave) LIN Bus?", "id": "Node (Merespon Permintaan/Request) Dari Master LIN." },
  { "en": "Apa Itu Mode Tidur (Sleep Mode) LIN Bus?", "id": "Mode Hemat Daya (Bus Tidak Aktif) Di Jaringan LIN." },
  { "en": "Apa Itu Boot Aman (Secure Boot)?", "id": "Proses Verifikasi Tanda Tangan Digital Firmware (Anti-Malware)." },
  { "en": "Apa Itu ARM TrustZone (ARM TrustZone)?", "id": "Teknologi Keamanan (Memisahkan Eksekusi Aman Dan Normal)." },
  { "en": "Apa Itu Kotak Speaker Tertutup (Sealed Enclosure)?", "id": "Kotak Speaker Kedap Udara (Bass Akurat, Kurang Keras)." },
  { "en": "Apa Itu Kotak Speaker Berport (Ported/Bass Reflex)?", "id": "Kotak Speaker (Lubang/Port) Resonansi (Bass Keras)." },
  { "en": "Apa Itu Speaker Elektrostatik (Electrostatic Speaker)?", "id": "Speaker (Diafragma Tipis) Bergerak Akibat Medan Statis." },
  { "en": "Apa Itu Speaker Dinamis (Dynamic Speaker)?", "id": "Speaker Umum (Menggunakan Kumparan Bergerak Dan Magnet)." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect)?", "id": "Perubahan Frekuensi Akibat Gerakan Sumber (Ambulans)." },
  { "en": "Apa Itu Getaran (Vibration)?", "id": "Gerakan Mekanis Bolak-Balik (Osilasi) Di Sekitar Titik." },
  { "en": "Apa Itu Analisis Modal (Modal Analysis)?", "id": "Studi (Analisis) Frekuensi Resonansi Alami Struktur." },
  { "en": "Apa Itu Frekuensi Alami (Natural Frequency)?", "id": "Frekuensi Getaran Alami Benda (Jika Diganggu)." },
  { "en": "Apa Itu Resonansi (Resonance) Mekanis?", "id": "Getaran Amplitudo Besar (Saat Frekuensi Paksa = Frekuensi Alami)." },
  { "en": "Apa Itu Peredam (Damping) Getaran?", "id": "Proses Mengurangi (Disipasi) Energi Getaran (Menjadi Panas)." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor Mengukur Percepatan (Untuk Getaran, Orientasi)." },
  { "en": "Apa Itu Alat Getar (Shaker) Elektrodinamik?", "id": "Alat Uji (Mirip Speaker) Menghasilkan Getaran Terkontrol." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Perangkat Mengubah Satu Bentuk Energi Ke Bentuk Lain." },
  { "en": "Apa Kepanjangan ANC (Active Noise Cancellation)?", "id": "Pembatalan Derau (Kebisingan) Aktif." },
  { "en": "Bagaimana ANC (Active Noise Cancellation) Bekerja?", "id": "Mendeteksi Noise, Membuat Gelombang Suara Anti-Fasa." },
  { "en": "Apa Itu Ultrasonik (Ultrasonic)?", "id": "Gelombang Suara (Frekuensi Di Atas Pendengaran Manusia)." },
  { "en": "Apa Aplikasi Ultrasonik (Ultrasonic)?", "id": "Sensor Jarak, Pencitraan Medis (USG), Pembersihan." },
  { "en": "Apa Itu Infrasonik (Infrasonic)?", "id": "Gelombang Suara (Frekuensi Di Bawah Pendengaran Manusia)." },
  { "en": "Apa Itu Akustik (Acoustics)?", "id": "Ilmu Fisika Yang Mempelajari Tentang Suara." },
  { "en": "Apa Itu Gema (Reverberation/Reverb)?", "id": "Kumpulan Pantulan Suara (Gaung) Di Dalam Ruangan." },
  { "en": "Apa Itu Penyerapan (Absorption) Suara?", "id": "Material Lembut (Busa) Mengurangi Energi Pantulan Suara." },
  { "en": "Apa Itu Difusi (Diffusion) Suara?", "id": "Material (Permukaan Tidak Rata) Menyebarkan Pantulan Suara." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
