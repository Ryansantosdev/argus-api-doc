# Guia de Manutenção e Expansão - Documentação API Argus

Este documento serve como um manual prático e detalhado para desenvolvedores e mantenedores responsáveis por atualizar, expandir ou modificar o código da **Documentação da API Argus**. 

A aplicação foi projetada como uma **SPA (Single Page Application)** leve, performática e altamente visual, utilizando **HTML5**, **Tailwind CSS (via CDN)** e **Vanilla JavaScript (JavaScript Puro)**.

---

## 1. Visão Geral da Arquitetura do Layout

O layout foi concebido sob o conceito de *Glassmorphism* (efeito de vidro borrado com fundo escuro e glows ambientais). Ele possui um comportamento responsivo duplo rigidamente controlado por breakpoints do Tailwind (`lg:` mapeado para `1024px`).

### Comportamento em Telas Grandes (Desktop >= 1024px)
* **Estrutura:** Dividida em 3 colunas fixas horizontais (`flex-row`).
  1. **Coluna Esquerda (`aside`):** Menu de navegação lateral fixo com busca em tempo real e botão de download da Postman Collection (largura fixa de `320px`).
  2. **Coluna Central (`main`):** Conteúdo técnico contendo títulos, caminhos de endpoints, descrições detalhadas e tabelas de parâmetros (largura fluida `flex-1`).
  3. **Coluna Direita (`aside`):** Área de exemplos práticos contendo abas de seleção de linguagem (Node.js, Python, PHP, cURL) e os blocos de códigos/respostas formatados (`width: 500px`).
* **Rolagem:** O corpo da página (`body`) é fixo e não rola (`lg:overflow-hidden`). Cada coluna possui seu próprio eixo de rolagem vertical independente através da classe `lg:overflow-y-auto`.

### Comportamento em Telas Pequenas (Mobile < 1024px)
* **Estrutura:** Colunas empilhadas verticalmente (`flex-col`).
* **Menu Lateral:** Fica oculto por padrão por trás de um botão **Hambúrguer** flutuante. Quando ativado, abre como um painel flutuante em overlay (`fixed z-50`) que cobre o topo do visor.
* **Rolagem:** A página inteira ganha um comportamento fluido de rolagem vertical (`h-auto overflow-y-auto`). O usuário lê as instruções textuais no miolo da página e, ao continuar descendo, encontra a área de exemplos de código logo abaixo.

---

## 2. Como Adicionar Novas Integrações (Endpoints)

Toda a inteligência e o conteúdo da documentação são baseados em dados. Você **nunca** deve escrever texto cru diretamente nas tags HTML de conteúdo. Todo novo endpoint deve ser inserido no array global JavaScript chamado `apiData`.

### Onde localizar?
Abra o arquivo e procure por: `const apiData = [`.

### Estrutura Padrão de um Objeto de Integração
Cada categoria possui um grupo de `items`. Veja abaixo a anatomia completa de um objeto com todos os parâmetros possíveis:

```javascript
{
    id: "identificador-unico-da-rota",     // String (Sem espaços ou acentos, usado na URL/Navegação)
    title: "Nome Amigável do Endpoint",      // String (Título que aparece no menu e no h3)
    method: "POST",                          // String (GET, POST, INFO ou vazio para guias textuais)
    url: "[https://argus.app.br/api/rota](https://argus.app.br/api/rota)",    // String (URL principal do endpoint)
    getUrl: "https://argus...",              // String (Opcional: Usado se a rota aceitar POST e GET simultâneos)
    acceptsGet: true,                        // Boolean (Opcional: Define exibição de badge duplo POST/GET no menu)
    isWebhook: false,                        // Boolean (Opcional: Se for true, muda o título do bloco para 'BODY JSON')
    desc: "Explicação em texto ou HTML...",  // String (Aceita tags HTML como <br>, <strong>, <a>, <div>)
    hasBody: true,                           // Boolean (Define se o endpoint exige envio de payload JSON)
    rawPayload: `{\n  "campo": 1\n}`,        // String/Template literal (Exemplo de JSON enviado no body)
    rawResponse: `{\n  "status": 1\n}`,      // String/Template literal (Exemplo de JSON retornado pela API)
    params: [                                // Array de Objetos (Tabela de parâmetros de entrada)
        { 
            name: "nome_do_campo", 
            req: "Obrigatório", 
            desc: "Descrição detalhada do campo e comportamento esperado." 
        }
    ],
    responseParams: [                        // Array de Objetos (Opcional: Tabela de dicionário de status/erros)
        { 
            name: "codStatus = 1", 
            desc: "Sinaliza sucesso na operação." 
        }
    ],
    agendaParams: [                          // Array de Objetos (Opcional: Dicionário complementar para agendamentos)
        { 
            name: "idTipoAgenda = 4", 
            desc: "Agenda Individual." 
        }
    ]
}
