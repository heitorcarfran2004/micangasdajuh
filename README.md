# Funil — 96 Projetos de Brincos de Miçanga

Página de vendas do entregável em `entregaveis/50-brincos-micanga`.

    node servidor.cjs     # http://localhost:4180

## Antes de subir — 3 coisas obrigatórias

1. ~~UTMify~~ — **feito.** Script deste produto instalado no `<head>`, carregando
   `cdn.utmify.com.br/scripts/utms/latest.js`.
2. ~~Checkout~~ — **feito.** Os quatro links do ggcheckout instalados:
   Essencial R$ 10 · Completa R$ 25,90 · popup R$ 16,90 · saída R$ 8,90.
3. ~~Depoimentos~~ — **feito.** São 5 prints reais de conversa, de alunas que receberam
   o material para testar em 03/09/2026, em `assets/depoimentos/`.

Falta também: e-mail de contato e CNPJ no rodapé (`TROCAR@email.com`, `CNPJ TROCAR`).

## Página de saída (back redirect)

`promo.html` — servida em `/promo` (cleanUrls da Vercel; o `servidor.cjs` local imita
isso). É para onde vai quem tenta sair da index.

Curta, uma tela só: alerta fixo no topo, headline de retenção, **vitrine deslizante** com
6 páginas de projeto (prova visual antes do preço), card único com a Coleção
Completa de R$ 99,90 por **R$ 8,90**, timer de 10 min, escassez, o que inclui, os 5 bônus
com valor riscado, botão, garantia e rodapé. Mesma identidade da index.

**Ela NÃO tem back redirect** — dois back redirects se chamam em loop e prendem a pessoa.

O checkout dela é um QUARTO link (`TROCAR-CHECKOUT-PROMO`), com oferta própria de R$ 8,90
no gateway.

## Estrutura da página

barra de urgência · hero · dores · desejo (usar/presentear/vender/relaxar) · antes×depois
· o que recebe · carrossel de amostras · bônus · depoimentos · oferta/planos · garantia
· FAQ · fechamento com oferta · rodapé

## Popup de upsell

Clicar no plano de R$ 10 abre um modal oferecendo a Coleção Completa por **R$ 16,90**
(em vez de R$ 25,90) — R$ 6,90 a mais que o básico. Quem recusa segue para o checkout
do básico normalmente.

O botão do Essencial está fora do seletor `a[href*=ggcheckout]` que marca
`__indoParaCheckout`: ele é um link de checkout, mas o clique só abre o popup. Sem essa
exceção, quem visse o popup e desistisse sairia da página com o back redirect desarmado.

São **quatro** ofertas no gateway, uma por preço — R$ 10, R$ 25,90, R$ 16,90 e R$ 8,90.

O plano de R$ 10 leva **só o Bônus 1** (Tabela de Conversão de Cores); os 5 ficam no
Completo. Abaixo do botão do básico há um microaviso apontando para a opção melhor.

## Público e ângulo da copy

A página fala com **lead frio**: quem conhece miçanga, acha bonito e tem vontade de
começar, ou quem já fez pulseira e colar mas **nunca fez brinco**. Ela não pressupõe
que a pessoa já tentou e desistiu.

O ângulo é o mesmo que funciona no funil das carteiras de crochê:

- o **desejo primeiro** — "o brinco que todo mundo vai perguntar onde você comprou"
- a culpa no **material, não na pessoa** — "não é falta de jeito, é foto bonita sem
  instrução nenhuma"
- a seção de **aplicações** (usar, presentear, vender, relaxar), que é o que transforma
  curiosidade em intenção de compra
- o FAQ abre com "nunca fiz brinco de miçanga, consigo?" e tem uma pergunta dedicada a
  quem já faz pulseira e colar

Se um dia for testar o ângulo oposto — falar com quem já tentou brinco e desistiu — a
seção de dores inteira muda, junto com a headline e o primeiro item do FAQ.

## Prova de mercado (seção "Quanto vale a peça pronta")

Os 9 prints em `assets/provas/venda-01..09.webp` são anúncios reais da Shopee, capturados
em **setembro de 2026**. Trazem preço E quantidade vendida — a prova social vem junto com
a de preço, o que é mais forte que só o valor. Faixa: R$ 70 a R$ 180,50; o campeão de
vendas tem 203 unidades. O custo de R$ 8 por par vem de ~12 g de miçanga mais fio e ferragem.

Os prints originais estão em `Downloads/Design sem nome/1..9.png`. O recorte usado foi
`crop=1080:1210:0:35` — tira a barra de navegação do topo e o rodapé de frete, deixando
foto, preço, vendidos e o nome do anúncio.

**Número de mercado envelhece.** Ao atualizar os preços, trocar também a data na nota ao
pé da seção — preço sem data vira promessa vaga.

O enquadramento é proposital: a seção diz **o que o mercado cobra**, não o que a cliente
vai ganhar. Promessa de renda derruba conta de anúncio no Meta e é a diferença entre
prova de mercado e propaganda enganosa. A nota ao pé deixa explícito que o preço depende
de acabamento, fotos e divulgação.

O carrossel **desliza sozinho** para a esquerda (~27 px/s) e continua arrastável. As 9
imagens aparecem DUAS vezes no HTML: a segunda leva é a emenda que faz o loop voltar ao
começo sem salto visível. **Ao trocar as imagens, trocar nas duas levas.**

O deslize usa `requestAnimationFrame` empurrando `scrollLeft`, e não animação CSS —
`transform` brigaria com o scroll nativo e mataria o arraste. Passar o mouse por cima não
para; só o arrasto de verdade (`pointerdown`), e ao soltar volta na hora. Quem tem
`prefers-reduced-motion` não recebe o movimento.

**Ao testar por CDP, chame `Page.bringToFront` antes de medir:** o navegador congela o
`requestAnimationFrame` em aba oculta, e o carrossel parece parado quando na verdade está
funcionando.

## Carrossel de depoimentos

Esse é o TERCEIRO carrossel e funciona diferente dos outros dois: em leque, com o do
centro grande e nítido e os vizinhos menores e apagados. Ele **anda de um em um e para
4,2 s em cada** — a pessoa precisa de tempo para ler a conversa, então scroll contínuo
não serviria aqui.

É infinito por índice circular (`(i - atual + total) % total`), não por duplicação de
imagens como os outros. Tem setas, bolinhas e arraste por toque. Começa com o primeiro
no centro, o segundo à direita e o último à esquerda.

## Os dois carrosséis que deslizam

`#modelos` (páginas de projeto) e `#provas` (anúncios) usam a MESMA função `deslizar()`:
deslizam sozinhos, param só enquanto o dedo está pressionado e voltam ao soltar. Passar o
mouse por cima não para. Os dois têm as imagens duplicadas no HTML — a segunda leva é a
emenda do loop, então **trocar imagem exige trocar nas duas levas**.

O `scroll-snap` foi desligado nos dois: ele briga com o deslize automático e trava o
carrossel em cada item.

## Duas armadilhas que já morderam aqui

**`height:auto` nas imagens de carrossel.** Com `width`/`height` no atributo do `<img>`
e só `max-width:100%` no CSS, o navegador limita a largura mas mantém a altura do
atributo — a imagem estica na vertical. Toda imagem de carrossel precisa de
`width:100%;height:auto`.

**Especificidade em `.num`.** O número do passo é um `<span class="num">` dentro de
`.item-anat`, e `.item-anat span` (0-1-1) vence `.num` (0-1-0) — o número herdava a cor
cinza do texto de apoio e sumia no fundo turquesa. Por isso o seletor é
`.item-anat .num`, não `.num`.

## Lazy loading das imagens

As 4 primeiras amostras do carrossel e a página de exemplo **não** têm `loading="lazy"`,
de propósito: ficam visíveis sem rolar e o lazy só atrasaria a percepção de valor. As 7
últimas do carrossel seguem lazy, porque só entram em cena quando a pessoa arrasta.

Os botões `.ir-oferta` rolam até `#oferta` sem mexer no histórico — isso evita que o
back redirect dispare por engano. O back redirect aponta para `https://micangasdajuh.vercel.app/promo` — link absoluto de
produção, então **no localhost ele leva para o domínio publicado**, não para a cópia local.

## Assets

`assets/hero.webp` e `assets/amostras/*.webp` foram gerados a partir das páginas finais
da coleção com ffmpeg. Para refazer, veja o comando no histórico ou regenere a partir de
`entregaveis/50-brincos-micanga/paginas/entrega/`.

## Bônus

Os 5 bônus prometidos na página **ainda não existem como arquivo**:
Tabela de Conversão de Cores · Guia de Acabamento · Grades em Branco · Como Precificar
e Vender · Lista de Compras da Iniciante. Produzir antes de ligar o tráfego.
