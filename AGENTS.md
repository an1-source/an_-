# AGENTS.md

Coursework repo for 《高级网页设计》 with two independent parts. Not a git repo; no CI, tests, linters, or build step anywhere.

## Repo rules (user-mandated, follow for every task)

- **技术栈约束**：练习页只用原生 HTML、CSS 和 JavaScript（ES6+ 语法可以），不引入任何第三方库、框架（如 Vue/React/Bootstrap）或外部 CDN（如 unpkg、jsDelivr、BootstrapCDN）。
- **图标**：统一使用项目 `font/` 目录下的 iconfont（本地字体文件 + CSS），通过 `<i class="iconfont icon-xxx"></i>` 引用，不使用 Font Awesome、Lucide 或其他在线图标。
- Prefix **every** CSS class name, id, and JS variable with `an_` (e.g. `.an_bar`, `#an_rootSize`, `const an_slider`) in newly written/edited exercise pages.
- Every newly created or edited HTML exercise page must include an HTML comment near the top with the student's name, the creation date, and the page's functional purpose — e.g. `<!-- 姓名：安家成 | 日期：2026-10-22 | 功能：XXX 练习 -->`. Update the date when substantially editing an existing page.
- **Before** executing a task, append the prompt to `an_prompt.md` (append only, never overwrite) with: timestamp, model used (`opencode/mimo-v2.6-flash-free`), and the verbatim prompt.
- Every newly created root-level HTML exercise page must include a back-link to the homepage `index.html` (e.g. `<a class="an_back" href="index.html">← 返回首页</a>` at the top, class prefixed `an_`).

## Layout

- Repo root: standalone static HTML/CSS exercises (`2-1.html`, `8-1css3选择器.html`, …), `css.css`, `image/`. Open directly in a browser — no server. Root `index.html` is the student's portfolio/navigation page, unrelated to the web app below.
- `AI智能化编程/`: the actual app — an Express + vanilla-JS AI learning platform ("AI个性化学习平台"). This is the only part with Node tooling.

## Running the app (`AI智能化编程/`)

- Start: `npm start` (= `node server.js`) from `AI智能化编程/`. Port **3000** is hardcoded in `server.js:11`.
- Requires `.env` with `DOUBAO_API_KEY` (Volcengine Ark; model id is hardcoded at `server.js:16`). `JWT_SECRET` is optional (falls back to a hardcoded default).
- Entry: `server.js` (single file, all routes defined there). It serves `public/` statically, so the site is `http://localhost:3000/` (login at `/login.html`).
- Frontend must be loaded through the server: `public/js/main.js` sets `API_BASE = ''` and pages call relative `/api/...` paths, so opening `public/*.html` via `file://` breaks all API calls.
- Verify changes by manually exercising the page in a browser against a running server; there is no test/lint/typecheck command to run.

## API conventions (easy to get wrong)

- Every response is an envelope `{ code, msg?, data? }` with HTTP **200** even for errors (`code: 400/500`). Frontend checks `json.code`, never `res.ok`.
- AI endpoints (`/api/tutor`, `/api/question/generate`, `/api/path/plan`, `/api/knowledge`, `/api/essay`, `/api/report`, `/api/word/lookup`) prompt the model for pure JSON, then strip ```` ```json ```` fences and `JSON.parse` it. Any prompt change must keep output as parseable bare JSON or the endpoint returns `code: 500`.
- The home page's feature cards come from `GET /api/home/layout` (`server.js`), not hardcoded in `public/index.html` — register new pages there. Per-page nav bars are hardcoded inside each `public/pages/*.html` and must be edited individually.

## State

- Server state is in-memory: `users` array and `tutorSessions` object. Restarting wipes accounts and chat history. Don't look for a database.
- All client data (vocab, mistakes, notes, plans, auth token, tutor `sessionId`) lives in `localStorage`; word lists are imported from `public/data/words/*.json`.

## Style

- Code comments, UI strings, and error messages are in Chinese. Match that when editing.
- Pages under `public/pages/` are self-contained single files (inline `<style>`/`<script>`) except for `/css/style.css` and `/js/main.js`.
- `AI智能化编程/node_modules/` and `npm-cache/` are vendored locally; `.gitignore` there covers `node_modules/`, `.env`, `*.log`.
