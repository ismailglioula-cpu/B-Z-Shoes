<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Premium Shoe Store | Shop English</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f5f5f5;
        }

        .product-card {
            background-color: #ffffff;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }

        .product-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 20px -5px rgba(0, 0, 0, 0.1);
        }

        .line-clamp-2 {
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }

        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }

        .whatsapp-btn {
            background-color: #25D366;
            transition: opacity 0.2s;
        }

        .price-text {
            color: #ff0000;
        }

        /* AI Chat Bubble Animation */
        @keyframes pulse-border {
            0% { transform: scale(1); opacity: 1; }
            100% { transform: scale(1.2); opacity: 0; }
        }
        .ai-pulse::before {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            border: 2px solid #ea580c;
            border-radius: 9999px;
            animation: pulse-border 2s infinite;
        }
    </style>
</head>
<body class="text-gray-800">

    <!-- Top Promo Bar -->
    <div class="bg-black text-white text-xs py-2 px-4 flex justify-between items-center sticky top-0 z-50">
        <div class="flex items-center space-x-4">
            <span><i class="fas fa-truck mr-1"></i> Free Shipping > </span>
            <span class="hidden md:inline"><i class="fas fa-shield-alt mr-1"></i> Delivery Guarantee</span>
        </div>
        <div class="flex items-center space-x-4">
            <span>Download Store App</span>
        </div>
    </div>

    <!-- Main Header -->
    <header class="bg-white border-b sticky top-8 z-40">
        <div class="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between gap-4">
            <div class="text-2xl font-bold text-orange-600 tracking-tighter shrink-0 cursor-pointer" onclick="resetSearch()">
                SHOEHUB
            </div>

            <div class="flex-1 max-w-2xl relative">
                <input type="text" id="searchInput" placeholder="Search for shoes..." 
                    class="w-full border-2 border-black rounded-full py-2 px-6 pr-12 focus:outline-none focus:border-orange-500">
                <button onclick="handleSearch()" class="absolute right-2 top-1/2 -translate-y-1/2 bg-black text-white p-2 rounded-full w-8 h-8 flex items-center justify-center">
                    <i class="fas fa-search text-sm"></i>
                </button>
            </div>

            <div class="flex items-center space-x-6 text-sm font-medium shrink-0">
                <div class="hidden lg:flex flex-col items-center cursor-pointer">
                    <i class="far fa-user text-xl"></i>
                    <span>Account</span>
                </div>
                <div class="relative cursor-pointer">
                    <i class="fas fa-shopping-cart text-xl"></i>
                    <span id="cartCount" class="absolute -top-2 -right-2 bg-orange-600 text-white text-[10px] rounded-full w-4 h-4 flex items-center justify-center font-bold">0</span>
                </div>
            </div>
        </div>

        <nav class="max-w-7xl mx-auto px-4 flex items-center space-x-8 py-2 text-sm font-semibold overflow-x-auto no-scrollbar whitespace-nowrap">
            <button onclick="filterCategory('all')" class="category-btn text-orange-600 border-b-2 border-orange-600 pb-1">All Items</button>
            <button onclick="filterCategory('Sports')" class="category-btn hover:text-orange-600 pb-1">Sports</button>
            <button onclick="filterCategory('Casual')" class="category-btn hover:text-orange-600 pb-1">Casual</button>
            <button onclick="filterCategory('Formal')" class="category-btn hover:text-orange-600 pb-1">Formal</button>
            <button onclick="toggleAIConsultant()" class="ml-auto flex items-center gap-2 bg-orange-50 text-orange-700 px-3 py-1 rounded-full border border-orange-200">
                <span>✨ AI Consultant</span>
            </button>
        </nav>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 py-6 min-h-screen">
        <div id="resultsInfo" class="mb-4 text-sm text-gray-500 hidden">
            Showing results for "<span id="searchTermText" class="font-bold"></span>"
        </div>

        <div id="product-grid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4"></div>

        <div id="emptyState" class="hidden text-center py-20">
            <i class="fas fa-search text-4xl text-gray-300 mb-4"></i>
            <p class="text-gray-500">No shoes found matching your search.</p>
        </div>
    </main>

    <!-- ✨ AI Floating Consultant Modal -->
    <div id="aiModal" class="fixed inset-0 z-[60] hidden flex items-center justify-center bg-black/50 p-4">
        <div class="bg-white rounded-2xl w-full max-w-md overflow-hidden shadow-2xl flex flex-col max-h-[80vh]">
            <div class="bg-orange-600 p-4 text-white flex justify-between items-center">
                <div class="flex items-center gap-2">
                    <i class="fas fa-magic"></i>
                    <span class="font-bold tracking-tight">Style Consultant</span>
                </div>
                <button onclick="toggleAIConsultant()" class="text-white/80 hover:text-white"><i class="fas fa-times"></i></button>
            </div>
            <div id="aiChatWindow" class="p-4 overflow-y-auto flex-1 space-y-4 text-sm bg-orange-50/30">
                <div class="bg-white p-3 rounded-lg shadow-sm border border-orange-100">
                    Hello! I'm your ✨ AI Style Assistant. Tell me what you're looking for, and I'll recommend the best shoes from our collection!
                </div>
            </div>
            <div class="p-4 border-t bg-white flex gap-2">
                <input type="text" id="aiInput" placeholder="e.g. I need comfortable shoes for a trip..." 
                    class="flex-1 border rounded-full px-4 py-2 focus:outline-none focus:ring-2 focus:ring-orange-500">
                <button onclick="askAI()" class="bg-black text-white w-10 h-10 rounded-full flex items-center justify-center">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- ✨ Outfit Suggestion Modal -->
    <div id="outfitModal" class="fixed inset-0 z-[60] hidden flex items-center justify-center bg-black/50 p-4">
        <div class="bg-white rounded-xl w-full max-w-sm p-6 shadow-2xl relative">
            <button onclick="closeOutfitModal()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600"><i class="fas fa-times"></i></button>
            <div id="outfitContent" class="space-y-4">
                <div class="flex items-center gap-2 text-orange-600 font-bold mb-2">
                    <i class="fas fa-sparkles"></i> ✨ AI Outfit Suggestion
                </div>
                <div id="outfitLoading" class="flex flex-col items-center py-8">
                    <div class="animate-spin text-orange-600 text-3xl mb-2"><i class="fas fa-circle-notch"></i></div>
                    <p class="text-xs text-gray-500 italic">Styling your look...</p>
                </div>
                <div id="outfitText" class="text-sm leading-relaxed text-gray-700"></div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-[#111] text-gray-400 py-12 px-4 mt-12">
        <div class="max-w-7xl mx-auto grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-xs tracking-widest">Company Info</h3>
                <ul class="space-y-2 text-sm">
                    <li><a href="#" class="hover:text-white transition">About Us</a></li>
                    <li><a href="#" class="hover:text-white transition">Affiliate Program</a></li>
                    <li><a href="#" class="hover:text-white transition">Contact Us</a></li>
                </ul>
            </div>
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-xs tracking-widest">Customer Service</h3>
                <ul class="space-y-2 text-sm">
                    <li><a href="#" class="hover:text-white transition">Return Policy</a></li>
                    <li><a href="#" class="hover:text-white transition">Shipping Info</a></li>
                </ul>
            </div>
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-xs tracking-widest">Help & Support</h3>
                <ul class="space-y-2 text-sm">
                    <li><a href="#" class="hover:text-white transition">FAQ</a></li>
                    <li><a href="#" class="hover:text-white transition">Security Center</a></li>
                </ul>
            </div>
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-xs tracking-widest">Download App</h3>
                <div class="space-y-3">
                    <div class="border border-gray-700 rounded p-2 flex items-center cursor-pointer hover:bg-gray-800 transition">
                        <i class="fab fa-apple text-2xl mr-3 text-white"></i>
                        <span class="text-xs text-white">App Store</span>
                    </div>
                    <div class="border border-gray-700 rounded p-2 flex items-center cursor-pointer hover:bg-gray-800 transition">
                        <i class="fab fa-google-play text-2xl mr-3 text-white"></i>
                        <span class="text-xs text-white">Google Play</span>
                    </div>
                </div>
            </div>
        </div>
    </footer>

    <script>
        const apiKey = ""; // API key managed by environment
        const WHATSAPP_NUMBER = "YOUR_PHONE_NUMBER"; 
        
        const products = [
            { id: 1, name: "Professional Marathon Running Shoes", price: 165, oldPrice: 529, badge: "ECONOMY", image: "https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=400", sales: "5,000+", rating: 5, reviews: 285, category: "Sports" },
            { id: 2, name: "Breathable Mesh Casual Sneakers", price: 154, oldPrice: 479, badge: "BEST SELLER", image: "https://images.unsplash.com/photo-1606107557195-0e29a4b5b4aa?w=400", sales: "67,000+", rating: 5, reviews: 5034, category: "Casual" },
            { id: 3, name: "Men's Luxury Sports Shoes - Blue Edition", price: 207, oldPrice: 452, badge: "TOP RATED", image: "https://images.unsplash.com/photo-1595950653106-6c9ebd614d3a?w=400", sales: "28,000+", rating: 5, reviews: 2604, category: "Sports" },
            { id: 4, name: "Lightweight Breathable Training Shoes", price: 248, oldPrice: 310, badge: "NEW", image: "https://images.unsplash.com/photo-1560769629-975ec94e6a86?w=400", sales: "7,000+", rating: 4, reviews: 666, category: "Sports" },
            { id: 5, name: "Classic Comfortable Non-Slip Loafers", price: 180, oldPrice: 469, badge: "ECONOMIE", image: "https://images.unsplash.com/photo-1533867617858-e7b97e060509?w=400", sales: "8,000+", rating: 4, reviews: 920, category: "Casual" },
            { id: 6, name: "Premium Leather Formal Shoes", price: 320, oldPrice: 550, badge: "EXCLUSIVE", image: "https://images.unsplash.com/photo-1614252235316-8c857d38b5f4?w=400", sales: "1,200+", rating: 5, reviews: 142, category: "Formal" }
        ];

        let currentSearch = "";
        let currentCategory = "all";

        async function fetchWithRetry(url, payload, retries = 5, backoff = 1000) {
            for (let i = 0; i < retries; i++) {
                try {
                    const response = await fetch(url, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    if (response.ok) return await response.json();
                } catch (err) {}
                await new Promise(r => setTimeout(r, backoff * Math.pow(2, i)));
            }
            throw new Error("API failed after multiple retries.");
        }

        async function getOutfitSuggestion(shoeName) {
            document.getElementById('outfitModal').classList.remove('hidden');
            document.getElementById('outfitLoading').classList.remove('hidden');
            document.getElementById('outfitText').innerText = "";
            
            try {
                const systemPrompt = "You are a fashion expert. Suggest a complete outfit (pants, shirt, accessories) to match these shoes in 2 short sentences.";
                const userQuery = `What outfit goes with these shoes: ${shoeName}?`;
                
                const data = await fetchWithRetry(
                    `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`,
                    { contents: [{ parts: [{ text: userQuery }] }], systemInstruction: { parts: [{ text: systemPrompt }] } }
                );

                const text = data.candidates?.[0]?.content?.parts?.[0]?.text;
                document.getElementById('outfitLoading').classList.add('hidden');
                document.getElementById('outfitText').innerText = text || "Try pairing with neutral tones and slim-fit denim.";
            } catch (e) {
                document.getElementById('outfitLoading').classList.add('hidden');
                document.getElementById('outfitText').innerText = "Unable to load styling tips right now. Try again later!";
            }
        }

        async function askAI() {
            const input = document.getElementById('aiInput');
            const chat = document.getElementById('aiChatWindow');
            const query = input.value;
            if (!query) return;

            // Add user bubble
            chat.innerHTML += `<div class="bg-gray-200 p-3 rounded-lg self-end ml-8 text-right">${query}</div>`;
            input.value = "";
            chat.scrollTop = chat.scrollHeight;

            try {
                const systemPrompt = `You are a shoe expert for SHOEHUB. Use this catalog: ${JSON.stringify(products)}. Recommend 1-2 specific shoes based on the user's request. Keep it short and friendly. Mention the price.`;
                const data = await fetchWithRetry(
                    `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`,
                    { contents: [{ parts: [{ text: query }] }], systemInstruction: { parts: [{ text: systemPrompt }] } }
                );
                const text = data.candidates?.[0]?.content?.parts?.[0]?.text;
                chat.innerHTML += `<div class="bg-white p-3 rounded-lg shadow-sm border border-orange-100 mr-8">${text}</div>`;
                chat.scrollTop = chat.scrollHeight;
            } catch (e) {
                chat.innerHTML += `<div class="bg-red-50 text-red-500 p-3 rounded-lg text-xs">Error connecting to ✨ AI. Please try again later.</div>`;
            }
        }

        function renderProducts() {
            const grid = document.getElementById('product-grid');
            const empty = document.getElementById('emptyState');
            const filtered = products.filter(p => {
                const matchesSearch = p.name.toLowerCase().includes(currentSearch.toLowerCase());
                const matchesCat = currentCategory === 'all' || p.category === currentCategory;
                return matchesSearch && matchesCat;
            });

            if (filtered.length === 0) {
                grid.classList.add('hidden');
                empty.classList.remove('hidden');
            } else {
                grid.classList.remove('hidden');
                empty.classList.add('hidden');
                grid.innerHTML = filtered.map(p => `
                    <div class="product-card rounded-lg overflow-hidden flex flex-col border border-gray-100">
                        <div class="relative aspect-square overflow-hidden bg-gray-50">
                            <img src="${p.image}" alt="${p.name}" class="w-full h-full object-cover">
                            ${p.badge ? `<div class="absolute bottom-2 left-2 bg-orange-100 text-orange-700 text-[9px] font-bold px-1.5 py-0.5 rounded">${p.badge}</div>` : ''}
                        </div>
                        <div class="p-3 flex flex-col flex-1">
                            <h3 class="text-xs font-medium line-clamp-2 mb-2 text-gray-700 h-8">${p.name}</h3>
                            <button onclick="getOutfitSuggestion('${p.name}')" class="text-[9px] text-orange-600 bg-orange-50 self-start px-2 py-0.5 rounded-full mb-2 hover:bg-orange-100">✨ Style Me</button>
                            <div class="mt-auto">
                                <div class="flex items-baseline space-x-1">
                                    <span class="text-lg font-bold price-text">${p.price} DH</span>
                                    <span class="text-[10px] text-gray-400 line-through">${p.oldPrice} DH</span>
                                </div>
                                <div class="flex items-center justify-between mt-3">
                                    <div class="flex items-center text-[9px] text-yellow-500">
                                        ${Array(5).fill(0).map((_, i) => `<i class="${i < p.rating ? 'fas' : 'far'} fa-star"></i>`).join('')}
                                    </div>
                                    <button onclick="orderViaWhatsApp('${p.name}', ${p.price})" class="whatsapp-btn text-white px-3 py-1.5 rounded-full text-[10px] font-bold flex items-center gap-1">
                                        <i class="fab fa-whatsapp"></i> Order
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                `).join('');
            }
        }

        function toggleAIConsultant() {
            const modal = document.getElementById('aiModal');
            modal.classList.toggle('hidden');
        }

        function closeOutfitModal() {
            document.getElementById('outfitModal').classList.add('hidden');
        }

        function orderViaWhatsApp(name, price) {
            const message = `Hello! I would like to order: ${name} (Price: ${price} DH).`;
            window.open(`https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(message)}`, '_blank');
        }

        function handleSearch() {
            currentSearch = document.getElementById('searchInput').value;
            renderProducts();
        }

        function filterCategory(cat) {
            currentCategory = cat;
            document.querySelectorAll('.category-btn').forEach(btn => {
                btn.classList.toggle('text-orange-600', btn.innerText.toLowerCase().includes(cat.toLowerCase()) || (cat==='all' && btn.innerText==='All Items'));
                btn.classList.toggle('border-b-2', btn.classList.contains('text-orange-600'));
            });
            renderProducts();
        }

        window.onload = renderProducts;
    </script>
</body>
</html>
