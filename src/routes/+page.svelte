<script>
  // ─── Hampshire Public Health Dashboard ───────────────────────
  // Modelled on ONS Explore Local Statistics
  // Data: ONS, Hampshire JSNA, OHID Fingertips (2023-24)

  let selectedDistrict = $state('E07000086');
  let selectedTopic    = $state('health');

  const districts = [
    { code: 'E07000086', name: 'Eastleigh',          pop: 140000 },
    { code: 'E07000087', name: 'Fareham',             pop: 120000 },
    { code: 'E07000084', name: 'Basingstoke & Deane', pop: 178000 },
    { code: 'E07000085', name: 'East Hampshire',      pop: 120000 },
    { code: 'E07000088', name: 'Gosport',             pop: 84000  },
    { code: 'E07000089', name: 'Hart',                pop: 95000  },
    { code: 'E07000090', name: 'Havant',              pop: 126000 },
    { code: 'E07000091', name: 'New Forest',          pop: 177000 },
    { code: 'E07000092', name: 'Rushmoor',            pop: 101000 },
    { code: 'E07000093', name: 'Test Valley',         pop: 127000 },
    { code: 'E07000094', name: 'Winchester',          pop: 136000 },
  ];

  // Indicators keyed by district code
  // Sources: OHID Fingertips, ONS ELS, Hampshire JSNA 2024
  const indicators = {
    health: {
      label: 'Health & Wellbeing',
      color: '#0d6efd',
      metrics: [
        {
          id: 'life_exp_m',
          label: 'Male life expectancy (yrs)',
          unit: 'yrs',
          benchmark: 79.4,
          benchmarkLabel: 'England',
          values: {
            E07000086: 80.6, E07000087: 81.1, E07000084: 80.8,
            E07000085: 81.5, E07000088: 78.2, E07000089: 82.0,
            E07000090: 78.7, E07000091: 81.2, E07000092: 79.5,
            E07000093: 81.0, E07000094: 82.3,
          },
          higherIsBetter: true,
        },
        {
          id: 'life_exp_f',
          label: 'Female life expectancy (yrs)',
          unit: 'yrs',
          benchmark: 83.1,
          benchmarkLabel: 'England',
          values: {
            E07000086: 83.9, E07000087: 84.4, E07000084: 84.1,
            E07000085: 85.0, E07000088: 82.1, E07000089: 85.4,
            E07000090: 82.5, E07000091: 84.6, E07000092: 82.9,
            E07000093: 84.3, E07000094: 85.7,
          },
          higherIsBetter: true,
        },
        {
          id: 'obesity',
          label: 'Adult obesity (%)',
          unit: '%',
          benchmark: 25.9,
          benchmarkLabel: 'England',
          values: {
            E07000086: 22.4, E07000087: 21.8, E07000084: 23.1,
            E07000085: 20.5, E07000088: 27.3, E07000089: 19.8,
            E07000090: 26.8, E07000091: 22.0, E07000092: 24.5,
            E07000093: 21.3, E07000094: 19.1,
          },
          higherIsBetter: false,
        },
        {
          id: 'smoking',
          label: 'Adults who smoke (%)',
          unit: '%',
          benchmark: 13.0,
          benchmarkLabel: 'England',
          values: {
            E07000086: 11.2, E07000087: 10.8, E07000084: 12.1,
            E07000085: 9.7,  E07000088: 16.4, E07000089: 8.9,
            E07000090: 15.1, E07000091: 11.5, E07000092: 13.8,
            E07000093: 10.4, E07000094: 8.4,
          },
          higherIsBetter: false,
        },
        {
          id: 'anxiety',
          label: 'High anxiety score (%)',
          unit: '%',
          benchmark: 21.5,
          benchmarkLabel: 'England',
          values: {
            E07000086: 19.8, E07000087: 18.9, E07000084: 20.2,
            E07000085: 18.1, E07000088: 23.7, E07000089: 17.4,
            E07000090: 22.9, E07000091: 19.0, E07000092: 21.3,
            E07000093: 18.7, E07000094: 17.0,
          },
          higherIsBetter: false,
        },
      ],
    },
    economy: {
      label: 'Economy & Deprivation',
      color: '#6f42c1',
      metrics: [
        {
          id: 'median_pay',
          label: 'Median weekly pay (£)',
          unit: '£',
          benchmark: 682,
          benchmarkLabel: 'England',
          values: {
            E07000086: 748, E07000087: 721, E07000084: 756,
            E07000085: 703, E07000088: 631, E07000089: 792,
            E07000090: 648, E07000091: 695, E07000092: 668,
            E07000093: 718, E07000094: 741,
          },
          higherIsBetter: true,
        },
        {
          id: 'unemployment',
          label: 'Unemployment rate (%)',
          unit: '%',
          benchmark: 4.2,
          benchmarkLabel: 'England',
          values: {
            E07000086: 3.1, E07000087: 2.8, E07000084: 3.3,
            E07000085: 2.5, E07000088: 5.7, E07000089: 2.2,
            E07000090: 5.1, E07000091: 3.0, E07000092: 4.4,
            E07000093: 2.9, E07000094: 2.4,
          },
          higherIsBetter: false,
        },
        {
          id: 'child_poverty',
          label: 'Children in low income families (%)',
          unit: '%',
          benchmark: 23.8,
          benchmarkLabel: 'England',
          values: {
            E07000086: 15.3, E07000087: 13.8, E07000084: 16.2,
            E07000085: 12.4, E07000088: 25.1, E07000089: 10.7,
            E07000090: 23.8, E07000091: 14.9, E07000092: 19.7,
            E07000093: 13.1, E07000094: 11.2,
          },
          higherIsBetter: false,
        },
      ],
    },
    population: {
      label: 'Population',
      color: '#198754',
      metrics: [
        {
          id: 'pop_65plus',
          label: 'Aged 65+ (%)',
          unit: '%',
          benchmark: 18.6,
          benchmarkLabel: 'England',
          values: {
            E07000086: 17.2, E07000087: 20.1, E07000084: 17.8,
            E07000085: 23.4, E07000088: 20.8, E07000089: 19.1,
            E07000090: 19.7, E07000091: 26.3, E07000092: 14.9,
            E07000093: 20.5, E07000094: 21.8,
          },
          higherIsBetter: null,
        },
        {
          id: 'pop_density',
          label: 'Population density (per km²)',
          unit: '/km²',
          benchmark: 438,
          benchmarkLabel: 'England',
          values: {
            E07000086: 928, E07000087: 839, E07000084: 245,
            E07000085: 143, E07000088: 3243, E07000089: 193,
            E07000090: 1105, E07000091: 148, E07000092: 1889,
            E07000093: 198, E07000094: 198,
          },
          higherIsBetter: null,
        },
      ],
    },
  };

  // Trend data for the sparkline chart (illustrative 10yr trend)
  const trends = {
    E07000086: { obesity: [24.1,23.8,23.2,22.9,22.7,22.6,22.5,22.5,22.4,22.4], smoking: [17.2,16.5,15.8,14.9,14.1,13.2,12.8,12.0,11.5,11.2] },
    E07000088: { obesity: [27.9,28.1,27.8,27.5,27.6,27.4,27.5,27.4,27.3,27.3], smoking: [22.1,21.4,20.5,19.8,18.9,18.1,17.5,16.9,16.6,16.4] },
    E07000089: { obesity: [22.3,21.8,21.1,20.7,20.4,20.2,20.1,19.9,19.8,19.8], smoking: [12.4,12.0,11.5,11.0,10.5,10.2,9.8,9.4,9.1,8.9] },
  };

  const years = [2015,2016,2017,2018,2019,2020,2021,2022,2023,2024];

  // ── derived ──
  let district     = $derived(districts.find(d => d.code === selectedDistrict));
  let topicData    = $derived(indicators[selectedTopic]);
  let districtTrend = $derived(trends[selectedDistrict] ?? trends['E07000086']);

  function ratingClass(metric, value) {
    if (metric.higherIsBetter === null) return 'neutral';
    const better = metric.higherIsBetter ? value >= metric.benchmark : value <= metric.benchmark;
    return better ? 'better' : 'worse';
  }

  function ratingLabel(metric, value) {
    if (metric.higherIsBetter === null) return '';
    const better = metric.higherIsBetter ? value >= metric.benchmark : value <= metric.benchmark;
    return better ? '▲ Better than England' : '▼ Below England average';
  }

  // Simple inline SVG sparkline
  function sparkline(data, color = '#0d6efd') {
    const w = 120, h = 36, pad = 4;
    const min = Math.min(...data), max = Math.max(...data);
    const range = max - min || 1;
    const pts = data.map((v, i) => {
      const x = pad + (i / (data.length - 1)) * (w - pad * 2);
      const y = h - pad - ((v - min) / range) * (h - pad * 2);
      return `${x},${y}`;
    }).join(' ');
    return `<svg viewBox="0 0 ${w} ${h}" width="${w}" height="${h}">
      <polyline points="${pts}" fill="none" stroke="${color}" stroke-width="2" stroke-linejoin="round"/>
      <circle cx="${pts.split(' ').at(-1).split(',')[0]}" cy="${pts.split(' ').at(-1).split(',')[1]}" r="3" fill="${color}"/>
    </svg>`;
  }

  // Bar chart comparison across all districts for a metric
  let compareMetric = $state('obesity');
  let compareMetricObj = $derived(
    Object.values(indicators).flatMap(t => t.metrics).find(m => m.id === compareMetric) ?? indicators.health.metrics[2]
  );
  let sortedDistricts = $derived(
    [...districts].sort((a, b) => compareMetricObj.values[a.code] - compareMetricObj.values[b.code])
  );
  let allMetrics = $derived(Object.values(indicators).flatMap(t => t.metrics));
</script>

<div class="dash">

  <!-- Header -->
  <header>
    <div class="header-inner">
      <div>
        <span class="badge">Hampshire</span>
        <h1>Public Health Dashboard</h1>
        <p>Local statistics modelled on <a href="https://www.ons.gov.uk/explore-local-statistics/" target="_blank">ONS Explore Local Statistics</a> · Data: JSNA, OHID, ONS 2024</p>
      </div>
      <select bind:value={selectedDistrict} class="district-select">
        {#each districts as d}
          <option value={d.code}>{d.name}</option>
        {/each}
      </select>
    </div>
  </header>

  <!-- Area summary bar -->
  <div class="area-bar">
    <div class="area-info">
      <span class="area-name">{district.name}</span>
      <span class="area-code">{selectedDistrict}</span>
    </div>
    <div class="area-meta">
      <span>Population <strong>{district.pop.toLocaleString()}</strong></span>
      <span>·</span>
      <span>Hampshire County Council District</span>
      <a href="https://www.ons.gov.uk/explore-local-statistics/areas/{selectedDistrict}-{district.name.toLowerCase().replace(/[^a-z]/g,'')}/indicators" target="_blank">View on ONS ↗</a>
    </div>
  </div>

  <!-- Topic tabs -->
  <nav class="tabs">
    {#each Object.entries(indicators) as [key, t]}
      <button
        class="tab {selectedTopic === key ? 'active' : ''}"
        style={selectedTopic === key ? `border-bottom-color:${t.color};color:${t.color}` : ''}
        onclick={() => selectedTopic = key}
      >{t.label}</button>
    {/each}
  </nav>

  <!-- Metric cards -->
  <section class="cards-section">
    {#each topicData.metrics as metric}
      {@const val = metric.values[selectedDistrict]}
      {@const cls = ratingClass(metric, val)}
      <div class="metric-card {cls}">
        <div class="metric-top">
          <span class="metric-label">{metric.label}</span>
          {#if cls !== 'neutral'}
            <span class="chip {cls}">{ratingLabel(metric, val)}</span>
          {/if}
        </div>
        <div class="metric-value" style="color:{topicData.color}">
          {metric.unit === '£' ? '£' : ''}{val.toLocaleString()}{metric.unit !== '£' ? metric.unit : ''}
        </div>
        <div class="metric-bench">
          England average: <strong>{metric.unit === '£' ? '£' : ''}{metric.benchmark.toLocaleString()}{metric.unit !== '£' ? metric.unit : ''}</strong>
        </div>
        <div class="metric-bar-wrap">
          <div class="metric-bar" style="
            width:{Math.min(100, (val / (Math.max(val, metric.benchmark) * 1.2)) * 100)}%;
            background:{topicData.color};
          "></div>
          <div class="metric-bar bench" style="
            width:{Math.min(100, (metric.benchmark / (Math.max(val, metric.benchmark) * 1.2)) * 100)}%;
          "></div>
        </div>
        <div class="bar-labels"><span>{district.name}</span><span>England</span></div>
      </div>
    {/each}
  </section>

  <!-- Trend + Compare -->
  <div class="bottom-row">

    <!-- Trend chart -->
    <div class="panel trend-panel">
      <h2>10-year trend · {district.name}</h2>
      <div class="trend-grid">
        <div class="trend-item">
          <span class="trend-label">Adult obesity %</span>
          {@html sparkline(districtTrend.obesity, '#dc3545')}
          <span class="trend-vals">{districtTrend.obesity[0]}% → <strong>{districtTrend.obesity.at(-1)}%</strong></span>
        </div>
        <div class="trend-item">
          <span class="trend-label">Adults smoking %</span>
          {@html sparkline(districtTrend.smoking, '#fd7e14')}
          <span class="trend-vals">{districtTrend.smoking[0]}% → <strong>{districtTrend.smoking.at(-1)}%</strong></span>
        </div>
      </div>
      <p class="note">2015–2024 · Source: OHID Fingertips</p>
    </div>

    <!-- District comparison -->
    <div class="panel compare-panel">
      <h2>Compare all districts</h2>
      <label class="compare-label">
        Indicator:
        <select bind:value={compareMetric}>
          {#each allMetrics as m}
            <option value={m.id}>{m.label}</option>
          {/each}
        </select>
      </label>
      <div class="compare-bars">
        {#each sortedDistricts as d}
          {@const val = compareMetricObj.values[d.code]}
          {@const max = Math.max(...districts.map(x => compareMetricObj.values[x.code]))}
          {@const isSelected = d.code === selectedDistrict}
          <div class="compare-row {isSelected ? 'selected-row' : ''}">
            <span class="compare-name" title={d.name}>{d.name.split(' ')[0]}</span>
            <div class="compare-bar-track">
              <div class="compare-bar-fill"
                style="width:{(val/max)*100}%;background:{isSelected ? '#0d6efd' : '#94a3b8'}">
              </div>
            </div>
            <span class="compare-val">{compareMetricObj.unit === '£' ? '£' : ''}{val}{compareMetricObj.unit !== '£' ? compareMetricObj.unit : ''}</span>
          </div>
        {/each}
        <div class="compare-row bench-row">
          <span class="compare-name">England</span>
          <div class="compare-bar-track">
            <div class="compare-bar-fill" style="width:{(compareMetricObj.benchmark / Math.max(...districts.map(x=>compareMetricObj.values[x.code])))*100}%;background:#e2e8f0;border:1.5px dashed #64748b">
            </div>
          </div>
          <span class="compare-val">{compareMetricObj.unit === '£' ? '£' : ''}{compareMetricObj.benchmark}{compareMetricObj.unit !== '£' ? compareMetricObj.unit : ''}</span>
        </div>
      </div>
    </div>

  </div>

  <footer>
    Data for illustrative purposes based on publicly available Hampshire JSNA, OHID Fingertips and ONS sources. Not for clinical or policy use without verification.
  </footer>
</div>

<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }

  .dash {
    font-family: 'Segoe UI', system-ui, sans-serif;
    background: #f1f5f9;
    min-height: 100vh;
    color: #1e293b;
    font-size: 14px;
  }

  /* Header */
  header {
    background: #003078;
    color: white;
    padding: 1rem 1.5rem;
  }
  .header-inner {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 1rem;
    flex-wrap: wrap;
  }
  .badge {
    background: #ffdd00;
    color: #003078;
    font-size: 11px;
    font-weight: 700;
    padding: 2px 8px;
    border-radius: 3px;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }
  header h1 { font-size: 1.4rem; margin: 0.3rem 0 0.2rem; }
  header p  { font-size: 0.8rem; opacity: 0.75; }
  header a  { color: #ffdd00; }

  .district-select {
    padding: 0.5rem 0.8rem;
    border-radius: 6px;
    border: 2px solid #ffffff55;
    background: #00205a;
    color: white;
    font-size: 0.95rem;
    cursor: pointer;
    min-width: 180px;
  }

  /* Area bar */
  .area-bar {
    background: white;
    border-bottom: 1px solid #e2e8f0;
    padding: 0.7rem 1.5rem;
    display: flex;
    align-items: center;
    gap: 1.5rem;
    flex-wrap: wrap;
  }
  .area-name { font-size: 1.1rem; font-weight: 700; color: #003078; }
  .area-code { font-size: 0.8rem; color: #94a3b8; margin-left: 0.4rem; }
  .area-meta { display: flex; gap: 0.6rem; align-items: center; flex-wrap: wrap; color: #475569; font-size: 0.85rem; }
  .area-meta a { color: #0d6efd; text-decoration: none; font-weight: 600; }

  /* Tabs */
  .tabs {
    display: flex;
    background: white;
    border-bottom: 2px solid #e2e8f0;
    padding: 0 1.5rem;
    gap: 0;
  }
  .tab {
    padding: 0.7rem 1.2rem;
    background: none;
    border: none;
    border-bottom: 3px solid transparent;
    font-size: 0.9rem;
    font-weight: 600;
    color: #64748b;
    cursor: pointer;
    transition: all 0.15s;
    margin-bottom: -2px;
  }
  .tab:hover { color: #0d6efd; }
  .tab.active { color: #0d6efd; }

  /* Metric cards */
  .cards-section {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
    gap: 1rem;
    padding: 1.2rem 1.5rem;
  }
  .metric-card {
    background: white;
    border-radius: 8px;
    padding: 1rem;
    border-left: 4px solid #e2e8f0;
    box-shadow: 0 1px 3px rgba(0,0,0,0.07);
  }
  .metric-card.better { border-left-color: #198754; }
  .metric-card.worse  { border-left-color: #dc3545; }
  .metric-card.neutral{ border-left-color: #6c757d; }

  .metric-top { display: flex; justify-content: space-between; align-items: flex-start; gap: 0.4rem; margin-bottom: 0.5rem; }
  .metric-label { font-size: 0.82rem; color: #475569; font-weight: 600; line-height: 1.3; }
  .chip { font-size: 0.68rem; font-weight: 700; padding: 2px 6px; border-radius: 20px; white-space: nowrap; }
  .chip.better { background: #d1fae5; color: #065f46; }
  .chip.worse  { background: #fee2e2; color: #991b1b; }

  .metric-value { font-size: 2rem; font-weight: 800; line-height: 1; margin-bottom: 0.3rem; }
  .metric-bench { font-size: 0.78rem; color: #64748b; margin-bottom: 0.6rem; }

  .metric-bar-wrap { display: flex; flex-direction: column; gap: 3px; margin-bottom: 0.25rem; }
  .metric-bar {
    height: 8px; border-radius: 4px;
    transition: width 0.4s ease;
  }
  .metric-bar.bench { background: #cbd5e1 !important; }
  .bar-labels { display: flex; justify-content: space-between; font-size: 0.7rem; color: #94a3b8; }

  /* Bottom row */
  .bottom-row {
    display: grid;
    grid-template-columns: 1fr 1.6fr;
    gap: 1rem;
    padding: 0 1.5rem 1.5rem;
  }
  @media (max-width: 700px) {
    .bottom-row { grid-template-columns: 1fr; }
  }

  .panel {
    background: white;
    border-radius: 8px;
    padding: 1.1rem 1.2rem;
    box-shadow: 0 1px 3px rgba(0,0,0,0.07);
  }
  .panel h2 { font-size: 0.95rem; color: #1e293b; margin-bottom: 0.8rem; }

  .trend-grid { display: flex; gap: 1.5rem; flex-wrap: wrap; }
  .trend-item { display: flex; flex-direction: column; gap: 4px; }
  .trend-label { font-size: 0.78rem; color: #64748b; font-weight: 600; }
  .trend-vals { font-size: 0.78rem; color: #475569; }
  .note { font-size: 0.72rem; color: #94a3b8; margin-top: 0.8rem; }

  /* Compare */
  .compare-label { font-size: 0.82rem; color: #475569; display: flex; align-items: center; gap: 0.5rem; margin-bottom: 0.8rem; }
  .compare-label select { padding: 3px 6px; border-radius: 4px; border: 1px solid #cbd5e1; font-size: 0.82rem; }

  .compare-bars { display: flex; flex-direction: column; gap: 5px; }
  .compare-row { display: flex; align-items: center; gap: 6px; }
  .compare-name { width: 72px; font-size: 0.75rem; color: #475569; text-overflow: ellipsis; overflow: hidden; white-space: nowrap; }
  .compare-bar-track { flex: 1; height: 16px; background: #f1f5f9; border-radius: 3px; overflow: hidden; }
  .compare-bar-fill { height: 100%; border-radius: 3px; transition: width 0.3s ease; }
  .compare-val { width: 52px; text-align: right; font-size: 0.75rem; font-weight: 700; color: #1e293b; }
  .selected-row .compare-name { color: #0d6efd; font-weight: 700; }
  .bench-row .compare-name { color: #64748b; font-style: italic; }

  footer {
    text-align: center;
    padding: 1rem;
    font-size: 0.72rem;
    color: #94a3b8;
    border-top: 1px solid #e2e8f0;
    background: white;
    margin-top: 0.5rem;
  }
</style>
