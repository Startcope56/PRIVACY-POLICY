
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Blue Gallery Fixed</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap');

* { margin: 0; padding: 0; box-sizing: border-box; }
body {
    font-family: 'Poppins', sans-serif;
    background: #0a192f;
    background-image:
        radial-gradient(circle at 20% 50%, rgba(59, 130, 246, 0.15) 0%, transparent 50%),
        radial-gradient(circle at 80% 80%, rgba(100, 255, 218, 0.1) 0%, transparent 50%);
    color: #e6f1ff;
    min-height: 100vh;
}

/* Glass Header - tinaasan ko z-index */
header {
    background: rgba(17, 34, 64, 0.95);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid rgba(100, 255, 218, 0.3);
    padding: 20px 25px;
    position: sticky;
    top: 0;
    z-index: 9999;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
h1 {
    font-size: 26px;
    font-weight: 700;
    background: linear-gradient(90deg, #64ffda 0%, #3b82f6 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}
.menu-btn {
    background: rgba(100, 255, 218, 0.15);
    border: 2px solid #64ffda;
    color: #64ffda;
    padding: 12px 20px;
    border-radius: 14px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
    z-index: 10000;
}
.menu-btn:hover {
    background: #64ffda;
    color: #0a192f;
    transform: scale(1.05);
}

/* Menu - tinago ko sa wrapper para di ma-block */
.menu-wrapper {
    position: relative;
}
.menu {
    display: none;
    position: absolute;
    top: 55px;
    right: 0;
    background: rgba(17, 34, 64, 0.98);
    backdrop-filter: blur(20px);
    border: 2px solid rgba(100, 255, 218, 0.5);
    border-radius: 18px;
    padding: 15px;
    min-width: 220px;
    box-shadow: 0 25px 60px rgba(0,0,0,0.7);
    z-index: 10001;
}
.menu.active { display: block; animation: slideDown 0.3s ease; }
@keyframes slideDown {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 1; transform: translateY(0); }
}
.menu button {
    width: 100%;
    background: rgba(100, 255, 218, 0.08);
    color: #e2e8f0;
    border: none;
    padding: 15px;
    border-radius: 12px;
    font-size: 15px;
    margin-bottom: 8px;
    cursor: pointer;
    transition: all 0.3s;
    text-align: left;
}
.menu button:hover {
    background: rgba(100, 255, 218, 0.2);
    transform: translateX(8px);
    color: #64ffda;
}

/* Gallery */
.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(170px, 1fr));
    gap: 20px;
    padding: 30px 25px 120px;
    max-width: 1300px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
}
.img-box {
    position: relative;
    background: rgba(17, 34, 64, 0.4);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    overflow: hidden;
    aspect-ratio: 1;
    border: 1px solid rgba(100, 255, 218, 0.2);
    transition: all 0.3s;
}
.img-box:hover {
    transform: translateY(-8px) scale(1.03);
    border-color: #64ffda;
    box-shadow: 0 15px 40px rgba(100, 255, 218, 0.3);
}
.img-box img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    pointer-events: none;
}
.delete-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    background: rgba(255, 59, 48, 0.95);
    border: 2px solid white;
    color: white;
    width: 32px;
    height: 32px;
    border-radius: 50%;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    opacity: 0;
    transition: all 0.3s;
    z-index: 10;
}
.img-box:hover.delete-btn { opacity: 1; }

/* FAB Button */
.fab {
    position: fixed;
    bottom: 35px;
    right: 35px;
    width: 70px;
    height: 70px;
    border-radius: 50%;
    border: 3px solid #64ffda;
    font-size: 32px;
    cursor: pointer;
    background: linear-gradient(135deg, #64ffda 0%, #10b981 100%);
    color: #0a192f;
    box-shadow: 0 12px 35px rgba(100, 255, 218, 0.5);
    transition: all 0.4s;
    z-index: 9998;
}
.fab:hover {
    transform: scale(1.15) rotate(8deg);
}

/* Camera Modal */
.modal {
    display: none;
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: #000;
    z-index: 100000;
    flex-direction: column;
}
.modal.active { display: flex; }
video {
    flex: 1;
    width: 100%;
    object-fit: cover;
    background: #000;
}
.camera-controls {
    display: flex;
    gap: 25px;
    padding: 30px;
    background: rgba(10, 25, 47, 0.95);
    justify-content: center;
}
.cam-btn {
    padding: 18px 40px;
    border: 3px solid;
    border-radius: 60px;
    font-size: 17px;
    font-weight: 700;
    cursor: pointer;
}
.capture-btn {
    background: #64ffda;
    color: #0a192f;
    border-color: #64ffda;
}
.close-btn {
    background: transparent;
    color: #94a3b8;
    border-color: #475569;
}

/* About Modal */
.about-overlay {
    display: none;
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.85);
    backdrop-filter: blur(10px);
    z-index: 100001;
    justify-content: center;
    align-items: center;
}
.about-overlay.active { display: flex; }
.about-box {
    background: linear-gradient(135deg, rgba(17, 34, 64, 0.98) 0%, rgba(30, 41, 59, 0.98) 100%);
    border: 2px solid #64ffda;
    border-radius: 30px;
    padding: 40px 35px;
    max-width: 450px;
    width: 90%;
    text-align: center;
}
.about-box h2 {
    font-size: 32px;
    background: linear-gradient(90deg, #64ffda 0%, #3b82f6 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 15px;
}
.credits {
    background: rgba(100, 255, 218, 0.1);
    border: 1px solid rgba(100, 255, 218, 0.3);
    border-radius: 15px;
    padding: 20px;
    margin: 25px 0;
}
.credits h3 {
    color: #64ffda;
    margin-bottom: 12px;
}
.credits p {
    color: #f8fafc;
    font-weight: 600;
    margin: 8px 0;
}
.close-about {
    background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
    color: white;
    border: none;
    padding: 14px 40px;
    border-radius: 50px;
    font-size: 16px;
    font-weight: 700;
    cursor: pointer;
}

.empty {
    text-align: center;
    padding: 120px 30px;
    color: #64748b;
}
.empty h2 {
    font-size: 32px;
    background: linear-gradient(90deg, #64ffda 0%, #3b82f6 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 15px;
}
canvas { display: none; }
</style>
</head>
<body>

<header>
    <h1>💙 Blue Gallery</h1>
    <div class="menu-wrapper">
        <button class="menu-btn" id="menuBtn">☰ Menu</button>
        <div id="menu" class="menu">
            <button onclick="showInfo(); closeMenu()">📊 Storage Info</button>
            <button onclick="clearAll(); closeMenu()">🗑️ Delete All Photos</button>
            <button onclick="openAbout(); closeMenu()">ℹ️ About This App</button>
        </div>
    </div>
</header>

<div id="gallery" class="gallery"></div>
<div id="emptyMsg" class="empty">
    <h2>Gallery is Empty</h2>
    <p>Pindot mo camera button sa baba para mag-take photo</p>
</div>

<button class="fab" id="camBtn">📷</button>

<div id="cameraModal" class="modal">
    <video id="video" autoplay playsinline></video>
    <div class="camera-controls">
        <button class="cam-btn capture-btn" id="captureBtn">📷 TAKE PHOTO</button>
        <button class="cam-btn close-btn" id="closeCamBtn">✕ CLOSE</button>
    </div>
</div>

<div id="aboutOverlay" class="about-overlay">
    <div class="about-box">
        <h2>💙 Blue Gallery</h2>
        <p>Offline Photo Gallery with Unlimited Storage</p>
        <p>100% Offline. All photos saved on your phone.</p>

        <div class="credits">
            <h3>👨‍💻 Created By:</h3>
            <p>Zack Calix</p>
            <p>Christopher Smith</p>
            <p>Maleo Bussid</p>
            <p>Rejz</p>
        </div>

        <p style="font-size: 13px; color: #94a3b8;">Version 3.1 Fixed • Offline Support • 2026</p>
        <button class="close-about" id="closeAboutBtn">Got it!</button>
    </div>
</div>

<canvas id="canvas"></canvas>

<script>
const gallery = document.getElementById('gallery');
const emptyMsg = document.getElementById('emptyMsg');
const video = document.getElementById('video');
const canvas = document.getElementById('canvas');
const modal = document.getElementById('cameraModal');
const menu = document.getElementById('menu');
const menuBtn = document.getElementById('menuBtn');
const camBtn = document.getElementById('camBtn');
const captureBtn = document.getElementById('captureBtn');
const closeCamBtn = document.getElementById('closeCamBtn');
const aboutOverlay = document.getElementById('aboutOverlay');
const closeAboutBtn = document.getElementById('closeAboutBtn');
let stream = null;

function loadImages() {
    gallery.innerHTML = '';
    let images = JSON.parse(localStorage.getItem('blueGallery') || '[]');

    if(images.length === 0) {
        emptyMsg.style.display = 'block';
        return;
    }
    emptyMsg.style.display = 'none';

    images.forEach((img, index) => {
        let div = document.createElement('div');
        div.className = 'img-box';
        div.innerHTML = `
            <img src="${img}" alt="Photo ${index+1}">
            <button class="delete-btn" onclick="deleteImage(${index})">×</button>
        `;
        gallery.appendChild(div);
    });
}

function saveImage(dataURL) {
    let images = JSON.parse(localStorage.getItem('blueGallery') || '[]');
    images.unshift(dataURL);
    localStorage.setItem('blueGallery', JSON.stringify(images));
    loadImages();
}

function deleteImage(index) {
    let images = JSON.parse(localStorage.getItem('blueGallery') || '[]');
    images.splice(index, 1);
    localStorage.setItem('blueGallery', JSON.stringify(images));
    loadImages();
}

function toggleMenu(e) {
    e.stopPropagation();
    menu.classList.toggle('active');
}

function closeMenu() {
    menu.classList.remove('active');
}

// Close menu pag click outside
document.addEventListener('click', (e) => {
    if(!menu.contains(e.target) && !menuBtn.contains(e.target)) {
        closeMenu();
    }
});

function showInfo() {
    let images = JSON.parse(localStorage.getItem('blueGallery') || '[]');
    let size = (JSON.stringify(images).length / 1024 / 1024).toFixed(2);
    alert(`📊 Blue Gallery Stats\nTotal Photos: ${images.length}\nStorage Used: ${size} MB\nStatus: 100% Offline`);
}

function clearAll() {
    if(confirm('🗑️ Delete all photos? Permanent to!')) {
        localStorage.removeItem('blueGallery');
        loadImages();
    }
}

function openAbout() {
    aboutOverlay.classList.add('active');
}

function closeAbout() {
    aboutOverlay.classList.remove('active');
}

async function openModal() {
    modal.classList.add('active');
    try {
        stream = await navigator.mediaDevices.getUserMedia({
            video: { facingMode: 'environment', width: {ideal: 1280} }
        });
        video.srcObject = stream;
    } catch(err) {
        alert('Camera error: ' + err.message + '\n\nAllow camera permission sa browser settings');
        closeCamera();
    }
}

function takePhoto() {
    canvas.width = 1280;
    canvas.height = video.videoHeight * (1280 / video.videoWidth);
    canvas.getContext('2d').drawImage(video, 0, 0, canvas.width, canvas.height);
    let dataURL = canvas.toDataURL('image/jpeg', 0.8);
    saveImage(dataURL);
    closeCamera();
}

function closeCamera() {
    modal.classList.remove('active');
    if(stream) {
        stream.getTracks().forEach(track => track.stop());
        stream = null;
    }
    video.srcObject = null;
}

// Event listeners - direct binding para sure na clickable
menuBtn.addEventListener('click', toggleMenu);
camBtn.addEventListener('click', openModal);
captureBtn.addEventListener('click', takePhoto);
closeCamBtn.addEventListener('click', closeCamera);
closeAboutBtn.addEventListener('click', closeAbout);
aboutOverlay.addEventListener('click', closeAbout);

loadImages();
</script>

</body>
</html>
