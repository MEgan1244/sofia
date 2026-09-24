<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sofia's 16th Birthday</title>
<style>
  :root{
    --blue:#2f6fd6;
    --blue-dark:#1f4fa8;
    --cream:#faf3e3;
    color-scheme: light;
  }
  *{ box-sizing: border-box; }
  html,body{
    height:100%; margin:0; padding:0;
    background:#0e1b33;
    font-family: 'Georgia', 'Iowan Old Style', serif;
    overflow:hidden;
    -webkit-tap-highlight-color: transparent;
  }
  body{
    display:flex; align-items:center; justify-content:center;
    padding-top:env(safe-area-inset-top,0px);
    padding-bottom:env(safe-area-inset-bottom,0px);
  }
  #stage{
    position:relative;
    width:min(94vw, calc(94vh * 0.7069));
    height:min(94vh, calc(94vw / 0.7069));
    aspect-ratio: 595 / 842;
    background:#7fb0e8;
    border-radius:18px;
    overflow:hidden;
    box-shadow:0 30px 80px rgba(0,0,0,0.55), 0 0 0 1px rgba(255,255,255,0.08);
  }
  .slide{
    position:absolute; inset:0;
    background-size:cover; background-position:center;
    opacity:0; visibility:hidden;
    pointer-events:none;
    transition: opacity .55s ease;
  }
  .slide.active{
    opacity:1; visibility:visible;
    pointer-events:auto;
  }
  .slide img.bg{
    position:absolute; inset:0; width:100%; height:100%; object-fit:cover;
    display:block;
    -webkit-user-drag:none; user-select:none;
  }

  /* --- generic arrow button --- */
  .nav-arrow{
    position:absolute;
    right:16px; bottom:16px;
    width:58px; height:58px;
    border-radius:50%;
    border:none;
    background:var(--blue);
    color:white;
    display:flex; align-items:center; justify-content:center;
    box-shadow:0 8px 20px rgba(0,0,0,0.35);
    cursor:pointer;
    z-index:20;
    opacity:0; transform:translateY(10px) scale(0.85);
    pointer-events:none;
    transition: opacity .4s ease, transform .4s ease, background .2s ease;
  }
  .nav-arrow.show{ opacity:1; transform:translateY(0) scale(1); pointer-events:auto; }
  .nav-arrow:hover{ background:var(--blue-dark); }
  .nav-arrow svg{ width:26px; height:26px; }

  .back-arrow{
    left:16px; right:auto;
  }

  /* --- slide 1 : cover --- */
  #s1 .bg{ animation: coverIn 1.1s ease both; }
  @keyframes coverIn{
    from{ opacity:0; transform:scale(1.06); }
    to{ opacity:1; transform:scale(1); }
  }

  /* --- slide 2 : blow candles --- */
  #s2 .hint{
    position:absolute;
    left:50%; bottom:9%;
    transform:translateX(-50%);
    width:82%;
    text-align:center;
    color:#0d2b57;
    background:rgba(255,255,255,0.88);
    border-radius:16px;
    padding:12px 14px;
    font-size:clamp(11px, 2.6vw, 14px);
    line-height:1.45;
    box-shadow:0 6px 18px rgba(0,0,0,0.18);
    z-index:15;
  }
  #s2 .hint b{ color:var(--blue-dark); }
  #s2 .blow-btn{
    display:inline-flex; align-items:center; gap:6px;
    margin-top:8px;
    background:var(--blue);
    color:#fff;
    border:none;
    border-radius:999px;
    padding:8px 16px;
    font-family:inherit;
    font-size:clamp(11px,2.6vw,13px);
    cursor:pointer;
    user-select:none;
  }
  #s2 .blow-btn:active{ background:var(--blue-dark); }
  #s2 .mic-btn{
    background:transparent;
    border:1px solid var(--blue);
    color:var(--blue-dark);
    border-radius:999px;
    padding:8px 14px;
    font-family:inherit;
    font-size:clamp(11px,2.6vw,13px);
    cursor:pointer;
    margin-left:6px;
  }
  #s2 .status{
    display:block;
    margin-top:6px;
    font-size:clamp(10px,2.3vw,12px);
    color:#3a4a63;
    min-height:14px;
  }
  #s2 .progress-wrap{
    width:100%;
    height:9px;
    margin-top:9px;
    background:rgba(13,43,87,0.15);
    border-radius:999px;
    overflow:hidden;
  }
  #s2 .progress-bar{
    height:100%; width:0%;
    background:linear-gradient(90deg, var(--blue), #6fb1ff);
    border-radius:999px;
  }
  .flame-cover{
    position:absolute;
    width:3.4%; height:8%;
    left:0; top:0;
    transform:translate(-50%,-50%) scale(0.4);
    border-radius:50%;
    background:radial-gradient(ellipse at center, rgba(107,162,218,1) 55%, rgba(107,162,218,0) 100%);
    opacity:0;
    pointer-events:none;
    z-index:8;
    transition:opacity .35s ease, transform .35s ease;
  }
  .flame-cover.show{ opacity:1; transform:translate(-50%,-50%) scale(1); }
  .smoke{
    position:absolute;
    width:10px; height:10px;
    border-radius:50%;
    background:rgba(255,255,255,0.8);
    pointer-events:none;
    z-index:9;
    transform:translate(-50%,-50%);
    animation: smokeRise 1.1s ease-out forwards;
  }
  @keyframes smokeRise{
    0%{ opacity:0.85; transform:translate(-50%,-50%) scale(0.6); }
    100%{ opacity:0; transform:translate(-50%,-220%) scale(2.1); }
  }

  /* --- video-like auto sequence slides 3-6 --- */
  #s3, #s4, #s5, #s6{ background:#7fb0e8; }
  #s3 .bg, #s4 .bg{
    position:absolute; inset:0; width:100%; height:100%; object-fit:cover;
  }
  #s4.active .bg{ animation: flashPop .45s ease both; }
  @keyframes flashPop{
    0%{ opacity:0; transform:scale(0.97); filter:brightness(1); }
    35%{ opacity:1; transform:scale(1.01); filter:brightness(1.9); }
    100%{ opacity:1; transform:scale(1); filter:brightness(1); }
  }
  .whiteflash{
    position:absolute; inset:0; background:#fff; opacity:0; z-index:5; pointer-events:none;
  }
  #s4.active .whiteflash{ animation: flashWhite .45s ease both; }
  @keyframes flashWhite{
    0%{ opacity:0; }
    18%{ opacity:.95; }
    100%{ opacity:0; }
  }

  #s5 .strip{
    position:absolute; inset:0; width:100%; height:100%; object-fit:cover;
    transform:translateY(0);
  }
  #s5.active .strip{ animation: stripUp 1.05s cubic-bezier(.2,.8,.2,1) both; }
  @keyframes stripUp{
    0%{ transform:translateY(70%); opacity:0; }
    100%{ transform:translateY(0); opacity:1; }
  }

  #s6 .bg{ position:absolute; inset:0; width:100%; height:100%; object-fit:cover; opacity:0; }
  /* middle column (photobooth strip) stays put — no animation */
  #s6 .colM{ opacity:1; transform:none; }
  /* left & right columns rise up into place */
  #s6.active .colL{ animation: comeUp .75s cubic-bezier(.2,.8,.2,1) both; animation-delay:.1s; }
  #s6.active .colR{ animation: comeUp .75s cubic-bezier(.2,.8,.2,1) both; animation-delay:.35s; }
  @keyframes comeUp{ from{ transform:translateY(65%); opacity:0;} to{ transform:translateY(0); opacity:1;} }
  #s6 .col{
    position:absolute; top:0; height:100%;
    background-image:var(--fullbg);
    background-size: 300% 100%;
    background-repeat:no-repeat;
  }
  .colL{ left:0; width:33.4%; background-position: 0% 0; }
  .colM{ left:33.3%; width:33.4%; background-position: 50% 0; }
  .colR{ left:66.6%; width:33.4%; background-position: 100% 0; }

  .skip-hint{
    position:absolute; top:12px; left:50%; transform:translateX(-50%);
    color:rgba(255,255,255,0.85);
    background:rgba(0,0,0,0.28);
    padding:5px 12px; border-radius:999px;
    font-size:11px; letter-spacing:.03em;
    z-index:25;
  }

  /* --- balloons on the cover --- */
  .balloon{
    position:absolute;
    bottom:-18%;
    width:9%;
    aspect-ratio: 0.72;
    pointer-events:none;
    opacity:0;
    animation: balloonRise linear infinite;
    z-index:6;
    filter: drop-shadow(0 6px 10px rgba(0,0,0,0.15));
  }
  .balloon svg{ width:100%; height:100%; display:block; }
  @keyframes balloonRise{
    0%{ bottom:-18%; opacity:0; transform:translateX(0) rotate(-4deg); }
    8%{ opacity:0.95; }
    50%{ transform:translateX(16px) rotate(4deg); }
    92%{ opacity:0.95; }
    100%{ bottom:112%; opacity:0; transform:translateX(-12px) rotate(-3deg); }
  }

  /* --- floating music toggle --- */
  .music-btn{
    position:absolute;
    top:14px; right:14px;
    z-index:40;
    background:rgba(13,26,50,0.55);
    color:#fff;
    border:none;
    border-radius:999px;
    padding:8px 14px;
    font-size:12px;
    font-family:inherit;
    cursor:pointer;
    display:flex; align-items:center; gap:6px;
    box-shadow:0 4px 14px rgba(0,0,0,0.3);
    backdrop-filter: blur(4px);
  }
  .music-btn:active{ transform:scale(0.96); }

  .sound-toast{
    position:absolute;
    top:14px; right:14px;
    transform:translateY(52px);
    z-index:39;
    background:rgba(13,26,50,0.85);
    color:#fff;
    border-radius:10px;
    padding:6px 10px;
    font-size:10.5px;
