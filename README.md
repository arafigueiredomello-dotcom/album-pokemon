# Álbum Pokémon — Coleção da Família

App web estático para catalogar a coleção de cartas Pokémon dos meus filhos. Mostra cada carta com imagem, raridade, valor estimado, e permite buscar, filtrar e ver detalhes completos.

## Como funciona

O app é 100% estático — roda direto de HTML + JS no navegador, lendo as cartas de um arquivo `cartas.json`. Sem backend, sem banco de dados.

## Estrutura dos arquivos

```
album-pokemon/
├── index.html      ← Abra este arquivo no navegador
├── cartas.json     ← Lista de cartas (edite aqui para adicionar/remover)
└── README.md       ← Este arquivo
```

## Como testar localmente

Navegadores bloqueiam `fetch` de arquivos locais (`file://`) por segurança. Use um servidor simples:

```bash
# Com Python 3
cd album-pokemon
python3 -m http.server 8000

# Depois abra: http://localhost:8000
```

Ou use a extensão **Live Server** do VS Code, ou qualquer outro servidor estático.

## Como publicar no GitHub Pages

1. **Crie um repositório** no GitHub (pode ser público, tipo `album-pokemon`)
2. Suba os três arquivos (`index.html`, `cartas.json`, `README.md`)
3. No repo, vá em **Settings → Pages**
4. Em *Source*, escolha **Deploy from a branch**
5. Selecione a branch `main` (ou `master`) e pasta `/ (root)`
6. Salve. Em ~1 minuto o site estará em:
   ```
   https://SEU_USUARIO.github.io/album-pokemon/
   ```

## Como adicionar cartas novas

Edite o `cartas.json`. Cada carta é um objeto com este formato:

```json
{
  "id": "me-050-mmanectric-pt-01",
  "name": "Mega Manectric ex",
  "number": "050/132",
  "set": "Mega Evolution",
  "set_code": "ME01",
  "rarity": "ultra",
  "type": "Elétrico",
  "hp": 330,
  "stage": "Mega Evolução",
  "evolves_from": "Manectric",
  "lang": "pt",
  "value_brl": 45,
  "quantity": 1,
  "variant": "Padrão",
  "notes": "",
  "img": "",
  "attacks": [
    { "name": "Raio de Clarão", "cost": "LLL", "damage": "120", "text": "..." }
  ]
}
```

### Campos

| Campo | Obrigatório | Valores |
|-------|-------------|---------|
| `id` | Sim | ID único. Padrão: `{setcode}-{num}-{nome}-{lang}-{seq}` |
| `name` | Sim | Nome da carta |
| `number` | Sim | Ex: `050/132` |
| `set` | Sim | Nome da expansão |
| `set_code` | Não | Código interno da expansão |
| `rarity` | Sim | `common`, `rare`, `ultra` |
| `type` | Não | Elétrico, Fogo, Água, Grama, Psíquico, Metálico, Fada, Sombrio, Dragão, Normal, Incolor |
| `hp` | Não | Pontos de vida |
| `stage` | Não | Básico, Estágio 1, Estágio 2, Mega Evolução |
| `evolves_from` | Não | De qual Pokémon evolui |
| `lang` | Sim | `pt` ou `en` |
| `value_brl` | Não | Valor estimado em reais |
| `quantity` | Não | Quantas cópias a família tem (default: 1) |
| `variant` | Não | "Padrão", "Prize Pack", "Holo", "Reverse Holo", etc. |
| `notes` | Não | Observações livres |
| `img` | Não | URL da imagem. Se vazio, mostra placeholder colorido |
| `attacks` | Não | Lista de ataques |

### Sobre o campo `img`

O ideal é usar URLs de imagens oficiais. A API pública mais comum é:

```
https://images.pokemontcg.io/{set_code_oficial}/{numero}_hires.png
```

Mas nem todas as expansões estão indexadas, especialmente as mais recentes como Mega Evolution. Por isso deixei vazio por padrão — o placeholder colorido mostra o nome da carta com gradient do tipo.

Se quiser usar fotos próprias, coloque elas numa subpasta `img/` e referencie assim:

```json
"img": "img/mega-manectric-050.jpg"
```

## Como eu (Claude) ajudo a atualizar

O fluxo é:

1. Você tira foto das cartas novas
2. Me manda no chat do Claude
3. Eu identifico as cartas, estimo valor, e te devolvo o JSON pronto
4. Você cola no `cartas.json`, commita no GitHub
5. O Pages atualiza automaticamente

### Dicas pra identificação funcionar bem

- **Foto nítida** com bom foco no número (ex: `050/132`) e nome
- **Poucas cartas por foto** (4 a 9 no máximo)
- **Sem reflexo no plástico** — ajuda levemente angular
- **Avise o idioma** se não for português
- **Diga se tem repetidas** e quantas

## Personalização

- **Cores e tema**: edite as variáveis CSS no topo do `<style>` em `index.html`
- **Nome/dono**: edite o campo `meta.owners` em `cartas.json`
- **Novo tipo de Pokémon com gradient próprio**: adicione uma classe `.card-placeholder.type-Novo` no CSS

## Limitações conhecidas

- **Sem edição pelo app** — pra adicionar/editar cartas, é via `cartas.json` + commit. Isso é propositado: GitHub Pages é estático. Quem quiser editar pelo site precisaria de backend (não é o escopo).
- **Valores são estimativas** — preços de cartas oscilam. Trate como referência.
- **Cartas muito recentes** podem não ter imagem oficial indexada.
