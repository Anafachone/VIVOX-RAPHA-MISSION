# VIVOX Flow: página Acompanhamento (protótipo)

Protótipo navegável da página **Acompanhamento** do VIVOX Flow. Reúne, em um lugar só, tudo o que foi feito para um cliente: o antes e depois, as publicações do Instagram e os extras (pastas de fotos e vídeos).

É um único arquivo, `index.html`, sem instalação. A mesma página funciona no celular e no computador. O visual é creme e bronze, com a tipografia do VIVOX Flow, e tem versão escura.

> Os dados e as fotos são de exemplo. O que você adiciona na página fica só na memória do navegador e some ao recarregar. Veja "Anotações para a TI".

## Como abrir

1. Baixe o repositório ou só o arquivo `index.html`.
2. Abra o arquivo em qualquer navegador.
3. Para abrir direto em um cliente, use o id dele no endereço, por exemplo `index.html#clinica-exemplo`.

A fonte (Plus Jakarta Sans) vem do Google Fonts. Sem internet, a página usa uma fonte do sistema.

## Capturas

Desktop:

| Lista de clientes | Visão geral |
|---|---|
| ![Lista de clientes no desktop](docs/desktop-1-lista-de-clientes.png) | ![Visão geral no desktop](docs/desktop-2-visao-geral.png) |

| Publicações | Extras |
|---|---|
| ![Publicações no desktop](docs/desktop-3-publicacoes.png) | ![Extras no desktop](docs/desktop-4-extras.png) |

Celular:

| Lista | Visão geral | Publicações | Extras |
|---|---|---|---|
| ![Lista no celular](docs/celular-1-lista-de-clientes.png) | ![Visão geral no celular](docs/celular-2-visao-geral.png) | ![Publicações no celular](docs/celular-3-publicacoes.png) | ![Extras no celular](docs/celular-4-extras.png) |

As fotos das capturas são imagens de exemplo.

## O que tem na página

**Lista de clientes.** Busca pelo nome e, em cada linha, a categoria do cadastro: Médico, Médico veterinário, Advogado ou Hospital.

**Página do cliente.** Cabeçalho com foto e nome (botão "Editar cabeçalho") e três abas:

- **Visão geral:** antes e depois lado a lado, um cartão por caso. Cada caso tem canal (Google Meu Negócio, Instagram ou Facebook), a data da contratação e a foto do antes e do depois, todos obrigatórios. A data do depois é a de hoje, preenchida sozinha. Clicar na foto abre em tela cheia, com uma seta para alternar entre antes e depois.
- **Publicações:** gráfico de publicações por mês, com menu de ano, e a lista de publicações do Instagram (data, tipo, link, alcance, envios, visualizações, curtidas, comentários e salvamentos), com filtro por mês. O total de cada mês é contado automaticamente pela lista. As publicações entram por **CSV** (botão "Enviar CSV").
- **Extras:** cada extra é uma pasta de fotos e vídeos sobre um assunto, com título, descrição opcional, data da contratação (mês e ano) e capa. Fica em ordem de contratação, do mais recente para o mais antigo, com filtros de ano e mês. Dá para trocar a ordem dos arquivos, escolher a capa, selecionar quadros para editar ou apagar e arrastar arquivos para dentro do extra aberto. O limite é de **500 MB por arquivo**.

Tudo o que remove algo pede confirmação.

### Formato do CSV de publicações

A primeira linha é o cabeçalho. Aceita vírgula ou ponto e vírgula.

```
tipo,data,link,alcance,envios,visualizacoes,curtidas,comentarios,salvamentos
Reel,05/11/2026,https://www.instagram.com/reel/EXEMPLO/,5000,80,9000,600,40,90
```

O tipo aceita Post, Carrossel, Reel ou Vídeo. A data aceita `2026-11-05` ou `05/11/2026`. Links repetidos são ignorados.

## Dados e identificadores (id)

Os dados de exemplo ficam no começo do script, na lista `CLIENTS` dentro de `index.html`. Cada cliente é um objeto:

| Campo | O que é |
|---|---|
| `id` | Identificador do cliente. Vira o endereço `index.html#id`. |
| `nome` | Nome da empresa. |
| `seg` | Categoria: `Médico`, `Médico veterinário`, `Advogado` ou `Hospital`. |
| `ini` | Iniciais do avatar. |
| `metricas` | Marcador: qualquer valor diferente de `null` libera as abas. Com `null`, aparece "ainda não tem dados". |
| `pub` | `{ pacote, semanal, meses }`: texto do pacote, publicações por semana e meta por mês. |
| `posts` | Publicações: `[tipo, 'AAAA-MM-DD', link, alcance, envios, visualizacoes, curtidas, comentarios, salvamentos]`. |
| `ba` | Antes e depois: `{ canal, antes:{u}, depois:{u}, legAntes, legDepois, dataContrato, dataAtual }`. |
| `port` | Extras: `{ titulo, anoIni, mesIni, anoFim, mesFim, fotos:[{u,v}], capa, capaFora, desc, showDesc }`. Início e fim são iguais (uma data só). |
| `hdrFoto`, `hdrHist` | Foto de cabeçalho `{u,x,y,z}` e as anteriores. |
| `canaisBA` | Canais disponíveis no antes e depois. |

**Como mudar o id de um cliente:** em `CLIENTS`, troque o valor de `id`. Use letras minúsculas, sem acento e com hífen (por exemplo `clinica-nova-vida`), e cada `id` precisa ser único. O link passa a ser `index.html#clinica-nova-vida`, e links antigos deixam de abrir o cliente. Na integração com o sistema, use como `id` o identificador do cadastro de clientes, para o link ficar estável.

**Como adicionar um cliente:** inclua um objeto em `CLIENTS` com `id`, `nome`, `seg` e `ini`, e coloque `metricas` com qualquer valor diferente de `null`. Os demais campos podem começar vazios.

O script também depende de ids fixos nos elementos da página (por exemplo `#busca`, `#addbtn`, `#csvbtn`). Se renomear algum, atualize o script junto.

## Anotações para a TI (Kelson)

### Pedido: ligar com a página do cliente (não feito)

O sistema já tem a página do cliente, com dados e fotos cadastrados. O protótipo muda só o layout. Por isso:

1. **Manter tudo salvo.** O que já foi adicionado no sistema continua salvo, sem perder nada. O que for adicionado nesta página (antes e depois, publicações, extras, capas, ordem e cabeçalho) também precisa ser salvo e voltar nas próximas aberturas.
2. **Puxar da página do cliente, sem cadastrar duas vezes.** Os campos do protótipo que podem vir de lá:
   - `id`: usar o identificador do cadastro.
   - `nome`: nome da empresa.
   - `seg`: categoria (Médico, Médico veterinário, Advogado ou Hospital).
   - Foto de perfil: hoje o avatar mostra só as iniciais (`ini`). Usar a foto de perfil já cadastrada.
   - Foto de cabeçalho (`hdrFoto`): a opção "Usar a foto que já existe no sistema" está desativada, esperando isso.
   - Se necessário: canais contratados (`canais`), pacote e publicações por semana (`pub.pacote` e `pub.semanal`) e data da contratação.
3. Só o que não existe no cadastro é adicionado aqui. O que já existe lá aparece automático.

### Pedido: aceitar PDF e PPT nos Extras (ainda não está feito)

Hoje o extra aceita só fotos e vídeos. O pedido é poder guardar também PDF e apresentações (PPT e PPTX) na mesma pasta. O que precisa mudar:

1. **Aceitar os formatos novos** nos campos de envio: o do formulário de extra (`#a_f`), o do extra aberto (`#lbfile`) e o filtro do arrastar e soltar. Tipos: `.pdf` (`application/pdf`), `.ppt` (`application/vnd.ms-powerpoint`) e `.pptx` (`application/vnd.openxmlformats-officedocument.presentationml.presentation`).
2. **Guardar o tipo de cada arquivo.** Hoje cada arquivo é `{ u, v }` (endereço e se é vídeo). Falta um campo de tipo, por exemplo `t: 'foto' | 'video' | 'pdf' | 'ppt'`.
3. **Mostrar o arquivo.** O navegador não mostra PPT, então a miniatura pode ser um ícone com o nome e a extensão. O PDF pode abrir em uma nova aba.
4. **Capa.** Documento não deve poder ser capa. A capa continua sendo foto ou vídeo.
5. O limite de 500 MB, a troca de ordem por arrastar e a remoção com confirmação valem igual.

Funções do script envolvidas: `isVid`, `limite`, `mediaThumb`, `lightbox` e `openAdd`. Há um comentário "PEDIDO PARA A TI" no `index.html`, perto de `isVid`.

### Outras anotações

- **Envio de arquivos.** As fotos e vídeos são lidos no navegador. No sistema real precisam ser enviados e guardados no servidor, com o limite de 500 MB por arquivo.

## Limitações conhecidas

- O envio de arquivos no iPhone não foi confirmado em um aparelho. Alguns aplicativos bloqueiam o envio de arquivos dentro da página.
- Vídeos que o navegador não reproduz (alguns .mov e .avi) mostram um aviso pedindo MP4.
- Arquivos muito grandes podem deixar o celular lento, porque ficam na memória.

## Estrutura do repositório

```
index.html    a página inteira (HTML, CSS e JavaScript)
README.md     este arquivo
docs/         capturas de tela do desktop e do celular
```
