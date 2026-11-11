```html
<!DOCTYPE html>
<html lang="hi" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DRA Exam Practice Simulator (IIBF)</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#2563eb',
                            600: '#1d4ed8',
                            700: '#1e40af',
                            800: '#1e3a8a',
                            900: '#0f172a'
                        }
                    }
                }
            }
        }
    </script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Font Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- PDF.js library for extracting text from PDF files -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
    <style>
        body { font-family: 'Inter', sans-serif; }
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: rgba(156, 163, 175, 0.4); border-radius: 4px; }
    </style>
</head>
<body class="bg-gray-50 text-gray-900 dark:bg-gray-900 dark:text-gray-100 min-h-screen flex flex-col transition-colors duration-200">

    <!-- Header / Navbar -->
    <header class="border-b border-gray-200 dark:border-gray-800 bg-white/80 dark:bg-gray-900/80 backdrop-blur sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3 cursor-pointer" onclick="switchView('dashboard')">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center text-white font-bold text-xl shadow-lg shadow-blue-500/20">
                    <i class="fa-solid fa-graduation-cap"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg leading-none tracking-tight">DRA Prep Master</h1>
                    <span class="text-xs text-blue-600 dark:text-blue-400 font-semibold">IIBF Debt Recovery Agent Portal</span>
                </div>
            </div>

            <!-- Header Controls -->
            <div class="flex items-center space-x-3">
                <!-- Language Toggle -->
                <button id="langToggleBtn" onclick="toggleLanguage()" class="px-3 py-1.5 rounded-lg border border-gray-300 dark:border-gray-700 bg-gray-100 dark:bg-gray-800 text-xs font-semibold hover:bg-gray-200 dark:hover:bg-gray-700 transition flex items-center gap-1.5">
                    <i class="fa-solid fa-language text-blue-500"></i>
                    <span id="langText">Hindi / English</span>
                </button>

                <!-- Theme Toggle -->
                <button onclick="toggleDarkMode()" class="p-2 rounded-lg border border-gray-300 dark:border-gray-700 bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700 text-gray-600 dark:text-gray-300 transition">
                    <i id="themeIcon" class="fa-solid fa-moon"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">

        <!-- VIEW 1: DASHBOARD / HOME -->
        <div id="dashboardView" class="space-y-6">
            <!-- Hero Banner -->
            <div class="relative overflow-hidden rounded-2xl bg-gradient-to-r from-blue-600 via-indigo-600 to-purple-700 p-6 sm:p-8 text-white shadow-xl">
                <div class="relative z-10 max-w-2xl">
                    <span class="inline-block px-3 py-1 bg-white/20 rounded-full text-xs font-semibold mb-3 backdrop-blur-sm">IIBF Certification 2026</span>
                    <h2 class="text-2xl sm:text-3xl font-extrabold mb-2">DRA Recovery Agent Practice Exam</h2>
                    <p class="text-blue-100 text-sm sm:text-base mb-6">Master RBI Guidelines, Code of Ethics, Legal Regulations, and Recovery Techniques with targeted practice tests and instant PDF content import.</p>
                    <div class="flex flex-wrap gap-3">
                        <button onclick="startQuiz('all')" class="bg-white text-blue-700 font-semibold px-5 py-2.5 rounded-xl shadow-lg hover:bg-blue-50 transition transform active:scale-95 flex items-center gap-2">
                            <i class="fa-solid fa-play"></i> Start Full Practice Test
                        </button>
                        <button onclick="switchView('upload')" class="bg-blue-800/60 hover:bg-blue-800/80 text-white font-semibold px-5 py-2.5 rounded-xl border border-white/20 transition flex items-center gap-2 backdrop-blur-sm">
                            <i class="fa-solid fa-file-pdf"></i> Upload Study PDF / Text
                        </button>
                    </div>
                </div>
                <div class="absolute right-0 bottom-0 opacity-10 translate-x-1/4 translate-y-1/4 pointer-events-none">
                    <i class="fa-solid fa-building-columns text-[250px]"></i>
                </div>
            </div>

            <!-- Dashboard Stats Summary -->
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                <div class="bg-white dark:bg-gray-800 p-4 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm flex items-center gap-4">
                    <div class="p-3 bg-blue-100 dark:bg-blue-900/40 text-blue-600 dark:text-blue-400 rounded-lg">
                        <i class="fa-solid fa-question-circle text-xl"></i>
                    </div>
                    <div>
                        <p class="text-xs text-gray-500 dark:text-gray-400">Total Available</p>
                        <h3 id="totalQuestionsCount" class="text-lg font-bold">0 MCQs</h3>
                    </div>
                </div>
                <div class="bg-white dark:bg-gray-800 p-4 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm flex items-center gap-4">
                    <div class="p-3 bg-green-100 dark:bg-green-900/40 text-green-600 dark:text-green-400 rounded-lg">
                        <i class="fa-solid fa-trophy text-xl"></i>
                    </div>
                    <div>
                        <p class="text-xs text-gray-500 dark:text-gray-400">Best Score</p>
                        <h3 id="bestScoreText" class="text-lg font-bold">0%</h3>
                    </div>
                </div>
                <div class="bg-white dark:bg-gray-800 p-4 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm flex items-center gap-4">
                    <div class="p-3 bg-yellow-100 dark:bg-yellow-900/40 text-yellow-600 dark:text-yellow-400 rounded-lg">
                        <i class="fa-solid fa-clock-rotate-left text-xl"></i>
                    </div>
                    <div>
                        <p class="text-xs text-gray-500 dark:text-gray-400">Tests Taken</p>
                        <h3 id="testsTakenCount" class="text-lg font-bold">0</h3>
                    </div>
                </div>
                <div class="bg-white dark:bg-gray-800 p-4 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm flex items-center gap-4">
                    <div class="p-3 bg-purple-100 dark:bg-purple-900/40 text-purple-600 dark:text-purple-400 rounded-lg">
                        <i class="fa-solid fa-layer-group text-xl"></i>
                    </div>
                    <div>
                        <p class="text-xs text-gray-500 dark:text-gray-400">Modules</p>
                        <h3 class="text-lg font-bold">5 Topics</h3>
                    </div>
                </div>
            </div>

            <!-- Subject / Module Selection Grid -->
            <div>
                <h3 class="text-lg font-bold mb-4 flex items-center gap-2">
                    <i class="fa-solid fa-book-open text-blue-600"></i> Practice by Topic / Subject
                </h3>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4" id="moduleCardsContainer">
                    <!-- Dynamic Modules rendering -->
                </div>
            </div>
        </div>

        <!-- VIEW 2: QUIZ / PRACTICE TEST -->
        <div id="quizView" class="hidden space-y-4 max-w-4xl mx-auto">
            <!-- Quiz Control Header -->
            <div class="bg-white dark:bg-gray-800 rounded-2xl p-4 border border-gray-200 dark:border-gray-700 shadow-sm flex flex-wrap items-center justify-between gap-4">
                <div class="flex items-center space-x-3">
                    <button onclick="switchView('dashboard')" class="text-gray-500 hover:text-gray-700 dark:hover:text-white p-2 rounded-lg bg-gray-100 dark:bg-gray-700">
                        <i class="fa-solid fa-arrow-left"></i>
                    </button>
                    <div>
                        <h2 id="quizCategoryTitle" class="font-bold text-sm sm:text-base">DRA Practice Test</h2>
                        <span id="quizModeBadge" class="text-xs px-2 py-0.5 rounded bg-blue-100 dark:bg-blue-900/50 text-blue-700 dark:text-blue-300 font-medium">Practice Mode</span>
                    </div>
                </div>

                <div class="flex items-center space-x-4">
                    <!-- Timer -->
                    <div class="flex items-center gap-2 text-sm font-semibold bg-gray-100 dark:bg-gray-700 px-3 py-1.5 rounded-lg">
                        <i class="fa-regular fa-clock text-blue-500"></i>
                        <span id="timerDisplay">00:00</span>
                    </div>
                    <!-- Question Tracker -->
                    <div class="text-sm font-semibold">
                        Q <span id="currentQNum">1</span> / <span id="totalQNum">10</span>
                    </div>
                </div>
            </div>

            <!-- Progress Bar -->
            <div class="w-full bg-gray-200 dark:bg-gray-700 h-2 rounded-full overflow-hidden">
                <div id="progressBar" class="bg-blue-600 h-full w-0 transition-all duration-300"></div>
            </div>

            <!-- Main Question Card -->
            <div class="bg-white dark:bg-gray-800 rounded-2xl p-6 sm:p-8 border border-gray-200 dark:border-gray-700 shadow-md space-y-6">
                <!-- Question Tag & Language Toggle -->
                <div class="flex justify-between items-center text-xs font-semibold text-gray-500">
                    <span id="qCategoryTag" class="uppercase tracking-wider px-2.5 py-1 rounded bg-gray-100 dark:bg-gray-700">RBI Guidelines</span>
                    <span id="qDifficulty" class="text-blue-600 dark:text-blue-400">Medium</span>
                </div>

                <!-- Question Text -->
                <div class="space-y-2">
                    <h3 id="questionTextEng" class="text-base sm:text-lg font-semibold leading-relaxed text-gray-900 dark:text-white"></h3>
                    <p id="questionTextHin" class="text-sm sm:text-base text-gray-600 dark:text-gray-300 leading-relaxed italic border-t border-gray-100 dark:border-gray-700 pt-2"></p>
                </div>

                <!-- Options -->
                <div id="optionsContainer" class="space-y-3 pt-2">
                    <!-- Option Buttons Rendered Here -->
                </div>

                <!-- Instant Explanation Box -->
                <div id="explanationBox" class="hidden p-4 rounded-xl border bg-blue-50 dark:bg-gray-700/50 border-blue-200 dark:border-gray-600 space-y-2">
                    <h4 class="font-bold text-sm flex items-center gap-2 text-blue-800 dark:text-blue-300">
                        <i class="fa-solid fa-circle-info"></i> Explanation / स्पष्टीकरण:
                    </h4>
                    <p id="explanationText" class="text-xs sm:text-sm text-gray-700 dark:text-gray-300 leading-relaxed"></p>
                </div>
            </div>

            <!-- Footer Action Buttons -->
            <div class="flex justify-between items-center pt-2">
                <button id="prevBtn" onclick="navigateQuestion(-1)" class="px-4 py-2 rounded-xl border border-gray-300 dark:border-gray-700 text-sm font-semibold hover:bg-gray-100 dark:hover:bg-gray-800 disabled:opacity-40">
                    <i class="fa-solid fa-chevron-left mr-1"></i> Previous
                </button>
                <button id="nextBtn" onclick="navigateQuestion(1)" class="px-5 py-2.5 rounded-xl bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold shadow-md flex items-center gap-2">
                    Next <i class="fa-solid fa-chevron-right"></i>
                </button>
                <button id="submitBtn" onclick="finishQuiz()" class="hidden px-5 py-2.5 rounded-xl bg-green-600 hover:bg-green-700 text-white text-sm font-semibold shadow-md flex items-center gap-2">
                    Submit Exam <i class="fa-solid fa-check-circle"></i>
                </button>
            </div>
        </div>

        <!-- VIEW 3: RESULT & REVIEW -->
        <div id="resultView" class="hidden max-w-3xl mx-auto space-y-6">
            <div class="bg-white dark:bg-gray-800 rounded-2xl p-6 sm:p-8 border border-gray-200 dark:border-gray-700 shadow-xl text-center space-y-6">
                <!-- Result Badge -->
                <div class="inline-flex items-center justify-center w-20 h-20 rounded-full bg-blue-100 dark:bg-blue-900/50 text-blue-600 dark:text-blue-400 text-4xl mb-2 mx-auto">
                    <i id="resultIcon" class="fa-solid fa-award"></i>
                </div>
                
                <div>
                    <h2 class="text-2xl font-bold">Exam Completed!</h2>
                    <p class="text-sm text-gray-500 dark:text-gray-400">Here is your performance summary for the DRA practice session.</p>
                </div>

                <!-- Main Score Circle & Stats -->
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 py-4 border-y border-gray-100 dark:border-gray-700">
                    <div class="p-4 bg-gray-50 dark:bg-gray-700/40 rounded-xl">
                        <span class="text-xs text-gray-500 dark:text-gray-400 uppercase font-semibold">Score</span>
                        <p class="text-2xl font-black text-blue-600 dark:text-blue-400"><span id="resScore">0</span> / <span id="resTotal">0</span></p>
                    </div>
                    <div class="p-4 bg-gray-50 dark:bg-gray-700/40 rounded-xl">
                        <span class="text-xs text-gray-500 dark:text-gray-400 uppercase font-semibold">Percentage</span>
                        <p id="resPercent" class="text-2xl font-black text-green-600">0%</p>
                    </div>
                    <div class="p-4 bg-gray-50 dark:bg-gray-700/40 rounded-xl">
                        <span class="text-xs text-gray-500 dark:text-gray-400 uppercase font-semibold">Time Taken</span>
                        <p id="resTime" class="text-2xl font-black text-purple-600">00:00</p>
                    </div>
                </div>

                <!-- Status Banner -->
                <div id="statusBanner" class="p-3 rounded-xl font-semibold text-sm">
                    <!-- Passed/Failed Message -->
                </div>

                <!-- Action Buttons -->
                <div class="flex flex-wrap justify-center gap-4">
                    <button onclick="retakeQuiz()" class="px-5 py-2.5 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold text-sm shadow-md flex items-center gap-2">
                        <i class="fa-solid fa-rotate-right"></i> Retake Quiz
                    </button>
                    <button onclick="switchView('dashboard')" class="px-5 py-2.5 rounded-xl border border-gray-300 dark:border-gray-700 font-semibold text-sm hover:bg-gray-100 dark:hover:bg-gray-800">
                        <i class="fa-solid fa-house"></i> Home Dashboard
                    </button>
                </div>
            </div>

            <!-- Detailed Question Review List -->
            <div class="bg-white dark:bg-gray-800 rounded-2xl p-6 border border-gray-200 dark:border-gray-700 shadow-md space-y-4">
                <h3 class="font-bold text-lg border-b border-gray-200 dark:border-gray-700 pb-3 flex items-center gap-2">
                    <i class="fa-solid fa-list-check text-blue-500"></i> Detailed Answer Key & Review
                </h3>
                <div id="reviewList" class="space-y-4">
                    <!-- Review cards generated dynamically -->
                </div>
            </div>
        </div>

        <!-- VIEW 4: PDF / CUSTOM CONTENT IMPORT -->
        <div id="uploadView" class="hidden max-w-3xl mx-auto space-y-6">
            <div class="bg-white dark:bg-gray-800 rounded-2xl p-6 sm:p-8 border border-gray-200 dark:border-gray-700 shadow-md space-y-6">
                <div class="flex items-center space-x-3">
                    <button onclick="switchView('dashboard')" class="text-gray-500 hover:text-gray-700 dark:hover:text-white p-2 rounded-lg bg-gray-100 dark:bg-gray-700">
                        <i class="fa-solid fa-arrow-left"></i>
                    </button>
                    <div>
                        <h2 class="text-xl font-bold">Import Study Material & PDF</h2>
                        <p class="text-xs text-gray-500">Upload your PDF or paste notes to convert them into custom MCQs!</p>
                    </div>
                </div>

                <!-- Tabs: PDF Upload OR Manual Add -->
                <div class="flex border-b border-gray-200 dark:border-gray-700">
                    <button id="tabPdfBtn" onclick="switchUploadTab('pdf')" class="py-2.5 px-4 font-semibold text-sm border-b-2 border-blue-600 text-blue-600 dark:text-blue-400">
                        <i class="fa-solid fa-file-pdf mr-1"></i> Extract from PDF / Text
                    </button>
                    <button id="tabManualBtn" onclick="switchUploadTab('manual')" class="py-2.5 px-4 font-semibold text-sm text-gray-500 border-b-2 border-transparent hover:text-gray-700">
                        <i class="fa-solid fa-pen-to-square mr-1"></i> Add Custom Question
                    </button>
                </div>

                <!-- TAB 1: PDF/Text Generator -->
                <div id="pdfTabContent" class="space-y-4">
                    <div>
                        <label class="block text-xs font-bold uppercase text-gray-500 mb-2">Upload PDF File</label>
                        <input type="file" id="pdfFileInput" accept=".pdf" class="block w-full text-sm text-gray-500 file:mr-4 file:py-2.5 file:px-4 file:rounded-xl file:border-0 file:text-sm file:font-semibold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100 dark:file:bg-gray-700 dark:file:text-blue-300" />
                    </div>

                    <div class="relative flex items-center justify-center my-4">
                        <div class="border-t border-gray-200 dark:border-gray-700 w-full"></div>
                        <span class="bg-white dark:bg-gray-800 px-3 text-xs text-gray-400 font-semibold uppercase">OR Paste Text</span>
                    </div>

                    <div>
                        <label class="block text-xs font-bold uppercase text-gray-500 mb-2">Paste Book / PDF Text Notes</label>
                        <textarea id="rawTextContent" rows="6" placeholder="
