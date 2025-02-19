# Async Domain Flow

O Porquê dessa Arquitetura
📌 Motivação: Por que criar uma nova arquitetura?
Atualmente, a construção de sistemas escaláveis e flexíveis enfrenta três desafios principais:

Código monolítico e acoplado

Muitas arquiteturas acoplam regras de negócio diretamente na camada de infraestrutura, tornando difícil a evolução do sistema.
Sistemas sem separação clara entre domínio e infraestrutura acabam gerando código difícil de testar e manter.
Dificuldade em lidar com eventos e assíncronos

A maioria das arquiteturas tradicionais não lida bem com processos assíncronos e reativos, resultando em bloqueios e problemas de escalabilidade.
A transição para arquiteturas event-driven pode ser complexa sem um modelo bem definido.
Necessidade de integração com múltiplos canais

APIs REST, WebSockets, filas de mensagens, GraphQL e outras interfaces precisam ser suportadas simultaneamente.
Arquiteturas que dependem apenas de um modelo síncrono (ex.: REST puro) se tornam menos flexíveis.

📌 Para resolver esses problemas, uni DDD, Arquitetura Hexagonal, Event-Driven Architecture (EDA) e Agents (inspirado nos Agentes de IA) em uma estrutura chamada Async Domain Flow, que combina separação de responsabilidades, extensibilidade e reatividade.

Creio que consegui unir as melhores partes de cada uma para criar uma Arquitetura mais atualizada e focada em sistemas assíncronos, podendo ser locais ou distribuídos. A Arquitetura abrange tanto um sistema centralizado como descentralizado

## Por que do nome "Async Domain Flow"?

O nome Async Domain Flow reflete os três pilares fundamentais desta arquitetura:

1️⃣ Async (Assíncrono)
O primeiro elemento do nome, Async, enfatiza o uso nativo de eventos e processamento assíncrono no fluxo de execução do sistema. Diferente de arquiteturas tradicionais que operam predominantemente de forma síncrona, o Async Domain Flow prioriza a comunicação baseada em eventos para garantir escalabilidade, desacoplamento e eficiência.

🔹 Por que o foco em assíncrono?

Permite alta escalabilidade sem bloqueios, aproveitando filas de mensagens e eventos distribuídos.
Melhora a resiliência, pois falhas pontuais em um serviço não afetam todo o sistema.
Reduz latência, pois partes do sistema podem processar eventos em paralelo.

📌 Exemplo real:

> No e-commerce, ao processar um pagamento, não é necessário aguardar a resposta do sistema bancário para continuar com a experiência do usuário. Em vez disso, um evento assíncrono como "PagamentoRecebido" pode ser disparado, e os serviços de notificação, estoque e logística podem consumi-lo independentemente.

2️⃣ Domain (Domínio)
O segundo elemento, Domain, vem da filosofia do Domain-Driven Design (DDD). Ele reforça que o núcleo da aplicação é totalmente independente da infraestrutura e das interfaces externas.

🔹 Por que o foco no domínio?

O código de negócio é organizado em agregados e entidades bem definidos.
O domínio é rico e independente, permitindo mudanças sem impactar a infraestrutura.
Aplicamos eventos de domínio, garantindo que as regras de negócio se comuniquem de forma clara e autônoma.
📌 Exemplo real:
Em um sistema de gestão financeira, a lógica para validar um pagamento recorrente deve estar no Domínio, e não espalhada por diferentes camadas. Isso garante reusabilidade, testabilidade e flexibilidade.

3️⃣ Flow (Fluxo)
O último elemento, Flow, representa a fluidez e a orquestração dos eventos e interações dentro do sistema. O Async Domain Flow não é uma arquitetura rígida e linear, mas sim dinâmica e adaptável, onde eventos, comandos e queries fluem entre as camadas sem bloqueios desnecessários.

🔹 Por que o foco no fluxo?

O sistema não é monolítico ou preso a uma única abordagem; ele se adapta dinamicamente às interações.
Eventos fluem naturalmente entre componentes, reduzindo acoplamento e melhorando a observabilidade.
O design promove interoperabilidade entre diferentes partes da aplicação, permitindo evolução contínua.
📌 Exemplo real:
Em um CRM baseado nessa arquitetura, quando um cliente muda seu plano de assinatura, um evento "PlanoAtualizado" pode ser propagado automaticamente para serviços de cobrança, suporte e analytics, sem que nenhum deles precise chamar diretamente os outros.


🎯 Objetivo do Async Domain Flow
A proposta dessa arquitetura é garantir que sistemas sejam modularizados, escaláveis e preparados para eventos assíncronos, mantendo uma organização clara entre camadas de domínio, infraestrutura e orquestração de eventos.

O que queremos alcançar? ✅ Código modular e bem definido → Separação clara entre domínio e infraestrutura.
✅ Facilidade para evoluir → Suporte a novos casos de uso sem refatorações gigantes.
✅ Escalabilidade nativa → Uso de eventos para distribuir carga e processar tarefas assíncronas.
✅ Independência da tecnologia → Capacidade de trocar banco de dados, API ou mensageria sem afetar o domínio.
✅ Integração com múltiplos canais → API REST, WebSockets, filas, etc., sem reescrever regras de negócio.


🔀 A Fusão das Três Arquiteturas
O Async Domain Flow nasce da combinação dos melhores conceitos de DDD, Arquitetura Hexagonal e Event-Driven Architecture (EDA).



Essa fusão resolve problemas comuns de arquiteturas tradicionais:

Evita acoplamento entre domínio e infraestrutura

O domínio é puro e independente.
A infraestrutura implementa apenas adapters para persistência e comunicação externa.
Facilita a escalabilidade e o processamento assíncrono

Eventos permitem distribuir tarefas sem bloqueios.
Filas e streamings podem ser usados para desacoplar serviços.
Permite múltiplas interfaces sem modificar o domínio

REST, WebSockets, mensageria e GraphQL podem coexistir sem impactar as regras de negócio.

| Arquitetura                          | O que traz para o Async Domain Flow?                                      |
|--------------------------------------|---------------------------------------------------------------------------|
| **Domain-Driven Design (DDD)**       | Define um **modelo rico** com entidades, agregados e eventos de domínio.  |
| **Arquitetura Hexagonal (Ports & Adapters)** | Garante que o domínio seja **independente de tecnologia** e **infraestrutura**. |
| **Event-Driven Architecture (EDA)**  | Introduz **eventos como primeira classe**, garantindo **reatividade e escalabilidade**. |


## 📐 Estrutura do Async Domain Flow
A arquitetura é dividida em cinco camadas, cada uma com uma responsabilidade bem definida:

1️⃣ Interface Layer (Portas de Entrada e Saída)
Define múltiplas interfaces para interagir com o sistema (API, CLI, WebSockets, mensageria).
Converte formatos de entrada e saída.
Aplica autenticação e autorização.

2️⃣ Application Layer (Casos de Uso e Orquestração)
Orquestra as chamadas ao Domínio e gerencia fluxos de aplicação.
Manipula eventos assíncronos e chamadas síncronas.
Aplica validações e fluxos de trabalho.

3️⃣ Domain Layer (Núcleo de Negócio)
Define entidades, agregados, serviços de domínio e eventos.
Independente de infraestrutura e frameworks externos.
Modela regras de negócio puras.

4️⃣ Infrastructure Layer (Implementações de Adapters)
Implementa bancos de dados, APIs externas e mensageria.
Contém repositórios concretos e serviços externos.
Serve apenas como suporte ao domínio.

5️⃣ Agents Layer (Orquestração Reativa de Domínios)
Processa eventos internos e externos.
Garante baixa acoplamento e resiliência.
Permite integração com outros sistemas via eventos.


## 🚀 Como o Async Domain Flow Melhora o Desenvolvimento?
1. Escalabilidade Natural
Com eventos nativos, podemos distribuir o processamento facilmente, melhorando performance e resiliência.
Por exemplo, um serviço de pagamento pode processar transações em workers assíncronos, sem bloquear a aplicação principal.
2. Facilidade de Manutenção e Evolução
A separação de camadas permite que mudanças sejam feitas sem afetar todo o sistema.
Exemplo: Se um novo canal (GraphQL) for adicionado, basta criar um novo adapter na Interface Layer, sem alterar regras de negócio.
3. Testabilidade
O Domain Layer é 100% testável sem dependências externas, pois não acessa banco de dados ou APIs diretamente.
Isso melhora a qualidade do código e reduz tempo de desenvolvimento.
4. Facilidade de Integração
Como a arquitetura já é event-driven, integrações com sistemas externos (ERP, CRM, gateways de pagamento) ocorrem naturalmente através de eventos.
Exemplo: Um sistema de e-commerce pode publicar um evento "PedidoCriado", que é consumido por múltiplos serviços (pagamento, estoque, logística).


📊 Comparação com Arquiteturas Tradicionais
Aqui está a comparação das arquiteturas e o que o Async Domain Flow melhora:

| Característica           | DDD                         | Hexagonal                     | Event-Driven                 | Async Domain Flow (Novo)     |
|-------------------------|----------------------------|------------------------------|------------------------------|------------------------------|
| 📦 **Separação de Domínio** | ✅ Sim                      | ✅ Sim                        | ⚠️ Parcial (foco em eventos)  | ✅ Melhorada, com domínio reativo |
| 🔄 **Independência de Infra** | ⚠️ Depende de aplicação    | ✅ Sim                        | ✅ Sim                        | ✅ Sim (infra apenas na camada correta) |
| 🔀 **Eventos Assíncronos**  | ⚠️ Manual                  | ⚠️ Pouco usado                | ✅ Sim                        | ✅ Nativo e centralizado |
| 🚀 **Escalabilidade**      | ⚠️ Média                    | ⚠️ Média                      | ✅ Alta                        | ✅ Alta e otimizada |


## Layers

### Interface Layer (Portas de Entrada e Saída)
A Interface Layer é a camada responsável por receber e enviar dados para o mundo externo. Ela representa as "Portas" na Arquitetura Hexagonal, permitindo que múltiplos canais se comuniquem com a aplicação de forma desacoplada.

Essa camada não contém regras de negócio — apenas transforma, valida e direciona requisições para a Application Layer, garantindo que a aplicação possa ser exposta em diversos formatos sem modificar o domínio.

📌 Características Principais
✅ Multicanal → Suporte para APIs, WebSockets, CLI, Mensageria, etc.
✅ Independente da Regra de Negócio → Apenas recebe e encaminha dados.
✅ Conversão de Formato → Adapta a entrada/saída para o formato necessário.
✅ Validação e Autenticação → Verifica permissões antes de processar a requisição.

📍 Exemplos do Mundo Real
Aqui estão diferentes implementações da Interface Layer e como elas se aplicam em sistemas reais:

1️⃣ API REST para E-commerce
📌 Cenário: Uma loja virtual possui uma API REST para permitir que clientes façam pedidos.

📜 Exemplo de Endpoints (Express.js / NestJS):

```ts
@Post('/orders')
async createOrder(@Body() orderDto: CreateOrderDto) {
    return this.orderService.createOrder(orderDto);
}
```


✅ Porta de Entrada: Endpoint /orders recebe uma requisição POST.
✅ Conversão de Formato: JSON recebido é convertido em CreateOrderDto.
✅ Encaminhamento: O DTO é passado para a camada de aplicação.

2️⃣ WebSockets para Aplicativos de Mensagens
📌 Cenário: Um sistema de chat precisa de comunicação em tempo real via WebSockets.

📜 Exemplo de Implementação (Socket.IO + NestJS):

```ts
@WebSocketGateway()
export class ChatGateway {
  @SubscribeMessage('message')
  handleMessage(@MessageBody() data: ChatMessageDto): void {
    this.chatService.processMessage(data);
  }
}
```

✅ Porta de Entrada: Evento 'message' escutado via WebSockets.
✅ Encaminhamento: Mensagem validada e enviada para a camada de aplicação.
✅ Resposta Assíncrona: Mensagem pode ser salva e retransmitida para clientes.

3️⃣ CLI (Command Line Interface) para DevOps
📌 Cenário: Um desenvolvedor precisa rodar um comando para gerar relatórios diretamente no terminal.

📜 Exemplo de Comando CLI (Node.js / Commander.js):


```ts
program
  .command('generate-report <month>')
  .description('Gera um relatório de vendas para o mês especificado')
  .action((month) => {
    reportService.generate(month);
  });

program.parse(process.argv);
```

✅ Porta de Entrada: Usuário executa node cli.js generate-report 01.
✅ Conversão de Formato: O argumento "01" é interpretado como month.
✅ Encaminhamento: Passa o comando para a Application Layer para gerar o relatório.

4️⃣ Mensageria com RabbitMQ (Fila de Processamento)
📌 Cenário: Um sistema de pagamentos processa transações assíncronas usando filas.

📜 Exemplo de Consumer RabbitMQ (amqplib - Node.js):


```ts
channel.consume('payments', (msg) => {
  const paymentData = JSON.parse(msg.content.toString());
  paymentService.processPayment(paymentData);
});
```

✅ Porta de Entrada: Uma mensagem é recebida na fila payments.
✅ Conversão de Formato: O JSON é convertido para um objeto paymentData.
✅ Encaminhamento: A mensagem é enviada para o serviço de pagamentos.

5️⃣ GraphQL para APIs mais Flexíveis
📌 Cenário: Uma API GraphQL permite que um cliente solicite apenas os dados necessários.

📜 Exemplo de Resolver GraphQL (NestJS + Apollo):


```ts
@Resolver(() => Order)
export class OrderResolver {
  @Query(() => Order)
  async getOrder(@Args('id') id: string) {
    return this.orderService.getOrderById(id);
  }
}
```

✅ Porta de Entrada: Cliente faz uma query getOrder(id: "123").
✅ Conversão de Formato: O id recebido é convertido para uma string.
✅ Encaminhamento: A requisição é passada para a camada de aplicação.

📌 Resumo
A Interface Layer permite que diferentes interfaces de entrada (REST, WebSockets, CLI, Mensageria, GraphQL) acessem a aplicação sem interferir no núcleo do sistema.

Essa camada facilita:

Adição de novos canais de comunicação sem modificar regras de negócio.
Troca de tecnologias sem afetar o Domain Layer.
Validação e autenticação desacopladas da aplicação.

### Application Layer no Async Domain Flow
📌 Orquestração e Casos de Uso no CRM de WhatsApp

A Application Layer é a responsável por coordenar os casos de uso do sistema. Ela faz a ponte entre a Interface Layer (entrada de dados) e o Domain Layer (regra de negócio), garantindo que as operações sejam executadas da maneira correta.

📌 Função da Application Layer
Orquestração: Chama os serviços do Domínio e Infraestrutura, combinando suas funcionalidades.
Fluxos de Negócio: Implementa a lógica de casos de uso específicos, sem modificar as regras de negócio do Domínio.
Interação com Eventos: Escuta e publica eventos para processamento assíncrono.
Validação e Segurança: Antes de chamar o domínio, pode aplicar regras de acesso e validação.
📍 Estrutura no CRM de WhatsApp
Nosso CRM de WhatsApp possui dois principais domínios:

Vendas/Pedidos/Estoque
Mensagens/Chat/Tickets
Cada domínio tem casos de uso que devem ser coordenados na Application Layer.

1️⃣ Exemplo: Gerenciamento de Pedidos e Estoque
Cenário:

O cliente envia uma mensagem no WhatsApp pedindo um produto.
O sistema precisa verificar estoque, registrar o pedido e confirmar a compra.

```ts
import { Injectable } from '@nestjs/common';
import { OrderRepository } from '../repositories/order.repository';
import { StockService } from '../services/stock.service';
import { EventEmitter2 } from '@nestjs/event-emitter';

@Injectable()
export class CreateOrderUseCase {
  constructor(
    private readonly orderRepository: OrderRepository,
    private readonly stockService: StockService,
    private readonly eventEmitter: EventEmitter2,
  ) {}

  async execute(customerId: string, productId: string, quantity: number) {
    // 1️⃣ Verifica se o produto está disponível
    const available = await this.stockService.checkStock(productId, quantity);
    if (!available) {
      throw new Error('Produto sem estoque');
    }

    // 2️⃣ Cria o pedido
    const order = await this.orderRepository.createOrder(customerId, productId, quantity);

    // 3️⃣ Dispara evento assíncrono
    this.eventEmitter.emit('order.created', { orderId: order.id });

    return order;
  }
}
```

✅ Orquestração do fluxo: Valida estoque → Cria pedido → Dispara evento.
✅ Evita regras de negócio acopladas: Cada serviço tem sua responsabilidade.
✅ Eventos permitem escalabilidade: Outras partes do sistema podem reagir ao pedido.

2️⃣ Exemplo: Processamento de Mensagens no Chat
Cenário:

Um cliente envia uma mensagem no WhatsApp.
O sistema precisa criar um ticket de atendimento e armazenar a mensagem.

```ts
import { Injectable } from '@nestjs/common';
import { TicketRepository } from '../repositories/ticket.repository';
import { MessageRepository } from '../repositories/message.repository';
import { EventEmitter2 } from '@nestjs/event-emitter';

@Injectable()
export class ProcessMessageUseCase {
  constructor(
    private readonly ticketRepository: TicketRepository,
    private readonly messageRepository: MessageRepository,
    private readonly eventEmitter: EventEmitter2,
  ) {}

  async execute(chatId: string, customerId: string, text: string) {
    // 1️⃣ Cria um ticket se necessário
    let ticket = await this.ticketRepository.findOpenTicket(customerId);
    if (!ticket) {
      ticket = await this.ticketRepository.createTicket(customerId);
    }

    // 2️⃣ Salva a mensagem
    const message = await this.messageRepository.createMessage(chatId, customerId, text);

    // 3️⃣ Dispara evento para notificações/integrações
    this.eventEmitter.emit('message.received', { messageId: message.id });

    return message;
  }
}
```

✅ Separa lógica de negócios e orquestração.
✅ Verifica e cria tickets automaticamente.
✅ Eventos permitem automação: Notificações, análises, respostas automáticas.

3️⃣ Exemplo: Atualização do Estoque Após um Pedido
Cenário:

Quando um pedido é criado, precisamos atualizar o estoque.
Esse fluxo deve ser assíncrono para evitar lentidão na compra.

```ts
import { Injectable, OnModuleInit } from '@nestjs/common';
import { EventEmitter2, OnEvent } from '@nestjs/event-emitter';
import { StockService } from '../services/stock.service';

@Injectable()
export class OrderEventHandler {
  constructor(private readonly stockService: StockService) {}

  @OnEvent('order.created')
  async handleOrderCreated(event: { orderId: string }) {
    console.log(`Atualizando estoque para o pedido: ${event.orderId}`);

    await this.stockService.decreaseStock(event.orderId);
  }
}
```

✅ O estoque é atualizado apenas quando o pedido já foi criado.
✅ Processo assíncrono evita travamentos.
✅ Facilidade de escalabilidade: Podemos adicionar novos consumidores sem alterar o código principal.

### Domain Layer no Async Domain Flow
📌 Modelagem do Domínio no CRM de WhatsApp

A Domain Layer é o coração da arquitetura Async Domain Flow. Ela contém toda a lógica de negócio, garantindo que o sistema seja modular, testável e independente de infraestrutura.

📌 Função da Domain Layer
Modelagem do Domínio → Define Entidades, Objetos de Valor e Agregados.
Regra de Negócio Pura → Nenhuma dependência de infraestrutura ou tecnologia externa.
Eventos de Domínio → Comunicação entre agregados sem acoplamento direto.
Repositórios como Interfaces → Apenas contratos, sem implementações concretas.
📍 Estrutura no CRM de WhatsApp
Nosso CRM de WhatsApp possui dois domínios principais:

Vendas/Pedidos/Estoque
Mensagens/Chat/Tickets
Cada um contém Entidades, Objetos de Valor, Eventos de Domínio e Repositórios.

1️⃣ Exemplo: Modelagem do Pedido (Sales Domain)
📌 Cenário:

Cada pedido possui um cliente, produtos, valor total e status.
O status pode mudar conforme o fluxo (ex.: "pendente", "pago", "enviado").
O pedido precisa emitir um evento quando criado.

📜 Implementação da Entidade Order:
```ts
export class Order {
  constructor(
    public readonly id: string,
    public readonly customerId: string,
    public readonly items: OrderItem[],
    public status: OrderStatus,
    public readonly totalAmount: number
  ) {
    this.validate();
  }

  // 1️⃣ Validação de negócio
  private validate() {
    if (!this.customerId) {
      throw new Error('Cliente é obrigatório');
    }
    if (this.items.length === 0) {
      throw new Error('Pedido deve ter pelo menos um item');
    }
    if (this.totalAmount <= 0) {
      throw new Error('Total do pedido deve ser maior que zero');
    }
  }

  // 2️⃣ Atualiza o status do pedido
  updateStatus(newStatus: OrderStatus) {
    this.status = newStatus;
  }
}

// Enum para Status do Pedido
export enum OrderStatus {
  PENDING = 'PENDING',
  PAID = 'PAID',
  SHIPPED = 'SHIPPED',
}

// Objeto de Valor para Itens do Pedido
export class OrderItem {
  constructor(
    public readonly productId: string,
    public readonly quantity: number,
    public readonly price: number
  ) {
    if (quantity <= 0) throw new Error('Quantidade deve ser positiva');
    if (price <= 0) throw new Error('Preço deve ser positivo');
  }
}
```

✅ Regra de negócio encapsulada na entidade (ex.: validação e atualização de status).
✅ Uso de Objetos de Valor (OrderItem) para garantir consistência.
✅ Sem dependências externas → Código puro e testável.

2️⃣ Exemplo: Modelagem do Ticket de Suporte (Chat Domain)
📌 Cenário:

Cada ticket representa um atendimento de suporte.
Ele pode estar aberto ou fechado.
Quando fechado, não pode mais receber mensagens.


📜 Implementação da Entidade Ticket:
```ts
export class Ticket {
  constructor(
    public readonly id: string,
    public readonly customerId: string,
    public readonly status: TicketStatus,
    public readonly createdAt: Date = new Date(),
    public readonly messages: Message[] = []
  ) {}

  // 1️⃣ Adiciona mensagem ao ticket
  addMessage(message: Message) {
    if (this.status === TicketStatus.CLOSED) {
      throw new Error('Não é possível adicionar mensagem a um ticket fechado');
    }
    this.messages.push(message);
  }

  // 2️⃣ Fecha o ticket
  close() {
    if (this.status === TicketStatus.CLOSED) {
      throw new Error('Ticket já está fechado');
    }
    this.status = TicketStatus.CLOSED;
  }
}

// Enum para Status do Ticket
export enum TicketStatus {
  OPEN = 'OPEN',
  CLOSED = 'CLOSED',
}

// Entidade Mensagem
export class Message {
  constructor(
    public readonly id: string,
    public readonly chatId: string,
    public readonly senderId: string,
    public readonly content: string,
    public readonly timestamp: Date = new Date()
  ) {
    if (!content) throw new Error('Mensagem não pode estar vazia');
  }
}
```

✅ Encapsula regras de negócio dentro das entidades.
✅ Garante consistência → Tickets fechados não podem receber novas mensagens.
✅ Evita acoplamento → Apenas manipula os dados necessários.

3️⃣ Exemplo: Eventos de Domínio
📌 Cenário:

Quando um pedido é criado, outros serviços podem precisar saber disso (ex.: notificações, logística).
Para evitar acoplamento direto, emitimos um evento de domínio.

📜 Implementação do Evento de Domínio:
```ts
export class OrderCreatedEvent {
  constructor(public readonly orderId: string, public readonly customerId: string) {}
}
```

📌 Agora, dentro da entidade Order, disparamos o evento:

```ts
import { EventEmitter2 } from '@nestjs/event-emitter';

export class Order {
  constructor(
    public readonly id: string,
    public readonly customerId: string,
    public readonly items: OrderItem[],
    public status: OrderStatus,
    public readonly totalAmount: number,
    private readonly eventEmitter: EventEmitter2 // Injetamos o EventEmitter
  ) {
    this.validate();
    this.eventEmitter.emit('order.created', new OrderCreatedEvent(this.id, this.customerId));
  }
}
```

✅ Baixo acoplamento → Nenhum código precisa chamar serviços externos diretamente.
✅ Escalabilidade → Novos serviços podem ouvir esse evento sem modificar a entidade Order.
✅ Alta coesão → O evento faz parte do domínio e não da infraestrutura.

4️⃣ Exemplo: Contratos de Repositórios
📌 Por que usar interfaces?

O Domínio não deve depender de bancos de dados específicos.
O repositório apenas define a estrutura, e a implementação ocorre na Infra Layer.

📜 Interface para Repositório de Pedidos:
```ts
export interface OrderRepository {
  createOrder(order: Order): Promise<Order>;
  findById(orderId: string): Promise<Order | null>;
  findByCustomer(customerId: string): Promise<Order[]>;
}
```
📌 Como isso ajuda?

No domínio, apenas chamamos os métodos do repositório sem saber a implementação.
Podemos trocar de banco de dados sem alterar a Domain Layer.
Fácil de testar, pois podemos usar mocks dos repositórios.
📌 Resumo: Vantagens da Domain Layer
✅ Código independente de infraestrutura (Banco de dados, API, framework).
✅ Regras de negócio centralizadas → Evita lógica espalhada pelo sistema.
✅ Facilidade para testes → Código puro sem dependências externas.
✅ Baixo acoplamento com a Application Layer → Comunicação via eventos.
✅ Facilidade para escalar → Podemos adicionar serviços sem mudar o código existente.

### Infrastructure Layer no Async Domain Flow
📌 Implementação com Kafka no CRM de WhatsApp

A Infrastructure Layer é responsável por fornecer implementações concretas dos contratos definidos no domínio e na camada de aplicação. Aqui, implementamos repositórios para persistência e adapters para integração com sistemas externos.

No Async Domain Flow, essa camada: ✅ Implementa Repositórios → Concretiza a persistência (Banco de Dados, Cache).
✅ Gerencia Adapters → Comunicação com APIs externas, mensageria, notificações.
✅ Orquestra Mensageria → Envia e consome mensagens do Kafka de forma assíncrona.

📍 Kafka no CRM de WhatsApp
Nosso CRM tem dois domínios principais:

Vendas/Pedidos/Estoque → Eventos de pedidos, atualização de estoque.
Mensagens/Chat/Tickets → Processamento de mensagens em tempo real.
Usaremos o Apache Kafka para processar eventos assíncronos, garantindo que o sistema seja escalável e resiliente.

1️⃣ Produzindo Mensagens no Kafka (Pedidos)
📌 Cenário:

Quando um pedido é criado, queremos publicar um evento order.created.
Outros serviços (ex.: estoque, notificações) podem consumir esse evento.

📜 Producer Kafka para eventos de Pedido:
```ts
import { Injectable } from '@nestjs/common';
import { Kafka } from 'kafkajs';

@Injectable()
export class OrderKafkaProducer {
  private kafka = new Kafka({
    clientId: 'crm-whatsapp',
    brokers: ['localhost:9092'],
  });

  private producer = this.kafka.producer();

  async sendOrderCreatedEvent(orderId: string, customerId: string) {
    await this.producer.connect();

    await this.producer.send({
      topic: 'order.created',
      messages: [
        {
          key: orderId,
          value: JSON.stringify({ orderId, customerId, timestamp: new Date() }),
        },
      ],
    });

    console.log(`📨 Evento 'order.created' enviado para Kafka!`);
    await this.producer.disconnect();
  }
}
```

📌 O que acontece aqui?
✅ Conectamos ao Kafka e enviamos um evento order.created.
✅ O evento é publicado com chave única (orderId) para rastreamento.
✅ Outros serviços podem consumir esse evento sem acoplamento direto.

2️⃣ Consumindo Mensagens no Kafka (Estoque)
📌 Cenário:

Quando um pedido é criado, o serviço de estoque deve atualizar a quantidade dos produtos.
O consumo desse evento será feito de forma assíncrona.
📜 Consumer Kafka para atualizar Estoque:
```ts
import { Injectable, OnModuleInit } from '@nestjs/common';
import { Kafka } from 'kafkajs';
import { StockService } from '../services/stock.service';

@Injectable()
export class OrderKafkaConsumer implements OnModuleInit {
  private kafka = new Kafka({
    clientId: 'crm-whatsapp',
    brokers: ['localhost:9092'],
  });

  private consumer = this.kafka.consumer({ groupId: 'stock-service' });

  constructor(private readonly stockService: StockService) {}

  async onModuleInit() {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic: 'order.created', fromBeginning: true });

    await this.consumer.run({
      eachMessage: async ({ message }) => {
        const eventData = JSON.parse(message.value.toString());
        console.log(`📩 Evento 'order.created' recebido! Atualizando estoque...`);

        await this.stockService.decreaseStock(eventData.orderId);
      },
    });
  }
}
```

📌 O que acontece aqui?
✅ O serviço de estoque escuta eventos order.created e age automaticamente.
✅ Totalmente assíncrono → O pedido pode ser criado sem esperar a atualização de estoque.
✅ Resiliência → Se o serviço cair, ele pode consumir os eventos antigos ao reiniciar (fromBeginning: true).

3️⃣ Processando Mensagens do Chat via Kafka
📌 Cenário:

Quando um usuário envia uma mensagem no WhatsApp, o sistema processa e armazena a mensagem.
Esse evento pode ser usado para respostas automáticas, analytics e integrações.
📜 Producer Kafka para mensagens do chat:
```ts
import { Injectable } from '@nestjs/common';
import { Kafka } from 'kafkajs';

@Injectable()
export class ChatKafkaProducer {
  private kafka = new Kafka({
    clientId: 'crm-whatsapp',
    brokers: ['localhost:9092'],
  });

  private producer = this.kafka.producer();

  async sendMessageReceivedEvent(chatId: string, customerId: string, message: string) {
    await this.producer.connect();

    await this.producer.send({
      topic: 'chat.message.received',
      messages: [
        {
          key: chatId,
          value: JSON.stringify({ chatId, customerId, message, timestamp: new Date() }),
        },
      ],
    });

    console.log(`📨 Evento 'chat.message.received' enviado para Kafka!`);
    await this.producer.disconnect();
  }
}
```

📌 O que acontece aqui?
✅ Cada mensagem enviada no chat gera um evento chat.message.received.
✅ O evento pode ser processado por múltiplos serviços (ex.: IA de respostas, analytics).
✅ Permite escalar o sistema de chat sem afetar o tempo de resposta do usuário.

📜 Consumer Kafka para armazenar mensagens:
```ts
import { Injectable, OnModuleInit } from '@nestjs/common';
import { Kafka } from 'kafkajs';
import { MessageRepository } from '../repositories/message.repository';

@Injectable()
export class ChatKafkaConsumer implements OnModuleInit {
  private kafka = new Kafka({
    clientId: 'crm-whatsapp',
    brokers: ['localhost:9092'],
  });

  private consumer = this.kafka.consumer({ groupId: 'chat-service' });

  constructor(private readonly messageRepository: MessageRepository) {}

  async onModuleInit() {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic: 'chat.message.received', fromBeginning: false });

    await this.consumer.run({
      eachMessage: async ({ message }) => {
        const eventData = JSON.parse(message.value.toString());
        console.log(`📩 Nova mensagem recebida: ${eventData.message}`);

        await this.messageRepository.saveMessage(eventData.chatId, eventData.customerId, eventData.message);
      },
    });
  }
}
```

📌 O que acontece aqui?
✅ O serviço de mensagens processa novos eventos chat.message.received.
✅ Cada mensagem é salva no banco de dados sem bloquear outras operações.
✅ Escalabilidade → Podemos rodar múltiplos consumidores para processar mensagens em paralelo.

📌 Resumo: Vantagens da Infrastructure Layer com Kafka
✅ Desacoplamento completo → Serviços comunicam-se via eventos sem chamar diretamente outros serviços.
✅ Escalabilidade massiva → Múltiplos consumidores podem processar mensagens simultaneamente.
✅ Resiliência → Se um serviço cair, ele pode consumir eventos ao reiniciar.
✅ Performance otimizada → O processamento ocorre de forma assíncrona e distribuída.

1️⃣ MongoDB Generic Repository
📌 O que esse repositório faz?

Implementa CRUD genérico para qualquer entidade.
Usa Soft Delete ao invés de remoção direta.
Usa interfaces para garantir que a persistência seja desacoplada do domínio.
📜 Implementação do Repositório Genérico (MongoDBGenericRepository.ts):
```ts
import { Collection, ObjectId, Filter, IndexDescription, OptionalId } from "mongodb";
import { IGenericRepository } from "../interfaces/IGenericRepository";

export abstract class MongoDBGenericRepository<T extends object> implements IGenericRepository<T> {
  protected collection: Collection<T>;

  constructor(collection: Collection<T>) {
    this.collection = collection;
  }

  async insertOne(entity: T): Promise<void> {
    await this.collection.insertOne(entity as OptionalId<T>);
  }

  async findOne(filter: Filter<T>): Promise<T | null> {
    return await this.collection.findOne({ ...filter, deletedAt: { $exists: false } });
  }

  async findAll(filter: Filter<T> = {}): Promise<T[]> {
    return await this.collection.find({ ...filter, deletedAt: { $exists: false } }).toArray();
  }

  async findById(id: string): Promise<T | null> {
    return await this.collection.findOne({ _id: new ObjectId(id), deletedAt: { $exists: false } });
  }

  async updateOne(id: string, updateData: Partial<T>): Promise<void> {
    await this.collection.updateOne({ _id: new ObjectId(id), deletedAt: { $exists: false } }, { $set: updateData });
  }

  async deleteOne(id: string): Promise<boolean> {
    const result = await this.collection.deleteOne({ _id: new ObjectId(id) });
    return result.deletedCount > 0;
  }

  async softDelete(id: string): Promise<boolean> {
    const result = await this.collection.updateOne(
      { _id: new ObjectId(id), deletedAt: { $exists: false } },
      { $set: { deletedAt: new Date(), active: false } }
    );
    return result.modifiedCount > 0;
  }

  async insertMany(entities: T[]): Promise<void> {
    await this.collection.insertMany(entities as OptionalId<T>[]);
  }

  async createIndexes(indexes: IndexDescription[]): Promise<void> {
    await this.collection.createIndexes(indexes);
  }
}
```

✅ CRUD desacoplado para qualquer entidade.
✅ Soft Delete para evitar remoção permanente.
✅ Interface genérica para uso em qualquer módulo.

2️⃣ Adapter de Pagamento (MercadoPago e Stripe)
📌 O que esse adapter faz?

Usa uma única interface para dois serviços de pagamento.
Implementa os métodos processar pagamento, capturar pagamento e reembolsar.
📜 Interface para Pagamentos (IPaymentGateway.ts):
```ts
export interface IPaymentGateway {
  processPayment(amount: number, currency: string, source: string): Promise<string>;
  capturePayment(paymentId: string): Promise<boolean>;
  refundPayment(paymentId: string): Promise<boolean>;
}
```

📜 Implementação do Adapter (PaymentGateway.ts):
```ts
import { IPaymentGateway } from "../interfaces/IPaymentGateway";
import { MercadoPagoService } from "../services/MercadoPagoService";
import { StripeService } from "../services/StripeService";

export class PaymentGateway implements IPaymentGateway {
  private mercadoPago: MercadoPagoService;
  private stripe: StripeService;

  constructor() {
    this.mercadoPago = new MercadoPagoService();
    this.stripe = new StripeService();
  }

  async processPayment(amount: number, currency: string, source: string): Promise<string> {
    if (currency === "BRL") {
      return this.mercadoPago.createPayment(amount, currency, source);
    } else {
      return this.stripe.createCharge(amount, currency, source);
    }
  }

  async capturePayment(paymentId: string): Promise<boolean> {
    if (paymentId.startsWith("mp_")) {
      return this.mercadoPago.capturePayment(paymentId);
    } else {
      return this.stripe.captureCharge(paymentId);
    }
  }

  async refundPayment(paymentId: string): Promise<boolean> {
    if (paymentId.startsWith("mp_")) {
      return this.mercadoPago.refundPayment(paymentId);
    } else {
      return this.stripe.refundCharge(paymentId);
    }
  }
}
```

✅ Um único adapter para MercadoPago e Stripe.
✅ A lógica decide qual serviço usar baseada no tipo de moeda ou ID do pagamento.
✅ Baixo acoplamento → Se quisermos trocar de provedor, basta alterar a implementação interna.

3️⃣ Adapter para Envio de Mensagens (WhatsApp, Telegram, Facebook e Instagram)
📌 O que esse adapter faz?

Usa uma única interface para quatro serviços de mensagem.
Implementa envio de mensagens e recebimento.
📜 Interface para Mensagens (IMessageGateway.ts):
```ts
export interface IMessageGateway {
  sendMessage(chatId: string, message: string): Promise<void>;
  receiveMessage(): Promise<void>;
}
```

📜 Implementação do Adapter (MessagingGateway.ts):
```ts
import { IMessageGateway } from "../interfaces/IMessageGateway";
import { WhatsAppService } from "../services/WhatsAppService";
import { TelegramService } from "../services/TelegramService";
import { FacebookService } from "../services/FacebookService";
import { InstagramService } from "../services/InstagramService";

export class MessagingGateway implements IMessageGateway {
  private whatsapp: WhatsAppService;
  private telegram: TelegramService;
  private facebook: FacebookService;
  private instagram: InstagramService;

  constructor() {
    this.whatsapp = new WhatsAppService();
    this.telegram = new TelegramService();
    this.facebook = new FacebookService();
    this.instagram = new InstagramService();
  }

  async sendMessage(chatId: string, message: string): Promise<void> {
    if (chatId.startsWith("wa_")) {
      await this.whatsapp.sendMessage(chatId, message);
    } else if (chatId.startsWith("tg_")) {
      await this.telegram.sendMessage(chatId, message);
    } else if (chatId.startsWith("fb_")) {
      await this.facebook.sendMessage(chatId, message);
    } else if (chatId.startsWith("ig_")) {
      await this.instagram.sendMessage(chatId, message);
    } else {
      throw new Error("Plataforma não suportada.");
    }
  }

  async receiveMessage(): Promise<void> {
    await this.whatsapp.listenMessages();
    await this.telegram.listenMessages();
    await this.facebook.listenMessages();
    await this.instagram.listenMessages();
  }
}
```

📦 1. Testes para o MongoDBGenericRepository
📌 Objetivo:

Testar CRUD, Soft Delete e Indexes do repositório genérico.
Usaremos o MongoMemoryServer para simular um banco real.
📜 __tests__/MongoDBGenericRepository.spec.ts:

```ts
import { MongoMemoryServer } from 'mongodb-memory-server';
import { MongoClient, Db, Collection } from 'mongodb';
import { MongoDBGenericRepository } from '../repositories/MongoDBGenericRepository';

interface TestEntity {
  _id?: string;
  name: string;
  active?: boolean;
  deletedAt?: Date;
}

describe('MongoDBGenericRepository', () => {
  let mongoServer: MongoMemoryServer;
  let client: MongoClient;
  let db: Db;
  let collection: Collection<TestEntity>;
  let repository: MongoDBGenericRepository<TestEntity>;

  beforeAll(async () => {
    mongoServer = await MongoMemoryServer.create();
    client = await MongoClient.connect(mongoServer.getUri(), {});
    db = client.db('testdb');
    collection = db.collection<TestEntity>('test-collection');
    repository = new MongoDBGenericRepository<TestEntity>(collection);
  });

  afterAll(async () => {
    await client.close();
    await mongoServer.stop();
  });

  beforeEach(async () => {
    await collection.deleteMany({});
  });

  it('deve inserir um documento', async () => {
    const entity: TestEntity = { name: 'Produto A' };
    await repository.insertOne(entity);

    const result = await collection.findOne({ name: 'Produto A' });
    expect(result).toBeDefined();
    expect(result?.name).toBe('Produto A');
  });

  it('deve encontrar um documento por ID', async () => {
    const entity: TestEntity = { name: 'Produto B' };
    const { insertedId } = await collection.insertOne(entity);

    const result = await repository.findById(insertedId.toString());
    expect(result).toBeDefined();
    expect(result?.name).toBe('Produto B');
  });

  it('deve atualizar um documento', async () => {
    const { insertedId } = await collection.insertOne({ name: 'Produto C' });

    await repository.updateOne(insertedId.toString(), { name: 'Produto C Atualizado' });

    const updated = await collection.findOne({ _id: insertedId });
    expect(updated?.name).toBe('Produto C Atualizado');
  });

  it('deve realizar soft delete', async () => {
    const { insertedId } = await collection.insertOne({ name: 'Produto D' });

    const deleted = await repository.softDelete(insertedId.toString());
    expect(deleted).toBe(true);

    const result = await collection.findOne({ _id: insertedId });
    expect(result?.active).toBe(false);
    expect(result?.deletedAt).toBeDefined();
  });

  it('deve criar índices', async () => {
    await repository.createIndexes([{ key: { name: 1 }, unique: true }]);

    const indexes = await repository.listIndexes();
    expect(indexes.some(index => index.name === 'name_1')).toBe(true);
  });
});
```

✅ Testa inserção, busca, atualização e soft delete.
✅ Usa MongoMemoryServer para ambiente de teste isolado.
✅ Verifica índices criados na coleção.

💳 2. Testes para o PaymentGateway
📌 Objetivo:

Testar pagamentos usando MercadoPago e Stripe.
Mockar os serviços para evitar chamadas reais.
📜 __tests__/PaymentGateway.spec.ts:

```ts
import { PaymentGateway } from '../adapters/PaymentGateway';
import { MercadoPagoService } from '../services/MercadoPagoService';
import { StripeService } from '../services/StripeService';

jest.mock('../services/MercadoPagoService');
jest.mock('../services/StripeService');

describe('PaymentGateway', () => {
  let paymentGateway: PaymentGateway;
  let mercadoPagoMock: jest.Mocked<MercadoPagoService>;
  let stripeMock: jest.Mocked<StripeService>;

  beforeEach(() => {
    mercadoPagoMock = new MercadoPagoService() as jest.Mocked<MercadoPagoService>;
    stripeMock = new StripeService() as jest.Mocked<StripeService>;

    mercadoPagoMock.createPayment.mockResolvedValue('mp_12345');
    stripeMock.createCharge.mockResolvedValue('ch_12345');

    paymentGateway = new PaymentGateway();
  });

  it('deve processar pagamento via MercadoPago', async () => {
    const paymentId = await paymentGateway.processPayment(100, 'BRL', 'card_123');
    expect(paymentId).toBe('mp_12345');
    expect(mercadoPagoMock.createPayment).toHaveBeenCalled();
  });

  it('deve processar pagamento via Stripe', async () => {
    const paymentId = await paymentGateway.processPayment(100, 'USD', 'card_123');
    expect(paymentId).toBe('ch_12345');
    expect(stripeMock.createCharge).toHaveBeenCalled();
  });

  it('deve capturar pagamento', async () => {
    mercadoPagoMock.capturePayment.mockResolvedValue(true);
    const result = await paymentGateway.capturePayment('mp_12345');
    expect(result).toBe(true);
  });

  it('deve reembolsar pagamento', async () => {
    stripeMock.refundCharge.mockResolvedValue(true);
    const result = await paymentGateway.refundPayment('ch_12345');
    expect(result).toBe(true);
  });
});
```

✅ Mocka MercadoPago e Stripe para evitar custos reais.
✅ Testa processamento, captura e reembolso de pagamento.
✅ Verifica qual gateway é usado conforme a moeda.

💬 3. Testes para o MessagingGateway
📌 Objetivo:

Testar envio de mensagens via WhatsApp, Telegram, Facebook e Instagram.
📜 __tests__/MessagingGateway.spec.ts:

```ts
import { MessagingGateway } from '../adapters/MessagingGateway';
import { WhatsAppService } from '../services/WhatsAppService';
import { TelegramService } from '../services/TelegramService';
import { FacebookService } from '../services/FacebookService';
import { InstagramService } from '../services/InstagramService';

jest.mock('../services/WhatsAppService');
jest.mock('../services/TelegramService');
jest.mock('../services/FacebookService');
jest.mock('../services/InstagramService');

describe('MessagingGateway', () => {
  let gateway: MessagingGateway;
  let whatsappMock: jest.Mocked<WhatsAppService>;
  let telegramMock: jest.Mocked<TelegramService>;
  let facebookMock: jest.Mocked<FacebookService>;
  let instagramMock: jest.Mocked<InstagramService>;

  beforeEach(() => {
    whatsappMock = new WhatsAppService() as jest.Mocked<WhatsAppService>;
    telegramMock = new TelegramService() as jest.Mocked<TelegramService>;
    facebookMock = new FacebookService() as jest.Mocked<FacebookService>;
    instagramMock = new InstagramService() as jest.Mocked<InstagramService>;

    gateway = new MessagingGateway();
  });

  it('deve enviar mensagem via WhatsApp', async () => {
    whatsappMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage('wa_123', 'Olá pelo WhatsApp!');
    expect(whatsappMock.sendMessage).toHaveBeenCalledWith('wa_123', 'Olá pelo WhatsApp!');
  });

  it('deve enviar mensagem via Telegram', async () => {
    telegramMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage('tg_123', 'Olá pelo Telegram!');
    expect(telegramMock.sendMessage).toHaveBeenCalledWith('tg_123', 'Olá pelo Telegram!');
  });

  it('deve enviar mensagem via Facebook', async () => {
    facebookMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage('fb_123', 'Olá pelo Facebook!');
    expect(facebookMock.sendMessage).toHaveBeenCalledWith('fb_123', 'Olá pelo Facebook!');
  });

  it('deve enviar mensagem via Instagram', async () => {
    instagramMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage('ig_123', 'Olá pelo Instagram!');
    expect(instagramMock.sendMessage).toHaveBeenCalledWith('ig_123', 'Olá pelo Instagram!');
  });

  it('deve lançar erro para plataforma desconhecida', async () => {
    await expect(gateway.sendMessage('xx_123', 'Mensagem inválida')).rejects.toThrow(
      'Plataforma não suportada.'
    );
  });
});
```

✅ Testa envio de mensagem para WhatsApp, Telegram, Facebook e Instagram.
✅ Garante que o serviço correto é chamado com base no chatId.
✅ Verifica erros para plataformas não suportadas.



✅ Baixo acoplamento → WhatsApp, Telegram, Facebook e Instagram usam a mesma interface.
✅ Lógica centralizada → O chatId determina qual serviço será chamado.
✅ Escalável → Podemos adicionar novas plataformas sem modificar o código principal.

📌 Resumo: Vantagens da Infrastructure Layer
✅ Repositório genérico → Qualquer entidade pode ser gerenciada sem reescrever código.
✅ Adapters desacoplados → MercadoPago/Stripe e WhatsApp/Telegram/Facebook/Instagram usam interfaces únicas.
✅ Facilidade para troca de tecnologia → Basta mudar a implementação sem afetar a aplicação.
✅ Escalabilidade e flexibilidade → Novos serviços podem ser adicionados facilmente.


### Agents Layer: Orquestração Inteligente e Reativa
📌 A Agents Layer é uma evolução da Event Layer, onde cada agente representa um contexto de domínio e executa suas responsabilidades de forma autônoma. Essa camada combina a reatividade dos eventos com a coordenação das transações distribuídas, permitindo integração perfeita entre sistemas centralizados e descentralizados.

Cada Agent encapsula os casos de uso (UseCases) e gerencia o ciclo de vida das operações, aplicando CQRS (Command/Query Responsibility Segregation) e Event Sourcing para garantir transparência nas mudanças de estado. Além de implementar nativamente técnicas de Circuit Breaker, Retry Policy, DLQ (Dead Letter), Idempotência, Observabilidade e Consistência, aplicando toda técnica necessária para garantir o total controle das ações internas do sistema.

Ela é composta por Agente Reativo, Agente Cognitivo e Agente Coordenador

Agente Reativo: Escuta eventos (ex.: novo pedido) e executa tarefas simples.
Agente Cognitivo: Toma decisões com base em regras de negócio (ex.: verificar estoque, sugerir substitutos).
Agente Coordenador: Orquestra o fluxo entre múltiplos agentes (ex.: pedido → pagamento → entrega).

A Agent Layer no Async Domain Flow gerencia a comunicação assíncrona entre domínios usando eventos. Ela encapsula o UseCase e a forma de comunicação, agregando reatividade à lógica de negócio, fazendo com que o desenvolvedor pense em como as mutações de estado dos seus domínios afetam o resto do sistema.

🎯 Objetivos da Agent Layer
✅ Desacoplamento: Cada serviço age de forma independente, sem dependência direta.
✅ Resiliência: Se um serviço falha, as mensagens são armazenadas e reprocessadas.
✅ Escalabilidade: Processos assíncronos permitem aumentar consumidores conforme a demanda.
✅ Observabilidade: Logs detalhados de cada evento, com rastreamento em tempo real.

## 🔑 Como Cada Técnica é Aplicada em Cada Agente

| **Técnica**            | **Execution Agent** 🟢 | **Signal Agent** 📣 | **Coordinator Agent** 🔄 | **Observer Agent** 👀 | **Cognitive Agent** 🤖 |
|------------------------|--------------------------|----------------------|---------------------------|-------------------------|------------------------|
| **Event Sourcing**     | Garante histórico das ações. | Emite eventos após execução. | Orquestra eventos em sequência. | Registra cada evento processado. | Usa eventos para treinar modelos. |
| **CQRS**               | Processa comandos (Write). | Publica eventos (Read). | Separa comandos e eventos. | Monitora projeções de leitura. | Usa dados para análise preditiva. |
| **Saga Pattern**       | Executa cada etapa da transação. | Emite sinais entre as etapas. | Garante consistência com compensação. | Observa falhas e reprocessamentos. | Toma decisão de rollback. |
| **Circuit Breaker**    | Evita sobrecarga nas APIs. | Impede notificações em cascata. | Isola falhas entre etapas. | Monitora estado do circuito. | Decide continuar ou abortar. |
| **Retry Policy**       | Tenta reexecutar após falha. | Reenvia sinal se falhar. | Reexecuta transação com backoff. | Registra tentativas e falhas. | Decide se o retry é viável. |
| **DLQ (Dead Letter)**  | Envia tarefa falha para a fila. | Encaminha eventos não entregues. | Registra falhas em longa duração. | Monitora mensagens enviadas para DLQ. | Decide reprocessar ou arquivar. |
| **Idempotência**       | Garante execução única. | Emite sinal único por evento. | Evita repetição nas etapas da Saga. | Registra eventos duplicados. | Detecta duplicidade nos dados. |
| **Observabilidade**    | Registra logs locais. | Emite eventos rastreáveis. | Garante rastreamento entre serviços. | Logging, Tracing e Métricas. | Registra métricas de inferência. |
| **Consistência**       | Confirma ou aborta transação. | Emite eventos síncronos ou assíncronos. | Define fluxo como atômico ou eventual. | Monitora status das transações. | Decide melhor modo de consistência. |

Essa é a estrutura 

domain-agents/
├─ agent-core/               # Base genérica para qualquer agente
│  ├─ state/                 # Gerenciamento de estado
│  ├─ cqrs/                  # Comandos (execute) e consultas (query)
│  ├─ events/                # Event Sourcing e publicação de eventos
│  ├─ policies/              # Circuit Breaker, Retry, DLQ
│  ├─ observability/         # Logs, métricas e tracing
│  └─ config/                # Configurações de ambiente (atômico/eventual)
│  └─ tools/                 # Funcionalidades externas

│
├─ order-agent/              # Exemplo: Agente do domínio de pedidos
│  ├─ state/                 # Estado do pedido
│  ├─ cqrs/                  # Casos de uso e consultas
│  ├─ events/                # Eventos do ciclo de vida do pedido
│  ├─ infrastructure/        # Banco de dados e APIs externas
│  ├─ policies/              # Resiliência e consistência
│  └─ observability/         # Monitoramento do agente
│  └─ tools/                 # Funcionalidades externas
│
├─ stock-agent/              # Agente do domínio de estoque (mesma estrutura)
│
└─ payment-agent/            # Agente do domínio de pagamento (mesma estrutura)


📦 Exemplos no CRM de Vendas via WhatsApp/Telegram
Vamos usar eventos para os seguintes fluxos:

Novo Pedido: Quando um cliente envia uma mensagem pedindo um produto.
Atualização de Estoque: Após um pedido, o estoque é atualizado.
Confirmação de Pagamento: Quando um pagamento é processado.
Envio de Mensagem: Notificar o cliente sobre status do pedido.
🚀 Tecnologias Usadas na Event Layer
Aqui está como podemos organizar a Event Layer no nosso CRM:

Tecnologia	Função

- Kafka Streams	Processamento contínuo de eventos em tempo real.
BullMQ	Gerenciamento de filas no Redis para processamento assíncrono.
Apache Airflow	Orquestração de fluxos de trabalho baseados em tempo/eventos.
AWS SQS/SNS	Fila de mensagens e notificações em nuvem.
Azure Service Bus	Fila e tópico para comunicação distribuída.
🔗 1. Implementando com Kafka Streams
📌 Cenário: Quando um pedido é criado, publicamos o evento order.created e serviços como estoque e notificações reagem.

📜 Configuração do Kafka Producer:
```ts
import { Kafka } from 'kafkajs';

export class KafkaProducer {
  private kafka = new Kafka({ clientId: 'crm-app', brokers: ['localhost:9092'] });
  private producer = this.kafka.producer();

  async sendOrderCreatedEvent(orderId: string, customerId: string) {
    await this.producer.connect();
    await this.producer.send({
      topic: 'order.created',
      messages: [{ key: orderId, value: JSON.stringify({ orderId, customerId }) }],
    });
    await this.producer.disconnect();
  }
}
```

📜 Consumindo o Evento (Kafka Consumer):
```ts
import { Kafka } from 'kafkajs';

export class KafkaConsumer {
  private kafka = new Kafka({ clientId: 'crm-app', brokers: ['localhost:9092'] });
  private consumer = this.kafka.consumer({ groupId: 'stock-service' });

  async start() {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic: 'order.created', fromBeginning: false });

    await this.consumer.run({
      eachMessage: async ({ topic, partition, message }) => {
        const data = JSON.parse(message.value.toString());
        console.log(`📩 Novo pedido recebido: ${data.orderId}`);

        // Atualiza estoque ou notifica cliente
      },
    });
  }
}
```

✅ Vantagens:

Processamento em tempo real.
Capacidade de escalar consumidores horizontalmente.
Garantia de entrega com offsets.
⏳ 2. Gerenciamento de Fila com BullMQ
📌 Cenário: Quando um pagamento é confirmado, enfileiramos a atualização de estoque e a notificação ao cliente.

📜 Configuração da Fila (BullMQ):
```ts
import { Queue } from 'bullmq';

const orderQueue = new Queue('order-processing', { connection: { host: 'localhost', port: 6379 } });

export async function enqueueOrder(orderId: string) {
  await orderQueue.add('process-order', { orderId });
  console.log(`✅ Pedido ${orderId} enfileirado!`);
}
```

📜 Processamento da Fila (BullMQ Worker):
```ts
import { Worker } from 'bullmq';

const worker = new Worker('order-processing', async job => {
  const { orderId } = job.data;
  console.log(`🚀 Processando pedido ${orderId}...`);

  // Atualiza estoque e envia notificação
});
```

✅ Vantagens:

Reprocessamento automático em caso de falha.
Controle de concorrência.
Monitoramento em tempo real.
🌬️ 3. Orquestração com Apache Airflow
📌 Cenário: Um fluxo de Airflow para processar pedidos, atualizar estoque e enviar notificações.

📜 DAG de Exemplo (Python):
```ts
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def process_order(**kwargs):
    order_id = kwargs['dag_run'].conf.get('order_id')
    print(f"✅ Processando pedido: {order_id}")

def update_stock(**kwargs):
    order_id = kwargs['dag_run'].conf.get('order_id')
    print(f"📦 Atualizando estoque para pedido: {order_id}")

def notify_customer(**kwargs):
    order_id = kwargs['dag_run'].conf.get('order_id')
    print(f"📨 Notificando cliente sobre pedido: {order_id}")

with DAG('order_processing_dag', start_date=datetime(2024, 2, 19), schedule_interval=None, catchup=False) as dag:
    process_order_task = PythonOperator(task_id='process_order', python_callable=process_order)
    update_stock_task = PythonOperator(task_id='update_stock', python_callable=update_stock)
    notify_customer_task = PythonOperator(task_id='notify_customer', python_callable=notify_customer)

    process_order_task >> update_stock_task >> notify_customer_task
```

✅ Vantagens:

Orquestração visual de fluxos complexos.
Reprocessamento automático em falhas.
Agendamento flexível e integração com serviços externos.
☁️ 4. Usando AWS SQS e SNS
📌 Cenário: Publicar eventos para notificações via SNS e processar via SQS.

📜 Publicando Evento SNS:
```ts
import { SNSClient, PublishCommand } from '@aws-sdk/client-sns';

const sns = new SNSClient({ region: 'us-east-1' });

export async function publishOrderEvent(orderId: string) {
  const command = new PublishCommand({
    TopicArn: 'arn:aws:sns:us-east-1:123456789012:OrderTopic',
    Message: JSON.stringify({ orderId }),
  });

  await sns.send(command);
  console.log(`✅ Evento 'order.created' enviado para SNS!`);
}
```

📜 Consumindo Evento SQS:
```ts
import { SQSClient, ReceiveMessageCommand } from '@aws-sdk/client-sqs';

const sqs = new SQSClient({ region: 'us-east-1' });

export async function receiveOrderEvents() {
  const command = new ReceiveMessageCommand({
    QueueUrl: 'https://sqs.us-east-1.amazonaws.com/123456789012/OrderQueue',
    MaxNumberOfMessages: 10,
  });

  const response = await sqs.send(command);
  for (const message of response.Messages || []) {
    const orderData = JSON.parse(message.Body);
    console.log(`📥 Novo evento de pedido: ${orderData.orderId}`);
  }
}
```

✅ Vantagens:

Alta disponibilidade.
Escalabilidade automática.
Integração com outros serviços da AWS.
🔷 5. Usando Azure Service Bus
📌 Cenário: Processar eventos order.created usando filas ou tópicos no Azure.

📜 Enviando Evento para Service Bus:
```ts
import { ServiceBusClient } from "@azure/service-bus";

const sbClient = new ServiceBusClient("Endpoint=sb://seu-nome.servicebus.windows.net/");
const sender = sbClient.createSender("order-created");

export async function sendOrderCreated(orderId: string) {
  await sender.sendMessages({ body: { orderId } });
  console.log(`✅ Evento 'order.created' enviado para Azure Service Bus!`);
}
```

📜 Consumindo Evento do Service Bus:
```ts
const receiver = sbClient.createReceiver("order-created");

receiver.subscribe({
  processMessage: async message => {
    console.log(`📥 Pedido recebido: ${message.body.orderId}`);
  },
  processError: async error => {
    console.error(`❌ Erro no processamento: ${error}`);
  }
});
```

✅ Vantagens:

Alta integração com serviços do Azure.
Suporte para filas e tópicos.
Processamento em lote.


📦 1. Interface Genérica para Consumers e Publishers

Para implementarmos uma interface genérica para Consumers e Publishers devemos definir quais são suas funcionalidades mínimas, além do publish para o Publisher e do consume para o Consumer eu resolvi declarar também as funções: ack e nack.


📌 O que são ack e nack?

- ack(message): Confirma o processamento da mensagem, removendo-a da fila.
- nack(message, requeue?): Indica falha no processamento.
  - Se requeue = true, a mensagem volta para a fila.
  - Se requeue = false, a mensagem é descartada ou enviada para uma Dead Letter Queue (DLQ).
📦 Como diferentes tecnologias tratam ack e nack?

## 📦 Como diferentes tecnologias tratam `ack` e `nack`?

| Mensageria           | `ack` (Confirmação)                           | `nack` (Rejeição)                                         |
|----------------------|-----------------------------------------------|------------------------------------------------------------|
| **Kafka**            | Offset é movido para frente automaticamente   | Não há `nack` nativo; controle manual do reprocessamento   |
| **RabbitMQ**         | `channel.ack(msg)` remove a mensagem da fila  | `channel.nack(msg, requeue?)` devolve ou descarta a mensagem |
| **AWS SQS**          | `DeleteMessageCommand` remove a mensagem      | Sem `nack` nativo; a mensagem retorna após o timeout        |
| **Azure Service Bus**| `completeMessage(msg)` remove a mensagem      | `abandonMessage(msg)` devolve para a fila                   |

Acho importante que a gente tenha uma forma genérica 

Essa interface garante que qualquer tecnologia siga o mesmo contrato.

📜 interfaces/IMessagePublisher.ts:
```ts
export interface IMessagePublisher {
  publish(topic: string, message: object): Promise<void>;
}
```

📜 interfaces/IMessageConsumer.ts:
```ts
export interface IMessageConsumer {
  consume(
    topic: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void>;
}
```

🏗 2. PublisherFactory
A PublisherFactory cria instâncias de publicadores para cada tecnologia.

factories/PublishersModules.ts
```ts

import { IMessagePublisher } from "../interfaces/IMessagePublisher";
import { KafkaPublisher } from "../publishers/KafkaPublisher";
import { RabbitMQPublisher } from "../publishers/RabbitMQPublisher";
import { SNSPublisher } from "../publishers/SNSPublisher";
import { ServiceBusPublisher } from "../publishers/ServiceBusPublisher";

const PUBLISHERS = {
  "kafka": new KafkaPublisher(),
  "rabbitmq": new RabbitMQPublisher(),
  "sns": new SNSPublisher(),
  "servicebus": new ServiceBusPublisher()
}

export default PUBLISHERS;
```

📜 factories/PublisherFactory.ts:
```ts
import PUBLISHERS from "./PublishersModules"

export class PublisherFactory {
  static createPublisher(provider: string): IMessagePublisher {
    return PUBLISHERS[provider.toLowerCase()];
  }
}

```

⚙️ 3. ConsumerFactory
A ConsumerFactory cria consumidores para cada tecnologia.

📜 factories/ConsumerFactory.ts:
```ts
import { IMessageConsumer } from "../interfaces/IMessageConsumer";
import { KafkaConsumer } from "../consumers/KafkaConsumer";
import { RabbitMQConsumer } from "../consumers/RabbitMQConsumer";
import { SQSConsumer } from "../consumers/SQSConsumer";
import { ServiceBusConsumer } from "../consumers/ServiceBusConsumer";

const CONSUMERS = {
  "kafka": KafkaConsumer,
  "rabbitmq": RabbitMQConsumer,
  "sns": SQSConsumer,
  "servicebus": ServiceBusConsumer
}

export class ConsumerFactory {
  static createConsumer(provider: string, useSingleton = process.env.QUEUE_CONSUMER_SINGLETON | true): IMessageConsumer {
    const key = `consumer:${provider.toLowerCase()}`;

    if (!useSingleton) {
      return this.instantiateConsumer(provider);
    }

    return InstanceManager.getInstance<IMessageConsumer>(key, () => {
      return this.instantiateConsumer(provider);
    });
  }

  private static instantiateConsumer(provider: string): IMessageConsumer {
    return new CONSUMERS[provider.toLowerCase()]();
  }
}
```

🚀 4. Implementação dos Publishers
Vamos criar os publicadores para cada tecnologia.

📜 publishers/KafkaPublisher.ts:
```ts
import { Kafka } from 'kafkajs';
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class KafkaPublisher implements IMessagePublisher {
  private kafka = new Kafka({ clientId: 'crm-app', brokers: ['localhost:9092'] });
  private producer = this.kafka.producer();

  async publish(topic: string, message: object): Promise<void> {
    await this.producer.connect();
    await this.producer.send({
      topic,
      messages: [{ value: JSON.stringify(message) }],
    });
    console.log(`✅ Mensagem publicada no Kafka - Tópico: ${topic}`);
    await this.producer.disconnect();
  }
}
```

📜 publishers/RabbitMQPublisher.ts:
```ts
import amqp from 'amqplib';
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class RabbitMQPublisher implements IMessagePublisher {
  private connection: amqp.Connection;
  private channel: amqp.Channel;

  async publish(queue: string, message: object): Promise<void> {
    this.connection = await amqp.connect('amqp://localhost');
    this.channel = await this.connection.createChannel();
    await this.channel.assertQueue(queue);
    this.channel.sendToQueue(queue, Buffer.from(JSON.stringify(message)));
    console.log(`✅ Mensagem publicada no RabbitMQ - Fila: ${queue}`);
    await this.channel.close();
    await this.connection.close();
  }
}
```

📜 publishers/SNSPublisher.ts:
```ts
import { SNSClient, PublishCommand } from '@aws-sdk/client-sns';
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class SNSPublisher implements IMessagePublisher {
  private sns = new SNSClient({ region: 'us-east-1' });

  async publish(topicArn: string, message: object): Promise<void> {
    const command = new PublishCommand({
      TopicArn: topicArn,
      Message: JSON.stringify(message),
    });
    await this.sns.send(command);
    console.log(`✅ Mensagem publicada no SNS - Tópico: ${topicArn}`);
  }
}
```

📜 publishers/ServiceBusPublisher.ts:
```ts
import { ServiceBusClient } from '@azure/service-bus';
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class ServiceBusPublisher implements IMessagePublisher {
  private client = new ServiceBusClient(process.env.AZURE_SERVICE_BUS_CONNECTION!);
  private sender = this.client.createSender('order-topic');

  async publish(topic: string, message: object): Promise<void> {
    await this.sender.sendMessages({ body: message });
    console.log(`✅ Mensagem publicada no Azure Service Bus - Tópico: ${topic}`);
    await this.sender.close();
  }
}
```

📥 5. Implementação dos Consumers
Vamos criar os consumidores para cada tecnologia.

📜 consumers/KafkaConsumer.ts:
```ts
import { Kafka } from 'kafkajs';
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class KafkaConsumer implements IMessageConsumer {
  private kafka = new Kafka({ clientId: 'crm-app', brokers: ['localhost:9092'] });
  private consumer = this.kafka.consumer({ groupId: 'crm-group' });

  async consume(
    topic: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic, fromBeginning: false });

    await this.consumer.run({
      eachMessage: async ({ message, partition, topic }) => {
        const content = JSON.parse(message.value?.toString() || '{}');
        console.log(`📩 Kafka - Mensagem recebida: ${JSON.stringify(content)}`);

        // `ack` é apenas um log, pois Kafka confirma automaticamente
        const ack = () => console.log(`✅ Kafka - Mensagem processada: ${message.offset}`);

        // `nack` simulado movendo offset manualmente
        const nack = (requeue: boolean) => {
          if (requeue) {
            console.warn(`⚠️ Kafka - Mensagem não processada. Reprocessar manualmente.`);
          }
        };

        await handler(content, ack, nack);
      },
    });
  }
}

```

📜 consumers/RabbitMQConsumer.ts:
```ts
import amqp from 'amqplib';
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class RabbitMQConsumer implements IMessageConsumer {
  private connection!: amqp.Connection;
  private channel!: amqp.Channel;

  async consume(
    queue: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    this.connection = await amqp.connect('amqp://localhost');
    this.channel = await this.connection.createChannel();
    await this.channel.assertQueue(queue);

    this.channel.consume(queue, async (msg) => {
      if (msg) {
        const content = JSON.parse(msg.content.toString());
        console.log(`📩 RabbitMQ - Mensagem recebida: ${JSON.stringify(content)}`);

        // `ack` confirma o processamento e remove a mensagem da fila
        const ack = () => {
          this.channel.ack(msg);
          console.log(`✅ RabbitMQ - Mensagem processada.`);
        };

        // `nack` devolve a mensagem para a fila se `requeue = true`
        const nack = (requeue = true) => {
          this.channel.nack(msg, false, requeue);
          console.log(`⚠️ RabbitMQ - Mensagem falhou. Reenviar? ${requeue}`);
        };

        await handler(content, ack, nack);
      }
    });
  }
}

```

📜 consumers/SQSConsumer.ts:
```ts
import { SQSClient, ReceiveMessageCommand, DeleteMessageCommand } from '@aws-sdk/client-sqs';
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class SQSConsumer implements IMessageConsumer {
  private sqs = new SQSClient({ region: 'us-east-1' });

  async consume(
    queueUrl: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    const command = new ReceiveMessageCommand({
      QueueUrl: queueUrl,
      MaxNumberOfMessages: 10,
      WaitTimeSeconds: 5,
    });

    setInterval(async () => {
      const response = await this.sqs.send(command);
      for (const msg of response.Messages || []) {
        const body = JSON.parse(msg.Body!);
        console.log(`📩 AWS SQS - Mensagem recebida: ${JSON.stringify(body)}`);

        const ack = async () => {
          await this.sqs.send(new DeleteMessageCommand({
            QueueUrl: queueUrl,
            ReceiptHandle: msg.ReceiptHandle!,
          }));
          console.log(`✅ AWS SQS - Mensagem processada e removida.`);
        };

        const nack = (requeue = true) => {
          if (!requeue) console.warn(`⚠️ AWS SQS - Mensagem falhou e será descartada.`);
          // Mensagem será automaticamente reprocessada após timeout
        };

        await handler(body, ack, nack);
      }
    }, 5000);
  }
}

```

📜 consumers/ServiceBusConsumer.ts:
```ts
import { ServiceBusClient } from '@azure/service-bus';
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class ServiceBusConsumer implements IMessageConsumer {
  private client = new ServiceBusClient(process.env.AZURE_SERVICE_BUS_CONNECTION!);
  private receiver = this.client.createReceiver('order-topic');

  async consume(
    topic: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    this.receiver.subscribe({
      processMessage: async (msg) => {
        const content = msg.body;
        console.log(`📩 Azure Service Bus - Mensagem recebida: ${JSON.stringify(content)}`);

        const ack = async () => {
          await this.receiver.completeMessage(msg);
          console.log(`✅ Azure Service Bus - Mensagem processada.`);
        };

        const nack = async (requeue = true) => {
          if (requeue) {
            await this.receiver.abandonMessage(msg);
            console.warn(`⚠️ Azure Service Bus - Mensagem retornada à fila.`);
          } else {
            console.warn(`❌ Azure Service Bus - Mensagem descartada.`);
          }
        };

        await handler(content, ack, nack);
      },
      processError: async (err) => {
        console.error(`❌ Erro no processamento: ${err}`);
      },
    });
  }
}

```

🎯 6. Exemplo de Uso no CRM
Vamos integrar tudo para o fluxo de pedidos via WhatsApp.

📜 Publicando um Pedido::
```ts
import { PublisherFactory } from './factories/PublisherFactory';

async function createOrder(orderId: string) {
  const publisher = PublisherFactory.createPublisher('kafka');
  await publisher.publish('order.created', { orderId, customer: '12345' });
}

createOrder('abc123');
```

📜 Consumindo o Pedido::
```ts
import { ConsumerFactory } from './factories/ConsumerFactory';

async function startOrderConsumer() {
  const consumer = ConsumerFactory.createConsumer('kafka');
  await consumer.consume('order.created', async (message) => {
    console.log(`📥 Processando pedido: ${JSON.stringify(message)}`);
    // Lógica de negócio
  });
}

startOrderConsumer();
```

📜 utils/RetryFallback.ts:
```ts
type RetryHandler = (message: any) => Promise<void>;

interface RetryMetadata {
  attempts: number;
  nextRetry: Date;
}

const retryMetadata: Map<string, RetryMetadata> = new Map();

export async function retryWithFallback(
  messageId: string,
  message: any,
  handler: RetryHandler,
  nack: (requeue?: boolean) => void
): Promise<void> {
  const maxRetries = parseInt(process.env.QUEUE_MAX_RETRIES ?? '5', 10);
  const retryInterval = parseInt(process.env.QUEUE_MAX_RETRIES_TIME ?? '10', 10) * 1000;

  // Obtem tentativas anteriores (ou inicializa)
  const metadata = retryMetadata.get(messageId) || { attempts: 0, nextRetry: new Date() };
  const { attempts } = metadata;

  if (attempts >= maxRetries) {
    console.error(`❌ Mensagem ${messageId} atingiu o máximo de ${maxRetries} tentativas. Movendo para DLQ.`);
    nack(false);  // Descarte final ou envio para DLQ
    retryMetadata.delete(messageId);
    return;
  }

  // Agendamento do retry
  const nextRetry = new Date(Date.now() + retryInterval);
  retryMetadata.set(messageId, { attempts: attempts + 1, nextRetry });

  console.warn(`⚠️ Tentativa ${attempts + 1} para mensagem ${messageId}. Nova tentativa em ${retryInterval / 1000} segundos.`);

  setTimeout(async () => {
    try {
      await handler(message);
      console.log(`✅ Mensagem ${messageId} processada com sucesso após ${attempts + 1} tentativa(s).`);
      retryMetadata.delete(messageId);
    } catch (error) {
      console.error(`❌ Falha no retry ${attempts + 1} para mensagem ${messageId}.`, error);
      await retryWithFallback(messageId, message, handler, nack);  // Retry recursivo
    }
  }, retryInterval);
}

```

📜 Exemplo com KafkaConsumer:
```ts
import { Kafka } from 'kafkajs';
import { IMessageConsumer } from "../interfaces/IMessageConsumer";
import { retryWithFallback } from '../utils/RetryFallback';

export class KafkaConsumer implements IMessageConsumer {
  private kafka = new Kafka({ clientId: 'crm-app', brokers: ['localhost:9092'] });
  private consumer = this.kafka.consumer({ groupId: 'crm-group' });

  async consume(topic: string, handler: (message: any, ack: () => void, nack: (requeue?: boolean) => void) => Promise<void>): Promise<void> {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic, fromBeginning: false });

    await this.consumer.run({
      eachMessage: async ({ message }) => {
        const messageId = message.key?.toString() || Date.now().toString();
        const content = JSON.parse(message.value?.toString() || '{}');

        console.log(`📩 Kafka - Mensagem recebida: ${JSON.stringify(content)}`);

        const ack = () => {
          console.log(`✅ Kafka - Mensagem ${messageId} processada.`);
        };

        const nack = (requeue = true) => {
          console.warn(`⚠️ Kafka - Falha na mensagem ${messageId}. Reenviar: ${requeue}`);
          if (requeue) {
            retryWithFallback(messageId, content, async () => handler(content, ack, nack), nack);
          }
        };

        try {
          await handler(content, ack, nack);
        } catch (error) {
          console.error(`❌ Kafka - Erro ao processar mensagem ${messageId}:`, error);
          nack(true);
        }
      },
    });
  }
}
```

4️⃣ Exemplo com RabbitMQConsumer:
```ts
import amqp from 'amqplib';
import { IMessageConsumer } from "../interfaces/IMessageConsumer";
import { retryWithFallback } from '../utils/RetryFallback';

export class RabbitMQConsumer implements IMessageConsumer {
  private connection!: amqp.Connection;
  private channel!: amqp.Channel;

  async consume(queue: string, handler: (message: any, ack: () => void, nack: (requeue?: boolean) => void) => Promise<void>): Promise<void> {
    this.connection = await amqp.connect('amqp://localhost');
    this.channel = await this.connection.createChannel();
    await this.channel.assertQueue(queue);

    this.channel.consume(queue, async (msg) => {
      if (msg) {
        const messageId = msg.properties.messageId || Date.now().toString();
        const content = JSON.parse(msg.content.toString());

        console.log(`📩 RabbitMQ - Mensagem recebida: ${JSON.stringify(content)}`);

        const ack = () => {
          this.channel.ack(msg);
          console.log(`✅ RabbitMQ - Mensagem ${messageId} processada.`);
        };

        const nack = (requeue = true) => {
          console.warn(`⚠️ RabbitMQ - Falha na mensagem ${messageId}. Reenviar: ${requeue}`);
          if (requeue) {
            retryWithFallback(messageId, content, async () => handler(content, ack, nack), nack);
          } else {
            this.channel.nack(msg, false, false); // Descarte final
          }
        };

        try {
          await handler(content, ack, nack);
        } catch (error) {
          console.error(`❌ RabbitMQ - Erro ao processar mensagem ${messageId}:`, error);
          nack(true);
        }
      }
    });
  }
}
```

5️⃣ Exemplo de Uso (Aplicação Real)
Vamos criar um consumidor simples com falha simulada e retry automático.
```ts
import { ConsumerFactory } from './factories/ConsumerFactory';

async function startConsumer() {
  const consumer = ConsumerFactory.createConsumer("kafka");

  await consumer.consume("order.created", async (message, ack, nack) => {
    console.log(`📥 Processando mensagem: ${JSON.stringify(message)}`);

    // Simulando falha
    if (!message.orderId) {
      console.error("❌ Falha: OrderId ausente.");
      nack(true);  // Reenviar com retry automático
      return;
    }

    console.log(`✅ Pedido ${message.orderId} processado com sucesso.`);
    ack();  // Confirmação final
  });
}

startConsumer();
```


## 📊 Benefícios da Solução

| Recurso               | Descrição                                                       |
|-----------------------|-----------------------------------------------------------------|
| ✅ **Segurança**       | Valida provedores para evitar erros de execução.                |
| 🔄 **Flexibilidade**   | Alterna entre Singleton e Factory conforme necessidade.         |
| 📦 **Gerenciamento**   | Inclui método `clearInstance` para reiniciar instâncias.         |
| 💪 **Tipagem Forte**   | Usa `Record<string, { new(): IMessageConsumer }>` para classes. |
| ⚙️ **Configuração**    | Controlado via `process.env.QUEUE_CONSUMER_SINGLETON`.           |
| 🚀 **Extensibilidade** | Fácil adicionar novos provedores no objeto `CONSUMERS`.         |
| 🔒 **Eficiência**      | Singleton evita múltiplas conexões, economizando recursos.       |


## Modelagem

## 🌐 Diagrama de Integração das Camadas na Agents Layer

```mermaid
flowchart TD
  %% Entrada de Mensagem
  subgraph Entrada
    API[API]
    WhatsApp[WhatsApp Webhook]
    Kafka[Kafka Topic]
  end

  %% Camada de Mensageria
  subgraph Mensageria
    RabbitMQ[RabbitMQ Queue]
    SQS[AWS SQS Queue]
    SNS[AWS SNS Topic]
  end

  %% Agents Layer
  subgraph Agents Layer
    ExecutionAgent[Execution Agent(Processamento de Pedidos)]
    SignalAgent[Signal Agent 📣\n(Emissão de Eventos)]
    CoordinatorAgent[Coordinator Agent 🔄\n(Gerenciamento de Saga)]
    ObserverAgent[Observer Agent 👀\n(Monitoramento e Logs)]
    CognitiveAgent[Cognitive Agent 🤖\n(Análise com IA)]
  end

  %% Camada de Persistência
  subgraph Persistência
    EventStore[(Event Store 🕰️\n(Event Sourcing))]
    ReadModel[(Read Model 📊\n(CQRS - Projeções))]
    DLQ[(Dead Letter Queue ☠️)]
  end

  %% Monitoramento
  subgraph Monitoramento
    Prometheus[(Prometheus 📈\n(Métricas))]
    Jaeger[(Jaeger 🔍\n(Tracing))]
    ELK[(ELK Stack 📊\n(Logs))]
  end

  %% Fluxo de Integração
  API -->|Mensagem Recebida| Kafka & RabbitMQ & WhatsApp
  WhatsApp -->|Nova Mensagem| ExecutionAgent
  Kafka -->|Evento Criado| ExecutionAgent
  RabbitMQ -->|Comando| ExecutionAgent
  ExecutionAgent -->|Processa Pedido| EventStore
  ExecutionAgent -->|Emite Evento| SignalAgent
  SignalAgent -->|Notificação| API
  SignalAgent -->|Confirmação| Kafka

  ExecutionAgent -->|Falha?| Retry{Retry Policy?}
  Retry -- Falhou --> DLQ
  Retry -- Sucesso --> EventStore

  EventStore -->|Projeção CQRS| ReadModel
  EventStore -->|Evento Saga| CoordinatorAgent
  CoordinatorAgent -->|Inicia Fluxo Saga| ExecutionAgent
  CoordinatorAgent -->|Falha?| DLQ
  CoordinatorAgent -->|Sucesso| SignalAgent

  ObserverAgent -->|Logs e Métricas| Prometheus & Jaeger & ELK
  CognitiveAgent -->|Análise de Mensagem| EventStore

  EventStore -->|Histórico| ReadModel
  ReadModel -->|Consulta| API
```

## MicroServiços

Essa Arquitetura define uma forma lógica de como utilizar microserviços em seu sistema, cada microserviço é um Agente pois ele é reativo e contém uma Tool(Ferramenta/Lógica) própria, exemplo:

microservices/
├─ api-gateway/         # Interface Layer (Porta de Entrada)
├─ execution-agent/     # Order Service (Pedido)
├─ signal-agent/        # Notification Service (Notificação)
├─ coordinator-agent/   # Payment Service (Orquestração)
├─ observer-agent/      # Monitoring Service (Observabilidade)
├─ cognitive-agent/     # Recommendation Service (IA)
├─ event-bus/           # Comunicação entre serviços (Kafka/RabbitMQ)
├─ event-store/         # Event Sourcing (MongoDB)
└─ shared/              # Código comum (JWT, Idempotência, DLQ, Tools)


🏗️ Arquitetura de Microserviços: Visão Geral
API Gateway (Interface Layer): Recebe as requisições REST.

Microserviços:
- Order Agent: Cria pedidos (Agents Layer).
- Payment Agent: Processa pagamentos (Agents Layer).
- Stock Agent: Verifica estoque (Agents Layer).
- Notification Agent: Envia notificações (Agents Layer).
- Event Bus: Comunicação entre serviços via Kafka ou RabbitMQ (Infrasctructure Layer).
- Event Store: Registra eventos para Event Sourcing (Infrasctructure Layer).
- DLQ: Gerencia mensagens falhas (Infrasctructure Layer).


Observabilidade: Monitoramento com Prometheus, Jaeger e ELK Stack.

📜 Fluxo Completo (Exemplo: Criar um Pedido):

- O cliente envia um POST /orders para o API Gateway.
- O Order Service emite o evento order.create para o Order Agent 
  - que cria o pedido e publica o evento order.created.
- O Stock Agent recebe esse evento e verifica o estoque respondendo com stock.checked
  - enviando a quantidade total do produto no estoque.
- O Payment Service processa o pagamento (payment.processed)
  - que o Payment Agent recebe e adiciona o valor no Sistema Contábil.

Se tudo der certo, o Notification Agent notifica o cliente.
Se alguma etapa falhar, a Saga compensa a transação e cancela o pedido.

🌐 Estrutura de Pastas:

```
microservices/
├─ api-gateway/         # Interface REST com Express
├─ services
├─ order-service/       # Serviço de pedidos (Execution Agent)
├─ stock-service/       # Serviço de estoque (Signal Agent)
├─ payment-service/     # Serviço de pagamento (Coordinator Agent)
├─ notification-service/ # Serviço de notificação (Signal Agent)
├─ event-bus/           # Kafka/RabbitMQ para Event Sourcing
├─ event-store/         # MongoDB como Event Store
├─ shared/              # Código compartilhado (Tools, DLQ, Idempotência)
├─ monitoring/          # Prometheus, Jaeger, ELK para Observabilidade
└─ docker-compose.yml   # Infraestrutura dos microserviços
```

1️⃣ API Gateway: Entrada das Requisições
O API Gateway expõe os endpoints REST e roteia as requisições.

javascript
Copiar
Editar
// api-gateway/index.js
import express from 'express';
import axios from 'axios';

const app = express();
app.use(express.json());

// Criar pedido (rota principal)
app.post('/orders', async (req, res) => {
  const { customerId, items } = req.body;

  try {
    const response = await axios.post('http://order-service:4001/orders', { customerId, items });
    res.status(201).json(response.data);
  } catch (error) {
    console.error('Erro ao criar pedido:', error.message);
    res.status(500).json({ error: 'Falha ao processar o pedido.' });
  }
});

// Health Check
app.get('/health', (req, res) => res.send('API Gateway rodando 🚀'));

app.listen(4000, () => console.log('🚪 API Gateway rodando na porta 4000'));
✅ Motivo: O Gateway é o ponto central para entrada de APIs REST.

2️⃣ Order Service: Criar Pedidos (Execution Agent)
O Order Service cria pedidos e publica eventos.

javascript
Copiar
Editar
// order-service/index.js
import express from 'express';
import { EventBus } from '../shared/EventBus.js';
import { EventStore } from '../shared/EventStore.js';
import { Idempotency } from '../shared/Idempotency.js';

const app = express();
app.use(express.json());

// Criar pedido
app.post('/orders', async (req, res) => {
  const { customerId, items } = req.body;
  const orderId = `order-${Date.now()}`;

  if (await Idempotency.isProcessed(orderId)) {
    return res.status(409).json({ error: 'Pedido já processado.' });
  }

  // Salva no Event Store (Event Sourcing)
  await EventStore.saveEvent('order.created', { orderId, customerId, items });

  // Publica evento para o Event Bus
  EventBus.publish('order.created', { orderId, customerId, items });

  res.status(201).json({ orderId, message: 'Pedido criado com sucesso.' });
});

// Consome resposta do estoque
EventBus.subscribe('stock.checked', async (event) => {
  const { orderId, success } = event;

  if (success) {
    console.log(`✅ Estoque confirmado para pedido #${orderId}`);
    EventBus.publish('payment.process', { orderId, amount: 100 });
  } else {
    console.warn(`❌ Estoque insuficiente para pedido #${orderId}`);
    EventBus.publish('order.failed', { orderId, reason: 'Estoque insuficiente' });
  }
});

app.listen(4001, () => console.log('📦 Order Service rodando na porta 4001'));
✅ Motivo:

Execution Agent: Processa o pedido.
Event Sourcing: Salva no Event Store.
Idempotência: Evita duplicidade.
3️⃣ Stock Service: Verificar Estoque (Signal Agent)
O Stock Service verifica o estoque.

javascript
Copiar
Editar
// stock-service/index.js
import express from 'express';
import { EventBus } from '../shared/EventBus.js';

const app = express();
app.use(express.json());

// Consome evento de pedido criado
EventBus.subscribe('order.created', async (event) => {
  const { orderId, items } = event;
  console.log(`🔍 Verificando estoque para pedido #${orderId}`);

  const estoqueDisponivel = Math.random() > 0.2; // Simula disponibilidade

  // Publica evento baseado na verificação
  EventBus.publish('stock.checked', { orderId, success: estoqueDisponivel });
});

app.listen(4002, () => console.log('🏷️ Stock Service rodando na porta 4002'));
✅ Motivo:

Signal Agent: Emite sinal após execução.
4️⃣ Payment Service: Processar Pagamento (Coordinator Agent)
O Payment Service gerencia a transação da Saga.

javascript
Copiar
Editar
// payment-service/index.js
import express from 'express';
import { EventBus } from '../shared/EventBus.js';
import { CircuitBreaker } from '../shared/CircuitBreaker.js';

const app = express();
app.use(express.json());

EventBus.subscribe('payment.process', async (event) => {
  const { orderId, amount } = event;
  console.log(`💳 Processando pagamento de R$${amount} para pedido #${orderId}`);

  const breaker = new CircuitBreaker(async () => {
    const sucesso = Math.random() > 0.3; // Simula falha 30% das vezes
    if (!sucesso) throw new Error('Falha no pagamento.');

    EventBus.publish('payment.processed', { orderId });
  });

  breaker.execute().catch(() => {
    console.error(`❌ Pagamento falhou para pedido #${orderId}`);
    EventBus.publish('saga.failed', { orderId, reason: 'Falha no pagamento' });
  });
});

app.listen(4003, () => console.log('💰 Payment Service rodando na porta 4003'));
✅ Motivo:

Coordinator Agent: Gerencia o fluxo da Saga Pattern.
Circuit Breaker: Evita falhas repetidas.
5️⃣ Notification Service: Enviar Notificações (Signal Agent)
javascript
Copiar
Editar
// notification-service/index.js
import express from 'express';
import { EventBus } from '../shared/EventBus.js';

const app = express();
app.use(express.json());

// Notificar cliente após pagamento aprovado
EventBus.subscribe('payment.processed', (event) => {
  const { orderId } = event;
  console.log(`📣 Enviando notificação: Pedido #${orderId} confirmado!`);
});

app.listen(4004, () => console.log('📨 Notification Service rodando na porta 4004'));
✅ Motivo:

Signal Agent: Emite sinal após evento processado.
6️⃣ Event Bus (RabbitMQ/Kafka)
Um EventBus simples para comunicação entre microserviços.

javascript
Copiar
Editar
// shared/EventBus.js
import { EventEmitter } from 'events';

export class EventBus {
  static bus = new EventEmitter();

  static publish(event, payload) {
    console.log(`📣 [EventBus] Evento publicado: ${event}`);
    this.bus.emit(event, payload);
  }

  static subscribe(event, handler) {
    console.log(`🔔 [EventBus] Assinando evento: ${event}`);
    this.bus.on(event, handler);
  }
}
✅ Motivo: Comunicação assíncrona entre microserviços.

7️⃣ Event Store (MongoDB para Event Sourcing)
javascript
Copiar
Editar
// shared/EventStore.js
import mongoose from 'mongoose';

mongoose.connect('mongodb://event-store:27017/eventstore');

const eventSchema = new mongoose.Schema({
  eventType: String,
  payload: Object,
  timestamp: { type: Date, default: Date.now }
});

const Event = mongoose.model('Event', eventSchema);

export class EventStore {
  static async saveEvent(eventType, payload) {
    await Event.create({ eventType, payload });
    console.log(`💾 [EventStore] Evento salvo: ${eventType}`);
  }

  static async getEvents(eventType) {
    return await Event.find({ eventType }).lean();
  }
}
✅ Motivo: Armazena eventos para Event Sourcing e recuperação do estado.

8️⃣ Docker-Compose para Infraestrutura
yaml
Copiar
Editar
version: '3.8'

services:
  api-gateway:
    build: ./api-gateway
    ports:
      - "4000:4000"
    depends_on:
      - order-service

  order-service:
    build: ./order-service
    ports:
      - "4001:4001"
    depends_on:
      - event-store

  stock-service:
    build: ./stock-service
    ports:
      - "4002:4002"

  payment-service:
    build: ./payment-service
    ports:
      - "4003:4003"

  notification-service:
    build: ./notification-service
    ports:
      - "4004:4004"

  event-store:
    image: mongo:6.0
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
✅ Motivo: Orquestra todos os serviços com Docker.

9️⃣ Observabilidade: Prometheus, Jaeger e ELK
Prometheus: Coleta métricas via /metrics em cada serviço.
Jaeger: Rastreia cada requisição de ponta a ponta.
ELK Stack: Centraliza logs.
✅ Motivo: Acompanha a saúde e o desempenho dos microserviços.