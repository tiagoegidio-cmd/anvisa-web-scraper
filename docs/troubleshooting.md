# 🛠️ Troubleshooting - ANVISA Web Scraper

> **Soluções rápidas para problemas comuns e debugging avançado**

## 🚨 Diagnóstico Rápido

### 🔍 **Identificar o Problema**

| Sintoma | Categoria | Urgência |
|---------|-----------|----------|
| Actor não inicia | 🔧 **Configuração** | 🔴 Alta |
| Sem resultados encontrados | 📊 **Dados** | 🟡 Média |
| Timeout/Lentidão | ⚡ **Performance** | 🟡 Média |
| Resultados incorretos | 🎯 **Qualidade** | 🟡 Média |
| Erro de memória | 💾 **Recursos** | 🔴 Alta |
| Site ANVISA inacessível | 🌐 **Externa** | 🟠 Depende |

### ⚡ **Soluções Rápidas (< 2 min)**

#### **❌ "Actor failed to start"**
```bash
✅ SOLUÇÃO RÁPIDA:
1. Verificar se código foi salvo (Ctrl+S)
2. Fazer rebuild: Settings → Build → Force rebuild
3. Verificar sintaxe JSON no input
```

#### **❌ "No results found"**
```bash
✅ SOLUÇÃO RÁPIDA:
1. Testar com termo mais genérico: "dipirona"
2. Remover filtros extras temporariamente
3. Verificar se ANVISA está online
```

#### **❌ "Request timeout"**
```bash
✅ SOLUÇÃO RÁPIDA:
1. Reduzir maxResults para 20
2. Tentar novamente em 5 minutos
3. Desabilitar incluirDetalhes temporariamente
```

---

## 🔧 Problemas de Configuração

### ❌ **Build Failures**

#### **Problema: "Package.json invalid"**
```json
// ❌ ERRADO:
{
  "dependencies": {
    "apify": "latest"  // Versão genérica
  }
}

// ✅ CORRETO:
{
  "name": "anvisa-scraper",
  "version": "1.0.0",
  "dependencies": {
    "apify": "^3.0.0",
    "crawlee": "^3.0.0"
  }
}
```

#### **Problema: "Syntax Error in main.js"**
```javascript
// ❌ ERROS COMUNS:
import { Actor } from 'apify'  // Faltou ponto-e-vírgula
const input = await Actor.getInput()  // Fora do main()

// ✅ CORRETO:
import { Actor } from 'apify';

await Actor.main(async () => {
    const input = await Actor.getInput();
    // resto do código...
});
```

#### **Problema: "Input schema validation failed"**
```json
// ❌ ERRO: Campo obrigatório ausente
{
  "situacaoRegularizacao": "Ativo"
}

// ✅ CORRETO: Incluir campo obrigatório
{
  "nomeProduto": "dipirona",
  "situacaoRegularizacao": "Ativo"
}
```

### ⚙️ **Configurações de Actor**

#### **Memory Issues**
```json
// Configuração recomendada por cenário:

// Busca simples (< 50 resultados)
{
  "memoryMbytes": 1024,
  "timeoutSecs": 1800
}

// Análise média (50-200 resultados)
{
  "memoryMbytes": 2048,
  "timeoutSecs": 3600
}

// Análise completa (> 200 resultados)
{
  "memoryMbytes": 4096,
  "timeoutSecs": 7200
}
```

#### **Timeout Settings**
```javascript
// No código, ajustar timeouts por operação:
const CONFIG = {
    navigationTimeout: 60000,      // 60s para carregar página
    selectorTimeout: 30000,        // 30s para encontrar elementos
    submitTimeout: 45000,          // 45s para processar busca
    detailsTimeout: 90000          // 90s para páginas individuais
};
```

---

## 📊 Problemas de Dados

### ❌ **Nenhum Resultado Encontrado**

#### **Diagnóstico: Verificar termo de busca**
```bash
# Teste progressivo:
1. Termo muito específico: "dipirona sódica injetável 500mg"
   → Tentar: "dipirona injetável"
   → Tentar: "dipirona"

2. Medicamento inexistente: "remédio-fantasma"
   → Verificar grafia no site da ANVISA manualmente

3. Filtros muito restritivos: CNPJ + Data + Situação
   → Remover filtros um por vez
```

#### **Solução: Estratégia de busca progressiva**
```json
// PASSO 1: Busca básica
{
  "nomeProduto": "ibuprofeno",
  "maxResults": 10
}

// PASSO 2: Se encontrou resultados, refinar
{
  "nomeProduto": "ibuprofeno",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 50
}

// PASSO 3: Se ainda sem resultados, ampliar
{
  "nomeProduto": "ibuprofeno",
  "estrategiasCustomizadas": ["ibuprofen", "ibuprofeno sódico"],
  "maxResults": 100
}
```

### ❌ **Resultados Duplicados**

#### **Problema: Mesmo registro aparece múltiplas vezes**
```javascript
// Verificar se o sistema de deduplicação está funcionando:
const registrosUnicos = new Set();
const chaveUnica = resultado.textoCompleto.replace(/\s+/g, ' ').trim();

if (registrosUnicos.has(chaveUnica)) {
    console.log('⚠️ Duplicata detectada:', chaveUnica);
    return; // Pular registro
}
```

#### **Solução: Melhorar chave única**
```javascript
// Chave mais robusta para deduplicação:
function gerarChaveUnica(registro) {
    const elementos = [
        registro.dados['Produto']?.toLowerCase(),
        registro.dados['Empresa']?.toLowerCase(),
        registro.dados['Nº Registro']
    ].filter(Boolean);
    
    return elementos.join('|').replace(/\s+/g, ' ').trim();
}
```

### ❌ **Dados Incompletos**

#### **Problema: Campos importantes vazios**
```javascript
// Diagnóstico: Verificar mapeamento de colunas
console.log('Cabeçalhos encontrados:', cabecalhos);
console.log('Células por linha:', cells.length);
console.log('Mapeamento atual:', registro.dados);
```

#### **Solução: Mapeamento robusto**
```javascript
// Mapeamento flexível de colunas:
function mapearDados(cells, cabecalhos) {
    const dados = {};
    const mapeamento = {
        'produto': ['produto', 'nome', 'medicamento'],
        'empresa': ['empresa', 'detentor', 'fabricante'],
        'registro': ['registro', 'número', 'nº registro'],
        'situacao': ['situação', 'status', 'ativo']
    };
    
    cells.forEach((cell, index) => {
        const valor = cell.innerText.trim();
        const cabecalho = cabecalhos[index]?.toLowerCase() || '';
        
        // Buscar mapeamento
        for (const [chave, aliases] of Object.entries(mapeamento)) {
            if (aliases.some(alias => cabecalho.includes(alias))) {
                dados[chave] = valor;
                break;
            }
        }
        
        // Fallback: usar índice
        dados[`coluna_${index + 1}`] = valor;
    });
    
    return dados;
}
```

---

## ⚡ Problemas de Performance

### ❌ **Execução Muito Lenta**

#### **Diagnóstico: Identificar gargalos**
```javascript
// Adicionar timestamps para medir performance:
const metricas = {
    inicioFormulario: Date.now(),
    inicioExtracao: null,
    inicioDetalhes: null,
    fim: null
};

// Após cada etapa:
metricas.inicioExtracao = Date.now();
console.log(`⏱️ Formulário: ${metricas.inicioExtracao - metricas.inicioFormulario}ms`);
```

#### **Soluções por gargalo identificado:**

##### **Lentidão no preenchimento do formulário**
```javascript
// Otimização: Seletores mais específicos
const SELETORES_OTIMIZADOS = {
    nome: 'input[name="nomeProduto"]',           // Mais específico
    tipo: 'input[value="MEDICAMENTO REGISTRADO"]', // Exato
    situacao: 'input[value="Ativo"]'             // Direto
};

// Timeout reduzido para seletores conhecidos:
await page.waitForSelector(SELETORES_OTIMIZADOS.nome, { timeout: 10000 });
```

##### **Lentidão na extração de dados**
```javascript
// Otimização: Limitar elementos processados
const MAX_LINHAS_PROCESSAR = 100;
const linhas = tabelaResultados.querySelectorAll('tbody tr')
    .slice(0, MAX_LINHAS_PROCESSAR);
```

##### **Lentidão nas páginas individuais**
```javascript
// Processamento paralelo controlado:
const BATCH_SIZE = 5;
for (let i = 0; i < registros.length; i += BATCH_SIZE) {
    const batch = registros.slice(i, i + BATCH_SIZE);
    await Promise.all(batch.map(processarDetalhes));
    await page.waitForTimeout(2000); // Pausa entre batches
}
```

### ❌ **Memory Exceeded**

#### **Solução: Gestão de memória**
```javascript
// Limpeza periódica de recursos:
function limparMemoria() {
    // Limpar arrays grandes
    resultadosConsolidados.splice(0, resultadosConsolidados.length - 100);
    
    // Forçar garbage collection (se disponível)
    if (global.gc) {
        global.gc();
    }
    
    // Log de uso de memória
    const uso = process.memoryUsage();
    console.log(`💾 Memória: ${Math.round(uso.heapUsed / 1024 / 1024)}MB`);
}

// Chamar a cada 50 registros processados:
if (contador % 50 === 0) {
    limparMemoria();
}
```

#### **Processamento em chunks**
```javascript
// Dividir trabalho em pedaços menores:
async function processarEmChunks(registros, tamanhoChunk = 20) {
    const resultados = [];
    
    for (let i = 0; i < registros.length; i += tamanhoChunk) {
        const chunk = registros.slice(i, i + tamanhoChunk);
        console.log(`📦 Processando chunk ${Math.floor(i/tamanhoChunk) + 1}`);
        
        const resultadosChunk = await processarRegistros(chunk);
        resultados.push(...resultadosChunk);
        
        // Salvar incrementalmente
        await Actor.pushData(resultadosChunk);
        
        // Limpeza entre chunks
        limparMemoria();
    }
    
    return resultados;
}
```

---

## 🌐 Problemas com Site da ANVISA

### ❌ **Site Indisponível ou Lento**

#### **Detecção automática de problemas**
```javascript
async function verificarSaude(page) {
    try {
        const inicioCarregamento = Date.now();
        await page.goto('https://consultas.anvisa.gov.br/#/medicamentos/', {
            timeout: 30000,
            waitUntil: 'domcontentloaded'
        });
        
        const tempoCarregamento = Date.now() - inicioCarregamento;
        
        if (tempoCarregamento > 20000) {
            throw new Error(`Site lento: ${tempoCarregamento}ms`);
        }
        
        // Verificar se formulário está presente
        const formulario = await page.$('input[placeholder*="Nome"]');
        if (!formulario) {
            throw new Error('Formulário não encontrado - possível manutenção');
        }
        
        return { status: 'ok', tempo: tempoCarregamento };
        
    } catch (error) {
        return { status: 'erro', mensagem: error.message };
    }
}
```

#### **Estratégias de recuperação**
```javascript
// Retry com backoff exponencial:
async function tentarComBackoff(operacao, maxTentativas = 3) {
    for (let tentativa = 1; tentativa <= maxTentativas; tentativa++) {
        try {
            return await operacao();
        } catch (error) {
            const delayMs = Math.pow(2, tentativa) * 1000; // 2s, 4s, 8s
            
            if (tentativa === maxTentativas) {
                throw new Error(`Falha após ${maxTentativas} tentativas: ${error.message}`);
            }
            
            console.log(`⚠️ Tentativa ${tentativa} falhou, aguardando ${delayMs}ms...`);
            await new Promise(resolve => setTimeout(resolve, delayMs));
        }
    }
}
```

### ❌ **Estrutura do Site Mudou**

#### **Detecção de mudanças**
```javascript
// Verificar seletores conhecidos:
const SELETORES_CRITICOS = [
    'input[placeholder*="Nome"]',
    'input[value*="REGISTRADO"]',
    'table',
    'button[type="submit"]'
];

async function verificarEstrutura(page) {
    const problemas = [];
    
    for (const seletor of SELETORES_CRITICOS) {
        const elemento = await page.$(seletor);
        if (!elemento) {
            problemas.push(`Seletor não encontrado: ${seletor}`);
        }
    }
    
    if (problemas.length > 0) {
        throw new Error(`Estrutura do site mudou: ${problemas.join(', ')}`);
    }
}
```

#### **Seletores alternativos**
```javascript
// Fallback para seletores alternativos:
async function encontrarInput(page, tipo) {
    const alternativas = {
        nome: [
            'input[placeholder*="Nome"]',
            'input[name*="nome"]',
            'input[id*="produto"]',
            'input[type="text"]:first-of-type'
        ],
        submit: [
            'button[type="submit"]',
            'input[type="submit"]',
            'button:has-text("Buscar")',
            'button:has-text("Consultar")',
            '.btn-primary'
        ]
    };
    
    for (const seletor of alternativas[tipo] || []) {
        try {
            const elemento = await page.waitForSelector(seletor, { timeout: 3000 });
            if (elemento) {
                console.log(`✅ Encontrado com seletor alternativo: ${seletor}`);
                return elemento;
            }
        } catch (e) {
            continue;
        }
    }
    
    throw new Error(`Nenhum seletor funcionou para: ${tipo}`);
}
```

---

## 🎯 Problemas de Qualidade

### ❌ **Muitos Falsos Positivos**

#### **Problema: Medicamentos incorretos nos resultados**
```javascript
// Filtros mais rigorosos:
function validarRelevancia(registro, termoBusca) {
    const texto = registro.textoCompleto.toLowerCase();
    const termo = termoBusca.toLowerCase();
    
    // Verificações de relevância
    const checks = {
        contemTermo: texto.includes(termo),
        naoEAssociacao: !texto.includes(' + ') && !texto.includes('associação'),
        naoEGenerico: !texto.includes('genérico de '),
        temFormato: /\d+\s*mg/.test(texto) // Tem dosagem
    };
    
    const pontuacao = Object.values(checks).filter(Boolean).length;
    return pontuacao >= 3; // Mínimo 3 de 4 critérios
}
```

#### **Filtros dinâmicos baseados no contexto**
```javascript
function gerarFiltrosContextuais(nomeProduto) {
    const filtrosEspecificos = {
        'dipirona': ['paracetamol', 'cafeína', 'orfenadrina'],
        'ibuprofeno': ['paracetamol', 'cafeína', 'pseudoefedrina'],
        'antibiótico': ['antisséptico', 'desinfetante', 'cosmético']
    };
    
    const produto = nomeProduto.toLowerCase();
    return filtrosEspecificos[produto] || ['associação', 'composto'];
}
```

### ❌ **Dados Inconsistentes**

#### **Validação de dados extraídos**
```javascript
function validarDados(registro) {
    const problemas = [];
    
    // Validar número de registro ANVISA
    if (registro.dados['Nº Registro'] && 
        !/\d{1}\.\d{4}\.\d{8}/.test(registro.dados['Nº Registro'])) {
        problemas.push('Formato de registro inválido');
    }
    
    // Validar empresa
    if (!registro.dados['Empresa'] || registro.dados['Empresa'].length < 3) {
        problemas.push('Nome de empresa muito curto');
    }
    
    // Validar situação
    const situacoesValidas = ['ativo', 'inativo', 'suspenso', 'cancelado'];
    const situacao = registro.dados['Situação']?.toLowerCase();
    if (!situacoesValidas.includes(situacao)) {
        problemas.push('Situação não reconhecida: ' + situacao);
    }
    
    registro.validacao = {
        valido: problemas.length === 0,
        problemas
    };
    
    return registro;
}
```

---

## 🔌 Problemas de Integração

### ❌ **API Integration Issues**

#### **Rate limiting do Apify**
```javascript
// Monitorar uso de créditos:
const client = new ApifyApi({ token: process.env.APIFY_TOKEN });

async function verificarCreditos() {
    const usage = await client.user().get();
    const creditosRestantes = usage.plan.monthlyUsageCredits;
    
    if (creditosRestantes < 100) {
        console.log('⚠️ Poucos créditos restantes:', creditosRestantes);
        return false;
    }
    
    return true;
}
```

#### **Webhook failures**
```javascript
// Retry para webhooks:
async function enviarWebhook(dados, tentativas = 3) {
    for (let i = 0; i < tentativas; i++) {
        try {
            const response = await fetch(WEBHOOK_URL, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(dados),
                timeout: 10000
            });
            
            if (response.ok) {
                return await response.json();
            }
            
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            
        } catch (error) {
            if (i === tentativas - 1) throw error;
            
            const delay = Math.pow(2, i) * 1000;
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
}
```

### ❌ **Database Integration**

#### **Connection issues**
```javascript
// Pool de conexões robusto:
const pool = new Pool({
    connectionString: process.env.DATABASE_URL,
    max: 20,
    idleTimeoutMillis: 30000,
    connectionTimeoutMillis: 10000,
    maxUses: 7500
});

pool.on('error', (err) => {
    console.error('❌ Erro no pool de conexões:', err);
});

async function salvarNoBanco(dados) {
    const client = await pool.connect();
    try {
        await client.query('BEGIN');
        
        // Inserção com upsert
        const query = `
            INSERT INTO medicamentos (nome, empresa, registro, dados_json, updated_at)
            VALUES ($1, $2, $3, $4, NOW())
            ON CONFLICT (registro) 
            DO UPDATE SET 
                nome = EXCLUDED.nome,
                empresa = EXCLUDED.empresa,
                dados_json = EXCLUDED.dados_json,
                updated_at = NOW()
        `;
        
        await client.query(query, [dados.nome, dados.empresa, dados.registro, dados]);
        await client.query('COMMIT');
        
    } catch (error) {
        await client.query('ROLLBACK');
        throw error;
    } finally {
        client.release();
    }
}
```

---

## 🔬 Debugging Avançado

### 🖼️ **Screenshots para Debug**

```javascript
// Sistema de screenshots automático:
async function capturarDebug(page, contexto, erro = null) {
    const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
    const filename = `debug_${contexto}_${timestamp}.png`;
    
    try {
        await page.screenshot({
            path: filename,
            fullPage: true,
            type: 'png'
        });
        
        console.log(`📸 Screenshot salva: ${filename}`);
        
        if (erro) {
            console.log(`🐛 Contexto do erro: ${erro.message}`);
        }
        
        return filename;
    } catch (screenshotError) {
        console.log('⚠️ Falha ao capturar screenshot:', screenshotError.message);
    }
}

// Uso em pontos críticos:
try {
    await preencherFormulario(page, parametros);
} catch (error) {
    await capturarDebug(page, 'erro_formulario', error);
    throw error;
}
```

### 📊 **Logging Estruturado**

```javascript
// Sistema de logs estruturado:
class Logger {
    constructor(contexto) {
        this.contexto = contexto;
        this.nivel = process.env.LOG_LEVEL || 'info';
    }
    
    log(nivel, mensagem, dados = {}) {
        const timestamp = new Date().toISOString();
        const logEntry = {
            timestamp,
            nivel,
            contexto: this.contexto,
            mensagem,
            ...dados
        };
        
        console.log(JSON.stringify(logEntry));
    }
    
    info(mensagem, dados) { this.log('info', mensagem, dados); }
    warn(mensagem, dados) { this.log('warn', mensagem, dados); }
    error(mensagem, dados) { this.log('error', mensagem, dados); }
    debug(mensagem, dados) { this.log('debug', mensagem, dados); }
}

// Uso:
const logger = new Logger('anvisa-scraper');
logger.info('Iniciando busca', { termo: nomeProduto, estrategias: 4 });
logger.error('Falha na extração', { erro: error.message, url: page.url() });
```

### 🔍 **Análise de Performance**

```javascript
// Profiling detalhado:
class Profiler {
    constructor() {
        this.metricas = new Map();
    }
    
    iniciar(operacao) {
        this.metricas.set(operacao, {
            inicio: Date.now(),
            memoria: process.memoryUsage()
        });
    }
    
    finalizar(operacao) {
        const dados = this.metricas.get(operacao);
        if (!dados) return;
        
        const duracao = Date.now() - dados.inicio;
        const memoriaFinal = process.memoryUsage();
        
        const resultado = {
            operacao,
            duracao,
            memoriaInicial: dados.memoria.heapUsed,
            memoriaFinal: memoriaFinal.heapUsed,
            diferencaMemoria: memoriaFinal.heapUsed - dados.memoria.heapUsed
        };
        
        console.log('📊 Profiling:', JSON.stringify(resultado));
        this.metricas.delete(operacao);
        
        return resultado;
    }
}

// Uso:
const profiler = new Profiler();
profiler.iniciar('busca_medicamentos');
// ... operação ...
profiler.finalizar('busca_medicamentos');
```

---

## 🆘 Quando Procurar Ajuda

### 🔴 **Problemas Críticos (Procurar ajuda imediatamente)**
- Actor não consegue fazer build há mais de 1 hora
- Perda de dados após execução bem-sucedida
- Custos inesperadamente altos
- Possível banimento do site da ANVISA

### 🟡 **Problemas Médios (Procurar ajuda se persistir)**
- Taxa de sucesso < 70% consistentemente
- Performance degradando ao longo do tempo
- Resultados inconsistentes entre execuções

### 🟢 **Problemas Menores (Tentar resolver primeiro)**
- Erros ocasionais (< 10% das execuções)
- Pequenas discrepâncias nos dados
- Lentidão em horários específicos

### 📞 **Onde Procurar Ajuda**

1. **Documentação Apify**: https://docs.apify.com/
2. **Fórum da Comunidade**: https://community.apify.com/
3. **Suporte Apify**: help@apify.com (usuários pagos)
4. **Issues no GitHub**: Se usando versão open source

### 📋 **Informações para Incluir no Suporte**

```markdown
**Problema**: Descrição clara do que está acontecendo vs. esperado

**Configuração**:
- Input usado: {JSON do input}
- Versão do Actor: X.X.X
- Configurações de memória/timeout

**Logs relevantes**:
```
[Incluir logs específicos do erro]
```

**Screenshots**: [Se aplicável]

**Tentativas de solução**: O que já foi tentado
```

---

## 🛡️ Prevenção de Problemas

### ✅ **Checklist Pré-Execução**

```bash
□ Código salvo e build bem-sucedido
□ Input validado e testado
□ Créditos Apify suficientes
□ Site ANVISA acessível
□ Configurações de timeout adequadas
□ Backup de configurações importantes
```

### ✅ **Monitoramento Contínuo**

```javascript
// Alertas automáticos:
function configurarAlertas() {
    // Alerta de performance
    if (tempoExecucao > TEMPO_MAXIMO_ESPERADO) {
        enviarAlerta('Performance degradada', { tempo: tempoExecucao });
    }
    
    // Alerta de qualidade
    if (taxaSucesso < 0.8) {
        enviarAlerta('Taxa de sucesso baixa', { taxa: taxaSucesso });
    }
    
    // Alerta de uso de recursos
    if (usoMemoria > LIMITE_MEMORIA) {
        enviarAlerta('Uso de memória alto', { memoria: usoMemoria });
    }
}
```

### ✅ **Manutenção Preventiva**

```bash
# Semanal:
□ Verificar logs de erro
□ Validar qualidade dos dados
□ Revisar performance metrics

# Mensal:
□ Atualizar dependências
□ Revisar configurações
□ Testar com dados conhecidos

# Trimestral:
□ Backup completo de configurações
□ Análise de tendências de uso
□ Planejamento de melhorias
```

---

> **💡 Dica Final:** A maioria dos problemas pode ser evitada com testes pequenos antes de execuções grandes. Sempre comece com `maxResults: 10` para validar configurações!

**🚀 Próximo:** Para ver um exemplo completo de solução de problemas na prática, consulte o [Caso de Estudo - Dipirona](Caso-Estudo-Dipirona.md)!
