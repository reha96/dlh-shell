# Shell — Permissions

A progressive study of Linux file permissions: user management, ownership, file modes (symbolic and octal), directory permissions, and conditional operations.

---

## Learning Objectives

| # | Concept |
|---|---------|
| 1 | Switch users with `su` |
| 2 | Identify the current user with `whoami` |
| 3 | List group memberships with `groups` |
| 4 | Change file ownership with `chown` |
| 5 | Create empty files with `touch` |
| 6 | Modify permissions with `chmod` (symbolic and octal notation) |
| 7 | Set permissions for owner, group, and others independently |
| 8 | Mirror permissions between files with `--reference` |
| 9 | Apply permissions recursively to directories |
| 10 | Create directories with specific permissions via `mkdir -m` |
| 11 | Change group ownership with `chgrp` |
| 12 | Change owner and group simultaneously |
| 13 | Handle symbolic link ownership with `chown -h` |
| 14 | Conditionally change ownership with `chown --from` |

---

## Task-by-Task Reference

Each task below highlights the **unique challenge** it posed and the **new command/flag** introduced — commands from earlier tasks are not repeated.

---

### Task 0 — Switch User (`0-iam_betty`)

**Challenge:** Change the active user identity to `betty` — introducing user switching.

**Approach:** Use `su betty` to switch the current session to user `betty`.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `su <username>` | Switch user — change the effective user ID for the session |

> **Key takeaway:** `su` (switch user) lets you assume another user's identity. Without arguments, it defaults to `root`. Use `su -` for a full login shell with the target user's environment.

---

### Task 1 — Current User (`1-who_am_i`)

**Challenge:** Display the effective username of the current user — introducing identity introspection.

**Approach:** Use `whoami` to print the username of the user running the script.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `whoami` | Print the effective username of the current user |

> **Key takeaway:** `whoami` tells you who you are in the current shell — useful after `su` to confirm the user switch worked.

---

### Task 2 — Group Memberships (`2-groups`)

**Challenge:** List all groups the current user belongs to — introducing group introspection.

**Approach:** Use `groups` to print all group memberships.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `groups` | Print all groups the current user is a member of |

> **Key takeaway:** A user can belong to multiple groups. `groups` shows them all. File permissions check both the user and group ownership.

---

### Task 3 — Change Owner (`3-new_owner`)

**Challenge:** Transfer ownership of a file to another user — introducing `chown`.

**Approach:** Use `chown betty hello` to change the owner of `hello` to `betty`.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chown <user> <file>` | Change the owner of a file |

> **Key takeaway:** Only `root` can change file ownership. `chown` affects the user owner; use `chown user:group` to change both.

---

### Task 4 — Create Empty File (`4-empty`)

**Challenge:** Create a file with no content — introducing `touch`.

**Approach:** Use `touch hello` to create an empty file named `hello`.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `touch <file>` | Create an empty file, or update the timestamp of an existing file |

> **Key takeaway:** `touch` is the simplest way to create an empty file. If the file already exists, `touch` updates its access and modification timestamps without changing content.

---

### Task 5 — Add Execute Permission (`5-execute`)

**Challenge:** Make a file executable by its owner — introducing `chmod` with symbolic notation.

**Approach:** Use `chmod u+x hello` to add execute permission for the owner.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chmod u+x <file>` | Add (+) execute (x) permission for the user/owner (u) |

> **Key takeaway:** Symbolic `chmod` uses `u` (user/owner), `g` (group), `o` (others), `a` (all). The operators are `+` (add), `-` (remove), `=` (set exactly). Permissions: `r` (read), `w` (write), `x` (execute).

---

### Task 6 — Multiple Permissions (`6-multiple_permissions`)

**Challenge:** Set different permissions for different categories in one command — combining owner, group, and other permission changes.

**Approach:** Use `chmod ug+x,o+r hello` — add execute for owner and group, add read for others.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chmod ug+x,o+r <file>` | Apply different permission changes to different categories in one command |
| Comma-separated `chmod` clauses | Chain multiple symbolic permission changes |

> **Key takeaway:** `chmod` accepts comma-separated changes — `u+x,g-w,o=r` all in one command. Read from left to right; each clause applies independently.

---

### Task 7 — Execute for Everyone (`7-everybody`)

**Challenge:** Grant execute permission to all users — introducing the `a` (all) shorthand.

**Approach:** Use `chmod a+x hello` or equivalently `chmod +x hello` to add execute for owner, group, and others.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chmod a+x <file>` | Add execute permission for all (owner + group + others) |
| `chmod +x <file>` | Shorthand — same as `a+x` |

> **Key takeaway:** `a` (all) is the default when no category is specified. `chmod +x` is the most common way to make a script executable.

---

### Task 8 — James Bond Permissions (`8-James_Bond`)

**Challenge:** Set permissions to 007 — owner and group have no permissions, others have full access — introducing octal notation.

**Approach:** Use `chmod 007 hello`. Octal 007 means: owner=0 (---), group=0 (---), others=7 (rwx).

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chmod 007 <file>` | Octal notation: set permissions numerically |
| Octal permission encoding | Each digit: 4=read, 2=write, 1=execute; sum for combinations |

> **Key takeaway:** Octal (numeric) `chmod` is precise and absolute — it sets permissions exactly, not relative to existing ones. Three digits: owner, group, others. Each digit is the sum of r(4) + w(2) + x(1).

---

### Task 9 — John Doe Permissions (`9-John_Doe`)

**Challenge:** Set a file's mode to `rwxr-x-wx` — reinforcing octal notation with a specific permission pattern.

**Approach:** Use `chmod 753 hello`. Octal 753 = owner:7 (rwx), group:5 (r-x), others:3 (-wx).

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chmod 753 <file>` | Octal 753: rwx for owner, r-x for group, -wx for others |
| r-x = 5 (4+1), -wx = 3 (2+1) | Permission arithmetic for non-standard combinations |

> **Key takeaway:** Octal permissions are a compact way to express any permission combination. Common patterns: 755 (rwxr-xr-x) for executables, 644 (rw-r--r--) for data files, 600 (rw-------) for private files.

---

### Task 10 — Mirror Permissions (`10-mirror_permissions`)

**Challenge:** Copy the exact permissions from one file to another — introducing `--reference`.

**Approach:** Use `chmod --reference=olleh hello` to set `hello`'s mode to match `olleh`'s.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chmod --reference=<source> <target>` | Copy permissions from the reference file to the target |

> **Key takeaway:** `--reference` clones permissions without needing to know or type the octal value. Useful for enforcing consistent permissions across related files.

---

### Task 11 — Directory Execute Permissions (`11-directories_permissions`)

**Challenge:** Add execute permission to all subdirectories (not files) — introducing directory-specific operations.

**Approach:** Use `find` or `chmod` with `*/` glob: `chmod a+x */` adds execute to all subdirectories in the current directory.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chmod a+x */` | Apply `chmod` to all subdirectories (the `*/` glob matches directories) |
| Execute on directories | Execute on a directory means permission to `cd` into it and access its contents |

> **Key takeaway:** Execute permission on directories controls traversal (`cd`), not execution. Without `x` on a directory, you cannot access files inside it, even if you have read on those files.

---

### Task 12 — Create Directory with Mode (`12-directory_permissions`)

**Challenge:** Create a directory with specific permissions in one step — introducing `mkdir -m`.

**Approach:** Use `mkdir -m 751 my_dir` to create `my_dir` with mode 751 (rwxr-x--x).

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `mkdir -m <mode> <dir>` | Create a directory with specific permissions atomically |
| Atomic creation + permission | Avoid race condition of `mkdir` then `chmod` |

> **Key takeaway:** `mkdir -m` sets permissions at creation time, avoiding the tiny window where a directory exists with default (usually 755) permissions before `chmod` runs.

---

### Task 13 — Change Group (`13-change_group`)

**Challenge:** Change the group ownership of a file — introducing `chgrp`.

**Approach:** Use `chgrp school hello` to set the group owner to `school`.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chgrp <group> <file>` | Change the group owner of a file |

> **Key takeaway:** `chgrp` changes only the group. Use `chown :group` as an alternative syntax. A user can change group ownership to any group they belong to.

---

### Task 14 — Change Owner and Group (`14-change_owner_and_group`)

**Challenge:** Change both owner and group for all files in the current directory — using a single `chown` command.

**Approach:** Use `chown vincent:staff *` — the `user:group` syntax changes both simultaneously. The `*` wildcard applies to all files and directories.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chown <user>:<group> <file>` | Change both owner and group in one command |
| `chown <user>:<group> *` | Apply ownership change to all files in the current directory |

> **Key takeaway:** `chown user:group` combines owner and group changes. The wildcard `*` applies the command to everything in the current directory (but not hidden files unless `.*` is used).

---

### Task 15 — Symbolic Link Ownership (`15-symbolic_link_permissions`)

**Challenge:** Change the owner of a symbolic link itself (not its target) — introducing `chown -h`.

**Approach:** Use `chown -h vincent:staff _hello` — the `-h` flag affects the symlink, not the file it points to.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chown -h <user>:<group> <symlink>` | Change ownership of the symbolic link itself (not the target) |

> **Key takeaway:** By default, `chown` dereferences symlinks and changes the target's owner. `-h` (or `--no-dereference`) operates on the link itself. Same behavior applies to `chmod` and `chgrp`.

---

### Task 16 — Conditional Ownership (`16-if_only`)

**Challenge:** Change a file's owner only if it's currently owned by a specific user — introducing conditional `chown`.

**Approach:** Use `chown --from=guillaume vincent hello` — only changes owner from `guillaume` to `vincent` if the current owner matches.

**New commands introduced:**

| Command | Purpose |
|---------|---------|
| `chown --from=<current> <new> <file>` | Conditionally change owner — only if current owner matches |

> **Key takeaway:** `--from` makes `chown` conditional — a safety guard against changing ownership of the wrong file. If the current owner doesn't match, the command does nothing (no error).

---

## Technique Inventory

| Task | New technique summarized | Category |
|------|--------------------------|----------|
| 0 | `su <user>` — switch user | User Management |
| 1 | `whoami` — print current user | Identity |
| 2 | `groups` — list group memberships | Identity |
| 3 | `chown <user> <file>` — change owner | Ownership |
| 4 | `touch <file>` — create empty file | File Operations |
| 5 | `chmod u+x` — symbolic execute permission | Permissions |
| 6 | `chmod ug+x,o+r` — chained symbolic permissions | Permissions |
| 7 | `chmod a+x` / `chmod +x` — executable for all | Permissions |
| 8 | `chmod 007` — octal notation, James Bond pattern | Octal Permissions |
| 9 | `chmod 753` — rwxr-x-wx octal pattern | Octal Permissions |
| 10 | `chmod --reference=<src> <tgt>` — mirror permissions | Permissions |
| 11 | `chmod a+x */` — directory execute, glob matching | Directories |
| 12 | `mkdir -m <mode>` — atomic directory creation | Directories |
| 13 | `chgrp <group> <file>` — change group only | Ownership |
| 14 | `chown <user>:<group> *` — combined owner+group | Ownership |
| 15 | `chown -h` — symlink ownership (no dereference) | Symlinks |
| 16 | `chown --from=<current> <new>` — conditional ownership | Ownership |

---

## Resources

- [chmod — Linux Manual](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [chown — Linux Manual](https://man7.org/linux/man-pages/man1/chown.1.html)
- [Linux File Permissions Explained](https://www.redhat.com/sysadmin/linux-file-permissions-explained)
- [Symbolic vs Octal chmod](https://www.gnu.org/software/coreutils/manual/html_node/File-permissions.html)
