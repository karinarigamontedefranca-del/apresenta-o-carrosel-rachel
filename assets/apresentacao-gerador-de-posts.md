# Gerador automático de carrosséis — Rachel Patrocínio
### Material para avaliação da ferramenta

> Valores em dólar são **estimativas** baseadas nas tabelas de preço públicas das APIs e no uso real durante os testes. Os números exatos devem ser conferidos nos painéis da Anthropic e da OpenAI.

---

## 1. O que é

Um site que cria carrosséis de Instagram completos no estilo da Rachel (textos, layout, fontes, cores, legenda e fotos de fundo) a partir de um tema. O que antes era feito à mão, slide por slide, passa a ser gerado em cerca de 1 a 2 minutos.

**Status atual:** o site está **no ar e funcionando**, já foi usado para gerar posts reais de teste (ex.: McFish, Rock in Rio, Nike/vôlei) e já tem biblioteca de posts salvos.

---

## 2. Como funciona (visão geral)

O uso é um passo a passo em 4 etapas:

1. **Tema** — a ferramenta sugere temas em alta (marcas, campanhas, notícias que "todo mundo está comentando", com no máximo 1 a 2 semanas), ou a Rachel digita o próprio tema.
2. **Post** — o sistema gera o carrossel inteiro (em média 6 a 12 slides).
3. **Legenda** — gerada automaticamente, ou a Rachel escreve a dela.
4. **Baixar** — PDF ou ZIP com as imagens prontas.

Tudo fica salvo numa **biblioteca**, onde dá para rever e baixar posts antigos e trocar a imagem de fundo de qualquer slide.

### O que acontece por trás
| Etapa | Quem faz | Para quê |
|---|---|---|
| Buscar temas em alta | Claude (Anthropic) com busca na web | Sugerir pautas atuais, sem repetir as recentes |
| Escrever o carrossel | Claude (Anthropic) | Roteiro, textos, escolha do padrão de post e layout de cada slide |
| Gerar as fotos de fundo | OpenAI (gpt-image-1) | Imagem de fundo de cada slide |
| Montar as imagens finais | Servidor do site | Aplica fontes, cores e posições e gera os PNG |
| Guardar os posts | Supabase | Biblioteca permanente de posts e temas |
| Hospedar o site | Render | Mantém o site no ar |
| Guardar o código | GitHub | Versão do sistema; cada alteração gera novo deploy |

---

## 3. Funcionalidades disponíveis

- Sugestão automática de temas em alta, filtrados para um público generalista, sem repetir temas já sugeridos.
- Campo para a Rachel digitar o próprio tema ou ideia.
- Geração de carrossel completo com 3 formatos de post: **A** (denso/didático), **B** (leve/case visual) e **C** (post-frase, peça única).
- Textos seguindo a lógica de conteúdo da Rachel: contextualização do fato, interpretação estratégica, princípio de branding e generalização para qualquer negócio.
- Identidade visual aplicada automaticamente: fontes Gallient e Montserrat, paleta oficial, @ no topo, tagline no rodapé, negrito cirúrgico.
- Fotos de fundo geradas por IA e escurecidas para dar leitura ao texto.
- Legenda pronta, ou campo para a Rachel escrever a própria.
- Download em **PDF** ou **ZIP** (imagens separadas).
- **Biblioteca** com todos os posts e temas já gerados (nada se perde se o servidor reiniciar).
- **Edição posterior** de cada slide: enviar uma imagem de fundo própria ou pedir para a IA gerar de novo.
- Interface com a identidade visual da Rachel.

---

## 4. Referências de posts e imagens

**Hoje, de três formas diferentes:**

**a) Já embutido no sistema.** Os posts de exemplo da Rachel e o manual de marca foram analisados e transformados em regras fixas: os 3 padrões de post, fontes, tamanhos, cores, posições e lógica de roteiro. É por isso que os posts saem "com a cara dela". Para ensinar um novo estilo ou formato, as regras podem ser atualizadas no código (rápido de fazer, mas passa por alguém técnico).

**b) Referência pontual pelo campo de tema.** Dá para descrever, no campo de tema, a estrutura de um post que ela gostou (ex.: "carrossel com gatilhos variados e uma frase fixa embaixo, no estilo de tal post"). O gerador tenta seguir, mas mantém alguma liberdade de redação. Na prática, funciona bem quando o roteiro é bem detalhado.

**c) Novo: anexar um post de referência (PDF ou prints).** Essa era a melhor candidata a evolução da ferramenta e já foi construída. Os detalhes estão abaixo.

### 4.1 Como funciona
Na etapa **Tema** há um bloco novo: **"Inspirar-se em um post que você gostou"**.

1. A pessoa anexa um **PDF** (até 20 páginas) ou **prints** (PNG, JPG, WEBP; até 10 arquivos de 20 MB cada).
2. A IA **lê o post** (texto e visual) e mostra um resumo do que entendeu: formato, abertura, ritmo, visual, tom, fechamento, assunto, ideia central e pontos abordados.
3. Ao gerar, o sistema cria um **post novo** com a identidade da Rachel (fontes, cores, @, tagline), usando a referência de acordo com o modo escolhido.

| Modo | O que faz |
|---|---|
| **Mesmo assunto, texto novo** (padrão) | Trata do mesmo assunto, da mesma ideia central e dos mesmos pontos do original, mas **reescreve tudo do zero**, com outro gancho de abertura e outras palavras. |
| **Só o formato, assunto diferente** | Aproveita apenas a estrutura e o estilo (nº de slides, ritmo, layout) e muda o assunto. Oferece 3 ideias de assunto para clicar. |

No modo padrão o campo de tema pode ficar **vazio**: o sistema usa o assunto do post enviado. Se a pessoa digitar um tema, ele é combinado com o conteúdo do original.

### 4.2 Como o risco de plágio é reduzido
- A leitura **não guarda as frases do original**: os pontos são resumidos com palavras da própria IA.
- A geração recebe a instrução de não copiar frases, expressões de destaque, trechos nem a ordem de frases, de não copiar fotos ou elementos de marca, e de **não inventar fatos** que não estejam no original ou no tema.
- A identidade visual e as regras do sistema **sempre valem mais** que a referência.
- Ressalva: se o original tem dados ou fatos específicos, a Rachel deve **conferir antes de publicar**.

### 4.3 Proteções técnicas
- Limites de tamanho e de nº de páginas (seguram custo e memória do Render gratuito).
- PDF protegido por senha ou corrompido gera mensagem amigável.
- O texto vindo da IA é limitado em tamanho e mostrado na tela sem executar código.
- Posts **sem referência** continuam **exatamente como antes** (a mensagem enviada à IA é idêntica).

### 4.4 Custo da referência
Estimativas pela tabela pública da Anthropic (Sonnet 4.5: US$ 3 por milhão de tokens de entrada e US$ 15 de saída; Haiku 4.5: US$ 1 e US$ 5). O custo real de cada leitura aparece no site, logo abaixo da leitura.

| Tamanho da referência | Sonnet | Haiku (opcional) |
|---|---|---|
| 3 páginas / prints | ~US$ 0,04 | ~US$ 0,015 |
| 8 páginas (típico) | ~US$ 0,06 | ~US$ 0,02 |
| 20 páginas (limite) | ~US$ 0,12 | ~US$ 0,04 |

| Componente | Custo |
|---|---|
| Leitura da referência (8 páginas, Sonnet) | ~US$ 0,06 |
| Texto e layout do carrossel (Claude) | ~US$ 0,10 a 0,15 |
| Extra da referência na geração | ~US$ 0,004 |
| Fotos de fundo (OpenAI, 6 a 10 fotos) | ~US$ 0,35 a 0,60 |
| **Total** | **~US$ 0,52 a 0,81 (≈ R$ 2,90 a R$ 4,50)** |

Um post **sem** referência custa ~US$ 0,50 a 0,75. A referência acrescenta menos de 10 centavos de dólar (≈ R$ 0,35), porque as fotos são a maior parte do custo.

Com os ~US$ 5 da Anthropic: ~80 leituras de referência (Sonnet, 8 páginas), ~240 leituras (Haiku) ou ~25 posts completos com referência (parte Claude: ~US$ 0,19 cada). O saldo da OpenAI (fotos) tende a acabar primeiro.

**Para reduzir o custo:** usar o modelo Haiku na leitura (~3x mais barato), enviar só as páginas que importam e, no futuro, guardar a leitura para reaproveitar quando a mesma referência for usada várias vezes.

### 4.5 Detalhes técnicos
| Arquivo | Mudança |
|---|---|
| `src/routes/referencia.js` | **Novo.** Rota de upload e análise (`POST /api/referencia/analisar`). |
| `src/lib/anthropic.js` | Leitura da referência (PDF e imagem), cálculo de custo, instruções dos dois modos, limpeza dos dados. |
| `src/routes/generate.js` | Recebe a referência e o modo; usa o assunto do original se o tema estiver vazio. |
| `server.js` | Registra a rota nova. |
| `public/index.html`, `public/app.js`, `public/styles.css` | Bloco de referência, escolha do modo, exibição do resultado e do custo. |
| `README.md` | Documentação da função. |

Variável opcional no Render: `ANALYSIS_MODEL=claude-haiku-4-5-20251001` (leitura cerca de 3x mais barata). Não é preciso mexer em Supabase, OpenAI nem fontes.

---

## 5. Custos

### 5.1 Orçamento investido até agora
| Item | Valor |
|---|---|
| Crédito total colocado | **US$ 15** |
| API da Anthropic (Claude) | US$ 10 (restam cerca de **US$ 5**) |
| API da OpenAI (imagens) | restante do orçamento (poucos dólares) |
| Render (hospedagem) | US$ 0 (plano gratuito) |
| Supabase (biblioteca) | US$ 0 (plano gratuito) |
| GitHub | US$ 0 |

Parte do gasto da Anthropic foi com testes e correções durante a construção (inclusive posts que falharam e foram cobrados), então o custo do dia a dia tende a ser menor que o histórico.

### 5.2 Custo estimado por post (modelo de cobrança: pagar pelo uso)
Não há mensalidade da ferramenta em si. Cada post consome crédito nas duas APIs:

| Componente | Estimativa por post |
|---|---|
| Texto e layout (Claude, ~9 slides) | **~US$ 0,10 a 0,15** |
| Sugestão de temas (cada busca) | ~US$ 0,02 a 0,05 |
| Fotos de fundo (OpenAI, ~US$ 0,06 por imagem em qualidade média) | **~US$ 0,35 a 0,60** (6 a 10 fotos) |
| **Total por post completo** | **~US$ 0,50 a 0,75 (cerca de R$ 3 a R$ 4)** |

Referência: Claude Sonnet cobra aproximadamente US$ 3 por milhão de tokens de entrada e US$ 15 por milhão de tokens de saída. As imagens são a maior parte do custo.

### 5.3 O que o saldo atual rende
- Os ~US$ 5 restantes na Anthropic cobrem algumas dezenas de posts (texto).
- O limitante tende a ser o crédito da OpenAI, porque as fotos custam mais que o texto.
- Estimativa conservadora: **algumas dezenas de posts** com o orçamento atual, o suficiente para uma fase de avaliação.

### 5.4 Formas de reduzir o custo
- Usar a qualidade "baixa" de imagem (cerca de US$ 0,016 por foto), com menos detalhe.
- Gerar fotos só nos slides mais importantes e usar fundos próprios (upload) nos demais.
- Usar um modelo mais barato para a parte de sugestão de temas.
- Migrar o layout para modelos pré-prontos, em que a IA só preenche o texto (reduz bastante o custo de texto). Está mapeado, mas ainda não foi feito.

### 5.5 Custos futuros a considerar
- **Fonte Gallient:** o arquivo usado hoje é uma versão **demo/teste, sem licença comercial**. Antes de publicar posts reais é preciso **comprar a licença comercial** (ex.: Creative Fabrica, DYSA Studio/MyFonts). A Montserrat é gratuita para uso comercial.
- Se o volume crescer, os planos gratuitos do Render (hospedagem) e do Supabase (banco/armazenamento) podem ficar pequenos e exigir plano pago.

---

## 6. Personalização possível

- **Estilo e regras de conteúdo:** tom de voz, estrutura dos posts, vocabulário, quantidade de slides, formato da tagline, tudo ajustável em um único arquivo de regras.
- **Visual:** fontes, tamanhos, cores, margens, posição do @ e da tagline.
- **Novos formatos de post:** é possível criar padrões novos (ex.: o formato "gatilho variável + frase fixa" que a Rachel gostou).
- **Tipos de tema:** ajustar o filtro de sugestões (público, atualidade, categorias).
- **Fundos:** trocar por imagem própria ou regenerar por IA, slide a slide.
- **Legenda:** automática ou manual.

---

## 7. Limitações atuais

- **Texto e layout vêm de IA**, então pode haver pequenas variações de redação. Revisar antes de postar é recomendado. O sistema não garante texto idêntico a um roteiro fixo.
- **Pessoas reais e famosas:** o sistema **nunca gera fotos de pessoas reais** (risco legal e de política). Nesses slides entra um fundo neutro e a Rachel insere a foto real manualmente. Rostos gerados por IA em pessoas genéricas podem parecer artificiais.
- **Fotos de IA** nem sempre acertam de primeira. Dá para regenerar ou enviar outra imagem.
- **Tempo de geração:** cerca de 1 a 2 minutos por post. O plano gratuito do Render "dorme" quando fica parado, então o primeiro acesso do dia pode demorar mais, e há limite de tempo por requisição (~100 s).
- **Atualidade dos temas:** a busca na web é boa, mas a Rachel deve conferir os fatos antes de publicar (a IA pode errar detalhes).
- **Densidade de texto:** já foram feitos ajustes para evitar texto demais e sobreposição, mas ainda vale revisar os slides.
- **Post de referência ainda sem teste real:** a função foi testada com respostas simuladas da IA. No primeiro uso real, conferir se o PDF é lido e se o post sai como esperado (ver seção 4).
- **Chave da Anthropic no Render:** o erro 401 ("invalid x-api-key") precisa ser corrigido criando uma chave nova em `ANTHROPIC_API_KEY`. Sem isso nada do Claude funciona.
- **Acesso e segurança:** a versão atual não tem login/senha por usuário, e quem tiver o link consegue gerar posts e gastar crédito (agora também com leituras de referência). Para uso em equipe, o ideal é adicionar acesso protegido antes de compartilhar amplamente.
- **Licença da fonte Gallient** pendente (seção 5.5).

---

## 8. Como seria o acesso e o uso no dia a dia

1. A Rachel (ou a equipe) abre o link do site no navegador, no computador ou no celular.
2. Escolhe um tema sugerido ou digita o dela.
3. Clica para gerar e aguarda 1 a 2 minutos.
4. Revisa os slides, escolhe legenda automática ou escreve a própria.
5. Baixa em PDF ou ZIP e posta no Instagram.
6. Se quiser mudar uma imagem de fundo, abre o post na **Biblioteca** e troca o slide desejado.
7. Todos os posts e temas ficam salvos na biblioteca para consulta e novo download.

**Custo de operação:** só o crédito consumido nas APIs. A hospedagem e o banco estão em planos gratuitos hoje. Alguém precisa acompanhar o saldo nas contas da Anthropic e da OpenAI.

**Manutenção:** ajustes de estilo e regras são feitos no código (GitHub) e entram no ar automaticamente após o deploy.

---

## 9. Acesso para teste

- **Link do site:** https://geradordecarrosel.onrender.com/
- **Login:** não há. O link é de acesso aberto (ver limitação na seção 7).
- **Sugestão para a avaliação:** gerar 2 a 3 posts de temas diferentes (um tema em alta sugerido pelo sistema, um tema próprio, e um a partir de um post de referência em PDF ou prints) e comparar com os posts manuais.

---

## 10. Próximos passos sugeridos

1. Avaliar a ferramenta na prática com alguns posts de teste.
2. Comprar a licença comercial da fonte Gallient antes de publicar.
3. Renovar a chave da Anthropic no Render e testar o **post de referência** com um PDF real.
4. Adicionar proteção de acesso (login) caso mais pessoas vão usar.
5. Definir o orçamento mensal de crédito das APIs conforme o volume de posts desejado.

