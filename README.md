<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="球球日曆">
<meta name="theme-color" content="#ffb3cc">
<title>球球の魔法日曆 🌸</title>
<link rel="manifest" href="manifest.json">
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700;900&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{font-family:'Noto Sans TC',sans-serif;background:linear-gradient(160deg,#ffd6e7 0%,#ffecf5 45%,#fff0fa 100%);min-height:100vh;min-height:-webkit-fill-available;padding-bottom:74px;overflow-x:hidden}

/* 頂欄 */
.hdr{background:rgba(255,255,255,.84);backdrop-filter:blur(14px);border-bottom:3px dashed #ffb3cc;padding:11px 14px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:50}
.hdr-logo{font-weight:900;color:#d4547a;font-size:1rem}
.hdr-date{font-size:.67rem;color:#e87fa0;font-weight:700}

/* 橫幅 */
#banners{position:fixed;top:0;left:0;right:0;z-index:200}
.banner{padding:10px 14px;display:flex;align-items:center;gap:10px;box-shadow:0 3px 12px rgba(255,80,120,.35)}
.banner.tomorrow{background:linear-gradient(135deg,#ff85aa,#ffb3cc)}
.banner.soon{background:linear-gradient(135deg,#ff5070,#ff85aa)}
.banner-close{background:rgba(255,255,255,.25);border:none;color:#fff;border-radius:50%;width:26px;height:26px;cursor:pointer;font-weight:900;font-size:.8rem;margin-left:auto;flex-shrink:0}

/* body */
.body{padding:12px 12px 0;max-width:430px;margin:0 auto}

/* 卡片 */
.card{background:rgba(255,255,255,.82);border:2.5px solid #ffb3cc;border-radius:22px;padding:14px;margin-bottom:12px;box-shadow:3px 4px 0 #ffcce0;position:relative;overflow:hidden}
.card::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;background:linear-gradient(90deg,#ff85aa,#ffb3cc,#ffe0a0,#98e8b8)}
.sec-lbl{font-weight:900;color:#d4547a;font-size:.78rem;letter-spacing:.12em;margin-bottom:10px;display:flex;align-items:center;gap:6px}

/* 導航按鈕 */
.nav-btn{width:33px;height:33px;border-radius:50%;border:2px solid #ffaacc;background:linear-gradient(135deg,#fff0f5,#ffe0ef);color:#d4547a;font-weight:900;font-size:1.05rem;cursor:pointer;display:flex;align-items:center;justify-content:center;flex-shrink:0}

/* 週日期列 */
.days-row{display:grid;gap:4px;margin-bottom:12px}
.day-pill{background:rgba(255,255,255,.65);border:2px solid #ffd0e4;border-radius:13px;padding:6px 2px;text-align:center;cursor:pointer}
.day-pill.active{background:linear-gradient(160deg,#ff85aa,#ff5070);border-color:#ff6090}
.day-pill.today-border{border-color:#ff6090}
.day-pill.red-day .day-num{color:#d4547a}
.day-pill.active .day-wd,.day-pill.active .day-num{color:#fff}
.day-wd{font-size:.55rem;font-weight:700;color:#e87fa0;margin-bottom:2px}
.day-num{font-size:.8rem;font-weight:900;color:#b08a9a}
.day-red-dot{font-size:.45rem;color:#ff5060}
.day-pill.active .day-red-dot{color:#ffe0ec}

/* 休假大字 */
.off-day{text-align:center;padding:22px 10px}
.off-kanji{font-size:5.5rem;font-weight:900;color:#ffb3cc;-webkit-text-stroke:2.5px #ff85aa;text-shadow:4px 4px 0 #ff85aa;line-height:1;display:block}
.off-sub{font-weight:700;color:#e87fa0;margin-top:8px;font-size:.85rem;letter-spacing:.1em}
.off-emoji{font-size:1.8rem;margin-top:8px}

/* 球球班次卡 */
.shift-main{text-align:center;background:linear-gradient(135deg,rgba(255,200,220,.3),rgba(255,230,240,.25));border-radius:18px;padding:13px;margin-bottom:11px;border:2px dashed #ffb3cc}
.shift-label{font-size:.63rem;font-weight:900;color:#e87fa0;letter-spacing:.18em;margin-bottom:3px}
.shift-name{font-size:1.2rem;font-weight:900;color:#d4547a}
.shift-time{font-size:1.9rem;font-weight:900;color:#c03060;margin:4px 0;letter-spacing:.04em}
.shift-sub{font-size:.7rem;color:#e87fa0;font-weight:700}

/* 資訊格 */
.info-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.info-box{border-radius:15px;padding:10px;text-align:center;border:2px solid #ffd4e4;background:rgba(255,255,255,.65)}
.info-box.has-early{background:rgba(200,255,220,.4);border-color:#a8e6cf}
.info-box.no-early{background:rgba(255,210,210,.3);border-color:#ffc0c0}
.info-box.full{grid-column:1/-1}
.info-icon{font-size:1.1rem;margin-bottom:3px}
.info-lbl{font-size:.58rem;font-weight:900;color:#e87fa0;margin-bottom:3px}
.info-val{font-size:.72rem;font-weight:700;color:#c03060}
.info-val.empty{color:#ffc0d0}

/* 全日班表 */
.shift-row{display:flex;align-items:center;gap:9px;background:rgba(255,255,255,.65);border:1.5px solid #ffd4e4;border-radius:13px;padding:8px 11px;margin-bottom:5px}
.shift-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0}
.shift-slot{font-size:.65rem;font-weight:700;color:#e87fa0;min-width:52px}
.shift-people{font-size:.75rem;font-weight:700;color:#c03060;flex:1;line-height:1.5}
.me-tag{background:rgba(255,100,150,.15);border-radius:7px;padding:0 5px;color:#ff3060;font-weight:900}

/* 日曆 */
.cal-nav{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px}
.cal-title{font-weight:900;color:#d4547a;font-size:1rem}
.cal-dow{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;text-align:center;margin-bottom:3px}
.cal-dow-item{font-size:.58rem;font-weight:700;color:#e87fa0;padding:2px 0}
.cal-dow-item.weekend{color:#ff7090}
.cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:2px}
.cal-cell{border-radius:9px;padding:4px 1px;text-align:center;background:rgba(255,255,255,.5);border:2px solid #ffd8e8}
.cal-cell.today{background:linear-gradient(135deg,#ff85aa,#ff5070);border-color:#ff3060}
.cal-cell.holiday{background:rgba(255,200,200,.22)}
.cal-cell.red-border{border-color:#ffaacc}
.cal-cell-num{font-size:.74rem;font-weight:700;color:#b08a9a}
.cal-cell.today .cal-cell-num{color:#fff}
.cal-cell.red-border .cal-cell-num,.cal-cell.holiday .cal-cell-num{color:#d4547a}
.cal-cell-icon{font-size:.5rem}
.cal-cell-work{font-size:.48rem;color:#ff85aa}
.cal-cell.today .cal-cell-work{color:#fff}
.cal-cell-dot{width:4px;height:4px;border-radius:50%;background:#7ecfa0;margin:1px auto 0}
.cal-cell.today .cal-cell-dot{background:#fff}
.cal-legend{display:flex;gap:10px;flex-wrap:wrap;margin-top:8px;font-size:.6rem;font-weight:700}

/* 節假日列表 */
.holiday-row{display:flex;align-items:center;gap:10px;background:rgba(255,200,200,.15);border:1.5px solid #ffcccc;border-radius:13px;padding:8px 11px;margin-bottom:5px}
.holiday-icon{font-size:1.2rem}
.holiday-date{font-weight:700;color:#d4547a;font-size:.8rem}
.holiday-name{font-size:.7rem;color:#e87fa0}

/* 靈性卡 */
.spirit-card{background:linear-gradient(135deg,rgba(245,210,255,.3),rgba(220,200,255,.2))}
.akashic-card{background:linear-gradient(135deg,rgba(255,235,200,.25),rgba(255,210,230,.2))}
.spirit-section{background:rgba(255,255,255,.7);border:1.5px solid rgba(200,160,255,.45);border-radius:14px;padding:10px 12px;margin-bottom:8px}
.spirit-title{font-weight:900;color:#7030b0;font-size:.75rem;margin-bottom:3px}
.spirit-content{font-size:.8rem;color:#5a2080;line-height:1.85;white-space:pre-wrap}
.akashic-section{border-radius:16px;padding:12px 13px;margin-bottom:9px}
.akashic-section:nth-child(1){background:linear-gradient(135deg,rgba(255,240,200,.65),rgba(255,225,175,.5));border:1.5px solid #ffd080}
.akashic-section:nth-child(2){background:linear-gradient(135deg,rgba(255,220,235,.65),rgba(255,200,220,.5));border:1.5px solid #ffaacc}
.akashic-section:nth-child(3){background:linear-gradient(135deg,rgba(210,205,255,.6),rgba(230,210,255,.45));border:1.5px solid #c0a0ff}
.akashic-title{font-weight:900;font-size:.75rem;margin-bottom:5px}
.akashic-section:nth-child(1) .akashic-title{color:#8a5000}
.akashic-section:nth-child(2) .akashic-title{color:#b02060}
.akashic-section:nth-child(3) .akashic-title{color:#4020a0}
.akashic-content{font-size:.8rem;color:#5a3060;line-height:1.88;white-space:pre-wrap}
.spirit-btn{width:100%;margin-top:10px;padding:10px;background:linear-gradient(135deg,#c090ff,#e060b0);border:2px solid #c080e0;border-radius:13px;color:#fff;font-family:inherit;font-weight:700;font-size:.78rem;cursor:pointer;box-shadow:2px 3px 0 #9050c0}
.spirit-btn:disabled{background:rgba(200,180,220,.5);box-shadow:none;cursor:not-allowed}
.spirit-loading{text-align:center;padding:18px;color:#8040c0}
.spirit-error{font-size:.72rem;color:#e87fa0;text-align:center;margin-bottom:6px}
.spin{display:inline-block;font-size:2rem;animation:spin 1.2s linear infinite}

/* 行程 */
.event-row{display:flex;align-items:center;gap:9px;border-radius:13px;padding:9px 12px;margin-bottom:6px;border:1.5px solid #ffd4e4;background:rgba(255,255,255,.65)}
.event-row.done-row{background:rgba(180,240,200,.25);border-color:#80d0a0}
.event-row.today-row{background:rgba(255,200,220,.25);border-color:#ff85aa}
.event-row.past-row{opacity:.6}
.check-circle{width:22px;height:22px;border-radius:50%;border:2.5px solid #ffb3cc;background:transparent;display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0;transition:all .2s}
.check-circle.checked{background:linear-gradient(135deg,#80d0a0,#50b070);border-color:#60c080}
.check-mark{color:#fff;font-size:.7rem;font-weight:900}
.event-title{font-weight:700;font-size:.8rem;color:#c03060}
.event-title.done-title{text-decoration:line-through;color:#60a070}
.event-title.past-title{color:#b09090}
.event-meta{font-size:.62rem;color:#e87fa0}
.del-btn{background:none;border:none;color:#ffaacc;cursor:pointer;font-weight:900;font-size:.95rem}
.inp{width:100%;padding:9px 11px;border:2px solid #ffb3cc;border-radius:12px;font-family:inherit;font-size:.82rem;color:#c03060;background:#fff9fb;outline:none}
.inp-lbl{font-size:.63rem;font-weight:700;color:#e87fa0;margin-bottom:3px}
.inp-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:9px}
.add-btn{width:100%;padding:11px;background:linear-gradient(135deg,#ff85aa,#ff5070);color:#fff;border:none;border-radius:14px;font-family:inherit;font-weight:900;font-size:.88rem;cursor:pointer;box-shadow:2px 3px 0 #cc3060}
.empty-hint{text-align:center;color:#ffc0d0;font-size:.82rem;padding:14px}
.tomorrow-card{background:linear-gradient(135deg,rgba(255,185,205,.28),rgba(255,215,230,.22));border:2.5px solid #ff85aa}

/* Tab Bar */
.tabbar{display:flex;justify-content:space-around;background:rgba(255,255,255,.92);border-top:3px dashed #ffb3cc;position:fixed;bottom:0;left:0;right:0;padding:7px 0;z-index:50}
.tab-btn{flex:1;text-align:center;padding:6px 4px;border-radius:14px;border:2px solid transparent;background:none;font-family:inherit;font-size:.7rem;font-weight:700;color:#e87fa0;cursor:pointer}
.tab-btn.active{background:linear-gradient(135deg,#ff85aa,#ff5070);border-color:#ff3060;color:#fff}
.tab-icon{font-size:1.05rem}

@keyframes spin{to{transform:rotate(360deg)}}
@media(max-width:380px){.shift-time{font-size:1.5rem}}
</style>
</head>
<body>

<div id="banners"></div>
<div class="hdr" id="hdr">
  <div>
    <div class="hdr-logo">🌸 球球の魔法日曆</div>
    <div class="hdr-date" id="hdr-date"></div>
  </div>
  <div style="font-size:1.4rem">🐾</div>
</div>

<div class="body">
  <div id="tab-schedule"></div>
  <div id="tab-calendar" style="display:none"></div>
  <div id="tab-planner" style="display:none"></div>
</div>

<div class="tabbar">
  <button class="tab-btn active" onclick="switchTab('schedule')" id="tb-schedule"><div class="tab-icon">🐾</div>班表</button>
  <button class="tab-btn" onclick="switchTab('calendar')" id="tb-calendar"><div class="tab-icon">🌸</div>日曆</button>
  <button class="tab-btn" onclick="switchTab('planner')" id="tb-planner"><div class="tab-icon">📝</div>行程</button>
</div>

<script>
// ══ 填入你的 Anthropic API Key ══
const ANTHROPIC_API_KEY = "在這裡填入你的API_KEY";

// ══ 班表資料 ══
const RAW = [
  {date:"2025-03-01",凌晨:"球球 0000-1000",早上:"曾姐 7-11",下午:"Tien Yun 1-5",晚上:"韶芸 1800-2400",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-02",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"小茹 1500-2100\nYing",晚上:"萱汶\n小茹 1500-2100\n群翔",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-03",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"球球 1200-1600\n小茹",晚上:"韶芸 1800-2400\n阿瑛 1800-2200",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-04",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"阿瑛 1300-1800\n球球 1200-1800",晚上:"韶芸 1800-2400\n小茹",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-05",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"Ying 1300-1900\n球球 1200-1600",晚上:"阿瑛 1800-2300\n萱汶",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-06",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"Ying 1200-1800\n群翔 1200-1700",晚上:"韶芸 1800-2400\n小茹",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-07",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"萱汶\n群翔",晚上:"韶芸 2100-0300\n小茹",過夜:""},
  {date:"2025-03-08",凌晨:"曾姐 6-10",早上:"蔡小習 1000-1400\nEmily 9-12",下午:"Tien Yun 1-5\n淑珍",晚上:"韶芸 1800-2400",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-09",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"Ying 1200-1800\n球球 1200-1600",晚上:"小茹\n萱汶",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-10",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"小茹\n書書 12-4",晚上:"韶芸 1800-2400\n阿瑛 1800-2200",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-11",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"阿瑛 1300-1800\n球球 1200-1800",晚上:"韶芸 1800-2400\n萱汶",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-12",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"Ying 1300-1900\n球球 1200-1600",晚上:"阿瑛 1800-2300\n群翔 1800-2300",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-13",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"Ying 1200-1800",晚上:"韶芸 1800-2400\n小茹",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-14",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"婉寧 12-5\nYing",晚上:"韶芸 1800-2400\n群翔 1800-2300",過夜:"采淳 0200-0900"},
  {date:"2025-03-15",凌晨:"采淳 0200-0900",早上:"Emily 9-12",下午:"Tien Yun 1-5\n淑珍",晚上:"韶芸 1800-2400",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-16",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"球球 1200-1600\n群翔 1200-1700",晚上:"小茹\n書書 1700-2000",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-17",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"Ying 1200-1800\n小茹 1400-2000",晚上:"阿瑛 1800-2200\n小茹 1400-2000",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-18",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"阿瑛 1300-1800\n球球 1200-1800",晚上:"韶芸 1800-2400\n群翔 1800-2300",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-19",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"Ying 1300-1900\n球球 1200-1600",晚上:"阿瑛 1800-2300\n萱汶",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-20",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200\nEmily 9-12",下午:"Ying 1200-1800\n球球 1200-1800",晚上:"韶芸 1800-2400\n新人",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-21",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"萱汶 12-21\n婉寧 12-5",晚上:"韶芸 2100-0300\n書書",過夜:""},
  {date:"2025-03-22",凌晨:"Emily 0600-1000",早上:"蔡小習 1000-1400",下午:"Tien Yun 1-5\n淑珍",晚上:"韶芸 1800-2400",過夜:""},
  {date:"2025-03-23",凌晨:"曾姐 6-10",早上:"",下午:"Ying 1200-1800\n球球 1200-1600",晚上:"萱汶\n群翔 1800-2300",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-24",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"Ying",晚上:"韶芸 1800-2400\n阿瑛 1800-2200",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-25",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"阿瑛 1300-1800\n球球 1200-1800",晚上:"韶芸 1800-2400",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-26",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"球球 1200-1600\nYing",晚上:"阿瑛 1800-2300\n萱汶",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-27",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"Ying 1200-1800\n球球 1200-1800",晚上:"韶芸 1800-2400\n群翔 1800-2300",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-28",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"萱汶 1300-2100\n群翔 1200-1800",晚上:"萱汶 1300-2100\n韶芸 2100-0300",過夜:""},
  {date:"2025-03-29",凌晨:"曾姐 6-10",早上:"蔡小習 1000-1400\nEmily 9-12",下午:"Tien Yun 1-5\n淑珍",晚上:"韶芸 1800-2400\n群翔 1800-2300",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-30",凌晨:"采淳 ~0200-1200",早上:"采淳 ~0200-1200",下午:"Ying 1200-1800\n球球 1200-1600",晚上:"萱汶\n群翔 1800-2300",過夜:"采淳 ~0200-1200"},
  {date:"2025-03-31",凌晨:"采淳 ~0200-1200\n曾姐 6-10",早上:"采淳 ~0200-1200",下午:"Ying 1200-1800\n群翔 1200-1700",晚上:"韶芸 1800-2400\n阿瑛 1800-2200",過夜:"采淳 ~0200-1200"},
];
const DATA = {};
RAW.forEach(r => DATA[r.date] = r);

// ══ 週清單（直接照試算表定義）══
const WEEKS = [
  {label:"3/1（週日）",          dates:["2025-03-01"]},
  {label:"3/2（一）～ 3/8（日）", dates:["2025-03-02","2025-03-03","2025-03-04","2025-03-05","2025-03-06","2025-03-07","2025-03-08"]},
  {label:"3/9（一）～ 3/15（日）",dates:["2025-03-09","2025-03-10","2025-03-11","2025-03-12","2025-03-13","2025-03-14","2025-03-15"]},
  {label:"3/16（一）～ 3/22（日）",dates:["2025-03-16","2025-03-17","2025-03-18","2025-03-19","2025-03-20","2025-03-21","2025-03-22"]},
  {label:"3/23（一）～ 3/29（日）",dates:["2025-03-23","2025-03-24","2025-03-25","2025-03-26","2025-03-27","2025-03-28","2025-03-29"]},
  {label:"3/30（一）～ 3/31（二）",dates:["2025-03-30","2025-03-31"]},
];
// 直接對照標籤（不靠 JS getDay()）
const WD_LABEL = {
  "2025-03-01":"日",
  "2025-03-02":"一","2025-03-03":"二","2025-03-04":"三","2025-03-05":"四","2025-03-06":"五","2025-03-07":"六","2025-03-08":"日",
  "2025-03-09":"一","2025-03-10":"二","2025-03-11":"三","2025-03-12":"四","2025-03-13":"五","2025-03-14":"六","2025-03-15":"日",
  "2025-03-16":"一","2025-03-17":"二","2025-03-18":"三","2025-03-19":"四","2025-03-20":"五","2025-03-21":"六","2025-03-22":"日",
  "2025-03-23":"一","2025-03-24":"二","2025-03-25":"三","2025-03-26":"四","2025-03-27":"五","2025-03-28":"六","2025-03-29":"日",
  "2025-03-30":"一","2025-03-31":"二",
};

// ══ 台灣國定假日 2025–2030 ══
const TW_RED = {
  "2025-01-01":"元旦","2025-01-27":"除夕","2025-01-28":"春節","2025-01-29":"春節","2025-01-30":"春節","2025-01-31":"春節補假",
  "2025-02-28":"和平紀念日","2025-04-03":"兒童節補假","2025-04-04":"兒童節","2025-04-05":"清明節",
  "2025-05-01":"勞動節","2025-05-30":"端午節","2025-09-28":"教師節","2025-10-10":"國慶日","2025-12-25":"行憲紀念日",
  "2026-01-01":"元旦","2026-02-15":"小年夜補假","2026-02-16":"小年夜","2026-02-17":"春節","2026-02-18":"春節","2026-02-19":"春節","2026-02-20":"春節補假","2026-02-21":"春節補假",
  "2026-02-27":"和平紀念日補假","2026-02-28":"和平紀念日",
  "2026-04-03":"兒童節補假","2026-04-04":"兒童節","2026-04-05":"清明節","2026-04-06":"清明節補假",
  "2026-05-01":"勞動節","2026-06-19":"端午節","2026-06-20":"端午節補假",
  "2026-09-25":"中秋節","2026-09-26":"中秋節補假","2026-09-27":"中秋節補假","2026-09-28":"教師節",
  "2026-10-09":"國慶補假","2026-10-10":"國慶日","2026-10-25":"台灣光復節","2026-10-26":"光復節補假",
  "2026-12-25":"行憲紀念日","2026-12-26":"行憲紀念日補假","2026-12-27":"行憲紀念日補假",
  "2027-01-01":"元旦","2027-02-17":"除夕","2027-02-18":"春節","2027-02-19":"春節","2027-02-20":"春節",
  "2027-02-28":"和平紀念日","2027-04-04":"兒童節","2027-04-05":"清明節","2027-05-01":"勞動節","2027-06-09":"端午節",
  "2027-09-15":"中秋節","2027-09-28":"教師節","2027-10-10":"國慶日","2027-10-25":"台灣光復節","2027-12-25":"行憲紀念日",
  "2028-01-01":"元旦","2028-02-05":"除夕","2028-02-06":"春節","2028-02-07":"春節","2028-02-08":"春節",
  "2028-02-28":"和平紀念日","2028-04-04":"兒童節+清明","2028-05-01":"勞動節","2028-05-28":"端午節",
  "2028-09-03":"中秋節","2028-09-28":"教師節","2028-10-10":"國慶日","2028-10-25":"台灣光復節","2028-12-25":"行憲紀念日",
  "2029-01-01":"元旦","2029-01-26":"除夕","2029-01-27":"春節","2029-01-28":"春節","2029-01-29":"春節",
  "2029-02-28":"和平紀念日","2029-04-04":"兒童節","2029-04-05":"清明節","2029-05-01":"勞動節","2029-06-16":"端午節",
  "2029-09-22":"中秋節","2029-09-28":"教師節","2029-10-10":"國慶日","2029-10-25":"台灣光復節","2029-12-25":"行憲紀念日",
  "2030-01-01":"元旦","2030-02-12":"除夕","2030-02-13":"春節","2030-02-14":"春節","2030-02-15":"春節",
  "2030-02-28":"和平紀念日","2030-04-04":"兒童節","2030-04-05":"清明節","2030-05-01":"勞動節","2030-06-05":"端午節",
  "2030-09-12":"中秋節","2030-09-28":"教師節","2030-10-10":"國慶日","2030-10-25":"台灣光復節","2030-12-25":"行憲紀念日",
};
const H_ICON = {
  "元旦":"🎊","小年夜":"🏮","小年夜補假":"🏮","除夕":"🏮","春節":"🧧","春節補假":"🧧",
  "和平紀念日":"🕊️","和平紀念日補假":"🕊️","兒童節":"🎈","兒童節補假":"🎈","兒童節+清明":"🌸",
  "清明節":"🌿","清明節補假":"🌿","勞動節":"💪","端午節":"🎋","端午節補假":"🎋",
  "中秋節":"🥮","中秋節補假":"🥮","教師節":"📚","教師節補假":"📚",
  "國慶日":"🇹🇼","國慶補假":"🇹🇼","台灣光復節":"🎌","光復節補假":"🎌","行憲紀念日":"📜","行憲紀念日補假":"📜",
};
const SHIFTS = ["凌晨","早上","下午","晚上","過夜"];
const SICON  = {凌晨:"🌙",早上:"☀️",下午:"🌤️",晚上:"🌆",過夜:"🌛"};
const SCOL   
