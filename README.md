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


  { "en": "Apa partikel subatomik yang memiliki muatan negatif?", "id": "Elektron." },
  { "en": "Apa yang dimaksud dengan arus listrik?", "id": "Aliran muatan listrik, biasanya elektron, melalui sebuah konduktor." },
  { "en": "Apa satuan untuk mengukur arus listrik?", "id": "Ampere (A)." },
  { "en": "Apa yang dimaksud dengan tegangan listrik?", "id": "Perbedaan potensial listrik antara dua titik yang menyebabkan arus mengalir." },
  { "en": "Apa satuan untuk mengukur tegangan listrik?", "id": "Volt (V)." },
  { "en": "Apa yang dimaksud dengan resistansi?", "id": "Ukuran hambatan terhadap aliran arus listrik dalam sebuah sirkuit." },
  { "en": "Apa satuan untuk mengukur resistansi?", "id": "Ohm (Ω)." },
  { "en": "Apa yang dijelaskan oleh Hukum Ohm?", "id": "Hubungan antara tegangan, arus, dan resistansi dalam sebuah sirkuit listrik." },
  { "en": "Disebut apakah bahan yang dapat menghantarkan listrik dengan mudah?", "id": "Konduktor." },
  { "en": "Sebutkan contoh bahan konduktor!", "id": "Tembaga, perak, emas, dan aluminium." },
  { "en": "Disebut apakah bahan yang tidak dapat menghantarkan listrik?", "id": "Isolator atau dielektrik." },
  { "en": "Sebutkan contoh bahan isolator!", "id": "Karet, plastik, kaca, dan kayu kering." },
  { "en": "Disebut apakah bahan yang sifatnya berada di antara konduktor dan isolator?", "id": "Semikonduktor." },
  { "en": "Sebutkan contoh bahan semikonduktor yang paling umum!", "id": "Silikon (Si) dan Germanium (Ge)." },
  { "en": "Apa perbedaan mendasar antara arus DC dan AC?", "id": "Arus DC (Direct Current) mengalir dalam satu arah, sedangkan arus AC (Alternating Current) arahnya bolak-balik secara periodik." },
  { "en": "Sumber tegangan DC yang paling umum ditemukan adalah?", "id": "Baterai dan aki." },
  { "en": "Sumber tegangan AC yang paling umum adalah?", "id": "Jaringan listrik PLN (genset atau pembangkit listrik)." },
  { "en": "Apa fungsi dasar dari sebuah resistor?", "id": "Untuk menghambat atau membatasi aliran arus listrik." },
  { "en": "Disebut apakah resistor yang nilainya dapat diubah-ubah?", "id": "Resistor variabel atau potensiometer." },
  { "en": "Apa fungsi utama dari potensiometer dalam sebuah sirkuit?", "id": "Untuk mengatur level sinyal, seperti volume suara atau tingkat kecerahan lampu." },
  { "en": "Apa yang ditunjukkan oleh gelang-gelang warna pada sebuah resistor?", "id": "Nilai resistansi dan toleransinya." },
  { "en": "Apa fungsi dari gelang toleransi pada resistor?", "id": "Menunjukkan persentase seberapa besar nilai resistansi sebenarnya bisa berbeda dari nilai yang tertera." },
  { "en": "Apa itu LDR (Light Dependent Resistor)?", "id": "Resistor yang nilai resistansinya bergantung pada intensitas cahaya yang diterimanya." },
  { "en": "Bagaimana karakteristik LDR terhadap cahaya?", "id": "Semakin terang cahaya yang mengenainya, semakin kecil nilai resistansinya." },
  { "en": "Apa itu termistor?", "id": "Resistor yang nilai resistansinya sangat peka terhadap perubahan suhu." },
  { "en": "Apa perbedaan antara termistor NTC dan PTC?", "id": "NTC (Negative Temperature Coefficient) resistansinya turun saat suhu naik, sedangkan PTC (Positive Temperature Coefficient) resistansinya naik saat suhu naik." },
  { "en": "Apa fungsi dasar dari sebuah kapasitor?", "id": "Untuk menyimpan muatan listrik sementara." },
  { "en": "Apa satuan dari kapasitansi?", "id": "Farad (F)." },
  { "en": "Mengapa kapasitor elektrolit (elco) memiliki polaritas?", "id": "Karena konstruksi internalnya yang menggunakan lapisan oksida dielektrik yang harus diberi tegangan dengan benar agar tidak rusak." },
  { "en": "Apa yang terjadi jika kapasitor elektrolit dipasang terbalik?", "id": "Kapasitor bisa menjadi panas, menggelembung, atau bahkan meledak." },
  { "en": "Apa fungsi kapasitor dalam rangkaian catu daya (power supply)?", "id": "Sebagai filter atau perata untuk menghaluskan tegangan DC setelah proses penyearahan." },
  { "en": "Apa fungsi kapasitor sebagai 'kopling' (coupling capacitor)?", "id": "Untuk melewatkan sinyal AC dari satu tingkat rangkaian ke tingkat berikutnya sambil memblokir komponen DC." },
  { "en": "Disebut apakah kapasitor yang nilainya bisa diubah?", "id": "Kapasitor variabel atau Varco (Variable Capacitor)." },
  { "en": "Di manakah kapasitor variabel sering digunakan?", "id": "Dalam rangkaian penala (tuner) radio untuk memilih frekuensi." },
  { "en": "Apa bahan dielektrik yang umum pada kapasitor non-polar?", "id": "Keramik, mika, dan poliester." },
  { "en": "Apa fungsi dasar dari sebuah induktor?", "id": "Menyimpan energi dalam bentuk medan magnet ketika dialiri arus listrik." },
  { "en": "Apa satuan dari induktansi?", "id": "Henry (H)." },
  { "en": "Terbuat dari apa sebuah induktor sederhana?", "id": "Kawat lilitan (koil) yang biasanya dililitkan pada inti (core)." },
  { "en": "Apa fungsi inti (core) pada induktor?", "id": "Untuk meningkatkan nilai induktansi dengan memusatkan medan magnet." },
  { "en": "Apa fungsi induktor sebagai 'choke'?", "id": "Untuk menahan (memblokir) sinyal AC berfrekuensi tinggi dan melewatkan sinyal DC atau AC berfrekuensi rendah." },
  { "en": "Apa itu transformator atau trafo?", "id": "Komponen yang menggunakan prinsip induksi elektromagnetik untuk menaikkan atau menurunkan tegangan AC." },
  { "en": "Apa nama lilitan pada trafo tempat masuknya tegangan?", "id": "Lilitan primer." },
  { "en": "Apa nama lilitan pada trafo tempat keluarnya tegangan?", "id": "Lilitan sekunder." },
  { "en": "Apa fungsi trafo step-down?", "id": "Untuk menurunkan tegangan AC." },
  { "en": "Apa fungsi trafo step-up?", "id": "Untuk menaikkan tegangan AC." },
  { "en": "Apakah transformator dapat bekerja pada tegangan DC?", "id": "Tidak, karena transformator memerlukan perubahan medan magnet yang hanya dihasilkan oleh arus AC." },
  { "en": "Apa fungsi dasar dari sebuah dioda?", "id": "Untuk mengalirkan arus listrik hanya ke satu arah dan menghambatnya dari arah sebaliknya." },
  { "en": "Apa nama terminal positif pada dioda?", "id": "Anoda." },
  { "en": "Apa nama terminal negatif pada dioda?", "id": "Katoda." },
  { "en": "Kondisi dioda saat arus dapat mengalir disebut?", "id": "Bias maju (Forward Bias)." },
  { "en": "Kondisi dioda saat arus tidak dapat mengalir disebut?", "id": "Bias mundur (Reverse Bias)." },
  { "en": "Apa nama proses mengubah tegangan AC menjadi DC menggunakan dioda?", "id": "Penyearahan (Rectification)." },
  { "en": "Apa itu LED (Light Emitting Diode)?", "id": "Jenis dioda yang dapat memancarkan cahaya ketika diberi bias maju." },
  { "en": "Mengapa LED memerlukan resistor seri?", "id": "Untuk membatasi arus yang mengalir melaluinya agar tidak terbakar." },
  { "en": "Apa fungsi dari dioda Zener?", "id": "Untuk menstabilkan tegangan pada nilai tertentu ketika diberi bias mundur." },
  { "en": "Di manakah aplikasi utama dioda Zener?", "id": "Sebagai regulator tegangan sederhana (voltage regulator)." },
  { "en": "Apa itu dioda bridge?", "id": "Rangkaian empat dioda yang disusun untuk penyearah gelombang penuh (full-wave rectifier)." },
  { "en": "Apa kelebihan penyearah jembatan (bridge rectifier) dibanding penyearah gelombang penuh biasa?", "id": "Tidak memerlukan transformator CT (Center Tap)." },
  { "en": "Apa itu dioda Schottky?", "id": "Jenis dioda dengan tegangan jatuh (voltage drop) yang sangat rendah dan kecepatan switching yang tinggi." },
  { "en": "Apa itu fotodioda (photodiode)?", "id": "Jenis dioda yang dapat mengubah energi cahaya menjadi arus listrik." },
  { "en": "Apa fungsi dasar dari sebuah transistor?", "id": "Sebagai saklar (switch) elektronik dan penguat (amplifier) sinyal listrik." },
  { "en": "Sebutkan dua jenis utama transistor!", "id": "Transistor Bipolar (BJT) dan Transistor Efek Medan (FET)." },
  { "en": "Sebutkan tiga terminal pada transistor BJT!", "id": "Basis, Kolektor, dan Emitor." },
  { "en": "Terminal mana pada BJT yang berfungsi untuk mengontrol aliran arus utama?", "id": "Basis." },
  { "en": "Apa perbedaan utama antara transistor NPN dan PNP?", "id": "Arah aliran arus dan polaritas tegangan bias yang dibutuhkan untuk mengaktifkannya." },
  { "en": "Apa yang dimaksud dengan 'gain' atau hFE pada transistor?", "id": "Faktor penguatan arus, yaitu perbandingan antara arus kolektor dan arus basis." },
  { "en": "Kondisi transistor saat berfungsi seperti saklar tertutup disebut?", "id": "Saturasi (Jenuh)." },
  { "en": "Kondisi transistor saat berfungsi seperti saklar terbuka disebut?", "id": "Cut-off." },
  { "en": "Sebutkan tiga terminal pada transistor FET!", "id": "Gate, Drain, dan Source." },
  { "en": "Terminal mana pada FET yang berfungsi untuk mengontrol aliran arus utama?", "id": "Gate." },
  { "en": "Apa kelebihan utama FET dibandingkan BJT?", "id": "Impedansi input yang sangat tinggi, sehingga hampir tidak membebani sumber sinyal." },
  { "en": "Apa kepanjangan dari MOSFET?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Apa fungsi dari heatsink (pendingin) yang dipasang pada transistor daya?", "id": "Untuk membuang panas yang dihasilkan oleh transistor agar tidak cepat rusak." },
  { "en": "Apa yang dimaksud dengan sirkuit terpadu atau IC (Integrated Circuit)?", "id": "Sebuah komponen elektronik yang terdiri dari ribuan atau jutaan transistor, resistor, dan kapasitor dalam satu keping silikon kecil." },
  { "en": "Apa keuntungan menggunakan IC dibandingkan sirkuit diskrit?", "id": "Ukuran yang jauh lebih kecil, konsumsi daya lebih rendah, dan biaya produksi massal lebih murah." },
  { "en": "Apa fungsi dari IC 555 timer?", "id": "Sebagai timer (pewaktu), generator pulsa, atau osilator yang sangat serbaguna." },
  { "en": "Apa itu Op-Amp (Operational Amplifier)?", "id": "Sebuah penguat diferensial dengan gain tegangan yang sangat tinggi, impedansi input tinggi, dan impedansi output rendah." },
  { "en": "Sebutkan dua mode konfigurasi dasar Op-Amp!", "id": "Penguat membalik (Inverting Amplifier) dan penguat tak-membalik (Non-inverting Amplifier)." },
  { "en": "Apa fungsi Op-Amp dalam konfigurasi komparator?", "id": "Membandingkan dua level tegangan input dan memberikan output HIGH atau LOW." },
  { "en": "Apa itu mikrokontroler?", "id": "Sebuah IC yang berisi CPU, memori (RAM, ROM), dan pin input/output (I/O) yang dapat diprogram." },
  { "en": "Apa perbedaan mendasar antara mikroprosesor dan mikrokontroler?", "id": "Mikroprosesor hanya berisi CPU, sedangkan mikrokontroler adalah sistem komputer lengkap dalam satu chip." },
  { "en": "Apa fungsi dari IC regulator tegangan seperti 7805?", "id": "Untuk menghasilkan tegangan output DC yang stabil sebesar +5 Volt." },
  { "en": "Apa arti dua digit terakhir pada seri IC 78xx?", "id": "Menunjukkan level tegangan output positifnya (contoh: 7812 berarti +12V)." },
  { "en": "Apa fungsi dari IC regulator tegangan seri 79xx?", "id": "Untuk menghasilkan tegangan output DC negatif yang stabil." },
  { "en": "Apa yang dimaksud dengan sinyal analog?", "id": "Sinyal kontinu yang nilainya dapat bervariasi secara tak terbatas dalam rentang tertentu." },
  { "en": "Apa yang dimaksud dengan sinyal digital?", "id": "Sinyal diskrit yang hanya memiliki dua level keadaan, biasanya direpresentasikan sebagai 0 dan 1 (atau LOW dan HIGH)." },
  { "en": "Sistem bilangan apa yang digunakan dalam elektronika digital?", "id": "Sistem bilangan biner." },
  { "en": "Apa sebutan untuk satu digit biner (0 atau 1)?", "id": "Bit." },
  { "en": "Apa itu gerbang logika (logic gate)?", "id": "Sirkuit elektronik dasar yang mengimplementasikan operasi logika Boolean." },
  { "en": "Gerbang logika apa yang outputnya akan HIGH hanya jika semua inputnya HIGH?", "id": "Gerbang AND." },
  { "en": "Gerbang logika apa yang outputnya akan HIGH jika salah satu atau semua inputnya HIGH?", "id": "Gerbang OR." },
  { "en": "Gerbang logika apa yang outputnya selalu merupakan kebalikan dari inputnya?", "id": "Gerbang NOT (Inverter)." },
  { "en": "Gerbang logika apa yang merupakan gabungan dari gerbang AND dan NOT?", "id": "Gerbang NAND." },
  { "en": "Gerbang logika apa yang merupakan gabungan dari gerbang OR dan NOT?", "id": "Gerbang NOR." },
  { "en": "Gerbang logika apa yang outputnya akan HIGH hanya jika input-inputnya berbeda?", "id": "Gerbang XOR (Exclusive OR)." },
  { "en": "Apa fungsi dari rangkaian flip-flop?", "id": "Sebagai elemen memori dasar yang dapat menyimpan satu bit data (0 atau 1)." },
  { "en": "Apa yang dimaksud dengan rangkaian osilator?", "id": "Rangkaian yang menghasilkan sinyal periodik yang berulang (gelombang sinus, kotak, atau segitiga) tanpa memerlukan input sinyal." },
  { "en": "Apa fungsi osilator kristal (crystal oscillator)?", "id": "Menghasilkan sinyal clock dengan frekuensi yang sangat stabil dan akurat." },
  { "en": "Apa yang dimaksud dengan rangkaian filter?", "id": "Rangkaian yang dirancang untuk melewatkan frekuensi tertentu dan meredam (memblokir) frekuensi lainnya." },
  { "en": "Apa fungsi dari filter lolos-rendah (low-pass filter)?", "id": "Melewatkan sinyal berfrekuensi rendah dan meredam sinyal berfrekuensi tinggi." },
  { "en": "Apa fungsi dari filter lolos-tinggi (high-pass filter)?", "id": "Melewatkan sinyal berfrekuensi tinggi dan meredam sinyal berfrekuensi rendah." },
  { "en": "Apa fungsi dari rangkaian catu daya (power supply)?", "id": "Mengubah tegangan listrik dari sumber (seperti PLN) menjadi level tegangan dan jenis arus yang sesuai untuk perangkat elektronik." },
  { "en": "Sebutkan blok diagram dasar dari catu daya linier!", "id": "Transformator, penyearah (rectifier), filter, dan regulator." },
  { "en": "Apa perbedaan utama catu daya linier dan catu daya switching (SMPS)?", "id": "SMPS (Switched-Mode Power Supply) jauh lebih efisien dan ukurannya lebih kecil dibandingkan catu daya linier untuk daya yang sama." },
  { "en": "Apa fungsi dari sekring (fuse)?", "id": "Untuk melindungi rangkaian dari arus berlebih dengan cara memutus sirkuit (terbakar) jika arus melebihi batas." },
  { "en": "Apa itu PCB (Printed Circuit Board)?", "id": "Papan yang digunakan untuk menghubungkan komponen elektronik secara fisik dan elektrik menggunakan jalur konduktif (tembaga)." },
  { "en": "Apa nama lapisan tembaga pada PCB?", "id": "Jalur (trace)." },
  { "en": "Apa nama lubang pada PCB untuk memasang kaki komponen?", "id": "Pad." },
  { "en": "Apa perbedaan antara komponen THT (Through-Hole Technology) dan SMD (Surface-Mount Device)?", "id": "Komponen THT memiliki kaki kawat yang dimasukkan melalui lubang di PCB, sedangkan SMD dipasang langsung di permukaan PCB." },
  { "en": "Apa keuntungan menggunakan komponen SMD?", "id": "Ukuran lebih kecil, memungkinkan perakitan otomatis, dan kepadatan komponen lebih tinggi." },
  { "en": "Apa fungsi dari breadboard (project board)?", "id": "Papan untuk membuat prototipe rangkaian elektronik sementara tanpa perlu menyolder." },
  { "en": "Apa fungsi dari multimeter?", "id": "Alat ukur elektronik yang dapat mengukur beberapa besaran seperti tegangan (voltmeter), arus (ammeter), dan resistansi (ohmmeter)." },
  { "en": "Bagaimana cara memasang ammeter untuk mengukur arus?", "id": "Secara seri dengan beban atau komponen yang arusnya akan diukur." },
  { "en": "Bagaimana cara memasang voltmeter untuk mengukur tegangan?", "id": "Secara paralel dengan komponen atau titik yang tegangannya akan diukur." },
  { "en": "Apa fungsi dari osiloskop?", "id": "Untuk menampilkan bentuk gelombang (grafik) dari sinyal listrik terhadap waktu." },
  { "en": "Besaran apa yang ditampilkan pada sumbu vertikal (Y) osiloskop?", "id": "Tegangan (Amplitudo)." },
  { "en": "Besaran apa yang ditampilkan pada sumbu horizontal (X) osiloskop?", "id": "Waktu (Periode/Frekuensi)." },
  { "en": "Apa fungsi dari generator sinyal (signal generator)?", "id": "Alat untuk menghasilkan berbagai bentuk gelombang standar (sinus, kotak, segitiga) dengan frekuensi dan amplitudo yang dapat diatur." },
  { "en": "Apa itu solder (timah solder)?", "id": "Logam paduan (biasanya timah dan timbal) yang digunakan untuk membuat sambungan listrik permanen pada proses penyolderan." },
  { "en": "Apa fungsi dari flux dalam proses penyolderan?", "id": "Untuk membersihkan permukaan logam dari oksidasi dan membantu timah solder mengalir dengan baik." },
  { "en": "Apa yang dimaksud dengan 'ground' atau 'tanah' dalam elektronika?", "id": "Titik referensi umum dalam sebuah sirkuit yang dianggap memiliki potensial 0 Volt." },
  { "en": "Apa yang dimaksud dengan frekuensi?", "id": "Jumlah siklus atau getaran gelombang yang terjadi dalam satu detik." },
  { "en": "Apa satuan dari frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Apa itu impedansi?", "id": "Hambatan total terhadap arus bolak-balik (AC) yang merupakan kombinasi dari resistansi dan reaktansi (dari kapasitor dan induktor)." },
  { "en": "Apa itu reaktansi kapasitif?", "id": "Hambatan yang ditimbulkan oleh kapasitor terhadap arus AC." },
  { "en": "Apa itu reaktansi induktif?", "id": "Hambatan yang ditimbulkan oleh induktor terhadap arus AC." },
  { "en": "Apa fungsi saklar (switch)?", "id": "Untuk menghubungkan atau memutuskan aliran arus dalam sebuah sirkuit secara manual." },
  { "en": "Apa yang dimaksud dengan rangkaian seri?", "id": "Rangkaian di mana komponen-komponennya dihubungkan ujung ke ujung, membentuk satu jalur tunggal untuk aliran arus." },
  { "en": "Apa yang dimaksud dengan rangkaian paralel?", "id": "Rangkaian di mana komponen-komponennya dihubungkan pada dua titik yang sama, membentuk beberapa cabang untuk aliran arus." },
  { "en": "Apa itu relay?", "id": "Saklar yang dioperasikan secara elektromagnetik, di mana sirkuit berarus kecil dapat mengontrol sirkuit berarus besar." },
  { "en": "Apa bagian utama dari sebuah relay?", "id": "Koil (elektromagnet) dan kontak saklar (contacts)." },
  { "en": "Apa keuntungan menggunakan relay?", "id": "Memberikan isolasi listrik (galvanic isolation) antara sirkuit kontrol dan sirkuit beban." },
  { "en": "Apa itu optocoupler (opto-isolator)?", "id": "Komponen yang mentransfer sinyal listrik antara dua sirkuit terisolasi menggunakan cahaya." },
  { "en": "Terdiri dari apa sebuah optocoupler?", "id": "Sebuah LED dan sebuah fotodetektor (seperti fototransistor) dalam satu kemasan." },
  { "en": "Apa itu sensor?", "id": "Perangkat yang mendeteksi perubahan besaran fisik (seperti suhu, cahaya, tekanan) dan mengubahnya menjadi sinyal listrik." },
  { "en": "Apa itu aktuator?", "id": "Perangkat yang mengubah sinyal listrik menjadi gerakan atau aksi fisik (seperti motor, solenoida)." },
  { "en": "Apa fungsi dari motor DC?", "id": "Mengubah energi listrik DC menjadi energi gerak putar." },
  { "en": "Apa itu PWM (Pulse Width Modulation)?", "id": "Teknik untuk mengontrol daya yang dikirim ke perangkat listrik dengan cara mengubah-ubah lebar pulsa sinyal digital." },
  { "en": "Di mana PWM sering digunakan?", "id": "Untuk mengontrol kecepatan motor DC atau kecerahan lampu LED." },
  { "en": "Apa yang dimaksud dengan 'noise' (derau) dalam sinyal elektronik?", "id": "Sinyal acak yang tidak diinginkan yang menumpangi (mengganggu) sinyal asli." },
  { "en": "Apa itu bandwidth (lebar pita)?", "id": "Rentang frekuensi di mana sebuah sistem elektronik dapat beroperasi secara efektif." },
  { "en": "Apa fungsi dari penguat (amplifier)?", "id": "Meningkatkan amplitudo atau kekuatan dari sebuah sinyal listrik." },
  { "en": "Apa itu 'gain' dari sebuah penguat?", "id": "Perbandingan antara level sinyal output dan level sinyal input." },
  { "en": "Apa itu DAC (Digital-to-Analog Converter)?", "id": "Rangkaian yang mengubah data digital (biner) menjadi sinyal analog." },
  { "en": "Apa itu ADC (Analog-to-Digital Converter)?", "id": "Rangkaian yang mengubah sinyal analog (seperti dari sensor) menjadi data digital." },
  { "en": "Mengapa bahan semikonduktor perlu di-'doping'?", "id": "Untuk meningkatkan konduktivitasnya dengan menambahkan atom ketidakmurnian (impurity)." },
  { "en": "Apa yang dihasilkan jika silikon didoping dengan Fosfor?", "id": "Semikonduktor tipe-N (kelebihan elektron)." },
  { "en": "Apa yang dihasilkan jika silikon didoping dengan Boron?", "id": "Semikonduktor tipe-P (kelebihan 'hole' atau lubang)." },
  { "en": "Apa yang terbentuk pada pertemuan antara semikonduktor tipe-P dan tipe-N?", "id": "Sambungan P-N (P-N Junction), yang merupakan dasar dari dioda." },
  { "en": "Apa itu dioda varactor (varicap)?", "id": "Jenis dioda yang nilai kapasitansinya berubah-ubah tergantung pada tegangan bias mundur yang diberikan." },
  { "en": "Apa itu SCR (Silicon Controlled Rectifier)?", "id": "Jenis thyristor yang berfungsi sebagai saklar searah yang dapat dikontrol." },
  { "en": "Bagaimana cara mematikan SCR setelah 'on'?", "id": "Dengan membuat arusnya turun di bawah 'holding current' atau menghilangkan tegangan anoda-katoda." },
  { "en": "Apa itu TRIAC?", "id": "Komponen semikonduktor yang berfungsi seperti dua SCR yang dihubungkan anti-paralel, dapat mengalirkan arus AC dua arah." },
  { "en": "Di mana TRIAC sering digunakan?", "id": "Dalam rangkaian dimmer lampu atau kontrol kecepatan motor AC." },
  { "en": "Apa yang dimaksud dengan 'solder mask' pada PCB?", "id": "Lapisan pelindung (biasanya berwarna hijau) yang menutupi jalur tembaga untuk mencegah hubungan pendek dan korosi." },
  { "en": "Apa yang dimaksud dengan 'silkscreen' pada PCB?", "id": "Lapisan tinta (biasanya putih) yang digunakan untuk mencetak label komponen, logo, atau informasi lainnya di atas PCB." },
  { "en": "Apa yang dimaksud dengan rangkaian logika kombinasional?", "id": "Rangkaian digital di mana outputnya hanya bergantung pada kondisi input saat itu juga." },
  { "en": "Apa yang dimaksud dengan rangkaian logika sekuensial?", "id": "Rangkaian digital di mana outputnya bergantung pada kondisi input saat ini dan kondisi output sebelumnya (memiliki memori)." },
  { "en": "Apa itu register geser (shift register)?", "id": "Rangkaian logika sekuensial yang digunakan untuk menyimpan dan menggeser data biner." },
  { "en": "Apa itu 'multiplexer' (MUX)?", "id": "Rangkaian digital yang memilih salah satu dari beberapa jalur input untuk diteruskan ke satu jalur output." },
  { "en": "Apa itu 'demultiplexer' (DEMUX)?", "id": "Rangkaian digital yang meneruskan data dari satu jalur input ke salah satu dari beberapa jalur output yang dipilih." },
  { "en": "Apa itu 'encoder' dalam elektronika digital?", "id": "Rangkaian yang mengubah data dari format satu ke format lain, misalnya dari desimal ke biner." },
  { "en": "Apa itu 'decoder' dalam elektronika digital?", "id": "Rangkaian yang mengubah data yang dikodekan (seperti biner) menjadi format aslinya (seperti desimal)." },
  { "en": "Apa itu ESD (Electrostatic Discharge)?", "id": "Pelepasan muatan listrik statis secara tiba-tiba yang dapat merusak komponen elektronik yang sensitif." },
  { "en": "Bagaimana cara mencegah kerusakan akibat ESD?", "id": "Menggunakan gelang anti-statis, alas kerja anti-statis, dan menyimpan komponen dalam kantong anti-statis." },
  { "en": "Apa perbedaan antara tegangan RMS dan tegangan puncak (peak voltage) pada sinyal AC?", "id": "Tegangan puncak adalah nilai maksimum dari gelombang, sedangkan tegangan RMS (Root Mean Square) adalah nilai efektifnya yang setara dengan tegangan DC untuk menghasilkan daya yang sama." },
  { "en": "Apa itu 'ripple' pada catu daya?", "id": "Sisa variasi tegangan AC kecil yang masih ada pada output DC setelah proses penyearahan dan filtering." },
  { "en": "Apa fungsi dari dioda flyback (flyback diode) yang dipasang paralel dengan relay atau motor?", "id": "Untuk melindungi komponen kontrol (seperti transistor) dari lonjakan tegangan induktif yang terjadi saat beban dimatikan." },
  { "en": "Apa itu ROM (Read-Only Memory)?", "id": "Jenis memori yang datanya hanya bisa dibaca dan tidak bisa diubah, serta bersifat non-volatile (data tidak hilang saat daya mati)." },
  { "en": "Apa itu RAM (Random-Access Memory)?", "id": "Jenis memori yang datanya bisa dibaca dan ditulis, serta bersifat volatile (data hilang saat daya mati)." },
  { "en": "Apa itu 'bus' dalam sistem digital?", "id": "Sekumpulan jalur konduktor paralel yang digunakan untuk mentransfer data, alamat, dan sinyal kontrol antar komponen." },
  { "en": "Apa itu 'clock signal' (sinyal detak)?", "id": "Sinyal periodik yang digunakan untuk menyinkronkan operasi berbagai bagian dalam sebuah sirkuit digital." },
  { "en": "Apa fungsi dari rangkaian 'debouncing' untuk sebuah tombol?", "id": "Untuk menghilangkan efek pantulan (bouncing) mekanis dari kontak tombol yang dapat menghasilkan beberapa pulsa palsu." },
  { "en": "Apa itu 'firmware'?", "id": "Perangkat lunak yang secara permanen ditanamkan ke dalam perangkat keras (seperti mikrokontroler atau ROM)." },
  { "en": "Apa fungsi dari 'bootloader' pada mikrokontroler?", "id": "Program kecil yang berjalan saat startup untuk memungkinkan pengisian program utama ke memori tanpa memerlukan programmer eksternal." },
  { "en": "Apa itu JFET?", "id": "Junction Field-Effect Transistor, jenis FET yang paling sederhana." },
  { "en": "Apa itu penguat kelas A?", "id": "Kelas penguat di mana transistor output selalu aktif (menghantarkan arus) selama seluruh siklus sinyal input." },
  { "en": "Apa kelemahan utama penguat kelas A?", "id": "Efisiensinya sangat rendah karena menghasilkan banyak panas." },
  { "en": "Apa itu penguat kelas B?", "id": "Kelas penguat yang menggunakan dua transistor (push-pull) di mana masing-masing hanya aktif selama setengah siklus sinyal." },
  { "en": "Apa masalah yang ada pada penguat kelas B?", "id": "Crossover distortion, yaitu cacat sinyal pada titik persilangan antara dua transistor." },
  { "en": "Bagaimana penguat kelas AB mengatasi masalah crossover distortion?", "id": "Dengan memberikan sedikit bias pada kedua transistor sehingga keduanya sedikit aktif bahkan tanpa sinyal input." },
  { "en": "Apa itu penguat kelas D?", "id": "Penguat switching yang sangat efisien, sering digunakan pada perangkat audio modern." },
  { "en": "Apa itu 'common emitter' pada konfigurasi BJT?", "id": "Konfigurasi di mana terminal emitor digunakan bersama untuk input dan output, menghasilkan penguatan tegangan dan arus." },
  { "en": "Apa itu 'common collector' (emitter follower) pada konfigurasi BJT?", "id": "Konfigurasi di mana terminal kolektor digunakan bersama, memiliki penguatan tegangan mendekati 1 tetapi penguatan arus tinggi." },
  { "en": "Apa fungsi utama dari konfigurasi emitter follower?", "id": "Sebagai buffer atau penyangga impedansi." },
  { "en": "Apa itu 'common base' pada konfigurasi BJT?", "id": "Konfigurasi di mana terminal basis digunakan bersama, memiliki penguatan arus mendekati 1 tetapi penguatan tegangan tinggi." },
  { "en": "Apa itu dioda Tunnel?", "id": "Jenis dioda yang menunjukkan fenomena resistansi negatif pada sebagian kurva karakteristiknya." },
  { "en": "Apa itu 'ground loop'?", "id": "Kondisi yang tidak diinginkan di mana ada lebih dari satu jalur ke ground, yang dapat menyebabkan noise atau dengung." },
  { "en": "Apa fungsi kapasitor bypass (decoupling capacitor)?", "id": "Ditempatkan dekat pin catu daya IC untuk menyaring noise frekuensi tinggi dan menstabilkan tegangan lokal." },
  { "en": "Apa itu 'skin effect'?", "id": "Kecenderungan arus AC berfrekuensi tinggi untuk mengalir hanya di dekat permukaan konduktor." },
  { "en": "Apa itu 'slew rate' pada Op-Amp?", "id": "Tingkat perubahan tegangan output maksimum yang dapat dihasilkan oleh Op-Amp." },
  { "en": "Apa itu 'common-mode rejection ratio' (CMRR) pada Op-Amp?", "id": "Kemampuan Op-Amp untuk menolak sinyal yang sama (common) yang muncul di kedua inputnya." },
  { "en": "Apa itu 'power factor' (faktor daya)?", "id": "Ukuran efisiensi penggunaan daya listrik dalam sistem AC." },
  { "en": "Apa itu rangkaian 'crowbar'?", "id": "Rangkaian proteksi yang dengan sengaja membuat hubungan pendek (short circuit) pada catu daya untuk memutus sekring saat terjadi tegangan berlebih (overvoltage)." },
  { "en": "Apa itu 'datasheet' komponen elektronik?", "id": "Dokumen dari pabrikan yang berisi semua spesifikasi teknis, karakteristik, dan informasi penggunaan sebuah komponen." },
  { "en": "Apa itu 'breadth' dari sebuah pulsa digital?", "id": "Durasi atau lebar dari pulsa tersebut." },
  { "en": "Apa yang dimaksud dengan 'duty cycle' dari sebuah sinyal PWM?", "id": "Persentase waktu di mana sinyal berada dalam keadaan HIGH dalam satu periode." },
  { "en": "Apa itu 'latching' pada SCR atau relay?", "id": "Kondisi di mana komponen tetap dalam keadaan 'on' bahkan setelah sinyal pemicunya dihilangkan." },
  { "en": "Apa itu 'ZIF socket'?", "id": "Zero Insertion Force socket, soket yang dirancang agar IC dapat dimasukkan dan dilepas tanpa memerlukan gaya." },
  { "en": "Apa itu 'pull-up resistor'?", "id": "Resistor yang digunakan untuk memastikan sebuah pin input digital berada pada level HIGH ketika tidak ada sinyal input aktif (terhubung ke ground)." },
  { "en": "Apa itu 'pull-down resistor'?", "id": "Resistor yang digunakan untuk memastikan sebuah pin input digital berada pada level LOW ketika tidak ada sinyal input aktif (terhubung ke VCC)." },
  { "en": "Apa itu 'open-collector' atau 'open-drain' output?", "id": "Jenis output digital yang hanya dapat menarik sinyal ke level LOW (ground) dan memerlukan resistor pull-up eksternal untuk mencapai level HIGH." },
  { "en": "Apa keuntungan dari output open-collector?", "id": "Memungkinkan beberapa output dihubungkan bersama untuk membentuk logika 'wired-AND'." },
  { "en": "Apa itu 'hysteresis' dalam sebuah komparator atau Schmitt trigger?", "id": "Perbedaan antara level tegangan ambang atas dan ambang bawah, yang mencegah output berosilasi saat input berada di dekat ambang batas." },
  { "en": "Apa fungsi dari Schmitt Trigger?", "id": "Untuk mengubah sinyal analog yang 'berisik' atau lambat berubah menjadi sinyal digital yang bersih dan tegas." },
  { "en": "Apa itu 'feedback' (umpan balik) dalam sebuah rangkaian?", "id": "Proses mengambil sebagian sinyal output dan mengembalikannya ke input." },
  { "en": "Apa perbedaan antara umpan balik positif dan negatif?", "id": "Umpan balik negatif digunakan untuk menstabilkan dan mengontrol penguatan (seperti pada amplifier), sedangkan umpan balik positif digunakan untuk membuat osilasi (seperti pada osilator)." },
  { "en": "Apa itu 'virtual ground' pada Op-Amp inverting?", "id": "Titik di mana input inverting (-) memiliki tegangan yang sama dengan input non-inverting (+) (yaitu 0V jika non-inverting digroundkan), meskipun tidak terhubung langsung ke ground." },
  { "en": "Apa itu 'solder bridge'?", "id": "Kelebihan solder yang tidak sengaja menghubungkan dua titik atau jalur yang seharusnya terpisah pada PCB." },
  { "en": "Apa itu 'cold solder joint'?", "id": "Sambungan solder yang buruk karena suhu yang tidak cukup, terlihat kusam dan retak, serta memiliki konektivitas yang tidak andal." },
  { "en": "Apa itu 'circuit breaker'?", "id": "Saklar otomatis yang dirancang untuk melindungi sirkuit dari kerusakan akibat arus berlebih, dapat direset setelah trip." },
  { "en": "Apa itu 'inrush current'?", "id": "Lonjakan arus sesaat yang tinggi saat perangkat elektronik pertama kali dinyalakan." },
  { "en": "Mengapa multimeter harus diatur ke rentang yang lebih tinggi sebelum mengukur besaran yang tidak diketahui?", "id": "Untuk mencegah kerusakan pada alat ukur akibat tegangan atau arus yang melebihi batas rentang yang dipilih." },
  { "en": "Apa itu 'ghost voltage' atau 'phantom voltage'?", "id": "Tegangan yang terukur oleh multimeter impedansi tinggi pada kabel yang tidak terhubung, disebabkan oleh kopling kapasitif dari kabel bertegangan di dekatnya." },
  { "en": "Apa itu 'True RMS' multimeter?", "id": "Multimeter yang dapat mengukur nilai RMS secara akurat untuk bentuk gelombang AC non-sinusoidal (tidak murni)." },
  { "en": "Apa itu 'diode test' mode pada multimeter?", "id": "Mode untuk mengukur tegangan jatuh (forward voltage drop) pada sebuah dioda, berguna untuk memeriksa apakah dioda berfungsi." },
  { "en": "Apa itu 'continuity test' mode pada multimeter?", "id": "Mode untuk memeriksa apakah ada jalur konduktif (hambatan sangat rendah) antara dua titik, biasanya ditandai dengan bunyi 'beep'." },
  { "en": "Apa fungsi dari 'ferrite bead' atau 'ferrite core' yang sering ditemukan pada kabel?", "id": "Untuk menekan atau memfilter noise frekuensi tinggi (EMI - Electromagnetic Interference)." },
  { "en": "Apa itu 'twisted pair' cable?", "id": "Sepasang kabel yang dipilin bersama untuk mengurangi interferensi elektromagnetik dari sumber eksternal." },
  { "en": "Apa itu 'shielded cable'?", "id": "Kabel yang memiliki lapisan konduktif (perisai) untuk melindunginya dari interferensi elektromagnetik." },
  { "en": "Apa fungsi dari 'ground plane' pada PCB?", "id": "Lapisan tembaga besar yang terhubung ke ground, berfungsi sebagai jalur kembali arus dan perisai terhadap noise." },
  { "en": "Apa itu 'via' pada PCB?", "id": "Lubang konduktif yang menghubungkan jalur (trace) pada lapisan PCB yang berbeda." },
  { "en": "Apa itu 'schematic diagram' (diagram skematik)?", "id": "Representasi grafis dari sebuah sirkuit elektronik menggunakan simbol-simbol standar." },
  { "en": "Apa itu 'layout diagram'?", "id": "Representasi dari penempatan fisik komponen dan jalur pada sebuah PCB." },
  { "en": "Apa perbedaan utama antara RAM statis (SRAM) dan RAM dinamis (DRAM)?", "id": "SRAM menggunakan flip-flop (lebih cepat, lebih mahal), sedangkan DRAM menggunakan kapasitor (lebih lambat, lebih murah, perlu di-'refresh')." },
  { "en": "Apa itu 'EEPROM'?", "id": "Electrically Erasable Programmable Read-Only Memory, jenis memori non-volatile yang isinya dapat dihapus dan ditulis ulang secara elektrik." },
  { "en": "Apa itu memori Flash?", "id": "Jenis EEPROM yang memungkinkan penghapusan dan penulisan data dalam blok, banyak digunakan di USB drive dan SSD." },
  { "en": "Apa itu 'bus contention'?", "id": "Kondisi di mana dua atau lebih perangkat mencoba mengirim data melalui bus yang sama pada saat yang bersamaan." },
  { "en": "Apa itu 'tri-state logic'?", "id": "Jenis output logika yang memiliki tiga keadaan: HIGH, LOW, dan High-Impedance (impedansi tinggi atau terputus)." },
  { "en": "Apa fungsi dari keadaan High-Impedance?", "id": "Memungkinkan beberapa perangkat berbagi bus data tanpa saling mengganggu ketika tidak aktif." },
  { "en": "Apa itu 'propagation delay'?", "id": "Waktu singkat yang dibutuhkan oleh sinyal untuk berjalan dari input ke output sebuah gerbang logika atau sirkuit." },
  { "en": "Apa itu 'race condition' dalam sirkuit digital?", "id": "Situasi yang tidak diinginkan di mana output sirkuit bergantung pada urutan atau waktu sinyal yang sedikit berbeda." },
  { "en": "Apa itu 'glitch' dalam sinyal digital?", "id": "Pulsa sesaat yang tidak diinginkan yang muncul pada output, seringkali disebabkan oleh race condition." },
  { "en": "Apa itu 'logic family' (keluarga logika)?", "id": "Sekelompok IC digital yang memiliki karakteristik listrik yang sama (seperti level tegangan, kecepatan, dan konsumsi daya)." },
  { "en": "Sebutkan dua keluarga logika yang populer!", "id": "TTL (Transistor-Transistor Logic) dan CMOS (Complementary Metal-Oxide-Semiconductor)." },
  { "en": "Apa keunggulan utama dari logika CMOS dibandingkan TTL?", "id": "Konsumsi daya statis yang jauh lebih rendah." },
  { "en": "Apa itu 'antena'?", "id": "Perangkat yang digunakan untuk mengubah gelombang listrik terpandu (di kabel) menjadi gelombang elektromagnetik bebas (gelombang radio), atau sebaliknya." },
  { "en": "Apa itu 'modulasi'?", "id": "Proses menumpangkan sinyal informasi (misalnya suara) ke sinyal pembawa (carrier wave) berfrekuensi tinggi." },
  { "en": "Apa itu 'demodulasi'?", "id": "Proses memisahkan kembali sinyal informasi asli dari sinyal pembawa." },
  { "en": "Sebutkan dua jenis modulasi analog yang umum!", "id": "Modulasi Amplitudo (AM) dan Modulasi Frekuensi (FM)." },
  { "en": "Apa itu 'solenoida'?", "id": "Sebuah koil kawat yang dirancang untuk menghasilkan medan magnet yang seragam di dalamnya, sering digunakan sebagai aktuator elektromagnetik." },
  { "en": "Apa itu 'magnetoresistance'?", "id": "Sifat beberapa bahan untuk mengubah nilai resistansi listriknya di bawah pengaruh medan magnet eksternal." },
  { "en": "Apa itu 'efek Hall' (Hall effect)?", "id": "Produksi beda tegangan (tegangan Hall) di sepanjang konduktor listrik, yang melintang terhadap arus listrik dan medan magnet." },
  { "en": "Apa fungsi dari sensor efek Hall?", "id": "Untuk mendeteksi keberadaan atau kekuatan medan magnet, sering digunakan untuk mengukur kecepatan putaran atau posisi." },
  { "en": "Apa itu 'efek piezoelektrik'?", "id": "Kemampuan beberapa bahan (seperti kristal kuarsa) untuk menghasilkan tegangan listrik saat mengalami tekanan mekanis, atau sebaliknya." },
  { "en": "Sebutkan aplikasi dari efek piezoelektrik!", "id": "Pemantik api gas, mikrofon kristal, buzzer, dan sensor getaran." },
  { "en": "Apa itu 'efek Seebeck'?", "id": "Fenomena di mana beda tegangan dihasilkan antara dua sambungan logam yang berbeda jenis ketika sambungan tersebut berada pada suhu yang berbeda." },
  { "en": "Apa dasar dari komponen 'termokopel' (thermocouple)?", "id": "Efek Seebeck, digunakan untuk mengukur suhu." },
  { "en": "Apa itu 'superkonduktivitas'?", "id": "Fenomena di mana beberapa bahan menunjukkan resistansi listrik nol di bawah suhu kritis tertentu." },
  { "en": "Apa itu 'kapasitansi parasit'?", "id": "Kapasitansi yang tidak diinginkan dan tidak dapat dihindari yang ada antara bagian-bagian komponen atau sirkuit." },
  { "en": "Apa itu 'induktansi parasit'?", "id": "Induktansi yang tidak diinginkan dan tidak dapat dihindari yang ada pada kaki komponen atau jalur PCB." },
  { "en": "Apa itu 'breakdown voltage' pada sebuah isolator atau dioda?", "id": "Tegangan minimum yang menyebabkan isolator atau dioda (dalam bias mundur) gagal dan mulai menghantarkan arus secara signifikan." },
  { "en": "Apa itu 'arus bocor' (leakage current)?", "id": "Arus kecil yang masih mengalir melalui komponen (seperti kapasitor atau dioda reverse-biased) yang seharusnya idealnya memblokir arus sepenuhnya." },
  { "en": "Apa itu 'tegangan saturasi' pada transistor?", "id": "Tegangan kecil yang masih ada antara kolektor dan emitor ketika transistor berada dalam kondisi saturasi penuh." },
  { "en": "Apa fungsi dari 'snubber circuit'?", "id": "Rangkaian kecil (biasanya RC) yang digunakan untuk menekan lonjakan tegangan dan meredam osilasi saat saklar (seperti relay atau TRIAC) dibuka." },
  { "en": "Apa itu 'kapasitor film'?", "id": "Jenis kapasitor non-polarisasi yang menggunakan film plastik tipis sebagai dielektrik." },
  { "en": "Apa itu 'kapasitor tantalum'?", "id": "Jenis kapasitor elektrolit yang menggunakan tantalum sebagai anoda, dikenal karena kepadatan kapasitansi yang tinggi dan stabilitasnya." },
  { "en": "Apa itu 'power rating' dari sebuah resistor?", "id": "Jumlah daya maksimum (dalam Watt) yang dapat didisipasikan oleh resistor secara aman tanpa menjadi terlalu panas." },
  { "en": "Apa yang dimaksud dengan 'rangkaian tangki' (tank circuit)?", "id": "Rangkaian resonansi paralel yang terdiri dari induktor dan kapasitor, digunakan dalam osilator dan filter." },
  { "en": "Apa itu 'faktor Q' (Quality factor) dari sebuah rangkaian resonansi?", "id": "Ukuran 'kebaikan' atau selektivitas dari rangkaian resonansi, menunjukkan seberapa tajam puncaknya." },
  { "en": "Apa itu 'matching impedance' (penyesuaian impedansi)?", "id": "Praktik merancang impedansi output dari sumber agar sama dengan impedansi input dari beban." },
  { "en": "Mengapa penyesuaian impedansi penting?", "id": "Untuk memastikan transfer daya maksimum dari sumber ke beban dan untuk mencegah pantulan sinyal." },
  { "en": "Apa itu 'decibel' (dB)?", "id": "Satuan logaritmik yang digunakan untuk menyatakan perbandingan atau rasio, seperti gain atau atenuasi." },
  { "en": "Apa itu 'isolasi galvanik'?", "id": "Prinsip memisahkan dua bagian sirkuit sehingga tidak ada jalur konduktif langsung untuk arus, biasanya menggunakan transformator, optocoupler, atau kapasitor." },
  { "en": "Apa tujuan utama dari isolasi galvanik?", "id": "Untuk keselamatan (mencegah sengatan listrik) dan untuk memutus ground loop." },
  { "en": "Apa itu 'DAC R-2R Ladder'?", "id": "Jenis konverter digital-ke-analog yang menggunakan jaringan resistor dengan hanya dua nilai (R dan 2R)." },
  { "en": "Apa itu 'Flash ADC'?", "id": "Jenis konverter analog-ke-digital yang sangat cepat, menggunakan sejumlah besar komparator secara paralel." },
  { "en": "Apa itu 'Successive Approximation ADC'?", "id": "Jenis ADC yang umum digunakan, yang secara bertahap mendekati nilai digital yang benar dengan membandingkan input analog dengan output DAC internal." },
  { "en": "Apa itu 'Delta-Sigma ADC'?", "id": "Jenis ADC yang menggunakan oversampling dan noise shaping untuk mencapai resolusi yang sangat tinggi." },
  { "en": "Apa itu 'sampling rate' pada ADC?", "id": "Seberapa sering ADC mengambil 'sampel' atau mengukur sinyal analog input per detik." },
  { "en": "Apa yang dinyatakan oleh 'Teorema Nyquist-Shannon'?", "id": "Bahwa frekuensi sampling harus setidaknya dua kali frekuensi tertinggi dalam sinyal agar sinyal tersebut dapat direkonstruksi dengan sempurna." },
  { "en": "Apa itu 'aliasing' dalam proses sampling?", "id": "Efek distorsi yang terjadi ketika sinyal disampling pada frekuensi yang terlalu rendah, menyebabkan frekuensi tinggi muncul sebagai frekuensi rendah palsu." },
  { "en": "Apa fungsi dari 'anti-aliasing filter'?", "id": "Filter low-pass yang ditempatkan sebelum ADC untuk menghilangkan komponen frekuensi yang lebih tinggi dari setengah frekuensi sampling." },
  { "en": "Apa itu 'kuantisasi' (quantization) dalam proses ADC?", "id": "Proses memetakan nilai sampel analog yang kontinu ke salah satu dari sejumlah level digital yang diskrit." },
  { "en": "Apa itu 'quantization error'?", "id": "Perbedaan antara nilai analog aktual dan nilai digital yang dikuantisasi." },
  { "en": "Apa yang dimaksud dengan 'resolusi' sebuah ADC atau DAC?", "id": "Jumlah bit yang digunakannya, yang menentukan jumlah level diskrit yang dapat direpresentasikannya." },
  { "en": "Apa itu 'multiplexing'?", "id": "Teknik mengirim beberapa sinyal atau aliran informasi melalui satu jalur komunikasi atau media yang sama." },
  { "en": "Apa itu 'Time-Division Multiplexing' (TDM)?", "id": "Jenis multiplexing di mana setiap sinyal diberi slot waktu (time slot) yang berbeda pada jalur yang sama." },
  { "en": "Apa itu 'Frequency-Division Multiplexing' (FDM)?", "id": "Jenis multiplexing di mana setiap sinyal diberi pita frekuensi (frequency band) yang berbeda pada jalur yang sama." },
  { "en": "Apa itu 'full-duplex' communication?", "id": "Komunikasi dua arah yang dapat terjadi secara bersamaan (misalnya, telepon)." },
  { "en": "Apa itu 'half-duplex' communication?", "id": "Komunikasi dua arah, tetapi tidak dapat terjadi secara bersamaan; harus bergantian (misalnya, walkie-talkie)." },
  { "en": "Apa itu 'simplex' communication?", "id": "Komunikasi satu arah saja (misalnya, siaran radio atau TV)." },
  { "en": "Apa itu 'baud rate'?", "id": "Jumlah perubahan simbol atau sinyal per detik pada sebuah jalur komunikasi." },
  { "en": "Apa perbedaan antara baud rate dan bit rate?", "id": "Keduanya bisa sama, tetapi bit rate bisa lebih tinggi dari baud rate jika satu simbol merepresentasikan lebih dari satu bit." },
  { "en": "Apa itu 'protokol komunikasi'?", "id": "Satu set aturan dan standar yang mengatur bagaimana data ditransmisikan dan diterima antara dua perangkat atau lebih." },
  { "en": "Sebutkan contoh protokol komunikasi serial!", "id": "UART, SPI, dan I2C." },
  { "en": "Apa itu 'UART' (Universal Asynchronous Receiver-Transmitter)?", "id": "Perangkat keras yang menangani komunikasi serial asinkron, sering digunakan untuk komunikasi antar mikrokontroler atau ke PC (via RS-232)." },
  { "en": "Apa itu 'SPI' (Serial Peripheral Interface)?", "id": "Protokol komunikasi serial sinkron yang cepat, sering digunakan untuk berkomunikasi dengan kartu SD, display, dan sensor." },
  { "en": "Apa itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol komunikasi serial dua-kawat (SDA dan SCL) yang memungkinkan banyak perangkat (master dan slave) terhubung pada bus yang sama." },
  { "en": "Apa itu 'Master' dan 'Slave' dalam protokol SPI atau I2C?", "id": "Master adalah perangkat yang memulai komunikasi dan menyediakan sinyal clock, sedangkan Slave adalah perangkat yang merespons permintaan dari Master." },
  { "en": "Apa itu 'parity bit' dalam komunikasi serial?", "id": "Bit tambahan yang dikirim bersama data untuk mendeteksi kesalahan transmisi sederhana." },
  { "en": "Apa itu 'checksum'?", "id": "Nilai yang dihitung dari sekumpulan data, digunakan sebagai metode sederhana untuk memeriksa integritas data setelah transmisi." },
  { "en": "Apa itu 'CRC' (Cyclic Redundancy Check)?", "id": "Metode pengecekan kesalahan yang lebih canggih dan andal daripada checksum atau parity bit." },
  { "en": "Apa itu 'handshaking' dalam komunikasi?", "id": "Proses pertukaran sinyal antara perangkat pengirim dan penerima untuk mengatur aliran data (misalnya, sinyal 'siap menerima' atau 'berhenti mengirim')." },
  { "en": "Apa fungsi dari kristal kuarsa dalam jam tangan atau komputer?", "id": "Sebagai referensi waktu yang sangat stabil dan akurat (osilator) untuk menjaga ketepatan waktu." },
  { "en": "Apa itu 'saklar DIP' (DIP switch)?", "id": "Sekelompok saklar kecil dalam satu kemasan yang digunakan untuk mengkonfigurasi pengaturan perangkat keras." },
  { "en": "Apa itu 'jumper'?", "id": "Konektor kecil yang digunakan untuk menghubungkan dua pin pada header untuk mengatur atau memilih mode operasi sirkuit." },
  { "en": "Apa itu 'arus eddy' (eddy current)?", "id": "Arus melingkar yang diinduksi dalam konduktor oleh medan magnet yang berubah, seringkali menyebabkan kerugian energi dalam inti transformator." },
  { "en": "Bagaimana cara mengurangi kerugian akibat arus eddy pada inti trafo?", "id": "Dengan membuat inti dari lempengan-lempengan logam tipis yang dilaminasi (terisolasi satu sama lain)." },
  { "en": "Apa itu 'histeresis' pada bahan magnetik?", "id": "Kecenderungan bahan magnetik untuk tetap termagnetisasi setelah medan magnet eksternal dihilangkan, juga menyebabkan kerugian energi pada trafo." },
  { "en": "Apa itu 'tegangan tembus' atau 'dielectric strength'?", "id": "Kekuatan maksimum medan listrik yang dapat ditahan oleh bahan isolator sebelum gagal (breakdown)." },
  { "en": "Apa itu 'rangkaian terintegrasi monolitik'?", "id": "IC di mana semua komponen dan interkoneksinya dibentuk di atas satu substrat semikonduktor tunggal (satu keping)." },
  { "en": "Apa itu 'rangkaian terintegrasi hibrida'?", "id": "Sirkuit yang dibuat dengan menghubungkan beberapa komponen (seperti IC monolitik dan komponen diskrit) pada substrat isolasi yang sama." },
  { "en": "Apa itu 'VLSI' (Very Large Scale Integration)?", "id": "Proses pembuatan sirkuit terpadu dengan mengintegrasikan ratusan ribu hingga jutaan transistor ke dalam satu chip." },
  { "en": "Apa itu 'SoC' (System on a Chip)?", "id": "IC yang mengintegrasikan hampir semua komponen dari sebuah sistem komputer atau sistem elektronik lainnya ke dalam satu chip tunggal." },
  { "en": "Apa itu 'ASIC' (Application-Specific Integrated Circuit)?", "id": "IC yang dirancang khusus untuk tujuan atau aplikasi tertentu, bukan untuk penggunaan umum." },
  { "en": "Apa itu 'FPGA' (Field-Programmable Gate Array)?", "id": "IC yang berisi blok-blok logika yang dapat diprogram dan interkoneksi yang dapat dikonfigurasi ulang oleh pengguna setelah diproduksi." },
  { "en": "Apa keuntungan FPGA dibandingkan ASIC?", "id": "Fleksibilitas (dapat diprogram ulang), waktu pengembangan lebih cepat, dan biaya awal yang lebih rendah untuk volume kecil." },
  { "en": "Apa keuntungan ASIC dibandingkan FPGA?", "id": "Kinerja lebih tinggi, konsumsi daya lebih rendah, dan biaya per unit lebih murah untuk volume produksi yang sangat besar." },
  { "en": "Apa itu 'GPIO' (General-Purpose Input/Output)?", "id": "Pin pada IC (seperti mikrokontroler) yang fungsinya dapat dikontrol oleh pengguna untuk menjadi input atau output digital." },
  { "en": "Apa itu 'watchdog timer' pada mikrokontroler?", "id": "Timer internal yang akan me-reset mikrokontroler jika tidak di-reset secara berkala oleh program yang berjalan, berguna untuk memulihkan sistem dari 'hang'." },
  { "en": "Apa itu 'interrupt' (interupsi)?", "id": "Sinyal yang menyebabkan CPU menghentikan sementara tugas utamanya untuk menangani kejadian penting atau mendesak." },
  { "en": "Apa itu 'ISR' (Interrupt Service Routine)?", "id": "Fungsi atau kode khusus yang dieksekusi oleh CPU sebagai respons terhadap sebuah interupsi." },
  { "en": "Apa itu 'DMA' (Direct Memory Access)?", "id": "Fitur yang memungkinkan periferal untuk mentransfer data langsung ke atau dari memori tanpa melibatkan CPU, sehingga meningkatkan efisiensi sistem." },
  { "en": "Apa itu 'cache memory'?", "id": "Memori kecil berkecepatan sangat tinggi yang digunakan oleh CPU untuk menyimpan salinan data atau instruksi yang sering diakses dari memori utama yang lebih lambat." },
  { "en": "Apa itu 'pipelining' dalam arsitektur CPU?", "id": "Teknik di mana beberapa instruksi dieksekusi secara tumpang tindih dalam beberapa tahap, untuk meningkatkan throughput instruksi." },
  { "en": "Apa itu 'arus konvensional'?", "id": "Model aliran arus yang mengasumsikan muatan positif mengalir dari terminal positif ke terminal negatif (digunakan dalam analisis rangkaian)." },
  { "en": "Apa perbedaan antara arus konvensional dan aliran elektron?", "id": "Arahnya berlawanan; elektron sebenarnya mengalir dari negatif ke positif, tetapi arus konvensional (positif ke negatif) adalah standar yang digunakan dalam skematik." },
  { "en": "Apa itu 'osilator relaksasi'?", "id": "Jenis osilator non-linier di mana elemen penyimpan energi (seperti kapasitor) secara berkala diisi dan dikosongkan." },
  { "en": "Apa itu 'osilator Colpitts'?", "id": "Jenis osilator LC di mana umpan baliknya diambil dari pembagi tegangan yang terbuat dari dua kapasitor." },
  { "en": "Apa itu 'osilator Hartley'?", "id": "Jenis osilator LC di mana umpan baliknya diambil dari pembagi tegangan yang terbuat dari induktor yang di-tap (tapped inductor)." },
  { "en": "Apa itu 'filter aktif'?", "id": "Rangkaian filter yang menggunakan komponen aktif (seperti Op-Amp) selain resistor dan kapasitor." },
  { "en": "Apa keuntungan filter aktif dibandingkan filter pasif?", "id": "Dapat memberikan penguatan (gain), tidak memerlukan induktor (yang besar dan mahal), dan memiliki impedansi yang lebih baik." },
  { "en": "Apa itu 'filter Butterworth'?", "id": "Jenis desain filter yang dikenal karena memiliki respons frekuensi yang paling datar (flat) di passband." },
  { "en": "Apa itu 'filter Chebyshev'?", "id": "Jenis desain filter yang memiliki transisi yang lebih tajam antara passband dan stopband, tetapi dengan ripple (riak) di passband." },
  { "en": "Apa itu 'filter Bessel'?", "id": "Jenis desain filter yang dikenal karena memiliki respons fasa yang linier, yang menjaga bentuk gelombang tanpa distorsi." },
  { "en": "Apa itu 'orde' dari sebuah filter?", "id": "Ukuran kompleksitas filter yang menentukan seberapa curam kemiringan (roll-off) atenuasinya di luar passband." },
  { "en": "Apa itu 'frekuensi cut-off' pada sebuah filter?", "id": "Frekuensi di mana daya sinyal output turun menjadi setengah dari daya di passband (atau atenuasi -3dB)." },
  { "en": "Apa itu 'speaker' atau 'loudspeaker'?", "id": "Transduser yang mengubah sinyal listrik audio menjadi gelombang suara." },
  { "en": "Apa itu 'mikrofon'?", "id": "Transduser yang mengubah gelombang suara menjadi sinyal listrik." },
  { "en": "Apa itu 'mikrofon dinamis'?", "id": "Jenis mikrofon yang menggunakan diafragma, koil, dan magnet untuk menghasilkan sinyal listrik melalui induksi elektromagnetik." },
  { "en": "Apa itu 'mikrofon kondensor'?", "id": "Jenis mikrofon yang bekerja berdasarkan prinsip kapasitansi, di mana gelombang suara menggetarkan diafragma yang merupakan salah satu lempeng kapasitor." },
  { "en": "Mengapa mikrofon kondensor memerlukan daya (phantom power atau baterai)?", "id": "Untuk memberikan muatan pada lempeng kapasitor dan untuk memberi daya pada sirkuit pre-amplifier internalnya." },
  { "en": "Apa itu 'crossover' pada sistem speaker?", "id": "Rangkaian filter yang memisahkan sinyal audio menjadi pita frekuensi yang berbeda untuk dikirim ke driver speaker yang sesuai (woofer, midrange, tweeter)." },
  { "en": "Apa fungsi 'woofer' dalam sistem speaker?", "id": "Untuk mereproduksi suara frekuensi rendah (bass)." },
  { "en": "Apa fungsi 'tweeter' dalam sistem speaker?", "id": "Untuk mereproduksi suara frekuensi tinggi (treble)." },
  { "en": "Apa itu 'arus quiescent' atau 'arus diam'?", "id": "Arus DC yang mengalir dalam sebuah rangkaian (seperti amplifier) ketika tidak ada sinyal input yang diterapkan." },
  { "en": "Apa itu 'headroom' dalam sebuah amplifier?", "id": "Perbedaan antara level sinyal operasi normal dan level maksimum yang dapat ditangani oleh amplifier sebelum terjadi 'clipping' (distorsi)." },
  { "en": "Apa itu 'clipping'?", "id": "Bentuk distorsi yang terjadi ketika sinyal output amplifier mencoba melebihi tegangan catu dayanya, menyebabkan puncak gelombang menjadi terpotong (datar)." },
  { "en": "Apa itu 'distorsi harmonik total' atau THD (Total Harmonic Distortion)?", "id": "Ukuran seberapa besar sebuah amplifier menambahkan komponen frekuensi harmonik yang tidak diinginkan ke sinyal output." },
  { "en": "Apa itu 'sinyal-ke-derau' atau SNR (Signal-to-Noise Ratio)?", "id": "Rasio antara kekuatan sinyal yang diinginkan dan kekuatan noise latar belakang." },
  { "en": "Apakah SNR yang lebih tinggi lebih baik atau lebih buruk?", "id": "Lebih baik, karena itu berarti sinyal jauh lebih kuat dibandingkan dengan noise." },
  { "en": "Apa itu 'kabel koaksial' (coaxial cable)?", "id": "Jenis kabel yang memiliki konduktor dalam di tengah, dikelilingi oleh lapisan isolator, kemudian perisai konduktif, dan jaket luar." },
  { "en": "Di mana kabel koaksial sering digunakan?", "id": "Untuk sinyal frekuensi tinggi seperti sinyal TV kabel, antena, dan jaringan komputer awal." },
  { "en": "Apa itu 'impedansi karakteristik' dari sebuah kabel?", "id": "Impedansi efektif dari sebuah kabel transmisi untuk sinyal AC, yang harus disesuaikan untuk transfer daya maksimum." },
  { "en": "Apa itu 'saklar SPST'?", "id": "Single Pole, Single Throw; saklar on-off paling sederhana yang memiliki satu input dan satu output." },
  { "en": "Apa itu 'saklar SPDT'?", "id": "Single Pole, Double Throw; saklar yang menghubungkan satu input ke salah satu dari dua output." },
  { "en": "Apa itu 'saklar DPDT'?", "id": "Double Pole, Double Throw; saklar yang setara dengan dua saklar SPDT yang dioperasikan bersamaan." },
  { "en": "Apa itu 'saklar momentary'?", "id": "Saklar yang hanya aktif selama ditekan (seperti bel pintu atau tombol keyboard)." },
  { "en": "Apa itu 'saklar toggle'?", "id": "Saklar yang tetap berada di posisinya setelah diaktifkan (seperti saklar lampu)." },
  { "en": "Apa itu 'resistor film logam' (metal film resistor)?", "id": "Jenis resistor yang dibuat dengan menyemprotkan lapisan tipis logam pada inti keramik, dikenal karena akurasi dan stabilitasnya yang baik." },
  { "en": "Apa itu 'resistor film karbon' (carbon film resistor)?", "id": "Jenis resistor umum yang lebih murah tetapi kurang akurat dibandingkan resistor film logam." },
  { "en": "Apa itu 'resistor wirewound'?", "id": "Resistor yang dibuat dengan melilitkan kawat resistif di sekitar inti isolator, digunakan untuk aplikasi daya tinggi." },
  { "en": "Apa itu 'LCD' (Liquid Crystal Display)?", "id": "Jenis display datar yang menggunakan kristal cair yang sifat optiknya dapat diubah oleh medan listrik." },
  { "en": "Mengapa LCD memerlukan 'backlight' (cahaya latar)?", "id": "Karena kristal cair itu sendiri tidak memancarkan cahaya, hanya memblokir atau melewatkan cahaya dari backlight." },
  { "en": "Apa itu 'OLED' (Organic Light Emitting Diode)?", "id": "Jenis display di mana setiap pikselnya adalah dioda organik yang dapat memancarkan cahayanya sendiri." },
  { "en": "Apa keuntungan utama OLED dibandingkan LCD?", "id": "Tidak memerlukan backlight, sehingga dapat menghasilkan warna hitam yang sempurna, kontras lebih tinggi, dan bodi lebih tipis." },
  { "en": "Apa itu 'display 7-segmen'?", "id": "Perangkat display elektronik untuk menampilkan angka desimal, terdiri dari tujuh segmen LED atau LCD." },
  { "en": "Apa itu 'common anode' pada display 7-segmen?", "id": "Konfigurasi di mana semua anoda dari segmen LED terhubung bersama ke VCC." },
  { "en": "Apa itu 'common cathode' pada display 7-segmen?", "id": "Konfigurasi di mana semua katoda dari segmen LED terhubung bersama ke ground." },
  { "en": "Apa itu 'efek termoelektrik'?", "id": "Istilah umum yang mencakup konversi langsung dari perbedaan suhu menjadi tegangan listrik (efek Seebeck) dan sebaliknya (efek Peltier)." },
  { "en": "Apa itu 'efek Peltier'?", "id": "Fenomena di mana panas diserap atau dilepaskan pada sambungan dua bahan berbeda ketika arus listrik dialirkan melaluinya." },
  { "en": "Apa aplikasi dari efek Peltier?", "id": "Pendingin termoelektrik atau 'Peltier cooler', yang dapat mendinginkan benda tanpa bagian yang bergerak." },
  { "en": "Apa itu 'baterai primer'?", "id": "Baterai sekali pakai yang tidak dapat diisi ulang (misalnya, baterai alkaline)." },
  { "en": "Apa itu 'baterai sekunder'?", "id": "Baterai yang dapat diisi ulang (misalnya, baterai Li-ion, Ni-MH, atau aki timbal-asam)." },
  { "en": "Apa yang dimaksud dengan 'kapasitas' baterai?", "id": "Jumlah muatan yang dapat disimpannya, biasanya diukur dalam Ampere-jam (Ah) atau miliampere-jam (mAh)." },
  { "en": "Apa yang dimaksud dengan 'self-discharge' pada baterai?", "id": "Kecenderungan baterai untuk kehilangan muatannya secara perlahan bahkan ketika tidak digunakan." },
  { "en": "Apa itu 'siklus hidup' (cycle life) dari baterai isi ulang?", "id": "Jumlah siklus pengisian-pengosongan penuh yang dapat ditangani oleh baterai sebelum kapasitasnya turun secara signifikan." },
  { "en": "Apa itu 'BMS' (Battery Management System)?", "id": "Sirkuit elektronik yang melindungi baterai isi ulang dari kondisi berbahaya seperti pengisian berlebih, pengosongan berlebih, dan arus berlebih." },
  { "en": "Apa itu 'sel surya' (solar cell) atau 'sel fotovoltaik'?", "id": "Perangkat semikonduktor yang mengubah energi cahaya matahari secara langsung menjadi energi listrik." },
  { "en": "Apa itu 'superkapasitor' atau 'ultrakapasitor'?", "id": "Kapasitor dengan nilai kapasitansi yang sangat tinggi, berada di antara kapasitor konvensional dan baterai isi ulang dalam hal kepadatan energi dan daya." },
  { "en": "Apa keunggulan superkapasitor dibandingkan baterai?", "id": "Dapat diisi dan dikosongkan jauh lebih cepat dan memiliki siklus hidup yang hampir tak terbatas." },
  { "en": "Apa kekurangan superkapasitor dibandingkan baterai?", "id": "Kepadatan energinya (jumlah energi yang disimpan per unit berat) jauh lebih rendah." },
  { "en": "Apa itu 'arus drift' dalam semikonduktor?", "id": "Aliran pembawa muatan (elektron dan hole) yang disebabkan oleh adanya medan listrik eksternal." },
  { "en": "Apa itu 'arus difusi' dalam semikonduktor?", "id": "Aliran pembawa muatan dari daerah konsentrasi tinggi ke daerah konsentrasi rendah, bahkan tanpa medan listrik." },
  { "en": "Apa yang dimaksud dengan 'pembawa muatan mayoritas'?", "id": "Jenis pembawa muatan yang paling melimpah dalam semikonduktor yang didoping (elektron di tipe-N, hole di tipe-P)." },
  { "en": "Apa yang dimaksud dengan 'pembawa muatan minoritas'?", "id": "Jenis pembawa muatan yang jumlahnya lebih sedikit dalam semikonduktor yang didoping (hole di tipe-N, elektron di tipe-P)." },
  { "en": "Apa itu 'daerah deplesi' (depletion region) pada sambungan P-N?", "id": "Daerah di sekitar sambungan yang kekurangan pembawa muatan bebas karena rekombinasi elektron dan hole." },
  { "en": "Apa itu 'tegangan penghalang' (barrier potential) pada sambungan P-N?", "id": "Tegangan internal yang terbentuk di daerah deplesi yang harus diatasi agar arus dapat mengalir (sekitar 0.7V untuk silikon)." },
  { "en": "Apa fungsi dari sensor ultrasonik?", "id": "Untuk mengukur jarak dengan cara memancarkan gelombang suara frekuensi tinggi dan mengukur waktu yang dibutuhkan untuk pantulannya kembali." },
  { "en": "Apa fungsi dari sensor PIR (Passive Infrared)?", "id": "Untuk mendeteksi energi panas (radiasi inframerah) yang dipancarkan oleh benda bergerak, seperti manusia atau hewan." },
  { "en": "Apa fungsi dari sebuah akselerometer (accelerometer)?", "id": "Untuk mengukur percepatan atau getaran, sering digunakan untuk mendeteksi orientasi atau gerakan perangkat." },
  { "en": "Apa fungsi dari sebuah giroskop (gyroscope)?", "id": "Untuk mengukur atau mempertahankan orientasi dan kecepatan sudut, sering digunakan untuk stabilisasi gambar atau navigasi." },
  { "en": "Apa fungsi dari sensor kelembapan (humidity sensor)?", "id": "Untuk mengukur jumlah uap air di udara (kelembapan relatif)." },
  { "en": "Apa fungsi dari sensor tekanan (pressure sensor)?", "id": "Untuk mengukur tekanan gas atau cairan, sering digunakan dalam aplikasi cuaca atau industri." },
  { "en": "Apa fungsi dari sensor gas?", "id": "Untuk mendeteksi keberadaan gas-gas tertentu di udara, seperti LPG, karbon monoksida, atau asap." },
  { "en": "Apa itu 'strain gauge'?", "id": "Sensor yang resistansinya berubah ketika diregangkan atau ditekan, digunakan untuk mengukur tegangan mekanis atau beban." },
  { "en": "Apa itu 'load cell'?", "id": "Transduser yang mengubah gaya atau beban menjadi sinyal listrik, biasanya menggunakan beberapa strain gauge." },
  { "en": "Apa fungsi dari 'rotary encoder'?", "id": "Sensor untuk mengukur posisi sudut atau kecepatan putaran sebuah poros." },
  { "en": "Apa perbedaan antara rotary encoder absolut dan inkremental?", "id": "Encoder absolut memberikan posisi sudut yang sebenarnya, sedangkan encoder inkremental hanya memberikan informasi tentang perubahan posisi." },
  { "en": "Apa itu 'termal runaway' pada transistor?", "id": "Kondisi tidak stabil di mana peningkatan suhu menyebabkan peningkatan arus, yang selanjutnya meningkatkan suhu, dan dapat merusak transistor." },
  { "en": "Apa mode kegagalan umum pada kapasitor elektrolit?", "id": "Mengeringnya elektrolit (kehilangan kapasitansi), hubungan pendek (short), atau kebocoran (leakage)." },
  { "en": "Apa itu 'kemasan' (package) komponen seperti TO-92 atau TO-220?", "id": "Bentuk fisik dan wadah dari sebuah komponen semikonduktor yang melindungi chip silikon dan menyediakan koneksi (kaki)." },
  { "en": "Apa perbedaan utama kemasan TO-92 dan TO-220?", "id": "TO-220 lebih besar dan memiliki tab logam untuk pemasangan heatsink, dirancang untuk daya yang lebih tinggi daripada TO-92." },
  { "en": "Apa itu kemasan DIP (Dual In-line Package)?", "id": "Jenis kemasan IC untuk teknologi through-hole (THT) yang memiliki dua baris pin paralel." },
  { "en": "Apa itu kemasan SOIC (Small Outline Integrated Circuit)?", "id": "Jenis kemasan untuk komponen surface-mount (SMD) yang mirip dengan DIP tetapi lebih kecil dan pinnya ditekuk keluar." },
  { "en": "Apa itu 'reflow soldering'?", "id": "Proses penyolderan komponen SMD dengan cara memanaskan seluruh PCB di dalam oven hingga pasta solder meleleh." },
  { "en": "Apa fungsi dari 'solder wick' (kawat solder)?", "id": "Kawat tembaga jalinan yang digunakan untuk mengangkat atau membersihkan solder berlebih dari PCB (desoldering)." },
  { "en": "Apa fungsi dari 'solder pump' (pompa solder)?", "id": "Alat vakum untuk menyedot solder cair dari sambungan saat melakukan desoldering." },
  { "en": "Apa itu 'gelombang mikro' (microwave)?", "id": "Gelombang elektromagnetik dengan frekuensi sangat tinggi, digunakan dalam radar, komunikasi satelit, dan oven microwave." },
  { "en": "Apa itu 'waveguide' (pemandu gelombang)?", "id": "Pipa logam berongga yang digunakan untuk memandu gelombang mikro dari satu titik ke titik lain." },
  { "en": "Apa itu 'microstrip'?", "id": "Jenis jalur transmisi frekuensi tinggi pada PCB yang terdiri dari jalur tembaga di satu sisi dan ground plane di sisi lain." },
  { "en": "Apa fungsi dari 'mixer' dalam penerima radio?", "id": "Untuk mengubah sinyal frekuensi radio (RF) yang diterima ke frekuensi yang lebih rendah yang disebut frekuensi menengah (IF)." },
  { "en": "Apa itu 'superheterodyne receiver'?", "id": "Jenis arsitektur penerima radio yang paling umum, yang menggunakan prinsip konversi frekuensi ke IF." },
  { "en": "Apa fungsi dari 'Low-Noise Amplifier' (LNA)?", "id": "Penguat pertama dalam penerima radio, dirancang untuk memperkuat sinyal RF yang sangat lemah tanpa menambahkan banyak noise." },
  { "en": "Apa fungsi dari 'Power Amplifier' (PA) dalam pemancar?", "id": "Penguat terakhir dalam pemancar, dirancang untuk meningkatkan kekuatan sinyal RF ke level yang cukup untuk dipancarkan oleh antena." },
  { "en": "Apa itu 'Standing Wave Ratio' (SWR)?", "id": "Ukuran ketidaksesuaian impedansi antara jalur transmisi (kabel) dan bebannya (antena), yang menunjukkan seberapa banyak daya yang dipantulkan kembali." },
  { "en": "Berapa nilai SWR yang ideal?", "id": "1:1, yang berarti tidak ada daya yang dipantulkan dan semua daya ditransfer ke antena." },
  { "en": "Apa fungsi dari 'antenna tuner' atau 'matching network'?", "id": "Sirkuit untuk mencocokkan impedansi antara pemancar dan antena guna memaksimalkan transfer daya dan meminimalkan SWR." },
  { "en": "Apa itu 'konverter Buck'?", "id": "Jenis konverter DC-DC step-down, yang menurunkan tegangan DC secara efisien." },
  { "en": "Apa itu 'konverter Boost'?", "id": "Jenis konverter DC-DC step-up, yang menaikkan tegangan DC secara efisien." },
  { "en": "Apa itu 'konverter Buck-Boost'?", "id": "Jenis konverter DC-DC yang dapat menghasilkan tegangan output yang lebih tinggi atau lebih rendah dari input, dengan polaritas terbalik." },
  { "en": "Apa itu 'konverter Flyback'?", "id": "Jenis konverter DC-DC terisolasi yang menggunakan transformator untuk menyimpan dan mentransfer energi." },
  { "en": "Apa itu 'inverter'?", "id": "Sirkuit elektronik yang mengubah daya DC menjadi daya AC." },
  { "en": "Apa itu 'logic analyzer'?", "id": "Alat ukur yang dapat menangkap dan menampilkan banyak sinyal digital dari sebuah sistem secara bersamaan." },
  { "en": "Apa perbedaan utama antara osiloskop dan logic analyzer?", "id": "Osiloskop menampilkan informasi timing dan tegangan (analog) dari beberapa sinyal, sedangkan logic analyzer menampilkan keadaan logika (1 atau 0) dari banyak sinyal." },
  { "en": "Apa itu 'spectrum analyzer'?", "id": "Alat ukur yang menampilkan kekuatan sinyal dalam domain frekuensi (menunjukkan frekuensi apa saja yang ada dalam sebuah sinyal)." },
  { "en": "Apa fungsi dari mode 'AC coupling' pada input osiloskop?", "id": "Untuk memblokir komponen DC dari sinyal input, sehingga hanya menampilkan komponen AC-nya saja." },
  { "en": "Apa fungsi dari mode 'DC coupling' pada input osiloskop?", "id": "Untuk menampilkan sinyal input secara keseluruhan, termasuk komponen DC dan AC-nya." },
  { "en": "Apa fungsi 'probe compensation' pada osiloskop?", "id": "Menyesuaikan kapasitansi probe agar sesuai dengan input osiloskop untuk memastikan pengukuran yang akurat pada frekuensi tinggi." },
  { "en": "Apa yang dimaksud dengan 'thermal resistance' pada komponen?", "id": "Ukuran seberapa efektif sebuah komponen dapat membuang panas ke lingkungan sekitarnya." },
  { "en": "Mengapa pasta termal (thermal paste) digunakan antara IC dan heatsink?", "id": "Untuk mengisi celah udara mikroskopis dan meningkatkan transfer panas dari IC ke heatsink." },
  { "en": "Apa itu 'failure mode' dari sebuah komponen?", "id": "Cara spesifik di mana sebuah komponen gagal berfungsi, misalnya 'open circuit' atau 'short circuit'." },
  { "en": "Apa kegagalan 'open circuit'?", "id": "Kegagalan di mana jalur konduktif di dalam komponen terputus, sehingga tidak ada arus yang bisa lewat." },
  { "en": "Apa kegagalan 'short circuit'?", "id": "Kegagalan di mana terjadi hubungan ber-resistansi rendah yang tidak diinginkan di dalam komponen." },
  { "en": "Apa itu 'derating' komponen?", "id": "Praktik mengoperasikan komponen di bawah batas maksimumnya (tegangan, arus, daya) untuk meningkatkan keandalan dan masa pakainya." },
  { "en": "Apa yang dimaksud dengan 'hot-swap'?", "id": "Kemampuan untuk memasang atau melepas komponen dari sistem saat sistem tersebut masih menyala tanpa menyebabkan kerusakan." },
  { "en": "Apa itu 'crowbar circuit' pada catu daya?", "id": "Sirkuit proteksi yang sengaja membuat hubungan pendek (short circuit) untuk memutus sekering jika terjadi tegangan berlebih (overvoltage)." },
  { "en": "Apa itu 'soft start' pada catu daya?", "id": "Fitur yang secara bertahap meningkatkan tegangan output saat dinyalakan untuk membatasi 'inrush current'." },
  { "en": "Apa itu 'Digital Signal Processing' (DSP)?", "id": "Pemrosesan sinyal menggunakan teknik matematika dan komputasi pada representasi digital dari sinyal tersebut." },
  { "en": "Apa fungsi dari Transformasi Fourier (Fourier Transform)?", "id": "Alat matematika untuk menguraikan sebuah sinyal dalam domain waktu menjadi komponen-komponen frekuensinya." },
  { "en": "Apa itu 'filter FIR' (Finite Impulse Response)?", "id": "Jenis filter digital yang outputnya hanya bergantung pada input saat ini dan input sebelumnya (tidak menggunakan feedback)." },
  { "en": "Apa itu 'filter IIR' (Infinite Impulse Response)?", "id": "Jenis filter digital yang menggunakan umpan balik (feedback), sehingga outputnya bergantung pada input dan output sebelumnya." },
  { "en": "Apa keunggulan filter FIR?", "id": "Selalu stabil dan dapat dengan mudah dirancang untuk memiliki fasa yang linier." },
  { "en": "Apa keunggulan filter IIR?", "id": "Jauh lebih efisien secara komputasi untuk mencapai respons frekuensi yang sama dengan filter FIR." },
  { "en": "Apa itu 'sampling' dalam DSP?", "id": "Proses mengubah sinyal analog kontinu menjadi serangkaian nilai diskrit pada interval waktu yang teratur." },
  { "en": "Apa itu 'isolator digital'?", "id": "Komponen yang menggunakan teknologi seperti kapasitif, magnetik, atau optik untuk mentransfer sinyal digital sambil menjaga isolasi galvanik." },
  { "en": "Apa itu 'MEMS' (Micro-Electro-Mechanical Systems)?", "id": "Teknologi miniaturisasi yang menggabungkan elemen mekanis, sensor, dan elektronik pada substrat silikon yang sama." },
  { "en": "Sebutkan contoh perangkat MEMS!", "id": "Akselerometer dan giroskop di smartphone, kepala inkjet printer, dan mikrofon silikon." },
  { "en": "Apa itu 'keluaran push-pull' (push-pull output)?", "id": "Jenis tahap keluaran yang menggunakan dua transistor (satu untuk 'mendorong' arus ke beban, satu lagi untuk 'menarik' arus dari beban) untuk menggerakkan sinyal HIGH dan LOW secara aktif." },
  { "en": "Apa itu 'class-G' atau 'class-H' amplifier?", "id": "Jenis penguat audio yang meningkatkan efisiensi dengan cara mengubah-ubah tegangan catu dayanya sesuai dengan level sinyal." },
  { "en": "Apa itu 'Linear Feedback Shift Register' (LFSR)?", "id": "Register geser yang inputnya merupakan fungsi linier dari keadaan sebelumnya, digunakan untuk menghasilkan urutan angka pseudo-acak." },
  { "en": "Apa itu 'angka pseudo-acak'?", "id": "Urutan angka yang tampak acak tetapi sebenarnya dihasilkan oleh proses deterministik dan akan berulang." },
  { "en": "Apa itu 'dead time' dalam rangkaian half-bridge atau full-bridge?", "id": "Waktu tunda singkat yang disisipkan saat mematikan satu saklar dan menyalakan saklar lainnya untuk mencegah 'shoot-through'." },
  { "en": "Apa itu 'shoot-through'?", "id": "Kondisi berbahaya di mana saklar atas dan bawah dalam satu 'kaki' jembatan menyala bersamaan, menyebabkan hubungan pendek pada catu daya." },
  { "en": "Apa itu 'kelvin connection' atau '4-terminal sensing'?", "id": "Teknik pengukuran untuk mengukur resistansi rendah secara akurat dengan menggunakan empat kabel: dua untuk membawa arus dan dua untuk mengukur tegangan langsung di komponen." },
  { "en": "Mengapa 'kelvin connection' diperlukan untuk resistansi rendah?", "id": "Untuk menghilangkan kesalahan pengukuran yang disebabkan oleh resistansi pada kabel dan titik kontak." },
  { "en": "Apa itu 'guarding' dalam teknik pengukuran?", "id": "Teknik menggunakan konduktor pelindung yang dijaga pada potensial yang sama dengan sinyal yang diukur untuk meminimalkan efek arus bocor." },
  { "en": "Apa itu 'kapasitor keramik multilayer' (MLCC)?", "id": "Jenis kapasitor SMD yang sangat umum, dibuat dengan menumpuk lapisan keramik dan elektroda logam." },
  { "en": "Apa itu 'induktor mode-common' (common-mode choke)?", "id": "Induktor dengan dua lilitan pada satu inti, dirancang untuk menekan noise yang muncul bersamaan (common-mode) pada kedua kabel." },
  { "en": "Apa itu 'noise diferensial' (differential-mode noise)?", "id": "Noise yang muncul dengan arah berlawanan antara dua konduktor dalam satu pasang." },
  { "en": "Apa itu 'transorb' atau 'TVS diode' (Transient Voltage Suppression)?", "id": "Dioda yang dirancang untuk melindungi sirkuit dari lonjakan tegangan sesaat (transient) dengan cara 'menjepit' (clamping) tegangan berlebih." },
  { "en": "Apa itu 'varistor' atau MOV (Metal-Oxide Varistor)?", "id": "Komponen yang resistansinya bergantung pada tegangan, digunakan untuk proteksi terhadap lonjakan tegangan tinggi seperti sambaran petir." },
  { "en": "Apa itu 'in-circuit emulator' (ICE)?", "id": "Alat debugging perangkat keras yang meniru (emulate) CPU dalam sistem target, memberikan kontrol penuh dan visibilitas ke internal sistem." },
  { "en": "Apa itu 'JTAG' (Joint Test Action Group)?", "id": "Standar antarmuka untuk verifikasi desain, pengujian, dan pemrograman sirkuit terpadu pada level papan." },
  { "en": "Apa fungsi dari 'boundary scan' pada JTAG?", "id": "Memungkinkan pengujian koneksi antar IC pada PCB tanpa memerlukan probe fisik di setiap pin." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sistem kontrol umpan balik yang menghasilkan sinyal output yang fasanya terkait secara tetap dengan fasa sinyal input referensi." },
  { "en": "Sebutkan aplikasi dari PLL!", "id": "Sintesis frekuensi (frequency synthesis), pemulihan clock (clock recovery), dan demodulasi FM." },
  { "en": "Apa itu 'Voltage-Controlled Oscillator' (VCO)?", "id": "Osilator yang frekuensi osilasinya dapat diubah-ubah dengan mengatur tegangan input DC." },
  { "en": "Apa itu 'charge pump'?", "id": "Jenis konverter DC-DC yang menggunakan kapasitor untuk menyimpan dan mentransfer energi, seringkali untuk menghasilkan tegangan yang lebih tinggi atau terbalik." },
  { "en": "Apa itu 'DAC string'?", "id": "Jenis DAC sederhana yang terdiri dari serangkaian resistor identik yang berfungsi sebagai pembagi tegangan." },
  { "en": "Apa itu 'DAC sigma-delta'?", "id": "Jenis DAC yang menggunakan oversampling dan noise shaping untuk mencapai resolusi tinggi, sering digunakan dalam aplikasi audio." },
  { "en": "Apa itu 'interleaving' dalam ADC?", "id": "Teknik menggunakan beberapa ADC secara paralel untuk mencapai frekuensi sampling efektif yang sangat tinggi." },
  { "en": "Apa itu 'dithering' dalam pemrosesan sinyal?", "id": "Proses menambahkan noise acak tingkat rendah ke sinyal sebelum kuantisasi untuk meningkatkan rentang dinamis efektif." },
  { "en": "Apa itu 'metastability' dalam sirkuit digital?", "id": "Keadaan tidak stabil dan tidak dapat diprediksi yang dapat terjadi pada flip-flop ketika inputnya berubah terlalu dekat dengan tepi clock." },
  { "en": "Apa itu 'sinkroniser'?", "id": "Sirkuit (biasanya terdiri dari dua flip-flop atau lebih) yang digunakan untuk mentransfer sinyal dengan aman dari satu domain clock ke domain clock lain." },
  { "en": "Apa itu 'clock domain crossing' (CDC)?", "id": "Situasi dalam desain digital di mana sinyal harus melewati dari satu bagian sirkuit yang dioperasikan oleh satu clock ke bagian lain yang dioperasikan oleh clock yang berbeda." },
  { "en": "Apa itu 'Gray code'?", "id": "Sistem pengkodean biner di mana dua nilai yang berurutan hanya berbeda satu bit, berguna untuk mencegah kesalahan pada rotary encoder." },
  { "en": "Apa itu 'one-hot encoding'?", "id": "Representasi digital di mana hanya satu bit yang aktif (HIGH) pada satu waktu untuk menunjukkan suatu keadaan." },
  { "en": "Apa itu 'state machine' (mesin keadaan)?", "id": "Model komputasi matematis yang terdiri dari serangkaian keadaan (states), transisi antar keadaan, dan aksi." },
  { "en": "Apa itu 'SPICE' (Simulation Program with Integrated Circuit Emphasis)?", "id": "Program simulasi sirkuit elektronik analog serbaguna yang menjadi standar industri." },
  { "en": "Apa itu 'analisis transien' dalam SPICE?", "id": "Simulasi yang menghitung perilaku sirkuit terhadap waktu sebagai respons terhadap input tertentu." },
  { "en": "Apa itu 'analisis AC' dalam SPICE?", "id": "Simulasi yang menghitung respons frekuensi dari sebuah sirkuit linier." },
  { "en": "Apa itu 'analisis DC' dalam SPICE?", "id": "Simulasi yang menghitung titik operasi DC (tegangan dan arus diam) dari sebuah sirkuit." },
  { "en": "Apa itu 'Verilog' atau 'VHDL'?", "id": "Hardware Description Language (HDL), yaitu bahasa berbasis teks yang digunakan untuk mendeskripsikan dan merancang sirkuit digital." },
  { "en": "Apa itu 'sintesis' dalam proses desain HDL?", "id": "Proses otomatis mengubah deskripsi HDL menjadi implementasi gerbang logika (netlist)." },
  { "en": "Apa itu 'Gerber file'?", "id": "Format file standar yang digunakan untuk mendeskripsikan setiap lapisan dari sebuah desain PCB untuk keperluan fabrikasi." },
  { "en": "Apa itu 'pick-and-place machine'?", "id": "Mesin robotik yang digunakan untuk mengambil komponen SMD dari gulungan atau baki dan menempatkannya secara akurat pada PCB." },
  { "en": "Apa itu 'electromigration'?", "id": "Fenomena perpindahan material secara bertahap dalam sebuah konduktor akibat aliran elektron, yang dapat menyebabkan kegagalan pada IC." },
  { "en": "Apa itu 'latch-up' pada sirkuit CMOS?", "id": "Kondisi seperti hubungan pendek antara VCC dan Ground yang disebabkan oleh terpicunya struktur parasit SCR, yang dapat merusak chip." },
  { "en": "Apa itu 'amplifier diferensial'?", "id": "Penguat yang memperkuat perbedaan tegangan antara dua sinyal input." },
  { "en": "Apa itu 'amplifier instrumentasi'?", "id": "Jenis penguat diferensial presisi tinggi dengan impedansi input yang sangat tinggi dan CMRR yang tinggi, sering digunakan untuk sinyal sensor." },
  { "en": "Apa itu 'negative feedback'?", "id": "Proses mengumpanbalikkan sebagian sinyal keluaran ke masukan secara berlawanan fasa untuk menstabilkan penguatan dan mengurangi distorsi." },
  { "en": "Apa itu 'positive feedback'?", "id": "Proses mengumpanbalikkan sebagian sinyal keluaran ke masukan secara sefasa untuk menciptakan regenerasi atau osilasi." },
  { "en": "Apa itu 'Bode plot'?", "id": "Grafik yang menunjukkan respons frekuensi dari sebuah sistem, terdiri dari plot magnitudo (gain) dan plot fasa terhadap frekuensi." },
  { "en": "Apa itu 'gain-bandwidth product' (GBP) pada Op-Amp?", "id": "Parameter yang menunjukkan bahwa hasil kali antara gain tegangan loop terbuka dan bandwidthnya adalah konstan." },
  { "en": "Apa itu 'tegangan offset input' pada Op-Amp?", "id": "Tegangan DC kecil yang harus diterapkan antara dua input untuk membuat tegangan output menjadi nol." },
  { "en": "Apa itu 'arus bias input' pada Op-Amp?", "id": "Arus DC kecil yang harus mengalir ke dalam (atau keluar dari) terminal input Op-Amp agar transistor internalnya dapat beroperasi dengan benar." },
  { "en": "Apa yang dimaksud dengan 'ESR' (Equivalent Series Resistance) pada sebuah kapasitor?", "id": "Resistansi internal parasit yang tidak diinginkan dalam sebuah kapasitor, yang dapat menyebabkan pemanasan dan memengaruhi kinerjanya pada frekuensi tinggi." },
  { "en": "Apa itu efek 'dielectric absorption' pada kapasitor?", "id": "Kecenderungan kapasitor untuk tidak sepenuhnya kosong dan secara perlahan memulihkan sebagian kecil tegangannya setelah dikosongkan, yang penting dalam rangkaian presisi." },
  { "en": "Apa fungsi dari 'third hand' tool (tangan ketiga) dalam proses penyolderan?", "id": "Alat bantu dengan penjepit untuk memegang PCB atau komponen agar kedua tangan bebas untuk memegang solder dan timah." },
  { "en": "Mengapa penting untuk membersihkan sisa 'flux' dari PCB setelah menyolder?", "id": "Karena sisa flux bisa bersifat korosif atau konduktif seiring waktu, yang dapat menyebabkan kegagalan rangkaian." },
  { "en": "Apa fungsi dari 'bleeder resistor' pada catu daya tegangan tinggi?", "id": "Untuk mengosongkan muatan pada kapasitor filter secara aman setelah catu daya dimatikan." },
  { "en": "Apa yang dimaksud dengan 'bi-amping' pada sistem audio?", "id": "Praktik menggunakan dua penguat terpisah untuk menggerakkan driver frekuensi rendah (woofer) dan frekuensi tinggi (tweeter) pada sebuah speaker." },
  { "en": "Apa perbedaan utama antara sinyal audio 'balanced' dan 'unbalanced'?", "id": "Sinyal unbalanced menggunakan dua kabel (sinyal dan ground), sedangkan sinyal balanced menggunakan tiga kabel (sinyal positif, sinyal negatif, dan ground) untuk penolakan noise yang lebih baik." },
  { "en": "Apa itu 'Karnaugh Map' (K-map) dalam logika digital?", "id": "Metode grafis yang digunakan untuk menyederhanakan ekspresi aljabar Boolean." },
  { "en": "Apa itu 'Lissajous figure' yang ditampilkan pada osiloskop?", "id": "Pola yang terbentuk ketika sinyal sinusoidal diterapkan ke input horizontal dan vertikal, digunakan untuk mengukur rasio frekuensi dan perbedaan fasa." },
  { "en": "Apa itu 'balun' (balanced to unbalanced)?", "id": "Sebuah transformator atau sirkuit yang digunakan untuk menghubungkan jalur transmisi yang seimbang (seperti kabel twin-lead) ke jalur yang tidak seimbang (seperti kabel koaksial)." },
  { "en": "Apa arti dari tanda titik (dot) pada simbol skematik transformator?", "id": "Menunjukkan polaritas atau fasa dari lilitan; terminal dengan titik memiliki fasa yang sama pada saat yang bersamaan." },
  { "en": "Apa itu 'tegangan kerja' (working voltage) pada sebuah komponen?", "id": "Tegangan maksimum yang dapat ditangani oleh komponen secara terus-menerus dalam kondisi operasi normal." }






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
            }, 7500);
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
