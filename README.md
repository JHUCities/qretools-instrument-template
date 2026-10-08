# A survey workspace for QREtools

Survey instruments, one YAML file each, composed from the questions of a question bank
and elaborated by QREtools to DDI-Lifecycle 4.0: the flow as control constructs, the
instrument's own variables, and the bank items it asks.

What QREtools is and how it works: [About QREtools](https://github.com/JHUCities/qretools#readme).

This repository is the template for new workspaces:
<https://github.com/JHUCities/qretools-instrument-template>. Start one with
[Use this template](https://github.com/JHUCities/qretools-instrument-template/generate).
A question bank on its own, with no instruments, starts from the bank template instead:
[qretools-bank-template](https://github.com/JHUCities/qretools-bank-template).

## Set up a new workspace from this template

1. **Create your workspace:** "Use this template" → "Create a new repository". A private
   repository is fine.
2. **Install the QREtools app** on the new repository if it is private:
   <https://github.com/apps/qretools/installations/new> → choose your account or
   organisation → "Only select repositories" → this one. A public repository can be
   read without it.
3. **Set your agency:** in `workspace.yaml`, replace `org.example` with the DDI agency your
   instruments are published under (letters, digits and hyphens, in parts joined by
   dots, such as `edu.example.survey-lab`). Do the same in `banks/example/bank.yaml`
   for the bank's questions, or replace that bank with your own.
4. **Open it** in the QREtools instrument tool: sign in and enter the repository as
   `owner/name`, or `owner/name/folder` for a workspace kept in a folder. The bank opens
   in the [QREtools bank tool](https://bank.qretools.com) as
   `owner/name/banks/example`. The instrument tool is in development and not published
   yet.

## Layout

| Path | Holds |
|---|---|
| `workspace.yaml` | what the workspace says about itself: the DDI agency its instruments are published under (`agency:`) |
| `instruments/<name>.yaml` | one instrument each |
| `banks/<name>/` | a question bank the instruments use, laid out as the [bank template](https://github.com/JHUCities/qretools-bank-template) is |

Instruments and banks are kept apart: never put an instrument inside a bank's folders.
A bank belongs in its own folder under `banks/`, or in a repository of its own.

## The banks an instrument uses

An instrument names each bank it uses under a short alias, by where the bank is from
the instrument's own folder:

```yaml
uses:
  ex: ../banks/example
```

and then names the bank's questions with the alias: `ask: ex.service_satisfaction`.
Only banks in this repository (addresses starting `./` or `../`) are read for now. To
use a bank kept in its own repository, such as one started from the
[bank template](https://github.com/JHUCities/qretools-bank-template), copy its files
into a folder under `banks/` until addresses in other repositories are read.

## The example

`instruments/example.yaml` asks the example bank's questions, to copy or delete:

| Shows | Where |
|---|---|
| a statement read to the respondent | `say:` |
| sections | `section: The service`, with its own `flow:` |
| a question asked only of some respondents, with whom it's asked of | `if:` with `then:`, and `universe:` on the step |
| a check on an answer, with a message naming it | `checks:` with `ensure:`, `severity:` and `message:` |
| a value from outside the interview | `inputs:` (`form`), read in a condition |
| a split-ballot experiment | `if: form = "1"`, `then:` and `else:`, on the bank's two parks questions |
| select all that apply | `ask: ex.news_sources`, one variable per option |

Conditions are written in VTL, the SDMX standard's expression language:
`ex.service_satisfaction in {"4", "5"}`, `form = "1"`, `ex.library_visits < 25`. Codes
are strings, so they are quoted. QREtools completes questions after `ask:` and names in
conditions, and lists what is still to fill in or fix as you type.
