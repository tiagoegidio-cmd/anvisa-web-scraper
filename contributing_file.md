# 🤝 Contribuindo para o ANVISA Web Scraper

Obrigado por considerar contribuir para o ANVISA Web Scraper! Este documento fornece diretrizes para contribuições.

## 📋 Código de Conduta

### Nossa Promessa
Queremos manter um ambiente acolhedor e inclusivo para todos os colaboradores.

### Comportamentos Esperados
- Use linguagem acolhedora e inclusiva
- Respeite pontos de vista diferentes
- Aceite críticas construtivas graciosamente
- Foque no que é melhor para a comunidade

## 🚀 Como Contribuir

### 🐛 Reportando Bugs

1. **Verifique** se o bug já foi reportado nas [Issues](https://github.com/username/anvisa-web-scraper/issues)
2. **Crie uma nova issue** com:
   - Título claro e descritivo
   - Passos para reproduzir o problema
   - Comportamento esperado vs atual
   - Screenshots (se aplicável)
   - Configuração utilizada
   - Logs de erro relevantes

### 💡 Sugerindo Melhorias

1. **Abra uma issue** descrevendo:
   - Problema que a melhoria resolveria
   - Solução proposta
   - Alternativas consideradas
   - Impacto esperado

### 🔧 Contribuindo com Código

#### Preparação
```bash
1. Fork o repositório
2. Clone seu fork:
   git clone https://github.com/seu-usuario/anvisa-web-scraper.git
3. Crie uma branch para sua feature:
   git checkout -b feature/nova-funcionalidade
```

#### Desenvolvimento
- Siga o estilo de código existente
- Adicione comentários explicativos
- Mantenha funções pequenas e focadas
- Use nomes descritivos para variáveis

#### Testes
- Teste suas mudanças com dados reais
- Verifique se não quebrou funcionalidades existentes
- Documente qualquer configuração especial necessária

#### Commit
```bash
# Use mensagens descritivas:
git commit -m "feat: adicionar filtro por categoria regulatória"
git commit -m "fix: corrigir timeout em páginas lentas"
git commit -m "docs: atualizar guia de setup"
```

#### Pull Request
1. **Push** para sua branch
2. **Abra um Pull Request** com:
   - Título claro
   - Descrição detalhada das mudanças
   - Link para issues relacionadas
   - Screenshots (se aplicável)

## 📝 Diretrizes de Código

### JavaScript
```javascript
// ✅ BOM
async function extrairDadosMedicamento(page, seletor) {
    try {
        const elemento = await page.waitForSelector(seletor, { timeout: 10000 });
        return elemento.innerText.trim();
    } catch (error) {
        console.log(`⚠️ Elemento não encontrado: ${seletor}`);
        return null;
    }
}

// ❌ EVITAR
function getData(p, s) {
    return p.$(s).innerText; // Sem tratamento de erro
}
```

### Documentação
- Mantenha README.md atualizado
- Adicione comentários JSDoc para funções principais
- Atualize exemplos quando necessário

### Rate Limiting
- **SEMPRE** respeite o rate limiting (1 req/seg)
- Use `await page.waitForTimeout()` entre requests
- Nunca aumente `maxConcurrency` acima de 1

## 🔍 Áreas de Contribuição

### 🎯 Prioridade Alta
- Melhorias na detecção de mudanças do site ANVISA
- Otimizações de performance
- Tratamento de edge cases
- Testes automatizados

### 🎯 Prioridade Média
- Novos casos de uso
- Melhorias na documentação
- Exemplos adicionais
- Integração com outras bases de dados

### 🎯 Prioridade Baixa
- Refatorações de código
- Melhorias de UI/UX
- Traduções
- Otimizações menores

## 🏷️ Convenções

### Issues
- `bug` - Problema no código
- `enhancement` - Nova funcionalidade
- `documentation` - Melhoria na documentação
- `good first issue` - Bom para iniciantes
- `help wanted` - Precisa de ajuda da comunidade

### Branches
- `main` - Código estável
- `develop` - Desenvolvimento ativo
- `feature/nome` - Nova funcionalidade
- `fix/nome` - Correção de bug
- `docs/nome` - Atualização de documentação

### Commits
- `feat:` - Nova funcionalidade
- `fix:` - Correção de bug
- `docs:` - Documentação
- `style:` - Formatação
- `refactor:` - Refatoração
- `test:` - Testes
- `chore:` - Manutenção

## 📚 Recursos

### Para Novos Contribuidores
- [Como fazer um Pull Request](https://docs.github.com/en/github/collaborating-with-pull-requests)
- [Apify Documentation](https://docs.apify.com/)
- [Playwright Guide](https://playwright.dev/docs/intro)

### Para Desenvolvedores Experientes
- [Documentação Técnica](docs/documentacao-tecnica.md)
- [Schemas de Dados](docs/schemas-dados.md)
- [Troubleshooting](docs/troubleshooting.md)

## 🎉 Reconhecimento

Todos os contribuidores serão reconhecidos no README.md e releases notes.

## ❓ Dúvidas?

- Abra uma [Discussion](https://github.com/username/anvisa-web-scraper/discussions)
- Comente em issues existentes
- Entre em contato via email (se disponível)

Obrigado por contribuir! 🚀