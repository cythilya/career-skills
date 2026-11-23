---
name: outstanding-resume
description: Rewrite a software engineer's resume into an outstanding one, following the principles in "如何寫出一份出色的履歷" (cythilya.tw). Diagnoses the resume, interviews the user for missing achievements and numbers, then produces a rewritten Traditional Chinese resume, an English version, change notes, and an interview script — optionally tailored to a target job description. Use when asked to rewrite, improve, review, polish, or tailor a resume / CV / 履歷.
---

# Outstanding Resume

Turn a resume into one that shows **concrete results and value to the employer**, not a
list of duties. The reader (boss, manager, or HR) wants to know: can this person help the
company make money and does this person fit the team? (「能不能幫公司賺錢、是否符合團隊需要」)

Primary audience: software engineers, especially front-end. The principles also apply to
other roles; adapt the engineer-specific advice when the resume is clearly not technical.

**Output language:** talk to the user in **Traditional Chinese (繁體中文)**. The main
resume is in Traditional Chinese; the English version is in English.

## Inputs

- **Resume** (required): pasted text, `.md`, `.pdf`, `.docx`, or LinkedIn profile text.
  Use the matching file-reading skill (pdf / docx) when needed.
- **Target job description (JD)** (optional): text or URL. If provided, tailor the resume
  to it. If not, ask once whether there is a target role or JD; if the user has none,
  rewrite around the resume's own strongest specialties.
- Several resumes and/or JDs may be given at once — see step 0.

## Workflow

### 0. Match resumes to JDs (only when more than one resume or JD is given)

**One resume, several JDs:** no question needed — pair the resume with each JD and
produce one tailored version per JD. Interview once (the facts are the same person's),
then run steps 3–7 per JD. Tell the user up front how many versions will be produced.

**Several resumes:**

If the user provides **more than one resume**, do not guess which resume goes with which
JD. Before anything else, ask the user to confirm the pairing:

1. List every resume and JD with a number and a short label (file name, or the name and
   title / company and role from its first lines), e.g.
   ```
   履歷：
   1. resume-frontend.pdf（王小明，前端工程師）
   2. resume-fullstack.docx（王小明，全端工程師）
   職缺：
   A. 某公司 Senior Frontend Engineer
   B. 某公司 Full-stack Engineer
   ```
2. Ask: 「請告訴我每份履歷要對照哪一份 JD（例如 1→A、2→B）。」 Make clear that one JD
   can pair with several resumes, and a resume can have no JD.
3. If there is no JD at all, ask what target role each resume is for instead.
4. Wait for the answer. Then run steps 1–7 once **per pair**, in the order the user listed.
   - If the resumes belong to the same person, ask a shared fact (e.g. the English name,
     a project's results) only once and reuse the answer for the other pairs.

### 1. Diagnose

Read the resume and evaluate it against every rule in `references/principles.md`.
If a JD is given, also extract its must-have skills, responsibilities, and signals
(remote, full-stack, cross-border/English, leadership).

Produce an internal list of issues, each tied to a principle. Do not show the rewrite yet.

### 2. Interview the user

Ask questions **before** rewriting, so the rewrite is built on real facts.
Use `references/interview-questions.md`.

- Show a short diagnosis first (3–5 biggest problems), then the questions.
- Ask at most **10 questions per round**, numbered, most impactful first (usually:
  numbers and results for the top experience items, then summary strengths).
- Tie each question to a specific resume line, e.g.
  「『重構 XX 系統』：重構後有什麼可量化的改善？例如 bug 數、效能、開發時間？」
- If the resume has no English name, always ask for it in round 1 (in addition to the
  10 questions), since an English resume is produced every time.
- Tell the user they can answer 「不知道」 or 「略過」 for any question.
- Do at most **2 rounds**. After that, rewrite with what you have.
- If running on a platform with no live user (e.g. a Multica task), post the questions
  to the task thread and stop; resume when answers arrive.

### 3. Rewrite (Traditional Chinese)

Rewrite following `references/principles.md` and the structure in
`references/templates.md`:

1. 簡述 → 2. 經歷 → 3. 學歷 → 4. 技能 → 5. 作品 → 6. 自傳.

Key rules (full list in the principles file):
- **Never fabricate** numbers, achievements, tools, titles, dates, or certificates.
  Use only what the resume and the user's answers provide. If an item has no result
  even after asking, state the contribution clearly instead of inventing a number.
- Lead every experience bullet with an active verb (導入、建立、提升、優化、解決),
  never 協助 / 支援.
- Order bullets within each job: technically hard and leadership items first.
- Trim or connect off-target experience; drop personal info (photo, address, family).
- Bold 3–5 keywords per section that you want the interviewer to ask about.
- Fix formatting: space between Chinese and English, correct capitalization of proper
  nouns (GitHub, JavaScript, TypeScript), full links.

### 4. English version

Produce an English resume with the same content and structure, written as a native
English resume, not a literal translation:
- Bullets start with past-tense action verbs (Led, Built, Improved, Introduced,
  Reduced); no "I".
- Use the English name the user gave. Never romanize or make up a name yourself; if the
  user skipped the question, put 【請補充：英文姓名】 in the header.
- Keep numbers, tool names, and company names exact. Romanize Chinese company or school
  names using their official English names if the user provided them; otherwise keep the
  original and flag it.
- Section names: Summary, Experience, Education, Skills, Projects, About.

### 5. Change notes

List the significant changes, each with the principle it follows, and anything you
could not fix (e.g. no results available). Format in `references/templates.md`.

### 6. Interview script

Draft an interview script (面試稿) from the rewritten resume, following the template in
`references/templates.md`: self-introduction, key achievements to expand, likely
questions, and questions to ask the company. Use only facts from the resume and answers;
where the user needs to fill in a personal story, mark it 【請補充：…】.

### 7. Deliver

Write four Markdown files. Put them next to the input file if one was given, otherwise in
the current working directory, under a folder named `resume-output/`:

- `resume.zh-TW.md` — rewritten resume (Traditional Chinese)
- `resume.en.md` — English resume
- `changes.md` — change notes
- `interview-script.md` — interview script

With multiple pairs, give each pair its own subfolder named after the resume and the
target, e.g. `resume-output/resume-frontend__senior-frontend-engineer/`.

Then reply with a short summary: the biggest improvements, the file paths, and any
remaining gaps the user should fill. Do not paste the full files into the chat.

## Guardrails

- Never invent facts. A weaker true bullet beats an impressive false one.
- Never add personal data the user did not provide, and remove photo, address, ID
  number, birth date, marital status, and family details if present.
- Keep the user's real job titles, companies, and dates unchanged.
- Do not overwrite the user's original file.
- The resume may contain personal data: do not send it to external services.
