<!doctype html>
<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>DonaUno - Order Online</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<style>
  :root{--pink:#ec4899;--bg:#fff0f6;--muted:#6b7280}
  body{font-family:'Inter',ui-sans-serif,system-ui,Arial; margin:0;background:linear-gradient(180deg,#fff 0%,#fff6fb 100%);color:#111}
  .wrap{max-width:980px;margin:20px auto;padding:18px}
  header{display:flex;align-items:center;justify-content:space-between;gap:12px}
  .brand{display:flex;align-items:center;gap:12px}
  .logo{width:64px;height:64px;border-radius:12px;background:var(--pink);display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:20px}
  h1{margin:0;font-size:20px}
  .grid{display:grid;grid-template-columns:1fr 360px;gap:18px;margin-top:18px}
  .card{background:#fff;border-radius:12px;padding:14px;box-shadow:0 6px 18px rgba(16,24,40,.06)}
  .products{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
  .prod{border:1px dashed #eee;padding:12px;border-radius:8px;display:flex;flex-direction:column;gap:8px}
  .prod .name{font-weight:600}
  .prod .price{color:var(--muted);font-size:14px}
  .btn{background:var(--pink);color:#fff;padding:8px 10px;border-radius:8px;cursor:pointer;border:none}
  .btn.secondary{background:#fff;color:var(--pink);border:1px solid #ffd6ec}
  .cart-item{display:flex;justify-content:space-between;gap:6px;padding:8px 0;border-bottom:1px solid #f3f4f6}
  .muted{color:var(--muted);font-size:13px}
  .total-row{display:flex;justify-content:space-between;font-weight:700;padding-top:8px}
  input[type="text"],textarea,select{width:100%;padding:8px;border-radius:8px;border:1px solid #eee; box-sizing:border-box;}
  .small{font-size:13px;color:var(--muted)}
  footer{margin-top:18px;text-align:center;color:var(--muted);font-size:13px}
  .alert-box {
      background-color: #ffe6e6;
      color: #b91c1c;
      padding: 12px;
      border-radius: 8px;
      margin-top: 10px;
      display: none;
  }
  @media(max-width:900px){ .grid{grid-template-columns:1fr} .cart-panel{order:-1} }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="brand">
      <div class="logo">DU</div>
      <div>
        <h1>Kedai Numero Uno</h1>
        <div class="small">Donat • Kue Pancong • Wedang Jahe Susu</div>
      </div>
    </div>
    <div>
      <div class="small">WA Pesan: <strong>0822-2020-0414</strong></div>
    </div>
  </header>

  <div class="grid">
    <!-- left: products -->
    <div>
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <strong>Menu DonaUno</strong>
            <div class="small">Pilih produk lalu tekan Tambah</div>
          </div>
          <div>
            <input id="search" placeholder="Cari menu..." style="padding:8px;border-radius:8px;border:1px solid #eee" />
          </div>
        </div>

        <div id="products" class="products" style="margin-top:12px"></div>
      </div>

      <div class="card" style="margin-top:12px">
        <strong>Informasi</strong>
        <p class="small" style="margin-top:6px">Pesan melalui WhatsApp. Setelah kirim, kami akan konfirmasi via WA. Simpan <span id="lastOrderId" style="font-weight:700"></span> sebagai nomor order.</p>
        <p class="small" style="margin-top:6px">Catatan: aplikasi ini hanya mengirim order ke nomor WA pemilik. Untuk status pesanan (Diterima / Diproses / Diantar) akan dikirim balik melalui WA oleh pemilik.</p>
      </div>
    </div>

    <!-- right: cart & checkout -->
    <aside class="card cart-panel">
      <strong>Keranjang</strong>
      <div id="cartList" style="margin-top:10px;min-height:120px"></div>
      <div style="margin-top:10px">
        <div class="total-row"><div>Subtotal</div><div id="subtotal">Rp 0</div></div>
        <div style="margin-top:8px">
          <label class="small">Pengiriman / Catatan</label>
          <select id="deliveryType" style="width:100%;padding:8px;border-radius:8px;margin-top:6px;border:1px solid #eee">
            <option value="pickup">Ambil di Toko (Pickup)</option>
            <option value="delivery">Antar (Delivery)</option>
          </select>
          <div style="margin-top:8px">
            <input id="custName" type="text" placeholder="Nama" />
          </div>
          <div style="margin-top:8px">
            <input id="custPhone" type="text" placeholder="No. WA / Telepon (contoh: 0822...)" />
          </div>
          <div style="margin-top:8px">
            <textarea id="custAddress" placeholder="Alamat / Catatan (jika antar)"></textarea>
          </div>
        </div>

        <div style="margin-top:10px;display:flex;gap:8px">
          <button id="btnCheckout" class="btn" style="flex:1">Checkout via WhatsApp</button>
          <button id="btnClear" class="btn secondary" style="flex:1">Bersihkan</button>
        </div>
        
        <div id="messageBox" class="alert-box"></div>

        <div id="afterSent" style="margin-top:12px;display:none">
          <div class="small">Order terkirim ke WhatsApp pemilik. Simpan <strong id="orderIdShown"></strong>. Tunggu konfirmasi via WA.</div>
          <div style="margin-top:8px">
            <button id="btnCopy" class="btn secondary">Salin Order ID</button>
          </div>
        </div>
      </div>
    </aside>
  </div>

  <footer class="small">© Kedai Numero Uno — Hub WA: 0822-2020-0414</footer>
</div>

<script>
/* ========== GANTI DATA PRODUK DI SINI (sesuaikan dgn daftar hargamu) ========== */
const PRODUCTS = [
  { id: "d1", name: "Donat Coklat karakter", price: 3000, desc: "Donat lembut, coklat full" },
  { id: "d2", name: "Donat salju", price: 2500, desc: "Donat klasik" },
  { id: "d3", name: "Donat strawbery karakter", price: 3000, desc: "donat lembut strawbery" },
  { id: "p1", name: "Kue Pancong", price: 7000, desc: "Pancong gurih" },
  { id: "j1", name: "Wedang Jahe Susu (Hot)", price: 5000, desc: "Jahe hangat + susu" },
  { id: "s1", name: "Sosis kecil", price: 2000, desc: "Sosis kecil" },
  { id: "s2", name: "Sosis sapi medium", price: 5000, desc: "Sosis sapi ukuran sedang" },
  { id: "s3", name: "Sosis sapi jumbo", price: 10000, desc: "Sosis sapi ukuran jumbo" },
  { id: "b1", name: "Bakso, scallop, kepiting", price: 2000, desc: "Bakso, scallop, dan kepiting" },
  { id: "k1", name: "Kopi", price: 4000, desc: "Kopi hangat atau dingin" },
  { id: "t1", name: "Teh manis", price: 3000, desc: "Teh manis hangat atau dingin" },
  { id: "jr1", name: "Jeruk", price: 4000, desc: "Minuman jeruk segar" }
];
/* ========================================================================== */

const PHONE_OWNER = "6282220220414";
const productsEl = document.getElementById('products');
const cartListEl = document.getElementById('cartList');
const subtotalEl = document.getElementById('subtotal');
const searchEl = document.getElementById('search');
const deliveryTypeEl = document.getElementById('deliveryType');
const btnCheckout = document.getElementById('btnCheckout');
const btnClear = document.getElementById('btnClear');
const custNameEl = document.getElementById('custName');
const custPhoneEl = document.getElementById('custPhone');
const custAddressEl = document.getElementById('custAddress');
const afterSent = document.getElementById('afterSent');
const orderIdShown = document.getElementById('orderIdShown');
const lastOrderId = document.getElementById('lastOrderId');
const btnCopy = document.getElementById('btnCopy');
const messageBox = document.getElementById('messageBox');

let CART = [];

function showMessage(text, type = 'error') {
  messageBox.textContent = text;
  messageBox.style.display = 'block';
  if (type === 'error') {
    messageBox.style.backgroundColor = '#ffe6e6';
    messageBox.style.color = '#b91c1c';
  } else if (type === 'success') {
    messageBox.style.backgroundColor = '#d1f4d8';
    messageBox.style.color = '#15803d';
  }
  setTimeout(() => {
    messageBox.style.display = 'none';
  }, 5000);
}

function renderProducts(list) {
  productsEl.innerHTML = "";
  list.forEach(p => {
    const wrap = document.createElement('div');
    wrap.className = 'prod';
    wrap.innerHTML = `<div style="display:flex;justify-content:space-between"><div>
        <div class="name">${p.name}</div>
        <div class="price">Rp ${p.price.toLocaleString('id-ID')}</div>
        <div class="small">${p.desc || ''}</div>
      </div>
      <div style="display:flex;flex-direction:column;gap:6px;align-items:flex-end">
        <button class="btn" data-id="${p.id}">Tambah</button>
      </div></div>`;
    productsEl.appendChild(wrap);
    wrap.querySelector('button').addEventListener('click', () => addToCart(p.id));
  });
}

searchEl.addEventListener('input',()=> {
  const q = searchEl.value.trim().toLowerCase();
  renderProducts(PRODUCTS.filter(p => p.name.toLowerCase().includes(q)));
});

function addToCart(id) {
  const prod = PRODUCTS.find(p=>p.id===id);
  if(!prod) return;
  const found = CART.find(c=>c.id===id);
  if(found) found.qty++;
  else CART.push({ ...prod, qty: 1 });
  renderCart();
  showMessage(`"${prod.name}" ditambahkan ke keranjang.`, 'success');
}
function removeFromCart(id) {
  CART = CART.filter(c=>c.id!==id);
  renderCart();
}
function changeQty(id, delta) {
  CART = CART.map(c=> c.id===id ? {...c, qty: Math.max(0,c.qty+delta)} : c).filter(c=>c.qty>0);
  renderCart();
}
function calculateSubtotal() {
  return CART.reduce((s,i)=> s + i.price*i.qty, 0);
}
function renderCart() {
  cartListEl.innerHTML = "";
  if(CART.length===0) cartListEl.innerHTML = '<div class="small">Keranjang kosong. Tambah produk dulu.</div>';
  CART.forEach(item => {
    const div = document.createElement('div');
    div.className = 'cart-item';
    div.innerHTML = `<div>
        <div style="font-weight:600">${item.name}</div>
        <div class="small">Rp ${item.price.toLocaleString('id-ID')} x ${item.qty}</div>
      </div>
      <div style="display:flex;flex-direction:column;gap:6px;align-items:flex-end">
        <div style="display:flex;gap:6px">
          <button class="small" onclick="changeQty('${item.id}',-1)">-</button>
          <div class="small" style="min-width:18px;text-align:center">${item.qty}</div>
          <button class="small" onclick="changeQty('${item.id}',1)">+</button>
        </div>
        <button class="small" style="color:#ef4444" onclick="removeFromCart('${item.id}')">Hapus</button>
      </div>`;
    cartListEl.appendChild(div);
  });
  const st = calculateSubtotal();
  subtotalEl.textContent = `Rp ${st.toLocaleString('id-ID')}`;
}

function generateOrderID() {
  return 'DU' + Date.now().toString().slice(-7);
}

btnCheckout.addEventListener('click', ()=>{
  if(CART.length===0) { showMessage('Keranjang kosong. Tambah produk dulu.'); return; }
  const name = custNameEl.value.trim();
  const phone = custPhoneEl.value.trim();
  if(!name || !phone) { showMessage('Isi nama dan nomor WA/telepon agar pemilik bisa konfirmasi pesanan.'); return; }

  const orderId = generateOrderID();
  const type = deliveryTypeEl.value;
  const address = custAddressEl.value.trim() || '-';
  const subtotal = calculateSubtotal();
  const deliveryFee = (type === 'delivery') ? 5000 : 0;
  const total = subtotal + deliveryFee;

  let msg = `*Pesanan DONAU NO. ${orderId}*%0A`;
  msg += `Nama: ${name}%0ANo: ${phone}%0ATipe: ${type === 'delivery' ? 'Antar' : 'Pickup'}%0AAlamat: ${address}%0A%0A`;
  msg += `*Detail Pesanan:*%0A`;
  CART.forEach((it, idx) => {
    msg += `${idx+1}. ${it.name} x ${it.qty} = Rp ${ (it.price*it.qty).toLocaleString('id-ID')}%0A`;
  });
  msg += `%0ASubtotal: Rp ${subtotal.toLocaleString('id-ID')}%0A`;
  if(deliveryFee) msg += `Ongkir: Rp ${deliveryFee.toLocaleString('id-ID')}%0A`;
  msg += `*Total: Rp ${total.toLocaleString('id-ID')}*%0A%0A`;
  msg += `Mohon konfirmasi ketersediaan & estimasi penjemputan / pengantaran. Terima kasih.`;

  const url = `https://wa.me/${PHONE_OWNER}?text=${msg}`;
  window.open(url, '_blank');

  const order = { id: orderId, name, phone, address, items: CART, subtotal, deliveryFee, total, status: 'Dikirim ke WA Pemilik' };
  const orders = JSON.parse(localStorage.getItem('donauno_orders')||'[]');
  orders.push(order);
  localStorage.setItem('donauno_orders', JSON.stringify(orders));

  afterSent.style.display = 'block';
  orderIdShown.textContent = orderId;
  lastOrderId.textContent = orderId;

  CART = [];
  renderCart();
});

btnCopy.addEventListener('click', ()=> {
  const id = orderIdShown.textContent;
  navigator.clipboard.writeText(id).then(()=> showMessage('Order ID disalin: ' + id, 'success')).catch(()=> showMessage('Gagal menyalin ID order.', 'error'));
});

btnClear.addEventListener('click', ()=> {
    showMessage('Keranjang berhasil dibersihkan.', 'success');
    CART=[];
    renderCart();
});

renderProducts(PRODUCTS);
renderCart();

window.changeQty = changeQty;
window.removeFromCart = removeFromCart;
</script>
</body>
</html>

