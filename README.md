# Planejando automações com n8n usando bons prompts

Desafio criativo da DIO. A automação do exemplo é o agendamento de horários de uma barbearia pelo WhatsApp, baseada num fluxo que eu construí no n8n. O prompt foi montado em três passos, do objetivo às regras, e termina no prompt final do Passo 3.

## Passo 1: a automação desejada

```text
Quero criar uma automação no N8N para agendar, remarcar e cancelar horários de uma barbearia pelo WhatsApp.

Público ou responsável:
Os clientes da barbearia, que marcam o horário pelo WhatsApp a qualquer hora, e o barbeiro, que recebe os avisos e acompanha a agenda.

Resultado esperado:
O cliente pede um horário com as próprias palavras, recebe as opções livres, confirma, e o agendamento fica gravado no banco de dados. O barbeiro é avisado quando um horário é cancelado ou quando o cliente quer falar com ele.
```

## Passo 2: contexto e regras

```text
Ferramentas envolvidas:
n8n, API do WhatsApp (entrada por webhook e envio de mensagem), PostgreSQL (clientes, agendamentos, bloqueios da agenda e tabela de serviços) e um modelo de linguagem para entender o pedido do cliente.

Fluxo desejado:
1. Receber a mensagem do cliente pelo webhook do WhatsApp.
2. Identificar o cliente pelo número e o que ele quer: ver horários, agendar, remarcar, cancelar, saber o preço ou falar com o barbeiro.
3. Converter a data e a hora pedidas ("amanhã às 10h", "sexta à tarde") em uma data exata.
4. Consultar no banco os horários livres, descontando os agendamentos e os bloqueios do barbeiro.
5. Gravar, mudar ou cancelar o agendamento.
6. Responder ao cliente no WhatsApp com o dia da semana, a data e a hora.
7. Avisar o barbeiro no WhatsApp quando houver cancelamento ou pedido de atendimento humano.

Regras importantes:
- Atendimento de segunda a sábado, das 8h às 17h, de hora em hora, com almoço das 12h às 14h. Domingo fechado.
- Recusar data passada e horário de hoje que já passou. Agendar no máximo 30 dias à frente.
- Cada horário aceita um agendamento ativo. Se o mesmo cliente repetir o pedido, responder que ele já está agendado, e não que o horário está ocupado.
- Remarcar e cancelar só acontecem depois de um "sim" do cliente.
- Preço só da tabela de serviços cadastrada. Serviço sem preço na tabela: oferecer chamar o barbeiro.
- Quando o cliente pede para falar com o barbeiro, a automação para de responder esse cliente por 30 minutos e avisa o barbeiro.
- Se o modelo de linguagem ou o envio falhar, o cliente recebe uma mensagem curta de erro (nunca fica sem resposta) e o responsável recebe um alerta.
```

## Passo 3: prompt final

```text
Atue como um especialista em N8N.

Crie uma automação para agendar, remarcar e cancelar horários de uma barbearia pelo WhatsApp, funcionando 24 horas por dia.

Público:
Os clientes da barbearia, que marcam o horário pelo WhatsApp, e o barbeiro, que recebe os avisos e acompanha a agenda.

Ferramentas envolvidas:
n8n, API do WhatsApp (entrada por webhook e envio de mensagem), PostgreSQL (clientes, agendamentos, bloqueios da agenda e tabela de serviços) e um modelo de linguagem para entender o pedido do cliente.

Fluxo:
1. Receber a mensagem do cliente pelo webhook do WhatsApp.
2. Identificar o cliente pelo número e o que ele quer: ver horários, agendar, remarcar, cancelar, saber o preço ou falar com o barbeiro.
3. Converter a data e a hora pedidas ("amanhã às 10h", "sexta à tarde") em uma data exata.
4. Consultar no banco os horários livres, descontando os agendamentos e os bloqueios do barbeiro.
5. Gravar, mudar ou cancelar o agendamento.
6. Responder ao cliente no WhatsApp com o dia da semana, a data e a hora.
7. Avisar o barbeiro no WhatsApp quando houver cancelamento ou pedido de atendimento humano.

Regras:
- Atendimento de segunda a sábado, das 8h às 17h, de hora em hora, com almoço das 12h às 14h. Domingo fechado.
- Recusar data passada e horário de hoje que já passou. Agendar no máximo 30 dias à frente.
- Cada horário aceita um agendamento ativo. Se o mesmo cliente repetir o pedido, responder que ele já está agendado, e não que o horário está ocupado.
- Remarcar e cancelar só acontecem depois de um "sim" do cliente.
- Preço só da tabela de serviços cadastrada. Serviço sem preço na tabela: oferecer chamar o barbeiro.
- Quando o cliente pede para falar com o barbeiro, a automação para de responder esse cliente por 30 minutos e avisa o barbeiro.
- Se o modelo de linguagem ou o envio falhar, o cliente recebe uma mensagem curta de erro (nunca fica sem resposta) e o responsável recebe um alerta.

Explique quais nós do N8N devem ser utilizados e a lógica de funcionamento do workflow. Para cada regra, diga em qual nó ela é aplicada e como testá-la sem enviar mensagem para um cliente de verdade.
```

## Por que o prompt foi escrito assim

- As regras têm número (horário, dias, prazo), então dá para conferir se o workflow gerado obedece cada uma.
- O fluxo está na ordem em que as coisas acontecem, o que ajuda a IA a montar os nós na sequência certa.
- A última frase pede onde cada regra fica e como testá-la, para que a resposta sirva para construir e para conferir o workflow.
