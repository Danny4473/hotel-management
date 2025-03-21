<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Restaurant Management System</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: 'Roboto', sans-serif; 
            background-image: url('https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8cmVzdGF1cmFudHxlbnwwfHwwfHw%3D&w=1000&q=80');
            background-size: cover;
            background-position: center;
            text-align: center; 
            padding: 20px; 
            margin: 0;
            background-attachment: fixed;
            background-repeat: no-repeat;
            opacity: 0.9;
        }
        .container { 
            width: 90%; 
            max-width: 800px; 
            margin: 0 auto; 
            background: #fff; 
            padding: 20px; 
            border-radius: 10px; 
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.1); 
        }
        button { 
            padding: 10px 20px; 
            margin: 10px; 
            border: none; 
            border-radius: 5px; 
            background: linear-gradient(45deg, #4CAF50, #45a049); 
            color: white; 
            cursor: pointer; 
            font-size: 16px; 
            transition: transform 0.2s ease, box-shadow 0.2s ease; 
        }
        button:hover { 
            transform: scale(1.05); 
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3); 
        }
        input, select { 
            padding: 10px; 
            margin: 5px; 
            width: 90%; 
            border-radius: 5px; 
            border: 1px solid #ccc; 
            box-sizing: border-box;
        }
        .order-list, .employee-list, .inventory-list { 
            text-align: left; 
            margin: 20px; 
        }
        .order-item, .employee-item, .inventory-item { 
            padding: 15px;
            margin-bottom: 10px;
            border: 1px solid #eee;
            border-radius: 5px;
            background-color: #fafafa;
            transition: background-color 0.3s;
        }
        .order-item:hover, .employee-item:hover, .inventory-item:hover {
            background-color: #f1f1f1;
        }
        .order-item, .inventory-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .order-details, .inventory-details {
            flex-grow: 1;
        }
        .employee-info {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }
        .employee-label {
            color: #666;
            font-size: 0.9em;
        }
        .employee-value {
            color: #333;
            font-weight: 500;
        }
        .login-container { 
            width: 300px; 
            margin: 20px auto; 
            background: #fff; 
            padding: 20px; 
            border-radius: 10px; 
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); 
        }
        #orderList, #savedEmployeeDetails, #inventoryList {
            list-style-type: none;
            padding: 0;
        }
        #attendanceMessage, #employeeDetailsMessage {
            margin-top: 10px;
            padding: 10px;
            border-radius: 5px;
            background-color: #f1f1f1;
            text-align: left;
        }
        .error { color: #ff4444; }
        .success { color: #00C851; }
        #loginButton { background: linear-gradient(45deg, #FF416C, #FF4B2B); }
        #logoutButton { background: linear-gradient(45deg, #FF416C, #FF4B2B); }
        .action-btn { 
            background: linear-gradient(45deg, #00c6ff, #0072ff); 
            padding: 5px 10px;
            margin: 0 5px;
        }
        .delete-btn {
            background: linear-gradient(45deg, #ff4444, #cc0000);
            padding: 8px 15px;
            margin: 0;
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
        button:active { animation: pulse 0.3s ease; }

        /* Menu Styling */
        .menu-category {
            margin: 20px 0;
            text-align: left;
        }
        .menu-category h3 {
            color: #333;
            border-bottom: 2px solid #4CAF50;
            padding-bottom: 5px;
            margin-bottom: 10px;
        }
        .menu-category ul {
            list-style: none;
            padding: 0;
        }
        .menu-category li {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            margin: 5px 0;
            background-color: #fafafa;
            border-radius: 5px;
            transition: background-color 0.3s;
        }
        .menu-category li:hover {
            background-color: #f1f1f1;
        }
        .price {
            font-weight: 500;
            color: #4CAF50;
        }

        /* Responsive Design */
        @media (max-width: 600px) {
            .container {
                width: 95%;
            }
            input, select {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Restaurant Management System</h1>
        <button onclick="showSection('placeOrder')">Place Order</button>
        <button onclick="showSection('viewOrders')">View Orders</button>
        <button onclick="showSection('employeeLogin')" id="loginButton">Employee Login</button>
        <button onclick="showSection('inventoryManagement')" id="inventoryButton">Update Inventory</button>
        <button onclick="showSection('completeMenuSection')">View Complete Menu</button>
        <button onclick="logout()" id="logoutButton" style="display:none;">Logout</button>
        
        <!-- Place Order Section -->
        <div id="placeOrder" style="display:none;">
            <h2>Place Order</h2>
            <input type="text" id="customerName" placeholder="Customer Name" required>
            <input type="tel" id="phoneNumber" placeholder="Phone Number (10 digits)" pattern="[0-9]{10}" required>
            
            <select id="orderItem" required>
                <option value="">Select Item</option>
                <option value="Chicken Biryani - ₹300">Chicken Biryani - ₹300</option>
                <option value="Mutton Biryani - ₹350">Mutton Biryani - ₹350</option>
                <option value="Chicken Dum Biryani - ₹350">Chicken Dum Biryani - ₹350</option>
                <option value="Mutton Dum Biryani - ₹400">Mutton Dum Biryani - ₹400</option>
                <option value="Family Pack Biryani - ₹800">Family Pack Biryani - ₹800</option>
                <option value="Chicken Noodles - ₹250">Chicken Noodles - ₹250</option>
                <option value="Veg Noodles - ₹200">Veg Noodles - ₹200</option>
                <option value="Veg Manchurian - ₹250">Veg Manchurian - ₹250</option>
                <option value="Chicken Manchurian - ₹300">Chicken Manchurian - ₹300</option>
                <option value="Paneer Tikka - ₹300">Paneer Tikka - ₹300</option>
                <option value="Spring Rolls - ₹200">Spring Rolls - ₹200</option>
                <option value="Butter Chicken - ₹350">Butter Chicken - ₹350</option>
                <option value="Paneer Butter Masala - ₹300">Paneer Butter Masala - ₹300</option>
                <option value="Dal Makhani - ₹250">Dal Makhani - ₹250</option>
                <option value="Jeera Rice - ₹150">Jeera Rice - ₹150</option>
                <option value="Pulao - ₹200">Pulao - ₹200</option>
                <option value="Gulab Jamun - ₹150">Gulab Jamun - ₹150</option>
                <option value="Ras Malai - ₹200">Ras Malai - ₹200</option>
                <option value="Masala Chai - ₹50">Masala Chai - ₹50</option>
                <option value="Lassi - ₹100">Lassi - ₹100</option>
                <option value="Samosa - ₹20">Samosa - ₹20</option>
                <option value="Pakora - ₹30">Pakora - ₹30</option>
                <option value="Vegetable Salad - ₹100">Vegetable Salad - ₹100</option>
                <option value="Fruit Salad - ₹150">Fruit Salad - ₹150</option>
                <option value="Chicken Wrap - ₹250">Chicken Wrap - ₹250</option>
                <option value="Veg Wrap - ₹200">Veg Wrap - ₹200</option>
                <option value="Tomato Soup - ₹150">Tomato Soup - ₹150</option>
                <option value="Hot and Sour Soup - ₹200">Hot and Sour Soup - ₹200</option>
                <option value="Veg Thali - ₹300">Veg Thali - ₹300</option>
                <option value="Non-Veg Thali - ₹350">Non-Veg Thali - ₹350</option>
                <option value="Fresh Lime Soda - ₹50">Fresh Lime Soda - ₹50</option>
                <option value="Mango Lassi - ₹150">Mango Lassi - ₹150</option>
            </select>
            
            <input type="number" id="orderQuantity" placeholder="Quantity" min="1" required>
            <select id="paymentMethod" required>
                <option value="">Select Payment Method</option>
                <option value="Cash">Cash</option>
                <option value="Card">Credit/Debit Card</option>
                <option value="UPI">UPI</option>
            </select>
            <button onclick="addOrder()">Submit Order</button>
        </div>

        <!-- View Orders Section -->
        <div id="viewOrders" class="order-list" style="display:none;">
            <h2>View Orders</h2>
            <ul id="orderList"></ul>
        </div>

        <!-- Employee Login Section -->
        <div id="employeeLogin" class="login-container" style="display:none;">
            <h2>Employee Login</h2>
            <input type="text" id="employeeId" placeholder="Employee ID" required>
            <input type="password" id="employeePassword" placeholder="Password" required>
            <button onclick="employeeLogin()">Login</button>
            <p id="loginMessage"></p>
        </div>

        <!-- Employee Dashboard Section -->
        <div id="employeeDashboard" style="display:none;">
            <h2>Employee Dashboard</h2>
            <!-- Attendance Section -->
            <h3>Submit Attendance</h3>
            <input type="text" id="employeeName" placeholder="Employee Name" required>
            <input type="date" id="attendanceDate" required>
            <select id="attendanceStatus" required>
                <option value="Present">Present</option>
                <option value="Absent">Absent</option>
            </select>
            <input type="number" id="workingHours" placeholder="Working Hours" min="0" max="24" required>
            <button onclick="submitAttendance()">Submit Attendance</button>
            <div id="attendanceMessage"></div>

            <!-- Employee Details Section -->
            <h3>Employee Details</h3>
            <input type="text" id="employeeIdDetails" placeholder="Employee ID" required>
            <input type="text" id="employeeNameDetails" placeholder="Employee Name" required>
            <input type="email" id="employeeEmailDetails" placeholder="Employee Email" required>
            <input type="tel" id="employeePhoneDetails" placeholder="Phone Number (10 digits)" pattern="[0-9]{10}" required>
            <input type="text" id="employeeAddressDetails" placeholder="Employee Address" required>
            <button onclick="saveEmployeeDetails()">Save Details</button>
            <div id="employeeDetailsMessage"></div>
            <h4>Saved Employee Details:</h4>
            <ul id="savedEmployeeDetails"></ul>
        </div>

        <!-- Inventory Management Section -->
        <div id="inventoryManagement" style="display:none;">
            <h2>Update Inventory</h2>
            <input type="text" id="inventoryItemName" placeholder="Item Name" required>
            <input type="number" id="inventoryQuantity" placeholder="Quantity" min="0" required>
            <input type="number" id="inventoryPrice" placeholder="Price" min="0" required>
            <button onclick="updateInventory()">Update Inventory</button>
            <h3>Current Inventory</h3>
            <ul id="inventoryList"></ul>
        </div>

        <!-- Complete Menu Section -->
        <div id="completeMenuSection" style="display:none;">
            <h2>Restaurant Menu</h2>
            
            <div class="menu-category">
                <h3>Biryani</h3>
                <ul>
                    <li>Chicken Biryani<span class="price">₹300</span></li>
                    <li>Mutton Biryani<span class="price">₹350</span></li>
                    <li>Chicken Dum Biryani<span class="price">₹350</span></li>
                    <li>Mutton Dum Biryani<span class="price">₹400</span></li>
                    <li>Family Pack Biryani<span class="price">₹800</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Noodles</h3>
                <ul>
                    <li>Chicken Noodles<span class="price">₹250</span></li>
                    <li>Veg Noodles<span class="price">₹200</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Manchurian</h3>
                <ul>
                    <li>Veg Manchurian<span class="price">₹250</span></li>
                    <li>Chicken Manchurian<span class="price">₹300</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Starters</h3>
                <ul>
                    <li>Paneer Tikka<span class="price">₹300</span></li>
                    <li>Spring Rolls<span class="price">₹200</span></li>
                    <li>Chicken Tikka<span class="price">₹300</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Curries</h3>
                <ul>
                    <li>Butter Chicken<span class="price">₹350</span></li>
                    <li>Paneer Butter Masala<span class="price">₹300</span></li>
                    <li>Dal Makhani<span class="price">₹250</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Rice Dishes</h3>
                <ul>
                    <li>Jeera Rice<span class="price">₹150</span></li>
                    <li>Pulao<span class="price">₹200</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Desserts</h3>
                <ul>
                    <li>Gulab Jamun<span class="price">₹150</span></li>
                    <li>Ras Malai<span class="price">₹200</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Beverages & Snacks</h3>
                <ul>
                    <li>Masala Chai<span class="price">₹50</span></li>
                    <li>Lassi<span class="price">₹100</span></li>
                    <li>Mango Lassi<span class="price">₹150</span></li>
                    <li>Fresh Lime Soda<span class="price">₹50</span></li>
                    <li>Samosa<span class="price">₹20</span></li>
                    <li>Pakora<span class="price">₹30</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Salads & Wraps</h3>
                <ul>
                    <li>Vegetable Salad<span class="price">₹100</span></li>
                    <li>Fruit Salad<span class="price">₹150</span></li>
                    <li>Chicken Wrap<span class="price">₹250</span></li>
                    <li>Veg Wrap<span class="price">₹200</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Soups</h3>
                <ul>
                    <li>Tomato Soup<span class="price">₹150</span></li>
                    <li>Hot and Sour Soup<span class="price">₹200</span></li>
                </ul>
            </div>

            <div class="menu-category">
                <h3>Thali</h3>
                <ul>
                    <li>Veg Thali<span class="price">₹300</span></li>
                    <li>Non-Veg Thali<span class="price">₹350</span></li>
                </ul>
            </div>
        </div>
    </div>

    <script>
        let orders = JSON.parse(localStorage.getItem('orders')) || [];
        let employees = JSON.parse(localStorage.getItem('employees')) || [];
        let inventory = JSON.parse(localStorage.getItem('inventory')) || [];
        let loggedIn = false;
        const loginTimeout = 30 * 60 * 1000;

        window.onload = () => {
            displayOrders();
            displaySavedEmployeeDetails();
            displayInventory();
        };

        function showSection(id) {
            if (id === 'employeeLogin' && loggedIn) {
                alert('You are already logged in.');
                return;
            }
            document.querySelectorAll('.container > div').forEach(div => div.style.display = 'none');
            document.getElementById(id).style.display = 'block';
        }

        function validatePhone(phone) {
            return /^\d{10}$/.test(phone);
        }

        function validateEmail(email) {
            return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
        }

        function addOrder() {
            const customerName = document.getElementById('customerName').value.trim();
            const phoneNumber = document.getElementById('phoneNumber').value.trim();
            const selectedItem = document.getElementById('orderItem').value;
            const quantity = parseInt(document.getElementById('orderQuantity').value);
            const paymentMethod = document.getElementById('paymentMethod').value;
            const orderDate = new Date().toLocaleString();

            if (!customerName || !validatePhone(phoneNumber) || !selectedItem || quantity < 1 || !paymentMethod) {
                alert('Please enter valid order details:\n- Valid 10-digit phone number\n- All fields required');
                return;
            }

            const itemDetails = selectedItem.split(' - ₹');
            const itemName = itemDetails[0];
            const itemPrice = parseInt(itemDetails[1]);
            const totalCost = itemPrice * quantity;

            const order = { 
                id: Date.now(), 
                customerName, 
                phoneNumber, 
                item: itemName, 
                quantity, 
                paymentMethod, 
                orderDate, 
                totalCost: ₹${totalCost} 
            };
            
            orders.push(order);
            localStorage.setItem('orders', JSON.stringify(orders));
            alert('Order placed successfully!');
            displayOrders();
            document.getElementById('placeOrder').querySelector('form')?.reset();
        }

        function deleteOrder(id) {
            orders = orders.filter(order => order.id !== id);
            localStorage.setItem('orders', JSON.stringify(orders));
            displayOrders();
        }

        function displayOrders() {
            const orderList = document.getElementById('orderList');
            orderList.innerHTML = '';
            orders.forEach(order => {
                const li = document.createElement('li');
                li.className = 'order-item';
                li.innerHTML = 
                    `<div class="order-details">
                        <div><strong>Name:</strong> ${order.customerName}</div>
                        <div><strong>Phone:</strong> ${order.phoneNumber}</div>
                        <div><strong>Item:</strong> ${order.item} (x${order.quantity})</div>
                        <div><strong>Payment:</strong> ${order.paymentMethod}</div>
                        <div><strong>Date:</strong> ${order.orderDate}</div>
                        <div><strong>Total Cost:</strong> ${order.totalCost}</div>
                    </div>
                    <div>
                        <button class="action-btn" onclick="deleteOrder(${order.id})">Delete</button>
                    </div>`
                ;
                orderList.appendChild(li);
            });
        }

        function employeeLogin() {
            const employeeId = document.getElementById('employeeId').value.trim();
            const employeePassword = document.getElementById('employeePassword').value;
            const validCredentials = { 'BATCH13': '1317' };
            const message = document.getElementById('loginMessage');

            if (validCredentials[employeeId] === employeePassword) {
                loggedIn = true;
                message.className = 'success';
                message.textContent = 'Login successful! Redirecting...';
                setTimeout(() => {
                    showSection('employeeDashboard');
                    document.getElementById('logoutButton').style.display = 'inline-block';
                    document.getElementById('loginButton').style.display = 'none';
                    setTimeout(logout, loginTimeout);
                }, 1000);
            } else {
                message.className = 'error';
                message.textContent = 'Incorrect ID or password.';
            }
        }

        function logout() {
            loggedIn = false;
            document.getElementById('logoutButton').style.display = 'none';
            document.getElementById('loginButton').style.display = 'inline-block';
            showSection('employeeLogin');
            alert('You have been logged out.');
        }

        function submitAttendance() {
            const employeeName = document.getElementById('employeeName').value.trim();
            const attendanceDate = document.getElementById('attendanceDate').value;
            const attendanceStatus = document.getElementById('attendanceStatus').value;
            const workingHours = parseInt(document.getElementById('workingHours').value);

            if (!employeeName || !attendanceDate || isNaN(workingHours) || workingHours < 0 || workingHours > 24) {
                alert('Please enter valid attendance details:\n- All fields required\n- Hours between 0-24');
                return;
            }

            const attendanceMessage = document.getElementById('attendanceMessage');
            attendanceMessage.innerHTML = 
                `<div><strong>Name:</strong> ${employeeName}</div>
                <div><strong>Date:</strong> ${attendanceDate}</div>
                <div><strong>Status:</strong> ${attendanceStatus}</div>
                <div><strong>Hours:</strong> ${workingHours}</div>`
            ;
            attendanceMessage.className = 'success';
        }

        function saveEmployeeDetails() {
            const employeeId = document.getElementById('employeeIdDetails').value.trim();
            const employeeName = document.getElementById('employeeNameDetails').value.trim();
            const employeeEmail = document.getElementById('employeeEmailDetails').value.trim();
            const employeePhone = document.getElementById('employeePhoneDetails').value.trim();
            const employeeAddress = document.getElementById('employeeAddressDetails').value.trim();
            const message = document.getElementById('employeeDetailsMessage');

            if (!employeeId || !employeeName || !validateEmail(employeeEmail) || !validatePhone(employeePhone) || !employeeAddress) {
                message.className = 'error';
                message.textContent = 'Please enter valid details:\n- Valid email\n- 10-digit phone';
                return;
            }

            if (employees.some(emp => emp.employeeId === employeeId)) {
                message.className = 'error';
                message.textContent = 'Employee ID already exists!';
                return;
            }

            employees.push({ employeeId, employeeName, employeeEmail, employeePhone, employeeAddress });
            localStorage.setItem('employees', JSON.stringify(employees));
            message.className = 'success';
            message.textContent = 'Employee details saved successfully!';
            displaySavedEmployeeDetails();
        }

        function displaySavedEmployeeDetails() {
            const savedEmployeeDetailsList = document.getElementById('savedEmployeeDetails');
            savedEmployeeDetailsList.innerHTML = '';
            
            employees.forEach(employee => {
                const li = document.createElement('li');
                li.className = 'employee-item';
                li.innerHTML = 
                    `<div class="employee-info">
                        <span class="employee-label">Employee ID:</span>
                        <span class="employee-value">${employee.employeeId}</span>
                        <span class="employee-label">Name:</span>
                        <span class="employee-value">${employee.employeeName}</span>
                        <span class="employee-label">Email:</span>
                        <span class="employee-value">${employee.employeeEmail}</span>
                    </div>
                    <div class="employee-info">
                        <span class="employee-label">Phone:</span>
                        <span class="employee-value">${employee.employeePhone}</span>
                        <span class="employee-label">Address:</span>
                        <span class="employee-value">${employee.employeeAddress}</span>
                    </div>
                    <div>
                        <button class="delete-btn" onclick="deleteEmployee('${employee.employeeId}')">Delete</button>
                    </div>`
                ;
                savedEmployeeDetailsList.appendChild(li);
            });
            
            if (employees.length === 0) {
                savedEmployeeDetailsList.innerHTML = 
                    `<li class="employee-item" style="grid-template-columns: 1fr; text-align: center; color: #666;">
                        No employee details saved yet
                    </li>`
                ;
            }
        }

        function deleteEmployee(employeeId) {
            if (confirm('Are you sure you want to delete this employee?')) {
                employees = employees.filter(emp => emp.employeeId !== employeeId);
                localStorage.setItem('employees', JSON.stringify(employees));
                displaySavedEmployeeDetails();
                const message = document.getElementById('employeeDetailsMessage');
                message.className = 'success';
                message.textContent = 'Employee deleted successfully!';
            }
        }

        // Inventory Management Functions
        function updateInventory() {
            const itemName = document.getElementById('inventoryItemName').value.trim();
            const quantity = parseInt(document.getElementById('inventoryQuantity').value);
            const price = parseFloat(document.getElementById('inventoryPrice').value);

            if (!itemName || quantity < 0 || price < 0) {
                alert('Please enter valid inventory details.');
                return;
            }

            const existingItem = inventory.find(item => item.name === itemName);
            if (existingItem) {
                existingItem.quantity += quantity; // Update quantity
                existingItem.price = price; // Update price
            } else {
                inventory.push({ name: itemName, quantity, price });
            }

            localStorage.setItem('inventory', JSON.stringify(inventory));
            displayInventory();
            clearInventoryFields();
        }

        function displayInventory() {
            const inventoryList = document.getElementById('inventoryList');
            inventoryList.innerHTML = '';
            inventory.forEach(item => {
                const li = document.createElement('li');
                li.className = 'inventory-item';
                li.innerHTML = 
                    `<div class="inventory-details">
                        <div><strong>Item:</strong> ${item.name}</div>
                        <div><strong>Quantity:</strong> ${item.quantity}</div>
                        <div><strong>Price:</strong> ₹${item.price.toFixed(2)}</div>
                    </div>
                    <div>
                        <button class="delete-btn" onclick="deleteInventoryItem('${item.name}')">Delete</button>
                    </div>`
                ;
                inventoryList.appendChild(li);
            });
        }

        function deleteInventoryItem(itemName) {
            if (confirm('Are you sure you want to delete this item from inventory?')) {
                inventory = inventory.filter(item => item.name !== itemName);
                localStorage.setItem('inventory', JSON.stringify(inventory));
                displayInventory();
            }
        }

        function clearInventoryFields() {
            document.getElementById('inventoryItemName').value = '';
            document.getElementById('inventoryQuantity').value = '';
            document.getElementById('inventoryPrice').value = '';
        }
    </script>
</body>
</html>
