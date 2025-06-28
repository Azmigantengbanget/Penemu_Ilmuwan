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


  { "en": "Siapa yang merumuskan hukum gravitasi universal?", "id": "Isaac Newton." },
  { "en": "Siapa yang mengembangkan teori relativitas (E=mc²)?", "id": "Albert Einstein." },
  { "en": "Siapa yang dikenal sebagai Bapak Fisika Modern?", "id": "Galileo Galilei." },
  { "en": "Siapa yang menemukan penisilin?", "id": "Alexander Fleming." },
  { "en": "Siapa yang merumuskan Teori Evolusi melalui Seleksi Alam?", "id": "Charles Darwin." },
  { "en": "Siapa yang dikenal sebagai pelopor di bidang radioaktivitas?", "id": "Marie Curie." },
  { "en": "Siapa yang menemukan struktur heliks ganda DNA?", "id": "James Watson dan Francis Crick." },
  { "en": "Siapa yang menemukan bola lampu praktis dan fonograf?", "id": "Thomas Edison." },
  { "en": "Siapa yang mengembangkan sistem arus bolak-balik (AC)?", "id": "Nikola Tesla." },
  { "en": "Siapa yang menciptakan Tabel Periodik Unsur?", "id": "Dmitri Mendeleev." },
  { "en": "Siapa yang dikenal sebagai Bapak Genetika?", "id": "Gregor Mendel." },
  { "en": "Siapa yang menemukan proses pasteurisasi?", "id": "Louis Pasteur." },
  { "en": "Siapa yang mengusulkan model heliosentris tata surya?", "id": "Nicolaus Copernicus." },
  { "en": "Siapa yang merumuskan hukum gerak planet?", "id": "Johannes Kepler." },
  { "en": "Siapa yang menemukan sinar-X?", "id": "Wilhelm Röntgen." },
  { "en": "Siapa yang menemukan telepon?", "id": "Alexander Graham Bell." },
  { "en": "Siapa yang menemukan vaksin cacar pertama?", "id": "Edward Jenner." },
  { "en": "Siapa yang dikenal sebagai Bapak Kimia Modern?", "id": "Antoine Lavoisier." },
  { "en": "Siapa yang merumuskan Teori Atom Modern pertama?", "id": "John Dalton." },
  { "en": "Siapa yang menemukan prinsip daya apung?", "id": "Archimedes." },
  { "en": "Siapa yang mengembangkan mesin cetak?", "id": "Johannes Gutenberg." },
  { "en": "Siapa yang pertama kali terbang dengan pesawat bermesin?", "id": "Wright Bersaudara (Orville dan Wilbur)." },
  { "en": "Siapa yang merumuskan hukum hubungan tekanan dan volume gas?", "id": "Robert Boyle." },
  { "en": "Siapa yang menemukan hubungan antara listrik dan magnetisme?", "id": "Michael Faraday." },
  { "en": "Siapa yang menyatukan listrik, magnet, dan cahaya dalam satu teori?", "id": "James Clerk Maxwell." },
  { "en": "Siapa yang dikenal sebagai Bapak Komputer?", "id": "Charles Babbage." },
  { "en": "Siapa yang dianggap sebagai pemrogram komputer pertama di dunia?", "id": "Ada Lovelace." },
  { "en": "Siapa yang merumuskan prinsip ketidakpastian dalam fisika kuantum?", "id": "Werner Heisenberg." },
  { "en": "Siapa yang mengusulkan model atom dengan elektron di orbit tertentu?", "id": "Niels Bohr." },
  { "en": "Siapa yang merumuskan dasar-dasar teori kuantum?", "id": "Max Planck." },
  { "en": "Siapa yang menemukan vaksin polio?", "id": "Jonas Salk." },
  { "en": "Siapa yang dikenal sebagai Bapak Taksonomi Modern?", "id": "Carl Linnaeus." },
  { "en": "Siapa yang menemukan dinamit?", "id": "Alfred Nobel." },
  { "en": "Siapa yang dikenal sebagai Bapak Geometri?", "id": "Euclid." },
  { "en": "Siapa yang terkenal dengan teorema segitiga siku-siku?", "id": "Pythagoras." },
  { "en": "Siapa yang mengembangkan mesin uap yang efisien?", "id": "James Watt." },
  { "en": "Siapa yang menemukan bahwa alam semesta mengembang?", "id": "Edwin Hubble." },
  { "en": "Siapa yang merumuskan persamaan gelombang dalam mekanika kuantum?", "id": "Erwin Schrödinger." },
  { "en": "Siapa yang menemukan fotografi sinar-X DNA (Foto 51)?", "id": "Rosalind Franklin." },
  { "en": "Siapa yang menemukan Postulat Koch untuk penyakit menular?", "id": "Robert Koch." },
  { "en": "Siapa yang pertama kali melihat mikroorganisme dengan mikroskop?", "id": "Antonie van Leeuwenhoek." },
  { "en": "Siapa yang menemukan efek fotolistrik?", "id": "Albert Einstein." },
  { "en": "Siapa yang merumuskan teori lubang hitam (black hole)?", "id": "Stephen Hawking." },
  { "en": "Siapa yang merumuskan hipotesis jumlah molekul dalam gas?", "id": "Amedeo Avogadro." },
  { "en": "Siapa yang menciptakan skala suhu Celsius?", "id": "Anders Celsius." },
  { "en": "Siapa yang menciptakan skala suhu Fahrenheit?", "id": "Daniel Gabriel Fahrenheit." },
  { "en": "Siapa yang menemukan telegraf dan Kode Morse?", "id": "Samuel Morse." },
  { "en": "Siapa yang dikenal sebagai Bapak Revolusi Hijau?", "id": "Norman Borlaug." },
  { "en": "Siapa yang menemukan radio?", "id": "Guglielmo Marconi." },
  { "en": "Siapa yang menemukan penangkal petir?", "id": "Benjamin Franklin." },
  { "en": "Siapa yang merumuskan konsep arsitektur komputer modern?", "id": "John von Neumann." },
  { "en": "Siapa yang dikenal sebagai Bapak Kecerdasan Buatan?", "id": "Alan Turing." },
  { "en": "Siapa yang menemukan proses sintesis amonia (Haber-Bosch)?", "id": "Fritz Haber dan Carl Bosch." },
  { "en": "Siapa yang merumuskan hukum kekekalan massa?", "id": "Antoine Lavoisier." },
  { "en": "Siapa yang menemukan unsur Radium dan Polonium?", "id": "Marie dan Pierre Curie." },
  { "en": "Siapa yang merumuskan hukum termodinamika?", "id": "Rudolf Clausius dan Lord Kelvin." },
  { "en": "Siapa yang menemukan elektron?", "id": "J.J. Thomson." },
  { "en": "Siapa yang menemukan inti atom (nukleus)?", "id": "Ernest Rutherford." },
  { "en": "Siapa yang menemukan neutron?", "id": "James Chadwick." },
  { "en": "Siapa yang menemukan golongan darah (A, B, O)?", "id": "Karl Landsteiner." },
  { "en": "Siapa yang menemukan insulin?", "id": "Frederick Banting dan Charles Best." },
  { "en": "Siapa yang merumuskan teori permainan (game theory)?", "id": "John Nash." },
  { "en": "Siapa yang merumuskan hukum perbandingan tetap?", "id": "Joseph Proust." },
  { "en": "Siapa yang mempopulerkan ilmu pengetahuan melalui seri 'Cosmos'?", "id": "Carl Sagan." },
  { "en": "Siapa yang menemukan anestesi (eter)?", "id": "William T.G. Morton." },
  { "en": "Siapa yang menemukan struktur benzena?", "id": "August Kekulé." },
  { "en": "Siapa yang dikenal sebagai Bapak Psikoanalisis?", "id": "Sigmund Freud." },
  { "en": "Siapa yang menemukan gelombang elektromagnetik?", "id": "Heinrich Hertz." },
  { "en": "Siapa yang menemukan efek Doppler?", "id": "Christian Doppler." },
  { "en": "Siapa yang merumuskan hukum tentang hambatan listrik?", "id": "Georg Ohm." },
  { "en": "Siapa yang menemukan prinsip induksi elektromagnetik?", "id": "Michael Faraday." },
  { "en": "Siapa yang menemukan efek piezoelektrik?", "id": "Jacques dan Pierre Curie." },
  { "en": "Siapa yang mengembangkan vaksin rabies?", "id": "Louis Pasteur." },
  { "en": "Siapa yang menemukan vitamin?", "id": "Casimir Funk." },
  { "en": "Siapa yang menemukan asam deoksiribonukleat (DNA)?", "id": "Friedrich Miescher." },
  { "en": "Siapa yang menemukan polimer nilon?", "id": "Wallace Carothers." },
  { "en": "Siapa yang menemukan teflon?", "id": "Roy J. Plunkett." },
  { "en": "Siapa yang menemukan plastik pertama (Bakelite)?", "id": "Leo Baekeland." },
  { "en": "Siapa yang merumuskan prinsip relativitas Galilean?", "id": "Galileo Galilei." },
  { "en": "Siapa yang menemukan kromosom?", "id": "Walther Flemming." },
  { "en": "Siapa yang menemukan proses vulkanisasi karet?", "id": "Charles Goodyear." },
  { "en": "Siapa yang menemukan laser?", "id": "Theodore Maiman." },
  { "en": "Siapa yang menemukan transistor?", "id": "John Bardeen, Walter Brattain, William Shockley." },
  { "en": "Siapa yang menemukan sirkuit terpadu (IC)?", "id": "Jack Kilby dan Robert Noyce." },
  { "en": "Siapa yang merumuskan teori Big Bang?", "id": "Georges Lemaître." },
  { "en": "Siapa yang menemukan quasar?", "id": "Maarten Schmidt." },
  { "en": "Siapa yang menemukan pulsar?", "id": "Jocelyn Bell Burnell." },
  { "en": "Siapa yang merumuskan hukum kekekalan energi?", "id": "James Prescott Joule." },
  { "en": "Siapa yang merumuskan hukum dasar elektrostatika?", "id": "Charles-Augustin de Coulomb." },
  { "en": "Siapa yang menemukan bakteri penyebab tuberkulosis?", "id": "Robert Koch." },
  { "en": "Siapa yang menemukan angka nol dalam matematika?", "id": "Matematikawan India (Brahmagupta)." },
  { "en": "Siapa yang mengembangkan kalkulus secara independen dari Newton?", "id": "Gottfried Wilhelm Leibniz." },
  { "en": "Siapa yang menemukan planet Uranus?", "id": "William Herschel." },
  { "en": "Siapa yang menemukan planet Neptunus melalui perhitungan matematis?", "id": "Urbain Le Verrier dan John Couch Adams." },
  { "en": "Siapa yang menemukan Pluto?", "id": "Clyde Tombaugh." },
  { "en": "Siapa yang menemukan sel?", "id": "Robert Hooke." },
  { "en": "Siapa yang merumuskan Teori Sel?", "id": "Theodor Schwann dan Matthias Schleiden." },
  { "en": "Siapa yang menemukan mitokondria?", "id": "Richard Altmann." },
  { "en": "Siapa yang menemukan stetoskop?", "id": "René Laennec." },
  { "en": "Siapa yang mengembangkan teori pengkondisian klasik (classical conditioning)?", "id": "Ivan Pavlov." },
  { "en": "Siapa yang merumuskan teori pengkondisian operan (operant conditioning)?", "id": "B.F. Skinner." },
  { "en": "Siapa yang mengusulkan teori pergeseran benua (continental drift)?", "id": "Alfred Wegener." },
  { "en": "Siapa yang dikenal sebagai Bapak Ilmu Saraf Modern?", "id": "Santiago Ramón y Cajal." },
  { "en": "Siapa yang merumuskan teori struktur kimia (termasuk struktur benzena)?", "id": "August Kekulé." },
  { "en": "Siapa yang dikenal sebagai Bapak Geologi Modern?", "id": "James Hutton." },
  { "en": "Siapa yang menemukan skala untuk mengukur kekuatan gempa?", "id": "Charles Richter." },
  { "en": "Siapa yang menemukan inti dalam Bumi?", "id": "Inge Lehmann." },
  { "en": "Siapa yang menciptakan bahasa pemrograman COBOL dan mempopulerkan istilah 'bug'?", "id": "Grace Hopper." },
  { "en": "Siapa yang menciptakan World Wide Web (WWW)?", "id": "Tim Berners-Lee." },
  { "en": "Siapa yang merumuskan Aljabar Boolean, dasar dari komputasi modern?", "id": "George Boole." },
  { "en": "Siapa yang dikenal karena Teorema Terakhirnya dalam matematika?", "id": "Pierre de Fermat." },
  { "en": "Siapa yang melakukan eksperimen 'oil drop' untuk mengukur muatan elektron?", "id": "Robert Millikan." },
  { "en": "Siapa yang merumuskan teori ikatan kovalen dan struktur Lewis?", "id": "Gilbert N. Lewis." },
  { "en": "Siapa yang pertama kali menyintesis senyawa organik (urea) dari bahan anorganik?", "id": "Friedrich Wöhler." },
  { "en": "Siapa yang dikenal sebagai Bapak Industri Pupuk?", "id": "Justus von Liebig." },
  { "en": "Siapa yang memenangkan Hadiah Nobel untuk struktur protein menggunakan kristalografi sinar-X?", "id": "Dorothy Hodgkin." },
  { "en": "Siapa yang dikenal sebagai Bapak Kimia Polimer?", "id": "Hermann Staudinger." },
  { "en": "Siapa yang pertama kali mengisolasi unsur natrium dan kalium?", "id": "Humphry Davy." },
  { "en": "Siapa yang mengembangkan notasi kimia modern (simbol unsur)?", "id": "Jöns Jacob Berzelius." },
  { "en": "Siapa yang menjelaskan efek rumah kaca pertama kali?", "id": "Svante Arrhenius." },
  { "en": "Siapa yang merumuskan teori tentang asam dan basa berdasarkan ion H+ dan OH-?", "id": "Svante Arrhenius." },
  { "en": "Siapa yang memelopori penelitian primata, terutama simpanse?", "id": "Jane Goodall." },
  { "en": "Siapa yang menulis buku 'Silent Spring' yang memulai gerakan lingkungan modern?", "id": "Rachel Carson." },
  { "en": "Siapa yang pertama kali menemukan virus?", "id": "Dmitri Ivanovsky." },
  { "en": "Siapa yang menciptakan istilah 'antibiotik' dan menemukan streptomisin?", "id": "Selman Waksman." },
  { "en": "Siapa yang mengembangkan penisilin menjadi obat yang dapat digunakan secara massal?", "id": "Howard Florey dan Ernst Chain." },
  { "en": "Siapa yang dikenal sebagai Bapak Ekologi?", "id": "Eugene Odum." },
  { "en": "Siapa yang merumuskan hipotesis Gaia?", "id": "James Lovelock." },
  { "en": "Siapa yang menemukan kuark?", "id": "Murray Gell-Mann dan George Zweig." },
  { "en": "Siapa yang memprediksi keberadaan antimateri?", "id": "Paul Dirac." },
  { "en": "Siapa yang memimpin Proyek Manhattan untuk mengembangkan bom atom?", "id": "J. Robert Oppenheimer." },
  { "en": "Siapa yang membangun reaktor nuklir pertama di dunia?", "id": "Enrico Fermi." },
  { "en": "Siapa yang memberikan penjelasan teoretis untuk fisi nuklir?", "id": "Lise Meitner." },
  { "en": "Siapa yang memprediksi keberadaan Boson Higgs?", "id": "Peter Higgs." },
  { "en": "Siapa yang merumuskan teori gelombang cahaya?", "id": "Christiaan Huygens." },
  { "en": "Siapa yang merumuskan konsep nol mutlak dan skala Kelvin?", "id": "Lord Kelvin (William Thomson)." },
  { "en": "Siapa yang dikenal sebagai Bapak Termodinamika?", "id": "Sadi Carnot." },
  { "en": "Siapa yang menetapkan batas massa untuk bintang katai putih (white dwarf)?", "id": "Subrahmanyan Chandrasekhar." },
  { "en": "Siapa yang menemukan bukti pertama keberadaan materi gelap (dark matter)?", "id": "Vera Rubin." },
  { "en": "Siapa yang merumuskan Teorema Ketidaklengkapan dalam matematika?", "id": "Kurt Gödel." },
  { "en": "Siapa yang memperkenalkan deret Fibonacci ke dunia Barat?", "id": "Leonardo of Pisa (Fibonacci)." },
  { "en": "Siapa yang dikenal sebagai Bapak Psikologi?", "id": "Wilhelm Wundt." },
  { "en": "Siapa yang mengembangkan teori tahap perkembangan kognitif anak?", "id": "Jean Piaget." },
  { "en": "Siapa yang melakukan eksperimen penjara Stanford?", "id": "Philip Zimbardo." },
  { "en": "Siapa yang menemukan hubungan antara kelistrikan dan kontraksi otot katak?", "id": "Luigi Galvani." },
  { "en": "Siapa yang menciptakan baterai listrik pertama (tumpukan volta)?", "id": "Alessandro Volta." },
  { "en": "Siapa yang merumuskan hukum tentang arus dalam rangkaian listrik?", "id": "André-Marie Ampère." },
  { "en": "Siapa yang menemukan radiasi latar belakang gelombang mikro kosmik?", "id": "Arno Penzias dan Robert Wilson." },
  { "en": "Siapa yang memelopori bidang etologi (perilaku hewan)?", "id": "Konrad Lorenz." },
  { "en": "Siapa yang menemukan sirkulasi darah dalam tubuh manusia?", "id": "William Harvey." },
  { "en": "Siapa yang menemukan vitamin C dan hubungannya dengan penyakit kudis?", "id": "James Lind." },
  { "en": "Siapa yang menemukan vitamin A dan B?", "id": "Elmer McCollum." },
  { "en": "Siapa yang menemukan proses fiksasi nitrogen oleh bakteri?", "id": "Martinus Beijerinck." },
  { "en": "Siapa yang pertama kali menggunakan istilah 'ekologi'?", "id": "Ernst Haeckel." },
  { "en": "Siapa yang merumuskan teori endosimbiosis?", "id": "Lynn Margulis." },
  { "en": "Siapa yang menemukan telomerase, enzim yang terkait dengan penuaan?", "id": "Elizabeth Blackburn." },
  { "en": "Siapa yang menemukan interferon?", "id": "Alick Isaacs dan Jean Lindenmann." },
  { "en": "Siapa yang mengembangkan teknik Polymerase Chain Reaction (PCR)?", "id": "Kary Mullis." },
  { "en": "Siapa yang pertama kali mengkloning mamalia (Dolly si domba)?", "id": "Ian Wilmut." },
  { "en": "Siapa yang memimpin Proyek Genom Manusia?", "id": "Francis Collins." },
  { "en": "Siapa yang menemukan prinsip kromatografi?", "id": "Mikhail Tsvet." },
  { "en": "Siapa yang menemukan sakarin, pemanis buatan pertama?", "id": "Constantin Fahlberg." },
  { "en": "Siapa yang menemukan aspartam?", "id": "James M. Schlatter." },
  { "en": "Siapa yang menemukan 'hukum oktaf' dalam unsur-unsur kimia?", "id": "John Newlands." },
  { "en": "Siapa yang merumuskan hukum perbandingan volume?", "id": "Joseph Louis Gay-Lussac." },
  { "en": "Siapa yang melakukan eksperimen 'gold foil' untuk menemukan inti atom?", "id": "Ernest Rutherford." },
  { "en": "Siapa yang menemukan bahwa jalur planet adalah elips, bukan lingkaran?", "id": "Johannes Kepler." },
  { "en": "Siapa yang melakukan pengamatan astronomi paling akurat sebelum teleskop?", "id": "Tycho Brahe." },
  { "en": "Siapa yang menemukan cincin Saturnus?", "id": "Christiaan Huygens." },
  { "en": "Siapa yang menemukan empat bulan terbesar Jupiter?", "id": "Galileo Galilei." },
  { "en": "Siapa yang pertama kali mengusulkan bahwa fosil adalah sisa-sisa makhluk hidup?", "id": "Robert Hooke." },
  { "en": "Siapa yang merumuskan prinsip uniformitarianisme dalam geologi?", "id": "Charles Lyell." },
  { "en": "Siapa yang menemukan sel surya pertama?", "id": "Charles Fritts." },
  { "en": "Siapa yang menciptakan mesin diesel?", "id": "Rudolf Diesel." },
  { "en": "Siapa yang menemukan proses fotografi (daguerreotype)?", "id": "Louis Daguerre." },
  { "en": "Siapa yang menemukan film fotografi fleksibel?", "id": "George Eastman." },
  { "en": "Siapa yang menemukan semen Portland?", "id": "Joseph Aspdin." },
  { "en": "Siapa yang menemukan baja tahan karat (stainless steel)?", "id": "Harry Brearley." },
  { "en": "Siapa yang menemukan velcro?", "id": "George de Mestral." },
  { "en": "Siapa yang menemukan microwave oven?", "id": "Percy Spencer." },
  { "en": "Siapa yang menemukan persamaan matematika untuk fraktal?", "id": "Benoit Mandelbrot." },
  { "en": "Siapa yang merumuskan teori tentang intelijen ganda (multiple intelligences)?", "id": "Howard Gardner." },
  { "en": "Siapa yang mengembangkan terapi kognitif?", "id": "Aaron Beck." },
  { "en": "Siapa yang mengembangkan psikologi humanistik?", "id": "Abraham Maslow dan Carl Rogers." },
  { "en": "Siapa yang merumuskan konsep 'arketi-pe' (archetypes) dan ketidaksadaran kolektif?", "id": "Carl Jung." },
  { "en": "Siapa yang menemukan prinsip kerja transformator?", "id": "Michael Faraday." },
  { "en": "Siapa yang menemukan superkonduktivitas?", "id": "Heike Kamerlingh Onnes." },
  { "en": "Siapa yang menemukan radiasi Hawking dari lubang hitam?", "id": "Stephen Hawking." },
  { "en": "Siapa yang mengusulkan konsep 'demon' untuk termodinamika?", "id": "James Clerk Maxwell." },
  { "en": "Siapa yang menemukan konstanta fundamental 'h' (konstanta Planck)?", "id": "Max Planck." },
  { "en": "Siapa yang menemukan hubungan periode-luminositas bintang Cepheid?", "id": "Henrietta Swan Leavitt." },
  { "en": "Siapa yang pertama kali mengklasifikasikan galaksi berdasarkan bentuknya?", "id": "Edwin Hubble." },
  { "en": "Siapa yang menemukan bahwa sebagian besar massa atom ada di inti?", "id": "Ernest Rutherford." },
  { "en": "Siapa yang mengembangkan model 'puding prem' untuk atom?", "id": "J.J. Thomson." },
  { "en": "Siapa yang menemukan bahwa atom sebagian besar adalah ruang hampa?", "id": "Ernest Rutherford." },
  { "en": "Siapa yang dikenal sebagai Bapak Aljabar?", "id": "Al-Khwarizmi." },
  { "en": "Siapa yang menulis 'The Canon of Medicine', buku teks kedokteran selama berabad-abad?", "id": "Ibn Sina (Avicenna)." },
  { "en": "Siapa yang dianggap sebagai Bapak Optik karena karyanya 'Book of Optics'?", "id": "Ibn al-Haytham (Alhazen)." },
  { "en": "Siapa yang pertama kali secara akurat menghitung keliling Bumi?", "id": "Eratosthenes." },
  { "en": "Siapa yang pertama kali mengusulkan bahwa materi terdiri dari partikel tak terbagi bernama 'atomos'?", "id": "Democritus." },
  { "en": "Siapa yang dikenal sebagai Bapak Kedokteran?", "id": "Hippocrates." },
  { "en": "Siapa yang merumuskan teorema fundamental aljabar dan simetri dalam matematika?", "id": "Emmy Noether." },
  { "en": "Siapa yang menemukan 'gen pelompat' (transposon) dalam genetika?", "id": "Barbara McClintock." },
  { "en": "Siapa yang menemukan faktor pertumbuhan saraf (nerve growth factor)?", "id": "Rita Levi-Montalcini." },
  { "en": "Siapa yang membuktikan bahwa paritas tidak kekal dalam interaksi lemah (Wu experiment)?", "id": "Chien-Shiung Wu." },
  { "en": "Siapa yang menemukan banyak fosil penting dari era Jurassic di Inggris?", "id": "Mary Anning." },
  { "en": "Siapa yang menemukan delapan komet dan merupakan adik dari William Herschel?", "id": "Caroline Herschel." },
  { "en": "Siapa nama di balik konstanta Boltzmann (k) dalam termodinamika?", "id": "Ludwig Boltzmann." },
  { "en": "Siapa nama di balik satuan radioaktivitas 'becquerel'?", "id": "Henri Becquerel." },
  { "en": "Siapa nama di balik Efek Coriolis yang mempengaruhi cuaca dan arus laut?", "id": "Gaspard-Gustave de Coriolis." },
  { "en": "Siapa nama di balik Efek Meissner pada superkonduktor?", "id": "Walther Meissner." },
  { "en": "Siapa nama di balik Efek Peltier dalam pendinginan termoelektrik?", "id": "Jean Charles Athanase Peltier." },
  { "en": "Siapa nama di balik Efek Seebeck dalam pembangkitan termoelektrik?", "id": "Thomas Johann Seebeck." },
  { "en": "Siapa nama di balik Efek Raman dalam hamburan cahaya?", "id": "C. V. Raman." },
  { "en": "Siapa yang menemukan rem pengaman untuk elevator?", "id": "Elisha Otis." },
  { "en": "Siapa yang menciptakan sistem tulisan Braille untuk tunanetra?", "id": "Louis Braille." },
  { "en": "Siapa yang memelopori penelitian ilmiah tentang seksualitas manusia?", "id": "Alfred Kinsey." },
  { "en": "Siapa yang menemukan sistem televisi mekanis pertama?", "id": "John Logie Baird." },
  { "en": "Siapa yang menemukan sistem televisi elektronik pertama?", "id": "Philo Farnsworth." },
  { "en": "Siapa yang dikenal sebagai Bapak Roket Modern?", "id": "Robert Goddard." },
  { "en": "Siapa yang merumuskan persamaan roket dasar?", "id": "Konstantin Tsiolkovsky." },
  { "en": "Siapa yang mengembangkan roket V-2 dan Saturn V untuk misi Apollo?", "id": "Wernher von Braun." },
  { "en": "Siapa yang memenangkan Nobel untuk katalis dalam produksi polimer (Ziegler-Natta)?", "id": "Karl Ziegler dan Giulio Natta." },
  { "en": "Siapa yang mengembangkan skala elektronegativitas?", "id": "Linus Pauling." },
  { "en": "Siapa yang menemukan kompor Bunsen dan spektroskopi?", "id": "Robert Bunsen." },
  { "en": "Siapa yang merumuskan hukum sirkuit listrik (hukum Kirchhoff)?", "id": "Gustav Kirchhoff." },
  { "en": "Siapa yang secara tidak sengaja menemukan bahwa arus listrik menciptakan medan magnet?", "id": "Hans Christian Ørsted." },
  { "en": "Siapa yang mengusulkan teori evolusi tentang pewarisan sifat yang didapat?", "id": "Jean-Baptiste Lamarck." },
  { "en": "Siapa yang memimpin pengembangan pil kontrasepsi oral pertama?", "id": "Gregory Pincus." },
  { "en": "Siapa yang menemukan elektrokardiogram (EKG)?", "id": "Willem Einthoven." },
  { "en": "Siapa yang menemukan elektroensefalogram (EEG) untuk merekam aktivitas otak?", "id": "Hans Berger." },
  { "en": "Siapa yang menemukan organel sel lisosom dan peroksisom?", "id": "Christian de Duve." },
  { "en": "Siapa yang merumuskan hukum gerak ketiga (aksi-reaksi)?", "id": "Isaac Newton." },
  { "en": "Siapa yang melakukan penelitian gorila gunung di Rwanda?", "id": "Dian Fossey." },
  { "en": "Siapa yang menemukan golongan darah Rhesus (faktor Rh)?", "id": "Karl Landsteiner dan Alexander Wiener." },
  { "en": "Siapa yang menemukan vitamin D dan perannya dalam mencegah rakhitis?", "id": "Edward Mellanby." },
  { "en": "Siapa yang menemukan struktur molekul penisilin dan vitamin B12?", "id": "Dorothy Hodgkin." },
  { "en": "Siapa yang menemukan koenzim A?", "id": "Fritz Albert Lipmann." },
  { "en": "Siapa yang menjelaskan siklus asam sitrat (siklus Krebs)?", "id": "Hans Krebs." },
  { "en": "Siapa yang menemukan struktur ATP sintase?", "id": "John E. Walker." },
  { "en": "Siapa yang mengembangkan vaksin untuk antraks?", "id": "Louis Pasteur." },
  { "en": "Siapa yang mengembangkan teknik pewarnaan bakteri (pewarnaan Gram)?", "id": "Hans Christian Gram." },
  { "en": "Siapa yang menemukan prion, protein penyebab penyakit sapi gila?", "id": "Stanley B. Prusiner." },
  { "en": "Siapa yang merumuskan teori 'satu gen, satu enzim'?", "id": "George Beadle dan Edward Tatum." },
  { "en": "Siapa yang menemukan 'Unsur California' dan sembilan unsur transuranik lainnya?", "id": "Glenn T. Seaborg." },
  { "en": "Siapa yang dikenal sebagai Bapak Bom Hidrogen?", "id": "Edward Teller." },
  { "en": "Siapa yang merumuskan mekanika matriks dalam fisika kuantum?", "id": "Werner Heisenberg." },
  { "en": "Siapa yang mengusulkan dualitas gelombang-partikel untuk semua materi?", "id": "Louis de Broglie." },
  { "en": "Siapa yang menemukan efek Compton?", "id": "Arthur Compton." },
  { "en": "Siapa yang mengusulkan prinsip eksklusi dalam atom?", "id": "Wolfgang Pauli." },
  { "en": "Siapa yang menemukan bahwa bakteri dapat mentransfer informasi genetik (transformasi)?", "id": "Frederick Griffith." },
  { "en": "Siapa yang membuktikan bahwa DNA adalah materi genetik?", "id": "Avery, MacLeod, dan McCarty." },
  { "en": "Siapa yang melakukan eksperimen 'blender' untuk mengkonfirmasi DNA sebagai materi genetik?", "id": "Alfred Hershey dan Martha Chase." },
  { "en": "Siapa yang menemukan replikasi DNA semikonservatif?", "id": "Matthew Meselson dan Franklin Stahl." },
  { "en": "Siapa yang menemukan RNA duta (mRNA)?", "id": "Sydney Brenner, François Jacob, Matthew Meselson." },
  { "en": "Siapa yang memecahkan kode genetik?", "id": "Marshall Nirenberg dan Har Gobind Khorana." },
  { "en": "Siapa yang menemukan proses 'RNA splicing'?", "id": "Richard J. Roberts dan Phillip A. Sharp." },
  { "en": "Siapa yang dikenal sebagai Bapak Geografi?", "id": "Eratosthenes." },
  { "en": "Siapa yang menciptakan sistem klasifikasi iklim yang banyak digunakan?", "id": "Wladimir Köppen." },
  { "en": "Siapa yang mengusulkan hipotesis nebula untuk pembentukan tata surya?", "id": "Immanuel Kant dan Pierre-Simon Laplace." },
  { "en": "Siapa yang dikenal sebagai Bapak Anatomi Modern?", "id": "Andreas Vesalius." },
  { "en": "Siapa yang pertama kali secara akurat menggambarkan sistem katup jantung?", "id": "William Harvey." },
  { "en": "Siapa yang menemukan kelompok protein antigen leukosit manusia (HLA)?", "id": "Jean Dausset." },
  { "en": "Siapa yang mengembangkan metode bayi tabung (in vitro fertilization - IVF)?", "id": "Robert Edwards dan Patrick Steptoe." },
  { "en": "Siapa yang mengembangkan kontrasepsi IUD?", "id": "Richard Richter dan Jack Lippes." },
  { "en": "Siapa yang menemukan sel punca (stem cell)?", "id": "Ernest McCulloch dan James Till." },
  { "en": "Siapa yang menemukan proses apoptosis (kematian sel terprogram)?", "id": "Sydney Brenner, H. Robert Horvitz, John E. Sulston." },
  { "en": "Siapa yang menemukan telomer?", "id": "Hermann Joseph Muller." },
  { "en": "Siapa yang merumuskan teori benua super (Pangaea)?", "id": "Alfred Wegener." },
  { "en": "Siapa yang menemukan pemekaran lantai samudra (seafloor spreading)?", "id": "Harry Hess." },
  { "en": "Siapa yang menemukan ventilasi hidrotermal (hydrothermal vents)?", "id": "Robert Ballard." },
  { "en": "Siapa yang menemukan sabuk radiasi Van Allen di sekitar Bumi?", "id": "James Van Allen." },
  { "en": "Siapa yang mengembangkan skala kecerdasan (IQ test) pertama?", "id": "Alfred Binet." },
  { "en": "Siapa yang merumuskan hukum penawaran dan permintaan?", "id": "Adam Smith." },
  { "en": "Siapa yang dikenal sebagai Bapak Ekonomi Modern?", "id": "Adam Smith." },
  { "en": "Siapa yang mengembangkan teori Ekonomi Keynesian?", "id": "John Maynard Keynes." },
  { "en": "Siapa yang menemukan mesin hitung mekanis pertama (Pascaline)?", "id": "Blaise Pascal." },
  { "en": "Siapa yang merumuskan Geometri Non-Euclidean?", "id": "Nikolai Lobachevsky dan János Bolyai." },
  { "en": "Siapa yang menemukan bilangan transendental seperti 'e'?", "id": "Leonhard Euler." },
  { "en": "Siapa yang mengembangkan teori himpunan (set theory)?", "id": "Georg Cantor." },
  { "en": "Siapa yang menemukan konstanta gravitasi G melalui eksperimen torsi?", "id": "Henry Cavendish." },
  { "en": "Siapa yang menemukan difraksi cahaya?", "id": "Francesco Grimaldi." },
  { "en": "Siapa yang melakukan eksperimen celah ganda (double-slit) untuk cahaya?", "id": "Thomas Young." },
  { "en": "Siapa yang menemukan polarisasi cahaya?", "id": "Étienne-Louis Malus." },
  { "en": "Siapa yang merumuskan prinsip ketidakpastian energi-waktu?", "id": "Werner Heisenberg." },
  { "en": "Siapa yang mengembangkan diagram untuk interaksi partikel subatomik?", "id": "Richard Feynman." },
  { "en": "Siapa yang menemukan muon?", "id": "Carl D. Anderson dan Seth Neddermeyer." },
  { "en": "Siapa yang menemukan positron, antipartikel dari elektron?", "id": "Carl D. Anderson." },
  { "en": "Siapa yang merumuskan teori elektrolemah (electroweak)?", "id": "Sheldon Glashow, Abdus Salam, Steven Weinberg." },
  { "en": "Siapa yang menemukan arus listrik dari perubahan medan magnet?", "id": "Michael Faraday." },
  { "en": "Siapa yang merumuskan hukum tentang kalor yang dihasilkan oleh arus listrik?", "id": "James Prescott Joule." },
  { "en": "Siapa yang menemukan bahwa semua sel berasal dari sel yang sudah ada?", "id": "Rudolf Virchow." },
  { "en": "Siapa yang dikenal sebagai Bapak Teori Informasi?", "id": "Claude Shannon." },
  { "en": "Siapa yang dikenal sebagai Bapak Sosiologi?", "id": "Émile Durkheim." },
  { "en": "Siapa yang mengembangkan protokol TCP/IP, fondasi dari Internet?", "id": "Vint Cerf dan Bob Kahn." },
  { "en": "Siapa yang menciptakan bahasa pemrograman C dan sistem operasi UNIX?", "id": "Dennis Ritchie." },
  { "en": "Siapa yang menciptakan Kernel Linux?", "id": "Linus Torvalds." },
  { "en": "Siapa yang menciptakan bahasa pemrograman Python?", "id": "Guido van Rossum." },
  { "en": "Siapa yang mengembangkan metode penanggalan radiokarbon (Carbon-14 dating)?", "id": "Willard Libby." },
  { "en": "Siapa yang menemukan deuterium (hidrogen berat)?", "id": "Harold Urey." },
  { "en": "Siapa yang memelopori metode untuk menentukan urutan asam amino dalam protein?", "id": "Frederick Sanger." },
  { "en": "Siapa yang mengembangkan metode evolusi terarah (directed evolution) untuk enzim?", "id": "Frances Arnold." },
  { "en": "Siapa trio yang memenangkan Nobel untuk pengembangan baterai lithium-ion?", "id": "John Goodenough, M. Stanley Whittingham, Akira Yoshino." },
  { "en": "Siapa yang memelopori penelitian tentang karbokation dalam kimia?", "id": "George A. Olah." },
  { "en": "Siapa yang menemukan enzim 'reverse transcriptase'?", "id": "David Baltimore dan Howard Temin." },
  { "en": "Siapa yang menjelaskan dasar genetik dari keanekaragaman antibodi?", "id": "Susumu Tonegawa." },
  { "en": "Siapa yang menemukan mekanisme molekuler yang mendasari memori?", "id": "Eric Kandel." },
  { "en": "Siapa yang mengembangkan teknik untuk membuat sel punca pluripoten terinduksi (iPS cells)?", "id": "Shinya Yamanaka." },
  { "en": "Siapa yang memenangkan Nobel untuk penemuan mekanisme autofagi?", "id": "Yoshinori Ohsumi." },
  { "en": "Siapa yang memenangkan Nobel untuk pengembangan terapi kanker dengan imunoterapi?", "id": "James Allison dan Tasuku Honjo." },
  { "en": "Siapa duo ilmuwan wanita yang memenangkan Nobel untuk pengembangan gunting genetik CRISPR-Cas9?", "id": "Emmanuelle Charpentier dan Jennifer Doudna." },
  { "en": "Siapa yang memenangkan Nobel untuk penemuan reseptor suhu dan sentuhan?", "id": "David Julius dan Ardem Patapoutian." },
  { "en": "Siapa yang memenangkan Nobel untuk penelitian genom manusia purba (Neanderthal)?", "id": "Svante Pääbo." },
  { "en": "Siapa yang dikenal sebagai Bapak Sosiobiologi?", "id": "Edward O. Wilson." },
  { "en": "Siapa yang mengembangkan bahasa pemrograman Pascal?", "id": "Niklaus Wirth." },
  { "en": "Siapa yang mengembangkan bahasa pemrograman FORTRAN pertama?", "id": "John Backus." },
  { "en": "Siapa yang merancang arsitektur komputer Apple I dan Apple II?", "id": "Steve Wozniak." },
  { "en": "Siapa yang menulis 'The Art of Computer Programming' dan merupakan pionir analisis algoritma?", "id": "Donald Knuth." },
  { "en": "Siapa yang merumuskan teori sosiologi tentang birokrasi dan etika Protestan?", "id": "Max Weber." },
  { "en": "Siapa yang dikenal sebagai Bapak Antropologi Amerika?", "id": "Franz Boas." },
  { "en": "Siapa antropolog budaya yang terkenal karena penelitiannya di Samoa?", "id": "Margaret Mead." },
  { "en": "Siapa yang dikenal sebagai Bapak Linguistik Modern?", "id": "Ferdinand de Saussure." },
  { "en": "Siapa ekonom yang mempopulerkan teori monetarisme?", "id": "Milton Friedman." },
  { "en": "Siapa yang mengusulkan teori kuantum medan (Quantum Field Theory)?", "id": "Paul Dirac." },
  { "en": "Siapa yang mengembangkan teori superfluiditas helium cair?", "id": "Lev Landau." },
  { "en": "Siapa yang menemukan Resonansi Magnetik Nuklir (NMR)?", "id": "Isidor Isaac Rabi." },
  { "en": "Siapa trio yang memenangkan Nobel untuk penemuan gelombang gravitasi?", "id": "Rainer Weiss, Kip Thorne, dan Barry Barish." },
  { "en": "Siapa duo yang memenangkan Nobel untuk penemuan lubang hitam supermasif di pusat galaksi kita?", "id": "Reinhard Genzel dan Andrea Ghez." },
  { "en": "Siapa duo yang menemukan exoplanet pertama yang mengorbit bintang mirip matahari?", "id": "Michel Mayor dan Didier Queloz." },
  { "en": "Siapa yang melakukan sintesis total molekul organik kompleks seperti klorofil dan vitamin B12?", "id": "Robert Burns Woodward." },
  { "en": "Siapa yang mengembangkan reaksi metatesis olefin dalam sintesis organik?", "id": "Yves Chauvin, Robert H. Grubbs, Richard R. Schrock." },
  { "en": "Siapa yang mengembangkan reaksi kopling silang berkatatalis paladium?", "id": "Richard Heck, Ei-ichi Negishi, Akira Suzuki." },
  { "en": "Siapa yang menemukan biokimia penglihatan (siklus visual di retina)?", "id": "George Wald." },
  { "en": "Siapa yang menemukan mekanisme replikasi dan struktur genetik virus?", "id": "Max Delbrück." },
  { "en": "Siapa yang menunjukkan bahwa protein melipat ke bentuk 3D unik berdasarkan urutan asam aminonya?", "id": "Christian Anfinsen." },
  { "en": "Siapa yang menemukan interaksi antara virus tumor dan materi genetik sel?", "id": "Renato Dulbecco." },
  { "en": "Siapa duo yang menemukan regulasi metabolisme kolesterol?", "id": "Michael S. Brown dan Joseph L. Goldstein." },
  { "en": "Siapa yang merumuskan hukum tentang radiasi benda hitam?", "id": "Max Planck." },
  { "en": "Siapa yang menemukan bahwa energi dan massa adalah ekuivalen?", "id": "Albert Einstein." },
  { "en": "Siapa yang menemukan hukum perpindahan panas konduksi?", "id": "Jean-Baptiste Joseph Fourier." },
  { "en": "Siapa yang mengembangkan proses pembuatan aluminium secara komersial?", "id": "Charles Martin Hall dan Paul Héroult." },
  { "en": "Siapa yang menemukan selulosa nitrat (bubuk mesiu tak berasap)?", "id": "Paul Vieille." },
  { "en": "Siapa yang menemukan polietilena?", "id": "Eric Fawcett dan Reginald Gibson." },
  { "en": "Siapa yang menemukan polistirena?", "id": "Eduard Simon." },
  { "en": "Siapa yang menemukan PVC (Polivinil klorida)?", "id": "Eugen Baumann." },
  { "en": "Siapa yang menemukan Kevlar?", "id": "Stephanie Kwolek." },
  { "en": "Siapa yang menemukan Gore-Tex?", "id": "Wilbert L. Gore dan Robert W. Gore." },
  { "en": "Siapa yang menemukan Post-it Notes?", "id": "Arthur Fry dan Spencer Silver." },
  { "en": "Siapa yang mengembangkan skala pH untuk mengukur keasaman?", "id": "S. P. L. Sørensen." },
  { "en": "Siapa yang menemukan unsur Oksigen?", "id": "Carl Wilhelm Scheele dan Joseph Priestley." },
  { "en": "Siapa yang menemukan unsur Hidrogen?", "id": "Henry Cavendish." },
  { "en": "Siapa yang menemukan unsur Nitrogen?", "id": "Daniel Rutherford." },
  { "en": "Siapa yang menemukan gas mulia (argon, neon, kripton, xenon)?", "id": "William Ramsay dan Lord Rayleigh." },
  { "en": "Siapa yang menemukan unsur Fluorin?", "id": "Henri Moissan." },
  { "en": "Siapa yang menemukan unsur Fosfor?", "id": "Hennig Brand." },
  { "en": "Siapa yang menemukan vaksin campak?", "id": "John Franklin Enders." },
  { "en": "Siapa yang mengembangkan tes Pap untuk deteksi dini kanker serviks?", "id": "Georgios Papanikolaou." },
  { "en": "Siapa yang melakukan transplantasi jantung manusia pertama yang berhasil?", "id": "Christiaan Barnard." },
  { "en": "Siapa yang menemukan Helicobacter pylori dan perannya dalam tukak lambung?", "id": "Barry Marshall dan Robin Warren." },
  { "en": "Siapa yang menemukan Virus Imunodefisiensi Manusia (HIV)?", "id": "Luc Montagnier dan Françoise Barré-Sinoussi." },
  { "en": "Siapa yang mengembangkan teori kognitif disonansi?", "id": "Leon Festinger." },
  { "en": "Siapa yang merumuskan hierarki kebutuhan manusia?", "id": "Abraham Maslow." },
  { "en": "Siapa yang melakukan eksperimen kepatuhan terhadap otoritas (eksperimen kejut listrik)?", "id": "Stanley Milgram." },
  { "en": "Siapa yang menemukan efek plasebo dalam studi medis?", "id": "Henry K. Beecher." },
  { "en": "Siapa yang menemukan Hukum Pendinginan Newton?", "id": "Isaac Newton." },
  { "en": "Siapa yang mengembangkan analisis Fourier untuk memecah sinyal?", "id": "Joseph Fourier." },
  { "en": "Siapa yang merumuskan empat persamaan fundamental elektromagnetisme?", "id": "James Clerk Maxwell." },
  { "en": "Siapa yang merumuskan hukum kekekalan momentum?", "id": "John Wallis, Christopher Wren, Christiaan Huygens." },
  { "en": "Siapa yang merumuskan prinsip kerja tuas?", "id": "Archimedes." },
  { "en": "Siapa yang menemukan proses Bessemer untuk produksi baja massal?", "id": "Henry Bessemer." },
  { "en": "Siapa yang menciptakan mesin jahit praktis pertama?", "id": "Isaac Singer." },
  { "en": "Siapa yang menemukan bola lampu pijar yang tahan lama?", "id": "Thomas Edison." },
  { "en": "Siapa yang menemukan teori medan terpadu (unified field theory)?", "id": "Albert Einstein (meskipun tidak berhasil)." },
  { "en": "Siapa yang mempopulerkan konsep 'meme'?", "id": "Richard Dawkins." },
  { "en": "Siapa yang menulis 'On the Origin of Species'?", "id": "Charles Darwin." },
  { "en": "Siapa yang menemukan bahwa bakteri bisa menjadi resisten terhadap antibiotik?", "id": "Alexander Fleming." },
  { "en": "Siapa yang mengembangkan model gelembung sabun untuk mempelajari permukaan minimal?", "id": "Joseph Plateau." },
  { "en": "Siapa yang pertama kali mengamati dan menjelaskan Gerak Brown?", "id": "Robert Brown." },
  { "en": "Siapa yang memberikan penjelasan teoretis untuk Gerak Brown?", "id": "Albert Einstein." },
  { "en": "Siapa yang menemukan tabung sinar katoda?", "id": "William Crookes." },
  { "en": "Siapa yang menemukan elektron dengan menggunakan tabung sinar katoda?", "id": "J.J. Thomson." },
  { "en": "Siapa yang menemukan gelombang radio dari luar angkasa?", "id": "Karl Jansky." },
  { "en": "Siapa yang membangun teleskop radio pertama?", "id": "Grote Reber." },
  { "en": "Siapa yang merumuskan hukum Hubble tentang ekspansi alam semesta?", "id": "Edwin Hubble." },
  { "en": "Siapa yang menemukan kuantum kromodinamika (QCD) untuk gaya nuklir kuat?", "id": "David Gross, Frank Wilczek, David Politzer." },
  { "en": "Siapa yang menemukan partikel W dan Z boson?", "id": "Carlo Rubbia dan Simon van der Meer." },
  { "en": "Siapa yang menemukan quark top?", "id": "Tim eksperimen CDF dan DØ di Fermilab." },
  { "en": "Siapa yang merumuskan Teori String?", "id": "Banyak kontributor (bukan satu orang)." },
  { "en": "Siapa yang pertama kali mengusulkan konsep lubang cacing (wormhole)?", "id": "Ludwig Flamm, Albert Einstein, Nathan Rosen." },
  { "en": "Siapa yang menciptakan istilah 'lubang hitam' (black hole)?", "id": "John Archibald Wheeler." },
  { "en": "Siapa yang menciptakan tetikus komputer (computer mouse)?", "id": "Douglas Engelbart." },
  { "en": "Siapa yang mengembangkan protokol Ethernet untuk jaringan komputer?", "id": "Robert Metcalfe." },
  { "en": "Siapa yang merumuskan konsep 'paradigm shift' dalam sains?", "id": "Thomas Kuhn." },
  { "en": "Siapa filsuf sains yang mengusulkan prinsip falsifiabilitas?", "id": "Karl Popper." },
  { "en": "Siapa yang menemukan mikroskop elektron?", "id": "Ernst Ruska." },
  { "en": "Siapa yang menemukan mikroskop fase kontras?", "id": "Frits Zernike." },
  { "en": "Siapa yang mengembangkan teknik radioimmunoassay (RIA)?", "id": "Rosalyn Yalow." },
  { "en": "Siapa duo yang memenangkan Nobel untuk penemuan CT Scan?", "id": "Godfrey Hounsfield dan Allan Cormack." },
  { "en": "Siapa duo yang memenangkan Nobel untuk pengembangan pencitraan resonansi magnetik (MRI)?", "id": "Paul Lauterbur dan Peter Mansfield." },
  { "en": "Siapa yang dikenal sebagai penemu ginjal buatan dan mesin dialisis?", "id": "Willem Kolff." },
  { "en": "Siapa yang mengembangkan ultracentrifuge untuk memisahkan molekul besar?", "id": "Theodor Svedberg." },
  { "en": "Siapa duo yang menemukan kromatografi partisi?", "id": "Archer Martin dan Richard Synge." },
  { "en": "Siapa yang mengembangkan teknik 'Southern blot' untuk mendeteksi sekuens DNA tertentu?", "id": "Edwin Southern." },
  { "en": "Siapa yang pertama kali menggunakan pelacak radioaktif dalam studi biologi?", "id": "George de Hevesy." },
  { "en": "Siapa matematikawan yang membuktikan Teorema Terakhir Fermat?", "id": "Andrew Wiles." },
  { "en": "Siapa matematikawan otodidak dari India yang terkenal dengan kontribusinya pada teori bilangan?", "id": "Srinivasa Ramanujan." },
  { "en": "Siapa yang menciptakan bahasa pemrograman Lisp dan menciptakan istilah 'Kecerdasan Buatan'?", "id": "John McCarthy." },
  { "en": "Siapa yang merumuskan algoritma untuk menemukan jalur terpendek dalam graf?", "id": "Edsger Dijkstra." },
  { "en": "Siapa yang memenangkan Nobel untuk penentuan struktur ribosom?", "id": "Venkatraman Ramakrishnan, Thomas A. Steitz, dan Ada Yonath." },
  { "en": "Siapa yang mengusulkan teori 'sup purba' tentang asal-usul kehidupan?", "id": "Alexander Oparin." },
  { "en": "Siapa yang melakukan eksperimen Miller-Urey untuk menguji hipotesis asal-usul kehidupan?", "id": "Stanley Miller dan Harold Urey." },
  { "en": "Siapa tokoh kunci dalam sintesis evolusioner modern?", "id": "Ernst Mayr dan Theodosius Dobzhansky." },
  { "en": "Siapa yang memelopori bidang genetika populasi?", "id": "Sewall Wright dan J. B. S. Haldane." },
  { "en": "Siapa duo yang mengembangkan teknik untuk membuat antibodi monoklonal?", "id": "César Milstein dan Georges Köhler." },
  { "en": "Siapa duo yang menemukan interferensi RNA (RNAi)?", "id": "Andrew Fire dan Craig Mello." },
  { "en": "Siapa yang menunjukkan bahwa virus dapat menyebabkan kanker?", "id": "Peyton Rous." },
  { "en": "Siapa yang meneliti 'tarian kibasan' lebah madu untuk komunikasi?", "id": "Karl von Frisch." },
  { "en": "Siapa yang mengembangkan vaksin polio oral?", "id": "Albert Sabin." },
  { "en": "Siapa yang pertama kali mengusulkan konsep 'peluru ajaib' (magic bullet) untuk kemoterapi?", "id": "Paul Ehrlich." },
  { "en": "Siapa yang merupakan pionir dalam sintesis obat-obatan dari tumbuhan, seperti kortison?", "id": "Percy Lavon Julian." },
  { "en": "Siapa yang pertama kali mengkonsepkan reaksi berantai nuklir?", "id": "Leó Szilárd." },
  { "en": "Siapa yang secara eksperimental menemukan fisi nuklir?", "id": "Otto Hahn dan Fritz Strassmann." },
  { "en": "Siapa yang pertama kali mengusulkan ide fisi nuklir setelah menganalisis hasil eksperimen?", "id": "Ida Noddack." },
  { "en": "Siapa yang menunjukkan bahwa nomor atom adalah dasar tabel periodik, bukan massa atom?", "id": "Henry Moseley." },
  { "en": "Siapa yang menemukan difraksi sinar-X oleh kristal?", "id": "Max von Laue." },
  { "en": "Siapa duo ayah dan anak yang merumuskan Hukum Bragg untuk difraksi sinar-X?", "id": "William Henry Bragg dan William Lawrence Bragg." },
  { "en": "Siapa yang memprediksi keberadaan meson untuk menjelaskan gaya nuklir kuat?", "id": "Hideki Yukawa." },
  { "en": "Siapa yang menjelaskan proses nukleosintesis bintang (bagaimana bintang menghasilkan energi)?", "id": "Hans Bethe." },
  { "en": "Siapa yang merupakan pendukung utama teori Big Bang dan memprediksi radiasi latar kosmik?", "id": "George Gamow." },
  { "en": "Siapa yang memelopori metode observasi partisipan dalam antropologi?", "id": "Bronisław Malinowski." },
  { "en": "Siapa yang mengembangkan antropologi struktural?", "id": "Claude Lévi-Strauss." },
  { "en": "Siapa duo yang memenangkan Nobel untuk 'teori prospek' dalam ekonomi perilaku?", "id": "Daniel Kahneman dan Amos Tversky." },
  { "en": "Siapa yang merumuskan Hukum Pendinginan untuk benda panas?", "id": "Isaac Newton." },
  { "en": "Siapa yang merumuskan hukum dasar hidrodinamika (Prinsip Bernoulli)?", "id": "Daniel Bernoulli." },
  { "en": "Siapa yang menemukan Asas Black dalam termodinamika?", "id": "Joseph Black." },
  { "en": "Siapa yang mengembangkan proses sianidasi untuk mengekstraksi emas?", "id": "John Stewart MacArthur." },
  { "en": "Siapa yang menemukan saklar buluh (reed switch)?", "id": "Walter B. Ellwood." },
  { "en": "Siapa yang mengembangkan mesin fotokopi xerografi pertama?", "id": "Chester Carlson." },
  { "en": "Siapa yang menemukan hologram?", "id": "Dennis Gabor." },
  { "en": "Siapa yang menciptakan LED (Light Emitting Diode) praktis pertama?", "id": "Nick Holonyak Jr." },
  { "en": "Siapa yang menemukan sel surya silikon modern?", "id": "Daryl Chapin, Calvin Fuller, dan Gerald Pearson." },
  { "en": "Siapa yang merumuskan Hukum Fick tentang difusi?", "id": "Adolf Fick." },
  { "en": "Siapa yang menemukan osmosis?", "id": "Henri Dutrochet." },
  { "en": "Siapa yang mengembangkan sentrifus?", "id": "Antonin Prandtl." },
  { "en": "Siapa yang pertama kali mengamati pembelahan sel (mitosis)?", "id": "Walther Flemming." },
  { "en": "Siapa yang menemukan lisozim?", "id": "Alexander Fleming." },
  { "en": "Siapa yang mengisolasi hormon adrenalin?", "id": "Jokichi Takamine." },
  { "en": "Siapa yang menemukan neurotransmitter asetilkolin?", "id": "Otto Loewi." },
  { "en": "Siapa yang memenangkan Nobel untuk penelitian tentang tidur dan ritme sirkadian?", "id": "Jeffrey Hall, Michael Rosbash, dan Michael Young." },
  { "en": "Siapa yang menemukan sel dendritik dan perannya dalam sistem kekebalan?", "id": "Ralph M. Steinman." },
  { "en": "Siapa yang menemukan bahwa kerusakan otak pada area tertentu mempengaruhi kemampuan bicara?", "id": "Paul Broca." },
  { "en": "Siapa yang menemukan area otak yang bertanggung jawab untuk pemahaman bahasa?", "id": "Carl Wernicke." },
  { "en": "Siapa yang mengembangkan teori kecerdasan emosional?", "id": "Daniel Goleman." },
  { "en": "Siapa yang mengembangkan teori ikatan (attachment theory) pada anak-anak?", "id": "John Bowlby." },
  { "en": "Siapa yang dikenal karena eksperimen 'Little Albert'?", "id": "John B. Watson." },
  { "en": "Siapa yang merumuskan hukum efek dalam psikologi (law of effect)?", "id": "Edward Thorndike." },
  { "en": "Siapa yang merumuskan bilangan Reynolds dalam dinamika fluida?", "id": "Osborne Reynolds." },
  { "en": "Siapa yang merumuskan bilangan Mach dalam aerodinamika?", "id": "Ernst Mach." },
  { "en": "Siapa yang menemukan magnetisme?", "id": "Bangsa Yunani kuno (mengamati magnetit)." },
  { "en": "Siapa yang menemukan giroskop?", "id": "Léon Foucault." },
  { "en": "Siapa yang menggunakan pendulum untuk menunjukkan rotasi Bumi?", "id": "Léon Foucault." },
  { "en": "Siapa yang merumuskan hukum radiasi benda hitam Stefan-Boltzmann?", "id": "Jožef Stefan dan Ludwig Boltzmann." },
  { "en": "Siapa yang merumuskan hukum perpindahan Wien?", "id": "Wilhelm Wien." },
  { "en": "Siapa yang menemukan efek Zeeman (pemisahan garis spektrum dalam medan magnet)?", "id": "Pieter Zeeman." },
  { "en": "Siapa yang memberikan penjelasan teoretis untuk efek Zeeman?", "id": "Hendrik Lorentz." },
  { "en": "Siapa yang menemukan efek Stark (pemisahan garis spektrum dalam medan listrik)?", "id": "Johannes Stark." },
  { "en": "Siapa yang menemukan hamburan Compton?", "id": "Arthur Compton." },
  { "en": "Siapa yang menemukan hamburan Rayleigh (penyebab langit biru)?", "id": "Lord Rayleigh." },
  { "en": "Siapa yang menemukan struktur fullerene (buckyball)?", "id": "Richard Smalley, Robert Curl, dan Harold Kroto." },
  { "en": "Siapa yang pertama kali mengisolasi grafena (graphene)?", "id": "Andre Geim dan Konstantin Novoselov." },
  { "en": "Siapa yang mengembangkan mikroskop gaya atom (AFM)?", "id": "Gerd Binnig, Calvin Quate, dan Christoph Gerber." },
  { "en": "Siapa yang mengembangkan mikroskop penerowongan payaran (STM)?", "id": "Gerd Binnig dan Heinrich Rohrer." },
  { "en": "Siapa yang menemukan teori BCS untuk superkonduktivitas?", "id": "John Bardeen, Leon Cooper, dan Robert Schrieffer." },
  { "en": "Siapa yang menemukan efek kuantum Hall?", "id": "Klaus von Klitzing." },
  { "en": "Siapa yang mengembangkan sempoa (abacus)?", "id": "Peradaban kuno (Sumeria/Babilonia)." },
  { "en": "Siapa yang menemukan logaritma?", "id": "John Napier." },
  { "en": "Siapa yang menemukan mistar hitung (slide rule)?", "id": "William Oughtred." },
  { "en": "Siapa yang mengembangkan teori chaos?", "id": "Edward Lorenz." },
  { "en": "Siapa yang menemukan konsep entropi dalam teori informasi?", "id": "Claude Shannon." },
  { "en": "Siapa yang merumuskan conjucture Poincaré dalam topologi?", "id": "Henri Poincaré." },
  { "en": "Siapa yang membuktikan conjucture Poincaré?", "id": "Grigori Perelman." },
  { "en": "Siapa yang merumuskan Konjektur Goldbach?", "id": "Christian Goldbach." },
  { "en": "Siapa yang merumuskan Hipotesis Riemann?", "id": "Bernhard Riemann." },
  { "en": "Siapa yang mengembangkan diagram Venn?", "id": "John Venn." },
  { "en": "Siapa yang merumuskan teori probabilitas modern?", "id": "Andrey Kolmogorov." },
  { "en": "Siapa yang menemukan proses Haber-Bosch untuk membuat amonia?", "id": "Fritz Haber dan Carl Bosch." },
  { "en": "Siapa yang menemukan vaksin Human Papillomavirus (HPV)?", "id": "Ian Frazer dan Jian Zhou." },
  { "en": "Siapa yang dikenal sebagai Bapak Epidemiologi Modern karena melacak wabah kolera di London?", "id": "John Snow." },
  { "en": "Siapa yang pertama kali mengadvokasi pentingnya mencuci tangan di rumah sakit untuk mencegah demam nifas?", "id": "Ignaz Semmelweis." },
  { "en": "Siapa yang memelopori penggunaan antiseptik (asam karbol) dalam praktik bedah?", "id": "Joseph Lister." },
  { "en": "Siapa yang menemukan Aparatus Golgi dan mengembangkan teknik pewarnaan perak nitrat?", "id": "Camillo Golgi." },
  { "en": "Siapa yang membuktikan bahwa bintang-bintang sebagian besar terdiri dari hidrogen dan helium?", "id": "Cecilia Payne-Gaposchkin." },
  { "en": "Siapa yang mengembangkan sistem klasifikasi bintang modern (O, B, A, F, G, K, M)?", "id": "Annie Jump Cannon." },
  { "en": "Siapa yang mengusulkan keberadaan Awan Oort, reservoir komet di tepi tata surya?", "id": "Jan Oort." },
  { "en": "Siapa yang pertama kali menggunakan istilah 'materi gelap' (dark matter) berdasarkan pengamatan galaksi?", "id": "Fritz Zwicky." },
  { "en": "Siapa matematikawan muda yang mengembangkan teori Galois dalam aljabar abstrak?", "id": "Évariste Galois." },
  { "en": "Siapa yang mengembangkan konsep grup Abelian dalam matematika?", "id": "Niels Henrik Abel." },
  { "en": "Siapa yang merumuskan kalkulus lambda, sebuah fondasi untuk ilmu komputer teoretis?", "id": "Alonzo Church." },
  { "en": "Siapa yang secara formal mendefinisikan masalah P versus NP dalam teori komputasi?", "id": "Stephen Cook." },
  { "en": "Siapa yang mengembangkan model relasional untuk sistem manajemen database?", "id": "Edgar F. Codd." },
  { "en": "Siapa duo yang menciptakan bahasa pemrograman BASIC?", "id": "John G. Kemeny dan Thomas E. Kurtz." },
  { "en": "Siapa yang dikenal sebagai 'bapak' dari bahasa pemrograman Java?", "id": "James Gosling." },
  { "en": "Siapa yang merumuskan Teorema Bell, yang menguji realitas lokal dalam mekanika kuantum?", "id": "John Stewart Bell." },
  { "en": "Siapa yang memberikan interpretasi probabilistik ('Lahir's rule) untuk fungsi gelombang?", "id": "Max Born." },
  { "en": "Siapa yang pertama kali menciptakan kondensat Bose-Einstein di laboratorium?", "id": "Eric Cornell dan Carl Wieman." },
  { "en": "Siapa duo yang berhasil mendeteksi neutrino untuk pertama kalinya?", "id": "Clyde Cowan dan Frederick Reines." },
  { "en": "Siapa trio yang memenangkan Nobel untuk penemuan neutrino muon?", "id": "Leon Lederman, Melvin Schwartz, dan Jack Steinberger." },
  { "en": "Siapa duo wanita dan pria yang mengembangkan model kulit nuklir (nuclear shell model)?", "id": "Maria Goeppert Mayer dan J. Hans D. Jensen." },
  { "en": "Siapa yang mengembangkan metode analisis retrosintetik dalam sintesis kimia organik?", "id": "Elias James Corey." },
  { "en": "Siapa yang mengembangkan teori orbital molekuler perbatasan (aturan Woodward-Hoffmann)?", "id": "Roald Hoffmann dan Robert B. Woodward." },
  { "en": "Siapa yang mengembangkan penggunaan organoborana sebagai reagen penting dalam sintesis organik?", "id": "Herbert C. Brown." },
  { "en": "Siapa yang menemukan Reaksi Wittig untuk membentuk alkena?", "id": "Georg Wittig." },
  { "en": "Siapa yang memenangkan Nobel untuk studi reaksi kimia pada skala femtosekon (Femtochemistry)?", "id": "Ahmed Zewail." },
  { "en": "Siapa yang memenangkan Nobel untuk karyanya tentang proses kimia di permukaan padat (katalisis)?", "id": "Gerhard Ertl." },
  { "en": "Siapa duo yang memenangkan Nobel untuk pengembangan teori fungsional densitas (DFT)?", "id": "Walter Kohn dan John Pople." },
  { "en": "Siapa yang menggunakan lalat buah (Drosophila) untuk membuktikan peran kromosom dalam pewarisan sifat?", "id": "Thomas Hunt Morgan." },
  { "en": "Siapa yang pertama kali secara ilmiah mengusulkan keberadaan Zaman Es (Ice Ages)?", "id": "Louis Agassiz." },
  { "en": "Siapa yang merumuskan Siklus Milankovitch yang menjelaskan perubahan iklim jangka panjang?", "id": "Milutin Milanković." },
  { "en": "Siapa yang menemukan fosil hominin 'Lucy' (Australopithecus afarensis) di Ethiopia?", "id": "Donald Johanson." },
  { "en": "Siapa duo yang mengusulkan teori 'kesetimbangan bersela' (punctuated equilibrium) dalam evolusi?", "id": "Niles Eldredge dan Stephen Jay Gould." },
  { "en": "Siapa keluarga paleoantropolog yang menemukan banyak fosil penting di Ngarai Olduvai, Afrika?", "id": "Keluarga Leakey (Louis, Mary, Richard)." },
  { "en": "Siapa yang merumuskan teori seleksi klonal (clonal selection theory) untuk menjelaskan cara kerja sistem imun?", "id": "Frank Macfarlane Burnet." },
  { "en": "Siapa yang dihormati sebagai 'Bapak Imunologi Modern'?", "id": "Michael Heidelberger." },
  { "en": "Siapa yang menemukan siklus urea dalam metabolisme?", "id": "Hans Krebs dan Kurt Henseleit." },
  { "en": "Siapa yang pertama kali berhasil mengisolasi enzim DNA polimerase?", "id": "Arthur Kornberg." },
  { "en": "Siapa yang menemukan aquaporin, protein kanal air dalam membran sel?", "id": "Peter Agre." },
  { "en": "Siapa duo yang menemukan saluran ion dalam membran sel?", "id": "Erwin Neher dan Bert Sakmann." },
  { "en": "Siapa yang mengembangkan vaksin BCG untuk melawan tuberkulosis?", "id": "Albert Calmette dan Camille Guérin." },
  { "en": "Siapa yang menemukan proses Kontak untuk produksi massal asam sulfat?", "id": "Peregrine Phillips." },
  { "en": "Siapa yang menemukan unsur Magnesium, Kalsium, Stronsium, dan Barium?", "id": "Humphry Davy." },
  { "en": "Siapa yang pertama kali mengisolasi unsur Iodin?", "id": "Bernard Courtois." },
  { "en": "Siapa yang pertama kali mengisolasi unsur Bromin?", "id": "Antoine Jérôme Balard." },
  { "en": "Siapa yang menemukan unsur Vanadium?", "id": "Andrés Manuel del Río." },
  { "en": "Siapa yang menemukan unsur Litium?", "id": "Johan August Arfwedson." },
  { "en": "Siapa yang menemukan unsur Kadmium?", "id": "Friedrich Stromeyer." },
  { "en": "Siapa yang menemukan unsur Selenium, Silikon, dan Torium?", "id": "Jöns Jacob Berzelius." },
  { "en": "Siapa yang merumuskan hukum kekekalan momentum sudut?", "id": "Émilie du Châtelet." },
  { "en": "Siapa yang merumuskan Persamaan Lagrange dalam mekanika klasik?", "id": "Joseph-Louis Lagrange." },
  { "en": "Siapa yang mengembangkan Transformasi Laplace dalam matematika?", "id": "Pierre-Simon Laplace." },
  { "en": "Siapa yang mengembangkan teori probabilitas modern bersama Pascal?", "id": "Pierre de Fermat." },
  { "en": "Siapa yang menghitung kembalinya komet yang kemudian dinamai Komet Halley?", "id": "Edmond Halley." },
  { "en": "Siapa yang membuat katalog bintang komprehensif pertama pada zaman kuno?", "id": "Hipparchus." },
  { "en": "Siapa yang merumuskan model alam semesta geosentris yang paling berpengaruh (Almagest)?", "id": "Claudius Ptolemy." },
  { "en": "Siapa yang menemukan galvanometer untuk mendeteksi arus listrik kecil?", "id": "Johann Schweigger." },
  { "en": "Siapa yang menemukan tekanan atmosfer dengan eksperimen barometer air raksa?", "id": "Evangelista Torricelli." },
  { "en": "Siapa yang mengembangkan pompa vakum dan melakukan eksperimen bola Magdeburg?", "id": "Otto von Guericke." },
  { "en": "Siapa yang pertama kali menunjukkan bahwa kecepatan cahaya terbatas melalui pengamatan bulan Jupiter?", "id": "Ole Rømer." },
  { "en": "Siapa yang pertama kali melakukan pengukuran kecepatan cahaya yang akurat di Bumi?", "id": "Hippolyte Fizeau." },
  { "en": "Siapa yang menemukan aberasi cahaya bintang, bukti lain dari pergerakan Bumi?", "id": "James Bradley." },
  { "en": "Siapa yang membantah teori flogiston dan memberi nama pada Oksigen dan Hidrogen?", "id": "Antoine Lavoisier." },
  { "en": "Siapa yang menemukan proses elektrolisis air menjadi oksigen dan hidrogen?", "id": "William Nicholson dan Anthony Carlisle." },
  { "en": "Siapa yang menjelaskan Efek Tyndall (penghamburan cahaya oleh koloid)?", "id": "John Tyndall." },
  { "en": "Siapa yang mengembangkan statistik modern, termasuk konsep korelasi dan chi-kuadrat?", "id": "Karl Pearson." },
  { "en": "Siapa yang memelopori penggunaan sidik jari untuk tujuan identifikasi forensik?", "id": "Francis Galton." },
  { "en": "Siapa yang menemukan kromatin, substansi pembentuk kromosom?", "id": "Walther Flemming." },
  { "en": "Siapa yang menciptakan istilah 'hormon' dan menemukan sekretin?", "id": "Ernest Starling." },
  { "en": "Siapa yang menemukan reaksi Diels-Alder dalam kimia organik?", "id": "Otto Diels dan Kurt Alder." },
  { "en": "Siapa yang menemukan reaksi Friedel-Crafts?", "id": "Charles Friedel dan James Crafts." },
  { "en": "Siapa yang mengembangkan Reaksi Grignard menggunakan reagen organomagnesium?", "id": "Victor Grignard." },
  { "en": "Siapa yang pertama kali mengamati dan menjelaskan gerak Brown secara mikroskopis?", "id": "Robert Brown." },
  { "en": "Siapa yang melakukan eksperimen Michelson-Morley yang gagal mendeteksi eter luminiferous?", "id": "Albert A. Michelson dan Edward W. Morley." },
  { "en": "Siapa yang menemukan efek Aharonov-Bohm dalam mekanika kuantum?", "id": "Yakir Aharonov dan David Bohm." },
  { "en": "Siapa yang meramalkan Efek Casimir (gaya tarik antara dua pelat di ruang hampa)?", "id": "Hendrik Casimir." },
  { "en": "Siapa yang menciptakan istilah 'quark' untuk partikel subatomik?", "id": "Murray Gell-Mann." },
  { "en": "Siapa yang mengembangkan oftalmoskop untuk memeriksa bagian dalam mata?", "id": "Hermann von Helmholtz." },
  { "en": "Siapa yang merumuskan teori tentang asal-usul spesies melalui lompatan besar (saltasi)?", "id": "Hugo de Vries." },
  { "en": "Siapa yang mengembangkan Teori Ketergantungan (Dependency Theory) dalam sosiologi?", "id": "Raúl Prebisch dan Theotonio dos Santos." },
  { "en": "Siapa yang menemukan dan menamai hormon dopamin?", "id": "Arvid Carlsson." },
  { "en": "Siapa yang menemukan dan menamai hormon serotonin?", "id": "Irvine Page." },
  { "en": "Siapa yang mengembangkan sekolah psikologi Gestalt?", "id": "Max Wertheimer, Kurt Koffka, Wolfgang Köhler." },
  { "en": "Siapa yang merumuskan hukum Weber-Fechner tentang persepsi stimulus?", "id": "Ernst Heinrich Weber dan Gustav Fechner." },
  { "en": "Siapa duo yang menulis 'Principia Mathematica', sebuah karya monumental dalam logika formal?", "id": "Bertrand Russell dan Alfred North Whitehead." },
  { "en": "Siapa yang pertama kali mengisolasi kodein dan noskapin dari opium?", "id": "Pierre Jean Robiquet." },
  { "en": "Siapa yang menemukan mesin uap atmosferik pertama yang praktis?", "id": "Thomas Newcomen." },
  { "en": "Siapa yang menemukan proses untuk membuat potret 'blueprint' (sianotipe)?", "id": "John Herschel." },
  { "en": "Siapa yang pertama kali menggunakan istilah 'dinosaurus'?", "id": "Richard Owen." },
  { "en": "Siapa yang dianggap sebagai Bapak Paleontologi?", "id": "Georges Cuvier." },
  { "en": "Siapa yang mengembangkan skala untuk mengukur kekerasan mineral?", "id": "Friedrich Mohs." },
  { "en": "Siapa yang menemukan bahwa jalur komet bisa periodik?", "id": "Edmond Halley." },
  { "en": "Siapa yang menemukan asteroid pertama, Ceres?", "id": "Giuseppe Piazzi." },
  { "en": "Siapa yang menemukan proses untuk mempasturisasi susu?", "id": "Louis Pasteur." },
  { "en": "Siapa yang pertama kali membuktikan bahwa mikroba menyebabkan fermentasi?", "id": "Louis Pasteur." },
  { "en": "Siapa yang menemukan bahwa medan magnet dapat mempolarisasi cahaya (Efek Faraday)?", "id": "Michael Faraday." },
  { "en": "Siapa yang merumuskan hukum tentang gaya gerak listrik (GGL) induksi?", "id": "Michael Faraday." },
  { "en": "Siapa yang menemukan efek fotokonduktivitas?", "id": "Willoughby Smith." },
  { "en": "Siapa yang menemukan sel surya pewarna-sensitif (sel Grätzel)?", "id": "Michael Grätzel." },
  { "en": "Siapa yang dikenal karena eksperimen konformitas (Asch conformity experiments)?", "id": "Solomon Asch." },
  { "en": "Siapa yang mengembangkan teori belajar sosial (social learning theory) dan eksperimen boneka Bobo?", "id": "Albert Bandura." },
  { "en": "Siapa yang meneliti 'efek penonton' (bystander effect)?", "id": "John Darley dan Bibb Latané." },
  { "en": "Siapa yang meneliti 'ketidakberdayaan yang dipelajari' (learned helplessness)?", "id": "Martin Seligman." },
  { "en": "Siapa yang mengembangkan teori tahap perkembangan psikososial?", "id": "Erik Erikson." },
  { "en": "Siapa yang mengembangkan teori tahap perkembangan moral?", "id": "Lawrence Kohlberg." },
  { "en": "Siapa yang terkenal karena penelitiannya tentang memori palsu dan efek misinformasi?", "id": "Elizabeth Loftus." },
  { "en": "Siapa yang mengembangkan teori zona perkembangan proksimal (ZPD) dalam psikologi pendidikan?", "id": "Lev Vygotsky." },
  { "en": "Siapa duo yang merancang dan membangun komputer ENIAC?", "id": "John Mauchly dan J. Presper Eckert." },
  { "en": "Siapa yang menemukan hovercraft?", "id": "Christopher Cockerell." },
  { "en": "Siapa yang secara independen menemukan mesin jet?", "id": "Frank Whittle dan Hans von Ohain." },
  { "en": "Siapa yang mempopulerkan kubah geodesik (geodesic dome)?", "id": "Buckminster Fuller." },
  { "en": "Siapa yang menemukan proses untuk menyintesis pewarna ungu sintetis pertama (mauveine)?", "id": "William Henry Perkin." },
  { "en": "Siapa yang menemukan dan mematenkan Bakelite, plastik sintetis pertama?", "id": "Leo Baekeland." },
  { "en": "Siapa yang merumuskan teori induksi embrionik ('organizer effect')?", "id": "Hans Spemann." },
  { "en": "Siapa yang memelopori teknik kultur jaringan (tissue culture) pertama?", "id": "Ross Granville Harrison." },
  { "en": "Siapa duo yang mengusulkan Teori Kromosom Boveri–Sutton tentang pewarisan sifat?", "id": "Theodor Boveri dan Walter Sutton." },
  { "en": "Siapa yang membuat peta genetik pertama untuk sebuah kromosom?", "id": "Alfred Sturtevant." },
  { "en": "Siapa yang membuktikan secara konklusif teori kromosom pewarisan sifat?", "id": "Calvin Bridges." },
  { "en": "Siapa yang memenangkan Nobel untuk karyanya tentang dinamika kimia dan tekanan osmotik?", "id": "Jacobus Henricus van 't Hoff." },
  { "en": "Siapa yang dianggap sebagai salah satu pendiri kimia fisik dan meneliti katalisis?", "id": "Wilhelm Ostwald." },
  { "en": "Siapa yang merumuskan Hukum Ketiga Termodinamika?", "id": "Walther Nernst." },
  { "en": "Siapa yang memenangkan Nobel untuk hubungan timbal balik dalam termodinamika ireversibel?", "id": "Lars Onsager." },
  { "en": "Siapa yang memenangkan Nobel untuk teori struktur disipatif dan sistem kompleks?", "id": "Ilya Prigogine." },
  { "en": "Siapa yang menemukan nukleotida gula dan perannya dalam biokimia karbohidrat?", "id": "Luis Federico Leloir." },
  { "en": "Siapa yang memenangkan Nobel untuk pengembangan analisis konformasi dalam kimia?", "id": "Derek Barton dan Odd Hassel." },
  { "en": "Siapa yang memenangkan Nobel untuk karyanya dalam stereokimia molekul dan reaksi?", "id": "Vladimir Prelog." },
  { "en": "Siapa trio yang mengembangkan teknik 'flash photolysis' untuk mempelajari reaksi cepat?", "id": "George Porter, Ronald Norrish, dan Manfred Eigen." },
  { "en": "Siapa yang memenangkan Nobel untuk teori orbital molekul perbatasan?", "id": "Kenichi Fukui." },
  { "en": "Siapa yang dianggap sebagai pendiri geokimia modern?", "id": "Victor Moritz Goldschmidt." },
  { "en": "Siapa yang mempopulerkan konsep biosfer?", "id": "Vladimir Vernadsky." },
  { "en": "Siapa yang ikut menemukan Aqua-Lung dan mempopulerkan eksplorasi laut?", "id": "Jacques Cousteau." },
  { "en": "Siapa duo yang membuat peta ilmiah pertama dari seluruh dasar samudra?", "id": "Marie Tharp dan Bruce Heezen." },
  { "en": "Siapa ilmuwan yang namanya diabadikan pada Kurva Keeling, yang mengukur CO₂ di atmosfer?", "id": "Charles David Keeling." },
  { "en": "Siapa yang mengembangkan mesin Wankel (mesin rotari)?", "id": "Felix Wankel." },
  { "en": "Siapa yang menciptakan proses pengalengan makanan?", "id": "Nicolas Appert." },
  { "en": "Siapa yang menemukan AC (pendingin udara) modern?", "id": "Willis Carrier." },
  { "en": "Siapa yang mengembangkan proses vulkanisasi untuk karet?", "id": "Charles Goodyear." },
  { "en": "Siapa yang pertama kali mengusulkan model 'lubang kunci' (lock and key) untuk aksi enzim?", "id": "Emil Fischer." },
  { "en": "Siapa yang menemukan vitamin K?", "id": "Henrik Dam." },
  { "en": "Siapa yang menemukan vitamin E?", "id": "Herbert McLean Evans dan Katharine Scott Bishop." },
  { "en": "Siapa yang menemukan vitamin B6?", "id": "Paul György." },
  { "en": "Siapa yang merumuskan hukum kuadrat terbalik (inverse-square law) untuk gravitasi dan elektrostatika?", "id": "Isaac Newton dan Charles-Augustin de Coulomb." },
  { "en": "Siapa yang pertama kali menemukan dan mengisolasi morfin dari opium?", "id": "Friedrich Sertürner." },
  { "en": "Siapa yang pertama kali menyintesis kokain?", "id": "Albert Niemann." },
  { "en": "Siapa yang menemukan kafein?", "id": "Friedlieb Ferdinand Runge." },
  { "en": "Siapa yang menciptakan istilah 'biologi'?", "id": "Jean-Baptiste Lamarck dan Gottfried Reinhold Treviranus." },
  { "en": "Siapa yang menciptakan istilah 'ekosistem'?", "id": "Arthur Tansley." },
  { "en": "Siapa yang menciptakan istilah 'sibernetika' (cybernetics)?", "id": "Norbert Wiener." },
  { "en": "Siapa yang dikenal sebagai Bapak Robotika?", "id": "Joseph Engelberger." },
  { "en": "Siapa yang merumuskan Tiga Hukum Robotika dalam fiksi ilmiah?", "id": "Isaac Asimov." },
  { "en": "Siapa yang menemukan efek Josephson dalam superkonduktivitas?", "id": "Brian Josephson." },
  { "en": "Siapa yang menemukan efek Hall kuantum fraksional?", "id": "Robert B. Laughlin, Horst L. Störmer, dan Daniel C. Tsui." },
  { "en": "Siapa yang mengembangkan teori BCS untuk superkonduktivitas?", "id": "John Bardeen, Leon Cooper, dan Robert Schrieffer." },
  { "en": "Siapa yang mengembangkan teori inflasi kosmik?", "id": "Alan Guth." },
  { "en": "Siapa yang menemukan percepatan ekspansi alam semesta (energi gelap)?", "id": "Saul Perlmutter, Brian P. Schmidt, dan Adam G. Riess." },
  { "en": "Siapa yang menemukan reaksi berantai polimerase (PCR)?", "id": "Kary Mullis." },
  { "en": "Siapa yang mengembangkan metode sekuensing DNA (metode Sanger)?", "id": "Frederick Sanger." },
  { "en": "Siapa yang mengembangkan elektroforesis gel?", "id": "Arne Tiselius." },
  { "en": "Siapa yang menemukan 'Archaea' sebagai domain kehidupan ketiga?", "id": "Carl Woese." },
  { "en": "Siapa yang memelopori penggunaan kemoterapi?", "id": "Sidney Farber." },
  { "en": "Siapa yang menemukan sel T pembantu (helper T cell)?", "id": "Henry N. Claman." },
  { "en": "Siapa yang menemukan kompleks histokompatibilitas utama (MHC)?", "id": "George Snell, Jean Dausset, dan Baruj Benacerraf." },
  { "en": "Siapa yang mengembangkan vaksin untuk demam kuning?", "id": "Max Theiler." },
  { "en": "Siapa yang menemukan bahwa nyamuk menyebarkan demam kuning?", "id": "Walter Reed." },
  { "en": "Siapa yang menemukan bahwa kutu menyebarkan demam tifus?", "id": "Charles Nicolle." },
  { "en": "Siapa yang menemukan bahwa parasit malaria ditularkan oleh nyamuk?", "id": "Ronald Ross." },
  { "en": "Siapa yang menemukan siklus hidup parasit malaria?", "id": "Charles Louis Alphonse Laveran." },
  { "en": "Siapa yang mengembangkan teori atomisme dalam filsafat Yunani kuno?", "id": "Leucippus dan Democritus." },
  { "en": "Siapa yang dianggap sebagai 'filsuf pertama' dan mencari unsur dasar alam?", "id": "Thales dari Miletus." },
  { "en": "Siapa yang menulis 'De re metallica', sebuah risalah awal tentang pertambangan dan metalurgi?", "id": "Georgius Agricola." },
  { "en": "Siapa yang menulis 'The Sceptical Chymist', yang menandai transisi dari alkimia ke kimia?", "id": "Robert Boyle." },
  { "en": "Siapa yang menemukan fosforilasi oksidatif?", "id": "Peter D. Mitchell." },
  { "en": "Siapa yang mengembangkan teori kemiosmotik?", "id": "Peter D. Mitchell." },
  { "en": "Siapa yang menemukan siklus asam glikolat (siklus C2) dalam fotosintesis?", "id": "Nathan Tolbert." },
  { "en": "Siapa yang menguraikan jalur fotosintesis C4?", "id": "Marshall Davidson Hatch dan C. R. Slack." },
  { "en": "Siapa yang pertama kali mengamati dan menamai kloroplas?", "id": "Hugo von Mohl." },
  { "en": "Siapa yang mengembangkan hukum minimum untuk pertumbuhan tanaman?", "id": "Justus von Liebig." },
  { "en": "Siapa yang menciptakan istilah 'gen', 'genotipe', dan 'fenotipe'?", "id": "Wilhelm Johannsen." },
  { "en": "Siapa yang menemukan kembali karya Mendel tentang genetika secara independen?", "id": "Hugo de Vries, Carl Correns, dan Erich von Tschermak." },
  { "en": "Siapa yang menemukan pautan gen (genetic linkage)?", "id": "William Bateson, Edith Rebecca Saunders, dan Reginald Punnett." },
  { "en": "Siapa yang menciptakan diagram Punnett Square?", "id": "Reginald Punnett." },
  { "en": "Siapa yang merumuskan prinsip Hardy-Weinberg dalam genetika populasi?", "id": "G. H. Hardy dan Wilhelm Weinberg." },
  { "en": "Siapa yang mengembangkan model 'Operon' untuk regulasi gen?", "id": "François Jacob dan Jacques Monod." },
  { "en": "Siapa yang mengembangkan teori 'lautan primata' untuk evolusi manusia?", "id": "Alister Hardy." },
  { "en": "Siapa yang mengembangkan Teori Sistem Umum (General Systems Theory)?", "id": "Ludwig von Bertalanffy." },
  { "en": "Siapa yang menemukan efek plasebo dalam uji klinis?", "id": "Henry K. Beecher." },
  { "en": "Siapa yang pertama kali melakukan operasi transplantasi ginjal yang berhasil?", "id": "Joseph Murray." },
  { "en": "Siapa yang melakukan operasi transplantasi paru-paru pertama yang berhasil?", "id": "James Hardy." },
  { "en": "Siapa yang menemukan golongan darah ABO?", "id": "Karl Landsteiner." },
  { "en": "Siapa yang mengembangkan psikologi analitis?", "id": "Carl Jung." },
  { "en": "Siapa yang mengembangkan psikologi individual?", "id": "Alfred Adler." },
  { "en": "Siapa yang menemukan bilangan 'e'?", "id": "Leonhard Euler (meskipun dipelajari oleh Jacob Bernoulli)." },
  { "en": "Siapa yang mengembangkan deret Taylor?", "id": "Brook Taylor." },
  { "en": "Siapa yang pertama kali mengusulkan konsep medan listrik?", "id": "Michael Faraday." },
  { "en": "Siapa yang dianggap sebagai Bapak Fisiologi Modern dan mengusulkan konsep 'milieu intérieur' (lingkungan internal)?", "id": "Claude Bernard." },
  { "en": "Siapa yang menemukan ruang awan (cloud chamber) untuk mendeteksi partikel radiasi?", "id": "Charles Thomson Rees Wilson." },
  { "en": "Siapa yang menemukan ruang gelembung (bubble chamber) untuk melacak jejak partikel?", "id": "Donald A. Glaser." },
  { "en": "Siapa yang menciptakan istilah 'dinosaurus' (Dinosauria)?", "id": "Richard Owen." },
  { "en": "Siapa yang dianggap sebagai Bapak Paleontologi dan mempopulerkan konsep kepunahan?", "id": "Georges Cuvier." },
  { "en": "Siapa ahli hortikultura yang mengembangkan lebih dari 800 varietas tanaman, termasuk kentang Russet Burbank?", "id": "Luther Burbank." },
  { "en": "Siapa ilmuwan pertanian yang memimpin Revolusi Hijau di Meksiko dan menyelamatkan jutaan nyawa dari kelaparan?", "id": "Norman Borlaug." },
  { "en": "Siapa ilmuwan yang melakukan penelitian ekstensif tentang kacang tanah dan ubi jalar untuk memajukan pertanian di Selatan AS?", "id": "George Washington Carver." },
  { "en": "Siapa yang menemukan reaksi sikloadisi termal antara diena dan alkena (Reaksi Diels-Alder)?", "id": "Otto Diels dan Kurt Alder." },
  { "en": "Siapa yang merumuskan teori transisi keadaan (transition state theory) dalam kinetika kimia?", "id": "Henry Eyring." },
  { "en": "Siapa yang menciptakan istilah 'genetika', 'alel', dan 'homozigot'?", "id": "William Bateson." },
  { "en": "Siapa yang mengembangkan metode kloning mamalia pertama melalui transfer inti sel somatik?", "id": "John Gurdon." },
  { "en": "Siapa yang menggunakan C. elegans sebagai organisme model untuk mempelajari perkembangan dan apoptosis?", "id": "Sydney Brenner." },
  { "en": "Siapa yang menemukan faktor pertumbuhan epidermal (EGF)?", "id": "Stanley Cohen." },
  { "en": "Siapa yang merumuskan garis-garis Fraunhofer dalam spektrum matahari?", "id": "Joseph von Fraunhofer." },
  { "en": "Siapa yang menemukan seri Balmer, rumus untuk garis spektrum hidrogen?", "id": "Johann Balmer." },
  { "en": "Siapa yang mengembangkan rumus Rydberg yang menggeneralisasi seri Balmer?", "id": "Johannes Rydberg." },
  { "en": "Siapa yang menyempurnakan model atom Bohr dengan orbit elips dan konstanta struktur halus?", "id": "Arnold Sommerfeld." },
  { "en": "Siapa yang mengembangkan model Debye untuk kapasitas panas benda padat?", "id": "Peter Debye." },
  { "en": "Siapa yang merumuskan algoritma Simplex untuk pemrograman linear?", "id": "George Dantzig." },
  { "en": "Siapa yang menemukan kode Hamming untuk deteksi dan koreksi kesalahan?", "id": "Richard Hamming." },
  { "en": "Siapa duo yang menciptakan bahasa pemrograman berorientasi objek pertama, Simula?", "id": "Ole-Johan Dahl dan Kristen Nygaard." },
  { "en": "Siapa yang mempopulerkan istilah 'pemrograman berorientasi objek' dan mengembangkan Smalltalk?", "id": "Alan Kay." },
  { "en": "Siapa yang merumuskan hipotesis Sapir-Whorf tentang relativitas linguistik?", "id": "Edward Sapir dan Benjamin Lee Whorf." },
  { "en": "Siapa yang merumuskan konsep 'konsumsi yang mencolok' (conspicuous consumption)?", "id": "Thorstein Veblen." },
  { "en": "Siapa sosiolog yang mengembangkan konsep 'habitus' dan 'modal budaya'?", "id": "Pierre Bourdieu." },
  { "en": "Siapa yang mengembangkan teori medan kristal (crystal field theory)?", "id": "Hans Bethe dan John Hasbrouck Van Vleck." },
  { "en": "Siapa yang menemukan prinsip Le Chatelier tentang kesetimbangan kimia?", "id": "Henry Louis Le Châtelier." },
  { "en": "Siapa yang menemukan reaksi Cannizzaro?", "id": "Stanislao Cannizzaro." },
  { "en": "Siapa yang menemukan reaksi Reformatsky?", "id": "Sergey Reformatsky." },
  { "en": "Siapa yang menemukan reaksi Perkin?", "id": "William Henry Perkin." },
  { "en": "Siapa yang merumuskan hukum Raoult tentang tekanan uap larutan?", "id": "François-Marie Raoult." },
  { "en": "Siapa yang menemukan hukum Henry tentang kelarutan gas?", "id": "William Henry." },
  { "en": "Siapa yang mengembangkan proses Solvay untuk memproduksi soda abu?", "id": "Ernest Solvay." },
  { "en": "Siapa yang menciptakan istilah 'protein'?", "id": "Jöns Jacob Berzelius." },
  { "en": "Siapa yang menciptakan istilah 'katalisis'?", "id": "Jöns Jacob Berzelius." },
  { "en": "Siapa yang menciptakan istilah 'polimer'?", "id": "Jöns Jacob Berzelius." },
  { "en": "Siapa yang menemukan klasifikasi bakteri berdasarkan bentuknya (kokus, basil, spirilum)?", "id": "Ferdinand Cohn." },
  { "en": "Siapa yang menemukan proses kemoautotrofi (chemoautotrophy) pada bakteri?", "id": "Sergei Winogradsky." },
  { "en": "Siapa yang menemukan Anopheles sebagai vektor malaria?", "id": "Ronald Ross." },
  { "en": "Siapa yang pertama kali mengamati proses fagositosis?", "id": "Élie Metchnikoff." },
  { "en": "Siapa yang menemukan vitamin B1 (tiamin)?", "id": "Christiaan Eijkman." },
  { "en": "Siapa yang mengembangkan teori sel mast dan perannya dalam alergi?", "id": "Paul Ehrlich." },
  { "en": "Siapa yang menemukan lisosom?", "id": "Christian de Duve." },
  { "en": "Siapa yang pertama kali melakukan kromatografi gas?", "id": "Erika Cremer." },
  { "en": "Siapa yang menemukan supernova tipe Ia sebagai 'lilin standar' untuk mengukur jarak kosmik?", "id": "Tim Supernova Cosmology Project dan High-Z Supernova Search." },
  { "en": "Siapa yang merumuskan Teorema Noether, yang menghubungkan simetri dengan hukum kekekalan?", "id": "Emmy Noether." },
  { "en": "Siapa yang pertama kali mengusulkan keberadaan bintang neutron?", "id": "Walter Baade dan Fritz Zwicky." },
  { "en": "Siapa yang menemukan magnetar?", "id": "Robert Duncan dan Christopher Thompson." },
  { "en": "Siapa yang menciptakan istilah 'atrofi' dan 'hipertrofi'?", "id": "Rudolf Virchow." },
  { "en": "Siapa yang dikenal sebagai Bapak Patologi Modern?", "id": "Rudolf Virchow." },
  { "en": "Siapa yang mengembangkan metode bedah untuk operasi katarak modern?", "id": "Charles Kelman." },
  { "en": "Siapa yang menemukan hormon sekretin, hormon pertama yang ditemukan?", "id": "William Bayliss dan Ernest Starling." },
  { "en": "Siapa yang menemukan hormon gastrin?", "id": "John Sydney Edkins." },
  { "en": "Siapa yang menemukan hormon somatostatin?", "id": "Roger Guillemin dan Andrew Schally." },
  { "en": "Siapa yang pertama kali mengisolasi progesteron?", "id": "George W. Corner dan Willard Myron Allen." },
  { "en": "Siapa yang pertama kali menyintesis progesterteron?", "id": "Adolf Butenandt." },
  { "en": "Siapa yang mengembangkan tes kehamilan pertama menggunakan hormon hCG?", "id": "Selmar Aschheim dan Bernhard Zondek." },
  { "en": "Siapa yang menemukan bahwa penyakit beri-beri disebabkan oleh kekurangan nutrisi?", "id": "Christiaan Eijkman." },
  { "en": "Siapa yang menemukan bahwa penyakit pellagra disebabkan oleh kekurangan niasin?", "id": "Joseph Goldberger." },
  { "en": "Siapa yang menemukan siklus Cori (siklus asam laktat)?", "id": "Gerty dan Carl Cori." },
  { "en": "Siapa yang menemukan proses glikolisis?", "id": "Gustav Embden, Otto Meyerhof, dan Jakub Karol Parnas." },
  { "en": "Siapa yang menemukan jalur pentosa fosfat?", "id": "Frank Dickens." },
  { "en": "Siapa yang menemukan fotosistem I dan II dalam fotosintesis?", "id": "Robert Emerson." },
  { "en": "Siapa yang pertama kali mengamati dan menjelaskan gerak sitoplasma (cyclosis)?", "id": "Giovanni Battista Amici." },
  { "en": "Siapa yang mengembangkan stetoskop binaural (dua telinga)?", "id": "Arthur Leared." },
  { "en": "Siapa yang menemukan laringoskop?", "id": "Manuel García." },
  { "en": "Siapa yang menemukan oftalmoskop?", "id": "Hermann von Helmholtz." },
  { "en": "Siapa yang mengembangkan termometer klinis?", "id": "Thomas Clifford Allbutt." },
  { "en": "Siapa yang menemukan proses sianotipe (blueprint)?", "id": "John Herschel." },
  { "en": "Siapa yang menemukan proses fotografi kalotipe (negatif kertas)?", "id": "William Henry Fox Talbot." },
  { "en": "Siapa yang menciptakan gambar berwarna pertama?", "id": "James Clerk Maxwell." },
  { "en": "Siapa yang mengembangkan fotografi instan (kamera Polaroid)?", "id": "Edwin H. Land." },
  { "en": "Siapa yang mengembangkan teori medan ligan (ligand field theory)?", "id": "Hans Bethe dan John Hasbrouck Van Vleck." },
  { "en": "Siapa yang menemukan efek Auger?", "id": "Pierre Victor Auger." },
  { "en": "Siapa yang menemukan efek Cherenkov?", "id": "Pavel Cherenkov." },
  { "en": "Siapa yang menemukan efek Mössbauer?", "id": "Rudolf Mössbauer." },
  { "en": "Siapa yang mengembangkan teori BCS untuk superkonduktivitas?", "id": "John Bardeen, Leon Cooper, dan Robert Schrieffer." },
  { "en": "Siapa yang merumuskan Skala Pauling untuk elektronegativitas?", "id": "Linus Pauling." },
  { "en": "Siapa yang menemukan hukum difusi gas?", "id": "Thomas Graham." },
  { "en": "Siapa yang merumuskan hukum tekanan parsial?", "id": "John Dalton." },
  { "en": "Siapa yang mengembangkan metode Kjeldahl untuk menentukan kadar nitrogen?", "id": "Johan Kjeldahl." },
  { "en": "Siapa yang menemukan reaksi Cannizzaro?", "id": "Stanislao Cannizzaro." },
  { "en": "Siapa yang menemukan sintesis Fischer-Tropsch untuk menghasilkan bahan bakar cair?", "id": "Franz Fischer dan Hans Tropsch." },
  { "en": "Siapa yang menemukan reaksi Wurtz?", "id": "Charles Adolphe Wurtz." },
  { "en": "Siapa yang mengembangkan sintesis peptida fasa padat?", "id": "Robert Bruce Merrifield." },
  { "en": "Siapa yang mengembangkan tes ELISA (enzyme-linked immunosorbent assay)?", "id": "Eva Engvall dan Peter Perlmann." },
  { "en": "Siapa yang mengembangkan metode Western blot?", "id": "W. Neal Burnette." },
  { "en": "Siapa yang mengembangkan metode Northern blot?", "id": "James Alwine, David Kemp, dan George Stark." },
  { "en": "Siapa yang merumuskan Doktrin Neuron?", "id": "Santiago Ramón y Cajal." },
  { "en": "Siapa yang menemukan sinaps (synapse) dan perannya dalam transmisi saraf?", "id": "Charles Scott Sherrington." },
  { "en": "Siapa yang pertama kali mendemonstrasikan transmisi kimiawi di sinaps?", "id": "Otto Loewi." },
  { "en": "Siapa yang mengembangkan model 'fluid mosaic' untuk membran sel?", "id": "S. Jonathan Singer dan Garth L. Nicolson." },
  { "en": "Siapa yang menemukan endospora bakteri yang tahan panas?", "id": "Ferdinand Cohn." },
  { "en": "Siapa yang menemukan bahwa penyakit kudis disebabkan oleh kekurangan vitamin C?", "id": "James Lind." }



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
