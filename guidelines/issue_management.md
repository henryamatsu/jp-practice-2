# GitHub Issue + Project Commands (Agent Reference)

Use this as a reusable command reference for any repo/project.

## 0) Set Reusable Variables (PowerShell)

```powershell
$OWNER = "<owner-or-org>"
$REPO = "<repo-name>"
$PROJECT_NUMBER = <project-number>
$ISSUE = <issue-number> # not necessarily worth defining as a variable since it will change often
```

## 1) One-Time Setup

Install GitHub CLI:

```powershell
winget install --id GitHub.cli
```

Authenticate and ensure project scopes:

```powershell
gh auth login
gh auth refresh -s project -s read:project -s repo
gh auth status -t
```

## 2) Find Project Number

List projects for a user/org:

```powershell
gh project list --owner $OWNER
```

## 3) Create Issues (In Correct Order)

Create one issue:

```powershell
gh issue create --repo "$OWNER/$REPO" --title "SDF 01: <title>" --body "<body>"
```

Create multiple issues sequentially (not parallel) so numbering stays in intended order.

## 4) Add Issue to Project

Add by issue URL:

```powershell
gh project item-add $PROJECT_NUMBER --owner $OWNER --url "https://github.com/$OWNER/$REPO/issues/$ISSUE"
```

## 5) List Issues in a Project

All project items:

```powershell
gh project item-list $PROJECT_NUMBER --owner $OWNER
```

Open issues only:

```powershell
gh project item-list $PROJECT_NUMBER --owner $OWNER --query "is:issue is:open"
```

## 6) Read/Inspect Issues

List repo issues:

```powershell
gh issue list --repo "$OWNER/$REPO"
```

View one issue:

```powershell
gh issue view $ISSUE --repo "$OWNER/$REPO"
```

## 7) Update/Edit Issues

Edit title/body:

```powershell
gh issue edit $ISSUE --repo "$OWNER/$REPO" --title "<new title>" --body "<new body>"
```

Add/remove labels:

```powershell
gh issue edit $ISSUE --repo "$OWNER/$REPO" --add-label "label1,label2"
gh issue edit $ISSUE --repo "$OWNER/$REPO" --remove-label "label1"
```

Assign/unassign:

```powershell
gh issue edit $ISSUE --repo "$OWNER/$REPO" --add-assignee "<username>"
gh issue edit $ISSUE --repo "$OWNER/$REPO" --remove-assignee "<username>"
```

Comment:

```powershell
gh issue comment $ISSUE --repo "$OWNER/$REPO" --body "<comment>"
```

## 8) Close/Reopen Issues

Close:

```powershell
gh issue close $ISSUE --repo "$OWNER/$REPO"
```

Reopen:

```powershell
gh issue reopen $ISSUE --repo "$OWNER/$REPO"
```

## 9) Move/Update Project Item Fields (Status/Priority/etc.)

1. Get project metadata (project id, fields, options):

```powershell
gh project view $PROJECT_NUMBER --owner $OWNER --format json
```

2. Get item ids:

```powershell
gh project item-list $PROJECT_NUMBER --owner $OWNER --format json
```

3. Update a field:

```powershell
gh project item-edit --id <item-id> --project-id <project-id> --field-id <field-id> --single-select-option-id <option-id>
```

## 10) Agent Rules of Thumb

- Always create related issues sequentially when order matters.
- Prefix related issue titles (`SDF 01`, `SDF 02`, etc.) for stable human ordering.
- Add each issue to the project immediately after creation.
- Stop on first error and report command + error output.
