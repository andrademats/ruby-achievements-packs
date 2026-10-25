# Ruby Achievements — pacotes da comunidade

[English](README.md) · [**Português**](README.pt-BR.md)

Pacotes de conquistas (`.rubydb`) para o [Ruby Achievements](https://github.com/andrademats/ruby-achievements),
listados no catálogo que o app baixa no Nintendo Switch.

No app: abra um jogo > **Conquistas** > **Buscar pacotes da comunidade** (ou **Gerenciar / baixar
outros pacotes**). O app filtra o catálogo pelo Title ID do jogo, mostra o selo de compatibilidade e
instala o pacote como `/switch/ruby_achievements/achievements/<TitleID>.<pack_id>.rubydb`.

| Selo | Significado |
|---|---|
| Verde — Compatível | o build do jogo está em `supported_build_ids` (ou o pacote não tem lista) |
| Amarelo — Aviso | mesmo jogo, build diferente, ou o build ainda não foi lido (abra o jogo uma vez) |
| Vermelho — Incompatível | versão instalada abaixo de `game_version_min` |

## Estrutura

```
index.json                         catálogo lido pelo app (raw.githubusercontent.com, ramo main)
packs/<TitleID>.<pack_id>.rubydb   os pacotes
```

## Como acrescentar um pacote

1. Gere o `.rubydb` no AchieveSet Studio ou com `tools/json_to_rubydb.py` do repositório principal.
   Pacotes com condições de memória precisam de `supported_build_ids`.
2. Copie para `packs/<TitleID>.<pack_id>.rubydb` (`pack_id`: `a-z`, `0-9`, `_`, `-`, até 48).
3. Acrescente ou atualize a entrada do catálogo (título, autor, versão, Title ID, builds, número de
   conquistas e SHA-256 saem do próprio pacote):

   ```
   python -I tools/rubydb_index.py --index index.json --pack packs/<arquivo>.rubydb \
       --url https://raw.githubusercontent.com/andrademats/ruby-achievements-packs/main/packs/<arquivo>.rubydb \
       [--version-min 1.6.0]
   ```
4. Abra um pull request. Toda mudança num pacote exige gerar a entrada de novo: o app recusa um
   arquivo cujo SHA-256 difere do catálogo.

## Regras

- Só pacotes: nada de arquivos de jogo, chaves (`prod.keys`/`title.keys`), dumps, NSP/NSZ ou executáveis.
- Texto até 64 KB por pacote, ícones PNG até 256 KB e 512×512 (conferido pelas ferramentas e pelo app).
- Um pull request por pacote; descreva o que as conquistas exigem e em qual versão do jogo testou.
