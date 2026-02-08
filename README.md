<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>تطبيق مُؤْمِن - الإصدار الشامل</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&family=Amiri:wght@400;700&display=swap');
        body { font-family: 'Cairo', sans-serif; background-color: #ffffff; margin: 0; overflow-x: hidden; }
        
        .card { border-radius: 25px; padding: 25px; margin-bottom: 15px; display: flex; align-items: center; justify-content: space-between; color: white; cursor: pointer; position: relative; box-shadow: 0 4px 12px rgba(0,0,0,0.06); transition: 0.2s; }
        .card:active { transform: scale(0.97); }
        .card-blue { background-color: #3498db; }
        .card-green { background-color: #2ecc71; }
        .card-orange { background-color: #f39c12; }
        .card-dark { background: linear-gradient(135deg, #2c3e50, #34495e); }
        .card-purple { background-color: #9b59b6; }
        .card-teal { background-color: #1abc9c; }
        
        /* تصميم كرت الصلاة على النبي الجديد */
        .card-prayer { background: linear-gradient(135deg, #d81b60, #ad1457); border-radius: 25px; padding: 20px; margin-bottom: 15px; color: white; display: flex; align-items: center; justify-content: space-between; cursor: pointer; box-shadow: 0 4px 15px rgba(216, 27, 96, 0.3); }

        .bottom-nav { position: fixed; bottom: 15px; left: 50%; transform: translateX(-50%); width: 92%; background: #1a1a1a; border-radius: 40px; display: flex; justify-content: space-around; padding: 15px; z-index: 1000; box-shadow: 0 5px 20px rgba(0,0,0,0.3); }
        .nav-item { color: #555; font-size: 1.4rem; cursor: pointer; transition: 0.3s; }
        .nav-item.active { color: #2ecc71; }

        .screen { display: none; padding: 20px; padding-bottom: 120px; animation: fadeIn 0.4s ease; }
        .screen.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        .zikr-row { background: #ffffff; padding: 25px; border-radius: 20px; margin-bottom: 15px; border: 1px solid #f0f0f0; border-right: 8px solid #2ecc71; position: relative; box-shadow: 0 2px 8px rgba(0,0,0,0.04); cursor: pointer; }
        .count-badge { position: absolute; top: -10px; right: 20px; background: #2ecc71; color: white; padding: 3px 15px; border-radius: 12px; font-size: 0.8rem; font-weight: bold; }
        
        .font-quran { 
            font-family: 'Amiri', serif; 
            font-size: 2.4rem; 
            line-height: 2; 
            text-align: center; 
            direction: rtl; 
            color: #111827;
            font-weight: 700;
            padding: 10px;
        }

        .sura-title-box {
            background: #fef3c7;
            border-bottom: 3px solid #f59e0b;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 15px;
            text-align: center;
            font-size: 1.5rem;
            font-weight: 900;
            color: #92400e;
        }
        
        #qibla-screen { background-color: #0f172a; color: white; min-height: 100vh; margin: -20px; padding: 20px; }
        .qibla-radar-container { position: relative; width: 280px; height: 280px; margin: 30px auto; border-radius: 50%; border: 2px solid rgba(255,255,255,0.1); display: flex; align-items: center; justify-content: center; background: radial-gradient(circle, rgba(30, 41, 59, 0.5) 0%, rgba(15, 23, 42, 1) 70%); }
        .target-arrow { position: absolute; top: -15px; width: 0; height: 0; border-left: 15px solid transparent; border-right: 15px solid transparent; border-bottom: 25px solid #ef4444; z-index: 20; }
        .kaaba-diamond { position: absolute; width: 65px; height: 65px; background: #1e293b; border-radius: 12px; transform: rotate(45deg); display: flex; align-items: center; justify-content: center; transition: all 0.1s ease-out; z-index: 15; }
        .kaaba-diamond.active { background: #22c55e !important; box-shadow: 0 0 35px #22c55e; }
        .kaaba-icon-inner { transform: rotate(-45deg); font-size: 32px; }

        select { width: 100%; padding: 14px; border-radius: 15px; background: #f8fafc; border: 1px solid #e2e8f0; font-family: 'Cairo'; font-weight: 700; margin-bottom: 10px; outline: none; }

        /* ستايل مواقيت الصلاة والوقت */
        .prayer-section { background: #f8fafc; border-radius: 25px; padding: 20px; margin-bottom: 20px; border: 1px solid #e2e8f0; }
        .prayer-time-box { text-align: center; flex: 1; }
        .time-now { font-size: 2.5rem; font-weight: 900; color: #1e293b; line-height: 1; }
        .date-now { font-size: 0.9rem; color: #64748b; font-weight: 600; }
    </style>
</head>
<body>

    <div class="max-w-md mx-auto min-h-screen">
        
        <!-- الرئيسية -->
        <div id="home-screen" class="screen active">
            <div class="flex justify-between items-center mb-6 pt-4">
                <i class="fa-solid fa-bars-staggered text-xl text-gray-700"></i>
                <h1 class="text-xl font-black text-gray-800">مُؤْمِن</h1>
                <i class="fa-solid fa-kaaba text-xl text-emerald-600" onclick="showScreen('qibla-screen')"></i>
            </div>

            <!-- إضافة الوقت والتاريخ ومواقيت الصلاة -->
            <div class="prayer-section shadow-sm">
                <div class="flex justify-between items-center mb-4">
                    <div class="text-right">
                        <div id="clock" class="time-now">00:00</div>
                        <div id="full-date" class="date-now">...</div>
                    </div>
                    <div class="text-left bg-emerald-50 p-2 rounded-xl text-emerald-700 font-bold text-xs" id="hijri-box">
                        جاري التحميل...
                    </div>
                </div>
                <div class="flex justify-between gap-1 overflow-x-auto pb-2" id="prayer-times">
                    <!-- سيتم ملؤها بالجافا سكريبت -->
                </div>
            </div>

            <!-- إضافة الصلاة على النبي ﷺ -->
            <div class="card-prayer" onclick="renderZikrs('tasbeeh', 'ts_3')">
                <div class="text-right">
                    <b class="text-xl block">الصلاة على النبي ﷺ</b>
                    <span class="opacity-80 text-sm">رددها الآن لتنال الشفاعة</span>
                </div>
                <div class="flex flex-col items-center">
                    <i class="fa-solid fa-heart-pulse text-3xl mb-1"></i>
                    <span id="prophet-count" class="bg-white/20 px-3 py-0.5 rounded-full text-xs font-bold">100 مرة</span>
                </div>
            </div>

            <div class="card card-blue" onclick="renderSubMenu('daily')">
                <div class="text-right">
                    <b class="text-xl block">أذكار اليوم والليلة</b>
                    <span class="opacity-80 text-sm">المساء، النوم، الاستيقاظ، الصلاة</span>
                </div>
                <i class="fa-solid fa-sun text-4xl"></i>
            </div>

            <div class="card card-green" onclick="renderSubMenu('prayer')">
                <div class="text-right">
                    <b class="text-xl block">أذكار المسجد والوضوء</b>
                    <span class="opacity-80 text-sm">الركوع، السجود، الأذان</span>
                </div>
                <i class="fa-solid fa-mosque text-4xl"></i>
            </div>

            <div class="card card-orange" onclick="renderSubMenu('life')">
                <div class="text-right">
                    <b class="text-xl block">أدعية ووضوء وسفر</b>
                    <span class="opacity-80 text-sm">الطعام، المنزل، الثياب</span>
                </div>
                <i class="fa-solid fa-heart text-4xl"></i>
            </div>

            <div class="card card-teal" onclick="renderSubMenu('tasbeeh')">
                <div class="text-right">
                    <b class="text-xl block">تسبيح وأذكار عامة</b>
                    <span class="opacity-80 text-sm">صيغ التسبيح والاستغفار</span>
                </div>
                <i class="fa-solid fa-hand-holding-heart text-4xl"></i>
            </div>

            <div class="card card-purple" onclick="showScreen('quran-read-screen')">
                <div class="text-right">
                    <b class="text-xl block">المصحف المكتوب</b>
                    <span class="opacity-80 text-sm">قراءة بخط واضح جداً</span>
                </div>
                <i class="fa-solid fa-book-open text-4xl"></i>
            </div>

            <div class="card card-dark" onclick="showScreen('quran-audio-screen')">
                <div class="text-right">
                    <b class="text-xl block">المكتبة الصوتية</b>
                    <span class="opacity-80 text-sm">جميع القراء والسور</span>
                </div>
                <i class="fa-solid fa-headphones text-4xl"></i>
            </div>
        </div>

        <!-- شاشة قراءة القرآن المطورة -->
        <div id="quran-read-screen" class="screen">
            <div class="flex items-center gap-4 mb-6">
                <i class="fa-solid fa-arrow-right text-xl" onclick="showScreen('home-screen')"></i>
                <h2 class="text-xl font-bold">المصحف الشريف</h2>
            </div>
            
            <div class="bg-slate-100 p-4 rounded-3xl mb-4 shadow-inner">
                <p class="text-xs text-gray-500 mb-2 font-bold px-2">اختر القارئ والسورة:</p>
                <select id="read-reader-select" onchange="updateReadContent()"></select>
                <select id="read-sura-select" onchange="updateReadContent()"></select>
            </div>

            <div class="bg-white p-6 rounded-3xl border-4 border-emerald-50 shadow-xl mb-6 min-h-[500px]">
                <div id="sura-header" class="sura-title-box">سُورَةُ الفَاتِحَة</div>
                <div class="font-quran" id="sura-full-text">
                    بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ
                </div>
            </div>

            <button id="read-play-btn" onclick="toggleReadAudio()" class="w-full bg-emerald-600 text-white py-5 rounded-2xl font-bold shadow-lg flex items-center justify-center gap-3 transition-all hover:bg-emerald-700 active:scale-95">
                <i class="fa-solid fa-play"></i> استماع لهذه السورة
            </button>
        </div>

        <!-- المكتبة الصوتية -->
        <div id="quran-audio-screen" class="screen">
             <div class="flex items-center gap-4 mb-6"><i class="fa-solid fa-arrow-right text-xl" onclick="showScreen('home-screen')"></i><h2 class="text-xl font-bold">المكتبة الصوتية</h2></div>
             <div class="bg-slate-50 p-6 rounded-3xl mb-6">
                 <select id="main-reader-select"></select>
                 <select id="main-sura-select"></select>
                 <button onclick="playMainAudio()" class="w-full bg-emerald-600 text-white py-4 rounded-2xl font-bold text-lg shadow-xl"><i class="fa-solid fa-play-circle ml-2"></i> بدء الاستماع</button>
             </div>
             <div id="player-card" class="hidden bg-emerald-600 p-5 rounded-3xl text-white flex items-center justify-between shadow-2xl animate-pulse">
                 <div class="flex items-center gap-4"><div><p id="p-reader" class="text-xs opacity-80"></p><p id="p-sura" class="text-lg font-black"></p></div></div>
                 <i class="fa-solid fa-circle-stop text-4xl cursor-pointer" onclick="stopAudio()"></i>
             </div>
        </div>

        <div id="sub-menu-screen" class="screen">
            <div class="flex items-center gap-4 mb-8"><i class="fa-solid fa-arrow-right text-xl" onclick="showScreen('home-screen')"></i><h2 id="sub-title" class="text-xl font-bold"></h2></div>
            <div id="sub-container" class="space-y-4"></div>
        </div>

        <div id="zikr-list-screen" class="screen">
            <div class="flex items-center justify-between mb-8"><div class="flex items-center gap-4"><i class="fa-solid fa-arrow-right text-xl" onclick="showScreen('sub-menu-screen')"></i><h2 id="zikr-title" class="text-xl font-bold"></h2></div><i class="fa-solid fa-rotate-right text-gray-400" onclick="resetZikrs()"></i></div>
            <div id="zikr-items-container"></div>
        </div>

        <div id="qibla-screen" class="screen text-center"><div class="flex items-center mb-6 gap-4"><i class="fa-solid fa-arrow-right text-xl text-gray-400" onclick="showScreen('home-screen')"></i><h2 class="text-xl font-bold text-white">بوصلة القبلة</h2></div><div class="qibla-radar-container"><div class="target-arrow"></div><div id="kaaba-radar-item" class="kaaba-diamond"><div class="kaaba-icon-inner">🕋</div></div></div><div class="text-6xl font-black text-white" id="degree-val">0°</div><div class="status-msg text-emerald-400 mt-4 font-bold" id="status-text">جاري المعايرة...</div></div>

        <!-- التنقل -->
        <div class="bottom-nav">
            <div class="nav-item" onclick="showScreen('qibla-screen')"><i class="fa-solid fa-kaaba"></i></div>
            <div class="nav-item" onclick="showScreen('quran-audio-screen')"><i class="fa-solid fa-headphones"></i></div>
            <div class="nav-item active" id="nav-home" onclick="showScreen('home-screen')"><i class="fa-solid fa-house"></i></div>
            <div class="nav-item" onclick="showScreen('quran-read-screen')"><i class="fa-solid fa-book-open"></i></div>
            <div class="nav-item" onclick="renderSubMenu('daily')"><i class="fa-solid fa-fingerprint"></i></div>
        </div>
    </div>

    <script>
        const allReaders = [
            { id: "minsh", name: "محمد صديق المنشاوي", s: "https://server10.mp3quran.net/minsh" },
            { id: "husr", name: "محمود خليل الحصري", s: "https://server13.mp3quran.net/husr" },
            { id: "basit", name: "عبد الباسط عبد الصمد", s: "https://server7.mp3quran.net/basit" },
            { id: "afs", name: "مشاري العفاسي", s: "https://server8.mp3quran.net/afs" },
            { id: "maher", name: "ماهر المعيقلي", s: "https://server12.mp3quran.net/maher" },
            { id: "shur", name: "سعود الشريم", s: "https://server7.mp3quran.net/shur" },
            { id: "ajm", name: "أحمد العجمي", s: "https://server10.mp3quran.net/ajm" },
            { id: "abkr", name: "إدريس أبكر", s: "https://server6.mp3quran.net/abkr" },
            { id: "yasser", name: "ياسر الدوسري", s: "https://server11.mp3quran.net/yasser" }
        ];

        const allSuras = ["الفاتحة","البقرة","آل عمران","النساء","المائدة","الأنعام","الأعراف","الأنفال","التوبة","يونس","هود","يوسف","الرعد","إبراهيم","الحجر","النحل","الإسراء","الكهف","مريم","طه","الأنبياء","الحج","المؤمنون","النور","الفرقان","الشعراء","النمل","القصص","العنكبوت","الروم","لقمان","السجدة","الأحزاب","سبأ","فاطر","يس","الصافات","ص","الزمر","غافر","فصلت","الشورى","الزخرف","الدخان","الجاثية","الأحقاف","محمد","الفتح","الحجرات","ق","الذاريات","الطور","النجم","القمر","الرحمن","الواقعة","الحديد","المجادلة","الحشر","الممتحنة","الصف","الجمعة","المنافقون","التغابن","الطلاق","التحريم","الملك","القلم","الحاقة","المعارج","نوح","الجن","المزمل","المدثر","القيامة","الإنسان","المرسلات","النبأ","النازعات","عبس","التكوير","الانفطار","المطففين","الانشقاق","البروج","الطارق","الأعلى","الغاشية","الفجر","البلد","الشمس","الليل","الضحى","الشرح","التين","العلق","القدر","البينة","الزلزلة","العاديات","القارعة","التكاثر","العصر","الهمزة","الفيل","قريش","الماعون","الكوثر","الكافرون","النصر","المسد","الإخلاص","الفلق","الناس"];

        const database = {
            daily: {
                title: "أذكار اليوم والليلة",
                items: [
                    { id: 'm_ev', n: "أذكار المساء (كاملاً)", texts: [
                        {t: "أَمْسَيْنَا وَأَمْسَى المُلْكُ لِلَّهِ، وَالْحَمْدُ لِلَّهِ لا إِلَهَ إِلا اللَّهُ، وَحْدَهُ لا شَرِيكَ لَهُ.", c: 1},
                        {t: "اللَّهُمَّ بِكَ أَمْسَيْنَا، وَبِكَ أَصْبَحْنَا، وَبِكَ نَحْيَا، وَبِكَ نَمُوتُ، وَإِلَيْكَ المَصِيرُ.", c: 1},
                        {t: "اللَّهُمَّ أَنْتَ رَبِّي لا إِلَهَ إِلا أَنْتَ، خَلَقْتَنِي وَأَنَا عَبْدُكَ، وَأَنَا عَلَى عَهْدِكَ وَوَعْدِكَ مَا اسْتَطَعْتُ.", c: 1},
                        {t: "بِسْمِ اللَّهِ الَّذِي لا يَضُرُّ مَعَ اسْمِهِ شَيْءٌ فِي الأَرْضِ وَلا فِي السَّمَاءِ وَهُوَ السَّمِيعُ الْعَلِيمُ.", c: 3},
                        {t: "أَعُوذُ بِكَلِمَاتِ اللَّهِ التَّامَّاتِ مِنْ شَرِّ مَا خَلَقَ.", c: 3},
                        {t: "رَضِيتُ بِاللَّهِ رَبًّا، وَبِالإِسْلامِ دِينًا، وَبِمُحَمَّدٍ ﷺ نَبِيًّا.", c: 3},
                        {t: "يَا حَيُّ يَا قَيُّومُ بِرَحْمَتِكَ أَسْتَغِيثُ أَصْلِحْ لِي شأْنِي كُلَّهُ وَلا تَكِلْنِي إِلَى نَفْسِي طَرْفَةَ عَيْنٍ.", c: 1},
                        {t: "آية الكرسي: اللَّهُ لَا إِلَهَ إِلَّا هُوَ الْحَيُّ الْقَيُّومُ...", c: 1},
                        {t: "سورة الإخلاص والمعوذتين.", c: 3}
                    ] },
                    { id: 'n_sl', n: "أذكار قبل النوم (كاملاً)", texts: [
                        {t: "بِاسْمِكَ رَبِّي وَضَعْتُ جَنْبِي، وَبِكَ أَرْفَعُهُ، فَإِنْ أَمْسَكْتَ نَفْسِي فَارْحَمْهَا.", c: 1},
                        {t: "اللَّهُمَّ خَلَقْتَ نَفْسِي وَأَنْتَ تَوَفَّاهَا، لَكَ مَمَاتُهَا وَمحيَاهَا.", c: 1},
                        {t: "اللَّهُمَّ قِنِي عَذَابَكَ يَوْمَ تَبْعَثُ عِبَادَكَ.", c: 3},
                        {t: "بِاسْمِكَ اللَّهُمَّ أَمُوتُ وَأَحْيَا.", c: 1},
                        {t: "سُبْحَانَ اللَّهِ (33) ، الْحَمْدُ لِلَّهِ (33) ، اللَّهُ أَكْبَرُ (34).", c: 1}
                    ] },
                    { id: 'u_sl', n: "أذكار الاستيقاظ (كاملاً)", texts: [
                        {t: "الْحَمْدُ لِلَّهِ الَّذِي أَحْيَانَا بَعْدَ مَا أَمَاتَنَا وَإِلَيْهِ النُّشُورُ.", c: 1},
                        {t: "الْحَمْدُ لِلَّهِ الَّذِي عَافَانِي فِي جَسَدِي، وَرَدَّ عَلَيَّ رُوحِي.", c: 1}
                    ] }
                ]
            },
            tasbeeh: {
                title: "تسبيح وأذكار عامة",
                items: [
                    { id: 'ts_1', n: "صيغ التسبيح", texts: [
                        {t: "سُبْحَانَ اللَّهِ وَبِحَمْدِهِ.", c: 100},
                        {t: "سُبْحَانَ اللَّهِ الْعَظِيمِ وَبِحَمْدِهِ.", c: 100},
                        {t: "سُبْحَانَ اللَّهِ، وَالْحَمْدُ لِلَّهِ، وَلا إِلَهَ إِلا اللَّهُ، وَاللَّهُ أَكْبَرُ.", c: 33},
                        {t: "لا حَوْلَ وَلا قُوَّةَ إِلا بِاللَّهِ.", c: 100}
                    ] },
                    { id: 'ts_2', n: "الاستغفار", texts: [
                        {t: "أَسْتَغْفِرُ اللَّهَ وَأَتُوبُ إِلَيْهِ.", c: 100},
                        {t: "اللَّهُمَّ أَنْتَ رَبِّي لا إِلَهَ إِلا أنتَ، خَلَقْتَنِي وَأَنَا عَبْدُكَ.", c: 1}
                    ] },
                    { id: 'ts_3', n: "الصلاة على النبي", texts: [
                        {t: "اللَّهُمَّ صَلِّ وَسَلِّمْ عَلَى نَبِيِّنَا مُحَمَّدٍ.", c: 100}
                    ] }
                ]
            },
            prayer: {
                title: "أذكار المسجد والوضوء",
                items: [
                    { id: 'r_k', n: "الدعاء عند الركوع", texts: [
                        {t: "سُبْحَانَ رَبِّيَ الْعَظِيمِ.", c: 3},
                        {t: "سُبْحَانَكَ اللَّهُمَّ رَبَّنَا وَبِحَمْدِكَ اللَّهُمَّ اغْفِرْ لِي.", c: 1}
                    ] },
                    { id: 's_j', n: "الدعاء عند السجود", texts: [
                        {t: "سُبْحَانَ رَبِّيَ الأَعْلَى.", c: 3},
                        {t: "اللَّهُمَّ لَكَ سَجَدْتُ، وَبِكَ آمَنْتُ، وَلَكَ أَسْلَمْتُ.", c: 1}
                    ] }
                ]
            },
            life: {
                title: "أدعية ووضوء وسفر",
                items: [
                    { id: 'a_w', n: "الذكر بعد الوضوء", texts: [
                        {t: "أَشْهَدُ أَنْ لا إِلَهَ إِلا اللَّهُ وَحْدَهُ لا شَرِيكَ لَهُ.", c: 1}
                    ] },
                    { id: 'travel', n: "دعاء السفر", texts: [
                        {t: "سُبْحَانَ الَّذِي سَخَّرَ لَنَا هَذَا وَمَا كُنَّا لَهُ مُقْرِنِينَ.", c: 1}
                    ] }
                ]
            }
        };

        let audio = new Audio();
        let isPlaying = false;

        function init() {
            const rOpts = allReaders.map(r => `<option value="${r.id}">${r.name}</option>`).join('');
            const sOpts = allSuras.map((s, i) => `<option value="${String(i+1).padStart(3, '0')}">${s}</option>`).join('');
            document.getElementById('read-reader-select').innerHTML = rOpts;
            document.getElementById('read-sura-select').innerHTML = sOpts;
            document.getElementById('main-reader-select').innerHTML = rOpts;
            document.getElementById('main-sura-select').innerHTML = sOpts;
            
            updateReadContent();
            startTime();
            updatePrayerTimes();
        }

        // تحديث الوقت والتاريخ
        function startTime() {
            const today = new Date();
            const h = today.getHours().toString().padStart(2, '0');
            const m = today.getMinutes().toString().padStart(2, '0');
            document.getElementById('clock').innerHTML = h + ":" + m;
            
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('full-date').innerText = today.toLocaleDateString('ar-EG', options);
            
            setTimeout(startTime, 10000);
        }

        // جلب مواقيت الصلاة (محاكاة أو API)
        async function updatePrayerTimes() {
            try {
                // استخدام API لجلب المواقيت بناءً على الموقع (افتراضياً القاهرة)
                const res = await fetch('https://api.aladhan.com/v1/timingsByCity?city=Cairo&country=Egypt&method=5');
                const data = await res.json();
                const timings = data.data.timings;
                const hijri = data.data.date.hijri;

                const prayerNames = {
                    Fajr: 'الفجر',
                    Dhuhr: 'الظهر',
                    Asr: 'العصر',
                    Maghrib: 'المغرب',
                    Isha: 'العشاء'
                };

                let html = '';
                for (let key in prayerNames) {
                    html += `
                        <div class="prayer-time-box">
                            <div class="text-[10px] text-slate-400 font-bold">${prayerNames[key]}</div>
                            <div class="text-xs font-black text-slate-700">${timings[key]}</div>
                        </div>
                    `;
                }
                document.getElementById('prayer-times').innerHTML = html;
                document.getElementById('hijri-box').innerText = `${hijri.day} ${hijri.month.ar} ${hijri.year}`;

            } catch (error) {
                document.getElementById('prayer-times').innerHTML = "<span class='text-xs text-red-400'>تعذر تحميل المواقيت</span>";
            }
        }

        function showScreen(id) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            if(id === 'home-screen') document.getElementById('nav-home').classList.add('active');
            if(id === 'qibla-screen') initRadar();
        }

        function updateReadContent() {
            const sid = document.getElementById('read-sura-select').value;
            const rid = document.getElementById('read-reader-select').value;
            const r = allReaders.find(x => x.id === rid);
            audio.src = `${r.s}/${sid}.mp3`;
            const sName = allSuras[parseInt(sid)-1];
            
            document.getElementById('sura-header').innerText = `سُورَةُ ${sName}`;
            
            const quranSample = [
                "بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ",
                "الْحَمْدُ لِلَّهِ رَبِّ الْعَالَمِينَ (1) الرَّحْمَنِ الرَّحِيمِ (2) مَالِكِ يَوْمِ الدِّينِ (3) إِيَّاكَ نَعْبُدُ وَإِيَّاكَ نَسْتَعِينُ (4) اهْدِنَا الصِّرَاطَ الْمُسْتَقِيمَ (5) صِرَاطَ الَّذِينَ أَنْعَمْتَ عَلَيْهِمْ غَيْرِ الْمَغْضُوبِ عَلَيْهِمْ وَلَا الضَّالِّينَ (6)"
            ];
            
            document.getElementById('sura-full-text').innerHTML = quranSample.join("<br><br>");
            if(isPlaying) { audio.pause(); isPlaying = false; document.getElementById('read-play-btn').innerHTML = `<i class="fa-solid fa-play"></i> استماع لهذه السورة`; }
        }

        function toggleReadAudio() {
            const btn = document.getElementById('read-play-btn');
            if(isPlaying) { 
                audio.pause(); 
                btn.innerHTML = `<i class="fa-solid fa-play"></i> استماع لهذه السورة`; 
            } else { 
                audio.play(); 
                btn.innerHTML = `<i class="fa-solid fa-pause"></i> إيقاف مؤقت`; 
            }
            isPlaying = !isPlaying;
        }

        function playMainAudio() {
            const rid = document.getElementById('main-reader-select').value;
            const sid = document.getElementById('main-sura-select').value;
            const r = allReaders.find(x => x.id === rid);
            const s = allSuras[parseInt(sid)-1];
            audio.src = `${r.s}/${sid}.mp3`;
            audio.play(); isPlaying = true;
            document.getElementById('player-card').classList.remove('hidden');
            document.getElementById('p-reader').innerText = r.name;
            document.getElementById('p-sura').innerText = `سورة ${s}`;
        }

        function stopAudio() { audio.pause(); isPlaying = false; document.getElementById('player-card').classList.add('hidden'); }

        function renderSubMenu(cat) {
            const container = document.getElementById('sub-container');
            document.getElementById('sub-title').innerText = database[cat].title;
            container.innerHTML = database[cat].items.map(i => `<div class="bg-white p-5 rounded-2xl flex justify-between items-center shadow-sm" onclick="renderZikrs('${cat}', '${i.id}')"><b>${i.n}</b><i class="fa-solid fa-chevron-left text-gray-200"></i></div>`).join('');
            showScreen('sub-menu-screen');
        }

        function renderZikrs(cat, subId) {
            const item = database[cat].items.find(i => i.id === subId);
            document.getElementById('zikr-title').innerText = item.n;
            document.getElementById('zikr-items-container').innerHTML = item.texts.map(obj => `<div class="zikr-row" onclick="let b=this.querySelector('.count-badge'); if(parseInt(b.innerText)>0) b.innerText--"><div class="count-badge">${obj.c}</div><p class="text-lg font-bold text-gray-800 leading-relaxed">${obj.t}</p></div>`).join('');
            showScreen('zikr-list-screen');
        }

        function initRadar() {
            const r = document.getElementById('kaaba-radar-item');
            const d = document.getElementById('degree-val');
            window.addEventListener('deviceorientation', (e) => {
                let h = Math.round(e.webkitCompassHeading || e.alpha || 0);
                d.innerText = h + "°";
                let rot = 136 - h;
                const rad = (rot - 90) * (Math.PI / 180);
                r.style.transform = `translate(${95 * Math.cos(rad)}px, ${95 * Math.sin(rad)}px) rotate(45deg)`;
                document.getElementById('status-text').innerText = Math.abs(h - 136) < 10 ? "القبلة صحيحة ✅" : "اتبع السهم الأحمر";
            });
        }

        function resetZikrs() { showScreen('home-screen'); }
        window.onload = init;
    </script>
</body>
</html>

