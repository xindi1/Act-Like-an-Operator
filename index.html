\
const STORAGE_KEY = 'act-like-an-operator-v1';

const defaultState = {
  missions: [],
  sessions: [],
  activeMissionId: '',
  timer: {
    isRunning: false,
    startTime: null,
    elapsedMs: 0
  }
};

let state = loadState();
let timerInterval = null;

const els = {
  pills: [...document.querySelectorAll('.pill')],
  panels: [...document.querySelectorAll('.panel')],
  missionTitle: document.getElementById('missionTitle'),
  missionSource: document.getElementById('missionSource'),
  missionHorizon: document.getElementById('missionHorizon'),
  missionPriority: document.getElementById('missionPriority'),
  missionWhy: document.getElementById('missionWhy'),
  missionDone: document.getElementById('missionDone'),
  missionFirstStep: document.getElementById('missionFirstStep'),
  saveMissionBtn: document.getElementById('saveMissionBtn'),
  clearMissionBtn: document.getElementById('clearMissionBtn'),
  activeMissionSelect: document.getElementById('activeMissionSelect'),
  blockMode: document.getElementById('blockMode'),
  timerDisplay: document.getElementById('timerDisplay'),
  currentMissionDisplay: document.getElementById('currentMissionDisplay'),
  startBlockBtn: document.getElementById('startBlockBtn'),
  pauseBlockBtn: document.getElementById('pauseBlockBtn'),
  stopBlockBtn: document.getElementById('stopBlockBtn'),
  doingNow: document.getElementById('doingNow'),
  frictionNow: document.getElementById('frictionNow'),
  outcomeNow: document.getElementById('outcomeNow'),
  missionSearch: document.getElementById('missionSearch'),
  sessionSearch: document.getElementById('sessionSearch'),
  missionsList: document.getElementById('missionsList'),
  sessionsList: document.getElementById('sessionsList'),
  statMissions: document.getElementById('statMissions'),
  statBlocks: document.getElementById('statBlocks'),
  statHours: document.getElementById('statHours'),
  statCompleted: document.getElementById('statCompleted'),
  openCriticalList: document.getElementById('openCriticalList'),
  frictionPatterns: document.getElementById('frictionPatterns'),
  exportBtn: document.getElementById('exportBtn'),
  importFile: document.getElementById('importFile'),
  demoBtn: document.getElementById('demoBtn'),
  resetBtn: document.getElementById('resetBtn')
};

init();

function init() {
  bindEvents();
  renderAll();
  restoreTimer();
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('service-worker.js').catch(console.error);
  }
}

function bindEvents() {
  els.pills.forEach(pill => pill.addEventListener('click', () => setView(pill.dataset.view)));
  els.saveMissionBtn.addEventListener('click', saveMission);
  els.clearMissionBtn.addEventListener('click', clearMissionForm);

  els.activeMissionSelect.addEventListener('change', () => {
    state.activeMissionId = els.activeMissionSelect.value;
    persist();
    renderHeaderMission();
  });

  els.startBlockBtn.addEventListener('click', startTimer);
  els.pauseBlockBtn.addEventListener('click', pauseTimer);
  els.stopBlockBtn.addEventListener('click', stopTimerAndLog);

  els.missionSearch.addEventListener('input', renderMissions);
  els.sessionSearch.addEventListener('input', renderSessions);

  els.exportBtn.addEventListener('click', exportData);
  els.importFile.addEventListener('change', importData);
  els.demoBtn.addEventListener('click', loadDemoData);
  els.resetBtn.addEventListener('click', resetAll);
}

function setView(viewName) {
  els.pills.forEach(p => p.classList.toggle('active', p.dataset.view === viewName));
  els.panels.forEach(panel => panel.classList.toggle('active', panel.id === `view-${viewName}`));
}

function saveMission() {
  const title = els.missionTitle.value.trim();
  if (!title) {
    alert('Add a mission first.');
    return;
  }

  const mission = {
    id: crypto.randomUUID(),
    title,
    source: els.missionSource.value,
    horizon: els.missionHorizon.value,
    priority: els.missionPriority.value,
    why: els.missionWhy.value.trim(),
    done: els.missionDone.value.trim(),
    firstStep: els.missionFirstStep.value.trim(),
    status: 'Open',
    createdAt: new Date().toISOString()
  };

  state.missions.unshift(mission);
  state.activeMissionId = mission.id;
  persist();
  clearMissionForm();
  renderAll();
  setView('execute');
}

function clearMissionForm() {
  els.missionTitle.value = '';
  els.missionSource.value = 'Insight';
  els.missionHorizon.value = 'Now';
  els.missionPriority.value = 'Critical';
  els.missionWhy.value = '';
  els.missionDone.value = '';
  els.missionFirstStep.value = '';
}

function getActiveMission() {
  return state.missions.find(m => m.id === state.activeMissionId) || state.missions[0] || null;
}

function startTimer() {
  const missionId = els.activeMissionSelect.value || state.activeMissionId;
  if (!missionId) {
    alert('Save a mission first.');
    return;
  }

  state.activeMissionId = missionId;
  if (state.timer.isRunning) return;
  state.timer.isRunning = true;
  state.timer.startTime = Date.now();
  persist();
  timerInterval = setInterval(updateTimerDisplay, 1000);
  updateTimerDisplay();
}

function pauseTimer() {
  if (!state.timer.isRunning) return;
  const now = Date.now();
  state.timer.elapsedMs += now - state.timer.startTime;
  state.timer.isRunning = false;
  state.timer.startTime = null;
  clearInterval(timerInterval);
  timerInterval = null;
  persist();
  updateTimerDisplay();
}

function stopTimerAndLog() {
  const mission = getActiveMission();
  if (!mission) return;

  if (state.timer.isRunning) {
    const now = Date.now();
    state.timer.elapsedMs += now - state.timer.startTime;
  }

  const elapsedMs = state.timer.elapsedMs;
  if (elapsedMs < 1000) {
    state.timer = { isRunning: false, startTime: null, elapsedMs: 0 };
    persist();
    updateTimerDisplay();
    return;
  }

  state.sessions.unshift({
    id: crypto.randomUUID(),
    missionId: mission.id,
    missionTitle: mission.title,
    blockMode: els.blockMode.value,
    durationMs: elapsedMs,
    doing: els.doingNow.value.trim(),
    friction: els.frictionNow.value.trim(),
    outcome: els.outcomeNow.value.trim(),
    createdAt: new Date().toISOString()
  });

  mission.status = inferMissionStatus(mission.id);

  state.timer = { isRunning: false, startTime: null, elapsedMs: 0 };
  els.doingNow.value = '';
  els.frictionNow.value = '';
  els.outcomeNow.value = '';
  clearInterval(timerInterval);
  timerInterval = null;
  persist();
  renderAll();
  updateTimerDisplay();
}

function inferMissionStatus(missionId) {
  const related = state.sessions.filter(s => s.missionId === missionId);
  if (!related.length) return 'Open';
  const latest = related[0];
  const text = `${latest.outcome} ${latest.doing}`.toLowerCase();
  if (text.includes('done') || text.includes('sent') || text.includes('completed') || text.includes('shipped') || text.includes('finished')) {
    return 'Completed';
  }
  return 'In Progress';
}

function restoreTimer() {
  if (state.timer.isRunning && state.timer.startTime) {
    timerInterval = setInterval(updateTimerDisplay, 1000);
  }
  updateTimerDisplay();
}

function updateTimerDisplay() {
  let elapsed = state.timer.elapsedMs;
  if (state.timer.isRunning && state.timer.startTime) {
    elapsed += Date.now() - state.timer.startTime;
  }
  els.timerDisplay.textContent = formatDuration(elapsed);
  renderHeaderMission();
}

function renderHeaderMission() {
  const mission = getActiveMission();
  els.currentMissionDisplay.textContent = mission ? `${mission.title} · ${mission.priority}` : 'No active mission';
}

function renderAll() {
  renderMissionSelect();
  renderMissions();
  renderSessions();
  renderReview();
  renderHeaderMission();
}

function renderMissionSelect() {
  const currentValue = els.activeMissionSelect.value;
  els.activeMissionSelect.innerHTML = '';

  if (!state.missions.length) {
    const option = document.createElement('option');
    option.value = '';
    option.textContent = 'No missions yet';
    els.activeMissionSelect.appendChild(option);
    return;
  }

  state.missions.forEach(mission => {
    const option = document.createElement('option');
    option.value = mission.id;
    option.textContent = `${mission.title} · ${mission.status}`;
    els.activeMissionSelect.appendChild(option);
  });

  els.activeMissionSelect.value = state.activeMissionId || currentValue || state.missions[0].id;
  state.activeMissionId = els.activeMissionSelect.value;
}

function renderMissions() {
  const q = els.missionSearch.value.trim().toLowerCase();
  const filtered = state.missions.filter(m => {
    const blob = `${m.title} ${m.source} ${m.horizon} ${m.priority} ${m.why} ${m.done} ${m.firstStep} ${m.status}`.toLowerCase();
    return blob.includes(q);
  });

  if (!filtered.length) {
    els.missionsList.className = 'stack-list empty-state';
    els.missionsList.textContent = 'No matching missions.';
    return;
  }

  els.missionsList.className = 'stack-list';
  els.missionsList.innerHTML = '';

  filtered.forEach(mission => {
    const item = document.createElement('div');
    item.className = 'list-item';
    const priorityClass = mission.priority === 'Critical' ? 'critical' : '';
    const statusClass = mission.status === 'Completed' ? 'complete' : 'progress';
    item.innerHTML = `
      <h3>${escapeHtml(mission.title)}</h3>
      <div class="meta">${escapeHtml(mission.source)} · ${escapeHtml(mission.horizon)} · ${formatDate(mission.createdAt)}</div>
      <div class="tag-row">
        <span class="tag ${priorityClass}">${escapeHtml(mission.priority)}</span>
        <span class="tag ${statusClass}">${escapeHtml(mission.status)}</span>
      </div>
      ${mission.why ? `<div class="body-text"><strong>Why:</strong> ${escapeHtml(mission.why)}</div>` : ''}
      ${mission.done ? `<div class="body-text"><strong>Done means:</strong> ${escapeHtml(mission.done)}</div>` : ''}
      ${mission.firstStep ? `<div class="body-text"><strong>First step:</strong> ${escapeHtml(mission.firstStep)}</div>` : ''}
      <div class="item-actions">
        <button class="secondary" data-action="activate" data-id="${mission.id}">Set Active</button>
        <button class="secondary" data-action="toggle" data-id="${mission.id}">${mission.status === 'Completed' ? 'Mark Open' : 'Mark Complete'}</button>
        <button class="secondary" data-action="deleteMission" data-id="${mission.id}">Delete</button>
      </div>
    `;
    els.missionsList.appendChild(item);
  });

  els.missionsList.querySelectorAll('button').forEach(btn => btn.addEventListener('click', handleListAction));
}

function renderSessions() {
  const q = els.sessionSearch.value.trim().toLowerCase();
  const filtered = state.sessions.filter(s => {
    const blob = `${s.missionTitle} ${s.blockMode} ${s.doing} ${s.friction} ${s.outcome}`.toLowerCase();
    return blob.includes(q);
  });

  if (!filtered.length) {
    els.sessionsList.className = 'stack-list empty-state';
    els.sessionsList.textContent = 'No matching execution blocks.';
    return;
  }

  els.sessionsList.className = 'stack-list';
  els.sessionsList.innerHTML = '';

  filtered.forEach(session => {
    const item = document.createElement('div');
    item.className = 'list-item';
    item.innerHTML = `
      <h3>${escapeHtml(session.missionTitle)}</h3>
      <div class="meta">${escapeHtml(session.blockMode)} · ${formatDuration(session.durationMs)} · ${formatDate(session.createdAt)}</div>
      ${session.doing ? `<div class="body-text"><strong>Doing:</strong> ${escapeHtml(session.doing)}</div>` : ''}
      ${session.friction ? `<div class="body-text"><strong>Friction:</strong> ${escapeHtml(session.friction)}</div>` : ''}
      ${session.outcome ? `<div class="body-text"><strong>Outcome:</strong> ${escapeHtml(session.outcome)}</div>` : ''}
      <div class="item-actions">
        <button class="secondary" data-action="deleteSession" data-id="${session.id}">Delete</button>
      </div>
    `;
    els.sessionsList.appendChild(item);
  });

  els.sessionsList.querySelectorAll('button').forEach(btn => btn.addEventListener('click', handleListAction));
}

function renderReview() {
  els.statMissions.textContent = String(state.missions.length);
  els.statBlocks.textContent = String(state.sessions.length);
  els.statHours.textContent = (state.sessions.reduce((sum, s) => sum + s.durationMs, 0) / 3600000).toFixed(1);
  els.statCompleted.textContent = String(state.missions.filter(m => m.status === 'Completed').length);

  const criticalOpen = state.missions.filter(m => m.priority === 'Critical' && m.status !== 'Completed');
  if (!criticalOpen.length) {
    els.openCriticalList.className = 'stack-list empty-state';
    els.openCriticalList.textContent = 'No critical open missions.';
  } else {
    els.openCriticalList.className = 'stack-list';
    els.openCriticalList.innerHTML = '';
    criticalOpen.forEach(m => {
      const div = document.createElement('div');
      div.className = 'list-item';
      div.innerHTML = `
        <h3>${escapeHtml(m.title)}</h3>
        <div class="meta">${escapeHtml(m.horizon)} · ${escapeHtml(m.status)}</div>
        ${m.firstStep ? `<div class="body-text">${escapeHtml(m.firstStep)}</div>` : ''}
      `;
      els.openCriticalList.appendChild(div);
    });
  }

  const frictionCounts = {};
  state.sessions.forEach(s => {
    if (!s.friction) return;
    s.friction
      .split(/[,\n;]/)
      .map(x => x.trim().toLowerCase())
      .filter(Boolean)
      .forEach(token => {
        frictionCounts[token] = (frictionCounts[token] || 0) + 1;
      });
  });

  const topFriction = Object.entries(frictionCounts).sort((a,b) => b[1]-a[1]).slice(0, 8);
  if (!topFriction.length) {
    els.frictionPatterns.className = 'stack-list empty-state';
    els.frictionPatterns.textContent = 'No friction patterns yet.';
  } else {
    els.frictionPatterns.className = 'stack-list';
    els.frictionPatterns.innerHTML = '';
    topFriction.forEach(([name, count]) => {
      const div = document.createElement('div');
      div.className = 'list-item';
      div.innerHTML = `
        <h3>${escapeHtml(name)}</h3>
        <div class="meta">${count} block${count === 1 ? '' : 's'}</div>
      `;
      els.frictionPatterns.appendChild(div);
    });
  }
}

function handleListAction(e) {
  const id = e.currentTarget.dataset.id;
  const action = e.currentTarget.dataset.action;

  if (action === 'activate') {
    state.activeMissionId = id;
    persist();
    renderAll();
    setView('execute');
  }

  if (action === 'toggle') {
    const mission = state.missions.find(m => m.id === id);
    if (!mission) return;
    mission.status = mission.status === 'Completed' ? 'Open' : 'Completed';
    persist();
    renderAll();
  }

  if (action === 'deleteMission') {
    if (!confirm('Delete this mission and its related blocks?')) return;
    state.missions = state.missions.filter(m => m.id !== id);
    state.sessions = state.sessions.filter(s => s.missionId !== id);
    if (state.activeMissionId === id) state.activeMissionId = state.missions[0]?.id || '';
    persist();
    renderAll();
  }

  if (action === 'deleteSession') {
    if (!confirm('Delete this execution block?')) return;
    state.sessions = state.sessions.filter(s => s.id !== id);
    persist();
    renderAll();
  }
}

function exportData() {
  const blob = new Blob([JSON.stringify(state, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `act-like-an-operator-export-${new Date().toISOString().slice(0,10)}.json`;
  a.click();
  URL.revokeObjectURL(url);
}

function importData(e) {
  const file = e.target.files?.[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = (event) => {
    try {
      const parsed = JSON.parse(event.target.result);
      state = {
        ...structuredClone(defaultState),
        ...parsed,
        timer: { isRunning: false, startTime: null, elapsedMs: 0 }
      };
      persist();
      renderAll();
      updateTimerDisplay();
      alert('Import complete.');
    } catch {
      alert('That file could not be imported.');
    } finally {
      els.importFile.value = '';
    }
  };
  reader.readAsText(file);
}

function loadDemoData() {
  if (state.missions.length || state.sessions.length) {
    if (!confirm('Demo data will be added to your existing data. Continue?')) return;
  }

  const id1 = crypto.randomUUID();
  const id2 = crypto.randomUUID();

  state.missions.unshift(
    {
      id: id1,
      title: 'Finalize outreach sequence for first buyer',
      source: 'Opportunity',
      horizon: 'Today',
      priority: 'Critical',
      why: 'This directly supports deal creation and revenue movement.',
      done: 'Three emails are finalized and ready to send.',
      firstStep: 'Write the first outreach email in final form.',
      status: 'In Progress',
      createdAt: new Date().toISOString()
    },
    {
      id: id2,
      title: 'Package product screenshots for deck',
      source: 'Deadline',
      horizon: 'This Week',
      priority: 'High',
      why: 'The deck becomes more credible and easier to sell.',
      done: 'Screenshot set is organized and inserted into deck.',
      firstStep: 'Select the 6 strongest screens across the apps.',
      status: 'Open',
      createdAt: new Date().toISOString()
    }
  );

  state.sessions.unshift(
    {
      id: crypto.randomUUID(),
      missionId: id1,
      missionTitle: 'Finalize outreach sequence for first buyer',
      blockMode: 'Outreach',
      durationMs: 28 * 60 * 1000,
      doing: 'Drafted and tightened the first outreach message.',
      friction: 'perfectionism, overthinking',
      outcome: 'First message materially improved but not yet sent.',
      createdAt: new Date().toISOString()
    },
    {
      id: crypto.randomUUID(),
      missionId: id2,
      missionTitle: 'Package product screenshots for deck',
      blockMode: 'Build',
      durationMs: 36 * 60 * 1000,
      doing: 'Collected screenshots from two flagship apps.',
      friction: 'file organization',
      outcome: 'Started asset set for deck.',
      createdAt: new Date().toISOString()
    }
  );

  state.activeMissionId = id1;
  persist();
  renderAll();
}

function resetAll() {
  if (!confirm('Reset the full app? This cannot be undone.')) return;
  localStorage.removeItem(STORAGE_KEY);
  state = structuredClone(defaultState);
  clearInterval(timerInterval);
  timerInterval = null;
  renderAll();
  updateTimerDisplay();
}

function loadState() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (!raw) return structuredClone(defaultState);
    const parsed = JSON.parse(raw);
    return {
      ...structuredClone(defaultState),
      ...parsed,
      timer: {
        isRunning: false,
        startTime: null,
        elapsedMs: parsed?.timer?.elapsedMs || 0
      }
    };
  } catch {
    return structuredClone(defaultState);
  }
}

function persist() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
}

function formatDuration(ms) {
  const totalSeconds = Math.floor(ms / 1000);
  const hours = Math.floor(totalSeconds / 3600).toString().padStart(2, '0');
  const minutes = Math.floor((totalSeconds % 3600) / 60).toString().padStart(2, '0');
  const seconds = Math.floor(totalSeconds % 60).toString().padStart(2, '0');
  return `${hours}:${minutes}:${seconds}`;
}

function formatDate(iso) {
  const d = new Date(iso);
  return d.toLocaleString([], {
    year: 'numeric',
    month: 'short',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
    hour12: false
  });
}

function escapeHtml(str='') {
  return str
    .replaceAll('&','&amp;')
    .replaceAll('<','&lt;')
    .replaceAll('>','&gt;')
    .replaceAll('"','&quot;')
    .replaceAll("'",'&#039;');
}
