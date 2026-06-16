---
date: 2026-06-01T00:00:00-00:00
title: "Isolating AI Coding Agents with Anka"
linkTitle: "Isolating AI Coding Agents"
weight: 9
description: >
  Use Anka VMs to isolate AI coding agents from your macOS host.
---

AI coding agents can execute shell commands, edit files, install packages, and read project context. If you run them directly on your host, their effective permissions are the same as the user session that started them. This is especially important when using permissive agent modes that reduce or skip command approval prompts.

Anka can be used to move that work into a macOS VM. The agent runs inside the VM, the project directory is mounted into the VM, and Anka networking controls can limit communication between the VM, the host, and other local VMs.

> [!NOTE]
> **[Crypt](https://github.com/veertuinc/crypt)** is a helper binary that automates this workflow. It clones a prepared base Anka VM, runs coding agents (Claude Code, Codex, and others) inside the clone, and handles mount, SSH, and cleanup for you. Run `crypt claude` or `crypt codex` from a project directory for an interactive session, or `crypt claude --mount "your prompt"` / `crypt codex --mount "your prompt"` for an unattended task run. See the [Crypt README](https://github.com/veertuinc/crypt/blob/edge/README.md) for usage, flags, and examples.
>
> **Install.**
>
> ```bash
> brew tap veertuinc/crypt https://github.com/veertuinc/crypt
> brew trust veertuinc/crypt
> brew install --cask crypt
> ```
>
> Or install from the release script:
>
> ```bash
> /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/veertuinc/crypt/edge/scripts/install.sh)"
> ```
>
> **Prepare a base VM (one time).** Crypt does not ship VM templates. Create a base VM, install and authenticate your agents inside it, then stop it:
>
> ```bash
> anka create crypt-base latest
> anka start crypt-base
> anka run crypt-base zsh -lc '/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" && brew install node'
> anka run crypt-base zsh -lc 'npm install -g @anthropic-ai/claude-code'
> anka run crypt-base zsh -lc 'npm install -g @openai/codex'
> anka run crypt-base zsh -lc 'claude'    # follow the login prompts
> anka run crypt-base zsh -lc 'codex'     # follow the login prompts
> anka run crypt-base sudo systemsetup -setremotelogin on
> anka stop crypt-base
> ```
>
> Name the VM `crypt-base`, or pass `--vm <name>` when running Crypt. Clones inherit whatever you install and configure in the base VM.

## Isolation model

The recommended model is:

1. Prepare a reusable VM Template with the developer tools your agent needs.
2. Clone that template for a specific project or task.
3. Mount only the project directory the agent should access.
4. Restrict VM to host and VM to VM networking.
5. Limit the agent's tools, credentials, and MCP servers to the current task.
6. Run the AI coding agent from inside the VM.
7. Delete or discard the clone when the task is complete.

This keeps the agent away from your host home directory, host credentials, browser state, and unrelated source trees unless you explicitly mount or copy them into the VM.

## Security context for tool-enabled agents

The NSA's [Model Context Protocol (MCP): Security Design Considerations for AI-Driven Automation](https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf?ver=bmgiSbNQLP6Z_GiWtRt6bg%3D%3D) highlights risks that apply directly to coding-agent workflows, especially when agents can call tools, MCP servers, shell commands, repository APIs, browsers, or GUI automation. The main concern is that tool-enabled agents expand the trust boundary: a prompt, tool response, serialized context object, or MCP server description can influence later tool calls and potentially reach code execution, private repositories, credentials, or internal services.

Anka provides an operating-system isolation boundary for the agent runtime, but it should be combined with application-level controls:

1. Use supported and maintained MCP servers or tool integrations. Avoid unreviewed, abandoned, or dynamically discovered tools for sensitive work.
2. Prefer local MCP servers and local tools inside the VM when working with private source or confidential data.
3. Scope credentials to the project and operation. Do not give the VM broad GitHub, cloud, password manager, or internal network tokens unless the task requires them.
4. Treat tool parameters and tool outputs as untrusted. Validate inputs before execution, and do not blindly feed one tool's output into another privileged tool.
5. Keep approval prompts meaningful. A tool gaining new capabilities, new data access, or write permissions should require review.
6. Log tool invocations, parameters, and generated scripts where practical so you can reconstruct what the agent did.
7. Use network restrictions, outbound proxies, or DLP controls when the agent does not need unrestricted internet or internal network access.

The VM should be considered a containment layer, not the only control. If an MCP server or agent tool can read a mounted secret, use a broad bearer token, or reach a sensitive internal service, it can still misuse that access from inside the VM. Keep the mounted filesystem, credentials, network routes, and tool registry as small as the task allows.

## Prepare an agent VM

Start with the normal VM creation flow, then install the tools needed by your coding agent. This usually includes your package manager, language runtimes, version control tooling, and the agent CLI itself.

For VM creation and template preparation, see [Creating VMs]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md" >}}).

Once the base VM is ready, stop it and use clones for individual projects or tasks:

```bash
anka clone macos-agent-base project-agent
```

Using a clone gives you a disposable working VM without rebuilding the base environment each time.

## Mount only the project

{{< hint warning >}}
Do not mount broad paths like your full home directory unless the agent truly needs them. Anything available in the mounted directory should be treated as available to the agent.
{{< /hint >}}

Mount the source directory the agent should work on:

```bash
anka mount project-agent /path/to/project
```

Inside the VM, mounted folders are available under `/Volumes/My Shared Files`.

```bash
cd "/Volumes/My Shared Files/project"
```

For the full mount behavior, including sync limitations and automount options, see [Sharing host directories inside of the VM]({{< relref "anka-virtualization-cli/getting-started/creating-vms.md#sharing-host-directories-inside-of-the-vm" >}}).

{{< hint info >}}
When you need changes to flow back to the host, prefer making those edits from inside the VM. Host to guest updates can be affected by macOS shared folder caching behavior.
{{< /hint >}}

## Restrict networking

If the agent does not need to connect back to services on the host or to other local VMs, you can use Anka IP filtering to block local VM/host traffic. Filter rules can block local traffic, block inbound services, allow only approved destinations, or deny specific sensitive endpoints. For example, you can embed a ruleset that blocks local VM/host traffic:

```bash
printf "block local\n" | anka modify project-agent network -f-
```

You can also combine several rules. This example blocks inbound SSH to the VM and blocks local VM/host traffic:

```bash
cat <<'EOF' | anka modify project-agent network -f-
block in to port 22
block local
EOF
```

If the agent should only reach an approved internal service or proxy, place the allowed destinations before a broad `block any` rule:

```bash
cat <<'EOF' | anka modify project-agent network -f-
pass out to 10.20.30.40
pass in from 10.20.30.40
block any
EOF
```

You can also keep general network access but deny known sensitive destinations, such as metadata services or internal admin endpoints:

```bash
cat <<'EOF' | anka modify project-agent network -f-
block out to 169.254.169.254
block out to 10.20.30.10 port 443
block local
EOF
```

Applying a new filter replaces the previous filter for that VM, so keep the full intended policy in one ruleset.

See [IP Filtering Rules]({{< relref "anka-virtualization-cli/advanced-security-features.md#ip-filtering-rules" >}}) for rule syntax and examples.

{{< hint warning >}}
Advanced network security features require an Enterprise or Enterprise Plus license.
{{< /hint >}}

## Usage examples

### Run the agent inside the VM

After the project is mounted and networking is restricted, you can use `anka run` from the host to execute non-interactive commands inside the VM. If the VM is stopped, `anka run` will start it automatically:

```bash
anka run project-agent bash -lc 'cd "/Volumes/My Shared Files/project" && your-agent-command --print "review this project"'
```

The command is issued from the host terminal, but `bash`, the working directory change, and `your-agent-command` run inside `project-agent`.

{{< hint warning >}}
`anka run` does not provide TTY mode. Interactive terminal applications, including interactive coding agents, should be run through SSH or VNC instead.
{{< /hint >}}

For longer interactive sessions, start the VM and SSH or VNC into it, then run the same command directly in the guest:

```bash
cd "/Volumes/My Shared Files/project"
your-agent-command
```

The exact command depends on the coding agent you use. The important part is that the agent process executes inside the VM, not directly on the host.

#### Claude example

For a Claude Code workflow, prepare a reusable VM Template once with the `claude` CLI and any shared development tools:

```bash
anka clone macos-agent-base claude-agent-base
anka run claude-agent-base zsh -lc 'brew install node && npm install -g @anthropic-ai/claude-code'
```

Then start the VM and complete any interactive Claude authentication or default configuration inside the guest:

```bash
anka start claude-agent-base
anka show claude-agent-base
ssh -tt anka@192.168.64.10 'zsh -lc "claude"'
anka stop claude-agent-base
```

Replace `192.168.64.10` with the VM IP from `anka show`.

For each project or task, clone from that prepared template, start the clone, mount only the current project directory, and run Claude from the mounted path over SSH with a forced TTY:

```bash
PROJECT_AGENT="claude-agent-$(date +%Y%m%d-%H%M%S)"
PROJECT_DIR="$(pwd)"
PROJECT_NAME="$(basename "$PROJECT_DIR")"
PROJECT_AGENT_IP="192.168.64.10" # Replace with the IP from anka show.

anka clone claude-agent-base "$PROJECT_AGENT"
anka start "$PROJECT_AGENT"
anka mount "$PROJECT_AGENT" "$PROJECT_DIR"
anka show "$PROJECT_AGENT"
ssh -tt "anka@$PROJECT_AGENT_IP" "zsh -lc 'cd \"/Volumes/My Shared Files/$PROJECT_NAME\" && claude --dangerously-skip-permissions'"
```

`ssh -tt` is important here. Without a TTY, Claude may treat the session as non-interactive and require input through stdin or a prompt argument.

This gives Claude access to the mounted project, while the Claude process, shell commands, package installs, and generated files execute inside the VM. When the task is complete, stop and delete the clone:

```bash
anka stop "$PROJECT_AGENT"
anka delete "$PROJECT_AGENT"
```

{{< hint warning >}}
Only use permissive agent flags inside a VM that has been scoped for the task. Keep mounts, credentials, MCP servers, and network access as narrow as possible.
{{< /hint >}}

### Use Cursor Editor over SSH

You can also use [Cursor](https://www.cursor.com/) as the editor while keeping the development environment inside the Anka VM. Install a Remote SSH extension in Cursor, connect to the VM over SSH, and open the project folder from the remote session. Cursor's remote server, integrated terminals, language servers, and agent tooling then run inside the VM instead of directly on the host.

Start the VM and find its IP address:

```bash
anka start project-agent
anka show project-agent
```

If the VM was created with `anka create`, Remote Login is enabled by default. You can test SSH from the host:

```bash
ssh anka@192.168.64.10
```

Replace `192.168.64.10` with the VM IP from `anka show`. If you have changed the VM user or password, use those values instead.

In Cursor:

1. Install a Remote SSH extension.
2. Run `Remote-SSH: Connect to Host...`.
3. Connect to `anka@192.168.64.10`.
4. Open the project folder inside the VM.

There are two possible folder layouts:

1. Open the mounted host project at `/Volumes/My Shared Files/<folder>` only if all edits will be made from inside the VM. Guest changes are written back to the host, but host-side edits are not reliably reflected back into the guest for already-accessed files.
2. Clone the repository inside the VM, for example under `/Users/anka/project`. This is the recommended Cursor over SSH layout because the project contents, dependency cache, generated files, and agent workspace live entirely in the VM clone.

{{< hint warning >}}
Cursor over SSH requires host-to-VM SSH access. Do not apply a blanket `block local` or `block in to port 22` rule before connecting unless you also add a rule that allows SSH from the host. For higher isolation, use an in-VM repository checkout and restrict SSH after the session is finished.
{{< /hint >}}

---

### Automate GUI interactions with Anka Click scripts

Some macOS setup steps and app workflows do not expose a complete command line interface. Dialogs, System Settings panels, installer prompts, simulator menus, and app-specific preferences may require UI interaction. For those cases, you can use [Anka Click Scripts](https://github.com/veertuinc/anka-click-scripts) inside the VM to automate the same interactions a user would perform through the GUI.

Starting in Anka 3.3.0, supported VMs include the click executable at:

```bash
/Library/Application\ Support/Veertu/Anka/bin/click
```

Click scripts can target UI elements by text, image, or coordinate, wait for text or images to appear, send keystrokes, and use keyboard shortcuts. This lets the agent handle GUI-only workflows without giving it direct access to your host desktop.

For example, if an app opens a confirmation dialog inside the VM, the agent can write a small click script in the mounted project or a temporary VM path:

```bash
cat > allow-dialog.click <<'EOF'
+"Allow"
("Allow")
EOF

"/Library/Application Support/Veertu/Anka/bin/click" allow-dialog.click
```

In an AI-agent workflow, you can give the agent the Anka Click syntax and examples, then let it generate the script it needs for the current VM state. A useful pattern is:

1. Ask the agent to describe the GUI action it needs to perform.
2. Have it write a focused `.click` script for that action.
3. Run the script inside the VM.
4. Have the agent inspect the result and adjust the script if the UI changed.

Keep generated click scripts narrow and task-specific. They are easier to review, safer to rerun, and less likely to break when macOS or the application UI changes.

## Cleanup

When the task is complete, stop and delete the project clone if you do not need to keep its VM state:

```bash
anka stop project-agent
anka delete project-agent
```

Keep long-lived tools and common dependencies in the base template, and keep project-specific work in short-lived clones. This makes it easier to return to a known-good environment for each new task.
