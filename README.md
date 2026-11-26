# model-vram-calculator-skill

An agent skill (`SKILL.md`) that teaches an agent to estimate LLM inference memory with the `vramcalc` CLI.

- CLI source: `/home/bitnom/Code/vramcalc` (Go).
- Skill: `SKILL.md` in this folder.

## Install

```sh
cd /home/bitnom/Code/vramcalc && go install ./cmd/vramcalc   # puts vramcalc in $(go env GOPATH)/bin
ln -s /home/bitnom/Code/model-vram-calculator-skill ~/.agents/skills/vramcalc
```

Make sure `$(go env GOPATH)/bin` is on your `PATH`.
