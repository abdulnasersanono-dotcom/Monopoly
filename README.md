<!DOCTYPE html>
<html lang="ar-de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MONOPOLY PRO</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Oswald:wght@400;600;700;900&family=Barlow+Condensed:ital,wght@0,400;0,600;0,700;1,700&display=swap" rel="stylesheet">
<style>
/* ═══════════════════ ROOT ═══════════════════ */
:root{
  --C:80px;--CW:60px;--CH:80px;--BS:700px;
  --gold:#C8A000;--gold-lt:#FFD700;
  --board-green:#C5E3B8;--border-col:#3a3a3a;
  --sidebar-bg:linear-gradient(170deg,#0e3a0e 0%,#071507 100%);
  --th-body1:#163800;--th-body2:#2a1000;--th-bodybg:#060e02;
  --th-setup1:#0e3a0e;--th-setup2:#071507;
}
body.th-gold{--th-body1:#3a2a00;--th-body2:#1a0e00;--th-bodybg:#120a00;--th-setup1:#3a2a00;--th-setup2:#1a0e00;--sidebar-bg:linear-gradient(170deg,#3a2a00,#1a0e00)}
body.th-night{--th-body1:#0a0a1a;--th-body2:#000010;--th-bodybg:#000008;--th-setup1:#0a0a1a;--th-setup2:#000010;--sidebar-bg:linear-gradient(170deg,#0a0a1a,#000010);--gold:#7799FF;--gold-lt:#aabbff}

*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;font-family:'Barlow Condensed',sans-serif}
/* Override for board so 700px = inner content area, not including border */
#board{box-sizing:content-box!important}
body{background:radial-gradient(ellipse 55% 45% at 20% 15%,var(--th-body1) 0%,transparent 65%),radial-gradient(ellipse 60% 50% at 85% 85%,var(--th-body2) 0%,transparent 60%),var(--th-bodybg);min-height:100vh;display:flex;align-items:flex-start;justify-content:center;padding:16px;overflow-x:hidden;perspective:2200px}

/* ═══════════════════ SETUP SCREEN ═══════════════════ */
#setup{max-width:820px;width:100%;color:#fff;animation:setupIn .4s ease}
@keyframes setupIn{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}

/* Header */
.setup-header{text-align:center;margin-bottom:28px}
.setup-logo{font-family:'Oswald',sans-serif;font-size:72px;font-weight:900;letter-spacing:8px;color:#fff;text-shadow:4px 4px 0 #7a0000,0 0 40px rgba(200,160,0,.3);line-height:1;display:inline-block}
.setup-edition{font-size:10px;letter-spacing:6px;color:var(--gold);text-transform:uppercase;border:1px solid rgba(200,160,0,.35);display:inline-block;padding:4px 18px;border-radius:3px;margin-top:8px}

/* Theme bar */
.setup-theme-bar{display:flex;justify-content:center;gap:8px;margin-bottom:24px}
.thbtn{padding:7px 16px;border:2px solid rgba(200,160,0,.3);background:transparent;color:#bbb;border-radius:6px;cursor:pointer;font-family:inherit;font-size:11px;letter-spacing:1px;transition:all .15s}
.thbtn.on,.thbtn:hover{border-color:var(--gold);color:var(--gold);background:rgba(200,160,0,.1)}

/* Card container */
.setup-card{background:linear-gradient(160deg,var(--th-setup1),var(--th-setup2));border:2px solid rgba(200,160,0,.3);border-radius:16px;padding:28px;margin-bottom:16px;box-shadow:0 8px 40px rgba(0,0,0,.6),inset 0 1px 0 rgba(200,160,0,.12)}
.setup-card-title{font-size:9px;text-transform:uppercase;letter-spacing:4px;color:var(--gold);margin-bottom:16px;display:flex;align-items:center;gap:10px}
.setup-card-title::after{content:'';flex:1;height:1px;background:rgba(200,160,0,.2)}
.step-badge{width:22px;height:22px;border-radius:50%;background:var(--gold);color:#000;font-weight:700;font-size:10px;display:inline-flex;align-items:center;justify-content:center;flex-shrink:0}

/* ── Player Count Selector ── */
.player-count-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:8px}
.pc-btn{
  aspect-ratio:1;border:2px solid rgba(200,160,0,.25);background:rgba(0,0,0,.35);
  color:rgba(255,255,255,.5);border-radius:12px;cursor:pointer;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  gap:4px;transition:all .2s;position:relative;overflow:hidden
}
.pc-btn .pc-num{font-family:'Oswald',sans-serif;font-size:28px;font-weight:900;line-height:1;transition:all .2s}
.pc-btn .pc-label{font-size:8px;letter-spacing:2px;text-transform:uppercase;transition:all .2s}
.pc-btn .pc-icons{font-size:10px;opacity:.5;transition:all .2s}
.pc-btn:hover{border-color:rgba(200,160,0,.6);background:rgba(200,160,0,.08);color:rgba(255,255,255,.8)}
.pc-btn.sel{border-color:var(--gold);background:rgba(200,160,0,.15);color:#fff;box-shadow:0 0 20px rgba(200,160,0,.25),inset 0 1px 0 rgba(200,160,0,.2)}
.pc-btn.sel .pc-num{color:var(--gold-lt);text-shadow:0 0 12px rgba(255,215,0,.4)}
.pc-btn.sel .pc-icons{opacity:1}
.pc-btn::before{content:'';position:absolute;inset:0;background:radial-gradient(circle at 50% 0%,rgba(200,160,0,.15),transparent 70%);opacity:0;transition:opacity .2s}
.pc-btn.sel::before{opacity:1}

/* ── Player Cards Grid ── */
.player-cards-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px}
.player-card{
  background:rgba(0,0,0,.35);border:1.5px solid rgba(200,160,0,.18);
  border-radius:12px;padding:14px;transition:border-color .2s;position:relative
}
.player-card.is-ai{border-color:rgba(0,180,255,.3);background:rgba(0,40,70,.35)}
.player-card:hover{border-color:rgba(200,160,0,.35)}
.pc-header{display:flex;align-items:center;gap:10px;margin-bottom:12px}
.pc-avatar{
  width:48px;height:48px;border-radius:50%;display:flex;align-items:center;
  justify-content:center;font-size:24px;border:2.5px solid rgba(255,255,255,.6);
  flex-shrink:0;box-shadow:0 3px 12px rgba(0,0,0,.5);transition:all .2s;cursor:pointer
}
.pc-avatar:hover{transform:scale(1.08)}
.pc-header-info{flex:1;min-width:0}
.pc-player-num{font-size:8px;letter-spacing:3px;color:rgba(200,160,0,.7);text-transform:uppercase;margin-bottom:3px}
.pc-name-input{
  width:100%;background:transparent;border:none;border-bottom:1.5px solid rgba(200,160,0,.4);
  color:#fff;font-size:15px;font-family:inherit;padding:2px 0;outline:none;
  transition:border-color .15s
}
.pc-name-input:focus{border-bottom-color:var(--gold-lt)}
.pc-name-input:disabled{color:#88ccff;border-bottom-color:rgba(0,180,255,.3)}

/* Token picker */
.pc-section-label{font-size:8px;letter-spacing:2px;color:rgba(255,255,255,.4);text-transform:uppercase;margin-bottom:6px;margin-top:10px}
.tok-grid{display:flex;gap:5px;flex-wrap:wrap}
.tok-btn{
  width:32px;height:32px;border:1.5px solid rgba(200,160,0,.2);background:rgba(0,0,0,.3);
  border-radius:6px;font-size:16px;cursor:pointer;display:flex;align-items:center;
  justify-content:center;transition:all .15s;position:relative
}
.tok-btn:hover{border-color:rgba(200,160,0,.5);background:rgba(200,160,0,.12)}
.tok-btn.sel{border-color:var(--gold);background:rgba(200,160,0,.2);box-shadow:0 0 8px rgba(200,160,0,.3)}
.tok-btn.sel::after{content:'✓';position:absolute;bottom:-3px;right:-3px;background:var(--gold);color:#000;width:11px;height:11px;border-radius:50%;font-size:7px;font-weight:900;display:flex;align-items:center;justify-content:center;line-height:11px;text-align:center}

/* Color swatches */
.color-grid{display:flex;gap:5px;flex-wrap:wrap}
.color-swatch{
  width:22px;height:22px;border-radius:50%;cursor:pointer;
  border:2px solid transparent;transition:all .15s;flex-shrink:0
}
.color-swatch:hover{transform:scale(1.2)}
.color-swatch.sel{border-color:#fff;transform:scale(1.25);box-shadow:0 0 8px rgba(255,255,255,.5)}

/* AI toggle */
.ai-btn{
  display:flex;align-items:center;gap:6px;font-size:10px;color:#88ccff;
  cursor:pointer;padding:4px 10px;border:1px solid rgba(0,180,255,.3);
  border-radius:20px;background:rgba(0,100,200,.1);transition:all .15s;margin-top:10px;
  width:fit-content
}
.ai-btn.on{background:rgba(0,150,255,.2);border-color:#00aaff;color:#00ccff}
.ai-btn::before{content:'🤖';font-size:12px}

/* ── Rules Grid ── */
.rules-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.rule-item{background:rgba(0,0,0,.28);border:1px solid rgba(200,160,0,.12);border-radius:8px;padding:10px 12px}
.rule-item label{font-size:11px;color:#ddd;display:flex;align-items:center;gap:7px;cursor:pointer}
.rule-item input[type=checkbox]{accent-color:var(--gold);width:13px;height:13px;flex-shrink:0}
.rule-item select{background:rgba(0,0,0,.4);color:#FFD700;border:1px solid rgba(200,160,0,.4);border-radius:4px;padding:4px 8px;font-family:inherit;font-size:11px;margin-top:5px;width:100%;cursor:pointer}

/* Start button */
.start-btn{
  width:100%;padding:18px;
  background:linear-gradient(180deg,#e02a00,#880000);
  border:3px solid var(--gold-lt);color:#fff;
  font-family:'Oswald',sans-serif;font-size:22px;font-weight:700;
  letter-spacing:5px;text-transform:uppercase;cursor:pointer;
  border-radius:10px;margin-top:6px;
  position:relative;overflow:hidden;
  box-shadow:0 6px 0 #5a0000,0 10px 30px rgba(160,0,0,.5),inset 0 2px 4px rgba(255,255,255,.15);
  transition:all .15s;
}
.start-btn:hover{
  filter:brightness(1.12);
  transform:translateY(-3px);
  box-shadow:0 9px 0 #5a0000,0 16px 40px rgba(160,0,0,.6),inset 0 2px 4px rgba(255,255,255,.15);
}
.start-btn:active{
  transform:translateY(5px);
  box-shadow:0 1px 0 #5a0000,0 4px 12px rgba(160,0,0,.4),inset 0 2px 4px rgba(0,0,0,.2);
}
.start-btn:hover{filter:brightness(1.15);transform:translateY(-2px);box-shadow:0 10px 32px rgba(160,0,0,.55)}
.start-btn:active{transform:translateY(0)}
.start-btn::after{content:'';position:absolute;inset:0;background:linear-gradient(90deg,transparent,rgba(255,255,255,.08),transparent);transform:translateX(-100%);transition:transform .5s}
.start-btn:hover::after{transform:translateX(100%)}

/* ═══════════════════ GAME LAYOUT ═══════════════════ */
#game{display:none;flex-direction:row;gap:16px;align-items:flex-start;max-width:1320px;width:100%}

/* ═══ BOARD ═══ */
#bwrap{
  position:relative;flex-shrink:0;
  perspective:1400px;
}
/* Dynamic drop shadow — updated by JS parallax */
#board-shadow{
  position:absolute;
  bottom:-60px; left:5%;
  width:90%; height:60px;
  background:radial-gradient(ellipse 80% 60% at 50% 0%, rgba(0,0,0,.7) 0%, transparent 100%);
  filter:blur(18px);
  pointer-events:none;
  z-index:-1;
  transition:transform .1s;
}
#board-3d-wrap{
  transform-style:preserve-3d;
  transform:rotateX(18deg) rotateZ(-1.5deg);
  display:inline-block;
  position:relative;
}

/* Board face */
#board{
  width:var(--BS);height:var(--BS);position:relative;
  background:var(--board-green);
  border:7px solid #0a2200;
  box-shadow:
    0 0 0 2px #1a4a00,
    inset 0 0 0 4px rgba(255,255,255,.07),
    inset 0 0 80px rgba(0,60,0,.15);
  overflow:hidden;
  transform-style:preserve-3d;
}

/* 3D thick edge - bottom */
#board-3d-wrap::after{
  content:'';
  position:absolute;
  left:0;right:0;
  bottom:-22px;
  height:22px;
  background:linear-gradient(180deg,#061a03 0%,#030e01 100%);
  transform:rotateX(-90deg);
  transform-origin:top center;
  border:7px solid #030e01;
  border-top:none;
  box-shadow:0 8px 30px rgba(0,0,0,.8);
}
/* 3D thick edge - right */
#board-3d-wrap::before{
  content:'';
  position:absolute;
  top:0;bottom:-14px;
  right:-22px;
  width:22px;
  background:linear-gradient(90deg,#082005 0%,#040f02 100%);
  transform:rotateY(90deg);
  transform-origin:left center;
  border:7px solid #030e01;
  border-left:none;
}

/* Ambient light shimmer on board surface */
#board::before{content:'';position:absolute;inset:7px;border:2px solid rgba(0,90,0,.3);pointer-events:none;z-index:0}
#board::after{
  content:'';position:absolute;inset:0;
  background:
    linear-gradient(135deg,rgba(255,255,255,.08) 0%,transparent 50%,rgba(0,0,0,.04) 100%),
    repeating-linear-gradient(0deg,transparent,transparent 59px,rgba(0,0,0,.03) 59px,rgba(0,0,0,.03) 60px),
    repeating-linear-gradient(90deg,transparent,transparent 59px,rgba(0,0,0,.03) 59px,rgba(0,0,0,.03) 60px);
  pointer-events:none;z-index:0
}

/* ── Token landing cell highlight ── */
.cell.tok-landing{
  z-index:9!important;
  animation:cellPulse .3s ease-out forwards;
}
@keyframes cellPulse{
  0%  {box-shadow:0 0 0 0 rgba(255,215,0,0);filter:brightness(1)}
  40% {box-shadow:0 0 0 5px rgba(255,215,0,.9),0 0 40px rgba(255,215,0,.7);filter:brightness(1.3)}
  100%{box-shadow:0 0 0 2px rgba(255,215,0,.5),0 0 16px rgba(255,215,0,.3);filter:brightness(1.08)}
}
/* Zoom lens ring on the moving token */
.anim-tok.tracking{
  transition:width .15s,height .15s,font-size .15s,box-shadow .15s,left .22s cubic-bezier(.4,0,.2,1),top .22s cubic-bezier(.4,0,.2,1)!important;
  width:36px!important;height:36px!important;font-size:19px!important;
  z-index:50!important;
  box-shadow:
    0 0 0 3px rgba(255,215,0,.95),
    0 0 22px rgba(255,215,0,.7),
    0 6px 18px rgba(0,0,0,.8),
    inset 0 2px 5px rgba(255,255,255,.45)!important;
}
.corner{
  position:absolute;width:var(--C);height:var(--C);
  border:1.5px solid var(--border-col);
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  text-align:center;z-index:4;cursor:pointer;overflow:hidden;
  transition:filter .15s,transform .15s,box-shadow .15s;
  transform-style:preserve-3d;
}
.corner:hover{
  filter:brightness(1.08);
  box-shadow:0 0 0 2px rgba(200,160,0,.6),0 6px 20px rgba(0,0,0,.5);
  z-index:10;
}
#c0{right:0;bottom:0;background:linear-gradient(135deg,#d4edda,#b8dfc0)}
.go-arrow{font-size:26px;line-height:1;margin-bottom:2px}
.go-los{font-family:'Oswald',sans-serif;font-size:22px;font-weight:900;color:#cc0000;letter-spacing:2px;line-height:1}
.go-sub{font-size:6.5px;font-weight:700;color:#006600;letter-spacing:.3px;margin-top:3px}
.go-amt{font-size:10px;font-weight:700;color:#006600}
#c10{left:0;bottom:0;background:#d4edda;padding:0}
.jail-wrap{width:100%;height:100%;position:relative;overflow:hidden}
.jail-stripe{position:absolute;inset:0;background:linear-gradient(135deg,#f9d71c 0%,#f9d71c 50%,#d4edda 50%,#d4edda 100%)}
.jail-box{position:absolute;right:5px;bottom:5px;width:52px;height:52px;background:#f9d71c;border:3px solid #2a2a2a;border-radius:3px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:1px}
.jail-bars{font-size:20px;line-height:1}
.jail-txt{font-size:5.5px;font-weight:700;color:#1a1a1a;letter-spacing:.4px;text-transform:uppercase;line-height:1.3;text-align:center}
.visit-lbl{position:absolute;left:3px;top:16px;font-size:6px;font-weight:700;color:#444;transform:rotate(-45deg);white-space:nowrap;letter-spacing:.3px}
#c20{left:0;top:0;background:linear-gradient(135deg,#d4edda,#b8dfc0)}
.fp-icon{font-size:28px;line-height:1}
.fp-txt{font-family:'Oswald',sans-serif;font-size:10px;font-weight:700;color:#1a1a1a;letter-spacing:.8px;line-height:1.4;margin-top:4px}
#c30{right:0;top:0;background:linear-gradient(135deg,#ffd4d4,#ffb8b8)}
.gtj-deco{position:absolute;inset:0;background:repeating-linear-gradient(45deg,transparent,transparent 7px,rgba(180,0,0,.07) 7px,rgba(180,0,0,.07) 14px)}
.gtj-inner{position:relative;z-index:1;display:flex;flex-direction:column;align-items:center;gap:3px}
.gtj-icon{font-size:24px;line-height:1}
.gtj-txt{font-family:'Oswald',sans-serif;font-size:8px;font-weight:700;color:#cc0000;letter-spacing:.6px;line-height:1.4;text-align:center}

/* Cells */
.cell{
  position:absolute;border:1.5px solid var(--border-col);
  overflow:hidden;cursor:pointer;z-index:1;
  transition:filter .12s,box-shadow .15s,transform .15s,z-index 0s;
  transform-origin:center center;
  transform-style:preserve-3d;
}
.cell:hover{
  filter:brightness(1.06);
  z-index:8;
  box-shadow:
    inset 0 0 0 2px rgba(200,160,0,.7),
    0 0 20px rgba(200,160,0,.25),
    0 8px 24px rgba(0,0,0,.5);
  transform:translateZ(6px) scale(1.04);
}

/* ── Cell inner layout ──
   Strategy: no CSS rotation. Each side uses flex to place the color band
   on the edge TOWARD the board center.
   B (bottom row)  → band at TOP    → flex column
   T (top row)     → band at BOTTOM → flex column-reverse
   L (left col)    → band at RIGHT  → flex row-reverse
   R (right col)   → band at LEFT   → flex row
*/
.ci{position:absolute;inset:0;display:flex;overflow:hidden}
.ci-B{flex-direction:column}
.ci-T{flex-direction:column-reverse}
.ci-L{flex-direction:row-reverse}
.ci-R{flex-direction:row}

/* Color band — raised 3D look */
.cband{flex-shrink:0;position:relative}
.ci-B .cband,.ci-T .cband{
  width:100%;height:20px;
  border-bottom:1.5px solid rgba(0,0,0,.3);
  box-shadow:inset 0 -3px 6px rgba(0,0,0,.2),inset 0 3px 6px rgba(255,255,255,.18);
}
.ci-L .cband,.ci-R .cband{
  height:100%;width:20px;
  border-right:1.5px solid rgba(0,0,0,.3);
  box-shadow:inset -3px 0 6px rgba(0,0,0,.2),inset 3px 0 6px rgba(255,255,255,.18);
}

/* Cell body */
.cbody{
  flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;
  padding:3px 2px;text-align:center;gap:1px;
  background:#f2faee;
  min-width:0;min-height:0;overflow:hidden;
  /* subtle inner light from center direction */
  box-shadow:inset 0 0 8px rgba(255,255,255,.4);
}
.cell.ch-cell .cbody{background:#fffbe8}
.cell.cm-cell .cbody{background:#edf4ff}
.cell.tx-cell .cbody{background:#fff0f0}
.cell.rl-cell .cbody{background:#f5f5ec}
.cell.ut-cell .cbody{background:#edfaff}

/* Text in L/R cells needs rotation to be readable (rotated around cell body center) */
.ci-L .cbody{writing-mode:vertical-lr;transform:rotate(180deg)}
.ci-R .cbody{writing-mode:vertical-lr}

.cname{font-size:5.6px;font-weight:700;text-transform:uppercase;letter-spacing:.12px;line-height:1.28;color:#111;word-break:break-word;hyphens:auto}
.cprice{font-size:5.2px;font-weight:700;color:#333;margin-top:1px}
.cicon{font-size:12px;line-height:1;margin-bottom:1px}
.own-dot{position:absolute;top:2px;left:2px;width:9px;height:9px;border-radius:50%;border:1.5px solid rgba(0,0,0,.45);z-index:5;display:none}
.hrow{position:absolute;top:2px;right:2px;display:flex;gap:1px;z-index:5;flex-wrap:wrap;max-width:28px}
.hdot{width:7px;height:7px;background:#1aaa1a;border:1px solid #087a08;border-radius:1px}
.hhotel{width:10px;height:9px;background:#cc2200;border:1px solid #880000;border-radius:1px}
.mortg-ov{position:absolute;inset:0;background:rgba(20,20,20,.45);display:none;align-items:center;justify-content:center;z-index:6}
.mortg-ov span{font-size:5px;color:#FFD700;font-weight:700;letter-spacing:.5px;transform:rotate(-25deg);display:block}
.tok-wrap{position:absolute;bottom:2px;left:50%;transform:translateX(-50%);display:flex;flex-wrap:wrap;gap:1px;z-index:20;justify-content:center;pointer-events:none;max-width:56px}
.anim-tok{
  position:absolute;width:30px;height:30px;border-radius:50%;
  display:flex;align-items:center;justify-content:center;font-size:16px;
  border:3px solid rgba(255,255,255,.95);
  box-shadow:
    0 4px 12px rgba(0,0,0,.7),
    0 2px 0 rgba(0,0,0,.4),
    inset 0 2px 4px rgba(255,255,255,.35);
  z-index:30;pointer-events:none;
  transition:left .22s cubic-bezier(.4,0,.2,1),top .22s cubic-bezier(.4,0,.2,1);
  transform-style:preserve-3d;
}
.anim-tok.tok-jump{animation:tokBounce3d .30s ease-out}
@keyframes tokBounce3d{
  0%  {transform:scale(1)   translateY(0)   translateZ(0)}
  25% {transform:scale(1.3) translateY(-18px) translateZ(20px)}
  55% {transform:scale(1.5) translateY(-28px) translateZ(30px)}
  75% {transform:scale(.92) translateY(3px)  translateZ(-4px)}
  90% {transform:scale(1.05) translateY(-4px) translateZ(4px)}
  100%{transform:scale(1)   translateY(0)   translateZ(0)}
}
.btok{width:15px;height:15px;border-radius:50%;border:2px solid rgba(255,255,255,.9);display:flex;align-items:center;justify-content:center;font-size:7.5px;box-shadow:0 1px 5px rgba(0,0,0,.6);flex-shrink:0}
.btok.jailed{border-color:#f00!important;box-shadow:0 0 6px rgba(255,0,0,.8)!important}
.anim-tok.ai-player{animation:aiPulse 2s ease-in-out infinite}
@keyframes aiPulse{0%,100%{box-shadow:0 3px 10px rgba(0,0,0,.6),0 0 8px rgba(0,180,255,.4)}50%{box-shadow:0 3px 10px rgba(0,0,0,.6),0 0 20px rgba(0,180,255,.8)}}

/* Board center */
#bcenter{position:absolute;left:calc(var(--C) + 1.5px);top:calc(var(--C) + 1.5px);width:calc(var(--BS) - 2*var(--C) - 3px);height:calc(var(--BS) - 2*var(--C) - 3px);display:flex;flex-direction:column;align-items:center;justify-content:space-around;z-index:1;padding:20px 16px}
#bcenter::before{content:'';position:absolute;inset:0;background:radial-gradient(ellipse 75% 55% at 50% 50%,rgba(144,210,144,.35) 0%,transparent 75%);pointer-events:none}
.deck-row{display:flex;justify-content:space-around;width:100%;position:relative;z-index:2}
.deck{
  width:90px;height:124px;border-radius:12px;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  font-family:'Oswald',sans-serif;font-size:9.5px;font-weight:600;letter-spacing:1.2px;
  text-align:center;line-height:1.5;color:#fff;padding:10px 7px;
  border:2.5px solid rgba(255,255,255,.3);
  transform-style:preserve-3d;
  transition:transform .2s,box-shadow .2s;
  position:relative;
}
.deck:hover{transform:rotate(0deg) translateZ(12px) scale(1.06)!important;box-shadow:0 20px 50px rgba(0,0,0,.6)!important}
.deck-ev{
  background:linear-gradient(150deg,#f08020,#b84800);
  box-shadow:4px 5px 0 #7a2a00,6px 7px 0 #5a1e00,0 12px 30px rgba(0,0,0,.5);
  transform:rotate(7deg);
}
.deck-ev::before{
  content:'';position:absolute;inset:3px;border-radius:9px;
  background:linear-gradient(135deg,rgba(255,255,255,.2) 0%,transparent 50%);
  pointer-events:none;
}
.deck-gem{
  background:linear-gradient(150deg,#2268c0,#093070);
  box-shadow:4px 5px 0 #062050,6px 7px 0 #041030,0 12px 30px rgba(0,0,0,.5);
  transform:rotate(-6deg);
}
.deck-gem::before{
  content:'';position:absolute;inset:3px;border-radius:9px;
  background:linear-gradient(135deg,rgba(255,255,255,.18) 0%,transparent 50%);
  pointer-events:none;
}
.deck-ico{font-size:32px;display:block;margin-bottom:6px}
.center-title{position:relative;z-index:2;text-align:center}
.mono-logo{
  font-family:'Oswald',sans-serif;font-size:52px;font-weight:900;
  color:#CC0000;letter-spacing:6px;
  background:#fff;padding:5px 22px 4px;
  border:3.5px solid #CC0000;border-radius:5px;display:inline-block;line-height:1.15;
  box-shadow:
    5px 5px 0 #880000,
    8px 8px 0 #660000,
    0 12px 30px rgba(0,0,0,.3),
    inset 0 2px 4px rgba(255,255,255,.9);
  text-shadow:2px 2px 0 rgba(180,0,0,.3);
  transform:perspective(300px) rotateX(4deg);
  transition:transform .2s,box-shadow .2s;
}
.mono-logo:hover{
  transform:perspective(300px) rotateX(0deg) scale(1.03);
  box-shadow:6px 6px 0 #880000,10px 10px 0 #660000,0 16px 40px rgba(0,0,0,.4),inset 0 2px 4px rgba(255,255,255,.9);
}
.mono-sub{font-size:10px;color:#445;letter-spacing:5px;text-transform:uppercase;margin-top:6px;font-weight:600}
#dice-sec{position:relative;z-index:2;display:flex;flex-direction:column;align-items:center;gap:6px}
#dice-row{display:flex;gap:18px;align-items:center}
.die{
  width:60px;height:60px;
  background:linear-gradient(145deg,#fffffc,#e8e8e0);
  border:3px solid #1a1a1a;border-radius:14px;
  display:flex;align-items:center;justify-content:center;font-size:38px;
  box-shadow:
    4px 6px 0 #666,
    6px 8px 0 #444,
    0 12px 28px rgba(0,0,0,.4),
    inset 0 2px 4px rgba(255,255,255,.8),
    inset 0 -2px 4px rgba(0,0,0,.1);
  user-select:none;cursor:pointer;
  transition:transform .07s,box-shadow .07s;
  transform-style:preserve-3d;
}
.die:hover{
  transform:translateY(-3px) rotateZ(-3deg);
  box-shadow:6px 10px 0 #555,8px 12px 0 #333,0 18px 36px rgba(0,0,0,.5),inset 0 2px 4px rgba(255,255,255,.8);
}
.die:active{
  transform:translateY(3px);
  box-shadow:1px 2px 0 #888,0 4px 10px rgba(0,0,0,.3),inset 0 2px 4px rgba(255,255,255,.6);
}
.die.rolling{animation:spindie .75s cubic-bezier(.22,.61,.36,1)}
@keyframes spindie{0%{transform:rotate(0)scale(1)}20%{transform:rotate(60deg)scale(1.35)}42%{transform:rotate(-38deg)scale(.86)}62%{transform:rotate(24deg)scale(1.13)}80%{transform:rotate(-10deg)scale(1.05)}100%{transform:rotate(0)scale(1)}}
#dice-info{font-size:11px;color:#445;font-weight:700;letter-spacing:.5px;background:rgba(255,255,255,.6);padding:3px 10px;border-radius:3px}
#dice-result-big{font-family:'Oswald',sans-serif;font-size:14px;font-weight:700;color:var(--gold);letter-spacing:2px;text-align:center;min-height:18px}
#dice-double-banner{display:none;background:rgba(200,160,0,.18);border:1.5px solid rgba(200,160,0,.55);border-radius:5px;padding:4px 14px;font-family:'Oswald',sans-serif;font-size:10px;font-weight:700;color:#FFD700;letter-spacing:2px;text-align:center}
#dice-double-banner.on{display:block;animation:doublePop .35s cubic-bezier(.34,1.4,.64,1)}
@keyframes doublePop{from{transform:scale(.6);opacity:0}to{transform:scale(1);opacity:1}}

/* ═══ SIDEBAR ═══ */
#sidebar{flex:1;min-width:250px;max-width:290px;display:flex;flex-direction:column;gap:10px}
.sc{
  background:var(--sidebar-bg);border:2px solid var(--gold);border-radius:10px;padding:13px;color:#fff;
  box-shadow:
    0 4px 22px rgba(0,0,0,.45),
    inset 0 1px 0 rgba(200,160,0,.1),
    4px 4px 0 rgba(0,0,0,.3),
    6px 6px 0 rgba(0,0,0,.15);
  transition:transform .15s,box-shadow .15s;
  position:relative;
}
.sc::before{
  content:'';position:absolute;inset:0;border-radius:10px;
  background:linear-gradient(135deg,rgba(200,160,0,.06) 0%,transparent 50%);
  pointer-events:none;
}
.sc-title{font-size:9px;text-transform:uppercase;letter-spacing:3px;color:var(--gold);border-bottom:1px solid rgba(200,160,0,.28);padding-bottom:5px;margin-bottom:12px;font-weight:700}
.cp-row{display:flex;align-items:center;gap:12px;margin-bottom:10px}
.cp-tok{width:48px;height:48px;border-radius:50%;border:3px solid rgba(255,255,255,.85);display:flex;align-items:center;justify-content:center;font-size:24px;flex-shrink:0;box-shadow:0 3px 12px rgba(0,0,0,.55);animation:cpGlow 2.5s ease-in-out infinite alternate}
@keyframes cpGlow{from{box-shadow:0 0 0 3px rgba(200,160,0,.2),0 3px 12px rgba(0,0,0,.55)}to{box-shadow:0 0 0 5px rgba(200,160,0,.45),0 6px 20px rgba(0,0,0,.7)}}
.cp-name{font-size:17px;font-weight:700;line-height:1.2}
.cp-money{font-size:16px;color:#FFD700;font-weight:700;margin-top:2px}
.cp-wealth{font-size:9px;color:#888;margin-top:1px}
.cp-pos{font-size:10px;color:#999;margin-top:2px}
#turn-timer{display:flex;align-items:center;gap:8px;margin-bottom:10px;background:rgba(0,0,0,.3);border-radius:6px;padding:7px 10px;border:1px solid rgba(200,160,0,.15)}
.timer-bar-wrap{flex:1;height:6px;background:rgba(255,255,255,.12);border-radius:3px;overflow:hidden}
#timer-bar{height:100%;background:linear-gradient(90deg,#00cc44,#FFD700);border-radius:3px;transition:width .9s linear,background .5s}
#timer-bar.warn{background:linear-gradient(90deg,#ff8800,#ff2200)}
#timer-txt{font-size:13px;font-weight:700;color:#FFD700;min-width:26px;text-align:right;font-family:'Oswald',sans-serif}
.acts{display:flex;flex-direction:column;gap:6px}
.btn{
  padding:9px 14px;border:2px solid;border-radius:6px;
  font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:1px;
  cursor:pointer;transition:all .12s;font-family:inherit;width:100%;
  position:relative;
  box-shadow:0 4px 0 rgba(0,0,0,.4),0 6px 12px rgba(0,0,0,.25);
}
.btn:hover:not(:disabled){transform:translateY(-2px);box-shadow:0 6px 0 rgba(0,0,0,.4),0 10px 18px rgba(0,0,0,.3)}
.btn:active:not(:disabled){transform:translateY(3px);box-shadow:0 1px 0 rgba(0,0,0,.4),0 2px 6px rgba(0,0,0,.2)}
.b-roll{background:linear-gradient(180deg,#e02a00,#8a0000);border-color:#FFD700;color:#fff;font-size:14px;letter-spacing:2px;box-shadow:0 5px 0 #5a0000,0 8px 20px rgba(160,0,0,.4)}
.b-roll:hover:not(:disabled){filter:brightness(1.12);transform:translateY(-3px);box-shadow:0 8px 0 #5a0000,0 12px 28px rgba(160,0,0,.5)}
.b-end{background:transparent;border-color:var(--gold);color:var(--gold)}
.b-end:hover:not(:disabled){background:rgba(200,160,0,.12)}
.b-house{background:rgba(0,0,0,.3);border-color:#43a047;color:#88ee88}
.b-house:hover:not(:disabled){background:rgba(30,140,30,.18)}
.b-trade{background:rgba(0,0,0,.3);border-color:#1e88e5;color:#aad4ff}
.b-trade:hover:not(:disabled){background:rgba(20,80,200,.18)}
.b-bail{background:rgba(0,0,0,.3);border-color:#bbb;color:#ccc}
.b-stats{background:rgba(0,0,0,.3);border-color:#9c27b0;color:#ce93d8}
.b-stats:hover:not(:disabled){background:rgba(100,20,140,.18)}
.b-undo{background:rgba(0,0,0,.3);border-color:#607d8b;color:#b0bec5;font-size:11px}
.b-undo:hover:not(:disabled){background:rgba(60,80,90,.3)}
.btn:disabled{opacity:.28;cursor:not-allowed!important;transform:none!important;box-shadow:none!important}
.pl-list{display:flex;flex-direction:column;gap:5px}
.pl-item{display:flex;align-items:center;gap:8px;padding:7px 9px;background:rgba(0,0,0,.22);border-radius:6px;border:1px solid rgba(255,255,255,.06);transition:all .18s;cursor:pointer}
.pl-item:hover{background:rgba(255,255,255,.05)}
.pl-item.active{background:rgba(200,160,0,.15);border-color:rgba(200,160,0,.45)}
.pl-item.bust{opacity:.3}
.pl-item .ai-tag{font-size:8px;background:rgba(0,150,255,.25);color:#88ddff;border-radius:3px;padding:1px 4px;letter-spacing:.5px}
.pl-tsm{width:29px;height:29px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:15px;border:2px solid rgba(255,255,255,.4);flex-shrink:0}
.pl-info{flex:1;min-width:0}
.pl-name{font-size:11px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.pl-money{font-size:10px;color:#FFD700;margin-top:1px}
.pl-wealth-bar{height:3px;border-radius:2px;background:rgba(255,255,255,.08);margin-top:3px;overflow:hidden}
.pl-wealth-fill{height:100%;border-radius:2px;transition:width .5s ease}
#log{max-height:130px;overflow-y:auto;display:flex;flex-direction:column;gap:2px}
.log-e{font-size:9.5px;padding:3px 7px;border-radius:2px;line-height:1.5;border-left:2.5px solid transparent;animation:logSlide .2s ease-out}
@keyframes logSlide{from{opacity:0;transform:translateX(-6px)}to{opacity:1}}
.le-action{color:#ddd;background:rgba(255,255,255,.04);border-color:#555}
.le-money{color:#FFD700;background:rgba(200,160,0,.07);border-color:var(--gold)}
.le-warn{color:#ff8866;background:rgba(200,50,20,.07);border-color:#ff5533}
.le-info{color:#88ccff;background:rgba(20,80,200,.07);border-color:#4488cc}
.le-card{color:#ffcc55;background:rgba(200,130,0,.09);border-color:#ff9900}
.le-ai{color:#88eeff;background:rgba(0,150,255,.07);border-color:#00aacc}
#log::-webkit-scrollbar{width:3px}
#log::-webkit-scrollbar-thumb{background:rgba(200,160,0,.35);border-radius:2px}

/* ═══ FLOATING MONEY ═══ */
.float-money{position:fixed;font-family:'Oswald',sans-serif;font-size:22px;font-weight:700;pointer-events:none;z-index:9999;text-shadow:0 2px 8px rgba(0,0,0,.8);animation:floatUp .9s ease-out forwards}
.float-money.gain{color:#00ee88}
.float-money.lose{color:#ff4444}
@keyframes floatUp{0%{opacity:1;transform:translateY(0)scale(1)}100%{opacity:0;transform:translateY(-70px)scale(.8)}}

/* ═══ MOVE INDICATOR ═══ */
#move-indicator{position:fixed;top:16px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,.92);color:#FFD700;padding:8px 22px;border-radius:6px;font-size:13px;font-weight:700;z-index:500;border:1px solid var(--gold);display:none;letter-spacing:1px;box-shadow:0 4px 20px rgba(0,0,0,.6)}

/* ═══ MODAL ═══ */
#moverlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.82);z-index:1000;align-items:center;justify-content:center}
#moverlay.on{display:flex}
#mbox{background:linear-gradient(160deg,#0e3a0e,#071507);border:3px solid var(--gold);border-radius:12px;padding:26px;color:#fff;max-width:420px;width:94%;text-align:center;box-shadow:0 16px 70px rgba(0,0,0,.9),inset 0 1px 0 rgba(200,160,0,.12);max-height:90vh;overflow-y:auto}
.mt{font-family:'Oswald',sans-serif;font-size:20px;color:#FFD700;margin-bottom:10px;letter-spacing:3px;text-transform:uppercase}
.mbody{font-size:13px;line-height:1.75;margin-bottom:12px;color:#ccc}
.mprice{font-size:30px;font-weight:700;color:#FFD700;margin:8px 0;font-family:'Oswald',sans-serif}
.macts{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin-top:12px}
.mb{padding:11px 22px;border:2px solid;border-radius:6px;font-size:12px;font-weight:700;cursor:pointer;transition:all .18s;text-transform:uppercase;letter-spacing:1px;font-family:inherit}
.mb-y{background:linear-gradient(180deg,#008800,#004400);border-color:#00cc00;color:#fff}
.mb-y:hover{filter:brightness(1.18)}
.mb-n{background:transparent;border-color:var(--gold);color:var(--gold)}
.mb-n:hover{background:rgba(200,160,0,.12)}
.mb-ok{background:linear-gradient(180deg,#cc2600,#840000);border-color:#FFD700;color:#fff;min-width:110px}
.mb-ok:hover{filter:brightness(1.18)}
.brow{display:flex;align-items:center;gap:8px;padding:9px;background:rgba(0,0,0,.35);border-radius:6px;margin-bottom:6px;border:1px solid rgba(200,160,0,.12)}

/* ═══ TURN BANNER ═══ */
#turn-banner{display:none;position:fixed;inset:0;z-index:4000;align-items:center;justify-content:center;pointer-events:none}
#turn-banner.on{display:flex;pointer-events:all}
.tb-bg{position:absolute;inset:0;background:rgba(0,0,0,.75);animation:tbFade .25s ease}
@keyframes tbFade{from{opacity:0}to{opacity:1}}
.tb-box{position:relative;z-index:1;text-align:center;animation:tbPop .45s cubic-bezier(.34,1.56,.64,1);cursor:pointer}
@keyframes tbPop{from{transform:scale(.3)translateY(40px);opacity:0}to{transform:scale(1);opacity:1}}
.tb-tok{width:90px;height:90px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:46px;margin:0 auto 18px;border:5px solid rgba(255,255,255,.9);box-shadow:0 0 60px rgba(0,0,0,.8)}
.tb-lbl{font-size:12px;letter-spacing:5px;text-transform:uppercase;color:rgba(255,255,255,.5);margin-bottom:10px;font-family:'Oswald',sans-serif}
.tb-name{font-family:'Oswald',sans-serif;font-size:52px;font-weight:900;color:#fff;letter-spacing:3px;text-shadow:0 4px 20px rgba(0,0,0,.8)}
.tb-money{font-size:22px;font-weight:700;color:#FFD700;margin-top:6px;font-family:'Oswald',sans-serif}
.tb-hint{font-size:13px;color:rgba(255,255,255,.4);margin-top:10px;letter-spacing:2px}

/* ═══ CARD REVEAL ═══ */
#card-reveal{display:none;position:fixed;inset:0;z-index:3500;align-items:center;justify-content:center}
#card-reveal.on{display:flex}
.cr-bg{position:absolute;inset:0;background:rgba(0,0,0,.88)}
.cr-inner{position:relative;z-index:1;perspective:900px}
.cr-card{width:300px;border-radius:18px;padding:36px 28px;text-align:center;color:#fff;box-shadow:0 30px 80px rgba(0,0,0,.9),0 0 0 3px rgba(255,255,255,.15);animation:cardFlip .55s cubic-bezier(.34,1.2,.64,1)}
@keyframes cardFlip{0%{transform:rotateY(-90deg)scale(.8);opacity:0}60%{transform:rotateY(8deg)scale(1.02)}100%{transform:rotateY(0)scale(1);opacity:1}}
.cr-card.ev{background:linear-gradient(160deg,#d46000,#7a2d00)}
.cr-card.gem{background:linear-gradient(160deg,#1558b0,#051a40)}
.cr-icon{font-size:64px;display:block;margin-bottom:18px;filter:drop-shadow(0 4px 12px rgba(0,0,0,.5))}
.cr-type{font-size:11px;letter-spacing:6px;text-transform:uppercase;opacity:.7;margin-bottom:16px;font-family:'Oswald',sans-serif;border:1px solid rgba(255,255,255,.25);display:inline-block;padding:4px 16px;border-radius:3px}
.cr-text{font-size:19px;font-weight:700;line-height:1.6;margin-bottom:24px;font-family:'Playfair Display',serif;text-shadow:0 2px 8px rgba(0,0,0,.5)}
.cr-btn{padding:14px 48px;background:rgba(255,255,255,.18);border:2px solid rgba(255,255,255,.45);color:#fff;font-family:'Oswald',sans-serif;font-size:15px;font-weight:700;letter-spacing:3px;cursor:pointer;border-radius:8px;transition:all .2s;text-transform:uppercase}
.cr-btn:hover{background:rgba(255,255,255,.3);transform:translateY(-2px)}

/* ═══ DEED MODAL ═══ */
#deed-modal{display:none;position:fixed;inset:0;z-index:3600;align-items:center;justify-content:center}
#deed-modal.on{display:flex}
.deed-bg{position:absolute;inset:0;background:rgba(0,0,0,.88)}
.deed-wrap{position:relative;z-index:1;animation:deedSlide .45s cubic-bezier(.34,1.3,.64,1)}
@keyframes deedSlide{from{transform:translateY(60px)scale(.85);opacity:0}to{transform:translateY(0)scale(1);opacity:1}}
.deed{width:290px;background:#f9f5e8;border-radius:12px;overflow:hidden;box-shadow:0 30px 80px rgba(0,0,0,.9),8px 8px 0 rgba(0,0,0,.3)}
.deed-band{height:60px;display:flex;align-items:center;justify-content:center;flex-direction:column;padding:0 12px;gap:4px}
.deed-band-label{font-family:'Oswald',sans-serif;font-size:8px;letter-spacing:4px;color:rgba(255,255,255,.75);text-transform:uppercase}
.deed-band-title{font-family:'Oswald',sans-serif;font-size:13px;letter-spacing:2px;color:#fff;font-weight:700;text-transform:uppercase;text-align:center}
.deed-body{background:#f9f5e8;color:#111;padding:0 14px 14px}
.deed-name{font-family:'Playfair Display',serif;font-size:20px;font-weight:700;text-align:center;padding:12px 0 8px;border-bottom:2px solid #333;margin-bottom:10px}
.deed-price-row{display:flex;justify-content:space-between;font-size:10px;font-weight:700;margin-bottom:10px;color:#333;border-bottom:1px solid #bbb;padding-bottom:7px}
.deed-tbl{width:100%;border-collapse:collapse;font-size:11px}
.deed-tbl tr{border-bottom:1px solid #ddd}
.deed-tbl td{padding:4px 6px}
.deed-tbl td:last-child{text-align:right;font-weight:700}
.deed-tbl tr:first-child{background:#333;color:#fff}
.deed-tbl tr:first-child td{font-weight:700;padding:5px 6px}
.deed-tbl tr.highlight{background:#f0ebe0;font-weight:600}
.deed-owner{margin-top:8px;padding:7px;background:rgba(0,0,0,.06);border-radius:5px;font-size:10px;text-align:center;color:#444}
.deed-actions{display:flex;gap:8px;padding:10px 14px;background:#f0ebe0;border-top:2px solid #ddd}
.deed-btn{flex:1;padding:11px;border-radius:6px;font-family:'Oswald',sans-serif;font-size:12px;font-weight:700;letter-spacing:1px;cursor:pointer;transition:all .18s;border:2px solid}
.deed-btn.buy{background:linear-gradient(180deg,#006600,#003300);border-color:#00aa00;color:#fff}
.deed-btn.buy:hover{filter:brightness(1.15);transform:translateY(-1px)}
.deed-btn.pass{background:transparent;border-color:#888;color:#555}
.deed-btn.pass:hover{background:rgba(0,0,0,.08)}
.deed-rail-tbl{width:100%;border-collapse:collapse;font-size:11px;margin-top:10px}
.deed-rail-tbl td{padding:5px 7px;border-bottom:1px solid #ccc}
.deed-rail-tbl td:last-child{text-align:right;font-weight:700}
.deed-rail-icon{font-size:38px;text-align:center;padding:14px 0}
.deed-mortgage{margin-top:8px;padding:6px;background:#fff8e1;border:1px solid #e0c060;border-radius:4px;font-size:10px;text-align:center;color:#555}

/* ═══ RENT SPLASH ═══ */
#rent-splash{display:none;position:fixed;inset:0;z-index:3400;align-items:center;justify-content:center}
#rent-splash.on{display:flex}
.rent-bg{position:absolute;inset:0;background:rgba(0,0,0,.82)}
.rent-box{position:relative;z-index:1;text-align:center;animation:rentPop .5s cubic-bezier(.34,1.4,.64,1)}
@keyframes rentPop{from{transform:scale(.2);opacity:0}to{transform:scale(1);opacity:1}}
.rent-icon{font-size:60px;display:block;margin-bottom:14px}
.rent-title{font-family:'Oswald',sans-serif;font-size:15px;letter-spacing:4px;color:#ff8888;margin-bottom:8px;text-transform:uppercase}
.rent-prop{font-family:'Playfair Display',serif;font-size:22px;color:#fff;margin-bottom:6px;font-weight:700}
.rent-owner-row{display:flex;align-items:center;justify-content:center;gap:10px;background:rgba(255,255,255,.08);border-radius:8px;padding:8px 16px;margin-bottom:14px}
.rent-owner-tok{width:36px;height:36px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:18px;border:2px solid rgba(255,255,255,.5)}
.rent-owner-name{font-size:14px;color:#fff;font-weight:700}
.rent-amount{font-family:'Oswald',sans-serif;font-size:72px;font-weight:900;color:#FF4444;text-shadow:0 0 30px rgba(255,0,0,.5);line-height:1;margin-bottom:4px}
.rent-currency{font-size:16px;color:#ff8888;margin-bottom:22px}
.rent-ok{padding:14px 48px;background:linear-gradient(180deg,#cc2600,#840000);border:2px solid #FFD700;color:#fff;font-family:'Oswald',sans-serif;font-size:15px;font-weight:700;letter-spacing:3px;cursor:pointer;border-radius:8px;transition:all .2s}
.rent-ok:hover{filter:brightness(1.15);transform:translateY(-2px)}

/* ═══ STATS ═══ */
.wealth-chart{display:flex;align-items:flex-end;gap:4px;height:80px;margin-top:8px;padding:0 4px}
.wealth-bar{flex:1;border-radius:4px 4px 0 0;min-height:4px;display:flex;align-items:flex-start;justify-content:center;padding-top:3px;font-size:8px;font-weight:700;color:rgba(255,255,255,.9);transition:height .4s}

/* ═══ TOAST ═══ */
#toast{position:fixed;bottom:22px;left:50%;transform:translateX(-50%);background:rgba(4,16,4,.96);color:#fff;padding:12px 28px;border-radius:7px;font-size:13px;font-weight:600;z-index:2000;transition:opacity .5s;pointer-events:none;text-align:center;max-width:440px;border:1px solid rgba(200,160,0,.38);box-shadow:0 5px 24px rgba(0,0,0,.65);opacity:0}

/* ═══ WIN SCREEN ═══ */
#winscr{display:none;position:fixed;inset:0;background:radial-gradient(ellipse 80% 70% at 50% 40%,rgba(30,10,0,.98),rgba(0,0,0,.99));z-index:3000;flex-direction:column;align-items:center;justify-content:center;text-align:center}
#winscr.on{display:flex}
.wt{font-family:'Oswald',sans-serif;font-size:72px;font-weight:900;color:#FFD700;letter-spacing:7px;animation:wglow 1.4s ease-in-out infinite alternate}
@keyframes wglow{from{text-shadow:2px 2px 0 #aa6600}to{text-shadow:0 0 40px #FFD700,0 0 80px #FF8C00,2px 2px 0 #aa6600}}
.win-tok{width:80px;height:80px;border-radius:50%;border:4px solid var(--gold-lt);display:flex;align-items:center;justify-content:center;font-size:40px;margin:16px auto 8px;animation:winTok 1s ease-out;box-shadow:0 0 40px rgba(200,160,0,.5)}
@keyframes winTok{from{transform:scale(0)rotate(-180deg)}80%{transform:scale(1.15)rotate(10deg)}to{transform:scale(1)rotate(0)}}
.wn{font-family:'Oswald',sans-serif;font-size:32px;color:#fff;margin:6px 0 4px;letter-spacing:2px}
.wm{font-size:20px;color:#FFD700;margin-bottom:20px}
.win-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:10px 0 24px;max-width:500px;width:90%}
.win-stat{background:rgba(255,255,255,.07);border:1px solid rgba(200,160,0,.2);border-radius:8px;padding:10px;font-size:11px;color:#aaa}
.win-stat strong{display:block;font-size:18px;color:#FFD700;font-family:'Oswald',sans-serif;margin-bottom:2px}
.wb{padding:18px 50px;background:linear-gradient(180deg,#cc2600,#840000);border:3px solid #FFD700;color:#fff;font-family:'Oswald',sans-serif;font-size:18px;font-weight:700;letter-spacing:3px;cursor:pointer;border-radius:8px;transition:all .2s}
.wb:hover{filter:brightness(1.18);transform:translateY(-2px)}

/* ═══ AI THINKING ═══ */
#ai-thinking{display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,20,40,.95);border:2px solid #00aaff;border-radius:12px;padding:20px 36px;color:#88ccff;font-size:14px;font-weight:700;z-index:2500;text-align:center;box-shadow:0 0 40px rgba(0,150,255,.3)}
#ai-thinking .ai-dots span{display:inline-block;width:8px;height:8px;background:#00aaff;border-radius:50%;margin:0 3px;animation:aiDot 1s infinite}
#ai-thinking .ai-dots span:nth-child(2){animation-delay:.2s}
#ai-thinking .ai-dots span:nth-child(3){animation-delay:.4s}
@keyframes aiDot{0%,80%,100%{transform:scale(.4);opacity:.3}40%{transform:scale(1);opacity:1}}

/* ═══ SAVE BADGE ═══ */
#save-badge{position:fixed;top:14px;right:14px;background:rgba(0,0,0,.8);border:1px solid rgba(200,160,0,.3);color:#888;font-size:10px;padding:5px 10px;border-radius:5px;z-index:400;letter-spacing:.5px}

/* ══════════════════════════════════════════
   🔨 AUCTION OVERLAY
══════════════════════════════════════════ */
#auction-overlay{
  display:none;position:fixed;inset:0;z-index:4500;
  align-items:center;justify-content:center;
  background:rgba(0,0,0,.92);
}
#auction-overlay.on{display:flex;animation:auctionIn .4s cubic-bezier(.34,1.3,.64,1)}
@keyframes auctionIn{from{opacity:0;transform:scale(.85)}to{opacity:1;transform:scale(1)}}
.auc-box{
  width:520px;max-width:95vw;
  background:linear-gradient(160deg,#1a0800,#0a0300);
  border:3px solid #C8A000;border-radius:18px;
  overflow:hidden;
  box-shadow:0 0 0 1px rgba(200,160,0,.2),0 40px 100px rgba(0,0,0,.9),0 0 60px rgba(200,100,0,.15);
}
.auc-header{
  background:linear-gradient(135deg,#7a2a00,#3d1000);
  padding:18px 24px 14px;
  display:flex;align-items:center;gap:14px;
  border-bottom:2px solid rgba(200,160,0,.3);
}
.auc-hammer{font-size:36px;animation:hammerSwing .6s ease-in-out infinite alternate}
@keyframes hammerSwing{from{transform:rotate(-20deg)}to{transform:rotate(10deg)}}
.auc-title-wrap{}
.auc-title{font-family:'Oswald',sans-serif;font-size:11px;letter-spacing:5px;color:rgba(255,180,80,.7);text-transform:uppercase}
.auc-prop-name{font-family:'Oswald',sans-serif;font-size:22px;font-weight:900;color:#FFD700;letter-spacing:2px;margin-top:2px}
.auc-color-bar{height:6px;border-radius:3px;margin-top:6px;width:60px}

/* Price display */
.auc-price-section{
  padding:22px 24px 16px;
  display:flex;align-items:center;justify-content:space-between;
  border-bottom:1px solid rgba(255,255,255,.07);
}
.auc-current-bid{text-align:center}
.auc-bid-label{font-size:9px;letter-spacing:4px;color:rgba(255,200,100,.6);text-transform:uppercase;margin-bottom:4px}
.auc-bid-amount{
  font-family:'Oswald',sans-serif;font-size:56px;font-weight:900;
  color:#FFD700;line-height:1;
  text-shadow:0 0 30px rgba(255,200,0,.5);
  transition:all .15s;
}
.auc-bid-amount.bump{animation:bidBump .2s cubic-bezier(.34,1.6,.64,1)}
@keyframes bidBump{0%{transform:scale(1)}50%{transform:scale(1.18);color:#fff}100%{transform:scale(1)}}
.auc-leader{text-align:center}
.auc-leader-label{font-size:9px;letter-spacing:3px;color:rgba(255,255,255,.4);text-transform:uppercase;margin-bottom:6px}
.auc-leader-tok{width:50px;height:50px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:26px;border:3px solid rgba(255,255,255,.8);margin:0 auto 4px;box-shadow:0 0 20px rgba(255,200,0,.3)}
.auc-leader-name{font-size:12px;color:#fff;font-weight:700}

/* Timer */
.auc-timer-bar{height:5px;background:rgba(255,255,255,.1);border-radius:3px;margin:0 24px}
.auc-timer-fill{height:100%;border-radius:3px;background:linear-gradient(90deg,#00cc44,#FFD700);transition:width .9s linear,background .3s}
.auc-timer-fill.warn{background:linear-gradient(90deg,#ff8800,#ff2200)}

/* Bidders */
.auc-bidders{padding:14px 24px;display:flex;flex-direction:column;gap:8px;max-height:220px;overflow-y:auto}
.auc-bidder-row{
  display:flex;align-items:center;gap:10px;
  padding:10px 14px;border-radius:10px;
  background:rgba(255,255,255,.04);
  border:1.5px solid rgba(255,255,255,.07);
  transition:all .2s;
}
.auc-bidder-row.current{
  background:rgba(200,160,0,.12);
  border-color:rgba(200,160,0,.45);
  box-shadow:0 0 16px rgba(200,160,0,.1);
}
.auc-bidder-row.passed{opacity:.35;filter:grayscale(.6)}
.auc-bidder-row.winner{
  background:rgba(0,180,80,.12);
  border-color:rgba(0,220,100,.4);
}
.auc-bidder-tok{width:38px;height:38px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:20px;border:2.5px solid rgba(255,255,255,.6);flex-shrink:0}
.auc-bidder-info{flex:1;min-width:0}
.auc-bidder-name{font-size:13px;font-weight:700;color:#fff}
.auc-bidder-money{font-size:10px;color:#FFD700;margin-top:1px}
.auc-bidder-status{font-size:9px;margin-top:2px;letter-spacing:1px}
.auc-bidder-btns{display:flex;gap:6px;align-items:center}
.auc-bid-input{
  width:80px;background:rgba(0,0,0,.4);color:#FFD700;
  border:1.5px solid rgba(200,160,0,.5);border-radius:6px;
  padding:6px 8px;font-size:13px;font-family:'Oswald',sans-serif;font-weight:700;
  text-align:center;outline:none;
}
.auc-bid-input:focus{border-color:#FFD700;box-shadow:0 0 8px rgba(200,160,0,.3)}
.auc-btn-bid{
  padding:7px 14px;background:linear-gradient(180deg,#006600,#003300);
  border:2px solid #00cc00;color:#fff;border-radius:6px;
  font-family:'Oswald',sans-serif;font-size:11px;font-weight:700;letter-spacing:1px;
  cursor:pointer;transition:all .15s;white-space:nowrap;
}
.auc-btn-bid:hover{filter:brightness(1.2);transform:translateY(-1px)}
.auc-btn-pass{
  padding:7px 12px;background:transparent;border:2px solid #555;color:#888;
  border-radius:6px;font-family:'Oswald',sans-serif;font-size:11px;font-weight:700;
  cursor:pointer;transition:all .15s;
}
.auc-btn-pass:hover{border-color:#888;color:#aaa}

/* ══════════════════════════════════════════
   🏠 3D HOUSES & HOTELS ON BOARD
══════════════════════════════════════════ */
.house-3d{
  width:8px;height:8px;
  background:linear-gradient(170deg,#33ee33,#1aaa1a);
  border:1px solid #087a08;
  border-radius:1px 1px 0 0;
  position:relative;
  box-shadow:1px 1px 0 #063a06,0 2px 4px rgba(0,0,0,.4),inset 0 1px 2px rgba(255,255,255,.3);
  flex-shrink:0;
}
.house-3d::before{
  content:'';position:absolute;
  top:-5px;left:-1px;right:-1px;
  height:6px;
  background:linear-gradient(170deg,#ff4444,#cc2200);
  clip-path:polygon(50% 0%,0% 100%,100% 100%);
}
.house-3d::after{
  content:'';position:absolute;
  left:1px;right:0;bottom:-2px;
  height:2px;
  background:rgba(0,0,0,.3);
  border-radius:0 0 1px 1px;
}
.hotel-3d{
  width:11px;height:10px;
  background:linear-gradient(170deg,#ff5555,#cc0000);
  border:1px solid #880000;
  border-radius:1px;
  position:relative;
  box-shadow:2px 2px 0 #660000,0 3px 6px rgba(0,0,0,.5),inset 0 1px 3px rgba(255,255,255,.25);
  flex-shrink:0;
}
.hotel-3d::before{
  content:'';position:absolute;
  top:-4px;left:-1px;right:-1px;height:5px;
  background:linear-gradient(170deg,#ff8888,#cc4444);
  clip-path:polygon(50% 0%,0% 100%,100% 100%);
}
/* Positioned house/hotel row */
.hrow{position:absolute;top:2px;right:2px;display:flex;gap:2px;z-index:5;flex-wrap:wrap;max-width:32px;align-items:flex-end}

/* ══════════════════════════════════════════
   🏆 MONOPOLY GROUP GLOW
══════════════════════════════════════════ */
.cell.monopoly-owned{
  animation:monopolyGlow 2s ease-in-out infinite alternate;
}
@keyframes monopolyGlow{
  from{box-shadow:0 0 0 2px gold,0 0 8px rgba(255,215,0,.4)}
  to  {box-shadow:0 0 0 3px gold,0 0 18px rgba(255,215,0,.7),0 0 30px rgba(255,180,0,.3)}
}
.cband.monopoly-band{
  animation:bandShimmer 1.8s ease-in-out infinite alternate;
}
@keyframes bandShimmer{
  from{filter:brightness(1)}
  to  {filter:brightness(1.35) saturate(1.2)}
}

/* ══════════════════════════════════════════
   📜 UPGRADED DEED CARD
══════════════════════════════════════════ */
.deed{
  width:310px;background:#f9f5e8;border-radius:14px;overflow:hidden;
  box-shadow:0 30px 80px rgba(0,0,0,.9),10px 10px 0 rgba(0,0,0,.25),0 0 0 1px rgba(0,0,0,.15);
}
.deed-band{
  height:70px;display:flex;align-items:center;justify-content:center;flex-direction:column;
  padding:0 14px;gap:4px;position:relative;overflow:hidden;
}
.deed-band::before{
  content:'';position:absolute;inset:0;
  background:linear-gradient(135deg,rgba(255,255,255,.18) 0%,transparent 60%);
}
.deed-band-label{font-family:'Oswald',sans-serif;font-size:8px;letter-spacing:5px;color:rgba(255,255,255,.7);text-transform:uppercase}
.deed-band-title{font-family:'Oswald',sans-serif;font-size:15px;letter-spacing:2px;color:#fff;font-weight:900;text-transform:uppercase;text-align:center;text-shadow:0 2px 6px rgba(0,0,0,.4)}
.deed-monopoly-badge{
  font-size:7px;letter-spacing:3px;color:rgba(255,255,255,.85);text-transform:uppercase;
  background:rgba(255,255,255,.2);border-radius:10px;padding:2px 8px;margin-top:2px;
  border:1px solid rgba(255,255,255,.35);
}
.deed-body{background:#f9f5e8;color:#111;padding:0 16px 16px}
.deed-name{font-family:'Playfair Display',serif;font-size:21px;font-weight:700;text-align:center;padding:14px 0 10px;border-bottom:2.5px solid #222;margin-bottom:10px}
.deed-price-row{display:flex;justify-content:space-between;font-size:10px;font-weight:700;margin-bottom:10px;color:#444;border-bottom:1px solid #ccc;padding-bottom:8px}
.deed-tbl{width:100%;border-collapse:collapse;font-size:11px}
.deed-tbl tr{border-bottom:1px solid #e0d8c8}
.deed-tbl td{padding:5px 6px}
.deed-tbl td:last-child{text-align:right;font-weight:700}
.deed-tbl tr:first-child{background:#333;color:#fff}
.deed-tbl tr:first-child td{font-weight:700;padding:6px 6px}
.deed-tbl tr.highlight{background:#fff3d4;font-weight:600;color:#7a4400}
.deed-tbl tr.highlight td:last-child{color:#cc6600}
.deed-owner{
  margin-top:10px;padding:8px 10px;
  background:rgba(0,0,0,.05);border-radius:7px;
  font-size:10px;text-align:center;color:#444;
  border:1px solid rgba(0,0,0,.1);
}
.deed-actions{display:flex;gap:10px;padding:12px 16px;background:#ede8d8;border-top:2px solid #ccc}
.deed-btn{
  flex:1;padding:13px;border-radius:8px;
  font-family:'Oswald',sans-serif;font-size:13px;font-weight:700;letter-spacing:1.5px;
  cursor:pointer;transition:all .15s;border:2px solid;
}
.deed-btn.buy{
  background:linear-gradient(180deg,#007700,#004400);
  border-color:#00cc00;color:#fff;
  box-shadow:0 4px 0 #002200;
}
.deed-btn.buy:hover{filter:brightness(1.15);transform:translateY(-2px);box-shadow:0 6px 0 #002200}
.deed-btn.buy:active{transform:translateY(2px);box-shadow:0 1px 0 #002200}
.deed-btn.pass{background:transparent;border-color:#999;color:#666}
.deed-btn.pass:hover{background:rgba(0,0,0,.06);border-color:#666}
</style>
</head>
<body class="th-dark">

<!-- ═══════════ SETUP ═══════════ -->
<div id="setup">
  <!-- Header -->
  <div class="setup-header">
    <div class="setup-logo">MONOPOLY</div><br>
    <div class="setup-edition">Pro Edition · Deutsche Klassik</div>
  </div>

  <!-- Theme -->
  <div class="setup-theme-bar">
    <button class="thbtn on" onclick="setTheme('dark',this)">🌙 Dunkel</button>
    <button class="thbtn" onclick="setTheme('gold',this)">✨ Gold</button>
    <button class="thbtn" onclick="setTheme('night',this)">🔵 Nacht</button>
  </div>

  <!-- Step 1: Player Count -->
  <div class="setup-card">
    <div class="setup-card-title"><span class="step-badge">1</span>Anzahl der Spieler wählen</div>
    <div class="player-count-grid" id="cnt-row"></div>
  </div>

  <!-- Step 2: Player Config -->
  <div class="setup-card">
    <div class="setup-card-title"><span class="step-badge">2</span>Spieler konfigurieren</div>
    <div class="player-cards-grid" id="p-rows"></div>
  </div>

  <!-- Step 3: Rules -->
  <div class="setup-card">
    <div class="setup-card-title"><span class="step-badge">3</span>Spielregeln</div>
    <div class="rules-grid">
      <div class="rule-item">
        <label><input type="checkbox" id="r-timer" checked> ⏱ Zugzeit-Limit</label>
        <select id="r-timer-sec">
          <option value="60">60 Sekunden</option>
          <option value="90" selected>90 Sekunden</option>
          <option value="120">120 Sekunden</option>
          <option value="0">Kein Limit</option>
        </select>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-auction" checked> 🔨 Pflicht-Auktion</label>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-pool" checked> 🚗 Freiparkgeld-Pool</label>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-undo" checked> 🔄 Undo erlaubt</label>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-sound" checked> 🔊 Sounds an</label>
      </div>
      <div class="rule-item">
        <label>💰 Startkapital</label>
        <select id="r-start-money">
          <option value="1500" selected>M 1500 (Standard)</option>
          <option value="2000">M 2000 (Leicht)</option>
          <option value="3000">M 3000 (Schnell)</option>
        </select>
      </div>
    </div>
  </div>

  <button class="start-btn" onclick="startGame()">🎲 &nbsp; SPIEL STARTEN</button>
</div>

<!-- ═══════════ GAME ═══════════ -->
<div id="game">
  <div id="bwrap">
    <div id="board-shadow"></div>
    <div id="board-3d-wrap">
    <div id="board">
      <div class="corner" id="c0" onclick="showSpaceInfo(0)">
        <div class="go-arrow">🚦</div><div class="go-los">LOS</div>
        <div class="go-sub">Sammle beim Passieren</div><div class="go-amt">M 200</div>
      </div>
      <div class="corner" id="c10" onclick="showSpaceInfo(10)">
        <div class="jail-wrap"><div class="jail-stripe"></div><div class="visit-lbl">NUR ZU BESUCH</div>
          <div class="jail-box"><div class="jail-bars">⛓</div><div class="jail-txt">GEFÄNG<br>NIS</div></div>
        </div>
      </div>
      <div class="corner" id="c20" onclick="showSpaceInfo(20)">
        <div class="fp-icon">🚗</div><div class="fp-txt">FREI<br>PARKEN</div>
      </div>
      <div class="corner" id="c30" onclick="showSpaceInfo(30)">
        <div class="gtj-deco"></div>
        <div class="gtj-inner"><div class="gtj-icon">👮</div><div class="gtj-txt">GEHE INS<br>GEFÄNGNIS!</div></div>
      </div>
      <div id="bcenter">
        <div class="deck-row">
          <div class="deck deck-ev"><span class="deck-ico">❓</span>EREIGNIS<br>KARTE</div>
          <div class="deck deck-gem"><span class="deck-ico">📋</span>GEMEIN-<br>SCHAFT</div>
        </div>
        <div class="center-title">
          <div class="mono-logo">MONOPOLY</div>
          <div class="mono-sub">Pro Edition</div>
        </div>
        <div id="dice-sec">
          <div id="dice-row">
            <div class="die" id="die1" onclick="rollDice()">🎲</div>
            <div class="die" id="die2" onclick="rollDice()">🎲</div>
          </div>
          <div id="dice-info">Bereit zum Würfeln…</div>
          <div id="dice-result-big"></div>
          <div id="dice-double-banner">⚡ PASCH — NOCHMAL!</div>
        </div>
      </div>
    </div>
    </div><!-- /#board-3d-wrap -->
  </div>

  <!-- SIDEBAR -->
  <div id="sidebar">
    <div class="sc">
      <div class="sc-title">Aktueller Spieler</div>
      <div class="cp-row">
        <div class="cp-tok" id="cp-tok">🎩</div>
        <div>
          <div class="cp-name" id="cp-name">–</div>
          <div class="cp-money" id="cp-money">M 0</div>
          <div class="cp-wealth" id="cp-wealth">Vermögen: M 0</div>
          <div class="cp-pos" id="cp-pos">–</div>
        </div>
      </div>
      <div id="turn-timer" style="display:none">
        <div class="timer-bar-wrap"><div id="timer-bar"></div></div>
        <div id="timer-txt">90</div>
      </div>
      <div class="acts">
        <button class="btn b-roll" id="b-roll" onclick="rollDice()">🎲 &nbsp;WÜRFELN</button>
        <button class="btn b-bail" id="b-bail" style="display:none" onclick="payBail()">⛓ Kaution M 50</button>
        <button class="btn b-house" id="b-house" onclick="openBuild()">🏠 Bauen / Hypothek</button>
        <button class="btn b-trade" id="b-trade" onclick="openTrade()">🤝 Handel</button>
        <button class="btn b-undo" id="b-undo" onclick="doUndo()" style="display:none">🔄 Zug rückgängig</button>
        <button class="btn b-stats" onclick="openStats()">📊 Statistiken</button>
        <button class="btn b-end" id="b-end" onclick="endTurn()" disabled>➡ Zug beenden</button>
      </div>
    </div>
    <div class="sc">
      <div class="sc-title">Spieler</div>
      <div class="pl-list" id="pl-list"></div>
    </div>
    <div class="sc">
      <div class="sc-title">Spielprotokoll</div>
      <div id="log"></div>
    </div>
  </div>
</div>

<div id="move-indicator"></div>

<!-- ═══════════ AUCTION OVERLAY ═══════════ -->
<div id="auction-overlay">
  <div class="auc-box">
    <div class="auc-header">
      <div class="auc-hammer">🔨</div>
      <div class="auc-title-wrap">
        <div class="auc-title">Versteigerung</div>
        <div class="auc-prop-name" id="auc-prop-name">–</div>
        <div class="auc-color-bar" id="auc-color-bar"></div>
      </div>
    </div>
    <div class="auc-price-section">
      <div class="auc-current-bid">
        <div class="auc-bid-label">Höchstgebot</div>
        <div class="auc-bid-amount" id="auc-amount">M 0</div>
      </div>
      <div class="auc-leader">
        <div class="auc-leader-label">Führend</div>
        <div class="auc-leader-tok" id="auc-leader-tok">–</div>
        <div class="auc-leader-name" id="auc-leader-name">–</div>
      </div>
    </div>
    <div class="auc-timer-bar"><div class="auc-timer-fill" id="auc-timer-fill" style="width:100%"></div></div>
    <div class="auc-bidders" id="auc-bidders"></div>
  </div>
</div>

<!-- TURN BANNER -->
<div id="turn-banner">
  <div class="tb-bg" onclick="hideTurnBanner()"></div>
  <div class="tb-box" onclick="hideTurnBanner()">
    <div class="tb-tok" id="tb-tok">🎩</div>
    <div class="tb-lbl">AN DER REIHE IST</div>
    <div class="tb-name" id="tb-name">–</div>
    <div class="tb-money" id="tb-money">M 0</div>
    <div class="tb-hint">Antippen zum Weiterspielen</div>
  </div>
</div>

<!-- CARD REVEAL -->
<div id="card-reveal">
  <div class="cr-bg" onclick="dismissCardReveal()"></div>
  <div class="cr-inner">
    <div class="cr-card" id="cr-card">
      <span class="cr-icon" id="cr-icon">❓</span>
      <div class="cr-type" id="cr-type">EREIGNISKARTE</div>
      <div class="cr-text" id="cr-text">–</div>
      <button class="cr-btn" onclick="dismissCardReveal()">OK &nbsp;›</button>
    </div>
  </div>
</div>

<!-- DEED MODAL -->
<div id="deed-modal">
  <div class="deed-bg" onclick="if(window._deedClosable)closeDeed()"></div>
  <div class="deed-wrap">
    <div class="deed" id="deed-card"></div>
  </div>
</div>

<!-- RENT SPLASH -->
<div id="rent-splash">
  <div class="rent-bg"></div>
  <div class="rent-box">
    <span class="rent-icon">💸</span>
    <div class="rent-title">MIETE ZAHLEN!</div>
    <div class="rent-prop" id="rs-prop">–</div>
    <div class="rent-owner-row">
      <div class="rent-owner-tok" id="rs-tok" style="background:#888">🎩</div>
      <div class="rent-owner-name" id="rs-owner">–</div>
    </div>
    <div class="rent-amount" id="rs-amount">0</div>
    <div class="rent-currency">M (Mark)</div>
    <button class="rent-ok" id="rs-ok">OK — Weiter</button>
  </div>
</div>

<div id="moverlay" onclick="if(event.target===this&&mClose)closeModal()">
  <div id="mbox"><div class="mt" id="mt">–</div><div id="mc"></div><div class="macts" id="ma"></div></div>
</div>
<div id="winscr">
  <div class="wt">🏆 GEWINNER!</div>
  <div class="win-tok" id="win-tok">🎩</div>
  <div class="wn" id="wn">–</div>
  <div class="wm" id="wm">–</div>
  <div class="win-stats" id="win-stats"></div>
  <button class="wb" onclick="location.reload()">🎮 Neues Spiel</button>
</div>
<div id="toast"></div>
<div id="ai-thinking">🤖 KI denkt nach…<div class="ai-dots"><span></span><span></span><span></span></div></div>
<div id="save-badge">💾 Gespeichert</div>

<script>
/* ═══════════ BOARD DATA ═══════════ */
const PCOLS=['#e53935','#43a047','#1e88e5','#fb8c00','#8e24aa','#00acc1','#e91e63','#795548'];
const ALL_COLORS=[
  '#e53935','#43a047','#1e88e5','#fb8c00',
  '#8e24aa','#00acc1','#e91e63','#795548',
  '#f4511e','#039be5','#7cb342','#fdd835',
  '#00897b','#546e7a','#d81b60','#6d4c41'
];
const TOKENS=['🎩','🚂','🐕','⚓','🏎','🎯','♟','👛'];
const TNAMES=['Zylinder','Lokomotive','Hund','Anker','Auto','Bügeleisen','Figur','Geldsack'];
const DFACES=['⚀','⚁','⚂','⚃','⚄','⚅'];

const BOARD=[
  {id:0,  type:'go',      name:'LOS',               side:'B'},
  {id:1,  type:'prop',    name:'Badstraße',          color:'#8B4513',price:60,  rent:[2,10,30,90,160,250],   hp:50,  side:'B'},
  {id:2,  type:'chest',   name:'Gemeinschaft',       side:'B'},
  {id:3,  type:'prop',    name:'Turmstraße',         color:'#8B4513',price:60,  rent:[4,20,60,180,320,450],  hp:50,  side:'B'},
  {id:4,  type:'tax',     name:'Einkommensteuer',    amount:200,                side:'B'},
  {id:5,  type:'rail',    name:'Südbahnhof',         price:200,                 side:'B'},
  {id:6,  type:'prop',    name:'Elisenstraße',       color:'#5BC8F5',price:100, rent:[6,30,90,270,400,550],  hp:50,  side:'B'},
  {id:7,  type:'chance',  name:'Ereignis',           side:'B'},
  {id:8,  type:'prop',    name:'Poststraße',         color:'#5BC8F5',price:100, rent:[6,30,90,270,400,550],  hp:50,  side:'B'},
  {id:9,  type:'prop',    name:'Seestraße',          color:'#5BC8F5',price:120, rent:[8,40,100,300,450,600], hp:50,  side:'B'},
  {id:10, type:'jail',    name:'Gefängnis / Besuch', side:'B'},
  {id:11, type:'prop',    name:'Münchener Str.',     color:'#E91E8C',price:140, rent:[10,50,150,450,625,750],hp:100, side:'L'},
  {id:12, type:'utility', name:'Elektrizitätswerk',  price:150,                 side:'L'},
  {id:13, type:'prop',    name:'Wiener Straße',      color:'#E91E8C',price:140, rent:[10,50,150,450,625,750],hp:100, side:'L'},
  {id:14, type:'prop',    name:'Berliner Straße',    color:'#E91E8C',price:160, rent:[12,60,180,500,700,900],hp:100, side:'L'},
  {id:15, type:'rail',    name:'Westbahnhof',        price:200,                 side:'L'},
  {id:16, type:'prop',    name:'Theaterstraße',      color:'#FF7700',price:180, rent:[14,70,200,550,750,950],hp:100, side:'L'},
  {id:17, type:'chest',   name:'Gemeinschaft',       side:'L'},
  {id:18, type:'prop',    name:'Museumstraße',       color:'#FF7700',price:180, rent:[14,70,200,550,750,950],hp:100, side:'L'},
  {id:19, type:'prop',    name:'Opernplatz',         color:'#FF7700',price:200, rent:[16,80,220,600,800,1000],hp:100,side:'L'},
  {id:20, type:'parking', name:'Frei Parken',        side:'L'},
  {id:21, type:'prop',    name:'Lessingstraße',      color:'#CC0000',price:220, rent:[18,90,250,700,875,1050],hp:150,side:'T'},
  {id:22, type:'chance',  name:'Ereignis',           side:'T'},
  {id:23, type:'prop',    name:'Schillerstraße',     color:'#CC0000',price:220, rent:[18,90,250,700,875,1050],hp:150,side:'T'},
  {id:24, type:'prop',    name:'Goethestraße',       color:'#CC0000',price:240, rent:[20,100,300,750,925,1100],hp:150,side:'T'},
  {id:25, type:'rail',    name:'Nordbahnhof',        price:200,                 side:'T'},
  {id:26, type:'prop',    name:'Rathausplatz',       color:'#FFD700',price:260, rent:[22,110,330,800,975,1150],hp:150,side:'T'},
  {id:27, type:'utility', name:'Wasserwerk',         price:150,                 side:'T'},
  {id:28, type:'prop',    name:'Hauptstraße',        color:'#FFD700',price:260, rent:[22,110,330,800,975,1150],hp:150,side:'T'},
  {id:29, type:'prop',    name:'Bahnhofstraße',      color:'#FFD700',price:280, rent:[24,120,360,850,1025,1200],hp:150,side:'T'},
  {id:30, type:'gotojail',name:'Geh ins Gefängnis',  side:'T'},
  {id:31, type:'prop',    name:'Parkstraße',         color:'#006400',price:300, rent:[26,130,390,900,1100,1275],hp:200,side:'R'},
  {id:32, type:'prop',    name:'Stadtpark',          color:'#006400',price:300, rent:[26,130,390,900,1100,1275],hp:200,side:'R'},
  {id:33, type:'chest',   name:'Gemeinschaft',       side:'R'},
  {id:34, type:'prop',    name:'Domplatz',           color:'#006400',price:320, rent:[28,150,450,1000,1200,1400],hp:200,side:'R'},
  {id:35, type:'rail',    name:'Hauptbahnhof',       price:200,                 side:'R'},
  {id:36, type:'chance',  name:'Ereignis',           side:'R'},
  {id:37, type:'prop',    name:'Ringstraße',         color:'#000099',price:350, rent:[35,175,500,1100,1300,1500],hp:200,side:'R'},
  {id:38, type:'tax',     name:'Zusatzsteuer',       amount:100,                side:'R'},
  {id:39, type:'prop',    name:'Schlossallee',       color:'#000099',price:400, rent:[50,200,600,1400,1700,2000],hp:200,side:'R'},
];

function getSpaceCenter(id){
  const S=700,C=80,CW=60;
  if(id===0)  return {x:S-C/2,y:S-C/2};
  if(id===10) return {x:C/2,  y:S-C/2};
  if(id===20) return {x:C/2,  y:C/2};
  if(id===30) return {x:S-C/2,y:C/2};
  const def=BOARD[id];
  if(def.side==='B'){const idx=id-1; return {x:C+(8-idx)*CW+CW/2, y:S-C/2};}
  if(def.side==='L'){const idx=id-11;return {x:C/2, y:S-C-idx*CW-CW/2};}
  if(def.side==='T'){const idx=id-21;return {x:C+idx*CW+CW/2, y:C/2};}
  const idx=id-31; return {x:S-C/2, y:C+idx*CW+CW/2};
}

/* ═══════════ CARD DECKS ═══════════ */
const CHANCE=[
  {t:'Rücke vor bis LOS! Erhalte M 200.',fn(p,g,cb){animMoveTo(p,0,true,cb);}},
  {t:'Fahre nach Berliner Straße. Falls du LOS passierst, erhalte M 200.',fn(p,g,cb){animMoveTo(p,14,true,cb);}},
  {t:'Fahre nach Theaterstraße. Falls du LOS passierst, erhalte M 200.',fn(p,g,cb){animMoveTo(p,16,true,cb);}},
  {t:'Fahre zum nächsten Bahnhof.',fn(p,g,cb){const r=[5,15,25,35];const n=r.find(x=>x>p.pos)||r[0];animMoveTo(p,n,true,cb);}},
  {t:'Gehe ins Gefängnis! Rücke direkt dorthin vor.',fn(p,g,cb){goJail(p,cb);}},
  {t:'Schulpreis! Erhalte M 150.',fn(p,g,cb){addM(p,150);cb();}},
  {t:'Bankdividende: Erhalte M 50.',fn(p,g,cb){addM(p,50);cb();}},
  {t:'Rücke 3 Felder zurück.',fn(p,g,cb){animMoveTo(p,(p.pos-3+40)%40,false,cb,true);}},
  {t:'Straßenreparaturen: M 40 pro Haus, M 115 pro Hotel.',fn(p,g,cb){let h=0,ho=0;G.sp.filter(s=>s.own===p.id&&s.h>0).forEach(s=>s.h===5?ho++:h+=s.h);const a=h*40+ho*115;if(a>0)spendM(p,a);cb();}},
  {t:'Gehe zurück zu Badstraße.',fn(p,g,cb){animMoveTo(p,1,false,cb,true);}},
  {t:'Zahle jedem Spieler M 50.',fn(p,g,cb){G.pl.filter(x=>!x.bust&&x.id!==p.id).forEach(o=>{p.money-=50;addM(o,50);});gLog(`${p.name} zahlt M 50 an alle.`,'warn');renderAll();cb();}},
  {t:'Kreuzworträtsel! Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Fahre nach Schlossallee.',fn(p,g,cb){animMoveTo(p,39,true,cb);}},
  {t:'Darlehen fällig. Zahle M 100.',fn(p,g,cb){spendM(p,100);cb();}},
  {t:'Fahre nach Südbahnhof.',fn(p,g,cb){animMoveTo(p,5,true,cb);}},
  {t:'Gefängnisfreikarte! Diese Karte aufbewahren.',fn(p,g,cb){p.jailCard=(p.jailCard||0)+1;gLog(`${p.name} erhält Freikarte!`,'card');cb();}},
];
const CHEST=[
  {t:'Bankirrtum zu Ihren Gunsten! Erhalte M 200.',fn(p,g,cb){addM(p,200);cb();}},
  {t:'Arztrechnung: Zahle M 50.',fn(p,g,cb){spendM(p,50);cb();}},
  {t:'Erbschaft: Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Rücke vor bis LOS! Erhalte M 200.',fn(p,g,cb){animMoveTo(p,0,true,cb);}},
  {t:'Steuernachzahlung: Zahle M 20.',fn(p,g,cb){spendM(p,20);cb();}},
  {t:'Schönheitspreis: Erhalte M 10.',fn(p,g,cb){addM(p,10);cb();}},
  {t:'Auto verkauft: Erhalte M 45.',fn(p,g,cb){addM(p,45);cb();}},
  {t:'Lebensversicherung fällig: Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Gehe ins Gefängnis! Begib dich direkt dorthin.',fn(p,g,cb){goJail(p,cb);}},
  {t:'Straßenbauinspektor: M 40 pro Haus, M 115 pro Hotel.',fn(p,g,cb){let h=0,ho=0;G.sp.filter(s=>s.own===p.id&&s.h>0).forEach(s=>s.h===5?ho++:h+=s.h);const a=h*40+ho*115;if(a>0)spendM(p,a);cb();}},
  {t:'Glückstag! Erhalte M 10 von jedem Mitspieler.',fn(p,g,cb){G.pl.filter(x=>!x.bust&&x.id!==p.id).forEach(o=>{if(o.money>=10){o.money-=10;p.money+=10;}});renderAll();cb();}},
  {t:'Gefängnisfreikarte! Diese Karte aufbewahren.',fn(p,g,cb){p.jailCard=(p.jailCard||0)+1;gLog(`${p.name} erhält Freikarte!`,'card');cb();}},
  {t:'Urlaubsgeld: Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Steuererstattung: Erhalte M 20.',fn(p,g,cb){addM(p,20);cb();}},
  {t:'Schulgebühren: Zahle M 150.',fn(p,g,cb){spendM(p,150);cb();}},
  {t:'Spezialhonorar: Zahle M 100.',fn(p,g,cb){spendM(p,100);cb();}},
];

/* ═══════════ AUDIO ═══════════ */
let AC=null;
function getAC(){if(!AC&&window.AudioContext)AC=new AudioContext();return AC;}
function playSound(type){
  const ac=getAC();
  if(!ac||!RULES.sound)return;
  if(ac.state==='suspended')ac.resume();
  const t=ac.currentTime;
  const g=ac.createGain();g.connect(ac.destination);
  const o=ac.createOscillator();o.connect(g);
  if(type==='roll'){
    o.type='square';o.frequency.setValueAtTime(220,t);o.frequency.exponentialRampToValueAtTime(440,t+.08);o.frequency.exponentialRampToValueAtTime(180,t+.18);
    g.gain.setValueAtTime(.18,t);g.gain.exponentialRampToValueAtTime(.001,t+.22);o.start(t);o.stop(t+.22);
  }else if(type==='move'){
    o.type='sine';o.frequency.setValueAtTime(440,t);o.frequency.exponentialRampToValueAtTime(520,t+.05);
    g.gain.setValueAtTime(.08,t);g.gain.exponentialRampToValueAtTime(.001,t+.07);o.start(t);o.stop(t+.07);
  }else if(type==='money'){
    o.type='triangle';o.frequency.setValueAtTime(660,t);o.frequency.exponentialRampToValueAtTime(880,t+.1);
    g.gain.setValueAtTime(.15,t);g.gain.exponentialRampToValueAtTime(.001,t+.15);o.start(t);o.stop(t+.15);
  }else if(type==='buy'){
    [523,659,784].forEach((f,i)=>{const o2=ac.createOscillator();const g2=ac.createGain();o2.connect(g2);g2.connect(ac.destination);o2.type='triangle';o2.frequency.value=f;g2.gain.setValueAtTime(.12,t+i*.1);g2.gain.exponentialRampToValueAtTime(.001,t+i*.1+.15);o2.start(t+i*.1);o2.stop(t+i*.1+.15);});
  }else if(type==='jail'){
    o.type='sawtooth';o.frequency.setValueAtTime(200,t);o.frequency.exponentialRampToValueAtTime(80,t+.4);
    g.gain.setValueAtTime(.2,t);g.gain.exponentialRampToValueAtTime(.001,t+.45);o.start(t);o.stop(t+.45);
  }else if(type==='rent'){
    o.type='triangle';o.frequency.setValueAtTime(300,t);o.frequency.exponentialRampToValueAtTime(150,t+.3);
    g.gain.setValueAtTime(.18,t);g.gain.exponentialRampToValueAtTime(.001,t+.35);o.start(t);o.stop(t+.35);
  }else if(type==='win'){
    [523,659,784,1047].forEach((f,i)=>{const o2=ac.createOscillator();const g2=ac.createGain();o2.connect(g2);g2.connect(ac.destination);o2.type='triangle';o2.frequency.value=f;g2.gain.setValueAtTime(.15,t+i*.12);g2.gain.exponentialRampToValueAtTime(.001,t+i*.12+.3);o2.start(t+i*.12);o2.stop(t+i*.12+.3);});
  }else if(type==='double'){
    [440,550,660].forEach((f,i)=>{const o2=ac.createOscillator();const g2=ac.createGain();o2.connect(g2);g2.connect(ac.destination);o2.type='sine';o2.frequency.value=f;g2.gain.setValueAtTime(.1,t+i*.08);g2.gain.exponentialRampToValueAtTime(.001,t+i*.08+.12);o2.start(t+i*.08);o2.stop(t+i*.08+.12);});
  }
}

/* ═══════════ GAME STATE ═══════════ */
let G={};
let RULES={timer:90,auction:true,pool:true,undo:true,sound:true,startMoney:1500};
let mClose=false,toastT=null,moving=false,timerInt=null,timerLeft=0,undoState=null;
let _turnBannerCb=null,_cardRevealCb=null;

function setMoving(v){moving=v;_refreshButtons();}

function initG(players){
  G={
    pl:players.map((p,i)=>({
      id:i,name:p.name,token:p.token,color:p.color,isAI:p.isAI||false,
      money:RULES.startMoney,pos:0,jailed:false,jailRounds:0,jailCard:0,bust:false,
      stats:{turns:0,totalRent:0,totalPaid:0,spaceLanded:Array(40).fill(0)}
    })),
    sp:BOARD.map(b=>({...b,own:null,h:0,mortg:false})),
    cur:0,rolled:false,doubleCount:0,pool:0,
    chanceDeck:[...CHANCE].sort(()=>Math.random()-.5),
    chestDeck:[...CHEST].sort(()=>Math.random()-.5),
    chanceI:0,chestI:0,lastDice:[1,1],turn:0,wealthHistory:[]
  };
}

/* ═══════════ THEME ═══════════ */
function setTheme(t,btn){
  document.body.className='th-'+t;
  document.querySelectorAll('.thbtn').forEach(b=>b.classList.remove('on'));
  btn.classList.add('on');
}

/* ═══════════ SETUP UI ═══════════ */
let sc=4, sp2=[];

const PC_ICONS=['','','👥','👥👤','👥👥','👥👥👤','👥👥👥','👥👥👥👤','👥👥👥👥'];

function buildSetup(){
  // Count buttons
  const cr=document.getElementById('cnt-row');cr.innerHTML='';
  [2,3,4,5,6,7,8].forEach(n=>{
    const b=document.createElement('button');
    b.className='pc-btn'+(n===sc?' sel':'');
    b.innerHTML=`<span class="pc-num">${n}</span><span class="pc-label">${n===2?'Duo':n===3?'Trio':n+'er'}</span><span class="pc-icons">${'👤'.repeat(n)}</span>`;
    b.onclick=()=>{sc=n;buildSetup();};
    cr.appendChild(b);
  });

  // Maintain existing player data
  while(sp2.length<sc){
    const idx=sp2.length;
    sp2.push({name:`Spieler ${idx+1}`,token:idx%TOKENS.length,color:ALL_COLORS[idx%ALL_COLORS.length],isAI:false});
  }
  sp2=sp2.slice(0,sc);

  // Player cards
  const pr=document.getElementById('p-rows');pr.innerHTML='';
  for(let i=0;i<sc;i++){
    const card=document.createElement('div');
    card.className='player-card'+(sp2[i].isAI?' is-ai':'');
    card.id='pcard'+i;

    card.innerHTML=`
      <div class="pc-header">
        <div class="pc-avatar" id="pav${i}" style="background:${sp2[i].color}" title="Token auswählen">${sp2[i].isAI?'🤖':TOKENS[sp2[i].token]}</div>
        <div class="pc-header-info">
          <div class="pc-player-num">Spieler ${i+1}</div>
          <input class="pc-name-input" id="pname${i}" type="text" value="${sp2[i].name}" maxlength="14"
            placeholder="Name eingeben…" ${sp2[i].isAI?'disabled':''}
            oninput="sp2[${i}].name=this.value">
        </div>
      </div>
      <div class="pc-section-label">Spielfigur</div>
      <div class="tok-grid" id="tr${i}"></div>
      <div class="pc-section-label">Farbe</div>
      <div class="color-grid" id="cp${i}"></div>
      <div class="ai-btn ${sp2[i].isAI?'on':''}" id="aibtn${i}" onclick="toggleAI(${i})"> KI-Spieler</div>`;

    pr.appendChild(card);

    // Token buttons
    const tr=card.querySelector('#tr'+i);
    TOKENS.forEach((tk,ti)=>{
      const b=document.createElement('button');
      b.className='tok-btn'+(sp2[i].token===ti?' sel':'');
      b.textContent=tk;b.title=TNAMES[ti];
      b.onclick=()=>{
        if(sp2[i].isAI)return;
        sp2[i].token=ti;
        tr.querySelectorAll('.tok-btn').forEach((x,j)=>x.classList.toggle('sel',j===ti));
        document.getElementById('pav'+i).textContent=TOKENS[ti];
      };
      tr.appendChild(b);
    });

    // Color swatches
    const cp=card.querySelector('#cp'+i);
    ALL_COLORS.forEach((c,ci)=>{
      const sw=document.createElement('div');
      sw.className='color-swatch'+(sp2[i].color===c?' sel':'');
      sw.style.background=c;sw.title=c;
      sw.onclick=()=>{
        sp2[i].color=c;
        cp.querySelectorAll('.color-swatch').forEach(x=>x.classList.remove('sel'));
        sw.classList.add('sel');
        document.getElementById('pav'+i).style.background=c;
      };
      cp.appendChild(sw);
    });
  }
}

function toggleAI(i){
  sp2[i].isAI=!sp2[i].isAI;
  if(sp2[i].isAI) sp2[i].name='KI-'+TOKENS[sp2[i].token];
  buildSetup();
}

function startGame(){
  RULES.timer=document.getElementById('r-timer').checked?parseInt(document.getElementById('r-timer-sec').value)||90:0;
  RULES.auction=document.getElementById('r-auction').checked;
  RULES.pool=document.getElementById('r-pool').checked;
  RULES.undo=document.getElementById('r-undo').checked;
  RULES.sound=document.getElementById('r-sound').checked;
  RULES.startMoney=parseInt(document.getElementById('r-start-money').value)||1500;

  const players=sp2.map((p,i)=>({
    name:(p.name||`Spieler ${i+1}`).trim()||`Spieler ${i+1}`,
    token:p.token%TOKENS.length,
    color:p.color||ALL_COLORS[i%ALL_COLORS.length],
    isAI:p.isAI||false
  }));
  initG(players);
  moving=false;
  document.getElementById('setup').style.display='none';
  document.getElementById('game').style.display='flex';
  document.getElementById('b-undo').style.display=RULES.undo?'block':'none';
  buildBoard();
  buildAnimTokens();
  renderAll();
  gLog('Spiel gestartet! '+G.pl[0].name+' beginnt.','info');
  autoSave();
  showTurnBanner(G.pl[0],()=>{
    if(G.pl[0].isAI)setTimeout(aiTakeTurn,800);
    else startTimer();
  });
}

/* ═══════════ TIMER ═══════════ */
function startTimer(){
  if(!RULES.timer)return;
  stopTimer();
  timerLeft=RULES.timer;
  document.getElementById('turn-timer').style.display='flex';
  updateTimerUI();
  timerInt=setInterval(()=>{
    timerLeft--;updateTimerUI();
    if(timerLeft<=0){
      stopTimer();
      gLog('⏱ Zeit abgelaufen!','warn');toast('⏱ Zeit abgelaufen!');
      if(!G.rolled)G.rolled=true;
      endTurn();
    }
  },1000);
}
function stopTimer(){
  clearInterval(timerInt);timerInt=null;
  document.getElementById('turn-timer').style.display='none';
}
function updateTimerUI(){
  const pct=timerLeft/RULES.timer*100;
  const bar=document.getElementById('timer-bar');
  bar.style.width=pct+'%';
  document.getElementById('timer-txt').textContent=timerLeft;
  bar.className=pct<25?'warn':'';
}

/* ═══════════ UNDO ═══════════ */
function saveUndoState(){
  if(!RULES.undo)return;
  undoState=JSON.parse(JSON.stringify({
    pl:G.pl,sp:G.sp,cur:G.cur,rolled:G.rolled,
    doubleCount:G.doubleCount,pool:G.pool,
    chanceI:G.chanceI,chestI:G.chestI,lastDice:G.lastDice,turn:G.turn
  }));
}
function doUndo(){
  if(!undoState||moving)return;
  G.pl=undoState.pl;G.sp=undoState.sp;G.cur=undoState.cur;
  G.rolled=undoState.rolled;G.doubleCount=undoState.doubleCount;
  G.pool=undoState.pool;G.chanceI=undoState.chanceI;
  G.chestI=undoState.chestI;G.lastDice=undoState.lastDice;
  G.turn=undoState.turn;undoState=null;
  document.getElementById('die1').textContent='🎲';
  document.getElementById('die2').textContent='🎲';
  document.getElementById('dice-info').textContent='Bereit zum Würfeln…';
  document.getElementById('dice-result-big').textContent='';
  document.getElementById('dice-double-banner').classList.remove('on');
  G.pl.forEach(p=>{if(!p.bust)placeAnimTok(p.id,p.pos);});
  renderAll();
  gLog('🔄 Zug rückgängig.','info');toast('🔄 Rückgängig!');
  document.getElementById('b-undo').disabled=true;
  startTimer();
}

/* ═══════════ BUILD BOARD ═══════════ */
function buildBoard(){
  const board=document.getElementById('board');
  // Remove old cells (keep corners)
  board.querySelectorAll('.cell').forEach(e=>e.remove());
  board.querySelectorAll('.tok-wrap').forEach(e=>e.remove());
  const S=700,C=80,CW=60,CH=80;
  BOARD.forEach(def=>{
    if([0,10,20,30].includes(def.id))return;
    let x,y,w,h;
    if(def.side==='B'){const idx=def.id-1;x=C+(8-idx)*CW;y=S-C;w=CW;h=CH;}
    else if(def.side==='L'){const idx=def.id-11;x=0;y=S-C-(idx+1)*CW;w=CH;h=CW;}
    else if(def.side==='T'){const idx=def.id-21;x=C+idx*CW;y=0;w=CW;h=CH;}
    else{const idx=def.id-31;x=S-C;y=C+idx*CW;w=CH;h=CW;}
    const el=document.createElement('div');
    el.className='cell'+(def.type==='chance'?' ch-cell':def.type==='chest'?' cm-cell':def.type==='tax'?' tx-cell':def.type==='rail'?' rl-cell':def.type==='utility'?' ut-cell':'');
    el.style.cssText=`left:${x}px;top:${y}px;width:${w}px;height:${h}px`;
    el.dataset.pos=def.id;el.onclick=()=>showSpaceInfo(def.id);el.title=def.name;
    const ci=document.createElement('div');ci.className='ci ci-'+def.side;ci.id='ci'+def.id;
    if(def.type==='prop'){const band=document.createElement('div');band.className='cband';band.style.background=def.color;ci.appendChild(band);}
    const body=document.createElement('div');body.className='cbody';
    if(def.type==='prop')body.innerHTML=`<div class="cname">${def.name}</div><div class="cprice">M ${def.price}</div>`;
    else if(def.type==='rail')body.innerHTML=`<div class="cicon">🚂</div><div class="cname">${def.name}</div><div class="cprice">M 200</div>`;
    else if(def.type==='utility')body.innerHTML=`<div class="cicon">${def.name.includes('Elek')?'⚡':'💧'}</div><div class="cname">${def.name}</div><div class="cprice">M 150</div>`;
    else if(def.type==='chance')body.innerHTML=`<div class="cicon">❓</div><div class="cname" style="color:#b04400">EREIGNIS</div>`;
    else if(def.type==='chest')body.innerHTML=`<div class="cicon">📋</div><div class="cname" style="color:#0d3d8a">GEMEIN-SCHAFT</div>`;
    else if(def.type==='tax')body.innerHTML=`<div class="cicon">💸</div><div class="cname">${def.name}</div><div class="cprice">M ${def.amount}</div>`;
    ci.appendChild(body);
    const od=document.createElement('div');od.className='own-dot';od.id='od'+def.id;ci.appendChild(od);
    const hr=document.createElement('div');hr.className='hrow';hr.id='hr'+def.id;ci.appendChild(hr);
    const mo=document.createElement('div');mo.className='mortg-ov';mo.id='mo'+def.id;mo.innerHTML='<span>HYPO</span>';ci.appendChild(mo);
    el.appendChild(ci);
    const tw=document.createElement('div');tw.className='tok-wrap';tw.id='tw'+def.id;el.appendChild(tw);
    board.appendChild(el);
  });
  [0,10,20,30].forEach(id=>{
    const c=document.getElementById('c'+id);if(!c)return;
    const tw=document.createElement('div');tw.className='tok-wrap';tw.id='tw'+id;c.appendChild(tw);
  });
}

/* ═══════════ ANIMATED TOKENS ═══════════ */
function buildAnimTokens(){
  document.querySelectorAll('.anim-tok').forEach(e=>e.remove());
  G.pl.forEach(p=>{
    const el=document.createElement('div');
    el.className='anim-tok'+(p.isAI?' ai-player':'');
    el.id='atk'+p.id;el.style.background=p.color;el.textContent=TOKENS[p.token];
    const c=getSpaceCenter(0);const off=p.id*5;
    el.style.left=(c.x-13+off%10)+'px';el.style.top=(c.y-13+Math.floor(off/10)*5)+'px';
    el.style.display='flex';
    document.getElementById('board').appendChild(el);
  });
}
function getAnimTokEl(pid){return document.getElementById('atk'+pid);}
function placeAnimTok(pid,spaceId){
  const el=getAnimTokEl(pid);if(!el)return;
  const c=getSpaceCenter(spaceId);const off=pid*5;
  el.style.transition='none';
  el.style.left=(c.x-13+off%10)+'px';el.style.top=(c.y-13+Math.floor(off/10)*5)+'px';
  el.style.display='flex';
}
function animateMove(pid,fromPos,toPos,onDone){
  const el=getAnimTokEl(pid);if(!el){onDone();return;}
  const steps=[];
  if(fromPos!==toPos){let cur=fromPos;while(cur!==toPos){cur=(cur+1)%40;steps.push(cur);}}
  if(steps.length===0){el.style.display='flex';onDone();return;}
  el.style.display='flex';
  const MI=document.getElementById('move-indicator');MI.style.display='block';

  // ── 3D Zoom tracking ──
  const BOARD_SIZE=700;
  const wrap=document.getElementById('board-3d-wrap');
  const BASE_RX=18,BASE_RZ=-1.5;
  // Pause parallax while moving
  window._zoomTracking=true;

  function getCenterNorm(spaceId){
    // Returns {nx,ny} normalized 0-1 from board top-left
    const c=getSpaceCenter(spaceId);
    return {nx:c.x/BOARD_SIZE,ny:c.y/BOARD_SIZE};
  }

  function applyZoom(spaceId,progress){
    // progress 0=start 1=end; zoom builds up then releases at end
    const {nx,ny}=getCenterNorm(spaceId);
    // Tilt board so the token's space faces the viewer
    // Map 0-1 → tilt: center=0 tilt, edges=max tilt
    const tiltX=BASE_RX + (ny-0.5)*(-14);   // top spaces tilt forward
    const tiltZ=BASE_RZ + (nx-0.5)*(10);    // left/right lean
    // Zoom scale: peak at mid-journey
    const zoomScale=1+Math.sin(progress*Math.PI)*0.18;
    // Translate to keep token centered in view
    const shiftX=(0.5-nx)*80;
    const shiftY=(0.5-ny)*60;
    if(wrap){
      wrap.style.transition='transform 0.22s cubic-bezier(.4,0,.2,1)';
      wrap.style.transform=
        `rotateX(${tiltX.toFixed(2)}deg) `+
        `rotateZ(${tiltZ.toFixed(2)}deg) `+
        `scale(${zoomScale.toFixed(3)}) `+
        `translate(${shiftX.toFixed(1)}px,${shiftY.toFixed(1)}px)`;
    }
  }

  // Add tracking glow to token
  el.classList.add('tracking');

  let i=0;
  let prevCellEl=null;
  function step(){
    if(i>=steps.length){
      MI.style.display='none';
      // Remove tracking glow
      el.classList.remove('tracking');
      // Flash the final cell
      const finalCellEl=document.querySelector(`[data-pos="${toPos}"]`);
      if(finalCellEl){
        finalCellEl.classList.remove('tok-landing');void finalCellEl.offsetWidth;
        finalCellEl.classList.add('tok-landing');
        setTimeout(()=>finalCellEl.classList.remove('tok-landing'),800);
      }
      // Smoothly return to base position
      if(wrap){
        wrap.style.transition='transform 0.7s cubic-bezier(.25,.46,.45,.94)';
        wrap.style.transform=`rotateX(${BASE_RX}deg) rotateZ(${BASE_RZ}deg)`;
      }
      window._zoomTracking=false;
      onDone();
      return;
    }
    const sp=steps[i];
    MI.textContent='→ '+BOARD[sp].name;
    playSound('move');
    const c=getSpaceCenter(sp);const off=pid*5;
    el.style.transition='left .22s cubic-bezier(.4,0,.2,1),top .22s cubic-bezier(.4,0,.2,1)';
    el.style.left=(c.x-15+off%10)+'px';el.style.top=(c.y-15+Math.floor(off/10)*5)+'px';
    el.classList.remove('tok-jump');void el.offsetWidth;el.classList.add('tok-jump');
    // Flash each cell as token passes through
    if(prevCellEl) prevCellEl.classList.remove('tok-landing');
    const curCellEl=document.querySelector(`[data-pos="${sp}"]`);
    if(curCellEl){curCellEl.classList.add('tok-landing');prevCellEl=curCellEl;}
    // Apply zoom tracking
    applyZoom(sp, i/steps.length);
    i++;setTimeout(step,240);
  }
  step();
}

/* ═══════════ RENDER ═══════════ */
function renderAll(){
  renderStaticToks();renderSidebar();renderCurPlayer();renderOwners();
}
function renderStaticToks(){
  document.querySelectorAll('.tok-wrap').forEach(el=>el.innerHTML='');
  G.pl.forEach(p=>{
    if(p.bust)return;
    const tw=document.getElementById('tw'+p.pos);if(!tw)return;
    const d=document.createElement('div');
    d.className='btok'+(p.jailed?' jailed':'');
    d.style.background=p.color;d.textContent=TOKENS[p.token];d.title=p.name;
    tw.appendChild(d);
  });
}
function renderSidebar(){
  const list=document.getElementById('pl-list');list.innerHTML='';
  const alive=G.pl.filter(p=>!p.bust);
  const maxW=Math.max(...alive.map(p=>totalWealth(p)),1);
  G.pl.forEach((p,i)=>{
    const w=totalWealth(p);
    const pct=p.bust?0:Math.round(w/maxW*100);
    const props=G.sp.filter(s=>s.own===p.id).length;
    const el=document.createElement('div');
    el.className='pl-item'+(i===G.cur?' active':'')+(p.bust?' bust':'');
    el.onclick=()=>showPlayerCard(p.id);
    el.innerHTML=`
      <div class="pl-tsm" style="background:${p.color}">${TOKENS[p.token]}</div>
      <div class="pl-info">
        <div class="pl-name">${p.name}${p.bust?' 💀':''}${p.isAI?'<span class="ai-tag">KI</span>':''}${p.jailed&&!p.bust?' 🔒':''}</div>
        <div class="pl-money">M ${p.money.toLocaleString('de-DE')}${props>0?` <span style="font-size:8px;color:#888">🏘${props}</span>`:''}</div>
        <div class="pl-wealth-bar"><div class="pl-wealth-fill" style="width:${pct}%;background:${p.color}"></div></div>
      </div>`;
    list.appendChild(el);
  });
}
function renderCurPlayer(){
  const p=G.pl[G.cur];
  const tok=document.getElementById('cp-tok');tok.textContent=TOKENS[p.token];tok.style.background=p.color;
  document.getElementById('cp-name').textContent=p.name+(p.isAI?' 🤖':'');
  document.getElementById('cp-money').textContent='M '+p.money.toLocaleString('de-DE');
  document.getElementById('cp-wealth').textContent='Gesamtvermögen: M '+totalWealth(p).toLocaleString('de-DE');
  document.getElementById('cp-pos').textContent=p.jailed?`🔒 Gefängnis (Runde ${p.jailRounds}/3)`:BOARD[p.pos].name;
  _refreshButtons();
}
function _refreshButtons(){
  const p=G.pl[G.cur];const mv=moving;
  const rollBtn=document.getElementById('b-roll');
  const endBtn=document.getElementById('b-end');
  rollBtn.disabled=(!G.rolled?mv:true);
  endBtn.disabled=(G.rolled?mv:true);
  document.getElementById('b-house').disabled=mv;
  document.getElementById('b-trade').disabled=mv;
  const undoBtn=document.getElementById('b-undo');
  if(undoBtn)undoBtn.disabled=!undoState||mv||G.rolled;
  const bail=document.getElementById('b-bail');
  if(p.jailed){
    bail.style.display='block';
    bail.disabled=(p.money<50&&!p.jailCard)||mv;
    bail.textContent=p.jailCard?'🎟 Freikarte benutzen':'⛓ Kaution M 50';
    if(!G.rolled)rollBtn.disabled=mv;
    rollBtn.textContent='🎲 WÜRFELN (Knast)';
  }else{
    bail.style.display='none';rollBtn.textContent='🎲 WÜRFELN';
  }
}
function renderOwners(){
  // Pre-compute which color groups are fully owned (monopoly)
  const monopolyColors=new Set();
  const colorGroups={};
  G.sp.forEach(s=>{
    const d=BOARD[s.id];
    if(s.type!=='prop'||!d.color)return;
    if(!colorGroups[d.color])colorGroups[d.color]={cells:[],owner:s.own,mono:true};
    colorGroups[d.color].cells.push(s);
    if(s.own===null||s.own!==colorGroups[d.color].owner)colorGroups[d.color].mono=false;
  });
  Object.entries(colorGroups).forEach(([col,grp])=>{
    if(grp.mono&&grp.owner!==null)monopolyColors.add(col);
  });

  G.sp.forEach(s=>{
    const od=document.getElementById('od'+s.id);
    const hr=document.getElementById('hr'+s.id);
    const mo=document.getElementById('mo'+s.id);
    const ciEl=document.getElementById('ci'+s.id);
    const cellEl=ciEl?ciEl.closest('.cell'):null;
    if(!od)return;

    // Ownership dot
    if(s.own!==null){
      od.style.display='block';od.style.background=G.pl[s.own].color;
      if(mo)mo.style.display=s.mortg?'flex':'none';
    }else{
      od.style.display='none';if(mo)mo.style.display='none';
    }

    // Monopoly glow on cell + band shimmer
    const def=BOARD[s.id];
    const isMonopoly=def.type==='prop'&&def.color&&monopolyColors.has(def.color);
    if(cellEl){
      cellEl.classList.toggle('monopoly-owned',isMonopoly&&!s.mortg);
    }
    if(ciEl){
      const band=ciEl.querySelector('.cband');
      if(band)band.classList.toggle('monopoly-band',isMonopoly&&!s.mortg);
    }

    // 3D Houses & Hotels
    if(hr){
      hr.innerHTML='';
      if(s.h>0&&!s.mortg){
        if(s.h===5){
          // Hotel
          const d=document.createElement('div');
          d.className='hotel-3d';
          d.title='🏨 Hotel';
          hr.appendChild(d);
        }else{
          // Houses — stacked mini 3D
          for(let i=0;i<s.h;i++){
            const d=document.createElement('div');
            d.className='house-3d';
            d.title=`🏠 Haus ${i+1}`;
            hr.appendChild(d);
          }
        }
      }
    }
  });
}

/* ═══════════ FLOATING MONEY ═══════════ */
function floatMoney(amount,positive){
  const el=document.createElement('div');
  el.className='float-money '+(positive?'gain':'lose');
  el.textContent=(positive?'+':'-')+' M '+Math.abs(amount).toLocaleString('de-DE');
  el.style.left=Math.random()*60+20+'vw';
  el.style.top=Math.random()*30+30+'vh';
  document.body.appendChild(el);
  setTimeout(()=>el.remove(),1000);
}

/* ═══════════ MONEY ═══════════ */
function addM(p,a){
  p.money+=a;
  gLog(`${p.name} erhält M ${a}.`,'money');
  floatMoney(a,true);playSound('money');
}
function spendM(p,a){
  p.money-=a;
  gLog(`${p.name} zahlt M ${a}.`,'warn');
  floatMoney(a,false);
  if(p.money<0)doBankrupt(p);
}
function transfer(from,to,a){
  from.money-=a;to.money+=a;
  gLog(`${from.name} → M ${a} → ${to.name}.`,'money');
  floatMoney(a,true);
  if(from.money<0)doBankrupt(from);
}
function doBankrupt(p){
  // Sell houses first
  G.sp.filter(s=>s.own===p.id&&s.h>0).forEach(s=>{p.money+=Math.floor(BOARD[s.id].hp/2)*s.h;s.h=0;});
  if(p.money>=0){gLog(`${p.name} rettet sich (M ${p.money}).`,'info');renderAll();return;}
  // Mortgage properties
  G.sp.filter(s=>s.own===p.id&&!s.mortg).forEach(s=>{p.money+=Math.floor(BOARD[s.id].price/2);s.mortg=true;});
  if(p.money>=0){gLog(`${p.name} rettet sich per Hypothek.`,'info');renderAll();return;}
  // Bankrupt
  p.bust=true;p.money=0;
  G.sp.filter(s=>s.own===p.id).forEach(s=>{s.own=null;s.h=0;s.mortg=false;});
  const tok=getAnimTokEl(p.id);if(tok)tok.style.display='none';
  gLog(`${p.name} ist bankrott! 💀`,'warn');toast(`${p.name} ist bankrott! 💀`);
  checkWin();
}
function checkWin(){
  const alive=G.pl.filter(p=>!p.bust);
  if(alive.length===1){
    const w=alive[0];
    playSound('win');
    const winTok=document.getElementById('win-tok');
    if(winTok){winTok.textContent=TOKENS[w.token];winTok.style.background=w.color;}
    document.getElementById('wn').textContent=w.name;
    document.getElementById('wm').textContent='Vermögen: M '+totalWealth(w).toLocaleString('de-DE');
    const ws=document.getElementById('win-stats');ws.innerHTML='';
    [{label:'Runden gespielt',val:G.turn},{label:'Miete kassiert',val:'M '+w.stats.totalRent.toLocaleString('de-DE')},{label:'Grundstücke',val:G.sp.filter(s=>s.own===w.id).length}].forEach(d=>{
      ws.innerHTML+=`<div class="win-stat"><strong>${d.val}</strong>${d.label}</div>`;
    });
    document.getElementById('winscr').classList.add('on');
    autoSave(true);
  }
}
function totalWealth(p){
  if(!G.sp)return p.money;
  return p.money+G.sp.filter(s=>s.own===p.id).reduce((sum,s)=>{
    const d=BOARD[s.id];
    return sum+(s.mortg?Math.floor(d.price/2):d.price)+(s.h<5?s.h*(d.hp||0):5*(d.hp||0));
  },0);
}

/* ═══════════ DICE & MOVEMENT ═══════════ */
function rollDice(){
  if(moving)return;
  const p=G.pl[G.cur];
  if(p.jailed){jailRoll();return;}
  if(G.rolled)return;
  saveUndoState();
  document.getElementById('b-roll').disabled=true;
  document.getElementById('b-undo').disabled=true;
  document.getElementById('dice-double-banner').classList.remove('on');
  playSound('roll');
  spinDice((d1,d2)=>{
    G.lastDice=[d1,d2];
    const tot=d1+d2,dbl=d1===d2;
    if(dbl){
      G.doubleCount++;playSound('double');
      if(G.doubleCount>=3){
        gLog(`${p.name} — 3× Pasch → Gefängnis!`,'warn');
        G.rolled=true;goJail(p,()=>enableEnd());G.doubleCount=0;return;
      }
      gLog(`${p.name} — Pasch (${d1}+${d2})! Noch einmal würfeln.`,'info');
    }else{G.doubleCount=0;}
    gLog(`${p.name} würfelt ${d1}+${d2}=${tot}.`,'action');
    doMove(p,tot,dbl);
  });
}
function jailRoll(){
  if(moving)return;
  document.getElementById('b-roll').disabled=true;
  const p=G.pl[G.cur];
  playSound('roll');
  spinDice((d1,d2)=>{
    G.lastDice=[d1,d2];const tot=d1+d2,dbl=d1===d2;
    if(dbl){
      p.jailed=false;p.jailRounds=0;
      gLog(`${p.name} — Pasch! Frei! 🎉`,'info');toast(`${p.name} ist frei! 🎉`);
      G.rolled=true;doMove(p,tot,false);
    }else{
      p.jailRounds++;
      if(p.jailRounds>=3){
        spendM(p,50);p.jailed=false;p.jailRounds=0;
        gLog(`${p.name} verlässt Gefängnis (Kaution M 50).`,'warn');
        G.rolled=true;doMove(p,tot,false);
      }else{
        gLog(`${p.name} — kein Pasch. Im Gefängnis (${p.jailRounds}/3).`,'info');
        G.rolled=true;renderAll();enableEnd();
      }
    }
  });
}
function spinDice(cb){
  const d1e=document.getElementById('die1'),d2e=document.getElementById('die2');
  const dblBanner=document.getElementById('dice-double-banner');
  dblBanner.classList.remove('on');
  d1e.className='die rolling';d2e.className='die rolling';
  setTimeout(()=>{
    const d1=Math.floor(Math.random()*6)+1,d2=Math.floor(Math.random()*6)+1;
    d1e.textContent=DFACES[d1-1];d2e.textContent=DFACES[d2-1];
    d1e.className='die';d2e.className='die';
    document.getElementById('dice-info').textContent=`${d1} + ${d2} = ${d1+d2}`;
    document.getElementById('dice-result-big').textContent=`Summe: ${d1+d2}`;
    if(d1===d2)dblBanner.classList.add('on');
    cb(d1,d2);
  },800);
}
function doMove(p,steps,dbl){
  const oldPos=p.pos,newPos=(oldPos+steps)%40,passedGO=(oldPos+steps)>=40;
  setMoving(true);
  animateMove(p.id,oldPos,newPos,()=>{
    p.pos=newPos;
    p.stats.spaceLanded[newPos]=(p.stats.spaceLanded[newPos]||0)+1;
    if(passedGO){addM(p,200);gLog(`${p.name} passiert LOS → +M 200!`,'money');}
    gLog(`→ ${BOARD[p.pos].name}`,'action');
    G.rolled=true;setMoving(false);renderAll();
    setTimeout(()=>land(p,dbl),250);
  });
}
function land(p,dbl){
  if(p.bust)return;
  const s=G.sp[p.pos],def=BOARD[p.pos];
  switch(def.type){
    case 'go':if(!dbl)enableEnd();else enableReroll();break;
    case 'prop':case 'rail':case 'utility':
      if(!s.mortg&&s.own===null){
        if(p.money>=def.price)showBuy(p,s,def,dbl);
        else if(RULES.auction){gLog(`${p.name} kann ${def.name} nicht kaufen. Auktion!`,'info');runAuction(s,def,dbl);}
        else{if(!dbl)enableEnd();else enableReroll();}
      }else if(!s.mortg&&s.own!==null&&s.own!==p.id){
        payRent(p,s,def,dbl);
      }else{if(!dbl)enableEnd();else enableReroll();}
      break;
    case 'tax':
      gLog(`${p.name} zahlt Steuer M ${def.amount}.`,'warn');
      if(RULES.pool)G.pool+=def.amount;
      spendM(p,def.amount);renderAll();
      if(!dbl)enableEnd();else enableReroll();break;
    case 'chance':drawCard('chance',p,dbl);break;
    case 'chest':drawCard('chest',p,dbl);break;
    case 'jail':gLog(`${p.name} — nur zu Besuch.`,'info');if(!dbl)enableEnd();else enableReroll();break;
    case 'gotojail':G.rolled=true;goJail(p,()=>enableEnd());break;
    case 'parking':
      if(RULES.pool&&G.pool>0){addM(p,G.pool);gLog(`${p.name} kassiert Freiparkpool M ${G.pool}!`,'money');G.pool=0;}
      else gLog(`${p.name} parkt kostenlos.`,'info');
      renderAll();if(!dbl)enableEnd();else enableReroll();break;
    default:if(!dbl)enableEnd();else enableReroll();
  }
}
function enableEnd(){
  document.getElementById('b-end').disabled=false;
  document.getElementById('b-roll').disabled=true;
}
function enableReroll(){
  G.rolled=false;
  document.getElementById('b-roll').disabled=false;
  document.getElementById('b-end').disabled=true;
  _refreshButtons();
}
function endTurn(){
  if(!G.rolled||moving)return;
  stopTimer();
  undoState=null;
  if(!G.wealthHistory)G.wealthHistory=[];
  G.wealthHistory.push({turn:G.turn,wealth:G.pl.map(p=>totalWealth(p))});
  G.turn++;
  G.pl[G.cur].stats.turns++;
  let n=(G.cur+1)%G.pl.length;
  while(G.pl[n].bust&&n!==G.cur)n=(n+1)%G.pl.length;
  G.cur=n;G.rolled=false;G.doubleCount=0;
  document.getElementById('die1').textContent='🎲';
  document.getElementById('die2').textContent='🎲';
  document.getElementById('dice-info').textContent='Bereit zum Würfeln…';
  document.getElementById('dice-result-big').textContent='';
  document.getElementById('dice-double-banner').classList.remove('on');
  renderAll();
  gLog('— '+G.pl[G.cur].name+' ist dran —','info');
  autoSave();
  showTurnBanner(G.pl[G.cur],()=>{
    if(G.pl[G.cur].isAI)setTimeout(aiTakeTurn,800);
    else startTimer();
  });
}

/* ═══════════ JAIL ═══════════ */
function goJail(p,onDone){
  p.jailed=true;p.jailRounds=0;playSound('jail');
  setMoving(true);
  animateMove(p.id,p.pos,10,()=>{
    p.pos=10;setMoving(false);renderAll();
    gLog(`${p.name} → Gefängnis! 🔒`,'warn');toast(`${p.name} ins Gefängnis! 🔒`);
    if(onDone)onDone();
  });
}
function payBail(){
  if(moving)return;
  const p=G.pl[G.cur];if(!p.jailed)return;
  if(p.jailCard>0){p.jailCard--;p.jailed=false;p.jailRounds=0;gLog(`${p.name} nutzt Freikarte.`,'info');toast('Frei! 🎟');}
  else{if(p.money<50){toast('Nicht genug Geld!');return;}spendM(p,50);p.jailed=false;p.jailRounds=0;gLog(`${p.name} zahlt Kaution M 50.`,'info');toast('Frei! ⛓');}
  renderAll();
}

/* ═══════════ CARD MOVE HELPER ═══════════ */
function animMoveTo(p,target,collectGO,onDone,backwards=false){
  if(collectGO&&!backwards&&target<=p.pos){addM(p,200);gLog(`${p.name} passiert LOS → +M 200!`,'money');}
  const old=p.pos;setMoving(true);
  if(backwards){
    const el=getAnimTokEl(p.id);
    if(el){
      const c=getSpaceCenter(target);const off=p.id*5;
      setTimeout(()=>{
        el.style.transition='left .35s ease,top .35s ease';
        el.style.left=(c.x-13+off%10)+'px';el.style.top=(c.y-13+Math.floor(off/10)*5)+'px';
        setTimeout(()=>{p.pos=target;setMoving(false);renderAll();gLog(`→ ${BOARD[target].name}`,'action');if(onDone)onDone();},400);
      },50);
    }else{p.pos=target;setMoving(false);renderAll();if(onDone)onDone();}
    return;
  }
  animateMove(p.id,old,target,()=>{
    p.pos=target;setMoving(false);renderAll();
    gLog(`→ ${BOARD[target].name}`,'action');
    if(onDone)setTimeout(onDone,200);
  });
}

/* ═══════════ TURN BANNER ═══════════ */
function showTurnBanner(p,cb){
  _turnBannerCb=cb;
  const tok=document.getElementById('tb-tok');
  tok.textContent=TOKENS[p.token];tok.style.background=p.color;
  tok.style.boxShadow=`0 0 60px rgba(0,0,0,.8),0 0 30px ${p.color}`;
  document.getElementById('tb-name').textContent=p.name+(p.isAI?' 🤖':'');
  document.getElementById('tb-name').style.color=p.color;
  document.getElementById('tb-money').textContent='Guthaben: M '+p.money.toLocaleString('de-DE');
  document.getElementById('turn-banner').classList.add('on');
  if(p.isAI)setTimeout(hideTurnBanner,1600);
}
function hideTurnBanner(){
  document.getElementById('turn-banner').classList.remove('on');
  if(_turnBannerCb){const cb=_turnBannerCb;_turnBannerCb=null;setTimeout(cb,100);}
}

/* ═══════════ CARD REVEAL ═══════════ */
function showCardReveal(type,text,cb){
  _cardRevealCb=cb;
  const card=document.getElementById('cr-card');
  const isChance=type==='chance';
  card.className='cr-card '+(isChance?'ev':'gem');
  card.style.animation='none';void card.offsetWidth;card.style.animation='';
  document.getElementById('cr-icon').textContent=isChance?'❓':'📋';
  document.getElementById('cr-type').textContent=isChance?'EREIGNISKARTE':'GEMEINSCHAFTSKARTE';
  document.getElementById('cr-text').textContent=text;
  document.getElementById('card-reveal').classList.add('on');
  playSound('roll');
}
function dismissCardReveal(){
  document.getElementById('card-reveal').classList.remove('on');
  if(_cardRevealCb){const cb=_cardRevealCb;_cardRevealCb=null;cb();}
}

/* ═══════════ DEED MODAL ═══════════ */
window._deedClosable=false;
function showDeed(p,s,def,dbl,onBuy,onPass){
  const col=def.color||'#555';
  let html='';
  if(def.type==='prop'){
    const isOwned=s.own!==null;
    const owner=isOwned?G.pl[s.own]:null;
    // Check monopoly
    const grpAll=G.sp.filter(x=>x.type==='prop'&&BOARD[x.id].color===def.color);
    const isMonopoly=isOwned&&grpAll.every(x=>x.own===s.own);
    html=`<div class="deed-band" style="background:${col}">
      <div class="deed-band-label">Titel-Urkunde</div>
      <div class="deed-band-title">${def.name}</div>
      ${isMonopoly?'<div class="deed-monopoly-badge">★ MONOPOL ★</div>':''}
    </div>
    <div class="deed-body">
      <div class="deed-name">${def.name}</div>
      <div class="deed-price-row">
        <span>Kaufpreis: <b>M ${def.price}</b></span>
        <span>Hypothek: <b>M ${Math.floor(def.price/2)}</b></span>
      </div>
      <table class="deed-tbl">
        <tr><td>Grundmiete</td><td>M ${def.rent[0]}</td></tr>
        <tr class="highlight"><td>🏘 Farbgruppe komplett</td><td>M ${def.rent[0]*2}</td></tr>
        <tr><td>🏠 1 Haus</td><td>M ${def.rent[1]}</td></tr>
        <tr><td>🏠🏠 2 Häuser</td><td>M ${def.rent[2]}</td></tr>
        <tr><td>🏠🏠🏠 3 Häuser</td><td>M ${def.rent[3]}</td></tr>
        <tr><td>🏠🏠🏠🏠 4 Häuser</td><td>M ${def.rent[4]}</td></tr>
        <tr><td>🏨 Hotel</td><td>M ${def.rent[5]}</td></tr>
      </table>
      <div class="deed-mortgage">Haus/Hotel-Bau: M ${def.hp} &nbsp;|&nbsp; Ablösung: M ${Math.ceil(def.price/2*1.1)}</div>
      ${isOwned?`<div class="deed-owner">
        <div style="width:28px;height:28px;border-radius:50%;background:${owner.color};display:inline-flex;align-items:center;justify-content:center;font-size:14px;border:2px solid rgba(255,255,255,.6);vertical-align:middle;margin-right:6px">${TOKENS[owner.token]}</div>
        <b>${owner.name}</b>${s.h>0?' · '+(s.h===5?'🏨 Hotel':s.h+' 🏠'):''}${s.mortg?' · ⚠ Hypotheziert':''}${isMonopoly?' · ★ Monopol':''}
      </div>`:''}
    </div>`;
  }else if(def.type==='rail'){
    html=`<div class="deed-band" style="background:#222">
      <div class="deed-band-label">BAHNHOF</div>
      <div class="deed-band-title">${def.name}</div>
    </div>
    <div class="deed-body">
      <div class="deed-name">${def.name}</div>
      <div class="deed-rail-icon">🚂</div>
      <table class="deed-rail-tbl">
        <tr><td>1 Bahnhof</td><td>M 25</td></tr>
        <tr><td>2 Bahnhöfe</td><td>M 50</td></tr>
        <tr><td>3 Bahnhöfe</td><td>M 100</td></tr>
        <tr><td>4 Bahnhöfe</td><td>M 200</td></tr>
      </table>
      <div class="deed-mortgage" style="margin-top:10px">Hypothek: M ${Math.floor(def.price/2)}</div>
    </div>`;
  }else if(def.type==='utility'){
    const isElec=def.name.includes('Elek');
    html=`<div class="deed-band" style="background:${isElec?'#e65100':'#0277bd'}">
      <div class="deed-band-label">VERSORGUNGSWERK</div>
      <div class="deed-band-title">${def.name}</div>
    </div>
    <div class="deed-body">
      <div class="deed-name">${isElec?'⚡':'💧'} ${def.name}</div>
      <div class="deed-rail-icon" style="font-size:44px">${isElec?'⚡':'💧'}</div>
      <table class="deed-rail-tbl">
        <tr><td>1 Versorgungswerk</td><td>4 × Würfel</td></tr>
        <tr><td>2 Versorgungswerke</td><td>10 × Würfel</td></tr>
      </table>
      <div class="deed-mortgage" style="margin-top:10px">Hypothek: M ${Math.floor(def.price/2)}</div>
    </div>`;
  }
  // Buttons
  const player=p||G.pl[G.cur];
  const canAfford=player&&player.money>=def.price;
  html+=`<div class="deed-actions">
    ${onBuy?`<button class="deed-btn buy" onclick="window._deedBuy()" ${canAfford?'':'style="opacity:.5;cursor:not-allowed"'}>✅ Kaufen – M${def.price}</button>`:''}
    ${onPass?`<button class="deed-btn pass" onclick="window._deedPass()">${RULES.auction?'🔨 Auktion':'❌ Ablehnen'}</button>`:''}
    ${(!onBuy&&!onPass)?`<button class="deed-btn pass" style="background:#444;color:#ccc;border-color:#666" onclick="closeDeed()">Schließen ×</button>`:''}
  </div>`;
  document.getElementById('deed-card').innerHTML=html;
  window._deedBuy=onBuy||null;
  window._deedPass=onPass||null;
  window._deedClosable=(!onBuy&&!onPass);
  document.getElementById('deed-modal').classList.add('on');
}
function closeDeed(){
  document.getElementById('deed-modal').classList.remove('on');
}

/* ═══════════ RENT SPLASH ═══════════ */
function showRentSplash(propName,owner,ownerColor,ownerToken,amount,cb){
  document.getElementById('rs-prop').textContent=propName;
  const tok=document.getElementById('rs-tok');tok.textContent=ownerToken;tok.style.background=ownerColor;
  document.getElementById('rs-owner').textContent=owner;
  document.getElementById('rs-amount').textContent=amount.toLocaleString('de-DE');
  const btn=document.getElementById('rs-ok');
  btn.onclick=()=>{document.getElementById('rent-splash').classList.remove('on');if(cb)cb();};
  document.getElementById('rent-splash').classList.add('on');
  playSound('rent');
}

/* ═══════════ CARDS ═══════════ */
function drawCard(type,p,dbl){
  const deck=type==='chance'?G.chanceDeck:G.chestDeck;
  const idx=type==='chance'?G.chanceI++:G.chestI++;
  const card=deck[idx%deck.length];
  gLog(`${p.name} zieht ${type==='chance'?'Ereignis':'Gemeinschaft'}karte.`,'card');
  showCardReveal(type,card.t,()=>{
    let done=false;
    function afterCard(){
      if(done)return;done=true;renderAll();
      if(p.bust)return;
      if(p.jailed)enableEnd();
      else if(!dbl)enableEnd();else enableReroll();
    }
    card.fn(p,G,afterCard);
  });
}

/* ═══════════ RENT ═══════════ */
function payRent(p,s,def,dbl){
  const own=G.pl[s.own];let rent=0;
  if(def.type==='rail'){const n=G.sp.filter(x=>x.type==='rail'&&x.own===s.own).length;rent=[25,50,100,200][n-1]||25;}
  else if(def.type==='utility'){const n=G.sp.filter(x=>x.type==='utility'&&x.own===s.own).length;const dv=G.lastDice[0]+G.lastDice[1];rent=n===1?dv*4:dv*10;}
  else{const grp=G.sp.filter(x=>x.type==='prop'&&BOARD[x.id].color===def.color);const full=grp.every(x=>x.own===s.own);rent=s.h===0?(full?def.rent[0]*2:def.rent[0]):def.rent[Math.min(s.h,5)];}
  p.stats.totalPaid+=rent;own.stats.totalRent+=rent;
  transfer(p,own,rent);renderAll();
  gLog(`${p.name} zahlt M ${rent} Miete an ${own.name}.`,'warn');
  showRentSplash(def.name,own.name,own.color,TOKENS[own.token],rent,()=>{
    if(!dbl)enableEnd();else enableReroll();
  });
}

/* ═══════════ BUY ═══════════ */
function showBuy(p,s,def,dbl){
  if(p.isAI){
    // AI decision
    const shouldBuy=p.money>(def.price*1.4)||Math.random()>.3;
    if(shouldBuy&&p.money>=def.price){
      spendM(p,def.price);s.own=p.id;
      gLog(`🤖 ${p.name} kauft ${def.name}.`,'ai');renderAll();
    }else{
      gLog(`🤖 ${p.name} kauft ${def.name} nicht.`,'ai');
      if(RULES.auction)runAuction(s,def,dbl);else{if(!dbl)enableEnd();else enableReroll();}
    }
    return;
  }
  playSound('buy');
  showDeed(p,s,def,dbl,
    ()=>{closeDeed();spendM(p,def.price);s.own=p.id;gLog(`${p.name} kauft ${def.name}.`,'money');renderAll();playSound('buy');if(!dbl)enableEnd();else enableReroll();},
    ()=>{closeDeed();if(RULES.auction)runAuction(s,def,dbl);else{if(!dbl)enableEnd();else enableReroll();}}
  );
}

function runAuction(s,def,dbl){
  /* ── Real-time sequential auction ──
     Each human gets a turn with a 12-second countdown.
     AI bids instantly. Players can raise or pass.
     Last bidder wins when everyone else passes. */
  const bidders=[...G.pl.filter(p=>!p.bust)];
  let highBid=Math.max(10,Math.floor(def.price*0.1));
  let highBidder=-1; // -1 = no winner yet
  const passed=new Set(); // ids that passed

  // Populate overlay header
  document.getElementById('auc-prop-name').textContent=def.name;
  const bar=document.getElementById('auc-color-bar');
  bar.style.background=def.color||'#555';
  bar.style.display=def.color?'block':'none';
  updateAucDisplay();
  document.getElementById('auction-overlay').classList.add('on');

  function updateAucDisplay(){
    document.getElementById('auc-amount').textContent='M '+highBid.toLocaleString('de-DE');
    const tokEl=document.getElementById('auc-leader-tok');
    const nameEl=document.getElementById('auc-leader-name');
    if(highBidder>=0){
      const w=G.pl[highBidder];
      tokEl.textContent=TOKENS[w.token];tokEl.style.background=w.color;
      nameEl.textContent=w.name;
    }else{tokEl.textContent='–';tokEl.style.background='#555';nameEl.textContent='Niemand';}
  }

  function renderBidderRows(curIdx){
    const cont=document.getElementById('auc-bidders');cont.innerHTML='';
    bidders.forEach((p,i)=>{
      const row=document.createElement('div');
      const isCur=i===curIdx;const hasPassed=passed.has(p.id);const isWinner=p.id===highBidder;
      row.className='auc-bidder-row'+(isCur?' current':'')+(hasPassed?' passed':'')+(isWinner&&!isCur?' winner':'');
      row.id='auc-row-'+p.id;
      const minBid=highBid+10;
      row.innerHTML=`
        <div class="auc-bidder-tok" style="background:${p.color}">${TOKENS[p.token]}</div>
        <div class="auc-bidder-info">
          <div class="auc-bidder-name">${p.name}${p.isAI?' 🤖':''}</div>
          <div class="auc-bidder-money">M ${p.money.toLocaleString('de-DE')}</div>
          <div class="auc-bidder-status" style="color:${hasPassed?'#f88':isWinner?'#4f8':'#888'}">
            ${hasPassed?'✗ Gepasst':isWinner?'★ Führend':'Warten…'}
          </div>
        </div>
        <div class="auc-bidder-btns" id="auc-btns-${p.id}">
          ${isCur&&!hasPassed&&!p.isAI&&p.money>=minBid?`
            <input class="auc-bid-input" id="auc-input-${p.id}" type="number" value="${minBid}" min="${minBid}" max="${p.money}" step="10">
            <button class="auc-btn-bid" onclick="auctionBid(${p.id})">Bieten</button>
            <button class="auc-btn-pass" onclick="auctionPass(${p.id})">Passen</button>
          `:(isCur&&!hasPassed&&!p.isAI&&p.money<minBid)?`
            <button class="auc-btn-pass" onclick="auctionPass(${p.id})">Passen (kein Geld)</button>
          `:''}
        </div>`;
      cont.appendChild(row);
    });
  }

  let auctionTimerInt=null;
  let timerLeft=12;

  function startAucTimer(idx,onExpire){
    clearInterval(auctionTimerInt);timerLeft=12;
    const fill=document.getElementById('auc-timer-fill');
    fill.style.transition='none';fill.style.width='100%';
    fill.className='auc-timer-fill';
    void fill.offsetWidth;
    fill.style.transition='width 1s linear';
    auctionTimerInt=setInterval(()=>{
      timerLeft--;
      fill.style.width=(timerLeft/12*100)+'%';
      if(timerLeft<=4)fill.className='auc-timer-fill warn';
      if(timerLeft<=0){clearInterval(auctionTimerInt);onExpire();}
    },1000);
  }
  function stopAucTimer(){clearInterval(auctionTimerInt);}

  let curIdx=0;

  window.auctionBid=function(pid){
    const input=document.getElementById('auc-input-'+pid);
    const amount=parseInt(input.value)||0;
    const p=G.pl[pid];
    if(amount<highBid+10){toast('Mindestgebot: M '+(highBid+10));return;}
    if(amount>p.money){toast('Nicht genug Geld!');return;}
    highBid=amount;highBidder=pid;
    gLog(`${p.name} bietet M ${amount} auf ${def.name}.`,'money');
    // Animate price
    const amtEl=document.getElementById('auc-amount');
    amtEl.classList.remove('bump');void amtEl.offsetWidth;amtEl.classList.add('bump');
    updateAucDisplay();
    stopAucTimer();nextBidder();
  };
  window.auctionPass=function(pid){
    passed.add(pid);
    gLog(`${G.pl[pid].name} passt.`,'info');
    stopAucTimer();nextBidder();
  };

  function nextBidder(){
    curIdx=(curIdx+1)%bidders.length;
    // Skip those who passed
    let loops=0;
    while(passed.has(bidders[curIdx].id)){
      curIdx=(curIdx+1)%bidders.length;
      loops++;
      if(loops>bidders.length)break;
    }
    // Check if only one active bidder left (or all passed)
    const active=bidders.filter(p=>!passed.has(p.id));
    if(active.length<=1){closeAuction();return;}
    processBidder();
  }

  function processBidder(){
    const p=bidders[curIdx];
    renderBidderRows(curIdx);
    if(p.isAI){
      // AI logic: bid up to 80% of property value if affordable
      const maxAI=Math.min(p.money,Math.floor(def.price*0.8));
      const minBid=highBid+10;
      const willBid=!passed.has(p.id)&&minBid<=maxAI&&Math.random()>0.38;
      setTimeout(()=>{
        if(willBid){
          const bid=Math.min(maxAI,minBid+Math.floor(Math.random()*3)*10);
          highBid=bid;highBidder=p.id;
          gLog(`🤖 ${p.name} bietet M ${bid}.`,'ai');
          const amtEl=document.getElementById('auc-amount');
          amtEl.classList.remove('bump');void amtEl.offsetWidth;amtEl.classList.add('bump');
          updateAucDisplay();
          renderBidderRows(curIdx);
        }else{
          passed.add(p.id);
          gLog(`🤖 ${p.name} passt.`,'ai');
        }
        nextBidder();
      },700+Math.random()*600);
    }else{
      // Human — start countdown
      startAucTimer(curIdx,()=>{
        // Time expired → auto-pass
        auctionPass(p.id);
      });
    }
  }

  function closeAuction(){
    stopAucTimer();
    document.getElementById('auction-overlay').classList.remove('on');
    if(highBidder>=0){
      const w=G.pl[highBidder];
      if(w.money>=highBid){
        spendM(w,highBid);s.own=highBidder;
        gLog(`🏆 ${w.name} gewinnt Auktion: ${def.name} für M ${highBid}!`,'money');
        toast(`${w.name} gewinnt ${def.name} für M ${highBid}! 🏆`);
        playSound('buy');renderAll();
      }
    }else{
      gLog(`${def.name} nicht verkauft (keine Gebote).`,'info');
    }
    if(!dbl)enableEnd();else enableReroll();
  }

  // Kick off
  processBidder();
}

/* ═══════════ AI PLAYER ═══════════ */
function aiTakeTurn(){
  const p=G.pl[G.cur];
  if(!p.isAI||p.bust)return;
  stopTimer();
  document.getElementById('ai-thinking').style.display='block';
  gLog(`🤖 ${p.name} denkt nach…`,'ai');
  setTimeout(()=>{
    document.getElementById('ai-thinking').style.display='none';
    if(p.jailed&&p.jailCard>0)payBail();
    else if(p.jailed&&p.money>200&&p.jailRounds>=2)payBail();
    setTimeout(()=>{
      if(p.jailed)jailRoll();
      else if(!G.rolled)rollDice();
    },500);
  },900+Math.random()*500);
}
function aiPostLand(p,dbl){
  setTimeout(()=>{
    aiBuildIfPossible(p);
    setTimeout(()=>{if(!dbl)endTurn();else setTimeout(aiTakeTurn,700);},600);
  },300);
}
function aiBuildIfPossible(p){
  const byColor={};
  G.sp.filter(s=>s.type==='prop'&&s.own===p.id).forEach(s=>{
    const d=BOARD[s.id];if(!byColor[d.color])byColor[d.color]=[];byColor[d.color].push(s);
  });
  Object.entries(byColor).forEach(([col,sps])=>{
    const all=G.sp.filter(s=>s.type==='prop'&&BOARD[s.id].color===col);
    if(all.every(s=>s.own===p.id)){
      sps.forEach(s=>{
        const d=BOARD[s.id];
        if(s.h<3&&p.money>d.hp*2&&!s.mortg){
          p.money-=d.hp;s.h++;
          gLog(`🤖 ${p.name} baut auf ${d.name}.`,'ai');
        }
      });
    }
  });
  renderAll();
}

// Hook AI into enableEnd/enableReroll
const _origEnableEnd=enableEnd;
function enableEnd(){
  _origEnableEnd();
  const p=G.pl[G.cur];
  if(p&&p.isAI){aiBuildIfPossible(p);setTimeout(endTurn,600);}
}
const _origEnableReroll=enableReroll;
function enableReroll(){
  _origEnableReroll();
  const p=G.pl[G.cur];
  if(p&&p.isAI)setTimeout(aiTakeTurn,800);
}

/* ═══════════ SPACE INFO ═══════════ */
function showSpaceInfo(pos){
  if(!G||!G.sp)return;
  const s=G.sp[pos],def=BOARD[pos];if(!def)return;
  if(def.type==='prop'||def.type==='rail'||def.type==='utility'){
    window._deedClosable=true;
    showDeed(null,s,def,false,null,null);return;
  }
  let html='';
  if(def.type==='go')html=`<div class="mbody" style="font-size:16px">🚦 Erhalte M 200 beim Passieren!</div>`;
  else if(def.type==='jail')html=`<div class="mbody">⛓ Gefängnis / Nur zu Besuch<br>Kaution: M 50 | Max. 3 Runden</div>`;
  else if(def.type==='parking')html=`<div class="mbody">🚗 Frei Parken — kostenlos.${RULES.pool?'<br>Pool: M '+G.pool:'  Kein Pool-Geld.'}</div>`;
  else if(def.type==='gotojail')html=`<div class="mbody">👮 Gehe direkt ins Gefängnis!</div>`;
  else if(def.type==='tax')html=`<div class="mbody">💸 ${def.name}<br><span style="font-size:28px;font-weight:700;color:#FFD700">M ${def.amount}</span></div>`;
  else if(def.type==='chance')html=`<div class="mbody">❓ Ereignis — ziehe eine Karte!</div>`;
  else if(def.type==='chest')html=`<div class="mbody">📋 Gemeinschaft — ziehe eine Karte!</div>`;
  else html=`<div class="mbody">${def.name}</div>`;
  showModal(def.name,html,[{l:'OK',c:'mb-ok',fn:closeModal}],true);
}

/* ═══════════ PLAYER CARD ═══════════ */
function showPlayerCard(pid){
  const p=G.pl[pid];
  const props=G.sp.filter(s=>s.own===pid);
  const railCount=props.filter(s=>s.type==='rail').length;
  const utilCount=props.filter(s=>s.type==='utility').length;
  let propsHtml='';
  const byColor={};
  props.filter(s=>s.type==='prop').forEach(s=>{const d=BOARD[s.id];if(!byColor[d.color])byColor[d.color]=[];byColor[d.color].push(s);});
  Object.entries(byColor).forEach(([col,sps])=>{
    const full=G.sp.filter(s=>s.type==='prop'&&BOARD[s.id].color===col).every(s=>s.own===pid);
    propsHtml+=`<div style="display:flex;align-items:center;gap:6px;margin-bottom:4px">
      <div style="width:10px;height:20px;background:${col};border-radius:2px;flex-shrink:0"></div>
      <div style="font-size:10px">${sps.map(s=>{const d=BOARD[s.id];return`<span>${d.name}${s.h>0?'('+(s.h===5?'🏨':s.h+'🏠')+')':''}</span>`;}).join(', ')}${full?' ✓':''}</div>
    </div>`;
  });
  if(railCount>0)propsHtml+=`<div style="font-size:10px;margin-bottom:3px">🚂 ${railCount} Bahnhof</div>`;
  if(utilCount>0)propsHtml+=`<div style="font-size:10px;margin-bottom:3px">⚡💧 ${utilCount} Versorgungswerk</div>`;
  showModal(`${TOKENS[p.token]} ${p.name}`,`
    <div style="text-align:left">
      <div class="mbody" style="margin-bottom:8px">💰 M ${p.money.toLocaleString('de-DE')} &nbsp;|&nbsp; Gesamtvermögen: <b>M ${totalWealth(p).toLocaleString('de-DE')}</b></div>
      <div style="font-size:9px;text-transform:uppercase;letter-spacing:2px;color:var(--gold);margin-bottom:8px">Grundstücke (${props.length})</div>
      ${propsHtml||'<div style="font-size:10px;color:#666">Keine</div>'}
      <div style="margin-top:10px;font-size:9px;color:#888">🎯 Runden: ${p.stats.turns} &nbsp;|&nbsp; 📥 Miete kassiert: M ${p.stats.totalRent.toLocaleString('de-DE')} &nbsp;|&nbsp; 📤 gezahlt: M ${p.stats.totalPaid.toLocaleString('de-DE')}</div>
    </div>`,
    [{l:'Schließen',c:'mb-ok',fn:closeModal}],true);
}

/* ═══════════ BUILD / MORTGAGE ═══════════ */
function openBuild(){
  const p=G.pl[G.cur];
  const mine=G.sp.filter(s=>s.type==='prop'&&s.own===p.id);
  if(!mine.length){showModal('🏠 Bauen','<div class="mbody">Keine Grundstücke im Besitz.</div>',[{l:'OK',c:'mb-ok',fn:closeModal}],true);return;}
  const byColor={};mine.forEach(s=>{const d=BOARD[s.id];if(!byColor[d.color])byColor[d.color]=[];byColor[d.color].push(s);});
  let html='';
  Object.entries(byColor).forEach(([col,sps])=>{
    const all=G.sp.filter(s=>s.type==='prop'&&BOARD[s.id].color===col);
    const full=all.every(s=>s.own===p.id);
    sps.forEach(s=>{
      const d=BOARD[s.id];
      html+=`<div class="brow">
        <div style="width:10px;height:38px;background:${col};border-radius:2px;flex-shrink:0"></div>
        <div style="flex:1;min-width:0">
          <div style="font-size:11px;font-weight:700">${d.name}</div>
          <div style="font-size:9px;color:#999">${s.h<5?s.h+' Häuser':'🏨 Hotel'} | Bau M ${d.hp}</div>
          ${!full?'<div style="font-size:9px;color:#f88">Gruppe unvollständig</div>':''}
        </div>
        <div style="display:flex;gap:4px;flex-wrap:wrap;justify-content:flex-end">
          ${full&&s.h<5&&!s.mortg?`<button class="mb mb-y" style="padding:5px 8px;font-size:10px" onclick="doBuild(${s.id})">+🏠 M${d.hp}</button>`:''}
          ${s.h>0?`<button class="mb" style="padding:5px 8px;font-size:10px;border-color:#f88;color:#f88" onclick="doSell(${s.id})">-🏠 M${Math.floor(d.hp/2)}</button>`:''}
          ${!s.mortg&&s.h===0?`<button class="mb" style="padding:5px 8px;font-size:10px;border-color:#888;color:#888" onclick="doMortg(${s.id})">Hyp M${Math.floor(d.price/2)}</button>`:''}
          ${s.mortg&&p.money>=Math.ceil(d.price/2*1.1)?`<button class="mb mb-y" style="padding:5px 8px;font-size:10px" onclick="doUnmortg(${s.id})">Ablösen M${Math.ceil(d.price/2*1.1)}</button>`:''}
        </div>
      </div>`;
    });
  });
  showModal('🏠 Bauen & Hypotheken',html,[{l:'Schließen',c:'mb-ok',fn:closeModal}],true);
}
function doBuild(id){const p=G.pl[G.cur],s=G.sp[id],d=BOARD[id];if(s.h>=5||p.money<d.hp){toast('Nicht möglich!');return;}spendM(p,d.hp);s.h++;gLog(`${p.name} baut auf ${d.name}.`,'money');renderAll();closeModal();openBuild();}
function doSell(id){const p=G.pl[G.cur],s=G.sp[id],d=BOARD[id];const v=Math.floor(d.hp/2);p.money+=v;s.h--;gLog(`${p.name} verkauft Haus (+M ${v}).`,'money');renderAll();closeModal();openBuild();}
function doMortg(id){const p=G.pl[G.cur],s=G.sp[id],d=BOARD[id];const v=Math.floor(d.price/2);p.money+=v;s.mortg=true;gLog(`${p.name} hypotheziert ${d.name} (+M ${v}).`,'warn');renderAll();closeModal();openBuild();}
function doUnmortg(id){const p=G.pl[G.cur],s=G.sp[id],d=BOARD[id];const v=Math.ceil(d.price/2*1.1);p.money-=v;s.mortg=false;gLog(`${p.name} löst Hypothek ab (-M ${v}).`,'money');renderAll();closeModal();openBuild();}

/* ═══════════ TRADE ═══════════ */
function openTrade(){
  const p=G.pl[G.cur];
  const others=G.pl.filter(x=>!x.bust&&x.id!==p.id&&!x.isAI);
  if(!others.length){showModal('🤝 Handel','<div class="mbody">Keine menschlichen Mitspieler verfügbar.</div>',[{l:'OK',c:'mb-ok',fn:closeModal}],true);return;}
  let html='<div style="text-align:left">';
  others.forEach(o=>{html+=`<div class="brow" style="cursor:pointer" onclick="tradeWith(${o.id})"><div style="width:30px;height:30px;border-radius:50%;background:${o.color};display:flex;align-items:center;justify-content:center;font-size:15px">${TOKENS[o.token]}</div><div style="flex:1"><div style="font-size:12px;font-weight:700">${o.name}</div><div style="font-size:10px;color:#999">M ${o.money.toLocaleString('de-DE')} | ${G.sp.filter(s=>s.own===o.id).length} Grundstücke</div></div><div style="color:#FFD700;font-size:18px">›</div></div>`;});
  html+='</div>';
  showModal('🤝 Handelspartner',html,[{l:'Abbrechen',c:'mb-n',fn:closeModal}],true);
}
function tradeWith(oid){
  closeModal();const p=G.pl[G.cur],o=G.pl[oid];
  const mp=G.sp.filter(s=>s.own===p.id&&s.h===0&&!s.mortg);
  const op=G.sp.filter(s=>s.own===o.id&&s.h===0&&!s.mortg);
  function propChecks(list,prefix){
    if(!list.length)return`<div style="font-size:10px;color:#666;padding:4px">Keine</div>`;
    return list.map(s=>`<label style="display:flex;align-items:center;gap:6px;padding:4px 6px;border-radius:4px;background:rgba(0,0,0,.3);margin-bottom:3px;cursor:pointer"><input type="checkbox" id="${prefix}${s.id}" style="accent-color:#FFD700;width:14px;height:14px"><span style="flex:1;font-size:10px">${BOARD[s.id].name}</span>${BOARD[s.id].color?`<span style="width:10px;height:10px;border-radius:2px;background:${BOARD[s.id].color};display:inline-block"></span>`:''}<span style="font-size:9px;color:#aaa">M${BOARD[s.id].price}</span></label>`).join('');
  }
  showModal(`🤝 ${p.name} ↔ ${o.name}`,`
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;text-align:left;font-size:11px">
      <div><div style="color:#FFD700;font-weight:700;margin-bottom:6px">${TOKENS[p.token]} ${p.name} bietet:</div>
        <div style="max-height:110px;overflow-y:auto;margin-bottom:5px">${propChecks(mp,'trp-p')}</div>
        <div style="color:#999;font-size:10px;margin-bottom:3px">+ Geld:</div>
        <input type="number" id="tr-mm" value="0" min="0" max="${p.money}" step="10" style="width:100%;background:#071507;color:#FFD700;border:1px solid #C8A000;padding:4px;border-radius:3px;font-size:12px">
        <div style="font-size:9px;color:#888;margin-top:2px">Guthaben: M ${p.money.toLocaleString('de-DE')}</div></div>
      <div><div style="color:#FFD700;font-weight:700;margin-bottom:6px">${TOKENS[o.token]} ${o.name} gibt:</div>
        <div style="max-height:110px;overflow-y:auto;margin-bottom:5px">${propChecks(op,'trp-o')}</div>
        <div style="color:#999;font-size:10px;margin-bottom:3px">+ Geld:</div>
        <input type="number" id="tr-om" value="0" min="0" max="${o.money}" step="10" style="width:100%;background:#071507;color:#FFD700;border:1px solid #C8A000;padding:4px;border-radius:3px;font-size:12px">
        <div style="font-size:9px;color:#888;margin-top:2px">Guthaben: M ${o.money.toLocaleString('de-DE')}</div></div>
    </div>`,
    [{l:'✅ Handel bestätigen',c:'mb-y',fn:()=>{
      const mm=parseInt(document.getElementById('tr-mm').value)||0;
      const om=parseInt(document.getElementById('tr-om').value)||0;
      if(mm>p.money){toast(`${p.name}: Nicht genug Geld!`);return;}
      if(om>o.money){toast(`${o.name}: Nicht genug Geld!`);return;}
      mp.filter(s=>document.getElementById('trp-p'+s.id)?.checked).forEach(s=>s.own=o.id);
      op.filter(s=>document.getElementById('trp-o'+s.id)?.checked).forEach(s=>s.own=p.id);
      p.money+=om-mm;o.money+=mm-om;
      gLog(`Handel: ${p.name} ↔ ${o.name} abgeschlossen.`,'info');
      renderAll();closeModal();toast('Handel abgeschlossen! 🤝');
    }},{l:'❌ Abbrechen',c:'mb-n',fn:closeModal}],true);
}

/* ═══════════ STATS ═══════════ */
function openStats(){
  const alive=G.pl.filter(p=>!p.bust);
  const maxW=Math.max(...alive.map(p=>totalWealth(p)))||1;
  let bars=alive.map(p=>{
    const w=totalWealth(p);const pct=Math.round(w/maxW*100);
    return `<div class="wealth-bar" style="background:${p.color};height:${pct}%" title="${p.name}: M ${w.toLocaleString('de-DE')}">${pct>20?p.name.slice(0,4):''}</div>`;
  }).join('');
  let rows=G.pl.map(p=>{
    if(p.bust)return `<div class="brow" style="opacity:.4"><div style="width:28px;height:28px;border-radius:50%;background:${p.color};display:flex;align-items:center;justify-content:center;font-size:14px">${TOKENS[p.token]}</div><div style="flex:1"><div style="font-size:11px;font-weight:700">${p.name} 💀</div></div></div>`;
    const w=totalWealth(p);
    const props=G.sp.filter(s=>s.own===p.id);
    const houses=props.reduce((s,x)=>s+(x.h<5?x.h:0),0);
    const hotels=props.filter(x=>x.h===5).length;
    return `<div class="brow" style="cursor:pointer" onclick="showPlayerCard(${p.id});closeModal()">
      <div style="width:32px;height:32px;border-radius:50%;background:${p.color};display:flex;align-items:center;justify-content:center;font-size:16px">${TOKENS[p.token]}</div>
      <div style="flex:1">
        <div style="font-weight:700;font-size:12px">${p.name}${p.isAI?' 🤖':''}</div>
        <div style="font-size:9px;color:#aaa">💰 M${p.money.toLocaleString('de-DE')} | 🏘 ${props.length} | 🏠${houses} 🏨${hotels}</div>
        <div style="font-size:9px;color:#888">📥 M${p.stats.totalRent.toLocaleString('de-DE')} | 📤 M${p.stats.totalPaid.toLocaleString('de-DE')}</div>
      </div>
      <div style="font-size:14px;font-weight:700;color:#FFD700">M ${w.toLocaleString('de-DE')}</div>
    </div>`;
  }).join('');
  showModal('📊 Vermögen & Statistiken',`
    <div class="wealth-chart">${bars}</div>
    <div style="text-align:left;margin-top:12px">${rows}</div>
    <div style="text-align:left;margin-top:8px;font-size:9px;color:#666">Runde ${G.turn} | Freiparkpool: M ${G.pool}</div>`,
    [{l:'Schließen',c:'mb-ok',fn:closeModal}],true);
}

/* ═══════════ SAVE / LOAD ═══════════ */
function autoSave(final=false){
  try{
    localStorage.setItem('monopoly_pro_save',JSON.stringify({G,RULES,ts:Date.now(),final}));
    const badge=document.getElementById('save-badge');
    badge.textContent='💾 '+new Date().toLocaleTimeString('de-DE',{hour:'2-digit',minute:'2-digit'});
    badge.style.color='#88cc88';
    setTimeout(()=>badge.style.color='#888',2000);
  }catch(e){}
}
function checkSavedGame(){
  try{
    const raw=localStorage.getItem('monopoly_pro_save');
    if(!raw)return;
    const save=JSON.parse(raw);
    if(save.final)return;
    const age=Date.now()-save.ts;
    if(age>1000*60*60*24)return;
    const mins=Math.round(age/60000);
    showModal('💾 Gespeichertes Spiel',`<div class="mbody">Ein gespeichertes Spiel von vor ${mins} Minute(n) gefunden.<br>Möchtest du weiterspielen?</div>`,
      [{l:'▶ Weiterspielen',c:'mb-y',fn:()=>{closeModal();loadGame(save);}},
       {l:'Neues Spiel',c:'mb-n',fn:closeModal}],false);
  }catch(e){}
}
function loadGame(save){
  try{
    Object.assign(RULES,save.RULES);
    G=save.G;
    if(!G.wealthHistory)G.wealthHistory=[];
    G.chanceDeck=G.chanceDeck.map((_,i)=>CHANCE[i%CHANCE.length]);
    G.chestDeck=G.chestDeck.map((_,i)=>CHEST[i%CHEST.length]);
    moving=false;
    document.getElementById('setup').style.display='none';
    document.getElementById('game').style.display='flex';
    document.getElementById('b-undo').style.display=RULES.undo?'block':'none';
    buildBoard();buildAnimTokens();
    G.pl.forEach(p=>{if(!p.bust)placeAnimTok(p.id,p.pos);});
    renderAll();
    gLog('Spiel geladen!','info');toast('Willkommen zurück! 👋');
    const cur=G.pl[G.cur];
    if(cur.isAI)setTimeout(aiTakeTurn,1200);
    else startTimer();
  }catch(e){toast('Fehler beim Laden!');}
}

/* ═══════════ MODAL / LOG / TOAST ═══════════ */
function showModal(title,content,actions,closable){
  mClose=closable;
  document.getElementById('mt').textContent=title;
  document.getElementById('mc').innerHTML=content;
  const acts=document.getElementById('ma');acts.innerHTML='';
  actions.forEach(a=>{const b=document.createElement('button');b.className='mb '+a.c;b.textContent=a.l;b.onclick=a.fn;acts.appendChild(b);});
  document.getElementById('moverlay').classList.add('on');
}
function closeModal(){document.getElementById('moverlay').classList.remove('on');}
function gLog(msg,type){
  const el=document.getElementById('log');
  const e=document.createElement('div');e.className='log-e le-'+type;e.textContent=msg;
  el.appendChild(e);el.scrollTop=el.scrollHeight;
  while(el.children.length>120)el.removeChild(el.firstChild);
}
function toast(msg){
  const el=document.getElementById('toast');el.textContent=msg;el.style.opacity='1';
  if(toastT)clearTimeout(toastT);
  toastT=setTimeout(()=>el.style.opacity='0',3200);
}

/* ═══════════ INIT ═══════════ */
buildSetup();
setTimeout(checkSavedGame,400);

/* ═══════════ 3D MOUSE PARALLAX ═══════════ */
(function(){
  const wrap   = ()=>document.getElementById('board-3d-wrap');
  const shadow = ()=>document.getElementById('board-shadow');
  const BASE_X = 18, BASE_Z = -1.5;
  let curX = BASE_X, curZ = BASE_Z;
  let targetX = BASE_X, targetZ = BASE_Z;

  document.addEventListener('mousemove', e => {
    if(window._zoomTracking) return;
    const w = wrap();
    if(!w) return;
    const rect = w.getBoundingClientRect();
    const cx = rect.left + rect.width/2;
    const cy = rect.top  + rect.height/2;
    const dx = (e.clientX - cx) / (rect.width/2);
    const dy = (e.clientY - cy) / (rect.height/2);
    targetX = BASE_X - dy * 9;
    targetZ = BASE_Z + dx * 3.5;
  });

  document.addEventListener('mouseleave', () => {
    if(window._zoomTracking) return;
    targetX = BASE_X; targetZ = BASE_Z;
  });

  function tick(){
    if(!window._zoomTracking){
      curX += (targetX - curX) * 0.07;
      curZ += (targetZ - curZ) * 0.07;
      const w = wrap();
      if(w) w.style.transform = `rotateX(${curX.toFixed(2)}deg) rotateZ(${curZ.toFixed(2)}deg)`;
      // Move shadow opposite to tilt for realism
      const s = shadow();
      if(s){
        const shiftX = (curZ - BASE_Z) * -6;
        const shiftY = (curX - BASE_X) * 3;
        s.style.transform = `translateX(${shiftX.toFixed(1)}px) translateY(${shiftY.toFixed(1)}px) scaleX(${(1 + Math.abs(curZ-BASE_Z)*0.03).toFixed(3)})`;
      }
    }
    requestAnimationFrame(tick);
  }
  tick();
})();
</script>
</body>
</html>
