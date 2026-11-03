# NOTICE

The skills in this folder are copies of third-party open-source work, collected on 2026-10-18.
Each source keeps its own license file next to the copied skills; that license governs those files,
not the license of bear-agent-manager.

| Folder | Upstream | License | License file |
|---|---|---|---|
| `banking/anthropic-financial-services/` | https://github.com/anthropics/financial-services-plugins | Apache-2.0 | `banking/anthropic-financial-services/LICENSE` |
| `slides/anthropic-financial-services/` | https://github.com/anthropics/financial-services-plugins | Apache-2.0 | `slides/anthropic-financial-services/LICENSE` |
| `banking/casemark/` | https://github.com/CaseMark/skills | Apache-2.0 | `banking/casemark/LICENSE` |
| `banking/second-line/` | https://github.com/anotb/second-line-financial-services | MIT | `banking/second-line/LICENSE` |
| `banking/finance-skills/` | https://github.com/JoelLewis/finance_skills | MIT | `banking/finance-skills/LICENSE` |
| `banking/kyc-analyst/` | https://github.com/vyayasan/kyc-analyst | MIT | `banking/kyc-analyst/LICENSE` |
| `slides/frontend-slides/` | https://github.com/zarazhangrui/frontend-slides | MIT | `slides/frontend-slides/LICENSE` |
| `slides/anthropic/*`, `general/anthropic/*` | https://github.com/anthropics/skills | Apache-2.0 | `LICENSE.txt` inside each skill |
| `banking/vietnam/`, `solar/engineering/`, `solar/vietnam/`, `bear/`, `mep/engineering/` | Written for bear-agent-manager | MIT | `LICENSE` in each folder |
| `slides/revealjs-skill/` | https://github.com/ryanbbrown/revealjs-skill | MIT | `slides/revealjs-skill/LICENSE` |
| `slides/agent-toolkit/` | https://github.com/softaworks/agent-toolkit | MIT | `slides/agent-toolkit/LICENSE` |
| `training/onewave/` | https://github.com/onewave-ai/claude-skills | MIT | `training/onewave/LICENSE` |
| `general/knowledge-work/` | https://github.com/anthropics/knowledge-work-plugins | Apache-2.0 | `general/knowledge-work/LICENSE` |
| `legal/anthropic-legal/` | https://github.com/anthropics/knowledge-work-plugins | Apache-2.0 | `legal/anthropic-legal/LICENSE` |
| `legal/casemark/`, `solar/casemark/` | https://github.com/CaseMark/skills | Apache-2.0 | `LICENSE` in each folder |
| `training/course-training-skills/` | https://github.com/Mosapmohamd/course-training-skills | MIT | `training/course-training-skills/LICENSE` |
| `dev/anthropic/*` | https://github.com/anthropics/skills @dbd4588 | Apache-2.0 | `LICENSE.txt` inside each skill |
| `dev/knowledge-work/`, `office/knowledge-work/`, `hr/knowledge-work/`, `marketing/knowledge-work/` | https://github.com/anthropics/knowledge-work-plugins @33d3dea | Apache-2.0 | `LICENSE` in each folder |
| `office/officecli/` | https://github.com/iOfficeAI/OfficeCLI @5e3e262 | Apache-2.0 | `office/officecli/LICENSE` |
| `office/brand-docs/` | https://github.com/ferdinandobons/brand-docs @97a6e38 | MIT | `office/brand-docs/LICENSE` |
| `office/spreadsheet-auditor/` | https://github.com/petehottelet/spreadsheet-auditor @9930ae7 | MIT | `office/spreadsheet-auditor/LICENSE` |
| `office/outlook-skill/` | https://github.com/dbhq-uk/outlook-skill @db4110c | MIT | `office/outlook-skill/LICENSE` |
| `office/o365-skill/` | https://github.com/kargausa/o365-skill @7969ff5 | MIT | `office/o365-skill/LICENSE` |
| `accounting/vietnam-accounting/` | https://github.com/cuongtranba/vietnam-accounting-skills @1b5ec26 | MIT | `accounting/vietnam-accounting/LICENSE` |
| `accounting/vn-accounting-codex/` | https://github.com/abm-dungtq/vn-accounting-codex-skills @caee1d3 | MIT | `accounting/vn-accounting-codex/LICENSE` |
| `construction/ddc/` | https://github.com/datadrivenconstruction/DDC_Skills_for_AI_Agents_in_Construction @410ed1e | MIT (SKILL.md and docs) | `construction/ddc/LICENSE` |
| `logistics/ai-evos/` | https://github.com/ai-evos/agent-skills @1eda1fe | Apache-2.0 | `logistics/ai-evos/LICENSE` |
| `health/life-sciences/*` | https://github.com/anthropics/life-sciences @6aab041 | Apache-2.0 | `LICENSE.txt` inside each skill |
| `health/bio-research/*` | https://github.com/anthropics/knowledge-work-plugins @33d3dea (`bio-research`) | Apache-2.0 | `LICENSE.txt` inside each skill |
| `health/redacta/` | https://github.com/nickjlamb/redacta @563f4a8 | MIT-0 | `health/redacta/LICENSE` |

Changes made to the copies:
- 20 skills (the core of each role pack) have one Vietnamese sentence appended to the `description` field of their SKILL.md frontmatter,
  so Claude also picks them up when the user writes in Vietnamese. The skill body is unchanged. `git log -p` on `skill-library/` shows each change.
- `banking/second-line/*`: the plugin-level `references/*.md` of each upstream plugin is copied into every skill's own `references/`
  so a single skill works on its own. `legal/anthropic-legal/*`: the plugin's `CONNECTORS.md` is copied into each skill folder for the same reason.
- Renamed to avoid clashing with generic names of other installed skills (folder and frontmatter `name`):
  `banking/kyc-analyst/{onboarding,refresh,screening,risk-assessment,transaction-monitoring}` → `kyc-*`;
  `legal/anthropic-legal/brief` → `legal-brief` (its `/brief` command examples now read `/legal-brief`);
  `knowledge-work` `onboarding` → `hr-onboarding` (its `/onboarding` examples now read `/hr-onboarding`);
  `knowledge-work` `human-resources/*` → `hr-*` (e.g. `draft-offer` → `hr-draft-offer`, `/x` examples read `/hr-x`);
  `knowledge-work` `marketing/*` → `mkt-*`, `sales/*` → `sales-*` (slash commands and backticked mentions of the copied skills follow the rename; mentions of sales skills not copied stay as upstream wrote them);
  `knowledge-work` `engineering/*` → `eng-*` (e.g. `code-review` → `eng-code-review`, its `/code-review` examples now read `/eng-code-review`).
- `dev/knowledge-work/eng-*`, `office/knowledge-work/status-report`, `hr/knowledge-work/hr-*`, `marketing/knowledge-work/*`: the plugin `CONNECTORS.md` is copied into the skill folder and the link changed from `../../CONNECTORS.md` to `CONNECTORS.md`.
- `office/spreadsheet-auditor/`: tests, benchmarks, examples, evals and the repo `CLAUDE.md`/`CONTRIBUTING.md`/`CHANGELOG.md` are left out; the skill, its scripts, package and references are unchanged.
- `office/brand-docs/brand-*`: the upstream engine `scripts/brandkit/` and `requirements.txt` are copied into each skill so a single-skill install works; `scripts/cli.py` gains one check that finds that bundled engine first.

- `accounting/vn-accounting-codex/*`: the repo-level `lib/`, `rules/` and `schemas/` are copied into every skill, each skill gets a `requirements.txt` from the upstream `pyproject.toml`, scripts look for them one level up (`parents[1]` instead of `parents[3]`) and the SKILL.md commands read `scripts/x.py` instead of `skills/<name>/scripts/x.py`. Tax rules are as of the upstream commit above.
- `construction/ddc/`: only 20 estimating, quantity take-off and scheduling skills are copied, unchanged (`SKILL.md`, `instructions.md`, `claw.json`). Left out: the DDC book (all rights reserved) and the skills built on the DDC CWICR cost database, which is CC BY-NC 4.0 with a separate commercial license. Product names (Revit, Primavera P6, MS Project…) belong to their owners, as the upstream NOTICE.md states.
- `logistics/ai-evos/*`: `SKILL.md` and `references/` copied unchanged; the upstream `evals/` folders and `shared/` eval framework are left out.
- `health/redacta/redacta`: only `SKILL.md`, `reference.md` and the two scripts it runs are copied; the apps, packages, gateway, benchmark and tests are left out.

Not included because their licenses do not allow redistribution here: Anthropic's `pptx`, `docx`, `xlsx`, `pdf`
skills (proprietary), Anthropic's `doc-coauthoring` (no license file), Anthropic's `skill-creator` (a newer copy is already synced from claude.ai), `narosemena/master-instructional-design` (CC BY-NC-ND 4.0), `espialuso/id-skills-for-claude`
and `zarazhangrui/codebase-to-course` (no license), `jiji262/claude-design-skill` (derived from a non-public system prompt), `anthropics/healthcare` (provided under Anthropic's terms of service, no open license).
