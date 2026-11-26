# model-vram-calculator-skill

An agent skill (`SKILL.md`) that teaches an agent to estimate LLM inference memory with the [vramcalc](https://github.com/Apothic-AI/vramcalc) CLI.

## Install

```sh
go install github.com/Apothic-AI/vramcalc/cmd/vramcalc@main   # puts vramcalc in $(go env GOPATH)/bin
git clone https://github.com/Apothic-AI/model-vram-calculator-skill ~/.agents/skills/vramcalc
```

Make sure `$(go env GOPATH)/bin` is on your `PATH`. The repo root is the skill folder, so cloning it into your skills directory installs the skill.

## License

Apache License 2.0. See [LICENSE](LICENSE).
