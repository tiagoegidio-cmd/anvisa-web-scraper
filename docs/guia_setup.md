# ⚙️ Guia de Setup - ANVISA Web Scraper

> **Instruções passo-a-passo para configurar e executar o scraper em 10 minutos**

## 🎯 Pré-requisitos

### ✅ **Requisitos Obrigatórios**
- [ ] Conexão com internet estável
- [ ] Navegador web atualizado (Chrome, Firefox, Safari)
- [ ] Email válido para conta Apify

### 📋 **Conhecimentos Recomendados**
- [ ] Básico de JSON (para configurar parâmetros)
- [ ] Conceitos básicos de web scraping (opcional)
- [ ] Noções de medicamentos e ANVISA (útil, mas não obrigatório)

---

## 🚀 Setup Rápido (10 minutos)

### **PASSO 1: Criar Conta Apify** ⏱️ *2 minutos*

1. **Acesse:** https://apify.com/
2. **Clique em:** "Sign up for free"
3. **Preencha:**
   - Email válido
   - Senha segura
   - Nome completo
4. **Confirme** o email recebido
5. **Faça login** na plataforma

> 💡 **Dica:** O plano gratuito inclui $5 em créditos mensais, suficiente para centenas de consultas!

### **PASSO 2: Criar Novo Actor** ⏱️ *3 minutos*

1. **No dashboard Apify:**
   - Clique em **"Actors"** no menu lateral
   - Clique em **"Create new"**
   - Selecione **"From scratch"**

2. **Configurar Actor:**
   ```
   Name: ANVISA-Medicamentos-Scraper
   Title: ANVISA Web Scraper - Medicamentos
   Description: Scraper para consulta automática de medicamentos na ANVISA
   Categories: Health, Data Extraction, Government
   ```

3. **Clique em "Create"**

### **PASSO 3: Instalar o Código** ⏱️ *2 minutos*

1. **Na aba "Source code":**
   - Apague todo o código existente
   - **Copie e cole** o código do arquivo [Codigo-Principal.js](Codigo-Principal.js)

2. **Configurar Package.json:**
   ```json
   {
     "name": "anvisa-web-scraper",
     "version": "1.0.0",
     "description": "ANVISA medicamentos web scraper",
     "main": "main.js",
     "dependencies": {
       "apify": "^3.0.0",
       "crawlee": "^3.0.0"
     }
   }
   ```

3. **Salvar:** Ctrl+S ou botão "Save"

### **PASSO 4: Configurar Input Schema** ⏱️ *1 minuto*

1. **Na aba "Input schema":**
   ```json
   {
     "title": "ANVISA Web Scraper Configuration",
     "type": "object",
     "schemaVersion": 1,
     "properties": {
       "nomeProduto": {
         "title": "Nome do Produto",
         "type": "string",
         "description": "Nome ou parte do nome do medicamento (obrigatório)",
         "editor": "textfield",
         "example": "dipirona"
       },
       "tipoRegularizacao": {
         "title": "Tipo de Regularização",
         "type": "string",
         "description": "Tipo de regularização do medicamento",
         "default": "MEDICAMENTO REGISTRADO",
         "enum": ["MEDICAMENTO REGISTRADO", "MEDICAMENTO NOTIFICADO"],
         "enumTitles": ["Medicamento Registrado", "Medicamento Notificado"]
       },
       "cnpjDetentor": {
         "title": "CNPJ do Detentor",
         "type": "string",
         "description": "CNPJ da empresa (formato: 12.345.678/0001-90)",
         "editor": "textfield",
         "pattern": "^\\d{2}\\.\\d{3}\\.\\d{3}/\\d{4}-\\d{2}$"
       },
       "dataRegularizacao": {
         "title": "Data da Regularização",
         "type": "string",
         "description": "Data no formato DD/MM/AAAA",
         "editor": "textfield",
         "pattern": "^\\d{2}/\\d{2}/\\d{4}$"
       },
       "situacaoRegularizacao": {
         "title": "Situação da Regularização",
         "type": "string",
         "description": "Situação atual do registro",
         "default": "Ativo",
         "enum": ["Ativo", "Inativo", "Ambos"],
         "enumTitles": ["Apenas Ativos", "Apenas Inativos", "Todos"]
       },
       "maxResults": {
         "title": "Máximo de Resultados",
         "type": "integer",
         "description": "Número máximo de registros a processar",
         "default": 100,
         "minimum": 1,
         "maximum": 1000
       },
       "incluirDetalhes": {
         "title": "Incluir Detalhes Individuais",
         "type": "boolean",
         "description": "Extrair informações detalhadas de cada registro",
         "default": true
       },
       "estrategiasCustomizadas": {
         "title": "Estratégias Personalizadas",
         "type": "array",
         "description": "Termos alternativos para busca",
         "editor": "stringList",
         "items": {
           "type": "string"
         }
       },
       "criteriosExclusao": {
         "title": "Critérios de Exclusão",
         "type": "array",
         "description": "Termos para excluir dos resultados",
         "editor": "stringList",
         "items": {
           "type": "string"
         }
       }
     },
     "required": ["nomeProduto"]
   }
   ```

### **PASSO 5: Primeiro Teste** ⏱️ *2 minutos*

1. **Clique em "Save & Build"**
2. **Aguarde** o build completar (≈30 segundos)
3. **Clique em "Start"**
4. **Configure input de teste:**
   ```json
   {
     "nomeProduto": "dipirona",
     "situacaoRegularizacao": "Ativo",
     "maxResults": 10
   }
   ```
5. **Clique em "Start"** novamente

> 🎉 **Sucesso!** Se tudo funcionou, você verá logs aparecendo e dados sendo extraídos!

---

## 🔧 Configuração Avançada

### 🛠️ **Configurações de Performance**

#### **Memory e Timeout**
```json
{
  "memoryMbytes": 2048,
  "timeoutSecs": 3600,
  "buildTimeoutSecs": 600
}
```

#### **Proxy Configuration** (Opcional)
```json
{
  "useApifyProxy": true,
  "apifyProxyGroups": ["RESIDENTIAL"],
  "apifyProxyCountry": "BR"
}
```

### 🔒 **Configurações de Segurança**

#### **Environment Variables**
Para uso em produção, configure variáveis de ambiente:

1. **Na aba "Environment":**
   ```
   ANVISA_USER_AGENT=ANVISA-Research-Bot/1.0
   MAX_CONCURRENT_REQUESTS=1
   REQUEST_DELAY_MS=2000
   ```

#### **Input Validation**
```json
{
  "validateInput": true,
  "sanitizeInput": true,
  "maxExecutionTime": 3600
}
```

### 📊 **Configurações de Output**

#### **Dataset Settings**
```json
{
  "cleanItemCount": 100,
  "format": "json",
  "simplified": false,
  "skipHidden": true
}
```

#### **Storage Configuration**
```json
{
  "defaultDatasetId": "anvisa-medicamentos",
  "retentionDays": 30,
  "compressionType": "gzip"
}
```

---

## 🌐 Execução via API

### 🔗 **API Endpoints**

#### **Iniciar Execução**
```bash
curl -X POST \
  https://api.apify.com/v2/acts/{ACTOR_ID}/runs \
  -H "Authorization: Bearer {API_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "nomeProduto": "dipirona",
    "situacaoRegularizacao": "Ativo",
    "maxResults": 50
  }'
```

#### **Obter Resultados**
```bash
curl -X GET \
  https://api.apify.com/v2/datasets/{DATASET_ID}/items \
  -H "Authorization: Bearer {API_TOKEN}"
```

### 📱 **Integração JavaScript**

```javascript
import { ApifyApi } from 'apify-client';

const client = new ApifyApi({ token: 'seu_token_aqui' });

// Executar scraper
const run = await client.actor('seu_actor_id').call({
  nomeProduto: 'ibuprofeno',
  situacaoRegularizacao: 'Ativo',
  maxResults: 100
});

// Obter resultados
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(`Encontrados ${items.length} medicamentos`);
```

### 🐍 **Integração Python**

```python
from apify_client import ApifyClient

client = ApifyClient("seu_token_aqui")

# Executar scraper
run = client.actor("seu_actor_id").call(run_input={
    "nomeProduto": "paracetamol",
    "situacaoRegularizacao": "Ativo",
    "maxResults": 100
})

# Obter resultados
dataset_items = client.dataset(run["defaultDatasetId"]).list_items().items
print(f"Encontrados {len(dataset_items)} medicamentos")
```

---

## 🔄 Automação e Agendamento

### ⏰ **Scheduler Setup**

1. **Na aba "Schedules":**
   - Clique em "Create schedule"
   - **Name:** "Monitoramento Diário ANVISA"
   - **Cron:** `0 8 * * *` (todo dia às 8h)

2. **Input para execução agendada:**
   ```json
   {
     "nomeProduto": "dipirona",
     "situacaoRegularizacao": "Ativo",
     "maxResults": 500,
     "incluirDetalhes": true
   }
   ```

### 📨 **Notificações**

#### **Webhook Configuration**
```json
{
  "webhookUrl": "https://seu-site.com/webhook/anvisa",
  "httpMethod": "POST",
  "eventTypes": ["ACTOR.RUN.SUCCEEDED", "ACTOR.RUN.FAILED"]
}
```

#### **Email Alerts**
Configure alertas por email em **Settings → Notifications**:
- ✅ Run succeeded
- ✅ Run failed  
- ✅ Run timed out
- ✅ Weekly summary

---

## 🔍 Monitoramento e Logs

### 📊 **Dashboard Monitoring**

1. **Acesse:** Actor → Runs
2. **Métricas importantes:**
   - ✅ **Success rate** (taxa de sucesso)
   - ⏱️ **Average duration** (tempo médio)
   - 💰 **Cost per run** (custo por execução)
   - 📈 **Items scraped** (itens extraídos)

### 🐛 **Debug e Troubleshooting**

#### **Logs Detalhados**
```javascript
// No código, adicionar logs customizados:
console.log('🔍 Iniciando busca para:', nomeProduto);
console.log('📊 Registros encontrados:', resultados.length);
console.log('⚠️ Avisos:', warnings);
```

#### **Screenshots de Erro**
```javascript
// Captura automática em caso de erro
await page.screenshot({ 
  path: `erro_${Date.now()}.png`, 
  fullPage: true 
});
```

#### **Performance Monitoring**
```javascript
const metricas = {
  inicioExecucao: new Date().toISOString(),
  tempoTotal: 0,
  registrosProcessados: 0,
  erros: 0
};
```

---

## 🛡️ Boas Práticas

### ⚡ **Performance**

#### **Rate Limiting**
```javascript
// Sempre configurar delays apropriados
const CONFIG = {
  maxConcurrency: 1,           // Máximo 1 requisição simultânea
  requestDelay: 2000,          // 2 segundos entre requests
  navigationTimeout: 60000,    // 60 segundos timeout
  retries: 3                   // 3 tentativas em caso de erro
};
```

#### **Memory Management**
```javascript
// Limpar recursos após uso
if (newPage) {
  await newPage.close();
}

// Evitar vazamentos de memória
registrosUnicos.clear();
resultadosConsolidados.length = 0;
```

### 🔒 **Segurança**

#### **Respeitar Rate Limits**
- ❌ **Nunca** usar mais de 1 requisição simultânea
- ✅ **Sempre** aguardar entre requests
- ✅ **Monitorar** resposta do servidor
- ✅ **Parar** se detectar bloqueio

#### **User Agent Apropriado**
```javascript
const userAgent = 'ANVISA-Research-Bot/1.0 (+https://seu-site.com/bot)';
await page.setUserAgent(userAgent);
```

#### **Headers Éticos**
```javascript
await page.setExtraHTTPHeaders({
  'Accept-Language': 'pt-BR,pt;q=0.9,en;q=0.8',
  'Accept-Encoding': 'gzip, deflate, br',
  'DNT': '1',
  'Connection': 'keep-alive'
});
```

### 📋 **Compliance**

#### **Termos de Uso**
- ✅ Usar apenas para pesquisa legítima
- ✅ Não sobrecarregar servidores
- ✅ Atribuir fonte dos dados
- ✅ Respeitar robots.txt

#### **Transparência**
- ✅ Identificar claramente o bot
- ✅ Fornecer contato válido
- ✅ Documentar uso dos dados
- ✅ Manter logs de acesso

---

## 🚨 Resolução de Problemas

### ❌ **Problemas Comuns**

#### **"Actor build failed"**
```bash
# Solução:
1. Verificar sintaxe do código JavaScript
2. Conferir package.json está correto
3. Limpar cache: Settings → Clear build cache
4. Tentar build novamente
```

#### **"No results found"**
```bash
# Solução:
1. Verificar se site ANVISA está acessível
2. Conferir parâmetros de busca
3. Testar com termo mais genérico (ex: "dipirona")
4. Verificar logs para erros específicos
```

#### **"Request timeout"**
```bash
# Solução:
1. Aumentar timeout nas configurações
2. Verificar conexão de internet
3. Tentar em horário de menor tráfego
4. Reduzir maxResults temporariamente
```

#### **"Memory exceeded"**
```bash
# Solução:
1. Aumentar memória alocada (Settings → Memory)
2. Reduzir maxResults
3. Desabilitar incluirDetalhes temporariamente
4. Implementar processamento em lotes
```

### 🔧 **Debugging Avançado**

#### **Habilitar Debug Mode**
```json
{
  "debugMode": true,
  "verbose": true,
  "saveScreenshots": true,
  "logLevel": "debug"
}
```

#### **Análise de Logs**
```bash
# Buscar por padrões nos logs:
- "✅" = Operações bem-sucedidas
- "⚠️" = Avisos (não críticos)
- "❌" = Erros críticos
- "📊" = Métricas e estatísticas
```

---

## 🎯 Próximos Passos

### 📖 **Continue Aprendendo**
1. **[Exemplos Práticos](Exemplos-Uso.md)** - Casos de uso reais
2. **[Troubleshooting](Troubleshooting.md)** - Problemas específicos
3. **[Caso de Estudo](Caso-Estudo-Dipirona.md)** - Análise completa

### 🚀 **Otimizações Futuras**
- Implementar cache de resultados
- Adicionar análise de tendências
- Criar dashboard personalizado
- Integrar com banco de dados

### 🤝 **Comunidade**
- Participe das discussões
- Reporte bugs encontrados
- Sugira novas funcionalidades
- Compartilhe casos de uso

---

> **🎉 Parabéns!** Seu ANVISA Web Scraper está configurado e funcionando. Agora você pode automatizar consultas que antes levavam horas em poucos minutos!

**💡 Dica Final:** Comece com buscas simples e vá aumentando a complexidade conforme ganha experiência com a ferramenta.
