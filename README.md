<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WAREHOUSE MANAGEMENT SYSTEM</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .slide { display: none; }
        .active { display: block; }
        
        @media (max-width: 768px) {
            .table-responsive {
                overflow-x: auto;
            }
        }
        
        .hidden { display: none; }
        .login-form, .register-form {
            transition: all 0.3s ease;
        }
        /* PWA */
        #installBtn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 100;
        }
    </style>
</head>
<body class="bg-gray-100">
    <!-- Login Screen -->
    <div id="loginScreen" class="fixed inset-0 bg-gray-900 bg-opacity-90 flex items-center justify-center z-50">
        <div class="bg-white p-8 rounded-lg shadow-xl w-full max-w-md">
            <div class="text-center mb-8">
                <h1 class="text-2xl font-bold text-gray-800">WAREHOUSE MANAGEMENT SYSTEM</h1>
                <div class="flex justify-center space-x-4 mt-4">
                    <button id="showLogin" class="border-b-2 border-blue-600 font-medium">Login</button>
                    <button id="showRegister" class="text-gray-500">Register</button>
                </div>
            </div>
            <form id="loginForm" class="login-form">
                <div class="mb-4">
                    <label for="username" class="block text-gray-700 mb-2">Username</label>
                    <input type="text" id="username" 
                           class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" 
                           placeholder="Username" required>
                </div>
                <div class="mb-6">
                    <label for="password" class="block text-gray-700 mb-2">Password</label>
                    <input type="password" id="password" 
                           class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" 
                           placeholder="Enter password" required>
                </div>
                <button type="submit" 
                        class="w-full bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700 transition duration-200">
                    Login
                </button>
                <p id="loginStatus" class="text-center mt-4 text-red-500 hidden"></p>
            </form>
            <form id="registerForm" class="register-form hidden">
                <div class="mb-4">
                    <label for="regName" class="block text-gray-700 mb-2">Full Name</label>
                    <input type="text" id="regName" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                </div>
                <div class="mb-4">
                    <label for="regUsername" class="block text-gray-700 mb-2">Username</label>
                    <input type="text" id="regUsername" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                </div>
                <div class="mb-4">
                    <label for="regPassword" class="block text-gray-700 mb-2">Password</label>
                    <input type="password" id="regPassword" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" required minlength="8">
                </div>
                <div class="mb-6">
                    <label for="regConfirmPassword" class="block text-gray-700 mb-2">Confirm Password</label>
                    <input type="password" id="regConfirmPassword" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                </div>
                <button type="submit" class="w-full bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700 transition duration-200">
                    Register
                </button>
            </form>
        </div>
    </div>

    <!-- Main App -->
    <div id="app" class="container mx-auto p-4" style="display: none;">
        <!-- Header -->
        <header class="bg-white shadow-sm p-4 rounded-lg mb-6 flex justify-between items-center">
            <div>
                <h1 class="text-xl font-bold text-gray-800">WAREHOUSE MANAGEMENT SYSTEM</h1>
                <div class="flex items-center">
                    <p id="currentDate" class="text-gray-600 mr-4"></p>
                    <input type="date" id="dateSelector" class="border rounded px-2 py-1">
                </div>
            </div>
            <div class="flex items-center space-x-4">
                <select id="languageSwitcher" class="border rounded px-2 py-1 text-sm">
                    <option value="en">English</option>
                    <option value="id">Indonesia</option>
                </select>
                <button id="logoutBtn" class="text-red-600 hover:text-red-800">
                    <i class="fas fa-sign-out-alt"></i> Logout
                </button>
            </div>
        </header>

        <!-- Navigation -->
        <nav class="flex mb-6 bg-white rounded-lg shadow-sm overflow-hidden">
            <button class="tab-btn flex-1 py-3 px-4 text-center hover:bg-gray-50 transition" data-slide="1">Inventory Tracking</button>
            <button class="tab-btn flex-1 py-3 px-4 text-center hover:bg-gray-50 transition" data-slide="2">Stock Management</button>
            <button class="tab-btn flex-1 py-3 px-4 text-center hover:bg-gray-50 transition" data-slide="3">Reports Dashboard</button>
        </nav>

        <!-- Slide 1: Inventory Tracking -->
        <div id="slide1" class="slide active bg-white p-4 rounded-lg shadow-sm">
            <div class="mb-6">
                <h2 class="text-lg font-semibold mb-2">Inventory Tracking</h2>
                <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-4 mb-4">
                    <div class="relative flex-1">
                        <input type="text" id="searchInput" 
                               class="w-full pl-10 pr-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" 
                               placeholder="Search item, rack code or employee...">
                        <i class="fas fa-search absolute left-3 top-3 text-gray-400"></i>
                    </div>
                    <button id="addItemBtn" class="bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700 transition">
                        <i class="fas fa-plus mr-2"></i> Add Item
                    </button>
                </div>
                
                <div class="table-responsive">
                    <table class="min-w-full bg-white border">
                        <thead>
                            <tr class="bg-gray-100">
                                <th class="py-2 px-4 border">Rack Code</th>
                                <th class="py-2 px-4 border">Item Name</th>
                                <th class="py-2 px-4 border">Return Time</th>
                                <th class="py-2 px-4 border">Take Time</th>
                                <th class="py-2 px-4 border">Taken By</th>
                                <th class="py-2 px-4 border">Status</th>
                                <th class="py-2 px-4 border">Actions</th>
                            </tr>
                        </thead>
                        <tbody id="inventoryTable">
                            <!-- Data will be filled by JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Slide 2: Stock Management -->
        <div id="slide2" class="slide bg-white p-4 rounded-lg shadow-sm">
            <div class="mb-8">
                <h2 class="text-lg font-semibold mb-4">Stock Management</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                    <!-- Low Stock -->
                    <div class="bg-red-50 p-4 rounded-lg border border-red-200">
                        <h3 class="font-medium text-red-700 mb-4 flex items-center">
                            <i class="fas fa-exclamation-triangle mr-2"></i> Low Stock (Below 10)
                        </h3>
                        <div class="table-responsive">
                            <table class="min-w-full">
                                <thead>
                                    <tr>
                                        <th class="py-1 px-2 text-left">Item</th>
                                        <th class="py-1 px-2 text-left">Rack</th>
                                        <th class="py-1 px-2 text-right">Stock</th>
                                    </tr>
                                </thead>
                                <tbody id="lowStockTable">
                                    <!-- Data will be filled by JavaScript -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                    
                    <!-- Sufficient Stock -->
                    <div class="bg-green-50 p-4 rounded-lg border border-green-200">
                        <h3 class="font-medium text-green-700 mb-4 flex items-center">
                            <i class="fas fa-check-circle mr-2"></i> Sufficient Stock (10+)
                        </h3>
                        <div class="table-responsive">
                            <table class="min-w-full">
                                <thead>
                                    <tr>
                                        <th class="py-1 px-2 text-left">Item</th>
                                        <th class="py-1 px-2 text-left">Rack</th>
                                        <th class="py-1 px-2 text-right">Stock</th>
                                    </tr>
                                </thead>
                                <tbody id="sufficientStockTable">
                                    <!-- Data will be filled by JavaScript -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
                
                <!-- Purchase Status -->
                <div>
                    <h3 class="font-medium mb-4 flex items-center">
                        <i class="fas fa-shopping-cart mr-2 text-blue-600"></i> Purchase Status
                    </h3>
                    <div class="table-responsive">
                        <table class="min-w-full bg-white border">
                            <thead>
                                <tr class="bg-gray-100">
                                    <th class="py-2 px-4 border">Item</th>
                                    <th class="py-2 px-4 border">Purchase Date</th>
                                    <th class="py-2 px-4 border">Price</th>
                                    <th class="py-2 px-4 border">Status</th>
                                </tr>
                            </thead>
                            <tbody id="purchaseTable">
                                <!-- Data will be filled by JavaScript -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- Slide 3: Reports Dashboard -->
        <div id="slide3" class="slide bg-white p-4 rounded-lg shadow-sm">
            <div class="mb-8">
                <h2 class="text-lg font-semibold mb-6">Reports Dashboard</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
                    <!-- Weekly Chart -->
                    <div class="bg-white p-4 border rounded-lg">
                        <h3 class="font-medium mb-4 flex items-center">
                            <i class="fas fa-chart-bar mr-2 text-blue-600"></i> Weekly Inventory Movement
                        </h3>
                        <canvas id="weeklyChart"></canvas>
                    </div>
                    
                    <!-- Monthly Chart -->
                    <div class="bg-white p-4 border rounded-lg">
                        <h3 class="font-medium mb-4 flex items-center">
                            <i class="fas fa-chart-line mr-2 text-green-600"></i> Monthly Inventory Analysis
                        </h3>
                        <canvas id="monthlyChart"></canvas>
                    </div>
                </div>
                
                <!-- Summary -->
                <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                    <h3 class="font-medium mb-4">Summary</h3>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        <div class="bg-white p-3 rounded border">
                            <h4 class="text-sm text-gray-500 mb-1">Total Items</h4>
                            <p id="totalItems" class="text-2xl font-bold">0</p>
                        </div>
                        <div class="bg-white p-3 rounded border">
                            <h4 class="text-sm text-gray-500 mb-1">Low Stock Items</h4>
                            <p id="lowStockItems" class="text-2xl font-bold text-red-600">0</p>
                        </div>
                        <div class="bg-white p-3 rounded border">
                            <h4 class="text-sm text-gray-500 mb-1">Pending Purchases</h4>
                            <p id="pendingPurchases" class="text-2xl font-bold text-orange-600">0</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Add Item Modal -->
        <div id="addItemModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-40" style="display: none;">
            <div class="bg-white p-6 rounded-lg w-full max-w-md">
                <h2 class="text-xl font-bold mb-4" id="modalTitle">Add New Item</h2>
                <form id="itemForm">
                    <div class="space-y-4">
                        <div>
                            <label for="rackCode" class="block text-gray-700">Rack Code</label>
                            <input type="text" id="rackCode" class="w-full px-4 py-2 border rounded" required>
                        </div>
                        <div>
                            <label class="block text-gray-700">Item Name</label>
                            <input type="text" id="itemName" class="w-full px-4 py-2 border rounded" required>
                        </div>
                        <div>
                            <label class="block text-gray-700">Quantity</label>
                            <input type="number" id="itemQty" class="w-full px-4 py-2 border rounded" required>
                        </div>
                    </div>
                    <div class="flex justify-end space-x-2 mt-6">
                        <button type="button" id="cancelAddBtn" class="px-4 py-2 border rounded hover:bg-gray-100">Cancel</button>
                        <button type="submit" class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700">Save</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- PWA Install Button -->
    <button id="installBtn" class="bg-blue-600 text-white p-3 rounded-full shadow-lg hover:bg-blue-700" style="display: none;">
        <i class="fas fa-download"></i>
    </button>

    <!-- Web Access Info -->
    <div id="webAccessInfo" class="fixed bottom-0 left-0 right-0 bg-gray-100 p-4 text-center border-t border-gray-300 hidden">
        <p>Access this app anytime at: <span id="webUrl" class="font-mono text-blue-600">https://wms.r3inventory.com</span></p>
    </div>

    <script>
        // Language Translations
        const translations = {
            en: {
                pageTitle: "Warehouse Management System",
                loginTitle: "Warehouse Management",
                loginBtn: "Login",
                inventoryTracking: "Inventory Tracking",
                stockManagement: "Stock Management",
                reportsDashboard: "Reports Dashboard",
                rackCode: "Rack Code",
                itemName: "Item Name",
                returnTime: "Return Time",
                takeTime: "Take Time",
                takenBy: "Taken By",
                status: "Status",
                actions: "Actions",
                addItem: "Add Item",
                searchPlaceholder: "Search item, rack code or employee..."
            },
            id: {
                pageTitle: "Sistem Manajemen Gudang",
                loginTitle: "Manajemen Gudang",
                username: "Nama Pengguna",
                password: "Kata Sandi",
                namePrompt: "Masukkan nama Anda:",
                consumedPrompt: "Apakah barang ini habis terpakai? (contoh: elektroda)",
                loginBtn: "Masuk",
                logoutBtn: "Keluar",
                inventoryTracking: "Pelacakan Inventaris", 
                stockManagement: "Manajemen Stok",
                reportsDashboard: "Dasbor Laporan",
                rackCode: "Kode Rak",
                itemName: "Nama Barang",
                returnTime: "Waktu Kembali",
                takeTime: "Waktu Ambil",
                takenBy: "Diambil Oleh",
                status: "Status",
                actions: "Aksi",
                addItem: "Tambah Barang",
                searchPlaceholder: "Cari barang, kode rak atau karyawan...",
                item: "Barang",
                rack: "Rak",
                stock: "Stok",
                price: "Harga",
                purchaseStatus: "Status Pembelian",
                lowStock: "Stok Rendah (<10)",
                sufficientStock: "Stok Cukup (10+)",
                purchaseDate: "Tanggal Pembelian",
                totalItems: "Total Barang",
                lowStockItems: "Barang Stok Rendah",
                pendingPurchases: "Pembelian Tertunda",
                addNewItem
