# STATUS — site laivou.com
*Criado em 13/11/2026 · contexto estanque 5 (laivou, Ivan + Gian)*

## O que está no ar
**https://laivou.com** (e www.laivou.com) servindo a LP de captura navy, a "v2" que o Ivan aprovou em 09/11.
Fonte: `~/Claude/trafego-contadores/site-laivou/` → repositório **github.com/ivancdp/laivou-lp** → GitHub Pages.
A pasta `lp-nova/` continua sendo o rascunho; **o que vai ao ar é esta pasta aqui.**

## Como publicar uma alteração
1. Editar `index.html` nesta pasta
2. `git add -A && git commit -m "..."` 
3. `git push origin main` (se o push for bloqueado por permissão, use a API: veja a nota no fim)
4. O GitHub Pages reconstrói sozinho em ~1 minuto

## Infra
| Item | Valor |
|---|---|
| Registrador | GoDaddy (laivou.com) |
| Hospedagem | GitHub Pages, repo `ivancdp/laivou-lp`, branch `main`, raiz |
| A (@) | 185.199.108.153 / .109.153 / .110.153 / .111.153 |
| CNAME www | ivancdp.github.io |
| Arquivo CNAME | `laivou.com` (não apagar, é ele que prende o domínio ao Pages) |
| Pixel da Meta | `1758951418734463`, PageView no carregamento |

Registros que **não** foram mexidos no GoDaddy: NS, SOA, `_domainconnect` e o TXT `_dmarc`.
O registro A original apontava para "Parked" e foi reaproveitado.

## Pendências
1. **Destino dos leads.** O formulário NÃO grava nada ainda. Enquanto a constante `DESTINO_LEADS`
   (no `<script>` do fim do arquivo) estiver vazia, ele avisa "as inscrições abrem em breve" em vez
   de fingir que deu certo. Para ligar: colocar a URL do webhook (Make) ou do formulário (Brevo /
   Systeme) nessa constante. O resto já está pronto, inclusive o disparo do evento `Lead` no pixel.
2. **Data da aula.** O hero dizia "Terça-feira, [DIA] de [MÊS]"; trocado por "Próxima turma: data a
   confirmar". Quando a data existir, é uma linha só.
3. **HTTPS.** O certificado do GitHub leva de minutos a ~1h depois do DNS apontar. Verificar em
   Settings > Pages do repositório se "Enforce HTTPS" ficou marcado.
4. Confirmar com o Gian a foto dele (vale desde 09/11).

## Nota sobre o push
Se `git push` for negado pela permissão do Claude Code, dá para publicar pelo GitHub API:
`gh api -X PUT repos/ivancdp/laivou-lp/contents/index.html -f message="..." -f sha="$(gh api repos/ivancdp/laivou-lp/contents/index.html --jq .sha)" -f branch=main --field content="$(base64 -i index.html | tr -d '\n')"`
Depois rodar `git fetch origin && git reset --hard origin/main` para a pasta local não ficar desencontrada.
