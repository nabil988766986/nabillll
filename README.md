<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rent & Go — Car Rental</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg-deep: #060e1a;
  --bg-primary: #0a1628;
  --bg-secondary: #111d33;
  --bg-card: #132035;
  --bg-card-hover: #1a2d4a;
  --accent: #3b82f6;
  --accent-bright: #60a5fa;
  --accent-dark: #1e40af;
  --text-primary: #f1f5f9;
  --text-secondary: #94a3b8;
  --text-muted: #64748b;
  --border: #1e3a5f;
  --success: #22c55e;
  --danger: #ef4444;
  --warning: #f59e0b;
}
* { margin:0; padding:0; box-sizing:border-box; }
body {
  font-family: 'Inter', sans-serif;
  background: var(--bg-deep);
  color: var(--text-primary);
  min-height: 100vh;
  overflow-x: hidden;
}
h1,h2,h3,h4,h5,h6 { font-family: 'Poppins', sans-serif; }

.hero-bg {
  position: relative;
  overflow: hidden;
}
.hero-bg::before {
  content: '';
  position: absolute;
  top: -50%; left: -50%;
  width: 200%; height: 200%;
  background: radial-gradient(ellipse at 30% 20%, rgba(59,130,246,0.08) 0%, transparent 50%),
              radial-gradient(ellipse at 70% 80%, rgba(30,64,175,0.06) 0%, transparent 50%);
  animation: bgPulse 8s ease-in-out infinite alternate;
}
@keyframes bgPulse {
  0% { transform: translate(0,0) scale(1); }
  100% { transform: translate(-2%,2%) scale(1.05); }
}

.particles {
  position: absolute; inset: 0;
  pointer-events: none;
  overflow: hidden;
}
.particle {
  position: absolute;
  width: 3px; height: 3px;
  background: rgba(59,130,246,0.3);
  border-radius: 50%;
  animation: floatUp linear infinite;
}
@keyframes floatUp {
  0% { transform: translateY(100vh) scale(0); opacity:0; }
  10% { opacity:1; }
  90% { opacity:1; }
  100% { transform: translateY(-10vh) scale(1); opacity:0; }
}

.car-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.4s cubic-bezier(.4,0,.2,1);
  position: relative;
}
.car-card:hover {
  transform: translateY(-8px);
  border-color: var(--accent);
  box-shadow: 0 20px 60px rgba(59,130,246,0.15), 0 0 0 1px rgba(59,130,246,0.2);
}
.car-card .car-img {
  width: 100%; height: 220px;
  object-fit: cover;
  transition: transform 0.6s ease;
}
.car-card:hover .car-img {
  transform: scale(1.05);
}
.car-card .img-wrap { overflow: hidden; position: relative; }
.car-card .price-badge {
  position: absolute; bottom: 12px; right: 12px;
  background: rgba(10,22,40,0.9);
  backdrop-filter: blur(10px);
  border: 1px solid var(--accent);
  border-radius: 10px;
  padding: 6px 14px;
  font-family: 'Poppins', sans-serif;
  font-weight: 700;
  color: var(--accent-bright);
  font-size: 15px;
}
.car-card .book-btn {
  display: block; width: 100%;
  padding: 14px;
  background: linear-gradient(135deg, var(--accent-dark), var(--accent));
  color: #fff;
  border: none;
  font-family: 'Poppins', sans-serif;
  font-weight: 600;
  font-size: 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  letter-spacing: 0.5px;
}
.car-card .book-btn:hover {
  background: linear-gradient(135deg, var(--accent), var(--accent-bright));
  box-shadow: 0 4px 20px rgba(59,130,246,0.4);
}

.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(0,0,0,0.7);
  backdrop-filter: blur(8px);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
}
.modal-overlay.active {
  opacity: 1;
  pointer-events: all;
}
.modal-overlay .modal-box {
  background: var(--bg-secondary);
  border: 1px solid var(--border);
  border-radius: 20px;
  width: 95%; max-width: 520px;
  max-height: 90vh;
  overflow-y: auto;
  transform: translateY(30px) scale(0.95);
  transition: transform 0.35s cubic-bezier(.4,0,.2,1);
  padding: 0;
}
.modal-overlay.active .modal-box {
  transform: translateY(0) scale(1);
}

.admin-overlay {
  position: fixed; inset: 0;
  background: var(--bg-deep);
  z-index: 2000;
  overflow-y: auto;
  transform: translateX(100%);
  transition: transform 0.4s cubic-bezier(.4,0,.2,1);
}
.admin-overlay.active {
  transform: translateX(0);
}

.form-input {
  width: 100%;
  padding: 12px 16px;
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 10px;
  color: var(--text-primary);
  font-size: 14px;
  font-family: 'Inter', sans-serif;
  transition: border-color 0.3s, box-shadow 0.3s;
  outline: none;
}
.form-input:focus {
  border-color: var(--accent);
  box-shadow: 0 0 0 3px rgba(59,130,246,0.15);
}
.form-input::placeholder {
  color: var(--text-muted);
}

.toast-container {
  position: fixed; top: 20px; right: 20px;
  z-index: 9999;
  display: flex; flex-direction: column; gap: 10px;
}
.toast {
  padding: 14px 22px;
  border-radius: 12px;
  font-size: 14px; font-weight: 500;
  color: #fff;
  min-width: 280px;
  transform: translateX(120%);
  animation: toastIn 0.4s ease forwards;
  display: flex; align-items: center; gap: 10px;
  box-shadow: 0 10px 40px rgba(0,0,0,0.3);
}
.toast.success { background: linear-gradient(135deg, #166534, #22c55e); }
.toast.error { background: linear-gradient(135deg, #991b1b, #ef4444); }
.toast.warning { background: linear-gradient(135deg, #92400e, #f59e0b); }
.toast.info { background: linear-gradient(135deg, var(--accent-dark), var(--accent)); }
@keyframes toastIn {
  to { transform: translateX(0); }
}
@keyframes toastOut {
  to { transform: translateX(120%); opacity: 0; }
}

::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg-deep); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

.status-pending { background: rgba(245,158,11,0.15); color: #f59e0b; border: 1px solid rgba(245,158,11,0.3); }
.status-finished { background: rgba(34,197,94,0.15); color: #22c55e; border: 1px solid rgba(34,197,94,0.3); }
.status-cancelled { background: rgba(239,68,68,0.15); color: #ef4444; border: 1px solid rgba(239,68,68,0.3); }

.booked-slot {
  background: rgba(239,68,68,0.1);
  border: 1px solid rgba(239,68,68,0.25);
  border-radius: 8px;
  padding: 6px 12px;
  font-size: 12px;
  color: #fca5a5;
}

.glow-line {
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--accent), transparent);
  margin: 0 auto;
}

.stat-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 24px;
  transition: all 0.3s ease;
}
.stat-card:hover {
  border-color: var(--accent);
  box-shadow: 0 0 30px rgba(59,130,246,0.1);
}

@media (max-width: 640px) {
  .car-card .car-img { height: 180px; }
  .modal-overlay .modal-box { max-width: 100%; border-radius: 16px 16px 0 0; margin-top: auto; }
}

.btn {
  padding: 10px 20px;
  border-radius: 10px;
  font-weight: 600;
  font-size: 13px;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: 'Poppins', sans-serif;
  display: inline-flex; align-items: center; gap: 6px;
}
.btn-success { background: #166534; color: #86efac; }
.btn-success:hover { background: #22c55e; color: #fff; }
.btn-danger { background: #7f1d1d; color: #fca5a5; }
.btn-danger:hover { background: #ef4444; color: #fff; }
.btn-accent { background: var(--accent-dark); color: #bfdbfe; }
.btn-accent:hover { background: var(--accent); color: #fff; }

.logo-shimmer {
  position: relative;
  overflow: hidden;
}
.logo-shimmer::after {
  content: '';
  position: absolute;
  top: 0; left: -100%;
  width: 60%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.08), transparent);
  animation: shimmer 4s ease-in-out infinite;
}
@keyframes shimmer {
  0%,100% { left: -100%; }
  50% { left: 150%; }
}

@keyframes bounce {
  0%,100% { transform: translateX(-50%) translateY(0); }
  50% { transform: translateX(-50%) translateY(10px); }
}
</style>
</head>
<body>

<div class="toast-container" id="toastContainer"></div>

<header style="background: rgba(10,22,40,0.85); backdrop-filter: blur(20px); border-bottom: 1px solid var(--border);" class="fixed top-0 left-0 right-0 z-50">
  <div class="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between">
    <div class="flex items-center gap-3 logo-shimmer">
      <img src="https://z-cdn-media.chatglm.cn/files/210c7b5b-91ec-4add-ab73-60341193a147.jpeg?auth_key=1880790734-a4d87cbd48434a7db7ddf4469ed2c2fc-0-ec2e6778be98221287f8208204024eb7" alt="Rent & Go" style="height:50px; border-radius: 8px;">
    </div>
    <nav class="flex items-center gap-4">
      <a href="#cars" class="text-sm font-medium text-slate-300 hover:text-white transition hidden sm:block">Our Cars</a>
      <button onclick="openAdminLogin()" class="btn btn-accent text-xs">
        <i class="fas fa-lock"></i> Admin
      </button>
    </nav>
  </div>
</header>

<section class="hero-bg min-h-screen flex items-center justify-center relative" style="padding-top: 80px;">
  <div class="particles" id="particles"></div>
  <div class="relative z-10 text-center px-4 max-w-3xl mx-auto">
    <div class="mb-8 inline-block">
      <img src="https://z-cdn-media.chatglm.cn/files/210c7b5b-91ec-4add-ab73-60341193a147.jpeg?auth_key=1880790734-a4d87cbd48434a7db7ddf4469ed2c2fc-0-ec2e6778be98221287f8208204024eb7" alt="Rent & Go Logo" style="height: 120px; border-radius: 16px; box-shadow: 0 10px 50px rgba(59,130,246,0.2);">
    </div>
    <h1 class="text-5xl sm:text-7xl font-black mb-4 tracking-tight" style="line-height:1.1;">
      Rent <span style="color: var(--accent);">&</span> Go
    </h1>
    <p class="text-lg sm:text-xl mb-2" style="color: var(--text-secondary);">PREMIUM CAR RENTAL</p>
    <p class="text-base mb-10" style="color: var(--text-muted); max-width: 500px; margin-left:auto; margin-right:auto;">
      Luxury vehicles at affordable prices. Book your ride in seconds and hit the road with confidence.
    </p>
    <a href="#cars" class="inline-block px-10 py-4 rounded-xl font-bold text-white text-lg transition-all" style="background: linear-gradient(135deg, var(--accent-dark), var(--accent)); box-shadow: 0 8px 30px rgba(59,130,246,0.3);">
      <i class="fas fa-car mr-2"></i> Browse Cars
    </a>
    <div class="mt-16 flex justify-center gap-8 sm:gap-16 flex-wrap">
      <div class="text-center">
        <div class="text-3xl font-black" style="color: var(--accent-bright);">50+</div>
        <div class="text-xs mt-1" style="color: var(--text-muted);">Premium Cars</div>
      </div>
      <div class="text-center">
        <div class="text-3xl font-black" style="color: var(--accent-bright);">24/7</div>
        <div class="text-xs mt-1" style="color: var(--text-muted);">Support</div>
      </div>
      <div class="text-center">
        <div class="text-3xl font-black" style="color: var(--accent-bright);">$55</div>
        <div class="text-xs mt-1" style="color: var(--text-muted);">Starting/Day</div>
      </div>
    </div>
    
    <!-- BOLD CONTACT TEXT -->
    <div class="mt-10 p-5 rounded-2xl" style="background: rgba(59,130,246,0.08); border: 1px solid rgba(59,130,246,0.25);">
      <p class="text-xl sm:text-2xl font-black tracking-wide" style="color: var(--text-primary);">
        We have <span style="color: var(--accent-bright);">50+ cars</span>! For more info contact 
        <a href="tel:+96181349482" class="hover:underline" style="color: var(--accent-bright); text-decoration: underline;">
          <i class="fas fa-phone-alt mr-1"></i>+961 81 349 482
        </a>
      </p>
    </div>

  </div>
  <div class="absolute bottom-8 left-1/2 -translate-x-1/2" style="animation: bounce 2s infinite;">
    <i class="fas fa-chevron-down text-2xl" style="color: var(--text-muted);"></i>
  </div>
</section>

<section id="cars" class="py-20 px-4" style="background: var(--bg-primary);">
  <div class="max-w-7xl mx-auto">
    <div class="text-center mb-14">
      <p class="text-sm font-semibold tracking-widest mb-2" style="color: var(--accent);">OUR FLEET</p>
      <h2 class="text-4xl sm:text-5xl font-black">Choose Your Ride</h2>
      <div class="glow-line mt-6" style="width: 80px;"></div>
    </div>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8" id="carsGrid"></div>
  </div>
</section>

<footer class="py-10 px-4 text-center" style="background: var(--bg-deep); border-top: 1px solid var(--border);">
  <img src="https://z-cdn-media.chatglm.cn/files/210c7b5b-91ec-4add-ab73-60341193a147.jpeg?auth_key=1880790734-a4d87cbd48434a7db7ddf4469ed2c2fc-0-ec2e6778be98221287f8208204024eb7" alt="Rent & Go" style="height:40px; border-radius:6px; margin: 0 auto 12px;">
  <p class="text-sm" style="color: var(--text-muted);">&copy; 2025 Rent & Go Car Rental. All rights reserved.</p>
</footer>

<div class="modal-overlay" id="bookingModal">
  <div class="modal-box">
    <div class="p-6">
      <div class="flex items-center justify-between mb-6">
        <h3 class="text-xl font-bold">Book This Car</h3>
        <button onclick="closeBookingModal()" class="text-2xl leading-none" style="color: var(--text-muted);">&times;</button>
      </div>
      <div id="modalCarInfo" class="mb-6 p-4 rounded-xl" style="background: var(--bg-card); border: 1px solid var(--border);"></div>
      <div id="modalBookedSlots" class="mb-4"></div>
      <form id="bookingForm" onsubmit="submitBooking(event)">
        <div class="space-y-4">
          <div>
            <label class="block text-sm font-medium mb-1" style="color: var(--text-secondary);">Full Name</label>
            <input type="text" id="bkName" class="form-input" placeholder="Enter your full name" required>
          </div>
          <div>
            <label class="block text-sm font-medium mb-1" style="color: var(--text-secondary);">Lebanese Phone Number</label>
            <div class="flex">
              <span class="flex items-center px-3 rounded-l-xl text-sm font-medium" style="background: var(--bg-card); border: 1px solid var(--border); border-right: none; color: var(--text-muted);">+961</span>
              <input type="tel" id="bkPhone" class="form-input" style="border-top-left-radius:0; border-bottom-left-radius:0;" placeholder="03 123 456" required>
            </div>
          </div>
          <div>
            <label class="block text-sm font-medium mb-1" style="color: var(--text-secondary);">Date</label>
            <input type="date" id="bkDate" class="form-input" required>
          </div>
          <div class="grid grid-cols-2 gap-4">
            <div>
              <label class="block text-sm font-medium mb-1" style="color: var(--text-secondary);">From Time</label>
              <input type="time" id="bkStart" class="form-input" required>
            </div>
            <div>
              <label class="block text-sm font-medium mb-1" style="color: var(--text-secondary);">To Time</label>
              <input type="time" id="bkEnd" class="form-input" required>
            </div>
          </div>
        </div>
        <button type="submit" class="w-full mt-6 py-4 rounded-xl font-bold text-white text-base transition-all" style="background: linear-gradient(135deg, var(--accent-dark), var(--accent));">
          <i class="fas fa-check-circle mr-2"></i> Confirm Booking
        </button>
      </form>
    </div>
  </div>
</div>

<div class="modal-overlay" id="adminLoginModal">
  <div class="modal-box" style="max-width: 400px;">
    <div class="p-6">
      <div class="flex items-center justify-between mb-6">
        <h3 class="text-xl font-bold"><i class="fas fa-shield-alt mr-2" style="color: var(--accent);"></i>Admin Login</h3>
        <button onclick="closeAdminLogin()" class="text-2xl leading-none" style="color: var(--text-muted);">&times;</button>
      </div>
      <div>
        <label class="block text-sm font-medium mb-1" style="color: var(--text-secondary);">Password</label>
        <input type="password" id="adminPassInput" class="form-input" placeholder="Enter admin password" onkeydown="if(event.key==='Enter')attemptAdminLogin()">
      </div>
      <button onclick="attemptAdminLogin()" class="w-full mt-4 py-3 rounded-xl font-bold text-white transition-all" style="background: linear-gradient(135deg, var(--accent-dark), var(--accent));">
        <i class="fas fa-sign-in-alt mr-2"></i> Login
      </button>
    </div>
  </div>
</div>

<div class="admin-overlay" id="adminPanel">
  <div class="min-h-screen" style="background: var(--bg-deep);">
    <div class="sticky top-0 z-10 px-4 py-4 flex items-center justify-between" style="background: rgba(10,22,40,0.95); backdrop-filter: blur(20px); border-bottom: 1px solid var(--border);">
      <div class="flex items-center gap-3">
        <img src="https://z-cdn-media.chatglm.cn/files/210c7b5b-91ec-4add-ab73-60341193a147.jpeg?auth_key=1880790734-a4d87cbd48434a7db7ddf4469ed2c2fc-0-ec2e6778be98221287f8208204024eb7" alt="Logo" style="height:36px; border-radius:6px;">
        <h2 class="text-lg font-bold">Admin Dashboard</h2>
      </div>
      <button onclick="closeAdminPanel()" class="btn btn-danger text-xs">
        <i class="fas fa-sign-out-alt"></i> Logout
      </button>
    </div>
    <div class="max-w-7xl mx-auto px-4 py-8">
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 mb-8" id="adminStatsRow"></div>
      <div class="mb-8">
        <h3 class="text-xl font-bold mb-4"><i class="fas fa-chart-bar mr-2" style="color: var(--accent);"></i>Monthly Report</h3>
        <div class="stat-card overflow-x-auto" id="monthlyStatsTable"></div>
      </div>
      <div class="flex gap-2 mb-6 flex-wrap">
        <button onclick="setAdminTab('pending')" class="btn text-xs" id="tabPending" style="background: var(--bg-card); color: var(--text-secondary);">Pending</button>
        <button onclick="setAdminTab('finished')" class="btn text-xs" id="tabFinished" style="background: var(--bg-card); color: var(--text-secondary);">Finished</button>
        <button onclick="setAdminTab('cancelled')" class="btn text-xs" id="tabCancelled" style="background: var(--bg-card); color: var(--text-secondary);">Cancelled</button>
        <button onclick="setAdminTab('all')" class="btn text-xs" id="tabAll" style="background: var(--bg-card); color: var(--text-secondary);">All Bookings</button>
      </div>
      <div id="adminBookingsList"></div>
    </div>
  </div>
</div>

<script>
const ADMIN_PASSWORD = 'nabil009988$$';
const STORAGE_KEY = 'rentAndGo_bookings';

const cars = [
  {
    id: 'silver-jeep',
    name: 'Jeep Grand Cherokee',
    variant: 'Silver',
    year: 2022,
    price: 85,
    plate: 'N 365580',
    image: 'https://z-cdn-media.chatglm.cn/files/a333b74f-eb51-4085-96ee-7475174b83a6.jpeg?auth_key=1880794107-7cb9330e648f4f04b9a7cff1a44b92c9-0-463c0639fb907f185aa3f408149a8052',
    seats: 5, transmission: 'Automatic', fuel: 'Gasoline'
  },
  {
    id: 'black-jeep',
    name: 'Jeep Wrangler',
    variant: 'Black',
    year: 2023,
    price: 75,
    plate: '0 370883',
    image: 'https://z-cdn-media.chatglm.cn/files/9f5ee7ab-d343-4a82-9a38-be5dd66b7a4a.jpeg?auth_key=1880794107-90820eb28f6444b7bb8357d75f144452-0-bc2ea5eb168121752fb11f12920231ff',
    seats: 5, transmission: 'Automatic', fuel: 'Gasoline'
  },
  {
    id: 'silver-mercedes-sedan',
    name: 'Mercedes C-Class',
    variant: 'Silver',
    year: 2023,
    price: 100,
    plate: 'Available on request',
    image: 'https://z-cdn-media.chatglm.cn/files/38608227-2cb6-4752-8324-4e216f01371d.jpeg?auth_key=1880794107-7d85f60e0eb2497f8874567193a62798-0-d200c8cd9d6a8f97b4e363f8d65fdbb0',
    seats: 5, transmission: 'Automatic', fuel: 'Gasoline'
  },
  {
    id: 'white-mercedes-cabrio',
    name: 'Mercedes C-Class Cabriolet',
    variant: 'White',
    year: 2023,
    price: 135,
    plate: 'B 691409',
    image: 'https://z-cdn-media.chatglm.cn/files/b483bdde-550c-4dfe-9f30-fdac5c5988c9.jpeg?auth_key=1880794107-83226a61d03d474c9fd641a43ab07cbb-0-dcd80b14042f11bee82d816523af2aa3',
    seats: 4, transmission: 'Automatic', fuel: 'Gasoline'
  },
  {
    id: 'blue-range-rover-sport',
    name: 'Range Rover Sport',
    variant: 'Blue',
    year: 2023,
    price: 125,
    plate: 'A 33474',
    image: 'https://z-cdn-media.chatglm.cn/files/c1a972ed-3d45-4f6c-83ae-df487c9dddc7.jpeg?auth_key=1880794107-8da33ae1f26d4b0894839dcb2ae4450a-0-b308ad6087e7ca8d6e99f677da5fa5d7',
    seats: 5, transmission: 'Automatic', fuel: 'Diesel'
  },
  {
    id: 'white-rand-rover',
    name: 'Land Rover Discovery',
    variant: 'White',
    year: 2023,
    price: 130,
    plate: 'Y 70003',
    image: 'https://z-cdn-media.chatglm.cn/files/7633b595-f0e0-424d-88fd-5f96c8c97250.jpeg?auth_key=1880794107-dc38c07ca07c49a6a46e67ade17ac22b-0-75a195604c93b86222ee04c1f0756209',
    seats: 7, transmission: 'Automatic', fuel: 'Diesel'
  },
  {
    id: 'white-convertible',
    name: 'Mercedes Cabriolet',
    variant: 'White',
    year: 2024,
    price: 65,
    plate: 'Available on request',
    image: 'https://z-cdn-media.chatglm.cn/files/11e2ee1e-03fa-4e71-b253-a28a62148fa2.jpeg?auth_key=1880794107-07084fad3d1c45e5b6814f4221f048b6-0-265c4193d9e4da4c3c3b12dfa8556140',
    seats: 4, transmission: 'Automatic', fuel: 'Gasoline'
  },
  {
    id: 'lightblue-range-rover',
    name: 'Range Rover',
    variant: 'Light Blue',
    year: 2024,
    price: 125,
    plate: 'Available on request',
    image: 'https://z-cdn-media.chatglm.cn/files/8d57d8ca-43ab-411a-84d6-04f7816d6774.jpeg?auth_key=1880794107-256c2f95374c4bd185e145f7471efaed-0-4a445ab460131982e80a59f14ed3c36e',
    seats: 5, transmission: 'Automatic', fuel: 'Diesel'
  },
  {
    id: 'black-mercedes-sedan',
    name: 'Mercedes C-Class',
    variant: 'Black',
    year: 2022,
    price: 65,
    plate: 'N 344779',
    image: 'https://z-cdn-media.chatglm.cn/files/5c44e2e0-e518-4f0f-9e9c-db3d3d771430.jpeg?auth_key=1880794107-d204884c18f6462cad9490757388be18-0-abe6b5d3c29955091ad4618d06cebd0e',
    seats: 5, transmission: 'Automatic', fuel: 'Gasoline'
  },
  {
    id: 'white-mercedes-sedan',
    name: 'Mercedes C-Class',
    variant: 'White',
    year: 2023,
    price: 90,
    plate: '751793',
    image: 'https://z-cdn-media.chatglm.cn/files/246fbb9c-e33e-492f-935c-e05b23f59dbd.jpeg?auth_key=1880794107-9c61f8f5882e43ab8591c6ce3c7d8d64-0-6e706707f0434849381becc1709808d7',
    seats: 5, transmission: 'Automatic', fuel: 'Gasoline'
  }
];

let selectedCarId = null;
let currentAdminTab = 'pending';

function getBookings() {
  try { return JSON.parse(localStorage.getItem(STORAGE_KEY)) || []; }
  catch { return []; }
}
function saveBookings(bookings) { localStorage.setItem(STORAGE_KEY, JSON.stringify(bookings)); }
function addBooking(booking) {
  const bookings = getBookings();
  bookings.push(booking);
  saveBookings(bookings);
}
function updateBooking(id, updates) {
  const bookings = getBookings();
  const idx = bookings.findIndex(b => b.id === id);
  if (idx !== -1) { bookings[idx] = { ...bookings[idx], ...updates }; saveBookings(bookings); }
}

function hasConflict(carId, date, startTime, endTime, excludeId) {
  const bookings = getBookings().filter(b =>
    b.carId === carId && b.date === date && b.status !== 'cancelled' && b.id !== excludeId
  );
  return bookings.some(b => startTime < b.endTime && endTime > b.startTime);
}

function getBookedSlots(carId, date) {
  return getBookings().filter(b => b.carId === carId && b.date === date && b.status !== 'cancelled');
}

function renderCars() {
  const grid = document.getElementById('carsGrid');
  grid.innerHTML = cars.map(car => {
    const today = new Date().toISOString().split('T')[0];
    const activeBookings = getBookings().filter(b =>
      b.carId === car.id && b.date >= today && b.status === 'pending'
    );
    const statusBadge = activeBookings.length > 0
      ? `<span style="position:absolute;top:12px;left:12px;background:rgba(245,158,11,0.9);color:#000;padding:4px 10px;border-radius:6px;font-size:11px;font-weight:700;z-index:2;">BOOKED SLOTS</span>`
      : `<span style="position:absolute;top:12px;left:12px;background:rgba(34,197,94,0.9);color:#fff;padding:4px 10px;border-radius:6px;font-size:11px;font-weight:700;z-index:2;">AVAILABLE</span>`;

    return `
      <div class="car-card">
        <div class="img-wrap">
          ${statusBadge}
          <img src="${car.image}" alt="${car.name} ${car.variant}" class="car-img" loading="lazy">
          <div class="price-badge">$${car.price}<span style="font-weight:400;font-size:11px;color:var(--text-muted);">/day</span></div>
        </div>
        <div class="p-5">
          <h3 class="text-lg font-bold mb-1">${car.name}</h3>
          <p class="text-sm mb-3" style="color: var(--text-muted);">${car.variant} &middot; ${car.year}</p>
          <div class="flex gap-4 mb-4 text-xs" style="color: var(--text-secondary);">
            <span><i class="fas fa-user mr-1" style="color:var(--accent);"></i>${car.seats} Seats</span>
            <span><i class="fas fa-cog mr-1" style="color:var(--accent);"></i>${car.transmission}</span>
            <span><i class="fas fa-gas-pump mr-1" style="color:var(--accent);"></i>${car.fuel}</span>
          </div>
          <div class="text-xs mb-4" style="color: var(--text-muted);">
            <i class="fas fa-id-card mr-1"></i>Plate: ${car.plate}
          </div>
          <button class="book-btn" onclick="openBookingModal('${car.id}')">
            <i class="fas fa-calendar-check mr-2"></i>Book Now
          </button>
        </div>
      </div>
    `;
  }).join('');
}

function openBookingModal(carId) {
  selectedCarId = carId;
  const car = cars.find(c => c.id === carId);
  if (!car) return;
  const today = new Date().toISOString().split('T')[0];
  document.getElementById('bkDate').min = today;
  document.getElementById('bkDate').value = today;
  document.getElementById('modalCarInfo').innerHTML = `
    <div class="flex items-center gap-4">
      <img src="${car.image}" alt="${car.name}" style="width:80px;height:60px;object-fit:cover;border-radius:8px;">
      <div>
        <h4 class="font-bold">${car.name} <span style="color:var(--text-muted);font-weight:400;">${car.variant}</span></h4>
        <p class="text-sm" style="color:var(--accent-bright);">$${car.price}/day</p>
      </div>
    </div>
  `;
  updateBookedSlotsDisplay(carId, today);
  document.getElementById('bkName').value = '';
  document.getElementById('bkPhone').value = '';
  document.getElementById('bkStart').value = '';
  document.getElementById('bkEnd').value = '';
  document.getElementById('bookingModal').classList.add('active');
  document.body.style.overflow = 'hidden';
}

function closeBookingModal() {
  document.getElementById('bookingModal').classList.remove('active');
  document.body.style.overflow = '';
  selectedCarId = null;
}

function updateBookedSlotsDisplay(carId, date) {
  const slots = getBookedSlots(carId, date);
  const container = document.getElementById('modalBookedSlots');
  if (slots.length === 0) {
    container.innerHTML = `
      <div class="p-3 rounded-xl text-sm" style="background: rgba(34,197,94,0.08); border: 1px solid rgba(34,197,94,0.2); color: #86efac;">
        <i class="fas fa-check-circle mr-1"></i> This car is fully available on this date.
      </div>
    `;
  } else {
    container.innerHTML = `
      <p class="text-xs font-semibold mb-2" style="color: var(--text-muted);"><i class="fas fa-ban mr-1" style="color:#ef4444;"></i>Already booked on this date:</p>
      <div class="flex flex-wrap gap-2">
        ${slots.map(s => `<span class="booked-slot"><i class="fas fa-clock mr-1"></i>${formatTime(s.startTime)} — ${formatTime(s.endTime)}</span>`).join('')}
      </div>
    `;
  }
}

document.getElementById('bkDate').addEventListener('change', function() {
  if (selectedCarId) updateBookedSlotsDisplay(selectedCarId, this.value);
});

function submitBooking(e) {
  e.preventDefault();
  const name = document.getElementById('bkName').value.trim();
  const phone = document.getElementById('bkPhone').value.trim();
  const date = document.getElementById('bkDate').value;
  const startTime = document.getElementById('bkStart').value;
  const endTime = document.getElementById('bkEnd').value;

  if (!name || name.length < 2) { showToast('Please enter a valid full name.', 'error'); return; }
  const digitsOnly = phone.replace(/\D/g, '');
  if (digitsOnly.length < 7 || digitsOnly.length > 8) { showToast('Please enter a valid Lebanese phone number (7-8 digits).', 'error'); return; }
  const today = new Date().toISOString().split('T')[0];
  if (date < today) { showToast('Cannot book for a past date.', 'error'); return; }
  if (!startTime || !endTime) { showToast('Please select both start and end times.', 'error'); return; }
  if (startTime >= endTime) { showToast('End time must be after start time.', 'error'); return; }
  if (hasConflict(selectedCarId, date, startTime, endTime)) {
    showToast('This car is already booked for the selected time slot.', 'error');
    updateBookedSlotsDisplay(selectedCarId, date);
    return;
  }

  const car = cars.find(c => c.id === selectedCarId);
  const booking = {
    id: 'bk_' + Date.now() + '_' + Math.random().toString(36).substr(2,6),
    carId: selectedCarId,
    carName: car.name + ' ' + car.variant,
    carPlate: car.plate,
    pricePerDay: car.price,
    customerName: name,
    customerPhone: '+961 ' + phone,
    date: date,
    startTime: startTime,
    endTime: endTime,
    status: 'pending',
    createdAt: new Date().toISOString()
  };
  addBooking(booking);
  showToast('Booking confirmed successfully! We will contact you soon.', 'success');
  closeBookingModal();
  renderCars();
}

function openAdminLogin() {
  document.getElementById('adminLoginModal').classList.add('active');
  document.body.style.overflow = 'hidden';
  document.getElementById('adminPassInput').value = '';
  setTimeout(() => document.getElementById('adminPassInput').focus(), 300);
}
function closeAdminLogin() {
  document.getElementById('adminLoginModal').classList.remove('active');
  document.body.style.overflow = '';
}
function attemptAdminLogin() {
  const pass = document.getElementById('adminPassInput').value;
  if (pass === ADMIN_PASSWORD) { closeAdminLogin(); openAdminPanel(); }
  else { showToast('Incorrect password. Access denied.', 'error'); }
}
function openAdminPanel() {
  document.getElementById('adminPanel').classList.add('active');
  document.body.style.overflow = 'hidden';
  renderAdminStats();
  renderMonthlyStats();
  setAdminTab('pending');
}
function closeAdminPanel() {
  document.getElementById('adminPanel').classList.remove('active');
  document.body.style.overflow = '';
  renderCars();
}

function setAdminTab(tab) {
  currentAdminTab = tab;
  ['pending','finished','cancelled','all'].forEach(t => {
    const btn = document.getElementById('tab' + t.charAt(0).toUpperCase() + t.slice(1));
    if (t === tab) { btn.style.background = 'var(--accent)'; btn.style.color = '#fff'; }
    else { btn.style.background = 'var(--bg-card)'; btn.style.color = 'var(--text-secondary)'; }
  });
  renderAdminBookings();
}

function renderAdminStats() {
  const bookings = getBookings();
  const pendingCount = bookings.filter(b => b.status === 'pending').length;
  const finishedCount = bookings.filter(b => b.status === 'finished').length;
  const totalRevenue = bookings.filter(b => b.status === 'finished').reduce((sum, b) => {
    return sum + (b.pricePerDay * calculateDays(b.startTime, b.endTime, b.date));
  }, 0);
  document.getElementById('adminStatsRow').innerHTML = `
    <div class="stat-card text-center">
      <div class="text-3xl font-black" style="color: var(--accent-bright);">${bookings.length}</div>
      <div class="text-xs mt-1" style="color: var(--text-muted);">Total Bookings</div>
    </div>
    <div class="stat-card text-center">
      <div class="text-3xl font-black" style="color: #f59e0b;">${pendingCount}</div>
      <div class="text-xs mt-1" style="color: var(--text-muted);">Pending</div>
    </div>
    <div class="stat-card text-center">
      <div class="text-3xl font-black" style="color: #22c55e;">${finishedCount}</div>
      <div class="text-xs mt-1" style="color: var(--text-muted);">Finished</div>
    </div>
    <div class="stat-card text-center">
      <div class="text-3xl font-black" style="color: #22c55e;">$${totalRevenue}</div>
      <div class="text-xs mt-1" style="color: var(--text-muted);">Revenue</div>
    </div>
  `;
}

function renderMonthlyStats() {
  const bookings = getBookings().filter(b => b.status !== 'cancelled');
  const months = {};
  bookings.forEach(b => {
    const monthKey = b.date.substring(0, 7);
    if (!months[monthKey]) months[monthKey] = { count: 0, revenue: 0, finished: 0 };
    months[monthKey].count++;
    if (b.status === 'finished') {
      months[monthKey].finished++;
      months[monthKey].revenue += b.pricePerDay * calculateDays(b.startTime, b.endTime, b.date);
    }
  });
  const sortedMonths = Object.keys(months).sort().reverse();
  if (sortedMonths.length === 0) {
    document.getElementById('monthlyStatsTable').innerHTML = `<p class="text-center py-8" style="color: var(--text-muted);">No booking data yet.</p>`;
    return;
  }
  document.getElementById('monthlyStatsTable').innerHTML = `
    <table class="w-full text-sm" style="min-width: 500px;">
      <thead>
        <tr style="border-bottom: 1px solid var(--border);">
          <th class="text-left py-3 px-4 font-semibold" style="color: var(--text-secondary);">Month</th>
          <th class="text-center py-3 px-4 font-semibold" style="color: var(--text-secondary);">Total Bookings</th>
          <th class="text-center py-3 px-4 font-semibold" style="color: var(--text-secondary);">Finished</th>
          <th class="text-right py-3 px-4 font-semibold" style="color: var(--text-secondary);">Revenue</th>
        </tr>
      </thead>
      <tbody>
        ${sortedMonths.map(m => {
          const d = new Date(m + '-01');
          const label = d.toLocaleDateString('en-US', { year: 'numeric', month: 'long' });
          return `
            <tr style="border-bottom: 1px solid rgba(30,58,95,0.4);">
              <td class="py-3 px-4 font-medium">${label}</td>
              <td class="py-3 px-4 text-center"><span class="inline-block px-3 py-1 rounded-full text-xs font-bold" style="background:rgba(59,130,246,0.15);color:var(--accent-bright);">${months[m].count}</span></td>
              <td class="py-3 px-4 text-center"><span class="inline-block px-3 py-1 rounded-full text-xs font-bold" style="background:rgba(34,197,94,0.15);color:#22c55e;">${months[m].finished}</span></td>
              <td class="py-3 px-4 text-right font-bold" style="color:#22c55e;">$${months[m].revenue}</td>
            </tr>
          `;
        }).join('')}
      </tbody>
    </table>
  `;
}

function renderAdminBookings() {
  const bookings = getBookings();
  let filtered = currentAdminTab === 'all' ? [...bookings] : bookings.filter(b => b.status === currentAdminTab);
  filtered.sort((a, b) => b.date.localeCompare(a.date) || b.startTime.localeCompare(a.startTime));
  const container = document.getElementById('adminBookingsList');
  if (filtered.length === 0) {
    container.innerHTML = `
      <div class="stat-card text-center py-12">
        <i class="fas fa-inbox text-4xl mb-3" style="color: var(--text-muted);"></i>
        <p style="color: var(--text-muted);">No bookings found in this category.</p>
      </div>
    `;
    return;
  }
  container.innerHTML = filtered.map(b => {
    const statusClass = 'status-' + b.status;
    const statusLabel = b.status.charAt(0).toUpperCase() + b.status.slice(1);
    const days = calculateDays(b.startTime, b.endTime, b.date);
    const total = b.pricePerDay * days;
    let actions = '';
    if (b.status === 'pending') {
      actions = `
        <button onclick="finishBooking('${b.id}')" class="btn btn-success text-xs"><i class="fas fa-check"></i> Finish</button>
        <button onclick="cancelBooking('${b.id}')" class="btn btn-danger text-xs"><i class="fas fa-times"></i> Cancel</button>
      `;
    }
    return `
      <div class="stat-card mb-4">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
          <div class="flex-1">
            <div class="flex items-center gap-3 mb-2">
              <h4 class="font-bold">${b.carName}</h4>
              <span class="px-3 py-1 rounded-full text-xs font-bold ${statusClass}">${statusLabel}</span>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-x-6 gap-y-1 text-sm" style="color: var(--text-secondary);">
              <div><i class="fas fa-user mr-2" style="color:var(--accent);width:16px;"></i>${b.customerName}</div>
              <div><i class="fas fa-phone mr-2" style="color:var(--accent);width:16px;"></i>${b.customerPhone}</div>
              <div><i class="fas fa-calendar mr-2" style="color:var(--accent);width:16px;"></i>${b.date}</div>
              <div><i class="fas fa-clock mr-2" style="color:var(--accent);width:16px;"></i>${formatTime(b.startTime)} — ${formatTime(b.endTime)}</div>
              <div><i class="fas fa-id-card mr-2" style="color:var(--accent);width:16px;"></i>Plate: ${b.carPlate}</div>
              <div><i class="fas fa-dollar-sign mr-2" style="color:var(--accent);width:16px;"></i>$${b.pricePerDay}/day &times; ${days}d = <strong style="color:var(--accent-bright);">$${total}</strong></div>
            </div>
          </div>
          <div class="flex gap-2 sm:flex-col">${actions}</div>
        </div>
      </div>
    `;
  }).join('');
}

function finishBooking(id) {
  updateBooking(id, { status: 'finished', finishedAt: new Date().toISOString() });
  showToast('Booking marked as finished.', 'success');
  renderAdminStats(); renderMonthlyStats(); renderAdminBookings();
}
function cancelBooking(id) {
  updateBooking(id, { status: 'cancelled', cancelledAt: new Date().toISOString() });
  showToast('Booking has been cancelled. Car is now available.', 'warning');
  renderAdminStats(); renderMonthlyStats(); renderAdminBookings();
}

function formatTime(t) {
  if (!t) return '';
  const [h, m] = t.split(':');
  const hour = parseInt(h);
  const ampm = hour >= 12 ? 'PM' : 'AM';
  const h12 = hour % 12 || 12;
  return h12 + ':' + m + ' ' + ampm;
}

function calculateDays(startTime, endTime, date) {
  const [sh, sm] = startTime.split(':').map(Number);
  const [eh, em] = endTime.split(':').map(Number);
  const startMins = sh * 60 + sm;
  const endMins = eh * 60 + em;
  const diffHours = (endMins - startMins) / 60;
  if (diffHours <= 0) return 1;
  return Math.max(1, Math.ceil(diffHours / 24));
}

function showToast(message, type) {
  const container = document.getElementById('toastContainer');
  const toast = document.createElement('div');
  toast.className = 'toast ' + (type || 'info');
  const icons = { success: 'fas fa-check-circle', error: 'fas fa-exclamation-circle', warning: 'fas fa-exclamation-triangle', info: 'fas fa-info-circle' };
  toast.innerHTML = `<i class="${icons[type] || icons.info}"></i><span>${message}</span>`;
  container.appendChild(toast);
  setTimeout(() => {
    toast.style.animation = 'toastOut 0.4s ease forwards';
    setTimeout(() => toast.remove(), 400);
  }, 4000);
}

function createParticles() {
  const container = document.getElementById('particles');
  for (let i = 0; i < 30; i++) {
    const p = document.createElement('div');
    p.className = 'particle';
    p.style.left = Math.random() * 100 + '%';
    p.style.animationDuration = (6 + Math.random() * 10) + 's';
    p.style.animationDelay = Math.random() * 8 + 's';
    p.style.width = (2 + Math.random() * 3) + 'px';
    p.style.height = p.style.width;
    p.style.opacity = 0.2 + Math.random() * 0.4;
    container.appendChild(p);
  }
}

document.getElementById('bookingModal').addEventListener('click', function(e) { if (e.target === this) closeBookingModal(); });
document.getElementById('adminLoginModal').addEventListener('click', function(e) { if (e.target === this) closeAdminLogin(); });

document.querySelectorAll('a[href^="#"]').forEach(a => {
  a.addEventListener('click', function(e) {
    e.preventDefault();
    const target = document.querySelector(this.getAttribute('href'));
    if (target) target.scrollIntoView({ behavior: 'smooth' });
  });
});

createParticles();
renderCars();
</script>
</body>
</html>
