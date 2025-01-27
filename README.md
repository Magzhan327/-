<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Audi Dealership System</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 8px;
            text-align: left;
        }
        .form-group {
            margin-bottom: 10px;
        }
        input[type="text"], input[type="number"] {
            padding: 5px;
            width: 200px;
        }
        button {
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        img {
            width: 100px;
            height: auto;
            margin-top: 5px;
        }
    </style>
</head>
<body>

    <h1>Audi Dealership Management System</h1>

    <div>
        <h2>Add a new car</h2>
        <div class="form-group">
            <label for="model">Model:</label>
            <input type="text" id="model" placeholder="Enter model">
        </div>
        <div class="form-group">
            <label for="year">Year:</label>
            <input type="number" id="year" placeholder="Enter year">
        </div>
        <div class="form-group">
            <label for="price">Price:</label>
            <input type="number" id="price" placeholder="Enter price">
        </div>
        <div class="form-group">
            <label for="image">Car Image:</label>
            <input type="file" id="image" accept="image/*">
        </div>
        <button onclick="addCar()">Add Car</button>
    </div>

    <h2>Cars in Inventory</h2>
    <table id="carsTable">
        <thead>
            <tr>
                <th>ID</th>
                <th>Model</th>
                <th>Year</th>
                <th>Price</th>
                <th>Image</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <!-- Cars will be displayed here -->
        </tbody>
    </table>

    <script>
        const ACCESS_TOKEN = "audi123"; // Токен для доступа

        // Проверяем токен в URL
        function checkToken() {
            const urlParams = new URLSearchParams(window.location.search);
            const token = urlParams.get('token');
            if (token !== ACCESS_TOKEN) {
                // Если токен неверный, редиректим обратно на страницу входа
                window.location.href = "login.html";
            }
        }

        // Загрузка автомобилей из localStorage
        function loadCars() {
            const cars = JSON.parse(localStorage.getItem('cars')) || [];
            const tableBody = document.getElementById('carsTable').getElementsByTagName('tbody')[0];
            tableBody.innerHTML = '';

            cars.forEach((car, index) => {
                const row = tableBody.insertRow();
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${car.model}</td>
                    <td>${car.year}</td>
                    <td>${car.price}</td>
                    <td><img src="${car.image}" alt="${car.model}"></td>
                    <td><button onclick="deleteCar(${index})">Delete</button></td>
                `;
            });
        }

        // Добавление нового автомобиля
        function addCar() {
            const model = document.getElementById('model').value;
            const year = document.getElementById('year').value;
            const price = document.getElementById('price').value;
            const imageFile = document.getElementById('image').files[0];

            if (!model || !year || !price) {
                alert('Please fill in all fields.');
                return;
            }

            let imageBase64 = "";
            if (imageFile) {
                const reader = new FileReader();
                reader.onloadend = function() {
                    imageBase64 = reader.result;
                    saveCarData(model, year, price, imageBase64);
                };
                reader.readAsDataURL(imageFile);
            } else {
                saveCarData(model, year, price, imageBase64);
            }
        }

        // Сохранение данных автомобиля в localStorage
        function saveCarData(model, year, price, imageBase64) {
            const cars = JSON.parse(localStorage.getItem('cars')) || [];
            cars.push({ model, year, price, image: imageBase64 });
            localStorage.setItem('cars', JSON.stringify(cars));

            // Очистка полей формы
            document.getElementById('model').value = '';
            document.getElementById('year').value = '';
            document.getElementById('price').value = '';
            document.getElementById('image').value = '';

            loadCars();
        }

        // Удаление автомобиля из localStorage
        function deleteCar(index) {
            const cars = JSON.parse(localStorage.getItem('cars')) || [];
            cars.splice(index, 1);
            localStorage.setItem('cars', JSON.stringify(cars));
            loadCars();
        }

        // Загружаем список автомобилей при загрузке страницы
        window.onload = function() {
            checkToken();
            loadCars();
        }
    </script>

</body>
</html>


