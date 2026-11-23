<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="theme-color" content="#000000">
<title>Finora Camera</title>
<style>
  * { margin:0; padding:0; box-sizing:border-box; }
  html, body { width:100%; height:100%; background:#000; color:#fff; font-family:-apple-system,Roboto,sans-serif; overflow:hidden; }
  #camera-container { position:relative; width:100vw; height:100vh; background:#000; }
  #preview { width:100%; height:100%; object-fit:cover; }
  #top-bar { position:absolute; top:0; left:0; right:0; padding:16px; display:flex; justify-content:space-between; background:linear-gradient(180deg,rgba(0,0,0,.6),transparent); }
  .icon-btn { background:rgba(255,255,255,.15); border:none; border-radius:50%; width:44px; height:44px; font-size:20px; color:#fff; cursor:pointer; }
  #bottom-bar { position:absolute; bottom:32px; left:0; right:0; display:flex; justify-content:space-around; align-items:center; padding:0 32px; }
  .round-btn { width:56px; height:56px; border-radius:50%; background:rgba(255,255,255,.15); border:none; color:#fff; font-size:22px; cursor:pointer; }
  .capture-btn { width:76px; height:76px; border-radius:50%; background:#fff; border:4px solid rgba(255,255,255,.5); cursor:pointer; }
  #mode-bar { position:absolute; bottom:120px; left:50%; transform:translateX(-50%); display:flex; gap:8px; background:rgba(0,0,0,.4); border-radius:20px; padding:4px; }
  .mode-btn { background:transparent; border:none; color:#aaa; padding:6px 14px; border-radius:16px; font-size:13px; cursor:pointer; }
  .mode-btn.active { background:#fff; color:#000; }
  #watermark { position:absolute; bottom:120px; right:20px; font-size:14px; font-weight:700; text-shadow:0 1px 4px #000; pointer-events:none; }
  .panel { position:absolute; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,.92); padding:24px; overflow-y:auto; z-index:20; display:flex; flex-direction:column; gap:16px; }
  .panel h3 { margin-bottom:8px; }
  .panel button { background:#007aff; color:#fff; border:none; padding:10px; border-radius:8px; font-size:14px; cursor:pointer; }
  #gallery-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:6px; }
  #gallery-grid img, #gallery-grid video { width:100%; aspect-ratio:1; object-fit:cover; border-radius:4px; }
  #toast { position:absolute; bottom:200px; left:50%; transform:translateX(-50%); background:rgba(0,0,0,.85); padding:10px 20px; border-radius:20px; font-size:13px; z-index:30; text-align:center; width:80%; max-width:300px; }
  .hidden { display:none !important; }
</style>
</head>
<body>
<div id="camera-container">
  <video id="preview" autoplay playsinline muted></video>

  <div id="top-bar">
    <button id="btn-flash" class="icon-btn">⚡</button>
    <button id="btn-settings" class="icon-btn">⚙️</button>
  </div>

  <div id="bottom-bar">
    <button id="btn-gallery" class="round-btn">🖼️</button>
    <button id="btn-capture" class="capture-btn"></button>
    <button id="btn-record" class="round-btn">🎥</button>
  </div>

  <div id="mode-bar">
    <button class="mode-btn active" data-mode="photo">Photo</button>
    <button class="mode-btn" data-mode="video">Video</button>
  </div>

  <div id="watermark">FinoraTech</div>
</div>

<div id="settings-panel" class="panel hidden">
  <h3>Pengaturan</h3>
  <p style="font-size:13px; opacity:0.7;">Pengaturan manual akan ditambahkan di versi native Android. Versi web ini menggunakan pengaturan otomatis dari browser.</p>
  <button id="btn-close-settings">Tutup</button>
</div>

<div id="gallery-panel" class="panel hidden">
  <h3>Galeri</h3>
  <div id="gallery-grid"></div>
  <button id="btn-close-gallery">Tutup</button>
</div>

<canvas id="process-canvas" style="display:none"></canvas>
<div id="toast" class="hidden"></div>

<script>
class CameraManager {
  constructor() { this.stream=null; this.videoEl=null; this.imageCapture=null; this.mediaRecorder=null; this.chunks=[]; this.flashOn=false; }
  
  async init(videoEl) {
    this.videoEl = videoEl;
    try {
      this.stream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: 'environment', width: { ideal: 1920 }, height: { ideal: 1080 } },
        audio: false
      });
      this.videoEl.srcObject = this.stream;
      const track = this.stream.getVideoTracks()[0];
      if (window.ImageCapture) this.imageCapture = new ImageCapture(track);
      return { success: true };
    } catch (e) { return { success: false, error: e.message }; }
  }

  async toggleFlash() {
    const track = this.stream?.getVideoTracks()[0];
    const caps = track?.getCapabilities ? track.getCapabilities() : {};
    if (!caps.torch) return { success: false, error: 'Flash tidak didukung di perangkat/browser ini' };
    this.flashOn = !this.flashOn;
    await track.applyConstraints({ advanced: [{ torch: this.flashOn }] });
    return { success: true, flashOn: this.flashOn };
  }

  async capturePhoto() {
    if (this.imageCapture) {
      try { return { success: true, blob: await this.imageCapture.takePhoto() }; }
      catch (e) { console.warn("ImageCapture gagal, pakai fallback canvas", e); }
    }
    const c = document.getElementById('process-canvas');
    c.width = this.videoEl.videoWidth; c.height = this.videoEl.videoHeight;
    c.getContext('2d').drawImage(this.videoEl, 0, 0);
    return new Promise(r => c.toBlob(b => r({ success: true, blob: b }), 'image/jpeg', 0.95));
  }

  startRecording() {
    if (!window.MediaRecorder) return { success: false, error: 'Perekam video tidak didukung browser ini' };
    let mime = 'video/webm;codecs=vp9';
    if (!MediaRecorder.isTypeSupported(mime)) mime = 'video/webm';
    this.chunks = [];
    this.mediaRecorder = new MediaRecorder(this.stream, { mimeType: mime });
    this.mediaRecorder.ondataavailable = e => { if (e.data.size) this.chunks.push(e.data); };
    this.mediaRecorder.start(1000);
    return { success: true };
  }

  stopRecording() {
    return new Promise(r => {
      if (!this.mediaRecorder) return r({ success: false, error: "Tidak sedang merekam" });
      this.mediaRecorder.onstop = () => r({ success: true, blob: new Blob(this.chunks, { type: 'video/webm' }) });
      this.mediaRecorder.stop();
    });
  }
}

const cam = new CameraManager();
let mode = 'photo';
let recording = false;
let gallery = [];
const $ = id => document.getElementById(id);

async function boot() {
  // Minta izin kamera
  const res = await cam.init($('preview'));
  if (!res.success) { 
    showToast('Kamera gagal dibuka: ' + res.error); 
    return; 
  }

  // Tombol Ambil Foto
  $('btn-capture').onclick = async () => {
    if (mode !== 'photo') return showToast('Beralih ke mode Photo dulu');
    const r = await cam.capturePhoto();
    if (!r.success) return showToast('Gagal foto');
    
    // Tambah watermark
    const out = await applyWatermark(r.blob);
    const name = 'finora_' + Date.now() + '.jpg';
    download(out, name);
    gallery.push({ type: 'image', blob: out });
    showToast('Foto disimpan');
  };

  // Tombol Rekam Video
  $('btn-record').onclick = async () => {
    if (mode !== 'video') return showToast('Beralih ke mode Video dulu');
    
    if (!recording) {
      const r = cam.startRecording();
      if (!r.success) return showToast(r.error);
      recording = true;
      $('btn-record').style.background = '#ff3b30';
      showToast('Merekam...');
    } else {
      const r = await cam.stopRecording();
      recording = false;
      $('btn-record').style.background = '';
      if (!r.success) return showToast('Gagal merekam');
      
      const name = 'finora_' + Date.now() + '.webm';
      download(r.blob, name);
      gallery.push({ type: 'video', blob: r.blob });
      showToast('Video disimpan');
    }
  };

  // Tombol Flash
  $('btn-flash').onclick = async () => {
    const r = await cam.toggleFlash();
    showToast(r.success ? (r.flashOn ? 'Flash ON' : 'Flash OFF') : r.error);
  };

  // Panel Galeri & Pengaturan
  $('btn-gallery').onclick = () => { renderGallery(); $('gallery-panel').classList.remove('hidden'); };
  $('btn-close-gallery').onclick = () => $('gallery-panel').classList.add('hidden');
  $('btn-settings').onclick = () => $('settings-panel').classList.remove('hidden');
  $('btn-close-settings').onclick = () => $('settings-panel').classList.add('hidden');

  // Mode Selector
  document.querySelectorAll('.mode-btn').forEach(b => {
    b.onclick = () => {
      document.querySelectorAll('.mode-btn').forEach(x => x.classList.remove('active'));
      b.classList.add('active');
      mode = b.dataset.mode;
    };
  });
}

async function applyWatermark(blob) {
  const bmp = await createImageBitmap(blob);
  const c = document.createElement('canvas');
  c.width = bmp.width; c.height = bmp.height;
  const ctx = c.getContext('2d');
  ctx.drawImage(bmp, 0, 0);
  const fs = Math.max(24, c.width * 0.03);
  ctx.font = 'bold ' + fs + 'px sans-serif';
  ctx.fillStyle = 'rgba(255,255,255,0.85)';
  ctx.shadowColor = 'rgba(0,0,0,0.6)'; ctx.shadowBlur = 4;
  const t = 'FinoraTech';
  ctx.fillText(t, c.width - ctx.measureText(t).width - 20, c.height - 20);
  return new Promise(r => c.toBlob(r, 'image/jpeg', 0.95));
}

function download(blob, name) {
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = name; a.click();
  setTimeout(() => URL.revokeObjectURL(url), 5000);
}

function renderGallery() {
  const g = $('gallery-grid');
  g.innerHTML = '';
  if (!gallery.length) { 
    g.innerHTML = '<p style="grid-column:1/-1;text-align:center;opacity:.5">Belum ada media</p>'; 
    return; 
  }
  gallery.forEach(item => {
    const el = document.createElement(item.type === 'image' ? 'img' : 'video');
    el.src = URL.createObjectURL(item.blob);
    if (item.type === 'video') el.controls = true;
    g.appendChild(el);
  });
}

function showToast(msg) {
  const t = $('toast');
  t.textContent = msg;
  t.classList.remove('hidden');
  clearTimeout(t._timer);
  t._timer = setTimeout(() => t.classList.add('hidden'), 2500);
}

window.addEventListener('DOMContentLoaded', boot);
</script>
</body>
</html>
