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


  { "en": "Apa Satuan Tegangan Listrik?", "id": "Volt (V)." },
  { "en": "Apa Satuan Arus Listrik?", "id": "Ampere (A)." },
  { "en": "Apa Satuan Resistansi Listrik?", "id": "Ohm (Ω)." },
  { "en": "Siapa Penemu Hukum Ohm?", "id": "Georg Simon Ohm." },
  { "en": "Apa Bunyi Hukum Ohm?", "id": "Tegangan Sebanding Dengan Arus Dan Resistansi." },
  { "en": "Rumus Matematis Hukum Ohm?", "id": "V Sama Dengan I Kali R (V=IR)." },
  { "en": "Apa Itu Tegangan (Voltage)?", "id": "Beda Potensial Listrik Antara Dua Titik." },
  { "en": "Tegangan Juga Disebut?", "id": "Gaya Gerak Listrik (GGL)." },
  { "en": "Apa Itu Arus (Current)?", "id": "Aliran Muatan Listrik Dalam Suatu Rangkaian." },
  { "en": "Arus Mengalir Dari Potensial?", "id": "Tinggi Ke Rendah." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Terhadap Aliran Arus Listrik." },
  { "en": "Apa Itu KCL?", "id": "Hukum Arus Kirchhoff (Kirchhoff's Current Law)." },
  { "en": "Apa Bunyi KCL?", "id": "Jumlah Arus Masuk Simpul Sama Dengan Keluar." },
  { "en": "KCL Berbasis Hukum Apa?", "id": "Hukum Kekekalan Muatan." },
  { "en": "Apa Itu Simpul (Node)?", "id": "Titik Percabangan Dalam Rangkaian." },
  { "en": "Apa Itu KVL?", "id": "Hukum Tegangan Kirchhoff (Kirchhoff's Voltage Law)." },
  { "en": "Apa Bunyi KVL?", "id": "Jumlah Tegangan Dalam Loop Tertutup Adalah Nol." },
  { "en": "KVL Berbasis Hukum Apa?", "id": "Hukum Kekekalan Energi." },
  { "en": "Apa Itu Loop?", "id": "Lintasan Tertutup Dalam Suatu Rangkaian." },
  { "en": "Alat Ukur Tegangan?", "id": "Voltmeter." },
  { "en": "Alat Ukur Arus?", "id": "Amperemeter." },
  { "en": "Alat Ukur Resistansi?", "id": "Ohmmeter." },
  { "en": "Bagaimana Voltmeter Dipasang?", "id": "Dipasang Secara Paralel Dengan Komponen." },
  { "en": "Bagaimana Amperemeter Dipasang?", "id": "Dipasang Secara Seri Dalam Rangkaian." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Ujung Ke Ujung." },
  { "en": "Bagaimana Arus Di Rangkaian Seri?", "id": "Sama Di Setiap Titik Rangkaian." },
  { "en": "Bagaimana Tegangan Di Rangkaian Seri?", "id": "Terbagi Di Setiap Komponen." },
  { "en": "Analisis Rangkaian Seri Menggunakan?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Terhubung Di Dua Simpul Yang Sama." },
  { "en": "Bagaimana Tegangan Di Rangkaian Paralel?", "id": "Sama Di Setiap Cabang." },
  { "en": "Bagaimana Arus Di Rangkaian Paralel?", "id": "Terbagi Di Setiap Cabang." },
  { "en": "Analisis Rangkaian Paralel Menggunakan?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "Apa Itu Arus Searah (DC)?", "id": "Arus Yang Mengalir Dalam Satu Arah." },
  { "en": "Contoh Sumber Arus DC?", "id": "Baterai Dan Adaptor." },
  { "en": "Apa Itu Arus Bolak-Balik (AC)?", "id": "Arus Yang Arahnya Berubah Periodik." },
  { "en": "Contoh Sumber Arus AC?", "id": "Stop Kontak Listrik PLN." },
  { "en": "Apa Itu Daya Listrik?", "id": "Laju Energi Listrik Yang Digunakan." },
  { "en": "Apa Satuan Daya Listrik?", "id": "Watt (W)." },
  { "en": "Rumus Daya Listrik?", "id": "Daya Sama Dengan Tegangan Kali Arus (P=VI)." },
  { "en": "Rumus Daya Lain?", "id": "P Sama Dengan I Kuadrat Kali R." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Yang Mudah Menghantarkan Listrik." },
  { "en": "Contoh Konduktor?", "id": "Tembaga Perak Dan Aluminium." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Yang Sulit Menghantarkan Listrik." },
  { "en": "Contoh Isolator?", "id": "Karet Plastik Dan Kaca." },
  { "en": "Apa Itu Semikonduktor?", "id": "Bahan Di Antara Konduktor Dan Isolator." },
  { "en": "Contoh Semikonduktor?", "id": "Silikon Dan Germanium." },
  { "en": "Apa Itu Sumber Tegangan?", "id": "Perangkat Yang Menghasilkan Beda Potensial." },
  { "en": "Apa Itu Beban (Load)?", "id": "Komponen Yang Menggunakan Energi Listrik." },
  { "en": "Contoh Beban?", "id": "Lampu Motor Pemanas." },
  { "en": "Apa Itu Sirkuit Terbuka?", "id": "Rangkaian Terputus Arus Tidak Mengalir." },
  { "en": "Resistansi Sirkuit Terbuka?", "id": "Sangat Besar Atau Tak Terhingga." },
  { "en": "Apa Itu Hubungan Singkat?", "id": "Jalur Resistansi Sangat Rendah." },
  { "en": "Apa Akibat Hubungan Singkat?", "id": "Arus Sangat Besar Dan Berbahaya." },
  { "en": "Apa Itu Ground?", "id": "Titik Referensi Dengan Tegangan Nol." },
  { "en": "Fungsi Ground?", "id": "Untuk Keamanan Dan Stabilitas Rangkaian." },
  { "en": "Apa Itu Penurunan Tegangan (Voltage Drop)?", "id": "Tegangan Hilang Saat Melewati Resistor." },
  { "en": "Penurunan Tegangan Sesuai Hukum?", "id": "Hukum Ohm (V=IR)." },
  { "en": "Apa Itu Kenaikan Tegangan (Voltage Rise)?", "id": "Tegangan Diberikan Oleh Sumber." },
  { "en": "Arah Arus Konvensional?", "id": "Dari Terminal Positif Ke Negatif." },
  { "en": "Arah Aliran Elektron?", "id": "Dari Terminal Negatif Ke Positif." },
  { "en": "Yang Digunakan Dalam Analisis Rangkaian?", "id": "Arah Arus Konvensional." },
  { "en": "Resistansi Total Rangkaian Seri?", "id": "Jumlah Dari Semua Resistansi." },
  { "en": "Resistansi Total Rangkaian Paralel?", "id": "Kebalikan Dari Jumlah Semua Kebalikan." },
  { "en": "Apa Itu Konduktansi?", "id": "Kemudahan Arus Mengalir." },
  { "en": "Konduktansi Adalah Kebalikan Dari?", "id": "Resistansi." },
  { "en": "Apa Satuan Konduktansi?", "id": "Siemens (S)." },
  { "en": "Apa Itu Sumber Tegangan Ideal?", "id": "Memberikan Tegangan Konstan Tanpa Batas." },
  { "en": "Resistansi Internalnya Berapa?", "id": "Nol." },
  { "en": "Apa Itu Sumber Arus Ideal?", "id": "Memberikan Arus Konstan Tanpa Batas." },
  { "en": "Resistansi Internalnya Berapa?", "id": "Tak Terhingga." },
  { "en": "Apa Itu Aturan Pembagi Tegangan?", "id": "Menghitung Tegangan Di Komponen Seri." },
  { "en": "Aturan Ini Berasal Dari?", "id": "Hukum Ohm Dan KVL." },
  { "en": "Apa Itu Aturan Pembagi Arus?", "id": "Menghitung Arus Di Cabang Paralel." },
  { "en": "Aturan Ini Berasal Dari?", "id": "Hukum Ohm Dan KCL." },
  { "en": "Tegangan Diukur Secara?", "id": "Paralel." },
  { "en": "Arus Diukur Secara?", "id": "Seri." },
  { "en": "Apa Itu Rangkaian Linier?", "id": "Rangkaian Dengan Respon Proporsional." },
  { "en": "Hukum Ohm Berlaku Untuk Komponen?", "id": "Linier Seperti Resistor." },
  { "en": "Contoh Komponen Non-Linier?", "id": "Dioda." },
  { "en": "Apa Itu Cabang Rangkaian?", "id": "Jalur Antara Dua Simpul." },
  { "en": "KCL Adalah Hukum Simpul?", "id": "Ya Benar Sekali." },
  { "en": "KVL Adalah Hukum Loop?", "id": "Ya Benar Sekali." },
  { "en": "Apa Itu Arus Bocor?", "id": "Arus Kecil Yang Mengalir Melalui Isolator." },
  { "en": "Apa Itu Efek Joule?", "id": "Pemanasan Konduktor Akibat Aliran Arus." },
  { "en": "Energi Yang Hilang Menjadi?", "id": "Energi Panas." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Yang Berfungsi Pembagi Tegangan." },
  { "en": "Apa Itu Rheostat?", "id": "Resistor Variabel Untuk Mengontrol Arus." },
  { "en": "Hukum Kirchhoff Bekerja Untuk AC/DC?", "id": "Berlaku Untuk Keduanya." },
  { "en": "Untuk Rangkaian AC Kita Menggunakan?", "id": "Nilai RMS Atau Fasor." },
  { "en": "Apa Itu Nilai RMS?", "id": "Nilai Efektif Dari Tegangan Arus AC." },
  { "en": "Apa Itu Sirkuit?", "id": "Lintasan Tertutup Bagi Arus Listrik." },
  { "en": "Tegangan Menyebabkan Apa?", "id": "Arus Mengalir Jika Ada Jalur." },
  { "en": "Arus Dibatasi Oleh Apa?", "id": "Resistansi Total Dalam Rangkaian." },
  { "en": "Tiga Besaran Ini Saling Terkait?", "id": "Ya Melalui Hukum Ohm." },
  { "en": "Apa Itu Resistivitas?", "id": "Hambatan Jenis Suatu Material." },
  { "en": "Resistansi Bergantung Pada Geometri?", "id": "Ya Panjang Dan Luas Penampang." },
  { "en": "Hukum Kirchhoff Adalah Dasar?", "id": "Untuk Semua Analisis Rangkaian Listrik." },
  { "en": "Apa Beda Tegangan Dan Arus?", "id": "Tegangan Dorongan Arus Adalah Aliran." },
  { "en": "Apa Beda Resistansi Dan Konduktansi?", "id": "Resistansi Menghambat Konduktansi Membantu Aliran." },
  { "en": "Apa Beda Rangkaian Seri Dan Paralel?", "id": "Seri Satu Jalur Paralel Banyak Jalur." },
  { "en": "Apa Beda Arus DC Dan AC?", "id": "DC Searah AC Bolak-balik." },
  { "en": "Apa Beda Voltmeter Dan Amperemeter?", "id": "Voltmeter Ukur Tegangan Amperemeter Ukur Arus." },
  { "en": "Hukum Apa Yang Menghubungkan V I R?", "id": "Hukum Ohm (V = IR)." },
  { "en": "Hukum Apa Yang Mengatur Arus Di Simpul?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "Hukum Apa Yang Mengatur Tegangan Di Loop?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Satuan Muatan Listrik Adalah?", "id": "Coulomb (C)." },
  { "en": "Arus Adalah Aliran Apa?", "id": "Aliran Muatan Listrik (Elektron)." },
  { "en": "Satu Ampere Sama Dengan?", "id": "Satu Coulomb Per Detik." },
  { "en": "Tegangan Juga Dikenal Sebagai?", "id": "Beda Potensial Atau Gaya Gerak Listrik." },
  { "en": "Satuan Energi Listrik Adalah?", "id": "Joule (J)." },
  { "en": "Satu Volt Sama Dengan?", "id": "Satu Joule Per Coulomb." },
  { "en": "Apa Itu Daya (Power)?", "id": "Laju Penggunaan Energi Listrik." },
  { "en": "Satuan Daya Adalah?", "id": "Watt (W)." },
  { "en": "Satu Watt Sama Dengan?", "id": "Satu Joule Per Detik." },
  { "en": "Rumus Daya Terkait V Dan I?", "id": "P Sama Dengan V Kali I." },
  { "en": "Rumus Daya Terkait I Dan R?", "id": "P Sama Dengan I Kuadrat Kali R." },
  { "en": "Rumus Daya Terkait V Dan R?", "id": "P Sama Dengan V Kuadrat Dibagi R." },
  { "en": "Komponen Yang Menyerap Daya?", "id": "Resistor Atau Beban." },
  { "en": "Komponen Yang Menyuplai Daya?", "id": "Sumber Tegangan Atau Sumber Arus." },
  { "en": "Apa Itu Konvensi Tanda Pasif?", "id": "Arus Masuk Terminal Positif Menyerap Daya." },
  { "en": "Apa Itu Konvensi Tanda Aktif?", "id": "Arus Keluar Terminal Positif Menyuplai Daya." },
  { "en": "Total Daya Dalam Rangkaian?", "id": "Harus Sama Dengan Nol (Kekekalan Daya)." },
  { "en": "Tahanan Voltmeter Ideal?", "id": "Tak Terhingga." },
  { "en": "Kenapa Tahanan Voltmeter Harus Besar?", "id": "Agar Tidak Mengambil Arus Dari Rangkaian." },
  { "en": "Tahanan Amperemeter Ideal?", "id": "Nol." },
  { "en": "Kenapa Tahanan Amperemeter Harus Kecil?", "id": "Agar Tidak Menimbulkan Jatuh Tegangan." },
  { "en": "Arus Total Di Rangkaian Seri?", "id": "Sama Dengan Arus Setiap Komponen." },
  { "en": "Tegangan Total Di Rangkaian Seri?", "id": "Jumlah Tegangan Setiap Komponen." },
  { "en": "Tegangan Total Di Rangkaian Paralel?", "id": "Sama Dengan Tegangan Setiap Cabang." },
  { "en": "Arus Total Di Rangkaian Paralel?", "id": "Jumlah Arus Setiap Cabang." },
  { "en": "Apa Itu Simpul Referensi?", "id": "Titik Dengan Potensial Nol (Ground)." },
  { "en": "Tegangan Diukur Relatif Terhadap?", "id": "Simpul Referensi." },
  { "en": "Apa Itu Kawat Penghubung Ideal?", "id": "Memiliki Resistansi Nol." },
  { "en": "Tegangan Di Sepanjang Kawat Ideal?", "id": "Selalu Sama (Nol Jatuh Tegangan)." },
  { "en": "Apa Itu Saklar Terbuka?", "id": "Seperti Celah Udara Dalam Rangkaian." },
  { "en": "Arus Melalui Saklar Terbuka?", "id": "Nol Ampere." },
  { "en": "Apa Itu Saklar Tertutup?", "id": "Seperti Kawat Ideal Dalam Rangkaian." },
  { "en": "Tegangan Di Saklar Tertutup?", "id": "Nol Volt." },
  { "en": "Jika Resistansi Digandakan Arusnya?", "id": "Menjadi Setengahnya (Jika V Konstan)." },
  { "en": "Jika Tegangan Digandakan Arusnya?", "id": "Menjadi Dua Kali Lipat (Jika R Konstan)." },
  { "en": "Apa Itu Sumber Tegangan Independen?", "id": "Tegangan Tidak Bergantung Pada Rangkaian." },
  { "en": "Apa Itu Sumber Arus Independen?", "id": "Arus Tidak Bergantung Pada Rangkaian." },
  { "en": "Apa Itu Sumber Dependen (Terkendali)?", "id": "Nilainya Dikontrol Oleh Tegangan Arus Lain." },
  { "en": "Hukum Ohm Tetap Berlaku?", "id": "Ya Untuk Resistor." },
  { "en": "KCL Dan KVL Tetap Berlaku?", "id": "Ya Selalu." },
  { "en": "Apa Itu Analisis Simpul?", "id": "Metode Berbasis KCL Mencari Tegangan." },
  { "en": "Apa Itu Analisis Mesh?", "id": "Metode Berbasis KVL Mencari Arus." },
  { "en": "Resistansi Ekuivalen Seri Selalu?", "id": "Lebih Besar Dari Resistor Terbesar." },
  { "en": "Resistansi Ekuivalen Paralel Selalu?", "id": "Lebih Kecil Dari Resistor Terkecil." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Dengan Banyak Elektron Bebas." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Dengan Sedikit Elektron Bebas." },
  { "en": "Apa Yang Menyebabkan Pemanasan Di Resistor?", "id": "Tumbukan Elektron Dengan Atom." },
  { "en": "Energi Listrik Diubah Menjadi?", "id": "Energi Panas." },
  { "en": "Jika Arus Masuk Simpul Adalah 5A?", "id": "Maka Total Arus Keluar Juga 5A." },
  { "en": "Jika Sumber 9V Di Loop?", "id": "Total Jatuh Tegangan Juga Harus 9V." },
  { "en": "Arah Loop KVL Penting?", "id": "Tidak Asalkan Konsisten." },
  { "en": "Arah Asumsi Arus KCL Penting?", "id": "Tidak Hasil Negatif Berarti Arah Terbalik." },
  { "en": "Apa Itu Rangkaian Planar?", "id": "Rangkaian Yang Bisa Digambar Tanpa Persilangan." },
  { "en": "Analisis Mesh Hanya Untuk Rangkaian?", "id": "Planar." },
  { "en": "Analisis Simpul Untuk Rangkaian?", "id": "Planar Dan Non-Planar." },
  { "en": "Apa Itu Superposisi?", "id": "Menganalisis Efek Sumber Satu Per Satu." },
  { "en": "Superposisi Hanya Berlaku Untuk?", "id": "Rangkaian Linier." },
  { "en": "Bagaimana Sumber Tegangan Dimatikan?", "id": "Diganti Dengan Hubungan Singkat." },
  { "en": "Bagaimana Sumber Arus Dimatikan?", "id": "Diganti Dengan Rangkaian Terbuka." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Rangkaian Disetarakan Dengan Sumber Tegangan Seri." },
  { "en": "Apa Itu Teorema Norton?", "id": "Rangkaian Disetarakan Dengan Sumber Arus Paralel." },
  { "en": "Resistansi Thevenin Dan Norton?", "id": "Memiliki Nilai Yang Sama." },
  { "en": "Hukum Ohm Untuk Seluruh Rangkaian?", "id": "Tidak Hanya Untuk Komponen Tunggal." },
  { "en": "Hukum Kirchhoff Untuk Seluruh Rangkaian?", "id": "Ya Untuk Interaksi Antar Komponen." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Yang Menggabungkan Voltmeter Amperemeter Ohmmeter." },
  { "en": "Apa Itu Potensial Listrik?", "id": "Energi Potensial Per Satuan Muatan." },
  { "en": "Tegangan Adalah Perbedaan?", "id": "Potensial Listrik." },
  { "en": "Titik Ground Memiliki Potensial?", "id": "Nol Volt." },
  { "en": "KCL Adalah Generalisasi Dari?", "id": "Aturan Arus Rangkaian Paralel." },
  { "en": "KVL Adalah Generalisasi Dari?", "id": "Aturan Tegangan Rangkaian Seri." },
  { "en": "Apa Itu Resistivitas (Hambatan Jenis)?", "id": "Sifat Intrinsik Material Menahan Arus." },
  { "en": "Resistansi Bergantung Pada Resistivitas Dan?", "id": "Dimensi Fisik (Panjang Luas)." },
  { "en": "Kawat Lebih Panjang Resistansinya?", "id": "Lebih Besar." },
  { "en": "Kawat Lebih Tebal Resistansinya?", "id": "Lebih Kecil." },
  { "en": "Apa Itu Efek Suhu Pada Resistansi?", "id": "Suhu Naik Resistansi Konduktor Naik." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Jutaan Komponen Dalam Satu Chip." },
  { "en": "Hukum Kirchhoff Tetap Berlaku?", "id": "Ya Di Level Mikro." },
  { "en": "Arus Mengalir Melalui Tubuh Manusia?", "id": "Ya Sangat Berbahaya." },
  { "en": "Grounding Melindungi Dengan Cara?", "id": "Memberi Jalur Aman Untuk Arus Bocor." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Komponen Pengaman Yang Meleleh." },
  { "en": "Sekring Melindungi Dari Arus?", "id": "Arus Berlebih." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Untuk Arus Berlebih." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik)?", "id": "Tegangan Yang Dihasilkan Oleh Sumber." },
  { "en": "Apa Itu Tegangan Terminal?", "id": "Tegangan Aktual Di Ujung Sumber." },
  { "en": "Kenapa Tegangan Terminal Berbeda?", "id": "Karena Ada Jatuh Tegangan Internal." },
  { "en": "Hukum Kirchhoff Adalah Fondasi Dari?", "id": "Semua Teori Rangkaian Listrik." },
  { "en": "Apa Beda Hukum Ohm Dan Kirchhoff?", "id": "Ohm Untuk Komponen Kirchhoff Untuk Rangkaian." },
  { "en": "Apa Prinsip Dasar KCL?", "id": "Muatan Listrik Tidak Dapat Diciptakan." },
  { "en": "Apa Prinsip Dasar KVL?", "id": "Energi Listrik Dalam Loop Adalah Kekal." },
  { "en": "Arus Konvensional Mengalir Dari?", "id": "Potensial Tinggi Ke Rendah." },
  { "en": "Aliran Elektron Sebenarnya Mengalir Dari?", "id": "Potensial Rendah Ke Tinggi." },
  { "en": "Mana Yang Digunakan Dalam Analisis Rangkaian?", "id": "Arus Konvensional." },
  { "en": "Apakah Hasilnya Berbeda?", "id": "Tidak Hasil Akhirnya Akan Sama Saja." },
  { "en": "Apa Itu Titik Simpul (Node)?", "id": "Titik Pertemuan Tiga Komponen Atau Lebih." },
  { "en": "Apa Itu Lintasan Tertutup (Loop)?", "id": "Jalur Yang Kembali Ke Titik Awal." },
  { "en": "KCL Adalah Tentang Keseimbangan?", "id": "Arus Di Setiap Titik Simpul." },
  { "en": "KVL Adalah Tentang Keseimbangan?", "id": "Tegangan Di Setiap Lintasan Tertutup." },
  { "en": "Apa Itu Kenaikan Tegangan?", "id": "Saat Melewati Sumber Dari Negatif Ke Positif." },
  { "en": "Apa Itu Penurunan Tegangan?", "id": "Saat Melewati Resistor Searah Arus." },
  { "en": "Menurut KVL Jumlah Kenaikan Tegangan?", "id": "Sama Dengan Jumlah Penurunan Tegangan." },
  { "en": "Menurut KCL Jumlah Arus Masuk?", "id": "Sama Dengan Jumlah Arus Keluar." },
  { "en": "Resistansi Seri Dijumlahkan Berdasarkan?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Konduktansi Paralel Dijumlahkan Berdasarkan?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "Apa Itu Analisis Simpul?", "id": "Menyelesaikan Rangkaian Dengan Menerapkan KCL." },
  { "en": "Apa Yang Dicari Dalam Analisis Simpul?", "id": "Tegangan Di Setiap Simpul." },
  { "en": "Apa Itu Analisis Mesh?", "id": "Menyelesaikan Rangkaian Dengan Menerapkan KVL." },
  { "en": "Apa Yang Dicari Dalam Analisis Mesh?", "id": "Arus Fiktif Di Setiap Mesh." },
  { "en": "Apa Itu Ground?", "id": "Titik Referensi Tegangan Nol." },
  { "en": "Tegangan Adalah Besaran Absolut?", "id": "Tidak Tegangan Adalah Besaran Relatif." },
  { "en": "Arus Adalah Besaran Absolut?", "id": "Ya Arus Adalah Aliran Aktual." },
  { "en": "Apa Itu Sumber Tegangan Ideal?", "id": "Memiliki Resistansi Internal Nol." },
  { "en": "Apa Itu Sumber Arus Ideal?", "id": "Memiliki Resistansi Internal Tak Terhingga." },
  { "en": "Sumber Nyata Selalu Punya?", "id": "Resistansi Internal." },
  { "en": "Resistansi Internal Menyebabkan?", "id": "Penurunan Tegangan Di Terminal." },
  { "en": "Apa Itu Rangkaian Ekuivalen?", "id": "Rangkaian Sederhana Yang Berperilaku Sama." },
  { "en": "Teorema Thevenin Menghasilkan Ekuivalen?", "id": "Sumber Tegangan Dan Resistor Seri." },
  { "en": "Teorema Norton Menghasilkan Ekuivalen?", "id": "Sumber Arus Dan Resistor Paralel." },
  { "en": "Keduanya Adalah Konsekuensi Dari?", "id": "Hukum Kirchhoff Dan Sifat Linearitas." },
  { "en": "Hukum Ohm (V=IR) Menghubungkan?", "id": "Tegangan Arus Dan Resistansi." },
  { "en": "Daya (P=VI) Menghubungkan?", "id": "Daya Tegangan Dan Arus." },
  { "en": "Apa Itu Elemen Pasif?", "id": "Komponen Yang Hanya Menyerap Energi." },
  { "en": "Contoh Elemen Pasif?", "id": "Resistor Induktor Kapasitor." },
  { "en": "Apa Itu Elemen Aktif?", "id": "Komponen Yang Bisa Menyuplai Energi." },
  { "en": "Contoh Elemen Aktif?", "id": "Baterai Sumber Tegangan Sumber Arus." },
  { "en": "Total Daya Diserap Harus?", "id": "Sama Dengan Total Daya Disuplai." },
  { "en": "Prinsip Ini Disebut?", "id": "Keseimbangan Daya Atau Konservasi Daya." },
  { "en": "Keseimbangan Daya Berasal Dari?", "id": "Hukum Kekekalan Energi (KVL)." },
  { "en": "Apa Itu Arus Mesh?", "id": "Arus Hipotetis Yang Mengalir Dalam Loop." },
  { "en": "Arus Cabang Sebenarnya Adalah?", "id": "Penjumlahan Atau Pengurangan Arus Mesh." },
  { "en": "Apa Itu Tegangan Simpul?", "id": "Potensial Simpul Relatif Terhadap Ground." },
  { "en": "Tegangan Antara Dua Simpul?", "id": "Adalah Perbedaan Tegangan Simpul Keduanya." },
  { "en": "Apa Itu Superposisi?", "id": "Efek Total Adalah Jumlah Efek Individual." },
  { "en": "Superposisi Berlaku Jika Rangkaian?", "id": "Linier." },
  { "en": "Cara Mematikan Sumber Tegangan?", "id": "Menggantinya Dengan Hubungan Singkat." },
  { "en": "Cara Mematikan Sumber Arus?", "id": "Menggantinya Dengan Rangkaian Terbuka." },
  { "en": "Apakah Hukum Kirchhoff Terbatas Pada DC?", "id": "Tidak Berlaku Juga Untuk AC." },
  { "en": "Untuk AC Besaran Diubah Menjadi?", "id": "Fasor (Bilangan Kompleks)." },
  { "en": "Resistansi Diubah Menjadi?", "id": "Impedansi." },
  { "en": "KCL Untuk Fasor Arus?", "id": "Jumlah Fasor Arus Di Simpul Nol." },
  { "en": "KVL Untuk Fasor Tegangan?", "id": "Jumlah Fasor Tegangan Di Loop Nol." },
  { "en": "Arus Adalah Laju Perubahan?", "id": "Muatan (dq/dt)." },
  { "en": "KCL Memastikan Bahwa Muatan?", "id": "Tidak Berakumulasi Di Simpul." },
  { "en": "KVL Memastikan Medan Listrik Statis?", "id": "Adalah Medan Konservatif." },
  { "en": "Artinya Integral Lintasan Tertutupnya?", "id": "Selalu Nol." },
  { "en": "Apa Itu Cabang Dalam Rangkaian?", "id": "Representasi Untuk Satu Komponen Rangkaian." },
  { "en": "KCL Diterapkan Pada?", "id": "Simpul." },
  { "en": "KVL Diterapkan Pada?", "id": "Loop." },
  { "en": "Hukum Ohm Diterapkan Pada?", "id": "Cabang." },
  { "en": "Ketiganya Adalah Tiga Serangkai?", "id": "Analisis Rangkaian Listrik." },
  { "en": "Rangkaian Jembatan Digunakan Untuk?", "id": "Pengukuran Presisi." },
  { "en": "Apa Itu Kondisi Jembatan Setimbang?", "id": "Arus Melalui Detektor Adalah Nol." },
  { "en": "Ini Implikasi Dari Hukum Apa?", "id": "KCL Dan KVL." },
  { "en": "Apa Itu Sumber Terkendali Tegangan?", "id": "VCVS (Voltage-Controlled Voltage Source)." },
  { "en": "Apa Itu Sumber Terkendali Arus?", "id": "CCCS (Current-Controlled Current Source)." },
  { "en": "Apakah Hukum Kirchhoff Peduli Jenis Sumber?", "id": "Tidak Sama Sekali." },
  { "en": "Hukum Kirchhoff Peduli Pada?", "id": "Koneksi Dan Topologi Rangkaian." },
  { "en": "Apa Itu Model Elemen Terpusat?", "id": "Asumsi Ukuran Rangkaian Jauh Lebih Kecil." },
  { "en": "Dibandingkan Dengan Apa?", "id": "Panjang Gelombang Sinyal." },
  { "en": "Untuk Frekuensi 50/60 Hz Asumsi Ini?", "id": "Sangat Akurat." },
  { "en": "Untuk Frekuensi GigaHertz Asumsi Ini?", "id": "Mulai Tidak Berlaku Lagi." },
  { "en": "Jika Tidak Berlaku Kita Menggunakan?", "id": "Teori Gelombang Dan Jalur Transmisi." },
  { "en": "Apa Itu Arus Konvensional?", "id": "Aliran Muatan Positif Fiktif." },
  { "en": "Kenapa Digunakan?", "id": "Alasan Sejarah Dan Konvensi." },
  { "en": "Semua Hukum Rangkaian Didasarkan Pada?", "id": "Arah Arus Konvensional." },
  { "en": "Arus Searah Di Resistor Menghasilkan?", "id": "Polaritas Tegangan Tertentu." },
  { "en": "Sisi Tempat Arus Masuk Adalah?", "id": "Terminal Tegangan Positif." },
  { "en": "Sisi Tempat Arus Keluar Adalah?", "id": "Terminal Tegangan Negatif." },
  { "en": "Ini Penting Untuk Menerapkan?", "id": "KVL Dengan Benar." },
  { "en": "KCL Tidak Memerlukan Informasi Polaritas?", "id": "Benar Hanya Arah Arus." },
  { "en": "Tegangan Adalah Penyebab?", "id": "Arus Adalah Akibat." },
  { "en": "Resistansi Adalah Sifat?", "id": "Yang Menentang Akibat." },
  { "en": "Apa Itu Rangkaian Ekuivalen Thevenin?", "id": "Sumber Tegangan Seri Dengan Resistor." },
  { "en": "Apa Itu Rangkaian Ekuivalen Norton?", "id": "Sumber Arus Paralel Dengan Resistor." },
  { "en": "Keduanya Dapat Dipertukarkan?", "id": "Ya Melalui Transformasi Sumber." },
  { "en": "Transformasi Sumber Adalah Kombinasi?", "id": "Hukum Ohm Thevenin Dan Norton." },
  { "en": "Apa Itu Titik Referensi Bersama?", "id": "Sama Dengan Ground Atau Simpul Referensi." },
  { "en": "Semua Tegangan Simpul Diukur Terhadap?", "id": "Titik Referensi Bersama." },
  { "en": "Tegangan Ground Didefinisikan Sebagai?", "id": "Nol Volt." },
  { "en": "Apa Itu Arus Sirkulasi?", "id": "Arus Yang Mengalir Dalam Loop." },
  { "en": "Arus Sirkulasi Adalah Konsep?", "id": "Analisis Mesh." },
  { "en": "Jumlah Aljabar Arus Di Simpul Nol?", "id": "Adalah Pernyataan KCL." },
  { "en": "Jumlah Aljabar Tegangan Di Loop Nol?", "id": "Adalah Pernyataan KVL." },
  { "en": "Hukum-hukum Ini Adalah Abstraksi?", "id": "Dari Fenomena Elektromagnetik." },
  { "en": "Apa Beda Sumber Dependen Dan Independen?", "id": "Dependen Dikontrol Independen Tidak." },
  { "en": "Apa Beda Analisis Simpul Dan Superposisi?", "id": "Simpul Satu Analisis Superposisi Banyak." },
  { "en": "Apa Beda Teorema Thevenin Dan Norton?", "id": "Thevenin Sumber Tegangan Norton Sumber Arus." },
  { "en": "Apa Beda Sirkuit Terbuka Dan Tertutup?", "id": "Terbuka Arus Nol Tertutup Tegangan Nol." },
  { "en": "Apa Beda Resistansi Dan Konduktansi?", "id": "Resistansi Menghambat Konduktansi Membantu." },
  { "en": "Hukum Kekekalan Apa Yang Mendasari KCL?", "id": "Hukum Kekekalan Muatan Listrik." },
  { "en": "Hukum Kekekalan Apa Yang Mendasari KVL?", "id": "Hukum Kekekalan Energi." },
  { "en": "KCL Adalah Hukum Tentang Simpul?", "id": "Ya Benar." },
  { "en": "KVL Adalah Hukum Tentang Loop?", "id": "Ya Benar." },
  { "en": "Hukum Ohm Adalah Hukum Tentang Komponen?", "id": "Ya Benar." },
  { "en": "Apa Itu Titik Referensi (Ground)?", "id": "Titik Dengan Potensial Nol Untuk Pengukuran." },
  { "en": "Apakah Arus Mengalir Ke Ground?", "id": "Hanya Jika Ada Jalur Arus Kembali." },
  { "en": "Tegangan Diukur Antara Dua Titik?", "id": "Ya Selalu." },
  { "en": "Arus Diukur Melalui Satu Titik?", "id": "Ya Benar." },
  { "en": "Apa Itu Arus Sirkulasi?", "id": "Arus Yang Hanya Berputar Dalam Loop." },
  { "en": "Arus Sirkulasi Adalah Konsep Dari?", "id": "Analisis Mesh." },
  { "en": "Apa Itu Potensial Simpul?", "id": "Tegangan Relatif Terhadap Ground." },
  { "en": "Potensial Simpul Adalah Konsep Dari?", "id": "Analisis Simpul." },
  { "en": "Di Rangkaian Seri Apa Yang Sama?", "id": "Arus Listrik." },
  { "en": "Di Rangkaian Paralel Apa Yang Sama?", "id": "Tegangan Listrik." },
  { "en": "Resistansi Total Seri Adalah?", "id": "Penjumlahan Semua Resistor." },
  { "en": "Konduktansi Total Paralel Adalah?", "id": "Penjumlahan Semua Konduktansi." },
  { "en": "KCL Digunakan Untuk Menurunkan Rumus?", "id": "Pembagi Arus." },
  { "en": "KVL Digunakan Untuk Menurunkan Rumus?", "id": "Pembagi Tegangan." },
  { "en": "Hukum Kirchhoff Mengasumsikan Kawat?", "id": "Ideal Dengan Resistansi Nol." },
  { "en": "Hukum Kirchhoff Mengasumsikan Sinyal?", "id": "Merambat Secara Seketika." },
  { "en": "Asumsi Ini Gagal Pada Frekuensi?", "id": "Sangat Tinggi." },
  { "en": "Jika Tiga Arus Bertemu Di Simpul?", "id": "Jumlah Aljabarnya Harus Nol." },
  { "en": "Jika Loop Terdiri Dari Sumber Dan Beban?", "id": "Tegangan Sumber Sama Dengan Tegangan Beban." },
  { "en": "Apa Itu Rangkaian Linier?", "id": "Responnya Proporsional Dengan Rangsangan." },
  { "en": "Teorema Superposisi Berlaku Untuk?", "id": "Hanya Rangkaian Linier." },
  { "en": "Bagaimana Cara Mematikan Sumber Tegangan?", "id": "Menggantinya Dengan Kawat (Hubungan Singkat)." },
  { "en": "Bagaimana Cara Mematikan Sumber Arus?", "id": "Menggantinya Dengan Celah (Rangkaian Terbuka)." },
  { "en": "Apa Itu Dualitas?", "id": "Prinsip Kesamaan Bentuk Antara Persamaan." },
  { "en": "Persamaan KCL Dual Dengan?", "id": "Persamaan KVL." },
  { "en": "Analisis Simpul Dual Dengan?", "id": "Analisis Mesh." },
  { "en": "Kapasitor Dual Dengan?", "id": "Induktor." },
  { "en": "Hubungan Singkat Dual Dengan?", "id": "Rangkaian Terbuka." },
  { "en": "Apa Itu Elemen Bilateral?", "id": "Sifatnya Sama Untuk Kedua Arah Arus." },
  { "en": "Resistor Adalah Elemen Bilateral?", "id": "Ya Benar." },
  { "en": "Dioda Adalah Elemen Bilateral?", "id": "Tidak Hanya Menghantar Satu Arah." },
  { "en": "Hukum Kirchhoff Berlaku Untuk Non-Bilateral?", "id": "Ya Tentu Saja." },
  { "en": "Apa Itu Teorema Transfer Daya Maksimum?", "id": "Kondisi Untuk Menyalurkan Daya Paling Efisien." },
  { "en": "Kapan Terjadi?", "id": "Saat Resistansi Beban Sama Dengan Sumber." },
  { "en": "Pembuktiannya Membutuhkan?", "id": "KVL Dan Diferensiasi." },
  { "en": "Apa Itu Efisiensi Transfer Daya?", "id": "Rasio Daya Beban Terhadap Daya Total." },
  { "en": "Saat Transfer Maksimum Efisiensinya?", "id": "Lima Puluh Persen." },
  { "en": "Sisa Energi Hilang Di Mana?", "id": "Di Resistansi Internal Sumber." },
  { "en": "Hukum Arus Kirchhoff Dikenal Sebagai?", "id": "Hukum Pertama Kirchhoff." },
  { "en": "Hukum Tegangan Kirchhoff Dikenal Sebagai?", "id": "Hukum Kedua Kirchhoff." },
  { "en": "KCL Adalah Tentang Simpul?", "id": "Ya Benar." },
  { "en": "KVL Adalah Tentang Loop?", "id": "Ya Benar." },
  { "en": "Apa Itu Persamaan Simpul?", "id": "Persamaan KCL Yang Ditulis Untuk Satu Simpul." },
  { "en": "Apa Itu Persamaan Loop?", "id": "Persamaan KVL Yang Ditulis Untuk Satu Loop." },
  { "en": "Tujuan Analisis Rangkaian Adalah?", "id": "Menemukan Semua Arus Dan Tegangan." },
  { "en": "Apa Itu Resistansi Ekuivalen?", "id": "Satu Resistor Yang Mewakili Seluruh Jaringan." },
  { "en": "Mencari Resistansi Ekuivalen Menggunakan?", "id": "Kombinasi Aturan Seri Dan Paralel." },
  { "en": "Apa Itu Konfigurasi Delta (Pi)?", "id": "Tiga Komponen Terhubung Seperti Segitiga." },
  { "en": "Apa Itu Konfigurasi Bintang (Y)?", "id": "Tiga Komponen Terhubung Ke Satu Titik." },
  { "en": "Apa Itu Transformasi Bintang-Delta?", "id": "Mengubah Satu Konfigurasi Ke Yang Lain." },
  { "en": "Tujuannya Untuk?", "id": "Menyederhanakan Rangkaian Yang Kompleks." },
  { "en": "Dasar Transformasi Ini?", "id": "KCL Dan KVL." },
  { "en": "Apa Itu Polaritas Tegangan?", "id": "Menunjukkan Mana Terminal Positif Dan Negatif." },
  { "en": "Polaritas Penting Untuk Hukum?", "id": "KVL." },
  { "en": "Apa Itu Arah Arus?", "id": "Menunjukkan Arah Aliran Muatan Positif." },
  { "en": "Arah Arus Penting Untuk Hukum?", "id": "KCL." },
  { "en": "Jika Sebuah Rangkaian Punya N Simpul?", "id": "Ada N-1 Persamaan KCL Independen." },
  { "en": "Jika Sebuah Rangkaian Punya M Mesh?", "id": "Ada M Persamaan KVL Independen." },
  { "en": "Apa Itu Rangkaian Jembatan?", "id": "Memiliki Empat Lengan Dalam Bentuk Berlian." },
  { "en": "Jembatan Wheatstone Digunakan Untuk Mengukur?", "id": "Resistansi." },
  { "en": "Jembatan Lainnya Bisa Mengukur?", "id": "Kapasitansi Dan Induktansi." },
  { "en": "Analisis Jembatan Tidak Seimbang Memerlukan?", "id": "KCL Dan KVL Langsung." },
  { "en": "Apa Itu Matriks Insidensi?", "id": "Representasi Matematis Koneksi Simpul-Cabang." },
  { "en": "KCL Bisa Ditulis Dalam Bentuk?", "id": "Bentuk Matriks Menggunakan Matriks Insidensi." },
  { "en": "Apa Itu Matriks Loop?", "id": "Representasi Matematis Loop-Cabang." },
  { "en": "KVL Bisa Ditulis Dalam Bentuk?", "id": "Bentuk Matriks Menggunakan Matriks Loop." },
  { "en": "Pendekatan Ini Berguna Untuk?", "id": "Analisis Rangkaian Otomatis Oleh Komputer." },
  { "en": "Arus Yang Sama Mengalir Dalam Komponen?", "id": "Seri." },
  { "en": "Tegangan Yang Sama Terpasang Pada Komponen?", "id":- "Paralel." },
  { "en": "Total Arus Terbagi Di Antara Komponen?", "id": "Paralel." },
  { "en": "Total Tegangan Terbagi Di Antara Komponen?", "id": "Seri." },
  { "en": "Hukum-hukum Ini Adalah Abstraksi Dari?", "id": "Hukum Elektromagnetisme Maxwell." },
  { "en": "Berlaku Jika Panjang Gelombang Sangat?", "id": "Besar Dibandingkan Ukuran Rangkaian." },
  { "en": "Ini Disebut Juga Asumsi?", "id": "Asumsi Rangkaian Terpusat (Lumped Circuit)." },
  { "en": "Kapan Arus Dianggap Positif?", "id": "Jika Sesuai Dengan Arah Asumsi." },
  { "en": "Kapan Tegangan Dianggap Positif?", "id": "Jika Sesuai Dengan Polaritas Asumsi." },
  { "en": "Apa Itu Teorema Resiprositas?", "id": "Berlaku Untuk Rangkaian Resiprokal." },
  { "en": "Apa Itu Elemen Unilateral?", "id": "Sifat Berbeda Untuk Arah Berlawanan." },
  { "en": "Contoh Elemen Unilateral?", "id": "Dioda Dan Sumber Terkendali." },
  { "en": "Hukum Kirchhoff Berlaku Untuk Unilateral?", "id": "Ya." },
  { "en": "Teorema Superposisi Berlaku?", "id": "Ya Selama Rangkaiannya Linier." },
  { "en": "Apa Itu Jaringan Satu Port?", "id": "Jaringan Dengan Satu Pasang Terminal." },
  { "en": "Resistansi Input Adalah Contoh?", "id": "Karakteristik Jaringan Satu Port." },
  { "en": "Apa Itu Jaringan Dua Port?", "id": "Jaringan Dengan Terminal Input Dan Output." },
  { "en": "Analisisnya Menggunakan Parameter?", "id": "Parameter z y h atau ABCD." },
  { "en": "Parameter Ini Diturunkan Dari?", "id": "KCL Dan KVL." },
  { "en": "KCL/KVL Adalah Alat Paling?", "id": "Mendasar Dalam Teori Rangkaian." },
  { "en": "Apa Beda Arus Konvensional Dan Aliran Elektron?", "id": "Arahnya Saling Berlawanan." },
  { "en": "Apa Beda Sumber Ideal Dan Sumber Nyata?", "id": "Sumber Nyata Punya Resistansi Internal." },
  { "en": "Apa Beda Jembatan Setimbang Dan Tak Setimbang?", "id": "Setimbang Arus Detektor Nol." },
  { "en": "Apa Beda Analisis DC Dan AC?", "id": "DC Konstan AC Berubah-ubah." },
  { "en": "Apa Beda Resistansi Dan Reaktansi?", "id": "Resistansi Riil Reaktansi Imajiner." },
  { "en": "KCL Adalah Tentang Kekekalan?", "id": "Muatan." },
  { "en": "KVL Adalah Tentang Kekekalan?", "id": "Energi." },
  { "en": "Hukum Ohm Mendeskripsikan Sifat?", "id": "Komponen Resistor." },
  { "en": "Hukum Kirchhoff Mendeskripsikan Sifat?", "id": "Topologi Rangkaian." },
  { "en": "Arus Masuk Sebuah Simpul Dianggap?", "id": "Positif (Berdasarkan Konvensi)." },
  { "en": "Arus Keluar Sebuah Simpul Dianggap?", "id": "Negatif (Berdasarkan Konvensi)." },
  { "en": "Jumlah Keduanya Di Simpul?", "id": "Harus Selalu Nol." },
  { "en": "Melewati Sumber Menuju Positif?", "id": "Adalah Kenaikan Tegangan." },
  { "en": "Melewati Resistor Searah Arus?", "id": "Adalah Penurunan Tegangan." },
  { "en": "Jumlah Keduanya Di Loop?", "id": "Harus Selalu Nol." },
  { "en": "Rangkaian Seri Adalah Pembagi?", "id": "Tegangan." },
  { "en": "Rangkaian Paralel Adalah Pembagi?", "id": "Arus." },
  { "en": "Apa Itu Titik Referensi?", "id": "Simpul Dengan Tegangan Didefinisikan Nol." },
  { "en": "Apakah Arus Di Ground Selalu Nol?", "id": "Tidak Jika Ada Jalur Kembali." },
  { "en": "Analisis Simpul Menemukan?", "id": "Tegangan Simpul." },
  { "en": "Analisis Mesh Menemukan?", "id": "Arus Mesh." },
  { "en": "Untuk Menyelesaikan Rangkaian Linier Kita Perlu?", "id": "KCL KVL Dan Hukum Ohm." },
  { "en": "Apa Itu Impedansi?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Impedansi Terdiri Dari?", "id": "Resistansi Dan Reaktansi." },
  { "en": "KCL/KVL Berlaku Untuk Impedansi?", "id": "Ya Tentu Saja." },
  { "en": "Apa Itu Admittansi?", "id": "Kebalikan Dari Impedansi." },
  { "en": "KCL Lebih Mudah Dinyatakan Dengan?", "id": "Admittansi." },
  { "en": "KVL Lebih Mudah Dinyatakan Dengan?", "id": "Impedansi." },
  { "en": "Apa Itu Elemen Terpusat?", "id": "Dimensi Fisiknya Jauh Lebih Kecil." },
  { "en": "Dibandingkan Dengan Apa?", "id": "Panjang Gelombang Sinyal." },
  { "en": "Hukum Kirchhoff Adalah Model?", "id": "Model Elemen Terpusat." },
  { "en": "Apa Itu Sumber Tegangan AC?", "id": "Menghasilkan Tegangan Sinusoidal." },
  { "en": "Contoh Sumber Tegangan AC?", "id": "Generator Atau Stop Kontak." },
  { "en": "Apa Itu Nilai Puncak (Peak Value)?", "id": "Amplitudo Maksimum Gelombang AC." },
  { "en": "Apa Itu Nilai RMS?", "id": "Nilai Efektif Setara DC." },
  { "en": "Hubungan Puncak Dan RMS?", "id": "Puncak Sama Dengan RMS Kali Akar Dua." },
  { "en": "Voltmeter AC Mengukur Nilai?", "id": "Nilai RMS." },
  { "en": "Apa Itu Transfer Daya Maksimum?", "id": "Kondisi Untuk Daya Tersalur Terbesar." },
  { "en": "Terjadi Saat Impedansi Beban?", "id": "Adalah Konjugat Kompleks Sumber." },
  { "en": "Untuk Rangkaian DC?", "id": "Resistansi Beban Sama Dengan Sumber." },
  { "en": "Efisiensi Saat Transfer Daya Maksimum?", "id":- "Lima Puluh Persen." },
  { "en": "Apa Itu Teorema Tellegen?", "id": "Pernyataan Tentang Konservasi Daya." },
  { "en": "Berlaku Untuk Rangkaian Apapun?", "id": "Ya Bahkan Non-Linier Dan Bervariasi Waktu." },
  { "en": "Dasarnya Adalah Hukum?", "id": "Hukum Kirchhoff." },
  { "en": "Apa Itu Arus Fiktif?", "id": "Arus Mesh Yang Digunakan Untuk Analisis." },
  { "en": "Arus Nyata Adalah?", "id": "Arus Cabang." },
  { "en": "Apakah Polaritas Referensi Penting?", "id": "Ya Untuk Menerapkan KVL Dengan Benar." },
  { "en": "Apakah Arah Referensi Penting?", "id": "Ya Untuk Menerapkan KCL Dengan Benar." },
  { "en": "Apa Itu Jaringan Satu Gerbang?", "id": "Sama Dengan Jaringan Satu Port." },
  { "en": "Apa Itu Jaringan Dua Gerbang?", "id": "Sama Dengan Jaringan Dua Port." },
  { "en": "Analisis Jaringan Ini Bergantung Pada?", "id": "Penerapan KCL Dan KVL." },
  { "en": "Apa Itu Parameter h?", "id": "Parameter Hibrida." },
  { "en": "Digunakan Untuk Memodelkan Apa?", "id": "Transistor." },
  { "en": "Dasar Model Transistor?", "id": "Sumber Terkendali." },
  { "en": "Analisis Rangkaian Transistor Menggunakan?", "id": "KCL Dan KVL." },
  { "en": "Apa Itu Garis Beban?", "id": "Garis Yang Merepresentasikan Persamaan KVL." },
  { "en": "Digunakan Dalam Analisis Apa?", "id": "Analisis Grafis Komponen Non-Linier." },
  { "en": "Apa Itu Titik Operasi?", "id": "Perpotongan Garis Beban Dan Kurva Karakteristik." },
  { "en": "Titik Ini Menunjukkan Kondisi?", "id": "Kondisi DC (Bias) Dari Komponen." },
  { "en": "Apa Itu Model Sinyal Kecil?", "id": "Model Linier Dari Komponen Non-Linier." },
  { "en": "Berlaku Di Sekitar Mana?", "id": "Di Sekitar Titik Operasi DC." },
  { "en": "Analisis Sinyal Kecil Menggunakan?", "id": "KCL KVL Dan Teori Rangkaian Linier." },
  { "en": "KCL Berasal Dari Persamaan Maxwell?", "id": "Ya Dari Hukum Gauss Dan Persamaan Kontinuitas." },
  { "en": "KVL Berasal Dari Persamaan Maxwell?", "id": "Ya Dari Hukum Faraday." },
  { "en": "Hukum Kirchhoff Adalah Penyederhanaan?", "id": "Ya Untuk Kondisi Frekuensi Rendah." },
  { "en": "Apa Itu Resistansi Negatif?", "id": "Komponen Aktif Yang Menghasilkan Daya." },
  { "en": "Contoh Komponen Resistansi Negatif?", "id": "Dioda Terowongan (Tunnel Diode)." },
  { "en": "Hukum Kirchhoff Berlaku?", "id": "Ya Tetap Berlaku." },
  { "en": "Apa Itu Dualitas?", "id": "Setiap Konsep Punya Pasangan Dualnya." },
  { "en": "Tegangan Dual Dengan?", "id": "Arus." },
  { "en": "Loop Dual Dengan?", "id": "Simpul." },
  { "en": "KVL Dual Dengan?", "id": "KCL." },
  { "en": "Seri Dual Dengan?", "id": "Paralel." },
  { "en": "Hubungan Singkat Dual Dengan?", "id": "Rangkaian Terbuka." },
  { "en": "Induktor Dual Dengan?", "id": "Kapasitor." },
  { "en": "Apa Itu Daya Sesaat?", "id": "Daya Pada Setiap Momen Waktu." },
  { "en": "Rumusnya Adalah?", "id": "v(t) Dikalikan i(t)." },
  { "en": "Apa Itu Daya Rata-rata?", "id": "Integral Daya Sesaat Selama Satu Periode." },
  { "en": "Untuk Rangkaian AC Daya Bergantung?", "id": "Faktor Daya." },
  { "en": "Apa Itu Faktor Daya?", "id": "Kosinus Sudut Antara Tegangan Arus." },
  { "en": "Faktor Daya Ideal?", "id": "Satu." },
  { "en": "Koreksi Faktor Daya Dilakukan Dengan?", "id": "Menambahkan Kapasitor Atau Induktor." },
  { "en": "Apa Itu Transfer Energi?", "id": "Daya Dikalikan Dengan Waktu." },
  { "en": "Satuan Energi Yang Umum?", "id": "Kilowatt-jam (kWh)." },
  { "en": "Meteran Listrik Mengukur?", "id": "Energi Yang Digunakan." },
  { "en": "Apa Itu Arus Eddy?", "id": "Arus Sirkulasi Dalam Konduktor." },
  { "en": "Arus Eddy Disebabkan Oleh?", "id": "Perubahan Medan Magnet." },
  { "en": "Arus Eddy Mematuhi Hukum?", "id": "Hukum Kirchhoff Lokal." },
  { "en": "Apa Itu Efek Hall?", "id": "Tegangan Muncul Akibat Medan Magnet." },
  { "en": "Tegangan Hall Tegak Lurus?", "id": "Arah Arus Dan Medan Magnet." },
  { "en": "Digunakan Untuk Sensor Apa?", "id": "Sensor Medan Magnet Dan Sensor Arus." },
  { "en": "Apa Itu Termokopel?", "id": "Menghasilkan Tegangan Akibat Perbedaan Suhu." },
  { "en": "Efek Seebeck Adalah Prinsip?", "id": "Dasar Dari Termokopel." },
  { "en": "Hukum Loop Berlaku?", "id": "Ya Hukum Tegangan Termoelektrik." },
  { "en": "Apa Itu Piezoelektrik?", "id": "Menghasilkan Tegangan Akibat Tekanan Mekanis." },
  { "en": "Hukum Kirchhoff Adalah Perkakas Analisis?", "id": "Fundamental Dan Sangat Kuat." },
  { "en": "Apa Beda Arus Dan Muatan?", "id": "Arus Adalah Aliran Muatan." },
  { "en": "Apa Beda Tegangan Dan Energi?", "id": "Tegangan Adalah Energi Per Muatan." },
  { "en": "Apa Beda Daya Dan Energi?", "id": "Daya Adalah Laju Penggunaan Energi." },
  { "en": "Apa Beda Resistor Dan Resistansi?", "id": "Resistor Komponen Resistansi Sifatnya." },
  { "en": "Apa Beda Sumber Dan Beban?", "id": "Sumber Menyuplai Beban Mengonsumsi Energi." },
  { "en": "Hukum Kekekalan Mana Mendasari KCL?", "id": "Hukum Kekekalan Muatan." },
  { "en": "Hukum Kekekalan Mana Mendasari KVL?", "id": "Hukum Kekekalan Energi." },
  { "en": "Apa Esensi Dari Hukum Ohm?", "id": "Hubungan Linier Antara Tegangan Dan Arus." },
  { "en": "Apa Esensi Dari KCL?", "id": "Arus Tidak Bisa Hilang Di Percabangan." },
  { "en": "Apa Esensi Dari KVL?", "id": "Energi Potensial Awal Sama Dengan Akhir." },
  { "en": "Rangkaian Seri Mematuhi Hukum Apa?", "id": "KVL Secara Dominan." },
  { "en": "Rangkaian Paralel Mematuhi Hukum Apa?", "id": "KCL Secara Dominan." },
  { "en": "Apa Itu Titik Simpul?", "id": "Tempat KCL Diterapkan." },
  { "en": "Apa Itu Lintasan Tertutup?", "id": "Tempat KVL Diterapkan." },
  { "en": "Apa Itu Konvensi Tanda Pasif?", "id": "Arus Masuk Terminal Positif Menyerap Daya." },
  { "en": "Penting Untuk Perhitungan Apa?", "id": "Perhitungan Daya Listrik." },
  { "en": "Hasil Perhitungan Arus Negatif Berarti?", "id": "Arah Sebenarnya Berlawanan." },
  { "en": "Arah Referensi Ditentukan Secara?", "id": "Bebas Atau Arbitrer." },
  { "en": "Tegangan Jatuh Terjadi Pada?", "id": "Komponen Pasif Seperti Resistor." },
  { "en": "Tegangan Naik Disediakan Oleh?", "id": "Komponen Aktif Seperti Baterai." },
  { "en": "Voltmeter Ideal Punya Resistansi?", "id": "Tak Terhingga." },
  { "en": "Amperemeter Ideal Punya Resistansi?", "id": "Nol." },
  { "en": "Ohmmeter Bekerja Dengan Mengalirkan?", "id": "Arus Kecil Yang Diketahui." },
  { "en": "Pengukuran Resistansi Harus Dilakukan Saat?", "id": "Rangkaian Dalam Keadaan Mati." },
  { "en": "Apa Itu Analisis Rangkaian?", "id": "Proses Mencari Semua Tegangan Arus." },
  { "en": "Alat Matematis Utamanya Adalah?", "id": "KCL KVL Dan Hukum Ohm." },
  { "en": "Analisis Simpul Adalah Aplikasi Sistematis?", "id": "Dari KCL." },
  { "en": "Analisis Mesh Adalah Aplikasi Sistematis?", "id": "Dari KVL." },
  { "en": "Apa Itu Ground?", "id": "Titik Dengan Potensial Didefinisikan Nol." },
  { "en": "Tegangan Adalah Besaran Relatif?", "id": "Ya Selalu Diukur Antara Dua Titik." },
  { "en": "Arus Adalah Besaran Absolut?", "id": "Ya Aliran Melewati Satu Titik." },
  { "en": "Hukum-hukum Ini Berlaku Untuk AC?", "id": "Ya Dengan Menggunakan Fasor Dan Impedansi." },
  { "en": "Apa Itu Fasor?", "id": "Representasi Bilangan Kompleks Dari Sinusoid." },
  { "en": "Apa Itu Impedansi?", "id": "Generalisasi Resistansi Untuk Rangkaian AC." },
  { "en": "KCL Dan KVL Tetap Sama Bentuknya?", "id": "Ya Dalam Domain Fasor." },
  { "en": "Apa Itu Superposisi?", "id": "Memungkinkan Analisis Sumber Satu Per Satu." },
  { "en": "Berlaku Hanya Untuk Rangkaian?", "id": "Linier." },
  { "en": "Apa Itu Rangkaian Linier?", "id": "Terdiri Dari Komponen Linier." },
  { "en": "Apa Itu Komponen Linier?", "id": "Nilainya Tidak Berubah Dengan Arus Tegangan." },
  { "en": "Resistor Adalah Komponen Linier?", "id": "Ya Idealnya." },
  { "en": "Dioda Adalah Komponen Linier?", "id": "Tidak Dioda Adalah Non-Linier." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus." },
  { "en": "Keduanya Adalah Rangkaian Ekuivalen?", "id": "Ya Dilihat Dari Dua Terminal." },
  { "en": "Apa Itu Daya Rata-rata?", "id": "Daya Sebenarnya Yang Dihabiskan." },
  { "en": "Apa Itu Daya Reaktif?", "id": "Daya Yang Disimpan Dikembalikan." },
  { "en": "Apa Itu Daya Kompleks?", "id": "Menggabungkan Daya Rata-rata Dan Reaktif." },
  { "en": "Konservasi Daya Kompleks?", "id": "Ya Tetap Berlaku." },
  { "en": "KCL Adalah Tentang Aliran?", "id": "Ya Aliran Muatan." },
  { "en": "KVL Adalah Tentang Potensial?", "id": "Ya Perbedaan Potensial." },
  { "en": "Keseimbangan Daya Adalah Konsekuensi?", "id": "Langsung Dari KCL Dan KVL." },
  { "en": "Apa Itu Sumber Terkendali?", "id": "Nilainya Bergantung Pada Besaran Lain." },
  { "en": "Contoh Model Yang Menggunakan Sumber Terkendali?", "id": "Model Sinyal Kecil Transistor." },
  { "en": "Analisisnya Tetap Menggunakan?", "id": "KCL Dan KVL." },
  { "en": "Apa Itu Model Elemen Terpusat?", "id": "Asumsi Sinyal Merambat Seketika." },
  { "en": "Kapan Model Ini Gagal?", "id": "Pada Frekuensi Sangat Tinggi." },
  { "en": "Saat Gagal Kita Menggunakan?", "id": "Teori Jalur Transmisi Dan Gelombang." },
  { "en": "Hukum Kirchhoff Adalah Aproksimasi?", "id": "Frekuensi Rendah Dari Persamaan Maxwell." },
  { "en": "Untuk Elektronika Sehari-hari Aproksimasinya?", "id": "Sangat Akurat Dan Andal." },
  { "en": "Apa Itu Arus Bocor Tanah?", "id": "Arus Mengalir Ke Ground Saat Ada Gangguan." },
  { "en": "Sistem Grounding Melindungi Dengan?", "id": "Memberikan Jalur Aman Untuk Arus Ini." },
  { "en": "Apa Itu Tiga Hukum Dasar Rangkaian?", "id": "Hukum Ohm KCL Dan KVL." },
  { "en": "Ketiganya Cukup Untuk Menyelesaikan?", "id": "Hampir Semua Rangkaian Linier." },
  { "en": "Apa Itu Dualitas Rangkaian?", "id": "KCL Di Satu Rangkaian Mirip KVL Dualnya." },
  { "en": "Apa Itu Jembatan Setimbang?", "id": "Kondisi Arus Nol Di Detektor." },
  { "en": "Dapat Digunakan Untuk Mengukur?", "id": "Nilai Komponen Yang Tidak Diketahui." },
  { "en": "Apa Itu Potensial Mengambang (Floating)?", "id": "Simpul Yang Tidak Terhubung Ke Referensi." },
  { "en": "Tegangannya Sulit Didefinisikan?", "id": "Ya Betul." },
  { "en": "Apa Itu Arus Searah?", "id": "DC (Direct Current)." },
  { "en": "Apa Itu Arus Bolak-balik?", "id": "AC (Alternating Current)." },
  { "en": "Hukum Kirchhoff Bekerja Untuk Keduanya?", "id": "Ya." },
  { "en": "Apa Itu Jaringan Listrik?", "id": "Interkoneksi Kompleks Dari Komponen Listrik." },
  { "en": "Analisis Jaringan Bergantung Pada?", "id": "Penerapan KCL Dan KVL Secara Sistematis." },
  { "en": "Apa Itu Matriks Admittansi?", "id": "Representasi Matriks Dari Persamaan KCL." },
  { "en": "Apa Itu Matriks Impedansi?", "id": "Representasi Matriks Dari Persamaan KVL." },
  { "en": "Metode Ini Berguna Untuk?", "id": "Simulasi Rangkaian Menggunakan Komputer." },
  { "en": "Tegangan Total Pada Komponen Seri?", "id": "Adalah Jumlah Masing-masing Tegangan." },
  { "en": "Arus Total Pada Komponen Paralel?", "id": "Adalah Jumlah Masing-masing Arus." },
  { "en": "Hukum Mana Yang Menjelaskan Ini?", "id": "KVL Untuk Seri KCL Untuk Paralel." },
  { "en": "Apa Itu Elemen Unilateral?", "id": "Hanya Menghantar Dalam Satu Arah." },
  { "en": "Contoh Elemen Unilateral?", "id": "Dioda." },
  { "en": "Apa Itu Elemen Bilateral?", "id": "Menghantar Sama Baik Di Kedua Arah." },
  { "en": "Contoh Elemen Bilateral?", "id": "Resistor." },
  { "en": "KCL/KVL Berlaku Untuk Keduanya?", "id": "Ya Hukum Topologis Tidak Peduli Sifat Komponen." },
  { "en": "Arus Yang Sama Dalam Satu Cabang?", "id": "Ya." },
  { "en": "Tegangan Sama Antara Dua Simpul?", "id": "Ya." },
  { "en": "Apa Itu Konservasi Energi Di Rangkaian?", "id": "Daya Yang Dihasilkan Sama Dengan Yang Dikonsumsi." },
  { "en": "Ini Adalah Konsekuensi Dari?", "id": "Hukum Tegangan Kirchhoff." },
  { "en": "Apa Itu Konservasi Muatan Di Rangkaian?", "id": "Arus Masuk Simpul Sama Dengan Keluar." },
  { "en": "Ini Adalah Pernyataan Dari?", "id": "Hukum Arus Kirchhoff." },
  { "en": "Rangkaian Seri Adalah Pembagi Tegangan?", "id": "Ya Betul." },
  { "en": "Rangkaian Paralel Adalah Pembagi Arus?", "id": "Ya Betul." },
  { "en": "Semua Teori Rangkaian Dibangun Diatas?", "id": "Fondasi Hukum Kirchhoff." },
  { "en": "Memahami KCL Dan KVL Adalah?", "id": "Langkah Paling Penting Dalam Teknik Elektro." },
  { "en": "Apa Beda Konduktor Dan Semikonduktor?", "id": "Konduktor Hantarkan Semikonduktor Bisa Diatur." },
  { "en": "Apa Beda Kabel Serabut Dan Kabel Litz?", "id": "Serabut Litz Setiap Kawatnya Terisolasi." },
  { "en": "Apa Beda Jaket Kabel Dan Armor Kabel?", "id": "Jaket Lindungi Cuaca Armor Lindungi Fisik." },
  { "en": "Apa Beda Atenuasi Dan Crosstalk?", "id": "Atenuasi Pelemahan Crosstalk Gangguan Antar Pasang." },
  { "en": "Apa Beda Kabel Riser Dan Plenum?", "id": "Beda Peringkat Api Untuk Instalasi Berbeda." },
  { "en": "Apa Itu Kekuatan Mekanis Kabel?", "id": "Kemampuan Kabel Menahan Stres Fisik." },
  { "en": "Contoh Stres Fisik?", "id": "Tarikan Tekanan Dan Tekukan." },
  { "en": "Apa Itu Konduktor Tembaga Kaleng?", "id": "Tembaga Yang Dilapisi Dengan Lapisan Timah." },
  { "en": "Apa Keuntungan Tembaga Kaleng?", "id": "Tahan Korosi Dan Sangat Mudah Disolder." },
  { "en": "Apa Itu Kabel Audio Balanced?", "id": "Menggunakan Sinyal Diferensial Untuk Menolak Gangguan." },
  { "en": "Apa Itu Common Mode Rejection Ratio (CMRR)?", "id": "Ukuran Kemampuan Menolak Gangguan." },
  { "en": "CMRR Yang Baik Bernilai?", "id": "Sangat Tinggi." },
  { "en": "Apa Itu Konektor XLR?", "id": "Konektor Standar Untuk Audio Balanced Profesional." },
  { "en": "Apa Itu Konektor TRS?", "id": "Konektor Jack Stereo Untuk Audio Balanced." },
  { "en": "TRS Singkatan Dari?", "id": "Tip Ring Sleeve." },
  { "en": "Apa Itu Kabel Instrumentasi?", "id": "Kabel Berpelindung Untuk Sinyal Pengukuran Sensitif." },
  { "en": "Kenapa Pelindung Sangat Penting?", "id": "Agar Sinyal Pengukuran Tidak Terganggu." },
  { "en": "Apa Itu Loop Arus 4-20 mA?", "id": "Standar Sinyal Analog Industri." },
  { "en": "Kelebihan Loop Arus?", "id": "Sangat Tahan Terhadap Gangguan Tegangan." },
  { "en": "Kenapa Dimulai Dari 4 mA?", "id": "Untuk Mendeteksi Kabel Putus (0 mA)." },
  { "en": "Apa Itu Kabel Tahan Torsi?", "id": "Kabel Didesain Untuk Tahan Gerakan Memuntir." },
  { "en": "Digunakan Pada Aplikasi Apa?", "id": "Robot Industri Dan Turbin Angin." },
  { "en": "Apa Itu Kabel Kontrol?", "id": "Kabel Multi-Inti Untuk Sirkuit Kontrol." },
  { "en": "Apa Itu Kabel Tray Solid Bottom?", "id": "Kabel Tray Tanpa Lubang Ventilasi." },
  { "en": "Untuk Apa Digunakan?", "id": "Kabel Sinyal Sensitif." },
  { "en": "Apa Itu Kabel Tray Wire Mesh?", "id": "Kabel Tray Terbuat Dari Jaring Kawat." },
  { "en": "Keuntungannya Apa?", "id": "Sangat Fleksibel Dan Mudah Dipasang." },
  { "en": "Apa Itu Firestop?", "id": "Material Penyegel Tahan Api." },
  { "en": "Untuk Apa Firestop Digunakan?", "id": "Menutup Tembusan Kabel Di Dinding Tahan Api." },
  { "en": "Tujuannya Mencegah Penyebaran?", "id": "Api Dan Asap Antar Ruangan." },
  { "en": "Apa Itu Kabel Koaksial 50 Ohm?", "id": "Standar Untuk Komunikasi Data Dan Radio." },
  { "en": "Apa Itu Kabel Koaksial 75 Ohm?", "id": "Standar Untuk Video Dan Televisi." },
  { "en": "Mencampur Keduanya Menyebabkan Apa?", "id": "Ketidakcocokan Impedansi Dan Pantulan Sinyal." },
  { "en": "Apa Itu Velocity Factor (VF)?", "id": "Kecepatan Sinyal Dibanding Kecepatan Cahaya." },
  { "en": "Nilai VF Selalu?", "id": "Kurang Dari Satu." },
  { "en": "VF Bergantung Pada Apa?", "id": "Konstanta Dielektrik Bahan Isolator." },
  { "en": "Isolator Busa Punya VF Lebih Tinggi?", "id": "Ya Dibandingkan Isolator Padat." },
  { "en": "Apa Itu Kabel Tahan Radiasi?", "id": "Kabel Untuk Digunakan Di Lingkungan Nuklir." },
  { "en": "Apa Itu Kabel Cryogenic?", "id": "Kabel Untuk Suhu Sangat Rendah." },
  { "en": "Bisa Menjadi Superkonduktor?", "id": "Ya Beberapa Jenisnya." },
  { "en": "Apa Itu Superkonduktor Suhu Tinggi?", "id": "Superkonduktor Di Atas Suhu Nitrogen Cair." },
  { "en": "Apa Itu Peringkat Suhu Kabel?", "id": "Batas Suhu Aman Operasi Berkelanjutan." },
  { "en": "Contoh Peringkat Suhu?", "id": "60 75 Atau 90 Derajat Celcius." },
  { "en": "Peringkat Lebih Tinggi Memungkinkan Arus?", "id": "Lebih Tinggi Untuk Ukuran Kabel Sama." },
  { "en": "Apa Itu Kabel Penguburan Langsung?", "id": "Kabel Yang Bisa Dikubur Tanpa Pipa." },
  { "en": "Biasanya Memiliki Perlindungan Apa?", "id": "Selubung Sangat Kuat Atau Armor." },
  { "en": "Apa Itu Kabel Service Entrance?", "id": "Kabel Dari Jaringan Listrik Ke Meteran." },
  { "en": "Apa Itu Kabel Feeder?", "id": "Kabel Dari Meteran Ke Panel Distribusi." },
  { "en": "Apa Itu Kabel Cabang (Branch Circuit)?", "id": "Kabel Dari Panel Ke Stop Kontak." },
  { "en": "Apa Itu Kabel Welding?", "id": "Sama Dengan Kabel Las." },
  { "en": "Apa Itu Kabel Audio Digital?", "id": "Membawa Sinyal Audio Sebagai Data Biner." },
  { "en": "Contohnya Apa?", "id": "Kabel Toslink Atau Kabel AES/EBU." },
  { "en": "Apa Itu Kabel Audio Analog?", "id": "Membawa Sinyal Audio Sebagai Gelombang Kontinu." },
  { "en": "Contohnya Apa?", "id": "Kabel RCA Atau Kabel Speaker." },
  { "en": "Apa Itu Kode Warna Serat Optik?", "id": "Standar Warna Untuk Mengidentifikasi Serat Individual." },
  { "en": "Apa Itu Konektor SC Fiber?", "id": "Konektor Optik Dengan Mekanisme Push-Pull." },
  { "en": "Apa Itu Konektor LC Fiber?", "id": "Konektor Optik Berukuran Kecil." },
  { "en": "Apa Itu Konektor ST Fiber?", "id": "Konektor Optik Dengan Mekanisme Bayonet." },
  { "en": "Ujung Fiber Dipoles Untuk Apa?", "id": "Mengurangi Pantulan Dan Kehilangan Sinyal." },
  { "en": "Apa Itu Polish UPC?", "id": "Ultra Physical Contact." },
  { "en": "Apa Itu Polish APC?", "id": "Angled Physical Contact." },
  { "en": "Mana Yang Pantulannya Lebih Rendah?", "id": "APC." },
  { "en": "Warna Konektor APC Biasanya?", "id": "Hijau." },
  { "en": "Warna Konektor UPC Biasanya?", "id": "Biru." },
  { "en": "Apa Itu Attenuator Fiber Optik?", "id": "Komponen Untuk Melemahkan Sinyal Cahaya." },
  { "en": "Apa Itu Amplifier Fiber Optik?", "id": "Komponen Untuk Menguatkan Sinyal Cahaya." },
  { "en": "Contoh Amplifier Optik?", "id": "EDFA (Erbium Doped Fiber Amplifier)." },
  { "en": "Apa Itu Konduktor Terdampar?", "id": "Konduktor Terdiri Dari Banyak Helai Kawat." },
  { "en": "Apa Itu Konduktor Padat?", "id": "Konduktor Terdiri Dari Satu Kawat." },
  { "en": "Mana Yang Lebih Tahan Korosi?", "id": "Konduktor Padat." },
  { "en": "Mana Yang Lebih Tahan Putus Akibat Getaran?", "id": "Konduktor Terdampar." },
  { "en": "Apa Itu Sinyal Diferensial?", "id": "Informasi Dibawa Oleh Perbedaan Tegangan." },
  { "en": "Menggunakan Berapa Kawat?", "id": "Dua Kawat." },
  { "en": "Kelebihannya Adalah Penolakan Terhadap?", "id": "Gangguan Mode Bersama (Common Mode Noise)." },
  { "en": "Apa Itu Kabel Bus?", "id": "Satu Kabel Yang Dibagi Oleh Banyak Perangkat." },
  { "en": "Membutuhkan Apa Di Ujungnya?", "id": "Terminator Untuk Mencegah Pantulan." },
  { "en": "Apa Itu Kabel Pendant?", "id": "Kabel Bulat Untuk Kontrol Gantung." },
  { "en": "Apa Itu Kabel Berisolasi Kertas?", "id": "Teknologi Lama Untuk Kabel Bawah Tanah." },
  { "en": "Diimpregnasi Dengan Apa?", "id": "Minyak Isolasi." },
  { "en": "Apa Itu Kabel Self-Regulating?", "id": "Kabel Pemanas Yang Mengatur Suhunya Sendiri." },
  { "en": "Apa Itu Strippability?", "id": "Kemudahan Mengupas Isolasi Tanpa Merusak Konduktor." },
  { "en": "Apa Itu Tangent Delta Test?", "id": "Mengukur Kualitas Dielektrik Isolasi." },
  { "en": "Disebut Juga Uji?", "id": "Uji Faktor Disipasi." },
  { "en": "Nilai Yang Rendah Menunjukkan?", "id": "Isolasi Yang Baik Dan Kering." },
  { "en": "Apa Itu Kabel Fleksibel Kontinu?", "id": "Kabel Untuk Rantai Kabel Bergerak." },
  { "en": "Apa Itu Rantai Kabel (Cable Chain)?", "id": "Pelindung Fleksibel Untuk Kabel Bergerak." },
  { "en": "Digunakan Pada Mesin Apa?", "id": "Mesin CNC Dan Robot Industri." },
  { "en": "Apa Itu Peringkat Tegangan Vrms?", "id": "Volt Root Mean Square." },
  { "en": "Apa Itu Peringkat Tegangan Vdc?", "id": "Volt Direct Current." },
  { "en": "Apa Itu Kabel Tahan Kimia?", "id": "Kabel Dengan Jaket Khusus Tahan Kimia." },
  { "en": "Contoh Jaket Tahan Kimia?", "id": "Teflon (PTFE) Atau Karet Khusus." },
  { "en": "Apa Itu Kabel Layanan (Service Cable)?", "id": "Menghubungkan Jaringan Utilitas Ke Bangunan." },
  { "en": "Apa Itu Kabel Drop?", "id": "Bagian Terakhir Dari Jaringan Ke Pelanggan." },
  { "en": "Apa Itu Kabel Koaksial 93 Ohm?", "id": "Impedansi Khusus Untuk Aplikasi Video Lama." },
  { "en": "Apa Itu Kabel Gitar?", "id": "Kabel Coaxial Unbalanced." },
  { "en": "Kapasitansinya Rendah Atau Tinggi?", "id": "Harus Rendah Untuk Menjaga Frekuensi Tinggi." },
  { "en": "Apa Itu Kabel Mikrofon?", "id": "Biasanya Kabel Balanced Dengan Konektor XLR." },
  { "en": "Apa Beda Resistansi Dan Resistivitas?", "id": "Resistansi Sifat Objek Resistivitas Sifat Material." },
  { "en": "Apa Beda Konduktivitas Dan Konduktansi?", "id": "Konduktivitas Sifat Material Konduktansi Sifat Objek." },
  { "en": "Apa Beda Kabel Serabut Dan Bundled?", "id": "Bundled Beberapa Konduktor Terisolasi." },
  { "en": "Apa Beda Tegangan Tembus Dan Rating?", "id": "Tembus Titik Rusak Rating Batas Aman." },
  { "en": "Apa Beda Stripping Dan Crimping?", "id": "Stripping Mengupas Crimping Memasang." },
  { "en": "Apa Itu Arus DC?", "id": "Arus Searah." },
  { "en": "Apa Itu Arus AC?", "id": "Arus Bolak-balik." },
  { "en": "Skin Effect Terjadi Pada Arus?", "id": "Arus AC." },
  { "en": "Apa Itu Inti Konduktor?", "id": "Bagian Tengah Kabel Yang Menghantarkan Listrik." },
  { "en": "Apa Itu Isolasi Konduktor?", "id": "Lapisan Yang Membungkus Setiap Inti Konduktor." },
  { "en": "Apa Itu Pengisi (Filler)?", "id": "Material Untuk Membuat Kabel Menjadi Bulat." },
  { "en": "Apa Itu Pembungkus (Binder)?", "id": "Pita Untuk Menahan Semua Inti Bersama." },
  { "en": "Apa Itu Selubung Luar?", "id": "Lapisan Terluar Pelindung Kabel." },
  { "en": "Kabel Fleksibel Dikenal Juga Sebagai?", "id": "Kabel Terdampar (Stranded Cable)." },
  { "en": "Kabel Kaku Dikenal Juga Sebagai?", "id": "Kabel Padat (Solid Cable)." },
  { "en": "Apa Itu Kapasitansi Mutual?", "id": "Kapasitansi Antara Dua Konduktor Berbeda." },
  { "en": "Apa Itu Induktansi Mutual?", "id": "Induktansi Antara Dua Konduktor Berbeda." },
  { "en": "Keduanya Adalah Penyebab?", "id": "Crosstalk Antar Pasangan Kabel." },
  { "en": "Bagaimana Pilinan Kawat Bekerja?", "id": "Medan Magnet Yang Dihasilkan Saling Membatalkan." },
  { "en": "Semakin Rapat Pilinan?", "id": "Semakin Baik Penolakan Terhadap Gangguan." },
  { "en": "Kabel Kategori Lebih Tinggi Punya Pilinan?", "id": "Lebih Rapat." },
  { "en": "Apa Itu Kabel Koaksial ?", "id": "Membawa Sinyal Dalam Struktur Konsentris." },
  { "en": "Medan Elektromagnetiknya Terkurung Di?", "id": "Dalam Lapisan Pelindung (Shield)." },
  { "en": "Ini Membuatnya Tahan Gangguan?", "id": "Ya Sangat Tahan Gangguan." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Penyebaran Pulsa Cahaya Akibat Beda Kecepatan." },
  { "en": "Terjadi Pada Kabel Apa?", "id": "Kabel Serat Optik." },
  { "en": "Membatasi Apa?", "id": "Kecepatan Data Maksimum." },
  { "en": "Apa Itu Pigtail Fiber?", "id": "Serat Optik Dengan Konektor Di Satu Sisi." },
  { "en": "Sisi Lainnya Diapakan?", "id": "Disambung (Spliced) Ke Kabel Utama." },
  { "en": "Apa Itu Patch Panel?", "id": "Panel Terminasi Untuk Kabel Jaringan." },
  { "en": "Memudahkan Apa?", "id": "Pengelolaan Dan Perubahan Koneksi." },
  { "en": "Apa Itu Kabel UTP Straight-Through?", "id": "Urutan Pin Sama Di Kedua Ujung." },
  { "en": "Apa Itu Kabel UTP Cross-Over?", "id": "Urutan Pin Kirim Terima Dibalik." },
  { "en": "Kabel Cross-Over Untuk Menghubungkan?", "id": "Dua Perangkat Sejenis Tanpa Switch." },
  { "en": "Perangkat Modern Menggunakan Fitur?", "id": "Auto MDI-X." },
  { "en": "Fitur Ini Membuat Kabel Cross?", "id": "Tidak Lagi Terlalu Diperlukan." },
  { "en": "Apa Itu Peringkat AWG?", "id": "Semakin Kecil Angkanya Semakin Tebal Kawatnya." },
  { "en": "Kabel Listrik Rumah Biasanya Ukuran?", "id": "1.5mm² Atau 2.5mm²." },
  { "en": "Ini Setara Dengan AWG Berapa?", "id": "Sekitar 14 AWG Atau 12 AWG." },
  { "en": "Apa Itu Kabel Tahan Air (Waterproof)?", "id": "Kabel Dengan Rating IP Tinggi (IP67/IP68)." },
  { "en": "Apa Itu Kabel Tahan Cuaca (Weatherproof)?", "id": "Tahan Terhadap Sinar UV Hujan Angin." },
  { "en": "Semua Kabel Outdoor Harus?", "id": "Tahan Cuaca." },
  { "en": "Apa Itu Arus Konveksi?", "id": "Aliran Muatan Aktual (Elektron)." },
  { "en": "Apa Itu Arus Konvensional?", "id": "Aliran Muatan Positif Fiktif." },
  { "en": "Analisis Rangkaian Menggunakan Arus?", "id": "Konvensional." },
  { "en": "Arahnya Berlawanan Dengan Aliran?", "id": "Aliran Elektron." },
  { "en": "Apa Itu Grounding Pelindung (Shield)?", "id": "Menghubungkan Shield Ke Tanah." },
  { "en": "Di Mana Sebaiknya Ditanahkan?", "id": "Hanya Di Salah Satu Ujung Kabel." },
  { "en": "Untuk Menghindari Apa?", "id": "Arus Sirkulasi Di Shield (Ground Loop)." },
  { "en": "Apa Itu Kabel Audio Bintang-Quad?", "id": "Empat Konduktor Dipilin Untuk Penolakan Noise Maksimal." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Arus Terdistribusi Tidak Rata." },
  { "en": "Penyebabnya Apa?", "id": "Medan Magnet Dari Konduktor Terdekat." },
  { "en": "Meningkatkan Resistansi Efektif?", "id": "Ya Sama Seperti Skin Effect." },
  { "en": "Apa Itu Kabel Berisolasi Kertas?", "id": "Teknologi Lama Untuk Kabel Daya." },
  { "en": "Diisi Dengan Apa?", "id": "Minyak Untuk Meningkatkan Kekuatan Isolasi." },
  { "en": "Apa Itu Kabel XLPE?", "id": "Standar Modern Untuk Kabel Daya." },
  { "en": "Apa Itu Konduktor Berkas (Bundled)?", "id": "Beberapa Konduktor Per Fasa." },
  { "en": "Digunakan Pada Saluran Transmisi?", "id": "Tegangan Sangat Tinggi." },
  { "en": "Tujuannya Mengurangi Apa?", "id": "Efek Corona Dan Induktansi Saluran." },
  { "en": "Apa Itu Efek Corona?", "id": "Ionisasi Udara Di Sekitar Konduktor." },
  { "en": "Menyebabkan Kehilangan Daya?", "id": "Ya Dan Suara Berdesis." },
  { "en": "Apa Itu Kabel Semi-Kaku (Semi-Rigid)?", "id": "Kabel Coaxial Dengan Pelindung Pipa Tembaga." },
  { "en": "Bisa Dibengkokkan?", "id": "Ya Tapi Hanya Sekali." },
  { "en": "Apa Itu Kabel Conformable?", "id": "Alternatif Fleksibel Untuk Kabel Semi-Kaku." },
  { "en": "Apa Itu Terminasi 50 Ohm?", "id": "Resistor Beban Untuk Mencegah Pantulan." },
  { "en": "Dipasang Di Ujung Kabel?", "id": "Ya Di Ujung Jalur Transmisi." },
  { "en": "Apa Itu S-Parameter?", "id": "Parameter Untuk Mengkarakterisasi Jaringan Frekuensi Tinggi." },
  { "en": "S11 Terkait Dengan?", "id": "Pantulan Sinyal (Return Loss)." },
  { "en": "S21 Terkait Dengan?", "id": "Transmisi Sinyal (Insertion Loss)." },
  { "en": "Pengukuran Ini Dilakukan Dengan?", "id": "Vector Network Analyzer (VNA)." },
  { "en": "Apa Itu Kabel Fleksibel Datar (FFC)?", "id": "Kabel Sangat Rata Untuk Koneksi Elektronik." },
  { "en": "Apa Itu Konektor ZIF?", "id": "Zero Insertion Force." },
  { "en": "Digunakan Dengan Kabel?", "id": "FFC Dan FPC." },
  { "en": "Apa Itu Kabel Berinsulasi Ganda?", "id": "Tidak Memerlukan Koneksi Kabel Ground." },
  { "en": "Simbolnya Adalah?", "id": "Kotak Di Dalam Kotak." },
  { "en": "Apa Itu Penuaan Termal?", "id": "Degradasi Isolasi Akibat Panas." },
  { "en": "Aturan Umumnya Adalah?", "id": "Setiap 10 Derajat Celcius Umur Berkurang Setengah." },
  { "en": "Apa Itu Kabel Dengan Rating Suhu 105°C?", "id": "Bisa Beroperasi Terus Di Suhu Tersebut." },
  { "en": "Apa Itu Kabel Video Komponen?", "id": "Tiga Kabel (Merah Hijau Biru) Untuk Video." },
  { "en": "Memberikan Kualitas Terbaik?", "id": "Ya Untuk Video Analog." },
  { "en": "Merah Hijau Biru Mewakili?", "id": "Sinyal Luminansi Dan Perbedaan Warna." },
  { "en": "Apa Itu Kabel VGA?", "id": "Membawa Sinyal Video Analog RGBHV." },
  { "en": "H Dan V Adalah?", "id": "Sinyal Sinkronisasi Horizontal Dan Vertikal." },
  { "en": "Apa Itu Kabel Tahan Tumbukan?", "id": "Kabel Dengan Perlindungan Mekanis Tinggi." },
  { "en": "Apa Itu Kabel Berpelindung Anyam?", "id": "Sama Dengan Braid Shield." },
  { "en": "Apa Itu Kabel Berpelindung Foil?", "id": "Pelindung Terbuat Dari Aluminium Foil." },
  { "en": "Mana Yang Cakupannya Lebih Baik?", "id": "Foil (100%)." },
  { "en": "Mana Yang Lebih Fleksibel?", "id": "Anyam (Braid)." },
  { "en": "Kabel Terbaik Menggabungkan?", "id": "Keduanya (Foil Dan Braid)." },
  { "en": "Apa Itu Api Halogen?", "id": "Menghasilkan Asap Hitam Beracun." },
  { "en": "Kabel LSZH Mengurangi Resiko?", "id": "Keracunan Asap Saat Kebakaran." },
  { "en": "Apa Itu Kabel Coaxial Heliax?", "id": "Kabel Atenuasi Sangat Rendah Untuk Pemancar." },
  { "en": "Apa Itu Konduktor Terdampar Kompak?", "id": "Serabut Yang Dipres Menjadi Padat." },
  { "en": "Diameternya Lebih Kecil?", "id": "Ya Untuk Luas Penampang Sama." },
  { "en": "Apa Itu Arus Eddy?", "id": "Arus Sirkulasi Akibat Perubahan Medan Magnet." },
  { "en": "Menyebabkan Kehilangan Energi?", "id": "Ya Dalam Bentuk Panas." },
  { "en": "Apa Beda Kabel Serabut Dan Pejal?", "id": "Serabut Fleksibel Pejal Kaku." },
  { "en": "Apa Beda UTP Dan Coaxial?", "id": "UTP Balanced Coaxial Unbalanced." },
  { "en": "Apa Beda LSZH Dan PVC?", "id": "LSZH Lebih Aman Saat Terbakar." },
  { "en": "Apa Beda Single-Mode Dan Multi-Mode?", "id": "Single-Mode Jarak Jauh Multi-Mode Jarak Pendek." },
  { "en": "Apa Beda Crimping Dan Soldering?", "id": "Crimping Tekanan Mekanis Soldering Panas." },
  { "en": "Fungsi Utama Konduktor?", "id": "Membawa Arus Listrik." },
  { "en": "Fungsi Utama Isolator?", "id": "Mencegah Arus Bocor." },
  { "en": "Fungsi Utama Pelindung (Shield)?", "id": "Menolak Gangguan Elektromagnetik." },
  { "en": "Fungsi Utama Armor?", "id": "Memberi Perlindungan Fisik." },
  { "en": "Fungsi Utama Jaket?", "id": "Melindungi Dari Lingkungan." },
  { "en": "Ukuran Konduktor Diukur Dalam?", "id": "Luas Penampang (mm² Atau AWG)." },
  { "en": "Ukuran Lebih Besar Berarti Resistansi?", "id": "Lebih Kecil." },
  { "en": "Resistansi Lebih Kecil Berarti Jatuh Tegangan?", "id": "Lebih Kecil." },
  { "en": "Pilinan Kawat Pada Kabel UTP?", "id": "Mengurangi Crosstalk." },
  { "en": "Semakin Rapat Pilinan Semakin?", "id": "Baik Performanya Di Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Penting Untuk Transmisi Sinyal Cepat." },
  { "en": "Nilai Umum Untuk Coaxial?", "id": "50 Atau 75 Ohm." },
  { "en": "Nilai Umum Untuk UTP?", "id": "100 Ohm." },
  { "en": "Ketidakcocokan Impedansi Menyebabkan?", "id": "Pantulan Sinyal." },
  { "en": "Pantulan Diukur Sebagai?", "id": "Return Loss Atau SWR." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal." },
  { "en": "Atenuasi Membatasi Apa?", "id": "Jarak Maksimum Kabel." },
  { "en": "Atenuasi Meningkat Dengan?", "id": "Frekuensi Dan Jarak." },
  { "en": "Apa Itu Kabel Plenum?", "id": "Untuk Ruang Ventilasi." },
  { "en": "Apa Itu Kabel Riser?", "id": "Untuk Jalur Vertikal." },
  { "en": "Apa Itu Kabel Tahan Api?", "id": "Untuk Sirkuit Darurat." },
  { "en": "Apa Itu Kabel Direct Burial?", "id": "Bisa Dikubur Langsung." },
  { "en": "Apa Itu Serat Optik?", "id": "Menggunakan Cahaya Bukan Listrik." },
  { "en": "Kekebalannya Terhadap EMI?", "id": "Sempurna." },
  { "en": "Kapasitas Datanya?", "id": "Sangat Besar." },
  { "en": "Dispersi Pada Serat Optik?", "id": "Membatasi Jarak Dan Kecepatan." },
  { "en": "Apa Itu Splicing?", "id": "Menyambung Dua Ujung Kabel." },
  { "en": "Splicing Fiber Menggunakan?", "id": "Fusion Splicer." },
  { "en": "Apa Itu TDR?", "id": "Mencari Kerusakan Kabel Tembaga." },
  { "en": "Apa Itu OTDR?", "id": "Mencari Kerusakan Kabel Fiber." },
  { "en": "Warna Kabel Grounding?", "id": "Hijau-Kuning." },
  { "en": "Warna Kabel Netral?", "id": "Biru." },
  { "en": "Warna Kabel Fasa?", "id": "Selain Biru Hijau-Kuning." },
  { "en": "Apa Itu Kabel Balanced?", "id": "Menggunakan Sinyal Diferensial." },
  { "en": "Sinyal Diferensial Baik Untuk?", "id": "Menolak Gangguan." },
  { "en": "Apa Itu Kabel Unbalanced?", "id": "Menggunakan Sinyal Single-Ended." },
  { "en": "Lebih Rentan Terhadap?", "id": "Gangguan." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus AC Mengumpul Di Permukaan." },
  { "en": "Meningkatkan Resistansi Efektif?", "id": "Ya." },
  { "en": "Apa Itu Kabel Litz?", "id": "Solusi Untuk Skin Effect." },
  { "en": "Setiap Serabutnya Diisolasi?", "id": "Ya." },
  { "en": "Apa Itu Derating?", "id": "Koreksi KHA Karena Kondisi Panas." },
  { "en": "Kabel Yang Dibundel Perlu?", "id": "Di-derating." },
  { "en": "Apa Itu Standar SNI?", "id": "Menjamin Keamanan Dan Kualitas." },
  { "en": "Apa Itu Konektor RJ45?", "id": "Untuk Jaringan Ethernet." },
  { "en": "Apa Itu Konektor BNC?", "id": "Untuk Video Atau RF." },
  { "en": "Apa Itu Konektor F?", "id": "Untuk Antena TV." },
  { "en": "Apa Itu Konektor XLR?", "id": "Untuk Audio Profesional." },
  { "en": "Apa Itu Konektor RCA?", "id": "Untuk Audio/Video Konsumen." },
  { "en": "Apa Itu Jalur Kembali Arus?", "id": "Harus Sedekat Mungkin Jalur Pergi." },
  { "en": "Tujuannya Mengurangi?", "id": "Loop Induktansi." },
  { "en": "Loop Induktansi Kecil Mengurangi?", "id": "Emisi Gangguan." },
  { "en": "Pilinan Kawat Menciptakan Loop?", "id": "Sangat Kecil." },
  { "en": "Apa Itu Tinned Copper?", "id": "Tembaga Dilapisi Timah." },
  { "en": "Apa Itu Bare Copper?", "id": "Tembaga Murni." },
  { "en": "Apa Itu CCA?", "id": "Aluminium Berbaju Tembaga." },
  { "en": "Kualitasnya Di Bawah?", "id": "Tembaga Murni." },
  { "en": "Apa Itu Harmonik Arus?", "id": "Menyebabkan Pemanasan Kabel Netral." },
  { "en": "Apa Itu Firestop?", "id": "Mencegah Api Merambat Lewat Lubang Kabel." },
  { "en": "Apa Itu Ground Loop?", "id": "Menyebabkan Dengung Di Audio." },
  { "en": "Apa Itu SWR?", "id": "Ukuran Pantulan Sinyal." },
  { "en": "Nilai SWR Yang Baik?", "id": "Dekat Dengan 1." },
  { "en": "Penyebab SWR Buruk?", "id": "Ketidakcocokan Impedansi." },
  { "en": "Apa Itu Kabel Hibrida?", "id": "Gabungan Fiber Dan Tembaga." },
  { "en": "Apa Itu Kabel Pemanas?", "id": "Sengaja Dibuat Untuk Menghasilkan Panas." },
  { "en": "Apa Itu Kabel Pita?", "id": "Untuk Koneksi Internal Komputer." },
  { "en": "Apa Itu Uji Kontinuitas?", "id": "Memastikan Kawat Tidak Putus." },
  { "en": "Apa Itu Uji Isolasi?", "id": "Memastikan Isolasi Tidak Bocor." },
  { "en": "Apa Itu Kabel Listrik AC?", "id": "Menghubungkan Peralatan Ke Stop Kontak." },
  { "en": "Apa Itu Kabel Ekstensi?", "id": "Untuk Penggunaan Listrik Sementara." },
  { "en": "Apa Itu Kabel Solar?", "id": "Harus Tahan Sinar UV." },
  { "en": "Apa Itu Kabel Laut?", "id": "Harus Tahan Tekanan Dan Air Asin." },
  { "en": "Apa Itu Peringkat Suhu?", "id": "Batas Aman Suhu Operasi." },
  { "en": "Apa Itu Peringkat Tegangan?", "id": "Batas Aman Tegangan Operasi." },
  { "en": "Keduanya Tidak Boleh?", "id": "Dilebihi." },
  { "en": "Apa Itu Kabel Berinsulasi Ganda?", "id": "Tidak Butuh Pin Ground." },
  { "en": "Apa Itu Kabel Koil?", "id": "Bisa Memanjang Dan Memendek." },
  { "en": "Contoh Penggunaannya?", "id": "Gagang Telepon Lama." },
  { "en": "Apa Itu Tang Amper?", "id": "Mengukur Arus Tanpa Memutus Kabel." },
  { "en": "Prinsip Kerjanya Mengukur?", "id": "Medan Magnet." },
  { "en": "Apa Itu Kabel Messenger?", "id": "Kawat Penopang Untuk Kabel Udara." },
  { "en": "Biasanya Terbuat Dari?", "id": "Baja." },
  { "en": "Apa Itu Peredam Getaran?", "id": "Mengurangi Getaran Kabel Udara Akibat Angin." },
  { "en": "Apa Itu Kabel Inti Bersektor?", "id": "Membuat Diameter Kabel Lebih Kecil." },
  { "en": "Apa Itu Konduktor Kompak?", "id": "Serabut Yang Dipadatkan." },
  { "en": "Apa Itu Kawat Email?", "id": "Kawat Isolasi Tipis Untuk Gulungan." },
  { "en": "Apa Itu Kabel Tahan Torsi?", "id": "Untuk Lengan Robot." },
  { "en": "Apa Itu Kabel Festoon?", "id": "Untuk Derek Yang Bergerak." },
  { "en": "Pemilihan Jenis Kabel Bergantung?", "id": "Aplikasi Dan Lingkungannya." },
  { "en": "Apa Beda Resistansi Dan Konduktansi?", "id": "Resistansi Menghambat Konduktansi Membantu." },
  { "en": "Apa Beda Tegangan Dan Arus?", "id": "Tegangan Adalah Penyebab Arus Adalah Akibat." },
  { "en": "Apa Beda Kabel Analog Dan Digital?", "id": "Jenis Sinyal Yang Dibawa." },
  { "en": "Apa Beda Isolasi Termoplastik Dan Termoset?", "id": "Termoplastik Meleleh Termoset Tidak." },
  { "en": "Apa Beda Kabel Tahan Api Dan LSZH?", "id": "Tahan Api Soal Fungsi LSZH Soal Asap." },
  { "en": "Apa Itu Konduktivitas?", "id": "Sifat Material Menghantarkan Listrik." },
  { "en": "Logam Paling Konduktif?", "id": "Perak." },
  { "en": "Apa Itu Resistivitas?", "id": "Sifat Material Menghambat Listrik." },
  { "en": "Apa Itu Kekuatan Dielektrik?", "id": "Kekuatan Isolasi Menahan Tegangan." },
  { "en": "Apa Itu Konstanta Dielektrik?", "id": "Mempengaruhi Kapasitansi Dan Kecepatan." },
  { "en": "Konstanta Rendah Berarti Kecepatan?", "id": "Lebih Tinggi." },
  { "en": "Apa Itu KHA?", "id": "Arus Maksimum Aman Dan Berkelanjutan." },
  { "en": "Ditentukan Oleh Pemanasan?", "id": "Ya Batas Suhu Isolasi." },
  { "en": "Apa Itu Derating?", "id": "Penurunan KHA Karena Faktor Lingkungan." },
  { "en": "Faktor Lingkungan Apa Saja?", "id": "Suhu Tinggi Dan Bundling Kabel." },
  { "en": "Apa Itu Shield?", "id": "Perlindungan Terhadap Gangguan EMI." },
  { "en": "Apa Itu Armor?", "id": "Perlindungan Terhadap Kerusakan Mekanis." },
  { "en": "Apa Itu Konduit?", "id": "Pipa Pelindung Kabel." },
  { "en": "Apa Itu Kabel Tray?", "id": "Rak Penopang Kabel." },
  { "en": "Apa Itu Fleksibilitas?", "id": "Kemampuan Kabel Untuk Dibengkokkan." },
  { "en": "Ditentukan Oleh Konduktor?", "id": "Serabut." },
  { "en": "Apa Itu Stripping?", "id": "Mengupas Isolasi." },
  { "en": "Apa Itu Crimping?", "id": "Menekan Konektor Ke Kabel." },
  { "en": "Apa Itu Splicing?", "id": "Menyambung Dua Kabel." },
  { "en": "Apa Itu Terminasi?", "id": "Mengakhiri Kabel Dengan Konektor." },
  { "en": "Apa Itu Crosstalk?", "id": "Sinyal Bocor Antar Pasangan Kawat." },
  { "en": "Dikurangi Dengan Cara?", "id": "Memilin Pasangan Kawat." },
  { "en": "Apa Itu Kabel Balanced?", "id": "Menggunakan Sinyal Diferensial." },
  { "en": "Tahan Terhadap Gangguan?", "id": "Ya Sangat Baik." },
  { "en": "Apa Itu Kabel Unbalanced?", "id": "Menggunakan Sinyal Single-Ended." },
  { "en": "Apa Itu Common Mode Noise?", "id": "Gangguan Yang Sama Di Kedua Kawat." },
  { "en": "Ditolak Oleh Sistem?", "id": "Sistem Balanced." },
  { "en": "Apa Itu Ground Loop?", "id": "Penyebab Umum Dengung Audio." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus AC Terkonsentrasi Di Permukaan." },
  { "en": "Efektif Pada Frekuensi?", "id": "Tinggi." },
  { "en": "Apa Itu Kabel Litz?", "id": "Mengurangi Skin Effect." },
  { "en": "Apa Itu Proximity Effect?", "id": "Distribusi Arus Terganggu Konduktor Lain." },
  { "en": "Apa Itu Dispersi?", "id": "Di Fiber Optik Membatasi Bandwidth." },
  { "en": "Ada Berapa Jenis Dispersi?", "id": "Modal Dan Kromatik." },
  { "en": "Dispersi Modal Terjadi Di Fiber?", "id": "Multi-Mode." },
  { "en": "Apa Itu Fusion Splicer?", "id": "Alat Penyambung Fiber Optik." },
  { "en": "Apa Itu OTDR?", "id": "Mendeteksi Lokasi Putus Fiber." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan." },
  { "en": "Diukur Dalam Satuan?", "id": "Desibel (dB)." },
  { "en": "Apa Itu Peringkat Api?", "id": "Menentukan Perilaku Kabel Dalam Kebakaran." },
  { "en": "Plenum Adalah Peringkat Tertinggi?", "id": "Ya Untuk Udara Ventilasi." },
  { "en": "Apa Itu TDR?", "id": "Mendeteksi Lokasi Kerusakan Kabel Tembaga." },
  { "en": "Apa Itu SWR?", "id": "Mengukur Pantulan Sinyal." },
  { "en": "Disebabkan Oleh Ketidakcocokan?", "id": "Impedansi." },
  { "en": "Apa Itu Balun?", "id": "Mengubah Balanced Ke Unbalanced." },
  { "en": "Apa Itu Service Loop?", "id": "Simpanan Kabel Untuk Perbaikan." },
  { "en": "Apa Itu Harmonik?", "id": "Bisa Memanaskan Kabel Netral." },
  { "en": "Beban Non-Linier Menghasilkan?", "id": "Harmonik Arus." },
  { "en": "Apa Itu Kabel Koil?", "id": "Bisa Ditarik Memanjang." },
  { "en": "Apa Itu Konektor IDC?", "id": "Tidak Perlu Mengupas Isolasi." },
  { "en": "Apa Itu Sinyal Diferensial?", "id": "Menggunakan Perbedaan Tegangan Antara Dua Kawat." },
  { "en": "Sangat Kebal Terhadap?", "id": "Gangguan." },
  { "en": "USB Dan Ethernet Menggunakan?", "id": "Sinyal Diferensial." },
  { "en": "Apa Itu Kabel Tahan Torsi?", "id": "Untuk Gerakan Memuntir." },
  { "en": "Apa Itu Kabel Tahan Tekuk?", "id": "Untuk Gerakan Maju Mundur." },
  { "en": "Digunakan Di Mesin?", "id": "Robotik Dan Otomasi." },
  { "en": "Apa Itu Jalur Kembali Arus?", "id": "Harus Selalu Dekat Dengan Jalur Pergi." },
  { "en": "Mengurangi Apa?", "id": "Medan Magnet Yang Dipancarkan." },
  { "en": "Kabel Twisted Pair Adalah Contoh?", "id": "Desain Yang Sangat Baik." },
  { "en": "Apa Itu Umur Kabel?", "id": "Ditentukan Oleh Degradasi Isolasi." },
  { "en": "Faktor Degradasi Utama?", "id": "Panas." },
  { "en": "Apa Itu Uji Hi-Pot?", "id": "Uji Kekuatan Isolasi." },
  { "en": "Apa Itu Partial Discharge?", "id": "Tanda Adanya Cacat Di Dalam Isolasi." },
  { "en": "Apa Itu Pohon Air (Water Treeing)?", "id": "Degradasi Isolasi Kabel Bawah Tanah." },
  { "en": "Apa Itu Firestop?", "id": "Menyegel Tembusan Kabel." },
  { "en": "Apa Itu Kabel Berpendingin?", "id": "Untuk Aplikasi Daya Sangat Tinggi." },
  { "en": "Apa Itu Konduktor Berkas?", "id": "Mengurangi Efek Corona." },
  { "en": "Apa Itu Efek Corona?", "id": "Pelepasan Listrik Di Udara." },
  { "en": "Apa Itu Ikatan Kabel?", "id": "MerapiDan Mengatur Kabel." },
  { "en": "Apa Itu Penanda Kabel?", "id": "Memberi Label Pada Kabel." },
  { "en": "Apa Itu RoHS?", "id": "Direktif Pembatasan Bahan Berbahaya." },
  { "en": "Apa Itu REACH?", "id": "Standar Eropa Lainnya Untuk Bahan Kimia." },
  { "en": "Apa Itu UL Listed?", "id": "Sertifikasi Keamanan." },
  { "en": "Apa Itu CE Mark?", "id": "Tanda Kesesuaian Untuk Pasar Eropa." },
  { "en": "Apa Itu Kabel Data Optik Aktif?", "id": "Kabel Fiber Dengan Elektronik Di Konektor." },
  { "en": "Apa Itu Kabel Pengaman Intrinsik?", "id": "Energinya Dibatasi Agar Tidak Memicu Ledakan." },
  { "en": "Warnanya Selalu?", "id": "Biru." },
  { "en": "Apa Itu Kabel Heliax?", "id": "Kabel Coaxial Dengan Rugi Sangat Rendah." },
  { "en": "Dielektriknya Sering Berupa?", "id": "Udara." },
  { "en": "Apa Itu Peringkat Suhu Rendah?", "id": "Batas Aman Kabel Digunakan Di Dingin." },
  { "en": "Isolasi Bisa Retak?", "id": "Ya Jika Terlalu Dingin." },
  { "en": "Apa Itu Kabel Tahan Abrasi?", "id": "Jaketnya Tahan Terhadap Gesekan." },
  { "en": "Apa Itu Kabel Silikon?", "id": "Sangat Fleksibel Dan Tahan Suhu Tinggi." },
  { "en": "Apa Itu Kabel Teflon?", "id": "Tahan Suhu Sangat Tinggi Dan Bahan Kimia." },
  { "en": "Apa Itu Kabel LAN?", "id": "Kabel Untuk Jaringan Area Lokal." },
  { "en": "Apa Itu Kabel WAN?", "id": "Kabel Untuk Jaringan Area Luas." },
  { "en": "Kabel WAN Sering Berupa?", "id": "Serat Optik." },
  { "en": "Dasar Dari Kabel Adalah?", "id": "Menyalurkan Energi Atau Informasi Secara Terarah." },
  { "en": "Konduktor Memberi Jalan?", "id": "Isolator Memberi Batasan." },
  { "en": "Kombinasi Ini Mendasari?", "id": "Seluruh Dunia Elektronik Modern." },
  { "en": "Apa Dua Fungsi Dasar Kabel?", "id": "Menghantarkan Dan Mengisolasi Listrik." },
  { "en": "Apa Dua Material Konduktor Utama?", "id": "Tembaga Dan Aluminium." },
  { "en": "Apa Dua Jenis Konduktor Utama?", "id": "Pejal Dan Serabut." },
  { "en": "Apa Dua Jenis Kabel Jaringan Utama?", "id": "Twisted Pair Dan Serat Optik." },
  { "en": "Apa Dua Ancaman Utama Sinyal?", "id": "Atenuasi Dan Interferensi." },
  { "en": "Apa Peran Utama Isolasi?", "id": "Mencegah Hubungan Singkat Dan Kejutan Listrik." },
  { "en": "Apa Peran Utama Konduktor?", "id": "Menyediakan Jalur Resistansi Rendah." },
  { "en": "Apa Peran Utama Shield?", "id": "Memblokir Gangguan Elektromagnetik." },
  { "en": "Apa Peran Utama Armor?", "id": "Menahan Kerusakan Fisik." },
  { "en": "Apa Peran Utama Jaket?", "id": "Melindungi Dari Lingkungan (UV Air)." },
  { "en": "Ukuran Kabel Ditentukan Oleh?", "id": "Kebutuhan Arus Dan Batas Jatuh Tegangan." },
  { "en": "Fleksibilitas Ditentukan Oleh?", "id": "Konstruksi Konduktor (Serabut)." },
  { "en": "Perlindungan Gangguan Ditentukan Oleh?", "id": "Kualitas Pelindung (Shield)." },
  { "en": "Ketahanan Lingkungan Ditentukan Oleh?", "id": "Material Jaket Luar." },
  { "en": "Jarak Transmisi Ditentukan Oleh?", "id": "Tingkat Atenuasi Kabel." },
  { "en": "Kabel Tembaga Membawa?", "id": "Elektron." },
  { "en": "Kabel Optik Membawa?", "id": "Foton (Cahaya)." },
  { "en": "Mana Yang Lebih Cepat?", "id": "Kecepatan Sinyal Hampir Sama." },
  { "en": "Mana Yang Kapasitasnya Lebih Besar?", "id": "Kabel Serat Optik." },
  { "en": "Mana Yang Kebal Gangguan?", "id": "Kabel Serat Optik." },
  { "en": "Apa Itu KHA?", "id": "Ampasitas Arus Maksimum." },
  { "en": "Apa Itu Jatuh Tegangan?", "id": "Kehilangan Tegangan." },
  { "en": "Apa Itu Impedansi?", "id": "Hambatan Sinyal AC." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal." },
  { "en": "Apa Itu Crosstalk?", "id": "Bocoran Sinyal Antar Pasangan." },
  { "en": "Pilinan Kawat Untuk Mengatasi?", "id": "Crosstalk." },
  { "en": "Shielding Untuk Mengatasi?", "id": "Interferensi Eksternal (EMI)." },
  { "en": "Ukuran Besar Untuk Mengatasi?", "id": "Jatuh Tegangan." },
  { "en": "Isolasi Tebal Untuk Mengatasi?", "id": "Tegangan Tinggi." },
  { "en": "Jaket Khusus Untuk Mengatasi?", "id": "Lingkungan Keras (UV Kimia)." },
  { "en": "Apa Itu Konektor?", "id": "Antarmuka Sambungan." },
  { "en": "Apa Itu Terminal?", "id": "Titik Akhir Sambungan." },
  { "en": "Apa Itu Splicing?", "id": "Sambungan Permanen." },
  { "en": "Apa Itu Crimping?", "id": "Teknik Terminasi Tanpa Panas." },
  { "en": "Apa Itu Soldering?", "id": "Teknik Sambungan Menggunakan Timah." },
  { "en": "Kabel Instalasi Dalam Dinding?", "id": "Kabel Pejal (NYM)." },
  { "en": "Kabel Untuk Peralatan Bergerak?", "id": "Kabel Serabut (Fleksibel)." },
  { "en": "Kabel Untuk Internet Antar Benua?", "id": "Kabel Serat Optik Bawah Laut." },
  { "en": "Kabel Untuk Jaringan Di Kantor?", "id": "Kabel UTP Kategori 6." },
  { "en": "Kabel Untuk Antena TV?", "id": "Kabel Coaxial 75 Ohm." },
  { "en": "Apa Itu Standar?", "id": "Menjamin Interoperabilitas Dan Keamanan." },
  { "en": "Apa Itu Kode Warna?", "id": "Mencegah Kesalahan Pemasangan." },
  { "en": "Apa Itu Peringkat Api?", "id": "Menentukan Keselamatan Saat Kebakaran." },
  { "en": "Apa Itu Peringkat IP?", "id": "Menentukan Ketahanan Terhadap Lingkungan." },
  { "en": "Apa Itu Derating?", "id": "Menyesuaikan Kinerja Dengan Kondisi Nyata." },
  { "en": "Suhu Panas Berarti KHA?", "id": "Harus Diturunkan." },
  { "en": "Kabel Dibundel Berarti KHA?", "id": "Harus Diturunkan." },
  { "en": "Kabel Dalam Konduit Berarti KHA?", "id": "Harus Diturunkan." },
  { "en": "Apa Itu Skin Effect?", "id": "Mempengaruhi Arus AC." },
  { "en": "Apa Itu Proximity Effect?", "id": "Mempengaruhi Konduktor Berdekatan." },
  { "en": "Apa Itu Dispersi?", "id": "Mempengaruhi Sinyal Fiber Optik." },
  { "en": "Apa Itu Pantulan?", "id": "Mempengaruhi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Oksidasi?", "id": "Mempengaruhi Titik Sambungan." },
  { "en": "Tembaga Lebih Baik Dari?", "id": "Aluminium." },
  { "en": "Serabut Lebih Fleksibel Dari?", "id": "Pejal." },
  { "en": "STP Lebih Terlindung Dari?", "id": "UTP." },
  { "en": "XLPE Lebih Tahan Panas Dari?", "id": "PVC." },
  { "en": "Single-Mode Lebih Jauh Dari?", "id": "Multi-Mode." },
  { "en": "Apa Itu Sinyal Balanced?", "id": "Menggunakan Tiga Konduktor." },
  { "en": "Apa Keunggulannya?", "id": "Penolakan Gangguan." },
  { "en": "Apa Itu Sinyal Unbalanced?", "id": "Menggunakan Dua Konduktor." },
  { "en": "Apa Itu Ground Loop?", "id": "Masalah Umum Pada Sistem Unbalanced." },
  { "en": "Apa Itu TDR?", "id": "Mencari Jarak Ke Kerusakan." },
  { "en": "Bekerja Seperti Radar?", "id": "Ya Menggunakan Pantulan Pulsa." },
  { "en": "Apa Itu Megger?", "id": "Memberi Tegangan Tinggi Mengukur Arus Bocor." },
  { "en": "Menghitung Apa?", "id": "Resistansi Isolasi." },
  { "en": "Hasil Baiknya Adalah?", "id": "Resistansi Sangat Tinggi." },
  { "en": "Apa Itu Harmonik?", "id": "Menyebabkan Pemanasan Ekstra." },
  { "en": "Terutama Pada Konduktor?", "id": "Netral." },
  { "en": "Apa Itu Jalur Kembali?", "id": "Sangat Penting Untuk Integritas Sinyal." },
  { "en": "Harus Selalu Dekat Dengan?", "id": "Jalur Pergi." },
  { "en": "Untuk Meminimalkan?", "id": "Loop Area." },
  { "en": "Apa Itu Kabel Listrik?", "id": "Jalan Raya Untuk Elektron." },
  { "en": "Apa Itu Kabel Data?", "id": "Sistem Saraf Untuk Informasi." },
  { "en": "Apa Itu Kabel Fiber Optik?", "id": "Jalan Tol Cahaya." },
  { "en": "Apa Itu Isolator?", "id": "Pagar Pembatas Jalan Raya." },
  { "en": "Apa Itu Shield?", "id": "Dinding Peredam Suara." },
  { "en": "Apa Itu Konektor?", "id": "Gerbang Masuk Dan Keluar." },
  { "en": "Apa Itu KHA?", "id": "Batas Kecepatan Aman." },
  { "en": "Apa Itu Jatuh Tegangan?", "id": "Kehilangan Energi Di Perjalanan." },
  { "en": "Apa Itu Standar SNI?", "id": "Aturan Lalu Lintas Untuk Listrik." },
  { "en": "Apa Itu Peringkat Api?", "id": "Standar Keselamatan Kebakaran." },
  { "en": "Memilih Kabel Yang Tepat Adalah?", "id": "Kunci Keberhasilan Sebuah Sistem." },
  { "en": "Kabel Yang Salah Adalah?", "id": "Sumber Masalah Dan Bahaya." },
  { "en": "Konduktor Terbuat Dari?", "id": "Logam." },
  { "en": "Isolator Terbuat Dari?", "id": "Plastik Atau Karet." },
  { "en": "Kabel Modern Adalah Hasil?", "id": "Rekayasa Material Yang Canggih." },
  { "en": "Tujuannya Selalu Sama?", "id": "Transmisi Efisien Dan Aman." },
  { "en": "Dari Skala Mikroelektronik Hingga?", "id": "Skala Jaringan Listrik Global." },
  { "en": "Kabel Adalah Teknologi Fundamental?", "id": "Ya Sangat Fundamental." },
  { "en": "Memungkinkan Masyarakat Modern?", "id": "Ya Benar." },
  { "en": "Apa Itu Kabel Daya?", "id": "Fokus Pada Efisiensi Energi." },
  { "en": "Apa Itu Kabel Komunikasi?", "id": "Fokus Pada Integritas Informasi." },
  { "en": "Dua Tujuan Berbeda?", "id": "Membutuhkan Desain Kabel Berbeda." },
  { "en": "Memahami Dasar-dasar Ini?", "id": "Penting Untuk Semua Teknisi." }




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
