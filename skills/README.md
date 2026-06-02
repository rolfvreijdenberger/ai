# skills

skills for agents.

## generic installation
- copy the skill directory with it's content in the right configuration directory for your agent.
or
- interactive install with [skills.sh](https://skills.sh): `npx skills add rolfvreijdenberger/ai`   

## pi
- install (locally in `.pi/skills` or globally in `~/.pi/agent/skills/`): `npx skills add rolfvreijdenberger/ai --agent pi --skill <skill-name or *>`
- test a skill in isolation: `pi --name "skill test" --no-tools --no-extensions --no-skills --no-context-files --no-themes --no-prompt-templates --thinking "off" --skill <path-to-skill> "ping"` 

