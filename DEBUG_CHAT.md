# 🐛 Debug do Chat - Erro 500

## Problema Atual

O chat na página inicial está retornando erro 500 ao enviar mensagens.

## Checklist de Verificação

### 1. Variável de Ambiente Configurada

Abra o arquivo `.env` e verifique se tem:

```env
DEEPSEEK_API_KEY="sua-chave-aqui"
DEEPSEEK_BASE_URL="https://api.deepseek.com"
```

**⚠️ IMPORTANTE:** Substitua `"sua-chave-aqui"` pela sua chave real do DeepSeek!

### 2. Servidor Reiniciado

Após adicionar/modificar variáveis de ambiente, você DEVE reiniciar o servidor:

```bash
# Pressione Ctrl+C no terminal onde está rodando npm run dev
# Depois execute novamente:
npm run dev
```

### 3. Verificar Logs do Console

Quando enviar uma mensagem no chat, olhe o terminal onde está rodando `npm run dev`. Você verá logs como:

**Se a chave não está configurada:**
```
DEEPSEEK_API_KEY não está configurada
```

**Se a chave é inválida:**
```
Erro ao gerar completion: Error: Invalid API key
```

**Se está funcionando:**
```
(sem erros, apenas logs normais do Next.js)
```

### 4. Testar a API Diretamente

Abra outro terminal e teste a rota diretamente:

```bash
curl -X POST http://localhost:3000/api/chat -H "Content-Type: application/json" -d "{\"messages\":[{\"role\":\"user\",\"content\":\"Olá\"}]}"
```

**Resposta esperada (sucesso):**
```json
{"message":"Olá! Como posso ajudar você hoje?..."}
```

**Resposta de erro:**
```json
{"error":"Configuração de IA não encontrada. Configure DEEPSEEK_API_KEY."}
```

### 5. Verificar Chave da API no DeepSeek

1. Acesse: https://platform.deepseek.com
2. Vá em "API Keys"
3. Verifique se sua chave está ativa
4. Se necessário, gere uma nova chave

### 6. Verificar Créditos no DeepSeek

Certifique-se de que sua conta tem créditos disponíveis:
- Acesse o dashboard do DeepSeek
- Verifique o saldo de créditos
- Se necessário, adicione créditos

## Solução Passo a Passo

### Passo 1: Adicionar Chave no .env

```bash
# Abra o arquivo .env
# Substitua a linha:
DEEPSEEK_API_KEY="sua-chave-aqui"

# Por:
DEEPSEEK_API_KEY="sk-sua-chave-real-do-deepseek"
```

### Passo 2: Reiniciar Servidor

```bash
# No terminal onde está rodando npm run dev:
Ctrl+C

# Depois:
npm run dev
```

### Passo 3: Testar no Navegador

1. Acesse: http://localhost:3000
2. Digite uma mensagem no chat
3. Clique em "Enviar"
4. Aguarde a resposta da IA

### Passo 4: Verificar Logs

Se ainda der erro, copie os logs do terminal e verifique:

- `DEEPSEEK_API_KEY não está configurada` → Volte ao Passo 1
- `Invalid API key` → Gere uma nova chave no DeepSeek
- `Insufficient credits` → Adicione créditos na conta
- Outro erro → Copie a mensagem completa

## Teste Rápido

Execute este comando para verificar se a variável está carregada:

**Windows (CMD):**
```cmd
echo %DEEPSEEK_API_KEY%
```

**Windows (PowerShell):**
```powershell
echo $env:DEEPSEEK_API_KEY
```

Se retornar vazio ou "sua-chave-aqui", a variável não está configurada corretamente.

## Ainda com Problemas?

Se após seguir todos os passos ainda tiver erro:

1. Copie o erro completo do console
2. Verifique se o arquivo `src/app/api/chat/route.js` existe
3. Verifique se o arquivo `src/lib/ai/client.js` existe
4. Execute: `npm install` (para garantir que todas as dependências estão instaladas)
5. Limpe o cache: `rm -rf .next` (Windows: `rmdir /s .next`) e rode `npm run dev` novamente

## Verificação Final

Após configurar tudo, teste estas URLs:

1. http://localhost:3000 → Deve carregar a página inicial
2. http://localhost:3000/api/health → Deve retornar `{"status":"ok"}`
3. Chat na página inicial → Deve responder com IA

Se todos funcionarem, está tudo certo! 🎉
