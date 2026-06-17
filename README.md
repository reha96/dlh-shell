# DLH — Shell

A progressive study of Linux shell basics and file permissions — from user management and file operations to ownership, symbolic/octal permission modes, and conditional commands.

---

## Directory Structure

```
dlh-shell/
├── permissions/                 # Linux file permissions & ownership
│   ├── 0-iam_betty             through 16-if_only
│   └── README.md
└── README.md
```

---

## Quick Reference

| Module | Topics | Tasks |
|--------|--------|-------|
| [Permissions](permissions/) | `su`, `whoami`, `groups`, `chown`, `touch`, `chmod` (symbolic & octal), `chgrp`, directory permissions, symlink ownership, conditional `chown` | 17 |

---

## Learning Progression

1. **User Identity** (Tasks 0–2): `su`, `whoami`, `groups`
2. **File Operations** (Tasks 3–4): `chown`, `touch`
3. **Symbolic Permissions** (Tasks 5–7): `chmod u+x`, `chmod ug+x,o+r`, `chmod a+x`
4. **Octal Permissions** (Tasks 8–9): `chmod 007`, `chmod 753`
5. **Advanced Operations** (Tasks 10–12): `--reference`, directories, `mkdir -m`
6. **Group & Conditional** (Tasks 13–16): `chgrp`, `user:group`, `chown -h`, `chown --from`

---

## Resources

- [Linux File Permissions — Red Hat](https://www.redhat.com/sysadmin/linux-file-permissions-explained)
- [chmod Manual](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [chown Manual](https://man7.org/linux/man-pages/man1/chown.1.html)
