<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Result Uploader</title>
    <!-- Tailwind CSS CDN for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* Using Inter font as the default */
        body {
            font-family: 'Inter', sans-serif;
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-4 sm:p-6">

    <div class="w-full max-w-4xl mx-auto bg-white rounded-2xl shadow-lg p-6 sm:p-8">
        
        <!-- Header Section -->
        <div class="text-center mb-8">
            <h1 class="text-3xl sm:text-4xl font-bold text-gray-800">Student Result Manager</h1>
            <p class="text-gray-500 mt-2">Yahan student ke results add aur manage karein.</p>
        </div>

        <!-- Form to add new result -->
        <div class="bg-gray-50 p-6 rounded-xl border border-gray-200 mb-8">
            <h2 class="text-2xl font-semibold text-gray-700 mb-4">Naya Result Add Karein</h2>
            <form id="resultForm" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4 items-end">
                <!-- Student Name Input -->
                <div>
                    <label for="studentName" class="block text-sm font-medium text-gray-600 mb-1">Student ka Naam</label>
                    <input type="text" id="studentName" placeholder="Jaise: Anil Kumar" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition" required>
                </div>
                <!-- Class Input -->
                <div>
                    <label for="studentClass" class="block text-sm font-medium text-gray-600 mb-1">Class</label>
                    <input type="text" id="studentClass" placeholder="Jaise: 10th" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition" required>
                </div>
                <!-- Subject Input -->
                <div>
                    <label for="subject" class="block text-sm font-medium text-gray-600 mb-1">Subject</label>
                    <input type="text" id="subject" placeholder="Jaise: Maths" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition" required>
                </div>
                <!-- Score Input -->
                <div>
                    <label for="score" class="block text-sm font-medium text-gray-600 mb-1">Score (Out of 100)</label>
                    <input type="number" id="score" placeholder="Jaise: 85" min="0" max="100" class="w-full px-4 py-2 bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition" required>
                </div>
                <!-- Submit Button -->
                <button type="submit" class="w-full bg-blue-600 text-white font-semibold py-2 px-4 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition duration-300 ease-in-out transform hover:scale-105">
                    Add Result
                </button>
            </form>
             <!-- Error Message Display -->
            <p id="errorMessage" class="text-red-500 text-sm mt-2 hidden">Please sabhi fields bharein.</p>
        </div>

        <!-- Results Table -->
        <div class="overflow-x-auto">
            <h2 class="text-2xl font-semibold text-gray-700 mb-4">Uploaded Results</h2>
            <table class="min-w-full bg-white rounded-lg shadow overflow-hidden">
                <thead class="bg-gray-800 text-white">
                    <tr>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Student Naam</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Class</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Subject</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Score</th>
                        <th class="text-left py-3 px-4 uppercase font-semibold text-sm">Action</th>
                    </tr>
                </thead>
                <tbody id="resultsTableBody" class="text-gray-700">
                    <!-- Example Row (can be removed) -->
                    <tr class="border-b border-gray-200 hover:bg-gray-50">
                        <td class="py-3 px-4">Rohan Sharma</td>
                        <td class="py-3 px-4">12th</td>
                        <td class="py-3 px-4">Physics</td>
                        <td class="py-3 px-4">92 / 100</td>
                        <td class="py-3 px-4">
                            <button class="delete-btn bg-red-500 text-white text-xs font-semibold py-1 px-3 rounded-full hover:bg-red-600 transition">Delete</button>
                        </td>
                    </tr>
                </tbody>
            </table>
             <!-- No results message -->
            <div id="noResultsMessage" class="text-center py-10 text-gray-500 hidden">
                <p>Abhi tak koi result upload nahi hua hai.</p>
            </div>
        </div>
    </div>

    <script>
        // Get references to DOM elements
        const resultForm = document.getElementById('resultForm');
        const resultsTableBody = document.getElementById('resultsTableBody');
        const errorMessage = document.getElementById('errorMessage');
        const noResultsMessage = document.getElementById('noResultsMessage');

        // Function to check if table is empty and show/hide message
        function checkTableEmpty() {
            if (resultsTableBody.rows.length === 0) {
                noResultsMessage.classList.remove('hidden');
            } else {
                noResultsMessage.classList.add('hidden');
            }
        }

        // Handle form submission
        resultForm.addEventListener('submit', function(event) {
            // Prevent the default form submission behavior
            event.preventDefault();

            // Get values from the form inputs
            const studentName = document.getElementById('studentName').value.trim();
            const studentClass = document.getElementById('studentClass').value.trim();
            const subject = document.getElementById('subject').value.trim();
            const score = document.getElementById('score').value.trim();

            // Basic validation to ensure no fields are empty
            if (!studentName || !studentClass || !subject || !score) {
                errorMessage.classList.remove('hidden');
                return;
            }
            errorMessage.classList.add('hidden');

            // Create a new table row
            const newRow = document.createElement('tr');
            newRow.classList.add('border-b', 'border-gray-200', 'hover:bg-gray-50');

            // Populate the new row with data
            newRow.innerHTML = `
                <td class="py-3 px-4">${studentName}</td>
                <td class="py-3 px-4">${studentClass}</td>
                <td class="py-3 px-4">${subject}</td>
                <td class="py-3 px-4">${score} / 100</td>
                <td class="py-3 px-4">
                    <button class="delete-btn bg-red-500 text-white text-xs font-semibold py-1 px-3 rounded-full hover:bg-red-600 transition">Delete</button>
                </td>
            `;

            // Add the new row to the table body
            resultsTableBody.appendChild(newRow);

            // Reset the form fields
            resultForm.reset();
            
            // Check if table is empty
            checkTableEmpty();
        });
        
        // Use event delegation to handle delete button clicks
        resultsTableBody.addEventListener('click', function(event) {
            // Check if a delete button was clicked
            if (event.target.classList.contains('delete-btn')) {
                // Get the parent row (tr) of the button and remove it
                const rowToDelete = event.target.closest('tr');
                rowToDelete.remove();
                
                // Check if table is empty after deletion
                checkTableEmpty();
            }
        });

        // Initial check when the page loads
        checkTableEmpty();
    </script>

</body>
</html>
