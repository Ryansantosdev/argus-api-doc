# Documentação Técnica - API Argus

Este repositório contém a documentação interativa da API Argus. O projeto é construído em um layout de página única (SPA), garantindo performance e facilidade de deploy.

## 🚀 Como Executar
O projeto não exige instalação de dependências ou build complexo. Basta abrir o arquivo `index.html` em qualquer navegador moderno.

### Deploy
- **GitHub Pages:** A forma mais rápida. Basta subir o arquivo `index.html` para um repositório e habilitar o GitHub Pages nas configurações.
- **Servidores Estáticos:** Pode ser hospedado em Vercel, Netlify, Firebase Hosting ou qualquer servidor Nginx/Apache.

---

## 🛠 Como Adicionar ou Alterar Endpoints
Para adicionar uma nova funcionalidade ou atualizar um endpoint existente, você deve editar a variável `apiData` dentro da seção `<script>` no arquivo `index.html`.

### Regras de Ouro para não quebrar o Layout:
1. **Vírgulas:** Cada objeto dentro de um array deve ser separado por vírgula. O último item de um array **não** deve ter vírgula após fechar a chave `}`.
2. **IDs Únicos:** O campo `id` deve ser único para cada endpoint para que o sistema de navegação funcione corretamente.
3. **HTML no campo `desc`:** Você pode usar tags HTML básicas (como `<br>`, `<b>`, `<a>`, `<code>`) dentro da descrição para formatar o texto.

### Template de um Novo Endpoint
Copie e cole este bloco dentro do array `items` de uma categoria existente:

```javascript
{
    id: "meu-novo-endpoint",         // ID único (sem espaços ou acentos)
    title: "Título do Endpoint",    // O que aparecerá no menu lateral
    method: "POST",                 // "GET", "POST", etc.
    url: "[https://argus.app.br/apiargus/rota](https://argus.app.br/apiargus/rota)",
    desc: "Breve explicação do endpoint. Use <br> para quebras de linha.",
    hasBody: true,                  // Defina como 'true' se a rota aceita JSON no corpo
    rawPayload: `{\n "exemplo": "valor" \n}`, // JSON de exemplo
    params: [
        { name: "nomeDoParametro", req: "Obrigatório", desc: "Descrição do parâmetro" }
    ],
    // Opcional: Adicione responseParams se quiser mostrar a tabela de retornos
    responseParams: [
        { name: "1", desc: "Sucesso" }
    ]
}