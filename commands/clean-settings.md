You are auditing and cleaning `~/.claude/settings.local.json`. This file is gitignored but may accidentally be committed or shared. Your job is to validate its structure, ordering, and ensure no sensitive information has leaked in.

## Steps

1. Read `~/.claude/settings.local.json`
2. Validate it is well-formed JSON
3. Check the structure matches the expected schema (see below)
4. Scan every string value for sensitive data (see rules below)
5. Report findings and fix any issues found

## Expected structure

The file must contain only these top-level keys (in order):
- `permissions` (object)
- `alwaysThinkingEnabled` (boolean, optional)
- `skipDangerousModePermissionPrompt` (boolean, optional)

The `permissions` object must contain only these keys (in order):
- `allow` (array of strings)
- `deny` (array of strings, may be empty)
- `ask` (array of strings)

No other top-level keys or permissions keys should exist.

## Expected allow array ordering

Entries in `permissions.allow` must follow this section order, separated by blank lines between groups:

1. **Web tools** - `WebSearch`, `WebFetch`
2. **MCP servers** - entries starting with `mcp__`, grouped by server name (e.g. all `mcp__ado__*` together, then `mcp__github__*`, etc.)
3. **AWS CLI** - `Bash(aws *)` patterns
4. **Azure CLI** - `Bash(az *)` patterns
5. **File inspection** - `ls`, `pwd`, `file`, `stat`, `du`, `df`, `tree`, `wc`, `readlink`, `realpath`, `basename`, `dirname`, `find`, checksums
6. **Text processing** - `cat`, `head`, `tail`, `less`, `grep`, `rg`, `ag`, `diff`, `sort`, `uniq`, `cut`, `tr`, `awk`, `sed`, `jq`, `yq`, `xmllint`, `column`, `fmt`, `nl`, `tee`, `rev`, `expand`, `fold`
7. **Shell builtins** - `echo`, `printf`, `test`, `[`, `true`, `false`, `seq`, `expr`, `bc`
8. **System info** - `uname`, `hostname`, `whoami`, `id`, `date`, `uptime`, `sw_vers`, `arch`, `sysctl`, `system_profiler`, `lsb_release`, `env`, `printenv`, `locale`, `ulimit`, `which`, `where`, `whereis`, `type`, `command`, `man`
9. **Process inspection** - `ps`, `lsof`, `pgrep`, `top`, `vm_stat`, `iostat`, `vmstat`, `free`, `nproc`
10. **Network** - `ping`, `curl`, `wget`, `dig`, `nslookup`, `host`, `ifconfig`, `ip`, `netstat`, `ss`, `traceroute`, `tracepath`, `mtr`, `whois`, `nc`
11. **Git / GitHub CLI** - read-only `git` commands, then `gh` commands
12. **Docker** - read-only `docker` commands
13. **Kubernetes** - read-only `kubectl` commands
14. **Terraform** - read-only `terraform` commands
15. **Node.js / JS** - `node`, `npm`, `npx`, `yarn`, `pnpm`, `bun`
16. **Python** - `python`, `pip`, `pipenv`, `poetry`, `uv`
17. **Ruby** - `ruby`, `gem`, `bundle`
18. **Go** - `go`
19. **Rust** - `rustc`, `cargo`
20. **Java / JVM** - `java`, `javac`, `mvn`, `gradle`
21. **.NET** - `dotnet`
22. **Package managers** - `brew`, `apt`, `dpkg`, `rpm`

## Sensitive data rules

Flag and remove any entry or value that contains:
- Absolute file paths (e.g. `/Users/someone/...`, `/home/...`, `C:\Users\...`)
- Usernames or personal names (beyond generic tool names)
- Email addresses
- Passwords, tokens, API keys, or secrets
- Hashes that look like credentials (long hex strings, base64 blobs)
- Client or company names that are not tool/service names
- IP addresses or internal hostnames
- URLs containing auth tokens or query parameters with keys/secrets

Entries like `Bash(aws * describe-*)` or `mcp__github__get_*` are fine — they are generic glob patterns.

## Output

After analysis, report:
1. Whether the JSON is valid
2. Whether the structure and ordering are correct (list any out-of-order sections)
3. Any sensitive data found (quote the offending entry)
4. Any entries that don't belong in this file

Then apply fixes automatically:
- Remove any sensitive entries
- Reorder sections if needed
- Ensure consistent 2-space indentation
- Ensure blank lines separate section groups in the allow array
- Ensure the file ends with a single newline
