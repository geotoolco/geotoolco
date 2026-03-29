<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GeoTool — Generative Engine Optimization</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Mono:ital,wght@0,300;0,400;1,300&display=swap" rel="stylesheet" />
  <style>
    :root {
      --bg: #0a0a0f;
      --surface: #111118;
      --border: #1e1e2e;
      --accent: #7fffb2;
      --accent2: #38f9d7;
      --accent3: #ffe44d;
      --text: #e8e8f0;
      --muted: #6b6b88;
      --danger: #ff6b6b;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Syne', sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* Grid background */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        linear-gradient(rgba(127,255,178,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(127,255,178,0.03) 1px, transparent 1px);
      background-size: 60px 60px;
      pointer-events: none;
      z-index: 0;
    }

    /* Glow orbs */
    .orb {
      position: fixed;
      border-radius: 50%;
      filter: blur(120px);
      pointer-events: none;
      z-index: 0;
    }
    .orb-1 { width: 600px; height: 600px; background: rgba(127,255,178,0.06); top: -200px; right: -100px; }
    .orb-2 { width: 400px; height: 400px; background: rgba(56,249,215,0.05); bottom: 200px; left: -100px; }
    .orb-3 { width: 300px; height: 300px; background: rgba(255,228,77,0.04); top: 50%; left: 50%; transform: translate(-50%,-50%); }

    .container {
      max-width: 860px;
      margin: 0 auto;
      padding: 0 2rem;
      position: relative;
      z-index: 1;
    }

    /* ── NAV ── */
    nav {
      padding: 1.5rem 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid var(--border);
      position: sticky;
      top: 0;
      background: rgba(10,10,15,0.85);
      backdrop-filter: blur(12px);
      z-index: 100;
    }

    .nav-inner {
      max-width: 860px;
      margin: 0 auto;
      padding: 0 2rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      width: 100%;
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      font-weight: 800;
      font-size: 1.1rem;
      letter-spacing: -0.02em;
    }

    .logo-icon {
      width: 28px;
      height: 28px;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      border-radius: 6px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.75rem;
    }

    .badge-nav {
      font-family: 'DM Mono', monospace;
      font-size: 0.65rem;
      color: var(--accent);
      border: 1px solid rgba(127,255,178,0.3);
      padding: 0.2rem 0.5rem;
      border-radius: 4px;
      letter-spacing: 0.08em;
    }

    .nav-links {
      display: flex;
      gap: 1.5rem;
      font-size: 0.85rem;
      color: var(--muted);
    }

    .nav-links a {
      color: var(--muted);
      text-decoration: none;
      transition: color 0.2s;
    }

    .nav-links a:hover { color: var(--text); }

    /* ── HERO ── */
    .hero {
      padding: 5rem 0 4rem;
      animation: fadeUp 0.8s ease both;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(24px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .hero-eyebrow {
      font-family: 'DM Mono', monospace;
      font-size: 0.7rem;
      letter-spacing: 0.15em;
      color: var(--accent);
      text-transform: uppercase;
      margin-bottom: 1.2rem;
      display: flex;
      align-items: center;
      gap: 0.6rem;
    }

    .hero-eyebrow::before {
      content: '';
      display: block;
      width: 24px;
      height: 1px;
      background: var(--accent);
    }

    h1 {
      font-size: clamp(2.8rem, 6vw, 4.2rem);
      font-weight: 800;
      line-height: 1.05;
      letter-spacing: -0.03em;
      margin-bottom: 1.5rem;
    }

    h1 .highlight {
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .hero-desc {
      font-size: 1.15rem;
      color: var(--muted);
      line-height: 1.7;
      max-width: 600px;
      margin-bottom: 2.5rem;
      font-weight: 400;
    }

    .hero-cta {
      display: flex;
      gap: 1rem;
      flex-wrap: wrap;
      align-items: center;
    }

    .btn-primary {
      display: inline-flex;
      align-items: center;
      gap: 0.5rem;
      background: var(--accent);
      color: var(--bg);
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 0.9rem;
      padding: 0.8rem 1.6rem;
      border-radius: 8px;
      text-decoration: none;
      transition: all 0.2s;
      letter-spacing: 0.01em;
    }

    .btn-primary:hover {
      background: var(--accent2);
      transform: translateY(-1px);
      box-shadow: 0 8px 32px rgba(127,255,178,0.2);
    }

    .btn-ghost {
      display: inline-flex;
      align-items: center;
      gap: 0.5rem;
      color: var(--muted);
      font-size: 0.9rem;
      text-decoration: none;
      border: 1px solid var(--border);
      padding: 0.8rem 1.4rem;
      border-radius: 8px;
      transition: all 0.2s;
    }

    .btn-ghost:hover {
      color: var(--text);
      border-color: var(--muted);
    }

    /* ── TERMINAL ── */
    .terminal {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      overflow: hidden;
      margin: 3rem 0;
      animation: fadeUp 0.8s ease 0.15s both;
    }

    .terminal-bar {
      background: rgba(30,30,46,0.8);
      padding: 0.75rem 1rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      border-bottom: 1px solid var(--border);
    }

    .dot { width: 10px; height: 10px; border-radius: 50%; }
    .dot-r { background: #ff5f57; }
    .dot-y { background: #febc2e; }
    .dot-g { background: #28c840; }

    .terminal-label {
      font-family: 'DM Mono', monospace;
      font-size: 0.7rem;
      color: var(--muted);
      margin-left: 0.5rem;
    }

    .terminal-body {
      padding: 1.5rem;
      font-family: 'DM Mono', monospace;
      font-size: 0.82rem;
      line-height: 1.9;
    }

    .t-prompt { color: var(--muted); }
    .t-cmd { color: var(--accent2); }
    .t-comment { color: var(--muted); font-style: italic; }
    .t-key { color: var(--accent3); }
    .t-val { color: var(--accent); }
    .t-url { color: #a29bfe; text-decoration: underline; }
    .t-line { display: block; }

    /* ── SECTION LABELS ── */
    .section-label {
      font-family: 'DM Mono', monospace;
      font-size: 0.65rem;
      letter-spacing: 0.15em;
      color: var(--muted);
      text-transform: uppercase;
      border-bottom: 1px solid var(--border);
      padding-bottom: 0.75rem;
      margin-bottom: 2rem;
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .section-label::before {
      content: '#';
      color: var(--accent);
    }

    /* ── WHAT IS GEO ── */
    .what-section {
      padding: 2rem 0;
      animation: fadeUp 0.8s ease 0.2s both;
    }

    .what-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.5rem;
      margin-top: 1.5rem;
    }

    .compare-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 1.5rem;
    }

    .compare-card.old { border-top: 2px solid var(--danger); }
    .compare-card.new { border-top: 2px solid var(--accent); }

    .compare-title {
      font-size: 0.7rem;
      font-family: 'DM Mono', monospace;
      letter-spacing: 0.1em;
      margin-bottom: 1rem;
    }

    .compare-card.old .compare-title { color: var(--danger); }
    .compare-card.new .compare-title { color: var(--accent); }

    .compare-list {
      list-style: none;
      font-size: 0.88rem;
      color: var(--muted);
      line-height: 2;
    }

    .compare-list li::before {
      content: '→  ';
      font-family: 'DM Mono', monospace;
    }

    .compare-card.old .compare-list li::before { color: var(--danger); }
    .compare-card.new .compare-list li::before { color: var(--accent); }

    /* ── FEATURES ── */
    .features {
      padding: 2rem 0;
      animation: fadeUp 0.8s ease 0.25s both;
    }

    .features-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1rem;
      margin-top: 1.5rem;
    }

    .feature-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 1.5rem;
      transition: border-color 0.2s, transform 0.2s;
    }

    .feature-card:hover {
      border-color: rgba(127,255,178,0.3);
      transform: translateY(-2px);
    }

    .feature-icon {
      font-size: 1.4rem;
      margin-bottom: 0.75rem;
    }

    .feature-title {
      font-weight: 700;
      font-size: 0.95rem;
      margin-bottom: 0.4rem;
      letter-spacing: -0.01em;
    }

    .feature-desc {
      font-size: 0.8rem;
      color: var(--muted);
      line-height: 1.6;
    }

    /* ── INSTALL ── */
    .install-section {
      padding: 2rem 0;
      animation: fadeUp 0.8s ease 0.3s both;
    }

    .code-block {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 1rem 1.25rem;
      margin: 0.75rem 0;
      font-family: 'DM Mono', monospace;
      font-size: 0.82rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 1rem;
    }

    .code-block code { color: var(--accent2); }

    .copy-btn {
      background: none;
      border: 1px solid var(--border);
      color: var(--muted);
      font-family: 'DM Mono', monospace;
      font-size: 0.65rem;
      padding: 0.25rem 0.6rem;
      border-radius: 4px;
      cursor: pointer;
      transition: all 0.15s;
      white-space: nowrap;
      letter-spacing: 0.05em;
    }

    .copy-btn:hover { border-color: var(--accent); color: var(--accent); }

    .step-label {
      font-family: 'DM Mono', monospace;
      font-size: 0.7rem;
      color: var(--muted);
      margin-top: 1.25rem;
      margin-bottom: 0.3rem;
      letter-spacing: 0.08em;
    }

    /* ── HOW IT WORKS ── */
    .how-section {
      padding: 2rem 0;
      animation: fadeUp 0.8s ease 0.35s both;
    }

    .steps {
      display: flex;
      flex-direction: column;
      gap: 0;
      margin-top: 1.5rem;
      position: relative;
    }

    .steps::before {
      content: '';
      position: absolute;
      left: 20px;
      top: 20px;
      bottom: 20px;
      width: 1px;
      background: linear-gradient(to bottom, var(--accent), transparent);
    }

    .step {
      display: flex;
      gap: 1.5rem;
      padding: 1.25rem 0;
    }

    .step-num {
      flex-shrink: 0;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: var(--surface);
      border: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'DM Mono', monospace;
      font-size: 0.75rem;
      color: var(--accent);
      position: relative;
      z-index: 1;
    }

    .step-content h4 {
      font-weight: 700;
      font-size: 0.95rem;
      margin-bottom: 0.25rem;
    }

    .step-content p {
      font-size: 0.83rem;
      color: var(--muted);
      line-height: 1.6;
    }

    /* ── METRICS ── */
    .metrics {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1rem;
      margin: 2rem 0;
    }

    .metric {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 1.5rem;
      text-align: center;
    }

    .metric-val {
      font-size: 2rem;
      font-weight: 800;
      letter-spacing: -0.04em;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .metric-label {
      font-size: 0.75rem;
      color: var(--muted);
      margin-top: 0.25rem;
      font-family: 'DM Mono', monospace;
      letter-spacing: 0.05em;
    }

    /* ── FOOTER ── */
    footer {
      border-top: 1px solid var(--border);
      padding: 2.5rem 0;
      margin-top: 3rem;
    }

    .footer-inner {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 1rem;
    }

    .footer-copy {
      font-size: 0.8rem;
      color: var(--muted);
      font-family: 'DM Mono', monospace;
    }

    .footer-links {
      display: flex;
      gap: 1.5rem;
    }

    .footer-links a {
      color: var(--muted);
      text-decoration: none;
      font-size: 0.82rem;
      transition: color 0.2s;
    }

    .footer-links a:hover { color: var(--accent); }

    /* ── DIVIDER ── */
    hr {
      border: none;
      border-top: 1px solid var(--border);
      margin: 2rem 0;
    }

    /* ── PILL ── */
    .pill {
      display: inline-block;
      font-family: 'DM Mono', monospace;
      font-size: 0.65rem;
      letter-spacing: 0.08em;
      padding: 0.2rem 0.55rem;
      border-radius: 4px;
      margin-right: 0.4rem;
    }

    .pill-green { background: rgba(127,255,178,0.1); color: var(--accent); }
    .pill-blue  { background: rgba(56,249,215,0.1); color: var(--accent2); }
    .pill-yellow { background: rgba(255,228,77,0.1); color: var(--accent3); }
    .pill-purple { background: rgba(162,155,254,0.1); color: #a29bfe; }

    @media (max-width: 640px) {
      .what-grid, .features-grid, .metrics { grid-template-columns: 1fr; }
      h1 { font-size: 2.4rem; }
      .steps::before { display: none; }
    }
  </style>
</head>
<body>

  <div class="orb orb-1"></div>
  <div class="orb orb-2"></div>
  <div class="orb orb-3"></div>

  <!-- NAV -->
  <nav>
    <div class="nav-inner">
      <div class="logo">
        <div class="logo-icon">⬡</div>
        geotool.co
      </div>
      <div style="display:flex;align-items:center;gap:1.5rem;">
        <div class="nav-links">
          <a href="#what">What</a>
          <a href="#features">Features</a>
          <a href="#install">Install</a>
        </div>
        <span class="badge-nav">v1.0</span>
      </div>
    </div>
  </nav>

  <div class="container">

    <!-- HERO -->
    <section class="hero" id="top">
      <div class="hero-eyebrow">Generative Engine Optimization</div>
      <h1>Optimize for the<br /><span class="highlight">AI-first web</span></h1>
      <p class="hero-desc">
        GeoTool helps you audit, analyze, and optimize your content for AI-powered search engines — so your brand shows up when ChatGPT, Perplexity, and Google AI Overview answer your customers' questions.
      </p>
      <div class="hero-cta">
        <a href="https://geotool.co" class="btn-primary">
          ↗ Visit geotool.co
        </a>
        <a href="#install" class="btn-ghost">
          ⬇ Get started
        </a>
      </div>

      <div style="margin-top:2rem;">
        <span class="pill pill-green">GEO</span>
        <span class="pill pill-blue">AI Search</span>
        <span class="pill pill-yellow">ChatGPT</span>
        <span class="pill pill-purple">Perplexity</span>
        <span class="pill pill-green">Google AIO</span>
      </div>
    </section>

    <!-- TERMINAL -->
    <div class="terminal">
      <div class="terminal-bar">
        <div class="dot dot-r"></div>
        <div class="dot dot-y"></div>
        <div class="dot dot-g"></div>
        <span class="terminal-label">geotool.co — audit output</span>
      </div>
      <div class="terminal-body">
        <span class="t-line"><span class="t-prompt">$ </span><span class="t-cmd">geotool audit</span> <span class="t-comment">--url https://yoursite.com</span></span>
        <span class="t-line" style="height:0.5rem;display:block;"></span>
        <span class="t-line"><span class="t-key">✓ crawling</span>    <span class="t-val">47 pages indexed</span></span>
        <span class="t-line"><span class="t-key">✓ ai-score</span>    <span class="t-val">62 / 100</span>  <span class="t-comment">↑ improve entity coverage</span></span>
        <span class="t-line"><span class="t-key">✓ citations</span>   <span class="t-val">3 detected</span>  <span class="t-comment">in perplexity, chatgpt</span></span>
        <span class="t-line"><span class="t-key">✓ schema</span>      <span class="t-val">missing FAQ, HowTo markup</span></span>
        <span class="t-line"><span class="t-key">✓ e-e-a-t</span>     <span class="t-val">author bios incomplete</span></span>
        <span class="t-line" style="height:0.5rem;display:block;"></span>
        <span class="t-line"><span class="t-comment">→  report saved to </span><span class="t-url">./geo-report-2024.json</span></span>
      </div>
    </div>

    <!-- METRICS -->
    <div class="metrics">
      <div class="metric">
        <div class="metric-val">↑ 3×</div>
        <div class="metric-label">AI citation rate</div>
      </div>
      <div class="metric">
        <div class="metric-val">50+</div>
        <div class="metric-label">GEO signals tracked</div>
      </div>
      <div class="metric">
        <div class="metric-val">5</div>
        <div class="metric-label">AI engines audited</div>
      </div>
    </div>

    <!-- WHAT IS GEO -->
    <section class="what-section" id="what">
      <div class="section-label">what is geo</div>
      <p style="font-size:1rem;line-height:1.8;color:var(--muted);margin-bottom:1.5rem;">
        <strong style="color:var(--text);">Generative Engine Optimization (GEO)</strong> is the next evolution of SEO — optimizing your content to appear in AI-generated answers, not just blue-link search results.
      </p>

      <div class="what-grid">
        <div class="compare-card old">
          <div class="compare-title">— OLD: SEO</div>
          <ul class="compare-list">
            <li>Rank on page 1 of Google</li>
            <li>Optimize keywords & backlinks</li>
            <li>Chase algorithm updates</li>
            <li>Traffic via click-throughs</li>
          </ul>
        </div>
        <div class="compare-card new">
          <div class="compare-title">+ NEW: GEO</div>
          <ul class="compare-list">
            <li>Be cited in AI answers</li>
            <li>Build entity authority</li>
            <li>Optimize for LLM readability</li>
            <li>Visibility without the click</li>
          </ul>
        </div>
      </div>
    </section>

    <hr />

    <!-- FEATURES -->
    <section class="features" id="features">
      <div class="section-label">features</div>
      <div class="features-grid">
        <div class="feature-card">
          <div class="feature-icon">🔍</div>
          <div class="feature-title">AI Citation Tracker</div>
          <div class="feature-desc">Monitor where your brand appears in ChatGPT, Perplexity, Claude, and Google AIO responses.</div>
        </div>
        <div class="feature-card">
          <div class="feature-icon">📊</div>
          <div class="feature-title">GEO Score</div>
          <div class="feature-desc">A single score measuring your content's AI discoverability across all major generative engines.</div>
        </div>
        <div class="feature-card">
          <div class="feature-icon">🧠</div>
          <div class="feature-title">Entity Coverage</div>
          <div class="feature-desc">Identify topic gaps and entity relationships that LLMs use to understand and cite your brand.</div>
        </div>
        <div class="feature-card">
          <div class="feature-icon">⚡</div>
          <div class="feature-title">Schema Audit</div>
          <div class="feature-desc">Auto-detect missing structured data (FAQ, HowTo, Article) that boosts AI engine visibility.</div>
        </div>
        <div class="feature-card">
          <div class="feature-icon">✍️</div>
          <div class="feature-title">E-E-A-T Checker</div>
          <div class="feature-desc">Evaluate your content's experience, expertise, authoritativeness, and trustworthiness signals.</div>
        </div>
        <div class="feature-card">
          <div class="feature-icon">📝</div>
          <div class="feature-title">Content Optimizer</div>
          <div class="feature-desc">Rewrite suggestions to make your pages more quotable and citable for AI-generated responses.</div>
        </div>
      </div>
    </section>

    <hr />

    <!-- HOW IT WORKS -->
    <section class="how-section" id="how">
      <div class="section-label">how it works</div>
      <div class="steps">
        <div class="step">
          <div class="step-num">01</div>
          <div class="step-content">
            <h4>Connect your domain</h4>
            <p>Enter your URL — GeoTool crawls your site and maps all indexable pages and content assets.</p>
          </div>
        </div>
        <div class="step">
          <div class="step-num">02</div>
          <div class="step-content">
            <h4>AI engine audit</h4>
            <p>We query ChatGPT, Perplexity, Gemini, and Claude with prompts related to your industry to find where you appear (or don't).</p>
          </div>
        </div>
        <div class="step">
          <div class="step-num">03</div>
          <div class="step-content">
            <h4>Get your GEO score</h4>
            <p>Receive a 0–100 score with a breakdown of entity coverage, schema health, E-E-A-T signals, and citation frequency.</p>
          </div>
        </div>
        <div class="step">
          <div class="step-num">04</div>
          <div class="step-content">
            <h4>Optimize & monitor</h4>
            <p>Apply recommendations and track your citation rate over time as AI search engines discover and cite your improved content.</p>
          </div>
        </div>
      </div>
    </section>

    <hr />

    <!-- INSTALL / GET STARTED -->
    <section class="install-section" id="install">
      <div class="section-label">get started</div>

      <p style="font-size:0.9rem;color:var(--muted);line-height:1.7;margin-bottom:1.5rem;">
        GeoTool runs as a web app at <strong style="color:var(--accent);">geotool.co</strong> — no installation required. For CLI usage or API access:
      </p>

      <div class="step-label">// via npm</div>
      <div class="code-block">
        <code>npm install -g geotool-cli</code>
        <button class="copy-btn" onclick="navigator.clipboard.writeText('npm install -g geotool-cli');this.textContent='copied!';setTimeout(()=>this.textContent='copy',1500)">copy</button>
      </div>

      <div class="step-label">// run an audit</div>
      <div class="code-block">
        <code>geotool audit --url https://yourdomain.com</code>
        <button class="copy-btn" onclick="navigator.clipboard.writeText('geotool audit --url https://yourdomain.com');this.textContent='copied!';setTimeout(()=>this.textContent='copy',1500)">copy</button>
      </div>

      <div class="step-label">// export report</div>
      <div class="code-block">
        <code>geotool audit --url https://yourdomain.com --output report.json</code>
        <button class="copy-btn" onclick="navigator.clipboard.writeText('geotool audit --url https://yourdomain.com --output report.json');this.textContent='copied!';setTimeout(()=>this.textContent='copy',1500)">copy</button>
      </div>
    </section>

    <hr />

    <!-- CTA -->
    <section style="padding:2.5rem 0;text-align:center;animation:fadeUp 0.8s ease 0.4s both;">
      <div style="font-size:0.65rem;font-family:'DM Mono',monospace;letter-spacing:0.15em;color:var(--muted);margin-bottom:1rem;">READY?</div>
      <h2 style="font-size:2rem;font-weight:800;letter-spacing:-0.03em;margin-bottom:0.75rem;">
        Rank in the <span class="highlight">age of AI</span>
      </h2>
      <p style="color:var(--muted);font-size:0.9rem;margin-bottom:2rem;max-width:480px;margin-left:auto;margin-right:auto;line-height:1.7;">
        Your customers are asking AI questions. Make sure the answers include you.
      </p>
      <a href="https://geotool.co" class="btn-primary" style="font-size:1rem;padding:1rem 2rem;">
        ↗ Start free audit at geotool.co
      </a>
    </section>

  </div><!-- /container -->

  <!-- FOOTER -->
  <footer>
    <div class="container">
      <div class="footer-inner">
        <span class="footer-copy">© 2025 geotool.co — built for the AI-first web</span>
        <div class="footer-links">
          <a href="https://geotool.co">Website</a>
          <a href="https://geotool.co/docs">Docs</a>
          <a href="https://geotool.co/changelog">Changelog</a>
          <a href="https://twitter.com/geotoolco">Twitter/X</a>
        </div>
      </div>
    </div>
  </footer>

</body>
</html>
