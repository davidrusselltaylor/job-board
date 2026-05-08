<!DOCTYPE html>
<html lang="en-GB">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Job Board</title>
  <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>📋</text></svg>">
</head>
<body>
<div id="app"></div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600;9..144,700&family=Inter+Tight:wght@400;500;600;700&display=swap');

  * { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #f4f1ea;
    --bg-card: #ffffff;
    --ink: #1a1a1a;
    --ink-soft: #5a5a5a;
    --ink-faint: #8a8a8a;
    --line: #e0dcd2;
    --line-soft: #ebe7dd;
    --accent: #d94f30;
    --perm-bg: #ffffff;
    --perm-stripe: #d94f30;
    --contract-bg: #1a1a1a;
    --contract-ink: #f4f1ea;
    --contract-stripe: #f4d35e;
    --shadow: 0 1px 2px rgba(0,0,0,0.04), 0 4px 12px rgba(0,0,0,0.06);
    --shadow-lift: 0 8px 24px rgba(0,0,0,0.12), 0 2px 4px rgba(0,0,0,0.08);
  }

  body {
    background: var(--bg);
    color: var(--ink);
    font-family: 'Inter Tight', sans-serif;
    min-height: 100vh;
  }

  #app {
    padding: 32px 24px 48px;
    max-width: 100%;
  }

  .header {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    margin-bottom: 32px;
    flex-wrap: wrap;
    gap: 16px;
  }

  .title-block h1 {
    font-family: 'Fraunces', serif;
    font-weight: 600;
    font-size: 44px;
    line-height: 0.95;
    letter-spacing: -0.02em;
    color: var(--ink);
  }

  .title-block h1 em {
    font-style: italic;
    font-weight: 400;
    color: var(--accent);
  }

  .title-block .subtitle {
    font-size: 13px;
    color: var(--ink-soft);
    margin-top: 6px;
    letter-spacing: 0.02em;
  }

  .stats {
    display: flex;
    gap: 24px;
    align-items: flex-end;
  }

  .stat {
    text-align: right;
  }

  .stat .num {
    font-family: 'Fraunces', serif;
    font-size: 28px;
    font-weight: 500;
    line-height: 1;
    color: var(--ink);
  }

  .stat .label {
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--ink-faint);
    margin-top: 4px;
  }

  .add-btn {
    background: var(--ink);
    color: var(--bg);
    border: none;
    padding: 12px 20px;
    font-family: 'Inter Tight', sans-serif;
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 0.04em;
    text-transform: uppercase;
    cursor: pointer;
    border-radius: 2px;
    transition: all 0.2s ease;
    display: inline-flex;
    align-items: center;
    gap: 8px;
  }

  .add-btn:hover {
    background: var(--accent);
    transform: translateY(-1px);
  }

  .add-btn .plus {
    font-size: 18px;
    line-height: 0;
  }

  .icon-btn {
    background: transparent;
    color: var(--ink);
    border: 1px solid var(--line);
    width: 38px;
    height: 38px;
    cursor: pointer;
    border-radius: 2px;
    transition: all 0.2s ease;
    font-size: 14px;
    font-weight: 600;
  }

  .icon-btn:hover {
    border-color: var(--accent);
    color: var(--accent);
  }

  .board {
    display: grid;
    grid-template-columns: repeat(6, minmax(220px, 1fr));
    gap: 16px;
    overflow-x: auto;
    padding-bottom: 16px;
  }

  @media (max-width: 1200px) {
    .board { grid-template-columns: repeat(3, minmax(220px, 1fr)); }
  }

  @media (max-width: 700px) {
    .board { grid-template-columns: repeat(2, minmax(220px, 1fr)); }
  }

  .column {
    background: transparent;
    min-height: 400px;
    display: flex;
    flex-direction: column;
  }

  .column-header {
    padding: 0 4px 12px;
    border-bottom: 1px solid var(--line);
    margin-bottom: 12px;
    display: flex;
    align-items: baseline;
    justify-content: space-between;
  }

  .column-title {
    font-family: 'Fraunces', serif;
    font-size: 16px;
    font-weight: 500;
    color: var(--ink);
    letter-spacing: -0.01em;
  }

  .column-count {
    font-size: 11px;
    color: var(--ink-faint);
    font-variant-numeric: tabular-nums;
  }

  .column-drop {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 10px;
    min-height: 60px;
    padding: 4px;
    border-radius: 4px;
    transition: background 0.15s ease;
  }

  .column-drop.drag-over {
    background: rgba(217, 79, 48, 0.08);
    outline: 2px dashed var(--accent);
    outline-offset: -4px;
  }

  .card {
    background: var(--perm-bg);
    border: 1px solid var(--line);
    border-radius: 4px;
    padding: 14px 16px 14px 20px;
    cursor: grab;
    position: relative;
    box-shadow: var(--shadow);
    transition: all 0.15s ease;
  }

  .card::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 4px;
    background: var(--perm-stripe);
    border-radius: 4px 0 0 4px;
  }

  .card.contract {
    background: var(--contract-bg);
    color: var(--contract-ink);
    border-color: var(--contract-bg);
  }

  .card.contract::before {
    background: var(--contract-stripe);
  }

  .card:hover {
    box-shadow: var(--shadow-lift);
    transform: translateY(-2px);
  }

  .card:active {
    cursor: grabbing;
  }

  .card.dragging {
    opacity: 0.4;
  }

  .card-role {
    font-family: 'Fraunces', serif;
    font-size: 16px;
    font-weight: 500;
    line-height: 1.2;
    margin-bottom: 4px;
    letter-spacing: -0.01em;
  }

  .card-company {
    font-size: 13px;
    color: var(--ink-soft);
    margin-bottom: 10px;
  }

  .card.contract .card-company {
    color: rgba(244, 241, 234, 0.7);
  }

  .card-meta {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-bottom: 10px;
  }

  .meta-item {
    font-size: 11px;
    color: var(--ink-soft);
    background: rgba(0,0,0,0.04);
    padding: 3px 8px;
    border-radius: 2px;
    line-height: 1.4;
  }

  .meta-item.meta-salary {
    background: rgba(217, 79, 48, 0.1);
    color: var(--accent);
    font-weight: 500;
    font-variant-numeric: tabular-nums;
  }

  .card.contract .meta-item {
    background: rgba(244, 241, 234, 0.1);
    color: rgba(244, 241, 234, 0.85);
  }

  .card.contract .meta-item.meta-salary {
    background: rgba(244, 211, 94, 0.18);
    color: var(--contract-stripe);
  }

  .card-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 8px;
    margin-top: 8px;
    padding-top: 8px;
    border-top: 1px solid var(--line-soft);
  }

  .card.contract .card-footer {
    border-top-color: rgba(244, 241, 234, 0.15);
  }

  .card-tag {
    font-size: 9px;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-weight: 600;
    padding: 3px 7px;
    border-radius: 2px;
    background: rgba(217, 79, 48, 0.12);
    color: var(--accent);
  }

  .card.contract .card-tag {
    background: rgba(244, 211, 94, 0.18);
    color: var(--contract-stripe);
  }

  .card-actions {
    display: flex;
    gap: 6px;
  }

  .card-action {
    background: none;
    border: none;
    cursor: pointer;
    padding: 4px;
    color: var(--ink-faint);
    transition: color 0.15s ease;
    display: flex;
    align-items: center;
    font-size: 13px;
    text-decoration: none;
  }

  .card-action:hover {
    color: var(--accent);
  }

  .card.contract .card-action {
    color: rgba(244, 241, 234, 0.5);
  }

  .card.contract .card-action:hover {
    color: var(--contract-stripe);
  }

  .empty-col {
    color: var(--ink-faint);
    font-size: 12px;
    font-style: italic;
    padding: 20px 8px;
    text-align: center;
    opacity: 0.6;
  }

  /* Modal */
  .modal-backdrop {
    position: fixed;
    inset: 0;
    background: rgba(26, 26, 26, 0.5);
    backdrop-filter: blur(4px);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;
    animation: fadeIn 0.2s ease;
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  .modal {
    background: var(--bg-card);
    padding: 32px;
    border-radius: 4px;
    width: 90%;
    max-width: 480px;
    max-height: 90vh;
    overflow-y: auto;
    box-shadow: 0 24px 48px rgba(0,0,0,0.2);
    animation: slideUp 0.25s ease;
  }

  @keyframes slideUp {
    from { transform: translateY(20px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
  }

  .modal h2 {
    font-family: 'Fraunces', serif;
    font-weight: 500;
    font-size: 24px;
    margin-bottom: 24px;
    letter-spacing: -0.01em;
  }

  .field {
    margin-bottom: 18px;
  }

  .field label {
    display: block;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--ink-soft);
    margin-bottom: 6px;
    font-weight: 500;
  }

  .field input {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid var(--line);
    border-radius: 2px;
    font-family: 'Inter Tight', sans-serif;
    font-size: 14px;
    background: var(--bg);
    color: var(--ink);
    transition: border-color 0.15s ease;
  }

  .field input:focus {
    outline: none;
    border-color: var(--accent);
  }

  .field textarea {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid var(--line);
    border-radius: 2px;
    font-family: 'Inter Tight', sans-serif;
    font-size: 13px;
    background: var(--bg);
    color: var(--ink);
    transition: border-color 0.15s ease;
    resize: vertical;
    line-height: 1.5;
  }

  .field textarea:focus {
    outline: none;
    border-color: var(--accent);
  }

  .paste-field {
    background: rgba(217, 79, 48, 0.04);
    padding: 14px;
    border-radius: 4px;
    border: 1px dashed rgba(217, 79, 48, 0.3);
    margin-bottom: 24px;
  }

  .paste-hint {
    text-transform: none;
    letter-spacing: 0;
    font-weight: 400;
    color: var(--ink-faint);
    font-size: 11px;
  }

  .toggle-group {
    display: flex;
    gap: 8px;
  }

  .toggle-btn {
    flex: 1;
    padding: 10px 16px;
    border: 1px solid var(--line);
    background: var(--bg);
    cursor: pointer;
    font-family: 'Inter Tight', sans-serif;
    font-size: 13px;
    font-weight: 500;
    border-radius: 2px;
    transition: all 0.15s ease;
    color: var(--ink-soft);
  }

  .toggle-btn.active.perm {
    background: var(--ink);
    color: var(--bg);
    border-color: var(--ink);
  }

  .toggle-btn.active.contract {
    background: var(--contract-bg);
    color: var(--contract-ink);
    border-color: var(--contract-bg);
  }

  .toggle-btn.active-mode {
    background: var(--accent);
    color: white;
    border-color: var(--accent);
  }

  .modal-actions {
    display: flex;
    gap: 12px;
    margin-top: 28px;
    justify-content: flex-end;
  }

  .btn {
    padding: 10px 20px;
    font-family: 'Inter Tight', sans-serif;
    font-size: 13px;
    font-weight: 600;
    letter-spacing: 0.04em;
    text-transform: uppercase;
    cursor: pointer;
    border-radius: 2px;
    border: none;
    transition: all 0.15s ease;
  }

  .btn-secondary {
    background: transparent;
    color: var(--ink-soft);
  }

  .btn-secondary:hover {
    color: var(--ink);
  }

  .btn-primary {
    background: var(--accent);
    color: white;
  }

  .btn-primary:hover {
    background: var(--ink);
  }

  .btn-danger {
    background: transparent;
    color: var(--ink-faint);
    margin-right: auto;
  }

  .btn-danger:hover {
    color: var(--accent);
  }

  .loading-state {
    text-align: center;
    padding: 60px 20px;
    color: var(--ink-soft);
    font-style: italic;
  }
</style>

<script>
  const COLUMNS = [
    { id: 'applied', name: 'Applied' },
    { id: 'screening', name: 'Recruiter Screening' },
    { id: 'first', name: '1st Stage' },
    { id: 'second', name: '2nd Stage' },
    { id: 'offer', name: 'Offer' },
    { id: 'rejected', name: 'Not Selected' }
  ];

  let jobs = [];
  let editingId = null;
  let editingType = 'perm';
  let editingHybrid = 'hybrid';
  let draggedId = null;

  const STORAGE_KEY = 'dave_jobs_v1';

  async function loadJobs() {
    try {
      const stored = localStorage.getItem(STORAGE_KEY);
      if (stored) {
        jobs = JSON.parse(stored);
      }
    } catch (e) {
      jobs = [];
    }
    render();
  }

  async function saveJobs() {
    try {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(jobs));
    } catch (e) {
      console.error('Save failed', e);
    }
  }

  function uid() {
    return Date.now().toString(36) + Math.random().toString(36).slice(2, 7);
  }

  function openModal(jobId) {
    editingId = jobId;
    const job = jobId ? jobs.find(j => j.id === jobId) : null;
    editingType = job ? job.type : 'perm';
    editingHybrid = job ? (job.hybrid || 'hybrid') : 'hybrid';

    const modal = document.createElement('div');
    modal.className = 'modal-backdrop';
    modal.innerHTML = `
      <div class="modal" onclick="event.stopPropagation()">
        <h2>${job ? 'Edit role' : 'Add a role'}</h2>
        ${!job ? `
        <div class="field paste-field">
          <label>Paste job spec <span class="paste-hint">(optional, will auto-fill below)</span></label>
          <textarea id="f-paste" placeholder="Paste the full job spec text here and we'll try to extract salary, location and working pattern..." rows="4"></textarea>
        </div>
        ` : ''}
        <div class="field">
          <label>Role</label>
          <input type="text" id="f-role" placeholder="e.g. Marketing Manager CRM" value="${job ? escapeHtml(job.role) : ''}" autofocus>
        </div>
        <div class="field">
          <label>Company</label>
          <input type="text" id="f-company" placeholder="e.g. Unity Trust Bank" value="${job ? escapeHtml(job.company) : ''}">
        </div>
        <div class="field">
          <label>Job spec URL</label>
          <input type="url" id="f-url" placeholder="https://..." value="${job ? escapeHtml(job.url || '') : ''}">
        </div>
        <div class="field">
          <label>Salary / Day rate</label>
          <input type="text" id="f-salary" placeholder="e.g. £55-65k or £450/day" value="${job ? escapeHtml(job.salary || '') : ''}">
        </div>
        <div class="field">
          <label>Location</label>
          <input type="text" id="f-location" placeholder="e.g. Birmingham" value="${job ? escapeHtml(job.location || '') : ''}">
        </div>
        <div class="field">
          <label>Working pattern</label>
          <div class="toggle-group" id="hybrid-toggle">
            <button type="button" class="toggle-btn hybrid-btn ${editingHybrid === 'remote' ? 'active-mode' : ''}" data-hybrid="remote">Remote</button>
            <button type="button" class="toggle-btn hybrid-btn ${editingHybrid === 'hybrid' ? 'active-mode' : ''}" data-hybrid="hybrid">Hybrid</button>
            <button type="button" class="toggle-btn hybrid-btn ${editingHybrid === 'onsite' ? 'active-mode' : ''}" data-hybrid="onsite">Onsite</button>
          </div>
          <input type="text" id="f-hybrid-note" placeholder="e.g. 2 days office, 3 WFH" value="${job ? escapeHtml(job.hybridNote || '') : ''}" style="margin-top: 8px;">
        </div>
        <div class="field">
          <label>Type</label>
          <div class="toggle-group">
            <button type="button" class="toggle-btn perm ${editingType === 'perm' ? 'active' : ''}" data-type="perm">Permanent</button>
            <button type="button" class="toggle-btn contract ${editingType === 'contract' ? 'active' : ''}" data-type="contract">Contract</button>
          </div>
        </div>
        <div class="modal-actions">
          ${job ? '<button class="btn btn-danger" id="btn-delete">Delete</button>' : ''}
          <button class="btn btn-secondary" id="btn-cancel">Cancel</button>
          <button class="btn btn-primary" id="btn-save">${job ? 'Save' : 'Add role'}</button>
        </div>
      </div>
    `;

    modal.addEventListener('click', closeModal);
    document.body.appendChild(modal);

    modal.querySelectorAll('.toggle-btn:not(.hybrid-btn)').forEach(btn => {
      btn.addEventListener('click', (e) => {
        if (!e.target.dataset.type) return;
        editingType = e.target.dataset.type;
        modal.querySelectorAll('.toggle-btn:not(.hybrid-btn)').forEach(b => b.classList.remove('active'));
        e.target.classList.add('active');
      });
    });

    modal.querySelectorAll('.hybrid-btn').forEach(btn => {
      btn.addEventListener('click', (e) => {
        editingHybrid = e.target.dataset.hybrid;
        modal.querySelectorAll('.hybrid-btn').forEach(b => b.classList.remove('active-mode'));
        e.target.classList.add('active-mode');
      });
    });

    document.getElementById('btn-cancel').addEventListener('click', closeModal);
    document.getElementById('btn-save').addEventListener('click', saveJob);
    if (job) document.getElementById('btn-delete').addEventListener('click', deleteJob);

    const pasteEl = document.getElementById('f-paste');
    if (pasteEl) {
      pasteEl.addEventListener('paste', (e) => {
        setTimeout(() => parseAndFill(pasteEl.value), 50);
      });
      pasteEl.addEventListener('input', () => {
        if (pasteEl.value.length > 50) parseAndFill(pasteEl.value);
      });
    }

    document.getElementById('f-role').focus();
    document.getElementById('f-role').addEventListener('keydown', (e) => {
      if (e.key === 'Enter') saveJob();
    });
  }

  function closeModal() {
    const m = document.querySelector('.modal-backdrop');
    if (m) m.remove();
    editingId = null;
  }

  async function saveJob() {
    const role = document.getElementById('f-role').value.trim();
    const company = document.getElementById('f-company').value.trim();
    const url = document.getElementById('f-url').value.trim();
    const salary = document.getElementById('f-salary').value.trim();
    const location = document.getElementById('f-location').value.trim();
    const hybridNote = document.getElementById('f-hybrid-note').value.trim();

    if (!role || !company) {
      alert('Role and Company are required');
      return;
    }

    if (editingId) {
      const job = jobs.find(j => j.id === editingId);
      if (job) {
        job.role = role;
        job.company = company;
        job.url = url;
        job.salary = salary;
        job.location = location;
        job.hybrid = editingHybrid;
        job.hybridNote = hybridNote;
        job.type = editingType;
      }
    } else {
      jobs.push({
        id: uid(),
        role, company, url, salary, location,
        hybrid: editingHybrid,
        hybridNote,
        type: editingType,
        column: 'applied',
        created: Date.now()
      });
    }

    await saveJobs();
    closeModal();
    render();
  }

  async function deleteJob() {
    if (!editingId) return;
    if (!confirm('Delete this role?')) return;
    jobs = jobs.filter(j => j.id !== editingId);
    await saveJobs();
    closeModal();
    render();
  }

  function parseAndFill(text) {
    if (!text || text.length < 20) return;

    const setIfEmpty = (id, value) => {
      const el = document.getElementById(id);
      if (el && !el.value.trim() && value) el.value = value;
    };

    // SALARY: looks for £ amounts in various formats
    let salary = '';
    // Match ranges first: £45,000 - £65,000 or £45k-£65k or £45-65k
    const rangeMatch = text.match(/£\s?(\d{1,3}(?:,\d{3})*(?:\.\d+)?k?)\s*[-–to]+\s*£?\s?(\d{1,3}(?:,\d{3})*(?:\.\d+)?k?)/i);
    if (rangeMatch) {
      salary = `£${rangeMatch[1]}-${rangeMatch[2]}`.replace(/\s/g, '');
    } else {
      // Day rate: £450 per day, £500/day, £450 day rate
      const dayMatch = text.match(/£\s?(\d{2,4}(?:\.\d+)?)\s*(?:per\s*day|\/\s*day|day\s*rate|p\.?d\.?|a\s*day)/i);
      if (dayMatch) {
        salary = `£${dayMatch[1]}/day`;
      } else {
        // Single salary: £55,000 or £55k
        const singleMatch = text.match(/£\s?(\d{1,3}(?:,\d{3})*(?:\.\d+)?k?)\b/i);
        if (singleMatch) salary = `£${singleMatch[1]}`;
      }
    }
    setIfEmpty('f-salary', salary);

    // LOCATION: look for common UK cities
    const ukCities = ['London', 'Birmingham', 'Manchester', 'Leeds', 'Bristol', 'Liverpool', 'Edinburgh', 'Glasgow', 'Cardiff', 'Belfast', 'Newcastle', 'Sheffield', 'Nottingham', 'Reading', 'Brighton', 'Cambridge', 'Oxford', 'Swindon', 'Bath', 'Coventry', 'Leicester', 'Southampton', 'Portsmouth', 'York', 'Milton Keynes', 'Watford', 'Slough', 'Luton', 'Norwich', 'Aberdeen'];
    const locationMatch = ukCities.find(city => new RegExp(`\\b${city}\\b`, 'i').test(text));
    setIfEmpty('f-location', locationMatch || '');

    // HYBRID: detect pattern
    const lower = text.toLowerCase();
    let hybridMode = null;
    if (/\bfully\s+remote\b|\b100%\s+remote\b|\bremote[- ]first\b|\bremote\s+working\b|\bremote\s+role\b/.test(lower)) {
      hybridMode = 'remote';
    } else if (/\bhybrid\b/.test(lower)) {
      hybridMode = 'hybrid';
    } else if (/\bon[- ]site\b|\bin[- ]office\b|\boffice[- ]based\b|\bfull[- ]time\s+in\s+office\b/.test(lower)) {
      hybridMode = 'onsite';
    } else if (/\bremote\b/.test(lower)) {
      hybridMode = 'remote';
    }

    if (hybridMode) {
      editingHybrid = hybridMode;
      document.querySelectorAll('.hybrid-btn').forEach(b => {
        b.classList.toggle('active-mode', b.dataset.hybrid === hybridMode);
      });
    }

    // HYBRID NOTE: try to pick up day counts like "2 days in office", "3 days a week"
    const dayCountMatch = text.match(/(\d)\s*days?\s*(?:per\s*week|a\s*week|in\s*(?:the\s*)?office|on[- ]site|wfh|from\s*home|working\s*from\s*home)/i);
    if (dayCountMatch) {
      setIfEmpty('f-hybrid-note', dayCountMatch[0].trim());
    }

    // ROLE: look for "Job Title:" or first heading-like line containing common role words
    const roleKeywords = /(manager|specialist|executive|coordinator|director|lead|head\s+of|analyst|consultant|developer|engineer|officer|associate)/i;
    const lines = text.split(/\n/).map(l => l.trim()).filter(Boolean);
    let foundRole = '';
    // Check first 5 non-empty lines for something that looks like a job title
    for (const line of lines.slice(0, 5)) {
      if (line.length < 80 && roleKeywords.test(line) && !/[.!?]$/.test(line)) {
        foundRole = line.replace(/^(job\s*title|role|position)\s*[:\-]\s*/i, '').trim();
        break;
      }
    }
    setIfEmpty('f-role', foundRole);
  }

  function escapeHtml(s) {
    return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
  }

  function render() {
    const total = jobs.length;
    const active = jobs.filter(j => j.column !== 'rejected').length;
    const offers = jobs.filter(j => j.column === 'offer').length;

    const html = `
      <header class="header">
        <div class="title-block">
          <h1>The <em>job</em> board</h1>
          <div class="subtitle">Drag cards between columns. Click a card to edit.</div>
        </div>
        <div class="stats">
          <div class="stat"><div class="num">${active}</div><div class="label">Active</div></div>
          <div class="stat"><div class="num">${offers}</div><div class="label">Offers</div></div>
          <div class="stat"><div class="num">${total}</div><div class="label">Total</div></div>
          <button class="add-btn" id="add-btn"><span class="plus">+</span> Add role</button>
          <button class="icon-btn" id="export-btn" title="Export backup">↓</button>
          <button class="icon-btn" id="import-btn" title="Import backup">↑</button>
          <input type="file" id="import-input" accept=".json" style="display:none">
        </div>
      </header>
      <div class="board">
        ${COLUMNS.map(col => `
          <div class="column">
            <div class="column-header">
              <div class="column-title">${col.name}</div>
              <div class="column-count">${jobs.filter(j => j.column === col.id).length}</div>
            </div>
            <div class="column-drop" data-col="${col.id}">
              ${jobs.filter(j => j.column === col.id).map(renderCard).join('') || '<div class="empty-col">Drop roles here</div>'}
            </div>
          </div>
        `).join('')}
      </div>
    `;

    document.getElementById('app').innerHTML = html;
    bindEvents();
  }

  function renderCard(job) {
    const hybridLabel = {
      remote: 'Remote',
      hybrid: 'Hybrid',
      onsite: 'Onsite'
    }[job.hybrid] || '';

    const metaItems = [];
    if (job.salary) metaItems.push(`<span class="meta-item meta-salary">${escapeHtml(job.salary)}</span>`);
    if (job.location) metaItems.push(`<span class="meta-item">${escapeHtml(job.location)}</span>`);
    if (hybridLabel) {
      const note = job.hybridNote ? ` · ${escapeHtml(job.hybridNote)}` : '';
      metaItems.push(`<span class="meta-item meta-hybrid">${hybridLabel}${note}</span>`);
    }

    return `
      <div class="card ${job.type === 'contract' ? 'contract' : ''}" draggable="true" data-id="${job.id}">
        <div class="card-role">${escapeHtml(job.role)}</div>
        <div class="card-company">${escapeHtml(job.company)}</div>
        ${metaItems.length ? `<div class="card-meta">${metaItems.join('')}</div>` : ''}
        <div class="card-footer">
          <span class="card-tag">${job.type === 'contract' ? 'Contract' : 'Permanent'}</span>
          <div class="card-actions">
            ${job.url ? `<a class="card-action" href="${escapeHtml(job.url)}" target="_blank" rel="noopener" onclick="event.stopPropagation()" title="Open job spec">↗</a>` : ''}
            <button class="card-action" data-edit="${job.id}" title="Edit">✎</button>
          </div>
        </div>
      </div>
    `;
  }

  function bindEvents() {
    document.getElementById('add-btn').addEventListener('click', () => openModal(null));

    document.getElementById('export-btn').addEventListener('click', () => {
      const data = JSON.stringify(jobs, null, 2);
      const blob = new Blob([data], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      const date = new Date().toISOString().slice(0, 10);
      a.href = url;
      a.download = `job-board-backup-${date}.json`;
      a.click();
      URL.revokeObjectURL(url);
    });

    document.getElementById('import-btn').addEventListener('click', () => {
      document.getElementById('import-input').click();
    });

    document.getElementById('import-input').addEventListener('change', (e) => {
      const file = e.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = async (ev) => {
        try {
          const imported = JSON.parse(ev.target.result);
          if (!Array.isArray(imported)) throw new Error('Invalid file');
          if (!confirm(`Import ${imported.length} roles? This will replace your current board.`)) return;
          jobs = imported;
          await saveJobs();
          render();
        } catch (err) {
          alert('Could not read that file. Make sure it\'s a backup JSON file from this tracker.');
        }
      };
      reader.readAsText(file);
      e.target.value = '';
    });

    document.querySelectorAll('.card').forEach(card => {
      card.addEventListener('dragstart', (e) => {
        draggedId = card.dataset.id;
        card.classList.add('dragging');
        e.dataTransfer.effectAllowed = 'move';
      });
      card.addEventListener('dragend', () => {
        card.classList.remove('dragging');
        draggedId = null;
        document.querySelectorAll('.column-drop').forEach(c => c.classList.remove('drag-over'));
      });
      card.addEventListener('click', (e) => {
        if (e.target.closest('.card-action')) return;
        openModal(card.dataset.id);
      });
    });

    document.querySelectorAll('[data-edit]').forEach(btn => {
      btn.addEventListener('click', (e) => {
        e.stopPropagation();
        openModal(btn.dataset.edit);
      });
    });

    document.querySelectorAll('.column-drop').forEach(col => {
      col.addEventListener('dragover', (e) => {
        e.preventDefault();
        col.classList.add('drag-over');
      });
      col.addEventListener('dragleave', () => col.classList.remove('drag-over'));
      col.addEventListener('drop', async (e) => {
        e.preventDefault();
        col.classList.remove('drag-over');
        if (!draggedId) return;
        const job = jobs.find(j => j.id === draggedId);
        if (job) {
          job.column = col.dataset.col;
          await saveJobs();
          render();
        }
      });
    });
  }

  document.getElementById('app').innerHTML = '<div class="loading-state">Loading your job board...</div>';
  loadJobs();
</script>
</body>
</html>
[index.html](https://github.com/user-attachments/files/27517121/index.html)
