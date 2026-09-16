# Jornal Omni — versão protegida para o GitHub Pages

Tudo nesta pasta é independente do resto do projeto: nada aqui é lido pelos scripts principais (`gerar_html.py`, `gerar_pdf.py` etc.), e nenhum arquivo fora desta pasta é alterado pelo que está aqui. É seguro subir **só esta pasta** para o GitHub.

## O que tem aqui

Todos os arquivos ficam **soltos, sem subpastas** — isso é de propósito: o GitHub, ao fazer upload pelo navegador, nem sempre preserva pastas aninhadas corretamente, então ficou mais confiável manter tudo num nível só.

| Arquivo | O que faz |
|---|---|
| `index.html` | Tela de senha/rosto. É a página que abre primeiro. |
| `lock.css` | Visual da tela de bloqueio. |
| `auth.js` | Lógica de desbloqueio (senha e reconhecimento facial). |
| `content.enc.js` | **Gerado** — o jornal inteiro, criptografado. Não editar à mão. |
| `manifest.json` | Lista os nomes das fotos de referência para o login por rosto. |
| `*.jpg` / `*.png` | As fotos de referência em si (cadastradas no `manifest.json`). |
| `gerar_site_protegido.py` | Script que gera `content.enc.js` a partir do jornal já pronto. |

## Como gerar (ou atualizar) o conteúdo protegido

1. Gere o jornal do dia normalmente (atalho "Jornal Omni" na área de trabalho).
2. Rode, dentro desta pasta:
   ```powershell
   python gerar_site_protegido.py --password "sua senha aqui"
   ```
   (ou rode sem `--password` e digite quando for pedido, sem aparecer na tela)
3. Suba **todos os arquivos desta pasta direto na raiz do repositório** no GitHub (não dentro de uma subpasta — o GitHub Pages, no plano gratuito, só serve a partir da raiz ou de uma pasta chamada `docs`).
4. Em Settings → Pages, confirme que o Source está em "Deploy from a branch", branch `main`, pasta `/ (root)`.

## Cadastrar uma nova foto de rosto

1. Coloque a foto solta nesta pasta (ex.: `chefe.jpg`).
2. Adicione o nome do arquivo em `manifest.json`, ex.: `["Felipe Freitas.jpg", "chefe.jpg"]`.
3. Suba os dois arquivos pro GitHub.

## Como funciona a proteção — limites honestos

- **Senha**: o jornal inteiro fica criptografado (AES) dentro de `content.enc.js`. Sem a senha certa, o conteúdo não aparece — nem olhando o código-fonte da página, nem baixando o arquivo. Isso protege contra acesso casual (alguém que encontra o link, curiosos, mecanismos de busca).
- **Reconhecimento facial**: roda 100% no navegador (nenhuma foto ou vídeo é enviado para servidor nenhum). Para o rosto conseguir desbloquear o conteúdo sem digitar a senha, a senha precisa estar guardada (em base64, só para não aparecer literalmente) dentro de `content.enc.js`. **Isso significa que qualquer pessoa que abrir as Ferramentas do Desenvolvedor do navegador (F12) consegue encontrar a senha.** É uma limitação de qualquer site estático sem servidor — não existe como esconder de verdade um segredo que o próprio navegador do visitante precisa conseguir ler.
- **Resumindo o nível de proteção**: bom o suficiente para impedir que alguém tropece no conteúdo sem querer. Não é proteção contra alguém tecnicamente capaz e determinado a acessar de qualquer jeito. Para isso, seria necessário um servidor de verdade (fora do escopo de um site estático no GitHub Pages).
