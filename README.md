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


  { "en": "Apa Itu Sirkuit Terbuka (Open Circuit)?", "id": "Jalur Arus Yang Tidak Lengkap." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Jalur Arus Resistansi Sangat Rendah." },
  { "en": "Apa Itu Arus Konvensional?", "id": "Aliran Muatan Positif Hipotetis." },
  { "en": "Apa Itu Aliran Elektron?", "id": "Aliran Sebenarnya Partikel Elektron Negatif." },
  { "en": "Apa Satuan Konduktansi?", "id": "Siemens (S)." },
  { "en": "Apa Itu Konduktansi?", "id": "Kemudahan Arus Listrik Mengalir." },
  { "en": "Apa Hubungan Resistansi Dan Konduktansi?", "id": "Konduktansi Adalah Kebalikan Dari Resistansi." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Yang Nilainya Berubah Dengan Suhu." },
  { "en": "Apa Itu Varistor?", "id": "Resistor Yang Nilainya Berubah Dengan Tegangan." },
  { "en": "Apa Fungsi Utama Varistor?", "id": "Melindungi Rangkaian Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Fotokonduktor?", "id": "Resistor Yang Nilainya Berubah Dengan Cahaya." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Nama Lain Untuk Fotokonduktor." },
  { "en": "Apa Itu Superposisi?", "id": "Metode Analisis Sirkuit Linear." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Sirkuit Menjadi Sumber Tegangan Seri." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Sirkuit Menjadi Sumber Arus Paralel." },
  { "en": "Apa Hubungan Ekuivalen Thevenin Dan Norton?", "id": "Dapat Dikonversi Satu Sama Lain." },
  { "en": "Apa Itu Transfer Daya Maksimum?", "id": "Terjadi Saat Impedansi Beban Sesuai Sumber." },
  { "en": "Apa Itu Respon Transien?", "id": "Respon Sementara Sirkuit Terhadap Perubahan." },
  { "en": "Apa Itu Respon Keadaan Tunak?", "id": "Perilaku Sirkuit Setelah Waktu Lama." },
  { "en": "Apa Itu Konstanta Waktu Rangkaian RC?", "id": "Tau Sama Dengan R Dikalikan C." },
  { "en": "Apa Itu Konstanta Waktu Rangkaian RL?", "id": "Tau Sama Dengan L Dibagi R." },
  { "en": "Apa Itu Nilai Rata-rata (Average)?", "id": "Level DC Dari Suatu Bentuk Gelombang." },
  { "en": "Apa Nilai Rata-rata Gelombang Sinus Penuh?", "id": "Nol." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Suatu Bentuk Gelombang AC." },
  { "en": "Apa Hubungan Nilai RMS Dan Puncak Sinus?", "id": "Nilai RMS Adalah Puncak Dibagi Akar Dua." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor)?", "id": "Rasio Nilai RMS Terhadap Nilai Rata-rata." },
  { "en": "Apa Itu Faktor Puncak (Crest Factor)?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Apa Itu Frekuensi Sudut (Angular Frequency)?", "id": "Frekuensi Dalam Satuan Radian Per Detik." },
  { "en": "Apa Itu Siklus (Cycle)?", "id": "Satu Perulangan Penuh Bentuk Gelombang Periodik." },
  { "en": "Apa Itu Periode (Period)?", "id": "Waktu Yang Dibutuhkan Untuk Satu Siklus." },
  { "en": "Apa Itu Sistem Listrik Satu Fasa?", "id": "Sistem AC Dengan Dua Kawat." },
  { "en": "Apa Itu Sistem Listrik Tiga Fasa?", "id": "Sistem AC Dengan Tiga Atau Empat Kawat." },
  { "en": "Apa Keuntungan Sistem Tiga Fasa?", "id": "Daya Lebih Konstan Dan Efisien." },
  { "en": "Apa Itu Hubungan Bintang (Wye)?", "id": "Konfigurasi Tiga Fasa Dengan Titik Netral." },
  { "en": "Apa Itu Hubungan Delta (Segitiga)?", "id": "Konfigurasi Tiga Fasa Tanpa Titik Netral." },
  { "en": "Apa Itu Tegangan Jalur (Line Voltage)?", "id": "Tegangan Antara Dua Jalur Fasa." },
  { "en": "Apa Itu Tegangan Fasa (Phase Voltage)?", "id": "Tegangan Antara Jalur Fasa Dan Netral." },
  { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Tegangan Fasa Mencapai Puncaknya." },
  { "en": "Apa Itu Metode Dua Wattmeter?", "id": "Mengukur Daya Tiga Fasa Dengan Dua Wattmeter." },
  { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni Tanpa Ketidakmurnian." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Dengan Penambahan Dopant." },
  { "en": "Apa Itu Doping?", "id": "Proses Menambahkan Ketidakmurnian Ke Semikonduktor." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Semikonduktor Dengan Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu Pembawa Mayoritas?", "id": "Pembawa Muatan Paling Banyak Dalam Semikonduktor." },
  { "en": "Apa Itu Pembawa Minoritas?", "id": "Pembawa Muatan Paling Sedikit Dalam Semikonduktor." },
  { "en": "Apa Itu Simpangan P-N (P-N Junction)?", "id": "Batas Antara Material Tipe-P Dan Tipe-N." },
  { "en": "Apa Itu Daerah Deplesi?", "id": "Daerah Sekitar Simpangan Tanpa Pembawa Muatan." },
  { "en": "Apa Fungsi Dioda Zener?", "id": "Menstabilkan Tegangan Pada Level Tertentu." },
  { "en": "Apa Itu Efek Longsoran (Avalanche)?", "id": "Proses Multiplikasi Pembawa Muatan Cepat." },
  { "en": "Apa Itu Transistor Bipolar (BJT)?", "id": "Transistor Yang Dikendalikan Oleh Arus." },
  { "en": "Apa Saja Terminal BJT (Bipolar Junction Transistor)?", "id": "Emitor, Basis, Dan Kolektor." },
  { "en": "Apa Itu Transistor Efek Medan (FET)?", "id": "Transistor Yang Dikendalikan Oleh Tegangan." },
  { "en": "Apa Saja Terminal JFET (Junction Field-Effect Transistor)?", "id": "Source, Gate, Dan Drain." },
  { "en": "Apa Itu Garis Beban DC?", "id": "Garis Merepresentasikan Rangkaian Eksternal Transistor." },
  { "en": "Apa Itu Titik Operasi (Q-Point)?", "id": "Titik Bias DC Transistor Tanpa Sinyal." },
  { "en": "Apa Itu Penguat Common-Emitter?", "id": "Konfigurasi Penguat BJT Paling Umum." },
  { "en": "Apa Karakteristik Penguat Common-Emitter?", "id": "Penguatan Tegangan Dan Arus Tinggi." },
  { "en": "Apa Itu Pengikut Emitor (Common-Collector)?", "id": "Penguat BJT Dengan Penguatan Tegangan Hampir Satu." },
  { "en": "Apa Fungsi Pengikut Emitor?", "id": "Sebagai Penyangga Impedansi (Impedance Buffer)." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Penguatan Sangat Tinggi." },
  { "en": "Apa Sifat Op-Amp (Operational Amplifier) Ideal?", "id": "Penguatan Tak Terhingga, Impedansi Input Tak Terhingga." },
  { "en": "Apa Itu Ground Virtual?", "id": "Titik Tegangan Nol Tanpa Terhubung Langsung." },
  { "en": "Apa Itu Penguat Pembalik (Inverting Amplifier)?", "id": "Output Berbeda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat Tak-Pembalik (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Penjumlah (Summing Amplifier)?", "id": "Menjumlahkan Beberapa Sinyal Input." },
  { "en": "Apa Itu Integrator?", "id": "Menghasilkan Output Integral Dari Input." },
  { "en": "Apa Itu Diferensiator?", "id": "Menghasilkan Output Turunan Dari Input." },
  { "en": "Apa Itu Gerbang Logika AND?", "id": "Output Tinggi Jika Semua Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika OR?", "id": "Output Tinggi Jika Salah Satu Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika NOT (Inverter)?", "id": "Membalikkan Level Logika Input." },
  { "en": "Apa Itu Gerbang Logika NAND?", "id": "Kebalikan Dari Gerbang Logika AND." },
  { "en": "Apa Itu Gerbang Logika NOR?", "id": "Kebalikan Dari Gerbang Logika OR." },
  { "en": "Apa Itu Gerbang Logika XOR?", "id": "Output Tinggi Jika Input Berbeda." },
  { "en": "Apa Itu Gerbang Logika XNOR?", "id": "Output Tinggi Jika Input Sama." },
  { "en": "Apa Itu Aljabar Boolean?", "id": "Sistem Matematika Untuk Logika Digital." },
  { "en": "Apa Itu Hukum De Morgan?", "id": "Aturan Untuk Menyederhanakan Ekspresi Logika." },
  { "en": "Apa Itu Peta Karnaugh (K-Map)?", "id": "Metode Grafis Penyederhanaan Logika." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Dasar Sirkuit Sekuensial." },
  { "en": "Apa Itu Latch?", "id": "Jenis Flip-Flop Transparan Level." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Menampilkan Bentuk Gelombang." },
  { "en": "Apa Itu Multimeter?", "id": "Mengukur Tegangan, Arus, Dan Resistansi." },
  { "en": "Apa Itu Generator Fungsi?", "id": "Menghasilkan Berbagai Bentuk Gelombang Uji." },
  { "en": "Apa Itu Catu Daya (Power Supply)?", "id": "Menyediakan Tegangan DC Untuk Sirkuit." },
  { "en": "Apa Itu Galvanometer?", "id": "Alat Ukur Arus Listrik Sangat Kecil." },
  { "en": "Apa Itu Ammeter?", "id": "Alat Untuk Mengukur Arus Listrik." },
  { "en": "Apa Itu Voltmeter?", "id": "Alat Untuk Mengukur Tegangan Listrik." },
  { "en": "Apa Itu Ohmmeter?", "id": "Alat Untuk Mengukur Hambatan Listrik." },
  { "en": "Apa Itu Wattmeter?", "id": "Alat Untuk Mengukur Daya Listrik." },
  { "en": "Apa Itu Transformator Instrumen?", "id": "Trafo Untuk Pengukuran Arus, Tegangan Tinggi." },
  { "en": "Apa Itu Transformator Arus (CT)?", "id": "Menurunkan Arus Untuk Pengukuran Aman." },
  { "en": "Apa Itu Transformator Potensial (PT)?", "id": "Menurunkan Tegangan Untuk Pengukuran Aman." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Perbedaan Suhu Menghasilkan Tegangan." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Berdasarkan Efek Seebeck." },
  { "en": "Apa Itu Efek Piezoelektrik?", "id": "Tekanan Mekanis Menghasilkan Tegangan." },
  { "en": "Apa Itu Efek Hall?", "id": "Medan Magnet Menghasilkan Tegangan Melintang." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Mendeteksi Keberadaan Medan Magnet." },
  { "en": "Apa Itu Kapasitansi?", "id": "Kemampuan Menyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktansi?", "id": "Kemampuan Menentang Perubahan Arus." },
  { "en": "Apa Itu Fluks Magnetik?", "id": "Ukuran Medan Magnet Melalui Suatu Luas." },
  { "en": "Apa Satuan Fluks Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Kerapatan Fluks Magnetik?", "id": "Kekuatan Medan Magnet Per Satuan Luas." },
  { "en": "Apa Satuan Kerapatan Fluks Magnetik?", "id": "Tesla (T)." },
  { "en": "Apa Itu Model Saluran Transmisi Terdistribusi?", "id": "Model Memperlakukan R, L, C, G Terdistribusi." },
  { "en": "Apa Itu Persamaan Telegrafer?", "id": "Persamaan Diferensial Mendeskripsikan Saluran Transmisi." },
  { "en": "Apa Itu Kecepatan Propagasi?", "id": "Kecepatan Perambatan Gelombang Sepanjang Saluran." },
  { "en": "Apa Itu Saluran Transmisi Tanpa Distorsi?", "id": "Saluran Di Mana Bentuk Sinyal Tidak Berubah." },
  { "en": "Apa Syarat Saluran Tanpa Distorsi?", "id": "Rasio R/L Sama Dengan Rasio G/C." },
  { "en": "Apa Itu Lattice Diagram (Bounce Diagram)?", "id": "Diagram Menganalisis Pantulan Gelombang." },
  { "en": "Apa Itu Sirkuit Penyesuai Impedansi?", "id": "Rangkaian Untuk Mencapai Transfer Daya Maksimal." },
  { "en": "Apa Itu Penyesuai Stub Tunggal?", "id": "Menggunakan Satu Potongan Saluran Untuk Penyesuaian." },
  { "en": "Apa Itu Penyesuai Stub Ganda?", "id": "Menggunakan Dua Stub Pada Posisi Tetap." },
  { "en": "Apa Itu Penyesuai Transformator Seperempat Gelombang?", "id": "Menggunakan Saluran Panjang λ/4." },
  { "en": "Apa Itu Kawat Lecher?", "id": "Saluran Dua Kawat Paralel Untuk Mengukur Panjang Gelombang." },
  { "en": "Apa Itu Microstrip?", "id": "Saluran Transmisi Planar Dengan Substrat Dielektrik." },
  { "en": "Apa Itu Stripline?", "id": "Saluran Transmisi Diapit Dua Bidang Tanah." },
  { "en": "Apa Itu Coplanar Waveguide (CPW)?", "id": "Saluran Transmisi Konduktor Pusat, Tanah Samping." },
  { "en": "Apa Itu Konstanta Dielektrik Efektif?", "id": "Konstanta Dielektrik Dilihat Gelombang Di Microstrip." },
  { "en": "Apa Itu Penguat Daya Kelas A?", "id": "Transistor Selalu ON, Efisiensi Rendah." },
  { "en": "Apa Itu Penguat Daya Kelas B?", "id": "Setiap Transistor ON Setengah Siklus." },
  { "en": "Apa Itu Distorsi Crossover?", "id": "Distorsi Pada Amplifier Kelas B." },
  { "en": "Apa Itu Penguat Daya Kelas AB?", "id": "Kompromi Antara Kelas A Dan Kelas B." },
  { "en": "Apa Itu Penguat Daya Kelas C?", "id": "Transistor ON Kurang Dari Setengah Siklus." },
  { "en": "Di Mana Penguat Kelas C Digunakan?", "id": "Aplikasi Frekuensi Radio (RF) Termodulasi." },
  { "en": "Apa Itu Penguat Daya Kelas D?", "id": "Amplifier Switching Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu Sinyal Modulasi Lebar Pulsa (PWM)?", "id": "Sinyal Digunakan Dalam Amplifier Kelas D." },
  { "en": "Apa Itu Tegangan Saturasi Kolektor-Emitor (VCE(sat))?", "id": "Tegangan Pada Transistor Saat Jenuh." },
  { "en": "Apa Itu Garis Beban AC?", "id": "Garis Beban Untuk Analisis Sinyal AC." },
  { "en": "Apa Itu Ayunan Sinyal Maksimum?", "id": "Amplitudo Sinyal Maksimum Tanpa Terpotong." },
  { "en": "Apa Itu Pemasangan Pasangan Komplementer?", "id": "Menggunakan Transistor NPN Dan PNP Bersama." },
  { "en": "Apa Itu Efek Termal Pada Transistor?", "id": "Kinerja Transistor Berubah Dengan Suhu." },
  { "en": "Apa Itu Pelarian Termal (Thermal Runaway)?", "id": "Peningkatan Suhu Menyebabkan Peningkatan Arus." },
  { "en": "Bagaimana Mencegah Pelarian Termal?", "id": "Menggunakan Resistor Emitor Atau Kompensasi Suhu." },
  { "en": "Apa Itu Model Transistor Hibrida-Pi?", "id": "Model Sinyal Kecil Transistor Bipolar." },
  { "en": "Apa Itu Kapasitansi Difusi?", "id": "Kapasitansi Terkait Injeksi Pembawa Minoritas." },
  { "en": "Apa Itu Kapasitansi Transisi?", "id": "Kapasitansi Daerah Deplesi Simpangan P-N." },
  { "en": "Apa Itu Frekuensi Transisi (fT)?", "id": "Frekuensi Penguatan Arus Menjadi Satu." },
  { "en": "Apa Itu Produk Gain-Bandwidth?", "id": "Ukuran Kinerja Frekuensi Tinggi Transistor." },
  { "en": "Apa Itu Efek Miller?", "id": "Peningkatan Kapasitansi Input Akibat Penguatan." },
  { "en": "Apa Itu Konfigurasi Cascode?", "id": "Mengurangi Efek Miller, Meningkatkan Bandwidth." },
  { "en": "Apa Itu Pasangan Darlington?", "id": "Konfigurasi Dua Transistor Penguatan Arus Tinggi." },
  { "en": "Apa Itu Penguat Tuned?", "id": "Penguat Dirancang Untuk Pita Frekuensi Sempit." },
  { "en": "Apa Itu Netralisasi?", "id": "Membatalkan Umpan Balik Internal Mencegah Osilasi." },
  { "en": "Apa Itu Osilasi Parasitik?", "id": "Osilasi Tak Diinginkan Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Teori Stabilitas Nyquist?", "id": "Metode Grafis Menentukan Stabilitas Sistem." },
  { "en": "Apa Itu Plot Polar?", "id": "Plot Besaran Vs Fasa Respon Frekuensi." },
  { "en": "Apa Itu Umpan Balik Positif?", "id": "Umpan Balik Yang Dapat Menyebabkan Osilasi." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Umpan Balik Yang Menstabilkan Penguat." },
  { "en": "Apa Efek Umpan Balik Negatif Pada Impedansi?", "id": "Dapat Menaikkan Atau Menurunkan Impedansi." },
  { "en": "Apa Itu Desensitivitas Penguatan?", "id": "Penguatan Loop Tertutup Kurang Sensitif." },
  { "en": "Apa Itu Perluasan Bandwidth?", "id": "Umpan Balik Negatif Dapat Memperluas Bandwidth." },
  { "en": "Apa Itu Konfigurasi Umpan Balik Seri-Shunt?", "id": "Penguat Tegangan (Voltage Amplifier)." },
  { "en": "Apa Itu Konfigurasi Umpan Balik Shunt-Seri?", "id": "Penguat Arus (Current Amplifier)." },
  { "en": "Apa Itu Konfigurasi Umpan Balik Seri-Seri?", "id": "Penguat Transkonduktansi." },
  { "en": "Apa Itu Konfigurasi Umpan Balik Shunt-Shunt?", "id": "Penguat Transresistansi." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu CMRR (Common-Mode Rejection Ratio)?", "id": "Rasio Penolakan Mode Umum." },
  { "en": "Apa Itu Pasangan Ekor Panjang?", "id": "Nama Lain Penguat Diferensial Bipolar." },
  { "en": "Apa Itu Beban Aktif?", "id": "Menggunakan Sumber Arus Sebagai Beban." },
  { "en": "Apa Itu Tahap Keluaran (Output Stage)?", "id": "Tahap Akhir Penguat Memberikan Daya." },
  { "en": "Apa Itu Batas Arus (Current Limiting)?", "id": "Fitur Proteksi Pembatas Arus Output." },
  { "en": "Apa Itu Model Keteraturan (Orderliness Model)?", "id": "Model Keandalan Memperhitungkan Urutan Kegagalan." },
  { "en": "Apa Itu Diagram Blok Keandalan (RBD)?", "id": "Representasi Grafis Keandalan Sistem." },
  { "en": "Apa Itu Analisis Pohon Kegagalan (FTA)?", "id": "Analisis Top-Down Identifikasi Penyebab Kegagalan." },
  { "en": "Apa Itu Failure Mode and Effects Analysis (FMEA)?", "id": "Analisis Bottom-Up Mode Kegagalan." },
  { "en": "Apa Itu Derating?", "id": "Mengoperasikan Komponen Di Bawah Rating Maksimum." },
  { "en": "Apa Itu Kelembaban Relatif?", "id": "Ukuran Kandungan Uap Air Di Udara." },
  { "en": "Apa Efek Kelembaban Pada Elektronika?", "id": "Korosi, Perubahan Sifat Dielektrik." },
  { "en": "Apa Itu Lapisan Konformal (Conformal Coating)?", "id": "Lapisan Pelindung Tipis Pada PCB." },
  { "en": "Apa Itu Hermetic Sealing?", "id": "Penyegelan Kedap Udara Komponen Elektronik." },
  { "en": "Apa Itu Pengujian Getaran (Vibration Testing)?", "id": "Mensimulasikan Stres Mekanis Pada Produk." },
  { "en": "Apa Itu Pengujian Guncangan (Shock Testing)?", "id": "Mensimulasikan Dampak Mekanis Tiba-tiba." },
  { "en": "Apa Itu Siklus Termal (Thermal Cycling)?", "id": "Menguji Produk Dengan Perubahan Suhu Ekstrem." },
  { "en": "Apa Itu Elektromigrasi?", "id": "Pergerakan Ion Logam Akibat Arus Listrik." },
  { "en": "Apa Akibat Elektromigrasi?", "id": "Kegagalan Sirkuit Terpadu (IC)." },
  { "en": "Apa Itu Whiskering?", "id": "Pertumbuhan Filamen Logam Tipis." },
  { "en": "Apa Itu Outgassing?", "id": "Pelepasan Gas Terperangkap Dari Material." },
  { "en": "Mengapa Outgassing Menjadi Masalah Di Vakum?", "id": "Dapat Mengkontaminasi Permukaan Sensitif." },
  { "en": "Apa Itu Sistem Pengereman Regeneratif?", "id": "Mengubah Energi Kinetik Menjadi Listrik." },
  { "en": "Apa Itu Kontrol Medan Terorientasi (FOC)?", "id": "Metode Kontrol Presisi Motor AC." },
  { "en": "Apa Itu Transformasi Clark?", "id": "Mengubah Variabel Tiga Fasa Ke Dua Sumbu." },
  { "en": "Apa Itu Transformasi Park?", "id": "Mengubah Sumbu Diam Ke Sumbu Berputar." },
  { "en": "Apa Itu Pengamat Fluks (Flux Observer)?", "id": "Mengestimasi Fluks Magnet Motor." },
  { "en": "Apa Itu Kontrol Tanpa Sensor (Sensorless)?", "id": "Mengontrol Motor Tanpa Sensor Posisi." },
  { "en": "Bagaimana Kontrol Tanpa Sensor Bekerja?", "id": "Mengestimasi Posisi Dari Back-EMF." },
  { "en": "Apa Itu Riak Torsi (Torque Ripple)?", "id": "Variasi Torsi Periodik Selama Putaran." },
  { "en": "Apa Itu Cogging Torque?", "id": "Riak Torsi Akibat Interaksi Magnet Rotor." },
  { "en": "Apa Itu Motor Sinkron Reluktansi (SynRM)?", "id": "Motor Sinkron Tanpa Magnet, Lilitan Rotor." },
  { "en": "Apa Itu Bus CAN (Controller Area Network)?", "id": "Bus Komunikasi Kuat Untuk Otomotif." },
  { "en": "Apa Itu Protokol FlexRay?", "id": "Bus Komunikasi Kecepatan Tinggi Deterministik." },
  { "en": "Apa Itu Local Interconnect Network (LIN)?", "id": "Bus Komunikasi Biaya Rendah." },
  { "en": "Apa Itu Automotive Ethernet?", "id": "Penggunaan Ethernet Dalam Kendaraan." },
  { "en": "Apa Itu AUTOSAR (Automotive Open System Architecture)?", "id": "Standar Arsitektur Perangkat Lunak Otomotif." },
  { "en": "Apa Itu Standar Keamanan Fungsional ISO 26262?", "id": "Standar Keselamatan Sistem Listrik Otomotif." },
  { "en": "Apa Itu ASIL (Automotive Safety Integrity Level)?", "id": "Tingkat Risiko Dalam Standar ISO 26262." },
  { "en": "Apa Itu Sistem Rem Anti-Terkunci (ABS)?", "id": "Mencegah Roda Terkunci Saat Pengereman." },
  { "en": "Apa Itu Kontrol Stabilitas Elektronik (ESC)?", "id": "Membantu Menjaga Kontrol Kendaraan." },
  { "en": "Apa Itu Kontrol Traksi (Traction Control)?", "id": "Mencegah Roda Selip Saat Akselerasi." },
  { "en": "Apa Itu Electronic Control Unit (ECU)?", "id": "Komputer Tertanam Dalam Kendaraan." },
  { "en": "Apa Itu On-Board Diagnostics (OBD)?", "id": "Sistem Diagnostik Mandiri Kendaraan." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Mode Peningkatan?", "id": "Transistor OFF Saat Tegangan Gerbang-Source Nol." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Mode Deplesi?", "id": "Transistor ON Saat Tegangan Gerbang-Source Nol." },
  { "en": "Apa Itu Tegangan Treshold (Threshold Voltage) MOSFET?", "id": "Tegangan Gerbang Minimum Untuk Menghidupkan Transistor." },
  { "en": "Apa Itu Daerah Ohmik (Triode) MOSFET?", "id": "Daerah Operasi Di Mana MOSFET Bertindak Seperti Resistor." },
  { "en": "Apa Itu Daerah Saturasi MOSFET?", "id": "Daerah Operasi Di Mana Arus Drain Menjadi Konstan." },
  { "en": "Apa Itu Transkonduktansi (gm) MOSFET?", "id": "Ukuran Kontrol Tegangan Gerbang Terhadap Arus Drain." },
  { "en": "Apa Itu Inverter CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Gerbang NOT Menggunakan Transistor PMOS Dan NMOS." },
  { "en": "Apa Keuntungan Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Konsumsi Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Efek Kanal Pendek (Short-Channel Effects)?", "id": "Perilaku Non-ideal Pada MOSFET Berukuran Kecil." },
  { "en": "Apa Itu Latch-up Dalam Sirkuit CMOS?", "id": "Kondisi Hubung Singkat Parasitik." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Jaringan Interkoneksi Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Itu Pembangkitan Terpusat (Centralized Generation)?", "id": "Pembangkit Listrik Skala Besar Jauh Dari Beban." },
  { "en": "Apa Itu Sistem Transmisi HVAC (High-Voltage Alternating Current)?", "id": "Transmisi Arus Bolak-Balik Tegangan Tinggi." },
  { "en": "Apa Itu Sistem Transmisi HVDC (High-Voltage Direct Current)?", "id": "Transmisi Arus Searah Tegangan Tinggi." },
  { "en": "Apa Keuntungan HVDC (High-Voltage Direct Current) Untuk Jarak Jauh?", "id": "Kerugian Daya Lebih Rendah, Tidak Ada Batas Stabilitas." },
  { "en": "Apa Itu Gardu Induk?", "id": "Fasilitas Pengubah Level Tegangan, Pengalih Sirkuit." },
  { "en": "Apa Itu Busbar?", "id": "Batang Konduktor Pusat Dalam Gardu Induk." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Sakelar Otomatis Pelindung Dari Arus Berlebih." },
  { "en": "Apa Itu Sakelar Pemisah (Disconnect Switch)?", "id": "Sakelar Isolasi Visual Tanpa Kemampuan Memutus Beban." },
  { "en": "Apa Itu Relai Proteksi?", "id": "Perangkat Mendeteksi Kondisi Abnormal, Memicu Pemutus." },
  { "en": "Apa Itu Arus Gangguan (Fault Current)?", "id": "Arus Sangat Tinggi Selama Hubung Singkat." },
  { "en": "Apa Itu Studi Aliran Daya (Power Flow Study)?", "id": "Analisis Tegangan, Arus, Aliran Daya Jaringan." },
  { "en": "Apa Itu Analisis Stabilitas Transien?", "id": "Menganalisis Kemampuan Sistem Stabil Setelah Gangguan." },
  { "en": "Apa Itu Inersia Sistem?", "id": "Kemampuan Sistem Menahan Perubahan Frekuensi." },
  { "en": "Apa Itu Mesin Sinkron?", "id": "Mesin AC (Alternating Current) Berputar Pada Kecepatan Sinkron." },
  { "en": "Apa Itu Generator Sinkron?", "id": "Mesin Sinkron Digunakan Sebagai Pembangkit Listrik." },
  { "en": "Apa Itu Eksitasi (Excitation) Generator?", "id": "Menyuplai Arus DC Ke Lilitan Medan Rotor." },
  { "en": "Apa Itu Kurva Kemampuan Generator?", "id": "Grafik Menunjukkan Batas Operasi Aman Generator." },
  { "en": "Apa Itu Kontrol Frekuensi Beban (Load Frequency Control)?", "id": "Menjaga Keseimbangan Antara Pembangkitan, Beban." },
  { "en": "Apa Itu Automatic Generation Control (AGC)?", "id": "Kontrol Pembangkitan Otomatis." },
  { "en": "Apa Itu Sistem Kontrol?", "id": "Sistem Memanipulasi Input Untuk Mendapatkan Output." },
  { "en": "Apa Itu Setpoint?", "id": "Nilai Yang Diinginkan Untuk Variabel Terkontrol." },
  { "en": "Apa Itu Variabel Proses?", "id": "Variabel Fisik Yang Diukur Dan Dikontrol." },
  { "en": "Apa Itu Sinyal Error?", "id": "Perbedaan Antara Setpoint Dan Variabel Proses." },
  { "en": "Apa Itu Diagram Blok?", "id": "Representasi Grafis Sistem Kontrol." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input Dalam Domain-S." },
  { "en": "Apa Itu Kutub (Pole) Sistem?", "id": "Menentukan Karakteristik Respon Alami Sistem." },
  { "en": "Apa Itu Nol (Zero) Sistem?", "id": "Mempengaruhi Amplitudo Dan Fasa Respon Sistem." },
  { "en": "Apa Itu Stabilitas BIBO (Bounded-Input, Bounded-Output)?", "id": "Input Terbatas Menghasilkan Output Terbatas." },
  { "en": "Di Mana Posisi Kutub Untuk Sistem Stabil?", "id": "Di Setengah Kiri Bidang Kompleks S." },
  { "en": "Apa Itu Respon Langkah (Step Response)?", "id": "Output Sistem Terhadap Input Perubahan Mendadak." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Naik Dari 10% Ke 90%." },
  { "en": "Apa Itu Lewatan (Overshoot)?", "id": "Puncak Respon Melebihi Nilai Akhir." },
  { "en": "Apa Itu Waktu Tunak (Settling Time)?", "id": "Waktu Respon Masuk, Tetap Dalam Batas." },
  { "en": "Apa Itu Error Keadaan Tunak?", "id": "Perbedaan Antara Setpoint, Output Setelah Waktu Lama." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Respon Sistem Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Plot Bode?", "id": "Plot Logaritmik Penguatan, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Penguatan DC (Direct Current)?", "id": "Penguatan Sistem Pada Frekuensi Nol." },
  { "en": "Apa Itu Bandwidth Sistem?", "id": "Rentang Frekuensi Operasi Efektif Sistem." },
  { "en": "Apa Itu Sirkuit Terpadu (IC - Integrated Circuit)?", "id": "Komponen Elektronik Dengan Banyak Transistor." },
  { "en": "Apa Itu Fabrikasi Semikonduktor?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Wafer?", "id": "Kepingan Tipis Silikon Dasar Pembuatan IC." },
  { "en": "Apa Itu Fotolitografi?", "id": "Proses Transfer Pola Sirkuit Ke Wafer." },
  { "en": "Apa Itu Etsa (Etching)?", "id": "Menghilangkan Material Secara Selektif Dari Wafer." },
  { "en": "Apa Itu Deposisi?", "id": "Menambahkan Lapisan Tipis Material Ke Wafer." },
  { "en": "Apa Itu Doping Implantasi Ion?", "id": "Menanamkan Ion Dopant Ke Dalam Wafer." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Lingkungan Terkontrol Bebas Kontaminasi." },
  { "en": "Apa Itu Yield?", "id": "Persentase Chip Fungsional Per Wafer." },
  { "en": "Apa Itu Hukum Moore?", "id": "Prediksi Kepadatan Transistor Berlipat Ganda." },
  { "en": "Apa Itu Penskalaan Dennard?", "id": "Aturan Penskalaan Menjaga Kepadatan Daya Konstan." },
  { "en": "Apa Itu Teknologi Proses (Process Node)?", "id": "Mengacu Pada Ukuran Fitur Terkecil Di Chip." },
  { "en": "Apa Itu Die Sirkuit?", "id": "Satu Unit Sirkuit Individual Di Atas Wafer." },
  { "en": "Apa Itu Pengemasan IC (IC Packaging)?", "id": "Melindungi Die Dan Menyediakan Koneksi Eksternal." },
  { "en": "Apa Itu Ball Grid Array (BGA)?", "id": "Jenis Paket Dengan Bola Solder Di Bawahnya." },
  { "en": "Apa Itu Quad Flat Package (QFP)?", "id": "Jenis Paket Dengan Kaki Di Keempat Sisinya." },
  { "en": "Apa Itu Desain Analog?", "id": "Desain Sirkuit Untuk Sinyal Kontinu." },
  { "en": "Apa Itu Desain Digital?", "id": "Desain Sirkuit Untuk Sinyal Diskret." },
  { "en": "Apa Itu Desain Sinyal Campuran (Mixed-Signal)?", "id": "Desain Sirkuit Menggabungkan Analog Dan Digital." },
  { "en": "Apa Itu Bahasa Deskripsi Perangkat Keras (HDL)?", "id": "Bahasa Untuk Mendeskripsikan Sirkuit Digital." },
  { "en": "Apa Itu Sintesis Logika?", "id": "Mengubah Kode HDL Menjadi Implementasi Gerbang." },
  { "en": "Apa Itu Analisis Waktu Statis (STA)?", "id": "Menganalisis Waktu Penundaan Sirkuit Digital." },
  { "en": "Apa Itu Desain Fisik?", "id": "Proses Penempatan Dan Perutean Gerbang Logika." },
  { "en": "Apa Itu Tape-out?", "id": "Tahap Akhir Pengiriman Desain IC Ke Pabrik." },
  { "en": "Apa Itu Sistem pada Chip (SoC)?", "id": "Mengintegrasikan Seluruh Komponen Sistem Satu Chip." },
  { "en": "Apa Itu System-in-Package (SiP)?", "id": "Mengintegrasikan Beberapa Die Dalam Satu Paket." },
  { "en": "Apa Itu Desain Untuk Uji Coba (DFT)?", "id": "Mendesain Sirkuit Agar Mudah Diuji." },
  { "en": "Apa Itu Automatic Test Pattern Generation (ATPG)?", "id": "Menghasilkan Vektor Uji Otomatis." },
  { "en": "Apa Itu Built-In Self-Test (BIST)?", "id": "Sirkuit Pengujian Diri Terintegrasi Dalam Chip." },
  { "en": "Apa Itu Elektromigrasi?", "id": "Pergerakan Atom Logam Penyebab Kegagalan Sirkuit." },
  { "en": "Apa Itu Masalah Integritas Daya?", "id": "Tantangan Menjaga Tegangan Catu Daya Stabil." },
  { "en": "Apa Itu IR Drop?", "id": "Penurunan Tegangan Akibat Resistansi Jaringan Daya." },
  { "en": "Apa Itu Kapasitor Dekopling?", "id": "Menyediakan Cadangan Arus Lokal Mengurangi Derau." },
  { "en": "Apa Itu Crosstalk?", "id": "Interferensi Antara Sinyal Berdekatan." },
  { "en": "Apa Itu Penundaan Propagasi Sinyal?", "id": "Waktu Diperlukan Sinyal Merambat Melalui Kawat." },
  { "en": "Apa Itu Integritas Sinyal?", "id": "Menjaga Kualitas Sinyal Listrik." },
  { "en": "Apa Itu Radiasi Elektromagnetik?", "id": "Energi Yang Merambat Melalui Ruang." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Beroperasi Tanpa Interferensi." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Yang Tidak Diinginkan." },
  { "en": "Apa Itu Emisi Teradiasi?", "id": "EMI (Electromagnetic Interference) Yang Dipancarkan Melalui Udara." },
  { "en": "Apa Itu Emisi Terkonduksi?", "id": "EMI (Electromagnetic Interference) Merambat Melalui Kabel." },
  { "en": "Apa Itu Imunitas Elektromagnetik?", "id": "Kemampuan Perangkat Tahan Terhadap EMI." },
  { "en": "Apa Itu Kandang Faraday?", "id": "Selungkup Konduktif Memblokir Medan Elektromagnetik." },
  { "en": "Apa Itu Grounding?", "id": "Menyediakan Titik Referensi Potensial Nol." },
  { "en": "Apa Itu Shielding?", "id": "Menggunakan Material Konduktif Untuk Memblokir EMI." },
  { "en": "Apa Itu Filtering?", "id": "Menekan Frekuensi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Common-Mode Choke?", "id": "Filter Menekan Derau Common-Mode." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif Menekan Derau Frekuensi Tinggi." },
  { "en": "Apa Itu Fonon (Phonon)?", "id": "Kuantum Getaran Atom Dalam Kisi Kristal." },
  { "en": "Apa Itu Eksiton (Exciton)?", "id": "Keadaan Terikat Elektron Dan Lubang (Hole)." },
  { "en": "Apa Itu Polariton?", "id": "Partikel Kuasi Hasil Gandengan Foton, Eksitasi." },
  { "en": "Apa Itu Teori Pita Energi?", "id": "Model Menjelaskan Konduktivitas Padatan Kristal." },
  { "en": "Apa Itu Massa Efektif Elektron?", "id": "Massa Semu Elektron Dalam Kisi Kristal." },
  { "en": "Apa Itu Hamburan Fonon?", "id": "Penyebab Utama Resistivitas Logam Pada Suhu Kamar." },
  { "en": "Apa Itu Superkonduktor Tipe I?", "id": "Memiliki Satu Medan Kritis, Efek Meissner Sempurna." },
  { "en": "Apa Itu Superkonduktor Tipe II?", "id": "Memiliki Dua Medan Kritis, Keadaan Campuran." },
  { "en": "Apa Itu Pasangan Cooper?", "id": "Pasangan Elektron Penyebab Superkonduktivitas." },
  { "en": "Apa Itu Teori BCS (Bardeen–Cooper–Schrieffer)?", "id": "Teori Mikroskopis Superkonduktivitas." },
  { "en": "Apa Itu Efek Josephson?", "id": "Arus Mengalir Antara Dua Superkonduktor." },
  { "en": "Apa Itu Kuantum Fluks Magnetik?", "id": "Satuan Terkecil Fluks Magnet Dalam Superkonduktor." },
  { "en": "Apa Itu Spin Elektron?", "id": "Momentum Sudut Intrinsik Partikel Elektron." },
  { "en": "Apa Itu Momen Magnetik Elektron?", "id": "Momen Magnetik Akibat Spin Elektron." },
  { "en": "Apa Itu Aturan Tangan Kanan?", "id": "Menentukan Arah Gaya Lorentz, Medan Magnet." },
  { "en": "Apa Itu Efek Peltier?", "id": "Pemanasan Atau Pendinginan Di Sambungan Material." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Perbedaan Suhu Menghasilkan Tegangan Listrik." },
  { "en": "Apa Itu Efek Thomson?", "id": "Pemanasan Atau Pendinginan Konduktor Berarus." },
  { "en": "Apa Itu Angka Jasa Termoelektrik (ZT)?", "id": "Ukuran Efisiensi Material Termoelektrik." },
  { "en": "Apa Itu Efek Piroelektrik?", "id": "Perubahan Suhu Menghasilkan Polarisasi Listrik." },
  { "en": "Apa Itu Material Feroelektrik?", "id": "Material Dengan Polarisasi Listrik Spontan." },
  { "en": "Apa Itu Kurva Histeresis Feroelektrik?", "id": "Plot Polarisasi Terhadap Medan Listrik." },
  { "en": "Apa Itu Efek Elektrokalorik?", "id": "Perubahan Suhu Material Akibat Medan Listrik." },
  { "en": "Apa Itu Elektroluminesensi?", "id": "Emisi Cahaya Material Akibat Arus Listrik." },
  { "en": "Apa Itu Katodoluminesensi?", "id": "Emisi Cahaya Akibat Tumbukan Elektron." },
  { "en": "Apa Itu Fosfor (Phosphor)?", "id": "Material Yang Menunjukkan Fenomena Luminesensi." },
  { "en": "Apa Itu Pendaran (Luminescence)?", "id": "Emisi Cahaya Tanpa Melibatkan Panas." },
  { "en": "Apa Itu Fluoresensi?", "id": "Pendaran Yang Berhenti Segera Setelah Eksitasi." },
  { "en": "Apa Itu Fosforesensi?", "id": "Pendaran Yang Berlanjut Setelah Eksitasi." },
  { "en": "Apa Itu Efek Fotoresistif?", "id": "Nama Lain Untuk Efek Fotokonduktif." },
  { "en": "Apa Itu Hamburan Rayleigh?", "id": "Hamburan Cahaya Oleh Partikel Sangat Kecil." },
  { "en": "Mengapa Langit Berwarna Biru?", "id": "Akibat Hamburan Rayleigh Cahaya Matahari." },
  { "en": "Apa Itu Hamburan Mie?", "id": "Hamburan Cahaya Oleh Partikel Ukuran Sama." },
  { "en": "Mengapa Awan Berwarna Putih?", "id": "Akibat Hamburan Mie Oleh Tetesan Air." },
  { "en": "Apa Itu Emisivitas?", "id": "Efektivitas Permukaan Memancarkan Energi Termal." },
  { "en": "Apa Nilai Emisivitas Benda Hitam Sempurna?", "id": "Satu (1)." },
  { "en": "Apa Itu Reflektivitas?", "id": "Fraksi Radiasi Yang Dipantulkan Oleh Permukaan." },
  { "en": "Apa Itu Transmisivitas?", "id": "Fraksi Radiasi Yang Diteruskan Melalui Material." },
  { "en": "Apa Hubungan Ketiga Sifat Tersebut?", "id": "Jumlah Reflektivitas, Transmisivitas, Absorptivitas Adalah Satu." },
  { "en": "Apa Itu Absorptivitas?", "id": "Fraksi Radiasi Yang Diserap Oleh Permukaan." },
  { "en": "Apa Itu Hukum Kirchhoff Radiasi Termal?", "id": "Emisivitas Sama Dengan Absorptivitas." },
  { "en": "Apa Itu Sudut Padat (Solid Angle)?", "id": "Ukuran Dua Dimensi Sudut Dalam Ruang." },
  { "en": "Apa Satuan Sudut Padat?", "id": "Steradian (Sr)." },
  { "en": "Apa Itu Intensitas Radiasi?", "id": "Daya Radiasi Per Satuan Sudut Padat." },
  { "en": "Apa Itu Radiance?", "id": "Intensitas Radiasi Per Satuan Luas Proyeksi." },
  { "en": "Apa Itu Irradiance?", "id": "Daya Radiasi Diterima Per Satuan Luas." },
  { "en": "Apa Itu Sinyal Jam Diferensial?", "id": "Menggunakan Sepasang Sinyal Komplementer Untuk Jam." },
  { "en": "Apa Keuntungan Jam Diferensial?", "id": "Penolakan Derau Common-Mode Sangat Baik." },
  { "en": "Apa Itu Phase Frequency Detector (PFD)?", "id": "Detektor Fasa Mendeteksi Perbedaan Fasa, Frekuensi." },
  { "en": "Apa Beda PFD Dan Detektor Fasa Sederhana?", "id": "PFD Memiliki Rentang Tangkap Frekuensi Luas." },
  { "en": "Apa Itu Pompa Muatan (Charge Pump)?", "id": "Sirkuit Mengubah Sinyal PFD Menjadi Arus." },
  { "en": "Apa Itu Osilator Cincin (Ring Oscillator)?", "id": "Osilator Dibuat Dari Rantai Inverter." },
  { "en": "Apa Itu Osilator Relaksasi?", "id": "Osilator Berbasis Pengisian, Pengosongan Kapasitor." },
  { "en": "Apa Itu Bandwidth Loop PLL?", "id": "Menentukan Kecepatan Respon Dan Penekanan Derau." },
  { "en": "Apa Itu Spur (Spurious Signal)?", "id": "Komponen Frekuensi Tak Diinginkan Pada Output." },
  { "en": "Apa Itu Integer-N PLL?", "id": "PLL Dengan Pembagi Frekuensi Bilangan Bulat." },
  { "en": "Apa Itu Fractional-N PLL?", "id": "PLL Mengizinkan Rasio Pembagian Pecahan." },
  { "en": "Apa Keuntungan Fractional-N PLL?", "id": "Resolusi Frekuensi Lebih Tinggi, Lock Time Cepat." },
  { "en": "Apa Itu Modulator Delta-Sigma?", "id": "Digunakan Dalam Fractional-N PLL Untuk Membentuk Derau." },
  { "en": "Apa Itu Lock Time PLL?", "id": "Waktu Diperlukan PLL Untuk Mengunci Frekuensi." },
  { "en": "Apa Itu Derau Fasa (Phase Noise)?", "id": "Fluktuasi Acak Fasa Sinyal Osilator." },
  { "en": "Apa Dampak Derau Fasa?", "id": "Membatasi Kinerja Sistem Komunikasi." },
  { "en": "Apa Itu Rangkaian Resistor Diskrit?", "id": "Jaringan Resistor Dengan Nilai Individual." },
  { "en": "Apa Itu Tegangan Tembus Sekunder?", "id": "Fenomena Kegagalan Transistor Bipolar Daya." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Area Operasi Aman Untuk Transistor Daya." },
  { "en": "Apa Itu Forward-Biased Safe Operating Area (FBSOA)?", "id": "SOA Saat Transistor Aktif Menghantar." },
  { "en": "Apa Itu Reverse-Biased Safe Operating Area (RBSOA)?", "id": "SOA Selama Proses Mematikan Transistor." },
  { "en": "Apa Itu Hot Spotting Pada Transistor?", "id": "Pemanasan Lokal Tidak Merata Menyebabkan Kegagalan." },
  { "en": "Apa Itu Selang Waktu (Dead Time) Inverter?", "id": "Penundaan Mencegah Shoot-Through." },
  { "en": "Apa Itu Dioda Bodi (Body Diode)?", "id": "Dioda Parasitik Internal Dalam MOSFET." },
  { "en": "Apa Itu Muatan Pemulihan Balik (Qrr)?", "id": "Muatan Dihilangkan Dari Dioda Saat Mati." },
  { "en": "Apa Dampak Qrr (Reverse Recovery Charge) Tinggi?", "id": "Meningkatkan Kerugian Switching Dan EMI." },
  { "en": "Apa Itu Dioda Freewheeling?", "id": "Menyediakan Jalur Arus Untuk Beban Induktif." },
  { "en": "Apa Itu Soft Recovery Diode?", "id": "Dioda Dengan Proses Pemulihan Balik Halus." },
  { "en": "Apa Itu Kapasitansi Gerbang Miller?", "id": "Kapasitansi Efektif Gerbang-Drain (Cgd)." },
  { "en": "Apa Itu Resistansi Gerbang (Gate Resistance)?", "id": "Resistansi Total Jalur Menuju Gerbang MOSFET." },
  { "en": "Apa Fungsi Resistor Gerbang Eksternal?", "id": "Mengontrol Laju Switching, Meredam Deringan." },
  { "en": "Apa Itu Deringan Gerbang (Gate Ringing)?", "id": "Osilasi Tegangan Gerbang Selama Switching." },
  { "en": "Apa Itu Induktansi Sumber Umum?", "id": "Induktansi Parasitik Di Jalur Source MOSFET." },
  { "en": "Apa Dampak Induktansi Sumber Umum?", "id": "Memperlambat Laju Switching." },
  { "en": "Apa Itu Drive Kelvin-Source?", "id": "Koneksi Terpisah Untuk Jalur Daya, Drive." },
  { "en": "Apa Itu Kapasitansi Junction?", "id": "Kapasitansi Yang Terdapat Pada Simpangan Semikonduktor." },
  { "en": "Apa Itu Induktansi Parasitik?", "id": "Induktansi Tak Diinginkan Pada Jejak PCB." },
  { "en": "Apa Itu Induktansi Loop?", "id": "Induktansi Terkait Luas Loop Jalur Arus." },
  { "en": "Apa Itu Kapasitor Dekopling?", "id": "Menyediakan Jalur Impedansi Rendah Untuk Arus." },
  { "en": "Apa Itu Jalur Arus Balik?", "id": "Jalur Yang Diambil Arus Kembali Ke Sumber." },
  { "en": "Mengapa Jalur Arus Balik Penting?", "id": "Loop Arus Luas Menghasilkan EMI Tinggi." },
  { "en": "Apa Itu Bidang Tanah (Ground Plane)?", "id": "Lapisan Tembaga Luas Sebagai Referensi Ground." },
  { "en": "Apa Itu Bidang Daya (Power Plane)?", "id": "Lapisan Tembaga Luas Untuk Distribusi Daya." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Multi-lapis?", "id": "Papan Sirkuit Dengan Lebih Dari Dua Lapisan." },
  { "en": "Apa Itu Via Pada PCB?", "id": "Koneksi Vertikal Antar Lapisan PCB." },
  { "en": "Apa Itu Blind Via?", "id": "Menghubungkan Lapisan Luar Ke Lapisan Dalam." },
  { "en": "Apa Itu Buried Via?", "id": "Menghubungkan Dua Lapisan Dalam Tanpa Ke Luar." },
  { "en": "Apa Itu Impedansi Terkontrol?", "id": "Desain Jejak PCB Untuk Impedansi Spesifik." },
  { "en": "Mengapa Impedansi Terkontrol Diperlukan?", "id": "Untuk Integritas Sinyal Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Integritas Sinyal (Signal Integrity)?", "id": "Kualitas Sinyal Listrik." },
  { "en": "Apa Itu Refleksi Sinyal?", "id": "Terjadi Saat Impedansi Tidak Sesuai." },
  { "en": "Apa Itu Overshoot Dan Undershoot?", "id": "Sinyal Melebihi Atau Kurang Dari Level." },
  { "en": "Apa Itu Terminasi Saluran Transmisi?", "id": "Menambahkan Resistor Untuk Mencegah Refleksi." },
  { "en": "Apa Itu Terminasi Seri?", "id": "Resistor Ditempatkan Dekat Sumber Sinyal." },
  { "en": "Apa Itu Terminasi Paralel?", "id": "Resistor Ditempatkan Di Ujung Saluran." },
  { "en": "Apa Itu Terminasi Thevenin?", "id": "Pembagi Tegangan Sebagai Terminasi Paralel." },
  { "en": "Apa Itu Crosstalk?", "id": "Gangguan Antara Jejak Sinyal Berdekatan." },
  { "en": "Apa Itu Near-End Crosstalk (NEXT)?", "id": "Crosstalk Diukur Di Ujung Sumber." },
  { "en": "Apa Itu Far-End Crosstalk (FEXT)?", "id": "Crosstalk Diukur Di Ujung Beban." },
  { "en": "Bagaimana Mengurangi Crosstalk?", "id": "Menjauhkan Jejak, Menggunakan Bidang Tanah." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Alat Visual Menganalisis Kualitas Sinyal Digital." },
  { "en": "Apa Itu Jitter?", "id": "Deviasi Waktu Tepi Sinyal Dari Ideal." },
  { "en": "Apa Itu Pemrosesan Citra (Image Processing)?", "id": "Analisis Dan Manipulasi Gambar Digital." },
  { "en": "Apa Itu Piksel (Pixel)?", "id": "Elemen Individual Terkecil Dari Gambar." },
  { "en": "Apa Itu Resolusi Spasial?", "id": "Ukuran Detail Dalam Sebuah Gambar." },
  { "en": "Apa Itu Resolusi Intensitas?", "id": "Jumlah Level Keabuan Dalam Gambar." },
  { "en": "Apa Itu Histogram Gambar?", "id": "Grafik Distribusi Tingkat Keabuan Piksel." },
  { "en": "Apa Itu Ekualisasi Histogram?", "id": "Teknik Untuk Meningkatkan Kontras Gambar." },
  { "en": "Apa Itu Operasi Titik (Point Operation)?", "id": "Memodifikasi Piksel Berdasarkan Nilainya Sendiri." },
  { "en": "Apa Itu Operasi Spasial (Spatial Operation)?", "id": "Memodifikasi Piksel Berdasarkan Lingkungannya." },
  { "en": "Apa Itu Konvolusi Dalam Pemrosesan Citra?", "id": "Menerapkan Kernel Ke Setiap Piksel." },
  { "en": "Apa Itu Kernel Konvolusi?", "id": "Matriks Kecil Mendefinisikan Operasi Filter." },
  { "en": "Apa Itu Filter Penajaman (Sharpening Filter)?", "id": "Menonjolkan Tepi Dan Detail Gambar." },
  { "en": "Apa Itu Filter Penghalusan (Smoothing Filter)?", "id": "Mengurangi Derau Dan Detail Gambar." },
  { "en": "Apa Itu Filter Rerata (Mean Filter)?", "id": "Filter Penghalus Menggunakan Rata-rata Lingkungan." },
  { "en": "Apa Itu Filter Median?", "id": "Efektif Menghilangkan Derau Salt-and-Pepper." },
  { "en": "Apa Itu Filter Gaussian?", "id": "Filter Penghalus Berdasarkan Fungsi Gaussian." },
  { "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Mengidentifikasi Diskontinuitas Kecerahan Dalam Gambar." },
  { "en": "Apa Itu Operator Sobel?", "id": "Operator Deteksi Tepi Berbasis Gradien." },
  { "en": "Apa Itu Operator Prewitt?", "id": "Operator Deteksi Tepi Mirip Sobel." },
  { "en": "Apa Itu Operator Laplacian?", "id": "Operator Deteksi Tepi Orde Kedua." },
  { "en": "Apa Itu Detektor Tepi Canny?", "id": "Algoritma Deteksi Tepi Multi-tahap." },
  { "en": "Apa Itu Transformasi Hough?", "id": "Teknik Mendeteksi Bentuk (Garis, Lingkaran)." },
  { "en": "Apa Itu Segmentasi Citra?", "id": "Membagi Gambar Menjadi Beberapa Wilayah." },
  { "en": "Apa Itu Thresholding?", "id": "Metode Segmentasi Berdasarkan Nilai Ambang." },
  { "en": "Apa Itu Morfologi Matematis?", "id": "Operasi Citra Berbasis Teori Himpunan." },
  { "en": "Apa Itu Operasi Dilasi?", "id": "Memperluas Atau Menebalkan Objek Terang." },
  { "en": "Apa Itu Operasi Erosi?", "id": "Menyusutkan Atau Menipiskan Objek Terang." },
  { "en": "Apa Itu Operasi Pembukaan (Opening)?", "id": "Erosi Diikuti Dilasi, Menghilangkan Derau." },
  { "en": "Apa Itu Operasi Penutupan (Closing)?", "id": "Dilasi Diikuti Erosi, Menutup Lubang." },
  { "en": "Apa Itu Ekstraksi Fitur?", "id": "Mengidentifikasi Karakteristik Kunci Dalam Gambar." },
  { "en": "Apa Itu Model Warna RGB (Red, Green, Blue)?", "id": "Model Warna Aditif Untuk Layar." },
  { "en": "Apa Itu Model Warna HSV (Hue, Saturation, Value)?", "id": "Model Warna Representasi Persepsi Manusia." },
  { "en": "Apa Itu Citra Grayscale?", "id": "Gambar Dengan Informasi Intensitas Cahaya Saja." },
  { "en": "Apa Itu Kompresi Citra?", "id": "Mengurangi Ukuran Data Gambar." },
  { "en": "Apa Itu Kompresi JPEG (Joint Photographic Experts Group)?", "id": "Standar Kompresi Citra Lossy Populer." },
  { "en": "Apa Itu Kompresi PNG (Portable Network Graphics)?", "id": "Standar Kompresi Citra Lossless." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Bidang AI Menginterpretasi Dunia Visual." },
  { "en": "Apa Itu Pemulihan Citra (Image Restoration)?", "id": "Memperbaiki Gambar Yang Terdegradasi." },
  { "en": "Apa Itu Derau Gaussian?", "id": "Derau Acak Dengan Distribusi Normal." },
  { "en": "Apa Itu Derau Salt-and-Pepper?", "id": "Piksel Hitam, Putih Acak Dalam Gambar." },
  { "en": "Apa Itu Filter Wiener?", "id": "Filter Optimal Mengurangi Derau." },
  { "en": "Apa Itu Optical Flow?", "id": "Pola Gerakan Objek Dalam Urutan Video." },
  { "en": "Apa Itu Deteksi Objek?", "id": "Menemukan Instans Objek Dalam Gambar." },
  { "en": "Apa Itu Pengenalan Wajah?", "id": "Mengidentifikasi Seseorang Dari Gambar Wajah." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "Arsitektur Deep Learning Untuk Visi Komputer." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning)?", "id": "Algoritma Komputer Belajar Dari Data." },
  { "en": "Apa Itu Pembelajaran Terawasi?", "id": "Belajar Dari Contoh Data Berlabel." },
  { "en": "Apa Itu Pembelajaran Tak Terawasi?", "id": "Menemukan Pola Dalam Data Tak Berlabel." },
  { "en": "Apa Itu Klasifikasi?", "id": "Memprediksi Label Kategori Diskret." },
  { "en": "Apa Itu Regresi?", "id": "Memprediksi Nilai Numerik Kontinu." },
  { "en": "Apa Itu Clustering?", "id": "Mengelompokkan Titik Data Mirip." },
  { "en": "Apa Itu Overfitting?", "id": "Model Terlalu Kompleks, Gagal Generalisasi." },
  { "en": "Apa Itu Validasi Silang (Cross-Validation)?", "id": "Teknik Mengevaluasi Kinerja Model." },
  { "en": "Apa Itu Pohon Keputusan?", "id": "Model Klasifikasi Berbentuk Struktur Pohon." },
  { "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Algoritma Klasifikasi Mencari Hyperplane Optimal." },
  { "en": "Apa Itu K-Means Clustering?", "id": "Algoritma Pengelompokan Mempartisi Data K Kelompok." },
  { "en": "Apa Itu Pengurangan Dimensi?", "id": "Mengurangi Jumlah Variabel Input." },
  { "en": "Apa Itu Principal Component Analysis (PCA)?", "id": "Teknik Populer Untuk Pengurangan Dimensi." },
  { "en": "Apa Itu Gradient Descent?", "id": "Algoritma Optimisasi Meminimalkan Fungsi Biaya." },
  { "en": "Apa Itu Laju Pembelajaran (Learning Rate)?", "id": "Ukuran Langkah Dalam Algoritma Gradient Descent." },
  { "en": "Apa Itu Fungsi Kerugian (Loss Function)?", "id": "Mengukur Kesalahan Prediksi Model." },
  { "en": "Apa Itu Regularisasi?", "id": "Teknik Mencegah Overfitting Dalam Model." },
  { "en": "Apa Itu Regularisasi L1 (Lasso)?", "id": "Menambahkan Pinalti Absolut Ke Fungsi Kerugian." },
  { "en": "Apa Itu Regularisasi L2 (Ridge)?", "id": "Menambahkan Pinalti Kuadrat Ke Fungsi Kerugian." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Otak Manusia." },
  { "en": "Apa Itu Deep Learning?", "id": "Jaringan Saraf Dengan Banyak Lapisan Tersembunyi." },
  { "en": "Apa Itu Fungsi Aktivasi?", "id": "Memperkenalkan Non-Linearitas Ke Dalam Jaringan." },
  { "en": "Apa Itu Fungsi Aktivasi ReLU?", "id": "Rectified Linear Unit, Banyak Digunakan." },
  { "en": "Apa Itu Propagasi Balik (Backpropagation)?", "id": "Algoritma Melatih Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Dropout Dalam Jaringan Saraf?", "id": "Teknik Regularisasi Menonaktifkan Neuron Acak." },
  { "en": "Apa Itu Pemrosesan Bahasa Alami (NLP)?", "id": "Interaksi Komputer Dengan Bahasa Manusia." },
  { "en": "Apa Itu Tokenisasi?", "id": "Memecah Teks Menjadi Kata Atau Token." },
  { "en": "Apa Itu Stemming?", "id": "Mengurangi Kata Ke Bentuk Akarnya." },
  { "en": "Apa Itu Lemmatization?", "id": "Mengubah Kata Ke Bentuk Leksikalnya." },
  { "en": "Apa Itu Representasi Bag-of-Words (BoW)?", "id": "Model Merepresentasikan Teks Sebagai Kumpulan Kata." },
  { "en": "Apa Itu TF-IDF?", "id": "Term Frequency-Inverse Document Frequency." },
  { "en": "Apa Itu Penyematan Kata (Word Embedding)?", "id": "Representasi Vektor Kata Dalam Ruang." },
  { "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Jaringan Saraf Untuk Memproses Data Sekuensial." },
  { "en": "Apa Itu Vanishing Gradient Problem?", "id": "Masalah Pelatihan RNN (Recurrent Neural Network)." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Jenis RNN Mengatasi Masalah Vanishing Gradient." },
  { "en": "Apa Itu Attention Mechanism?", "id": "Mekanisme Memungkinkan Model Fokus Input." },
  { "en": "Apa Itu Arsitektur Transformer?", "id": "Model Populer NLP Berbasis Attention." },
  { "en": "Apa Itu Pembelajaran Penguatan (Reinforcement Learning)?", "id": "Agen Belajar Dari Interaksi Lingkungan." },
  { "en": "Apa Itu Kebijakan (Policy)?", "id": "Strategi Agen Memilih Tindakan." },
  { "en": "Apa Itu Fungsi Nilai (Value Function)?", "id": "Memprediksi Total Hadiah Di Masa Depan." },
  { "en": "Apa Itu Algoritma Q-Learning?", "id": "Algoritma Pembelajaran Penguatan Tanpa Model." },
  { "en": "Apa Itu Deep Q-Network (DQN)?", "id": "Menggabungkan Q-Learning Dengan Jaringan Saraf Dalam." },
  { "en": "Apa Itu Eksplorasi Vs Eksploitasi?", "id": "Dilema Dalam Pembelajaran Penguatan." },
  { "en": "Apa Itu Rekayasa Fitur (Feature Engineering)?", "id": "Membuat Fitur Input Baru Dari Data." },
  { "en": "Apa Itu Prapemrosesan Data?", "id": "Mempersiapkan Data Mentah Untuk Model." },
  { "en": "Apa Itu Normalisasi Data?", "id": "Menskalakan Fitur Ke Rentang Tertentu." },
  { "en": "Apa Itu Standarisasi Data?", "id": "Menskalakan Fitur Rata-rata Nol, Varian Satu." },
  { "en": "Apa Itu One-Hot Encoding?", "id": "Merepresentasikan Variabel Kategorikal Secara Biner." },
  { "en": "Apa Itu Matriks Konfusi (Confusion Matrix)?", "id": "Tabel Mengevaluasi Kinerja Klasifikasi." },
  { "en": "Apa Itu Akurasi?", "id": "Fraksi Prediksi Benar Dari Total." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Rasio Positif Benar Dari Prediksi Positif." },
  { "en": "Apa Itu Recall (Sensitivity)?", "id": "Rasio Positif Benar Dari Semua Aktual." },
  { "en": "Apa Itu F1-Score?", "id": "Rata-rata Harmonik Presisi Dan Recall." },
  { "en": "Apa Itu Kurva ROC (Receiver Operating Characteristic)?", "id": "Plot Kinerja Klasifikasi Biner." },
  { "en": "Apa Itu Area Under Curve (AUC)?", "id": "Ukuran Kinerja Model Klasifikasi." },
  { "en": "Apa Itu Pengujian Hipotesis?", "id": "Metode Statistik Mengambil Keputusan Data." },
  { "en": "Apa Itu P-value?", "id": "Probabilitas Mengamati Data, Hipotesis Nol." },
  { "en": "Apa Itu Tingkat Signifikansi (Alpha)?", "id": "Ambang Batas Menolak Hipotesis Nol." },
  { "en": "Apa Itu Statistik Deskriptif?", "id": "Meringkas Fitur Utama Kumpulan Data." },
  { "en": "Apa Itu Statistik Inferensial?", "id": "Membuat Kesimpulan Populasi Berdasarkan Sampel." },
  { "en": "Apa Itu Mean, Median, Dan Modus?", "id": "Ukuran Tendensi Sentral Dalam Statistik." },
  { "en": "Apa Itu Rentang (Range)?", "id": "Selisih Antara Nilai Maksimum Dan Minimum." },
  { "en": "Apa Itu Varians?", "id": "Ukuran Rata-rata Kuadrat Jarak Dari Mean." },
  { "en": "Apa Itu Simpangan Baku (Standard Deviation)?", "id": "Akar Kuadrat Dari Varians." },
  { "en": "Apa Itu Koefisien Korelasi?", "id": "Ukuran Kekuatan Hubungan Linear Dua Variabel." },
  { "en": "Apa Itu Distribusi Probabilitas?", "id": "Fungsi Matematis Menjelaskan Kemungkinan Hasil." },
  { "en": "Apa Itu Distribusi Normal (Gaussian)?", "id": "Distribusi Probabilitas Berbentuk Lonceng." },
  { "en": "Apa Itu Teorema Batas Pusat?", "id": "Jumlah Variabel Acak Cenderung Berdistribusi Normal." },
  { "en": "Apa Itu Sampel Acak?", "id": "Subset Populasi Dipilih Secara Acak." },
  { "en": "Apa Itu Bias Sampling?", "id": "Terjadi Saat Sampel Tidak Mewakili Populasi." },
  { "en": "Apa Itu Robotika?", "id": "Bidang Teknik Berurusan Desain, Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF)?", "id": "Jumlah Gerakan Independen Yang Dimiliki Robot." },
  { "en": "Apa Itu Kinematika Maju?", "id": "Menghitung Posisi End-Effector Dari Sudut Sendi." },
  { "en": "Apa Itu Kinematika Terbalik?", "id": "Menghitung Sudut Sendi Untuk Posisi End-Effector." },
  { "en": "Apa Itu Ruang Kerja (Workspace) Robot?", "id": "Area Jangkauan End-Effector Robot." },
  { "en": "Apa Itu Aktuator?", "id": "Komponen Yang Menghasilkan Gerakan (Motor)." },
  { "en": "Apa Itu End-Effector?", "id": "Alat Di Ujung Lengan Robot." },
  { "en": "Apa Itu Proprioceptive Sensor?", "id": "Sensor Mengukur Keadaan Internal Robot." },
  { "en": "Apa Itu Exteroceptive Sensor?", "id": "Sensor Mengukur Lingkungan Sekitar Robot." },
  { "en": "Apa Itu Perencanaan Jalur (Path Planning)?", "id": "Menemukan Jalur Bebas Hambatan Untuk Robot." },
  { "en": "Apa Itu Simultaneous Localization and Mapping (SLAM)?", "id": "Robot Membangun Peta, Melacak Posisinya." },
  { "en": "Apa Itu Sistem Kontrol Real-Time?", "id": "Sistem Menjamin Respon Dalam Batas Waktu." },
  { "en": "Apa Itu Deadline Dalam Sistem Real-Time?", "id": "Batas Waktu Maksimum Penyelesaian Tugas." },
  { "en": "Apa Itu Sistem Real-Time Keras (Hard)?", "id": "Keterlambatan Melebihi Deadline Menyebabkan Kegagalan." },
  { "en": "Apa Itu Sistem Real-Time Lunak (Soft)?", "id": "Keterlambatan Melebihi Deadline Menurunkan Kualitas." },
  { "en": "Apa Itu Jitter Dalam Sistem Real-Time?", "id": "Variasi Waktu Eksekusi Suatu Tugas." },
  { "en": "Apa Itu Inversi Prioritas?", "id": "Tugas Prioritas Rendah Menghalangi Prioritas Tinggi." },
  { "en": "Apa Itu Sistem Operasi Real-Time (RTOS)?", "id": "Sistem Operasi Dirancang Untuk Aplikasi Real-Time." },
  { "en": "Apa Itu Penjadwalan Rate-Monotonic (RMS)?", "id": "Algoritma Penjadwalan Statis Prioritas." },
  { "en": "Apa Itu Penjadwalan Earliest Deadline First (EDF)?", "id": "Algoritma Penjadwalan Dinamis Prioritas." },
  { "en": "Apa Itu Arsitektur Komputer?", "id": "Desain Konseptual, Struktur Operasional Komputer." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Menggunakan Memori Sama Untuk Instruksi, Data." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memisahkan Memori Untuk Instruksi Dan Data." },
  { "en": "Apa Itu Instruction Set Architecture (ISA)?", "id": "Bagian Arsitektur Terlihat Oleh Pemrogram." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Arsitektur Dengan Set Instruksi Sederhana." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Arsitektur Dengan Set Instruksi Kompleks." },
  { "en": "Apa Itu Pipelining?", "id": "Tumpang Tindih Eksekusi Beberapa Instruksi." },
  { "en": "Apa Itu Bahaya Pipeline (Pipeline Hazard)?", "id": "Kondisi Mencegah Instruksi Berikutnya Dijalankan." },
  { "en": "Apa Itu Memori Cache?", "id": "Memori Kecil, Cepat Menyimpan Data Sering." },
  { "en": "Apa Itu Prinsip Lokalitas?", "id": "Kecenderungan Mengakses Data, Instruksi Berdekatan." },
  { "en": "Apa Itu Cache Hit?", "id": "Data Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss?", "id": "Data Yang Dicari Tidak Ditemukan Di Cache." },
  { "en": "Apa Itu Koherensi Cache?", "id": "Menjaga Konsistensi Data Antar Beberapa Cache." },
  { "en": "Apa Itu Memori Virtual?", "id": "Menggunakan Penyimpanan Sekunder Seolah-olah Memori Utama." },
  { "en": "Apa Itu Unit Manajemen Memori (MMU)?", "id": "Menerjemahkan Alamat Virtual Ke Fisik." },
  { "en": "Apa Itu Komputasi Paralel?", "id": "Menjalankan Banyak Tugas Secara Bersamaan." },
  { "en": "Apa Itu Taksonomi Flynn?", "id": "Klasifikasi Arsitektur Komputer Paralel." },
  { "en": "Apa Itu SIMD (Single Instruction, Multiple Data)?", "id": "Satu Instruksi Diterapkan Ke Banyak Data." },
  { "en": "Apa Itu MIMD (Multiple Instruction, Multiple Data)?", "id": "Beberapa Instruksi Diterapkan Ke Banyak Data." },
  { "en": "Apa Itu Hukum Amdahl?", "id": "Mendefinisikan Batas Peningkatan Kinerja Paralel." },
  { "en": "Apa Itu Jaringan Interkoneksi?", "id": "Menghubungkan Prosesor Dan Memori Paralel." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Pola Susunan Komponen Jaringan." },
  { "en": "Apa Itu Komputasi Terdistribusi?", "id": "Komputasi Melibatkan Beberapa Komputer Otonom." },
  { "en": "Apa Itu Komputasi Grid?", "id": "Komputasi Terdistribusi Skala Besar." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Menyediakan Sumber Daya Komputasi Melalui Internet." },
  { "en": "Apa Itu Virtualisasi?", "id": "Membuat Versi Virtual Sumber Daya Komputasi." },
  { "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Membuat, Menjalankan Mesin Virtual." },
  { "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Menyediakan Infrastruktur Komputasi Virtual." },
  { "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Menyediakan Platform Pengembangan Aplikasi." },
  { "en": "Apa Itu SaaS (Software as a Service)?", "id": "Menyediakan Aplikasi Perangkat Lunak Melalui Internet." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Terdedikasi Fungsi Tertentu." },
  { "en": "Apa Karakteristik Sistem Tertanam?", "id": "Dibatasi Daya, Ukuran, Dan Biaya." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Sirkuit Terpadu Berisi Prosesor, Memori, I/O." },
  { "en": "Apa Itu GPIO (General-Purpose Input/Output)?", "id": "Pin Digital Dapat Diprogram Sebagai Input/Output." },
  { "en": "Apa Itu Interupsi?", "id": "Sinyal Memicu CPU Menghentikan Tugasnya." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Mereset Sistem Jika Terjadi Kegagalan Perangkat Lunak." },
  { "en": "Apa Itu Protokol Komunikasi Serial?", "id": "Contoh: UART, SPI, I2C." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak?", "id": "Disiplin Desain, Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Siklus Hidup Pengembangan Perangkat Lunak (SDLC)?", "id": "Proses Untuk Membangun Perangkat Lunak." },
  { "en": "Apa Itu Model Air Terjun (Waterfall)?", "id": "Model SDLC (Software Development Life Cycle) Sekuensial Linear." },
  { "en": "Apa Itu Metodologi Agile?", "id": "Pendekatan Iteratif Dan Inkremental." },
  { "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Manajemen Proyek Agile." },
  { "en": "Apa Itu Pengujian Perangkat Lunak?", "id": "Proses Mengevaluasi Fungsionalitas Perangkat Lunak." },
  { "en": "Apa Itu Pengujian Kotak Hitam (Black-Box)?", "id": "Pengujian Berdasarkan Fungsionalitas Tanpa Kode." },
  { "en": "Apa Itu Pengujian Kotak Putih (White-Box)?", "id": "Pengujian Berdasarkan Struktur Internal Kode." },
  { "en": "Apa Itu Sistem Kontrol Versi?", "id": "Mengelola Perubahan Kode Sumber Seiring Waktu." },
  { "en": "Apa Itu Git?", "id": "Sistem Kontrol Versi Terdistribusi Populer." },
  { "en": "Apa Itu DevOps?", "id": "Kultur Menggabungkan Pengembangan, Operasi TI." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Mengintegrasikan Perubahan Kode Secara Berkala." },
  { "en": "Apa Itu Continuous Delivery (CD)?", "id": "Merilis Perubahan Kode Ke Produksi Otomatis." },
  { "en": "Apa Itu Kontainerisasi?", "id": "Mengemas Aplikasi Dengan Dependensinya." },
  { "en": "Apa Itu Docker?", "id": "Platform Populer Untuk Kontainerisasi." },
  { "en": "Apa Itu Kubernetes?", "id": "Sistem Orkestrasi Kontainer Open-Source." },
  { "en": "Apa Itu Arsitektur Layanan Mikro (Microservices)?", "id": "Membangun Aplikasi Sebagai Kumpulan Layanan Kecil." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Menentukan Bagaimana Komponen Perangkat Lunak Berinteraksi." },
  { "en": "Apa Itu RESTful API?", "id": "API Yang Mengikuti Prinsip REST." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Melindungi Sistem Komputer Dari Ancaman." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Jahat (Virus, Worm)." },
  { "en": "Apa Itu Phishing?", "id": "Serangan Rekayasa Sosial Mendapatkan Informasi." },
  { "en": "Apa Itu Serangan Denial-of-Service (DoS)?", "id": "Membuat Layanan Tidak Tersedia Bagi Pengguna." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengubah Data Menjadi Kode Rahasia." },
  { "en": "Apa Itu Kriptografi Kunci Publik?", "id": "Menggunakan Pasangan Kunci Publik, Privat." },
  { "en": "Apa Itu Firewall Jaringan?", "id": "Memantau Dan Mengontrol Lalu Lintas Jaringan." },
  { "en": "Apa Itu Sistem Deteksi Intrusi (IDS)?", "id": "Memantau Aktivitas Mencurigakan." },
  { "en": "Apa Itu Database?", "id": "Kumpulan Data Terstruktur." },
  { "en": "Apa Itu Sistem Manajemen Database (DBMS)?", "id": "Perangkat Lunak Mengelola Database." },
  { "en": "Apa Itu Database Relasional?", "id": "Database Berbasis Model Relasional (Tabel)." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Standar Mengelola Database Relasional." },
  { "en": "Apa Itu Kunci Primer (Primary Key)?", "id": "Pengenal Unik Untuk Setiap Baris Tabel." },
  { "en": "Apa Itu Kunci Asing (Foreign Key)?", "id": "Menciptakan Hubungan Antara Dua Tabel." },
  { "en": "Apa Itu Normalisasi Database?", "id": "Mengurangi Redundansi Data." },
  { "en": "Apa Itu Transaksi ACID?", "id": "Atomicity, Consistency, Isolation, Durability." },
  { "en": "Apa Itu Database NoSQL?", "id": "Database Non-relasional Untuk Data Skala Besar." },
  { "en": "Apa Itu Teorema CAP?", "id": "Konsistensi, Ketersediaan, Dan Toleransi Partisi." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Menggunakan Prinsip Kuantum Untuk Komputasi." },
  { "en": "Apa Itu Qubit (Quantum Bit)?", "id": "Unit Dasar Informasi Dalam Komputasi Kuantum." },
  { "en": "Apa Itu Superposisi Kuantum?", "id": "Qubit Dapat Berada Dalam Beberapa Keadaan Sekaligus." },
  { "en": "Apa Itu Keterikatan Kuantum (Entanglement)?", "id": "Koneksi Tak Terlihat Antara Dua Qubit." },
  { "en": "Apa Itu Algoritma Shor?", "id": "Algoritma Kuantum Untuk Faktorisasi Bilangan Prima." },
  { "en": "Apa Itu Algoritma Grover?", "id": "Algoritma Kuantum Untuk Pencarian Database." },
  { "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital Terdistribusi, Tak Terubah." },
  { "en": "Apa Itu Kontrak Pintar (Smart Contract)?", "id": "Program Otomatis Berjalan Di Atas Blockchain." },
  { "en": "Apa Itu Teori Relativitas Khusus?", "id": "Teori Einstein Tentang Ruang Dan Waktu." },
  { "en": "Apa Itu Dilatasi Waktu?", "id": "Waktu Berjalan Lebih Lambat Bagi Pengamat Bergerak." },
  { "en": "Apa Itu Kontraksi Panjang?", "id": "Objek Tampak Lebih Pendek Saat Bergerak Cepat." },
  { "en": "Apa Itu E = mc²?", "id": "Kesetaraan Antara Massa Dan Energi." },
  { "en": "Apa Itu Teori Relativitas Umum?", "id": "Teori Einstein Tentang Gravitasi." },
  { "en": "Bagaimana Gravitasi Bekerja Menurut Einstein?", "id": "Sebagai Kelengkungan Dari Ruang-Waktu." },
  { "en": "Apa Itu Lensa Gravitasi?", "id": "Pembelokan Cahaya Oleh Objek Bermassa Besar." },
  { "en": "Apa Itu Lubang Hitam (Black Hole)?", "id": "Wilayah Gravitasi Sangat Kuat." },
  { "en": "Apa Itu Horison Peristiwa (Event Horizon)?", "id": "Batas Tanpa Jalan Kembali Dari Lubang Hitam." },
  { "en": "Apa Itu Model Standar Fisika Partikel?", "id": "Teori Partikel Elementer Dan Interaksinya." },
  { "en": "Apa Saja Empat Gaya Fundamental?", "id": "Gravitasi, Elektromagnetik, Kuat, Dan Lemah." },
  { "en": "Apa Itu Kuark (Quark)?", "id": "Partikel Elementer Penyusun Proton Dan Neutron." },
  { "en": "Apa Itu Boson Higgs?", "id": "Partikel Yang Memberikan Massa Pada Partikel Lain." },
  { "en": "Apa Itu Termodinamika?", "id": "Studi Tentang Panas, Kerja, Dan Energi." },
  { "en": "Apa Bunyi Hukum Pertama Termodinamika?", "id": "Energi Tidak Dapat Diciptakan Atau Dimusnahkan." },
  { "en": "Apa Bunyi Hukum Kedua Termodinamika?", "id": "Entropi Total Alam Semesta Selalu Meningkat." },
  { "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakteraturan Atau Keacakan Sistem." },
  { "en": "Apa Itu Efisiensi Carnot?", "id": "Efisiensi Teoritis Maksimum Mesin Panas." },
  { "en": "Apa Itu Mekanika Statistik?", "id": "Menghubungkan Perilaku Mikroskopis Dengan Sifat Makroskopis." },
  { "en": "Apa Itu Mekanika Kuantum?", "id": "Teori Fisika Di Tingkat Atom, Subatom." },
  { "en": "Apa Itu Fungsi Gelombang?", "id": "Deskripsi Matematis Keadaan Kuantum Suatu Sistem." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Batas Akurasi Pengukuran Pasangan Variabel." },
  { "en": "Apa Itu Optik Geometris?", "id": "Mempelajari Perambatan Cahaya Sebagai Sinar." },
  { "en": "Apa Itu Optik Fisik?", "id": "Mempelajari Sifat Gelombang Cahaya." },
  { "en": "Apa Itu Difraksi?", "id": "Penyebaran Gelombang Saat Melewati Celah." },
  { "en": "Apa Itu Interferensi?", "id": "Superposisi Dua Atau Lebih Gelombang." },
  { "en": "Apa Itu Laser?", "id": "Sumber Cahaya Koheren Dan Monokromatik." },
  { "en": "Apa Itu Koherensi Cahaya?", "id": "Hubungan Fasa Yang Konstan Antar Gelombang." },
  { "en": "Apa Itu Mekanika Fluida?", "id": "Studi Tentang Fluida (Cairan, Gas)." },
  { "en": "Apa Itu Viskositas?", "id": "Ukuran Ketahanan Fluida Terhadap Aliran." },
  { "en": "Apa Itu Prinsip Bernoulli?", "id": "Hubungan Antara Kecepatan, Tekanan Fluida." },
  { "en": "Apa Itu Bilangan Reynolds?", "id": "Menentukan Jenis Aliran (Laminar/Turbulen)." },
  { "en": "Apa Itu Ilmu Material?", "id": "Mempelajari Hubungan Struktur, Sifat Material." },
  { "en": "Apa Itu Struktur Kristal?", "id": "Susunan Atom Yang Teratur Dalam Padatan." },
  { "en": "Apa Itu Cacat Kristal?", "id": "Ketidaksempurnaan Dalam Struktur Kristal." },
  { "en": "Apa Itu Dislokasi?", "id": "Jenis Cacat Garis Dalam Kristal." },
  { "en": "Apa Itu Diagram Fasa?", "id": "Peta Menunjukkan Fasa Material Berbeda." },
  { "en": "Apa Itu Kekuatan Tarik (Tensile Strength)?", "id": "Tegangan Maksimum Material Sebelum Patah." },
  { "en": "Apa Itu Elastisitas?", "id": "Kemampuan Benda Kembali Ke Bentuk Semula." },
  { "en": "Apa Itu Plastisitas?", "id": "Perubahan Bentuk Permanen Suatu Material." },
  { "en": "Apa Itu Kelelahan (Fatigue) Material?", "id": "Kegagalan Akibat Beban Berulang." },
  { "en": "Apa Itu Rayapan (Creep)?", "id": "Deformasi Lambat Material Akibat Tegangan Konstan." },
  { "en": "Apa Itu Nanoteknologi?", "id": "Manipulasi Materi Pada Skala Atom, Molekuler." },
  { "en": "Apa Itu Tabung Nano Karbon?", "id": "Struktur Karbon Silinder Dengan Sifat Unik." },
  { "en": "Apa Itu Grafena (Graphene)?", "id": "Lapisan Tunggal Atom Karbon." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Mengubah Energi Kimia Menjadi Listrik." },
  { "en": "Apa Itu Elektrolisis?", "id": "Menggunakan Listrik Menguraikan Senyawa Kimia." },
  { "en": "Apa Itu Korosi?", "id": "Kerusakan Material Akibat Reaksi Kimia." },
  { "en": "Apa Itu Proteksi Katodik?", "id": "Metode Mencegah Korosi Pada Logam." },
  { "en": "Apa Itu Anodisasi?", "id": "Proses Pelapisan Oksida Pelindung Pada Logam." },
  { "en": "Apa Itu Sistem Kontrol Terdistribusi (DCS)?", "id": "Sistem Kontrol Otomasi Proses Industri." },
  { "en": "Apa Itu Ladder Logic?", "id": "Bahasa Pemrograman Grafis Untuk PLC." },
  { "en": "Apa Itu Fieldbus?", "id": "Jaringan Digital Industri Untuk Instrumen." },
  { "en": "Apa Itu Protokol HART?", "id": "Komunikasi Hibrida Analog Dan Digital." },
  { "en": "Apa Itu Sistem SCADA?", "id": "Sistem Pengawasan, Kontrol, Akuisisi Data." },
  { "en": "Apa Itu Human-Machine Interface (HMI)?", "id": "Antarmuka Operator Untuk Mengontrol Mesin." },
  { "en": "Apa Itu Akustik?", "id": "Ilmu Tentang Suara." },
  { "en": "Apa Itu Desibel (dB)?", "id": "Satuan Logaritmik Untuk Mengukur Level Suara." },
  { "en": "Apa Itu Efek Doppler Suara?", "id": "Perubahan Frekuensi Suara Akibat Gerakan." },
  { "en": "Apa Itu Gema (Reverberation)?", "id": "Kumpulan Pantulan Suara Dalam Ruangan." },
  { "en": "Apa Itu Pembatalan Derau Aktif (ANC)?", "id": "Mengurangi Derau Dengan Menghasilkan Anti-derau." },
  { "en": "Apa Itu Sonar?", "id": "Teknik Menggunakan Suara Untuk Navigasi." },
  { "en": "Apa Itu Ultrasonik?", "id": "Suara Dengan Frekuensi Di Atas Pendengaran." },
  { "en": "Apa Itu Infrasonik?", "id": "Suara Dengan Frekuensi Di Bawah Pendengaran." },
  { "en": "Apa Itu Rekayasa Biomedis?", "id": "Aplikasi Teknik Dalam Kedokteran, Biologi." },
  { "en": "Apa Itu Sinyal EKG (Elektrokardiogram)?", "id": "Rekaman Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Sinyal EEG (Elektroensefalogram)?", "id": "Rekaman Aktivitas Listrik Otak." },
  { "en": "Apa Itu Pencitraan Medis?", "id": "Teknik Visualisasi Interior Tubuh." },
  { "en": "Apa Itu Sinar-X (X-Ray)?", "id": "Radiasi Elektromagnetik Untuk Pencitraan Tulang." },
  { "en": "Apa Itu CT Scan (Computed Tomography)?", "id": "Menggunakan Sinar-X Membuat Gambar Penampang." },
  { "en": "Apa Itu MRI (Magnetic Resonance Imaging)?", "id": "Menggunakan Medan Magnet, Gelombang Radio." },
  { "en": "Apa Itu Pencitraan Ultrasound?", "id": "Menggunakan Gelombang Suara Frekuensi Tinggi." },
  { "en": "Apa Itu Biomekanika?", "id": "Studi Mekanika Sistem Biologis." },
  { "en": "Apa Itu Rekayasa Jaringan?", "id": "Menciptakan Jaringan Biologis Fungsional." },
  { "en": "Apa Itu Biomaterial?", "id": "Material Dirancang Berinteraksi Sistem Biologis." },
  { "en": "Apa Itu Prostetik?", "id": "Perangkat Buatan Pengganti Bagian Tubuh." },
  { "en": "Apa Itu Rekayasa Keuangan?", "id": "Aplikasi Metode Matematis Dalam Keuangan." },
  { "en": "Apa Itu Model Black-Scholes?", "id": "Model Matematis Penentuan Harga Opsi." },
  { "en": "Apa Itu Rekayasa Sistem?", "id": "Pendekatan Interdisipliner Merancang Sistem Kompleks." },
  { "en": "Apa Itu Analisis Persyaratan?", "id": "Proses Mendefinisikan Kebutuhan Pemangku Kepentingan." },
  { "en": "Apa Itu Verifikasi Dan Validasi?", "id": "Memastikan Sistem Dibangun Benar, Sesuai Kebutuhan." },
  { "en": "Apa Itu Manajemen Risiko?", "id": "Mengidentifikasi, Menganalisis, Memitigasi Risiko." },
  { "en": "Apa Itu Rekayasa Keandalan?", "id": "Memastikan Sistem Berfungsi Sesuai Harapan." },
  { "en": "Apa Itu Analisis Pohon Kegagalan (FTA)?", "id": "Metode Deduktif Analisis Kegagalan." },
  { "en": "Apa Itu Failure Modes and Effects Analysis (FMEA)?", "id": "Metode Induktif Analisis Kegagalan." },
  { "en": "Apa Itu Rekayasa Faktor Manusia?", "id": "Mendesain Sistem Sesuai Kemampuan Manusia." },
  { "en": "Apa Itu Ergonomi?", "id": "Ilmu Mendesain Lingkungan Kerja Aman, Nyaman." },
  { "en": "Apa Itu Antarmuka Pengguna (User Interface)?", "id": "Bagian Sistem Berinteraksi Dengan Pengguna." },
  { "en": "Apa Itu Pengalaman Pengguna (User Experience)?", "id": "Keseluruhan Pengalaman Pengguna Dengan Produk." },
  { "en": "Apa Itu Ergonomi Kognitif?", "id": "Berfokus Pada Proses Mental Manusia." },
  { "en": "Apa Itu Analisis Tugas Hirarkis?", "id": "Memecah Tugas Menjadi Sub-tugas." },
  { "en": "Apa Itu Model GOMS?", "id": "Goals, Operators, Methods, And Selection Rules." },
  { "en": "Apa Itu Hukum Fitts?", "id": "Model Waktu Untuk Menunjuk Target." },
  { "en": "Apa Itu Hukum Hick?", "id": "Waktu Membuat Keputusan Meningkat Logaritmik." },
  { "en": "Apa Itu Beban Kerja Mental?", "id": "Jumlah Sumber Daya Mental Yang Digunakan." },
  { "en": "Apa Itu Kesadaran Situasional?", "id": "Persepsi Elemen Lingkungan." },
  { "en": "Apa Itu Kesalahan Manusia?", "id": "Penyimpangan Dari Kinerja Yang Diharapkan." },
  { "en": "Apa Itu Slip Dalam Kesalahan Manusia?", "id": "Kesalahan Eksekusi Tindakan Yang Benar." },
  { "en": "Apa Itu Mistake Dalam Kesalahan Manusia?", "id": "Kesalahan Dalam Perencanaan Tindakan." },
  { "en": "Apa Itu Manajemen Proyek?", "id": "Merencanakan, Melaksanakan, Mengendalikan Proyek." },
  { "en": "Apa Itu Lingkup Proyek (Project Scope)?", "id": "Pekerjaan Yang Perlu Dilakukan Proyek." },
  { "en": "Apa Itu Work Breakdown Structure (WBS)?", "id": "Pemecahan Hierarkis Pekerjaan Proyek." },
  { "en": "Apa Itu Jalur Kritis (Critical Path)?", "id": "Urutan Tugas Terpanjang Dalam Proyek." },
  { "en": "Apa Itu Gantt Chart?", "id": "Bagan Batang Mengilustrasikan Jadwal Proyek." },
  { "en": "Apa Itu Diagram PERT?", "id": "Program Evaluation and Review Technique." },
  { "en": "Apa Itu Manajemen Nilai Hasil (EVM)?", "id": "Metode Pengukuran Kinerja Proyek." },
  { "en": "Apa Itu Rekayasa Nilai (Value Engineering)?", "id": "Menganalisis Fungsi Untuk Meningkatkan Nilai." },
  { "en": "Apa Itu Analisis Rantai Pasokan?", "id": "Manajemen Aliran Barang Dan Jasa." },
  { "en": "Apa Itu Just-In-Time (JIT) Manufacturing?", "id": "Sistem Produksi Mengurangi Persediaan." },
  { "en": "Apa Itu Six Sigma?", "id": "Metodologi Peningkatan Kualitas Proses." },
  { "en": "Apa Itu Lean Manufacturing?", "id": "Filosofi Produksi Meminimalkan Pemborosan." },
  { "en": "Apa Itu Total Quality Management (TQM)?", "id": "Pendekatan Manajemen Peningkatan Kualitas." },
  { "en": "Apa Itu Statistical Process Control (SPC)?", "id": "Menggunakan Statistik Memantau, Mengontrol Proses." },
  { "en": "Apa Itu Peta Kontrol (Control Chart)?", "id": "Grafik Memantau Variasi Proses." },
  { "en": "Apa Itu Desain Eksperimen (DoE)?", "id": "Pendekatan Terstruktur Merencanakan Eksperimen." },
  { "en": "Apa Itu Analisis Regresi?", "id": "Menganalisis Hubungan Antara Variabel." },
  { "en": "Apa Itu Peramalan (Forecasting)?", "id": "Memprediksi Nilai Masa Depan." },
  { "en": "Apa Itu Analisis Deret Waktu?", "id": "Menganalisis Titik Data Berurutan Waktu." },
  { "en": "Apa Itu Moving Average?", "id": "Metode Peramalan Menghaluskan Data." },
  { "en": "Apa Itu Riset Operasi?", "id": "Penerapan Metode Analitis Pengambilan Keputusan." },
  { "en": "Apa Itu Teori Permainan (Game Theory)?", "id": "Model Matematis Interaksi Strategis." },
  { "en": "Apa Itu Ekuilibrium Nash?", "id": "Kondisi Stabil Dalam Teori Permainan." },
  { "en": "Apa Itu Optimisasi?", "id": "Menemukan Solusi Terbaik Dari Alternatif." },
  { "en": "Apa Itu Pemrograman Linear?", "id": "Metode Optimisasi Masalah Linear." },
  { "en": "Apa Itu Simulasi?", "id": "Meniru Perilaku Sistem Dunia Nyata." },
  { "en": "Apa Itu Simulasi Monte Carlo?", "id": "Simulasi Menggunakan Angka Acak." },
  { "en": "Apa Itu Simulasi Kejadian Diskret?", "id": "Memodelkan Sistem Sebagai Urutan Kejadian." },
  { "en": "Apa Itu Teori Antrian?", "id": "Studi Matematis Mengenai Garis Tunggu." },
  { "en": "Apa Itu Sistem M/M/1?", "id": "Model Antrian Sederhana." },
  { "en": "Apa Itu Rekayasa Industri?", "id": "Mengoptimalkan Proses, Sistem, Organisasi Kompleks." },
  { "en": "Apa Itu Studi Waktu Dan Gerak?", "id": "Menganalisis Metode Kerja Meningkatkan Efisiensi." },
  { "en": "Apa Itu Tata Letak Fasilitas?", "id": "Susunan Fisik Sumber Daya Dalam Fasilitas." },
  { "en": "Apa Itu Penanganan Material?", "id": "Pergerakan, Penyimpanan, Kontrol Material." },
  { "en": "Apa Itu Logistik?", "id": "Manajemen Aliran Barang." },
  { "en": "Apa Itu Teknik Sipil?", "id": "Merancang, Membangun, Memelihara Lingkungan." },
  { "en": "Apa Itu Analisis Struktural?", "id": "Menentukan Efek Beban Pada Struktur." },
  { "en": "Apa Itu Tegangan (Stress)?", "id": "Gaya Internal Per Satuan Luas." },
  { "en": "Apa Itu Regangan (Strain)?", "id": "Deformasi Material Akibat Tegangan." },
  { "en": "Apa Itu Modulus Young?", "id": "Ukuran Kekakuan Material Elastis." },
  { "en": "Apa Itu Rangka (Truss)?", "id": "Struktur Terdiri Dari Elemen Ramping." },
  { "en": "Apa Itu Balok (Beam)?", "id": "Elemen Struktural Menahan Beban Lentur." },
  { "en": "Apa Itu Kolom (Column)?", "id": "Elemen Struktural Menahan Beban Tekan." },
  { "en": "Apa Itu Beton Bertulang?", "id": "Beton Diperkuat Dengan Batang Baja." },
  { "en": "Apa Itu Beton Prategang?", "id": "Beton Diberi Tekanan Internal Awal." },
  { "en": "Apa Itu Rekayasa Geoteknik?", "id": "Mempelajari Perilaku Material Bumi." },
  { "en": "Apa Itu Mekanika Tanah?", "id": "Cabang Mekanika Untuk Tanah." },
  { "en": "Apa Itu Pondasi?", "id": "Bagian Struktur Mentransfer Beban Ke Tanah." },
  { "en": "Apa Itu Rekayasa Transportasi?", "id": "Perencanaan, Desain, Operasi Sistem Transportasi." },
  { "en": "Apa Itu Rekayasa Sumber Daya Air?", "id": "Manajemen Kuantitas, Kualitas Air." },
  { "en": "Apa Itu Hidrologi?", "id": "Ilmu Pergerakan, Distribusi Air." },
  { "en": "Apa Itu Hidrolika?", "id": "Ilmu Sifat Mekanis Cairan." },
  { "en": "Apa Itu Rekayasa Lingkungan?", "id": "Melindungi Manusia Dari Dampak Lingkungan." },
  { "en": "Apa Itu Pengolahan Air Limbah?", "id": "Menghilangkan Kontaminan Dari Air Limbah." },
  { "en": "Apa Itu Teknik Mesin?", "id": "Merancang, Menganalisis, Memproduksi Sistem Mekanis." },
  { "en": "Apa Itu Statika?", "id": "Studi Benda Diam." },
  { "en": "Apa Itu Dinamika?", "id": "Studi Benda Bergerak." },
  { "en": "Apa Itu Mekanika Benda Padat?", "id": "Mempelajari Perilaku Material Padat." },
  { "en": "Apa Itu Kekuatan Material?", "id": "Kemampuan Benda Menahan Beban." },
  { "en": "Apa Itu Analisis Elemen Hingga (FEA)?", "id": "Metode Numerik Analisis Struktural." },
  { "en": "Apa Itu Desain Berbantuan Komputer (CAD)?", "id": "Menggunakan Komputer Untuk Mendesain." },
  { "en": "Apa Itu Manufaktur Berbantuan Komputer (CAM)?", "id": "Menggunakan Komputer Mengontrol Mesin." },
  { "en": "Apa Itu CNC (Computer Numerical Control)?", "id": "Kontrol Mesin Perkakas Otomatis." },
  { "en": "Apa Itu Perpindahan Panas?", "id": "Studi Transfer Energi Termal." },
  { "en": "Apa Itu Konduksi?", "id": "Transfer Panas Melalui Kontak Langsung." },
  { "en": "Apa Itu Konveksi?", "id": "Transfer Panas Melalui Gerakan Fluida." },
  { "en": "Apa Itu Radiasi?", "id": "Transfer Panas Melalui Gelombang Elektromagnetik." },
  { "en": "Apa Itu Penukar Panas (Heat Exchanger)?", "id": "Perangkat Mentransfer Panas Antar Fluida." },
  { "en": "Apa Itu Mesin Pembakaran Dalam?", "id": "Mesin Panas Pembakaran Terjadi Di Dalam." },
  { "en": "Apa Itu Siklus Otto?", "id": "Siklus Termodinamika Ideal Mesin Bensin." },
  { "en": "Apa Itu Siklus Diesel?", "id": "Siklus Termodinamika Ideal Mesin Diesel." },
  { "en": "Apa Itu Turbin Gas?", "id": "Mesin Pembakaran Dalam Rotari." },
  { "en": "Apa Itu Mesin Jet?", "id": "Mesin Menghasilkan Dorongan Dari Jet." },
  { "en": "Apa Itu HVAC (Heating, Ventilation, and Air Conditioning)?", "id": "Pemanasan, Ventilasi, Dan Pendingin Udara." },
  { "en": "Apa Itu Refrigerasi?", "id": "Proses Memindahkan Panas Dari Ruang." },
  { "en": "Apa Itu Siklus Kompresi Uap?", "id": "Siklus Paling Umum Refrigerasi." },
  { "en": "Apa Itu Refrigeran?", "id": "Fluida Kerja Dalam Sistem Refrigerasi." },
  { "en": "Apa Itu Teknik Kimia?", "id": "Mengubah Bahan Mentah Menjadi Produk." },
  { "en": "Apa Itu Neraca Massa?", "id": "Penerapan Hukum Kekekalan Massa." },
  { "en": "Apa Itu Neraca Energi?", "id": "Penerapan Hukum Pertama Termodinamika." },
  { "en": "Apa Itu Kinetika Reaksi?", "id": "Studi Laju Reaksi Kimia." },
  { "en": "Apa Itu Katalis?", "id": "Zat Mempercepat Reaksi Tanpa Terkonsumsi." },
  { "en": "Apa Itu Reaktor Kimia?", "id": "Wadah Tempat Reaksi Kimia." },
  { "en": "Apa Itu Operasi Perpindahan Massa?", "id": "Pemisahan Komponen Dalam Campuran." },
  { "en": "Apa Itu Distilasi?", "id": "Pemisahan Berdasarkan Perbedaan Titik Didih." },
  { "en": "Apa Itu Absorpsi?", "id": "Pemisahan Gas Menggunakan Pelarut Cair." },
  { "en": "Apa Itu Ekstraksi?", "id": "Pemisahan Menggunakan Pelarut Tak Tercampur." },
  { "en": "Apa Itu Teknik Dirgantara?", "id": "Desain Pesawat Terbang Dan Pesawat Luar Angkasa." },
  { "en": "Apa Itu Aerodinamika?", "id": "Studi Gerakan Udara." },
  { "en": "Apa Itu Gaya Angkat (Lift)?", "id": "Gaya Tegak Lurus Aliran Udara." },
  { "en": "Apa Itu Gaya Hambat (Drag)?", "id": "Gaya Sejajar Aliran Udara." },
  { "en": "Apa Itu Bilangan Mach?", "id": "Rasio Kecepatan Terhadap Kecepatan Suara." },
  { "en": "Apa Itu Gelombang Kejut (Shock Wave)?", "id": "Gangguan Tekanan Bergerak Cepat." },
  { "en": "Apa Itu Propulsi Pesawat?", "id": "Sistem Penghasil Gaya Dorong." },
  { "en": "Apa Itu Mekanika Orbital?", "id": "Studi Gerakan Benda Di Ruang Angkasa." },
  { "en": "Apa Itu Elektronika Forensik?", "id": "Investigasi Perangkat Elektronik Untuk Bukti." },
  { "en": "Apa Itu Akuisisi Data?", "id": "Proses Mengumpulkan Data Dari Sumber Digital." },
  { "en": "Apa Itu Write Blocker?", "id": "Mencegah Penulisan Data Ke Perangkat Bukti." },
  { "en": "Apa Itu Enkripsi Disk Penuh?", "id": "Mengenkripsi Seluruh Isi Drive Penyimpanan." },
  { "en": "Apa Itu BitLocker?", "id": "Fitur Enkripsi Disk Penuh Dari Microsoft." },
  { "en": "Apa Itu FileVault?", "id": "Fitur Enkripsi Disk Penuh Dari Apple." },
  { "en": "Apa Itu Steganografi?", "id": "Menyembunyikan Data Di Dalam Data Lain." },
  { "en": "Apa Itu Analisis Slack Space?", "id": "Memeriksa Ruang Tidak Terpakai Di Disk." },
  { "en": "Apa Itu Metadata?", "id": "Data Tentang Data Lain (Waktu, Lokasi)." },
  { "en": "Apa Itu Hash File?", "id": "Nilai Unik Dihitung Dari Isi Berkas." },
  { "en": "Mengapa Hash File Penting Dalam Forensik?", "id": "Untuk Memverifikasi Integritas Data Bukti." },
  { "en": "Apa Itu Algoritma Hash MD5?", "id": "Message Digest 5, Fungsi Hash Umum." },
  { "en": "Apa Itu Algoritma Hash SHA-1?", "id": "Secure Hash Algorithm 1." },
  { "en": "Apa Itu Chain of Custody?", "id": "Dokumentasi Kronologis Penanganan Bukti." },
  { "en": "Apa Itu Teknik Anti-Forensik?", "id": "Metode Menghambat Proses Investigasi Forensik." },
  { "en": "Apa Itu Wiper Data?", "id": "Perangkat Lunak Menghapus Data Secara Aman." },
  { "en": "Apa Itu Kriptografi Kuantum?", "id": "Menggunakan Prinsip Kuantum Untuk Enkripsi." },
  { "en": "Apa Itu Distribusi Kunci Kuantum (QKD)?", "id": "Metode Aman Berbagi Kunci Kriptografi." },
  { "en": "Apa Prinsip Dasar QKD (Quantum Key Distribution)?", "id": "Pengamatan Mengubah Keadaan Kuantum." },
  { "en": "Apa Itu Protokol BB84?", "id": "Protokol Distribusi Kunci Kuantum Pertama." },
  { "en": "Apa Itu Komputasi DNA?", "id": "Menggunakan DNA Untuk Melakukan Komputasi." },
  { "en": "Apa Itu Komputasi Optik?", "id": "Menggunakan Foton Cahaya Untuk Komputasi." },
  { "en": "Apa Itu Sistem Dinamis?", "id": "Sistem Yang Berubah Seiring Waktu." },
  { "en": "Apa Itu Teori Chaos?", "id": "Studi Sistem Dinamis Sangat Sensitif." },
  { "en": "Apa Itu Efek Kupu-kupu?", "id": "Perubahan Kecil Menyebabkan Hasil Sangat Berbeda." },
  { "en": "Apa Itu Fraktal?", "id": "Pola Geometris Berulang Pada Skala Berbeda." },
  { "en": "Apa Itu Himpunan Mandelbrot?", "id": "Contoh Himpunan Fraktal Yang Terkenal." },
  { "en": "Apa Itu Teori Jaringan Kompleks?", "id": "Studi Jaringan Skala Besar Dunia Nyata." },
  { "en": "Apa Itu Jaringan Dunia Kecil?", "id": "Jaringan Jarak Rata-rata Simpulnya Pendek." },
  { "en": "Apa Itu Jaringan Bebas Skala?", "id": "Distribusi Derajatnya Mengikuti Hukum Pangkat." },
  { "en": "Apa Itu Teori Otomata Seluler?", "id": "Model Diskret Terdiri Dari Grid Sel." },
  { "en": "Apa Itu Game of Life Conway?", "id": "Contoh Terkenal Otomata Seluler." },
  { "en": "Apa Itu Teori Informasi Algoritmik?", "id": "Mendefinisikan Kompleksitas Suatu Objek." },
  { "en": "Apa Itu Kompleksitas Kolmogorov?", "id": "Panjang Program Terpendek Menghasilkan Objek." },
  { "en": "Apa Itu Teori Komputabilitas?", "id": "Mempelajari Masalah Mana Yang Dapat Dipecahkan." },
  { "en": "Apa Itu Mesin Turing?", "id": "Model Matematis Abstrak Komputasi." },
  { "en": "Apa Itu Tesis Church-Turing?", "id": "Menyatakan Komputer Dapat Mensimulasikan Mesin Turing." },
  { "en": "Apa Itu Halting Problem?", "id": "Masalah Penentuan Apakah Program Akan Berhenti." },
  { "en": "Apa Itu Teori Kompleksitas Komputasi?", "id": "Mempelajari Sumber Daya Yang Dibutuhkan Algoritma." },
  { "en": "Apa Itu Kelas Kompleksitas P?", "id": "Masalah Diselesaikan Dalam Waktu Polinomial." },
  { "en": "Apa Itu Kelas Kompleksitas NP?", "id": "Solusi Dapat Diverifikasi Waktu Polinomial." },
  { "en": "Apa Itu Masalah P Versus NP?", "id": "Pertanyaan Terbuka Terbesar Ilmu Komputer." },
  { "en": "Apa Itu Masalah NP-Complete?", "id": "Masalah Tersulit Di Kelas NP." },
  { "en": "Apa Itu Reduksi Polinomial?", "id": "Transformasi Satu Masalah Ke Masalah Lain." },
  { "en": "Apa Itu Teori Tipe?", "id": "Studi Sistem Formal Klasifikasi Entitas." },
  { "en": "Apa Itu Teori Kategori?", "id": "Studi Abstrak Struktur Matematis." },
  { "en": "Apa Itu Logika Modal?", "id": "Logika Berurusan Dengan Modalitas (Kemungkinan)." },
  { "en": "Apa Itu Logika Deontik?", "id": "Logika Berurusan Dengan Kewajiban, Izin." },
  { "en": "Apa Itu Epistemologi?", "id": "Cabang Filsafat Tentang Pengetahuan." },
  { "en": "Apa Itu Metafisika?", "id": "Cabang Filsafat Tentang Sifat Realitas." },
  { "en": "Apa Itu Etika?", "id": "Cabang Filsafat Tentang Prinsip Moral." },
  { "en": "Apa Itu Estetika?", "id": "Cabang Filsafat Tentang Keindahan, Seni." },
  { "en": "Apa Itu Psikologi Kognitif?", "id": "Studi Proses Mental Internal." },
  { "en": "Apa Itu Neurosains?", "id": "Studi Ilmiah Sistem Saraf." },
  { "en": "Apa Itu Ekonomi Perilaku?", "id": "Menerapkan Wawasan Psikologis Ke Ekonomi." },
  { "en": "Apa Itu Sosiologi?", "id": "Studi Perilaku Sosial, Masyarakat." },
  { "en": "Apa Itu Antropologi?", "id": "Studi Kemanusiaan, Budaya Masa Lalu, Kini." },
  { "en": "Apa Itu Linguistik?", "id": "Studi Ilmiah Bahasa." },
  { "en": "Apa Itu Fonetik?", "id": "Studi Suara Ucapan Manusia." },
  { "en": "Apa Itu Fonologi?", "id": "Studi Pola Suara Dalam Bahasa." },
  { "en": "Apa Itu Morfologi?", "id": "Studi Struktur Internal Kata." },
  { "en": "Apa Itu Sintaks?", "id": "Studi Struktur Kalimat." },
  { "en": "Apa Itu Semantik?", "id": "Studi Makna Dalam Bahasa." },
  { "en": "Apa Itu Pragmatik?", "id": "Studi Penggunaan Bahasa Dalam Konteks." },
  { "en": "Apa Itu Genetika?", "id": "Studi Gen, Pewarisan Sifat, Variasi." },
  { "en": "Apa Itu DNA (Deoxyribonucleic Acid)?", "id": "Molekul Pembawa Instruksi Genetika." },
  { "en": "Apa Itu Gen?", "id": "Segmen DNA Yang Mengkode Protein." },
  { "en": "Apa Itu Kromosom?", "id": "Struktur DNA Yang Terorganisir." },
  { "en": "Apa Itu Genom?", "id": "Set Lengkap Materi Genetika." },
  { "en": "Apa Itu Mutasi?", "id": "Perubahan Permanen Dalam Urutan DNA." },
  { "en": "Apa Itu Rekayasa Genetika?", "id": "Manipulasi Langsung Gen Suatu Organisme." },
  { "en": "Apa Itu CRISPR?", "id": "Teknologi Penyuntingan Gen." },
  { "en": "Apa Itu Kloning?", "id": "Menciptakan Salinan Identik Secara Genetis." },
  { "en": "Apa Itu Biologi Molekuler?", "id": "Studi Biologi Di Tingkat Molekuler." },
  { "en": "Apa Itu Biokimia?", "id": "Studi Proses Kimia Dalam Organisme." },
  { "en": "Apa Itu Protein?", "id": "Molekul Besar Melakukan Banyak Fungsi." },
  { "en": "Apa Itu Enzim?", "id": "Protein Yang Bertindak Sebagai Katalis." },
  { "en": "Apa Itu Metabolisme?", "id": "Reaksi Kimia Penopang Kehidupan." },
  { "en": "Apa Itu Fotosintesis?", "id": "Proses Mengubah Energi Cahaya Menjadi Kimia." },
  { "en": "Apa Itu Respirasi Seluler?", "id": "Proses Menghasilkan Energi Dari Makanan." },
  { "en": "Apa Itu Evolusi?", "id": "Perubahan Sifat Terwariskan Seiring Waktu." },
  { "en": "Apa Itu Seleksi Alam?", "id": "Mekanisme Utama Pendorong Evolusi." },
  { "en": "Apa Itu Ekologi?", "id": "Studi Interaksi Antar Organisme, Lingkungannya." },
  { "en": "Apa Itu Ekosistem?", "id": "Komunitas Biologis Dan Lingkungan Fisiknya." },
  { "en": "Apa Itu Rantai Makanan?", "id": "Urutan Transfer Energi Dalam Ekosistem." },
  { "en": "Apa Itu Keanekaragaman Hayati?", "id": "Variasi Kehidupan Di Bumi." },
  { "en": "Apa Itu Geologi?", "id": "Ilmu Tentang Bumi Fisik." },
  { "en": "Apa Itu Tektonika Lempeng?", "id": "Teori Pergerakan Lempeng Litosfer Bumi." },
  { "en": "Apa Itu Gempa Bumi?", "id": "Getaran Permukaan Bumi." },
  { "en": "Apa Itu Gunung Berapi?", "id": "Retakan Kerak Bumi Tempat Magma Keluar." },
  { "en": "Apa Itu Siklus Batuan?", "id": "Transisi Antara Tiga Jenis Batuan Utama." },
  { "en": "Apa Itu Meteorologi?", "id": "Ilmu Atmosfer, Peramalan Cuaca." },
  { "en": "Apa Itu Iklim?", "id": "Pola Cuaca Jangka Panjang." },
  { "en": "Apa Itu Efek Rumah Kaca?", "id": "Pemanasan Permukaan Planet." },
  { "en": "Apa Itu Pemanasan Global?", "id": "Peningkatan Suhu Rata-rata Jangka Panjang." },
  { "en": "Apa Itu Oseanografi?", "id": "Studi Ilmiah Lautan." },
  { "en": "Apa Itu Pasang Surut?", "id": "Naik Turunnya Permukaan Laut." },
  { "en": "Apa Itu Arus Laut?", "id": "Gerakan Air Laut Terarah." },
  { "en": "Apa Itu Astronomi?", "id": "Ilmu Benda Langit, Alam Semesta." },
  { "en": "Apa Itu Tata Surya?", "id": "Sistem Gravitasi Matahari, Objek Mengorbitnya." },
  { "en": "Apa Itu Planet?", "id": "Benda Astronomi Mengorbit Bintang." },
  { "en": "Apa Itu Bintang?", "id": "Bola Plasma Luminous." },
  { "en": "Apa Itu Galaksi?", "id": "Sistem Masif Bintang, Debu, Gas." },
  { "en": "Apa Nama Galaksi Kita?", "id": "Galaksi Bima Sakti." },
  { "en": "Apa Itu Astrofisika?", "id": "Cabang Astronomi Menerapkan Hukum Fisika." },
  { "en": "Apa Itu Elektrodinamika?", "id": "Studi Medan Elektromagnetik Yang Berubah Waktu." },
  { "en": "Apa Itu Potensial Retarded?", "id": "Potensial Elektromagnetik Memperhitungkan Penundaan Waktu." },
  { "en": "Apa Itu Radiasi Dipol?", "id": "Pola Radiasi Dari Dipol Listrik Berosilasi." },
  { "en": "Apa Itu Teori Antena?", "id": "Analisis Dan Desain Antena." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Distribusi Arus Tidak Merata Akibat Konduktor." },
  { "en": "Apa Itu Kabel Litz?", "id": "Kawat Mengurangi Efek Kulit, Efek Kedekatan." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Cenderung Mengalir Di Permukaan." },
  { "en": "Apa Itu Kedalaman Kulit (Skin Depth)?", "id": "Ukuran Seberapa Dalam Arus Menembus." },
  { "en": "Apa Itu Medan Elektromagnetik Quasi-Statis?", "id": "Medan Berubah Lambat, Panjang Gelombang Besar." },
  { "en": "Apa Itu Hamburan (Scattering)?", "id": "Defleksi Gelombang Akibat Interaksi Dengan Objek." },
  { "en": "Apa Itu Penampang Hamburan?", "id": "Ukuran Efektif Target Untuk Menghamburkan Gelombang." },
  { "en": "Apa Itu Kondisi Batas Periodik?", "id": "Kondisi Batas Untuk Mensimulasikan Struktur Periodik." },
  { "en": "Apa Itu Absorber Gelombang Mikro?", "id": "Material Menyerap Radiasi Gelombang Mikro." },
  { "en": "Apa Itu Sirkuit Terintegrasi Frekuensi Radio (RFIC)?", "id": "Chip IC Untuk Aplikasi Frekuensi Radio." },
  { "en": "Apa Itu Sirkuit Terintegrasi Gelombang Mikro (MMIC)?", "id": "Chip IC Untuk Aplikasi Gelombang Mikro." },
  { "en": "Bahan Substrat Apa Digunakan Untuk MMIC?", "id": "Galium Arsenida (GaAs), Galium Nitrida (GaN)." },
  { "en": "Apa Itu Kemasan Flip-Chip?", "id": "Metode Pemasangan Die Sirkuit Terbalik." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Melalui Wafer Silikon." },
  { "en": "Apa Tujuan Through-Silicon Via (TSV)?", "id": "Untuk Integrasi Tiga Dimensi (3D IC)." },
  { "en": "Apa Itu Integrasi Tiga Dimensi (3D IC)?", "id": "Menumpuk Beberapa Die Sirkuit Secara Vertikal." },
  { "en": "Apa Itu Interposer?", "id": "Substrat Antarmuka Untuk Menghubungkan Beberapa Die." },
  { "en": "Apa Itu Heterogeneous Integration?", "id": "Mengintegrasikan Komponen Berbeda Satu Paket." },
  { "en": "Apa Itu Fotonika?", "id": "Ilmu Pembangkitan, Deteksi, Manipulasi Foton." },
  { "en": "Apa Itu Fotonik Silikon?", "id": "Komponen Fotonik Dibuat Di Atas Silikon." },
  { "en": "Apa Itu Modulator Optik?", "id": "Perangkat Memodulasi Sifat Sinar Cahaya." },
  { "en": "Apa Itu Modulator Elektro-Optik?", "id": "Menggunakan Medan Listrik Memodulasi Cahaya." },
  { "en": "Apa Itu Photodetector?", "id": "Sensor Mengubah Cahaya Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Fotodioda Avalanche (APD)?", "id": "Fotodioda Dengan Penguatan Internal." },
  { "en": "Apa Itu Single-Photon Avalanche Diode (SPAD)?", "id": "APD Cukup Sensitif Mendeteksi Foton Tunggal." },
  { "en": "Apa Itu Interkoneksi Optik?", "id": "Menggunakan Cahaya Untuk Komunikasi Antar Chip." },
  { "en": "Apa Itu Komputasi Optik?", "id": "Menggunakan Cahaya Untuk Melakukan Komputasi." },
  { "en": "Apa Itu Jaringan Optik?", "id": "Jaringan Komunikasi Menggunakan Serat Optik." },
  { "en": "Apa Itu Multiplexing Pembagian Panjang Gelombang (WDM)?", "id": "Mengirim Beberapa Sinyal Cahaya Berbeda." },
  { "en": "Apa Itu Sakelar Optik?", "id": "Mengalihkan Sinyal Cahaya Antar Jalur Berbeda." },
  { "en": "Apa Itu Sistem MEMS (Micro-Electro-Mechanical Systems) Optik?", "id": "MEMS Untuk Aplikasi Optik (Cermin Mikro)." },
  { "en": "Apa Itu Optofluida?", "id": "Integrasi Optik Dan Mikrofluida." },
  { "en": "Apa Itu Optomekanik?", "id": "Studi Interaksi Antara Cahaya, Gerak Mekanis." },
  { "en": "Apa Itu Penjepit Optik (Optical Tweezer)?", "id": "Menggunakan Laser Memanipulasi Objek Mikroskopis." },
  { "en": "Apa Itu Mikroskop Kekuatan Atom (AFM)?", "id": "Mikroskop Resolusi Sangat Tinggi." },
  { "en": "Apa Itu Mikroskop Terowongan Payaran (STM)?", "id": "Mikroskop Pencitraan Permukaan Tingkat Atom." },
  { "en": "Apa Itu Fabrikasi Nano?", "id": "Membuat Struktur Pada Skala Nanometer." },
  { "en": "Apa Itu Litografi Berkas Elektron (E-beam)?", "id": "Menggunakan Berkas Elektron Menggambar Pola." },
  { "en": "Apa Itu Litografi Nanoimprint?", "id": "Mencetak Pola Nano Menggunakan Cetakan." },
  { "en": "Apa Itu Perakitan Diri (Self-Assembly)?", "id": "Komponen Spontan Mengorganisir Diri." },
  { "en": "Apa Itu Elektronika Molekuler?", "id": "Menggunakan Molekul Tunggal Sebagai Komponen." },
  { "en": "Apa Itu Komputasi DNA?", "id": "Menggunakan DNA Untuk Menyimpan, Memproses Informasi." },
  { "en": "Apa Itu Teori Kendali Optimal?", "id": "Menemukan Sinyal Kontrol Meminimalkan Fungsi Biaya." },
  { "en": "Apa Itu Persamaan Hamilton-Jacobi-Bellman (HJB)?", "id": "Persamaan Diferensial Parsial Dalam Kendali Optimal." },
  { "en": "Apa Itu Linear-Quadratic Regulator (LQR)?", "id": "Jenis Masalah Kendali Optimal Umum." },
  { "en": "Apa Itu Kontrol Prediktif Model (MPC)?", "id": "Strategi Kontrol Menggunakan Model Proses." },
  { "en": "Apa Itu Horison Prediksi?", "id": "Interval Waktu Masa Depan Dipertimbangkan MPC." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Menyesuaikan Parameternya Secara Online." },
  { "en": "Apa Itu Identifikasi Sistem?", "id": "Membangun Model Matematis Dari Data." },
  { "en": "Apa Itu Kontrol Kuat (Robust Control)?", "id": "Desain Kontroler Bekerja Dengan Ketidakpastian." },
  { "en": "Apa Itu H-infinity Control?", "id": "Metode Desain Kontrol Kuat." },
  { "en": "Apa Itu Kontrol Cerdas (Intelligent Control)?", "id": "Menggunakan Teknik AI Dalam Sistem Kontrol." },
  { "en": "Apa Itu Kontrol Jaringan Saraf?", "id": "Menggunakan Jaringan Saraf Sebagai Kontroler." },
  { "en": "Apa Itu Kontrol Logika Fuzzy?", "id": "Sistem Kontrol Berbasis Aturan Fuzzy." },
  { "en": "Apa Itu Sistem Hibrida?", "id": "Sistem Menggabungkan Dinamika Kontinu, Diskret." },
  { "en": "Apa Itu Sistem Kejadian Diskret?", "id": "Sistem Dinamis Berbasis Kejadian." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel (WSN)?", "id": "Jaringan Spasial Terdistribusi Sensor Otonom." },
  { "en": "Apa Itu Simpul Sensor (Sensor Node)?", "id": "Komponen Dasar Dalam WSN." },
  { "en": "Apa Tantangan Utama Desain WSN?", "id": "Efisiensi Energi, Skalabilitas, Keandalan." },
  { "en": "Apa Itu Protokol Routing WSN?", "id": "Contoh: LEACH, AODV, DSR." },
  { "en": "Apa Itu Sinkronisasi Waktu Dalam WSN?", "id": "Menjaga Jam Simpul Tetap Sinkron." },
  { "en": "Apa Itu Agregasi Data?", "id": "Menggabungkan Data Dari Simpul Berbeda." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Objek Fisik Dengan Konektivitas." },
  { "en": "Apa Itu Protokol MQTT?", "id": "Protokol Pesan Ringan Untuk IoT." },
  { "en": "Apa Itu Protokol CoAP?", "id": "Protokol Aplikasi Terkendala Untuk Perangkat IoT." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Standar Nirkabel Hemat Daya." },
  { "en": "Apa Itu Zigbee?", "id": "Standar Nirkabel Untuk Jaringan Mesh." },
  { "en": "Apa Itu LoRaWAN?", "id": "Protokol Jaringan Area Luas Daya Rendah." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Memproses Data Dekat Sumbernya." },
  { "en": "Apa Itu Komputasi Kabut (Fog Computing)?", "id": "Lapisan Antara Tepi Dan Awan." },
  { "en": "Apa Itu Analisis Data?", "id": "Memeriksa Data Untuk Menarik Kesimpulan." },
  { "en": "Apa Itu Ilmu Data?", "id": "Bidang Interdisipliner Mengekstrak Pengetahuan Data." },
  { "en": "Apa Itu Rekayasa Data?", "id": "Membangun Sistem Mengumpulkan, Mengelola Data." },
  { "en": "Apa Itu Pipeline Data?", "id": "Serangkaian Langkah Pemrosesan Data." },
  { "en": "Apa Itu Gudang Data (Data Warehouse)?", "id": "Penyimpanan Data Terpusat Untuk Analisis." },
  { "en": "Apa Itu Proses ETL?", "id": "Extract, Transform, Load." },
  { "en": "Apa Itu Penambangan Data (Data Mining)?", "id": "Menemukan Pola Tersembunyi Dalam Data." },
  { "en": "Apa Itu Analisis Prediktif?", "id": "Membuat Prediksi Tentang Peristiwa Masa Depan." },
  { "en": "Apa Itu Analisis Preskriptif?", "id": "Menyarankan Tindakan Untuk Mengoptimalkan Hasil." },
  { "en": "Apa Itu Visualisasi Data?", "id": "Representasi Grafis Data Dan Informasi." },
  { "en": "Apa Itu Dasbor (Dashboard)?", "id": "Tampilan Visual Informasi Kunci." },
  { "en": "Apa Itu Kecerdasan Bisnis (BI)?", "id": "Menggunakan Data Untuk Pengambilan Keputusan Bisnis." },
  { "en": "Apa Itu Analitik Big Data?", "id": "Menganalisis Kumpulan Data Sangat Besar." },
  { "en": "Apa Itu Pemrosesan Aliran (Stream Processing)?", "id": "Menganalisis Data Secara Real-Time." },
  { "en": "Apa Itu Apache Kafka?", "id": "Platform Populer Untuk Pemrosesan Aliran." },
  { "en": "Apa Itu Apache Flink?", "id": "Kerangka Kerja Lain Untuk Pemrosesan Aliran." },
  { "en": "Apa Itu Lambda Architecture?", "id": "Arsitektur Data Menggabungkan Pemrosesan Batch, Aliran." },
  { "en": "Apa Itu Kappa Architecture?", "id": "Arsitektur Data Hanya Menggunakan Pemrosesan Aliran." },
  { "en": "Apa Itu Kualitas Data?", "id": "Ukuran Kondisi Data (Akurasi, Kelengkapan)." },
  { "en": "Apa Itu Tata Kelola Data?", "id": "Manajemen Ketersediaan, Penggunaan, Integritas Data." },
  { "en": "Apa Itu Metadata?", "id": "Data Yang Mendeskripsikan Data Lain." },
  { "en": "Apa Itu Katalog Data?", "id": "Inventaris Aset Data Dalam Organisasi." },
  { "en": "Apa Itu Garis Keturunan Data (Data Lineage)?", "id": "Melacak Asal Usul, Pergerakan Data." },
  { "en": "Apa Itu Privasi Data?", "id": "Perlindungan Informasi Pribadi." },
  { "en": "Apa Itu GDPR (General Data Protection Regulation)?", "id": "Regulasi Privasi Data Uni Eropa." },
  { "en": "Apa Itu Anonimisasi Data?", "id": "Menghapus Informasi Identifikasi Pribadi." },
  { "en": "Apa Itu Pseudonimisasi?", "id": "Mengganti Pengenal Dengan Nama Samaran." },
  { "en": "Apa Itu Model Kanal AWGN?", "id": "Kanal Dengan Derau Putih Gaussian Aditif." },
  { "en": "Apa Itu Batas Serikat (Union Bound)?", "id": "Batas Atas Probabilitas Gabungan Peristiwa." },
  { "en": "Apa Itu Probabilitas Error Simbol?", "id": "Probabilitas Simbol Diterima Secara Salah." },
  { "en": "Apa Itu Jarak Euclidean Minimum?", "id": "Jarak Terkecil Antara Titik Konstelasi." },
  { "en": "Apa Itu Pengkodean Gray Dalam QAM (Quadrature Amplitude Modulation)?", "id": "Mengurangi Error Bit Per Simbol Error." },
  { "en": "Apa Itu Deteksi Koheren?", "id": "Membutuhkan Informasi Fasa Pembawa (Carrier)." },
  { "en": "Apa Itu Deteksi Non-Koheren?", "id": "Tidak Membutuhkan Informasi Fasa Pembawa." },
  { "en": "Apa Itu Detektor Selubung (Envelope Detector)?", "id": "Digunakan Untuk Demodulasi AM (Amplitude Modulation)." },
  { "en": "Apa Itu Pemulihan Pembawa (Carrier Recovery)?", "id": "Mengekstrak Frekuensi, Fasa Pembawa Di Penerima." },
  { "en": "Apa Itu Loop Costas?", "id": "Rangkaian Pemulihan Pembawa Untuk Sinyal BPSK." },
  { "en": "Apa Itu Sinkronisasi Simbol (Waktu)?", "id": "Menyelaraskan Waktu Penerima Dengan Simbol Diterima." },
  { "en": "Apa Itu Algoritma Gardner?", "id": "Algoritma Sinkronisasi Waktu Untuk Sinyal Digital." },
  { "en": "Apa Itu Pemodelan Kanal Fading?", "id": "Model Statistik Untuk Kanal Nirkabel." },
  { "en": "Apa Itu Waktu Koherensi?", "id": "Durasi Waktu Kanal Dianggap Konstan." },
  { "en": "Apa Itu Bandwidth Koherensi?", "id": "Rentang Frekuensi Kanal Dianggap Datar." },
  { "en": "Apa Itu Fading Datar?", "id": "Semua Komponen Frekuensi Sinyal Mengalami Fading." },
  { "en": "Apa Itu Fading Selektif Frekuensi?", "id": "Komponen Frekuensi Berbeda Mengalami Fading Berbeda." },
  { "en": "Apa Itu Keanekaragaman (Diversity)?", "id": "Teknik Melawan Fading Dengan Jalur Ganda." },
  { "en": "Apa Itu Keanekaragaman Pemilihan (Selection Diversity)?", "id": "Memilih Sinyal Terbaik Dari Beberapa Antena." },
  { "en": "Apa Itu Penggabungan Rasio Maksimal (MRC)?", "id": "Menggabungkan Sinyal Tertimbang Berdasarkan SNR." },
  { "en": "Apa Itu Keanekaragaman Pengalihan (Switching Diversity)?", "id": "Beralih Antar Antena Saat Sinyal Melemah." },
  { "en": "Apa Itu Penguatan Keanekaragaman (Diversity Gain)?", "id": "Peningkatan Kinerja Akibat Penggunaan Keanekaragaman." },
  { "en": "Apa Itu Penguatan Pengkodean (Coding Gain)?", "id": "Peningkatan Kinerja Akibat Pengkodean Kanal." },
  { "en": "Apa Itu Sistem Kontrol SISO (Single-Input Single-Output)?", "id": "Sistem Dengan Satu Input, Satu Output." },
  { "en": "Apa Itu Sistem Kontrol MIMO (Multiple-Input Multiple-Output)?", "id": "Sistem Dengan Banyak Input, Banyak Output." },
  { "en": "Apa Itu Matriks Fungsi Transfer?", "id": "Representasi Sistem MIMO (Multiple-Input Multiple-Output)." },
  { "en": "Apa Itu Dekopling (Decoupling) Sistem MIMO?", "id": "Mendesain Kontroler Agar Input Mempengaruhi Output." },
  { "en": "Apa Itu Metode Ackermann?", "id": "Metode Penempatan Kutub (Pole Placement)." },
  { "en": "Apa Itu Persamaan Lyapunov?", "id": "Digunakan Untuk Menganalisis Stabilitas Sistem." },
  { "en": "Apa Itu Controllability Grammian?", "id": "Matriks Terkait Sifat Keterkontrolan Sistem." },
  { "en": "Apa Itu Observability Grammian?", "id": "Matriks Terkait Sifat Keteramatan Sistem." },
  { "en": "Apa Itu Realisasi Minimal Sistem?", "id": "Representasi Ruang Keadaan Orde Terendah." },
  { "en": "Apa Itu Persamaan Riccati?", "id": "Persamaan Diferensial Dalam Kendali Optimal." },
  { "en": "Apa Itu Horison Waktu Tak Terhingga?", "id": "Masalah Kendali Optimal Waktu Tak Terbatas." },
  { "en": "Apa Itu Regulator Kuadratik Linear (LQR)?", "id": "Jenis Pengontrol Optimal." },
  { "en": "Apa Itu Estimator Kalman-Bucy?", "id": "Versi Waktu Kontinu Dari Filter Kalman." },
  { "en": "Apa Itu Prinsip Pemisahan (Separation Principle)?", "id": "Desain Estimator, Kontroler Dapat Dilakukan Terpisah." },
  { "en": "Apa Itu Filter Notch?", "id": "Filter Menolak Pita Frekuensi Sangat Sempit." },
  { "en": "Apa Itu Equalizer Grafis?", "id": "Filter Dengan Kontrol Penguatan Pita Frekuensi." },
  { "en": "Apa Itu Equalizer Parametrik?", "id": "Kontrol Frekuensi Pusat, Bandwidth, Penguatan." },
  { "en": "Apa Itu Jaringan Crossover Pasif?", "id": "Jaringan RLC (Resistor-Induktor-Kapasitor) Membagi Frekuensi." },
  { "en": "Apa Itu Jaringan Crossover Aktif?", "id": "Menggunakan Filter Aktif Sebelum Penguat Daya." },
  { "en": "Apa Orde Jaringan Crossover?", "id": "Menentukan Kemiringan Roll-off (Contoh: 12 dB/Oktaf)." },
  { "en": "Apa Itu Jaringan Zobel?", "id": "Menstabilkan Impedansi Driver Pengeras Suara." },
  { "en": "Apa Itu Baffle Step Compensation?", "id": "Koreksi Respon Frekuensi Speaker Di Kotak." },
  { "en": "Apa Itu Port Bass Reflex?", "id": "Lubang Di Kotak Speaker Meningkatkan Respon Bass." },
  { "en": "Apa Itu Kotak Speaker Tertutup (Sealed)?", "id": "Desain Kotak Speaker Kedap Udara." },
  { "en": "Apa Itu Parameter Thiele/Small (T/S)?", "id": "Parameter Elektromekanis Driver Pengeras Suara." },
  { "en": "Apa Itu Faktor Redaman (Damping Factor) Amplifier?", "id": "Kemampuan Amplifier Mengontrol Gerakan Speaker." },
  { "en": "Apa Itu Headroom Amplifier?", "id": "Kemampuan Menangani Puncak Sinyal Tanpa Distorsi." },
  { "en": "Apa Itu Slew Rate?", "id": "Kecepatan Perubahan Maksimum Tegangan Output." },
  { "en": "Apa Itu Rangkaian Perlindungan DC Speaker?", "id": "Memutuskan Speaker Jika Amplifier Gagal." },
  { "en": "Apa Itu Ground Loop Dalam Audio?", "id": "Menimbulkan Derau Dengung (Hum) 50/60 Hz." },
  { "en": "Bagaimana Mengatasi Ground Loop Audio?", "id": "Menggunakan Isolator Ground Atau Kabel Seimbang." },
  { "en": "Apa Itu Sinyal Audio Seimbang (Balanced)?", "id": "Menggunakan Tiga Kabel (Positif, Negatif, Ground)." },
  { "en": "Apa Keuntungan Sinyal Audio Seimbang?", "id": "Sangat Tahan Terhadap Derau Interferensi." },
  { "en": "Apa Itu Konektor XLR?", "id": "Konektor Tiga Pin Umum Audio Profesional." },
  { "en": "Apa Itu Konektor TRS (Tip-Ring-Sleeve)?", "id": "Konektor Serbaguna Untuk Audio Seimbang/Stereo." },
  { "en": "Apa Itu Phantom Power?", "id": "Daya DC (Direct Current) Dikirim Melalui Kabel Mikrofon." },
  { "en": "Untuk Apa Phantom Power Digunakan?", "id": "Memberi Daya Pada Mikrofon Kondensor." },
  { "en": "Apa Itu Mikrofon Dinamis?", "id": "Menggunakan Prinsip Induksi Elektromagnetik." },
  { "en": "Apa Itu Mikrofon Kondensor?", "id": "Bekerja Seperti Kapasitor Variabel." },
  { "en": "Apa Itu Pola Kutub (Polar Pattern) Mikrofon?", "id": "Sensitivitas Mikrofon Terhadap Suara Dari Arah." },
  { "en": "Apa Itu Pola Kardioid?", "id": "Sensitif Di Depan, Kurang Sensitif Di Belakang." },
  { "en": "Apa Itu Pola Omnidirectional?", "id": "Sensitif Sama Dari Segala Arah." },
  { "en": "Apa Itu Pola Figure-8 (Dua Arah)?", "id": "Sensitif Di Depan Dan Belakang." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect) Mikrofon?", "id": "Peningkatan Respon Bass Saat Dekat Sumber." },
  { "en": "Apa Itu Direct Injection (DI) Box?", "id": "Mengubah Sinyal Instrumen Menjadi Sinyal Seimbang." },
  { "en": "Apa Itu Elektronika Tabung Vakum?", "id": "Menggunakan Tabung Hampa Udara Untuk Amplifikasi." },
  { "en": "Apa Itu Trioda?", "id": "Tabung Vakum Tiga Elemen (Katoda, Grid, Anoda)." },
  { "en": "Apa Fungsi Grid Kontrol?", "id": "Mengontrol Aliran Elektron Ke Anoda." },
  { "en": "Apa Itu Pentoda?", "id": "Tabung Vakum Lima Elemen." },
  { "en": "Apa Itu Distorsi Harmonik?", "id": "Penambahan Frekuensi Kelipatan Sinyal Asli." },
  { "en": "Apa Itu Distorsi Harmonik Genap?", "id": "Kelipatan Genap (2x, 4x) Dari Frekuensi Asli." },
  { "en": "Apa Itu Distorsi Harmonik Ganjil?", "id": "Kelipatan Ganjil (3x, 5x) Dari Frekuensi Asli." },
  { "en": "Distorsi Mana Dianggap Lebih Musikal?", "id": "Distorsi Harmonik Genap." },
  { "en": "Apa Itu Amplifier Push-Pull?", "id": "Menggunakan Dua Perangkat Menguatkan Sinyal Berlawanan." },
  { "en": "Apa Keuntungan Amplifier Push-Pull?", "id": "Membatalkan Distorsi Harmonik Genap." },
  { "en": "Apa Itu Trafo Output?", "id": "Menyesuaikan Impedansi Tinggi Tabung Ke Speaker." },
  { "en": "Apa Itu Rekayasa Akustik?", "id": "Ilmu Kontrol Suara Dalam Ruangan." },
  { "en": "Apa Itu Waktu Gema (Reverberation Time)?", "id": "Waktu Suara Meluruh Dalam Ruangan." },
  { "en": "Apa Itu Penyerapan Suara?", "id": "Proses Energi Suara Diubah Menjadi Panas." },
  { "en": "Apa Itu Difusi Suara?", "id": "Penyebaran Energi Suara Secara Merata." },
  { "en": "Apa Itu Peredaman Suara?", "id": "Mencegah Transmisi Suara Antar Ruang." },
  { "en": "Apa Itu Resonator Helmholtz?", "id": "Perangkat Menyerap Suara Frekuensi Rendah." },
  { "en": "Apa Itu Sound Transmission Class (STC)?", "id": "Rating Kemampuan Dinding Meredam Suara." },
  { "en": "Apa Itu Noise Reduction Coefficient (NRC)?", "id": "Ukuran Penyerapan Suara Suatu Material." },
  { "en": "Apa Itu Sistem Informasi Geografis (GIS)?", "id": "Sistem Menangkap, Menyimpan, Menganalisis Data Geospasial." },
  { "en": "Apa Itu Data Raster?", "id": "Data Geospasial Berbasis Grid Piksel." },
  { "en": "Apa Itu Data Vektor?", "id": "Data Geospasial Berbasis Titik, Garis, Poligon." },
  { "en": "Apa Itu Remote Sensing?", "id": "Akuisisi Informasi Tanpa Kontak Fisik." },
  { "en": "Apa Itu Resolusi Spektral?", "id": "Kemampuan Sensor Membedakan Panjang Gelombang." },
  { "en": "Apa Itu Resolusi Temporal?", "id": "Frekuensi Sensor Mengambil Gambar Area Sama." },
  { "en": "Apa Itu Fotogrametri?", "id": "Ilmu Pengukuran Dari Foto." },
  { "en": "Apa Itu LiDAR (Light Detection and Ranging)?", "id": "Mengukur Jarak Menggunakan Sinar Laser." },
  { "en": "Apa Itu Awan Titik (Point Cloud)?", "id": "Kumpulan Titik Data Dalam Ruang 3D." },
  { "en": "Apa Itu Global Navigation Satellite System (GNSS)?", "id": "Sistem Satelit Untuk Penentuan Posisi." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem GNSS (Global Navigation Satellite System) Milik Amerika." },
  { "en": "Apa Itu Trilaterasi?", "id": "Metode Penentuan Posisi Berdasarkan Jarak." },
  { "en": "Apa Itu Diferensial GPS (DGPS)?", "id": "Meningkatkan Akurasi Posisi GPS." },
  { "en": "Apa Itu Real-Time Kinematic (RTK)?", "id": "Teknik Penentuan Posisi Sangat Akurat." },
  { "en": "Apa Itu Datum Geodetik?", "id": "Sistem Referensi Untuk Mengukur Lokasi." },
  { "en": "Apa Itu Proyeksi Peta?", "id": "Merepresentasikan Permukaan Melengkung Bumi Datar." },
  { "en": "Apa Itu Universal Transverse Mercator (UTM)?", "id": "Sistem Koordinat Berbasis Proyeksi Peta." },
  { "en": "Apa Itu Sistem Informasi?", "id": "Sistem Mengumpulkan, Menyimpan, Memproses Informasi." },
  { "en": "Apa Itu Teori Informasi?", "id": "Studi Kuantifikasi Informasi." },
  { "en": "Apa Itu Bit?", "id": "Unit Dasar Informasi." },
  { "en": "Apa Itu Entropi (Teori Informasi)?", "id": "Ukuran Rata-rata Informasi Atau Ketidakpastian." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data." },
  { "en": "Apa Itu Teori Pengkodean?", "id": "Mempelajari Sifat Kode Dan Aplikasinya." },
  { "en": "Apa Itu Kode Koreksi Error?", "id": "Menambahkan Redundansi Untuk Memperbaiki Error." },
  { "en": "Apa Itu Jarak Hamming?", "id": "Jumlah Posisi Bit Yang Berbeda." },
  { "en": "Apa Itu Teori Graf?", "id": "Studi Grafik, Struktur Matematis." },
  { "en": "Apa Itu Teori Antrian?", "id": "Studi Matematis Garis Tunggu." },
  { "en": "Apa Itu Proses Poisson?", "id": "Model Kedatangan Acak Dalam Teori Antrian." },
  { "en": "Apa Itu Rantai Markov?", "id": "Model Stokastik Dengan Sifat Tanpa Memori." },
  { "en": "Apa Itu Teori Keputusan?", "id": "Studi Pengambilan Keputusan Rasional." },
  { "en": "Apa Itu Pohon Keputusan?", "id": "Alat Bantu Pengambilan Keputusan." },
  { "en": "Apa Itu Teori Permainan?", "id": "Studi Interaksi Strategis." },
  { "en": "Apa Itu Ekuilibrium Nash?", "id": "Hasil Stabil Dalam Permainan Non-kooperatif." },
  { "en": "Apa Itu Dilema Tahanan?", "id": "Contoh Paradoks Dalam Teori Permainan." },
  { "en": "Apa Itu Keuangan Komputasi?", "id": "Menggunakan Ilmu Komputer Dalam Keuangan." },
  { "en": "Apa Itu Perdagangan Frekuensi Tinggi?", "id": "Perdagangan Algoritmik Dengan Kecepatan Sangat Tinggi." },
  { "en": "Apa Itu Bioinformatika?", "id": "Menggunakan Komputasi Untuk Menganalisis Data Biologis." },
  { "en": "Apa Itu Perunutan Genom?", "id": "Menentukan Urutan Lengkap DNA Suatu Organisme." },
  { "en": "Apa Itu Analisis Filogenetik?", "id": "Mempelajari Hubungan Evolusioner." },
  { "en": "Apa Itu Pemodelan Molekuler?", "id": "Mensimulasikan Perilaku Molekul." },
  { "en": "Apa Itu Kimia Komputasi?", "id": "Menggunakan Simulasi Komputer Memecahkan Masalah Kimia." },
  { "en": "Apa Itu Teori Fungsional Kepadatan (DFT)?", "id": "Metode Populer Dalam Kimia Komputasi." },
  { "en": "Apa Itu Dinamika Molekuler?", "id": "Simulasi Pergerakan Atom Dan Molekul." },
  { "en": "Apa Itu Metode Monte Carlo?", "id": "Metode Komputasi Menggunakan Sampling Acak." },
  { "en": "Apa Itu Fisika Komputasi?", "id": "Penerapan Komputasi Numerik Dalam Fisika." },
  { "en": "Apa Itu Dinamika Fluida Komputasi (CFD)?", "id": "Mensimulasikan Aliran Fluida Menggunakan Komputer." },
  { "en": "Apa Itu Persamaan Navier-Stokes?", "id": "Persamaan Yang Mengatur Gerakan Fluida." },
  { "en": "Apa Itu Kondisi Batas?", "id": "Spesifikasi Kondisi Di Tepi Domain." },
  { "en": "Apa Itu Jala (Mesh) Komputasi?", "id": "Diskritisasi Domain Geometris Menjadi Sel." },
  { "en": "Apa Itu Teknik Nuklir?", "id": "Aplikasi Reaksi Nuklir." },
  { "en": "Apa Itu Fisi Nuklir?", "id": "Pembelahan Inti Atom Menjadi Bagian Kecil." },
  { "en": "Apa Itu Fusi Nuklir?", "id": "Penggabungan Dua Inti Atom Ringan." },
  { "en": "Apa Itu Reaktor Nuklir?", "id": "Perangkat Menginisiasi, Mengontrol Reaksi Fisi." },
  { "en": "Apa Itu Bahan Bakar Nuklir?", "id": "Material Digunakan Dalam Reaksi Nuklir." },
  { "en": "Apa Itu Moderator Dalam Reaktor?", "id": "Memperlambat Neutron Untuk Mendukung Fisi." },
  { "en": "Apa Itu Batang Kendali (Control Rod)?", "id": "Menyerap Neutron Untuk Mengontrol Laju Reaksi." },
  { "en": "Apa Itu Pendingin Reaktor?", "id": "Mentransfer Panas Dari Inti Reaktor." },
  { "en": "Apa Itu Peluruhan Radioaktif?", "id": "Proses Inti Atom Tidak Stabil." },
  { "en": "Apa Itu Waktu Paruh (Half-Life)?", "id": "Waktu Separuh Zat Radioaktif Meluruh." },
  { "en": "Apa Itu Radiasi Alfa?", "id": "Partikel Terdiri Dari Dua Proton, Neutron." },
  { "en": "Apa Itu Radiasi Beta?", "id": "Elektron Atau Positron Energi Tinggi." },
  { "en": "Apa Itu Radiasi Gamma?", "id": "Radiasi Elektromagnetik Energi Sangat Tinggi." },
  { "en": "Apa Itu Dosimetri?", "id": "Pengukuran Dosis Radiasi Pengion." },
  { "en": "Apa Itu Limbah Nuklir?", "id": "Material Radioaktif Sisa Dari Reaksi Nuklir." },
  { "en": "Apa Itu Teknik Perminyakan?", "id": "Berurusan Dengan Produksi Hidrokarbon." },
  { "en": "Apa Itu Reservoir Perminyakan?", "id": "Formasi Batuan Bawah Permukaan." },
  { "en": "Apa Itu Pengeboran (Drilling)?", "id": "Proses Membuat Lubang Ke Dalam Bumi." },
  { "en": "Apa Itu Penyelesaian Sumur?", "id": "Mempersiapkan Sumur Untuk Produksi." },
  { "en": "Apa Itu Enhanced Oil Recovery (EOR)?", "id": "Teknik Meningkatkan Produksi Minyak." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak?", "id": "Disiplin Rekayasa Untuk Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Paradigma Pemrograman?", "id": "Gaya Fundamental Pemrograman Komputer." },
  { "en": "Apa Itu Pemrograman Prosedural?", "id": "Berdasarkan Konsep Panggilan Prosedur." },
  { "en": "Apa Itu Pemrograman Berorientasi Objek?", "id": "Berdasarkan Konsep Objek." },
  { "en": "Apa Itu Pemrograman Fungsional?", "id": "Memperlakukan Komputasi Sebagai Evaluasi Fungsi." },
  { "en": "Apa Itu Tipe Data?", "id": "Klasifikasi Jenis Data." },
  { "en": "Apa Itu Variabel?", "id": "Lokasi Penyimpanan Dengan Nama Simbolis." },
  { "en": "Apa Itu Konstanta?", "id": "Nilai Yang Tidak Dapat Diubah." },
  { "en": "Apa Itu Operator?", "id": "Simbol Yang Melakukan Operasi." },
  { "en": "Apa Itu Ekspresi?", "id": "Kombinasi Nilai, Variabel, Operator." },
  { "en": "Apa Itu Pernyataan Kontrol?", "id": "Mengubah Alur Eksekusi Program." },
  { "en": "Apa Itu Pernyataan If-Else?", "id": "Eksekusi Bersyarat." },
  { "en": "Apa Itu Loop For?", "id": "Mengulangi Blok Kode." },
  { "en": "Apa Itu Loop While?", "id": "Mengulangi Selama Kondisi Benar." },
  { "en": "Apa Itu Fungsi (Subrutin)?", "id": "Urutan Kode Yang Dapat Digunakan Kembali." },
  { "en": "Apa Itu Parameter Fungsi?", "id": "Variabel Input Untuk Suatu Fungsi." },
  { "en": "Apa Itu Nilai Kembali (Return Value)?", "id": "Output Dari Suatu Fungsi." },
  { "en": "Apa Itu Ruang Lingkup (Scope) Variabel?", "id": "Konteks Di Mana Variabel Didefinisikan." },
  { "en": "Apa Itu Rekursi?", "id": "Fungsi Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Struktur Data?", "id": "Cara Mengorganisir Data." },
  { "en": "Apa Itu Array (Larik)?", "id": "Koleksi Elemen Berindeks." },
  { "en": "Apa Itu Linked List?", "id": "Koleksi Elemen Terhubung Pointer." },
  { "en": "Apa Itu Stack (Tumpukan)?", "id": "Struktur Data LIFO (Last-In, First-Out)." },
  { "en": "Apa Itu Queue (Antrian)?", "id": "Struktur Data FIFO (First-In, First-Out)." },
  { "en": "Apa Itu Pohon (Tree)?", "id": "Struktur Data Hierarkis." },
  { "en": "Apa Itu Grafik (Graph)?", "id": "Struktur Data Simpul Dan Sisi." },
  { "en": "Apa Itu Hash Table?", "id": "Struktur Data Pemetaan Kunci Ke Nilai." },
  { "en": "Apa Itu Algoritma?", "id": "Urutan Langkah Untuk Menyelesaikan Masalah." },
  { "en": "Apa Itu Analisis Algoritma?", "id": "Menentukan Sumber Daya Yang Dibutuhkan." },
  { "en": "Apa Itu Kompleksitas Waktu?", "id": "Waktu Eksekusi Sebagai Fungsi Ukuran Input." },
  { "en": "Apa Itu Kompleksitas Ruang?", "id": "Memori Yang Digunakan Sebagai Fungsi Ukuran." },
  { "en": "Apa Itu Notasi Big-O?", "id": "Mendeskripsikan Kasus Terburuk Kinerja." },
  { "en": "Apa Itu Algoritma Pengurutan (Sorting)?", "id": "Menyusun Elemen Dalam Urutan Tertentu." },
  { "en": "Apa Itu Algoritma Pencarian (Searching)?", "id": "Menemukan Elemen Dalam Kumpulan Data." },
  { "en": "Apa Itu Kompilasi?", "id": "Menerjemahkan Kode Sumber Ke Kode Mesin." },
  { "en": "Apa Itu Interpretasi?", "id": "Mengeksekusi Kode Sumber Baris Demi Baris." },
  { "en": "Apa Itu Debugging?", "id": "Proses Menemukan Dan Memperbaiki Bug." },
  { "en": "Apa Itu Sistem Operasi?", "id": "Perangkat Lunak Pengelola Perangkat Keras." },
  { "en": "Apa Itu Proses?", "id": "Instans Program Yang Sedang Dijalankan." },
  { "en": "Apa Itu Thread?", "id": "Unit Eksekusi Dalam Suatu Proses." },
  { "en": "Apa Itu Konkurensi?", "id": "Eksekusi Beberapa Tugas Tumpang Tindih." },
  { "en": "Apa Itu Paralelisme?", "id": "Eksekusi Beberapa Tugas Secara Bersamaan." },
  { "en": "Apa Itu Deadlock?", "id": "Situasi Dua Proses Saling Menunggu." },
  { "en": "Apa Itu Manajemen Memori?", "id": "Mengalokasikan Dan Mengelola Memori Komputer." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Kumpulan Komputer Terhubung." },
  { "en": "Apa Itu Internet?", "id": "Jaringan Global Jaringan Komputer." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
  { "en": "Apa Itu Topologi Bus?", "id": "Semua Perangkat Terhubung Ke Satu Kabel Pusat." },
  { "en": "Apa Itu Topologi Bintang (Star)?", "id": "Semua Perangkat Terhubung Ke Hub Pusat." },
  { "en": "Apa Itu Topologi Cincin (Ring)?", "id": "Perangkat Terhubung Dalam Rangkaian Lingkaran Tertutup." },
  { "en": "Apa Itu Topologi Mesh?", "id": "Setiap Perangkat Terhubung Ke Setiap Perangkat Lain." },
  { "en": "Apa Itu Topologi Pohon (Tree)?", "id": "Gabungan Topologi Bintang Dan Topologi Bus." },
  { "en": "Apa Itu Repeater?", "id": "Memperkuat Sinyal Jaringan Untuk Jarak Jauh." },
  { "en": "Apa Itu Bridge?", "id": "Menghubungkan Dua Segmen Jaringan LAN." },
  { "en": "Apa Itu Gateway?", "id": "Gerbang Penghubung Dua Jaringan Berbeda Protokol." },
  { "en": "Apa Itu Network Interface Card (NIC)?", "id": "Kartu Antarmuka Jaringan." },
  { "en": "Apa Fungsi NIC (Network Interface Card)?", "id": "Menghubungkan Komputer Ke Jaringan Komputer." },
  { "en": "Apa Itu Collision Domain?", "id": "Segmen Jaringan Tempat Tabrakan Paket Terjadi." },
  { "en": "Apa Itu Broadcast Domain?", "id": "Area Jaringan Tempat Frame Broadcast Dikirim." },
  { "en": "Apa Itu Unicast?", "id": "Pengiriman Paket Ke Satu Tujuan Tunggal." },
  { "en": "Apa Itu Multicast?", "id": "Pengiriman Paket Ke Grup Tujuan Tertentu." },
  { "en": "Apa Itu Broadcast?", "id": "Pengiriman Paket Ke Semua Tujuan Jaringan." },
  { "en": "Apa Itu Latency Jaringan?", "id": "Waktu Tunda Perambatan Data Jaringan." },
  { "en": "Apa Itu Throughput Jaringan?", "id": "Laju Transfer Data Sukses Sebenarnya." },
  { "en": "Apa Itu Jitter Jaringan?", "id": "Variasi Penundaan Paket Dalam Jaringan." },
  { "en": "Apa Itu Paket Loss?", "id": "Kegagalan Paket Data Mencapai Tujuannya." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Dengan Pasangan Kawat Yang Dipilin." },
  { "en": "Apa Tujuan Memilin Kabel?", "id": "Mengurangi Interferensi Elektromagnetik (EMI)." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Twisted Pair Tanpa Lapisan Pelindung." },
  { "en": "Apa Itu Kabel STP (Shielded Twisted Pair)?", "id": "Kabel Twisted Pair Dengan Lapisan Pelindung." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Dengan Konduktor Pusat Dan Pelindung." },
  { "en": "Apa Itu Konektor RJ45?", "id": "Konektor Standar Untuk Kabel Jaringan Ethernet." },
  { "en": "Apa Itu Konektor BNC (Bayonet Neill-Concelman)?", "id": "Konektor Koaksial Untuk Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Serat Optik?", "id": "Media Transmisi Menggunakan Pulsa Cahaya." },
  { "en": "Apa Keuntungan Serat Optik?", "id": "Bandwidth Tinggi, Tahan Interferensi Elektromagnetik." },
  { "en": "Apa Itu Mode Tunggal (Single-Mode) Fiber?", "id": "Serat Optik Untuk Transmisi Jarak Jauh." },
  { "en": "Apa Itu Mode Multi (Multi-Mode) Fiber?", "id": "Serat Optik Untuk Transmisi Jarak Pendek." },
  { "en": "Apa Itu Access Point (AP)?", "id": "Perangkat Penghubung Nirkabel Ke Jaringan Kabel." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Unik Jaringan Nirkabel (Wi-Fi)." },
  { "en": "Apa Itu Enkripsi WPA2 (Wi-Fi Protected Access 2)?", "id": "Standar Keamanan Jaringan Nirkabel Yang Kuat." },
  { "en": "Apa Itu MAC Address Filtering?", "id": "Membatasi Akses Jaringan Berdasarkan Alamat MAC." },
  { "en": "Apa Itu Teknik Spread Spectrum?", "id": "Menyebarkan Sinyal Ke Pita Frekuensi Lebar." },
  { "en": "Apa Itu FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Mengubah Frekuensi Pembawa Secara Cepat." },
  { "en": "Apa Itu DSSS (Direct-Sequence Spread Spectrum)?", "id": "Mengalikan Data Dengan Sinyal Noise Semu." },
  { "en": "Apa Itu Multiplexing?", "id": "Menggabungkan Beberapa Sinyal Menjadi Satu." },
  { "en": "Apa Itu FDM (Frequency-Division Multiplexing)?", "id": "Multiplexing Berdasarkan Pembagian Frekuensi." },
  { "en": "Apa Itu TDM (Time-Division Multiplexing)?", "id": "Multiplexing Berdasarkan Pembagian Slot Waktu." },
  { "en": "Apa Itu WDM (Wavelength-Division Multiplexing)?", "id": "Multiplexing Untuk Sinyal Cahaya Serat Optik." },
  { "en": "Apa Itu Kode Baris (Line Coding)?", "id": "Metode Representasi Data Digital." },
  { "en": "Apa Itu Pengkodean NRZ (Non-Return-to-Zero)?", "id": "Level Tegangan Konstan Selama Durasi Bit." },
  { "en": "Apa Itu Pengkodean Manchester?", "id": "Transisi Terjadi Di Tengah Setiap Bit." },
  { "en": "Apa Keuntungan Pengkodean Manchester?", "id": "Memiliki Komponen Clock Untuk Sinkronisasi." },
  { "en": "Apa Itu Teori Sampling?", "id": "Prinsip Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Laju Nyquist?", "id": "Laju Sampling Minimum (Dua Kali Frekuensi)." },
  { "en": "Apa Itu Kuantisasi Seragam?", "id": "Setiap Level Kuantisasi Memiliki Ukuran Sama." },
  { "en": "Apa Itu Kuantisasi Non-Seragam?", "id": "Ukuran Level Kuantisasi Bervariasi." },
  { "en": "Apa Itu Companding?", "id": "Kompresi, Ekspansi Sinyal Untuk Kuantisasi." },
  { "en": "Apa Itu A-Law Dan μ-Law?", "id": "Algoritma Companding Standar Dalam Telekomunikasi." },
  { "en": "Apa Itu Pulse Code Modulation (PCM)?", "id": "Metode Standar Konversi Analog Ke Digital." },
  { "en": "Apa Itu Differential Pulse Code Modulation (DPCM)?", "id": "Mengkodekan Selisih Antar Sampel Berurutan." },
  { "en": "Apa Itu Delta Modulation (DM)?", "id": "Versi Sederhana Dari DPCM (Differential Pulse Code Modulation)." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Dalam Rentang Frekuensi Aslinya." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sinyal Digeser Ke Pita Frekuensi Lebih Tinggi." },
  { "en": "Apa Itu Modulasi Amplitudo Kuadratur (QAM)?", "id": "Mengubah Amplitudo Dan Fasa Sinyal." },
  { "en": "Apa Itu Demodulasi?", "id": "Proses Mengekstrak Sinyal Informasi Asli." },
  { "en": "Apa Itu Deteksi Koheren?", "id": "Membutuhkan Sinyal Referensi Sinkron Di Penerima." },
  { "en": "Apa Itu Regenerative Repeater?", "id": "Repeater Yang Memulihkan Bentuk Sinyal Digital." },
  { "en": "Apa Itu Pengacakan Data (Scrambling)?", "id": "Mengacak Urutan Bit Menghindari Urutan Panjang." },
  { "en": "Apa Itu Sinkronisasi Bit?", "id": "Menyelaraskan Clock Penerima Dengan Bit Diterima." },
  { "en": "Apa Itu Sinkronisasi Frame?", "id": "Mengidentifikasi Awal Dan Akhir Frame Data." },
  { "en": "Apa Itu Error Deteksi?", "id": "Mendeteksi Adanya Kesalahan Dalam Data." },
  { "en": "Apa Itu Error Koreksi?", "id": "Mendeteksi Dan Memperbaiki Kesalahan Data." },
  { "en": "Apa Itu Bit Paritas?", "id": "Bit Tambahan Untuk Deteksi Error Sederhana." },
  { "en": "Apa Itu Cyclic Redundancy Check (CRC)?", "id": "Metode Deteksi Error Yang Kuat." },
  { "en": "Apa Itu Checksum?", "id": "Metode Deteksi Error Lainnya." },
  { "en": "Apa Itu Forward Error Correction (FEC)?", "id": "Menambahkan Redundansi Untuk Koreksi Error." },
  { "en": "Apa Itu Automatic Repeat Request (ARQ)?", "id": "Meminta Transmisi Ulang Jika Terjadi Error." },
  { "en": "Apa Itu Stop-and-Wait ARQ?", "id": "Mengirim Satu Frame, Menunggu Konfirmasi." },
  { "en": "Apa Itu Go-Back-N ARQ?", "id": "Mengirim Ulang Frame Error, Semua Frame Berikutnya." },
  { "en": "Apa Itu Selective Repeat ARQ?", "id": "Hanya Mengirim Ulang Frame Yang Error." },
  { "en": "Apa Itu Protokol Sliding Window?", "id": "Memungkinkan Pengiriman Beberapa Frame Sekaligus." },
  { "en": "Apa Itu Kontrol Aliran (Flow Control)?", "id": "Mencegah Pengirim Membanjiri Penerima." },
  { "en": "Apa Itu Kontrol Kemacetan (Congestion Control)?", "id": "Mengelola Kemacetan Dalam Jaringan." },
  { "en": "Apa Itu Public Switched Telephone Network (PSTN)?", "id": "Jaringan Telepon Global Berbasis Sirkuit." },
  { "en": "Apa Itu Circuit Switching?", "id": "Membangun Jalur Khusus Untuk Komunikasi." },
  { "en": "Apa Itu Packet Switching?", "id": "Data Dibagi, Dikirim Sebagai Paket Terpisah." },
  { "en": "Apa Itu Datagram?", "id": "Paket Independen Dalam Jaringan Packet-Switched." },
  { "en": "Apa Itu Sirkuit Virtual?", "id": "Jalur Logis Ditetapkan Sebelum Transfer Data." },
  { "en": "Apa Itu Integrated Services Digital Network (ISDN)?", "id": "Standar Jaringan Digital Untuk Suara, Data." },
  { "en": "Apa Itu Asynchronous Transfer Mode (ATM)?", "id": "Teknologi Switching Paket Berbasis Sel." },
  { "en": "Apa Itu Sel Dalam ATM (Asynchronous Transfer Mode)?", "id": "Paket Data Ukuran Tetap (53 Byte)." },
  { "en": "Apa Itu Frame Relay?", "id": "Teknologi Jaringan WAN (Wide Area Network) Packet-Switched." },
  { "en": "Apa Itu X.25?", "id": "Standar Protokol Awal Jaringan WAN." },
  { "en": "Apa Itu Voice over IP (VoIP)?", "id": "Layanan Suara Melalui Jaringan Internet Protocol." },
  { "en": "Apa Itu Session Initiation Protocol (SIP)?", "id": "Protokol Sinyal Umum Untuk VoIP." },
  { "en": "Apa Itu H.323?", "id": "Standar Protokol Lain Untuk Konferensi Multimedia." },
  { "en": "Apa Itu Real-time Transport Protocol (RTP)?", "id": "Protokol Pengiriman Data Real-time (Suara, Video)." },
  { "en": "Apa Itu Quality of Service (QoS)?", "id": "Kemampuan Jaringan Memberikan Layanan Prioritas." },
  { "en": "Apa Itu Jitter Buffer?", "id": "Menyimpan Paket VoIP Mengurangi Efek Jitter." },
  { "en": "Apa Itu Mean Opinion Score (MOS)?", "id": "Ukuran Subjektif Kualitas Suara VoIP." },
  { "en": "Apa Itu Komunikasi Optik?", "id": "Transmisi Informasi Menggunakan Cahaya." },
  { "en": "Apa Itu Light Emitting Diode (LED)?", "id": "Sumber Cahaya Semikonduktor Dalam Komunikasi Optik." },
  { "en": "Apa Itu Dioda Laser?", "id": "Sumber Cahaya Koheren Untuk Serat Optik." },
  { "en": "Apa Itu Fotodetektor?", "id": "Mengubah Sinyal Cahaya Kembali Menjadi Listrik." },
  { "en": "Apa Itu Dioda PIN?", "id": "Jenis Fotodetektor Umum Dalam Komunikasi Optik." },
  { "en": "Apa Itu Avalanche Photodiode (APD)?", "id": "Fotodetektor Dengan Penguatan Sinyal Internal." },
  { "en": "Apa Itu Dispersi Dalam Serat Optik?", "id": "Penyebaran Pulsa Cahaya Seiring Jarak." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Dispersi Akibat Perbedaan Kecepatan Panjang Gelombang." },
  { "en": "Apa Itu Dispersi Modus?", "id": "Dispersi Hanya Terjadi Dalam Serat Multi-mode." },
  { "en": "Apa Itu Atenuasi Serat Optik?", "id": "Pelemahan Kekuatan Sinyal Cahaya." },
  { "en": "Apa Itu Amplifier Serat Optik?", "id": "Menguatkan Sinyal Cahaya Langsung Di Serat." },
  { "en": "Apa Itu Erbium-Doped Fiber Amplifier (EDFA)?", "id": "Jenis Amplifier Serat Optik Paling Umum." },
  { "en": "Apa Itu Jaringan Optik Pasif (PON)?", "id": "Jaringan Serat Optik Tanpa Komponen Aktif." },
  { "en": "Apa Itu Komunikasi Seluler?", "id": "Komunikasi Nirkabel Menggunakan Jaringan Sel." },
  { "en": "Apa Itu Sel Dalam Jaringan Seluler?", "id": "Area Geografis Dilayani Satu Base Station." },
  { "en": "Apa Itu Base Station (BS)?", "id": "Titik Akses Nirkabel Untuk Perangkat Seluler." },
  { "en": "Apa Itu Mobile Switching Center (MSC)?", "id": "Pusat Pengalihan Jaringan Seluler." },
  { "en": "Apa Itu Handoff (Atau Handover)?", "id": "Proses Pengalihan Panggilan Antar Sel." },
  { "en": "Apa Itu Penggunaan Ulang Frekuensi?", "id": "Menggunakan Frekuensi Sama Di Sel Tidak Berdekatan." },
  { "en": "Apa Itu Interferensi Antar Sel?", "id": "Gangguan Dari Sel Tetangga." },
  { "en": "Apa Itu Global System for Mobile Communications (GSM)?", "id": "Standar Global Untuk Komunikasi Seluler 2G." },
  { "en": "Apa Itu Subscriber Identity Module (SIM)?", "id": "Kartu Cerdas Mengidentifikasi Pelanggan Jaringan." },
  { "en": "Apa Itu International Mobile Equipment Identity (IMEI)?", "id": "Nomor Unik Mengidentifikasi Perangkat Seluler." },
  { "en": "Apa Itu Universal Mobile Telecommunications System (UMTS)?", "id": "Standar Utama Untuk Jaringan Seluler 3G." },
  { "en": "Apa Itu Long-Term Evolution (LTE)?", "id": "Standar Untuk Jaringan Seluler 4G." },
  { "en": "Apa Itu Evolved Packet Core (EPC)?", "id": "Arsitektur Inti Jaringan LTE (Long-Term Evolution)." },
  { "en": "Apa Itu Orthogonal Frequency-Division Multiple Access (OFDMA)?", "id": "Skema Akses Digunakan Dalam Jaringan LTE." },
  { "en": "Apa Itu Single-Carrier Frequency-Division Multiple Access (SC-FDMA)?", "id": "Digunakan Untuk Uplink Dalam Jaringan LTE." },
  { "en": "Apa Itu Jaringan 5G?", "id": "Generasi Kelima Teknologi Jaringan Seluler." },
  { "en": "Apa Saja Tiga Kasus Penggunaan Utama 5G?", "id": "EMBB, URLLC, Dan MMTC." },
  { "en": "Apa Itu Enhanced Mobile Broadband (eMBB)?", "id": "Kecepatan Data Sangat Tinggi." },
  { "en": "Apa Itu Ultra-Reliable Low-Latency Communication (URLLC)?", "id": "Komunikasi Sangat Andal, Latensi Rendah." },
  { "en": "Apa Itu Massive Machine-Type Communications (mMTC)?", "id": "Menghubungkan Sejumlah Besar Perangkat IoT." },
  { "en": "Apa Itu Beamforming Dalam 5G?", "id": "Memfokuskan Sinyal Nirkabel Ke Arah Pengguna." },
  { "en": "Apa Itu Network Slicing?", "id": "Membuat Beberapa Jaringan Virtual Di Atas Fisik." },
  { "en": "Apa Itu Gelombang Milimeter (mmWave)?", "id": "Pita Frekuensi Sangat Tinggi Digunakan 5G." },
  { "en": "Apa Kelemahan Gelombang Milimeter?", "id": "Jangkauan Pendek, Mudah Terhalang." },
  { "en": "Apa Itu Small Cells Dalam 5G?", "id": "Base Station Kecil Meningkatkan Kapasitas Jaringan." },
  { "en": "Apa Itu Massive MIMO (Multiple-Input Multiple-Output)?", "id": "Menggunakan Ratusan Antena Di Base Station." },
  { "en": "Apa Itu Komunikasi Satelit?", "id": "Menggunakan Satelit Sebagai Relay Di Luar Angkasa." },
  { "en": "Apa Itu Orbit Geostasioner (GEO)?", "id": "Satelit Tampak Diam Dari Permukaan Bumi." },
  { "en": "Apa Itu Orbit Bumi Rendah (LEO)?", "id": "Satelit Mengorbit Dekat Dengan Permukaan Bumi." },
  { "en": "Apa Itu Orbit Bumi Menengah (MEO)?", "id": "Orbit Antara LEO Dan GEO." },
  { "en": "Apa Keuntungan Satelit LEO (Low Earth Orbit)?", "id": "Latensi Lebih Rendah, Cakupan Global." },
  { "en": "Apa Itu Konstelasi Satelit?", "id": "Sekelompok Satelit Bekerja Bersama." },
  { "en": "Apa Itu Stasiun Bumi (Ground Station)?", "id": "Antena Di Bumi Berkomunikasi Dengan Satelit." },
  { "en": "Apa Itu Uplink?", "id": "Transmisi Dari Stasiun Bumi Ke Satelit." },
  { "en": "Apa Itu Downlink?", "id": "Transmisi Dari Satelit Ke Stasiun Bumi." },
  { "en": "Apa Itu Transponder Satelit?", "id": "Menerima, Menguatkan, Dan Memancarkan Ulang Sinyal." },
  { "en": "Apa Itu Jejak Satelit (Satellite Footprint)?", "id": "Area Cakupan Sinyal Satelit Di Bumi." },
  { "en": "Apa Itu Anggaran Taut (Link Budget)?", "id": "Perhitungan Semua Penguatan, Kerugian Sinyal." },
  { "en": "Apa Itu Atenuasi Hujan?", "id": "Pelemahan Sinyal Satelit Akibat Hujan." },
  { "en": "Apa Itu Very Small Aperture Terminal (VSAT)?", "id": "Terminal Satelit Dua Arah Ukuran Kecil." },
  { "en": "Apa Itu Penyiaran Satelit Langsung (DBS)?", "id": "Layanan Televisi Langsung Dari Satelit." },
  { "en": "Apa Itu Radar?", "id": "Sistem Mendeteksi Objek Menggunakan Gelombang Radio." },
  { "en": "Apa Kepanjangan Radar?", "id": "Radio Detection And Ranging." },
  { "en": "Apa Itu Radar Pulsa?", "id": "Mengirim Pulsa Radio, Mendengarkan Gema." },
  { "en": "Apa Itu Radar Gelombang Kontinu (CW)?", "id": "Mengirim Sinyal Radio Secara Terus-menerus." },
  { "en": "Apa Itu Radar Doppler?", "id": "Mengukur Kecepatan Objek Menggunakan Efek Doppler." },
  { "en": "Apa Itu Persamaan Jangkauan Radar?", "id": "Menghitung Daya Gema Diterima Radar." },
  { "en": "Apa Itu Penampang Radar (RCS)?", "id": "Ukuran Seberapa Reflektif Objek Terhadap Radar." },
  { "en": "Apa Itu Clutter?", "id": "Gema Tak Diinginkan (Hujan, Tanah)." },
  { "en": "Apa Itu Moving Target Indication (MTI)?", "id": "Teknik Radar Membedakan Target Bergerak." },
  { "en": "Apa Itu Radar Phased Array?", "id": "Menggunakan Array Antena Arah Sorotan Elektronik." },
  { "en": "Apa Itu Radar Apertur Sintetik (SAR)?", "id": "Teknik Radar Menghasilkan Gambar Resolusi Tinggi." },
  { "en": "Apa Itu Ground Penetrating Radar (GPR)?", "id": "Radar Untuk Melihat Objek Di Bawah Tanah." },
  { "en": "Apa Itu Navigasi?", "id": "Proses Menentukan Posisi Dan Arah." },
  { "en": "Apa Itu Navigasi Inersia?", "id": "Menggunakan Akselerometer, Giroskop Menentukan Posisi." },
  { "en": "Apa Itu Inertial Measurement Unit (IMU)?", "id": "Sensor Mengukur Percepatan Dan Rotasi." },
  { "en": "Apa Itu Dead Reckoning?", "id": "Menghitung Posisi Saat Ini Berdasarkan Sebelumnya." },
  { "en": "Apa Itu Navigasi Radio?", "id": "Menggunakan Sinyal Radio Untuk Penentuan Posisi." },
  { "en": "Apa Itu LORAN (Long Range Navigation)?", "id": "Sistem Navigasi Radio Darat Jarak Jauh." },
  { "en": "Apa Itu Global Navigation Satellite System (GNSS)?", "id": "Sistem Navigasi Berbasis Konstelasi Satelit." },
  { "en": "Apa Itu Trilaterasi?", "id": "Menentukan Posisi Berdasarkan Jarak Ke Tiga Titik." },
  { "en": "Apa Itu Sinyal GPS (Global Positioning System)?", "id": "Sinyal Radio Mengandung Waktu, Informasi Posisi." },
  { "en": "Apa Itu Kode C/A (Coarse/Acquisition)?", "id": "Sinyal Sipil Standar GPS." },
  { "en": "Apa Itu Kode P(Y) (Precise)?", "id": "Sinyal Militer Terenkripsi GPS." },
  { "en": "Apa Itu Sumber Error GPS?", "id": "Jam Satelit, Atmosfer, Multipath." },
  { "en": "Apa Itu Multipath Error?", "id": "Error Akibat Sinyal Pantulan." },
  { "en": "Apa Itu Dilution of Precision (DOP)?", "id": "Ukuran Geometri Satelit Mempengaruhi Akurasi." },
  { "en": "Apa Itu Differential GPS (DGPS)?", "id": "Meningkatkan Akurasi Menggunakan Stasiun Referensi." },
  { "en": "Apa Itu Wide Area Augmentation System (WAAS)?", "id": "Sistem Augmentasi Berbasis Satelit Untuk GPS." },
  { "en": "Apa Itu GLONASS?", "id": "Sistem Navigasi Satelit Global Milik Rusia." },
  { "en": "Apa Itu Galileo?", "id": "Sistem Navigasi Satelit Global Milik Eropa." },
  { "en": "Apa Itu BeiDou?", "id": "Sistem Navigasi Satelit Global Milik Tiongkok." },
  { "en": "Apa Itu Komunikasi Optik Ruang Bebas (FSO)?", "id": "Menggunakan Cahaya Merambat Melalui Udara." },
  { "en": "Apa Keuntungan FSO (Free-Space Optical)?", "id": "Bandwidth Sangat Tinggi, Keamanan Baik." },
  { "en": "Apa Kerugian FSO (Free-Space Optical)?", "id": "Sensitif Terhadap Kondisi Atmosfer (Kabut)." },
  { "en": "Apa Itu Li-Fi (Light Fidelity)?", "id": "Komunikasi Nirkabel Menggunakan Cahaya Tampak." },
  { "en": "Apa Itu Jaringan Ad Hoc Nirkabel?", "id": "Jaringan Nirkabel Tanpa Infrastruktur Terpusat." },
  { "en": "Apa Itu Jaringan Mesh Nirkabel?", "id": "Setiap Node Terhubung Ke Beberapa Node Lain." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel?", "id": "Jaringan Sensor Terdistribusi Secara Spasial." },
  { "en": "Apa Itu Komunikasi Bawah Air Akustik?", "id": "Menggunakan Gelombang Suara Berkomunikasi Bawah Air." },
  { "en": "Apa Itu Modem Akustik?", "id": "Perangkat Mengubah Sinyal Digital, Akustik." },
  { "en": "Apa Tantangan Komunikasi Bawah Air?", "id": "Bandwidth Terbatas, Penundaan, Multipath." },
  { "en": "Apa Itu Komunikasi Molekuler?", "id": "Penggunaan Molekul Sebagai Pembawa Informasi." },
  { "en": "Apa Itu Komunikasi Daya Rendah?", "id": "Teknik Komunikasi Hemat Energi." },
  { "en": "Apa Itu Komunikasi Ultra-Wideband (UWB)?", "id": "Menggunakan Pulsa Sangat Pendek, Bandwidth Lebar." },
  { "en": "Apa Aplikasi UWB (Ultra-Wideband)?", "id": "Penentuan Lokasi Presisi, Radar Jarak Pendek." },
  { "en": "Apa Itu Radio Kognitif?", "id": "Radio Cerdas Dapat Mendeteksi, Beradaptasi Lingkungan." },
  { "en": "Apa Itu Sensing Spektrum?", "id": "Mendeteksi Pita Frekuensi Yang Tidak Digunakan." },
  { "en": "Apa Itu Dynamic Spectrum Access (DSA)?", "id": "Menggunakan Spektrum Secara Oportunistik." },
  { "en": "Apa Itu Komunikasi Kooperatif?", "id": "Terminal Saling Membantu Mengirimkan Data." },
  { "en": "Apa Itu Model Relay Amplify-and-Forward (AF)?", "id": "Relay Menguatkan, Meneruskan Sinyal Diterima." },
  { "en": "Apa Itu Model Relay Decode-and-Forward (DF)?", "id": "Relay Mendekode, Mengkode Ulang, Meneruskan Sinyal." },
  { "en": "Apa Itu Komunikasi Terahertz (THz)?", "id": "Komunikasi Di Pita Frekuensi Terahertz." },
  { "en": "Apa Potensi Komunikasi Terahertz?", "id": "Kecepatan Data Sangat Tinggi (Terabit/detik)." },
  { "en": "Apa Itu Keamanan Lapisan Fisik?", "id": "Menggunakan Sifat Kanal Fisik Keamanan." },
  { "en": "Apa Itu Sinyal Rahasia Buatan?", "id": "Menambahkan Derau Terstruktur Mengacaukan Penyadap." },
  { "en": "Apa Itu Propagasi Non-Line-of-Sight (NLOS)?", "id": "Sinyal Mencapai Penerima Tanpa Jalur Langsung." },
  { "en": "Apa Itu Pemodelan Kanal Deterministik?", "id": "Menggunakan Ray Tracing Memprediksi Perambatan." },
  { "en": "Apa Itu Pemodelan Kanal Stokastik?", "id": "Menggunakan Model Statistik Mengkarakterisasi Kanal." },
  { "en": "Apa Itu Fading Skala Kecil?", "id": "Fluktuasi Cepat Kekuatan Sinyal." },
  { "en": "Apa Itu Fading Skala Besar?", "id": "Pelemahan Sinyal Rata-rata Seiring Jarak." },
  { "en": "Apa Itu Komunikasi Nirkabel?", "id": "Transmisi Informasi Tanpa Menggunakan Kabel." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Frekuensi Radiasi Elektromagnetik." },
  { "en": "Apa Itu Gelombang Radio?", "id": "Gelombang Elektromagnetik Untuk Komunikasi Nirkabel." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Sinyal Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Demodulasi?", "id": "Mengekstrak Sinyal Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Itu Amplitudo?", "id": "Kekuatan Atau Ketinggian Maksimum Gelombang." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Gelombang Per Detik." },
  { "en": "Apa Itu Fasa?", "id": "Posisi Relatif Gelombang Dalam Siklusnya." },
  { "en": "Apa Itu Panjang Gelombang?", "id": "Jarak Antara Puncak Gelombang Yang Berurutan." },
  { "en": "Apa Itu Kanal Komunikasi?", "id": "Medium Fisik Tempat Sinyal Merambat." },
  { "en": "Apa Itu Derau (Noise)?", "id": "Gangguan Acak Yang Tidak Diinginkan." },
  { "en": "Apa Itu Interferensi?", "id": "Gangguan Dari Sinyal Lain Yang Tidak Diinginkan." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Kekuatan Sinyal Terhadap Kekuatan Derau." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Yang Digunakan Oleh Sinyal." },
  { "en": "Apa Itu Modulasi Analog?", "id": "Modulasi Menggunakan Sinyal Informasi Analog." },
  { "en": "Apa Itu Amplitude Modulation (AM)?", "id": "Amplitudo Pembawa Bervariasi Sesuai Sinyal Informasi." },
  { "en": "Apa Itu Frequency Modulation (FM)?", "id": "Frekuensi Pembawa Bervariasi Sesuai Sinyal Informasi." },
  { "en": "Mana Yang Lebih Tahan Derau, AM Atau FM?", "id": "Frequency Modulation (FM) Lebih Tahan Derau." },
  { "en": "Apa Itu Modulasi Digital?", "id": "Modulasi Menggunakan Sinyal Informasi Digital." },
  { "en": "Apa Itu Amplitude-Shift Keying (ASK)?", "id": "Data Digital Direpresentasikan Oleh Amplitudo." },
  { "en": "Apa Itu Frequency-Shift Keying (FSK)?", "id": "Data Digital Direpresentasikan Oleh Frekuensi." },
  { "en": "Apa Itu Phase-Shift Keying (PSK)?", "id": "Data Digital Direpresentasikan Oleh Fasa." },
  { "en": "Apa Itu Quadrature Amplitude Modulation (QAM)?", "id": "Menggabungkan Modulasi Amplitudo Dan Fasa." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Representasi Grafis Skema Modulasi Digital." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Persentase Bit Yang Diterima Salah." },
  { "en": "Apa Itu Antena?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang Radio." },
  { "en": "Apa Itu Antena Isotropik?", "id": "Antena Hipotetis Memancar Ke Segala Arah." },
  { "en": "Apa Itu Antena Dipol?", "id": "Antena Sederhana Terdiri Dari Dua Elemen." },
  { "en": "Apa Itu Penguatan Antena?", "id": "Kemampuan Antena Memfokuskan Energi." },
  { "en": "Apa Itu Pola Radiasi?", "id": "Pola Arah Pancaran Energi Antena." },
  { "en": "Apa Itu Propagasi Gelombang Radio?", "id": "Cara Gelombang Radio Merambat." },
  { "en": "Apa Itu Propagasi Garis Pandang (Line-of-Sight)?", "id": "Sinyal Merambat Langsung Tanpa Halangan." },
  { "en": "Apa Itu Refleksi?", "id": "Pemantulan Gelombang Saat Mengenai Permukaan." },
  { "en": "Apa Itu Difraksi?", "id": "Pembelokan Gelombang Saat Melewati Penghalang." },
  { "en": "Apa Itu Hamburan (Scattering)?", "id": "Penyebaran Gelombang Oleh Objek Kecil." },
  { "en": "Apa Itu Fading Multipath?", "id": "Fluktuasi Sinyal Akibat Banyaknya Jalur." },
  { "en": "Apa Itu Sistem Seluler?", "id": "Jaringan Nirkabel Berbasis Pembagian Area Sel." },
  { "en": "Apa Itu Base Station?", "id": "Pemancar Dan Penerima Di Pusat Sel." },
  { "en": "Apa Itu Handoff?", "id": "Proses Berpindah Antar Sel Tanpa Putus." },
  { "en": "Apa Itu Penggunaan Ulang Frekuensi?", "id": "Menggunakan Frekuensi Sama Di Sel Berjauhan." },
  { "en": "Apa Itu Wi-Fi?", "id": "Teknologi Jaringan Area Lokal Nirkabel." },
  { "en": "Apa Itu Access Point (AP)?", "id": "Perangkat Penghubung Jaringan Wi-Fi." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Jaringan Wi-Fi Anda." },
  { "en": "Apa Itu Bluetooth?", "id": "Teknologi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Pemasangan (Pairing) Bluetooth?", "id": "Proses Menghubungkan Dua Perangkat Bluetooth." },
  { "en": "Apa Itu Near Field Communication (NFC)?", "id": "Komunikasi Nirkabel Jarak Sangat Dekat." },
  { "en": "Apa Itu Radio-Frequency Identification (RFID)?", "id": "Menggunakan Gelombang Radio Untuk Identifikasi." },
  { "en": "Apa Itu Tag RFID (Radio-Frequency Identification)?", "id": "Label Elektronik Dengan Informasi." },
  { "en": "Apa Itu Sistem Pemosisian Global (GPS)?", "id": "Sistem Navigasi Berbasis Satelit." },
  { "en": "Bagaimana Cara Kerja GPS (Global Positioning System)?", "id": "Menerima Sinyal Dari Beberapa Satelit." },
  { "en": "Apa Itu Kode Biner?", "id": "Sistem Bilangan Berbasis Dua (0, 1)." },
  { "en": "Apa Itu Bit?", "id": "Unit Terkecil Data Digital." },
  { "en": "Apa Itu Byte?", "id": "Sekelompok Delapan Bit." },
  { "en": "Apa Itu Kode ASCII?", "id": "Standar Pengkodean Karakter Untuk Teks." },
  { "en": "Apa Itu Gerbang Logika?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Itu Sirkuit Kombinasional?", "id": "Output Hanya Tergantung Input Saat Ini." },
  { "en": "Apa Itu Sirkuit Sekuensial?", "id": "Output Tergantung Input, Keadaan Sebelumnya." },
  { "en": "Apa Itu Flip-Flop?", "id": "Sirkuit Sekuensial Penyimpan Satu Bit." },
  { "en": "Apa Itu Register?", "id": "Sekelompok Flip-Flop Menyimpan Data." },
  { "en": "Apa Itu Penghitung (Counter)?", "id": "Sirkuit Sekuensial Menghitung Pulsa." },
  { "en": "Apa Itu Memori Komputer?", "id": "Perangkat Penyimpan Data Digital." },
  { "en": "Apa Itu Random Access Memory (RAM)?", "id": "Memori Volatile Untuk Data Sementara." },
  { "en": "Apa Itu Read-Only Memory (ROM)?", "id": "Memori Non-Volatile Berisi Instruksi Tetap." },
  { "en": "Apa Itu Mikroprosesor?", "id": "Unit Pemrosesan Pusat (CPU) Pada Chip." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Pada Satu Chip." },
  { "en": "Apa Beda Prosesor Dan Kontroler?", "id": "Mikrokontroler Memiliki Memori, I/O Terintegrasi." },
  { "en": "Apa Itu Sistem Tertanam?", "id": "Sistem Komputer Dalam Perangkat Lebih Besar." },
  { "en": "Apa Itu Sensor?", "id": "Mengubah Besaran Fisik Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Aktuator?", "id": "Mengubah Sinyal Listrik Menjadi Aksi Fisik." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Menjadi Analog." },
  { "en": "Apa Itu Bus Komputer?", "id": "Jalur Komunikasi Antar Komponen Komputer." },
  { "en": "Apa Itu Bus Data?", "id": "Membawa Data Antar Komponen." },
  { "en": "Apa Itu Bus Alamat?", "id": "Menentukan Lokasi Memori Untuk Diakses." },
  { "en": "Apa Itu Bus Kontrol?", "id": "Membawa Sinyal Perintah Dan Waktu." },
  { "en": "Apa Itu Optoelektronika?", "id": "Studi Perangkat Sumber, Deteksi Cahaya." },
  { "en": "Apa Itu Light-Emitting Diode (LED)?", "id": "Dioda Yang Memancarkan Cahaya Saat Dialiri." },
  { "en": "Apa Itu Fotodioda?", "id": "Dioda Mengubah Cahaya Menjadi Arus Listrik." },
  { "en": "Apa Itu Sel Surya?", "id": "Mengubah Energi Cahaya Matahari Menjadi Listrik." },
  { "en": "Apa Itu Dioda Laser?", "id": "Sumber Cahaya Semikonduktor Koheren." },
  { "en": "Apa Itu Optocoupler (Optoisolator)?", "id": "Mengisolasi Dua Sirkuit Secara Elektrik." },
  { "en": "Apa Itu Serat Optik?", "id": "Pemandu Cahaya Tipis Untuk Komunikasi." },
  { "en": "Apa Itu Refleksi Internal Total?", "id": "Prinsip Dasar Perambatan Cahaya Serat Optik." },
  { "en": "Apa Itu Elektromagnet?", "id": "Magnet Dibuat Dengan Mengalirkan Arus Listrik." },
  { "en": "Apa Itu Solenoida?", "id": "Kumparan Kawat Berbentuk Silinder." },
  { "en": "Apa Itu Relay?", "id": "Sakelar Dioperasikan Secara Elektromagnetik." },
  { "en": "Apa Itu Motor Listrik?", "id": "Mengubah Energi Listrik Menjadi Gerak Mekanis." },
  { "en": "Apa Itu Generator Listrik?", "id": "Mengubah Energi Mekanis Menjadi Energi Listrik." },
  { "en": "Apa Itu Prinsip Induksi Elektromagnetik?", "id": "Medan Magnet Berubah Menghasilkan Arus." },
  { "en": "Apa Itu Transformator?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Lilitan Primer?", "id": "Lilitan Transformator Terhubung Ke Sumber." },
  { "en": "Apa Itu Lilitan Sekunder?", "id": "Lilitan Transformator Terhubung Ke Beban." },
  { "en": "Apa Itu Trafo Step-Up?", "id": "Menaikkan Tegangan, Menurunkan Arus." },
  { "en": "Apa Itu Trafo Step-Down?", "id": "Menurunkan Tegangan, Menaikkan Arus." },
  { "en": "Apa Itu Inti Transformator?", "id": "Material Memusatkan Fluks Magnetik." },
  { "en": "Apa Itu Arus Eddy?", "id": "Arus Sirkulasi Di Dalam Inti Konduktif." },
  { "en": "Bagaimana Mengurangi Kerugian Arus Eddy?", "id": "Menggunakan Inti Besi Berlaminasi." },
  { "en": "Apa Itu Efisiensi Transformator?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Apa Itu Autotransformer?", "id": "Transformator Dengan Hanya Satu Lilitan." },
  { "en": "Apa Itu Instrumentasi?", "id": "Seni Dan Ilmu Pengukuran, Kontrol." },
  { "en": "Apa Itu Transduser?", "id": "Mengubah Satu Bentuk Energi Ke Bentuk Lain." },
  { "en": "Apa Itu Akurasi?", "id": "Kedekatan Pengukuran Dengan Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi?", "id": "Kedekatan Pengukuran Berulang Satu Sama Lain." },
  { "en": "Apa Itu Resolusi?", "id": "Perubahan Terkecil Yang Dapat Dideteksi." },
  { "en": "Apa Itu Sensitivitas?", "id": "Rasio Perubahan Output Terhadap Input." },
  { "en": "Apa Itu Kesalahan Kalibrasi?", "id": "Penyimpangan Dari Skala Standar." },
  { "en": "Apa Itu Pengkondisian Sinyal?", "id": "Memodifikasi Sinyal Agar Sesuai Diproses." },
  { "en": "Apa Itu Amplifikasi?", "id": "Proses Meningkatkan Kekuatan Sinyal." },
  { "en": "Apa Itu Atenuasi?", "id": "Proses Melemahkan Kekuatan Sinyal." },
  { "en": "Apa Itu Filtering?", "id": "Menghilangkan Komponen Frekuensi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Akuisisi Data (DAQ)?", "id": "Proses Mengukur, Merekam Sinyal Dunia Nyata." },
  { "en": "Apa Itu Sistem Otomasi Gedung (BAS)?", "id": "Sistem Kontrol Terpusat Untuk Fasilitas Gedung." },
  { "en": "Fungsi Apa Saja Yang Diatur Oleh BAS?", "id": "HVAC, Pencahayaan, Keamanan, Dan Akses." },
  { "en": "Apa Kepanjangan HVAC (Heating, Ventilation, and Air Conditioning)?", "id": "Pemanasan, Ventilasi, Dan Pendingin Udara." },
  { "en": "Apa Itu Protokol BACnet?", "id": "Protokol Komunikasi Data Untuk Jaringan Otomasi Gedung." },
  { "en": "Apa Itu Protokol LonWorks?", "id": "Protokol Jaringan Lain Untuk Aplikasi Kontrol." },
  { "en": "Apa Itu Kontrol Pencahayaan?", "id": "Sistem Mengatur Tingkat Cahaya Secara Otomatis." },
  { "en": "Apa Itu Pemanenan Siang Hari (Daylight Harvesting)?", "id": "Menggunakan Cahaya Alami Mengurangi Pencahayaan Buatan." },
  { "en": "Apa Itu Sensor Kehadiran (Occupancy Sensor)?", "id": "Mendeteksi Keberadaan Orang Dalam Ruangan." },
  { "en": "Apa Itu Sensor Kekosongan (Vacancy Sensor)?", "id": "Membutuhkan Input Manual Untuk Menghidupkan Lampu." },
  { "en": "Apa Itu Protokol DALI?", "id": "Digital Addressable Lighting Interface, Untuk Pencahayaan." },
  { "en": "Apa Itu Sistem Tenaga Cadangan?", "id": "Menyediakan Listrik Selama Pemadaman Jaringan." },
  { "en": "Apa Itu Uninterruptible Power Supply (UPS)?", "id": "Menyediakan Cadangan Listrik Jangka Sangat Pendek." },
  { "en": "Apa Itu Generator Siaga (Standby Generator)?", "id": "Menyediakan Cadangan Listrik Jangka Panjang." },
  { "en": "Apa Itu Automatic Transfer Switch (ATS)?", "id": "Mengalihkan Beban Antara Jaringan, Generator." },
  { "en": "Apa Itu Biaya Permintaan (Demand Charge)?", "id": "Biaya Berdasarkan Penggunaan Listrik Puncak." },
  { "en": "Apa Itu Peak Shaving?", "id": "Mengurangi Konsumsi Listrik Selama Periode Puncak." },
  { "en": "Bagaimana Peak Shaving Dilakukan?", "id": "Menggunakan Generator Atau Sistem Penyimpanan Energi." },
  { "en": "Apa Itu Load Shedding?", "id": "Mengurangi Beban Listrik Secara Terencana." },
  { "en": "Apa Itu Teori Kontrol Klasik?", "id": "Analisis Berbasis Fungsi Transfer Domain Frekuensi." },
  { "en": "Apa Itu Teori Kontrol Modern?", "id": "Analisis Berbasis Representasi Ruang Keadaan." },
  { "en": "Apa Itu Variabel Keadaan (State Variable)?", "id": "Set Variabel Mendeskripsikan Keadaan Internal Sistem." },
  { "en": "Apa Itu Vektor Keadaan?", "id": "Vektor Yang Elemennya Adalah Variabel Keadaan." },
  { "en": "Apa Itu Ruang Keadaan (State Space)?", "id": "Ruang Vektor Mencakup Semua Kemungkinan Keadaan." },
  { "en": "Apa Itu Persamaan Keadaan?", "id": "Persamaan Diferensial Orde Pertama." },
  { "en": "Apa Itu Matriks Transisi Keadaan?", "id": "Menghubungkan Keadaan Awal Dengan Keadaan Kemudian." },
  { "en": "Apa Itu Keterkontrolan (Controllability)?", "id": "Kemampuan Menggerakkan Sistem Ke Keadaan Apapun." },
  { "en": "Apa Itu Keteramatan (Observability)?", "id": "Kemampuan Menentukan Keadaan Internal Dari Output." },
  { "en": "Apa Itu Penempatan Kutub (Pole Placement)?", "id": "Teknik Mendesain Umpan Balik Keadaan." },
  { "en": "Apa Itu Pengamat Keadaan (State Observer)?", "id": "Mengestimasi Keadaan Sistem Yang Tak Terukur." },
  { "en": "Apa Itu Estimator Luenberger?", "id": "Jenis Pengamat Keadaan Untuk Sistem Linear." },
  { "en": "Apa Itu Teori Kontrol Digital?", "id": "Analisis Dan Desain Sistem Kontrol Waktu-Diskret." },
  { "en": "Apa Itu Transformasi-Z?", "id": "Alat Matematis Untuk Menganalisis Sistem Diskret." },
  { "en": "Apa Itu Domain-Z?", "id": "Domain Frekuensi Kompleks Untuk Sistem Diskret." },
  { "en": "Bagaimana Stabilitas Sistem Diskret Ditentukan?", "id": "Semua Kutub Harus Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Transformasi Bintang (Star Transform)?", "id": "Nama Lain Untuk Transformasi-Z." },
  { "en": "Apa Itu Metode Tustin?", "id": "Nama Lain Untuk Transformasi Bilinear." },
  { "en": "Apa Itu Kontroler Deadbeat?", "id": "Mencapai Setpoint Dalam Jumlah Langkah Minimum." },
  { "en": "Apa Itu Teori Kontrol Non-linear?", "id": "Mempelajari Sistem Yang Tidak Mematuhi Superposisi." },
  { "en": "Apa Itu Linierisasi?", "id": "Mengaproksimasi Sistem Non-linear Dekat Titik Operasi." },
  { "en": "Apa Itu Fungsi Lyapunov?", "id": "Fungsi Skalar Menganalisis Stabilitas Non-linear." },
  { "en": "Apa Itu Teori Bifurkasi?", "id": "Studi Perubahan Kualitatif Perilaku Sistem." },
  { "en": "Apa Itu Sirkadian?", "id": "Siklus Biologis Sekitar 24 Jam." },
  { "en": "Apa Itu Pencahayaan Berpusat Pada Manusia (HCL)?", "id": "Pencahayaan Meniru Siklus Cahaya Alami." },
  { "en": "Apa Itu Suhu Warna (Color Temperature)?", "id": "Ukuran Tampilan Warna Sumber Cahaya." },
  { "en": "Apa Satuan Suhu Warna?", "id": "Kelvin (K)." },
  { "en": "Apa Itu Indeks Rendering Warna (CRI)?", "id": "Kemampuan Sumber Cahaya Menampilkan Warna Akurat." },
  { "en": "Apa Nilai CRI (Color Rendering Index) Ideal?", "id": "Seratus (100)." },
  { "en": "Apa Itu Efikasi Luminous?", "id": "Rasio Fluks Luminous Terhadap Daya Listrik." },
  { "en": "Apa Satuan Efikasi Luminous?", "id": "Lumen Per Watt (Lm/W)." },
  { "en": "Apa Itu Lumen?", "id": "Satuan Fluks Luminous (Kecerahan Tampak)." },
  { "en": "Apa Itu Lux?", "id": "Satuan Iluminansi (Lumen Per Meter Persegi)." },
  { "en": "Apa Itu Kandela?", "id": "Satuan Intensitas Luminous." },
  { "en": "Apa Itu Fotometri?", "id": "Ilmu Pengukuran Cahaya Terkait Persepsi Mata." },
  { "en": "Apa Itu Radiometri?", "id": "Ilmu Pengukuran Radiasi Elektromagnetik." },
  { "en": "Apa Itu Solid-State Lighting (SSL)?", "id": "Pencahayaan Menggunakan LED Atau OLED." },
  { "en": "Apa Itu Driver LED?", "id": "Catu Daya Khusus Untuk Lampu LED." },
  { "en": "Mengapa LED (Light Emitting Diode) Membutuhkan Driver?", "id": "Untuk Mengatur Arus Yang Mengalir." },
  { "en": "Apa Itu Peredupan (Dimming) PWM?", "id": "Mengatur Kecerahan LED Dengan Sinyal PWM." },
  { "en": "Apa Itu Peredupan Analog?", "id": "Mengatur Kecerahan LED Dengan Mengubah Arus." },
  { "en": "Apa Itu Kedipan (Flicker) Cahaya?", "id": "Variasi Cepat Intensitas Cahaya." },
  { "en": "Apa Itu Kontrol Kualitas?", "id": "Proses Memastikan Produk Memenuhi Standar." },
  { "en": "Apa Itu Kontrol Proses Statistik (SPC)?", "id": "Menggunakan Statistik Memantau, Mengontrol Proses." },
  { "en": "Apa Itu Kapabilitas Proses?", "id": "Kemampuan Proses Menghasilkan Output Sesuai Spesifikasi." },
  { "en": "Apa Itu Indeks Kapabilitas Proses (Cpk)?", "id": "Ukuran Kuantitatif Kapabilitas Suatu Proses." },
  { "en": "Apa Itu Batas Kontrol?", "id": "Batas Atas, Bawah Peta Kontrol." },
  { "en": "Apa Itu Penyebab Umum Variasi?", "id": "Variasi Inheren Dalam Suatu Proses." },
  { "en": "Apa Itu Penyebab Khusus Variasi?", "id": "Variasi Tidak Terduga, Dapat Diidentifikasi." },
  { "en": "Apa Itu Pengambilan Sampel Penerimaan?", "id": "Memeriksa Sampel Lot Menentukan Kualitas." },
  { "en": "Apa Itu Kurva Karakteristik Operasi (OC)?", "id": "Menunjukkan Probabilitas Menerima Lot." },
  { "en": "Apa Itu Acceptable Quality Level (AQL)?", "id": "Tingkat Kualitas Rata-rata Terburuk." },
  { "en": "Apa Itu Diagram Pareto?", "id": "Grafik Batang Mengurutkan Penyebab Masalah." },
  { "en": "Apa Itu Prinsip Pareto (Aturan 80/20)?", "id": "80% Masalah Disebabkan 20% Penyebab." },
  { "en": "Apa Itu Diagram Ishikawa (Tulang Ikan)?", "id": "Alat Menganalisis Penyebab Suatu Masalah." },
  { "en": "Apa Itu 5 Whys?", "id": "Teknik Menanyakan 'Mengapa' Berulang Kali." },
  { "en": "Apa Itu Desain Eksperimen (DoE)?", "id": "Pendekatan Sistematis Merencanakan Eksperimen." },
  { "en": "Apa Itu Desain Faktorial?", "id": "Mempelajari Efek Beberapa Faktor Sekaligus." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak?", "id": "Aplikasi Prinsip Rekayasa Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Metodologi Air Terjun (Waterfall)?", "id": "Model Pengembangan Perangkat Lunak Sekuensial." },
  { "en": "Apa Itu Metodologi Agile?", "id": "Pendekatan Iteratif Dan Inkremental." },
  { "en": "Apa Itu Sprint Dalam Scrum?", "id": "Iterasi Waktu Tetap (1-4 Minggu)." },
  { "en": "Apa Itu Product Backlog?", "id": "Daftar Prioritas Fitur Untuk Produk." },
  { "en": "Apa Itu Sprint Backlog?", "id": "Tugas Dipilih Dari Backlog Produk." },
  { "en": "Apa Itu Daily Scrum (Stand-up)?", "id": "Rapat Harian Singkat Tim." },
  { "en": "Apa Itu Kanban?", "id": "Metode Visualisasi Alur Kerja." },
  { "en": "Apa Itu Batas Work-in-Progress (WIP)?", "id": "Membatasi Jumlah Tugas Aktif Sekaligus." },
  { "en": "Apa Itu Extreme Programming (XP)?", "id": "Metodologi Agile Berfokus Praktik Teknis." },
  { "en": "Apa Itu Pair Programming?", "id": "Dua Programmer Bekerja Bersama." },
  { "en": "Apa Itu Test-Driven Development (TDD)?", "id": "Menulis Tes Otomatis Sebelum Kode." },
  { "en": "Apa Itu Refactoring?", "id": "Memperbaiki Struktur Kode Tanpa Mengubah Fungsi." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Mengintegrasikan Perubahan Kode Secara Teratur." },
  { "en": "Apa Itu Server CI?", "id": "Otomatis Membangun, Menguji Kode Setiap Perubahan." },
  { "en": "Apa Itu Continuous Deployment (CD)?", "id": "Merilis Perubahan Ke Produksi Secara Otomatis." },
  { "en": "Apa Itu Arsitektur Monolitik?", "id": "Aplikasi Dibangun Sebagai Satu Unit Tunggal." },
  { "en": "Apa Itu Arsitektur Microservices?", "id": "Aplikasi Kumpulan Layanan Kecil, Independen." },
  { "en": "Apa Itu API Gateway?", "id": "Titik Masuk Tunggal Untuk Semua Klien." },
  { "en": "Apa Itu Service Discovery?", "id": "Cara Layanan Saling Menemukan Lokasi." },
  { "en": "Apa Itu Circuit Breaker Pattern?", "id": "Mencegah Kegagalan Bertingkat Dalam Microservices." },
  { "en": "Apa Itu Idempotency?", "id": "Operasi Dapat Diulang Tanpa Efek Samping." },
  { "en": "Apa Itu REST (Representational State Transfer)?", "id": "Gaya Arsitektur Untuk API (Application Programming Interface) Web." },
  { "en": "Apa Itu GraphQL?", "id": "Bahasa Kueri Untuk API." },
  { "en": "Apa Itu gRPC?", "id": "Kerangka Kerja RPC (Remote Procedure Call) Kinerja Tinggi." }



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
