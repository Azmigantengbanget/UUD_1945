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


    { "en": "Bentuk negara Indonesia adalah kesatuan yang berbentuk Republik?", "id": "Bab I, Pasal 1 ayat (1)." },
    { "en": "Kedaulatan berada di tangan rakyat dan dilaksanakan menurut UUD?", "id": "Bab I, Pasal 1 ayat (2)." },
    { "en": "Indonesia adalah negara hukum?", "id": "Bab I, Pasal 1 ayat (3)." },
    { "en": "MPR terdiri atas anggota DPR dan anggota DPD?", "id": "Bab II, Pasal 2 ayat (1)." },
    { "en": "MPR berwenang mengubah dan menetapkan Undang-Undang Dasar?", "id": "Bab II, Pasal 3 ayat (1)." },
    { "en": "Presiden memegang kekuasaan pemerintahan menurut UUD?", "id": "Bab III, Pasal 4 ayat (1)." },
    { "en": "Syarat menjadi Calon Presiden dan Calon Wakil Presiden?", "id": "Bab III, Pasal 6 ayat (1)." },
    { "en": "Masa jabatan Presiden dan Wakil Presiden adalah lima tahun dan hanya bisa dipilih kembali satu kali?", "id": "Bab III, Pasal 7." },
    { "en": "Presiden dapat diberhentikan oleh MPR atas usul DPR?", "id": "Bab III, Pasal 7A." },
    { "en": "Presiden memegang kekuasaan yang tertinggi atas Angkatan Darat, Angkatan Laut dan Angkatan Udara?", "id": "Bab III, Pasal 10." },
    { "en": "Presiden menyatakan perang, membuat perdamaian dan perjanjian dengan negara lain dengan persetujuan DPR?", "id": "Bab III, Pasal 11 ayat (1)." },
    { "en": "Presiden menyatakan keadaan bahaya?", "id": "Bab III, Pasal 12." },
    { "en": "Presiden memberi grasi dan rehabilitasi dengan memperhatikan pertimbangan Mahkamah Agung?", "id": "Bab III, Pasal 14 ayat (1)." },
    { "en": "Presiden memberi amnesti dan abolisi dengan memperhatikan pertimbangan DPR?", "id": "Bab III, Pasal 14 ayat (2)." },
    { "en": "Pembentukan, pengubahan, dan pembubaran kementerian negara diatur dalam undang-undang?", "id": "Bab V, Pasal 17 ayat (4)." },
    { "en": "DPR memegang kekuasaan membentuk undang-undang?", "id": "Bab VII, Pasal 20 ayat (1)." },
    { "en": "Anggota DPR berhak mengajukan usul rancangan undang-undang?", "id": "Bab VII, Pasal 21." },
    { "en": "Hak DPR untuk mengajukan pertanyaan, menyampaikan usul dan pendapat, serta hak imunitas?", "id": "Bab VII, Pasal 20A ayat (2) & (3)." },
    { "en": "Pemerintah berhak menetapkan peraturan pemerintah pengganti undang-undang (Perpu) dalam kegentingan yang memaksa?", "id": "Bab VII, Pasal 22 ayat (1)." },
    { "en": "Anggota Dewan Perwakilan Daerah (DPD) dipilih dari setiap provinsi melalui pemilihan umum?", "id": "Bab VIIA, Pasal 22C ayat (1)." },
    { "en": "DPD dapat mengajukan RUU tertentu dan ikut membahasnya?", "id": "Bab VIIA, Pasal 22D ayat (1)." },
    { "en": "Pemilihan umum dilaksanakan secara langsung, umum, bebas, rahasia, jujur, dan adil setiap lima tahun sekali?", "id": "Bab VIIB, Pasal 22E ayat (1)." },
    { "en": "Peserta Pemilu untuk memilih anggota DPR dan DPRD adalah partai politik?", "id": "Bab VIIB, Pasal 22E ayat (3)." },
    { "en": "APBN ditetapkan setiap tahun dengan undang-undang dan dilaksanakan secara terbuka dan bertanggung jawab?", "id": "Bab VIII, Pasal 23 ayat (1)." },
    { "en": "Macam dan harga mata uang ditetapkan dengan undang-undang?", "id": "Bab VIII, Pasal 23B." },
    { "en": "Negara memiliki suatu bank sentral yang susunannya diatur dengan undang-undang?", "id": "Bab VIII, Pasal 23D." },
    { "en": "BPK berwenang memeriksa pengelolaan dan tanggung jawab keuangan negara?", "id": "Bab VIIIA, Pasal 23E ayat (1)." },
    { "en": "Mahkamah Agung berwenang mengadili pada tingkat kasasi dan menguji peraturan di bawah undang-undang?", "id": "Bab IX, Pasal 24A ayat (1)." },
    { "en": "Badan peradilan yang berada di bawah Mahkamah Agung?", "id": "Bab IX, Pasal 24A ayat (2)." },
    { "en": "Mahkamah Konstitusi berwenang mengadili pada tingkat pertama dan terakhir untuk menguji UU terhadap UUD?", "id": "Bab IX, Pasal 24C ayat (1)." },
    { "en": "Syarat menjadi hakim agung?", "id": "Bab IX, Pasal 24A ayat (3)." },
    { "en": "Komisi Yudisial bersifat mandiri yang berwenang mengusulkan pengangkatan hakim agung?", "id": "Bab IX, Pasal 24B ayat (1)." },
    { "en": "Pembagian daerah Indonesia atas daerah provinsi, kabupaten, dan kota?", "id": "Bab VI, Pasal 18 ayat (1)." },
    { "en": "Pemerintahan daerah menjalankan otonomi seluas-luasnya, kecuali urusan yang oleh UU ditentukan sebagai urusan Pemerintah Pusat?", "id": "Bab VI, Pasal 18 ayat (5)." },
    { "en": "Syarat-syarat untuk menjadi dan berhenti menjadi warga negara ditetapkan dengan undang-undang?", "id": "Bab X, Pasal 26 ayat (3)." },
    { "en": "Kemerdekaan berserikat dan berkumpul, mengeluarkan pikiran dengan lisan dan tulisan diatur dengan undang-undang?", "id": "Bab X, Pasal 28." },
    { "en": "Setiap orang berhak atas pengakuan, jaminan, perlindungan, dan kepastian hukum yang adil?", "id": "Bab XA, Pasal 28D ayat (1)." },
    { "en": "Setiap orang berhak bebas dari penyiksaan dan berhak memperoleh suaka politik dari negara lain?", "id": "Bab XA, Pasal 28G ayat (2)." },
    { "en": "Identitas budaya dan hak masyarakat tradisional dihormati selaras dengan perkembangan zaman?", "id": "Bab XA, Pasal 28I ayat (3)." },
    { "en": "Negara berdasar atas Ketuhanan Yang Maha Esa?", "id": "Bab XI, Pasal 29 ayat (1)." },
    { "en": "Negara menjamin kemerdekaan tiap-tiap penduduk untuk memeluk agamanya masing-masing?", "id": "Bab XI, Pasal 29 ayat (2)." },
    { "en": "Tiap-tiap warga negara berhak dan wajib ikut serta dalam usaha pertahanan dan keamanan negara?", "id": "Bab XII, Pasal 30 ayat (1)." },
    { "en": "Susunan dan kedudukan TNI, Kepolisian Negara RI, serta hubungan kewenangannya diatur dengan undang-undang?", "id": "Bab XII, Pasal 30 ayat (5)." },
    { "en": "Setiap warga negara berhak mendapat pendidikan?", "id": "Bab XIII, Pasal 31 ayat (1)." },
    { "en": "Pemerintah mengusahakan dan menyelenggarakan satu sistem pendidikan nasional?", "id": "Bab XIII, Pasal 31 ayat (3)." },
    { "en": "Negara memprioritaskan anggaran pendidikan sekurang-kurangnya 20% dari APBN dan APBD?", "id": "Bab XIII, Pasal 31 ayat (4)." },
    { "en": "Negara memajukan kebudayaan nasional Indonesia di tengah peradaban dunia?", "id": "Bab XIII, Pasal 32 ayat (1)." },
    { "en": "Perekonomian disusun sebagai usaha bersama berdasar atas asas kekeluargaan?", "id": "Bab XIV, Pasal 33 ayat (1)." },
    { "en": "Cabang-cabang produksi yang penting bagi negara dan menguasai hajat hidup orang banyak dikuasai oleh negara?", "id": "Bab XIV, Pasal 33 ayat (2)." },
    { "en": "Bumi dan air dan kekayaan alam yang terkandung di dalamnya dikuasai oleh negara dan dipergunakan untuk sebesar-besar kemakmuran rakyat?", "id": "Bab XIV, Pasal 33 ayat (3)." },
    { "en": "Fakir miskin dan anak-anak yang terlantar dipelihara oleh negara?", "id": "Bab XIV, Pasal 34 ayat (1)." },
    { "en": "Negara mengembangkan sistem jaminan sosial bagi seluruh rakyat?", "id": "Bab XIV, Pasal 34 ayat (2)." },
    { "en": "Bendera Negara Indonesia ialah Sang Merah Putih?", "id": "Bab XV, Pasal 35." },
    { "en": "Bahasa Negara ialah Bahasa Indonesia?", "id": "Bab XV, Pasal 36." },
    { "en": "Lambang Negara ialah Garuda Pancasila dengan semboyan Bhinneka Tunggal Ika?", "id": "Bab XV, Pasal 36A." },
    { "en": "Lagu Kebangsaan ialah Indonesia Raya?", "id": "Bab XV, Pasal 36B." },
    { "en": "Usul perubahan pasal-pasal UUD dapat diagendakan jika diajukan oleh sekurang-kurangnya 1/3 dari jumlah anggota MPR?", "id": "Bab XVI, Pasal 37 ayat (1)." },
    { "en": "Bentuk Negara Kesatuan Republik Indonesia tidak dapat dilakukan perubahan?", "id": "Bab XVI, Pasal 37 ayat (5)." },
    { "en": "Mahkamah Konstitusi dibentuk selambat-lambatnya pada 17 Agustus 2003?", "id": "Aturan Peralihan, Pasal III." },
    { "en": "MPR melantik Presiden dan/atau Wakil Presiden?", "id": "Bab II, Pasal 3 ayat (2)." },
    { "en": "Presiden dibantu oleh satu orang Wakil Presiden dalam melakukan kewajibannya?", "id": "Bab III, Pasal 4 ayat (2)." },
    { "en": "Presiden menetapkan peraturan pemerintah untuk menjalankan undang-undang?", "id": "Bab III, Pasal 5 ayat (2)." },
    { "en": "Pengusulan pasangan Calon Presiden dan Wakil Presiden oleh partai politik atau gabungan partai politik peserta pemilu?", "id": "Bab III, Pasal 6A ayat (2)." },
    { "en": "Penggantian Presiden oleh Wakil Presiden jika Presiden mangkat, berhenti, atau diberhentikan?", "id": "Bab III, Pasal 8 ayat (1)." },
    { "en": "Sumpah atau janji Presiden dan Wakil Presiden sebelum memangku jabatan?", "id": "Bab III, Pasal 9 ayat (1)." },
    { "en": "Presiden mengangkat duta dan konsul?", "id": "Bab III, Pasal 13 ayat (1)." },
    { "en": "Presiden menerima penempatan duta negara lain dengan memperhatikan pertimbangan DPR?", "id": "Bab III, Pasal 13 ayat (3)." },
    { "en": "Presiden memberi gelar, tanda jasa, dan lain-lain tanda kehormatan yang diatur dengan undang-undang?", "id": "Bab III, Pasal 15." },
    { "en": "Presiden membentuk suatu dewan pertimbangan yang bertugas memberikan nasihat kepada Presiden?", "id": "Bab III, Pasal 16." },
    { "en": "Setiap RUU dibahas oleh DPR dan Presiden untuk mendapat persetujuan bersama?", "id": "Bab VII, Pasal 20 ayat (2)." },
    { "en": "Jika RUU tidak disahkan bersama, RUU itu tidak boleh diajukan lagi dalam persidangan DPR masa itu?", "id": "Bab VII, Pasal 20 ayat (3)." },
    { "en": "RUU yang telah disetujui bersama menjadi UU meskipun tidak disahkan Presiden dalam 30 hari?", "id": "Bab VII, Pasal 20 ayat (5)." },
    { "en": "Anggota DPR dapat diberhentikan dari jabatannya, yang syarat-syarat dan tata caranya diatur dalam undang-undang?", "id": "Bab VII, Pasal 22B." },
    { "en": "DPD dapat melakukan pengawasan atas pelaksanaan UU tertentu?", "id": "Bab VIIA, Pasal 22D ayat (3)." },
    { "en": "Pajak dan pungutan lain yang bersifat memaksa untuk keperluan negara diatur dengan undang-undang?", "id": "Bab VIII, Pasal 23A." },
    { "en": "Calon hakim agung diusulkan Komisi Yudisial kepada DPR untuk mendapatkan persetujuan?", "id": "Bab IX, Pasal 24A ayat (3)." },
    { "en": "Syarat-syarat untuk menjadi dan diberhentikannya hakim konstitusi?", "id": "Bab IX, Pasal 24C ayat (5)." },
    { "en": "Negara mengakui dan menghormati satuan-satuan pemerintahan daerah yang bersifat khusus atau istimewa?", "id": "Bab VI, Pasal 18B ayat (1)." },
    { "en": "Hak setiap orang untuk membentuk keluarga dan melanjutkan keturunan melalui perkawinan yang sah?", "id": "Bab XA, Pasal 28B ayat (1)." },
    { "en": "Setiap anak berhak atas kelangsungan hidup, tumbuh, dan berkembang serta berhak atas perlindungan dari kekerasan dan diskriminasi?", "id": "Bab XA, Pasal 28B ayat (2)." },
    { "en": "Hak setiap orang untuk bekerja serta mendapat imbalan dan perlakuan yang adil dan layak dalam hubungan kerja?", "id": "Bab XA, Pasal 28D ayat (2)." },
    { "en": "Setiap warga negara berhak memperoleh kesempatan yang sama dalam pemerintahan?", "id": "Bab XA, Pasal 28D ayat (3)." },
    { "en": "Hak atas status kewarganegaraan?", "id": "Bab XA, Pasal 28D ayat (4)." },
    { "en": "Kebebasan memeluk agama dan beribadat, memilih pendidikan, pekerjaan, dan tempat tinggal?", "id": "Bab XA, Pasal 28E ayat (1)." },
    { "en": "Hak setiap orang untuk berkomunikasi dan memperoleh informasi?", "id": "Bab XA, Pasal 28F." },
    { "en": "Hak setiap orang atas jaminan sosial?", "id": "Bab XA, Pasal 28H ayat (3)." },
    { "en": "Perlindungan, pemajuan, penegakan, dan pemenuhan hak asasi manusia adalah tanggung jawab negara, terutama pemerintah?", "id": "Bab XA, Pasal 28I ayat (4)." },
    { "en": "Tentara Nasional Indonesia terdiri atas Angkatan Darat, Angkatan Laut, dan Angkatan Udara?", "id": "Bab XII, Pasal 30 ayat (3)." },
    { "en": "Kepolisian Negara RI sebagai alat negara yang menjaga keamanan dan ketertiban masyarakat?", "id": "Bab XII, Pasal 30 ayat (4)." },
    { "en": "Negara bertanggung jawab atas penyediaan fasilitas pelayanan kesehatan dan fasilitas pelayanan umum yang layak?", "id": "Bab XIV, Pasal 34 ayat (3)." },
    { "en": "Ketentuan lebih lanjut mengenai bendera, bahasa, lambang negara, dan lagu kebangsaan diatur dengan UU?", "id": "Bab XV, Pasal 36C." },
    { "en": "Segala peraturan perundangan yang ada masih tetap berlaku selama belum diadakan yang baru menurut UUD ini?", "id": "Aturan Peralihan, Pasal I." },
    { "en": "MPR bersidang sedikitnya sekali dalam lima tahun di ibu kota negara?", "id": "Bab II, Pasal 2 ayat (2)." },
    { "en": "Segala putusan MPR ditetapkan dengan suara yang terbanyak?", "id": "Bab II, Pasal 2 ayat (3)." },
    { "en": "MPR hanya dapat memberhentikan Presiden dan/atau Wakil Presiden dalam masa jabatannya menurut UUD?", "id": "Bab II, Pasal 3 ayat (3)." },
    { "en": "Presiden dan Wapres dipilih dalam satu pasangan secara langsung oleh rakyat?", "id": "Bab III, Pasal 6A ayat (1)." },
    { "en": "Tata cara pelaksanaan pemilihan Presiden dan Wakil Presiden lebih lanjut diatur dalam undang-undang?", "id": "Bab III, Pasal 6A ayat (5)." },
    { "en": "Permintaan DPR kepada Mahkamah Konstitusi untuk memeriksa dugaan pelanggaran oleh Presiden?", "id": "Bab III, Pasal 7B ayat (2)." },
    { "en": "Pelaksana tugas kepresidenan jika Presiden dan Wapres mangkat atau berhenti bersamaan?", "id": "Bab III, Pasal 8 ayat (3)." },
    { "en": "Pengucapan sumpah atau janji Presiden dan Wapres di hadapan pimpinan MPR?", "id": "Bab III, Pasal 9 ayat (2)." },
    { "en": "Tata cara pembentukan dewan pertimbangan Presiden diatur dalam undang-undang?", "id": "Bab III, Pasal 16." },
    { "en": "Dewan Perwakilan Rakyat bersidang sedikitnya sekali dalam setahun?", "id": "Bab VII, Pasal 19 ayat (3)." },
    { "en": "Hak interpelasi dan hak angket yang dimiliki oleh DPR?", "id": "Bab VII, Pasal 20A ayat (2)." },
    { "en": "Peraturan Pemerintah Pengganti Undang-Undang (Perpu) harus mendapat persetujuan DPR?", "id": "Bab VII, Pasal 22 ayat (2)." },
    { "en": "Anggota DPD berdomisili di daerah pemilihannya dan selama bersidang bertempat tinggal di ibu kota negara?", "id": "Bab VIIA, Pasal 22C ayat (4)." },
    { "en": "Badan Pemeriksa Keuangan (BPK) berkedudukan di ibu kota negara dan memiliki perwakilan di setiap provinsi?", "id": "Bab VIIIA, Pasal 23G ayat (1)." },
    { "en": "Susunan, kedudukan, keanggotaan, dan hukum acara Mahkamah Konstitusi diatur dengan undang-undang?", "id": "Bab IX, Pasal 24C ayat (6)." },
    { "en": "Anggota Komisi Yudisial diangkat dan diberhentikan oleh Presiden dengan persetujuan DPR?", "id": "Bab IX, Pasal 24B ayat (3)." },
    { "en": "Hubungan wewenang antara pemerintah pusat dan pemerintahan daerah diatur dengan memperhatikan kekhususan daerah?", "id": "Bab VI, Pasal 18A ayat (1)." },
    { "en": "Hak untuk memajukan dirinya dalam memperjuangkan haknya secara kolektif?", "id": "Bab XA, Pasal 28C ayat (2)." },
    { "en": "Hak untuk bebas dari perlakuan diskriminatif atas dasar apa pun?", "id": "Bab XA, Pasal 28I ayat (2)." },
    { "en": "Kewajiban setiap orang untuk menghormati hak asasi manusia orang lain?", "id": "Bab XA, Pasal 28J ayat (1)." },
    { "en": "Syarat-syarat dan tata cara pelaksanaan usaha pertahanan dan keamanan negara diatur dengan undang-undang?", "id": "Bab XII, Pasal 30 ayat (2)." },
    { "en": "Kewajiban pemerintah untuk menjalankan sistem pendidikan nasional?", "id": "Bab XIII, Pasal 31 ayat (3)." },
    { "en": "Negara menghormati dan memelihara bahasa daerah sebagai kekayaan budaya nasional?", "id": "Bab XIII, Pasal 32 ayat (2)." },
    { "en": "Ketentuan pelaksanaan perekonomian nasional dan kesejahteraan sosial diatur lebih lanjut dengan undang-undang?", "id": "Bab XIV, Pasal 34 ayat (4)." },
    { "en": "Syarat kehadiran (kuorum) sidang MPR untuk mengubah UUD?", "id": "Bab XVI, Pasal 37 ayat (3)." },
    { "en": "Syarat persetujuan (kuorum) untuk memutuskan perubahan UUD?", "id": "Bab XVI, Pasal 37 ayat (4)." },
    { "en": "Semua lembaga negara yang ada masih tetap berfungsi sepanjang belum diadakan yang baru menurut UUD ini?", "id": "Aturan Peralihan, Pasal II." }





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
            }, 8000);
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
