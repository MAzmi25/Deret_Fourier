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


{ "en": "Siapa penemu deret ini?", "id": "Jean-Baptiste Joseph Fourier." },
{ "en": "Apa tujuan utama Deret Fourier?", "id": "Menguraikan sinyal periodik menjadi sinusoida." },
{ "en": "Apa syarat utama sebuah sinyal?", "id": "Sinyal tersebut harus periodik." },
{ "en": "Fungsi dasar yang digunakan sebagai basis?", "id": "Fungsi sinus dan cosinus." },
{ "en": "Apa sifat utama dari fungsi basis?", "id": "Ortogonal satu sama lain." },
{ "en": "Apa artinya dua fungsi ortogonal?", "id": "Integral perkaliannya nol selama satu periode." },
{ "en": "Deret Fourier mengubah sinyal dari domain?", "id": "Waktu ke domain frekuensi." },
{ "en": "Komponen frekuensi terendah selain DC?", "id": "Frekuensi fundamental." },
{ "en": "Frekuensi fundamental sama dengan frekuensi?", "id": "Sinyal periodik aslinya." },
{ "en": "Kelipatan bulat dari frekuensi fundamental?", "id": "Harmonisa." },
{ "en": "Harmonisa kedua memiliki frekuensi?", "id": "Dua kali frekuensi fundamental." },
{ "en": "Nilai rata-rata sinyal disebut juga?", "id": "Komponen DC." },
{ "en": "Representasi grafis amplitudo vs frekuensi?", "id": "Spektrum amplitudo." },
{ "en": "Representasi grafis fasa vs frekuensi?", "id": "Spektrum fasa." },
{ "en": "Spektrum sinyal periodik bersifat?", "id": "Diskrit (berupa garis-garis)." },
{ "en": "Jarak antar garis pada spektrum frekuensi?", "id": "Sama dengan frekuensi fundamental." },
{ "en": "Periode sinyal f(t) dilambangkan dengan?", "id": "T." },
{ "en": "Frekuensi fundamental (f₀) adalah?", "id": "Satu dibagi periode (1/T)." },
{ "en": "Frekuensi sudut fundamental (ω₀)?", "id": "Dua pi dibagi periode (2π/T)." },
{ "en": "Deret Fourier adalah penjumlahan tak hingga dari?", "id": "Gelombang sinus dan cosinus." },
{ "en": "Apakah semua sinyal periodik bisa diurai?", "id": "Ya, jika memenuhi kondisi Dirichlet." },
{ "en": "Analisis Fourier adalah istilah umum untuk?", "id": "Deret dan Transformasi Fourier." },
{ "en": "Deret Fourier berlaku untuk sinyal?", "id": "Periodik." },
{ "en": "Transformasi Fourier berlaku untuk sinyal?", "id": "Non-periodik (aperiodik)." },
{ "en": "Apa itu sintesis Fourier?", "id": "Membangun kembali sinyal dari komponen frekuensinya." },
{ "en": "Apa itu analisis Fourier?", "id": "Menguraikan sinyal menjadi komponen frekuensinya." },
{ "en": "Koefisien Fourier merepresentasikan?", "id": "Amplitudo dan fasa setiap harmonisa." },
{ "en": "Semakin banyak suku deret yang digunakan?", "id": "Aproksimasi sinyal semakin baik." },
{ "en": "Dasar matematis Deret Fourier?", "id": "Ruang vektor dan basis ortogonal." },
{ "en": "Fungsi periodik f(t) sama dengan?", "id": "f(t + nT)." },
{ "en": "Bentuk gelombang paling dasar di alam?", "id": "Sinusoida." },
{ "en": "Setiap sinyal periodik kompleks adalah?", "id": "Kombinasi dari banyak sinusoida." },
{ "en": "Apa itu domain waktu?", "id": "Representasi sinyal sebagai fungsi waktu." },
{ "en": "Apa itu domain frekuensi?", "id": "Representasi sinyal sebagai fungsi frekuensi." },
{ "en": "Deret Fourier menyediakan jembatan antara domain?", "id": "Waktu dan frekuensi." },
{ "en": "Amplitudo harmonisa menunjukkan?", "id": "Kekuatan atau kontribusi frekuensi tersebut." },
{ "en": "Fasa harmonisa menunjukkan?", "id": "Pergeseran waktu awal frekuensi tersebut." },
{ "en": "Gelombang sinus murni memiliki berapa komponen frekuensi?", "id": "Satu komponen frekuensi (fundamental)." },
{ "en": "Sinyal DC murni memiliki frekuensi?", "id": "Nol." },
{ "en": "Apakah urutan suku dalam deret penting?", "id": "Tidak untuk hasil akhir, tapi biasanya diurutkan." },
{ "en": "Deret Fourier adalah alat penting dalam?", "id": "Pemrosesan sinyal dan komunikasi." },
{ "en": "Berapa banyak koefisien dalam deret tak hingga?", "id": "Tak terhingga banyaknya." },
{ "en": "Apa itu periode fundamental?", "id": "Nilai T terkecil yang memenuhi periodisitas." },
{ "en": "Frekuensi harmonisa ke-n (fn)?", "id": "n dikali f₀." },
{ "en": "Frekuensi sudut harmonisa ke-n (ωn)?", "id": "n dikali ω₀." },
{ "en": "Apakah Deret Fourier bisa untuk sinyal diskrit?", "id": "Ya, disebut Deret Fourier Diskrit." },
{ "en": "Sinyal periodik artinya sinyal yang?", "id": "Berulang secara teratur." },
{ "en": "Integral selama satu periode dilambangkan?", "id": "Integral dengan batas T." },
{ "en": "Fungsi basis {sin(nω₀t), cos(nω₀t)} adalah?", "id": "Himpunan ortogonal." },
{ "en": "Konsep kunci di balik perhitungan koefisien?", "id": "Sifat ortogonalitas." },
{ "en": "Setiap sinyal periodik unik memiliki?", "id": "Spektrum frekuensi yang unik." },
{ "en": "Apa itu alias dalam konteks sinyal?", "id": "Frekuensi tinggi tampak seperti frekuensi rendah." },
{ "en": "Deret Fourier dapat dianggap sebagai dekomposisi?", "id": "Dekomposisi sinyal." },
{ "en": "Konsep periodisitas sangat penting untuk?", "id": "Definisi Deret Fourier." },
{ "en": "Jika sinyal tidak periodik, apa yang digunakan?", "id": "Transformasi Fourier." },
{ "en": "Apa itu sinyal waktu-kontinu?", "id": "Sinyal yang terdefinisi di setiap waktu." },
{ "en": "Deret Fourier klasik untuk sinyal?", "id": "Waktu-kontinu dan periodik." },
{ "en": "Apa itu sinyal waktu-diskrit?", "id": "Sinyal yang hanya terdefinisi pada waktu tertentu." },
{ "en": "Apa itu truncating (pemotongan) deret?", "id": "Hanya menggunakan sejumlah terbatas suku." },
{ "en": "Pemotongan deret menyebabkan?", "id": "Error aproksimasi." },
{ "en": "Apakah koefisien bisa bernilai nol?", "id": "Ya, jika harmonisa tersebut tidak ada." },
{ "en": "Apa itu fungsi periodik piecewise-continuous?", "id": "Syarat untuk konvergensi." },
{ "en": "Deret Fourier adalah contoh dari deret?", "id": "Deret fungsional." },
{ "en": "Setiap suku dalam deret merepresentasikan?", "id": "Sebuah harmonisa." },
{ "en": "Suku pertama (n=1) disebut?", "id": "Komponen fundamental." },
{ "en": "Frekuensi fundamental kadang disebut?", "id": "Harmonisa pertama." },
{ "en": "Proses penguraian sinyal disebut juga?", "id": "Analisis harmonisa." },
{ "en": "Plot spektrum juga dikenal sebagai?", "id": "Plot garis." },
{ "en": "Tinggi garis pada spektrum menunjukkan?", "id": "Amplitudo harmonisa." },
{ "en": "Posisi garis pada spektrum menunjukkan?", "id": "Frekuensi harmonisa." },
{ "en": "Apakah Deret Fourier linier?", "id": "Ya, merupakan operator linier." },
{ "en": "Arti linieritas Deret Fourier?", "id": "F(af(t)+bg(t)) = aF(f(t))+bF(g(t))." },
{ "en": "Rentang integrasi untuk koefisien?", "id": "Satu periode penuh (misal: 0 ke T)." },
{ "en": "Apakah titik awal integrasi penting?", "id": "Tidak, selama intervalnya satu periode." },
{ "en": "Deret Fourier dapat merepresentasikan fungsi?", "id": "Dengan diskontinuitas (lompatan)." },
{ "en": "Apa itu nada (tone) dalam musik?", "id": "Frekuensi fundamental." },
{ "en": "Apa itu overtone dalam musik?", "id": "Harmonisa." },
{ "en": "Warna suara (timbre) ditentukan oleh?", "id": "Amplitudo relatif dari harmonisa." },
{ "en": "Alat musik berbeda, nada sama, suara beda?", "id": "Karena spektrum harmonisanya berbeda." },
{ "en": "Suara manusia adalah sinyal?", "id": "Periodik (saat menahan nada)." },
{ "en": "Analisis spektrum suara disebut?", "id": "Analisis spektral." },
{ "en": "Deret Fourier adalah dasar dari?", "id": "Pemrosesan sinyal digital (Digital Signal Processing)." },
{ "en": "Frekuensi dalam Hertz (Hz) adalah?", "id": "Siklus per detik." },
{ "en": "Frekuensi sudut (rad/s) adalah?", "id": "Ukuran laju rotasi." },
{ "en": "Hubungan ω dan f?", "id": "ω = 2πf." },
{ "en": "Apakah koefisien bisa bilangan kompleks?", "id": "Ya, dalam bentuk eksponensial." },
{ "en": "Amplitudo selalu bernilai?", "id": "Positif atau nol." },
{ "en": "Fasa bisa bernilai?", "id": "Positif atau negatif." },
{ "en": "Deret Fourier menyederhanakan analisis sistem?", "id": "Sistem Linier Invarian Waktu (LTI)." },
{ "en": "Bentuk umum Deret Fourier trigonometri?", "id": "a₀ + Σ(aₙcos + bₙsin)." },
{ "en": "Koefisien yang merepresentasikan komponen DC?", "id": "a₀." },
{ "en": "Rumus untuk koefisien a₀?", "id": "1/T integral f(t) dt." },
{ "en": "a₀ adalah nilai ... dari sinyal?", "id": "Nilai rata-rata (average)." },
{ "en": "Koefisien yang merepresentasikan amplitudo cosinus?", "id": "aₙ." },
{ "en": "Rumus untuk koefisien aₙ?", "id": "2/T integral f(t)cos(nω₀t) dt." },
{ "en": "Koefisien yang merepresentasikan amplitudo sinus?", "id": "bₙ." },
{ "en": "Rumus untuk koefisien bₙ?", "id": "2/T integral f(t)sin(nω₀t) dt." },
{ "en": "Bentuk lain dari Deret Fourier?", "id": "Bentuk amplitudo-fasa (kompak)." },
{ "en": "Bentuk amplitudo-fasa menggunakan fungsi?", "id": "Hanya cosinus dengan pergeseran fasa." },
{ "en": "Bentuknya: A₀ + Σ Aₙ cos(nω₀t + φₙ)?", "id": "Bentuk kompak." },
{ "en": "Hubungan A₀ dan a₀?", "id": "A₀ = a₀." },
{ "en": "Hubungan Aₙ dengan aₙ dan bₙ?", "id": "Aₙ = akar(aₙ² + bₙ²)." },
{ "en": "Hubungan φₙ dengan aₙ dan bₙ?", "id": "φₙ = -arctan(bₙ/aₙ)." },
{ "en": "Aₙ disebut juga?", "id": "Amplitudo harmonisa ke-n." },
{ "en": "φₙ disebut juga?", "id": "Sudut fasa harmonisa ke-n." },
{ "en": "Bentuk paling ringkas Deret Fourier?", "id": "Bentuk eksponensial kompleks." },
{ "en": "Bentuk eksponensial menggunakan fungsi?", "id": "e^(jnω₀t)." },
{ "en": "Rumus bentuk eksponensial?", "id": "Σ cₙ e^(jnω₀t)." },
{ "en": "Indeks n pada bentuk eksponensial?", "id": "Dari -∞ hingga +∞." },
{ "en": "Koefisien pada bentuk eksponensial?", "id": "cₙ." },
{ "en": "Rumus untuk koefisien cₙ?", "id": "1/T integral f(t)e^(-jnω₀t) dt." },
{ "en": "Koefisien c₀ sama dengan?", "id": "a₀ (nilai rata-rata)." },
{ "en": "Hubungan cₙ dengan aₙ dan bₙ?", "id": "cₙ = (aₙ - jbₙ) / 2." },
{ "en": "Hubungan c₋ₙ dengan cₙ (untuk sinyal riil)?", "id": "c₋ₙ adalah konjugat kompleks cₙ." },
{ "en": "Hubungan aₙ dengan cₙ dan c₋ₙ?", "id": "aₙ = 2 Re(cₙ)." },
{ "en": "Hubungan bₙ dengan cₙ dan c₋ₙ?", "id": "bₙ = -2 Im(cₙ)." },
{ "en": "Spektrum dari bentuk eksponensial disebut?", "id": "Spektrum dua sisi." },
{ "en": "Spektrum dari bentuk kompak disebut?", "id": "Spektrum satu sisi." },
{ "en": "Spektrum dua sisi ada untuk frekuensi?", "id": "Positif dan negatif." },
{ "en": "Apakah frekuensi negatif memiliki arti fisik?", "id": "Tidak, hanya konstruksi matematis." },
{ "en": "Magnitudo spektrum dua sisi?", "id": "|cₙ|." },
{ "en": "Fasa spektrum dua sisi?", "id": "Sudut dari cₙ." },
{ "en": "Spektrum magnitudo dua sisi bersifat?", "id": "Simetri genap (|cₙ| = |c₋ₙ|)." },
{ "en": "Spektrum fasa dua sisi bersifat?", "id": "Simetri ganjil (sudut(cₙ) = -sudut(c₋ₙ))." },
{ "en": "Hubungan magnitudo spektrum satu sisi (Aₙ) dan dua sisi (|cₙ|)?", "id": "Aₙ = 2|cₙ| untuk n > 0." },
{ "en": "Jika sinyal tidak punya komponen DC?", "id": "a₀ = 0." },
{ "en": "Jika sinyal hanya cosinus, koefisien mana yang ada?", "id": "a₀ dan aₙ." },
{ "en": "Jika sinyal hanya sinus, koefisien mana yang ada?", "id": "Hanya bₙ." },
{ "en": "Proses menghitung integral untuk koefisien?", "id": "Bisa rumit, perlu teknik integrasi." },
{ "en": "Apa gunanya mengetahui berbagai bentuk deret?", "id": "Setiap bentuk berguna untuk aplikasi berbeda." },
{ "en": "Bentuk eksponensial berguna dalam?", "id": "Teori komunikasi dan sistem." },
{ "en": "Bentuk trigonometri berguna untuk?", "id": "Visualisasi dan pemahaman awal." },
{ "en": "Bentuk kompak berguna untuk?", "id": "Menunjukkan amplitudo dan fasa secara langsung." },
{ "en": "Apa itu ortonormalitas?", "id": "Ortogonal dan memiliki norma satu." },
{ "en": "Norma dari sin(nω₀t)?", "id": "T/2." },
{ "en": "Basis {e^(jnω₀t)} bersifat?", "id": "Ortogonal." },
{ "en": "Rumus Euler menghubungkan eksponensial dengan?", "id": "Sinus dan cosinus." },
{ "en": "e^(jx) = ?", "id": "cos(x) + jsin(x)." },
{ "en": "cos(x) dalam bentuk eksponensial?", "id": "(e^(jx) + e^(-jx)) / 2." },
{ "en": "sin(x) dalam bentuk eksponensial?", "id": "(e^(jx) - e^(-jx)) / 2j." },
{ "en": "Jika f(t) adalah konstan C?", "id": "a₀ = C, koefisien lain nol." },
{ "en": "Jika f(t) = cos(ω₀t)?", "id": "a₁ = 1, koefisien lain nol." },
{ "en": "Jika f(t) = sin(ω₀t)?", "id": "b₁ = 1, koefisien lain nol." },
{ "en": "Faktor 1/T dan 2/T adalah faktor?", "id": "Normalisasi." },
{ "en": "Apakah nilai koefisien bergantung pada T?", "id": "Ya, melalui faktor normalisasi." },
{ "en": "Perhitungan koefisien adalah proyeksi f(t) ke?", "id": "Setiap fungsi basis." },
{ "en": "Istilah n=0 merepresentasikan?", "id": "Komponen DC." },
{ "en": "Istilah n=1 merepresentasikan?", "id": "Komponen fundamental." },
{ "en": "Istilah n > 1 merepresentasikan?", "id": "Overtones atau harmonisa." },
{ "en": "Jika bₙ=0 untuk semua n, sinyal bersifat?", "id": "Genap." },
{ "en": "Jika aₙ=0 (n≥0) untuk semua n, sinyal bersifat?", "id": "Ganjil." },
{ "en": "Koefisien cₙ adalah bilangan?", "id": "Kompleks." },
{ "en": "Magnitudo |cₙ| adalah?", "id": "Akar(Re(cₙ)² + Im(cₙ)²)." },
{ "en": "Koefisien cₙ mengandung informasi?", "id": "Amplitudo dan fasa." },
{ "en": "Apakah kita bisa mengkonversi dari satu bentuk ke bentuk lain?", "id": "Ya, menggunakan rumus konversi." },
{ "en": "Koefisien bₙ bisa dianggap sebagai korelasi antara?", "id": "Sinyal dan gelombang sinus." },
{ "en": "Koefisien aₙ bisa dianggap sebagai korelasi antara?", "id": "Sinyal dan gelombang cosinus." },
{ "en": "Jika sinyal bergeser waktu, apa yang berubah?", "id": "Fasa (φₙ atau sudut cₙ)." },
{ "en": "Jika amplitudo sinyal berubah, apa yang berubah?", "id": "Amplitudo semua koefisien." },
{ "en": "Perhitungan koefisien adalah jantung dari?", "id": "Analisis Fourier." },
{ "en": "Untuk sinyal riil, aₙ dan bₙ selalu?", "id": "Bilangan riil." },
{ "en": "Spektrum satu sisi hanya didefinisikan untuk?", "id": "Frekuensi non-negatif." },
{ "en": "Frekuensi negatif membantu menjaga?", "id": "Simetri matematis." },
{ "en": "Penting untuk konsisten dengan definisi?", "id": "Faktor normalisasi (1/T atau 2/T)." },
{ "en": "Variasi definisi koefisien ada?", "id": "Ya, terutama pada faktor normalisasi." },
{ "en": "Deret Fourier adalah alat dekomposisi?", "id": "Linear." },
{ "en": "Setiap suku Aₙ cos(nω₀t + φₙ) adalah?", "id": "Sebuah fasor dalam domain frekuensi." },
{ "en": "Panjang fasor adalah?", "id": "Amplitudo Aₙ." },
{ "en": "Sudut fasor adalah?", "id": "Sudut fasa φₙ." },
{ "en": "Bentuk eksponensial paling efisien untuk?", "id": "Manipulasi matematis." },
{ "en": "Apakah aₙ dan bₙ bergantung pada n?", "id": "Ya, nilainya berbeda untuk setiap harmonisa." },
{ "en": "Perhitungan koefisien melibatkan operasi?", "id": "Integral." },
{ "en": "Untuk sinyal sederhana, integral bisa dihitung?", "id": "Secara analitis." },
{ "en": "Untuk sinyal kompleks, integral dihitung?", "id": "Secara numerik." },
{ "en": "Algoritma komputasi untuk ini disebut?", "id": "Transformasi Fourier Cepat (FFT)." },
{ "en": "Tanda φₙ bergantung pada?", "id": "Definisi (cos(x+φ) atau cos(x-φ))." },
{ "en": "Hubungan |cₙ|² dengan daya?", "id": "Proporsional dengan daya harmonisa ke-n." },
{ "en": "Jika sinyalnya konstan, spektrumnya?", "id": "Hanya satu garis di frekuensi nol." },
{ "en": "Jika sinyalnya impuls periodik (Dirac comb)?", "id": "Semua koefisien cₙ sama." },
{ "en": "Spektrum dari impuls periodik?", "id": "Juga sebuah impuls periodik." },
{ "en": "Apa keuntungan menggunakan simetri sinyal?", "id": "Menyederhanakan perhitungan koefisien Fourier." },
{ "en": "Simetri apa yang paling umum?", "id": "Genap, ganjil, dan setengah gelombang." },
{ "en": "Definisi fungsi genap (even)?", "id": "f(t) = f(-t)." },
{ "en": "Contoh fungsi genap?", "id": "Cosinus." },
{ "en": "Sinyal genap simetris terhadap sumbu?", "id": "Sumbu vertikal (y-axis)." },
{ "en": "Untuk sinyal genap, koefisien mana yang nol?", "id": "bₙ = 0 untuk semua n." },
{ "en": "Deret Fourier untuk sinyal genap hanya mengandung?", "id": "Suku DC dan cosinus." },
{ "en": "Integral sinyal genap dari -T/2 hingga T/2?", "id": "Dua kali integral dari 0 hingga T/2." },
{ "en": "Ini menyederhanakan rumus koefisien?", "id": "a₀ dan aₙ." },
{ "en": "Rumus aₙ untuk sinyal genap?", "id": "4/T integral f(t)cos(nω₀t) dari 0 ke T/2." },
{ "en": "Definisi fungsi ganjil (odd)?", "id": "f(t) = -f(-t)." },
{ "en": "Contoh fungsi ganjil?", "id": "Sinus." },
{ "en": "Sinyal ganjil simetris terhadap?", "id": "Titik asal (origin)." },
{ "en": "Untuk sinyal ganjil, koefisien mana yang nol?", "id": "a₀ dan aₙ = 0 untuk semua n." },
{ "en": "Deret Fourier untuk sinyal ganjil hanya mengandung?", "id": "Suku sinus." },
{ "en": "Nilai rata-rata (DC) sinyal ganjil?", "id": "Selalu nol." },
{ "en": "Integral sinyal ganjil selama satu periode simetris?", "id": "Selalu nol." },
{ "en": "Rumus bₙ untuk sinyal ganjil?", "id": "4/T integral f(t)sin(nω₀t) dari 0 ke T/2." },
{ "en": "Perkalian dua fungsi genap?", "id": "Fungsi genap." },
{ "en": "Perkalian dua fungsi ganjil?", "id": "Fungsi genap." },
{ "en": "Perkalian fungsi genap dan ganjil?", "id": "Fungsi ganjil." },
{ "en": "Sifat ini berguna saat mengalikan f(t) dengan?", "id": "Sinus atau cosinus dalam integral." },
{ "en": "Definisi simetri setengah gelombang?", "id": "f(t) = -f(t - T/2)." },
{ "en": "Apa artinya simetri setengah gelombang?", "id": "Setengah siklus kedua adalah negatif dari yang pertama." },
{ "en": "Konsekuensi simetri setengah gelombang?", "id": "Hanya mengandung harmonisa ganjil." },
{ "en": "Koefisien mana yang nol?", "id": "aₙ dan bₙ nol untuk n genap." },
{ "en": "Apakah komponen DC (a₀) terpengaruh?", "id": "Ya, a₀ juga nol." },
{ "en": "Rumus aₙ (n ganjil) untuk simetri setengah gelombang?", "id": "4/T integral f(t)cos(nω₀t) dari 0 ke T/2." },
{ "en": "Rumus bₙ (n ganjil) untuk simetri setengah gelombang?", "id": "4/T integral f(t)sin(nω₀t) dari 0 ke T/2." },
{ "en": "Contoh sinyal dengan simetri setengah gelombang?", "id": "Gelombang persegi dan segitiga." },
{ "en": "Apakah sinyal bisa punya lebih dari satu simetri?", "id": "Ya, contohnya genap dan setengah gelombang." },
{ "en": "Gelombang persegi (ganjil) memiliki simetri?", "id": "Ganjil dan setengah gelombang." },
{ "en": "Koefisien untuk gelombang persegi (ganjil)?", "id": "Hanya bₙ dengan n ganjil." },
{ "en": "Gelombang cosinus memiliki simetri?", "id": "Genap." },
{ "en": "Gelombang sinus memiliki simetri?", "id": "Ganjil." },
{ "en": "Sebuah fungsi f(t) bisa diurai menjadi?", "id": "Jumlah bagian genap dan ganjilnya." },
{ "en": "Bagian genap dari f(t)?", "id": "fe(t) = 1/2 [f(t) + f(-t)]." },
{ "en": "Bagian ganjil dari f(t)?", "id": "fo(t) = 1/2 [f(t) - f(-t)]." },
{ "en": "Deret Fourier dari fe(t) mengandung?", "id": "Suku a₀ dan aₙ dari f(t)." },
{ "en": "Deret Fourier dari fo(t) mengandung?", "id": "Suku bₙ dari f(t)." },
{ "en": "Simetri seperempat gelombang?", "id": "Simetri tambahan pada setengah periode." },
{ "en": "Simetri genap seperempat gelombang?", "id": "Simetri genap + simetri terhadap titik T/4." },
{ "en": "Konsekuensi simetri genap seperempat gelombang?", "id": "bₙ=0; aₙ=0 untuk n genap." },
{ "en": "Simetri ganjil seperempat gelombang?", "id": "Simetri ganjil + simetri terhadap titik T/4." },
{ "en": "Konsekuensi simetri ganjil seperempat gelombang?", "id": "aₙ=0; bₙ=0 untuk n genap." },
{ "en": "Gelombang gigi gergaji (sawtooth) punya simetri?", "id": "Tidak ada simetri genap/ganjil." },
{ "en": "Koefisien untuk gelombang gigi gergaji?", "id": "a₀, aₙ, dan bₙ ada." },
{ "en": "Gelombang segitiga punya simetri?", "id": "Genap dan setengah gelombang." },
{ "en": "Koefisien untuk gelombang segitiga?", "id": "Hanya aₙ dengan n ganjil." },
{ "en": "Menambahkan komponen DC ke sinyal?", "id": "Menggeser sinyal secara vertikal." },
{ "en": "Efek penambahan DC pada koefisien?", "id": "Hanya mengubah a₀." },
{ "en": "Apakah pergeseran waktu merusak simetri genap/ganjil?", "id": "Ya." },
{ "en": "Bagaimana memeriksa simetri sinyal?", "id": "Dengan mengamati grafiknya." },
{ "en": "Jika sinyal ganjil, cₙ bersifat?", "id": "Imajiner murni." },
{ "en": "Jika sinyal genap, cₙ bersifat?", "id": "Riil murni." },
{ "en": "Jika sinyal riil dan genap, bₙ?", "id": "bₙ = 0." },
{ "en": "Jika sinyal riil dan ganjil, aₙ?", "id": "aₙ = 0 (n≥0)." },
{ "en": "Simetri tersembunyi (hidden symmetry)?", "id": "Simetri setelah komponen DC dihilangkan." },
{ "en": "Sinyal f(t) = |sin(t)| punya simetri?", "id": "Genap dan setengah gelombang." },
{ "en": "Periode dari |sin(t)|?", "id": "Setengah dari periode sin(t)." },
{ "en": "Simetri rotasi?", "id": "Istilah lain untuk simetri ganjil." },
{ "en": "Simetri cermin?", "id": "Istilah lain untuk simetri genap." },
{ "en": "Penting untuk memilih T yang benar?", "id": "Ya, periode fundamental terkecil." },
{ "en": "Jika sinyal tidak punya simetri?", "id": "Semua koefisien mungkin ada." },
{ "en": "Perhitungan koefisien untuk sinyal tak simetris?", "id": "Harus menggunakan rumus umum." },
{ "en": "Apakah simetri selalu jelas terlihat?", "id": "Tidak, kadang perlu digeser." },
{ "en": "Jika sinyal punya simetri setengah gelombang, spektrumnya?", "id": "Tidak memiliki harmonisa genap." },
{ "en": "Ketiadaan harmonisa genap berguna dalam?", "id": "Sistem tenaga dan audio." },
{ "en": "Distorsi pada amplifier dapat menghilangkan?", "id": "Simetri sinyal asli." },
{ "en": "Penyearah setengah gelombang merusak simetri?", "id": "Ya, menghasilkan komponen DC dan harmonisa genap." },
{ "en": "Penyearah gelombang penuh (full-wave rectifier)?", "id": "Memiliki simetri genap." },
{ "en": "Output penyearah gelombang penuh tidak memiliki?", "id": "Harmonisa ganjil." },
{ "en": "Pengecekan simetri adalah langkah pertama yang baik?", "id": "Ya, sebelum melakukan integral." },
{ "en": "Jika bₙ=0, bentuk kompak φₙ?", "id": "0 atau 180 derajat." },
{ "en": "Jika aₙ=0, bentuk kompak φₙ?", "id": "-90 derajat." },
{ "en": "Apakah koefisien Fourier unik?", "id": "Ya, untuk sinyal tertentu." },
{ "en": "Jika sebuah sinyal tidak punya simetri apa pun?", "id": "Mungkin memiliki koefisien aₙ dan bₙ." },
{ "en": "Sinyal pulsa (pulse train) simetris terhadap?", "id": "Pusat pulsanya (jika dipilih sebagai t=0)." },
{ "en": "Jika pulsa simetris, deretnya mengandung?", "id": "Hanya suku cosinus." },
{ "en": "Integral fungsi genap kali ganjil?", "id": "Selalu nol." },
{ "en": "Pengecekan hasil: koefisien harus?", "id": "Berkurang amplitudonya saat n meningkat." },
{ "en": "Jika sinyal halus, koefisien?", "id": "Berkurang dengan cepat." },
{ "en": "Jika sinyal punya sudut tajam, koefisien?", "id": "Berkurang lebih lambat." },
{ "en": "Penggunaan simetri mengurangi jumlah?", "id": "Integral yang perlu dihitung." },
{ "en": "Gelombang persegi yang digeser vertikal?", "id": "Memiliki komponen DC (a₀)." },
{ "en": "Gelombang persegi yang digeser horizontal?", "id": "Memiliki suku sinus dan cosinus." },
{ "en": "Simetri genap pada koefisien kompleks?", "id": "Im(cₙ) = 0." },
{ "en": "Simetri ganjil pada koefisien kompleks?", "id": "Re(cₙ) = 0." },
{ "en": "Simetri setengah gelombang pada koefisien kompleks?", "id": "cₙ = 0 untuk n genap." },
{ "en": "Dapatkah kita membuat sinyal genap dari sinyal ganjil?", "id": "Ya, dengan menderivasikannya." },
{ "en": "Dapatkah kita membuat sinyal ganjil dari sinyal genap?", "id": "Ya, dengan menderivasikannya." },
{ "en": "Penting untuk memilih batas integrasi yang?", "id": "Menyederhanakan fungsi." },
{ "en": "Jika f(t) diskontinu, simetri tetap berlaku?", "id": "Ya." },
{ "en": "Simetri adalah konsep yang kuat dalam?", "id": "Fisika dan matematika." },
{ "en": "Apa itu sifat linieritas?", "id": "Deret dari penjumlahan adalah penjumlahan deret." },
{ "en": "Jika f(t) punya koefisien cₙ, af(t) punya?", "id": "Koefisien acₙ." },
{ "en": "Sifat ini berlaku untuk koefisien?", "id": "aₙ, bₙ, dan cₙ." },
{ "en": "Apa itu sifat pergeseran waktu (time shifting)?", "id": "Menggeser sinyal dalam waktu." },
{ "en": "Jika f(t) digeser menjadi f(t-t₀)?", "id": "Magnitudo koefisien tidak berubah." },
{ "en": "Apa yang berubah saat pergeseran waktu?", "id": "Sudut fasa koefisien." },
{ "en": "Koefisien cₙ baru menjadi?", "id": "cₙ lama dikali e^(-jnω₀t₀)." },
{ "en": "Pergeseran fasa yang ditambahkan?", "id": "-nω₀t₀." },
{ "en": "Apakah pergeseran waktu mempengaruhi spektrum amplitudo?", "id": "Tidak." },
{ "en": "Apakah pergeseran waktu mempengaruhi spektrum fasa?", "id": "Ya, menambahkan pergeseran fasa linier." },
{ "en": "Apa itu sifat penskalaan waktu (time scaling)?", "id": "Mengubah laju sinyal (mempercepat/memperlambat)." },
{ "en": "Jika f(t) menjadi f(at)?", "id": "Bentuk sinyal berubah, periode berubah." },
{ "en": "Periode baru dari f(at)?", "id": "T' = T/a." },
{ "en": "Frekuensi fundamental baru?", "id": "ω₀' = aω₀." },
{ "en": "Koefisien baru untuk f(at)?", "id": "Sama dengan koefisien lama, tapi spektrum melebar." },
{ "en": "Mempercepat sinyal di domain waktu?", "id": "Melebarkan spektrum di domain frekuensi." },
{ "en": "Memperlambat sinyal di domain waktu?", "id": "Menyempitkan spektrum di domain frekuensi." },
{ "en": "Apa itu sifat pembalikan waktu (time reversal)?", "id": "Mengganti t dengan -t." },
{ "en": "Jika f(t) punya koefisien cₙ, f(-t) punya?", "id": "Koefisien c₋ₙ." },
{ "en": "Pembalikan waktu sama dengan?", "id": "Membalik spektrum frekuensi." },
{ "en": "Untuk sinyal riil, c₋ₙ adalah?", "id": "Konjugat dari cₙ." },
{ "en": "Apa itu sifat diferensiasi (turunan)?", "id": "Mencari deret dari turunan sinyal." },
{ "en": "Jika f(t) punya koefisien cₙ, f'(t) punya?", "id": "Koefisien (jnω₀)cₙ." },
{ "en": "Diferensiasi di domain waktu menjadi?", "id": "Perkalian dengan jnω₀ di domain frekuensi." },
{ "en": "Efek diferensiasi pada spektrum amplitudo?", "id": "Menguatkan harmonisa frekuensi tinggi." },
{ "en": "Apa itu sifat integrasi?", "id": "Mencari deret dari integral sinyal." },
{ "en": "Jika f(t) punya koefisien cₙ, integralnya punya?", "id": "Koefisien cₙ / (jnω₀)." },
{ "en": "Integrasi di domain waktu menjadi?", "id": "Pembagian dengan jnω₀ di domain frekuensi." },
{ "en": "Efek integrasi pada spektrum amplitudo?", "id": "Melemahkan harmonisa frekuensi tinggi." },
{ "en": "Syarat untuk sifat integrasi?", "id": "Komponen DC (c₀) harus nol." },
{ "en": "Apa itu sifat perkalian (konvolusi)?", "id": "Deret dari perkalian dua sinyal." },
{ "en": "Perkalian di domain waktu menjadi?", "id": "Konvolusi diskrit di domain frekuensi." },
{ "en": "Jika f(t) punya cₙ dan g(t) punya dₙ?", "id": "Koefisien f(t)g(t) adalah Σ cₖdₙ₋ₖ." },
{ "en": "Operasi konvolusi dilambangkan dengan?", "id": "Tanda bintang (*)." },
{ "en": "Apa itu Teorema Konvolusi?", "id": "Menghubungkan perkalian dan konvolusi." },
{ "en": "Konvolusi di domain waktu menjadi?", "id": "Perkalian di domain frekuensi." },
{ "en": "Jika f(t)*g(t), koefisien barunya?", "id": "T * cₙ * dₙ." },
{ "en": "Sifat ini sangat penting dalam analisis?", "id": "Sistem LTI." },
{ "en": "Output sistem LTI adalah?", "id": "Konvolusi input dengan respons impuls." },
{ "en": "Apa itu sifat dualitas?", "id": "Hubungan simetris antara domain waktu dan frekuensi." },
{ "en": "Dualitas tidak berlaku langsung untuk?", "id": "Deret Fourier (tapi ya untuk Transformasi)." },
{ "en": "Teorema Parseval berhubungan dengan?", "id": "Konservasi daya." },
{ "en": "Apa isi Teorema Parseval?", "id": "Daya rata-rata di domain waktu = domain frekuensi." },
{ "en": "Daya rata-rata dari sinyal f(t)?", "id": "1/T integral |f(t)|² dt." },
{ "en": "Daya rata-rata dari koefisien cₙ?", "id": "Σ |cₙ|²." },
{ "en": "Rumus Teorema Parseval?", "id": "1/T integral |f(t)|² dt = Σ |cₙ|²." },
{ "en": "Plot |cₙ|² vs frekuensi disebut?", "id": "Spektrum daya." },
{ "en": "Spektrum daya menunjukkan distribusi?", "id": "Daya pada setiap harmonisa." },
{ "en": "Menggeser sinyal secara vertikal (menambah DC)?", "id": "Hanya mengubah koefisien c₀ (atau a₀)." },
{ "en": "Mengalikan sinyal dengan konstanta k?", "id": "Mengalikan semua koefisien dengan k." },
{ "en": "Sifat-sifat ini berguna untuk?", "id": "Mencari deret baru dari deret yang diketahui." },
{ "en": "Contoh: mencari deret gelombang segitiga dari?", "id": "Integrasi deret gelombang persegi." },
{ "en": "Pergeseran waktu 180 derajat (T/2)?", "id": "Mengalikan cₙ dengan e^(-jnπ)." },
{ "en": "e^(-jnπ) sama dengan?", "id": "(-1)ⁿ." },
{ "en": "Jika f(t) riil, maka |cₙ|?", "id": "Simetri genap." },
{ "en": "Jika f(t) riil, maka sudut cₙ?", "id": "Simetri ganjil." },
{ "en": "Jika f(t) imajiner murni, maka cₙ?", "id": "Memiliki simetri yang berbeda." },
{ "en": "Apa itu modulasi amplitudo (AM)?", "id": "Perkalian sinyal informasi dengan pembawa." },
{ "en": "Modulasi adalah contoh dari sifat?", "id": "Perkalian (konvolusi frekuensi)." },
{ "en": "Perkalian dengan cos(ωct) di domain waktu?", "id": "Menggeser spektrum ke ±ωc di frekuensi." },
{ "en": "Deret dari f(t)cos(ω₀t)?", "id": "1/2 [cₙ₋₁ + cₙ₊₁]." },
{ "en": "Ini adalah dasar dari?", "id": "Heterodyning atau mixing." },
{ "en": "Sifat-sifat ini menyederhanakan banyak?", "id": "Perhitungan matematis." },
{ "en": "Sifat turunan berguna untuk sinyal?", "id": "Dengan sudut tajam atau diskontinuitas." },
{ "en": "Deret dari turunan konvergen lebih?", "id": "Lambat." },
{ "en": "Sifat integrasi berguna untuk sinyal?", "id": "Yang lebih halus." },
{ "en": "Deret dari integral konvergen lebih?", "id": "Cepat." },
{ "en": "Sifat linieritas adalah sifat paling?", "id": "Fundamental." },
{ "en": "Jika f(t) = f₁(t) + f₂(t), maka aₙ?", "id": "aₙ₁ + aₙ₂." },
{ "en": "Diferensiasi berulang kali?", "id": "Mengalikan cₙ dengan (jnω₀)ᵏ." },
{ "en": "Sifat parseval untuk bentuk trigonometri?", "id": "Daya = a₀² + 1/2 Σ (aₙ² + bₙ²)." },
{ "en": "Jika sinyal diperlambat (a < 1), spektrum?", "id": "Menjadi lebih rapat." },
{ "en": "Jika sinyal dipercepat (a > 1), spektrum?", "id": "Menjadi lebih renggang." },
{ "en": "Pembalikan waktu f(-t) membalik sumbu?", "id": "Waktu." },
{ "en": "Efek f(-t) pada koefisien bₙ?", "id": "Mengubah tandanya (bₙ → -bₙ)." },
{ "en": "Efek f(-t) pada koefisien aₙ?", "id": "Tidak ada perubahan (aₙ → aₙ)." },
{ "en": "Frekuensi sesaat dari sinyal?", "id": "Turunan dari fasa terhadap waktu." },
{ "en": "Sifat-sifat ini berlaku untuk semua?", "id": "Sinyal periodik." },
{ "en": "Syarat agar Deret Fourier ada?", "id": "Kondisi Dirichlet." },
{ "en": "Apa saja Kondisi Dirichlet?", "id": "Bernilai tunggal, jumlah min/maks terbatas, diskontinuitas terbatas." },
{ "en": "Apakah semua fungsi periodik memenuhi ini?", "id": "Hampir semua fungsi dalam praktik." },
{ "en": "Contoh fungsi yang tidak memenuhi?", "id": "sin(1/t)." },
{ "en": "Apa itu konvergensi pointwise?", "id": "Deret konvergen ke f(t) di setiap titik." },
{ "en": "Di mana konvergensi pointwise terjadi?", "id": "Pada titik di mana f(t) kontinu." },
{ "en": "Bagaimana jika f(t) diskontinu (lompat)?", "id": "Deret konvergen ke titik tengah lompatan." },
{ "en": "Titik tengah lompatan dihitung sebagai?", "id": "1/2 [f(t⁺) + f(t⁻)]." },
{ "en": "Fenomena apa yang terjadi di dekat diskontinuitas?", "id": "Fenomena Gibbs." },
{ "en": "Apa itu Fenomena Gibbs?", "id": "Overshoot dan undershoot yang tidak hilang." },
{ "en": "Berapa besar overshoot pada Fenomena Gibbs?", "id": "Sekitar 9% dari tinggi lompatan." },
{ "en": "Apakah overshoot hilang dengan menambah suku?", "id": "Tidak, hanya menjadi lebih sempit." },
{ "en": "Fenomena Gibbs adalah sifat dari?", "id": "Konvergensi non-uniform." },
{ "en": "Apa itu konvergensi uniform?", "id": "Error aproksimasi mendekati nol secara seragam." },
{ "en": "Deret Fourier konvergen uniform jika?", "id": "Fungsi f(t) kontinu." },
{ "en": "Apa itu konvergensi dalam rata-rata kuadrat?", "id": "Energi dari error mendekati nol." },
{ "en": "Konvergensi ini juga disebut?", "id": "Konvergensi L²." },
{ "en": "Apakah Deret Fourier selalu konvergen dalam L²?", "id": "Ya, jika energinya terbatas." },
{ "en": "Tingkat konvergensi tergantung pada?", "id": "Kehalusan (smoothness) fungsi." },
{ "en": "Jika fungsi lebih halus, koefisien?", "id": "Menurun lebih cepat." },
{ "en": "Jika f(t) punya k turunan kontinu?", "id": "Koefisien |cₙ| menurun secepat 1/n^(k+1)." },
{ "en": "Gelombang persegi (diskontinu) koefisiennya?", "id": "Menurun secepat 1/n." },
{ "en": "Gelombang segitiga (kontinu, turunan diskontinu)?", "id": "Koefisiennya menurun secepat 1/n²." },
{ "en": "Sinyal yang lebih halus memiliki spektrum?", "id": "Yang lebih terkonsentrasi di frekuensi rendah." },
{ "en": "Sinyal dengan sudut tajam memiliki spektrum?", "id": "Yang lebih menyebar ke frekuensi tinggi." },
{ "en": "Pemotongan deret (truncation) setara dengan?", "id": "Filter lolos bawah ideal." },
{ "en": "Error pemotongan disebut juga?", "id": "Error Gibbs atau residual." },
{ "en": "Apakah Deret Fourier konvergen absolut?", "id": "Tidak selalu." },
{ "en": "Konvergensi absolut berarti?", "id": "Jumlah dari nilai absolut koefisien konvergen." },
{ "en": "Kondisi Dirichlet adalah kondisi?", "id": "Cukup (sufficient), bukan perlu (necessary)." },
{ "en": "Fenomena Gibbs menunjukkan keterbatasan dari?", "id": "Aproksimasi deret terpotong." },
{ "en": "Bagaimana mengurangi efek Gibbs?", "id": "Menggunakan teknik windowing." },
{ "en": "Apa itu fungsi jendela (window function)?", "id": "Fungsi yang memperhalus diskontinuitas." },
{ "en": "Contoh fungsi jendela?", "id": "Hamming, Hanning, Blackman." },
{ "en": "Windowing mengurangi overshoot tapi?", "id": "Memperlebar transisi." },
{ "en": "Deret Fourier dari sinyal yang terpotong?", "id": "Konvolusi spektrum sinyal dengan spektrum jendela." },
{ "en": "Spektrum dari jendela persegi?", "id": "Fungsi sinc." },
{ "en": "Side lobe pada fungsi sinc menyebabkan?", "id": "Kebocoran spektral (spectral leakage)." },
{ "en": "Kebocoran spektral adalah?", "id": "Energi menyebar ke frekuensi yang salah." },
{ "en": "Jendela selain persegi punya side lobe?", "id": "Lebih rendah." },
{ "en": "Konvergensi Deret Fourier adalah topik?", "id": "Penting dalam analisis matematis." },
{ "en": "Bagi insinyur, konvergensi L² seringkali?", "id": "Sudah cukup." },
{ "en": "Error rata-rata kuadrat terkecil didapat saat?", "id": "Menggunakan koefisien Fourier." },
{ "en": "Artinya, Deret Fourier adalah aproksimasi?", "id": "Terbaik dalam arti rata-rata kuadrat." },
{ "en": "Teorema Carleson menyatakan?", "id": "Deret Fourier konvergen pointwise hampir di mana-mana." },
{ "en": "Diskontinuitas pada sinyal menyebabkan harmonisa?", "id": "Berkontribusi signifikan di frekuensi tinggi." },
{ "en": "Kualitas audio digital tergantung pada?", "id": "Seberapa baik harmonisa tinggi direproduksi." },
{ "en": "Bandwidth sebuah sinyal adalah?", "id": "Rentang frekuensi signifikan yang dikandungnya." },
{ "en": "Sinyal periodik ideal punya bandwidth?", "id": "Tak hingga (karena ada semua harmonisa)." },
{ "en": "Dalam praktik, kita mengasumsikan bandwidth?", "id": "Terbatas (harmonisa tinggi diabaikan)." },
{ "en": "Aturan praktis untuk bandwidth?", "id": "Frekuensi di mana daya turun signifikan." },
{ "en": "Konvergensi adalah tentang perilaku deret saat?", "id": "Jumlah suku mendekati tak hingga." },
{ "en": "Fenomena Gibbs dinamai dari?", "id": "Josiah Willard Gibbs." },
{ "en": "Apakah Gibbs terjadi pada sinyal kontinu?", "id": "Tidak, hanya pada diskontinuitas." },
{ "en": "Lokasi overshoot Gibbs?", "id": "Sangat dekat dengan titik diskontinuitas." },
{ "en": "Apakah Fenomena Gibbs adalah error numerik?", "id": "Bukan, ini adalah sifat matematis." },
{ "en": "Jumlah minimum/maksimum terbatas berarti?", "id": "Tidak ada osilasi tak hingga." },
{ "en": "Bernilai tunggal (single-valued) berarti?", "id": "Satu nilai y untuk setiap x." },
{ "en": "Konsep konvergensi penting untuk validitas?", "id": "Representasi Deret Fourier." },
{ "en": "Ketidaksamaan Bessel menyatakan?", "id": "Energi dari koefisien ≤ energi sinyal." },
{ "en": "Kapan Ketidaksamaan Bessel menjadi kesamaan?", "id": "Jika himpunan basisnya lengkap (complete)." },
{ "en": "Kesamaan ini adalah?", "id": "Identitas Parseval." },
{ "en": "Apakah himpunan sinus/cosinus lengkap?", "id": "Ya, untuk ruang fungsi periodik." },
{ "en": "Apa itu basis ortonormal?", "id": "Basis ortogonal dengan norma satu." },
{ "en": "Fungsi yang tidak terbatas (unbounded)?", "id": "Contoh: tan(t)." },
{ "en": "Aproksimasi sinyal dengan N suku?", "id": "Disebut deret parsial ke-N." },
{ "en": "Error aproksimasi eN(t) = ?", "id": "f(t) - SN(t)." },
{ "en": "Konvergensi L² berarti integral |eN(t)|²?", "id": "Mendekati nol saat N tak hingga." },
{ "en": "Ruang fungsi L² adalah?", "id": "Ruang fungsi yang kuadratnya dapat diintegralkan." },
{ "en": "Apa itu Cesàro summation?", "id": "Metode untuk mengatasi masalah konvergensi." },
{ "en": "Rata-rata Fejér dari deret parsial?", "id": "Konvergen uniform ke f(t)." },
{ "en": "Kernel Dirichlet adalah?", "id": "Fungsi yang digunakan dalam deret parsial." },
{ "en": "Kernel Fejér adalah?", "id": "Rata-rata dari Kernel Dirichlet." },
{ "en": "Kernel Fejér selalu?", "id": "Non-negatif." },
{ "en": "Teorema Fejér menjamin konvergensi?", "id": "Yang lebih baik daripada konvergensi pointwise." },
{ "en": "Konvergensi dalam distribusi?", "id": "Konsep konvergensi yang lebih lemah." },
{ "en": "Deret dari delta Dirac konvergen?", "id": "Dalam arti distribusi." },
{ "en": "Apakah ada osilasi pada titik kontinu?", "id": "Tidak ada osilasi seperti Gibbs." },
{ "en": "Pemulusan (smoothing) sinyal di domain waktu?", "id": "Atenuasi frekuensi tinggi di domain frekuensi." },
{ "en": "Operasi ini setara dengan?", "id": "Filter lolos bawah." },
{ "en": "Penting untuk memahami jenis konvergensi yang?", "id": "Relevan dengan aplikasi." },
{ "en": "Diskontinuitas tak hingga jumlahnya?", "id": "Melanggar kondisi Dirichlet." },
{ "en": "Lebar dari 'tanduk' Gibbs?", "id": "Menyempit seiring penambahan suku." },
{ "en": "Amplitudo 'tanduk' Gibbs?", "id": "Tetap konstan." },
{ "en": "Apa itu daya rata-rata sinyal periodik?", "id": "Energi per satuan waktu." },
{ "en": "Rumus daya rata-rata di domain waktu?", "id": "P_avg = 1/T integral |f(t)|² dt." },
{ "en": "Teorema yang menghubungkan daya di dua domain?", "id": "Teorema Parseval." },
{ "en": "Pernyataan Teorema Parseval?", "id": "Daya di domain waktu sama dengan di frekuensi." },
{ "en": "Rumus Parseval untuk bentuk eksponensial?", "id": "P_avg = Σ |cₙ|² (n dari -∞ ke ∞)." },
{ "en": "Rumus Parseval untuk bentuk trigonometri?", "id": "P_avg = a₀² + 1/2 Σ (aₙ² + bₙ²)." },
{ "en": "Suku |cₙ|² merepresentasikan?", "id": "Daya rata-rata pada harmonisa ke-n." },
{ "en": "Plot dari |cₙ|² vs frekuensi?", "id": "Spektrum daya (power spectrum)." },
{ "en": "Spektrum daya menunjukkan distribusi?", "id": "Daya di antara berbagai harmonisa." },
{ "en": "Daya pada komponen DC?", "id": "c₀² atau a₀²." },
{ "en": "Daya pada harmonisa fundamental?", "id": "|c₁|² + |c₋₁|² = A₁²/2." },
{ "en": "Total daya adalah penjumlahan dari?", "id": "Daya setiap komponen harmonisa." },
{ "en": "Apakah ada daya silang antar harmonisa?", "id": "Tidak, karena ortogonalitas." },
{ "en": "Teorema Parseval adalah bentuk dari?", "id": "Konservasi energi." },
{ "en": "Satuan daya rata-rata?", "id": "Watt (jika sinyal adalah tegangan/arus)." },
{ "en": "Jika sinyal adalah tegangan V(t) melintasi resistor 1 Ohm?", "id": "Daya adalah P_avg." },
{ "en": "Jika resistornya R, dayanya?", "id": "P_avg / R." },
{ "en": "Spektrum daya selalu bernilai?", "id": "Riil dan non-negatif." },
{ "en": "Spektrum daya tidak mengandung informasi?", "id": "Fasa." },
{ "en": "Sinyal berbeda bisa punya spektrum daya sama?", "id": "Ya, jika hanya berbeda fasa." },
{ "en": "Apa itu Kepadatan Spektral Daya (PSD)?", "id": "Konsep untuk sinyal non-periodik." },
{ "en": "Hubungan PSD dengan spektrum daya?", "id": "Spektrum daya adalah versi diskrit dari PSD." },
{ "en": "Fungsi autokorelasi dari sinyal periodik?", "id": "Juga periodik dengan periode yang sama." },
{ "en": "Teorema Wiener-Khinchin menyatakan?", "id": "PSD adalah Transformasi Fourier dari autokorelasi." },
{ "en": "Untuk deret, spektrum daya adalah?", "id": "Deret Fourier dari fungsi autokorelasi." },
{ "en": "Daya pada frekuensi negatif?", "id": "Konstruksi matematis, daya fisik positif." },
{ "en": "Daya pada harmonisa ke-n (satu sisi)?", "id": "Aₙ²/2 (untuk n>0)." },
{ "en": "Mengapa ada faktor 1/2?", "id": "Karena nilai RMS = Amplitudo/akar(2)." },
{ "en": "Daya dari (Acos(ωt))?", "id": "(A/akar(2))² = A²/2." },
{ "en": "Jika sinyal melewati filter, spektrum dayanya?", "id": "Berubah." },
{ "en": "Jika sinyal melewati filter H(jω)?", "id": "Spektrum daya output = |H(jω)|² * spektrum input." },
{ "en": "|H(jω)|² disebut juga?", "id": "Respons magnitudo kuadrat." },
{ "en": "Filter lolos bawah akan mengurangi daya pada?", "id": "Harmonisa frekuensi tinggi." },
{ "en": "Daya total output akan?", "id": "Lebih kecil atau sama dengan daya input." },
{ "en": "Daya harmonisa fundamental digunakan untuk menghitung?", "id": "Distorsi Harmonisa Total (THD)." },
{ "en": "Daya distorsi adalah?", "id": "Total daya dari semua harmonisa non-fundamental." },
{ "en": "Rumus THD menggunakan daya?", "id": "Akar(P_distorsi / P_fundamental)." },
{ "en": "Parseval berguna untuk verifikasi?", "id": "Perhitungan koefisien Fourier." },
{ "en": "Jika perhitungan daya tidak cocok?", "id": "Ada kesalahan dalam menghitung koefisien." },
{ "en": "Daya sinyal gelombang persegi?", "id": "Amplitudo kuadrat." },
{ "en": "Daya sinyal riil bisa dihitung dari?", "id": "Hanya koefisien frekuensi non-negatif." },
{ "en": "Parseval untuk sinyal riil (satu sisi)?", "id": "P_avg = A₀² + 1/2 Σ Aₙ²." },
{ "en": "Daya sinyal kompleks f(t)?", "id": "Sama, menggunakan |f(t)|²." },
{ "en": "Fraksi daya pada harmonisa pertama?", "id": "P₁ / P_total." },
{ "en": "Bandwidth 90% daya?", "id": "Rentang frekuensi yang mengandung 90% daya total." },
{ "en": "Apakah fasa mempengaruhi daya rata-rata?", "id": "Tidak." },
{ "en": "Apakah pergeseran waktu mempengaruhi daya rata-rata?", "id": "Tidak." },
{ "en": "Daya rata-rata bersifat invarian terhadap?", "id": "Pergeseran waktu." },
{ "en": "Energi total sinyal periodik?", "id": "Tak hingga." },
{ "en": "Itulah mengapa kita menggunakan?", "id": "Daya rata-rata." },
{ "en": "Sinyal daya adalah sinyal dengan?", "id": "Daya rata-rata terbatas, energi tak hingga." },
{ "en": "Sinyal energi adalah sinyal dengan?", "id": "Energi terbatas, daya rata-rata nol." },
{ "en": "Sinusoida adalah contoh sinyal?", "id": "Sinyal daya." },
{ "en": "Pulsa tunggal adalah contoh sinyal?", "id": "Sinyal energi." },
{ "en": "Teorema Parseval berlaku untuk sinyal?", "id": "Sinyal daya." },
{ "en": "Versi untuk sinyal energi?", "id": "Teorema Rayleigh." },
{ "en": "Teorema Rayleigh menyatakan?", "id": "Energi di domain waktu = domain frekuensi." },
{ "en": "Daya rata-rata dari f(t) + g(t)?", "id": "Bukan jumlah daya jika tidak ortogonal." },
{ "en": "Daya rata-rata dari Σcₙe^(jnω₀t)?", "id": "Σ|cₙ|² karena basisnya ortogonal." },
{ "en": "Apa itu cross-power?", "id": "Interaksi daya antar frekuensi." },
{ "en": "Dalam Deret Fourier, cross-power?", "id": "Nol." },
{ "en": "Daya reaktif dalam konteks Fourier?", "id": "Tidak didefinisikan secara langsung." },
{ "en": "Spektrum daya juga disebut?", "id": "Spektrum Wiener." },
{ "en": "Perhitungan daya sangat penting dalam?", "id": "Teknik telekomunikasi dan tenaga listrik." },
{ "en": "Daya yang hilang dalam filter?", "id": "Daya input dikurangi daya output." },
{ "en": "Untuk filter pasif, daya output?", "id": "Selalu lebih kecil dari daya input." },
{ "en": "Untuk filter aktif, daya output?", "id": "Bisa lebih besar (karena ada sumber DC)." },
{ "en": "P_avg juga bisa ditulis sebagai?", "id": "Nilai RMS kuadrat." },
{ "en": "RMS dari f(t)?", "id": "Akar(P_avg)." },
{ "en": "RMS total = akar(Σ RMS_n²)?", "id": "Ya, benar." },
{ "en": "Daya komponen DC adalah?", "id": "DC² / R." },
{ "en": "Daya komponen AC adalah?", "id": "RMS_AC² / R." },
{ "en": "Daya total = Daya DC + Daya AC?", "id": "Ya." },
{ "en": "Aplikasi utama Deret Fourier?", "id": "Analisis sinyal dan sistem." },
{ "en": "Bagaimana filter dianalisis menggunakan Fourier?", "id": "Melihat bagaimana filter mempengaruhi setiap harmonisa." },
{ "en": "Fungsi transfer filter H(jω) adalah?", "id": "Respons filter terhadap frekuensi." },
{ "en": "Output filter = Input * H(jω)?", "id": "Ya, untuk setiap komponen frekuensi." },
{ "en": "Filter lolos bawah akan melemahkan?", "id": "Harmonisa frekuensi tinggi." },
{ "en": "Efeknya pada sinyal di domain waktu?", "id": "Memperhalus sinyal, menghilangkan sudut tajam." },
{ "en": "Filter lolos atas akan melemahkan?", "id": "Komponen DC dan harmonisa frekuensi rendah." },
{ "en": "Aplikasi dalam audio?", "id": "Equalizer (pengatur nada bass/treble)." },
{ "en": "Bass berhubungan dengan frekuensi?", "id": "Rendah." },
{ "en": "Treble berhubungan dengan frekuensi?", "id": "Tinggi." },
{ "en": "Aplikasi dalam sintesis suara?", "id": "Menjumlahkan sinusoida untuk membuat suara kompleks." },
{ "en": "Ini disebut sintesis?", "id": "Sintesis aditif." },
{ "en": "Aplikasi dalam kompresi data?", "id": "Menghilangkan harmonisa yang amplitudonya kecil." },
{ "en": "Contoh kompresi yang menggunakan ini?", "id": "Kompresi gambar JPEG dan audio MP3." },
{ "en": "JPEG menggunakan varian Fourier yang disebut?", "id": "Transformasi Cosinus Diskrit (DCT)." },
{ "en": "Aplikasi dalam menyelesaikan persamaan diferensial?", "id": "Mengubah PD menjadi persamaan aljabar." },
{ "en": "Ini sangat efektif untuk PD?", "id": "Linier dengan koefisien konstan." },
{ "en": "Input periodik ke sistem LTI?", "id": "Outputnya juga periodik." },
{ "en": "Respons keadaan tunak dari sistem LTI?", "id": "Ditemukan dengan mudah menggunakan Fourier." },
{ "en": "Aplikasi dalam analisis getaran (vibrasi)?", "id": "Mengidentifikasi frekuensi resonansi mekanis." },
{ "en": "Analisis spektrum getaran dapat mendeteksi?", "id": "Kerusakan pada mesin berputar." },
{ "en": "Aplikasi dalam astronomi?", "id": "Menganalisis periodisitas cahaya bintang." },
{ "en": "Aplikasi dalam analisis rangkaian listrik AC?", "id": "Untuk sumber tegangan non-sinusoidal." },
{ "en": "Daya pada beban dihitung untuk setiap?", "id": "Harmonisa, lalu dijumlahkan." },
{ "en": "Aplikasi dalam kristalografi?", "id": "Menentukan struktur kristal dari pola difraksi." },
{ "en": "Pola difraksi adalah terkait dengan?", "id": "Transformasi Fourier dari kisi kristal." },
{ "en": "Aplikasi dalam pemrosesan gambar?", "id": "Filter gambar, penajaman, pengurangan noise." },
{ "en": "Frekuensi tinggi pada gambar merepresentasikan?", "id": "Tepi dan detail." },
{ "en": "Frekuensi rendah pada gambar merepresentasikan?", "id": "Area yang warnanya halus." },
{ "en": "Filter lolos rendah pada gambar?", "id": "Membuat gambar kabur (blur)." },
{ "en": "Filter lolos tinggi pada gambar?", "id": "Menonjolkan tepi (edge detection)." },
{ "en": "Aplikasi dalam spektroskopi?", "id": "FTIR (Fourier Transform Infrared Spectroscopy)." },
{ "en": "FTIR menganalisis spektrum serapan?", "id": "Material kimia." },
{ "en": "Aplikasi dalam oseanografi?", "id": "Menganalisis data pasang surut dan gelombang." },
{ "en": "Pasang surut adalah fenomena?", "id": "Sangat periodik." },
{ "en": "Aplikasi dalam ekonomi?", "id": "Mencari siklus dalam data pasar saham." },
{ "en": "Aplikasi dalam biologi?", "id": "Menganalisis sekuens DNA." },
{ "en": "Alat yang menghitung spektrum secara real-time?", "id": "Penganalisis spektrum (spectrum analyzer)." },
{ "en": "Aplikasi dalam telekomunikasi?", "id": "Modulasi dan multiplexing." },
{ "en": "Frequency Division Multiplexing (FDM)?", "id": "Menempatkan sinyal berbeda di pita frekuensi berbeda." },
{ "en": "Setiap saluran radio atau TV menempati?", "id": "Sebuah pita frekuensi." },
{ "en": "Demodulasi sinyal AM melibatkan?", "id": "Ekstraksi sinyal frekuensi rendah." },
{ "en": "Apa itu 'noise shaping'?", "id": "Memindahkan daya noise ke frekuensi lain." },
{ "en": "Aplikasi dalam akustik ruangan?", "id": "Menganalisis mode resonansi ruangan." },
{ "en": "Aplikasi dalam geofisika?", "id": "Analisis data seismik." },
{ "en": "Gelombang seismik adalah sinyal?", "id": "Kompleks yang dapat dianalisis." },
{ "en": "Aplikasi dalam studi cuaca?", "id": "Mencari pola periodik dalam data iklim." },
{ "en": "Aplikasi pada electrocardiogram (ECG)?", "id": "Mendeteksi kelainan irama jantung." },
{ "en": "Sinyal ECG jantung normal adalah?", "id": "Sinyal periodik." },
{ "en": "Aplikasi pada electroencephalogram (EEG)?", "id": "Menganalisis gelombang otak." },
{ "en": "Gelombang alfa, beta, delta pada EEG?", "id": "Pita frekuensi yang berbeda." },
{ "en": "Dasar dari 'vocoder' suara?", "id": "Analisis dan sintesis spektral." },
{ "en": "Aplikasi dalam pembatalan gema (echo cancellation)?", "id": "Memfilter sinyal gema." },
{ "en": "Aplikasi dalam pengenalan suara?", "id": "Mengekstrak fitur spektral dari suara." },
{ "en": "Deret Fourier adalah fondasi untuk?", "id": "Banyak teknologi modern." },
{ "en": "Memahami spektrum sinyal memungkinkan kita?", "id": "Memanipulasi dan memprosesnya." },
{ "en": "Penggunaan dalam sistem kontrol?", "id": "Analisis respons frekuensi (plot Bode)." },
{ "en": "Analisis kestabilan sistem umpan balik?", "id": "Menggunakan plot Nyquist." },
{ "en": "Menghilangkan 'hum' 50/60 Hz dari audio?", "id": "Menggunakan filter tolak pita (notch filter)." },
{ "en": "Spektrogram adalah plot?", "id": "Spektrum vs waktu." },
{ "en": "Spektrogram berguna untuk menganalisis sinyal?", "id": "Yang spektrumnya berubah seiring waktu." },
{ "en": "Contohnya adalah sinyal?", "id": "Ucapan atau musik." },
{ "en": "Fourier mengubah masalah kalkulus menjadi?", "id": "Masalah aljabar." },
{ "en": "Filter analog dirancang dalam domain?", "id": "Frekuensi." },
{ "en": "Aplikasi dalam mekanika kuantum?", "id": "Menghubungkan posisi dan momentum." },
{ "en": "Prinsip ketidakpastian Heisenberg terkait dengan?", "id": "Sifat Transformasi Fourier." },
{ "en": "Penyempitan di domain waktu menyebabkan?", "id": "Pelebaran di domain frekuensi." },
{ "en": "Aplikasi pada radar Doppler?", "id": "Mengukur kecepatan dari pergeseran frekuensi." },
{ "en": "Aplikasi pada pencitraan medis (MRI)?", "id": "Membangun kembali gambar dari data frekuensi." },
{ "en": "Data mentah MRI berada di domain?", "id": "Frekuensi (k-space)." },
{ "en": "Gambar akhir MRI didapat dengan?", "id": "Transformasi Fourier 2D." },
{ "en": "Aplikasi dalam optik?", "id": "Pola difraksi Fraunhofer." },
{ "en": "Pola difraksi adalah?", "id": "Transformasi Fourier dari celah." },
{ "en": "Koefisien bₙ untuk gelombang persegi ganjil?", "id": "4A/nπ untuk n ganjil, 0 lainnya." },
{ "en": "Koefisien aₙ untuk gelombang segitiga genap?", "id": "8A/(nπ)² untuk n ganjil, 0 lainnya." },
{ "en": "Deret untuk gelombang gigi gergaji?", "id": "Mengandung suku sinus dengan tanda bolak-balik." },
{ "en": "Deret untuk pulsa periodik (pulse train)?", "id": "Koefisien cₙ berbentuk fungsi sinc." },
{ "en": "Apa itu siklus kerja (duty cycle)?", "id": "Rasio lebar pulsa terhadap periode." },
{ "en": "Siklus kerja mempengaruhi?", "id": "Amplitudo koefisien (bentuk sinc)." },
{ "en": "Jika siklus kerja 50%, harmonisa mana yang hilang?", "id": "Semua harmonisa genap." },
{ "en": "Ini karena sinyalnya memiliki simetri?", "id": "Setengah gelombang." },
{ "en": "Deret Fourier untuk delta Dirac periodik?", "id": "Semua koefisien cₙ bernilai sama." },
{ "en": "Spektrum dari delta Dirac periodik?", "id": "Juga delta Dirac periodik." },
{ "en": "Hubungan Deret dan Transformasi Fourier?", "id": "Deret adalah kasus khusus untuk sinyal periodik." },
{ "en": "Transformasi Fourier dari sinyal periodik?", "id": "Serangkaian impuls di frekuensi harmonisa." },
{ "en": "Amplitudo impuls sebanding dengan?", "id": "Koefisien cₙ." },
{ "en": "Deret Fourier Waktu Diskrit (DFTS)?", "id": "Untuk sinyal sekuens periodik." },
{ "en": "Transformasi Fourier Waktu Diskrit (DTFT)?", "id": "Untuk sinyal sekuens non-periodik." },
{ "en": "Spektrum dari DTFT bersifat?", "id": "Kontinu dan periodik." },
{ "en": "Transformasi Fourier Diskrit (DFT)?", "id": "Versi diskrit dari Deret Fourier." },
{ "en": "DFT mengambil sampel dari?", "id": "Satu periode sinyal waktu diskrit." },
{ "en": "Hasil dari DFT?", "id": "Sampel dari spektrum frekuensi." },
{ "en": "DFT adalah dasar dari?", "id": "Algoritma FFT." },
{ "en": "Transformasi Z adalah generalisasi dari?", "id": "DTFT." },
{ "en": "Jika z = e^(jω) pada Transformasi Z?", "id": "Kita mendapatkan DTFT." },
{ "en": "Transformasi Laplace adalah generalisasi dari?", "id": "Transformasi Fourier." },
{ "en": "Jika s = jω pada Transformasi Laplace?", "id": "Kita mendapatkan Transformasi Fourier." },
{ "en": "Deret Fourier Walsh-Hadamard menggunakan fungsi?", "id": "Fungsi persegi (bukan sinusoida)." },
{ "en": "Basis Walsh-Hadamard juga?", "id": "Ortogonal." },
{ "en": "Deret Fourier Generalized?", "id": "Menggunakan himpunan basis ortogonal apa pun." },
{ "en": "Contoh basis lain?", "id": "Polinomial Legendre, Chebyshev." },
{ "en": "Deret Fourier pada interval terbatas?", "id": "Sinyal dianggap periodik di luar interval itu." },
{ "en": "Ini disebut ekspansi?", "id": "Ekspansi setengah rentang (half-range)." },
{ "en": "Ekspansi sinus setengah rentang?", "id": "Membuat sinyal menjadi ganjil." },
{ "en": "Ekspansi cosinus setengah rentang?", "id": "Membuat sinyal menjadi genap." },
{ "en": "Masalah Sturm-Liouville berkaitan dengan?", "id": "Menemukan himpunan fungsi eigen ortogonal." },
{ "en": "Fungsi sinus dan cosinus adalah solusi dari?", "id": "Masalah Sturm-Liouville sederhana." },
{ "en": "Deret Fourier dalam 2D atau 3D?", "id": "Digunakan dalam pemrosesan gambar dan kristalografi." },
{ "en": "Basis untuk Deret Fourier 2D?", "id": "Produk dari fungsi sinus/cosinus." },
{ "en": "Apa itu windowing?", "id": "Mengalikan sinyal dengan fungsi jendela." },
{ "en": "Tujuan windowing?", "id": "Mengurangi kebocoran spektral." },
{ "en": "Efek samping windowing?", "id": "Mengurangi resolusi frekuensi." },
{ "en": "Apa itu zero-padding?", "id": "Menambahkan nol di akhir sinyal." },
{ "en": "Tujuan zero-padding?", "id": "Meningkatkan kepadatan titik DFT (interpolasi)." },
{ "en": "Apakah zero-padding meningkatkan resolusi asli?", "id": "Tidak." },
{ "en": "Konvolusi sirkular vs linier?", "id": "Sirkular muncul dari perkalian DFT." },
{ "en": "Metode untuk menghitung konvolusi linier pakai DFT?", "id": "Metode overlap-add atau overlap-save." },
{ "en": "Apa itu cepstrum?", "id": "Transformasi Fourier dari logaritma spektrum." },
{ "en": "Cepstrum berguna untuk?", "id": "Mendeteksi periodisitas dalam spektrum." },
{ "en": "Aplikasi cepstrum?", "id": "Analisis ucapan (pitch detection)." },
{ "en": "Hilbert Transform menghasilkan?", "id": "Sinyal analitik." },
{ "en": "Sinyal analitik adalah sinyal?", "id": "Kompleks yang spektrum frekuensi negatifnya nol." },
{ "en": "Wavelet Transform adalah alternatif dari?", "id": "Analisis Fourier." },
{ "en": "Kelebihan Wavelet?", "id": "Memberikan resolusi waktu dan frekuensi yang baik." },
{ "en": "Fourier baik untuk sinyal?", "id": "Stasioner." },
{ "en": "Wavelet baik untuk sinyal?", "id": "Non-stasioner." },
{ "en": "Fungsi sinc adalah Transformasi Fourier dari?", "id": "Pulsa persegi." },
{ "en": "Fungsi persegi adalah Transformasi Fourier dari?", "id": "Fungsi sinc." },
{ "en": "Ini adalah contoh dari sifat?", "id": "Dualitas." },
{ "en": "Deret Fourier dari f(t)²?", "id": "Konvolusi dari koefisien cₙ dengan dirinya sendiri." },
{ "en": "Jika sinyal kausal, bagian riil dan imajiner spektrum?", "id": "Saling terkait (relasi Kramers-Kronig)." },
{ "en": "Amplitudo harmonisa ke-n dari pulsa periodik?", "id": "|cₙ| = Aτ/T |sinc(nω₀τ/2)|." },
{ "en": "Apa itu τ (tau)?", "id": "Lebar pulsa." },
{ "en": "Nol dari fungsi sinc terjadi saat?", "id": "Argumennya kelipatan bulat dari π." },
{ "en": "Ini menyebabkan harmonisa tertentu?", "id": "Hilang dari spektrum." },
{ "en": "Spektrum pulsa periodik memiliki bentuk?", "id": "Amplop fungsi sinc." },
{ "en": "Jika lebar pulsa menyempit, spektrum?", "id": "Melebar." },
{ "en": "Jika pulsa menjadi impuls (τ→0)?", "id": "Spektrum menjadi datar." },
{ "en": "Jika periode T melebar (T→∞)?", "id": "Garis spektrum menjadi sangat rapat." },
{ "en": "Limit saat T→∞ adalah?", "id": "Transformasi Fourier." },
{ "en": "Spektrum daya dari gelombang persegi?", "id": "Menurun secepat 1/n²." },
{ "en": "Spektrum daya dari gelombang segitiga?", "id": "Menurun secepat 1/n⁴." },
{ "en": "Apakah ada Deret Taylor untuk sinyal?", "id": "Ya, tapi aproksimasi lokal." },
{ "en": "Deret Fourier memberikan aproksimasi?", "id": "Global (selama satu periode)." },
{ "en": "Ruang abstrak tempat fungsi sinyal berada?", "id": "Ruang Hilbert." },
{ "en": "Himpunan fungsi basis ortogonal membentuk?", "id": "Basis untuk ruang fungsi." },
{ "en": "Proyeksi sinyal ke fungsi basis menghasilkan?", "id": "Koefisien Fourier." },
{ "en": "Operasi yang digunakan untuk proyeksi ini?", "id": "Integral hasil kali dalam (inner product)." },
{ "en": "Deret Fourier adalah ekspansi dalam basis?", "id": "Basis Fourier (sinusoida)." },
{ "en": "Apakah basis Fourier satu-satunya basis ortogonal?", "id": "Tidak, ada banyak basis lain." },
{ "en": "Contoh basis ortogonal selain Fourier?", "id": "Polinomial Legendre." },
{ "en": "Jika sinyal periodik dikalikan sinyal periodik lain?", "id": "Periode hasilnya adalah KPK dari periode." },
{ "en": "Spektrum dari sinyal yang di-window?", "id": "Konvolusi spektrum sinyal dan spektrum window." },
{ "en": "Jendela (window) rektangular menyebabkan kebocoran spektral?", "id": "Paling besar." },
{ "en": "Jendela Hanning mengurangi kebocoran tapi?", "id": "Memperlebar lobus utama (resolusi lebih rendah)." },
{ "en": "Resolusi frekuensi dalam DFT ditentukan oleh?", "id": "Panjang total waktu observasi." },
{ "en": "Interpolasi spektrum DFT dilakukan dengan?", "id": "Zero-padding." },
{ "en": "Apakah zero-padding menambah informasi baru?", "id": "Tidak, hanya menghaluskan tampilan spektrum." },
{ "en": "Kompleksitas komputasi DFT langsung?", "id": "O(N²)." },
{ "en": "Kompleksitas komputasi algoritma FFT?", "id": "O(N log N)." },
{ "en": "FFT adalah implementasi efisien dari?", "id": "Transformasi Fourier Diskrit (DFT)." },
{ "en": "Syarat panjang sinyal untuk FFT dasar?", "id": "Pangkat dua (power of two)." },
{ "en": "Algoritma FFT yang umum?", "id": "Algoritma Cooley-Tukey." },
{ "en": "Deret Fourier untuk sinyal hasil penyearahan setengah gelombang?", "id": "Mengandung komponen DC dan semua harmonisa." },
{ "en": "Deret Fourier untuk sinyal hasil penyearahan gelombang penuh?", "id": "Mengandung DC dan harmonisa genap." },
{ "en": "Sinyal suara vokal (misal 'aah') bersifat?", "id": "Sangat periodik." },
{ "en": "Frekuensi fundamental suara vokal menentukan?", "id": "Nada (pitch) suara." },
{ "en": "Harmonisa suara vokal menentukan?", "id": "Warna suara (timbre)." },
{ "en": "Forman dalam spektrum suara adalah?", "id": "Puncak resonansi saluran vokal." },
{ "en": "Mengapa |cₙ|² digunakan untuk daya?", "id": "Karena daya sebanding dengan amplitudo kuadrat." },
{ "en": "Sinyal pita terbatas (band-limited)?", "id": "Koefisien Fourier nol di atas frekuensi tertentu." },
{ "en": "Bisakah sinyal terbatas waktu menjadi pita terbatas?", "id": "Tidak mungkin secara matematis." },
{ "en": "Ini adalah pernyataan dari?", "id": "Prinsip ketidakpastian Fourier." },
{ "en": "Jika sinyal hanya punya N harmonisa?", "id": "Dapat direkonstruksi sempurna dari 2N+1 sampel." },
{ "en": "Apa itu Gibb's ears?", "id": "Nama lain untuk overshoot pada Fenomena Gibbs." },
{ "en": "Deret Fourier dapat digunakan untuk mengaproksimasi?", "id": "Bentuk geometris seperti lingkaran." },
{ "en": "Bagaimana filter mengubah deret Fourier?", "id": "Mengalikan setiap koefisien dengan gain filter." },
{ "en": "Jika filter melemahkan frekuensi tinggi?", "id": "Koefisien cₙ untuk n besar menjadi kecil." },
{ "en": "Konsep 'dekat' dalam konvergensi L²?", "id": "Jarak rata-rata kuadrat kecil." },
{ "en": "Deret Fourier adalah alat analisis?", "id": "Non-parametrik." },
{ "en": "Apa itu 'phase vocoder'?", "id": "Alat untuk memanipulasi fasa dan magnitudo." },
{ "en": "Phase vocoder dapat mengubah?", "id": "Pitch tanpa mengubah tempo." },
{ "en": "Apa itu autotune?", "id": "Menggeser harmonisa agar sesuai nada." },
{ "en": "Deret Fourier dalam sistem koordinat polar?", "id": "Berguna untuk bentuk melingkar." },
{ "en": "Efek diferensiasi pada komponen DC?", "id": "Menghilangkannya (menjadi nol)." },
{ "en": "Efek integrasi jika komponen DC ada?", "id": "Menghasilkan suku linear (ramp)." },
{ "en": "Untuk sinyal riil, a₋ₙ = ?", "id": "aₙ." },
{ "en": "Untuk sinyal riil, b₋ₙ = ?", "id": "-bₙ." },
{ "en": "Representasi ini menunjukkan aₙ adalah fungsi?", "id": "Genap dari n." },
{ "en": "Representasi ini menunjukkan bₙ adalah fungsi?", "id": "Ganjil dari n." },
{ "en": "Sinyal analitik didapat dari?", "id": "Menghilangkan spektrum frekuensi negatif." },
{ "en": "Sinyal analitik berguna untuk?", "id": "Menghitung amplitudo dan fasa sesaat." },
{ "en": "Apa itu 'aliasing'?", "id": "Salah tafsir frekuensi akibat sampling rendah." },
{ "en": "Syarat sampling Nyquist-Shannon?", "id": "Frekuensi sampling > 2 kali bandwidth." },
{ "en": "Ini berlaku untuk sinyal?", "id": "Pita terbatas (band-limited)." },
{ "en": "Deret Fourier menyiratkan sinyal periodik tidak?", "id": "Pita terbatas (band-limited)." },
{ "en": "Jadi, sampling sinyal periodik selalu menyebabkan?", "id": "Aliasing (secara teori)." },
{ "en": "Dalam praktik, aliasing diminimalkan dengan?", "id": "Filter anti-aliasing sebelum sampling." },
{ "en": "Apa itu filter anti-aliasing?", "id": "Filter lolos bawah." },
{ "en": "Daya total dari sinyal f(t) + g(t)?", "id": "P_f + P_g jika f dan g ortogonal." },
{ "en": "Kapan sinyal periodik ortogonal?", "id": "Jika tidak ada harmonisa yang tumpang tindih." },
{ "en": "Analisis Fourier pada data keuangan?", "id": "Mencari siklus musiman atau ekonomi." },
{ "en": "Deret Fourier dari sinyal yang dikuantisasi?", "id": "Memperkenalkan noise kuantisasi." },
{ "en": "Noise kuantisasi menyebar ke seluruh?", "id": "Spektrum frekuensi." },
{ "en": "Apa itu 'jitter'?", "id": "Variasi waktu pada sinyal periodik." },
{ "en": "Efek jitter pada spektrum?", "id": "Menyebabkan noise fasa." },
{ "en": "Noise fasa tampak seperti?", "id": "'Rok' di sekitar garis spektrum." },
{ "en": "Apa itu 'harmonics' dalam musik rock?", "id": "Getaran senar gitar yang dipaksa." },
{ "en": "Apa itu 'undertone series'?", "id": "Konsep teoretis, kebalikan dari 'overtone'." },
{ "en": "Spektrum dari sinyal AM (modulasi amplitudo)?", "id": "Pembawa (carrier) dan dua sideband." },
{ "en": "Spektrum dari sinyal FM (modulasi frekuensi)?", "id": "Lebih kompleks, banyak sideband (fungsi Bessel)." },
{ "en": "Apa itu 'deterministic signal'?", "id": "Sinyal yang bisa diprediksi, tanpa keacakan." },
{ "en": "Deret Fourier untuk sinyal?", "id": "Deterministik dan periodik." },
{ "en": "Analisis spektral untuk sinyal acak?", "id": "Menggunakan Kepadatan Spektral Daya (PSD)." },
{ "en": "Estimasi PSD dari data?", "id": "Periodogram." },
{ "en": "Metode Welch untuk estimasi PSD?", "id": "Merata-ratakan periodogram yang tumpang tindih." },
{ "en": "Apa itu 'cross-correlation'?", "id": "Mengukur kemiripan dua sinyal berbeda." },
{ "en": "Transformasi Fourier dari cross-correlation?", "id": "Cross-power spectral density." },
{ "en": "Sistem LTI mengubah fasa setiap harmonisa?", "id": "Secara berbeda, sesuai sudut H(jω)." },
{ "en": "Ini dapat menyebabkan distorsi?", "id": "Distorsi fasa." },
{ "en": "Jika fasa adalah fungsi linier dari frekuensi?", "id": "Tidak ada distorsi, hanya pergeseran waktu." },
{ "en": "Ini disebut fasa?", "id": "Fasa linier." },
{ "en": "Filter FIR dapat dirancang untuk memiliki?", "id": "Fasa linier sempurna." },
{ "en": "Hubungan antara |cₙ| dan laju peluruhan?", "id": "Semakin cepat meluruh, sinyal semakin halus." },
{ "en": "Konstanta waktu sebuah sistem berhubungan dengan?", "id": "Lokasi pole di domain-s." },
{ "en": "Pole yang lebih jauh dari sumbu jω?", "id": "Konstanta waktu lebih kecil." },
{ "en": "Konstanta waktu kecil berarti transien?", "id": "Cepat hilang." },
{ "en": "Bagaimana jika periode T sangat besar?", "id": "Frekuensi fundamental menjadi sangat kecil." },
{ "en": "Jarak antar harmonisa menjadi?", "id": "Sangat rapat." },
{ "en": "Saat T mendekati tak hingga, spektrum menjadi?", "id": "Kontinu (Transformasi Fourier)." },
{ "en": "Deret Fourier adalah alat fundamental untuk?", "id": "Memahami sinyal dari perspektif frekuensi." }



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
