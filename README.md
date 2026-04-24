<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#1a0533">
<title>Тест: HTML и компьютерные сети</title>
<style>
:root{
  --bg:#1a0533;
  --surface:#2d0f52;
  --surface2:#3d1668;
  --border:#5a2d8a;
  --accent:#c084fc;
  --accent2:#a855f7;
  --accentd:#7c3aed;
  --correct:#34d399;
  --wrong:#f87171;
  --warn:#fbbf24;
  --text:#f3e8ff;
  --muted:#a78bca;
  --radius:14px;
  --font:-apple-system,"SF Pro Display","Helvetica Neue",Arial,sans-serif;
  --safe-bottom:env(safe-area-inset-bottom,0px);
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
html{scroll-behavior:smooth;-webkit-text-size-adjust:100%}
body{background:var(--bg);color:var(--text);font-family:var(--font);min-height:100vh;overflow-x:hidden}

/* grid bg */
body::before{content:"";position:fixed;inset:0;
  background:radial-gradient(ellipse at 20% 20%,rgba(168,85,247,0.15) 0%,transparent 60%),
             radial-gradient(ellipse at 80% 80%,rgba(192,132,252,0.1) 0%,transparent 60%);
  pointer-events:none;z-index:0}

.screen{display:none;position:relative;z-index:1}
.screen.active{display:block}

/* ── START SCREEN ── */
#start-screen{
  min-height:100vh;display:flex;flex-direction:column;
  align-items:center;justify-content:center;
  padding:2rem 1.25rem;text-align:center
}
.badge{
  display:inline-flex;align-items:center;gap:8px;
  background:rgba(192,132,252,0.12);border:1px solid rgba(192,132,252,0.35);
  border-radius:100px;padding:6px 18px;margin-bottom:1.75rem;
  font-size:11px;color:var(--accent);letter-spacing:2px;text-transform:uppercase;font-weight:600
}
.badge-dot{width:7px;height:7px;border-radius:50%;background:var(--accent);animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:0.4;transform:scale(0.75)}}
.start-title{
  font-size:clamp(24px,8vw,48px);font-weight:800;
  line-height:1.1;margin-bottom:0.75rem;
  background:linear-gradient(135deg,#e9d5ff,#c084fc,#a855f7);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text
}
.start-sub{font-size:14px;color:var(--muted);margin-bottom:2.5rem;line-height:1.7}
.info-grid{
  display:grid;grid-template-columns:repeat(2,1fr);
  gap:10px;width:100%;max-width:340px;margin:0 auto 2.5rem
}
.info-card{
  background:rgba(255,255,255,0.05);border:1px solid rgba(192,132,252,0.2);
  border-radius:var(--radius);padding:1rem 0.75rem;text-align:center
}
.info-icon{font-size:24px;margin-bottom:6px}
.info-val{font-size:20px;font-weight:700;color:var(--accent);margin-bottom:3px}
.info-lbl{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;font-weight:600}
.btn-primary{
  display:inline-flex;align-items:center;justify-content:center;gap:10px;
  background:linear-gradient(135deg,#a855f7,#7c3aed);
  color:#fff;font-size:15px;font-weight:700;
  border:none;border-radius:100px;padding:16px 48px;cursor:pointer;
  letter-spacing:0.5px;min-height:54px;width:100%;max-width:300px;
  box-shadow:0 8px 32px rgba(168,85,247,0.4);touch-action:manipulation
}
.btn-primary:active{transform:scale(0.97);opacity:0.9}

/* ── QUIZ SCREEN ── */
#quiz-screen{max-width:640px;margin:0 auto;padding:0;}

/* Header */
.q-header{
  background:var(--surface);border-bottom:1px solid var(--border);
  padding:12px 1rem;position:sticky;top:0;z-index:50
}
.q-header-top{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px}
.q-title{font-size:13px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:1px}
.timer-pill{
  display:flex;align-items:center;gap:5px;
  background:rgba(255,255,255,0.07);border:1px solid var(--border);
  border-radius:100px;padding:5px 14px;
}
.timer-pill.warn{border-color:var(--warn);background:rgba(251,191,36,0.1)}
.timer-pill.danger{border-color:var(--wrong);background:rgba(248,113,113,0.15);animation:blink 1s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:0.5}}
.timer-val{font-size:15px;font-weight:700;letter-spacing:2px;min-width:50px;text-align:center}
.timer-pill.warn .timer-val{color:var(--warn)}
.timer-pill.danger .timer-val{color:var(--wrong)}
.q-progress-row{display:flex;align-items:center;gap:10px}
.q-counter{font-size:12px;font-weight:600;color:var(--accent);white-space:nowrap}
.prog-bar{flex:1;height:5px;background:rgba(255,255,255,0.1);border-radius:3px;overflow:hidden}
.prog-fill{height:100%;background:linear-gradient(90deg,var(--accent2),var(--accentd));border-radius:3px;transition:width 0.35s}

/* Question card */
.q-body{padding:1.25rem 1rem calc(90px + var(--safe-bottom))}
.q-card{
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--radius);padding:1.25rem;
  animation:fadeIn 0.2s ease
}
@keyframes fadeIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}
.q-label{font-size:11px;font-weight:700;color:var(--accent);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:10px}
.q-text{font-size:15px;line-height:1.65;color:var(--text);margin-bottom:1.25rem;font-weight:500}
.opts{display:flex;flex-direction:column;gap:9px}
.opt{
  display:flex;align-items:flex-start;gap:12px;
  padding:13px 14px;
  background:rgba(255,255,255,0.04);border:1.5px solid rgba(255,255,255,0.1);
  border-radius:12px;cursor:pointer;touch-action:manipulation;
  -webkit-user-select:none;user-select:none;min-height:50px;
  transition:border-color 0.15s,background 0.15s
}
.opt:active{background:rgba(168,85,247,0.1);border-color:rgba(192,132,252,0.5)}
.opt.selected{border-color:var(--accent);background:rgba(168,85,247,0.12)}
.opt.correct{border-color:var(--correct);background:rgba(52,211,153,0.1)}
.opt.wrong{border-color:var(--wrong);background:rgba(248,113,113,0.1)}
.opt-letter{
  font-size:11px;font-weight:800;min-width:26px;height:26px;
  border-radius:7px;display:flex;align-items:center;justify-content:center;
  background:rgba(255,255,255,0.08);color:var(--muted);flex-shrink:0;margin-top:1px
}
.opt.selected .opt-letter{background:var(--accent2);color:#fff}
.opt.correct .opt-letter{background:var(--correct);color:#052e16}
.opt.wrong .opt-letter{background:var(--wrong);color:#fff}
.opt-text{font-size:14px;color:var(--text);line-height:1.5;padding-top:3px;word-break:break-word}

/* Bottom nav */
.q-nav{
  position:fixed;bottom:0;left:0;right:0;z-index:60;
  background:var(--surface);border-top:1px solid var(--border);
  padding:12px 1rem;padding-bottom:calc(12px + var(--safe-bottom))
}
.q-nav-inner{max-width:640px;margin:0 auto;display:flex;align-items:center;gap:10px}
.btn-skip{
  flex:1;padding:13px;border:1.5px solid var(--border);border-radius:100px;
  background:transparent;color:var(--muted);font-size:13px;font-weight:600;
  cursor:pointer;touch-action:manipulation;min-height:48px
}
.btn-skip:active{background:rgba(255,255,255,0.05)}
.btn-next{
  flex:2;padding:13px;
  background:linear-gradient(135deg,#a855f7,#7c3aed);
  color:#fff;font-size:14px;font-weight:700;
  border:none;border-radius:100px;cursor:pointer;touch-action:manipulation;min-height:48px;
  box-shadow:0 4px 20px rgba(168,85,247,0.35)
}
.btn-next:active{transform:scale(0.97);opacity:0.9}
.btn-next.last{background:linear-gradient(135deg,#34d399,#059669)}
.btn-next:disabled{opacity:0.4;cursor:default}

/* ── RESULT SCREEN ── */
#result-screen{
  min-height:100vh;padding:2rem 1rem 3rem;
  display:flex;flex-direction:column;align-items:center
}
.result-wrap{max-width:600px;width:100%}
.result-header{text-align:center;margin-bottom:2rem;padding-top:1rem}
.result-grade{
  font-size:clamp(32px,10vw,60px);font-weight:800;margin-bottom:0.5rem;
  line-height:1
}
.result-grade.excellent{background:linear-gradient(135deg,#34d399,#059669);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.result-grade.good{background:linear-gradient(135deg,#c084fc,#7c3aed);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text}
.result-grade.ok{color:var(--warn)}
.result-grade.fail{color:var(--wrong)}
.result-sub{font-size:13px;color:var(--muted);font-weight:600;letter-spacing:1px;text-transform:uppercase}
.stats-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-bottom:1.75rem}
.stat-card{
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--radius);padding:1rem;text-align:center
}
.stat-val{font-size:28px;font-weight:800;margin-bottom:3px}
.stat-val.g{color:var(--correct)}
.stat-val.r{color:var(--wrong)}
.stat-val.b{color:var(--accent)}
.stat-val.m{color:var(--muted)}
.stat-lbl{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:1px;font-weight:600}
.result-btns{display:flex;flex-direction:column;gap:10px;margin-bottom:2rem}
.btn-restart{
  display:flex;align-items:center;justify-content:center;gap:8px;
  background:linear-gradient(135deg,#a855f7,#7c3aed);color:#fff;
  font-size:14px;font-weight:700;border:none;border-radius:100px;
  padding:16px;cursor:pointer;touch-action:manipulation;min-height:52px;
  box-shadow:0 4px 20px rgba(168,85,247,0.35)
}
.btn-restart:active{transform:scale(0.97)}
.btn-review{
  display:flex;align-items:center;justify-content:center;
  background:transparent;color:var(--accent);
  font-size:13px;font-weight:600;border:1.5px solid var(--border);
  border-radius:100px;padding:14px;cursor:pointer;touch-action:manipulation;min-height:48px
}
.btn-review:active{background:rgba(255,255,255,0.05)}
.review-sep{
  display:flex;align-items:center;gap:10px;
  font-size:11px;font-weight:700;color:var(--muted);
  text-transform:uppercase;letter-spacing:2px;margin-bottom:1rem
}
.review-sep::after{content:"";flex:1;height:1px;background:var(--border)}
.rv-card{
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--radius);padding:1rem;margin-bottom:8px;word-break:break-word
}
.rv-card.ok{border-left:3px solid var(--correct)}
.rv-card.err{border-left:3px solid var(--wrong)}
.rv-card.skip{border-left:3px solid var(--muted)}
.rv-q{font-size:13px;color:var(--text);margin-bottom:8px;line-height:1.6;font-weight:500}
.rv-ans{display:flex;align-items:flex-start;gap:7px;font-size:12px;padding:6px 10px;border-radius:8px;margin-top:4px;line-height:1.5}
.rv-ans.ra-ok{background:rgba(52,211,153,0.1);color:var(--correct)}
.rv-ans.ra-err{background:rgba(248,113,113,0.1);color:var(--wrong)}
.rv-ans.ra-skip{background:rgba(255,255,255,0.05);color:var(--muted)}
.rv-tag{font-size:9px;font-weight:800;padding:2px 6px;border-radius:4px;flex-shrink:0;margin-top:2px;white-space:nowrap}
.ra-ok .rv-tag{background:var(--correct);color:#052e16}
.ra-err .rv-tag{background:var(--wrong);color:#fff}
.ra-skip .rv-tag{background:var(--muted);color:#fff}

/* TIMEOUT */
#timeout-overlay{
  display:none;position:fixed;inset:0;z-index:9999;
  background:rgba(26,5,51,0.97);
  align-items:center;justify-content:center;
  flex-direction:column;text-align:center;padding:2rem
}
#timeout-overlay.show{display:flex}
.to-icon{font-size:64px;margin-bottom:1rem;animation:shake 0.5s 0.1s}
@keyframes shake{0%,100%{transform:translateX(0)}25%{transform:translateX(-10px)}75%{transform:translateX(10px)}}
.to-title{font-size:clamp(24px,7vw,44px);font-weight:800;color:var(--wrong);margin-bottom:0.5rem}
.to-sub{font-size:13px;color:var(--muted);margin-bottom:2rem;line-height:1.6}

::-webkit-scrollbar{width:4px}
::-webkit-scrollbar-track{background:var(--bg)}
::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}
</style>
</head>
<body>

<!-- START -->
<div id="start-screen" class="screen active">
  <div class="badge"><span class="badge-dot"></span>Экзаменационный тест</div>
  <h1 class="start-title">HTML и компьютерные сети</h1>
  <p class="start-sub">20 случайных вопросов из базы 97 уникальных</p>
  <div class="info-grid">
    <div class="info-card"><div class="info-icon">📋</div><div class="info-val">20</div><div class="info-lbl">Вопросов</div></div>
    <div class="info-card"><div class="info-icon">⏱</div><div class="info-val">40:00</div><div class="info-lbl">Время</div></div>
    <div class="info-card"><div class="info-icon">🎯</div><div class="info-val">5</div><div class="info-lbl">Вариантов</div></div>
    <div class="info-card"><div class="info-icon">🔀</div><div class="info-val">∞</div><div class="info-lbl">Перемешиваний</div></div>
  </div>
  <button class="btn-primary" onclick="startQuiz()">▶ Начать тест</button>
</div>

<!-- QUIZ -->
<div id="quiz-screen" class="screen">
  <div class="q-header">
    <div class="q-header-top">
      <span class="q-title">Тест</span>
      <div class="timer-pill" id="timer-box">
        <span>⏱</span>
        <span class="timer-val" id="timer-display">40:00</span>
      </div>
    </div>
    <div class="q-progress-row">
      <span class="q-counter" id="q-counter">1 / 20</span>
      <div class="prog-bar"><div class="prog-fill" id="prog-fill" style="width:5%"></div></div>
    </div>
  </div>
  <div class="q-body">
    <div id="q-card-wrap"></div>
  </div>
  <div class="q-nav">
    <div class="q-nav-inner">
      <button class="btn-skip" id="btn-prev" onclick="goPrev()">← Назад</button>
      <button class="btn-next" id="btn-next" onclick="goNext()">Следующий →</button>
    </div>
  </div>
</div>

<!-- RESULT -->
<div id="result-screen" class="screen">
  <div class="result-wrap">
    <div class="result-header">
      <div class="result-grade" id="result-grade"></div>
      <div class="result-sub" id="result-pct"></div>
    </div>
    <div class="stats-grid" id="stats-grid"></div>
    <div class="result-btns">
      <button class="btn-restart" onclick="newQuiz()">🔀 Новый тест</button>
      <button class="btn-review" onclick="scrollToReview()">📋 Разбор ошибок</button>
    </div>
    <div class="review-sep">Разбор ответов</div>
    <div id="review-container"></div>
    <div style="height:2rem"></div>
  </div>
</div>

<!-- TIMEOUT -->
<div id="timeout-overlay">
  <div class="to-icon">⏰</div>
  <div class="to-title">Время вышло!</div>
  <div class="to-sub">40 минут истекли.<br>Результат посчитан автоматически.</div>
  <button class="btn-primary" onclick="dismissTimeout()">Посмотреть результат</button>
</div>

<script>
const ALL_Q=[
{q:"Как называется сетевая система, которая позволяет любому компьютеру обмениваться и передавать информацию по всему миру?",o:["Интернет","Сеть","Сервер","Протокол","Мессенджер"]},
{q:"Как называется особый способ соединения компьютеров друг с другом в локальной сети?",o:["Клиент-сервер","Сервер","Интернет-сеть","Протокол","Платеж"]},
{q:"протокол передачи файлов",o:["ФТП",".PTP","LDAP","TCP/IP","VPN"]},
{q:"Какие тексты можно читать на компьютере или другом электронном устройстве?",o:["Гипертекст","Гипермедиа","Гиперссылка","Тексты","Файл"]},
{q:"Ученый, изобретший HTML",o:["Тим Бернерс-Ли","Андрис ван Дам","Тед Нельсон","Тим –Бриттани-Ли","Марко ван Бастен"]},
{q:"Ниже перечислены нестандартные теги:",o:["&lt;hr&gt;, &lt;img&gt;","&lt;html&gt;&lt;/html&gt;,","&lt;hl&gt;&lt;/hl&gt;","&lt;p&gt;&lt;/p&gt;","&lt;https&gt;&lt;/https&gt;"]},
{q:"Какой тег используется для создания заголовка строки заголовка?",o:["&lt;title&gt;&lt;/title&gt;","&lt;body&gt;&lt;/body&gt;","&lt;html&gt;&lt;/html&gt;","&lt;big&gt; &lt;/big&gt;","&lt;n&gt;&lt;/n&gt;"]},
{q:"Какой тег используется для выделения текста жирным шрифтом?",o:["&lt;B&gt;","&lt;I&gt;","&lt;U&gt;","&lt;P&gt;","&lt;S&gt;"]},
{q:"Какой термин ввёл в обиход Тед Нельсон в 1963 году?",o:["Гипертекст","Дескриптор","Ярлык","HTML","Сервер"]},
{q:"Основной текст располагается в теле документа между тегами ........... Замените многоточие соответствующим словом.",o:["&lt;head&gt;&lt;/head&gt;","&lt;html&gt;&lt;/html&gt;","&lt;title&gt;&lt;/title&gt;","&lt;body&gt;&lt;/body&gt;","&lt;air&gt; &lt;/air&gt;"]},
{q:"Какова функция тега &lt;BR&gt;?",o:["Оставьте пустую строку","Кнопка","Выделить текст","Создание рамки","тимбилдинг"]},
{q:"Какой тег используется для установки размера шрифта, превышающего базовый размер шрифта?",o:["&lt;БОЛЬШОЙ&gt;","&lt;BR&gt;","&lt;Подпись&gt;","&lt;HR&gt;","&lt;FR&gt;"]},
{q:"Укажите тег, используемый для разделения текста на абзацы.",o:["&lt;P&gt;&lt;/P&gt;","&lt;H1&gt;","&lt;BR&gt;","&lt;U&gt;","&lt;R&gt;"]},
{q:"Что реализует список в &lt;UL&gt;?",o:["Маркированный список","Пронумерованный список","Список определений","Правильный ответ не показан.","Запланированный список"]},
{q:"Web – Название тега, используемого для создания таблицы в документе.",o:["&lt;таблица&gt;","&lt;Подпись&gt;","&lt;РАМКА&gt;","&lt;ШРИФТ&gt;","&lt;правая&gt;"]},
{q:"Отображение HTML-документов одновременно на странице браузера.",o:["Рамки","Формы","веб-страница","Списки","Серверы"]},
{q:"Укажите атрибут, определяющий количество вертикальных кадров.",o:["столбцы","ряды","цвет","выровнять","основной"]},
{q:"Какова функция атрибута Rows?",o:["Возвращает количество горизонтальных кадров.","Возвращает количество вертикальных кадров.","Возвращает количество горизонтальных фигур","Указывает количество вертикальных фигур","Восстанавливает формы"]},
{q:"Как ещё можно назвать этот тег?",o:["Дескриптор","Код","Веб-тег","Дизайнер","Верхний/нижний колонтитул"]},
{q:"Покажите синтаксис для записи поля пароля.",o:["&lt;input type=\"text\" name \"password\"/&gt;","&lt;input type=\"text\" name \"firstname\"/&gt;","&lt;input type=\"text\" name \"user\"/&gt;","&lt;input type=\"text\" name \"lastname\"/&gt;","&lt;input type=\"text\" name \"person\"/&gt;"]},
{q:"Что такое TCP/IP?",o:["Протокол, контролирующий прием/передачу информации.","программа сортировки информации","сеть хранения информации","Интернет-адрес","Антивирус"]},
{q:"Из скольких частей (чисел) состоит IP-адрес?",o:["4 части (числа), разделённые точкой.","5 чисел, разделённых дефисом.","Из 10 чисел в последовательности","2 цифры","Из 4 сложенных цифр"]},
{q:"Что такое браузер?",o:["Программа на компьютере","Антивирус","Уборщик","Интернет","БАРАН"]},
{q:"Основная функция браузерной программы:",o:["Откройте веб-страницу по введенному адресу.","Управление компьютером","Изобретение новых идей","Скрыть IP-адрес","Написание веб-ссылок"]},
{q:"Какой код используется для отправки электронных писем пользователю в компьютерной сети?",o:["Двоичный","Шестнадцатеричная система счисления","Графический подход","Цифровой","Символический"]},
{q:"Символ, разделяющий две части адреса электронной почты.",o:["@","!","*","Нет.",":"]},
{q:"Из чего состоит адрес почтового сервера?",o:["Из доменов","Из чисел","Из символов","Из писем","Из ничего"]},
{q:"Какой символ разделяет части адреса почтового сервера?",o:[":","*","!","@","Нет."]},
{q:"Что такое FTP?",o:["протокол передачи файлов","программа защиты файлов","Как создать программу","Программа вывода гипертекста","Программа для чтения гипертекста"]},
{q:"Популярные российские поисковые системы:",o:["Рамблер, Яндекс, Апорт","Google, Firefox","Microsoft Edge, Google, Яндекс","Бинг, Яху!","Яндекс, Google, Нигма"]},
{q:"Что такое веб-страница?",o:["Электронный документ в системе WWW","Веб-сайт написан на языке HTTPS.","Документ, расположенный в гипертекстовой системе.","Документ написан в формате HTML.","Документ хранится на жестком диске."]},
{q:"Интернет — это...",o:["крупномасштабная всемирная сеть","локальная сеть","региональная информационно-компьютерная сеть","локальная информационная сеть","корпоративная сеть"]},
{q:"В каком году появился интернет?",o:["1983","1993","1985","1982","1987"]},
{q:"Основные ячейки интернет-сети:",o:["локальная сеть","хост-компьютеры","Волоконно-оптические кабели с очень высокой проводимостью","телефонные линии","телефонные линии"]},
{q:"Стандартный интерфейс CGI выглядит следующим образом:",o:["Реализует серверные приложения, вызываемые по URL-адресу.","Волоконно-оптические кабели с очень высокой проводимостью","состоит из типов компьютеров, каналов связи и механических механизмов.","Состоит из типов компьютеров, их устройств и панелей управления.","Состоит из различных типов компьютеров, сетевых информационных компонентов и электрооборудования."]},
{q:"Протоколы, не являющиеся сетевыми протоколами:",o:["HTML","TCP/IP","СОСКАЛЬЗЫВАТЬ","ФТП","ППП"]},
{q:"Сколько слоев имеет модель OSI?",o:["Уровень 7","Уровень 8","Уровень 9","Уровень 6","Уровень 5"]},
{q:"Пятый слой модели OSI называется...",o:["сессионный","применяемый","транспорт","онлайн","канал"]},
{q:"Первый слой модели OSI называется...",o:["физический","сессионный","канал","транспорт","онлайн"]},
{q:"Общий объем информации, передаваемой через один сетевой компьютер в интернете, называется...",o:["трафик","расписание","транзит","фонд","количество"]},
{q:"Мы называем это налаживанием связей...",o:["Три слоя модели OSI","Слой 2 модели OSI","4 слоя модели OSI","6 слоев модели OSI","5 слоев модели OSI"]},
{q:"Мы говорим о практичности...",o:["7 слоев модели OSI","4 слоя модели OSI","6 слоев модели OSI","5 слоев модели OSI","Слой 0 модели OSI"]},
{q:"Интернет-провайдер",o:["поставщик","поставщик","исполнитель","дилер","продавец"]},
{q:"Коммутатор компьютера в сети называется...",o:["Центр","Хаб","Хозяин","ICQ","ВАН"]},
{q:"Единица измерения скорости передачи данных через модем:",o:["тело","вши","фасоль","баннер","Бонар"]},
{q:"Устройство для подключения компьютеров к сети через телефонный канал называется...",o:["модем","сканер","порт","слот","сервер"]},
{q:"Язык разметки гипертекста называется...",o:["HTML","JavaScript","Турбо Паскаль","Дельфы","C++"]},
{q:"Что такое веб-страница (HTML-документ)?",o:["текстовый файл типа htm или htm","графический файл типа gif или jpg","текстовый файл типа txt или doc","Двоичный файл типа Com или exe","текстовый файл типа g или txt"]},
{q:"Укажите протокол, реализующий адресацию пакетов в сети...",o:["IP","TCP","DNS","URL","НКП/ ИП"]},
{q:"Какова функция протокола TCP?",o:["выполняет задачу отправки данных.","объединяет информацию в единую систему","разделяет данные на несколько частей","собирает данные","обрабатывает и организует данные"]},
{q:"Информационная составляющая сети…",o:["состоит из типов компьютеров, их устройств и электрических механизмов.","Состоит из типов компьютеров, их устройств, каналов связи и механизмов связи.","состоит из типов компьютеров, каналов связи и механических механизмов.","Состоит из типов компьютеров, их устройств и панелей управления.","Состоит из различных типов компьютеров, сетевых информационных компонентов и электрооборудования."]},
{q:"Какой из телефонных каналов удобнее для связи с поставщиком услуг?",o:["цифровой телефонный канал","Телефонный канал с дополнительным устройством AVU","аналоговый телефонный канал","телефонный канал с блокировщиком","радиотелефонный канал"]},
{q:"С какого символа начинаются и заканчиваются все теги?",o:["Все теги начинаются с символа «меньше» (&lt;) и заканчиваются символом «больше» (&gt;).","Все теги начинаются с символа «открывающей кавычки» (\") и заканчиваются символом «закрывающей кавычки» (\").","Все теги начинаются со знака плюс (+) и заканчиваются знаком минус (-).","все тег \"звезда\" (*) символ начинается, и \"точно по делу\" с символом (.) конец","Все теги начинаются с восклицательного знака (!) и заканчиваются вопросительным знаком (?)."]},
{q:"Какой атрибут достаточно использовать для тега &lt;FONT&gt;, чтобы изменить цвет шрифта?",o:["Чтобы изменить цвет шрифта, просто используйте атрибут COLOR-=\"X\" в теге &lt;FONT&gt;.","Чтобы изменить цвет шрифта, просто используйте атрибут COLOR-=\"V\" в теге &lt;TXT&gt;.","Чтобы изменить цвет шрифта, просто добавьте атрибут COLOR-=\"C\" к тегу &lt;BAS&gt;.","Чтобы изменить цвет шрифта, просто используйте атрибут COLOR-=\"E\" в теге &lt;DOC&gt;.","Чтобы изменить цвет шрифта, просто добавьте атрибут COLOR-=\"F\" к тегу &lt;COM&gt;."]},
{q:"Элемент, образующий строку таблицы:",o:["ТР","HR","ТД","БР","ТХ"]},
{q:"Элемент ячейки, представляющий собой имя столбца или строки в таблице:",o:["ТХ","ТД","ТР","HR","БР"]},
{q:"Какой тег используется для представления обычных ячеек:",o:["ТД","ТХ","HR","БР","ТР"]},
{q:"Какой атрибут следует использовать для объединения ячеек столбца:",o:["колспан","расстояние между клетками","строка span","клеточная закладка","правила"]},
{q:"Какой атрибут следует использовать для объединения ячеек строки:",o:["строка span","расстояние между клетками","колспан","правила","клеточная закладка"]},
{q:"Какой атрибут используется для указания количества пустого пространства в ячейках, окружающих данные?",o:["клеточная закладка","расстояние между клетками","фон","колспан","выше"]},
{q:"Какой атрибут типа double используется для изменения цвета заданного текста?",o:["ЦВЕТ ШРИФТА","BGCOLOR","ЦВЕТ ТЕКСТА","ШРИФТ","РАЗМЕР ШРИФТА"]},
{q:"Какие значения принимает атрибут ALIGN?",o:["левый, центр, правый, обосновать","ширина, высота, свет, выравнивание","свет, центр, справа, обосновать","верх, середина, выравнивание, низ","левый, средний, обосновать, правый"]},
{q:"Какой атрибут используется для вставки изображения в документ?",o:["IMG","КАК ДЕЛА","ВСТРОЕННЫЙ","ФОН","ПОД"]},
{q:"Какой атрибут определяет структуру фреймов?",o:["НАБОР РАМОК","IFRAME","РАМКА","NOFRAMES","ВСТРОЕННЫЙ"]},
{q:"Какой атрибут используется для установки информации во фреймах:",o:["РАМКА","IFRAME","НАБОР РАМОК","ВСТРОЕННЫЙ","NOFRAMES"]},
{q:"Какой атрибут мы используем для работы с музыкальными файлами:",o:["ВСТРОЕННЫЙ","IMG","СКР","КАК ДЕЛА","ПОД"]},
{q:"Атрибут, указывающий на файл, который необходимо загрузить:",o:["СКР","КАК ДЕЛА","IMG","ПОД","ВСТРОЕННЫЙ"]},
{q:"Элемент, из которого состоит направляющая:",o:["МАРКЕТ","ВСТРОЕННЫЙ","ВЫСОТА МАРГИНХАЙТ","НАБОР РАМОК","IMG"]},
{q:"Атрибут, указывающий, сколько раз ползунок проходит по экрану:",o:["Петля","Поведение","Scrollamount","Направление","Scrolldelay"]},
{q:"Какой атрибут используется для обозначения скорости движения:",o:["Scrollamount","Направление","Scrolldelay","Петля","Поведение"]},
{q:"Атрибут, управляющий полосой прокрутки в пределах определенной области:",o:["Прокрутка","Scrollamount","Поведение","Направление","Scrolldelay"]},
{q:"Параметры атрибута SCROLING:",o:["да, нет, авто","слева, в центре, справа","левый, средний, правый","верх, центр, низ","выравнивание, верх, низ"]},
{q:"Устройство, организующее диалог с пользователем на веб-странице:",o:["форма","рамка","скользящая направляющая","стол","связь"]},
{q:"С какого двойного тега начинается шаблон?",o:["Форма и /Форма","Frame и /Frame","iframe и /Iframe","Frameset и /Framese","Таблица и /Таблица"]},
{q:"Какой атрибут используется для указания шрифта?",o:["ШРИФТ","ЦВЕТ ТЕКСТА","ЦВЕТ ШРИФТА","РАЗМЕР ШРИФТА","BGCOLOR"]},
{q:"Команда для создания элемента управления выглядит следующим образом:",o:["ВХОД","ЦЕНИТЬ","ТИП","КНОПКА","ИМЯ"]},
{q:"Параметр, определяющий элемент управления:",o:["ТИП","ВХОД","ЦЕНИТЬ","КНОПКА","ИМЯ"]},
{q:"Параметр, определяющий текст, отображаемый в элементе управления:",o:["ЦЕНИТЬ","ИМЯ","ТИП","ВХОД","КНОПКА"]},
{q:"Окно для ввода текста в одну строку:",o:["TYPE=text","TYPE=radio","TYPE=checkbox","TYPE=hidden","TYPE=password"]},
{q:"Окно для ввода ключа:",o:["TYPE=password","TYPE=checkbox","TYPE=text","TYPE=radio","TYPE=hidden"]},
{q:"Текстовое окно:",o:["Текстовое поле","Вариант","Ценить","Скрытый","Выбирать"]},
{q:"Переключатель:",o:["TYPE=radio","TYPE=checkbox","TYPE=hidden","TYPE=reset","TYPE=text"]},
{q:"Отмеченный прямоугольник:",o:["TYPE=checkbox","TYPE=password","TYPE=text","TYPE=hidden","TYPE=radio"]},
{q:"Удалить кнопку:",o:["TYPE=reset","TYPE=radio","TYPE=password","TYPE=text","TYPE=hidden"]},
{q:"Для создания кнопки используется следующий элемент:",o:["КНОПКА","ВАРИАНТ","ВЫБИРАТЬ","ВХОД","Текстовая область"]},
{q:"Атрибут, определяющий размер и состав кадров на экране:",o:["НАБОР РАМОК","IFRAME","РАМКА","ФОРМА","NOFRAMES"]},
{q:"Атрибут, определяющий HTML-файл для каждого фрейма, выглядит следующим образом:",o:["РАМКА","НАБОР РАМОК","IFRAME","NOFRAMES","ФОРМА"]},
{q:"Атрибут, не позволяющий перемещать границы фреймов с помощью мыши:",o:["Изменение размера недоступно","Ширина поля","Без рамок","Высота поля","Прокрутка"]},
{q:"Что такое знак?",o:["Указывает точку в этом документе, к которой следует перейти по ссылке.","Указывает на наличие одной запятой в данном документе, которая чередуется с другой буквой.","Указывает место в этом документе, которое перемещается по номеру.","Указывает один цвет в этом документе, который будет чередоваться с другими цветами.","Этот документ, представляющий собой последовательность изображений, раскрывает скрытую в них красоту."]},
{q:"Объяснение форматов &lt;Р ALIGN=« left» x/ P&gt;, используемых в форматировании HTML-страниц...",o:["выравнивание автомагистралей","сглаживание прямых дорог","сглаживание гладких дорог","выравнивание обработанных дорог","сглаживание изогнутых дорог"]},
{q:"Объяснение формата &lt;FONT FACE=*&gt;&lt;/FONT&gt;, используемого в HTML...",o:["список шрифтов","список каталога","список бумаг","список дисков","список файлов"]},
{q:"Объяснение формата &lt;А HREF=\"URL\"&gt; TEKCT&lt;/А&gt; для вставки гиперссылок, используемых в HTML...",o:["ссылка на другую страницу","ссылка на другой диск","ссылка на другой файл","ссылка на эту страницу","ссылка на другой каталог"]},
{q:"В HTML используется понятие URL — унифицированный указатель ресурса...",o:["универсальный указатель ресурса (веб-адрес)","универсальный графический указатель (адрес документа в каталоге)","универсальный указатель темы (адрес документа в файловой сети)","Универсальный указатель документа (адрес документа в сети в программе)","Универсальный музыкальный локатор (адрес документа в интернете для музыкальных произведений)"]},
{q:"&lt;HR&gt;",o:["Этот символ обозначает горизонтальную линию.","Начало и конец окна браузера в HTML-разметке","Этот атрибут определяет толщину линии.","Этот символ используется для перехода на новую строку без разрыва абзаца.","Теперь это будет определяться шириной экрана."]},
{q:"Протоколы сетевого уровня...",o:["Она создается в виде программного модуля и выполняется в сети, называемой хостом, и на компьютере, называемом шлюзом, расположенном посередине.","протокол управления сообщениями","Протокол передачи дейтаграмм. Этот протокол предоставляет услуги маршрутизации.","дополнительный протокол, решающий проблему маршрутизации на коротких путях","Определяет адреса машин, используя обратный протокол ARP."]},
{q:"RARP – Протокол обратного разрешения адресов – Протокол маршрутизации...",o:["Определяет адреса машин, используя обратный протокол ARP.","Она создается в виде программного модуля и выполняется в сети, называемой хостом, и на компьютере, называемом шлюзом, расположенном посередине.","протокол управления сообщениями","Протокол передачи дейтаграмм. Этот протокол предоставляет услуги маршрутизации.","дополнительный протокол, решающий проблему маршрутизации на коротких путях"]},
{q:"NIS – Network Information Service – дополнительный протокол обслуживания...",o:["хранит информацию о клиентах в интернете.","отправляет информацию о маршрутизации во внутренние сети","Позволяет получать доступ к файлам на удаленном компьютере так, как если бы они находились на локальном компьютере.","передает информацию о маршрутизации между шлюзами.","отправляет информацию о маршрутизации во внешние сети"]}
];

const LETTERS=['А','Б','С','Д','Е'];
const TOTAL=20;
const TIME_LIMIT=40*60;
let questions=[],answers=[],cur=0,timerInterval=null,timeLeft=TIME_LIMIT,done=false;

function shuffle(a){return a.slice().sort(()=>Math.random()-0.5)}

function startQuiz(){
  const pool=shuffle(ALL_Q).slice(0,TOTAL);
  questions=pool.map(q=>{
    const sh=shuffle(q.o.map((_,i)=>i));
    return{q:q.q,o:sh.map(i=>q.o[i]),a:sh.indexOf(0)};
  });
  answers=Array(TOTAL).fill(null);
  cur=0;timeLeft=TIME_LIMIT;done=false;
  showScreen('quiz-screen');
  renderCurrent();
  startTimer();
}

function startTimer(){
  clearInterval(timerInterval);
  timerInterval=setInterval(()=>{
    timeLeft--;updateTimer();
    if(timeLeft<=0){clearInterval(timerInterval);triggerTimeout();}
  },1000);
}

function updateTimer(){
  const m=Math.floor(timeLeft/60),s=timeLeft%60;
  const el=document.getElementById('timer-display');
  const box=document.getElementById('timer-box');
  if(el)el.textContent=String(m).padStart(2,'0')+':'+String(s).padStart(2,'0');
  if(box){
    box.className='timer-pill';
    if(timeLeft<=300)box.classList.add('danger');
    else if(timeLeft<=600)box.classList.add('warn');
  }
}

function renderCurrent(){
  const q=questions[cur];
  const wrap=document.getElementById('q-card-wrap');
  const ua=answers[cur];
  wrap.innerHTML=`
    <div class="q-card">
      <div class="q-label">Вопрос ${cur+1} из ${TOTAL}</div>
      <div class="q-text">${q.q}</div>
      <div class="opts">
        ${q.o.map((opt,oi)=>{
          let cls='opt';
          if(ua===oi)cls+=' selected';
          return`<div class="${cls}" id="opt-${oi}" onclick="pick(${oi})">
            <span class="opt-letter">${LETTERS[oi]}</span>
            <span class="opt-text">${opt}</span>
          </div>`;
        }).join('')}
      </div>
    </div>`;

  // counter & progress
  const cnt=document.getElementById('q-counter');
  if(cnt)cnt.textContent=(cur+1)+' / '+TOTAL;
  const fill=document.getElementById('prog-fill');
  if(fill)fill.style.width=(Math.round((cur+1)/TOTAL*100))+'%';

  // nav buttons
  const prev=document.getElementById('btn-prev');
  const next=document.getElementById('btn-next');
  if(prev)prev.style.visibility=cur===0?'hidden':'visible';
  if(next){
    if(cur===TOTAL-1){
      next.textContent='Завершить ✓';
      next.className='btn-next last';
    }else{
      next.textContent='Следующий →';
      next.className='btn-next';
    }
  }
}

function pick(oi){
  if(done)return;
  answers[cur]=oi;
  // update visuals
  document.querySelectorAll('.opt').forEach((el,i)=>{
    el.className='opt'+(i===oi?' selected':'');
  });
}

function goNext(){
  if(cur===TOTAL-1){
    const answered=answers.filter(a=>a!==null).length;
    if(answered<TOTAL){
      if(!confirm('Вы ответили на '+answered+' из '+TOTAL+' вопросов. Завершить тест?'))return;
    }
    clearInterval(timerInterval);
    finalizeResults();
  }else{
    cur++;renderCurrent();window.scrollTo(0,0);
  }
}

function goPrev(){
  if(cur>0){cur--;renderCurrent();window.scrollTo(0,0);}
}

function triggerTimeout(){document.getElementById('timeout-overlay').classList.add('show');}
function dismissTimeout(){document.getElementById('timeout-overlay').classList.remove('show');finalizeResults();}

function finalizeResults(){
  done=true;
  const score=answers.filter((a,i)=>a===questions[i].a).length;
  const unanswered=answers.filter(a=>a===null).length;
  const wrong=TOTAL-score-unanswered;
  const pct=Math.round(score/TOTAL*100);
  const used=TIME_LIMIT-timeLeft;
  const mn=Math.floor(used/60),sc=used%60;
  const grade=pct>=90?'Отлично!':pct>=70?'Хорошо!':pct>=50?'Удовлетворительно':'Нужно повторить';
  const gc=pct>=90?'excellent':pct>=70?'good':pct>=50?'ok':'fail';
  document.getElementById('result-grade').textContent=grade;
  document.getElementById('result-grade').className='result-grade '+gc;
  document.getElementById('result-pct').textContent=score+' / '+TOTAL+' правильных — '+pct+'%';
  document.getElementById('stats-grid').innerHTML=`
    <div class="stat-card"><div class="stat-val g">${score}</div><div class="stat-lbl">Правильно</div></div>
    <div class="stat-card"><div class="stat-val r">${wrong}</div><div class="stat-lbl">Неверно</div></div>
    <div class="stat-card"><div class="stat-val m">${unanswered}</div><div class="stat-lbl">Пропущено</div></div>
    <div class="stat-card"><div class="stat-val b">${mn}м ${String(sc).padStart(2,'0')}с</div><div class="stat-lbl">Затрачено</div></div>`;
  const review=questions.map((q,i)=>{
    const ua=answers[i],ca=q.a;
    const isOk=ua===ca,isSkip=ua===null;
    const cc=isSkip?'skip':isOk?'ok':'err';
    let ah='';
    if(isSkip)ah=`<div class="rv-ans ra-skip"><span class="rv-tag">ОТВЕТ</span><span>${q.o[ca]}</span></div>`;
    else if(isOk)ah=`<div class="rv-ans ra-ok"><span class="rv-tag">✓</span><span>${q.o[ca]}</span></div>`;
    else ah=`<div class="rv-ans ra-err"><span class="rv-tag">✗ ВЫ</span><span>${q.o[ua]}</span></div>
      <div class="rv-ans ra-ok"><span class="rv-tag">✓ ВЕРНО</span><span>${q.o[ca]}</span></div>`;
    return`<div class="rv-card ${cc}"><div class="rv-q">${i+1}. ${q.q}</div>${ah}</div>`;
  }).join('');
  document.getElementById('review-container').innerHTML=review;
  showScreen('result-screen');
}

function newQuiz(){showScreen('start-screen');}
function scrollToReview(){document.querySelector('.review-sep').scrollIntoView({behavior:'smooth'});}
function showScreen(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0,0);
}
</script>
</body>
</html>
