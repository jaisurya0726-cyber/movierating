
<h2 class="sr-only">Movie Rating Predictor </h2>

<style>
  :root { --accent: #e8c547; }
  * { box-sizing: border-box; margin: 0; padding: 0; }

  .app { padding: 1.5rem 0; font-family: var(--font-sans); }

  .hero { display: flex; align-items: center; gap: 14px; margin-bottom: 1.75rem; padding-bottom: 1.25rem; border-bottom: 0.5px solid var(--color-border-tertiary); }
  .hero-icon { width: 48px; height: 48px; border-radius: 12px; background: #1a1a2e; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
  .hero-icon i { font-size: 24px; color: #e8c547; }
  .hero-title { font-size: 20px; font-weight: 500; color: var(--color-text-primary); }
  .hero-sub { font-size: 13px; color: var(--color-text-secondary); margin-top: 2px; }

  .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px; }
  .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; margin-bottom: 12px; }
  .field { display: flex; flex-direction: column; gap: 5px; }
  .field label { font-size: 12px; font-weight: 500; color: var(--color-text-secondary); letter-spacing: 0.03em; }
  select, input[type=number] { width: 100%; }

  .slider-row { display: flex; align-items: center; gap: 10px; }
  .slider-row input[type=range] { flex: 1; }
  .slider-val { font-size: 13px; font-weight: 500; color: var(--color-text-primary); min-width: 52px; text-align: right; }

  .predict-btn { width: 100%; padding: 11px; margin-top: 8px; border-radius: var(--border-radius-md); background: #1a1a2e; border: 1.5px solid #e8c547; color: #e8c547; font-size: 15px; font-weight: 500; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; transition: background 0.15s, transform 0.1s; }
  .predict-btn:hover { background: #252540; }
  .predict-btn:active { transform: scale(0.98); }
  .predict-btn i { font-size: 18px; }

  .result-card { margin-top: 1.25rem; padding: 1.25rem; border-radius: var(--border-radius-lg); border: 0.5px solid var(--color-border-tertiary); background: var(--color-background-secondary); display: none; }
  .result-card.visible { display: block; }

  .result-top { display: flex; align-items: center; gap: 16px; margin-bottom: 1rem; }
  .star-ring { position: relative; width: 80px; height: 80px; flex-shrink: 0; }
  .star-ring svg { width: 80px; height: 80px; transform: rotate(-90deg); }
  .star-ring .ring-val { position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; font-size: 20px; font-weight: 500; color: var(--color-text-primary); }
  .result-meta { flex: 1; }
  .result-label { font-size: 12px; color: var(--color-text-secondary); margin-bottom: 4px; }
  .result-verdict { font-size: 18px; font-weight: 500; color: var(--color-text-primary); }
  .result-desc { font-size: 13px; color: var(--color-text-secondary); margin-top: 4px; }

  .factors { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
  .factor { padding: 10px 12px; border-radius: var(--border-radius-md); background: var(--color-background-primary); border: 0.5px solid var(--color-border-tertiary); }
  .factor-name { font-size: 11px; color: var(--color-text-secondary); margin-bottom: 4px; }
  .factor-bar-wrap { height: 4px; background: var(--color-border-tertiary); border-radius: 2px; margin-bottom: 5px; }
  .factor-bar { height: 4px; border-radius: 2px; background: #e8c547; transition: width 0.6s ease; }
  .factor-val { font-size: 12px; font-weight: 500; color: var(--color-text-primary); }

  .history-section { margin-top: 1.5rem; padding-top: 1rem; border-top: 0.5px solid var(--color-border-tertiary); }
  .history-title { font-size: 13px; font-weight: 500; color: var(--color-text-secondary); margin-bottom: 8px; }
  .history-list { display: flex; flex-direction: column; gap: 6px; }
  .history-item { display: flex; align-items: center; justify-content: space-between; padding: 8px 10px; border-radius: var(--border-radius-md); background: var(--color-background-secondary); font-size: 13px; }
  .history-item-left { color: var(--color-text-primary); }
  .history-item-right { font-weight: 500; }

  .loading-dots { display: inline-flex; gap: 4px; }
  .loading-dots span { width: 6px; height: 6px; border-radius: 50%; background: #e8c547; animation: dot-bounce 1s ease-in-out infinite; }
  .loading-dots span:nth-child(2) { animation-delay: 0.15s; }
  .loading-dots span:nth-child(3) { animation-delay: 0.3s; }
  @keyframes dot-bounce { 0%, 80%, 100% { transform: scale(0.6); opacity: 0.4; } 40% { transform: scale(1); opacity: 1; } }

  @media (max-width: 500px) { .grid { grid-template-columns: 1fr; } .grid-3 { grid-template-columns: 1fr; } .factors { grid-template-columns: 1fr; } }
</style>

<div class="app">
  <div class="hero">
    <div class="hero-icon"><i class="ti ti-movie" aria-hidden="true"></i></div>
    <div>
      <div class="hero-title">Movie Rating Predictor</div>
      <div class="hero-sub">IMDB India — AI-powered rating estimation</div>
    </div>
  </div>

  <div class="grid">
    <div class="field">
      <label for="genre">Genre</label>
      <select id="genre">
        <option>Action</option><option>Drama</option><option selected>Comedy</option>
        <option>Thriller</option><option>Romance</option><option>Horror</option>
        <option>Biography</option><option>Crime</option><option>Adventure</option><option>Sci-Fi</option>
      </select>
    </div>
    <div class="field">
      <label for="director">Director</label>
      <select id="director">
        <option>Rajkumar Hirani</option><option selected>SS Rajamouli</option>
        <option>Zoya Akhtar</option><option>Anurag Kashyap</option>
        <option>Sanjay Leela Bhansali</option><option>Imtiaz Ali</option>
        <option>Kabir Khan</option><option>Farah Khan</option>
        <option>Vishal Bhardwaj</option><option>Shoojit Sircar</option>
      </select>
    </div>
  </div>

  <div class="grid-3">
    <div class="field">
      <label for="actor1">Lead Actor</label>
      <select id="actor1">
        <option>Amitabh Bachchan</option><option selected>Shah Rukh Khan</option>
        <option>Aamir Khan</option><option>Salman Khan</option>
        <option>Deepika Padukone</option><option>Priyanka Chopra</option>
        <option>Ranveer Singh</option><option>Ranbir Kapoor</option>
        <option>Akshay Kumar</option><option>Hrithik Roshan</option>
        <option>Kangana Ranaut</option><option>Katrina Kaif</option>
        <option>Ayushmann Khurrana</option><option>Rajkummar Rao</option>
        <option>Vicky Kaushal</option>
      </select>
    </div>
    <div class="field">
      <label for="actor2">Supporting Actor</label>
      <select id="actor2">
        <option>Amitabh Bachchan</option><option>Shah Rukh Khan</option>
        <option>Aamir Khan</option><option>Salman Khan</option>
        <option>Deepika Padukone</option><option selected>Priyanka Chopra</option>
        <option>Ranveer Singh</option><option>Ranbir Kapoor</option>
        <option>Akshay Kumar</option><option>Hrithik Roshan</option>
        <option>Kangana Ranaut</option><option>Katrina Kaif</option>
        <option>Ayushmann Khurrana</option><option>Rajkummar Rao</option>
        <option>Vicky Kaushal</option>
      </select>
    </div>
    <div class="field">
      <label for="year">Release year</label>
      <input type="number" id="year" value="2024" min="1990" max="2025" />
    </div>
  </div>

  <div class="grid">
    <div class="field">
      <label>Duration (min) — <span id="dur-out">145</span></label>
      <div class="slider-row">
        <input type="range" id="duration" min="60" max="220" step="5" value="145" oninput="document.getElementById('dur-out').textContent=this.value" />
        <span class="slider-val" aria-hidden="true"><i class="ti ti-clock" style="font-size:14px"></i></span>
      </div>
    </div>
    <div class="field">
      <label>Vote count — <span id="votes-out">150K</span></label>
      <div class="slider-row">
        <input type="range" id="votes" min="500" max="500000" step="500" value="150000"
          oninput="document.getElementById('votes-out').textContent=formatVotes(+this.value)" />
        <span class="slider-val" aria-hidden="true"><i class="ti ti-users" style="font-size:14px"></i></span>
      </div>
    </div>
  </div>

  <button class="predict-btn" id="predictBtn" onclick="runPrediction()">
    <i class="ti ti-sparkles" aria-hidden="true"></i> Predict Rating
  </button>

  <div class="result-card" id="resultCard" role="region" aria-live="polite" aria-label="Prediction result">
    <div class="result-top">
      <div class="star-ring">
        <svg viewBox="0 0 80 80" aria-hidden="true">
          <circle cx="40" cy="40" r="34" fill="none" stroke="var(--color-border-tertiary)" stroke-width="6"/>
          <circle cx="40" cy="40" r="34" fill="none" stroke="#e8c547" stroke-width="6"
            stroke-dasharray="213.6" id="ratingArc" stroke-dashoffset="213.6" stroke-linecap="round"/>
        </svg>
        <div class="ring-val" id="ratingNum">—</div>
      </div>
      <div class="result-meta">
        <div class="result-label">Predicted IMDB Rating</div>
        <div class="result-verdict" id="verdict">—</div>
        <div class="result-desc" id="resultDesc">—</div>
      </div>
    </div>
    <div class="factors" id="factorGrid"></div>
  </div>

  <div class="history-section" id="historySection" style="display:none">
    <div class="history-title"><i class="ti ti-history" style="font-size:13px; vertical-align:-1px" aria-hidden="true"></i> Recent predictions</div>
    <div class="history-list" id="historyList"></div>
  </div>
</div>

<script>
const DIRECTOR_PRESTIGE = {
  'Rajkumar Hirani': 0.97, 'SS Rajamouli': 0.95, 'Zoya Akhtar': 0.88,
  'Anurag Kashyap': 0.82, 'Sanjay Leela Bhansali': 0.85, 'Imtiaz Ali': 0.78,
  'Kabir Khan': 0.74, 'Farah Khan': 0.70, 'Vishal Bhardwaj': 0.86, 'Shoojit Sircar': 0.90
};
const ACTOR_PRESTIGE = {
  'Amitabh Bachchan': 0.96, 'Shah Rukh Khan': 0.94, 'Aamir Khan': 0.97, 'Salman Khan': 0.88,
  'Deepika Padukone': 0.87, 'Priyanka Chopra': 0.85, 'Ranveer Singh': 0.84, 'Ranbir Kapoor': 0.86,
  'Akshay Kumar': 0.80, 'Hrithik Roshan': 0.89, 'Kangana Ranaut': 0.75, 'Katrina Kaif': 0.72,
  'Ayushmann Khurrana': 0.91, 'Rajkummar Rao': 0.89, 'Vicky Kaushal': 0.88
};
const GENRE_BONUS = {
  Drama: 0.4, Biography: 0.45, Crime: 0.35, Thriller: 0.25,
  Comedy: 0.1, Romance: 0.1, Adventure: 0.05, Action: 0.0, Horror: -0.25, 'Sci-Fi': -0.2
};
const VERDICTS = [
  [1,4,'Unwatchable','Likely to disappoint even casual viewers.'],
  [4,5.5,'Below average','Struggles to find an audience.'],
  [5.5,6.5,'Average','A decent watch for fans of the genre.'],
  [6.5,7.2,'Good','Enjoyable with some memorable moments.'],
  [7.2,8,'Very good','A crowd-pleaser worth your time.'],
  [8,8.8,'Excellent','A standout film — highly recommended.'],
  [8.8,10,'Masterpiece','A rare cinematic achievement.']
];

const history = [];

function formatVotes(v) {
  if (v >= 1000000) return (v/1000000).toFixed(1)+'M';
  if (v >= 1000) return Math.round(v/1000)+'K';
  return v;
}

function computeRating() {
  const dir    = document.getElementById('director').value;
  const a1     = document.getElementById('actor1').value;
  const a2     = document.getElementById('actor2').value;
  const genre  = document.getElementById('genre').value;
  const year   = +document.getElementById('year').value;
  const dur    = +document.getElementById('duration').value;
  const votes  = +document.getElementById('votes').value;

  const dp  = DIRECTOR_PRESTIGE[dir] || 0.75;
  const ap1 = ACTOR_PRESTIGE[a1]    || 0.75;
  const ap2 = ACTOR_PRESTIGE[a2]    || 0.70;
  const gb  = GENRE_BONUS[genre]    || 0;
  const logV = Math.log1p(votes) / Math.log1p(500000);
  const ageBonus = Math.max(0, (year - 1990)) * 0.005;
  const durScore = dur >= 120 && dur <= 170 ? 0.1 : (dur > 170 ? 0.05 : 0);

  let r = 4.5 + dp*2.4 + ap1*1.4 + ap2*0.7 + gb + logV*0.5 + ageBonus + durScore;
  r += (Math.random()-0.5)*0.2;
  return Math.max(1, Math.min(10, +r.toFixed(1)));
}

function getFactors(dir, a1, a2, genre, votes) {
  const dp  = DIRECTOR_PRESTIGE[dir]  || 0.75;
  const ap1 = ACTOR_PRESTIGE[a1]      || 0.75;
  const ap2 = ACTOR_PRESTIGE[a2]      || 0.70;
  const gRaw = (GENRE_BONUS[genre] + 0.45) / 0.9;
  const vRaw = Math.log1p(votes) / Math.log1p(500000);
  return [
    { name:'Director impact',   pct: Math.round(dp*100)  },
    { name:'Lead actor',        pct: Math.round(ap1*100) },
    { name:'Supporting actor',  pct: Math.round(ap2*100) },
    { name:'Genre appeal',      pct: Math.round(Math.max(0,Math.min(1,gRaw))*100) },
    { name:'Audience reach',    pct: Math.round(vRaw*100) },
    { name:'Production era',    pct: Math.round(Math.min(100, (+document.getElementById('year').value-1990)/34*100)) }
  ];
}

function runPrediction() {
  const btn = document.getElementById('predictBtn');
  btn.disabled = true;
  btn.innerHTML = '<div class="loading-dots"><span></span><span></span><span></span></div>';

  setTimeout(() => {
    const rating = computeRating();
    const dir   = document.getElementById('director').value;
    const a1    = document.getElementById('actor1').value;
    const a2    = document.getElementById('actor2').value;
    const genre = document.getElementById('genre').value;
    const votes = +document.getElementById('votes').value;

    const verdict = VERDICTS.find(([lo,hi]) => rating>=lo && rating<hi) || VERDICTS[VERDICTS.length-1];
    const ratingColor = rating >= 8 ? '#4ade80' : rating >= 6.5 ? '#e8c547' : rating >= 5 ? '#fb923c' : '#f87171';

    document.getElementById('ratingNum').textContent = rating.toFixed(1);
    document.getElementById('verdict').textContent   = verdict[2];
    document.getElementById('resultDesc').textContent= verdict[3];

    const arc = document.getElementById('ratingArc');
    const circ = 213.6;
    arc.style.stroke = ratingColor;
    arc.style.strokeDashoffset = (circ - circ*(rating/10)).toFixed(1);

    const factors = getFactors(dir, a1, a2, genre, votes);
    document.getElementById('factorGrid').innerHTML = factors.map(f => `
      <div class="factor">
        <div class="factor-name">${f.name}</div>
        <div class="factor-bar-wrap"><div class="factor-bar" style="width:0%" data-pct="${f.pct}"></div></div>
        <div class="factor-val">${f.pct}%</div>
      </div>`).join('');

    setTimeout(() => {
      document.querySelectorAll('.factor-bar').forEach(b => {
        b.style.width = b.dataset.pct + '%';
      });
    }, 50);

    document.getElementById('resultCard').className = 'result-card visible';

    history.unshift({ label: `${genre} · ${dir.split(' ').pop()}`, rating });
    if (history.length > 5) history.pop();
    renderHistory();

    btn.disabled = false;
    btn.innerHTML = '<i class="ti ti-refresh" aria-hidden="true"></i> Predict again';
  }, 900);
}

function renderHistory() {
  const sec = document.getElementById('historySection');
  const list = document.getElementById('historyList');
  if (!history.length) { sec.style.display='none'; return; }
  sec.style.display = 'block';
  const colors = ['#4ade80','#4ade80','#e8c547','#fb923c','#f87171'];
  list.innerHTML = history.map((h,i) => {
    const col = h.rating >= 8 ? '#4ade80' : h.rating >= 6.5 ? '#e8c547' : h.rating >= 5 ? '#fb923c' : '#f87171';
    return `<div class="history-item">
      <span class="history-item-left">${h.label}</span>
      <span class="history-item-right" style="color:${col}">${h.rating.toFixed(1)} ★</span>
    </div>`;
  }).join('');
}
</script>
