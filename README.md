# Hangul-Syllable-Generator

html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hangul Syllabary Generator</title>

    <!-- Embedded SVG Favicon & Apple Touch Icon (Royal Blue square with white '한') -->
    <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 512 512'><rect width='512' height='512' rx='108' fill='%234F46E5'/><text x='50%25' y='64%25' font-family='Gowun Batang, Noto Sans KR, serif' font-size='320' font-weight='700' fill='%23ffffff' text-anchor='middle' dominant-baseline='middle'>한</text></svg>">
    <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 512 512'><rect width='512' height='512' rx='108' fill='%234F46E5'/><text x='50%25' y='64%25' font-family='Gowun Batang, Noto Sans KR, serif' font-size='320' font-weight='700' fill='%23ffffff' text-anchor='middle' dominant-baseline='middle'>한</text></svg>">

    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts for Korean & English typography -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Gowun+Batang:wght@400;700&family=Inter:wght@300;400;500;600;700&family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'Noto Sans KR', 'sans-serif'],
                        hangul: ['Gowun Batang', 'Noto Sans KR', 'serif']
                    }
                }
            }
        }
    </script>
    <style>
        .hangul-display {
            font-family: 'Gowun Batang', 'Noto Sans KR', serif;
        }
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen flex flex-col">

    <!-- Navigation Header -->
    <header class="bg-slate-900 text-white border-b border-slate-800 sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-4 sm:px-6 lg:px-8 flex flex-col md:flex-row md:items-center md:justify-between gap-4">
            <div class="flex items-center space-x-3">
                <!-- Custom "한" Icon matching requested design -->
                <div class="w-10 h-10 bg-indigo-600 rounded-xl flex items-center justify-center text-2xl font-bold font-hangul text-white shadow-lg shadow-indigo-500/30 select-none">
                    한
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight text-white flex items-center gap-2">
                        Hangul Syllabary Generator
                        <span class="text-xs font-normal px-2 py-0.5 bg-amber-500/20 text-amber-300 border border-amber-500/30 rounded-full">Archaic + Modern</span>
                    </h1>
                    <p class="text-xs text-slate-400">Explore 1,608,528 Unicode Hangul Combinations</p>
                </div>
            </div>
            
            <div class="flex items-center gap-4 text-xs">
                <div class="bg-slate-800 px-3 py-1.5 rounded-lg border border-slate-700 flex items-center gap-2">
                    <span class="text-slate-400">Total Possibilities:</span>
                    <span id="total-permutations-badge" class="font-mono font-bold text-emerald-400">1,608,528</span>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Workspace -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 flex-1 w-full space-y-6">

        <!-- Tab Navigation Bar -->
        <nav class="flex border-b border-slate-200 gap-2 overflow-x-auto pb-1">
            <button onclick="switchTab('interactive')" id="tab-btn-interactive" class="px-4 py-2 text-sm font-medium rounded-t-lg transition-colors border-b-2 border-indigo-600 text-indigo-600 bg-white shadow-sm flex items-center gap-2 whitespace-nowrap">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"></path></svg>
                Interactive Creator
            </button>
            <button onclick="switchTab('random')" id="tab-btn-random" class="px-4 py-2 text-sm font-medium rounded-t-lg transition-colors text-slate-600 hover:text-slate-900 hover:bg-slate-100 flex items-center gap-2 whitespace-nowrap">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"></path></svg>
                Random Generator
            </button>
            <button onclick="switchTab('matrix')" id="tab-btn-matrix" class="px-4 py-2 text-sm font-medium rounded-t-lg transition-colors text-slate-600 hover:text-slate-900 hover:bg-slate-100 flex items-center gap-2 whitespace-nowrap">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path></svg>
                Batch Matrix & Exporter
            </button>
            <button onclick="switchTab('unicode-info')" id="tab-btn-unicode-info" class="px-4 py-2 text-sm font-medium rounded-t-lg transition-colors text-slate-600 hover:text-slate-900 hover:bg-slate-100 flex items-center gap-2 whitespace-nowrap">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                Unicode Ranges & Stats
            </button>
        </nav>

        <!-- Tab 1: Interactive Creator View -->
        <div id="tab-interactive" class="tab-content block">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <!-- Syllable Preview Display Card -->
                <div class="lg:col-span-4 flex flex-col gap-6">
                    <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 flex flex-col items-center justify-center text-center relative overflow-hidden">
                        <button onclick="copyCurrentSyllable()" class="absolute top-3 right-3 p-2 text-slate-400 hover:text-indigo-600 transition-colors rounded-lg hover:bg-slate-100" title="Copy Syllable">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                        </button>

                        <!-- Large Syllable Render -->
                        <div class="my-6">
                            <div id="main-syllable-display" class="hangul-display text-8xl md:text-9xl font-bold text-slate-900 tracking-normal min-h-[160px] flex items-center justify-center select-all">
                                가
                            </div>
                        </div>

                        <!-- Component Breakdown Table -->
                        <div class="w-full space-y-3 border-t border-slate-100 pt-4 text-left">
                            <div class="text-xs font-semibold uppercase tracking-wider text-slate-400">Component Breakdown</div>
                            
                            <div class="flex items-center justify-between text-sm bg-slate-50 p-2.5 rounded-lg border border-slate-100">
                                <span class="text-slate-500">Choseong (Initial):</span>
                                <span class="font-mono font-medium text-slate-800 flex items-center gap-2">
                                    <span id="display-choseong-char" class="hangul-display text-base font-bold text-indigo-600">ᄀ</span>
                                    <span id="display-choseong-code" class="text-xs bg-indigo-50 text-indigo-700 px-1.5 py-0.5 rounded">U+1100</span>
                                </span>
                            </div>
                            
                            <div class="flex items-center justify-between text-sm bg-slate-50 p-2.5 rounded-lg border border-slate-100">
                                <span class="text-slate-500">Jungseong (Medial):</span>
                                <span class="font-mono font-medium text-slate-800 flex items-center gap-2">
                                    <span id="display-jungseong-char" class="hangul-display text-base font-bold text-indigo-600">ᅡ</span>
                                    <span id="display-jungseong-code" class="text-xs bg-indigo-50 text-indigo-700 px-1.5 py-0.5 rounded">U+1161</span>
                                </span>
                            </div>

                            <div class="flex items-center justify-between text-sm bg-slate-50 p-2.5 rounded-lg border border-slate-100">
                                <span class="text-slate-500">Jongseong (Final):</span>
                                <span class="font-mono font-medium text-slate-800 flex items-center gap-2">
                                    <span id="display-jongseong-char" class="hangul-display text-base font-bold text-indigo-600">&nbsp;</span>
                                    <span id="display-jongseong-code" class="text-xs bg-slate-200 text-slate-600 px-1.5 py-0.5 rounded">None</span>
                                </span>
                            </div>

                            <div class="flex items-center justify-between text-xs text-slate-500 pt-2 border-t border-slate-100">
                                <span>Sequence:</span>
                                <span id="display-sequence" class="font-mono text-slate-700 font-medium">U+1100 U+1161</span>
                            </div>
                        </div>

                        <!-- Toast Notification -->
                        <div id="copy-toast" class="opacity-0 transition-opacity duration-300 absolute bottom-3 bg-slate-800 text-white text-xs px-3 py-1.5 rounded-full shadow-lg pointer-events-none">
                            Copied to clipboard!
                        </div>
                    </div>
                </div>

                <!-- Jamo Interactive Selector Controls -->
                <div class="lg:col-span-8 bg-white rounded-2xl shadow-sm border border-slate-200 p-6 flex flex-col">
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 border-b border-slate-200 pb-4 mb-4">
                        <div class="flex items-center gap-2">
                            <h2 class="font-bold text-slate-900 text-lg">Jamo Selector</h2>
                            <span class="text-xs text-slate-500">Select components to build syllable</span>
                        </div>
                        
                        <!-- Filter Toggle buttons -->
                        <div class="flex items-center gap-2 bg-slate-100 p-1 rounded-lg">
                            <button id="filter-all" onclick="setJamoFilter('all')" class="px-3 py-1 text-xs font-medium rounded-md bg-white shadow-xs text-slate-800">All Jamo</button>
                            <button id="filter-modern" onclick="setJamoFilter('modern')" class="px-3 py-1 text-xs font-medium rounded-md text-slate-600 hover:text-slate-900">Modern Only</button>
                            <button id="filter-archaic" onclick="setJamoFilter('archaic')" class="px-3 py-1 text-xs font-medium rounded-md text-slate-600 hover:text-slate-900">Archaic Only</button>
                        </div>
                    </div>

                    <!-- Inner Category Tabs -->
                    <div class="flex gap-2 mb-4 border-b border-slate-100 pb-2">
                        <button onclick="switchJamoCategory('choseong')" id="jamo-cat-choseong" class="px-3 py-1.5 text-xs font-bold rounded-lg bg-indigo-50 text-indigo-700 border border-indigo-200 flex items-center gap-1.5">
                            Choseong (Initial) <span id="count-choseong" class="px-1.5 py-0.2 bg-indigo-200 text-indigo-800 rounded-full text-[10px]">124</span>
                        </button>
                        <button onclick="switchJamoCategory('jungseong')" id="jamo-cat-jungseong" class="px-3 py-1.5 text-xs font-bold rounded-lg text-slate-600 hover:bg-slate-100 flex items-center gap-1.5">
                            Jungseong (Medial) <span id="count-jungseong" class="px-1.5 py-0.2 bg-slate-200 text-slate-700 rounded-full text-[10px]">94</span>
                        </button>
                        <button onclick="switchJamoCategory('jongseong')" id="jamo-cat-jongseong" class="px-3 py-1.5 text-xs font-bold rounded-lg text-slate-600 hover:bg-slate-100 flex items-center gap-1.5">
                            Jongseong (Final) <span id="count-jongseong" class="px-1.5 py-0.2 bg-slate-200 text-slate-700 rounded-full text-[10px]">138</span>
                        </button>
                    </div>

                    <!-- Grid Layout for Jamo selection -->
                    <div id="jamo-grid-container" class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 lg:grid-cols-10 gap-2 max-h-[500px] overflow-y-auto p-2 bg-slate-50 rounded-xl border border-slate-200/80">
                        <!-- Dynamic Jamo grid content -->
                    </div>
                </div>
            </div>
        </div>

        <!-- Tab 2: Random Generator Tab -->
        <div id="tab-random" class="tab-content hidden">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 space-y-6">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 border-b border-slate-200 pb-4">
                    <div>
                        <h2 class="text-xl font-bold text-slate-900">Random Syllable Generator</h2>
                        <p class="text-xs text-slate-500">Generate batches of random Hangul syllables based on your criteria.</p>
                    </div>
                    
                    <div class="flex items-center gap-3">
                        <select id="random-count" class="px-3 py-2 border border-slate-300 rounded-lg text-sm bg-slate-50 font-medium">
                            <option value="12">12 Syllables</option>
                            <option value="24" selected>24 Syllables</option>
                            <option value="48">48 Syllables</option>
                            <option value="96">96 Syllables</option>
                        </select>
                        <button onclick="generateRandomSyllables()" class="px-5 py-2 bg-indigo-600 hover:bg-indigo-700 text-white font-medium rounded-lg text-sm transition-colors flex items-center gap-2 shadow-sm">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path></svg>
                            Generate New Batch
                        </button>
                    </div>
                </div>

                <!-- Controls -->
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 bg-slate-50 p-4 rounded-xl border border-slate-200">
                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-500 mb-2">Syllable Type</label>
                        <select id="random-mode" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm bg-white">
                            <option value="all">Any (Modern & Archaic)</option>
                            <option value="modern">Modern Standard Only (11,172)</option>
                            <option value="archaic">Archaic / Extended Only</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-500 mb-2">Final Consonant (Jongseong)</label>
                        <select id="random-jong-option" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm bg-white">
                            <option value="mix">Mix (With & Without Final)</option>
                            <option value="always">Always Include Final</option>
                            <option value="never">No Final Consonant (Open Syllables)</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-500 mb-2">Filter Constraints</label>
                        <div class="flex items-center gap-2 pt-2">
                            <label class="inline-flex items-center text-xs text-slate-700 cursor-pointer">
                                <input type="checkbox" id="random-unique" class="rounded text-indigo-600 focus:ring-indigo-500" checked>
                                <span class="ml-2">Ensure Unique Cards</span>
                            </label>
                        </div>
                    </div>
                </div>

                <!-- Cards Grid -->
                <div id="random-output-grid" class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-6 lg:grid-cols-8 gap-3 min-h-[250px]">
                    <!-- Dynamic batch content -->
                </div>
            </div>
        </div>

        <!-- Tab 3: Matrix / Batch Exporter Tab -->
        <div id="tab-matrix" class="tab-content hidden">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 space-y-6">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 border-b border-slate-200 pb-4">
                    <div>
                        <h2 class="text-xl font-bold text-slate-900">Batch Syllabary Matrix Generator</h2>
                        <p class="text-xs text-slate-500">Cross-multiply selected Choseong and Jungseong to produce combinations with optional Jongseong.</p>
                    </div>

                    <div class="flex items-center gap-2">
                        <button onclick="exportMatrix('txt')" class="px-4 py-2 border border-slate-300 hover:bg-slate-50 text-slate-700 font-medium rounded-lg text-xs transition-colors flex items-center gap-1.5">
                            Export TXT
                        </button>
                        <button onclick="exportMatrix('csv')" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white font-medium rounded-lg text-xs transition-colors flex items-center gap-1.5">
                              Export CSV
                        </button>
                    </div>
                </div>

                <!-- Matrix Controls -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 bg-slate-50 p-4 rounded-xl border border-slate-200">
                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-500 mb-2">Choseong Selection</label>
                        <select id="matrix-choseong-preset" onchange="updateMatrixPresets()" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-xs bg-white mb-2">
                            <option value="modern_basic">Basic Modern (14: ㄱㄴㄷㄹㅁㅂㅅㅇㅈㅊㅋㅌㅍㅎ)</option>
                            <option value="modern_all">All Modern Initial (19)</option>
                            <option value="archaic_sample">Sample Archaic (ㅿ ㆁ ㆆ...)</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-500 mb-2">Jungseong Selection</label>
                        <select id="matrix-jungseong-preset" onchange="updateMatrixPresets()" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-xs bg-white mb-2">
                            <option value="modern_basic">Basic Modern Vowels (10: ㅏㅑㅓㅕㅗㅛㅜㅠㅡㅣ)</option>
                            <option value="modern_all">All Modern Vowels (21)</option>
                            <option value="archaic_sample">Sample Archaic Vowels (ㆍ ㆎ...)</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase text-slate-500 mb-2">Jongseong (Optional)</label>
                        <select id="matrix-jongseong-preset" onchange="updateMatrixPresets()" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-xs bg-white mb-2">
                            <option value="none">None (Open Syllables)</option>
                            <option value="modern_basic">Basic Final Consonants (ㄱ, ㄴ, ㄹ, ㅁ, ㅂ, ㅇ)</option>
                            <option value="modern_all">All Modern Final Consonants (27)</option>
                        </select>
                    </div>
                </div>

                <!-- Table Preview -->
                <div class="space-y-2">
                    <div class="flex items-center justify-between text-xs text-slate-500">
                        <span>Generated Matrix (<span id="matrix-count">0</span> syllables)</span>
                        <button onclick="copyMatrixText()" class="text-indigo-600 hover:text-indigo-800 font-medium">Copy Raw Text</button>
                    </div>
                    <div id="matrix-output-table-container" class="overflow-x-auto border border-slate-200 rounded-xl max-h-[400px]">
                        <!-- Rendered Matrix -->
                    </div>
                </div>
            </div>
        </div>

        <!-- Tab 4: Unicode Specifications & Stats Tab -->
        <div id="tab-unicode-info" class="tab-content hidden">
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200 p-6 space-y-6">
                <div class="border-b border-slate-200 pb-4">
                    <h2 class="text-xl font-bold text-slate-900">Unicode Hangul Jamo Specifications</h2>
                    <p class="text-xs text-slate-500">Comprehensive breakdown of Unicode blocks supported by this generator (1,608,528 total combinations).</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <!-- Choseong Card -->
                    <div class="bg-slate-50 p-5 rounded-xl border border-slate-200 space-y-3">
                        <div class="flex items-center justify-between">
                            <h3 class="font-bold text-slate-900">Choseong (Initial)</h3>
                            <span class="px-2 py-0.5 bg-indigo-100 text-indigo-800 font-mono text-xs font-bold rounded" id="info-count-choseong">124 Total</span>
                        </div>
                        <p class="text-xs text-slate-600">Leading consonants positioned at the beginning of a Hangul syllable block.</p>
                        <div class="text-xs font-mono bg-white p-3 rounded-lg border border-slate-200 space-y-1">
                            <div class="flex justify-between"><span>Primary Range:</span> <span class="text-indigo-600">U+1100 – U+115E (95)</span></div>
                            <div class="flex justify-between"><span>Extended-A:</span> <span class="text-indigo-600">U+A960 – U+A97C (29)</span></div>
                            <div class="flex justify-between pt-1 border-t border-slate-100"><span>Modern Standard:</span> <span>19</span></div>
                            <div class="flex justify-between"><span>Archaic / Extended:</span> <span>105</span></div>
                        </div>
                    </div>

                    <!-- Jungseong Card -->
                    <div class="bg-slate-50 p-5 rounded-xl border border-slate-200 space-y-3">
                        <div class="flex items-center justify-between">
                            <h3 class="font-bold text-slate-900">Jungseong (Medial)</h3>
                            <span class="px-2 py-0.5 bg-indigo-100 text-indigo-800 font-mono text-xs font-bold rounded" id="info-count-jungseong">94 Total</span>
                        </div>
                        <p class="text-xs text-slate-600">Medial vowels or diphthongs positioned in the middle of a syllable block.</p>
                        <div class="text-xs font-mono bg-white p-3 rounded-lg border border-slate-200 space-y-1">
                            <div class="flex justify-between"><span>Primary Range:</span> <span class="text-indigo-600">U+1161 – U+11A7 (71)</span></div>
                            <div class="flex justify-between"><span>Extended-B:</span> <span class="text-indigo-600">U+D7B0 – U+D7C6 (23)</span></div>
                            <div class="flex justify-between pt-1 border-t border-slate-100"><span>Modern Standard:</span> <span>21</span></div>
                            <div class="flex justify-between"><span>Archaic / Extended:</span> <span>73</span></div>
                        </div>
                    </div>

                    <!-- Jongseong Card -->
                    <div class="bg-slate-50 p-5 rounded-xl border border-slate-200 space-y-3">
                        <div class="flex items-center justify-between">
                            <h3 class="font-bold text-slate-900">Jongseong (Final)</h3>
                            <span class="px-2 py-0.5 bg-indigo-100 text-indigo-800 font-mono text-xs font-bold rounded" id="info-count-jongseong">138 Total</span>
                        </div>
                        <p class="text-xs text-slate-600">Trailing consonants at the base of the syllable block (includes None/Empty space).</p>
                        <div class="text-xs font-mono bg-white p-3 rounded-lg border border-slate-200 space-y-1">
                            <div class="flex justify-between"><span>Primary Range:</span> <span class="text-indigo-600">U+11A8 – U+11FF (88)</span></div>
                            <div class="flex justify-between"><span>Extended-B:</span> <span class="text-indigo-600">U+D7CB – U+D7FB (49)</span></div>
                            <div class="flex justify-between pt-1 border-t border-slate-100"><span>Modern Standard:</span> <span>27 (+1 Empty)</span></div>
                            <div class="flex justify-between"><span>Archaic / Extended:</span> <span>110</span></div>
                        </div>
                    </div>
                </div>

                <!-- Equation Summary -->
                <div class="p-4 bg-indigo-900 text-white rounded-xl space-y-2">
                    <h4 class="font-bold text-sm text-indigo-200 uppercase tracking-wider">Permutation Calculation</h4>
                    <div class="font-mono text-base flex flex-wrap items-center gap-2">
                        <span>Choseong (124)</span>
                        <span class="text-indigo-400">×</span>
                        <span>Jungseong (94)</span>
                        <span class="text-indigo-400">×</span>
                        <span>Jongseong (138)</span>
                        <span class="text-indigo-400">=</span>
                        <span class="text-amber-300 font-bold text-xl">1,608,528 Syllable Blocks</span>
                    </div>
                    <p class="text-xs text-indigo-300">
                        While standard modern precomposed Hangul in Unicode (U+AC00–U+D7A3) covers exactly 11,172 syllables, dynamically combining Unicode Conjoining Jamo (NFD/NFC) yields exactly 1,608,528 phonetically valid structural Hangul blocks including Middle Korean and archaic historical manuscripts.
                    </p>
                </div>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="bg-slate-900 text-slate-400 text-xs py-6 border-t border-slate-800 mt-auto">
        <div class="max-w-7xl mx-auto px-4 text-center space-y-2">
            <p>Hangul Syllabary Generator — Designed for Korean Linguistics & Unicode Explorers</p>
            <p class="text-slate-500">Supports Unicode 15.0+ Hangul Jamo, Conjoining Jamo, Extended-A & Extended-B Blocks.</p>
        </div>
    </footer>

    <script>
        // Exact Unicode ranges requested
        const RANGES = {
            choseong: [
                { start: 0x1100, end: 0x115E }, // 95 items
                { start: 0xA960, end: 0xA97C }  // 29 items -> Total = 124
            ],
            jungseong: [
                { start: 0x1161, end: 0x11A7 }, // 71 items
                { start: 0xD7B0, end: 0xD7C6 }  // 23 items -> Total = 94
            ],
            jongseong: [
                { start: 0x11A8, end: 0x11FF }, // 88 items
                { start: 0xD7CB, end: 0xD7FB }  // 49 items -> Total = 137 (+ 1 None = 138)
            ]
        };

        // Modern Jamo codepoint sets for filtering
        const MODERN_CHOSEONG_CODES = new Set([
            0x1100, 0x1101, 0x1102, 0x1103, 0x1104, 0x1105, 0x1106, 0x1107, 0x1108, 0x1109,
            0x110A, 0x110B, 0x110C, 0x110D, 0x110E, 0x110F, 0x1110, 0x1111, 0x1112
        ]);

        const MODERN_JUNGSEONG_CODES = new Set([
            0x1161, 0x1162, 0x1163, 0x1164, 0x1165, 0x1166, 0x1167, 0x1168, 0x1169, 0x116A,
            0x116B, 0x116C, 0x116D, 0x116E, 0x116F, 0x1170, 0x1171, 0x1172, 0x1173, 0x1174, 0x1175
        ]);

        const MODERN_JONGSEONG_CODES = new Set([
            0x11A8, 0x11A9, 0x11AA, 0x11AB, 0x11AC, 0x11AD, 0x11AE, 0x11AF, 0x11B0, 0x11B1,
            0x11B2, 0x11B3, 0x11B4, 0x11B5, 0x11B6, 0x11B7, 0x11B8, 0x11B9, 0x11BA, 0x11BB,
            0x11BC, 0x11BD, 0x11BE, 0x11BF, 0x11C0, 0x11C1, 0x11C2
        ]);

        // State Store
        let jamoData = {
            choseong: [],
            jungseong: [],
            jongseong: []
        };

        let activeState = {
            filter: 'all', // 'all', 'modern', 'archaic'
            category: 'choseong', // 'choseong', 'jungseong', 'jongseong'
            selectedChoseong: null,
            selectedJungseong: null,
            selectedJongseong: null
        };

        function initJamoData() {
            jamoData.choseong = [];
            jamoData.jungseong = [];
            jamoData.jongseong = [];

            // Choseong (Initial)
            RANGES.choseong.forEach(r => {
                for (let code = r.start; code <= r.end; code++) {
                    jamoData.choseong.push({
                        code: code,
                        char: String.fromCodePoint(code),
                        hex: 'U+' + code.toString(16).toUpperCase(),
                        isModern: MODERN_CHOSEONG_CODES.has(code)
                    });
                }
            });

            // Jungseong (Medial)
            RANGES.jungseong.forEach(r => {
                for (let code = r.start; code <= r.end; code++) {
                    jamoData.jungseong.push({
                        code: code,
                        char: String.fromCodePoint(code),
                        hex: 'U+' + code.toString(16).toUpperCase(),
                        isModern: MODERN_JUNGSEONG_CODES.has(code)
                    });
                }
            });

            // Jongseong (Final) - First option is None (blank space)
            jamoData.jongseong.push({
                code: null,
                char: ' ', // Empty blank space instead of text
                hex: 'None',
                isModern: true
            });

            RANGES.jongseong.forEach(r => {
                for (let code = r.start; code <= r.end; code++) {
                    jamoData.jongseong.push({
                        code: code,
                        char: String.fromCodePoint(code),
                        hex: 'U+' + code.toString(16).toUpperCase(),
                        isModern: MODERN_JONGSEONG_CODES.has(code)
                    });
                }
            });

            // Default selection: ㄱ (U+1100) + ㅏ (U+1161) + None
            activeState.selectedChoseong = jamoData.choseong[0];
            activeState.selectedJungseong = jamoData.jungseong[0];
            activeState.selectedJongseong = jamoData.jongseong[0];

            // Calculate total permutations
            const total = jamoData.choseong.length * jamoData.jungseong.length * jamoData.jongseong.length;
            document.getElementById('total-permutations-badge').innerText = total.toLocaleString();

            // Info Tab counts
            document.getElementById('info-count-choseong').innerText = `${jamoData.choseong.length} Total`;
            document.getElementById('info-count-jungseong').innerText = `${jamoData.jungseong.length} Total`;
            document.getElementById('info-count-jongseong').innerText = `${jamoData.jongseong.length} Total`;
        }

        function renderJamoGrid() {
            const container = document.getElementById('jamo-grid-container');
            container.innerHTML = '';

            const items = jamoData[activeState.category].filter(item => {
                if (activeState.filter === 'modern') return item.isModern;
                if (activeState.filter === 'archaic') return !item.isModern;
                return true;
            });

            items.forEach(item => {
                const btn = document.createElement('button');
                const isSelected = activeState['selected' + capitalize(activeState.category)]?.code === item.code;

                btn.className = `p-2.5 rounded-xl border transition-all flex flex-col items-center justify-center gap-1 ${
                    isSelected 
                        ? 'bg-indigo-600 text-white border-indigo-600 shadow-md scale-105 z-10' 
                        : 'bg-white text-slate-800 border-slate-200 hover:border-indigo-300 hover:bg-indigo-50/50'
                }`;

                btn.onclick = () => selectJamo(activeState.category, item);

                const displayChar = (item.code === null) ? '&nbsp;' : item.char;

                btn.innerHTML = `
                    <span class="hangul-display text-2xl font-bold leading-none">${displayChar}</span>
                    <span class="text-[10px] font-mono ${isSelected ? 'text-indigo-200' : 'text-slate-400'}">${item.hex}</span>
                    ${!item.isModern && item.code ? '<span class="w-1.5 h-1.5 rounded-full bg-amber-400"></span>' : ''}
                `;

                container.appendChild(btn);
            });

            updateCategoryCounts();
        }

        function updateCategoryCounts() {
            const categories = ['choseong', 'jungseong', 'jongseong'];
            categories.forEach(cat => {
                const count = jamoData[cat].filter(item => {
                    if (activeState.filter === 'modern') return item.isModern;
                    if (activeState.filter === 'archaic') return !item.isModern;
                    return true;
                }).length;
                document.getElementById(`count-${cat}`).innerText = count;
            });
        }

        function selectJamo(category, item) {
            activeState['selected' + capitalize(category)] = item;
            renderJamoGrid();
            updateSyllableDisplay();
        }

        function updateSyllableDisplay() {
            const cho = activeState.selectedChoseong;
            const jung = activeState.selectedJungseong;
            const jong = activeState.selectedJongseong;

            let codes = [cho.code, jung.code];
            if (jong && jong.code) {
                codes.push(jong.code);
            }

            const rawSequence = String.fromCodePoint(...codes);
            const normalizedSyllable = rawSequence.normalize('NFC');

            const displayElem = document.getElementById('main-syllable-display');
            displayElem.innerText = normalizedSyllable;

            document.getElementById('display-choseong-char').innerText = cho.char;
            document.getElementById('display-choseong-code').innerText = cho.hex;

            document.getElementById('display-jungseong-char').innerText = jung.char;
            document.getElementById('display-jungseong-code').innerText = jung.hex;

            // Blank space displayed for empty Jongseong
            document.getElementById('display-jongseong-char').innerHTML = (jong.code === null) ? '&nbsp;' : jong.char;
            document.getElementById('display-jongseong-code').innerText = jong.hex;

            const seqHex = codes.map(c => 'U+' + c.toString(16).toUpperCase()).join(' ');
            document.getElementById('display-sequence').innerText = seqHex;
        }

        function generateRandomSyllables() {
            const count = parseInt(document.getElementById('random-count').value, 10);
            const mode = document.getElementById('random-mode').value;
            const jongOption = document.getElementById('random-jong-option').value;
            const ensureUnique = document.getElementById('random-unique').checked;

            let choPool = jamoData.choseong;
            let jungPool = jamoData.jungseong;
            let jongPool = jamoData.jongseong;

            if (mode === 'modern') {
                choPool = choPool.filter(x => x.isModern);
                jungPool = jungPool.filter(x => x.isModern);
                jongPool = jongPool.filter(x => x.isModern);
            } else if (mode === 'archaic') {
                choPool = choPool.filter(x => !x.isModern);
                jungPool = jungPool.filter(x => !x.isModern);
                jongPool = jongPool.filter(x => !x.isModern || x.code === null);
            }

            const results = [];
            const seen = new Set();

            for (let i = 0; i < count; i++) {
                let cho = choPool[Math.floor(Math.random() * choPool.length)];
                let jung = jungPool[Math.floor(Math.random() * jungPool.length)];
                
                let jong;
                if (jongOption === 'never') {
                    jong = jamoData.jongseong[0]; // None (blank space)
                } else if (jongOption === 'always') {
                    const validJongs = jongPool.filter(x => x.code !== null);
                    jong = validJongs[Math.floor(Math.random() * validJongs.length)];
                } else {
                    jong = jongPool[Math.floor(Math.random() * jongPool.length)];
                }

                let codes = [cho.code, jung.code];
                if (jong && jong.code) codes.push(jong.code);

                const syllable = String.fromCodePoint(...codes).normalize('NFC');

                if (ensureUnique && seen.has(syllable) && seen.size < (choPool.length * jungPool.length)) {
                    i--;
                    continue;
                }

                seen.add(syllable);
                results.push({
                    syllable,
                    seq: codes.map(c => 'U+' + c.toString(16).toUpperCase()).join(' '),
                    isModern: cho.isModern && jung.isModern && (jong.code === null || jong.isModern)
                });
            }

            renderRandomGrid(results);
        }

        function renderRandomGrid(items) {
            const container = document.getElementById('random-output-grid');
            container.innerHTML = '';

            items.forEach(item => {
                const card = document.createElement('div');
                card.className = "bg-white p-4 rounded-xl border border-slate-200 shadow-xs flex flex-col items-center justify-center text-center hover:border-indigo-400 transition-colors group relative cursor-pointer";
                card.onclick = () => {
                    copyText(item.syllable);
                    showToast(`Copied "${item.syllable}"`);
                };

                card.innerHTML = `
                    <span class="hangul-display text-4xl font-bold text-slate-800 group-hover:scale-110 transition-transform">${item.syllable}</span>
                    <span class="text-[10px] font-mono text-slate-400 mt-2">${item.seq}</span>
                    <span class="absolute top-2 right-2 text-[9px] px-1.5 py-0.5 rounded ${item.isModern ? 'bg-emerald-50 text-emerald-600' : 'bg-amber-50 text-amber-600'}">
                        ${item.isModern ? 'Modern' : 'Archaic'}
                    </span>
                `;

                container.appendChild(card);
            });
        }

        let currentMatrixData = [];

        function updateMatrixPresets() {
            const choVal = document.getElementById('matrix-choseong-preset').value;
            const jungVal = document.getElementById('matrix-jungseong-preset').value;
            const jongVal = document.getElementById('matrix-jongseong-preset').value;

            let choseongs = [];
            if (choVal === 'modern_basic') {
                choseongs = [0x1100, 0x1102, 0x1103, 0x1105, 0x1106, 0x1107, 0x1109, 0x110B, 0x110C, 0x110E, 0x110F, 0x1110, 0x1111, 0x1112];
            } else if (choVal === 'modern_all') {
                choseongs = Array.from(MODERN_CHOSEONG_CODES);
            } else {
                choseongs = [0x1140, 0x114C, 0x1159, 0x110B, 0x1100];
            }

            let jungseongs = [];
            if (jungVal === 'modern_basic') {
                jungseongs = [0x1161, 0x1163, 0x1165, 0x1167, 0x1169, 0x116D, 0x116E, 0x1172, 0x1173, 0x1175];
            } else if (jungVal === 'modern_all') {
                jungseongs = Array.from(MODERN_JUNGSEONG_CODES);
            } else {
                jungseongs = [0x119E, 0x11A1, 0x1161, 0x1169];
            }

            let jongseongs = [null];
            if (jongVal === 'modern_basic') {
                jongseongs = [null, 0x11A8, 0x11AB, 0x11AF, 0x11B7, 0x11B8, 0x11BC];
            } else if (jongVal === 'modern_all') {
                jongseongs = [null, ...Array.from(MODERN_JONGSEONG_CODES)];
            }

            currentMatrixData = [];
            choseongs.forEach(c => {
                jungseongs.forEach(j => {
                    jongseongs.forEach(k => {
                        let codes = [c, j];
                        if (k) codes.push(k);
                        const syl = String.fromCodePoint(...codes).normalize('NFC');
                        currentMatrixData.push({
                            syllable: syl,
                            choseongHex: 'U+' + c.toString(16).toUpperCase(),
                            jungseongHex: 'U+' + j.toString(16).toUpperCase(),
                            jongseongHex: k ? 'U+' + k.toString(16).toUpperCase() : 'None'
                        });
                    });
                });
            });

            document.getElementById('matrix-count').innerText = currentMatrixData.length;
            renderMatrixTable(choseongs, jungseongs, jongseongs);
        }

        function renderMatrixTable(choseongs, jungseongs, jongseongs) {
            const container = document.getElementById('matrix-output-table-container');
            
            let html = `<table class="w-full text-center border-collapse text-sm bg-white">
                <thead>
                    <tr class="bg-slate-100 text-slate-700 border-b border-slate-200">
                        <th class="p-2 border-r border-slate-200">Cho \\ Jung</th>`;
            
            jungseongs.forEach(j => {
                html += `<th class="p-2 border-r border-slate-200 hangul-display text-base font-bold">${String.fromCodePoint(j)}</th>`;
            });
            html += `</tr></thead><tbody>`;

            choseongs.forEach(c => {
                html += `<tr class="border-b border-slate-100"><td class="p-2 bg-slate-50 font-bold border-r border-slate-200 hangul-display text-base">${String.fromCodePoint(c)}</td>`;
                jungseongs.forEach(j => {
                    html += `<td class="p-2 border-r border-slate-100">`;
                    jongseongs.forEach(k => {
                        let codes = [c, j];
                        if (k) codes.push(k);
                        const syl = String.fromCodePoint(...codes).normalize('NFC');
                        html += `<span class="hangul-display text-xl inline-block mx-1 cursor-pointer hover:text-indigo-600" onclick="copyText('${syl}')" title="Click to copy">${syl}</span>`;
                    });
                    html += `</td>`;
                });
                html += `</tr>`;
            });

            html += `</tbody></table>`;
            container.innerHTML = html;
        }

        function exportMatrix(format) {
            if (!currentMatrixData.length) return;

            let content = "";
            let filename = `hangul_syllabary.${format}`;

            if (format === 'csv') {
                content = "Syllable,Choseong,Jungseong,Jongseong\n";
                currentMatrixData.forEach(row => {
                    content += `"${row.syllable}","${row.choseongHex}","${row.jungseongHex}","${row.jongseongHex}"\n`;
                });
            } else {
                content = currentMatrixData.map(r => r.syllable).join(" ");
            }

            const blob = new Blob([content], { type: 'text/plain;charset=utf-8' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = filename;
            link.click();
        }

        function copyMatrixText() {
            const text = currentMatrixData.map(r => r.syllable).join(" ");
            copyText(text);
            showToast("Copied full matrix text to clipboard!");
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');

            document.querySelectorAll('nav button').forEach(btn => {
                btn.className = "px-4 py-2 text-sm font-medium rounded-t-lg transition-colors text-slate-600 hover:text-slate-900 hover:bg-slate-100 flex items-center gap-2 whitespace-nowrap";
            });

            const activeBtn = document.getElementById(`tab-btn-${tabId}`);
            activeBtn.className = "px-4 py-2 text-sm font-medium rounded-t-lg transition-colors border-b-2 border-indigo-600 text-indigo-600 bg-white shadow-sm flex items-center gap-2 whitespace-nowrap";

            if (tabId === 'random' && document.getElementById('random-output-grid').children.length === 0) {
                generateRandomSyllables();
            }
            if (tabId === 'matrix' && currentMatrixData.length === 0) {
                updateMatrixPresets();
            }
        }

        function switchJamoCategory(category) {
            activeState.category = category;
            
            ['choseong', 'jungseong', 'jongseong'].forEach(cat => {
                const btn = document.getElementById(`jamo-cat-${cat}`);
                if (cat === category) {
                    btn.className = "px-3 py-1.5 text-xs font-bold rounded-lg bg-indigo-50 text-indigo-700 border border-indigo-200 flex items-center gap-1.5";
                } else {
                    btn.className = "px-3 py-1.5 text-xs font-bold rounded-lg text-slate-600 hover:bg-slate-100 flex items-center gap-1.5";
                }
            });

            renderJamoGrid();
        }

        function setJamoFilter(filter) {
            activeState.filter = filter;
            ['all', 'modern', 'archaic'].forEach(f => {
                const btn = document.getElementById(`filter-${f}`);
                if (f === filter) {
                    btn.className = "px-3 py-1 text-xs font-medium rounded-md bg-white shadow-xs text-slate-800";
                } else {
                    btn.className = "px-3 py-1 text-xs font-medium rounded-md text-slate-600 hover:text-slate-900";
                }
            });
            renderJamoGrid();
        }

        function capitalize(str) {
            return str.charAt(0).toUpperCase() + str.slice(1);
        }

        function copyCurrentSyllable() {
            const text = document.getElementById('main-syllable-display').innerText;
            copyText(text);
            showToast(`Copied "${text}"`);
        }

        function copyText(text) {
            const textarea = document.createElement('textarea');
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);
        }

        function showToast(msg) {
            const toast = document.getElementById('copy-toast');
            toast.innerText = msg;
            toast.classList.remove('opacity-0');
            toast.classList.add('opacity-100');
            setTimeout(() => {
                toast.classList.remove('opacity-100');
                toast.classList.add('opacity-0');
            }, 2000);
        }

        // Initialize Application on Window Load
        window.onload = function() {
            initJamoData();
            renderJamoGrid();
            updateSyllableDisplay();
        };
    </script>
</body>
</html>
