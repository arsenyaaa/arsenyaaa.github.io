<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Billex Org Chart</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: #0a0a0a;
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 40px 20px;
    overflow-x: hidden;
  }

  .header {
    text-align: center;
    margin-bottom: 48px;
  }

  .header h1 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 52px;
    letter-spacing: 6px;
    color: #fff;
  }

  .header p {
    font-size: 12px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: #555;
    margin-top: 6px;
  }

  .chart {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0;
    width: 100%;
    max-width: 900px;
  }

  /* NODE */
  .node {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 14px 24px;
    border-radius: 4px;
    text-align: center;
    transition: transform 0.2s;
    cursor: default;
    min-width: 160px;
  }

  .node:hover { transform: translateY(-2px); }

  .node .title {
    font-family: 'Bebas Neue', sans-serif;
    letter-spacing: 2px;
    font-size: 15px;
  }

  .node .sub {
    font-size: 10px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    margin-top: 3px;
    opacity: 0.6;
  }

  /* CEO */
  .ceo-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .node-ceo {
    background: #ffffff;
    color: #0a0a0a;
    padding: 18px 40px;
    min-width: 200px;
    box-shadow: 0 0 40px rgba(255,255,255,0.1);
  }

  .node-ceo .title { font-size: 20px; letter-spacing: 4px; }

  /* CONNECTOR LINES */
  .v-line {
    width: 2px;
    background: #2a2a2a;
  }

  .v-line-short { height: 28px; }
  .v-line-med { height: 40px; }

  .h-line-wrap {
    display: flex;
    align-items: flex-start;
    position: relative;
    width: 100%;
    justify-content: center;
  }

  /* MIDDLE ROW — 3 managers */
  .managers-row {
    display: flex;
    align-items: flex-start;
    justify-content: center;
    gap: 0;
    width: 100%;
    position: relative;
  }

  .manager-col {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    flex: 1;
    max-width: 280px;
  }

  .h-connector {
    position: absolute;
    top: 0;
    left: 50%;
    right: 0;
    height: 2px;
    background: #2a2a2a;
  }

  .h-connector.left  { left: 0; right: 50%; }
  .h-connector.right { left: 50%; right: 0; }
  .h-connector.full  { left: 0; right: 0; }

  /* horizontal bar across all 3 */
  .h-bar-wrap {
    width: 100%;
    display: flex;
    justify-content: center;
    position: relative;
    height: 2px;
  }

  /* Manager nodes */
  .node-manager {
    background: #1a1a1a;
    border: 1px solid #2e2e2e;
    color: #fff;
    min-width: 160px;
  }

  .node-manager .title { color: #ffffff; }

  /* Sub nodes */
  .node-sub {
    background: #111;
    border: 1px solid #222;
    color: #aaa;
    min-width: 140px;
    padding: 10px 18px;
  }

  .node-sub .title { font-size: 12px; color: #ddd; }

  /* China node special */
  .node-china {
    background: #111;
    border: 1px solid #444;
    color: #aaa;
    min-width: 160px;
    padding: 10px 18px;
  }

  .node-china .title { font-size: 12px; color: #ffffff; }
  .node-china .badge {
    font-size: 9px;
    letter-spacing: 1px;
    background: #222;
    color: #999;
    border: 1px solid #444;
    border-radius: 2px;
    padding: 2px 6px;
    margin-top: 5px;
    display: inline-block;
  }

  /* sub cols */
  .sub-col {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .sub-row {
    display: flex;
    gap: 16px;
    align-items: flex-start;
    justify-content: center;
  }

  /* legend */
  .legend {
    margin-top: 48px;
    display: flex;
    gap: 28px;
    opacity: 0.5;
    font-size: 11px;
    letter-spacing: 1px;
    color: #666;
    text-transform: uppercase;
  }

  .legend-item { display: flex; align-items: center; gap: 8px; }
  .legend-dot { width: 10px; height: 10px; border-radius: 2px; }
  .dot-gold { background: #ffffff; }
  .dot-mgr  { background: #1a1a1a; border: 1px solid #2e2e2e; }
  .dot-sub  { background: #111; border: 1px solid #222; }

  /* fade in */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .chart > * {
    animation: fadeUp 0.5s ease both;
  }
  .chart > *:nth-child(1) { animation-delay: 0.05s; }
  .chart > *:nth-child(2) { animation-delay: 0.1s; }
  .chart > *:nth-child(3) { animation-delay: 0.15s; }
  .chart > *:nth-child(4) { animation-delay: 0.2s; }
  .chart > *:nth-child(5) { animation-delay: 0.25s; }
  .chart > *:nth-child(6) { animation-delay: 0.3s; }
</style>
</head>
<body>

<div class="header">
  <h1>BILLEX</h1>
  <p>Organizational Structure — Modified Functional</p>
</div>

<div class="chart">

  <!-- ceo -->
  <div class="ceo-wrap">
    <div class="node node-ceo">
      <div class="title">CEO</div>
      <div class="sub">Executive Leadership</div>
    </div>
  </div>

  <!-- v line down from CEO -->
  <div class="v-line v-line-med"></div>

  <!-- Horizontal bar spanning all 3 manager columns -->
  <div style="width:100%; max-width:780px; height:2px; background:#2a2a2a; position:relative;">
    <!-- tick down at left -->
    <div style="position:absolute; left:16.66%; top:0; width:2px; height:28px; background:#2a2a2a;"></div>
    <!-- tick down at center -->
    <div style="position:absolute; left:50%; top:0; width:2px; height:28px; background:#2a2a2a; transform:translateX(-50%);"></div>
    <!-- tick down at right -->
    <div style="position:absolute; right:16.66%; top:0; width:2px; height:28px; background:#2a2a2a;"></div>
  </div>

  <!-- mnagers Row -->
  <div class="managers-row" style="max-width:780px; margin-top:28px;">

    <!-- operations -->
    <div class="manager-col">
      <div class="node node-manager">
        <div class="title">Operations</div>
        <div class="sub">Manager</div>
      </div>
      <div class="v-line" style="height:24px;"></div>
      <!-- H bar for 2 sub nodes -->
      <div style="width:180px; height:2px; background:#2a2a2a; position:relative;">
        <div style="position:absolute; left:25%; top:0; width:2px; height:20px; background:#2a2a2a;"></div>
        <div style="position:absolute; right:25%; top:0; width:2px; height:20px; background:#2a2a2a;"></div>
      </div>
      <div class="sub-row" style="margin-top:20px;">
        <div class="sub-col">
          <div class="node node-sub">
            <div class="title">Customer Service</div>
            <div class="sub">Support Team</div>
          </div>
        </div>
        <div class="sub-col">
          <div class="node node-china">
            <div class="title">Manufacturing</div>
            <div class="sub">China Operations</div>
            <div class="badge">~40 Factory Workers</div>
          </div>
        </div>
      </div>
    </div>

    <!-- marketing -->
    <div class="manager-col">
      <div class="node node-manager">
        <div class="title">Marketing</div>
        <div class="sub">Manager</div>
      </div>
      <div class="v-line" style="height:24px;"></div>
      <div class="node node-sub">
        <div class="title">Marketing Team</div>
        <div class="sub">Content & Ads</div>
      </div>
    </div>

    <!-- accounting -->
    <div class="manager-col">
      <div class="node node-manager">
        <div class="title">Accounting</div>
        <div class="sub">Department</div>
      </div>
      <div class="v-line" style="height:24px;"></div>
      <div class="node node-sub">
        <div class="title">Finance & Records</div>
        <div class="sub">Proprietary Software</div>
      </div>
    </div>

  </div>

</div>

<div class="legend">
  <div class="legend-item"><div class="legend-dot dot-gold"></div> Executive</div>
  <div class="legend-item"><div class="legend-dot dot-mgr"></div> Management</div>
  <div class="legend-item"><div class="legend-dot dot-sub"></div> Department / Team</div>
</div>

</body>
</html>
