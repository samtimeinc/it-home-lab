# 2026-11-26 — the loopback setting that wasn't in the list

**Lab:** 01 · **Machines involved:** DC01, CLIENT01

## Goal

Make every machine in the `Workstations` OU display the company wallpaper through Group Policy, so the setting could be proven with a before/after pair instead of a claim.

## What happened

Built the GPO `IT-Wallpaper`, linked it to the **Workstations** OU, and set *User Configuration → Policies → Administrative Templates → Desktop → Desktop → Desktop Wallpaper* to Enabled with a local path. Then I went looking for **Configure user Group Policy loopback processing mode** and it simply wasn't there.

**Computer Configuration → Administrative Templates → System → Group Policy** listed eight settings and jumped straight from `Configure Group Policy slow link detection` to `Set Group Policy refresh interval for users`. The setting I needed should have been between them. Nothing in the console said anything was hidden.

## Root cause

Two separate mistakes stacked on each other.

**1. Wrong half of the editor.** I was reading the *User Configuration* tree. The folder list gave it away in hindsight — Desktop, Shared Folders, Start Menu and Taskbar are user-side nodes. The loopback policy is declared `class="Machine"` in `GroupPolicy.admx`, so it can only ever appear under **Computer Configuration**. Confirmed by reading the file directly on the host:

```text
policy name="UserPolicyMode" class="Machine" key="Software\Policies\Microsoft\Windows\System"
parentCategory="PolicyPolicies"  →  System → Group Policy
Merge = 1, Replace = 2
```

**2. A design mistake underneath it.** The wallpaper is a *User* setting, but I had linked the GPO to an OU full of *computer* objects. User settings follow where the **user** object lives — Mara Chen sits in `Employees\Sales`, not `Workstations` — so the policy would have applied to nobody even if it were configured correctly.

## Fix

Located the setting under Computer Configuration → Administrative Templates → System → Group Policy, enabled it with mode **Merge** (`UserPolicyMode = 1`; Replace is `2`), and left the GPO linked to the Workstations OU. Loopback is exactly what makes a machine's OU able to deliver user settings to whoever logs on there.

Verified on CLIENT01 as `mara.chen`:

```text
gpresult /r  →  Applied Group Policy Objects:  IT-Wallpaper
               CN=Mara Chen,OU=Sales,OU=Employees,DC=rainiertrading,DC=local
```

A Sales user, in a GPO linked to the Workstations OU, receiving the setting — that is loopback processing working.

## What I learned

A Group Policy object has two halves and each follows a different object. Computer settings follow the computer's OU; user settings follow the user's OU. When a setting "doesn't exist," check which half of the console you are standing in before assuming the template is missing it.

And the bigger one: **`gpresult` saying "applied" answers a different question than "did it work."** The policy reports as applied here while the desktop stayed black — the setting arrived, the outcome did not. Those are two separate tests, and an artifact that only shows the first is not proof.
