# Contribuindo para Cheatsheets

Obrigado por se interessar em contribuir! Este documento fornece diretrizes e instruções para adicionar ou melhorar cheatsheets.

## 🎯 Como Contribuir

### 1. Fork e Clone

```bash
# Clone o repositório
git clone https://github.com/dpandolfo/cheatsheets.git
cd cheatsheets

# Crie uma branch para sua contribuição
git checkout -b feature/seu-nome-do-cheatsheet
```

### 2. Estrutura de Pastas

O repositório está organizado por categorias:

```
cheatsheets/
├── aws-cli/          # AWS CLI (ECS, EKS)
├── kubernetes/       # Kubernetes & kubectl
├── devops/           # DevOps tools (Terraform, Docker)
├── databases/        # Database tools (PostgreSQL, Redis)
└── README.md
```

Para adicionar um novo cheatsheet:

1. **Crie uma pasta apropriada** (ou use uma existente)
2. **Crie um arquivo `README.md` ou com nome descritivo** (ex: `kubectl.md`)
3. **Atualize o índice** no README principal

### 3. Padrão de Formatação

Siga este padrão ao criar cheatsheets:

```markdown
# [Ferramenta/Serviço] Cheatsheet

## Seção Principal

### Subseção

Descrição breve (opcional).

\`\`\`bash
# Comentário explicativo
comando exemplo

# Outro comando
comando com argumentos
\`\`\`

### Outra Subseção

Descrição.

\`\`\`bash
comando1
comando2
\`\`\`
```

### Diretrizes de Formatação

- **Títulos**: Use `# Titulo Principal`, `## Seção`, `### Subseção`
- **Código**: Sempre use blocos com backticks e especifique a linguagem:
  - `bash` para comandos shell
  - `sql` para SQL
  - `json` para JSON
  - `yaml` para YAML
- **Variáveis**: Use `$VARIABLE` ou `$variable` (seja consistente)
- **Comentários**: Adicione comentários explicativos em blocos de código
- **Um-liners**: Agrupe comandos relacionados
- **Seções de Troubleshooting**: Incluir quando possível

### 4. Melhorias de Conteúdo

Ao contribuir, considere:

- ✅ **Exemplos práticos**: Comandos reais e funcionais
- ✅ **Explicações**: Comentários claros sobre o que cada comando faz
- ✅ **Erros comuns**: Incluir troubleshooting quando relevante
- ✅ **Links úteis**: Referenciar documentação oficial
- ✅ **Consistência**: Manter estilo com outros cheatsheets
- ❌ Evitar: Comandos muito básicos ou óbvios
- ❌ Evitar: Documentação repetida de outras fontes sem adaptação

### 5. Atualizar README Principal

Se adicionar um novo cheatsheet:

1. Edite [`README.md`](./README.md)
2. Adicione link na seção apropriada:

```markdown
### Nova Categoria
* [Ferramenta](path/to/cheatsheet.md) — Descrição breve
```

### 6. Commit e Push

```bash
# Adicione seus arquivos
git add aws-cli/novo-cheatsheet.md
git add README.md

# Commit com mensagem descritiva
git commit -m "Add AWS Lambda cheatsheet with common patterns"

# Push para sua branch
git push origin feature/seu-nome-do-cheatsheet
```

### 7. Abra um Pull Request

1. Acesse o repositório no GitHub
2. Clique em "New Pull Request"
3. Selecione sua branch
4. Descreva as mudanças:
   - O que foi adicionado/melhorado
   - Seções cobertas
   - Qualquer contexto adicional

## 📝 Padrões de Código

### Variáveis de Exemplo

Use nomes claros e consistentes:

```bash
# ✅ BOM
export CLUSTER_NAME="my-cluster"
export SERVICE_NAME="my-service"

# ❌ EVITAR
export cluster_name = cluster_name  # Espaço ao redor de =
export foo = bar                    # Nome não descritivo
```

### Estrutura de Comandos

```bash
# ✅ BOM - Comando simples com comentário
# Get all pods in namespace
kubectl get pods -n my-namespace

# ✅ BOM - Múltiplos comandos com labels
# List deployments
kubectl get deploy

# Get deployment details
kubectl describe deploy my-deployment

# ✅ BOM - Comando complexo com quebra de linha
aws eks update-nodegroup-config \
  --cluster-name $CLUSTER_NAME \
  --nodegroup-name my-nodegroup \
  --scaling-config minSize=1,maxSize=5
```

### Tratamento de Erros

Quando relevante, inclua exemplos de troubleshooting:

```markdown
## Troubleshooting

### Problema: Serviço não inicia

\`\`\`bash
# Verifique logs
kubectl logs my-pod

# Verifique eventos
kubectl describe pod my-pod
\`\`\`
```

## 🔍 Checklist antes de Submeter PR

- [ ] Código segue o padrão de formatação
- [ ] Todos os comandos foram testados
- [ ] Variáveis usam nomes descritivos
- [ ] Comentários são claros e úteis
- [ ] Links para documentação externa funcionam
- [ ] README principal foi atualizado
- [ ] Sem erros de digitação
- [ ] Estrutura de pastas está correta

## 📋 Tipos de Contribuições

### Novo Cheatsheet

```bash
git checkout -b feature/add-[tool-name]-cheatsheet
```

Crie um cheatsheet completo com seções principais.

### Melhorar Cheatsheet Existente

```bash
git checkout -b improve/[tool-name]-enhancements
```

- Corrigir erros
- Adicionar exemplos
- Melhorar formatação
- Atualizar comandos obsoletos

### Correções de Bugs

```bash
git checkout -b fix/[issue-description]
```

Corrigir erros de sintaxe, links quebrados, etc.

## 🎓 Boas Práticas

### Organização

- Agrupe comandos relacionados
- Use títulos descritivos
- Mantenha seções lógicas

### Clareza

- Use linguagem simples
- Explique o que cada comando faz
- Forneça contexto para casos de uso

### Completude

- Cubra os comandos mais comuns
- Inclua opções importantes
- Adicione troubleshooting quando útil

## ❓ Dúvidas?

- Abra uma issue para discussão
- Envie email ou entre em contato
- Revise PRs existentes para exemplos

## 📄 Licença

Ao contribuir, você concorda que suas contribuições serão licenciadas sob a mesma licença do projeto.

---

Obrigado por contribuir! 🙏
