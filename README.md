# <!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MONOPOLY PRO — Deutsche Klassik Edition</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Oswald:wght@400;600;700;900&family=Barlow+Condensed:ital,wght@0,400;0,600;0,700;1,700&display=swap" rel="stylesheet">
<style>
/* ══════════════════════════════════════════════════ ROOT ══ */
:root{
  --C:80px; --CW:60px; --CH:80px; --BS:700px;
  --gold:#C8A000; --gold-lt:#FFD700;
  --board-green:#C5E3B8; --border-col:#3a3a3a;
  --sidebar-bg:linear-gradient(170deg,#0e3a0e 0%,#071507 100%);
  /* THEME vars */
  --th-body1:#163800; --th-body2:#2a1000; --th-body3:#0a2200; --th-bodybg:#060e02;
  --th-setup1:#0e3a0e; --th-setup2:#071507;
  --th-board:#C5E3B8; --th-corner0:#d4edda; --th-corner0b:#b8dfc0;
}
/* ── DARK THEME (default) ── */
body.th-dark{ --th-body1:#163800; --th-body2:#2a1000; --th-bodybg:#060e02; }
/* ── GOLD THEME ── */
body.th-gold{
  --th-body1:#3a2a00; --th-body2:#1a0e00; --th-bodybg:#120a00;
  --th-setup1:#3a2a00; --th-setup2:#1a0e00;
  --sidebar-bg:linear-gradient(170deg,#3a2a00 0%,#1a0e00 100%);
}
/* ── NIGHT THEME ── */
body.th-night{
  --th-body1:#0a0a1a; --th-body2:#000010; --th-bodybg:#000008;
  --th-setup1:#0a0a1a; --th-setup2:#000010;
  --sidebar-bg:linear-gradient(170deg,#0a0a1a 0%,#000010 100%);
  --gold:#7799FF; --gold-lt:#aabbff;
}

*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;font-family:'Barlow Condensed',sans-serif}
body{
  background:
    radial-gradient(ellipse 55% 45% at 20% 15%,var(--th-body1) 0%,transparent 65%),
    radial-gradient(ellipse 60% 50% at 85% 85%,var(--th-body2) 0%,transparent 60%),
    var(--th-bodybg);
  min-height:100vh;
  display:flex;align-items:center;justify-content:center;
  padding:10px;overflow-x:hidden;
}

/* ══ SETUP ══ */
#setup{
  max-width:700px;width:100%;
  background:linear-gradient(170deg,var(--th-setup1),var(--th-setup2));
  border:3px solid var(--gold);border-radius:14px;
  padding:42px 38px;text-align:center;color:#fff;
  box-shadow:0 0 80px rgba(0,0,0,.85),inset 0 1px 0 rgba(200,160,0,.15);
}
#setup h1{font-family:'Oswald',sans-serif;font-size:68px;font-weight:900;letter-spacing:10px;color:#fff;line-height:1;text-shadow:5px 5px 0 #7a0000;margin-bottom:6px}
.s-ed{font-size:11px;letter-spacing:6px;color:var(--gold);text-transform:uppercase;border:1px solid rgba(200,160,0,.3);display:inline-block;padding:4px 16px;border-radius:3px;margin-bottom:28px}
.s-lbl{display:block;font-size:10px;text-transform:uppercase;letter-spacing:2.5px;color:var(--gold);font-weight:700;margin-bottom:9px}
.s-sec{margin-bottom:20px;text-align:left}
.cnt-row{display:flex;gap:8px;flex-wrap:wrap}
.cb{width:48px;height:48px;border:2px solid var(--gold);background:transparent;color:var(--gold);font-size:19px;font-weight:700;cursor:pointer;border-radius:7px;transition:all .18s;font-family:inherit}
.cb.on,.cb:hover{background:var(--gold);color:#000}
.p-rows{display:grid;gap:8px}
.p-row{display:flex;align-items:center;gap:9px;background:rgba(0,0,0,.42);padding:9px 12px;border-radius:7px;border:1px solid rgba(200,160,0,.12)}
.p-row.ai-row{border-color:rgba(0,180,255,.25);background:rgba(0,50,80,.3)}
.p-sw{width:24px;height:24px;border-radius:50%;flex-shrink:0;border:2px solid rgba(255,255,255,.4)}
.p-row input{flex:1;background:transparent;border:none;border-bottom:1px solid rgba(200,160,0,.35);color:#fff;font-size:14px;padding:2px 4px;outline:none;font-family:inherit}
.tok-row{display:flex;gap:4px;flex-wrap:wrap}
.to{width:33px;height:33px;border:2px solid rgba(200,160,0,.22);background:rgba(0,0,0,.3);cursor:pointer;border-radius:5px;font-size:15px;display:flex;align-items:center;justify-content:center;transition:all .15s}
.to:hover,.to.sel{border-color:#FFD700;background:rgba(200,160,0,.22)}
/* Rules panel in setup */
.rules-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.rule-item{background:rgba(0,0,0,.3);border:1px solid rgba(200,160,0,.15);border-radius:7px;padding:10px 12px}
.rule-item label{font-size:11px;color:#ddd;display:flex;align-items:center;gap:7px;cursor:pointer}
.rule-item input[type=checkbox]{accent-color:var(--gold);width:14px;height:14px}
.rule-item select{background:#071507;color:#FFD700;border:1px solid var(--gold);border-radius:4px;padding:3px 6px;font-family:inherit;font-size:11px;margin-top:4px;width:100%}
.theme-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:4px}
.thbtn{padding:6px 14px;border:2px solid rgba(200,160,0,.3);background:transparent;color:#ddd;border-radius:5px;cursor:pointer;font-family:inherit;font-size:11px;transition:all .15s}
.thbtn.on,.thbtn:hover{border-color:var(--gold);color:var(--gold);background:rgba(200,160,0,.1)}
/* AI badge */
.ai-toggle{display:flex;align-items:center;gap:6px;font-size:10px;color:#88ccff;cursor:pointer;padding:3px 8px;border:1px solid rgba(0,180,255,.3);border-radius:4px;background:rgba(0,100,200,.1);transition:all .15s}
.ai-toggle.on{background:rgba(0,150,255,.2);border-color:#00aaff;color:#00ccff}
.st-btn{width:100%;padding:18px;background:linear-gradient(180deg,#cc2600,#840000);border:3px solid var(--gold-lt);color:#fff;font-family:'Oswald',sans-serif;font-size:22px;font-weight:700;letter-spacing:5px;text-transform:uppercase;cursor:pointer;border-radius:7px;margin-top:14px;transition:all .2s}
.st-btn:hover{filter:brightness(1.15);transform:translateY(-2px);box-shadow:0 8px 28px rgba(160,0,0,.5)}

/* ══ GAME LAYOUT ══ */
#game{display:none;flex-direction:row;gap:16px;align-items:flex-start;max-width:1320px;width:100%}

/* ══ BOARD ══ */
#bwrap{position:relative;flex-shrink:0}
#board{width:var(--BS);height:var(--BS);position:relative;background:var(--board-green);border:7px solid #0a2200;box-shadow:0 0 0 2px #1a4a00,0 20px 80px rgba(0,0,0,.9),inset 0 0 0 4px rgba(255,255,255,.07);overflow:hidden}
#board::before{content:'';position:absolute;inset:7px;border:2px solid rgba(0,90,0,.3);pointer-events:none;z-index:0}
#board::after{content:'';position:absolute;inset:0;background:repeating-linear-gradient(0deg,transparent,transparent 59px,rgba(0,0,0,.03) 59px,rgba(0,0,0,.03) 60px),repeating-linear-gradient(90deg,transparent,transparent 59px,rgba(0,0,0,.03) 59px,rgba(0,0,0,.03) 60px);pointer-events:none;z-index:0}

/* corners */
.corner{position:absolute;width:var(--C);height:var(--C);border:1.5px solid var(--border-col);display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;z-index:2;cursor:pointer;overflow:hidden}
.corner:hover{filter:brightness(.9)}
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

/* cells */
.cell{position:absolute;border:1.5px solid var(--border-col);overflow:hidden;cursor:pointer;z-index:1;transition:filter .12s,box-shadow .12s}
.cell:hover{filter:brightness(.87);z-index:3;box-shadow:inset 0 0 0 2px rgba(200,160,0,.5)}
.ci{position:absolute;width:60px;height:80px;display:flex;flex-direction:column;left:0;top:0}
.ci-B{transform:rotate(180deg);transform-origin:center}
.ci-L{transform:rotate(90deg);transform-origin:40px 40px;left:0;top:0}
.ci-T{transform:none}
.ci-R{transform:rotate(-90deg);transform-origin:40px 40px;left:0;top:-20px}
.cband{height:22px;flex-shrink:0;border-bottom:1.5px solid rgba(0,0,0,.22)}
.cbody{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:2px;text-align:center;gap:1px;background:#f2faee}
.cell.ch-cell .cbody{background:#fffbe8}
.cell.cm-cell .cbody{background:#edf4ff}
.cell.tx-cell .cbody{background:#fff0f0}
.cell.rl-cell .cbody{background:#f5f5ec}
.cell.ut-cell .cbody{background:#edfaff}
.cname{font-size:5.6px;font-weight:700;text-transform:uppercase;letter-spacing:.12px;line-height:1.28;color:#111;word-break:break-word;hyphens:auto;max-width:54px}
.cprice{font-size:5.2px;font-weight:700;color:#333;margin-top:1px}
.cicon{font-size:13px;line-height:1;margin-bottom:1px}
.own-dot{position:absolute;top:2px;left:2px;width:9px;height:9px;border-radius:50%;border:1.5px solid rgba(0,0,0,.45);z-index:5;display:none}
.hrow{position:absolute;top:2px;right:2px;display:flex;gap:1px;z-index:5;flex-wrap:wrap;max-width:28px}
.hdot{width:7px;height:7px;background:#1aaa1a;border:1px solid #087a08;border-radius:1px}
.hhotel{width:10px;height:9px;background:#cc2200;border:1px solid #880000;border-radius:1px}
.mortg-ov{position:absolute;inset:0;background:rgba(20,20,20,.45);display:none;align-items:center;justify-content:center;z-index:6}
.mortg-ov span{font-size:5px;color:#FFD700;font-weight:700;letter-spacing:.5px;transform:rotate(-25deg);display:block}
.tok-wrap{position:absolute;bottom:1px;right:1px;display:flex;flex-wrap:wrap-reverse;gap:1px;z-index:20;max-width:46px;justify-content:flex-end;pointer-events:none}
.anim-tok{position:absolute;width:26px;height:26px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:14px;border:2.5px solid rgba(255,255,255,.95);box-shadow:0 3px 10px rgba(0,0,0,.6);z-index:30;pointer-events:none;transition:left .20s cubic-bezier(.4,0,.2,1),top .20s cubic-bezier(.4,0,.2,1)}
.anim-tok.tok-jump{animation:tokBounce .20s ease-out}
@keyframes tokBounce{0%{transform:scale(1) translateY(0)}35%{transform:scale(1.45) translateY(-10px)}70%{transform:scale(.9) translateY(2px)}100%{transform:scale(1) translateY(0)}}
.btok{width:15px;height:15px;border-radius:50%;border:2px solid rgba(255,255,255,.9);display:flex;align-items:center;justify-content:center;font-size:7.5px;box-shadow:0 1px 5px rgba(0,0,0,.6);flex-shrink:0}
.btok.jailed{border-color:#f00!important;box-shadow:0 0 6px rgba(255,0,0,.8)!important}
/* AI token pulse */
.anim-tok.ai-player{box-shadow:0 3px 10px rgba(0,0,0,.6),0 0 12px rgba(0,180,255,.5);animation-name:aiPulse;animation-duration:2s;animation-iteration-count:infinite;animation-timing-function:ease-in-out}
@keyframes aiPulse{0%,100%{box-shadow:0 3px 10px rgba(0,0,0,.6),0 0 8px rgba(0,180,255,.4)}50%{box-shadow:0 3px 10px rgba(0,0,0,.6),0 0 20px rgba(0,180,255,.8)}}

/* ══ BOARD CENTER ══ */
#bcenter{position:absolute;left:calc(var(--C) + 1.5px);top:calc(var(--C) + 1.5px);width:calc(var(--BS) - 2*var(--C) - 3px);height:calc(var(--BS) - 2*var(--C) - 3px);display:flex;flex-direction:column;align-items:center;justify-content:space-around;z-index:1;padding:20px 16px}
#bcenter::before{content:'';position:absolute;inset:0;background:radial-gradient(ellipse 75% 55% at 50% 50%,rgba(144,210,144,.35) 0%,transparent 75%);pointer-events:none}
.deck-row{display:flex;justify-content:space-around;width:100%;position:relative;z-index:2}
.deck{width:86px;height:118px;border-radius:11px;display:flex;flex-direction:column;align-items:center;justify-content:center;font-family:'Oswald',sans-serif;font-size:9.5px;font-weight:600;letter-spacing:1.2px;text-align:center;line-height:1.5;color:#fff;padding:10px 7px;border:2.5px solid rgba(255,255,255,.25)}
.deck-ev{background:linear-gradient(150deg,#e87700,#b04500);box-shadow:4px 5px 16px rgba(0,0,0,.4);transform:rotate(7deg)}
.deck-gem{background:linear-gradient(150deg,#1558b0,#0a3070);box-shadow:4px 5px 16px rgba(0,0,0,.4);transform:rotate(-6deg)}
.deck-ico{font-size:32px;display:block;margin-bottom:6px}
.center-title{position:relative;z-index:2;text-align:center}
.mono-logo{font-family:'Oswald',sans-serif;font-size:50px;font-weight:900;color:#CC0000;letter-spacing:6px;background:#fff;padding:5px 22px 4px;border:3.5px solid #CC0000;border-radius:5px;display:inline-block;line-height:1.15;box-shadow:4px 4px 0 rgba(0,0,0,.18)}
.mono-sub{font-size:10px;color:#445;letter-spacing:5px;text-transform:uppercase;margin-top:6px;font-weight:600}
#dice-sec{position:relative;z-index:2;display:flex;flex-direction:column;align-items:center;gap:10px}
#dice-row{display:flex;gap:18px;align-items:center}
.die{width:56px;height:56px;background:linear-gradient(145deg,#fffffc,#f0f0e8);border:3px solid #1a1a1a;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:36px;box-shadow:3px 4px 0 #888,0 6px 18px rgba(0,0,0,.25);user-select:none;cursor:pointer;transition:transform .07s,box-shadow .07s}
.die:active{transform:translateY(2px);box-shadow:1px 2px 0 #888}
.die.rolling{animation:spindie .75s cubic-bezier(.22,.61,.36,1)}
@keyframes spindie{0%{transform:rotate(0)scale(1)}20%{transform:rotate(60deg)scale(1.35)}42%{transform:rotate(-38deg)scale(.86)}62%{transform:rotate(24deg)scale(1.13)}80%{transform:rotate(-10deg)scale(1.05)}100%{transform:rotate(0)scale(1)}}
#dice-info{font-size:11px;color:#445;font-weight:700;letter-spacing:.5px;background:rgba(255,255,255,.6);padding:3px 10px;border-radius:3px}

/* ══ SIDEBAR ══ */
#sidebar{flex:1;min-width:250px;max-width:290px;display:flex;flex-direction:column;gap:10px}
.sc{background:var(--sidebar-bg);border:2px solid var(--gold);border-radius:10px;padding:13px;color:#fff;box-shadow:0 4px 22px rgba(0,0,0,.45),inset 0 1px 0 rgba(200,160,0,.1)}
.sc-title{font-size:9px;text-transform:uppercase;letter-spacing:3px;color:var(--gold);border-bottom:1px solid rgba(200,160,0,.28);padding-bottom:5px;margin-bottom:12px;font-weight:700}
.cp-row{display:flex;align-items:center;gap:12px;margin-bottom:12px}
.cp-tok{width:48px;height:48px;border-radius:50%;border:3px solid rgba(255,255,255,.85);display:flex;align-items:center;justify-content:center;font-size:24px;flex-shrink:0;box-shadow:0 3px 12px rgba(0,0,0,.55)}
.cp-name{font-size:17px;font-weight:700;line-height:1.2}
.cp-money{font-size:16px;color:#FFD700;font-weight:700;margin-top:2px}
.cp-pos{font-size:10px;color:#999;margin-top:2px}

/* ── TIMER ── */
#turn-timer{display:flex;align-items:center;gap:8px;margin-bottom:10px;background:rgba(0,0,0,.3);border-radius:6px;padding:7px 10px;border:1px solid rgba(200,160,0,.15)}
.timer-bar-wrap{flex:1;height:6px;background:rgba(255,255,255,.12);border-radius:3px;overflow:hidden}
#timer-bar{height:100%;background:linear-gradient(90deg,#00cc44,#FFD700);border-radius:3px;transition:width .9s linear,background .5s}
#timer-bar.warn{background:linear-gradient(90deg,#ff8800,#ff2200)}
#timer-txt{font-size:13px;font-weight:700;color:#FFD700;min-width:26px;text-align:right;font-family:'Oswald',sans-serif}

/* buttons */
.acts{display:flex;flex-direction:column;gap:6px}
.btn{padding:9px 14px;border:2px solid;border-radius:6px;font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:1px;cursor:pointer;transition:all .18s;font-family:inherit;width:100%}
.b-roll{background:linear-gradient(180deg,#cc2600,#840000);border-color:#FFD700;color:#fff;font-size:14px;letter-spacing:2px}
.b-roll:hover:not(:disabled){filter:brightness(1.18);transform:translateY(-1px);box-shadow:0 4px 16px rgba(160,0,0,.5)}
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

/* player list */
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
.pl-pos{font-size:9px;color:#888}

/* log */
#log{max-height:130px;overflow-y:auto;display:flex;flex-direction:column;gap:2px}
.log-e{font-size:9.5px;padding:3px 7px;border-radius:2px;line-height:1.5;border-left:2.5px solid transparent}
.le-action{color:#ddd;background:rgba(255,255,255,.04);border-color:#555}
.le-money{color:#FFD700;background:rgba(200,160,0,.07);border-color:var(--gold)}
.le-warn{color:#ff8866;background:rgba(200,50,20,.07);border-color:#ff5533}
.le-info{color:#88ccff;background:rgba(20,80,200,.07);border-color:#4488cc}
.le-card{color:#ffcc55;background:rgba(200,130,0,.09);border-color:#ff9900}
.le-ai{color:#88eeff;background:rgba(0,150,255,.07);border-color:#00aacc}
#log::-webkit-scrollbar{width:3px}
#log::-webkit-scrollbar-thumb{background:rgba(200,160,0,.35);border-radius:2px}

/* ══ FLOATING MONEY ══ */
.float-money{position:fixed;font-family:'Oswald',sans-serif;font-size:22px;font-weight:700;pointer-events:none;z-index:9999;text-shadow:0 2px 8px rgba(0,0,0,.8);animation:floatUp .9s ease-out forwards}
.float-money.gain{color:#00ee88}
.float-money.lose{color:#ff4444}
@keyframes floatUp{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(-70px) scale(.8)}}

/* ══ MOVE INDICATOR ══ */
#move-indicator{position:fixed;top:16px;left:50%;transform:translateX(-50%);background:rgba(0,0,0,.92);color:#FFD700;padding:8px 22px;border-radius:6px;font-size:13px;font-weight:700;z-index:500;border:1px solid var(--gold);display:none;letter-spacing:1px;box-shadow:0 4px 20px rgba(0,0,0,.6)}

/* ══ MODAL ══ */
#moverlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.82);z-index:1000;align-items:center;justify-content:center}
#moverlay.on{display:flex}
#mbox{background:linear-gradient(160deg,#0e3a0e,#071507);border:3px solid var(--gold);border-radius:12px;padding:26px;color:#fff;max-width:420px;width:94%;text-align:center;box-shadow:0 16px 70px rgba(0,0,0,.9),inset 0 1px 0 rgba(200,160,0,.12);max-height:90vh;overflow-y:auto}
.mt{font-family:'Oswald',sans-serif;font-size:20px;color:#FFD700;margin-bottom:10px;letter-spacing:3px;text-transform:uppercase}
.mband{height:22px;border-radius:4px;margin-bottom:10px;border-bottom:1.5px solid rgba(0,0,0,.2)}
.mbody{font-size:13px;line-height:1.75;margin-bottom:12px;color:#ccc}
.mprice{font-size:30px;font-weight:700;color:#FFD700;margin:8px 0;font-family:'Oswald',sans-serif}
.rtbl{font-size:10px;width:100%;border-collapse:collapse;margin:6px 0;text-align:left}
.rtbl td{padding:3.5px 8px;border:1px solid rgba(200,160,0,.15)}
.rtbl tr:first-child td{background:rgba(0,0,0,.5);font-weight:700;color:#FFD700}
.rtbl tr:nth-child(even) td{background:rgba(255,255,255,.04)}
.macts{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin-top:12px}
.mb{padding:11px 22px;border:2px solid;border-radius:6px;font-size:12px;font-weight:700;cursor:pointer;transition:all .18s;text-transform:uppercase;letter-spacing:1px;font-family:inherit}
.mb-y{background:linear-gradient(180deg,#008800,#004400);border-color:#00cc00;color:#fff}
.mb-y:hover{filter:brightness(1.18)}
.mb-n{background:transparent;border-color:var(--gold);color:var(--gold)}
.mb-n:hover{background:rgba(200,160,0,.12)}
.mb-ok{background:linear-gradient(180deg,#cc2600,#840000);border-color:#FFD700;color:#fff;min-width:110px}
.mb-ok:hover{filter:brightness(1.18)}
.ccard{padding:18px;border-radius:9px;margin:10px 0;font-size:14px;line-height:1.7;font-weight:700;color:#fff;border:2px solid rgba(255,255,255,.18)}
.ccard-ev{background:linear-gradient(135deg,#c05500,#8a3200)}
.ccard-gem{background:linear-gradient(135deg,#1050a0,#082060)}
.brow{display:flex;align-items:center;gap:8px;padding:9px;background:rgba(0,0,0,.35);border-radius:6px;margin-bottom:6px;border:1px solid rgba(200,160,0,.12)}

/* Property card popup */
.prop-card{border-radius:8px;overflow:hidden;margin:8px 0;border:2px solid rgba(255,255,255,.15)}
.prop-card-band{height:28px;display:flex;align-items:center;justify-content:center;font-family:'Oswald',sans-serif;font-size:11px;font-weight:700;letter-spacing:2px;color:#fff;text-shadow:0 1px 3px rgba(0,0,0,.5)}
.prop-card-body{background:#f9f5e8;color:#111;padding:10px}
.prop-card-name{font-family:'Oswald',sans-serif;font-size:15px;font-weight:700;text-align:center;margin-bottom:4px;letter-spacing:1px}
.prop-card-price{text-align:center;font-size:11px;color:#555;margin-bottom:8px}
.prop-card-tbl{width:100%;font-size:10px;border-collapse:collapse}
.prop-card-tbl td{padding:2px 6px;border-bottom:1px solid #ddd}
.prop-card-tbl td:last-child{text-align:right;font-weight:700}
.prop-card-tbl tr:first-child td{background:#f0ebe0;font-weight:700}

/* ══ STATS CHART ══ */
.wealth-chart{display:flex;align-items:flex-end;gap:4px;height:80px;margin-top:8px;padding:0 4px}
.wealth-bar{flex:1;border-radius:4px 4px 0 0;min-height:4px;display:flex;align-items:flex-start;justify-content:center;padding-top:3px;font-size:8px;font-weight:700;color:rgba(255,255,255,.9);transition:height .4s}

/* ══ TOAST ══ */
#toast{position:fixed;bottom:22px;left:50%;transform:translateX(-50%);background:rgba(4,16,4,.96);color:#fff;padding:12px 28px;border-radius:7px;font-size:13px;font-weight:600;z-index:2000;transition:opacity .5s;pointer-events:none;text-align:center;max-width:440px;border:1px solid rgba(200,160,0,.38);box-shadow:0 5px 24px rgba(0,0,0,.65);opacity:0}

/* ══ WIN SCREEN ══ */
#winscr{display:none;position:fixed;inset:0;background:rgba(0,0,0,.95);z-index:3000;flex-direction:column;align-items:center;justify-content:center;text-align:center}
#winscr.on{display:flex}
.wt{font-family:'Oswald',sans-serif;font-size:72px;font-weight:900;color:#FFD700;letter-spacing:7px;animation:wglow 1.4s ease-in-out infinite alternate}
@keyframes wglow{from{text-shadow:2px 2px 0 #aa6600}to{text-shadow:0 0 40px #FFD700,0 0 80px #FF8C00,2px 2px 0 #aa6600}}
.wn{font-family:'Oswald',sans-serif;font-size:32px;color:#fff;margin:14px 0 4px;letter-spacing:2px}
.wm{font-size:20px;color:#FFD700;margin-bottom:20px}
.win-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:10px 0 24px;max-width:500px;width:90%}
.win-stat{background:rgba(255,255,255,.07);border:1px solid rgba(200,160,0,.2);border-radius:8px;padding:10px;font-size:11px;color:#aaa}
.win-stat strong{display:block;font-size:18px;color:#FFD700;font-family:'Oswald',sans-serif;margin-bottom:2px}
.wb{padding:18px 50px;background:linear-gradient(180deg,#cc2600,#840000);border:3px solid #FFD700;color:#fff;font-family:'Oswald',sans-serif;font-size:18px;font-weight:700;letter-spacing:3px;cursor:pointer;border-radius:8px;transition:all .2s}
.wb:hover{filter:brightness(1.18);transform:translateY(-2px)}

/* ══ AI THINKING INDICATOR ══ */
#ai-thinking{display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:rgba(0,20,40,.95);border:2px solid #00aaff;border-radius:12px;padding:20px 36px;color:#88ccff;font-size:14px;font-weight:700;z-index:2500;text-align:center;box-shadow:0 0 40px rgba(0,150,255,.3)}
#ai-thinking .ai-dots span{display:inline-block;width:8px;height:8px;background:#00aaff;border-radius:50%;margin:0 3px;animation:aiDot 1s infinite}
#ai-thinking .ai-dots span:nth-child(2){animation-delay:.2s}
#ai-thinking .ai-dots span:nth-child(3){animation-delay:.4s}
@keyframes aiDot{0%,80%,100%{transform:scale(0.4);opacity:.3}40%{transform:scale(1);opacity:1}}

/* ══ SAVE/LOAD BADGE ══ */
#save-badge{position:fixed;top:14px;right:14px;background:rgba(0,0,0,.8);border:1px solid rgba(200,160,0,.3);color:#888;font-size:10px;padding:5px 10px;border-radius:5px;z-index:400;letter-spacing:.5px}
</style>
</head>
<body class="th-dark">

<!-- ═══════════ SETUP ═══════════ -->
<div id="setup">
  <h1>MONOPOLY</h1>
  <div class="s-ed">Pro Edition · Deutsche Klassik</div>

  <!-- Theme -->
  <div class="s-sec">
    <label class="s-lbl">🎨 Thema</label>
    <div class="theme-row">
      <button class="thbtn on" onclick="setTheme('dark',this)">🌙 Dunkel</button>
      <button class="thbtn" onclick="setTheme('gold',this)">✨ Gold</button>
      <button class="thbtn" onclick="setTheme('night',this)">🔵 Nacht</button>
    </div>
  </div>

  <!-- Players -->
  <div class="s-sec">
    <label class="s-lbl">Anzahl der Spieler</label>
    <div class="cnt-row" id="cnt-row"></div>
  </div>
  <div class="s-sec">
    <label class="s-lbl">Spieler konfigurieren</label>
    <div class="p-rows" id="p-rows"></div>
  </div>

  <!-- Rules -->
  <div class="s-sec">
    <label class="s-lbl">⚙️ Spielregeln</label>
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
        <label><input type="checkbox" id="r-auction" checked> 🏦 Muss-Auktion</label>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-pool" checked> 🚗 Freiparkgeld</label>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-undo" checked> 🔄 Undo erlaubt</label>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-quick"> ⚡ Schnellmodus</label>
        <select id="r-start-money">
          <option value="1500">M 1500 Start</option>
          <option value="2000">M 2000 Start</option>
          <option value="3000">M 3000 Start</option>
        </select>
      </div>
      <div class="rule-item">
        <label><input type="checkbox" id="r-sound" checked> 🔊 Sounds</label>
      </div>
    </div>
  </div>

  <button class="st-btn" onclick="startGame()">🎲 &nbsp;Spiel starten</button>
</div>

<!-- ═══════════ GAME ═══════════ -->
<div id="game">
  <div id="bwrap">
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
        </div>
      </div>
    </div>
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
          <div class="cp-pos" id="cp-pos">–</div>
        </div>
      </div>
      <!-- Timer -->
      <div id="turn-timer" style="display:none">
        <div class="timer-bar-wrap"><div id="timer-bar"></div></div>
        <div id="timer-txt">90</div>
      </div>
      <div class="acts">
        <button class="btn b-roll" id="b-roll" onclick="rollDice()">🎲 &nbsp;WÜRFELN</button>
        <button class="btn b-bail" id="b-bail" style="display:none" onclick="payBail()">⛓ Kaution M 50</button>
        <button class="btn b-house" id="b-house" onclick="openBuild()">🏠 Bauen / Hypothek</button>
        <button class="btn b-trade" id="b-trade" onclick="openTrade()">🤝 Handel</button>
        <button class="btn b-undo" id="b-undo" onclick="doUndo()" style="display:none">🔄 Undo (Würfeln rückgängig)</button>
        <button class="btn b-stats" onclick="openStats()">📊 Vermögen & Statistiken</button>
        <button class="btn b-end" id="b-end" onclick="endTurn()" disabled>➡ &nbsp;Zug beenden</button>
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
<div id="moverlay" onclick="if(event.target===this&&mClose)closeModal()">
  <div id="mbox"><div class="mt" id="mt">–</div><div id="mc"></div><div class="macts" id="ma"></div></div>
</div>
<div id="winscr">
  <div class="wt">🏆 GEWINNER!</div>
  <div class="wn" id="wn">–</div>
  <div class="wm" id="wm">–</div>
  <div class="win-stats" id="win-stats"></div>
  <button class="wb" onclick="location.reload()">🎮 Neues Spiel</button>
</div>
<div id="toast"></div>
<div id="ai-thinking">🤖 KI denkt nach…<div class="ai-dots"><span></span><span></span><span></span></div></div>
<div id="save-badge">💾 Gespeichert</div>

<script>
/* ══════════════════════════════════════════════════
   BOARD DATA
══════════════════════════════════════════════════ */
const PCOLS=['#e53935','#43a047','#1e88e5','#fb8c00','#8e24aa','#00acc1','#e91e63','#795548'];
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

/* ── Space pixel centers ── */
function getSpaceCenter(id){
  const S=700,C=80,CW=60;
  if(id===0)  return {x:S-C/2, y:S-C/2};
  if(id===10) return {x:C/2,   y:S-C/2};
  if(id===20) return {x:C/2,   y:C/2};
  if(id===30) return {x:S-C/2, y:C/2};
  const def=BOARD[id];
  if(def.side==='B'){const idx=id-1; return {x:C+(8-idx)*CW+CW/2, y:S-C/2};}
  if(def.side==='L'){const idx=id-11;return {x:C/2, y:S-C-idx*CW-CW/2};}
  if(def.side==='T'){const idx=id-21;return {x:C+idx*CW+CW/2, y:C/2};}
  const idx=id-31; return {x:S-C/2, y:C+idx*CW+CW/2};
}

/* ══════════════════════════════════════════════════
   CARD DECKS
══════════════════════════════════════════════════ */
const CHANCE=[
  {t:'Rücke vor bis LOS! Erhalte M 200.',fn(p,g,cb){animMoveTo(p,0,true,cb);}},
  {t:'Fahre nach Berliner Straße. Falls du LOS passierst, erhalte M 200.',fn(p,g,cb){animMoveTo(p,14,true,cb);}},
  {t:'Fahre nach Theaterstraße. Falls du LOS passierst, erhalte M 200.',fn(p,g,cb){animMoveTo(p,16,true,cb);}},
  {t:'Fahre zum nächsten Bahnhof.',fn(p,g,cb){const r=[5,15,25,35];const n=r.find(x=>x>p.pos)||r[0];animMoveTo(p,n,true,cb);}},
  {t:'Gehe ins Gefängnis!',fn(p,g,cb){goJail(p,cb);}},
  {t:'Schulpreis! Erhalte M 150.',fn(p,g,cb){addM(p,150);cb();}},
  {t:'Bankdividende: Erhalte M 50.',fn(p,g,cb){addM(p,50);cb();}},
  {t:'Rücke 3 Felder zurück.',fn(p,g,cb){animMoveTo(p,(p.pos-3+40)%40,false,cb,true);}},
  {t:'Straßenreparaturen: M 40/Haus, M 115/Hotel.',fn(p,g,cb){let h=0,ho=0;G.sp.filter(s=>s.own===p.id&&s.h>0).forEach(s=>s.h===5?ho++:h+=s.h);const a=h*40+ho*115;if(a)spendM(p,a);cb();}},
  {t:'Gehe zurück zu Badstraße.',fn(p,g,cb){animMoveTo(p,1,false,cb,true);}},
  {t:'Zahle jedem Spieler M 50.',fn(p,g,cb){G.pl.filter(x=>!x.bust&&x.id!==p.id).forEach(o=>{p.money-=50;addM(o,50);});gLog(`${p.name} zahlt M 50 an alle.`,'warn');renderAll();cb();}},
  {t:'Kreuzworträtsel! Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Fahre nach Schlossallee.',fn(p,g,cb){animMoveTo(p,39,true,cb);}},
  {t:'Darlehen fällig. Zahle M 100.',fn(p,g,cb){spendM(p,100);cb();}},
  {t:'Fahre nach Südbahnhof.',fn(p,g,cb){animMoveTo(p,5,true,cb);}},
  {t:'Gefängnisfreikarte!',fn(p,g,cb){p.jailCard=(p.jailCard||0)+1;gLog(`${p.name} erhält Freikarte!`,'card');cb();}},
];
const CHEST=[
  {t:'Bankirrtum! Erhalte M 200.',fn(p,g,cb){addM(p,200);cb();}},
  {t:'Arztrechnung: Zahle M 50.',fn(p,g,cb){spendM(p,50);cb();}},
  {t:'Erbschaft: Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Rücke vor bis LOS! Erhalte M 200.',fn(p,g,cb){animMoveTo(p,0,true,cb);}},
  {t:'Steuernachzahlung: Zahle M 20.',fn(p,g,cb){spendM(p,20);cb();}},
  {t:'Schönheitspreis: Erhalte M 10.',fn(p,g,cb){addM(p,10);cb();}},
  {t:'Auto verkauft: Erhalte M 45.',fn(p,g,cb){addM(p,45);cb();}},
  {t:'Lebensversicherung: Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Gehe ins Gefängnis!',fn(p,g,cb){goJail(p,cb);}},
  {t:'Straßenbauinspektor: M 40/Haus, M 115/Hotel.',fn(p,g,cb){let h=0,ho=0;G.sp.filter(s=>s.own===p.id&&s.h>0).forEach(s=>s.h===5?ho++:h+=s.h);const a=h*40+ho*115;if(a)spendM(p,a);cb();}},
  {t:'Glückstag! M 10 von jedem Mitspieler.',fn(p,g,cb){G.pl.filter(x=>!x.bust&&x.id!==p.id).forEach(o=>{o.money-=10;p.money+=10;});renderAll();cb();}},
  {t:'Gefängnisfreikarte!',fn(p,g,cb){p.jailCard=(p.jailCard||0)+1;gLog(`${p.name} erhält Freikarte!`,'card');cb();}},
  {t:'Urlaubsgeld: Erhalte M 100.',fn(p,g,cb){addM(p,100);cb();}},
  {t:'Steuererstattung: Erhalte M 20.',fn(p,g,cb){addM(p,20);cb();}},
  {t:'Schulgebühren: Zahle M 150.',fn(p,g,cb){spendM(p,150);cb();}},
  {t:'Spezialhonorar: Zahle M 100.',fn(p,g,cb){spendM(p,100);cb();}},
];

/* ══════════════════════════════════════════════════
   AUDIO ENGINE
══════════════════════════════════════════════════ */
const AC = window.AudioContext ? new AudioContext() : null;
function playSound(type){
  if(!AC || !RULES.sound) return;
  if(AC.state==='suspended') AC.resume();
  const t=AC.currentTime;
  const g=AC.createGain();g.connect(AC.destination);
  const o=AC.createOscillator();o.connect(g);
  if(type==='roll'){
    o.type='square';
    o.frequency.setValueAtTime(220,t);o.frequency.exponentialRampToValueAtTime(440,t+.08);
    o.frequency.exponentialRampToValueAtTime(180,t+.18);
    g.gain.setValueAtTime(.18,t);g.gain.exponentialRampToValueAtTime(.001,t+.22);
    o.start(t);o.stop(t+.22);
  } else if(type==='move'){
    o.type='sine';o.frequency.setValueAtTime(440,t);o.frequency.exponentialRampToValueAtTime(520,t+.05);
    g.gain.setValueAtTime(.08,t);g.gain.exponentialRampToValueAtTime(.001,t+.07);
    o.start(t);o.stop(t+.07);
  } else if(type==='money'){
    o.type='triangle';o.frequency.setValueAtTime(660,t);o.frequency.exponentialRampToValueAtTime(880,t+.1);
    g.gain.setValueAtTime(.15,t);g.gain.exponentialRampToValueAtTime(.001,t+.15);
    o.start(t);o.stop(t+.15);
  } else if(type==='buy'){
    [523,659,784].forEach((f,i)=>{
      const o2=AC.createOscillator();const g2=AC.createGain();o2.connect(g2);g2.connect(AC.destination);
      o2.type='triangle';o2.frequency.value=f;
      g2.gain.setValueAtTime(.12,t+i*.1);g2.gain.exponentialRampToValueAtTime(.001,t+i*.1+.15);
      o2.start(t+i*.1);o2.stop(t+i*.1+.15);
    });
  } else if(type==='jail'){
    o.type='sawtooth';o.frequency.setValueAtTime(200,t);o.frequency.exponentialRampToValueAtTime(80,t+.4);
    g.gain.setValueAtTime(.2,t);g.gain.exponentialRampToValueAtTime(.001,t+.45);
    o.start(t);o.stop(t+.45);
  } else if(type==='win'){
    [523,659,784,1047].forEach((f,i)=>{
      const o2=AC.createOscillator();const g2=AC.createGain();o2.connect(g2);g2.connect(AC.destination);
      o2.type='triangle';o2.frequency.value=f;
      g2.gain.setValueAtTime(.15,t+i*.12);g2.gain.exponentialRampToValueAtTime(.001,t+i*.12+.3);
      o2.start(t+i*.12);o2.stop(t+i*.12+.3);
    });
  } else if(type==='double'){
    [440,550,660].forEach((f,i)=>{
      const o2=AC.createOscillator();const g2=AC.createGain();o2.connect(g2);g2.connect(AC.destination);
      o2.type='sine';o2.frequency.value=f;
      g2.gain.setValueAtTime(.1,t+i*.08);g2.gain.exponentialRampToValueAtTime(.001,t+i*.08+.12);
      o2.start(t+i*.08);o2.stop(t+i*.08+.12);
    });
  }
}

/* ══════════════════════════════════════════════════
   GAME STATE
══════════════════════════════════════════════════ */
let G={};
let RULES={timer:90,auction:true,pool:true,undo:true,sound:true,startMoney:1500};
let mClose=false;
let toastT=null;
let moving=false;
let timerInt=null;
let timerLeft=0;
let undoState=null; // snapshot before roll

function setMoving(v){
  moving=v;
  _refreshButtons();
}

function initG(players){
  G={
    pl:players.map((p,i)=>({
      id:i,name:p.name,token:p.token,color:p.color,isAI:p.isAI||false,
      money:RULES.startMoney,pos:0,jailed:false,jailRounds:0,
      jailCard:0,bust:false,
      stats:{turns:0,totalRent:0,totalPaid:0,spaceLanded:Array(40).fill(0)}
    })),
    sp:BOARD.map(b=>({...b,own:null,h:0,mortg:false})),
    cur:0, rolled:false, doubleCount:0, pool:0,
    chanceDeck:[...CHANCE].sort(()=>Math.random()-.5),
    chestDeck:[...CHEST].sort(()=>Math.random()-.5),
    chanceI:0, chestI:0,
    lastDice:[1,1],
    turn:0,
    wealthHistory:[], // [{turn, wealth:[...per player]}]
  };
}

/* ══════════════════════════════════════════════════
   THEME
══════════════════════════════════════════════════ */
function setTheme(t,btn){
  document.body.className='th-'+t;
  document.querySelectorAll('.thbtn').forEach(b=>b.classList.remove('on'));
  btn.classList.add('on');
}

/* ══════════════════════════════════════════════════
   SETUP UI
══════════════════════════════════════════════════ */
let sc=4, sp2=[];

function buildSetup(){
  const cr=document.getElementById('cnt-row');cr.innerHTML='';
  [2,3,4,5,6,7,8].forEach(n=>{
    const b=document.createElement('button');
    b.className='cb'+(n===sc?' on':'');b.textContent=n;
    b.onclick=()=>{sc=n;buildSetup()};cr.appendChild(b);
  });
  while(sp2.length<sc) sp2.push({name:`Spieler ${sp2.length+1}`,token:sp2.length%TOKENS.length,color:PCOLS[sp2.length%PCOLS.length],isAI:false});
  sp2=sp2.slice(0,sc);
  const pr=document.getElementById('p-rows');pr.innerHTML='';
  for(let i=0;i<sc;i++){
    const row=document.createElement('div');
    row.className='p-row'+(sp2[i].isAI?' ai-row':'');
    row.id='prow'+i;
    row.innerHTML=`
      <div class="p-sw" style="background:${PCOLS[i]}"></div>
      <input type="text" value="${sp2[i].name}" maxlength="16" placeholder="Name…" oninput="sp2[${i}].name=this.value" ${sp2[i].isAI?'disabled':''}>
      <div class="tok-row" id="tr${i}"></div>
      <div class="ai-toggle ${sp2[i].isAI?'on':''}" onclick="toggleAI(${i})" title="KI-Spieler">🤖 KI</div>`;
    pr.appendChild(row);
    const tr=row.querySelector(`#tr${i}`);
    TOKENS.forEach((tk,ti)=>{
      const b=document.createElement('button');
      b.className='to'+(sp2[i].token===ti?' sel':'');
      b.textContent=tk;b.title=TNAMES[ti];
      b.onclick=()=>{sp2[i].token=ti;tr.querySelectorAll('.to').forEach((x,j)=>x.classList.toggle('sel',j===ti));};
      tr.appendChild(b);
    });
  }
}

function toggleAI(i){
  sp2[i].isAI=!sp2[i].isAI;
  if(sp2[i].isAI) sp2[i].name='KI-'+TOKENS[sp2[i].token];
  buildSetup();
}

function startGame(){
  RULES.timer = document.getElementById('r-timer').checked ? parseInt(document.getElementById('r-timer-sec').value)||90 : 0;
  RULES.auction = document.getElementById('r-auction').checked;
  RULES.pool = document.getElementById('r-pool').checked;
  RULES.undo = document.getElementById('r-undo').checked;
  RULES.sound = document.getElementById('r-sound').checked;
  RULES.startMoney = parseInt(document.getElementById('r-start-money').value)||1500;

  const players=sp2.map((p,i)=>({
    name:p.name||`Spieler ${i+1}`,
    token:p.token%TOKENS.length,
    color:PCOLS[i%PCOLS.length],
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
  toast(G.pl[0].name+' ist dran! '+TOKENS[G.pl[0].token]);
  autoSave();
  if(G.pl[0].isAI) setTimeout(aiTakeTurn,1200);
  else startTimer();
}

/* ══════════════════════════════════════════════════
   TIMER
══════════════════════════════════════════════════ */
function startTimer(){
  if(!RULES.timer) return;
  stopTimer();
  timerLeft=RULES.timer;
  document.getElementById('turn-timer').style.display='flex';
  updateTimerUI();
  timerInt=setInterval(()=>{
    timerLeft--;
    updateTimerUI();
    if(timerLeft<=0){
      stopTimer();
      gLog('⏱ Zeit abgelaufen! Zug wird beendet.','warn');
      toast('⏱ Zeit abgelaufen!');
      if(!G.rolled){G.rolled=true;}
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
  const txt=document.getElementById('timer-txt');
  bar.style.width=pct+'%';
  txt.textContent=timerLeft;
  bar.className=pct<25?'warn':'';
}

/* ══════════════════════════════════════════════════
   UNDO SYSTEM
══════════════════════════════════════════════════ */
function saveUndoState(){
  if(!RULES.undo) return;
  undoState=JSON.parse(JSON.stringify({
    pl:G.pl,sp:G.sp,cur:G.cur,rolled:G.rolled,
    doubleCount:G.doubleCount,pool:G.pool,
    chanceI:G.chanceI,chestI:G.chestI,lastDice:G.lastDice,turn:G.turn
  }));
}
function doUndo(){
  if(!undoState||moving){return;}
  G.pl=undoState.pl; G.sp=undoState.sp; G.cur=undoState.cur;
  G.rolled=undoState.rolled; G.doubleCount=undoState.doubleCount;
  G.pool=undoState.pool; G.chanceI=undoState.chanceI;
  G.chestI=undoState.chestI; G.lastDice=undoState.lastDice;
  G.turn=undoState.turn;
  undoState=null;
  document.getElementById('die1').textContent='🎲';
  document.getElementById('die2').textContent='🎲';
  document.getElementById('dice-info').textContent='Bereit zum Würfeln…';
  // Rebuild anim tokens positions
  G.pl.forEach(p=>{if(!p.bust)placeAnimTok(p.id,p.pos);});
  renderAll();
  gLog('🔄 Zug rückgängig gemacht.','info');
  toast('🔄 Rückgängig!');
  document.getElementById('b-undo').disabled=true;
  startTimer();
}

/* ══════════════════════════════════════════════════
   BUILD BOARD
══════════════════════════════════════════════════ */
function buildBoard(){
  const board=document.getElementById('board');
  const S=700,C=80,CW=60,CH=80;
  BOARD.forEach(def=>{
    if([0,10,20,30].includes(def.id)) return;
    let x,y,w,h;
    if(def.side==='B'){const idx=def.id-1;x=C+(8-idx)*CW;y=S-C;w=CW;h=CH;}
    else if(def.side==='L'){const idx=def.id-11;x=0;y=S-C-(idx+1)*CW;w=CH;h=CW;}
    else if(def.side==='T'){const idx=def.id-21;x=C+idx*CW;y=0;w=CW;h=CH;}
    else{const idx=def.id-31;x=S-C;y=C+idx*CW;w=CH;h=CW;}
    const el=document.createElement('div');
    el.className='cell'+(def.type==='chance'?' ch-cell':'')+(def.type==='chest'?' cm-cell':'')+(def.type==='tax'?' tx-cell':'')+(def.type==='rail'?' rl-cell':'')+(def.type==='utility'?' ut-cell':'');
    el.style.cssText=`left:${x}px;top:${y}px;width:${w}px;height:${h}px`;
    el.dataset.pos=def.id;el.onclick=()=>showSpaceInfo(def.id);el.title=def.name;
    const ci=document.createElement('div');ci.className='ci ci-'+def.side;ci.id='ci'+def.id;
    if(def.type==='prop'){const band=document.createElement('div');band.className='cband';band.style.background=def.color;ci.appendChild(band);}
    const body=document.createElement('div');body.className='cbody';
    if(def.type==='prop') body.innerHTML=`<div class="cname">${def.name}</div><div class="cprice">M ${def.price}</div>`;
    else if(def.type==='rail') body.innerHTML=`<div class="cicon">🚂</div><div class="cname">${def.name}</div><div class="cprice">M 200</div>`;
    else if(def.type==='utility') body.innerHTML=`<div class="cicon">${def.name.includes('Elek')?'⚡':'💧'}</div><div class="cname">${def.name}</div><div class="cprice">M 150</div>`;
    else if(def.type==='chance') body.innerHTML=`<div class="cicon">❓</div><div class="cname" style="color:#b04400">EREIGNIS</div>`;
    else if(def.type==='chest') body.innerHTML=`<div class="cicon">📋</div><div class="cname" style="color:#0d3d8a">GEMEIN-SCHAFT</div>`;
    else if(def.type==='tax') body.innerHTML=`<div class="cicon">💸</div><div class="cname">${def.name}</div><div class="cprice">M ${def.amount}</div>`;
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

/* ══════════════════════════════════════════════════
   ANIMATED TOKENS
══════════════════════════════════════════════════ */
function buildAnimTokens(){
  document.querySelectorAll('.anim-tok').forEach(e=>e.remove());
  G.pl.forEach(p=>{
    const el=document.createElement('div');
    el.className='anim-tok'+(p.isAI?' ai-player':'');
    el.id='atk'+p.id;el.style.background=p.color;
    el.textContent=TOKENS[p.token];
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
  let i=0;
  function step(){
    if(i>=steps.length){MI.style.display='none';onDone();return;}
    const sp=steps[i];MI.textContent='→ '+BOARD[sp].name;
    playSound('move');
    const c=getSpaceCenter(sp);const off=pid*5;
    el.style.transition='left .20s cubic-bezier(.4,0,.2,1),top .20s cubic-bezier(.4,0,.2,1)';
    el.style.left=(c.x-13+off%10)+'px';el.style.top=(c.y-13+Math.floor(off/10)*5)+'px';
    el.classList.remove('tok-jump');void el.offsetWidth;el.classList.add('tok-jump');
    i++;setTimeout(step,230);
  }
  step();
}

/* ══════════════════════════════════════════════════
   RENDER
══════════════════════════════════════════════════ */
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
  G.pl.forEach((p,i)=>{
    const el=document.createElement('div');
    el.className='pl-item'+(i===G.cur?' active':'')+(p.bust?' bust':'');
    el.onclick=()=>showPlayerCard(p.id);
    el.innerHTML=`
      <div class="pl-tsm" style="background:${p.color}">${TOKENS[p.token]}</div>
      <div class="pl-info">
        <div class="pl-name">${p.name}${p.bust?' 💀':''}${p.isAI?'<span class="ai-tag">KI</span>':''}</div>
        <div class="pl-money">M ${p.money.toLocaleString('de-DE')}</div>
        <div class="pl-pos">${BOARD[p.pos].name}${p.jailed?' 🔒':''}</div>
      </div>`;
    list.appendChild(el);
  });
}
function renderCurPlayer(){
  const p=G.pl[G.cur];
  const tok=document.getElementById('cp-tok');tok.textContent=TOKENS[p.token];tok.style.background=p.color;
  document.getElementById('cp-name').textContent=p.name+(p.isAI?' 🤖':'');
  document.getElementById('cp-money').textContent='M '+p.money.toLocaleString('de-DE');
  document.getElementById('cp-pos').textContent=p.jailed?`🔒 Gefängnis (${p.jailRounds}/3)`:BOARD[p.pos].name;
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
  if(undoBtn) undoBtn.disabled=!undoState||mv||G.rolled;
  const bail=document.getElementById('b-bail');
  if(p.jailed){
    bail.style.display='block';bail.disabled=(p.money<50&&!p.jailCard)||mv;
    bail.textContent=p.jailCard?'🎟 Freikarte benutzen':'⛓ Kaution M 50';
    if(!G.rolled) rollBtn.disabled=mv;
    rollBtn.textContent='🎲  WÜRFELN (Gefängnis)';
  } else {
    bail.style.display='none';rollBtn.textContent='🎲  WÜRFELN';
  }
}
function renderOwners(){
  G.sp.forEach(s=>{
    const od=document.getElementById('od'+s.id),hr=document.getElementById('hr'+s.id),mo=document.getElementById('mo'+s.id);
    if(!od)return;
    if(s.own!==null){od.style.display='block';od.style.background=G.pl[s.own].color;if(mo)mo.style.display=s.mortg?'flex':'none';}
    else{od.style.display='none';if(mo)mo.style.display='none';}
    if(hr){
      hr.innerHTML='';
      if(s.h>0&&!s.mortg){
        if(s.h===5){const d=document.createElement('div');d.className='hhotel';hr.appendChild(d);}
        else for(let i=0;i<s.h;i++){const d=document.createElement('div');d.className='hdot';hr.appendChild(d);}
      }
    }
  });
}

/* ══════════════════════════════════════════════════
   FLOATING MONEY
══════════════════════════════════════════════════ */
function floatMoney(amount, positive){
  const el=document.createElement('div');
  el.className='float-money '+(positive?'gain':'lose');
  el.textContent=(positive?'+':'-')+' M '+Math.abs(amount).toLocaleString('de-DE');
  el.style.left=Math.random()*60+20+'vw';
  el.style.top=Math.random()*30+30+'vh';
  document.body.appendChild(el);
  setTimeout(()=>el.remove(),1000);
}

/* ══════════════════════════════════════════════════
   MONEY
══════════════════════════════════════════════════ */
function addM(p,a){
  p.money+=a;
  gLog(`${p.name} erhält M ${a}.`,'money');
  floatMoney(a,true);
  playSound('money');
}
function spendM(p,a){
  p.money-=a;
  gLog(`${p.name} zahlt M ${a}.`,'warn');
  floatMoney(a,false);
  if(p.money<0) doBankrupt(p);
}
function transfer(from,to,a){
  from.money-=a;to.money+=a;
  gLog(`${from.name} → M ${a} → ${to.name}.`,'money');
  floatMoney(a,true);
  if(from.money<0) doBankrupt(from);
}
function doBankrupt(p){
  G.sp.filter(s=>s.own===p.id&&s.h>0).forEach(s=>{p.money+=Math.floor(BOARD[s.id].hp/2)*s.h;s.h=0;});
  if(p.money>=0){gLog(`${p.name} rettet sich M ${p.money}.`,'info');return;}
  G.sp.filter(s=>s.own===p.id&&!s.mortg).forEach(s=>{p.money+=Math.floor(BOARD[s.id].price/2);s.mortg=true;});
  if(p.money>=0){gLog(`${p.name} rettet sich per Hypothek.`,'info');return;}
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
    document.getElementById('wn').textContent=w.name+' '+TOKENS[w.token];
    document.getElementById('wm').textContent='Vermögen: M '+totalWealth(w).toLocaleString('de-DE');
    // Win stats
    const ws=document.getElementById('win-stats');ws.innerHTML='';
    const data=[
      {label:'Runden gespielt',val:G.turn},
      {label:'Gesamtmiete kassiert',val:'M '+w.stats.totalRent.toLocaleString('de-DE')},
      {label:'Grundstücke',val:G.sp.filter(s=>s.own===w.id).length},
    ];
    data.forEach(d=>{ws.innerHTML+=`<div class="win-stat"><strong>${d.val}</strong>${d.label}</div>`;});
    document.getElementById('winscr').classList.add('on');
    autoSave(true);
  }
}
function totalWealth(p){
  return p.money+G.sp.filter(s=>s.own===p.id).reduce((sum,s)=>{
    const d=BOARD[s.id];
    return sum+(s.mortg?Math.floor(d.price/2):d.price)+(s.h<5?s.h*(d.hp||0):5*(d.hp||0));
  },0);
}

/* ══════════════════════════════════════════════════
   DICE & MOVEMENT
══════════════════════════════════════════════════ */
function rollDice(){
  if(moving)return;
  const p=G.pl[G.cur];
  if(p.jailed){jailRoll();return;}
  if(G.rolled)return;
  saveUndoState();
  document.getElementById('b-roll').disabled=true;
  document.getElementById('b-undo').disabled=true;
  playSound('roll');
  spinDice((d1,d2)=>{
    G.lastDice=[d1,d2];
    const tot=d1+d2,dbl=d1===d2;
    if(dbl){
      G.doubleCount++;
      playSound('double');
      if(G.doubleCount>=3){
        gLog(`${p.name} — 3× Pasch → Gefängnis!`,'warn');
        G.rolled=true;goJail(p,()=>{enableEnd();});G.doubleCount=0;return;
      }
      gLog(`${p.name} — Pasch (${d1}+${d2})! Nochmal.`,'info');
    } else {G.doubleCount=0;}
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
    } else {
      p.jailRounds++;
      if(p.jailRounds>=3){
        spendM(p,50);p.jailed=false;p.jailRounds=0;
        gLog(`${p.name} verlässt Gefängnis (M 50).`,'warn');
        G.rolled=true;doMove(p,tot,false);
      } else {
        gLog(`${p.name} kein Pasch. Im Gefängnis (${p.jailRounds}/3).`,'info');
        G.rolled=true;renderAll();enableEnd();
      }
    }
  });
}
function spinDice(cb){
  const d1e=document.getElementById('die1'),d2e=document.getElementById('die2');
  d1e.classList.add('rolling');d2e.classList.add('rolling');
  setTimeout(()=>{
    const d1=Math.floor(Math.random()*6)+1,d2=Math.floor(Math.random()*6)+1;
    d1e.textContent=DFACES[d1-1];d2e.textContent=DFACES[d2-1];
    d1e.classList.remove('rolling');d2e.classList.remove('rolling');
    document.getElementById('dice-info').textContent=`${d1} + ${d2} = ${d1+d2}${d1===d2?' — PASCH!':''}`;
    cb(d1,d2);
  },800);
}
function doMove(p,steps,dbl){
  const oldPos=p.pos,newPos=(oldPos+steps)%40,passedGO=(oldPos+steps)>=40;
  setMoving(true);
  animateMove(p.id,oldPos,newPos,()=>{
    p.pos=newPos;
    p.stats.spaceLanded[newPos]=(p.stats.spaceLanded[newPos]||0)+1;
    if(passedGO){addM(p,200);gLog(`${p.name} passiert LOS → M 200!`,'money');}
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
      if(!s.mortg&&s.own===null&&p.money>=def.price)showBuy(p,s,def,dbl);
      else if(!s.mortg&&s.own!==null&&s.own!==p.id)payRent(p,s,def,dbl);
      else{if(!dbl)enableEnd();else enableReroll();}break;
    case 'tax':
      gLog(`${p.name} zahlt Steuer M ${def.amount}.`,'warn');
      if(RULES.pool) G.pool+=def.amount;
      spendM(p,def.amount);renderAll();
      if(!dbl)enableEnd();else enableReroll();break;
    case 'chance':drawCard('chance',p,dbl);break;
    case 'chest':drawCard('chest',p,dbl);break;
    case 'jail':gLog(`${p.name} — nur zu Besuch.`,'info');if(!dbl)enableEnd();else enableReroll();break;
    case 'gotojail':G.rolled=true;goJail(p,()=>{enableEnd();});break;
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
  // Record wealth snapshot
  G.wealthHistory.push({turn:G.turn,wealth:G.pl.map(p=>totalWealth(p))});
  G.turn++;
  G.pl[G.cur].stats.turns++;
  let n=(G.cur+1)%G.pl.length;
  while(G.pl[n].bust&&n!==G.cur) n=(n+1)%G.pl.length;
  G.cur=n;G.rolled=false;G.doubleCount=0;
  document.getElementById('die1').textContent='🎲';
  document.getElementById('die2').textContent='🎲';
  document.getElementById('dice-info').textContent='Bereit zum Würfeln…';
  renderAll();
  const cur=G.pl[G.cur];
  toast(cur.name+(cur.isAI?' 🤖':'')+' ist dran! '+TOKENS[cur.token]);
  gLog('— '+cur.name+' ist dran —','info');
  autoSave();
  if(cur.isAI){setTimeout(aiTakeTurn,1200);}
  else{startTimer();}
}

/* ══════════════════════════════════════════════════
   JAIL
══════════════════════════════════════════════════ */
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

/* ══════════════════════════════════════════════════
   CARD MOVE HELPER
══════════════════════════════════════════════════ */
function animMoveTo(p,target,collectGO,onDone,backwards=false){
  if(collectGO&&!backwards&&target<=p.pos){addM(p,200);gLog(`${p.name} passiert LOS → M 200!`,'money');}
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
    } else {p.pos=target;setMoving(false);renderAll();if(onDone)onDone();}
    return;
  }
  animateMove(p.id,old,target,()=>{
    p.pos=target;setMoving(false);renderAll();
    gLog(`→ ${BOARD[target].name}`,'action');
    if(onDone)setTimeout(onDone,200);
  });
}

/* ══════════════════════════════════════════════════
   CARDS
══════════════════════════════════════════════════ */
function drawCard(type,p,dbl){
  const deck=type==='chance'?G.chanceDeck:G.chestDeck;
  const idx=type==='chance'?G.chanceI++:G.chestI++;
  const card=deck[idx%deck.length];
  gLog(`${p.name} zieht ${type==='chance'?'Ereignis':'Gemeinschaft'}karte.`,'card');
  showModal(type==='chance'?'❓ Ereigniskarte':'📋 Gemeinschaftskarte',
    `<div class="ccard ${type==='chance'?'ccard-ev':'ccard-gem'}">${card.t}</div>`,
    [{l:'OK',c:'mb-ok',fn:()=>{
      closeModal();let done=false;
      function afterCard(){if(done)return;done=true;renderAll();if(p.bust)return;if(p.jailed)enableEnd();else if(!dbl)enableEnd();else enableReroll();}
      card.fn(p,G,afterCard);
    }}],false);
}

/* ══════════════════════════════════════════════════
   RENT
══════════════════════════════════════════════════ */
function payRent(p,s,def,dbl){
  const own=G.pl[s.own];let rent=0;
  if(def.type==='rail'){const n=G.sp.filter(x=>x.type==='rail'&&x.own===s.own).length;rent=[25,50,100,200][n-1]||25;}
  else if(def.type==='utility'){const n=G.sp.filter(x=>x.type==='utility'&&x.own===s.own).length;const dv=G.lastDice[0]+G.lastDice[1];rent=n===1?dv*4:dv*10;}
  else{const grp=G.sp.filter(x=>x.type==='prop'&&BOARD[x.id].color===def.color);const full=grp.every(x=>x.own===s.own);rent=s.h===0?(full?def.rent[0]*2:def.rent[0]):def.rent[Math.min(s.h,5)];}
  p.stats.totalPaid+=rent;own.stats.totalRent+=rent;
  transfer(p,own,rent);renderAll();
  showModal(`💸 Miete: ${def.name}`,`
    <div class="mbody">Gehört <b>${own.name}</b> ${TOKENS[own.token]}<br>
    Du zahlst <span style="color:#FFD700;font-size:28px;font-weight:700">M ${rent}</span></div>`,
    [{l:'OK',c:'mb-ok',fn:()=>{closeModal();if(!dbl)enableEnd();else enableReroll();}}],false);
}

/* ══════════════════════════════════════════════════
   BUY
══════════════════════════════════════════════════ */
function showBuy(p,s,def,dbl){
  playSound('buy');
  const col=def.color||'#888';
  let propCardHtml='';
  if(def.type==='prop'){
    propCardHtml=`<div class="prop-card">
      <div class="prop-card-band" style="background:${col}">TITEL-URKUNDE</div>
      <div class="prop-card-body">
        <div class="prop-card-name">${def.name}</div>
        <div class="prop-card-price">Kaufpreis: M ${def.price} &nbsp;|&nbsp; Haus: M ${def.hp}</div>
        <table class="prop-card-tbl">
          <tr><td>Miete</td><td>M ${def.rent[0]}</td></tr>
          <tr><td>Farbgruppe komplett</td><td>M ${def.rent[0]*2}</td></tr>
          <tr><td>1 Haus</td><td>M ${def.rent[1]}</td></tr>
          <tr><td>2 Häuser</td><td>M ${def.rent[2]}</td></tr>
          <tr><td>3 Häuser</td><td>M ${def.rent[3]}</td></tr>
          <tr><td>4 Häuser</td><td>M ${def.rent[4]}</td></tr>
          <tr><td>🏨 Hotel</td><td>M ${def.rent[5]}</td></tr>
        </table>
      </div>
    </div>`;
  } else if(def.type==='rail'){
    propCardHtml=`<table class="rtbl"><tr><td>1 Bahn</td><td>M 25</td></tr><tr><td>2 Bahnen</td><td>M 50</td></tr><tr><td>3 Bahnen</td><td>M 100</td></tr><tr><td>4 Bahnen</td><td>M 200</td></tr></table>`;
  } else if(def.type==='utility'){
    propCardHtml=`<table class="rtbl"><tr><td>1 Versorgungswerk</td><td>4× Würfel</td></tr><tr><td>2 Versorgungswerke</td><td>10× Würfel</td></tr></table>`;
  }
  showModal(`🏠 ${def.name}`,`
    <div class="mprice">M ${def.price}</div>
    <div class="mbody">Guthaben: M ${p.money.toLocaleString('de-DE')}</div>
    ${propCardHtml}`,
    [
      {l:'✅ Kaufen',c:'mb-y',fn:()=>{spendM(p,def.price);s.own=p.id;gLog(`${p.name} kauft ${def.name}.`,'money');renderAll();closeModal();playSound('buy');if(!dbl)enableEnd();else enableReroll();}},
      {l:RULES.auction?'🔨 Auktion':'❌ Ablehnen',c:'mb-n',fn:()=>{closeModal();if(RULES.auction)runAuction(s,def,dbl);else{if(!dbl)enableEnd();else enableReroll();}}},
    ],false);
}

function runAuction(s,def,dbl){
  const bidders=[...G.pl.filter(p=>!p.bust)];
  let high=0,winner=-1,i=0;
  function next(){
    if(i>=bidders.length){
      if(winner>=0){spendM(G.pl[winner],high);s.own=winner;gLog(`${G.pl[winner].name} gewinnt Auktion ${def.name} M ${high}.`,'money');renderAll();}
      else gLog(`${def.name} nicht verkauft.`,'info');
      if(!dbl)enableEnd();else enableReroll();return;
    }
    const bp=bidders[i];
    // AI auto-bids
    if(bp.isAI){
      const maxBid=Math.min(bp.money,Math.floor(def.price*0.85));
      if(high+10<=maxBid&&Math.random()>.35){high+=10;winner=bp.id;gLog(`🤖 ${bp.name} bietet M ${high}.`,'ai');}
      else gLog(`🤖 ${bp.name} passt.`,'ai');
      i++;next();return;
    }
    const min=high+10;
    showModal(`🔨 Auktion: ${def.name}`,`<div class="mbody">${bp.name}<br>Höchstgebot: <b>M ${high}</b><br>Mindestgebot: M ${min}<br>Guthaben: M ${bp.money}</div>`,[
      {l:`Biete M ${min}`,c:'mb-y',fn:()=>{if(bp.money>=min){high=min;winner=bp.id;}i++;closeModal();next();}},
      {l:'Passen',c:'mb-n',fn:()=>{i++;closeModal();next();}},
    ],false);
  }
  next();
}

/* ══════════════════════════════════════════════════
   AI PLAYER
══════════════════════════════════════════════════ */
function aiTakeTurn(){
  const p=G.pl[G.cur];
  if(!p.isAI||p.bust)return;
  stopTimer();
  document.getElementById('ai-thinking').style.display='block';
  gLog(`🤖 ${p.name} denkt nach…`,'ai');

  setTimeout(()=>{
    document.getElementById('ai-thinking').style.display='none';
    // AI: try to pay bail first
    if(p.jailed&&p.jailCard>0){payBail();}
    else if(p.jailed&&p.money>200&&p.jailRounds>=2){payBail();}
    // Roll
    setTimeout(()=>{
      if(p.jailed) jailRoll();
      else if(!G.rolled) rollDice();
    },600);
  },900+Math.random()*600);
}

function aiPostLand(p,dbl){
  // AI decides: buy buildings?
  setTimeout(()=>{
    aiBuildIfPossible(p);
    setTimeout(()=>{
      if(!dbl)endTurn();
      else{setTimeout(aiTakeTurn,800);}
    },700);
  },400);
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

// Hook AI into buy/rent decisions
const _origShowBuy=showBuy;
function showBuy(p,s,def,dbl){
  if(p.isAI){
    // AI decides to buy
    const shouldBuy=p.money>(def.price*1.4)||Math.random()>.3;
    if(shouldBuy&&p.money>=def.price){
      spendM(p,def.price);s.own=p.id;
      gLog(`🤖 ${p.name} kauft ${def.name}.`,'ai');renderAll();
      if(!dbl)enableEnd();else enableReroll();
    } else {
      gLog(`🤖 ${p.name} kauft ${def.name} nicht.`,'ai');
      if(RULES.auction) runAuction(s,def,dbl);
      else{if(!dbl)enableEnd();else enableReroll();}
    }
    return;
  }
  _origShowBuy(p,s,def,dbl);
}

// Override enableEnd to trigger AI next actions
const _origEnableEnd=enableEnd;
function enableEnd(){
  _origEnableEnd();
  const p=G.pl[G.cur];
  if(p.isAI){aiBuildIfPossible(p);setTimeout(endTurn,700);}
}
const _origEnableReroll=enableReroll;
function enableReroll(){
  _origEnableReroll();
  const p=G.pl[G.cur];
  if(p.isAI){setTimeout(aiTakeTurn,900);}
}

/* ══════════════════════════════════════════════════
   SPACE INFO (with property card)
══════════════════════════════════════════════════ */
function showSpaceInfo(pos){
  const s=G.sp[pos],def=BOARD[pos];if(!def)return;
  let html='';
  if(def.type==='prop'){
    const own=s.own!==null?G.pl[s.own]:null;
    html=`<div class="prop-card">
      <div class="prop-card-band" style="background:${def.color}">TITEL-URKUNDE</div>
      <div class="prop-card-body">
        <div class="prop-card-name">${def.name}</div>
        <div class="prop-card-price">M ${def.price} &nbsp;|&nbsp; Haus M ${def.hp}</div>
        <table class="prop-card-tbl">
          <tr><td>Miete</td><td>M ${def.rent[0]}</td></tr>
          <tr><td>Farbgruppe</td><td>M ${def.rent[0]*2}</td></tr>
          <tr><td>1 Haus</td><td>M ${def.rent[1]}</td></tr>
          <tr><td>2 Häuser</td><td>M ${def.rent[2]}</td></tr>
          <tr><td>3 Häuser</td><td>M ${def.rent[3]}</td></tr>
          <tr><td>4 Häuser</td><td>M ${def.rent[4]}</td></tr>
          <tr><td>🏨 Hotel</td><td>M ${def.rent[5]}</td></tr>
        </table>
        <div style="margin-top:8px;font-size:10px;color:#555">${own?'Gehört: '+own.name+' '+TOKENS[own.token]+(s.h>0?'&nbsp;|&nbsp;'+(s.h===5?'🏨 Hotel':s.h+' Haus'):'')+(s.mortg?' | ⚠ Hypotheziert':''):'Unverkauft'}</div>
      </div>
    </div>`;
  } else if(def.type==='rail'){
    const own=s.own!==null?G.pl[s.own]:null;
    html=`<div class="mbody">🚂 ${own?'Gehört: '+own.name+' '+TOKENS[own.token]:'Unverkauft'}</div>
      <table class="rtbl"><tr><td>1 Bahn</td><td>M 25</td></tr><tr><td>2 Bahnen</td><td>M 50</td></tr><tr><td>3 Bahnen</td><td>M 100</td></tr><tr><td>4 Bahnen</td><td>M 200</td></tr></table>`;
  } else html=`<div class="mbody">${def.name}</div>`;
  showModal(def.name,html,[{l:'OK',c:'mb-ok',fn:closeModal}],true);
}

/* Player card popup */
function showPlayerCard(pid){
  const p=G.pl[pid];
  const props=G.sp.filter(s=>s.own===pid);
  const railCount=props.filter(s=>s.type==='rail').length;
  const utilCount=props.filter(s=>s.type==='utility').length;
  let propsHtml='';
  const byColor={};
  props.filter(s=>s.type==='prop').forEach(s=>{const d=BOARD[s.id];if(!byColor[d.color])byColor[d.color]=[];byColor[d.color].push(s);});
  Object.entries(byColor).forEach(([col,sps])=>{
    const all=G.sp.filter(s=>s.type==='prop'&&BOARD[s.id].color===col);
    const full=all.every(s=>s.own===pid);
    propsHtml+=`<div style="display:flex;align-items:center;gap:6px;margin-bottom:4px">
      <div style="width:10px;height:20px;background:${col};border-radius:2px;flex-shrink:0"></div>
      <div style="font-size:10px">${sps.map(s=>{const d=BOARD[s.id];return `<span>${d.name}${s.h>0?'('+( s.h===5?'🏨':s.h+'🏠')+')':''}</span>`;}).join(', ')}${full?' ✓':''}</div>
    </div>`;
  });
  if(railCount>0) propsHtml+=`<div style="font-size:10px;margin-bottom:3px">🚂 ${railCount} Bahnhof/Bahnhöfe</div>`;
  if(utilCount>0) propsHtml+=`<div style="font-size:10px;margin-bottom:3px">⚡💧 ${utilCount} Versorgungswerk(e)</div>`;
  showModal(`${TOKENS[p.token]} ${p.name}`,`
    <div style="text-align:left">
      <div class="mbody" style="margin-bottom:8px">💰 M ${p.money.toLocaleString('de-DE')}&nbsp;&nbsp;|&nbsp;&nbsp;Gesamtvermögen: <b>M ${totalWealth(p).toLocaleString('de-DE')}</b></div>
      <div style="font-size:9px;text-transform:uppercase;letter-spacing:2px;color:var(--gold);margin-bottom:8px">Grundstücke (${props.length})</div>
      ${propsHtml||'<div style="font-size:10px;color:#666">Keine</div>'}
      <div style="margin-top:10px;font-size:9px;color:#888">🎯 Runden: ${p.stats.turns} &nbsp;|&nbsp; 📥 Miete kassiert: M ${p.stats.totalRent.toLocaleString('de-DE')} &nbsp;|&nbsp; 📤 Miete gezahlt: M ${p.stats.totalPaid.toLocaleString('de-DE')}</div>
    </div>`,
    [{l:'Schließen',c:'mb-ok',fn:closeModal}],true);
}

/* ══════════════════════════════════════════════════
   BUILD / MORTGAGE
══════════════════════════════════════════════════ */
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
        <div style="width:11px;height:38px;background:${col};border-radius:2px;flex-shrink:0"></div>
        <div style="flex:1;min-width:0">
          <div style="font-size:11px;font-weight:700">${d.name}</div>
          <div style="font-size:9px;color:#999">${s.h<5?s.h+' Haus':'🏨 Hotel'} | Bau M ${d.hp}</div>
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
function doSell(id){const p=G.pl[G.cur],s=G.sp[id],d=BOARD[id];const v=Math.floor(d.hp/2);p.money+=v;s.h--;gLog(`${p.name} verkauft Haus M ${v}.`,'money');renderAll();closeModal();openBuild();}
function doMortg(id){const p=G.pl[G.cur],s=G.sp[id],d=BOARD[id];const v=Math.floor(d.price/2);p.money+=v;s.mortg=true;gLog(`${p.name} hypotheziert ${d.name} M ${v}.`,'warn');renderAll();closeModal();openBuild();}
function doUnmortg(id){const p=G.pl[G.cur],s=G.sp[id],d=BOARD[id];const v=Math.ceil(d.price/2*1.1);p.money-=v;s.mortg=false;gLog(`${p.name} löst Hypothek ab M ${v}.`,'money');renderAll();closeModal();openBuild();}

/* ══════════════════════════════════════════════════
   TRADE
══════════════════════════════════════════════════ */
function openTrade(){
  const p=G.pl[G.cur];
  const others=G.pl.filter(x=>!x.bust&&x.id!==p.id&&!x.isAI);
  if(!others.length){showModal('🤝 Handel','<div class="mbody">Keine menschlichen Mitspieler verfügbar.</div>',[{l:'OK',c:'mb-ok',fn:closeModal}],true);return;}
  let html='<div style="text-align:left">';
  others.forEach(o=>{html+=`<div class="brow" style="cursor:pointer" onclick="tradeWith(${o.id})"><div style="width:28px;height:28px;border-radius:50%;background:${o.color};display:flex;align-items:center;justify-content:center;font-size:14px">${TOKENS[o.token]}</div><div style="flex:1"><div style="font-size:12px;font-weight:700">${o.name}</div><div style="font-size:10px;color:#999">M ${o.money.toLocaleString('de-DE')} | ${G.sp.filter(s=>s.own===o.id).length} Gr.</div></div><div style="color:#FFD700;font-size:18px">›</div></div>`;});
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
        <div style="max-height:120px;overflow-y:auto;margin-bottom:6px">${propChecks(mp,'trp-p')}</div>
        <div style="color:#999;font-size:10px;margin-bottom:3px">+ Geld:</div>
        <input type="number" id="tr-mm" value="0" min="0" max="${p.money}" step="10" style="width:100%;background:#071507;color:#FFD700;border:1px solid #C8A000;padding:5px;border-radius:3px;font-size:13px">
        <div style="font-size:9px;color:#888;margin-top:2px">Guthaben: M ${p.money.toLocaleString('de-DE')}</div></div>
      <div><div style="color:#FFD700;font-weight:700;margin-bottom:6px">${TOKENS[o.token]} ${o.name} gibt:</div>
        <div style="max-height:120px;overflow-y:auto;margin-bottom:6px">${propChecks(op,'trp-o')}</div>
        <div style="color:#999;font-size:10px;margin-bottom:3px">+ Geld:</div>
        <input type="number" id="tr-om" value="0" min="0" max="${o.money}" step="10" style="width:100%;background:#071507;color:#FFD700;border:1px solid #C8A000;padding:5px;border-radius:3px;font-size:13px">
        <div style="font-size:9px;color:#888;margin-top:2px">Guthaben: M ${o.money.toLocaleString('de-DE')}</div></div>
    </div>`,
    [{l:'✅ Handel',c:'mb-y',fn:()=>{
      const mm=parseInt(document.getElementById('tr-mm').value)||0;
      const om=parseInt(document.getElementById('tr-om').value)||0;
      if(mm>p.money){toast(`${p.name} hat nicht genug!`);return;}
      if(om>o.money){toast(`${o.name} hat nicht genug!`);return;}
      const pSel=mp.filter(s=>document.getElementById('trp-p'+s.id)?.checked);
      const oSel=op.filter(s=>document.getElementById('trp-o'+s.id)?.checked);
      pSel.forEach(s=>{s.own=o.id;});oSel.forEach(s=>{s.own=p.id;});
      p.money+=om-mm;o.money+=mm-om;
      gLog(`Handel: ${p.name}↔${o.name} abgeschlossen.`,'info');
      renderAll();closeModal();toast('Handel! 🤝');
    }},{l:'❌',c:'mb-n',fn:closeModal}],true);
}

/* ══════════════════════════════════════════════════
   STATS
══════════════════════════════════════════════════ */
function openStats(){
  const alive=G.pl.filter(p=>!p.bust);
  const maxW=Math.max(...alive.map(p=>totalWealth(p)))||1;
  let bars=alive.map(p=>{
    const w=totalWealth(p);
    const pct=Math.round(w/maxW*100);
    return `<div class="wealth-bar" style="background:${p.color};height:${pct}%" title="${p.name}: M ${w.toLocaleString('de-DE')}">${pct>20?p.name.slice(0,4):''}</div>`;
  }).join('');

  let rows=G.pl.map(p=>{
    if(p.bust) return `<div class="brow" style="opacity:.4"><div style="width:28px;height:28px;border-radius:50%;background:${p.color};display:flex;align-items:center;justify-content:center;font-size:14px">${TOKENS[p.token]}</div><div style="flex:1"><div style="font-size:11px;font-weight:700">${p.name} 💀</div></div></div>`;
    const w=totalWealth(p);
    const props=G.sp.filter(s=>s.own===p.id);
    const houses=props.reduce((s,x)=>s+(x.h<5?x.h:0),0);
    const hotels=props.filter(x=>x.h===5).length;
    return `<div class="brow" style="cursor:pointer" onclick="showPlayerCard(${p.id});closeModal()">
      <div style="width:32px;height:32px;border-radius:50%;background:${p.color};display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0">${TOKENS[p.token]}</div>
      <div style="flex:1">
        <div style="font-weight:700;font-size:12px">${p.name}${p.isAI?' 🤖':''}</div>
        <div style="font-size:9px;color:#aaa;margin-top:2px">💰 M${p.money.toLocaleString('de-DE')} | 🏘 ${props.length} Gr. | 🏠${houses} 🏨${hotels}</div>
        <div style="font-size:9px;color:#888">📥 M${p.stats.totalRent.toLocaleString('de-DE')} kassiert | 📤 M${p.stats.totalPaid.toLocaleString('de-DE')} gezahlt</div>
      </div>
      <div style="text-align:right;font-size:14px;font-weight:700;color:#FFD700">M ${w.toLocaleString('de-DE')}</div>
    </div>`;
  }).join('');

  showModal('📊 Vermögen & Statistiken',`
    <div class="wealth-chart">${bars}</div>
    <div style="text-align:left;margin-top:12px">${rows}</div>
    <div style="text-align:left;margin-top:10px;font-size:9px;color:#666">Runde ${G.turn} | Freiparkpool: M ${G.pool}</div>`,
    [{l:'Schließen',c:'mb-ok',fn:closeModal}],true);
}

/* ══════════════════════════════════════════════════
   SAVE / LOAD
══════════════════════════════════════════════════ */
function autoSave(final=false){
  try{
    const save={G,RULES,ts:Date.now(),final};
    localStorage.setItem('monopoly_pro_save',JSON.stringify(save));
    const badge=document.getElementById('save-badge');
    badge.textContent='💾 Gespeichert '+new Date().toLocaleTimeString('de-DE',{hour:'2-digit',minute:'2-digit'});
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
    if(age>1000*60*60*24)return; // ignore saves older than 24h
    const mins=Math.round(age/60000);
    showModal('💾 Gespeichertes Spiel',`<div class="mbody">Es gibt ein gespeichertes Spiel von vor ${mins} Minute(n).<br>Möchtest du weiterspielen?</div>`,
      [{l:'▶ Weiterspielen',c:'mb-y',fn:()=>{closeModal();loadGame(save);}},
       {l:'Neues Spiel',c:'mb-n',fn:closeModal}],false);
  }catch(e){}
}
function loadGame(save){
  Object.assign(RULES,save.RULES);
  G=save.G;
  // Restore card decks (functions aren't JSON-serializable, so re-map)
  G.chanceDeck=G.chanceDeck.map((_,i)=>CHANCE[i%CHANCE.length]);
  G.chestDeck=G.chestDeck.map((_,i)=>CHEST[i%CHEST.length]);
  moving=false;
  document.getElementById('setup').style.display='none';
  document.getElementById('game').style.display='flex';
  document.getElementById('b-undo').style.display=RULES.undo?'block':'none';
  buildBoard();buildAnimTokens();
  G.pl.forEach(p=>{if(!p.bust)placeAnimTok(p.id,p.pos);});
  renderAll();
  gLog('Spiel geladen!','info');
  toast('Spiel geladen! Willkommen zurück.');
  const cur=G.pl[G.cur];
  if(cur.isAI) setTimeout(aiTakeTurn,1200);
  else startTimer();
}

/* ══════════════════════════════════════════════════
   MODAL / LOG / TOAST
══════════════════════════════════════════════════ */
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
  while(el.children.length>100)el.removeChild(el.firstChild);
}
function toast(msg){
  const el=document.getElementById('toast');el.textContent=msg;el.style.opacity='1';
  if(toastT)clearTimeout(toastT);toastT=setTimeout(()=>el.style.opacity='0',3200);
}

/* ══════════════════════════════════════════════════
   INIT
══════════════════════════════════════════════════ */
buildSetup();
setTimeout(checkSavedGame, 400);
</script>
</body>
</html>
