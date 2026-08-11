# responsive-template
#!/usr/bin/env python3
"""
create_site.py
Gera um site simples (index.html) com um template responsivo e inicia um servidor local.
Uso: python3 create_site.py [--port PORT] [--dir DIR] [--no-open]
"""

import os
import argparse
import http.server
import socketserver
import webbrowser
from pathlib import Path
import textwrap
import sys

INDEX_HTML = """<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Template Responsivo — anareeis02-png</title>
  <meta name="description" content="Template responsivo básico (mobile-first) por anareeis02-png">
  <style>
    :root{
      --max-width:1200px;
      --gap:1rem;
      --brand:#0b6cf0;
      --bg:#f7f8fb;
      --text:#222;
      --muted:#6b7280;
    }
    *,*::before,*::after{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      color:var(--text);
      background:var(--bg);
      line-height:1.4;
    }
    .container{max-width:var(--max-width);margin:0 auto;padding:0 1rem}
    header{background:#fff;border-bottom:1px solid #e6e9ee;position:sticky;top:0;z-index:50}
    .header-inner{display:flex;align-items:center;justify-content:space-between;gap:var(--gap);height:64px}
    .brand{display:flex;align-items:center;gap:.5rem;font-weight:700;color:var(--brand);text-decoration:none}
    nav{display:flex;align-items:center;gap:var(--gap)}
    .nav-list{display:none;list-style:none;margin:0;padding:0;gap:var(--gap)}
    .nav-list a{text-decoration:none;color:var(--text);padding:.5rem .75rem;border-radius:6px;font-weight:600}
    .menu-toggle{background:transparent;border:0;padding:.5rem;font-size:1.1rem;display:inline-flex;align-items:center;gap:.5rem;cursor:pointer}
    .hero{background:linear-gradient(180deg, rgba(11,108,240,0.07), transparent 60%);padding:2.25rem 0}
    .hero-grid{display:grid;gap:1rem;align-items:center;grid-template-columns: 1fr}
    h1{font-size:clamp(1.5rem,4vw,2.25rem); margin:.25rem 0}
    p.lead{color:var(--muted); margin:0 0 .5rem}
    .cards{display:grid;gap:1rem;grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));margin:1.25rem 0}
    .card{background:#fff;padding:1rem;border-radius:8px;box-shadow:0 1px 3px rgba(16,24,40,0.04);display:flex;flex-direction:column;gap:.75rem;min-height:140px}
    footer{padding:1.5rem 0;color:var(--muted);font-size:.95rem}
    @media (min-width: 768px){
      .nav-list{display:flex}
      .menu-toggle{display:none}
      .hero-grid{grid-template-columns: 1fr 380px}
    }
    .btn{display:inline-block;background:var(--brand);color:#fff;padding:.5rem .85rem;border-radius:8px;text-decoration:none;font-weight:600}
  </style>
</head>
<body>
  <header>
    <div class="container header-inner">
      <a class="brand" href="#" aria-label="Início">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none" aria-hidden="true">
          <rect x="2" y="2" width="20" height="20" rx="6" fill="#0b6cf0"/>
          <path d="M7 12h10" stroke="#fff" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        anareeis02-png
      </a>

      <nav>
        <button class="menu-toggle" aria-expanded="false" aria-controls="primary-nav">
          <span class="sr-only">Abrir menu</span>
          ☰
        </button>

        <ul id="primary-nav" class="nav-list" role="menubar">
          <li role="none"><a role="menuitem" href="#features">Recursos</a></li>
          <li role="none"><a role="menuitem" href="#pricing">Preços</a></li>
          <li role="none"><a role="menuitem" href="#about">Sobre</a></li>
          <li role="none"><a role="menuitem" class="btn" href="#start">Começar</a></li>
        </ul>
      </nav>
    </div>
  </header>

  <main>
    <section class="hero">
      <div class="container hero-grid">
        <div class="hero-inner">
          <h1>Template Responsivo — pronto para iniciar</h1>
          <p class="lead">Mobile-first, com Grid/Flexbox e imagens responsivas. Criado enquanto estudo GitHub.</p>
          <p style="margin-top:.75rem"><a class="btn" href="#features">Ver recursos</a></p>
        </div>

        <aside>
          <picture>
            <source type="image/avif" srcset="https://via.placeholder.com/800x500.avif 800w, https://via.placeholder.com/480x300.avif 480w" sizes="(max-width:600px) 100vw, 380px">
            <img src="https://via.placeholder.com/800x500.png" alt="Exemplo de imagem" srcset="https://via.placeholder.com/800x500.png 800w, https://via.placeholder.com/480x300.png 480w" sizes="(max-width:600px) 100vw, 380px" loading="lazy" style="border-radius:8px; width:100%; height:auto;">
          </picture>
        </aside>
      </div>
    </section>

    <section class="container" id="features" aria-label="Recursos">
      <h2 style="margin-top:1.25rem">Recursos</h2>
      <div class="cards">
        <article class="card">
          <h3>Layout flexível</h3>
          <p>Grid & Flexbox para composições que se adaptam bem.</p>
        </article>

        <article class="card">
          <h3>Imagens responsivas</h3>
          <p>Exemplo com srcset/sizes e loading lazy.</p>
        </article>

        <article class="card">
          <h3>Tipografia fluida</h3>
          <p>Clamps e unidades relativas para manter legibilidade.</p>
        </article>

        <article class="card">
          <h3>Acessibilidade</h3>
          <p>Foco em targets touch, contraste e navegação por teclado.</p>
        </article>
      </div>
    </section>
  </main>

  <footer>
    <div class="container">
      <div style="display:flex;justify-content:space-between;align-items:center;gap:1rem;flex-wrap:wrap">
        <small>© 2026 anareeis02-png — Projeto de estudo</small>
        <nav aria-label="Links de rodapé">
          <a href="https://github.com/anareeis02-png" style="color:var(--muted);text-decoration:none;margin-left:.5rem">github.com/anareeis02-png</a>
        </nav>
      </div>
    </div>
  </footer>

  <script>
    const btn = document.querySelector('.menu-toggle');
    const nav = document.getElementById('primary-nav');
    if (btn) {
      btn.addEventListener('click', () => {
        const expanded = btn.getAttribute('aria-expanded') === 'true';
        btn.setAttribute('aria-expanded', String(!expanded));
        nav.style.display = expanded ? 'none' : 'flex';
      });
    }
    window.addEventListener('resize', () => {
      if (window.innerWidth >= 768) {
        nav.style.display = 'flex';
        if (btn) btn.setAttribute('aria-expanded','false');
      } else {
        nav.style.display = 'none';
        if (btn) btn.setAttribute('aria-expanded','false');
      }
    });
    if (window.innerWidth < 768 && nav) nav.style.display = 'none';
  </script>
</body>
</html>
"""

