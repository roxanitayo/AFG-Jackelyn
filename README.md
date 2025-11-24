# AFG-Jack
Agente notificador por e-mail com dois abordagem
# 🤖 Agente de Notificação com Azure: Duas abordagems realizadas 

Este documento explora a criação de um **Agente de Notificação** para o envio de e-mails recorrentes, o agente tem 1 ação funcional e duas abordagens de implementação no Azure são analisadas:

1. **Agente de IA**  usando um Aplicativo Lógico como ferramenta.

2. **Solução com o Azure Logic App** usando a Recorrência como gatilho principal.

O objetivo é criar uma solução que envie um e-mail de uma conta do Outlook para uma conta pessoal do Gmail segundo a hora estabelecida (considerado horario de Brasilia).

---

## 🛠️ 1. Preparação do Ambiente (Grupo de Recursos)

Foi criado um Grupo de recursos do Azure, onde foram ingresados dados como Nome do Grupo de recursos, Região, tags, entre outros, como é observado nas capturas de tela seguintes.

* **Nome do Grupo de Recursos:** `rg-inter` 
* **Região:** *Brasil* 
* **Tags:** Tags foram usadas para organização, incluindo `owner` e `purpose`.
  
<img width="886" height="491" alt="image" src="https://github.com/user-attachments/assets/714179c8-4d7a-4367-85cb-69ea493a9f8e" />
<img width="886" height="590" alt="image" src="https://github.com/user-attachments/assets/483b639b-83f8-419e-9219-489bcf92adee" />
<img width="769" height="684" alt="image" src="https://github.com/user-attachments/assets/d2b412ca-8b09-4235-ad82-e5704be072b2" />


---

## 2. Abordagem A: Agente do AI Studio

Inicialmente, explorou-se a possibilidade de construir a automação como um **Agente de IA** no **Azure AI Studio**, seguindo estes passos:

### 2.1. Implantação do Recurso Foundry e do Modelo de IA

Foi criado um recurso do **Azure AI Foundry**  `aif-inter` para hospedar o projeto e o agente. 
<img width="886" height="722" alt="image" src="https://github.com/user-attachments/assets/7f08b16a-b603-453e-bf55-a5768d1eaa68" />
<img width="886" height="521" alt="image" src="https://github.com/user-attachments/assets/c729f45e-af7f-48a3-9d99-7dbe2b65ebad" />
<img width="886" height="695" alt="image" src="https://github.com/user-attachments/assets/c3e5d891-6ca7-4d15-b5fb-bacb6667111e" />

 Posteriormente, um modelo de linguagem leve foi selecionado e implementado.
 
 <img width="886" height="461" alt="image" src="https://github.com/user-attachments/assets/44cc415b-8fdf-4533-8107-67486543d31f" />


* **Modelo selecionado:** **gpt-4o-mini** <img width="886" height="1168" alt="image" src="https://github.com/user-attachments/assets/fd409106-4ab6-4108-936e-748e471be7a7" />

*  Este modelo é suficiente para a orquestração básica desta ferramenta. 
* **Nome da implementação:** `gpt-4o-mini` 

### 2.2. Integração da ferramenta "Enviar e-mail"

Para permitir que o agente execute a ação de envio, uma ferramenta **Logic App** pré-configurada foi integrada para a ação **Enviar e-mail usando o Outlook**.

* **Tipo de ação:** **Azure Logic Apps** 
* **Função:** Um Logic App foi configurado para executar a ação **Enviar um e-mail (V2)** do Outlook, autenticando o serviço de e-mail. 
* **Resultado:** O Agente de IA foi configurado para invocar esta ferramenta. Foi testado com diferentes tarefas e horarios
* 
<img width="886" height="404" alt="image" src="https://github.com/user-attachments/assets/a70c78b4-9357-42bd-b19a-e6835794a547" />
<img width="886" height="775" alt="image" src="https://github.com/user-attachments/assets/96ebe301-9099-42f8-90ed-217fd74e2fb7" />
<img width="886" height="458" alt="image" src="https://github.com/user-attachments/assets/f1885879-7c10-41fa-a0c0-690f9d36f5d3" />
<img width="886" height="862" alt="image" src="https://github.com/user-attachments/assets/28abed3f-ed55-48ef-b968-942a9def1799" />


Neste teste, as instruções para o Agente foi que envie uma mensagem motivacional em idioma espanhol com as girias de Lima-Perú para avançar com a escrita do projeto.

<img width="366" height="165" alt="image" src="https://github.com/user-attachments/assets/096ce333-74aa-4b76-a518-53711aac7da8" />

A consulta realizada ao agente foi escrita em portugues e o agente cumprió perfeitamente com as instruções realizadas e enviou o email em idioma espanhol, assinando com o apelido indicado

<img width="1307" height="365" alt="image" src="https://github.com/user-attachments/assets/d31e4c58-9570-40bb-8190-5363a278363f" />

Email recebido:

<img width="1488" height="112" alt="image" src="https://github.com/user-attachments/assets/fe601189-a525-4740-86bd-df2dd9168d90" />


---

## 3. Abordagem B: Azure Logic App (Segundo abordagem)

Foi realizada uma segunda abordagem para uma tarefa de notificação ao email por meio de **recorrência agendada** construindo o fluxo de trabalho no **Azure Logic Apps**.

### 3.1. Criando o Logic App 

Um novo recurso de **Logic App** foi criado com o plano **Multilocatário**, foi selecionado essa opção pois, segundo a informação do portal do Azure, é ideal para modelos de pagamento conforme o uso e gatilhos agendados.
<img width="886" height="390" alt="image" src="https://github.com/user-attachments/assets/322efe79-6cc1-41cf-9b77-0ab8965f0ce4" />
<img width="886" height="933" alt="image" src="https://github.com/user-attachments/assets/5532558c-e6d9-4217-b665-a185217ca7dd" />


### 3.2. Configuração do Gatilho (Recorrência)

O fluxo começa com o gatilho de Recorrência nativo, que atua como o "coração" do agente, definindo a programação de execução.

<img width="886" height="928" alt="image" src="https://github.com/user-attachments/assets/ae1cdf03-3f2c-4ac5-81a2-41abde0d72cc" />

* Gatilho: Recorrência, com agendamento
* Frequência: Diária (Intervalo: 1, Frequência: Dia)
* Horário de Execução: 1h45 (Nestas horas: 1, Nestes minutos: 45)
* Fuso Horário: O fuso horário de Brasília foi definido para garantir a precisão do horário.

  <img width="1896" height="777" alt="image" src="https://github.com/user-attachments/assets/5d383cf6-8630-4d66-9ef2-4c5126e55002" />
 



### 3.3. Configuração da Ação $\rightarrow$ Enviar um e-mail (V2)

Como próximo passo, a ação de envio de e-mail foi configurada usando o conector do **Office 365 Outlook**. 

* **Ação:** **Enviar um e-mail (V2)** (Office 365 Outlook) 
* **Autenticação:** Foi vinculada uma conta Outlook . 
* **Destinatário:** `r....o@gmail.com` (meu email de teste de gmail) 
* **Assunto:** `Comece a escritar sua tese!` 
* **Corpo do e-mail:** `cada dia vale ouro!!`
<img width="886" height="915" alt="image" src="https://github.com/user-attachments/assets/cf1317d3-758a-421d-ba93-9a760a2ad85c" />
<img width="886" height="564" alt="image" src="https://github.com/user-attachments/assets/c78b7579-49d7-4940-bf01-fd95af409d25" />
<img width="886" height="468" alt="image" src="https://github.com/user-attachments/assets/5819120c-cb3e-4d0f-b36b-c0a472df4664" />

---

## 4. Validação e Resultado Final

Após salvar o fluxo de trabalho do Aplicativo Lógico, a execução automática da tarefa agendada foi validada, como é observado na captura de tela seguinte.

* **Validação:** Foi confirmado o recebimento do e-mail na conta do Gmail.

* **Sucesso:** O e-mail foi recebido com sucesso no horário agendado: **1h45**. 
<img width="886" height="50" alt="image" src="https://github.com/user-attachments/assets/a0325b95-ee9e-479b-83c6-c1e397ccefe7" />

---

## 🎓 Conclusão 

A exploração inicial com o Azure Foundry demonstrou a capacidade de um Agente de IA orquestrar serviços do Azure como ferramentas, por meio de Logic Apps. No entanto, a segunda abrodagem permite incorporar tarefas adicionais como recurrencias onde pode ser construído um fluxo de trabajo alternativo.

## Links de referencia

https://ai.azure.com/doc/azure/ai-foundry/openai/how-to/assistants-logic-apps?tid=35a0b968-bbe3-4708-a94e-3e89f67d43a5
https://azure.microsoft.com/en-us/pricing/details/ai-foundry-models/aoai/
https://github.com/Igomes01/azure_frontier_girls_fundamentos_ia?tab=readme-ov-file
https://github.com/Miyake-Diogo/AzureFrontierGirls-AI-Challenge

