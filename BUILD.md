# Ansible — BUILD

- **Repo:** `UncleD0886475978/Ansible` — **public fork** (per task brief). A chapter-by-chapter Ansible learning collection, not a single deployable project.
- **Stack actually found** (via GitHub connector directory listing of the root):
  - Ansible playbooks/roles organised by topic folders: `Chapter 2 - Introduction to Playbooks` through `Chapter 10 - Importing & Including` (variables, loops, conditions, handlers, failure handling, templating), plus `Databases with Ansible`, `Linux Administration via Ansible`, `NTP Ansible Role`, `VSFTPD Ansible Role`, and `fail2ban`.
  - Root: `README.md`, `Test.txt`, `.gitattributes` — **no root `site.yml` or main playbook, no ansible.cfg, no Terraform/Docker/Python app** at root.
- **Inspection note:** the connector returned directory listings, not file bodies, and a GitHub HTTP 429 (rate limit) stopped inspection before individual chapter folders were enumerated. `setup.sh` discovers every playbook at runtime with `find`, so the catalogue it prints is always the real one.

## One-line install (fresh Ubuntu/Debian VM)

```bash
curl -fsSL https://raw.githubusercontent.com/UncleD0886475978/Ansible/main/setup.sh | bash
```

The script installs its own prerequisites — git/curl base tools and python3 + venv via apt (Ansible itself is installed with pip into a local `.venv`, never via apt) — then locates/clones the repo and catalogues it. Alternative: clone the repo yourself and run `./setup.sh` from its root, as described below.

## What setup.sh does

0. Platform check + base bootstrap: installs git/curl/ca-certificates/gnupg via apt when missing (on non-apt systems it only checks tools and prints install guidance).
1. Uses the current checkout if its git remote mentions `Ansible` (or the script sits in the repo root); otherwise clones into `~/repos/Ansible` (`git pull` if already there). Clone URL keeps the capital-A repo name.
2. Bootstraps python3 + venv via apt; on a non-apt system without python3 it prints the Python and Ansible install-doc URLs and exits cleanly.
3. Resolves Ansible: system `ansible-playbook` if present, else creates `.venv` and `pip install ansible`.
4. Lists the top-level chapters/sections and **every** `.yml`/`.yaml` playbook with a one-line usage — no single "main" playbook is forced.
5. Syntax-checks one sample playbook to prove the toolchain works, with a clear note that chapter playbooks often need their own folder-local inventory/variables.
6. Prints next commands: work chapter by chapter (quote folder names — they contain spaces), `--check` first, and notes on stopping/logs.

**Issue it prevents:** hunting for a non-existent `site.yml` entry point (or running a mid-course chapter cold) in what is actually a sequenced tutorial collection.
