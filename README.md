
# Atividade Avaliativa I - Prática

- Curso - Desenvolvimento de Sistemas
- Unidade Curricular - Desenvolvimento de Sistemas
- Docente - Gustavo Roberto de Souza

## Orientações Gerais
- A avaliação deverá ser realizada individualmente.
- Não é permitido o uso do celular durante a realização da atividade.
- Conceitos de entrada e saída de dados, variáveis, operadores, estruturas condicionais, estruturas de repetição e estruturas de dados.
- A entrega deverá ser feita no AVA. Deve ser enviado apenas o link do repositório do github.

## Passo-a-Passo (Clonar e Entrega)
1. Você deve fazer um fork desse repositório, na parte superior dessa página clique na botão de fork. 
2. Depois disso, você deve clonar o repositório para o seu computador, usando o seguinte comando.
   1. Selecione uma pasta no computador.
   2. Abra o CMD (Terminal).
   3. Execute o seguinte comando `git clone <url_do_repositório>`
3. Abra no seu VS Code a pasta do projeto.
4. Desenvolva os exercícios.
5. Ao finalizar você precisa comittar e enviar novamente para o github suas modificações.
   1. Primeiro precisamos adicionar as alterações ao stage, usando o comando  `git add .`.
   2.  Depois disso, você vai de fato commitar, usando o comando `git commit -m "sua mensagem"`.
   3.  Por fim, você precisa fazer push para o github, com o comando `git push origin master`.
6. Por fim, você deve copiar o link do seu repositório e fazer o envio no AVA. 
   1. Você deve adicionar como comentário na entrega do AVA.

## Situação Problema
Você como desenvolvedor de Softwares voltado para Internet das Coisas (IOT), ficou responsável de criar de comunicação e dados 
entre os dispositivos (Microcontroladores ESP32, Arduino, Demais "coisas") e uma base de dados na qual os posteriormente será
utilizada para tomada de decisão da empresa. Hoje na empresa DesiPescados existe diversos microcontroladores espalhados pela fábrica
de responsáveis por capturar informação de temperatura das câmeras frias e também dos estoques de armazenamento. Todos os dispositivos já estão configurados enviando e recebendo informações ao broker (MQTT).

Nessa primeira versão o fluxo geral de forma esperada é o seguinte:
  - Sempre que um dispositivo novo se conectar, deve se cadastrar no banco de dados, caso não exista.
  - O dispositivo vai enviar em uma determinada frequência o valor de temperatura do local.
    - Ao broker receber essa informação, o NodeRED deve lê-la, e processa-la.
    - Se a temperatura for < de 5º Celsius, está tudo OK, deve armazenar informações no MongoDB
        - Se isso acontecer, deve-ser enviado um comando de volta ao dispositivo para apagar o LED do local para servir de alerta aos funcionários.
    - Se a temperatura for >= que 5º Celsius, Existe um problema, a informação deve ser armazenada no Postgres numa tabela chamada alerta.
      - Se isso acontecer, deve-ser enviado um comando de volta ao dispositivo para acender o LED do local para servir de alerta aos funcionários.


### Topics Sugeridos
- Sempre que o dispositivo se conectar;
  - `device/{id}/info`
- Envio frequente da temperatura do dispositivo;
  - `device/{id}/status/temperatura`
- Envio do comando para o ativar ou desativar LED no dispositivo;
  - `device/{id}/cmd/light`
  
### Objetos Sugeridos

#### Dados Devices
```js
{
  _id: UUID,
  device_id: "ESP32-001",
  model: "ESP32-001",
  firmware: "v1.0.0",
  ip_address: "192.168.0.1",
  location: "Sala de Armazenamento",
  created_at: "10-10-2025",
  updated_at: "10-10-2025",
  light: "on", // "on" ou "off"
  raw: "Dados com JSON.stringfy()"
}
```

#### Dados de Temperatura
```js
{
  device_id: "ESP32-001",
  data: {
    temperature: 5.0,
    unit: "ºC"
  }
  created_at: "10-10-2025",
  updated_at: "10-10-2025",
  raw: "Dados com JSON.stringfy()"
}
```

#### Dados Comando ligar ou desligar LED
```js
{
  device_id: "ESP32-001",
  led: "on"  // "on" ou "off"
}
```