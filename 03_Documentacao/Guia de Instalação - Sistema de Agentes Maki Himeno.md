# Guia de Instalação - Sistema de Agentes Maki Himeno

## Pré-requisitos

### Software Necessário
- Python 3.11+
- Node.js 20+
- Docker (opcional, para n8n)
- Git

### Contas e APIs Necessárias
- Runpod (para ComfyUI + Flux)
- OpenAI API (para chatbots)
- Fanvue/Fanbox (monetização NSFW)
- Instagram/TikTok (conteúdo SFW)
- Mailchimp (e-mail marketing)

## Instalação Passo a Passo

### 1. Configuração do Ambiente Python

```bash
# Clonar repositório Kohya_ss
git clone https://github.com/bmaltais/kohya_ss.git
cd kohya_ss

# Criar ambiente virtual
python3.11 -m venv .venv
source .venv/bin/activate

# Instalar dependências
pip install -r requirements.txt
```

### 2. Configuração do n8n

#### Opção A: Via npm
```bash
npm install -g n8n
n8n
```

#### Opção B: Via Docker
```bash
# Criar docker-compose.yml
version: '3.8'
services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    volumes:
      - ~/.n8n:/home/node/.n8n
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=maki_massa
      - N8N_BASIC_AUTH_PASSWORD=MakiN8n2025!

# Iniciar n8n
docker-compose up -d
```

### 3. Configuração das APIs

#### Runpod
1. Acesse https://runpod.io
2. Vá em Settings > API Keys
3. Crie uma nova chave API
4. Configure no arquivo de ambiente:
```bash
export RUNPOD_API_KEY="sua_chave_aqui"
```

#### OpenAI
```bash
export OPENAI_API_KEY="sua_chave_openai"
export OPENAI_API_BASE="https://api.openai.com/v1"
```

### 4. Instalação do Sistema Maki

```bash
# Baixar arquivos do sistema
wget https://exemplo.com/maki_agent_system.py
wget https://exemplo.com/n8n_workflow_example.json

# Tornar executável
chmod +x maki_agent_system.py

# Testar instalação
python3 maki_agent_system.py
```

### 5. Importar Workflow no n8n

1. Acesse n8n em http://localhost:5678
2. Faça login com: maki_massa / MakiN8n2025!
3. Clique em "Import from file"
4. Selecione o arquivo `n8n_workflow_example.json`
5. Configure as credenciais das APIs
6. Ative o workflow

## Configuração das Plataformas de Monetização

### Fanvue (NSFW)
1. Criar conta em https://fanvue.com
2. Configurar perfil da Maki Himeno
3. Definir tiers de preços:
   - SFW: ¥500/mês
   - NSFW: ¥2000/mês
4. Configurar API para uploads automáticos

### Instagram/TikTok (SFW)
1. Criar contas comerciais
2. Configurar Instagram Business API
3. Configurar TikTok for Business
4. Implementar posting automático via n8n

### Mailchimp (E-mail Marketing)
1. Criar conta Mailchimp
2. Configurar lista de e-mails
3. Criar sequências de automação
4. Integrar com n8n para lead magnets

## Estrutura de Diretórios

```
/home/ubuntu/
├── maki_agent_system.py          # Sistema principal
├── n8n_workflow_example.json     # Workflow n8n
├── kohya_ss/                     # Treino de LoRA
├── generated_images/             # Imagens geradas
├── loras/                        # Modelos LoRA treinados
├── datasets/                     # Datasets para treino
└── logs/                         # Logs do sistema
```

## Execução e Monitorização

### Execução Manual
```bash
# Executar operações diárias
python3 maki_agent_system.py

# Executar com logs
python3 maki_agent_system.py > logs/daily_$(date +%Y%m%d).log 2>&1
```

### Execução Automática (Cron)
```bash
# Editar crontab
crontab -e

# Adicionar linha para execução diária às 9h
0 9 * * * cd /home/ubuntu && python3 maki_agent_system.py >> logs/daily.log 2>&1
```

### Monitorização via n8n
- Acesse http://localhost:5678
- Verifique execuções em "Executions"
- Configure alertas por e-mail para falhas

## Métricas e KPIs

### Métricas Financeiras
- Receita mensal total
- ROI (Return on Investment)
- Custo por lead
- Valor médio por cliente

### Métricas Operacionais
- Imagens geradas por semana
- Taxa de conversão dos funis
- Tempo de resposta dos chatbots
- Engajamento nas redes sociais

### Relatórios Automáticos
- Relatório diário por e-mail
- Dashboard no n8n
- Métricas em tempo real

## Solução de Problemas

### Problemas Comuns

#### n8n não inicia
```bash
# Verificar se a porta está ocupada
netstat -tulpn | grep 5678

# Reiniciar Docker
docker-compose restart
```

#### Erro na geração de imagens
```bash
# Verificar chave API Runpod
curl -H "Authorization: Bearer $RUNPOD_API_KEY" https://api.runpod.ai/v2/status

# Verificar logs
tail -f logs/daily.log
```

#### Chatbot não responde
```bash
# Verificar chave OpenAI
python3 -c "import openai; print(openai.api_key)"

# Testar conexão
curl -H "Authorization: Bearer $OPENAI_API_KEY" https://api.openai.com/v1/models
```

## Segurança e Melhores Práticas

### Segurança das APIs
- Nunca commitar chaves API no código
- Usar variáveis de ambiente
- Rotacionar chaves regularmente
- Monitorizar uso das APIs

### Backup e Recuperação
```bash
# Backup diário dos dados
tar -czf backup_$(date +%Y%m%d).tar.gz generated_images/ loras/ datasets/

# Backup da configuração n8n
docker exec n8n_container tar -czf - /home/node/.n8n > n8n_backup_$(date +%Y%m%d).tar.gz
```

### Conformidade Legal
- Aplicar censura adequada em conteúdo NSFW
- Marcar conteúdo como "AI-generated"
- Respeitar termos de serviço das plataformas
- Implementar idade mínima para conteúdo NSFW

## Suporte e Manutenção

### Atualizações Regulares
- Atualizar dependências Python mensalmente
- Atualizar n8n para versões estáveis
- Monitorizar mudanças nas APIs das plataformas

### Logs e Monitorização
```bash
# Visualizar logs em tempo real
tail -f logs/daily.log

# Analisar performance
grep "ERROR" logs/*.log
grep "SUCCESS" logs/*.log | wc -l
```

### Contato para Suporte
- Documentação: Este guia
- Logs: Diretório `/home/ubuntu/logs/`
- Configuração: Arquivos `.env` e `docker-compose.yml`

---

**Sistema Maki Himeno v1.0**  
Desenvolvido por Manus AI  
Data: 2025

