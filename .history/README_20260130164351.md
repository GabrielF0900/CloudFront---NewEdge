# 🚀 NovaEdge - Distribuição Global de Conteúdo Estático com AWS

> Uma jornada prática de arquitetura cloud com Amazon CloudFront, S3 e segurança de borda

---

## 📐 Arquitetura da Solução

![Arquitetura NovaEdge](assets/ArquiteturaDrawio.png)

A solução **NovaEdge** é uma arquitetura moderna e escalável que combina:
- **Amazon S3**: Armazenamento estático seguro e replicado
- **Amazon CloudFront**: Distribuição global via 600+ edge locations
- **Origin Access Control (OAC)**: Segurança de borda sem exposição pública
- **HTTPS com Cache**: Velocidade máxima e conformidade com segurança

---

## 📖 A Jornada: Do Zero à Distribuição Global

### Fase 1️⃣: Começando na Console AWS

Tudo começou na página principal da console AWS. Um novo projeto, uma nova missão. 

![Página inicial AWS Console](assets/01-pagina-inicial.jpeg)

*Aqui, na vastidão da AWS, você percebe que não é coisa de "multinacional" — é coisa de quem quer **entregar conteúdo rápido, seguro e barato** para o mundo todo.*

---

### Fase 2️⃣: Criando o Bucket S3

A próxima parada foi o **Amazon S3** — o serviço de armazenamento de objetos que será a raiz da nossa distribuição.

![Página principal S3](assets/02-pagina-inicial-s3.jpeg)

Cliquei em criar um novo bucket. Mas aqui, querido arquiteto, começam as decisões importantes.

---

### Fase 3️⃣: Configurando o Bucket S3

Na tela de configuração, nomeei o bucket como **`nova-edge-projeto-gabriel-falcao`**.

![Configuração S3](assets/03-configuracao-s3-01.jpeg)

**Por que esse nome?** Excelente pergunta que você provavelmente fez a si mesmo. A resposta é simples: eu queria criar uma nomenclatura que remetesse ao projeto e que preservasse meu nome pessoal contra a **maior exigência da AWS para buckets: UNICIDADE GLOBAL**.

Você leu certo. A partir do momento que criei esse bucket com esse nome, **ninguém mais poderia criar um bucket com esse mesmo nome durante todo o ciclo de vida do bucket**. É como registrar um domínio — quando você o toma, ninguém mais pode usá-lo. Isso me protege, mas também significa que preciso de nomes descritivos e únicos. 🔐

---

### Fase 4️⃣: Segurança em Primeiro Lugar

Agora vem a parte que **todo arquiteto cloud em formação deve se preocupar ESTRITAMENTE**: **Bloqueio de Acesso Público por Padrão**.

![Segurança S3 - Bloqueio de Acesso Público](assets/04-configuracao-s3-seguranca.jpeg)

Essa configuração é **crítica**. O bucket S3 fica protegido contra acessos diretos externos. Você está fechando a porta de entrada para:
- ✋ **Ataques de força bruta** (onde alguém tenta adivinhar credenciais)
- 🚫 **Exposição acidental de dados**
- 🔓 **Acesso não autorizado**

Somente o CloudFront, através de **OAC (Origin Access Control)**, poderá acessar o conteúdo do bucket. É como ter uma porta de garagem que só abre para um veículo específico.

---

### Fase 5️⃣: Bucket S3 Criado com Sucesso! ✅

![S3 Criado](assets/05-s3-criado.jpeg)

*O primeiro passo está completo. Agora temos um lugar seguro para armazenar nosso site estático.*

---

### 💡 Entendo: O que é Edge Location / Ponto de Presença?

Antes de continuar, deixa eu explicar o conceito que torna essa arquitetura tão poderosa:

**Edge Locations** (ou Pontos de Presença) são **pontos físicos espalhados estrategicamente pelo mundo** — desde data centers em São Paulo até pontos de presença na Rússia, Austrália, Singapura, etc.

O propósito deles é **levar conteúdo o mais próximo possível do cliente final**. A velocidade é absurda. Imagine isso:

1. Você hospeda seu site no Brasil (S3 na region us-east-1)
2. Um usuário na Rússia acessa seu site
3. **Se o conteúdo estiver em cache** no edge location próximo à Rússia:
   - CloudFront entrega **IMEDIATAMENTE** 🚀
4. **Se o conteúdo NÃO estiver em cache**:
   - CloudFront faz uma requisição ao S3 (Brasil)
   - S3 envia para CloudFront (Rússia)
   - CloudFront envia para o cliente
   - CloudFront **armazena em cache para o próximo acesso**

Resultado? **Alta disponibilidade + velocidade absurda** = receita para sucesso. Isso é o que as maiores empresas (Netflix, Amazon, Spotify) usam para servir conteúdo para bilhões de pessoas.

---

### Fase 6️⃣: Entrando no CloudFront

Agora vem o recurso que faz a mágica acontecer: **Amazon CloudFront**.

![Página inicial CloudFront](assets/06-pagina-inicial-cloudfront.jpeg)

*O portal para distribuição global está aberto.*

---

### Fase 7️⃣: Escolhendo o Modelo de Preço

Aqui chegamos ao primeiro "speed bump" da jornada. CloudFront oferece dois modelos de precificação.

![Modelo de Preço CloudFront](assets/07-modelo-de-preco-cloudfront.jpeg)

Escolhi o **modelo gratuito** pela razão simples: sou estudante da **Escola da Nuvem**, que tem limite de privilégios em recursos pagos. Porém, como você verá, isso gerou um pequeno problema que pesquisa e determinação resolveram. 😄

Este modelo gratuito oferece benefícios absurdos:
- 🛡️ **WAF (Web Application Firewall)**: Bloqueia injeção SQL, DDoS, IPs maliciosos
- 🔒 **Criptografia HTTPS** por padrão
- 🚀 **10 milhões de requisições gratuitas** por mês
- 📊 **1 TB de saída de dados gratuito** por mês

---

### Fase 8️⃣: Iniciando a Configuração do CloudFront

Comecei a configuração do CloudFront. Nomeei a distribuição como **`NovaEdge-Distribution`** com descrição apropriada.

![Configuração CloudFront](assets/08-cloudfront-configuracao.jpeg)

*Cada campo preenchido é uma decisão de arquitetura.*

---

### Fase 9️⃣: Escolhendo a Origem

Agora vem a pergunta: **O que é Origem?**

![Selecionando tipo de origem](assets/09-selecionando-bucket-no-cloudfront.jpeg)

**Origem** é o local onde seu conteúdo estático está armazenado. É o "ponto de partida" da sua distribuição. No nosso caso, é o bucket S3 que criamos anteriormente.

O CloudFront vai:
1. Solicitar o conteúdo da origem (S3)
2. Colocar em cache nas edge locations
3. Servir para usuários ao redor do mundo

---

### Fase 🔟: Selecionando o Bucket S3

Aqui, vejo a lista de buckets disponíveis.

![Bucket na lista](assets/10-bucket-cloudfront.jpeg)

Seleciono **`nova-edge-projeto-gabriel-falcao`** — aquele que criamos lá atrás.

![Bucket selecionado](assets/11-bucket-selecionado-cloudfront.jpeg)

---

### Fase 1️⃣1️⃣: O Problema do WAF (The Plot Twist 🎬)

Chegou ao ponto onde as coisas ficam interessantes.

![WAF habilitado](assets/12-waf-cloudfront.jpeg)

Na revisão final da configuração:

![Primeira revisão](assets/13-primeira-revisao.jpeg)

**Cliquei em criar e... recebi um erro.**

A AWS tentou habilitar automaticamente o **WAF** (Web Application Firewall) — um recurso pago que está fora dos limites da Escola da Nuvem. O problema? **O modelo de preço "gratuito" que escolhi na verdade habilitava recursos pagos por padrão no novo fluxo de criação do CloudFront.**

Pesquisei e descobri a solução: **existe uma maneira "antiga" de criar o CloudFront** — o modo **"Pay As You Go"** (pague conforme o uso).

---

### Fase 1️⃣2️⃣: O Contorno (Problema Resolvido ✅)

Voltei ao processo de criação do zero.

![Tentativa anterior](assets/14-seguranca-cloudfront.jpeg)

Encontrei a opção de modelo de preço **Pay As You Go**:

![Pay As You Go](assets/15-pay-as-you-go.jpeg)

*Essa é a maneira recomendada para quem está aprendendo AWS.*

Refiz a configuração, e quando cheguei na etapa do WAF:

![WAF na nova interface](assets/16-contorno-01.jpeg)

**EUREKA!** Dessa vez, eu tinha controle total. Desbiliteei o WAF:

![WAF desabilitado](assets/17-contorno-feito.jpeg)

---

### Fase 1️⃣3️⃣: CloudFront Criado com Sucesso! 🎉

![Distribuição criada](assets/18-distribuicao-criada.jpeg)

*Finalmente, a distribuição global está ao vivo.*

---

### Fase 1️⃣4️⃣: Configurando o Arquivo a Distribuir

Porém, o CloudFront precisa saber **qual arquivo servir como raiz**. Fui para as configurações:

![Configuração index.html](assets/19-index-html.jpeg)

Defini **`index.html`** como arquivo padrão — aquele que será servido quando alguém acessar a distribuição.

![Configuração completa](assets/20-configuracao-index-html-feita.jpeg)

---

### Fase 1️⃣5️⃣: A Conexão Crítica - S3 + CloudFront

Aqui vem a parte que faz tudo funcionar: **conectar o S3 ao CloudFront de forma segura**.

Precisei modificar a **Política de Bucket (Bucket Policy)** do S3 para permitir que o CloudFront acesse o conteúdo através de **OAC (Origin Access Control)**.

Fui para as configurações do CloudFront, aba de Origem:

![Configuração de origem](assets/22-politica-cloudfront.jpeg)

Copiei a política OAC sugerida pela AWS (que a tela azul oferece).

Depois, fui ao bucket S3:

![Bucket S3](assets/23-bucket.jpeg)

Acessei a política do bucket:

![Política inicial](assets/24-politica-inicial.jpeg)

Cliquei em editar e vi a política antiga:

![Política antiga (JSON)](assets/25-politica-antiga.jpeg)

Inseri a política do CloudFront (OAC) que copiei anteriormente:

![Nova política com OAC](assets/26-nova-politica.jpeg)

A atualização foi feita:

![Política atualizada](assets/27-atualizacao-politica-feita.jpeg)

**Resultado:** Agora CloudFront pode solicitar conteúdo ao S3, mas **ninguém mais pode acessar o S3 diretamente**. É segurança de borda pura. 🔐

---

### Fase 1️⃣6️⃣: Criando a Página Estática

Agora vem a parte criativa: a página HTML/CSS que será distribuída globalmente.

Gerei uma página estática em HTML e CSS (sim, com ajuda de IA, porque quem tem tempo para escrever HTML hoje em dia? 😄):

![Página criada](assets/29-pagina-criada.jpeg)

Salvei como **`index.html`** — o arquivo que configuramos no CloudFront.

---

### Fase 1️⃣7️⃣: Fazendo Upload para o S3

Fui ao bucket S3 e fiz upload do arquivo:

![Upload em andamento](assets/30-carregando-no-bucket.jpeg)

Arquivo carregado:

![Arquivo no bucket](assets/31-arquivo-carregado.jpeg)

*Agora o S3 tem o conteúdo. CloudFront sabe como distribuir. Falta testar.*

---

### Fase 1️⃣8️⃣: O Momento da Verdade - Testando! 🎯

Peguei o URL da distribuição CloudFront e acessei no navegador:

![Página online](assets/32-pagina-online-globalmente.jpeg)

**E lá estava ela: ONLINE E SENDO DISTRIBUÍDA GLOBALMENTE.** 🚀

Deixa eu detalhar o que está acontecendo aqui:

1. ✅ O site é hospedado no S3 (região Brasil - us-east-1)
2. ✅ CloudFront está distribuindo em 600+ edge locations ao redor do mundo
3. ✅ Quando você acessa de São Paulo: Entrega RÁPIDA (S3 próximo)
4. ✅ Quando você acessa de Tóquio: Se em cache, SUPER RÁPIDO (edge location próximo)
5. ✅ Quando você acessa de Moscou (primeira vez): CloudFront solicita ao S3 (Brasil), coloca em cache em Moscou, entrega
6. ✅ Quando a próxima pessoa acessa de Moscou: IMEDIATAMENTE do cache local

**Isso é o que as grandes empresas (Netflix, Amazon, Spotify, TikTok) fazem para bilhões de usuários.**

---

## 💸 O "Mito" do Preço na Nuvem

Muitas pequenas empresas têm medo da AWS por acharem que é "coisa de multinacional". Mas o projeto **NovaEdge** prova o contrário:

### Amazon CloudFront (Free Tier - Vitalício! 🎁)
- **1 TB de transferência de dados** para a internet por mês ✅
- **10 milhões de requisições HTTP/HTTPS** de forma gratuita ✅
- **Para um site de portfólio** como `gabrielfalcaodacruz.tech`: **Custo = US$ 0,00** 💰

### Amazon S3 (Free Tier - 12 meses)
- **5 GB de armazenamento gratuito** nos primeiros 12 meses
- Um site estático ocupa **alguns MB** no máximo
- Se ultrapassar (improvável): **menos de US$ 0,50/mês** 📦

### Sem Servidores (Serverless)
Ao contrário de:
- ❌ Contratar uma hospedagem tradicional (custo fixo)
- ❌ Ligar uma instância EC2 (cobra por hora, mesmo sem acesso)

**Você só paga se houver tráfego. Se ninguém acessar o site, você não paga quase nada.** ⚡

---

### 🏢 Quem Pode Usar Essa Arquitetura?

A versatilidade dessa solução é o que a torna tão poderosa:

| Perfil | Casos de Uso | Vantagem Principal |
|--------|-------------|-------------------|
| **Pequena Empresa / Autônomo** | Landing Pages, Portfólios, Sites Institucionais | Custo próximo de zero + manutenção nula |
| **Médias Empresas** | E-commerce (Frontend), Blogs, Portais de Notícias | Escalabilidade automática (ex: Black Friday) |
| **Grandes Corporações** | Vídeos, Bibliotecas de Imagens, Apps Web Globais | Baixíssima latência + segurança global |

---

## 🚀 Por Que Essa Arquitetura É Estratégica?

Para um desenvolvedor Full Stack Júnior como eu, saber vender essa arquitetura é um **diferencial enorme** em entrevistas:

> "Eu projetei o NovaEdge para ser custo-eficiente. Em vez de manter um servidor ligado 24/7, usei S3 e CloudFront com OAC. Isso garante:
> - ✅ Performance global (latência baixa em qualquer lugar)
> - ✅ Segurança de borda (sem exposição do S3)
> - ✅ Custo dentro da camada gratuita para baixos volumes
> - ✅ Escalabilidade automática conforme o crescimento real do negócio"

**Conclusão:** Ela é barata o suficiente para uma padaria do bairro usar, mas **robusta o suficiente para suportar milhões de acessos de uma startup unicórnio**. 🦄

---

## 📚 Recursos Utilizados

- ☁️ **Amazon S3** - Armazenamento seguro
- 🌍 **Amazon CloudFront** - Distribuição global  
- 🔐 **Origin Access Control (OAC)** - Segurança de borda
- 📄 **HTTPS** - Comunicação criptografada
- 💾 **Cache Strategy** - Performance máxima

---

## 🎓 Aprendizados Principais

1. **Nomes de bucket devem ser únicos globalmente** - Planeje bem!
2. **Segurança em primeiro lugar** - Bloqueio de acesso público é essencial
3. **CloudFront oferece dois modelos de criação** - "Gratuito" vs "Pay As You Go"
4. **OAC é a forma moderna de conectar S3 + CloudFront** - Mais seguro que Origin Access Identity
5. **Edge Locations mudam tudo** - Latência baixa em qualquer lugar do mundo
6. **Serverless significa custo dinâmico** - Pague apenas pelo que usar

---

## 👤 Autor

**Gabriel Falcão da Cruz**  
Desenvolvedor Full Stack em Formação | Estudante Escola da Nuvem | Aprendiz de Cloud Architecture

🔗 [gabrielfalcaodacruz.tech](https://gabrielfalcaodacruz.tech) - Hospedado com NovaEdge! 🚀

---

**Criado com ❤️ na jornada pela nuvem.**
