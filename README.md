# Radar de drops

A lista de drops que aparece na página Stream do bot vem daqui. O GitHub Actions consulta os drops públicos da
Twitch e grava o resultado em `data/drops.json`. O agendamento pede uma rodada a cada 10 minutos, mas pode atrasar.
O seu PC nunca fala com a Twitch pra isso.

## Instalação da Pamela

- Repositório: https://github.com/flyyerss/pamela-drops-radar
- Feed do bot: https://raw.githubusercontent.com/flyyerss/pamela-drops-radar/main/data/drops.json
- Execuções: https://github.com/flyyerss/pamela-drops-radar/actions/workflows/update-drops.yml

No `Config.env` da Pamela:

```dotenv
DROPS_RADAR_URL=https://raw.githubusercontent.com/flyyerss/pamela-drops-radar/main/data/drops.json
GITHUB_RADAR_REPO=flyyerss/pamela-drops-radar
GITHUB_RADAR_TOKEN=
```

O feed funciona sem token pessoal e sem GitHub Pages. O token abaixo é opcional, apenas para o bot solicitar
rodadas extras. Não coloque a chave da Twitch, chaves de IA, o `Config.env` ou outros arquivos da Pamela neste
repositório público. Ele contém somente o coletor, a visualização e os dados públicos de campanhas.

## Montar (uma vez só)

1. Crie uma conta no GitHub e um repositório **público** novo (ex.: `drops`).
2. Suba esta pasta inteira pra ele (o jeito mais fácil: na página do repositório, "Add file" → "Upload files" e
   arraste tudo daqui, **inclusive** a pasta `.github`).
3. **Actions**: se pedir, clique em "I understand my workflows, go ahead and enable them". Depois, em
   "Atualizar drops", clique em "Run workflow" pra rodar a primeira vez (leva ~2 minutos).
4. A lista que o bot lê fica em
   `https://raw.githubusercontent.com/SEU_USUARIO/NOME_DO_REPO/main/data/drops.json`. Ponha esse endereço em `DROPS_RADAR_URL`
   no `Config.env` do bot.

O HTML é uma visualização opcional. Para publicar uma versão que acompanhe cada coleta no Pages, acrescente
um deployment explícito com GitHub Actions. Publicar apenas a branch no Pages não basta: os commits do
`GITHUB_TOKEN` não disparam uma nova compilação do Pages. O feed raw acima acompanha os commits normalmente.

## Solicitar rodadas extras a cada 15 minutos (opcional)

O cron do GitHub não garante execução pontual. O bot pede uma rodada a cada 15 minutos se tiver
um token:

1. GitHub → foto → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate**.
2. "Repository access": só este repositório. "Permissions → Contents": **Read and write**.
3. Ponha o token em `GITHUB_RADAR_TOKEN` e o `SEU_USUARIO/NOME_DO_REPO` em `GITHUB_RADAR_REPO` no `Config.env`.

Não copie um token amplo do GitHub CLI para o `Config.env`. Sem esse token opcional, o workflow continua no
agendamento e pode ser executado pelo botão **Run workflow**.

## O que ele mostra

Só drop **aberto a qualquer canal** (campanha de canais convidados fica de fora). Drop de item de jogo e badge da
Twitch aparecem separados; drop que pede sub vem marcado. Pra esconder um jogo do site, escreva o nome dele no
`jogos-fora.txt` do repositório.
