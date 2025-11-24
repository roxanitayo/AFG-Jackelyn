# AFG-Jack
Agente notificador por e-mail com dois abordagem
# 🤖 Agente de Notificação com Azure: Duas abordagems realizadas 

Este documento explora a criação de um **Agente de Notificação** para o envio de e-mails recorrentes. O objetivo é criar uma solução que envie um e-mail de uma conta do Outlook para uma conta pessoal do Gmail segundo a hora estabelecida (considerado horario de Brasilia). O agente tem 1 ação funcional e duas abordagens de implementação no Azure são analisadas:

1. **Agente de IA**  usando um Aplicativo Lógico como ferramenta.

2. **Solução com o Azure Logic App** usando a Recorrência como gatilho principal.



---

## 🛠️ 1. Preparação do Ambiente (Grupo de Recursos)

Foi criado um Grupo de recursos do Azure, onde foram ingresados dados como Nome do Grupo de recursos, Região, tags, entre outros, como é observado nas capturas de tela seguintes.

* **Nome do Grupo de Recursos:** `rg-inter` 
* **Região:** *Brasil* 
* **Tags:** Tags foram usadas para organização, incluindo `owner` e `purpose`.
  <img width="756" height="333" alt="image" src="https://github.com/user-attachments/assets/9ef31e41-cb6c-472b-8288-8de2e1da18fc" />

  <img width="802" height="391" alt="image" src="https://github.com/user-attachments/assets/525e51be-8a92-4db1-8f18-8c5be27ef89a" />

<img width="418" height="433" alt="image" src="https://github.com/user-attachments/assets/9c625d6f-7d47-46f0-ad54-463a9eb5a985" />



---

## 2. Primeira Abordagem: Agente do AI Studio

Inicialmente, explorou-se a possibilidade de construir a automação como um **Agente de IA** no **Azure AI Studio**, seguindo estes passos:

### 2.1. Implantação do Recurso Foundry e do Modelo de IA

Foi criado um recurso do **Azure AI Foundry**  `aif-interr` para hospedar o projeto e o agente. 
<img width="798" height="833" alt="image" src="https://github.com/user-attachments/assets/fba47d48-d6be-45fe-b9ed-14054a0843ab" />

<img width="767" height="403" alt="image" src="https://github.com/user-attachments/assets/150c0d50-0e87-4fbd-98d7-7ea069383d3e" />

<img width="803" height="439" alt="image" src="https://github.com/user-attachments/assets/c18f7684-a638-4d62-9aef-5f31b076798b" />
complentando cada etapa e selecionando "Create"
<img width="580" height="392" alt="image" src="https://github.com/user-attachments/assets/901b3925-deb4-4b34-a42c-6e5f0b74a432" />

 Posteriormente, um modelo de linguagem leve foi selecionado e implementado.
 
 <img width="1089" height="708" alt="image" src="https://github.com/user-attachments/assets/5db3371f-439a-4b32-acbe-8208257db3da" />



* **Modelo selecionado:** **gpt-4o-mini** <img width="609" height="938" alt="image" src="https://github.com/user-attachments/assets/69c86411-a0ac-4a9a-8705-8db642d20364" />


*  Este modelo é suficiente para a orquestração básica desta ferramenta. 
* **Nome da implementação:** `gpt-4o-mini` 

### 2.2. Integração da ferramenta "Enviar e-mail"

Para permitir que o agente execute a ação de envio, uma ferramenta **Logic App** pré-configurada foi integrada para a ação **Enviar e-mail usando o Outlook**.

* **Tipo de ação:** **Azure Logic Apps** 
* **Função:** Um Logic App foi configurado para executar a ação **Enviar um e-mail (V2)** do Outlook, autenticando o serviço de e-mail. 
* **Resultado:** O Agente de IA foi configurado para invocar esta ferramenta. Foi testado com diferentes tarefas e horarios
  


Neste teste, as instruções para o Agente foi que envie uma mensagem motivacional em idioma espanhol com a maneira de falar caracteristica em Lima-Perú para avançar com a escrita do projeto.

<img width="373" height="175" alt="image" src="https://github.com/user-attachments/assets/6f4234f3-75a0-4bbc-8988-24998fab91df" />


A consulta realizada ao agente foi escrita em portugues e o agente cumprió perfeitamente com as instruções realizadas e enviou o email em idioma espanhol, assinando com o apelido indicado

<img width="1322" height="310" alt="image" src="https://github.com/user-attachments/assets/b9b6c00a-a26d-433c-8415-b2486ba6e342" />

Email recebido:

<img width="1488" height="112" alt="image" src="https://github.com/user-attachments/assets/fe601189-a525-4740-86bd-df2dd9168d90" />


---

## 3. Segundo Abordagem : Azure Logic App 

Foi realizada uma segunda abordagem para uma tarefa de notificação ao email por meio de **recorrência agendada** construindo o fluxo de trabalho no **Azure Logic Apps**.

### 3.1. Criando o Logic App 

Um novo recurso de **Logic App** foi criado com o plano **Multilocatário**, foi selecionado essa opção pois, segundo a informação do portal do Azure, é ideal para modelos de pagamento conforme o uso e gatilhos agendados.
<img width="1760" height="562" alt="image" src="https://github.com/user-attachments/assets/fa8119be-f9f7-453d-895a-f37798f6e45e" />

<img width="820" height="856" alt="image" src="https://github.com/user-attachments/assets/62f8bcb7-2830-4753-8199-637071a6ab05" />
Completamos cada etapa e selecionamos "Review + Create"
<img width="648" height="352" alt="image" src="https://github.com/user-attachments/assets/b4246166-43ef-44d9-b1d6-003e5950ab02" />
 Logo, vamos para "Go to Resource" 

### 3.2. Configuração do Gatilho (Recorrência)

O fluxo começa com o gatilho de Recorrência nativo, que atua como o "coração" do agente, definindo a programação de execução.
<img width="1217" height="443" alt="image" src="https://github.com/user-attachments/assets/fc3a3060-7ff3-4e34-a232-d9c7f00f4bd8" />

<img width="306" height="105" alt="image" src="https://github.com/user-attachments/assets/810cad53-c3b7-49ff-a07d-7b4825a4e5f0" />

* Gatilho: Recorrência, com agendamento
* Frequência: Diária (Intervalo: 1, Frequência: Dia)
* Horário de Execução: 1h45 (Nestas horas: 1, Nestes minutos: 45)
* Fuso Horário: O fuso horário de Brasília foi definido para garantir a precisão do horário.
<img width="610" height="546" alt="image" src="https://github.com/user-attachments/assets/cda745de-4b99-4dab-bbca-78d20a19e6bc" />

 


### 3.3. Configuração da Ação $\rightarrow$ Enviar um e-mail (V2)

Como próximo passo, a ação de envio de e-mail foi configurada usando o conector do **Office 365 Outlook**. 

* **Ação:** **Enviar um e-mail (V2)** (Office 365 Outlook) 
* **Autenticação:** Foi vinculada uma conta Outlook . 
* **Destinatário:** `r....o@gmail.com` (meu email de teste de gmail) 
* **Assunto:** `Comece a escritar sua tese!` 
* **Corpo do e-mail:** `cada dia vale ouro!!`
<img width="1052" height="811" alt="image" src="https://github.com/user-attachments/assets/d52f0560-fa3e-4bb3-82d3-29de8901a399" />
<img width="581" height="275" alt="image" src="https://github.com/user-attachments/assets/64ddfee9-25c1-433c-9e65-cc057b3b9534" />

.


---

## 4. Validação e Resultado Final

Após salvar o fluxo de trabalho do Aplicativo Lógico, a execução automática da tarefa agendada foi validada, como é observado na captura de tela seguinte.

* **Validação:** Foi confirmado o recebimento do e-mail na conta do Gmail.

* **Sucesso:** O e-mail foi recebido com sucesso no horário agendado: **1h45**.
 <img width="1358" height="76" alt="image" src="https://github.com/user-attachments/assets/c06203b0-4a95-44a2-9afb-6b87cfed8001" />

---

## 🎓 Conclusão 

A exploração inicial com o Azure Foundry demonstrou a capacidade de um Agente de IA orquestrar serviços do Azure como ferramentas, por meio de Logic Apps. No entanto, a segunda abrodagem permite incorporar tarefas adicionais como recurrencias onde pode ser construído um fluxo de trabajo alternativo.

## Links de referencia

https://ai.azure.com/doc/azure/ai-foundry/openai/how-to/assistants-logic-apps?tid=35a0b968-bbe3-4708-a94e-3e89f67d43a5
https://azure.microsoft.com/en-us/pricing/details/ai-foundry-models/aoai/
https://github.com/Igomes01/azure_frontier_girls_fundamentos_ia?tab=readme-ov-file
https://github.com/Miyake-Diogo/AzureFrontierGirls-AI-Challenge

