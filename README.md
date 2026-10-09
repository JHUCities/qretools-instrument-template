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
   dots, such as `edu.example.survey-lab`). Do the same in `banks/local/bank.yaml` for
   that bank's questions.
4. **Choose your banks:** the example instrument uses two: `banks/local`, a bank kept in
   this repository, and the [bank template](https://github.com/JHUCities/qretools-bank-template)
   at its `v1` tag. Point `uses:` at your own banks instead (see below).
5. **Open it** in QREtools: sign in and enter the repository as `owner/name`, or
   `owner/name/folder` for a workspace kept in a folder. The version that opens
   instruments is in development and not published yet.

## Layout

| Path | Holds |
|---|---|
| `workspace.yaml` | what the workspace says about itself: the DDI agency its instruments are published under (`agency:`) |
| `instruments/<name>.yaml` | one instrument each |
| `banks/<name>/` | a question bank kept here (optional), laid out as the [bank template](https://github.com/JHUCities/qretools-bank-template) is: the example's is `banks/local` |

Instruments and banks are kept apart: never put an instrument inside a bank's folders.
A bank belongs in its own folder under `banks/`, or in a repository of its own.

## The banks an instrument uses

An instrument names each bank it uses under a short alias, and then names the bank's
questions with it: `ask: tpl.service_satisfaction`. A bank in a repository of its own is
named by its repository and a tag, so the instrument always reads the same version of it
until you change the tag:

```yaml
uses:
  tpl: JHUCities/qretools-bank-template@v1   # owner/name@tag
  own: your-org/your-bank/banks/main@v2      # owner/name/folder@tag, for a bank in a folder
```

A bank kept in this repository is named by where it is from the instrument's own folder:

```yaml
uses:
  local: ../banks/local
```

Only a tag is read, never a branch, so the bank's owners decide what version is
published: they tag it. A private bank's repository needs the QREtools app installed
(step 2) for the instrument tool to read it.

## The example

`instruments/example.yaml` asks a question of its own bank and the bank template's example questions, to copy or delete:

| Shows | Where |
|---|---|
| a statement read to the respondent | `say:` |
| a question from this workspace's own bank, and stopping on its answer | `ask: local.consent`, then `stop:` with a `say:` |
| sections | `section: The service`, with its own `flow:` |
| a question asked only of some respondents, with whom it's asked of | `if:` with `then:`, and `universe:` on the step |
| a check on an answer, with a message naming it | `checks:` with `ensure:`, `severity:` and `message:` |
| a value from outside the interview | `inputs:` (`form`), read in a condition |
| a split-ballot experiment | `if: form = "1"`, `then:` and `else:`, on the bank's two parks questions |
| select all that apply | `ask: tpl.news_sources`, one variable per option |

Conditions are written in VTL, the SDMX standard's expression language:
`tpl.service_satisfaction in {"4", "5"}`, `form = "1"`, `tpl.library_visits < 25`. Codes
are strings, so they are quoted. QREtools completes questions after `ask:` and names in
conditions, and lists what is still to fill in or fix as you type.
