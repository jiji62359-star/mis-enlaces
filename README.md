<!--
  index.html
  Página de enlaces segura (cliente) — HTML + CSS + JavaScript
  Instrucciones: guarda este archivo como index.html y ábrelo en tu navegador.

  Contenido: página con accesos directos a Spotify, Instagram, Facebook y YouTube.
  Incluye un acceso con contraseña simple (solo en el navegador) — NO SALTA BLOQUEOS DE RED.
  Cambia la contraseña en la variable `PASSWORD` en el script si quieres.
-->

<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Mis Enlaces</title>
  <meta name="description" content="Página de enlaces rápidos: Spotify, Instagram, Facebook y YouTube." />

  <style>
    :root{ --bg:#0b1220; --card:#0f1724; --accent:#1db954; --muted:#9aa6b2 }
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,system-ui,-apple-system,'Segoe UI',Roboto,Arial;background:linear-gradient(180deg,#071021,#071422);color:#eaf5f2;min-height:100vh;display:flex;align-items:center;justify-content:center}
    .wrap{max-width:720px;width:94%;padding:20px}
    header{text-align:center;margin-bottom:18px}
    h1{margin:0;font-size:24px}
    p.lead{color:var(--muted);margin:8px 0 18px}

    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px}
    .card{background:rgba(255,255,255,0.03);padding:18px;border-radius:12px;text-align:center;box-shadow:0 6px 20px rgba(0,0,0,0.5)}
    .card a{display:block;text-decoration:none;color:inherit}
    .icon{font-size:28px;margin-bottom:8px}
    .label{font-weight:700}
    .sub{color:var(--muted);font-size:13px}

    .controls{display:flex;gap:8px;justify-content:center;margin-top:16px}
    .btn{background:var(--accent);color:#06140b;padding:10px 14px;border-radius:10px;text-decoration:none;font-weight:700;border:none;cursor:pointer}
    .btn.secondary{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--muted)}

    footer{margin-top:20px;text-align:center;color:var(--muted);font-size:13px}

    /* Modal simple */
    .modal{position:fixed;inset:0;display:grid;place-items:center;background:rgba(2,6,23,0.6);visibility:hidden;opacity:0;transition:all .18s}
    .modal.show{visibility:visible;opacity:1}
    .modal .box{background:var(--card);padding:20px;border-radius:12px;min-width:300px}
    input{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);margin-top:8px;background:transparent;color:inherit}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Mis Enlaces</h1>
      <p class="lead">Accesos rápidos — Spotify, Instagram, Facebook y YouTube.
      Esta página tiene un acceso con contraseña sencilla (solo local) para organizar los enlaces.</p>
      <div class="controls">
        <button class="btn" id="openAll">Abrir todos (nueva ventana)</button>
        <button class="btn secondary" id="unlockBtn">Mostrar enlaces</button>
      </div>
    </header>

    <main id="linksArea">
      <div class="grid">
        <!-- Cada tarjeta enlaza a la web correspondiente -->
        <div class="card">
          <a href="https://open.spotify.com/" target="_blank" rel="noopener noreferrer">
            <div class="icon">🎧</div>
            <div class="label">Spotify</div>
            <div class="sub">Escucha música</div>
          </a>
        </div>

        <div class="card">
          <a href="https://www.instagram.com/" target="_blank" rel="noopener noreferrer">
            <div class="icon">📸</div>
            <div class="label">Instagram</div>
            <div class="sub">Red social de fotos</div>
          </a>
        </div>

        <div class="card">
          <a href="https://www.facebook.com/" target="_blank" rel="noopener noreferrer">
            <div class="icon">👥</div>
            <div class="label">Facebook</div>
            <div class="sub">Conecta con amigos</div>
          </a>
        </div>

        <div class="card">
          <a href="https://www.youtube.com/" target="_blank" rel="noopener noreferrer">
            <div class="icon">▶️</div>
            <div class="label">YouTube</div>
            <div class="sub">Videos y entretenimiento</div>
          </a>
        </div>
      </div>

      <footer>
        <div>Consejo: si la escuela bloquea estas webs, no puedo ayudar a saltarte los filtros de red.</div>
      </footer>
    </main>
  </div>

  <!-- Modal de contraseña (solo cliente) -->
  <div id="modal" class="modal">
    <div class="box">
      <div style="font-weight:700">Introduce la contraseña</div>
      <input id="pwInput" type="password" placeholder="Contraseña" />
      <div style="display:flex;gap:8px;margin-top:12px;justify-content:flex-end">
        <button class="btn secondary" id="cancelPw">Cancelar</button>
        <button class="btn" id="okPw">Entrar</button>
      </div>
    </div>
  </div>

  <script>
    // PASSWORD: cambia esto si quieres otra contraseña (es visible en el código)
    const PASSWORD = '123';

    const modal = document.getElementById('modal');
    const unlockBtn = document.getElementById('unlockBtn');
    const cancelPw = document.getElementById('cancelPw');
    const okPw = document.getElementById('okPw');
    const pwInput = document.getElementById('pwInput');
    const linksArea = document.getElementById('linksArea');
    const openAll = document.getElementById('openAll');

    // Mostrar modal
    unlockBtn.addEventListener('click', ()=>{ modal.classList.add('show'); pwInput.value=''; pwInput.focus(); });
    cancelPw.addEventListener('click', ()=>{ modal.classList.remove('show'); });

    okPw.addEventListener('click', ()=>{
      const val = pwInput.value;
      if(val === PASSWORD){
        modal.classList.remove('show');
        // simplemente mostramos un pequeño mensaje de confirmación
        alert('Enlaces desbloqueados. Recuerda: esto no evita bloqueos de red.');
      } else {
        alert('Contraseña incorrecta');
      }
    });

    // Abrir todos en nuevas pestañas (nota: el navegador puede bloquear ventanas emergentes)
    openAll.addEventListener('click', ()=>{
      const anchors = linksArea.querySelectorAll('a');
      anchors.forEach(a=>{ window.open(a.href, '_blank', 'noopener'); });
    });

    // Seguridad / advertencia: esta contraseña solo funciona en el navegador donde se abra el archivo.
    // No es un bypass de filtros. Si la red bloquea esas páginas, abrirlas desde aquí seguirá sin funcionar.
  </script>
</body>
</html>
