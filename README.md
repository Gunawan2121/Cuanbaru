<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prediksi Angka</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }

        .container {
            margin: 20px auto;
            max-width: 600px;
            position: relative;
        }

        .table-container {
            max-height: 400px; /* Adjust height as needed */
            overflow-y: auto;
            border: 1px solid #ddd;
            padding: 0;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: center;
        }

        th, td {
            padding: 4px; /* Adjust padding to make columns tighter */
            border: 1px solid #ddd;
            white-space: nowrap;
        }

        input {
            width: 100%;
            padding: 3px;
            text-align: center;
            box-sizing: border-box;
        }

        .correct {
            background-color: #dff0d8; /* Light green background for correct predictions */
            color: #d9534f; /* Text color for correct values (red) */
        }

        .scroll-button {
            margin-top: 10px;
            padding: 10px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .scroll-button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="table-container" id="tableContainer">
            <table id="predictionTable">
                <thead>
                    <tr>
                        <th>Prediksi</th>
                        <th>Yes/No</th>
                        <th>Input</th>
                        <th>Poin</th>
                        <th>B/S</th>
                        <th>E/O</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Rows will be added here by JavaScript -->
                </tbody>
            </table>
        </div>
        <button class="scroll-button" onclick="scrollToNextPage()">Buat tabel baru</button>
    </div>

    <script>
        let currentPage = 0;
        const rowsPerPage = 10;

        function calculate(currentElement) {
            const row = currentElement.parentElement.parentElement;
            const inputNumber = row.querySelector('.inputNumber').value;
            let total = 0;

            for (let digit of inputNumber) {
                if (!isNaN(digit)) {
                    total += parseInt(digit);
                }
            }

            let bigSmallResult = '';
            let oddEvenResult = '';

            if (total >= 11 && total <= 18) {
                bigSmallResult = 'Big';
            } else if (total >= 3 && total <= 10) {
                bigSmallResult = 'Small';
            }

            if ([3, 5, 7, 9, 11, 13, 15, 17].includes(total)) {
                oddEvenResult = 'Odd';
            } else if ([4, 6, 8, 10, 12, 14, 16, 18].includes(total)) {
                oddEvenResult = 'Even';
            }

            const totalElement = row.querySelector('.total');
            totalElement.value = `${total}`;

            const bigSmallElement = row.querySelector('.bigSmall');
            const oddEvenElement = row.querySelector('.oddEven');
            bigSmallElement.value = bigSmallResult;
            oddEvenElement.value = oddEvenResult;

            const predictionElement = row.querySelector('.prediction');
            const prediction = predictionElement.value.toLowerCase();

            const yesNoElement = row.querySelector('.yesNo');

            const isBigSmallCorrect = prediction === bigSmallResult.charAt(0).toLowerCase();
            const isOddEvenCorrect = prediction === oddEvenResult.charAt(0).toLowerCase();

            if (isBigSmallCorrect || isOddEvenCorrect) {
                yesNoElement.value = 'Yes';
                predictionElement.classList.add('correct');
                bigSmallElement.classList.add('correct');
                oddEvenElement.classList.add('correct');
                yesNoElement.classList.add('correct');
                totalElement.classList.add('correct');
                row.querySelector('.inputNumber').classList.add('correct');
            } else {
                yesNoElement.value = 'No';
                predictionElement.classList.remove('correct');
                bigSmallElement.classList.remove('correct');
                oddEvenElement.classList.remove('correct');
                yesNoElement.classList.remove('correct');
                totalElement.classList.remove('correct');
                row.querySelector('.inputNumber').classList.remove('correct');
            }
        }

        function addNewRow() {
            const tableBody = document.querySelector('#predictionTable tbody');
            const newRow = document.createElement('tr');

            newRow.innerHTML = `
                <td><input type="text" class="prediction" maxlength="1"></td>
                <td><input type="text" class="yesNo" disabled></td>
                <td><input type="text" class="inputNumber" maxlength="3" oninput="calculate(this)"></td>
                <td><input type="text" class="total" disabled></td>
                <td><input type="text" class="bigSmall" disabled></td>
                <td><input type="text" class="oddEven" disabled></td>
            `;

            tableBody.appendChild(newRow);
        }

        function scrollToNextPage() {
            const tableBody = document.querySelector('#predictionTable tbody');
            const rows = tableBody.getElementsByTagName('tr');

            if (currentPage === 0) {
                // Initialize with 10 rows
                for (let i = 0; i < rowsPerPage; i++) {
                    addNewRow();
                }
                currentPage++;
                return;
            }

            // Add new rows for the next page
            for (let i = 0; i < rowsPerPage; i++) {
                addNewRow();
            }
            currentPage++;

            // Scroll to the bottom of the table to reveal new rows
            const tableContainer = document.getElementById('tableContainer');
            tableContainer.scrollTop = tableContainer.scrollHeight;
        }

        // Initialize with one row
        addNewRow();
    </script>
</body>
</html>
</html>