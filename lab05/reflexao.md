# Reflexão Final — Roteiro 03

**Aluno:** Arthur Araújo Mendonça
**Disciplina:** Laboratório de Desenvolvimento de Aplicações Móveis e Distribuídas — PUC Minas

---

## Tarefa 1 — XML-RPC

O mecanismo de *stub* e *skeleton* no XML-RPC permite que o cliente invoque funções remotas como se fossem locais, promovendo transparência na comunicação. A serialização XML ocorre no momento em que o cliente faz a chamada via `ServerProxy`, sendo transmitida pelo protocolo HTTP. O objeto `xmlrpc.client.Fault` representa exceções propagadas do servidor para o cliente, funcionando como um canal padronizado para erros remotos, diferente das exceções convencionais do Python, pois carrega informações serializadas e padronizadas para o contexto RPC. O método `system.listMethods()` demonstra introspecção remota, relacionada à transparência de acesso da ISO/RM-ODP, pois permite ao cliente descobrir dinamicamente as operações disponíveis no servidor.

## Tarefa 2 — Stub Manual

As quatro etapas do ciclo RPC descritas por Birrell & Nelson (1984) estão explicitamente implementadas: *marshalling* (serialização para JSON), *transmissão* (envio via socket TCP), *dispatching* (seleção e execução da função no servidor) e *unmarshalling* (desserialização da resposta). O uso de JSON, embora simples e legível, implica maior overhead de parsing e payloads maiores em comparação ao Protobuf, tornando-se menos eficiente para sistemas de alto volume. O *framing* (4 bytes de tamanho) é necessário porque o TCP é um protocolo orientado a fluxo, sem delimitação de mensagens; sem framing, seria impossível distinguir onde uma mensagem termina e outra começa.

## Tarefa 3 — REST com Flask

O uso do código de status `201 Created` em `POST /produtos` é fundamental para que clientes e intermediários (como proxies de cache) entendam que um novo recurso foi criado, permitindo otimizações e comportamentos corretos conforme o protocolo HTTP. Para que o servidor fosse genuinamente stateless, seria necessário persistir o estado dos produtos em um banco de dados externo, eliminando dependências de memória local. Comparando as abordagens, o REST deixa o contrato mais explícito e padronizado via interface uniforme, enquanto o RPC depende de convenções e documentação para alinhar expectativas entre cliente e servidor.

## Tarefa 4 — gRPC

O contrato explícito via `.proto` no gRPC garante que cliente e servidor estejam sempre sincronizados quanto à estrutura e tipos das mensagens, evitando erros de integração. Alterações não compatíveis no contrato, como mudar o tipo de um campo, resultam em falhas de compilação e integração, ao contrário do REST, onde tais erros podem passar despercebidos até o runtime. O mecanismo de status do gRPC é semanticamente mais rico que o HTTP puro, pois permite granularidade maior de códigos e detalhes de erro. O `grpc.RpcError` oferece informações estruturadas e padronizadas, superando o `xmlrpc.client.Fault` em robustez e clareza.

## Tarefa 5 — Comparativo

A tipagem forte do `.proto` é uma vantagem em microserviços internos, pois garante contratos rígidos, integração segura e detecção precoce de erros, promovendo robustez e eficiência. Em APIs públicas, essa rigidez pode ser uma barreira, pois limita a flexibilidade e a evolução do serviço, dificultando a adoção por clientes diversos e exigindo atualizações sincronizadas.

---

**Considerações Finais:**

A experiência prática com os três paradigmas evidencia que a escolha da tecnologia deve ser orientada pelo contexto e pelos requisitos do sistema. O domínio conceitual e técnico dessas abordagens é fundamental para projetar soluções distribuídas robustas, escaláveis e alinhadas às melhores práticas da Engenharia de Software.