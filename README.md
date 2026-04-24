<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#1a0533">
<title>Тест: Компьютерные сети</title>
<style>
:root{
  --bg:#1a0533;--surface:#2d0f52;--surface2:#3d1668;--border:#5a2d8a;
  --accent:#c084fc;--accent2:#a855f7;--accentd:#7c3aed;
  --correct:#34d399;--wrong:#f87171;--warn:#fbbf24;
  --text:#f3e8ff;--muted:#a78bca;--radius:14px;
  --font:-apple-system,"SF Pro Display","Helvetica Neue",Arial,sans-serif;
  --st:env(safe-area-inset-top,0px);
  --sb:env(safe-area-inset-bottom,0px);
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
html{height:100%;-webkit-text-size-adjust:100%}
body{height:100%;background:var(--bg);color:var(--text);font-family:var(--font);overflow:hidden}
body::before{content:"";position:fixed;inset:0;
  background:radial-gradient(ellipse at 20% 20%,rgba(168,85,247,0.15) 0%,transparent 60%),
             radial-gradient(ellipse at 80% 80%,rgba(192,132,252,0.1) 0%,transparent 60%);
  pointer-events:none;z-index:0}

/* screens */
.screen{position:fixed;inset:0;z-index:1;display:none;flex-direction:column}
.screen.active{display:flex}

/* ── START ── */
#start-screen{
  align-items:center;justify-content:center;text-align:center;
  padding:calc(1.5rem + var(--st)) 1.25rem calc(1.5rem + var(--sb));
  overflow-y:auto;-webkit-overflow-scrolling:touch;gap:0
}
.badge{display:inline-flex;align-items:center;gap:8px;
  background:rgba(192,132,252,0.12);border:1px solid rgba(192,132,252,0.35);
  border-radius:100px;padding:6px 18px;margin-bottom:1.5rem;
  font-size:11px;color:var(--accent);letter-spacing:2px;text-transform:uppercase;font-weight:600}
.dot{width:7px;height:7px;border-radius:50%;background:var(--accent);animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:0.4;transform:scale(0.7)}}
.s-title{font-size:clamp(22px,8vw,46px);font-weight:800;line-height:1.1;margin-bottom:0.75rem;
  background:linear-gradient(135deg,#e9d5ff,#c084fc,#a855f7);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.s-sub{font-size:14px;color:var(--muted);margin-bottom:2rem;line-height:1.7}
.igrid{display:grid;grid-template-columns:1fr 1fr;gap:10px;width:100%;max-width:320px;margin:0 auto 2rem}
.ic{background:rgba(255,255,255,0.05);border:1px solid rgba(192,132,252,0.2);
  border-radius:var(--radius);padding:0.9rem 0.6rem;text-align:center}
.ic-icon{font-size:22px;margin-bottom:5px}
.ic-val{font-size:20px;font-weight:700;color:var(--accent);margin-bottom:3px}
.ic-lbl{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;font-weight:600}
.btn-go{display:flex;align-items:center;justify-content:center;gap:10px;
  background:linear-gradient(135deg,#a855f7,#7c3aed);color:#fff;
  font-size:15px;font-weight:700;border:none;border-radius:100px;
  padding:16px;cursor:pointer;min-height:54px;width:100%;max-width:300px;
  box-shadow:0 8px 32px rgba(168,85,247,0.4);touch-action:manipulation}
.btn-go:active{opacity:0.85;transform:scale(0.97)}

/* ── QUIZ ── */
#quiz-screen{background:var(--bg)}

.topbar{
  flex-shrink:0;background:var(--surface);border-bottom:1px solid var(--border);
  padding:calc(10px + var(--st)) 1rem 10px
}
.topbar-r1{display:flex;align-items:center;justify-content:space-between;margin-bottom:8px}
.top-label{font-size:12px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:1px}
.timer{display:flex;align-items:center;gap:5px;
  background:rgba(255,255,255,0.07);border:1px solid var(--border);
  border-radius:100px;padding:5px 14px}
.timer.warn{border-color:var(--warn);background:rgba(251,191,36,0.1)}
.timer.danger{border-color:var(--wrong);background:rgba(248,113,113,0.15);animation:tblink 1s infinite}
@keyframes tblink{0%,100%{opacity:1}50%{opacity:0.45}}
.t-val{font-size:15px;font-weight:700;letter-spacing:2px;min-width:50px;text-align:center}
.timer.warn .t-val{color:var(--warn)}
.timer.danger .t-val{color:var(--wrong)}
.prog-row{display:flex;align-items:center;gap:10px}
.q-cnt{font-size:12px;font-weight:700;color:var(--accent);white-space:nowrap;min-width:40px}
.pbar{flex:1;height:5px;background:rgba(255,255,255,0.1);border-radius:3px;overflow:hidden}
.pfill{height:100%;background:linear-gradient(90deg,#a855f7,#7c3aed);border-radius:3px;transition:width 0.3s}

.qbody{flex:1;overflow-y:auto;-webkit-overflow-scrolling:touch;padding:1rem}
.qcard{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:1.25rem}
.qlabel{font-size:11px;font-weight:700;color:var(--accent);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:10px}
.qtext{font-size:15px;line-height:1.65;color:var(--text);margin-bottom:1.25rem;font-weight:500}
.opts{display:flex;flex-direction:column;gap:9px}
.opt{display:flex;align-items:flex-start;gap:12px;padding:13px 14px;
  background:rgba(255,255,255,0.04);border:1.5px solid rgba(255,255,255,0.1);
  border-radius:12px;cursor:pointer;touch-action:manipulation;
  -webkit-user-select:none;user-select:none;min-height:50px}
.opt:active{background:rgba(168,85,247,0.1);border-color:rgba(192,132,252,0.5)}
.opt.sel{border-color:var(--accent);background:rgba(168,85,247,0.12)}
.ol{font-size:11px;font-weight:800;min-width:26px;height:26px;border-radius:7px;
  display:flex;align-items:center;justify-content:center;
  background:rgba(255,255,255,0.08);color:var(--muted);flex-shrink:0;margin-top:1px}
.opt.sel .ol{background:var(--accent2);color:#fff}
.ot{font-size:14px;color:var(--text);line-height:1.5;padding-top:3px;word-break:break-word}

.botnav{flex-shrink:0;background:var(--surface);border-top:1px solid var(--border);
  padding:10px 1rem calc(10px + var(--sb))}
.botnav-inner{display:flex;gap:10px;max-width:640px;margin:0 auto}
.btn-back{flex:1;padding:13px;border:1.5px solid var(--border);border-radius:100px;
  background:transparent;color:var(--muted);font-size:13px;font-weight:600;
  cursor:pointer;touch-action:manipulation;min-height:48px}
.btn-back:active{background:rgba(255,255,255,0.05)}
.btn-nxt{flex:2;padding:13px;background:linear-gradient(135deg,#a855f7,#7c3aed);
  color:#fff;font-size:14px;font-weight:700;border:none;border-radius:100px;
  cursor:pointer;touch-action:manipulation;min-height:48px;
  box-shadow:0 4px 20px rgba(168,85,247,0.3)}
.btn-nxt:active{opacity:0.85;transform:scale(0.97)}
.btn-nxt.finish{background:linear-gradient(135deg,#34d399,#059669);box-shadow:0 4px 20px rgba(52,211,153,0.3)}

/* ── RESULT ── */
#result-screen{overflow-y:auto;-webkit-overflow-scrolling:touch;
  padding:calc(1.5rem + var(--st)) 1rem calc(1.5rem + var(--sb));align-items:center}
.rwrap{max-width:600px;width:100%}
.rhead{text-align:center;margin-bottom:1.75rem}
.rgrade{font-size:clamp(30px,10vw,58px);font-weight:800;margin-bottom:0.5rem;line-height:1}
.rgrade.excellent{background:linear-gradient(135deg,#34d399,#059669);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.rgrade.good{background:linear-gradient(135deg,#c084fc,#7c3aed);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.rgrade.ok{color:var(--warn)}
.rgrade.fail{color:var(--wrong)}
.rsub{font-size:13px;color:var(--muted);font-weight:600;letter-spacing:1px;text-transform:uppercase}
.sgrid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:1.5rem}
.sc{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:1rem;text-align:center}
.sv{font-size:28px;font-weight:800;margin-bottom:3px}
.sv.g{color:var(--correct)}.sv.r{color:var(--wrong)}.sv.b{color:var(--accent)}.sv.m{color:var(--muted)}
.sl{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;font-weight:600}
.rbtns{display:flex;flex-direction:column;gap:10px;margin-bottom:1.5rem}
.btn-new{display:flex;align-items:center;justify-content:center;gap:8px;
  background:linear-gradient(135deg,#a855f7,#7c3aed);color:#fff;
  font-size:14px;font-weight:700;border:none;border-radius:100px;
  padding:16px;cursor:pointer;touch-action:manipulation;min-height:52px;
  box-shadow:0 4px 20px rgba(168,85,247,0.35)}
.btn-new:active{opacity:0.85;transform:scale(0.97)}
.btn-err{display:flex;align-items:center;justify-content:center;
  background:transparent;color:var(--accent);font-size:13px;font-weight:600;
  border:1.5px solid var(--border);border-radius:100px;
  padding:14px;cursor:pointer;touch-action:manipulation;min-height:48px}
.btn-err:active{background:rgba(255,255,255,0.05)}
.rsep{display:flex;align-items:center;gap:10px;font-size:11px;font-weight:700;
  color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin-bottom:1rem}
.rsep::after{content:"";flex:1;height:1px;background:var(--border)}
.rvc{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);
  padding:1rem;margin-bottom:8px;word-break:break-word}
.rvc.ok{border-left:3px solid var(--correct)}.rvc.err{border-left:3px solid var(--wrong)}.rvc.skip{border-left:3px solid var(--muted)}
.rvq{font-size:13px;color:var(--text);margin-bottom:8px;line-height:1.6;font-weight:500}
.rva{display:flex;align-items:flex-start;gap:7px;font-size:12px;padding:6px 10px;border-radius:8px;margin-top:4px;line-height:1.5}
.rva.aok{background:rgba(52,211,153,0.1);color:var(--correct)}
.rva.aerr{background:rgba(248,113,113,0.1);color:var(--wrong)}
.rva.ask{background:rgba(255,255,255,0.05);color:var(--muted)}
.rtag{font-size:9px;font-weight:800;padding:2px 6px;border-radius:4px;flex-shrink:0;margin-top:2px;white-space:nowrap}
.aok .rtag{background:var(--correct);color:#052e16}
.aerr .rtag{background:var(--wrong);color:#fff}
.ask .rtag{background:var(--muted);color:#fff}

/* modal instead of confirm */
.modal-bg{display:none;position:fixed;inset:0;z-index:200;
  background:rgba(26,5,51,0.85);align-items:flex-end;justify-content:center}
.modal-bg.show{display:flex}
.modal{background:var(--surface);border:1px solid var(--border);
  border-radius:20px 20px 0 0;padding:1.5rem 1.25rem calc(1.5rem + var(--sb));
  width:100%;max-width:480px;text-align:center}
.modal h3{font-size:18px;font-weight:700;margin-bottom:0.75rem;color:var(--text)}
.modal p{font-size:14px;color:var(--muted);margin-bottom:1.5rem;line-height:1.6}
.modal-btns{display:flex;flex-direction:column;gap:10px}
.mbtn-yes{padding:14px;background:linear-gradient(135deg,#34d399,#059669);
  color:#fff;font-size:14px;font-weight:700;border:none;border-radius:100px;
  cursor:pointer;touch-action:manipulation;min-height:50px}
.mbtn-yes:active{opacity:0.85}
.mbtn-no{padding:14px;background:transparent;color:var(--muted);
  font-size:14px;font-weight:600;border:1.5px solid var(--border);border-radius:100px;
  cursor:pointer;touch-action:manipulation;min-height:50px}
.mbtn-no:active{background:rgba(255,255,255,0.05)}

/* timeout */
#tovl{display:none;position:fixed;inset:0;z-index:300;
  background:rgba(26,5,51,0.97);align-items:center;justify-content:center;
  flex-direction:column;text-align:center;padding:2rem}
#tovl.show{display:flex}
.to-icon{font-size:64px;margin-bottom:1rem;animation:shake .5s .1s both}
@keyframes shake{0%,100%{transform:translateX(0)}25%{transform:translateX(-10px)}75%{transform:translateX(10px)}}
.to-title{font-size:clamp(22px,7vw,42px);font-weight:800;color:var(--wrong);margin-bottom:0.5rem}
.to-sub{font-size:14px;color:var(--muted);margin-bottom:2rem;line-height:1.6}
</style>
</head>
<body>

<!-- START -->
<div id="start-screen" class="screen active">
  <div class="badge"><span class="dot"></span>Экзаменационный тест</div>
  <h1 class="s-title">Компьютерные сети</h1>
  <p class="s-sub">SUBКомпьютерные сети</p>
  <div class="igrid">
    <div class="ic"><div class="ic-icon">📋</div><div class="ic-val">20</div><div class="ic-lbl">Вопросов</div></div>
    <div class="ic"><div class="ic-icon">⏱</div><div class="ic-val">40:00</div><div class="ic-lbl">Время</div></div>
    <div class="ic"><div class="ic-icon">🎯</div><div class="ic-val">5</div><div class="ic-lbl">Вариантов</div></div>
    <div class="ic"><div class="ic-icon">🔀</div><div class="ic-val">∞</div><div class="ic-lbl">Случайно</div></div>
  </div>
  <button class="btn-go" onclick="startQuiz()">▶ Начать тест</button>
</div>

<!-- QUIZ -->
<div id="quiz-screen" class="screen">
  <div class="topbar">
    <div class="topbar-r1">
      <span class="top-label">Тест</span>
      <div class="timer" id="timer"><span>⏱</span><span class="t-val" id="tval">40:00</span></div>
    </div>
    <div class="prog-row">
      <span class="q-cnt" id="qcnt">1 / 20</span>
      <div class="pbar"><div class="pfill" id="pfill" style="width:5%"></div></div>
    </div>
  </div>
  <div class="qbody" id="qbody"><div id="qwrap"></div></div>
  <div class="botnav">
    <div class="botnav-inner">
      <button class="btn-back" id="bbk" onclick="goPrev()">← Назад</button>
      <button class="btn-nxt" id="bnxt" onclick="goNext()">Следующий →</button>
    </div>
  </div>
</div>

<!-- RESULT -->
<div id="result-screen" class="screen">
  <div class="rwrap">
    <div class="rhead">
      <div class="rgrade" id="rgrade"></div>
      <div class="rsub" id="rsub"></div>
    </div>
    <div class="sgrid" id="sgrid"></div>
    <div class="rbtns">
      <button class="btn-new" onclick="newQuiz()">🔀 Новый тест</button>
      <button class="btn-err" onclick="scrollToReview()">📋 Разбор ошибок</button>
    </div>
    <div class="rsep" id="rsep">Разбор ответов</div>
    <div id="rlist"></div>
    <div style="height:1.5rem"></div>
  </div>
</div>

<!-- MODAL (вместо confirm) -->
<div class="modal-bg" id="modal">
  <div class="modal">
    <h3>Завершить тест?</h3>
    <p id="modal-msg">Вы ответили не на все вопросы.</p>
    <div class="modal-btns">
      <button class="mbtn-yes" onclick="confirmFinish()">Да, завершить</button>
      <button class="mbtn-no" onclick="closeModal()">Продолжить</button>
    </div>
  </div>
</div>

<!-- TIMEOUT -->
<div id="tovl">
  <div class="to-icon">⏰</div>
  <div class="to-title">Время вышло!</div>
  <div class="to-sub">40 минут истекли.<br>Результат посчитан автоматически.</div>
  <button class="btn-go" onclick="dismissTimeout()">Посмотреть результат</button>
</div>

<script>
const ALL_Q=[
{q:"Предоставляет услуги локальной сети.",o:["сервер","клавиатура","мэйнфрейм","услуга","попутчик"]},
{q:"Каналы связи набор компьютеров, соединенных посредством",o:["компьютерная сеть","конференция","дешифрование","сервер","порт назначения"]},
{q:"Показать сетевые протоколы",o:["TCP/IP, IPX/SPX и NEWLINK, APP, APLLE TALK, DECnet","Протокол, порт, интерфейс, фрагмент","Шина, звездочка, комплекс, кольцо","Местные и региональные сети","HTML, SQL, LINUX, UNIX"]},
{q:"Выберите компонент, не относящийся к сети.",o:["концентратор","коммуникация","маршрутизатор","мост","шлюз"]},
{q:"Устройство, которое разделяет общую среду передачи данных в сети на сегменты и пересылает входящие данные в другой сегмент, основываясь исключительно на его адресе.",o:["мост","шлюз","трафик","коммуникатор","концентратор"]},
{q:"Распространяется более чем на три компьютера в сети.",o:["адрес","авторизоваться","имя","рисунок","мое устройство"]},
{q:"Использует волоконно-оптические кабели для передачи двоичной информации.",o:["световые импульсы","световые сигналы","рамка","звуки","электрические сигналы"]},
{q:"Физическая конфигурация подключения компьютера",o:["топология","шифрование","слиять","дешифрование","для подключения"]},
{q:"Типы сетевых конфигураций",o:["полное соединение неполное соединение","контакт полный контакт","общение — это не общение","не команда","не связан с дорогой"]},
{q:"Сеть с множественными методами контроля доступа и обнаружения коллизий.",o:["ЭН","ФДДИ","TokenRing","Arcnet","нет ответа"]},
{q:"Сеть, которая восстанавливает свою способность к многозадачности после многочисленных отказов.",o:["ФДДИ","ArcNet","TokenRing","ЭН","Такой сети не существует."]},
{q:"Эталонная сетевая модель",o:["ОСИ","ЭН","Ксерокс","IEEE","ACSII"]},
{q:"Уровень, на котором данные шифруются и расшифровываются.",o:["Предложение","транспорт","Физический канал","онлайн","канал"]},
{q:"Обеспечивает передачу данных между компьютерами.",o:["TCP/IP (протокол управления передачей)","Межсетевой обмен пакетами / Последовательный обмен пакетами (IPX/SPX)","Сетевая система Xerox (XNS)","набор протоколов OLI","Apple Talk-Apple Macintosh"]},
{q:"Степень ослабления зависит от того, какая сторона регистрирует активность какой стороны в данный момент.",o:["сессионный","физический","пригодный для использования","Транспорт","онлайн"]},
{q:"Уровень модели OSI, на котором работают маршрутизаторы.",o:["Сеть","канал","применяемый","транспорт","онлайн"]},
{q:"Тип обмена информацией, предполагающий использование технологии передачи данных между двумя точками.",o:["Только между компьютерами","Только между компьютерами и периферийными устройствами.","Между всеми компонентами сети","Между приемопередатчиками хост-системы и компьютерами",""]},
{q:"Уровень, определяющий тип подключения сетевого кабеля адаптера.",o:["Физический","канал","транспорт","онлайн","слияние"]},
{q:"Почтовый стек, способный фрагментировать пакеты, имеющий подходящую систему адресации и отклоняющий широковещательные сообщения большой зоны.",o:["TCP\\SPX","СНА","ОСИ","NETBIOS/SMB","IPX/SPX"]},
{q:"Сообщение, позволяющее приложениям взаимодействовать и обмениваться информацией.",o:["Применяемый","Транспорт","Сеть","Преданный","маршрут"]},
{q:"Пакет, который должен быть доставлен одновременно нескольким узлам, состоящим из пронумерованных узлов, указанных в поле адреса.",o:["Многоадресная рассылка","Ограниченное многостраничное повествование","обратная связь","Транслировать","Ограниченное вещание"]},
{q:"Приложение, поддерживающее все необходимые методы работы с группами новостей по электронной почте.",o:["Windows Internet Explorer","АвтоCAD","MS Word","CorelDraw","AdobeResiders"]},
{q:"Метод размещения псевдослучайной последовательности в исходном имени генерации на основе ключей.",o:["пообщаться","Смена места","замена","Смешанное шифрование","Аналитическое преобразование"]},
{q:"Обеспечивает передачу данных между компьютерами.",o:["TCP/IP (протокол управления передачей)","Межсетевой обмен пакетами / Последовательный обмен пакетами (IPX/SPX)","Сетевая система Xerox (XNS)","набор протоколов OLI","Apple Talk-Apple Macintosh"]},
{q:"Что такое протокол SMTP (Simple Mail Transfer Protocol)?",o:["Электронная почта","Интернет","Для сети","Часть набора протоколов TCP/IP","стек протоколов"]},
{q:"В каком году была внедрена система электронной почты?",o:["1971","1972","1961","1965","1975"]},
{q:"Техническое оснащение Интернета — это:",o:["компьютерные узлы, маршрутизаторы, канал связи, компьютеры","компьютерные узлы, маршрутизаторы, канал связи","разъемы, маршрутизаторы, канал связи, компьютеры","компьютерные узлы, модем, канал связи, компьютеры","компьютерные узлы, маршрутизаторы, адреса, компьютеры"]},
{q:"Поставщик услуг -",o:["Компания, предоставляющая доступ в Интернет и услуги интернет-провайдера.","один или несколько мощных компьютеров, постоянно подключенных к сети.","устройство, которое пересылает пакеты информации из одного направления в другое через сеть.","Устройство, которое подключает компьютер пользователя к Интернету через телефонную линию.","Соглашение о правилах создания и поддержания сообщений в интернете. Инструмент для компаний, предоставляющих доступ к интернету и оказывающих услуги."]},
{q:"Главные компьютеры -",o:["один или несколько мощных компьютеров, постоянно подключенных к сети.","пересылает пакеты информации из одного направления в другое через сеть.","Устройство, которое подключает компьютер пользователя к Интернету через телефонную линию.","Инструмент для согласования правил создания и поддержания сообщений в интернете.","Компания, предоставляющая доступ в Интернет и услуги интернет-провайдера."]},
{q:"Какой тип контента веб-сайта не включен?",o:["разветвленный","линейный","гибридный","Деревья","решетка"]},
{q:"Пучок из двух проводов, один внутри другого.",o:["коаксиальный","двухпроводной","Сеть","технологический","Спиральная линия"]},
{q:"Устройство, соединяющее несколько сетей в топологии «звезда» с одной сетью в топологии «звезда».",o:["Шина для шоссе","модем","Терминатор","конденсатор","Сетевая карта"]},
{q:"Узел для отправки данных с использованием метода \"Пакеты связи\".",o:["Обмен данными происходит в момент отправки.","Информация фиксированной длины передается блоками и по первому нажатому каналу.","Прямая связь, соединяющая канал внутри группы для отправки сообщений между двумя клиентами.","Информация передается блоками фиксированной длины и по первому свободному каналу.","Информация передается блоками фиксированной длины."]},
{q:"Маршрутизаторы -",o:["пересылает пакеты информации из одного направления в другое через сеть.","Устройство, которое подключает компьютер пользователя к Интернету через телефонную линию.","Инструмент для согласования правил создания и поддержания сообщений в интернете.","Компания, предоставляющая доступ в Интернет и услуги интернет-провайдера.","один или несколько мощных компьютеров, постоянно подключенных к сети."]},
{q:"Тип передачи данных в узкополосных системах",o:["Одночастотный цифровой сигнал","Односторонний сигнал","Аналоговый сигнал","Цифровой сигнал на разных частотах","Световые импульсы"]},
{q:"Слой, который группирует необработанные биты с физического уровня в кадр данных.",o:["Канал","Применяемый","сессионный","Сеть","транспорт"]},
{q:"Путем размещения контрольных точек в потоке данных",o:["сессионный","онлайн","применяемый","Транспорт","канал"]},
{q:"Группа сетевых протоколов, позволяющих узлам взаимодействовать друг с другом в сети, организованной иерархическим образом.",o:["стек протоколов","Порядок протоколов","Протокольный канал","Протоколы протоколов","Группа протоколов"]},
{q:"Проверка адреса, маршрута и ошибок, повторная отправка запроса.",o:["Трафик","Применяемый","Сеть","Маршрут","Канал"]},
{q:"Сеть, начинающаяся с 0, принадлежит к следующему классу.",o:["А","Д","Е","С","Б"]},
{q:"Сетевые адреса, начинающиеся с 10, относятся к следующему классу.",o:["Б","С","А","Д","Е"]},
{q:"Сетевые адреса, начинающиеся с 110, относятся к следующему классу.",o:["С","А","Д","в","Е"]},
{q:"Развитие интернета обусловлено стоимостью передачи данных.",o:["каналы спутниковой связи","Коаксиальный кабель","Каналы пакетной связи","Модем","Обмоточная пара"]},
{q:"Название доменного имени указано справа.",o:["Полное доменное имя узла","Имя узла","Полудоменное имя Node.","Зимнее доменное имя узла","Доменное имя узла"]},
{q:"Универсально доступное имя",o:["Открыть","Основной","Бесплатно","Закрыто","Электронный"]},
{q:"Первая страница веб-сайта является корнем дерева, но конец сайта показывает, что все его содержимое находится внутри неё.",o:["Дерево","Линия","Случайный","Структурированный","Сетка"]},
{q:"Количество типов структур вычислительных узлов",o:["5","6","2","3","4"]},
{q:"Как присвоить имя модулю вычислительного узла",o:["логический модуль","терминальный модуль","модуль распределения","модуль хранения","модуль управления"]},
{q:"Основной элемент вычислительного узла",o:["Модуль хоста","Терминальный модуль","Модуль связи","Интерфейсный модуль","Модуль управления с вычислительным узлом"]},
{q:"Модуль, использующий ресурсы вычислительного узла, когда они предоставляются модулем хоста, — это...",o:["Терминальный модуль","Модуль хоста","Интерфейсный модуль","Модуль управления с вычислительным узлом","Модуль связи"]},
{q:"Модуль, выполняющий все функции, связанные с проверкой и управлением информацией после ее последовательной отправки по каналам, а также с направлением передачи информации.",o:["Модуль связи","Модуль хоста","Терминальный модуль","Интерфейсный модуль","Модуль управления с вычислительным узлом"]},
{q:"Для интеграции высокопроизводительных модулей",o:["Требуется интерфейсный модуль.","Требуется модуль хоста","Требуется терминальный модуль.","- Требуется модуль связи","Требуется модуль управления с вычислительным узлом."]},
{q:"Осуществляет управление вычислительным узлом.",o:["Модуль управления с вычислительным узлом","Модуль хоста","Терминальный модуль","Модуль связи","Интерфейсный модуль"]},
{q:"Рабочая структура вычислительного узла для определения эффективности логических модулей в технических структурах.",o:["физический","математический","алгебраический","информативный","технический"]},
{q:"Количество основных элементов программной структуры вычислительного узла",o:["2","1","3","4","5"]},
{q:"Точка соприкосновения между программами управления технологическим процессом и программами управления транспортировкой находится в следующем:",o:["порт","фрагмент","процесс","транспортный канал","протокол"]},
{q:"Сложность структуры распространяемого набора информации -",o:["фрагмент","процесс","порт","транспортный канал","протокол"]},
{q:"Количество названий фрагментов",o:["2","3","4","5","6"]},
{q:"Линии, соединяющие управляющую программу программной структуры в вычислительном узле -",o:["транспортные каналы","процесс","фрагмент","порт","протокол"]},
{q:"Взаимосвязь между смежными слоями программной структуры -",o:["интерфейс","протокол","ресурс","сеть","сообщение"]},
{q:"Описана работа всей вычислительной сети.",o:["в протоколах","в ресурсах","онлайн","в сообщении","на интерфейсе"]},
{q:"На сколько групп разделены протоколы?",o:["3","2","4","5","6"]},
{q:"Сколько существует типов многомашинных ассоциаций?",o:["3","4","5","6","7"]},
{q:"Как просто дать название терминальному комплексу",o:["Условный","Логический","Интеграл","Находчивый","Коммуникация"]},
{q:"Новый тип многомашинного объединения –",o:["Компьютерный центр","Вычислительный комплекс","Терминальный комплекс","Математические модели","Обработка информации на компьютерах"]},
{q:"Системы, входящие в состав вычислительного комплекса...",o:["Информация и справочные материалы","Телеобработка информации","Информация о земном шаре","Широкий спектр в целом","На отдельных компьютерах"]},
{q:"Цифровой вычислительный комплекс считается...",o:["простые задачи, связанные с внешними структурами и памятью","отчеты о производственном процессе","одна или несколько вычислительных машин","небольшие калькуляторы собственного импеданса","отдельные вычислительные машины"]},
{q:"Какие услуги предоставляет пользователю вычислительная сеть?",o:["Информационные системы и математические программы, инструменты для проведения интервью.","Информационные системы и логические программы, инструменты для создания правил диалога.","в технологических отчетах о производстве","Обработка информации на компьютерах","На одном или нескольких компьютерах"]},
{q:"Единый тип пользовательского канала для создания машин, работающих с вычислительной сетью...",o:["SDLC","MDLC","Cdlc","Рдлк","Фдлк"]},
{q:"Односторонний режим передачи данных",o:["симплекс","сложный","терминальный комплекс","вычислительный комплекс","математические модели"]},
{q:"Объём внутренней памяти, сколько бит и слов содержит каждая из них...",o:["12 цифр, 4000 слов","21 цифра, 2000 слов","13 цифр, 4000 слов","21 цифра, 4000 слов","12 цифр, 5000 слов"]},
{q:"До скольких миллионов символов в последовательном порту система SDS-7600 может хранить данные?",o:["5000 миллионов","7000 миллионов","2000 миллионов","4000 миллионов","3000 миллионов"]},
{q:"Как подразделяются инструменты для проведения интервью на...",o:["3","4","6","5","2"]},
{q:"Вычислительный комплекс –",o:["Программы, используемые для решения конкретных проблем.","Программы, интегрирующие внешние структуры","Центральный электронный переключатель во внешней конструкции","Информационные системы и математические программы, инструменты для проведения интервью.","Информационные системы и логические программы, инструменты для создания правил диалога."]},
{q:"Какие существуют типы структур вычислительных сетей?",o:["5","6","8","3","2"]},
{q:"Какой элемент вычислительного узла хост-модуля представляет собой...?",o:["основной","самый важный","Терминал","информативный","информационный - справочный"]},
{q:"После последовательной передачи данных по каналам устройство выполняет все функции, связанные с их проверкой и управлением, а также направлением информации...",o:["коммуникационный модуль","модуль хоста","модуль управления вычислительным узлом","интерфейсный модуль","терминальный модуль"]},
{q:"Локальная сеть — это сеть из нескольких компьютеров, соединенных между собой.",o:["Интранет","Интернет","Интернет","Модуль связи","Модуль хоста"]},
{q:"Какой ответ верен для сетей с одним рангом?",o:["На основе сетей, содержащих не более десяти пользователей.","обеспечивает надежный уровень защиты и контроля","требуется мощный централизованный сервер","пользователи работают в глобальной сети","не требует мощного централизованного сервера"]},
{q:"Основные характеристики сети с «кольцевой» топологией:",o:["Требует меньше кабелей, чем другие топологии.","Система доставки проста и недорога.","Все компьютеры используются одинаково.","Для корректной работы необходим терминатор.","Система доставки сложна и дорогостояща."]},
{q:"Характеристики сетей с топологией «шина»:",o:["Система доставки проста и недорога.","Требует меньше кабелей, чем другие топологии.","Все компьютеры используются одинаково.","Для корректной работы необходим терминатор.","Нагрузка на компьютеры одинакова."]},
{q:"Характеристики сетей со звездообразной топологией:",o:["централизует управление и контроль сети.","обеспечивает надежный уровень защиты и контроля","требуется мощный централизованный сервер","пользователи работают в глобальной сети","пользователи работают с большой сетью"]},
{q:"Какая топология является пассивной?",o:["звездное кольцо","шина","с отправкой маркера","кольцо","смешанный"]},
{q:"Используется для нормализации сигналов в узкополосных системах.",o:["ретрансляторы","усилители","батарейки","блок питания","системный блок"]},
{q:"Линии, соединяющие сеть и управляющую программу в вычислительном узле, называются .........",o:["транспортные каналы","протоколы расчетных сетей","распределение","фрагмент","протоколы"]},
{q:"Что представляет собой вычислительный комплекс в качестве программного обеспечения?",o:["для решения определённых проблем","для записи данных","для отчетов о подсчете","Для доступа в Интернет","Предназначен для определения структуры компьютера."]},
{q:"Что может входить в состав вычислительной системы?",o:["процессор, память, структура хранения данных на диске и компакт-диске, ввод/вывод, печать на бумаге","блок питания, стример, жесткий диск, дисковод","CD-ROM и DVD","материнская плата, принтер, жесткий диск, дисковод, CD-ROM устройства","компьютерные вирусы"]},
{q:"Приёмники называются...",o:["точка доступа","мост","разъем","ретранслятор","с системным блоком"]},
{q:"Можно удлинить кабель без усиления сигнала:",o:["с цилиндрическим соединителем","с терминатором","с ретранслятором","с концентратором","с модемом"]},
{q:"Что мы подразумеваем под идентичностью пользовательской программы и элементами управления сеансом, которые связывают пользовательскую программу?",o:["процесс","лаял","узел","машина","порт"]},
{q:"Что такое порт?",o:["Мы имеем в виду точку между технологическим процессом и программой контроля транспортировки.","описывает взаимоотношения между пользователем и его сторонником.","интерпретируется как распределение","базовый расчет расчетов troab","нет четкого определения"]},
{q:"Фрагмент — это...",o:["сложность структуры распределенного набора информации","точка между процессом и транспортировкой","порт","это сеть","узел маршрутизации пакетов"]},
{q:"Количество тем во фрагменте",o:["2","1","нет","3","4"]},
{q:"Показать заголовок фрагмента -",o:["процесс и распределение","только дистрибуция","просто процесс","фрагментные элементы","порт"]},
{q:"Что такое транспортный канал?",o:["линия, соединяющая сеть с управляющей программой в вычислительном узле.","точка, расположенная между технологическим процессом и программой управления транспортировкой.","сложность структуры сбора информации;","фрагментные элементы","Линия, разделяющая сеть и управляющую программу в вычислительном узле."]},
{q:"Что такое сеть?",o:["интегрированная группа компьютеров или других устройств","локальная сеть","локальная сеть","обмен информацией внутри учреждения","просто процесс"]},
{q:"Типы компьютерных сетей -",o:["территориальный, региональный, местный","район, регион","территориальный региональный местный корпоративный","интернет","веб"]},
{q:"Серверная машина",o:["компьютер с общими ресурсами","компьютер с модемным подключением","физическое расположение компьютеров относительно друг друга","компьютерная память","глобальная система"]},
{q:"Офисное программное обеспечение для работы в глобальной сети?",o:["Интернет-Проверка","программы местных коммуникационных сетей","корпоративная программа налаживания деловых связей","модем","Интернет"]},
{q:"На какой сети основан Интернет?",o:["Арпанет","CompuServer","Америка на Лине","Сеть MS","Доктор Веб"]},
{q:"Интернет — это",o:["Всемирная паутина","локальная компьютерная сеть между учреждениями","национальная компьютерная сеть","выделенная компьютерная сеть","локальная сеть"]},
{q:"Простейшая топология сети",o:["шина","звезда","овальной формы","асимметрия","кольцо"]},
{q:"Опишите роль электронной почты в интернете.",o:["Адрес электронной почты","испытательное устройство","база данных","игра","работа с текстом"]},
{q:"Как называется устройство, преобразующее цифровую информацию в сигналы и сообщения по телефонным каналам?",o:["модем","компьютер","аптека","телетайп","телефон"]},
{q:"Какое устройство необходимо для подключения компьютеров к локальной сети?",o:["сетевая среда","ТВ-тюнер","видеокарта","шина","тренироваться"]},
{q:"Какой специальный символ используется при написании адреса электронной почты?",o:["@","*","#","&","$"]},
{q:"Программа, позволяющая напрямую взаимодействовать с другими компьютерами, подключенными к Интернету?",o:["Чат","UseNet","электронная почта","Суслик","телнет"]},
{q:"Для просмотра других компьютерных ресурсов в локальной сети любой пользователь...",o:["Необходимо предоставить возможность перераспределять ресурсы.","Доступ к ресурсу должен быть зашифрован.","Для использования ресурса без изменений необходимо получить разрешение.","Ресурсу необходимо предоставить глобальный доступ.","Возможность изменять ресурсы предоставляться не должна."]},
{q:"Зачем нужна электронная почта?",o:["для обмена текстовой информацией между различными компьютерными системами","Чтобы просмотреть страницу www","поиск информации на сервере","доступ в Интернет","Интернет-провайдер"]},
{q:"ВВС",o:["Всемирная паутина","ФТП","TCP/IP","Электронная почта","Windows Всемирная паутина"]},
{q:"Длина IP-адреса составляет -",o:["Состоит из 32 бит, разделенных на 2 или 3 части.","Состоит из 38 бит, разделенных на 2 или 3 части.","Состоит из 64 бит, разделенных на 2 или 3 части.","Состоит из 28 бит, разделенных на 2 или 3 части.","Состоит из 42 бит, разделенных на 2 или 3 части."]},
{q:"Отправка сообщений по электронной почте...",o:["Электронная почта","www","Интернет","электронная почта","Email.www"]}
];

const L=['А','Б','С','Д','Е'];
const TOTAL=20, TL=40*60;
let qs=[],ans=[],cur=0,tid=null,tl=TL,done=false;

function shuffle(a){return a.slice().sort(()=>Math.random()-0.5)}

function startQuiz(){
  const pool=shuffle(ALL_Q).slice(0,TOTAL);
  qs=pool.map(q=>{
    const sh=shuffle(q.o.map((_,i)=>i));
    return{q:q.q,o:sh.map(i=>q.o[i]),a:sh.indexOf(0)};
  });
  ans=Array(TOTAL).fill(null);
  cur=0;tl=TL;done=false;
  show('quiz-screen');
  render();
  startTimer();
}

function startTimer(){
  clearInterval(tid);
  tid=setInterval(()=>{
    tl--;
    const m=Math.floor(tl/60),s=tl%60;
    const el=document.getElementById('tval');
    const box=document.getElementById('timer');
    if(el) el.textContent=String(m).padStart(2,'0')+':'+String(s).padStart(2,'0');
    if(box){
      box.className='timer';
      if(tl<=300) box.classList.add('danger');
      else if(tl<=600) box.classList.add('warn');
    }
    if(tl<=0){clearInterval(tid);document.getElementById('tovl').classList.add('show');}
  },1000);
}

function render(){
  const q=qs[cur], ua=ans[cur];
  document.getElementById('qwrap').innerHTML=
    '<div class="qcard">'+
    '<div class="qlabel">Вопрос '+(cur+1)+' из '+TOTAL+'</div>'+
    '<div class="qtext">'+q.q+'</div>'+
    '<div class="opts">'+
    q.o.map((opt,oi)=>
      '<div class="opt'+(ua===oi?' sel':'')+'" onclick="pick('+oi+')">'+
      '<span class="ol">'+L[oi]+'</span>'+
      '<span class="ot">'+opt+'</span></div>'
    ).join('')+
    '</div></div>';

  document.getElementById('qbody').scrollTop=0;
  document.getElementById('qcnt').textContent=(cur+1)+' / '+TOTAL;
  document.getElementById('pfill').style.width=Math.round((cur+1)/TOTAL*100)+'%';
  document.getElementById('bbk').style.visibility=cur===0?'hidden':'visible';
  const nb=document.getElementById('bnxt');
  if(cur===TOTAL-1){nb.textContent='Завершить ✓';nb.className='btn-nxt finish';}
  else{nb.textContent='Следующий →';nb.className='btn-nxt';}
}

function pick(oi){
  if(done)return;
  ans[cur]=oi;
  document.querySelectorAll('.opt').forEach((el,i)=>{
    el.className='opt'+(i===oi?' sel':'');
  });
}

function goNext(){
  if(cur<TOTAL-1){cur++;render();return;}
  // last question
  const answered=ans.filter(a=>a!==null).length;
  if(answered<TOTAL){
    document.getElementById('modal-msg').textContent=
      'Вы ответили на '+answered+' из '+TOTAL+' вопросов. Всё равно завершить?';
    document.getElementById('modal').classList.add('show');
  }else{
    finish();
  }
}

function confirmFinish(){
  closeModal();
  finish();
}
function closeModal(){
  document.getElementById('modal').classList.remove('show');
}

function goPrev(){
  if(cur>0){cur--;render();}
}

function dismissTimeout(){
  document.getElementById('tovl').classList.remove('show');
  finish();
}

function finish(){
  done=true;
  clearInterval(tid);
  const score=ans.filter((a,i)=>a===qs[i].a).length;
  const skip=ans.filter(a=>a===null).length;
  const wrong=TOTAL-score-skip;
  const pct=Math.round(score/TOTAL*100);
  const used=TL-tl, mn=Math.floor(used/60), sc=used%60;
  const g=pct>=90?'Отлично!':pct>=70?'Хорошо!':pct>=50?'Удовлетворительно':'Нужно повторить';
  const gc=pct>=90?'excellent':pct>=70?'good':pct>=50?'ok':'fail';
  document.getElementById('rgrade').textContent=g;
  document.getElementById('rgrade').className='rgrade '+gc;
  document.getElementById('rsub').textContent=score+' / '+TOTAL+' правильных — '+pct+'%';
  document.getElementById('sgrid').innerHTML=
    '<div class="sc"><div class="sv g">'+score+'</div><div class="sl">Правильно</div></div>'+
    '<div class="sc"><div class="sv r">'+wrong+'</div><div class="sl">Неверно</div></div>'+
    '<div class="sc"><div class="sv m">'+skip+'</div><div class="sl">Пропущено</div></div>'+
    '<div class="sc"><div class="sv b">'+mn+'м '+String(sc).padStart(2,'0')+'с</div><div class="sl">Затрачено</div></div>';
  document.getElementById('rlist').innerHTML=qs.map((q,i)=>{
    const ua=ans[i],ca=q.a;
    const ok=ua===ca,sk=ua===null;
    const cc=sk?'skip':ok?'ok':'err';
    let ah=sk
      ?'<div class="rva ask"><span class="rtag">ОТВЕТ</span><span>'+q.o[ca]+'</span></div>'
      :ok
      ?'<div class="rva aok"><span class="rtag">✓</span><span>'+q.o[ca]+'</span></div>'
      :'<div class="rva aerr"><span class="rtag">✗ ВЫ</span><span>'+q.o[ua]+'</span></div>'
       +'<div class="rva aok"><span class="rtag">✓ ВЕРНО</span><span>'+q.o[ca]+'</span></div>';
    return'<div class="rvc '+cc+'"><div class="rvq">'+(i+1)+'. '+q.q+'</div>'+ah+'</div>';
  }).join('');
  show('result-screen');
}

function newQuiz(){show('start-screen');}
function scrollToReview(){document.getElementById('rsep').scrollIntoView({behavior:'smooth'});}
function show(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}
</script>
</body>
</html>
