# 📊 Schemas de Dados - ANVISA Web Scraper

> **Estruturas detalhadas de entrada e saída para uso correto do scraper**

## 📥 Schema de Entrada (Input)

### 🔧 **Configuração Básica**

```typescript
interface InputBasico {
  nomeProduto: string;                    // ⚠️ OBRIGATÓRIO
  situacaoRegularizacao?: "Ativo" | "Inativo" | "Ambos";
  maxResults?: number;
}
```

**Exemplo Mínimo:**
```json
{
  "nomeProduto": "dipirona"
}
```

### ⚙️ **Configuração Completa**

```typescript
interface InputCompleto {
  // === CAMPOS OBRIGATÓRIOS ===
  nomeProduto: string;                    // Nome ou parte do medicamento
  
  // === FILTROS DE BUSCA ===
  tipoRegularizacao?: "MEDICAMENTO REGISTRADO" | "MEDICAMENTO NOTIFICADO";
  cnpjDetentor?: string;                  // Formato: "12.345.678/0001-90"
  dataRegularizacao?: string;             // Formato: "DD/MM/AAAA"
  situacaoRegularizacao?: "Ativo" | "Inativo" | "Ambos";
  
  // === CONFIGURAÇÕES DE EXECUÇÃO ===
  maxResults?: number;                    // Máximo: 1000, Padrão: 100
  incluirDetalhes?: boolean;              // Padrão: true
  
  // === CONFIGURAÇÕES AVANÇADAS ===
  estrategiasCustomizadas?: string[];     // Termos alternativos
  criteriosExclusao?: string[];           // Filtros de exclusão
}
```

### 📋 **Validação de Campos**

| Campo | Tipo | Obrigatório | Validação | Exemplo |
|-------|------|-------------|-----------|---------|
| `nomeProduto` | string | ✅ Sim | 1-100 caracteres | `"dipirona"` |
| `tipoRegularizacao` | enum | ❌ Não | Valores predefinidos | `"MEDICAMENTO REGISTRADO"` |
| `cnpjDetentor` | string | ❌ Não | Formato CNPJ válido | `"12.345.678/0001-90"` |
| `dataRegularizacao` | string | ❌ Não | DD/MM/AAAA | `"01/01/2020"` |
| `situacaoRegularizacao` | enum | ❌ Não | Ativo/Inativo/Ambos | `"Ativo"` |
| `maxResults` | number | ❌ Não | 1-1000 | `100` |
| `incluirDetalhes` | boolean | ❌ Não | true/false | `true` |
| `estrategiasCustomizadas` | array | ❌ Não | Array de strings | `["metamizol", "dipirona sódica"]` |
| `criteriosExclusao` | array | ❌ Não | Array de strings | `["paracetamol", "cafeína"]` |

### 📝 **Exemplos de Entrada**

#### **Busca Simples**
```json
{
  "nomeProduto": "ibuprofeno",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 50
}
```

#### **Busca por Empresa**
```json
{
  "nomeProduto": "dipirona",
  "cnpjDetentor": "60.659.463/0029-92",
  "situacaoRegularizacao": "Ativo"
}
```

#### **Busca Avançada com Filtros**
```json
{
  "nomeProduto": "paracetamol",
  "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
  "situacaoRegularizacao": "Ativo",
  "maxResults": 200,
  "incluirDetalhes": true,
  "estrategiasCustomizadas": ["acetaminofeno", "paracetamol gotas"],
  "criteriosExclusao": ["cafeína", "codeína", "associação"]
}
```

#### **Busca Específica por Data**
```json
{
  "nomeProduto": "amoxicilina",
  "dataRegularizacao": "01/01/2020",
  "situacaoRegularizacao": "Ativo",
  "incluirDetalhes": false
}
```

---

## 📤 Schema de Saída (Output)

### 🎯 **Estrutura do Registro Individual**

```typescript
interface RegistroMedicamento {
  // === IDENTIFICAÇÃO ===
  numero: number;                         // Índice sequencial
  registroAnvisa?: string;                // Número oficial ANVISA
  numeroProcesso?: string;                // Processo administrativo
  
  // === DADOS BÁSICOS ===
  nomeProduto: string;                    // Nome comercial
  empresa: string;                        // Empresa detentora
  principioAtivo?: string;                // IFA principal
  formaFarmaceutica?: string;             // Comprimido, injetável, etc.
  concentracao?: string;                  // mg/mL, mg, etc.
  apresentacao?: string;                  // Embalagem completa
  categoria?: "Genérico" | "Similar" | "Referência" | "Específico";
  
  // === STATUS REGULATÓRIO ===
  situacao: string;                       // Ativo, Inativo, etc.
  dataRegistro?: string;                  // Data de concessão
  dataVencimento?: string;                // Data de expiração
  
  // === DADOS ESTRUTURADOS ===
  dados: Record<string, string>;          // Mapeamento coluna→valor
  textoCompleto: string;                  // Linha completa da tabela
  
  // === NAVEGAÇÃO ===
  temLink: boolean;                       // Se há página individual
  linkDetalhes?: string;                  // URL para detalhes
  textoLink?: string;                     // Texto do link
  detalhesCompletos?: DetalhesIndividuais; // Dados da página individual
  detalhesExtraidos?: boolean;            // Se foi possível extrair
  
  // === VALIDAÇÃO ===
  validado: {
    semAssociacoes: boolean;              // Não é medicamento composto
    textoLimpo: boolean;                  // Passou pelos filtros
  };
  
  // === METADADOS ===
  estrategiaBusca: string;                // Qual estratégia encontrou
  termoUsado: string;                     // Termo de busca usado
  pesoEstrategia: number;                 // Confiabilidade (0-1)
  dataExtracao: string;                   // ISO timestamp
  fonte: string;                          // "ANVISA - Consultas Medicamentos"
  parametrosBusca: InputCompleto;         // Parâmetros originais
}
```

### 🔍 **Detalhes Individuais**

```typescript
interface DetalhesIndividuais {
  titulo: string;                         // Título da página
  numeroRegistro?: string;                // Número do registro (se encontrado)
  empresa?: string;                       // Empresa detentora
  principioAtivo?: string;                // IFA detalhado
  concentracao?: string;                  // Concentração extraída
  apresentacao?: string;                  // Apresentação completa
  categoria?: string;                     // Categoria regulatória
  situacao?: string;                      // Situação atual
  dataRegistro?: string;                  // Data de registro
  dataVencimento?: string;                // Data de vencimento
}
```

### 📊 **Estrutura do Resumo Final**

```typescript
interface RespostaCompleta {
  resumoExecucao: {
    parametrosEntrada: InputCompleto;     // Parâmetros usados
    metricas: MetricasExecucao;          // Estatísticas da execução
    totalRegistros: number;               // Total encontrado
    estrategiasExecutadas: EstrategiaInfo[]; // Estratégias usadas
    garantiaCompletude: boolean;          // Se houve múltiplas estratégias
    qualidadeResultados: "Excelente" | "Boa" | "Satisfatória";
  };
  registrosEncontrados: RegistroMedicamento[]; // Lista de medicamentos
}
```

### 📈 **Métricas de Execução**

```typescript
interface MetricasExecucao {
  inicioExecucao: string;                 // ISO timestamp início
  fimExecucao: string;                    // ISO timestamp fim  
  tempoTotal: number;                     // Segundos totais
  estrategiasExecutadas: number;          // Quantas estratégias rodaram
  registrosEncontrados: number;           // Total de registros únicos
  registrosExcluidos: number;             // Filtrados por critérios
  erros: number;                          // Número de erros
}
```

### 🎯 **Informações da Estratégia**

```typescript
interface EstrategiaInfo {
  id: string;                             // ID da estratégia
  descricao: string;                      // Descrição legível
  termo: string;                          // Termo de busca usado
}
```

---

## 📋 **Exemplos de Saída**

### 🔍 **Registro Individual Completo**

```json
{
  "numero": 1,
  "registroAnvisa": "1.0387.00760019",
  "nomeProduto": "Dipirona Sódica",
  "empresa": "Hypofarma Indústria Farmacêutica",
  "principioAtivo": "dipirona monoidratada",
  "formaFarmaceutica": "solução injetável",
  "concentracao": "500 mg/mL",
  "apresentacao": "SOL INJ CX 100 AMP VD AMB X 2 ML",
  "categoria": "Genérico",
  "situacao": "Ativo",
  "dataRegistro": "15/08/2018",
  "dataVencimento": "15/08/2028",
  "dados": {
    "Produto": "Dipirona Sódica",
    "Empresa": "Hypofarma Indústria Farmacêutica",
    "Nº Registro": "1.0387.00760019",
    "Situação": "Ativo",
    "Data Vencimento": "15/08/2028"
  },
  "textoCompleto": "Dipirona Sódica Hypofarma Indústria Farmacêutica 1.0387.00760019 Ativo 15/08/2028",
  "temLink": true,
  "linkDetalhes": "https://consultas.anvisa.gov.br/#/medicamentos/426372?numeroProcesso=25351329759200535",
  "textoLink": "Ver detalhes",
  "detalhesCompletos": {
    "titulo": "Dipirona Sódica - Detalhes do Registro",
    "numeroRegistro": "1.0387.00760019",
    "empresa": "Hypofarma Indústria Farmacêutica Ltda",
    "principioAtivo": "dipirona monoidratada",
    "concentracao": "500 mg/mL",
    "apresentacao": "Solução injetável 500 mg/mL, caixa com 100 ampolas de 2 mL",
    "categoria": "Medicamento Genérico",
    "situacao": "Ativo",
    "dataRegistro": "15/08/2018",
    "dataVencimento": "15/08/2028"
  },
  "detalhesExtraidos": true,
  "validado": {
    "semAssociacoes": true,
    "textoLimpo": true
  },
  "estrategiaBusca": "Busca direta pelo termo fornecido",
  "termoUsado": "dipirona",
  "pesoEstrategia": 1.0,
  "dataExtracao": "2025-07-27T14:30:00.000Z",
  "fonte": "ANVISA - Consultas Medicamentos",
  "parametrosBusca": {
    "nomeProduto": "dipirona",
    "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
    "situacaoRegularizacao": "Ativo",
    "maxResults": 100,
    "incluirDetalhes": true
  }
}
```

### 📊 **Resumo de Execução Completo**

```json
{
  "resumoExecucao": {
    "parametrosEntrada": {
      "nomeProduto": "dipirona",
      "tipoRegularizacao": "MEDICAMENTO REGISTRADO",
      "situacaoRegularizacao": "Ativo",
      "maxResults": 100,
      "incluirDetalhes": true,
      "estrategiasCustomizadas": [],
      "criteriosExclusao": ["paracetamol", "cafeína", "associação"]
    },
    "metricas": {
      "inicioExecucao": "2025-07-27T14:28:00.000Z",
      "fimExecucao": "2025-07-27T14:32:15.000Z",
      "tempoTotal": 255,
      "estrategiasExecutadas": 4,
      "registrosEncontrados": 7,
      "registrosExcluidos": 3,
      "erros": 0
    },
    "totalRegistros": 7,
    "estrategiasExecutadas": [
      {
        "id": "direta",
        "descricao": "Busca direta pelo termo fornecido",
        "termo": "dipirona"
      },
      {
        "id": "quimica", 
        "descricao": "Busca pela denominação química",
        "termo": "dipirona sódica"
      },
      {
        "id": "principio_ativo",
        "descricao": "Busca pelo princípio ativo (DCI)",
        "termo": "metamizol"
      },
      {
        "id": "ampla",
        "descricao": "Busca ampla com filtro posterior",
        "termo": "dipirona"
      }
    ],
    "garantiaCompletude": true,
    "qualidadeResultados": "Excelente"
  },
  "registrosEncontrados": [
    // Array com todos os registros encontrados
  ]
}
```

---

## 🔄 **Formatos de Saída Disponíveis**

### 📄 **JSON (Padrão)**
- Estrutura completa com todos os metadados
- Ideal para integração com APIs
- Suporte a processamento programático

### 📊 **CSV Simplificado** 
```csv
numero,nomeProduto,empresa,registroAnvisa,situacao,dataVencimento,estrategiaBusca
1,Dipirona Sódica,Hypofarma,1.0387.00760019,Ativo,15/08/2028,Busca direta
2,Dipirona Sódica,Santisa,N/A,Ativo,N/A,Busca química
```

### 📈 **Relatório de Análise**
```json
{
  "analise": {
    "totalFabricantes": 5,
    "distribuicaoCategoria": {
      "Genérico": 4,
      "Similar": 3
    },
    "situacaoRegistros": {
      "Ativo": 7,
      "Inativo": 0
    },
    "formasEncontradas": ["solução injetável"],
    "concentracoesPadrao": ["500 mg/mL"]
  }
}
```

---

## ⚡ **Otimizações de Schema**

### 🔍 **Campos Opcionais Inteligentes**
- Campos não preenchidos = `undefined` (não `null`)
- Reduz tamanho do JSON final
- Facilita validação de dados

### 📊 **Estrutura Hierárquica**
- Dados básicos no nível raiz
- Detalhes em subobjetos específicos
- Metadados separados para processamento

### 🎯 **Indexação Eficiente**
- `numero` sequencial para ordenação
- `chaveUnica` baseada em texto para deduplicação
- `pesoEstrategia` para ranking de relevância

---

## 🛡️ **Validação de Dados**

### ✅ **Validação de Entrada**
```javascript
function validarEntrada(input) {
  // Campos obrigatórios
  if (!input.nomeProduto) throw new Error('nomeProduto obrigatório');
  
  // Validações de formato
  if (input.cnpjDetentor && !validarCNPJ(input.cnpjDetentor)) {
    throw new Error('CNPJ inválido');
  }
  
  // Sanitização
  return {
    nomeProduto: input.nomeProduto.trim().substring(0, 100),
    // ... outros campos sanitizados
  };
}
```

### ✅ **Validação de Saída**
```javascript
function validarSaida(registro) {
  const obrigatorios = ['nomeProduto', 'situacao', 'dataExtracao'];
  
  for (const campo of obrigatorios) {
    if (!registro[campo]) {
      throw new Error(`Campo obrigatório ausente: ${campo}`);
    }
  }
}
```

---

## 🔗 **Integração com Sistemas**

### 🌐 **APIs REST**
```javascript
// Exemplo de integração
const resultado = await fetch('/api/anvisa-scraper', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    nomeProduto: 'dipirona',
    situacaoRegularizacao: 'Ativo'
  })
});

const dados = await resultado.json();
console.log(`Encontrados: ${dados.resumoExecucao.totalRegistros} registros`);
```

### 🗄️ **Bancos de Dados**
```sql
-- Estrutura de tabela sugerida
CREATE TABLE medicamentos_anvisa (
  id SERIAL PRIMARY KEY,
  nome_produto VARCHAR(255) NOT NULL,
  empresa VARCHAR(255),
  registro_anvisa VARCHAR(50),
  situacao VARCHAR(50),
  data_registro DATE,
  data_vencimento DATE,
  dados_completos JSONB,
  data_extracao TIMESTAMP DEFAULT NOW()
);
```

### 📊 **Business Intelligence**
```python
# Exemplo em Python/Pandas
import pandas as pd

# Carregar dados do scraper
df = pd.read_json('resultados_anvisa.json')

# Análises rápidas
print(f"Total de medicamentos: {len(df)}")
print(f"Empresas únicas: {df['empresa'].nunique()}")
print(f"Distribuição por situação: {df['situacao'].value_counts()}")
```

---

> **💡 Próximo:** Com os schemas definidos, vamos para o [Guia de Setup](Guia-Setup.md) para aprender a configurar e executar o scraper na prática!

---

**📖 Referências de Schema:**
- [JSON Schema](https://json-schema.org/)
- [TypeScript Interfaces](https://www.typescriptlang.org/docs/handbook/interfaces.html)
- [Apify Data Formats](https://docs.apify.com/platform/storage/dataset)