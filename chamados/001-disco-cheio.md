# Chamado 001: Disco do Linux com uso elevado

**Data:** 18/10/2026
**Área:** Linux / Armazenamento
**Ambiente:** Chromebook, Linux (Debian) via Crostini
**Status:** Em andamento

## Sintoma
O Linux do Chromebook está com pouco espaço livre, o que pode atrapalhar instalações e projetos.

## Comandos utilizados

| Comando | Para que serve | Partes importantes |
|---|---|---|
| `df -h` | Mostra o espaço de cada disco ou partição | `df` = disk free; `-h` = tamanhos legíveis (GB, MB) |
| `du -sh ~/.[!.]* ~/* 2>/dev/null \| sort -h` | Mostra o tamanho de cada pasta da home, da menor para a maior | `du` = uso de disco; `-s` = só o total; `-h` = legível; `~` = pasta pessoal; `\|` = passa o resultado para o `sort`; `sort -h` = ordena por tamanho |
| `du -sh ~/.vscode/* ~/.config/* 2>/dev/null \| sort -h \| tail -n 10` | Mostra o tamanho do conteúdo de duas pastas, só os 10 maiores | `~/.vscode/*` = itens dentro da pasta; `tail -n 10` = últimas 10 linhas (as maiores) |

## Passo 1: visão geral do disco (`df -h`)

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/vdc         11G  8.9G  2.0G  82% /
(demais linhas omitidas)
```

| Item | Valor |
|---|---|
| Tamanho total | 11 GB |
| Usado | 8,9 GB |
| Livre | 2,0 GB |
| Uso | 82% |

**Leitura:** ainda há 2 GB livres, então não é uma emergência, mas convém investigar antes de chegar perto de 90%, quando instalações começam a falhar.

**Linhas ignoradas e por quê:** `tmpfs` e `devtmpfs` ficam na memória; `cros-containers` e `fonts/chromeos` parecem ser partes do ChromeOS, fora do meu controle.

## Passo 2: maiores pastas da home (`du`)

```text
$ du -sh ~/.[!.]* ~/* 2>/dev/null | sort -h
(saída resumida, apenas as maiores entradas)
97M     ~/.local
128M    ~/.cache
261M    ~/Meus_Projetos
287M    ~/Full-Stack developing
337M    ~/.npm
983M    ~/.config
1.4G    ~/.vscode
```

| Pasta | Tamanho | Hipótese |
|---|---|---|
| `.vscode` | 1,4 GB | Dados e extensões do VS Code (a verificar) |
| `.config` | 983 MB | Configurações de aplicativos (a verificar) |
| `.npm` | 337 MB | Cache do npm |
| `Full-Stack developing` | 287 MB | Projeto, possivelmente com `node_modules` |
| `Meus_Projetos` | 261 MB | Idem |

**Observação importante:** a soma da home é de cerca de 3,4 GB, mas o `df -h` mostrou 8,9 GB usados. Portanto, cerca de 5,5 GB estão **fora da home**, e preciso investigar o sistema também (passo 5).

**Plano atualizado:** passo 3 (por dentro de `.vscode` e `.config`), passo 4 (`node_modules`), passo 5 (fora da home), passo 6 (caches).

## Passo 3: por dentro de `.vscode` e `.config`

```text
$ du -sh ~/.vscode/* ~/.config/* 2>/dev/null | sort -h | tail -n 10
(saída resumida, apenas as maiores entradas)
232M    ~/.config/nvm
752M    ~/.config/Code
1.4G    ~/.vscode/extensions
```

| Pasta | Tamanho | O que parece ser |
|---|---|---|
| `~/.vscode/extensions` | 1,4 GB | Extensões instaladas no VS Code |
| `~/.config/Code` | 752 MB | Dados do VS Code (caches e workspaces), a detalhar |
| `~/.config/nvm` | 232 MB | NVM e as versões do Node (não apagar) |

**Leitura:** as extensões do VS Code são o maior item da home, ocupando mais do que todos os meus projetos juntos. Hipóteses a verificar: uma ou duas extensões muito grandes, ou várias versões da mesma extensão acumuladas.

**Próximo passo:** olhar por dentro de `extensions` e de `Code`.

## Passo 4: pastas `node_modules`
(preencher)

## Passo 5: espaço fora da home
(preencher)

## Passo 6: caches (`apt` e `npm`)
(preencher)

## Causa encontrada
(preencher)

## Solução aplicada
(preencher)

## Resultado

| | Antes | Depois |
|---|---|---|
| Uso de `/` | 82% | (preencher) |
| Espaço livre | 2,0 GB | (preencher) |

## O que aprendi
(escrever com suas palavras)
