<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>برنامج المصاريف الشهرية</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap');
        
        body {
            font-family: 'Tajawal', sans-serif;
            background-color: #f8fafc;
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .progress-bar {
            transition: width 0.5s ease-in-out;
        }
        
        .category-icon {
            transition: all 0.3s ease;
        }
        
        .category-icon:hover {
            transform: scale(1.1);
        }
        
        .modal-overlay {
            background-color: rgba(0, 0, 0, 0.5);
        }
        
        .confirmation-dialog {
            animation: slideIn 0.3s ease-out;
        }
        
        @keyframes slideIn {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        .toast {
            animation: toastIn 0.3s ease-out, toastOut 0.3s ease-in 2.7s;
        }
        
        @keyframes toastIn {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        @keyframes toastOut {
            from { transform: translateY(0); opacity: 1; }
            to { transform: translateY(-20px); opacity: 0; }
        }
        
        .expense-card:hover {
            transform: translateX(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }
        
        .expense-card {
            transition: all 0.3s ease;
        }
        
        .select-dropdown {
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' fill='none' viewBox='0 0 24 24' stroke='%236B7280'%3E%3Cpath stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M19 9l-7 7-7-7'%3E%3C/path%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 0.5rem center;
            background-size: 1.5em;
            padding-right: 2.5rem;
        }
        
        .shake {
            animation: shake 0.5s;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-5px); }
            40%, 80% { transform: translateX(5px); }
        }
        
        .year-selector {
            background-color: #6366f1;
            color: white;
            border-radius: 9999px;
            padding: 0.25rem 0.75rem;
            font-size: 0.875rem;
            border: none;
            outline: none;
            cursor: pointer;
        }
        
        .year-selector:focus {
            ring: 2px;
            ring-color: #a5b4fc;
        }
        
        .date-picker-container {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .savings-card {
            background: linear-gradient(135deg, #4f46e5 0%, #7c3aed 100%);
            color: white;
        }
        
        .month-savings-bar {
            height: 4px;
            background-color: rgba(255, 255, 255, 0.2);
            border-radius: 2px;
            overflow: hidden;
        }
        
        .month-savings-progress {
            height: 100%;
            background-color: white;
            border-radius: 2px;
        }
        
        .search-input {
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' class='h-5 w-5 text-gray-400' fill='none' viewBox='0 0 24 24' stroke='currentColor'%3E%3Cpath stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z'%3E%3C/path%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 0.75rem center;
            background-size: 1.25rem;
        }
        
        .search-options {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-top: 0.5rem;
        }
        
        .search-option {
            font-size: 0.75rem;
            padding: 0.25rem 0.5rem;
            border-radius: 0.25rem;
            cursor: pointer;
            background-color: #f3f4f6;
            color: #4b5563;
        }
        
        .search-option.active {
            background-color: #6366f1;
            color: white;
        }
        
        .category-tab {
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .category-tab.active {
            background-color: #6366f1;
            color: white;
        }
        
        .category-tab:hover:not(.active) {
            background-color: #e0e7ff;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">
    <div class="min-h-screen">
        <!-- Header -->
        <header class="bg-indigo-600 text-white shadow-lg">
            <div class="container mx-auto px-4 py-6">
                <div class="flex justify-between items-center">
                    <div>
                        <h1 class="text-2xl font-bold">برنامج المصاريف الشهرية</h1>
                        <p class="text-indigo-100 mt-1">تتبع مصروفاتك بسهولة وذكاء</p>
                    </div>
                    <div class="flex items-center space-x-4 space-x-reverse">
                        <div class="flex items-center space-x-2 space-x-reverse">
                            <div class="date-picker-container">
                                <select id="year-selector" class="year-selector">
                                    <!-- Years will be populated dynamically -->
                                </select>
                                <select id="month-selector" class="bg-indigo-500 text-white px-3 py-1 rounded-full border-none focus:ring-2 focus:ring-indigo-300 select-dropdown">
                                    <option value="0">يناير</option>
                                    <option value="1">فبراير</option>
                                    <option value="2">مارس</option>
                                    <option value="3">أبريل</option>
                                    <option value="4">مايو</option>
                                    <option value="5">يونيو</option>
                                    <option value="6">يوليو</option>
                                    <option value="7">أغسطس</option>
                                    <option value="8">سبتمبر</option>
                                    <option value="9">أكتوبر</option>
                                    <option value="10">نوفمبر</option>
                                    <option value="11">ديسمبر</option>
                                </select>
                            </div>
                            <select id="currency-selector" class="bg-indigo-500 text-white px-3 py-1 rounded-full border-none focus:ring-2 focus:ring-indigo-300 select-dropdown">
                                <option value="ر.س">ريال سعودي (ر.س)</option>
                                <option value="د.إ" selected>درهم إماراتي (د.إ)</option>
                                <option value="د.م">درهم مغربي (د.م)</option>
                                <option value="ج.م">جنيه مصري (ج.م)</option>
                                <option value="$">دولار ($)</option>
                            </select>
                        </div>
                        <button id="add-expense-btn" class="bg-white text-indigo-600 px-4 py-2 rounded-full hover:bg-indigo-50 transition flex items-center">
                            <i class="fas fa-plus ml-2"></i>
                            <span>إضافة مصروف</span>
                        </button>
                    </div>
                </div>
            </div>
        </header>

        <!-- Main Content -->
        <main class="container mx-auto px-4 py-8">
            <!-- Budget Summary -->
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                <!-- Total Budget -->
                <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-indigo-500">
                    <div class="flex justify-between items-start">
                        <div>
                            <p class="text-gray-500">الميزانية الشهرية</p>
                            <h3 class="text-2xl font-bold mt-1" id="total-budget">0 د.إ</h3>
                        </div>
                        <div class="bg-indigo-100 text-indigo-600 p-3 rounded-full">
                            <i class="fas fa-wallet text-lg"></i>
                        </div>
                    </div>
                    <div class="mt-4">
                        <button id="edit-budget-btn" class="text-indigo-600 hover:text-indigo-800 text-sm flex items-center">
                            <i class="fas fa-edit ml-1"></i>
                            تعديل الميزانية
                        </button>
                    </div>
                </div>

                <!-- Total Expenses -->
                <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-red-500">
                    <div class="flex justify-between items-start">
                        <div>
                            <p class="text-gray-500">إجمالي المصروفات</p>
                            <h3 class="text-2xl font-bold mt-1" id="total-expenses">0 د.إ</h3>
                        </div>
                        <div class="bg-red-100 text-red-600 p-3 rounded-full">
                            <i class="fas fa-money-bill-wave text-lg"></i>
                        </div>
                    </div>
                    <div class="mt-4">
                        <p class="text-sm text-gray-500" id="expenses-percentage">0% من الميزانية</p>
                    </div>
                </div>

                <!-- Remaining Budget -->
                <div class="bg-white rounded-xl shadow-md p-6 border-l-4 border-green-500">
                    <div class="flex justify-between items-start">
                        <div>
                            <p class="text-gray-500">المتبقي من الميزانية</p>
                            <h3 class="text-2xl font-bold mt-1" id="remaining-budget">0 د.إ</h3>
                        </div>
                        <div class="bg-green-100 text-green-600 p-3 rounded-full">
                            <i class="fas fa-coins text-lg"></i>
                        </div>
                    </div>
                    <div class="mt-4">
                        <div class="w-full bg-gray-200 rounded-full h-2">
                            <div id="remaining-progress" class="progress-bar h-2 rounded-full bg-green-500" style="width: 100%"></div>
                        </div>
                    </div>
                </div>

                <!-- Savings Card -->
                <div class="savings-card rounded-xl shadow-md p-6">
                    <div class="flex justify-between items-start">
                        <div>
                            <p class="text-indigo-100">إجمالي التوفير</p>
                            <h3 class="text-2xl font-bold mt-1 text-white" id="total-savings">0 د.إ</h3>
                        </div>
                        <div class="bg-white bg-opacity-20 p-3 rounded-full">
                            <i class="fas fa-piggy-bank text-lg text-white"></i>
                        </div>
                    </div>
                    <div class="mt-4">
                        <p class="text-sm text-indigo-100" id="savings-percentage">0% من الميزانية</p>
                        <div class="month-savings-bar mt-2">
                            <div id="month-savings-progress" class="month-savings-progress" style="width: 0%"></div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Expenses Overview -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
                <!-- Expenses by Category -->
                <div class="bg-white rounded-xl shadow-md p-6 lg:col-span-1">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-xl font-bold">المصروفات حسب الفئة</h2>
                        <div class="text-gray-500">
                            <i class="fas fa-tags"></i>
                        </div>
                    </div>
                    <div class="space-y-4" id="categories-list">
                        <!-- Categories will be added here dynamically -->
                    </div>
                </div>

                <!-- Recent Expenses -->
                <div class="bg-white rounded-xl shadow-md p-6 lg:col-span-2">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-xl font-bold">مصروفات الشهر</h2>
                        <div class="flex items-center space-x-4 space-x-reverse">
                            <div class="relative">
                                <input type="text" id="search-expenses" placeholder="ابحث عن مصروف..." class="search-input pr-10 pl-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 w-full">
                                <div class="search-options">
                                    <div class="search-option active" data-scope="current">هذا الشهر</div>
                                    <div class="search-option" data-scope="all">كل الشهور</div>
                                </div>
                            </div>
                            <div class="flex space-x-2 space-x-reverse">
                                <button id="filter-month" class="bg-indigo-100 text-indigo-600 px-3 py-1 rounded-full text-sm font-medium">الشهر</button>
                                <button id="filter-week" class="bg-gray-100 text-gray-600 px-3 py-1 rounded-full text-sm font-medium hover:bg-gray-200">الأسبوع</button>
                                <button id="filter-today" class="bg-gray-100 text-gray-600 px-3 py-1 rounded-full text-sm font-medium hover:bg-gray-200">اليوم</button>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Category Tabs -->
                    <div class="flex overflow-x-auto mb-4 pb-2 space-x-2 space-x-reverse">
                        <div class="category-tab active px-4 py-2 rounded-full text-sm font-medium" data-category="all">
                            الكل
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="food">
                            <i class="fas fa-utensils ml-1"></i> طعام
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="transport">
                            <i class="fas fa-car ml-1"></i> مواصلات
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="shopping">
                            <i class="fas fa-shopping-bag ml-1"></i> تسوق
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="bills">
                            <i class="fas fa-file-invoice-dollar ml-1"></i> فواتير
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="entertainment">
                            <i class="fas fa-film ml-1"></i> ترفيه
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="health">
                            <i class="fas fa-heartbeat ml-1"></i> صحة
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="education">
                            <i class="fas fa-book ml-1"></i> تعليم
                        </div>
                        <div class="category-tab px-4 py-2 rounded-full text-sm font-medium" data-category="other">
                            <i class="fas fa-ellipsis-h ml-1"></i> أخرى
                        </div>
                    </div>
                    
                    <div class="overflow-x-auto">
                        <table class="w-full">
                            <thead>
                                <tr class="text-gray-500 border-b">
                                    <th class="pb-3 text-right">الوصف</th>
                                    <th class="pb-3 text-right">المبلغ</th>
                                    <th class="pb-3 text-right">التاريخ</th>
                                    <th class="pb-3"></th>
                                </tr>
                            </thead>
                            <tbody id="expenses-table-body">
                                <!-- Expenses will be added here dynamically -->
                            </tbody>
                        </table>
                        <div id="no-expenses" class="py-12 text-center text-gray-500">
                            <i class="fas fa-money-bill-wave text-4xl mb-3"></i>
                            <p class="text-lg">لا توجد مصروفات مسجلة</p>
                            <button id="add-first-expense" class="mt-4 bg-indigo-600 text-white px-4 py-2 rounded-full hover:bg-indigo-700 transition">
                                إضافة أول مصروف
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Add Expense Modal -->
    <div id="add-expense-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-xl shadow-xl w-full max-w-md mx-4 fade-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-800">إضافة مصروف جديد</h3>
                    <button id="close-add-modal" class="text-gray-500 hover:text-gray-700">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <form id="add-expense-form" class="space-y-4">
                    <div>
                        <label for="expense-description" class="block text-sm font-medium text-gray-700 mb-1">الوصف</label>
                        <input type="text" id="expense-description" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500" placeholder="مثال: شراء مواد غذائية" required>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label for="expense-amount" class="block text-sm font-medium text-gray-700 mb-1">المبلغ (<span id="currency-symbol">د.إ</span>)</label>
                            <input type="number" id="expense-amount" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500" placeholder="0.00", min="0", step="0.01", required>
                        </div>
                        
                        <div>
                            <label for="expense-date" class="block text-sm font-medium text-gray-700 mb-1">التاريخ</label>
                            <input type="date" id="expense-date" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500" required>
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">الفئة</label>
                        <div class="grid grid-cols-4 gap-3">
                            <div>
                                <input type="radio", name="expense-category", id="category-food", value="food", class="hidden peer", checked>
                                <label for="category-food", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-utensils text-2xl text-orange-500 mb-1 category-icon"></i>
                                    <span class="text-sm">طعام</span>
                                </label>
                            </div>
                            
                            <div>
                                <input type="radio", name="expense-category", id="category-transport", value="transport", class="hidden peer">
                                <label for="category-transport", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-car text-2xl text-blue-500 mb-1 category-icon"></i>
                                    <span class="text-sm">مواصلات</span>
                                </label>
                            </div>
                            
                            <div>
                                <input type="radio", name="expense-category", id="category-shopping", value="shopping", class="hidden peer">
                                <label for="category-shopping", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-shopping-bag text-2xl text-purple-500 mb-1 category-icon"></i>
                                    <span class="text-sm">تسوق</span>
                                </label>
                            </div>
                            
                            <div>
                                <input type="radio", name="expense-category", id="category-bills", value="bills", class="hidden peer">
                                <label for="category-bills", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-file-invoice-dollar text-2xl text-green-500 mb-1 category-icon"></i>
                                    <span class="text-sm">فواتير</span>
                                </label>
                            </div>
                            
                            <div>
                                <input type="radio", name="expense-category", id="category-entertainment", value="entertainment", class="hidden peer">
                                <label for="category-entertainment", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-film text-2xl text-red-500 mb-1 category-icon"></i>
                                    <span class="text-sm">ترفيه</span>
                                </label>
                            </div>
                            
                            <div>
                                <input type="radio", name="expense-category", id="category-health", value="health", class="hidden peer">
                                <label for="category-health", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-heartbeat text-2xl text-pink-500 mb-1 category-icon"></i>
                                    <span class="text-sm">صحة</span>
                                </label>
                            </div>
                            
                            <div>
                                <input type="radio", name="expense-category", id="category-education", value="education", class="hidden peer">
                                <label for="category-education", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-book text-2xl text-yellow-500 mb-1 category-icon"></i>
                                    <span class="text-sm">تعليم</span>
                                </label>
                            </div>
                            
                            <div>
                                <input type="radio", name="expense-category", id="category-other", value="other", class="hidden peer">
                                <label for="category-other", class="flex flex-col items-center p-3 border border-gray-300 rounded-lg cursor-pointer peer-checked:border-indigo-500 peer-checked:bg-indigo-50">
                                    <i class="fas fa-ellipsis-h text-2xl text-gray-500 mb-1 category-icon"></i>
                                    <span class="text-sm">أخرى</span>
                                </label>
                            </div>
                        </div>
                    </div>
                    
                    <div class="pt-4">
                        <button type="submit", class="w-full bg-indigo-600 text-white py-3 rounded-lg font-medium hover:bg-indigo-700 transition flex items-center justify-center">
                            <i class="fas fa-plus ml-2"></i>
                            إضافة المصروف
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Edit Budget Modal -->
    <div id="edit-budget-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-xl shadow-xl w-full max-w-md mx-4 fade-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-800">تعديل الميزانية الشهرية</h3>
                    <button id="close-budget-modal", class="text-gray-500 hover:text-gray-700">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <form id="edit-budget-form", class="space-y-4">
                    <div>
                        <label for="monthly-budget", class="block text-sm font-medium text-gray-700 mb-1">الميزانية الشهرية (<span id="budget-currency-symbol">د.إ</span>)</label>
                        <input type="number", id="monthly-budget", class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500" placeholder="0.00", min="0", step="0.01", required>
                    </div>
                    
                    <div class="pt-4">
                        <button type="submit", class="w-full bg-indigo-600 text-white py-3 rounded-lg font-medium hover:bg-indigo-700 transition flex items-center justify-center">
                            <i class="fas fa-save ml-2"></i>
                            حفظ الميزانية
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Expense Details Modal -->
    <div id="expense-modal", class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-xl shadow-xl w-full max-w-md mx-4 fade-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-800">تفاصيل المصروف</h3>
                    <button id="close-modal", class="text-gray-500 hover:text-gray-700">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div class="space-y-4">
                    <div>
                        <p class="text-sm text-gray-500">الوصف</p>
                        <p id="modal-description", class="font-medium text-lg"></p>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <p class="text-sm text-gray-500">المبلغ</p>
                            <p id="modal-amount", class="font-medium text-lg"></p>
                        </div>
                        
                        <div>
                            <p class="text-sm text-gray-500">التاريخ</p>
                            <p id="modal-date", class="font-medium text-lg"></p>
                        </div>
                    </div>
                    
                    <div>
                        <p class="text-sm text-gray-500">الفئة</p>
                        <div class="flex items-center mt-1">
                            <div id="modal-category-icon", class="bg-gray-100 p-2 rounded-full mr-2"></div>
                            <p id="modal-category", class="font-medium text-lg"></p>
                        </div>
                    </div>
                </div>
                
                <div class="mt-8 flex justify-end space-x-3 space-x-reverse">
                    <button id="delete-expense", class="bg-red-100 text-red-600 px-4 py-2 rounded-lg hover:bg-red-200 transition flex items-center">
                        <i class="fas fa-trash ml-2"></i>
                        حذف
                    </button>
                    <button id="edit-expense", class="bg-indigo-100 text-indigo-600 px-4 py-2 rounded-lg hover:bg-indigo-200 transition flex items-center">
                        <i class="fas fa-edit ml-2"></i>
                        تعديل
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Confirmation Dialog -->
    <div id="confirmation-dialog", class="fixed inset-0 z-50 flex items-center justify-center hidden modal-overlay">
        <div class="bg-white rounded-xl shadow-xl w-full max-w-md mx-4 confirmation-dialog">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-gray-800 flex items-center">
                        <i class="fas fa-exclamation-triangle text-yellow-500 ml-2"></i>
                        تأكيد الحذف
                    </h3>
                    <button id="close-confirmation", class="text-gray-500 hover:text-gray-700">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div class="mb-6">
                    <p class="text-gray-700">هل أنت متأكد أنك تريد حذف هذا المصروف؟</p>
                    <p class="text-sm text-gray-500 mt-2">لا يمكن التراجع عن هذه العملية بعد التنفيذ.</p>
                </div>
                
                <div class="flex justify-end space-x-3 space-x-reverse">
                    <button id="cancel-delete", class="bg-gray-100 text-gray-700 px-4 py-2 rounded-lg hover:bg-gray-200 transition flex items-center">
                        <i class="fas fa-times ml-2"></i>
                        إلغاء
                    </button>
                    <button id="confirm-delete", class="bg-red-600 text-white px-4 py-2 rounded-lg hover:bg-red-700 transition flex items-center">
                        <i class="fas fa-trash ml-2"></i>
                        نعم، احذف
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast", class="fixed bottom-4 right-4 hidden">
        <div class="bg-gray-800 text-white px-6 py-3 rounded-lg shadow-lg flex items-center toast">
            <i id="toast-icon", class="fas fa-check-circle ml-2"></i>
            <span id="toast-message"></span>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM Elements
            const yearSelector = document.getElementById('year-selector');
            const monthSelector = document.getElementById('month-selector');
            const currencySelector = document.getElementById('currency-selector');
            const addExpenseBtn = document.getElementById('add-expense-btn');
            const addFirstExpenseBtn = document.getElementById('add-first-expense');
            const editBudgetBtn = document.getElementById('edit-budget-btn');
            const totalBudgetEl = document.getElementById('total-budget');
            const totalExpensesEl = document.getElementById('total-expenses');
            const expensesPercentageEl = document.getElementById('expenses-percentage');
            const remainingBudgetEl = document.getElementById('remaining-budget');
            const remainingProgressEl = document.getElementById('remaining-progress');
            const totalSavingsEl = document.getElementById('total-savings');
            const savingsPercentageEl = document.getElementById('savings-percentage');
            const monthSavingsProgressEl = document.getElementById('month-savings-progress');
            const categoriesListEl = document.getElementById('categories-list');
            const expensesTableBodyEl = document.getElementById('expenses-table-body');
            const noExpensesEl = document.getElementById('no-expenses');
            const currencySymbolEl = document.getElementById('currency-symbol');
            const budgetCurrencySymbolEl = document.getElementById('budget-currency-symbol');
            const expenseDateInput = document.getElementById('expense-date');
            const searchInput = document.getElementById('search-expenses');
            const searchOptions = document.querySelectorAll('.search-option');
            const categoryTabs = document.querySelectorAll('.category-tab');
            
            // Modal Elements
            const addExpenseModal = document.getElementById('add-expense-modal');
            const editBudgetModal = document.getElementById('edit-budget-modal');
            const expenseModal = document.getElementById('expense-modal');
            const confirmationDialog = document.getElementById('confirmation-dialog');
            
            // Form Elements
            const addExpenseForm = document.getElementById('add-expense-form');
            const editBudgetForm = document.getElementById('edit-budget-form');
            
            // Buttons
            const closeAddModalBtn = document.getElementById('close-add-modal');
            const closeBudgetModalBtn = document.getElementById('close-budget-modal');
            const closeModalBtn = document.getElementById('close-modal');
            const closeConfirmationBtn = document.getElementById('close-confirmation');
            const cancelDeleteBtn = document.getElementById('cancel-delete');
            const confirmDeleteBtn = document.getElementById('confirm-delete');
            const deleteExpenseBtn = document.getElementById('delete-expense');
            const editExpenseBtn = document.getElementById('edit-expense');
            
            // Filter Buttons
            const filterMonthBtn = document.getElementById('filter-month');
            const filterWeekBtn = document.getElementById('filter-week');
            const filterTodayBtn = document.getElementById('filter-today');
            
            // Toast Elements
            const toastEl = document.getElementById('toast');
            const toastIconEl = document.getElementById('toast-icon');
            const toastMessageEl = document.getElementById('toast-message');
            
            // Data
            let budgets = {}; // Object to store budgets for each month/year
            let expenses = [];
            let savings = 0;
            let selectedExpenseId = null;
            let currentFilter = 'month'; // Default to month view
            let currentCurrency = 'د.إ'; // Default to AED
            let currentYear = new Date().getFullYear();
            let currentMonth = new Date().getMonth();
            let searchQuery = '';
            let searchScope = 'current'; // 'current' or 'all'
            let selectedCategory = 'all'; // Default to all categories
            
            // Initialize
            init();
            
            function init() {
                // Set current month and year
                const now = new Date();
                monthSelector.value = now.getMonth();
                
                // Populate year selector
                populateYearSelector();
                
                // Set current year
                yearSelector.value = currentYear;
                
                // Load data from localStorage
                loadData();
                
                // Set today's date as default and configure date input
                setupDateInput();
                
                // Update UI
                updateBudgetSummary();
                updateSavings();
                updateCategoryAmounts();
                renderExpensesList();
                
                // Setup event listeners
                setupEventListeners();
            }
            
            function setupDateInput() {
                const now = new Date();
                const firstDay = new Date(currentYear, currentMonth, 1);
                const lastDay = new Date(currentYear, currentMonth + 1, 0);
                
                // Format dates for input (YYYY-MM-DD)
                const minDate = formatDateForInput(firstDay);
                const maxDate = formatDateForInput(lastDay);
                const todayDate = formatDateForInput(now);
                
                // Set attributes on date input
                expenseDateInput.min = minDate;
                expenseDateInput.max = maxDate;
                expenseDateInput.value = todayDate;
                
                // Set today's date if it's in the current month, otherwise set first day of month
                if (now.getMonth() === currentMonth && now.getFullYear() === currentYear) {
                    expenseDateInput.valueAsDate = now;
                } else {
                    expenseDateInput.value = minDate;
                }
            }
            
            function formatDateForInput(date) {
                const year = date.getFullYear();
                const month = String(date.getMonth() + 1).padStart(2, '0');
                const day = String(date.getDate()).padStart(2, '0');
                return `${year}-${month}-${day}`;
            }
            
            function populateYearSelector() {
                const currentYear = new Date().getFullYear();
                const years = [];
                
                // Add current year and 5 years before and after
                for (let i = currentYear - 5; i <= currentYear + 5; i++) {
                    years.push(i);
                }
                
                // Populate dropdown
                yearSelector.innerHTML = years.map(year => 
                    `<option value="${year}">${year}</option>`
                ).join('');
            }
            
            function loadData() {
                // Load budgets from localStorage
                const savedBudgets = localStorage.getItem('monthlyBudgets');
                if (savedBudgets) {
                    budgets = JSON.parse(savedBudgets);
                }
                
                // Load expenses from localStorage
                const savedExpenses = localStorage.getItem('expenses');
                if (savedExpenses) {
                    expenses = JSON.parse(savedExpenses);
                }
                
                // Load savings from localStorage
                const savedSavings = localStorage.getItem('savings');
                if (savedSavings) {
                    savings = parseFloat(savedSavings);
                }
                
                // Load currency from localStorage
                const savedCurrency = localStorage.getItem('currency');
                if (savedCurrency) {
                    currentCurrency = savedCurrency;
                    currencySelector.value = currentCurrency;
                    updateCurrencySymbols();
                }
                
                // Load year from localStorage
                const savedYear = localStorage.getItem('selectedYear');
                if (savedYear) {
                    currentYear = parseInt(savedYear);
                    yearSelector.value = currentYear;
                }
                
                // Load month from localStorage
                const savedMonth = localStorage.getItem('selectedMonth');
                if (savedMonth) {
                    currentMonth = parseInt(savedMonth);
                    monthSelector.value = currentMonth;
                }
                
                // Set current budget in edit modal if exists
                const budgetKey = `${currentYear}-${currentMonth}`;
                if (budgets[budgetKey]) {
                    document.getElementById('monthly-budget').value = budgets[budgetKey];
                }
            }
            
            function saveData() {
                // Save budgets to localStorage
                localStorage.setItem('monthlyBudgets', JSON.stringify(budgets));
                
                // Save expenses to localStorage
                localStorage.setItem('expenses', JSON.stringify(expenses));
                
                // Save savings to localStorage
                localStorage.setItem('savings', savings.toString());
                
                // Save currency to localStorage
                localStorage.setItem('currency', currentCurrency);
                
                // Save selected year and month
                localStorage.setItem('selectedYear', currentYear.toString());
                localStorage.setItem('selectedMonth', currentMonth.toString());
            }
            
            function updateCurrencySymbols() {
                currencySymbolEl.textContent = currentCurrency;
                budgetCurrencySymbolEl.textContent = currentCurrency;
            }
            
            function setupEventListeners() {
                // Add expense button
                addExpenseBtn.addEventListener('click', openAddExpenseModal);
                addFirstExpenseBtn.addEventListener('click', openAddExpenseModal);
                
                // Edit budget button
                editBudgetBtn.addEventListener('click', openEditBudgetModal);
                
                // Close modals
                closeAddModalBtn.addEventListener('click', closeAddExpenseModal);
                closeBudgetModalBtn.addEventListener('click', closeEditBudgetModal);
                closeModalBtn.addEventListener('click', closeExpenseModal);
                closeConfirmationBtn.addEventListener('click', closeConfirmationDialog);
                
                // Forms
                addExpenseForm.addEventListener('submit', handleAddExpense);
                editBudgetForm.addEventListener('submit', handleEditBudget);
                
                // Confirmation dialog
                cancelDeleteBtn.addEventListener('click', closeConfirmationDialog);
                confirmDeleteBtn.addEventListener('click', deleteSelectedExpense);
                
                // Delete expense button
                deleteExpenseBtn.addEventListener('click', function(e) {
                    e.stopPropagation();
                    showDeleteConfirmation();
                });
                
                // Search scope options
                searchOptions.forEach(option => {
                    option.addEventListener('click', function() {
                        searchOptions.forEach(opt => opt.classList.remove('active'));
                        this.classList.add('active');
                        searchScope = this.dataset.scope;
                        renderExpensesList();
                    });
                });
                
                // Category tabs
                categoryTabs.forEach(tab => {
                    tab.addEventListener('click', function() {
                        categoryTabs.forEach(t => t.classList.remove('active'));
                        this.classList.add('active');
                        selectedCategory = this.dataset.category;
                        renderExpensesList();
                    });
                });
                
                // Filter buttons
                filterMonthBtn.addEventListener('click', () => setFilter('month'));
                filterWeekBtn.addEventListener('click', () => setFilter('week'));
                filterTodayBtn.addEventListener('click', () => setFilter('today'));
                
                // Currency selector
                currencySelector.addEventListener('change', function() {
                    currentCurrency = this.value;
                    saveData();
                    updateCurrencySymbols();
                    updateBudgetSummary();
                    updateSavings();
                    updateCategoryAmounts();
                    renderExpensesList();
                });
                
                // Month selector
                monthSelector.addEventListener('change', function() {
                    currentMonth = parseInt(this.value);
                    saveData();
                    setupDateInput(); // Update date input constraints
                    updateBudgetSummary();
                    updateSavings();
                    renderExpensesList();
                });
                
                // Year selector
                yearSelector.addEventListener('change', function() {
                    currentYear = parseInt(this.value);
                    saveData();
                    setupDateInput(); // Update date input constraints
                    updateBudgetSummary();
                    updateSavings();
                    renderExpensesList();
                });
                
                // Search input
                searchInput.addEventListener('input', function() {
                    searchQuery = this.value.trim().toLowerCase();
                    renderExpensesList();
                });
            }
            
            function openAddExpenseModal() {
                addExpenseModal.classList.remove('hidden');
                document.getElementById('expense-description').focus();
            }
            
            function closeAddExpenseModal() {
                addExpenseModal.classList.add('hidden');
                addExpenseForm.reset();
                setupDateInput(); // Reset date input when closing modal
            }
            
            function openEditBudgetModal() {
                editBudgetModal.classList.remove('hidden');
                const budgetKey = `${currentYear}-${currentMonth}`;
                document.getElementById('monthly-budget').value = budgets[budgetKey] || '';
                document.getElementById('monthly-budget').focus();
            }
            
            function closeEditBudgetModal() {
                editBudgetModal.classList.add('hidden');
            }
            
            function openExpenseModal(id) {
                const expense = expenses.find(exp => exp.id === id);
                if (!expense) return;
                
                selectedExpenseId = id;
                
                // Set modal content
                document.getElementById('modal-description').textContent = expense.description;
                document.getElementById('modal-amount').textContent = `${expense.amount.toFixed(2)} ${currentCurrency}`;
                document.getElementById('modal-date').textContent = formatDate(expense.date);
                document.getElementById('modal-category').textContent = getCategoryName(expense.category);
                
                // Set category icon
                const iconEl = document.getElementById('modal-category-icon');
                iconEl.innerHTML = getCategoryIcon(expense.category);
                
                // Show modal
                expenseModal.classList.remove('hidden');
            }
            
            function closeExpenseModal() {
                expenseModal.classList.add('hidden');
                selectedExpenseId = null;
            }
            
            function closeConfirmationDialog() {
                confirmationDialog.classList.add('hidden');
            }
            
            function showDeleteConfirmation() {
                if (!selectedExpenseId) return;
                
                // Add shake animation to grab attention
                const dialogContent = confirmationDialog.querySelector('.confirmation-dialog');
                dialogContent.classList.add('shake');
                
                // Remove animation after it completes
                setTimeout(() => {
                    dialogContent.classList.remove('shake');
                }, 500);
                
                confirmationDialog.classList.remove('hidden');
            }
            
            function handleAddExpense(e) {
                e.preventDefault();
                
                const description = document.getElementById('expense-description').value.trim();
                const amount = parseFloat(document.getElementById('expense-amount').value);
                const date = document.getElementById('expense-date').value;
                const category = document.querySelector('input[name="expense-category"]:checked').value;
                
                // Validate amount
                if (isNaN(amount) || amount <= 0) {
                    showToast('الرجاء إدخال مبلغ صحيح', 'error');
                    return;
                }
                
                // Validate date is within current month
                const selectedDate = new Date(date);
                if (selectedDate.getMonth() !== currentMonth || selectedDate.getFullYear() !== currentYear) {
                    showToast('يجب أن يكون التاريخ ضمن الشهر المحدد', 'error');
                    return;
                }
                
                // Create new expense
                const newExpense = {
                    id: Date.now().toString(),
                    description,
                    amount,
                    date,
                    category,
                    year: selectedDate.getFullYear(),
                    month: selectedDate.getMonth()
                };
                
                // Add to expenses array
                expenses.unshift(newExpense);
                
                // Save data
                saveData();
                
                // Update UI
                updateBudgetSummary();
                updateSavings();
                updateCategoryAmounts();
                renderExpensesList();
                
                // Close modal and show success message
                closeAddExpenseModal();
                showToast('تم إضافة المصروف بنجاح', 'success');
            }
            
            function handleEditBudget(e) {
                e.preventDefault();
                
                const newBudget = parseFloat(document.getElementById('monthly-budget').value);
                
                // Validate budget
                if (isNaN(newBudget) || newBudget < 0) {
                    showToast('الرجاء إدخال ميزانية صحيحة', 'error');
                    return;
                }
                
                // Update budget for current month/year
                const budgetKey = `${currentYear}-${currentMonth}`;
                budgets[budgetKey] = newBudget;
                
                // Save data
                saveData();
                
                // Update UI
                updateBudgetSummary();
                updateSavings();
                
                // Close modal and show success message
                closeEditBudgetModal();
                showToast('تم تحديث الميزانية بنجاح', 'success');
            }
            
            function deleteSelectedExpense() {
                if (!selectedExpenseId) return;
                
                // Find expense index
                const expenseIndex = expenses.findIndex(exp => exp.id === selectedExpenseId);
                
                if (expenseIndex === -1) {
                    showToast('لم يتم العثور على المصروف', 'error');
                    return;
                }
                
                // Remove expense from array
                expenses.splice(expenseIndex, 1);
                
                // Save data
                saveData();
                
                // Update UI
                updateBudgetSummary();
                updateSavings();
                updateCategoryAmounts();
                renderExpensesList();
                
                // Close modals and show success message
                closeExpenseModal();
                closeConfirmationDialog();
                showToast('تم حذف المصروف بنجاح', 'success');
            }
            
            function updateBudgetSummary() {
                const budgetKey = `${currentYear}-${currentMonth}`;
                const currentBudget = budgets[budgetKey] || 0;
                
                // Calculate total expenses for current month and year
                const totalExpenses = expenses
                    .filter(exp => exp.year === currentYear && exp.month === currentMonth)
                    .reduce((sum, exp) => sum + exp.amount, 0);
                
                // Update UI
                totalBudgetEl.textContent = `${currentBudget.toFixed(2)} ${currentCurrency}`;
                totalExpensesEl.textContent = `${totalExpenses.toFixed(2)} ${currentCurrency}`;
                
                // Calculate percentage
                const percentage = currentBudget > 0 ? (totalExpenses / currentBudget * 100) : 0;
                expensesPercentageEl.textContent = `${percentage.toFixed(1)}% من الميزانية`;
                
                // Calculate remaining budget
                const remaining = currentBudget - totalExpenses;
                remainingBudgetEl.textContent = `${remaining.toFixed(2)} ${currentCurrency}`;
                
                // Update progress bar
                const remainingPercentage = currentBudget > 0 ? (remaining / currentBudget * 100) : 100;
                remainingProgressEl.style.width = `${Math.max(0, Math.min(100, remainingPercentage))}%`;
                
                // Change color based on remaining percentage
                if (remainingPercentage < 20) {
                    remainingProgressEl.classList.remove('bg-green-500', 'bg-yellow-500');
                    remainingProgressEl.classList.add('bg-red-500');
                } else if (remainingPercentage < 50) {
                    remainingProgressEl.classList.remove('bg-green-500', 'bg-red-500');
                    remainingProgressEl.classList.add('bg-yellow-500');
                } else {
                    remainingProgressEl.classList.remove('bg-red-500', 'bg-yellow-500');
                    remainingProgressEl.classList.add('bg-green-500');
                }
                
                // Show warning if budget is exceeded
                if (remaining < 0) {
                    showToast('لقد تجاوزت الميزانية المحددة!', 'warning');
                }
            }
            
            function updateSavings() {
                // Calculate total expenses for all months
                const allExpenses = expenses.reduce((sum, exp) => sum + exp.amount, 0);
                
                // Calculate total budget for all months
                let allBudgets = 0;
                for (const key in budgets) {
                    allBudgets += budgets[key];
                }
                
                // Calculate savings (total budget - total expenses)
                savings = Math.max(0, allBudgets - allExpenses);
                
                // Update UI
                totalSavingsEl.textContent = `${savings.toFixed(2)} ${currentCurrency}`;
                
                // Calculate savings percentage
                const savingsPercentage = allBudgets > 0 ? (savings / allBudgets * 100) : 0;
                savingsPercentageEl.textContent = `${savingsPercentage.toFixed(1)}% من الميزانية`;
                
                // Calculate current month savings percentage
                const budgetKey = `${currentYear}-${currentMonth}`;
                const currentBudget = budgets[budgetKey] || 0;
                
                const currentMonthExpenses = expenses
                    .filter(exp => exp.year === currentYear && exp.month === currentMonth)
                    .reduce((sum, exp) => sum + exp.amount, 0);
                
                const currentMonthRemaining = currentBudget - currentMonthExpenses;
                const currentMonthSavingsPercentage = currentBudget > 0 ? (currentMonthRemaining / currentBudget * 100) : 0;
                
                // Update month savings progress bar
                monthSavingsProgressEl.style.width = `${Math.max(0, Math.min(100, currentMonthSavingsPercentage))}%`;
                
                // Save data
                localStorage.setItem('savings', savings.toString());
            }
            
            function updateCategoryAmounts() {
                const budgetKey = `${currentYear}-${currentMonth}`;
                const currentBudget = budgets[budgetKey] || 0;
                
                // Group expenses by category for current month and year
                const categories = {
                    food: { name: 'طعام', icon: 'fa-utensils', color: 'text-orange-500', bg: 'bg-orange-100', total: 0 },
                    transport: { name: 'مواصلات', icon: 'fa-car', color: 'text-blue-500', bg: 'bg-blue-100', total: 0 },
                    shopping: { name: 'تسوق', icon: 'fa-shopping-bag', color: 'text-purple-500', bg: 'bg-purple-100', total: 0 },
                    bills: { name: 'فواتير', icon: 'fa-file-invoice-dollar', color: 'text-green-500', bg: 'bg-green-100', total: 0 },
                    entertainment: { name: 'ترفيه', icon: 'fa-film', color: 'text-red-500', bg: 'bg-red-100', total: 0 },
                    health: { name: 'صحة', icon: 'fa-heartbeat', color: 'text-pink-500', bg: 'bg-pink-100', total: 0 },
                    education: { name: 'تعليم', icon: 'fa-book', color: 'text-yellow-500', bg: 'bg-yellow-100', total: 0 },
                    other: { name: 'أخرى', icon: 'fa-ellipsis-h', color: 'text-gray-500', bg: 'bg-gray-100', total: 0 }
                };
                
                // Calculate totals for each category
                expenses
                    .filter(exp => exp.year === currentYear && exp.month === currentMonth)
                    .forEach(exp => {
                        if (categories[exp.category]) {
                            categories[exp.category].total += exp.amount;
                        }
                    });
                
                // Sort categories by total (descending)
                const sortedCategories = Object.entries(categories)
                    .sort((a, b) => b[1].total - a[1].total)
                    .filter(cat => cat[1].total > 0);
                
                // Update UI
                categoriesListEl.innerHTML = '';
                
                if (sortedCategories.length === 0) {
                    categoriesListEl.innerHTML = `
                        <div class="text-center py-4 text-gray-500">
                            <i class="fas fa-tags text-2xl mb-2"></i>
                            <p>لا توجد مصروفات مسجلة بعد</p>
                        </div>
                    `;
                    return;
                }
                
                sortedCategories.forEach(([key, cat]) => {
                    const percentage = currentBudget > 0 ? (cat.total / currentBudget * 100) : 0;
                    
                    const categoryEl = document.createElement('div');
                    categoryEl.className = 'flex items-center justify-between p-3 bg-gray-50 rounded-lg';
                    categoryEl.innerHTML = `
                        <div class="flex items-center">
                            <div class="${cat.bg} ${cat.color} p-2 rounded-full mr-3">
                                <i class="fas ${cat.icon}"></i>
                            </div>
                            <div>
                                <p class="font-medium">${cat.name}</p>
                                <p class="text-sm text-gray-500">${percentage.toFixed(1)}% من الميزانية</p>
                            </div>
                        </div>
                        <div class="font-bold">${cat.total.toFixed(2)} ${currentCurrency}</div>
                    `;
                    
                    categoriesListEl.appendChild(categoryEl);
                });
            }
            
            function renderExpensesList() {
                // Filter expenses based on current filter, month and year
                let filteredExpenses = searchScope === 'current' 
                    ? expenses.filter(exp => exp.year === currentYear && exp.month === currentMonth)
                    : expenses; // Show all expenses when search scope is 'all'
                
                // Apply category filter if not 'all'
                if (selectedCategory !== 'all') {
                    filteredExpenses = filteredExpenses.filter(exp => exp.category === selectedCategory);
                }
                
                // Apply search filter if search query exists
                if (searchQuery) {
                    filteredExpenses = filteredExpenses.filter(exp => 
                        exp.description.toLowerCase().includes(searchQuery)
                    );
                }
                
                const now = new Date();
                const todayStr = now.toISOString().split('T')[0];
                const oneWeekAgo = new Date(now.getTime() - 7 * 24 * 60 * 60 * 1000);
                
                if (currentFilter === 'week') {
                    filteredExpenses = filteredExpenses.filter(exp => {
                        const expenseDate = new Date(exp.date);
                        return expenseDate >= oneWeekAgo;
                    });
                } else if (currentFilter === 'today') {
                    filteredExpenses = filteredExpenses.filter(exp => exp.date === todayStr);
                }
                
                // Sort expenses by date (newest first)
                filteredExpenses.sort((a, b) => new Date(b.date) - new Date(a.date));
                
                // Update UI
                expensesTableBodyEl.innerHTML = '';
                
                if (filteredExpenses.length === 0) {
                    noExpensesEl.classList.remove('hidden');
                    return;
                }
                
                noExpensesEl.classList.add('hidden');
                
                filteredExpenses.forEach(exp => {
                    const row = document.createElement('tr');
                    row.className = 'border-b hover:bg-gray-50 expense-card cursor-pointer';
                    row.addEventListener('click', () => openExpenseModal(exp.id));
                    
                    // Format month name for display when showing all months
                    const monthName = searchScope === 'all' 
                        ? getMonthName(exp.month) + ' ' + exp.year
                        : formatDateShort(exp.date);
                    
                    row.innerHTML = `
                        <td class="py-4 text-right font-medium">
                            <div class="flex items-center">
                                <div class="${getCategoryBgColor(exp.category)} ${getCategoryTextColor(exp.category)} p-2 rounded-full ml-2">
                                    <i class="fas ${getCategoryIconClass(exp.category)}"></i>
                                </div>
                                ${exp.description}
                            </div>
                        </td>
                        <td class="py-4 text-right font-bold">${exp.amount.toFixed(2)} ${currentCurrency}</td>
                        <td class="py-4 text-right text-gray-500">
                            ${searchScope === 'all' ? monthName : formatDateShort(exp.date)}
                        </td>
                        <td class="py-4 text-right">
                            <button class="text-gray-400 hover:text-gray-600" onclick="event.stopPropagation(); openExpenseModal('${exp.id}')">
                                <i class="fas fa-ellipsis-v"></i>
                            </button>
                        </td>
                    `;
                    
                    expensesTableBodyEl.appendChild(row);
                });
            }
            
            function getMonthName(monthIndex) {
                const months = [
                    'يناير', 'فبراير', 'مارس', 'أبريل', 'مايو', 'يونيو',
                    'يوليو', 'أغسطس', 'سبتمبر', 'أكتوبر', 'نوفمبر', 'ديسمبر'
                ];
                return months[monthIndex];
            }
            
            function setFilter(filter) {
                currentFilter = filter;
                
                // Update active filter button
                filterMonthBtn.classList.remove('bg-indigo-100', 'text-indigo-600');
                filterWeekBtn.classList.remove('bg-indigo-100', 'text-indigo-600');
                filterTodayBtn.classList.remove('bg-indigo-100', 'text-indigo-600');
                
                filterMonthBtn.classList.add('bg-gray-100', 'text-gray-600');
                filterWeekBtn.classList.add('bg-gray-100', 'text-gray-600');
                filterTodayBtn.classList.add('bg-gray-100', 'text-gray-600');
                
                if (filter === 'month') {
                    filterMonthBtn.classList.remove('bg-gray-100', 'text-gray-600');
                    filterMonthBtn.classList.add('bg-indigo-100', 'text-indigo-600');
                } else if (filter === 'week') {
                    filterWeekBtn.classList.remove('bg-gray-100', 'text-gray-600');
                    filterWeekBtn.classList.add('bg-indigo-100', 'text-indigo-600');
                } else if (filter === 'today') {
                    filterTodayBtn.classList.remove('bg-gray-100', 'text-gray-600');
                    filterTodayBtn.classList.add('bg-indigo-100', 'text-indigo-600');
                }
                
                // Update expenses list
                renderExpensesList();
            }
            
            function showToast(message, type) {
                // Set icon and color based on type
                let icon = 'fa-check-circle';
                let bgColor = 'bg-gray-800';
                
                if (type === 'success') {
                    icon = 'fa-check-circle';
                    bgColor = 'bg-green-600';
                } else if (type === 'error') {
                    icon = 'fa-exclamation-circle';
                    bgColor = 'bg-red-600';
                } else if (type === 'warning') {
                    icon = 'fa-exclamation-triangle';
                    bgColor = 'bg-yellow-600';
                }
                
                // Update toast content
                toastIconEl.className = `fas ${icon} ml-2`;
                toastMessageEl.textContent = message;
                toastEl.firstElementChild.className = `${bgColor} text-white px-6 py-3 rounded-lg shadow-lg flex items-center toast`;
                
                // Show toast
                toastEl.classList.remove('hidden');
                
                // Hide after 3 seconds
                setTimeout(() => {
                    toastEl.classList.add('hidden');
                }, 3000);
            }
            
            // Helper functions
            function formatDate(dateStr) {
                const date = new Date(dateStr);
                const options = { year: 'numeric', month: 'long', day: 'numeric' };
                return date.toLocaleDateString('ar-EG', options);
            }
            
            function formatDateShort(dateStr) {
                const date = new Date(dateStr);
                const day = date.getDate();
                const month = date.getMonth() + 1;
                return `${day}/${month}`;
            }
            
            function getCategoryName(category) {
                const names = {
                    food: 'طعام',
                    transport: 'مواصلات',
                    shopping: 'تسوق',
                    bills: 'فواتير',
                    entertainment: 'ترفيه',
                    health: 'صحة',
                    education: 'تعليم',
                    other: 'أخرى'
                };
                return names[category] || 'أخرى';
            }
            
            function getCategoryIcon(category) {
                const icons = {
                    food: '<i class="fas fa-utensils text-orange-500"></i>',
                    transport: '<i class="fas fa-car text-blue-500"></i>',
                    shopping: '<i class="fas fa-shopping-bag text-purple-500"></i>',
                    bills: '<i class="fas fa-file-invoice-dollar text-green-500"></i>',
                    entertainment: '<i class="fas fa-film text-red-500"></i>',
                    health: '<i class="fas fa-heartbeat text-pink-500"></i>',
                    education: '<i class="fas fa-book text-yellow-500"></i>',
                    other: '<i class="fas fa-ellipsis-h text-gray-500"></i>'
                };
                return icons[category] || icons.other;
            }
            
            function getCategoryIconClass(category) {
                const icons = {
                    food: 'fa-utensils',
                    transport: 'fa-car',
                    shopping: 'fa-shopping-bag',
                    bills: 'fa-file-invoice-dollar',
                    entertainment: 'fa-film',
                    health: 'fa-heartbeat',
                    education: 'fa-book',
                    other: 'fa-ellipsis-h'
                };
                return icons[category] || icons.other;
            }
            
            function getCategoryBgColor(category) {
                const colors = {
                    food: 'bg-orange-100',
                    transport: 'bg-blue-100',
                    shopping: 'bg-purple-100',
                    bills: 'bg-green-100',
                    entertainment: 'bg-red-100',
                    health: 'bg-pink-100',
                    education: 'bg-yellow-100',
                    other: 'bg-gray-100'
                };
                return colors[category] || colors.other;
            }
            
            function getCategoryTextColor(category) {
                const colors = {
                    food: 'text-orange-500',
                    transport: 'text-blue-500',
                    shopping: 'text-purple-500',
                    bills: 'text-green-500',
                    entertainment: 'text-red-500',
                    health: 'text-pink-500',
                    education: 'text-yellow-500',
                    other: 'text-gray-500'
                };
                return colors[category] || colors.other;
            }
            
            // Expose functions to global scope for inline event handlers
            window.openExpenseModal = openExpenseModal;
        });
    </script>
</body>
</html>
