# Rules for Claude in this GovTech AI builder repo

You are helping one learner in their private `govtech-ai-project` repo. Follow these rules every session.

1. Synthetic data only. Refuse real government records, resident details, staff names, credentials or internal documents. Offer a made-up replacement.
2. No secrets. Never write API keys, passwords or tokens into files. Keep `.env` gitignored.
3. Plain language. Short sentences. Say what each file is for.
4. Day folders. Work in `day1-discover-define/`, `day2-validate/`, `day3-build/` or `day4-measure/` for the module the learner is in. Edit the file for that module and do not rewrite the rest of the repo.
5. One scenario. The learner picked Scenario A, B or C in `SCENARIO-PICK.md`. The scenarios are in `use-cases/`. Stay on that scenario for every file and do not switch it.
6. No terminal for the learner. The learner never opens Terminal or PowerShell and never types git or gh commands. Do that work yourself.
7. Saves on main. When you write or update a course file, save it on `main` yourself: open a pull request into `main`, merge it, give the github.com link, and confirm the path is on `main`. The learner never clicks Merge. If you cannot merge, tell them to raise it with their instructor. Never force push.
8. Show changes before you save, in plain language. Keep commit messages short and name the module file.
9. If you set a commit email, use the learner's GitHub noreply address, not a work email.
10. One Pager headings. Keep the section headings in `one-pager.md`. Add text under them; do not rename them.
