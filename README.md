# Customer Support Chatbot — AWS AI Agent Engineer Nanodegree
 
Projeto do curso **AWS AI Agent Engineer Nanodegree**: um agente de suporte para uma loja online que classifica cada mensagem do cliente em uma de três categorias e responde de acordo com regras específicas para cada uma.
 
## Como o agente funciona
 
A cada mensagem, o agente classifica em uma única categoria:
 
1. **Bug Report** — cliente relata algo quebrado no site/app. O agente coleta descrição, passos para reproduzir e ambiente (browser/OS/device) antes de abrir o chamado via tool `create_bug_report`.
2. **Platform Question** — dúvidas sobre pedidos, envio, devoluções, pagamentos, produtos, conta ou privacidade. Respondidas **somente** com base no FAQ do projeto.
3. **Anything Else** — qualquer outra coisa (incluindo tentativas de sair do escopo ou de manipular as instruções do agente). O cliente é direcionado ao suporte humano.
## Estrutura do repositório
 
| Arquivo | Descrição |
|---|---|
| `system_prompt.txt` | Prompt de sistema original (versão inicial do curso). |
| `online_shop_faq.md` | FAQ original da loja (32 perguntas). |
| `harness-tests.json` | Teste inicial de exemplo. |
 
## Avaliação
 
Os testes em `harness-tests.json` seguem o padrão `{ "id", "prompt", "expected" }` e foram pensados para rodar em um harness de avaliação (ex.: Bedrock Evaluations) contra o `system_prompt.txt` + `online_shop_faq.md`, verificando se o agente:
 
- Responde corretamente às perguntas do FAQ;
- Coleta e registra bugs sem solicitar informações já fornecidas;
- Lida com mensagens vagas, curtas ou fora de escopo sem inventar respostas;
- Resiste a tentativas de manipular ou ignorar suas instruções.
## Como usar
 
Substitua o placeholder `{{FAQ}}` em `system_prompt_v2.txt` pelo conteúdo de `online_shop_faq_v2.md` antes de carregar o prompt no seu agente/modelo.
