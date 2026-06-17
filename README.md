<!doctype html>
<html lang="it" class="h-full">
 <head><script src="/_sdk/telemetry_sdk.js"></script>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Virgo Universe</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="https://cdn.jsdelivr.net/npm/lucide@0.263.0/dist/umd/lucide.min.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
 <style>
    * { font-family: 'DM Sans', sans-serif; }
    .product-card:hover { transform: translateY(-4px); box-shadow: 0 12px 24px rgba(0,0,0,0.12); }
    .product-card { transition: all 0.2s ease; }
    .star-fill { color: #f59e0b; }
    .badge { animation: pulse 2s infinite; }
    @keyframes pulse { 0%,100%{ opacity:1 } 50%{ opacity:0.7 } }
    
    /* Hide scrollbar for category menu on mobile */
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

    /* Animazioni Carrello */
    @keyframes bump {
      0% { transform: scale(1); }
      50% { transform: scale(1.4); }
      100% { transform: scale(1); }
    }
    .animate-bump { animation: bump 0.3s ease-out; }
    .btn-success { background-color: #10b981 !important; transform: scale(1.1); }
  </style>
  <style>body { box-sizing: border-box; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full overflow-auto bg-[#FAF5FF] text-purple-950">
  <div class="w-full h-full flex flex-col">
   <header id="header" class="bg-[#E9D5FF] text-purple-950 sticky top-0 z-50">
    <div class="max-w-7xl mx-auto px-4 py-2.5 flex flex-wrap items-center justify-between gap-y-3 sm:gap-x-4">
     
     <div class="flex items-center gap-2 order-1">
      <img src="Loghi/Logo3.png" alt="Logo" class="w-9 h-9 sm:w-10 sm:h-10 object-contain">
      <span id="site-title" class="text-lg sm:text-xl font-bold tracking-tight whitespace-nowrap">Virgo Universe</span>
     </div>

     <div class="flex items-center gap-3 order-2 sm:order-3">
      <button onclick="toggleCart()" class="relative p-2 hover:bg-purple-300 rounded-lg transition">
       <i data-lucide="shopping-cart" style="width:20px;height:20px"></i>
       <span id="cart-count" class="absolute -top-1 -right-1 bg-[#C084FC] text-white text-xs w-5 h-5 rounded-full flex items-center justify-center font-bold">0</span>
      </button>
     </div>

     <div class="w-full sm:w-auto sm:flex-1 sm:max-w-2xl order-3 sm:order-2">
      <div class="relative">
       <input id="search-input" type="text" placeholder="Cerca oggetti 3D..." class="w-full pl-4 pr-10 py-2 rounded-lg text-gray-900 text-sm focus:outline-none focus:ring-2 focus:ring-purple-400 border border-purple-200 shadow-sm"> 
       <button onclick="filterProducts()" class="absolute right-2 top-1/2 -translate-y-1/2 text-gray-400 hover:text-purple-600">
        <i data-lucide="search" style="width:18px;height:18px"></i>
       </button>
      </div>
     </div>

    </div>
   </header>

   <div class="bg-[#FAF5FF] border-b border-purple-100 overflow-x-auto no-scrollbar shadow-sm">
    <div class="max-w-7xl mx-auto px-4 py-3 flex items-center gap-6 text-sm">
        <button onclick="setCategory('all')" class="cat-btn whitespace-nowrap text-purple-700 font-bold underline underline-offset-4">Tutti</button>
        <button onclick="setCategory('casa')" class="cat-btn whitespace-nowrap text-purple-500 hover:text-purple-800 transition">Casa & Decor</button>
        <button onclick="setCategory('gadget')" class="cat-btn whitespace-nowrap text-purple-500 hover:text-purple-800 transition">Gadget & Tech</button>
        <button onclick="setCategory('giochi')" class="cat-btn whitespace-nowrap text-purple-500 hover:text-purple-800 transition">Giochi & Hobby</button>
        <button onclick="setCategory('adulti')" class="cat-btn whitespace-nowrap text-purple-500 hover:text-purple-800 transition">Adulti</button>
        <button onclick="setCategory('arte')" class="cat-btn whitespace-nowrap text-purple-500 hover:text-purple-800 transition">Arte & Design</button>
    </div>
   </div>
   <section class="bg-gradient-to-r from-[#E9D5FF] via-[#F3E8FF] to-[#E9D5FF] text-purple-950 py-8 px-4 border-b border-purple-100">
    <div class="max-w-7xl mx-auto text-center">
     <h1 id="hero-text" class="text-2xl md:text-3xl font-bold mb-2">Trova oggetti personalizzati</h1>
     <p class="text-purple-700 text-sm">Idee Originali • Vasta Scelta • Materiali eco-friendly</p>
    </div>
   </section>
   
   <main class="flex-1 max-w-7xl mx-auto px-4 py-6 w-full">
    <div class="flex items-center justify-between mb-4">
     <p id="results-text" class="text-sm text-purple-800 font-medium"></p>
     
     <select id="sort-select" onchange="sortProducts()" class="text-sm border border-purple-200 rounded-lg px-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-purple-400 bg-white text-purple-900 shadow-sm cursor-pointer"> 
         <option value="price-low">Prezzo: basso → alto</option> 
         <option value="price-high">Prezzo: alto → basso</option> 
     </select>
     
    </div>
    <div id="product-grid" class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4"></div>
   </main>

   <div id="cart-overlay" class="fixed inset-0 bg-black/30 z-50 hidden backdrop-blur-sm" onclick="toggleCart()"></div>
   <div id="cart-panel" class="fixed top-0 right-0 h-full w-80 bg-white shadow-2xl z-50 translate-x-full transition-transform duration-300 flex flex-col">
    <div class="p-4 border-b border-purple-100 flex items-center justify-between bg-[#FAF5FF]">
     <h2 class="font-bold text-lg text-purple-950">Carrello</h2><button onclick="toggleCart()" class="p-1 hover:bg-purple-200 text-purple-700 rounded transition"><i data-lucide="x" style="width:20px;height:20px"></i></button>
    </div>
    <div id="cart-items" class="flex-1 overflow-auto p-4"></div>
    <div class="p-4 border-t border-purple-100 bg-[#FAF5FF]">
     <div class="flex justify-between font-bold mb-3 text-purple-950"><span>Totale</span><span id="cart-total">€0,00</span>
     </div><button onclick="sendToWhatsApp()" class="w-full bg-[#C084FC] text-white py-2.5 rounded-lg font-medium hover:bg-[#A855F7] transition flex items-center justify-center gap-2 shadow-sm"><i data-lucide="send" style="width:18px;height:18px"></i>Invia su WhatsApp</button>
    </div>
   </div>
  </div>

  <script>
const products = [
  // PRODOTTI
  { id:1, name:"Portachiavi Divertente", price:6.00, category:"adulti", badge:"Nuovo", color:"bg-amber-100", icon:"<img src='Prodotti/condom1.png' alt='Prodotto' class='w-full h-full object-cover'>" },
  { id:2, name:"Portachiavi Nome Custom", price:4.00, category:"arte", badge:"Nuovo", color:"bg-blue-100", icon:"<img src='Prodotti/PortachiaviNome1.png' alt='Prodotto' class='w-full h-full object-cover'>" },
  //{ id:3, name:"Vaso Voronoi Minimalista", price:28.50, rating:4.9, reviews:203, category:"casa", badge:"Top", color:"bg-green-100", icon:"🌿" },
  //{ id:4, name:"Set Dadi D&D Custom", price:19.90, rating:4.7, reviews:156, category:"giochi", badge:"", color:"bg-purple-100", icon:"🎲" },
  //{ id:5, name:"Organizer Scrivania Hex", price:22.00, rating:4.6, reviews:74, category:"organizer", badge:"Nuovo", color:"bg-pink-100", icon:"🗂️" },
  //{ id:6, name:"Scultura Astratta Wave", price:45.00, rating:4.9, reviews:61, category:"arte", badge:"Premium", color:"bg-orange-100", icon:"🎨" },
  //{ id:7, name:"Portachiavi Flexi Rex", price:8.50, rating:4.4, reviews:312, category:"giochi", badge:"Bestseller", color:"bg-yellow-100", icon:"🦖" },
  //{ id:8, name:"Fioriera Parete Modulare", price:18.90, rating:4.6, reviews:95, category:"casa", badge:"", color:"bg-teal-100", icon:"🌱" },
  //{ id:9, name:"Custodia AirPods Armor", price:14.99, rating:4.3, reviews:67, category:"gadget", badge:"Nuovo", color:"bg-slate-100", icon:"🎧" },
  //{ id:10, name:"Lithophane Personalizzata", price:25.00, rating:4.8, reviews:184, category:"arte", badge:"Top", color:"bg-rose-100", icon:"🖼️" },
  //{ id:11, name:"Cable Management Clip x10", price:9.90, rating:4.5, reviews:221, category:"organizer", badge:"", color:"bg-cyan-100", icon:"🔌" },
  //{ id:12, name:"Mini Scacchiera Travel", price:32.00, rating:4.7, reviews:48, category:"giochi", badge:"", color:"bg-indigo-100", icon:"♟️" },
];

let cart = [];
let currentCategory = 'all';
let currentSort = 'price-low'; // Ora di default ordina per prezzo basso dato che abbiamo rimosso i popolari

function renderProducts(list) {
  const grid = document.getElementById('product-grid');
  grid.innerHTML = list.map(p => `
    <div class="product-card bg-white rounded-xl overflow-hidden border border-gray-100 cursor-pointer flex flex-col shadow-sm">
      <div class="relative ${p.color} h-36 flex items-center justify-center text-5xl">
        ${p.badge ? `<span class="badge absolute top-2 left-2 bg-purple-600 text-white text-[10px] font-bold px-2 py-0.5 rounded-full shadow-sm">${p.badge}</span>` : ''}
        ${p.icon}
      </div>
      <div class="p-3 flex flex-col flex-1">
        <h3 class="text-sm font-medium text-purple-950 leading-tight mb-2 line-clamp-2">${p.name}</h3>
        
        <div class="mt-auto flex items-end justify-between">
          <span class="text-lg font-bold text-purple-900">€${p.price.toFixed(2)}</span>
          <button onclick="addToCart(${p.id}, event)" class="bg-[#C084FC] hover:bg-[#A855F7] text-white p-1.5 rounded-lg transition-all duration-300 shadow-sm flex items-center justify-center w-8 h-8">
            <i data-lucide="plus" style="width:16px;height:16px"></i>
          </button>
        </div>
      </div>
    </div>
  `).join('');
  document.getElementById('results-text').textContent = `${list.length} risultati`;
  lucide.createIcons();
}

function setCategory(cat) {
  currentCategory = cat;
  document.querySelectorAll('.cat-btn').forEach((b,i) => {
    const cats = ['all','casa','gadget','giochi','organizer','arte'];
    b.className = `cat-btn whitespace-nowrap ${cats[i]===cat ? 'text-purple-700 font-bold underline underline-offset-4' : 'text-purple-500 hover:text-purple-800 transition'}`;
  });
  applyFilters();
}

function filterProducts() {
  applyFilters();
}

function sortProducts() {
  currentSort = document.getElementById('sort-select').value;
  applyFilters();
}

function applyFilters() {
  let list = [...products];
  const q = document.getElementById('search-input').value.toLowerCase();
  if (q) list = list.filter(p => p.name.toLowerCase().includes(q));
  if (currentCategory !== 'all') list = list.filter(p => p.category === currentCategory);
  
  // Applica ordinamento base al prezzo
  if (currentSort === 'price-low') list.sort((a,b) => a.price - b.price);
  else if (currentSort === 'price-high') list.sort((a,b) => b.price - a.price);
  
  renderProducts(list);
}

function addToCart(id, event) {
  const p = products.find(x => x.id === id);
  const existing = cart.find(x => x.id === id);
  if (existing) existing.qty++;
  else cart.push({...p, qty: 1});
  updateCart();

  if (event) {
    const btn = event.currentTarget;
    const originalHTML = btn.innerHTML;

    btn.innerHTML = `<i data-lucide="check" style="width:16px;height:16px"></i>`;
    btn.classList.add('btn-success');
    lucide.createIcons({root: btn});

    setTimeout(() => {
      btn.innerHTML = originalHTML;
      btn.classList.remove('btn-success');
      lucide.createIcons({root: btn});
    }, 600);
  }

  const cartCount = document.getElementById('cart-count');
  cartCount.classList.remove('animate-bump');
  void cartCount.offsetWidth; 
  cartCount.classList.add('animate-bump');
}

function removeFromCart(id) {
  cart = cart.filter(x => x.id !== id);
  updateCart();
}

function updateCart() {
  document.getElementById('cart-count').textContent = cart.reduce((s,i) => s + i.qty, 0);
  const total = cart.reduce((s,i) => s + i.price * i.qty, 0);
  document.getElementById('cart-total').textContent = `€${total.toFixed(2)}`;
  const container = document.getElementById('cart-items');
  if (!cart.length) { container.innerHTML = '<p class="text-purple-300 text-center mt-8">Il carrello è vuoto</p>'; return; }
  container.innerHTML = cart.map(i => `
    <div class="flex items-center gap-3 mb-3 pb-3 border-b border-purple-50">
      <div class="w-10 h-10 ${i.color} rounded-lg flex items-center justify-center text-lg shadow-sm">${i.icon}</div>
      <div class="flex-1 min-w-0">
        <p class="text-sm font-medium text-purple-950 truncate">${i.name}</p>
        <p class="text-xs text-purple-500">x${i.qty} • €${(i.price*i.qty).toFixed(2)}</p>
      </div>
      <button onclick="removeFromCart(${i.id})" class="text-purple-300 hover:text-red-400 transition"><i data-lucide="trash-2" style="width:14px;height:14px"></i></button>
    </div>
  `).join('');
  lucide.createIcons();
}

function toggleCart() {
  const panel = document.getElementById('cart-panel');
  const overlay = document.getElementById('cart-overlay');
  const open = panel.classList.contains('translate-x-full');
  panel.classList.toggle('translate-x-full', !open);
  overlay.classList.toggle('hidden', !open);
}

function sendToWhatsApp() {
  if (!cart.length) {
    alert('Il carrello è vuoto!');
    return;
  }

  // 1. Genera un numero ordine univoco (es. V-8X2A9)
  const orderId = 'V-' + Math.random().toString(36).substr(2, 5).toUpperCase();
  
  const total = cart.reduce((s,i) => s + i.price * i.qty, 0);
  
  // 2. Aggiungi l'ordine al messaggio
  let message = '✦  *VIRGO UNIVERSE*  ✦\n';
  message += `｜ *Ordine:* #${orderId}｜\n\n`; // <--- Ecco l'ID ordine
  
  message += '*Articoli:*\n';
  cart.forEach(i => {
    message += `• ${i.name}\n`;
    message += `  Quantità: ${i.qty} × €${i.price.toFixed(2)} = €${(i.price * i.qty).toFixed(2)}\n\n`;
  });
  message += `*Totale: €${total.toFixed(2)}*\n\n`;
  message += 'Ordine Effettuato!';
  
  const whatsappNumber = '391234567890'; 
  const encodedMessage = encodeURIComponent(message);
  const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodedMessage}`;
  
  window.open(whatsappUrl, '_blank');
}
// Init
setCategory('all');
document.getElementById('search-input').addEventListener('keyup', (e) => { if(e.key==='Enter') applyFilters(); });
updateCart();
lucide.createIcons();

// Element SDK - Colori schiariti per il Lilla Pastello
const defaultConfig = { 
  site_title: "Virgo Universe", 
  hero_text: "Trova oggetti e personalizzati", 
  background_color: "#FAF5FF", 
  surface_color: "#ffffff", 
  text_color: "#3B0764", 
  primary_color: "#C084FC", 
  secondary_color: "#E9D5FF", 
  font_family: "DM Sans", 
  font_size: 14 
};

if (window.elementSdk) {
  window.elementSdk.init({
    defaultConfig,
    onConfigChange: async (config) => {
      document.getElementById('site-title').textContent = config.site_title || defaultConfig.site_title;
      document.getElementById('hero-text').textContent = config.hero_text || defaultConfig.hero_text;
      const bg = config.background_color || defaultConfig.background_color;
      const surface = config.surface_color || defaultConfig.surface_color;
      const text = config.text_color || defaultConfig.text_color;
      const primary = config.primary_color || defaultConfig.primary_color;
      const secondary = config.secondary_color || defaultConfig.secondary_color;
      const font = config.font_family || defaultConfig.font_family;
      const size = config.font_size || defaultConfig.font_size;
      
      document.body.style.backgroundColor = bg;
      document.body.style.color = text;
      document.body.style.fontFamily = `${font}, sans-serif`;
      document.body.style.fontSize = `${size}px`;
      
      const headerElem = document.getElementById('header');
      headerElem.style.backgroundColor = secondary;
      headerElem.style.color = text; 

      document.querySelectorAll('.product-card').forEach(c => { c.style.backgroundColor = surface; });
      document.querySelectorAll('button.bg-\\[\\#C084FC\\]').forEach(b => { b.style.backgroundColor = primary; });
    },
    mapToCapabilities: (config) => ({
      recolorables: [
        { get: () => config.background_color || defaultConfig.background_color, set: (v) => { config.background_color = v; window.elementSdk.setConfig({ background_color: v }); } },
        { get: () => config.surface_color || defaultConfig.surface_color, set: (v) => { config.surface_color = v; window.elementSdk.setConfig({ surface_color: v }); } },
        { get: () => config.text_color || defaultConfig.text_color, set: (v) => { config.text_color = v; window.elementSdk.setConfig({ text_color: v }); } },
        { get: () => config.primary_color || defaultConfig.primary_color, set: (v) => { config.primary_color = v; window.elementSdk.setConfig({ primary_color: v }); } },
        { get: () => config.secondary_color || defaultConfig.secondary_color, set: (v) => { config.secondary_color = v; window.elementSdk.setConfig({ secondary_color: v }); } },
      ],
      borderables: [],
      fontEditable: { get: () => config.font_family || defaultConfig.font_family, set: (v) => { config.font_family = v; window.elementSdk.setConfig({ font_family: v }); } },
      fontSizeable: { get: () => config.font_size || defaultConfig.font_size, set: (v) => { config.font_size = v; window.elementSdk.setConfig({ font_size: v }); } },
    }),
    mapToEditPanelValues: (config) => new Map([
      ["site_title", config.site_title || defaultConfig.site_title],
      ["hero_text", config.hero_text || defaultConfig.hero_text],
    ])
  });
} else {
  console.warn("Element SDK non disponibile: avvio in modalità offline.");
}

</script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML=\"window.__CF$cv$params={r:'a0d2b885c5c5c28a',t:'MTc4MTcwNjI5Ny4wMDAwMDA='};var a =document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);\";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>

<!-- Footer Informativo -->
    <footer class="mt-auto py-8 text-center text-purple-400 text-xs px-4 border-t border-purple-100">
        <p class="max-w-xl mx-auto">
            Questo sito è una vetrina virtuale. Non viene effettuata alcuna raccolta o memorizzazione di dati personali. 
            La comunicazione avviene tramite WhatsApp per la sola finalità di gestione degli ordini.
        </p>
    </footer>

    <!-- Posiziona il footer prima del tag di chiusura -->
</body>

</html># VirgoUniverse.github.io
