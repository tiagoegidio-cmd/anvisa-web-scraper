# 📚 Exemplos Práticos - ANVISA Web Scraper

> **Casos de uso reais com configurações prontas para diferentes cenários**

## 🎯 Guia de Exemplos

### 📈 **Níveis de Complexidade**
- 🟢 **Básico** - Para iniciantes (copy & paste)
- 🟡 **Intermediário** - Requer configuração básica
- 🔴 **Avançado** - Para usuários experientes

### 🏥 **Categorias de Uso**
- 👩‍⚕️ **Pesquisa Farmacêutica** - Análise de medicamentos
- 🏢 **Business Intelligence** - Inteligência de mercado
- 🔬 **Pesquisa Acadêmica** - Estudos e publicações
- 🏛️ **Compliance Regulatório** - Conformidade e auditoria

---

## 🟢 Exemplos Básicos

### 1️⃣ **Busca Simples por Medicamento** 
*Cenário: Verificar se um medicamento específico está registrado*

```json
{
  "nomeProduto": "dipirona",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 20
}
```

**O que faz:**
- Busca todos os registros ativos de dipirona
- Máximo 20 resultados para execução rápida
- Ideal para verificação pontual

**Tempo esperado:** ~1 minuto  
**Resultado esperado:** 5-15 registros de dipirona ativa

---

### 2️⃣ **Verificar Medicamento por Empresa**
*Cenário: Listar todos os produtos de uma empresa específica*

```json
{
  "nomeProduto": "paracetamol",
  "cnpjDetentor": "60.659.463/0029-92",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 50
}
```

**O que faz:**
- Busca paracetamol apenas da empresa Aché (CNPJ fornecido)
- Filtra por registros ativos
- Útil para análise de portfólio específico

**Tempo esperado:** ~2 minutos  
**Resultado esperado:** 3-8 registros específicos da Aché

---

### 3️⃣ **Busca por Forma Farmacêutica**
*Cenário: Encontrar todos os injetáveis de um princípio ativo*

```json
{
  "nomeProduto": "dipirona injetável",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 30,
  "criteriosExclusao": ["paracetamol", "cafeína", "associação"]
}
```

**O que faz:**
- Busca especificamente dipirona injetável
- Exclui medicamentos em associação
- Foca apenas em medicamentos registrados

**Tempo esperado:** ~1.5 minutos  
**Resultado esperado:** 5-10 registros de dipirona injetável pura

---

## 🟡 Exemplos Intermediários

### 4️⃣ **Análise Competitiva de Mercado**
*Cenário: Mapear todos os concorrentes de um princípio ativo*

```json
{
  "nomeProduto": "ibuprofeno",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 150,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "ibuprofeno sódico",
    "ibuprofeno arginina",
    "ibuprofeno gotas",
    "ibuprofeno comprimido"
  ]
}
```

**O que faz:**
- Busca ampla por ibuprofeno em diferentes formas
- Inclui detalhes completos de cada registro
- Múltiplas estratégias para cobertura total
- Extrai informações para análise competitiva

**Tempo esperado:** ~8 minutos  
**Resultado esperado:** 50-120 registros com dados detalhados

**📊 Análise possível:**
- Quantos fabricantes no mercado
- Formas farmacêuticas disponíveis
- Concentrações mais comuns
- Empresas líderes no segmento

---

### 5️⃣ **Monitoramento de Vencimentos**
*Cenário: Verificar medicamentos com registro próximo ao vencimento*

```json
{
  "nomeProduto": "amoxicilina",
  "dataRegularizacao": "01/01/2015",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 100,
  "incluirDetalhes": true
}
```

**O que faz:**
- Busca medicamentos registrados em data específica
- Inclui detalhes para verificar data de vencimento
- Útil para planejamento de renovações

**Tempo esperado:** ~5 minutos  
**Resultado esperado:** 10-30 registros com datas detalhadas

**📅 Uso prático:**
- Alertas de renovação necessária
- Planejamento regulatório
- Gestão de portfólio

---

### 6️⃣ **Pesquisa de Medicamentos Órfãos**
*Cenário: Buscar medicamentos para doenças raras*

```json
{
  "nomeProduto": "enzima",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 80,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "alfa",
    "beta",
    "terapia gênica",
    "biotecnológico"
  ],
  "criteriosExclusao": ["vitamina", "suplemento"]
}
```

**O que faz:**
- Busca medicamentos biotecnológicos/enzimáticos
- Exclui suplementos vitamínicos
- Foca em terapias especializadas

**Tempo esperado:** ~6 minutos  
**Resultado esperado:** 15-40 medicamentos especializados

---

## 🔴 Exemplos Avançados

### 7️⃣ **Análise Regulatória Completa**
*Cenário: Relatório completo de um segmento terapêutico*

```json
{
  "nomeProduto": "antibiótico",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
  "situacaoRegularizacao": "Ambos",
  "maxResults": 500,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "penicilina",
    "amoxicilina",
    "azitromicina",
    "ciprofloxacino",
    "cefalexina",
    "claritromicina",
    "doxiciclina",
    "eritromicina"
  ],
  "criteriosExclusao": [
    "antisséptico",
    "desinfetante",
    "cosmético"
  ]
}
```

**O que faz:**
- Análise abrangente do mercado de antibióticos
- Inclui medicamentos ativos e inativos para análise histórica
- Múltiplas estratégias por princípio ativo
- Dados completos para business intelligence

**Tempo esperado:** ~25 minutos  
**Resultado esperado:** 200-400 registros de antibióticos

**📊 Insights extraíveis:**
- Market share por empresa
- Evolução do mercado ao longo do tempo
- Principais players por categoria
- Tendências de aprovação/descontinuação

---

### 8️⃣ **Monitoramento de Genéricos**
*Cenário: Acompanhar entrada de genéricos no mercado*

```json
{
  "nomeProduto": "atorvastatina",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 200,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "atorvastatina cálcica",
    "atorvastatina genérico",
    "atorvastatina similar"
  ]
}
```

**O que faz:**
- Mapeia todos os genéricos e similares de atorvastatina
- Monitora concorrência pós-patente
- Analisa entrada de novos players

**Tempo esperado:** ~10 minutos  
**Resultado esperado:** 30-80 registros por categoria

**💼 Aplicação comercial:**
- Análise de erosão de mercado
- Identificação de oportunidades
- Monitoramento competitivo
- Estratégia de pricing

---

### 9️⃣ **Auditoria de Compliance**
*Cenário: Verificar conformidade regulatória de portfólio*

```json
{
  "nomeProduto": "insulina",
  "cnpjDetentor": "12.345.678/0001-90",
  "situacaoRegularizacao": "Ambos",
  "maxResults": 300,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "insulina humana",
    "insulina recombinante", 
    "insulina glargina",
    "insulina lispro",
    "insulina aspart"
  ]
}
```

**O que faz:**
- Auditoria completa do portfólio de insulinas
- Verifica status de todos os registros
- Identifica necessidades de renovação
- Mapeia produtos descontinuados

**Tempo esperado:** ~15 minutos  
**Resultado esperado:** 20-60 registros com status detalhado

**🔍 Verificações realizadas:**
- Registros vencidos ou próximos ao vencimento
- Produtos inativos que deveriam estar ativos
- Inconsistências nos dados
- Oportunidades de reativação

---

## 🎯 Casos de Uso Específicos

### 🏥 **Para Hospitais e Farmácias**

#### **Verificação de Fornecedores**
```json
{
  "nomeProduto": "morfina",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 50,
  "incluirDetalhes": true
}
```

**Objetivo:** Verificar fornecedores válidos de medicamentos controlados

#### **Planejamento de Compras**
```json
{
  "nomeProduto": "omeprazol",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 100,
  "estrategiasCustomizadas": ["omeprazol magnésico", "omeprazol sódico"]
}
```

**Objetivo:** Mapear todas as opções disponíveis para licitação

---

### 🔬 **Para Pesquisa Acadêmica**

#### **Análise de Inovação Farmacêutica**
```json
{
  "nomeProduto": "biotecnológico",
  "dataRegularizacao": "01/01/2020",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 200,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "monoclonal",
    "recombinante",
    "terapia gênica",
    "células-tronco"
  ]
}
```

**Objetivo:** Estudar tendências de aprovação de medicamentos inovadores

#### **Análise de Mercado de Genéricos**
```json
{
  "nomeProduto": "genérico",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO", 
  "situacaoRegularizacao": "Ativo",
  "maxResults": 1000,
  "criteriosExclusao": ["similar", "referência"]
}
```

**Objetivo:** Quantificar impacto dos genéricos no mercado brasileiro

---

### 🏢 **Para Indústria Farmacêutica**

#### **Inteligência Competitiva**
```json
{
  "nomeProduto": "anticoagulante",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 150,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "warfarina",
    "rivaroxabana", 
    "apixabana",
    "dabigatrana"
  ]
}
```

**Objetivo:** Mapear concorrentes em segmento específico

#### **Due Diligence Regulatória**
```json
{
  "nomeProduto": "adalimumab",
  "situacaoRegularizacao": "Ambos",
  "maxResults": 100,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": ["biosimilar", "biológico"]
}
```

**Objetivo:** Análise regulatória para M&A ou licenciamento

---

## 📊 Templates de Análise

### 📈 **Análise de Market Share**

```json
{
  "nomeProduto": "estatina",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 300,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "atorvastatina",
    "sinvastatina", 
    "rosuvastatina",
    "pravastatina"
  ]
}
```

**📊 Processamento pós-extração:**
```javascript
// Análise por empresa
const porEmpresa = resultados.reduce((acc, item) => {
  const empresa = item.empresa;
  acc[empresa] = (acc[empresa] || 0) + 1;
  return acc;
}, {});

// Top 5 empresas
const top5 = Object.entries(porEmpresa)
  .sort(([,a], [,b]) => b - a)
  .slice(0, 5);
```

### 📅 **Análise Temporal**

```json
{
  "nomeProduto": "covid",
  "dataRegularizacao": "01/03/2020",
  "situacaoRegularizacao": "Ativo", 
  "maxResults": 200,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": [
    "coronavirus",
    "sars-cov-2",
    "vacina covid",
    "remdesivir"
  ]
}
```

**📈 Insights temporais:**
- Velocidade de aprovação durante emergência
- Padrões de inovação em crises
- Resposta regulatória brasileira

---

## 🔧 Otimizações por Cenário

### ⚡ **Para Execução Rápida**
```json
{
  "incluirDetalhes": false,
  "maxResults": 50,
  "estrategiasCustomizadas": []
}
```

### 🔍 **Para Máxima Completude**
```json
{
  "incluirDetalhes": true,
  "maxResults": 1000,
  "estrategiasCustomizadas": [
    "termo1", "termo2", "termo3"
  ]
}
```

### 💰 **Para Economia de Créditos**
```json
{
  "incluirDetalhes": false,
  "maxResults": 30,
  "criteriosExclusao": ["termos", "irrelevantes"]
}
```

### 📊 **Para Business Intelligence**
```json
{
  "incluirDetalhes": true,
  "maxResults": 500,
  "situacaoRegularizacao": "Ambos"
}
```

---

## 🎯 Medição de Resultados

### 📈 **KPIs por Tipo de Busca**

| Tipo de Busca | Registros Esperados | Tempo Típico | Acurácia |
|---------------|-------------------|--------------|----------|
| **Medicamento específico** | 5-20 | 1-2 min | 95%+ |
| **Por empresa** | 10-50 | 2-5 min | 90%+ |
| **Análise de mercado** | 50-200 | 8-15 min | 85%+ |
| **Auditoria completa** | 100-500 | 15-30 min | 80%+ |

### ✅ **Critérios de Sucesso**

#### **Para Pesquisa Simples:**
- ✅ Encontrou pelo menos 1 registro
- ✅ Tempo < 5 minutos
- ✅ Zero falsos positivos

#### **Para Análise Competitiva:**
- ✅ Cobertura > 80% do mercado conhecido
- ✅ Dados detalhados > 90% dos registros
- ✅ Insights acionáveis identificados

#### **Para Compliance:**
- ✅ 100% do portfólio verificado
- ✅ Status atualizado de todos os registros
- ✅ Alertas de vencimento identificados

---

## 💡 Dicas de Otimização

### 🎯 **Melhorando Resultados**

1. **Use termos específicos** primeiro, depois generalize
2. **Combine múltiplas estratégias** para cobertura completa
3. **Ajuste critérios de exclusão** baseado nos resultados
4. **Execute testes pequenos** antes de análises grandes

### ⚡ **Melhorando Performance**

1. **Desabilite detalhes** para buscas exploratórias
2. **Use maxResults baixo** para testes
3. **Execute em horários** de menor tráfego
4. **Monitore créditos** do Apify regularmente

### 🔍 **Melhorando Precisão**

1. **Revise critérios de exclusão** após cada execução
2. **Use CNPJ específico** quando conhecer a empresa
3. **Combine data + situação** para filtros temporais
4. **Valide amostras** manualmente periodicamente

---

> **💡 Próximo:** Agora que você domina os exemplos práticos, consulte o [Troubleshooting](Troubleshooting.md) para resolver problemas específicos ou veja o [Caso de Estudo](Caso-Estudo-Dipirona.md) para uma análise completa real!

**🚀 Dica:** Comece sempre com exemplos básicos e vá progredindo para casos mais complexos conforme ganha experiência!
