<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Result Manager (Professional)</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- PDF Generation Libraries -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f2f5;
            background-image: url('https://images.unsplash.com/photo-1481627834876-b7833e8f5570?q=80&w=2728&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .main-container {
            background-color: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(8px);
        }
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-color: rgba(0, 0, 0, 0.6);
            display: flex; justify-content: center; align-items: center;
            z-index: 1000; opacity: 0; visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        .modal-overlay.active { opacity: 1; visibility: visible; }
        .modal-container {
            background-color: white;
            padding: 2rem;
            border-radius: 0.75rem;
            box-shadow: 0 10px 25px -5px rgba(0,0,0,0.1), 0 8px 10px -6px rgba(0,0,0,0.1);
            max-height: 90vh;
            overflow-y: auto;
            transform: scale(0.95);
            transition: transform 0.3s ease;
        }
        .modal-overlay.active .modal-container { transform: scale(1); }
    </style>
</head>
<body class="bg-slate-100 flex items-center justify-center min-h-screen p-4 sm:p-6">

    <div class="w-full max-w-7xl mx-auto main-container rounded-2xl shadow-xl p-6 sm:p-8">
        
        <!-- Header Section -->
        <div class="flex flex-col md:flex-row items-start md:items-center justify-between mb-8 pb-4 border-b border-gray-200">
            <div class="flex items-center gap-4">
                <div class="bg-blue-100 p-3 rounded-full">
                    <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-blue-600"><path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20"></path></svg>
                </div>
                <div>
                    <h1 class="text-2xl font-bold text-gray-800">IBAGRADS XI-XII</h1>
                    <p class="text-sm text-gray-500">Student Result Management System</p>
                </div>
            </div>
            <div class="text-left md:text-right mt-4 md:mt-0">
                <p id="currentDate" class="font-medium text-gray-700"></p>
                <p class="text-sm text-gray-500">karachi sindh</p>
            </div>
        </div>
        
        <!-- Form & Subject Management in a Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-5 gap-8 mb-8">
            <div class="lg:col-span-3 bg-gray-50 p-6 rounded-xl border border-gray-200">
                <h2 class="text-xl font-semibold text-gray-700 mb-4">ADD NEW RESULTS</h2>
                <form id="resultForm" class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                        <label for="studentName" class="block text-sm font-medium text-gray-600 mb-1">Student ka Naam</label>
                        <input type="text" id="studentName" placeholder="Jaise: Anil Kumar" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                    </div>
                    <div>
                        <label for="studentClass" class="block text-sm font-medium text-gray-600 mb-1">Class</label>
                        <select id="studentClass" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                            <option value="" disabled selected>Select Class</option>
                            <option value="XI">XI</option>
                            <option value="XII">XII</option>
                        </select>
                    </div>
                     <div>
                        <label for="degree" class="block text-sm font-medium text-gray-600 mb-1">Program</label>
                        <select id="degree" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                            <option value="" disabled selected>Select Program</option>
                            <option value="Pre Med">Pre-Medical</option>
                            <option value="Pre Eng">Pre-Engineering</option>
                            <option value="CS">Computer Science</option>
                        </select>
                    </div>
                    <div>
                        <label for="subject" class="block text-sm font-medium text-gray-600 mb-1">Subject</label>
                        <select id="subject" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                            <option value="" disabled selected>Select a Subject</option>
                        </select>
                    </div>
                    <div>
                        <label for="testType" class="block text-sm font-medium text-gray-600 mb-1">Test Type</label>
                        <select id="testType" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                            <option value="" disabled selected>Select Type</option>
                            <option value="Weekly Test">Weekly Test</option>
                            <option value="Monthly Test">Monthly Test</option>
                            <option value="Yearly Test">Yearly Test</option>
                        </select>
                    </div>
                    <div>
                        <label for="topicName" class="block text-sm font-medium text-gray-600 mb-1">Topic ka Naam</label>
                        <input type="text" id="topicName" placeholder="Jaise: Algebra" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                    </div>
                    <div>
                        <label for="score" class="block text-sm font-medium text-gray-600 mb-1">Score</label>
                        <input type="number" id="score" placeholder="Jaise: 85" min="0" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                    </div>
                    <div>
                        <label for="totalMarks" class="block text-sm font-medium text-gray-600 mb-1">Total Marks</label>
                        <input type="number" id="totalMarks" value="100" min="1" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                    </div>
                    <div>
                        <label for="resultDate" class="block text-sm font-medium text-gray-600 mb-1">Date</label>
                        <input type="date" id="resultDate" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition" required>
                    </div>
                    <div class="md:col-span-3 mt-2">
                        <button type="submit" class="w-full bg-blue-600 text-white font-semibold py-2.5 px-4 rounded-lg hover:bg-blue-700 flex items-center justify-center gap-2 transition duration-300">
                            <i data-lucide="plus-circle" class="w-5 h-5"></i> Add Result
                        </button>
                    </div>
                </form>
                <p id="errorMessage" class="text-red-500 text-sm mt-2 hidden">Please sabhi fields bharein.</p>
            </div>

            <div class="lg:col-span-2 bg-gray-50 p-6 rounded-xl border border-gray-200">
                <h2 class="text-xl font-semibold text-gray-700 mb-4">Subjects Manage Karein</h2>
                <div class="space-y-4">
                    <input type="text" id="newSubjectInput" placeholder="Naya Subject Add Karein" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition">
                    <div class="flex flex-col sm:flex-row gap-2">
                        <button id="addSubjectBtn" class="w-full bg-green-500 text-white font-semibold py-2 px-4 rounded-lg hover:bg-green-600 flex items-center justify-center gap-2 transition duration-300">
                            <i data-lucide="plus" class="w-5 h-5"></i> Add
                        </button>
                        <button id="removeSubjectBtn" class="w-full bg-red-500 text-white font-semibold py-2 px-4 rounded-lg hover:bg-red-600 flex items-center justify-center gap-2 transition duration-300">
                           <i data-lucide="trash-2" class="w-5 h-5"></i> Remove
                        </button>
                    </div>
                </div>
                 <p id="subjectError" class="text-red-600 text-sm mt-2 hidden"></p>
            </div>
        </div>

        <!-- Results Table Section -->
        <div class="overflow-x-auto">
            <div class="flex flex-col md:flex-row justify-between items-center mb-4 gap-4">
                <h2 class="text-xl font-semibold text-gray-700">Uploaded Results</h2>
                <div class="flex flex-wrap items-center gap-2">
                    <input type="text" id="searchInput" placeholder="Student ke naam se khojein..." class="w-full md:w-auto px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 transition">
                     <div id="testTypeFilters" class="flex items-center gap-2 bg-gray-100 p-1 rounded-lg">
                        <button data-filter="All" class="filter-btn bg-blue-500 text-white font-semibold py-1 px-3 rounded-md transition">All</button>
                        <button data-filter="Weekly Test" class="filter-btn bg-gray-200 text-gray-700 font-semibold py-1 px-3 rounded-md transition">Weekly</button>
                        <button data-filter="Monthly Test" class="filter-btn bg-gray-200 text-gray-700 font-semibold py-1 px-3 rounded-md transition">Monthly</button>
                        <button data-filter="Yearly Test" class="filter-btn bg-gray-200 text-gray-700 font-semibold py-1 px-3 rounded-md transition">Yearly</button>
                    </div>
                    <button id="showFinalResultBtn" class="bg-indigo-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-indigo-700 transition flex items-center gap-2"><i data-lucide="award" class="w-5 h-5"></i> Final</button>
                    <button id="exportPdfBtn" class="bg-red-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-red-700 transition flex items-center gap-2"><i data-lucide="file-text" class="w-5 h-5"></i> PDF</button>
                    <button id="exportExcelBtn" class="bg-green-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-green-700 transition flex items-center gap-2"><i data-lucide="file-spreadsheet" class="w-5 h-5"></i> Excel</button>
                </div>
            </div>

            <table id="resultsTable" class="min-w-full bg-white rounded-lg shadow-md overflow-hidden">
                <thead class="bg-gray-100 text-gray-600">
                    <tr>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Student Naam</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Class</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Program</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Test Type</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Subject</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Score</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Date</th>
                        <th class="text-center py-3 px-4 uppercase font-semibold text-sm">Actions</th>
                    </tr>
                </thead>
                <tbody id="resultsTableBody" class="text-gray-700 divide-y divide-gray-200"></tbody>
            </table>
            <div id="noResultsMessage" class="text-center py-10 text-gray-500 hidden"><p>Abhi tak koi result upload nahi hua hai.</p></div>
        </div>
    </div>

    <!-- Modals -->
    <div id="cardModal" class="modal-overlay"><div id="cardModalContainer" class="modal-container w-full max-w-2xl"></div></div>
    <div id="editModal" class="modal-overlay"><div id="editModalContainer" class="modal-container w-full max-w-lg"></div></div>
    <div id="finalResultModal" class="modal-overlay"><div id="finalResultModalContainer" class="modal-container w-full max-w-5xl"></div></div>
    
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons(); // Initialize icons
            
            // Set current date in header
            document.getElementById('currentDate').textContent = new Date().toLocaleDateString('en-GB', { day: 'numeric', month: 'long', year: 'numeric' });

            const resultForm = document.getElementById('resultForm');
            const resultsTableBody = document.getElementById('resultsTableBody');
            const noResultsMessage = document.getElementById('noResultsMessage');
            const searchInput = document.getElementById('searchInput'); 
            const subjectSelect = document.getElementById('subject');
            const newSubjectInput = document.getElementById('newSubjectInput');
            const addSubjectBtn = document.getElementById('addSubjectBtn');
            const removeSubjectBtn = document.getElementById('removeSubjectBtn');
            const subjectError = document.getElementById('subjectError');
            const testTypeFilters = document.getElementById('testTypeFilters');

            let currentResults = [];
            let subjects = [];
            let currentFilter = 'All';

            // --- Data Persistence ---
            const saveResultsToLocal = () => localStorage.setItem('studentResults', JSON.stringify(currentResults));
            const loadResultsFromLocal = () => currentResults = JSON.parse(localStorage.getItem('studentResults')) || [];
            const saveSubjectsToLocal = () => localStorage.setItem('studentSubjects', JSON.stringify(subjects));
            const loadSubjectsFromLocal = () => {
                subjects = JSON.parse(localStorage.getItem('studentSubjects')) || ["Physics", "Chemistry", "Maths", "English", "Biology"];
                if (subjects.length === 0) subjects = ["Physics", "Chemistry", "Maths", "English", "Biology"];
            };

            // --- Utilities ---
            const showModal = (modalId) => document.getElementById(modalId).classList.add('active');
            const hideModal = (modalId) => document.getElementById(modalId).classList.remove('active');
            const calculateGrade = (p) => {
                if (p >= 90) return 'A+'; if (p >= 80) return 'A'; if (p >= 70) return 'B';
                if (p >= 60) return 'C'; if (p >= 50) return 'D'; return 'F';
            };
            const getRemarks = (grade) => {
                const remarks = { 'A+': 'Outstanding Performance.', 'A': 'Excellent Work.', 'B': 'Good Effort.', 'C': 'Satisfactory.', 'D': 'Needs Improvement.', 'F': 'Requires Serious Attention.' };
                return remarks[grade] || 'N/A';
            };
            const getFilteredResults = () => {
                const searchTerm = searchInput.value.toLowerCase();
                const filteredByType = currentFilter === 'All' ? currentResults : currentResults.filter(r => r.testType === currentFilter);
                return filteredByType.filter(r => r.studentName.toLowerCase().includes(searchTerm));
            };


            // --- Rendering ---
            const renderResults = () => {
                resultsTableBody.innerHTML = '';
                const displayResults = getFilteredResults();
                const sorted = [...displayResults].sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
                sorted.forEach(data => {
                    const row = resultsTableBody.insertRow();
                    row.dataset.id = data.id;
                    row.dataset.studentName = data.studentName.toLowerCase();
                    const percentage = data.totalMarks > 0 ? ((data.score / data.totalMarks) * 100).toFixed(2) : 'N/A';
                    row.innerHTML = `
                        <td class="py-3 px-4 font-medium">${data.studentName}</td>
                        <td class="py-3 px-4">${data.studentClass}</td>
                        <td class="py-3 px-4">${data.degree}</td>
                        <td class="py-3 px-4">${data.testType}</td>
                        <td class="py-3 px-4">${data.subject}</td>
                        <td class="py-3 px-4">${data.score} / ${data.totalMarks} (${percentage}%)</td>
                        <td class="py-3 px-4">${new Date(data.resultDate).toLocaleDateString()}</td>
                        <td class="py-3 px-4">
                            <div class="flex items-center justify-center space-x-2">
                                <button data-id="${data.id}" class="view-card-btn p-2 text-blue-600 hover:bg-blue-100 rounded-full transition"><i data-lucide="eye" class="w-5 h-5"></i></button>
                                <button data-id="${data.id}" class="edit-btn p-2 text-yellow-600 hover:bg-yellow-100 rounded-full transition"><i data-lucide="pencil" class="w-5 h-5"></i></button>
                                <button data-id="${data.id}" class="delete-btn p-2 text-red-600 hover:bg-red-100 rounded-full transition"><i data-lucide="trash-2" class="w-5 h-5"></i></button>
                            </div>
                        </td>`;
                });
                lucide.createIcons();
                noResultsMessage.classList.toggle('hidden', displayResults.length > 0);
            };

            const populateSubjectDropdowns = () => {
                const editSubjectEl = document.getElementById('editSubject');
                [subjectSelect, editSubjectEl].forEach(selectEl => {
                    if (!selectEl) return;
                    selectEl.innerHTML = '<option value="" disabled selected>Select</option>';
                    subjects.forEach(s => selectEl.innerHTML += `<option value="${s}">${s}</option>`);
                });
            };

            // --- Subject Management ---
            addSubjectBtn.addEventListener('click', () => {
                const newSubject = newSubjectInput.value.trim();
                if (!newSubject || subjects.find(s => s.toLowerCase() === newSubject.toLowerCase())) {
                    subjectError.textContent = newSubject ? "Subject already exists." : "Please enter a subject name.";
                    subjectError.classList.remove('hidden');
                    return;
                }
                subjects.push(newSubject);
                saveSubjectsToLocal();
                populateSubjectDropdowns();
                newSubjectInput.value = '';
                subjectError.classList.add('hidden');
            });

            removeSubjectBtn.addEventListener('click', () => {
                const selected = subjectSelect.value;
                if (!selected) {
                    subjectError.textContent = "Please select a subject to remove.";
                    subjectError.classList.remove('hidden');
                    return;
                }
                subjects = subjects.filter(s => s !== selected);
                saveSubjectsToLocal();
                populateSubjectDropdowns();
                subjectError.classList.add('hidden');
            });
            
            // --- Add/Edit Result Forms ---
            resultForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const newResult = {
                    id: crypto.randomUUID(),
                    studentName: document.getElementById('studentName').value.trim(),
                    studentClass: document.getElementById('studentClass').value,
                    degree: document.getElementById('degree').value,
                    testType: document.getElementById('testType').value,
                    subject: subjectSelect.value,
                    topicName: document.getElementById('topicName').value.trim(),
                    score: parseInt(document.getElementById('score').value, 10),
                    totalMarks: parseInt(document.getElementById('totalMarks').value, 10),
                    resultDate: document.getElementById('resultDate').value,
                    timestamp: new Date().toISOString()
                };
                currentResults.push(newResult);
                saveResultsToLocal();
                renderResults();
                resultForm.reset();
                document.getElementById('studentClass').selectedIndex = 0;
                document.getElementById('degree').selectedIndex = 0;
                document.getElementById('testType').selectedIndex = 0;
                subjectSelect.selectedIndex = 0;
            });

            // --- Table Actions (View, Edit, Delete) ---
            resultsTableBody.addEventListener('click', (e) => {
                const btn = e.target.closest('button');
                if (!btn) return;
                const id = btn.dataset.id;
                
                if (btn.classList.contains('delete-btn')) {
                    if (confirm("Kya aap is result ko delete karna chahte hain?")) {
                        currentResults = currentResults.filter(r => r.id !== id);
                        saveResultsToLocal();
                        renderResults();
                    }
                } else if (btn.classList.contains('view-card-btn')) {
                    const data = currentResults.find(r => r.id === id);
                    if (data) showResultCard(data);
                } else if (btn.classList.contains('edit-btn')) {
                    const data = currentResults.find(r => r.id === id);
                    if (data) showEditModal(data);
                }
            });

            // --- Professional Result Card ---
            const createProfessionalCardHTML = (data) => {
                const percentage = data.totalMarks > 0 ? ((data.score / data.totalMarks) * 100).toFixed(2) : 0;
                const grade = calculateGrade(percentage);
                const issueDate = new Date().toLocaleDateString('en-GB', { day: 'numeric', month: 'long', year: 'numeric' });
                return `
                <div class="p-8 border-2 border-gray-800 bg-white relative">
                    <div class="absolute inset-0 flex items-center justify-center z-0">
                         <svg xmlns="http://www.w3.org/2000/svg" width="160" height="160" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="0.5" stroke-linecap="round" stroke-linejoin="round" class="text-gray-200 opacity-50"><path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20"></path></svg>
                    </div>
                    <div class="relative z-10">
                        <div class="flex items-center justify-between pb-4 border-b-2 border-gray-800">
                            <div class="flex items-center gap-3">
                                <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-blue-600"><path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20"></path></svg>
                                <div>
                                    <h3 class="text-xl font-bold text-gray-800">IBAGRADS XI-XII</h3>
                                    <p class="text-sm font-medium text-gray-500">OFFICIAL RESULT CARD</p>
                                </div>
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-x-8 gap-y-4 my-6 text-sm">
                            <p class="col-span-2"><strong>Student Name:</strong> ${data.studentName}</p>
                            <p><strong>Class:</strong> ${data.studentClass}</p>
                            <p><strong>Program:</strong> ${data.degree}</p>
                             <p><strong>Test Type:</strong> ${data.testType}</p>
                            <p><strong>Date of Issue:</strong> ${issueDate}</p>
                            <p><strong>Result ID:</strong> ${data.id.substring(0, 8).toUpperCase()}</p>
                        </div>
                        <table class="w-full text-sm border-collapse">
                            <thead class="bg-gray-100 text-gray-600">
                                <tr>
                                    <th class="border p-2 text-left">Subject</th>
                                    <th class="border p-2 text-left">Topic</th>
                                    <th class="border p-2 text-center">Score</th>
                                    <th class="border p-2 text-center">Total Marks</th>
                                    <th class="border p-2 text-center">Percentage</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <td class="border p-2">${data.subject}</td>
                                    <td class="border p-2">${data.topicName}</td>
                                    <td class="border p-2 text-center">${data.score}</td>
                                    <td class="border p-2 text-center">${data.totalMarks}</td>
                                    <td class="border p-2 text-center font-semibold">${percentage}%</td>
                                </tr>
                            </tbody>
                        </table>
                        <div class="grid grid-cols-3 gap-4 mt-6 text-center bg-gray-50 p-4 rounded-lg">
                            <div><p class="text-sm text-gray-600">Overall Percentage</p><p class="text-2xl font-bold text-blue-600">${percentage}%</p></div>
                            <div><p class="text-sm text-gray-600">Grade</p><p class="text-2xl font-bold text-blue-600">${grade}</p></div>
                            <div><p class="text-sm text-gray-600">Remarks</p><p class="text-lg font-semibold text-blue-600">${getRemarks(grade)}</p></div>
                        </div>
                        <div class="mt-12 pt-4 border-t text-center text-xs text-gray-500">
                            <p>This is a computer-generated document. | IBAGRADS XI-XII, karachi sindh, Pakistan</p>
                        </div>
                    </div>
                </div>`;
            };

            const showResultCard = (data) => {
                const container = document.getElementById('cardModalContainer');
                container.innerHTML = `
                    <div id="printableCard">${createProfessionalCardHTML(data)}</div>
                    <div class="flex justify-end space-x-2 mt-4">
                        <button id="exportCardPdfBtn" class="bg-blue-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-blue-700 transition">Export PDF</button>
                        <button id="closeCardModalBtn" class="bg-gray-300 text-gray-800 font-semibold py-2 px-4 rounded-lg hover:bg-gray-400 transition">Band Karein</button>
                    </div>`;
                document.getElementById('closeCardModalBtn').onclick = () => hideModal('cardModal');
                document.getElementById('exportCardPdfBtn').onclick = () => exportCardToPdf(document.getElementById('printableCard'), `${data.studentName}_result.pdf`);
                showModal('cardModal');
            };

            // --- Edit Modal ---
            const showEditModal = (data) => {
                const container = document.getElementById('editModalContainer');
                container.innerHTML = `
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">Result Edit Karein</h2>
                <form id="editForm">
                    <input type="hidden" id="editDocId" value="${data.id}">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div><label for="editStudentName" class="block text-sm font-medium">Naam</label><input type="text" id="editStudentName" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" value="${data.studentName}" required></div>
                        <div><label for="editStudentClass" class="block text-sm font-medium">Class</label><select id="editStudentClass" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" required><option value="XI">XI</option><option value="XII">XII</option></select></div>
                        <div><label for="editDegree" class="block text-sm font-medium">Program</label><select id="editDegree" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" required><option value="Pre Med">Pre-Medical</option><option value="Pre Eng">Pre-Engineering</option><option value="CS">Computer Science</option></select></div>
                        <div>
                            <label for="editTestType" class="block text-sm font-medium">Test Type</label>
                            <select id="editTestType" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" required>
                                <option value="Weekly Test">Weekly Test</option>
                                <option value="Monthly Test">Monthly Test</option>
                                <option value="Yearly Test">Yearly Test</option>
                            </select>
                        </div>
                        <div><label for="editSubject" class="block text-sm font-medium">Subject</label><select id="editSubject" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" required></select></div>
                        <div><label for="editTopicName" class="block text-sm font-medium">Topic</label><input type="text" id="editTopicName" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" value="${data.topicName}" required></div>
                        <div><label for="editScore" class="block text-sm font-medium">Score</label><input type="number" id="editScore" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" value="${data.score}" required></div>
                        <div><label for="editTotalMarks" class="block text-sm font-medium">Total Marks</label><input type="number" id="editTotalMarks" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" value="${data.totalMarks}" required></div>
                        <div class="col-span-2"><label for="editResultDate" class="block text-sm font-medium">Date</label><input type="date" id="editResultDate" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg" value="${data.resultDate}" required></div>
                    </div>
                    <div class="flex justify-end space-x-2 mt-6">
                        <button type="submit" class="bg-blue-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-blue-700">Save Changes</button>
                        <button type="button" id="closeEditModalBtn" class="bg-gray-300 text-gray-800 font-semibold py-2 px-4 rounded-lg hover:bg-gray-400">Cancel</button>
                    </div>
                </form>`;

                populateSubjectDropdowns(); // Populate subjects in edit form
                document.getElementById('editStudentClass').value = data.studentClass;
                document.getElementById('editDegree').value = data.degree;
                document.getElementById('editTestType').value = data.testType;
                document.getElementById('editSubject').value = data.subject;
                
                document.getElementById('closeEditModalBtn').onclick = () => hideModal('editModal');
                document.getElementById('editForm').onsubmit = (e) => {
                    e.preventDefault();
                    const id = document.getElementById('editDocId').value;
                    const index = currentResults.findIndex(r => r.id === id);
                    if (index > -1) {
                        currentResults[index] = { ...currentResults[index],
                            studentName: document.getElementById('editStudentName').value,
                            studentClass: document.getElementById('editStudentClass').value,
                            degree: document.getElementById('editDegree').value,
                            testType: document.getElementById('editTestType').value,
                            subject: document.getElementById('editSubject').value,
                            topicName: document.getElementById('editTopicName').value,
                            score: parseInt(document.getElementById('editScore').value, 10),
                            totalMarks: parseInt(document.getElementById('editTotalMarks').value, 10),
                            resultDate: document.getElementById('editResultDate').value,
                        };
                        saveResultsToLocal();
                        renderResults();
                        hideModal('editModal');
                    }
                };
                showModal('editModal');
            };

            // --- Final Results Modal ---
            document.getElementById('showFinalResultBtn').addEventListener('click', () => {
                const container = document.getElementById('finalResultModalContainer');
                container.innerHTML = `
                    <div id="printableFinalResults">
                        <div class="flex flex-col sm:flex-row justify-between items-center mb-4">
                             <h2 class="text-2xl font-semibold text-gray-700">Final Consolidated Results</h2>
                             <div id="finalResultFilters" class="flex justify-start space-x-2">
                                <button data-filter="XI" class="filter-btn bg-gray-500 text-white font-semibold py-1 px-3 rounded-lg hover:bg-gray-600 transition">XI</button>
                                <button data-filter="XII" class="filter-btn bg-gray-500 text-white font-semibold py-1 px-3 rounded-lg hover:bg-gray-600 transition">XII</button>
                                <button data-filter="All" class="filter-btn bg-blue-500 text-white font-semibold py-1 px-3 rounded-lg hover:bg-blue-600 transition">All</button>
                            </div>
                        </div>
                        <div class="overflow-x-auto"><table id="finalResultTable" class="min-w-full bg-white rounded-lg shadow-md overflow-hidden"><thead class="bg-gray-100">...</thead><tbody class="divide-y"></tbody></table></div>
                    </div>
                    <div class="flex justify-end space-x-2 mt-6">
                        <button id="exportFinalResultPdfBtn" class="bg-red-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-red-700">Export PDF</button>
                        <button id="closeFinalResultModalBtn" class="bg-gray-300 text-gray-800 font-semibold py-2 px-4 rounded-lg hover:bg-gray-400">Band Karein</button>
                    </div>`;

                const table = container.querySelector('#finalResultTable');
                table.querySelector('thead').innerHTML = `<tr>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Student</th>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Class</th>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Program</th>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Tests Attempted</th>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Total Score</th>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Total Marks</th>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Percentage</th>
                    <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Grade</th></tr>`;

                const renderFinalTable = (filter = 'All') => {
                    const summaries = {};
                    const filtered = currentResults.filter(r => filter === 'All' || r.studentClass === filter);
                    filtered.forEach(r => {
                        const key = `${r.studentName}-${r.studentClass}-${r.degree}`;
                        if (!summaries[key]) {
                            summaries[key] = { name: r.studentName, class: r.studentClass, degree: r.degree, totalScore: 0, totalMarks: 0, testsAttempted: 0 };
                        }
                        summaries[key].totalScore += r.score;
                        summaries[key].totalMarks += r.totalMarks;
                        summaries[key].testsAttempted += 1;
                    });
                    
                    const tbody = table.querySelector('tbody');
                    tbody.innerHTML = '';
                    Object.values(summaries).forEach(s => {
                        const p = s.totalMarks > 0 ? (s.totalScore / s.totalMarks * 100).toFixed(2) : 0;
                        const g = calculateGrade(p);
                        tbody.innerHTML += `<tr>
                            <td class="p-3">${s.name}</td>
                            <td class="p-3">${s.class}</td>
                            <td class="p-3">${s.degree}</td>
                            <td class="p-3">${s.testsAttempted}</td>
                            <td class="p-3">${s.totalScore}</td>
                            <td class="p-3">${s.totalMarks}</td>
                            <td class="p-3 font-semibold">${p}%</td>
                            <td class="p-3 font-bold">${g}</td></tr>`;
                    });
                };

                container.querySelector('#finalResultFilters').onclick = (e) => {
                    if (e.target.matches('.filter-btn')) {
                        const filter = e.target.dataset.filter;
                        renderFinalTable(filter);
                        container.querySelectorAll('.filter-btn').forEach(b => b.classList.replace('bg-blue-500', 'bg-gray-500'));
                        e.target.classList.replace('bg-gray-500', 'bg-blue-500');
                    }
                };

                document.getElementById('closeFinalResultModalBtn').onclick = () => hideModal('finalResultModal');
                document.getElementById('exportFinalResultPdfBtn').onclick = () => exportTableToPdf(document.getElementById('printableFinalResults'), 'final_results.pdf', 'Final Consolidated Results');
                
                renderFinalTable();
                showModal('finalResultModal');
            });
            
            // --- Single Card PDF Export (Fit to page) ---
            const exportCardToPdf = (element, filename) => {
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF({ orientation: 'p', unit: 'mm', format: 'a4' });

                html2canvas(element, { scale: 3, useCORS: true }).then(canvas => { // Increased scale for better quality
                    const imgData = canvas.toDataURL('image/png');
                    const imgProps = doc.getImageProperties(imgData);
                    
                    const pdfWidth = doc.internal.pageSize.getWidth();
                    const pdfHeight = doc.internal.pageSize.getHeight();
                    
                    const margin = 15; // 15mm margin on all sides
                    
                    const availableWidth = pdfWidth - (margin * 2);
                    const availableHeight = pdfHeight - (margin * 2);
                    
                    const imgAspectRatio = imgProps.width / imgProps.height;
                    
                    let finalImgWidth, finalImgHeight;

                    // Calculate final dimensions to fit within the available space while maintaining aspect ratio
                    if (imgAspectRatio > (availableWidth / availableHeight)) {
                        finalImgWidth = availableWidth;
                        finalImgHeight = finalImgWidth / imgAspectRatio;
                    } else {
                        finalImgHeight = availableHeight;
                        finalImgWidth = finalImgHeight * imgAspectRatio;
                    }

                    // Center the image
                    const x = margin + (availableWidth - finalImgWidth) / 2;
                    const y = margin + (availableHeight - finalImgHeight) / 2;

                    doc.addImage(imgData, 'PNG', x, y, finalImgWidth, finalImgHeight);
                    doc.save(filename);
                }).catch(e => console.error("Error creating card PDF:", e));
            };
            
            // --- Multi-page Table PDF Export ---
            const exportTableToPdf = (element, filename, title = 'Student Results') => {
                 const { jsPDF } = window.jspdf;
                 const doc = new jsPDF({ orientation: 'landscape', unit: 'mm', format: 'a4' });
                
                 html2canvas(element, { scale: 2, useCORS: true }).then(canvas => {
                    const imgData = canvas.toDataURL('image/png');
                    const topMargin = 30; // Increased top margin for header
                    const bottomMargin = 15;
                    const horizontalMargin = 15;
                    const pageContentWidth = doc.internal.pageSize.getWidth() - (horizontalMargin * 2);
                    const pageContentHeight = doc.internal.pageSize.getHeight() - topMargin - bottomMargin;
                    
                    const imgHeight = canvas.height * pageContentWidth / canvas.width;
                    let heightLeft = imgHeight;
                    let position = 0; // Position of the image slice

                    doc.deletePage(1); // Start with a fresh document

                    // Add pages
                    while (heightLeft > -pageContentHeight) { // check with negative to handle floating point issues and ensure the last bit is printed
                        doc.addPage();
                        doc.addImage(imgData, 'PNG', horizontalMargin, position, pageContentWidth, imgHeight);
                        heightLeft -= pageContentHeight;
                        position -= doc.internal.pageSize.getHeight();
                    }

                    // Add headers and footers to all pages
                    const addHeadersAndFooters = () => {
                        const pageCount = doc.internal.getNumberOfPages();
                        for (let i = 1; i <= pageCount; i++) {
                            doc.setPage(i);
                            // Header
                            doc.setFontSize(16);
                            doc.setFont('helvetica', 'bold');
                            doc.text('IBAGRADS XI-XII', horizontalMargin, 15);
                            doc.setFontSize(11);
                            doc.setFont('helvetica', 'normal');
                            doc.text(title, horizontalMargin, 21);
                            doc.setLineWidth(0.2);
                            doc.line(horizontalMargin, 25, doc.internal.pageSize.getWidth() - horizontalMargin, 25);
                            
                            // Footer
                            const footerY = doc.internal.pageSize.getHeight() - 10;
                            doc.setFontSize(8);
                            doc.text(`Generated on: ${new Date().toLocaleString()}`, horizontalMargin, footerY);
                            doc.text(`Page ${i} of ${pageCount}`, doc.internal.pageSize.getWidth() - horizontalMargin, footerY, { align: 'right' });
                        }
                    };
                     
                    addHeadersAndFooters();
                    doc.save(filename);
                 }).catch(e => console.error("Error creating PDF:", e));
            };
            
            // --- Excel Export ---
            document.getElementById('exportExcelBtn').addEventListener('click', () => {
                const resultsToExport = getFilteredResults();
                let csv = 'Student Naam,Class,Program,Test Type,Subject,Topic,Score,Total Marks,Percentage,Date\n';
                resultsToExport.forEach(r => {
                    const p = r.totalMarks > 0 ? (r.score / r.totalMarks * 100).toFixed(2) : 'N/A';
                    csv += `"${r.studentName}","${r.studentClass}","${r.degree}","${r.testType}","${r.subject}","${r.topicName}","${r.score}","${r.totalMarks}","${p}%","${r.resultDate}"\n`;
                });
                const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
                const link = document.createElement('a');
                link.href = URL.createObjectURL(blob);
                link.download = `student_results_${currentFilter}.csv`;
                link.click();
            });

            // Main table PDF export
            document.getElementById('exportPdfBtn').addEventListener('click', () => {
                const tempContainer = document.createElement('div');
                const tempTable = document.createElement('table');
                tempContainer.appendChild(tempTable);
                tempContainer.style.width = '1122px'; // A4 landscape width in pixels for html2canvas
                tempTable.className = 'min-w-full bg-white text-sm';
                
                // Clone header and remove the 'Actions' column
                const originalThead = document.getElementById('resultsTable').querySelector('thead');
                const headerClone = originalThead.cloneNode(true);
                headerClone.querySelector('tr').lastElementChild.remove();
                tempTable.appendChild(headerClone);

                const tbody = document.createElement('tbody');
                
                const resultsToExport = getFilteredResults();
                resultsToExport.forEach(data => {
                     const percentage = data.totalMarks > 0 ? ((data.score / data.totalMarks) * 100).toFixed(2) : 'N/A';
                     const row = document.createElement('tr');
                     row.className = 'border-b';
                     row.innerHTML = `
                        <td class="py-2 px-3">${data.studentName}</td>
                        <td class="py-2 px-3">${data.studentClass}</td>
                        <td class="py-2 px-3">${data.degree}</td>
                        <td class="py-2 px-3">${data.testType}</td>
                        <td class="py-2 px-3">${data.subject}</td>
                        <td class="py-2 px-3">${data.score}/${data.totalMarks} (${percentage}%)</td>
                        <td class="py-2 px-3">${new Date(data.resultDate).toLocaleDateString()}</td>`;
                    tbody.appendChild(row);
                });
                tempTable.appendChild(tbody);

                document.body.appendChild(tempContainer); // Must be in DOM for html2canvas
                exportTableToPdf(tempContainer, `results_${currentFilter}.pdf`, `Uploaded Results (${currentFilter})`);
                document.body.removeChild(tempContainer);
            });


            // --- Search and Filter ---
            searchInput.addEventListener('keyup', renderResults);
            testTypeFilters.addEventListener('click', (e) => {
                const btn = e.target.closest('.filter-btn');
                if (!btn) return;
                currentFilter = btn.dataset.filter;
                
                // Update button styles
                testTypeFilters.querySelectorAll('.filter-btn').forEach(b => {
                    b.classList.remove('bg-blue-500', 'text-white');
                    b.classList.add('bg-gray-200', 'text-gray-700');
                });
                btn.classList.add('bg-blue-500', 'text-white');
                btn.classList.remove('bg-gray-200', 'text-gray-700');

                renderResults();
            });

            // --- Initial Load ---
            loadSubjectsFromLocal();
            loadResultsFromLocal();
            populateSubjectDropdowns();
            renderResults();
        });
    </script>
</body>
</html>

