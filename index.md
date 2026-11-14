# Product Requirements Document (PRD)
## Mapa Mental de Conhecimento

**Versão:** 1.1  
**Data:** 03 de novembro de 2026  
**Status:** Draft para validação  
**Autor:** Baseado no plano original + refinamentos de produto + feedback de experiência com Obsidian  

---

## 1. Visão do Produto

**Mapa Mental de Conhecimento** é um caderno digital onde a pessoa anota o que aprende com a leitura de livros (e outros materiais), e o sistema **conecta automaticamente as anotações por tema e palavras relacionadas** — transformando notas soltas em uma rede de conhecimento viva, pesquisável e que cresce sozinha.

### Proposta de Valor
Anotar deve ser **tão simples quanto escrever num caderno físico**, mas o sistema faz sozinho o trabalho de organizar, conectar e tornar tudo recuperável.

> “Você só escreve. O sistema descobre as relações.”

**Inspiração consciente:**  
Pegar a sensação de “cérebro conectado” do Obsidian, mas eliminar completamente a complexidade de links manuais, tags obrigatórias e estrutura.

---

## 2. Problema

Quem lê e anota em caderno físico (ou em ferramentas genéricas) enfrenta três dores principais:

| # | Dor | Consequência |
|---|-----|--------------|
| 1 | Anotações ficam perdidas | Difícil encontrar uma ideia anotada meses atrás |
| 2 | Não há conexão entre livros | Ideias do livro A que dialogam com o livro B ficam isoladas; o aprendizado não se acumula |
| 3 | Esforço alto de organização | Ferramentas como Obsidian exigem que o próprio usuário crie e mantenha os links e palavras-chave manualmente |

Ferramentas existentes resolvem parcialmente o problema, mas cobram um preço alto de fricção (links manuais, estrutura complexa, curva de aprendizado elevada).

**Dor específica validada pelo usuário:**  
Gostar da dinâmica de “tudo conectado como um cérebro”, mas achar o processo de colocar informação + palavras-chave demasiado complexo.

---

## 3. Objetivos e Métricas de Sucesso

### Objetivos do MVP (Fase 0)
- Permitir anotar de forma rápida e sem atrito (zero necessidade de tags ou links).
- Gerar conexões automáticas entre notas por tema e palavras relacionadas.
- Tornar fácil encontrar notas antigas e descobrir relações.
- Validar se o usuário volta a usar o produto por vontade própria.

### Métricas de Validação (uso pessoal – 2 a 4 semanas)
| Métrica | Critério de sucesso |
|---------|---------------------|
| Frequência de uso | Usuário volta a anotar pelo menos 2x por semana |
| Qualidade das conexões | Pelo menos 60% das conexões mostradas fazem sentido (avaliação subjetiva) |
| Recuperação de informação | Consegue encontrar uma nota antiga em < 15 segundos |
| Friction de anotação | Tempo médio para criar uma nota < 40 segundos |
| Descoberta | Usuário encontra pelo menos 1 conexão inesperada útil por semana |

---

## 4. Público-Alvo

| Fase | Público | Objetivo |
|------|---------|----------|
| Fase 0 | Você mesmo | Validação do conceito e da qualidade das conexões |
| Fase 1–2 | Pequeno grupo de leitores/testadores (5–15 pessoas) | Feedback de usabilidade e relevância das conexões |
| Fase 3+ | Estudantes, pesquisadores, autodidatas e leitores intensivos | Produto multi-usuário |

**Persona principal (MVP):**  
Pessoa que lê regularmente, anota ideias importantes e sente frustração ao não conseguir conectar ou recuperar essas ideias depois — especialmente quem já tentou ferramentas poderosas (como Obsidian) e desistiu pela complexidade.

---

## 5. Escopo

### 5.1 Dentro do Escopo (MVP – Fase 0)
- Criar, editar e excluir notas
- Título + texto livre + livro de origem (opcional)
- **Zero necessidade de tags ou links manuais**
- Busca instantânea por palavra-chave
- Extração automática de temas (TF-IDF otimizado para português + palavras relacionadas)
- Conexões automáticas entre notas
- Seção “Notas relacionadas” (top 3–5) com justificativa dos temas em comum
- Feedback de relevância da conexão (“essa conexão não faz sentido”)
- **Temas como cidadãos de primeira classe** (chips clicáveis que filtram todas as notas daquele tema)
- Salvamento local no navegador (IndexedDB)
- Exportar / Importar todas as notas em JSON
- Interface limpa e focada em velocidade de anotação

### 5.2 Fora do Escopo (MVP)
- Login / autenticação
- Sincronização entre dispositivos
- Visualização em grafo interativo completo (fica para Fase 1)
- Embeddings semânticos avançados ou IA generativa (Fase 2)
- Colaboração em tempo real
- Tags manuais obrigatórias
- Aplicativo mobile nativo
- Importação de Kindle/Goodreads

---

## 6. User Stories (MVP)

### Anotações
1. Como leitor, quero criar uma nota rapidamente (título + texto + livro opcional) **sem precisar pensar em tags ou links** para registrar uma ideia sem atrito.
2. Como leitor, quero editar ou excluir uma nota para manter minhas anotações atualizadas.
3. Como leitor, quero buscar notas por palavra-chave para encontrar rapidamente algo que anotei no passado.

### Temas e Descoberta
4. Como leitor, quero que o sistema extraia automaticamente os temas principais da minha nota para eu não precisar categorizar nada.
5. Como leitor, quero clicar em um tema e ver todas as notas relacionadas a ele (de qualquer livro) para explorar um assunto em profundidade.
6. Como leitor, ao abrir uma nota, quero ver as 3–5 notas mais relacionadas para descobrir conexões que eu não havia percebido.
7. Como leitor, quero poder marcar uma conexão como “não relevante” para o sistema aprender e melhorar.

### Dados e Segurança
8. Como usuário, quero exportar todas as minhas notas em JSON para não depender exclusivamente do navegador.
9. Como usuário, quero importar um arquivo JSON para recuperar minhas notas em outro navegador ou após limpar dados.

---

## 7. Requisitos Funcionais Detalhados

### 7.1 Gestão de Notas
- Campos: `id`, `title`, `content`, `book` (opcional), `createdAt`, `updatedAt`, `themes[]`
- Criação e edição com foco em velocidade (atalhos de teclado desejáveis)
- Exclusão com confirmação simples
- Lista de notas ordenada por data de atualização (mais recentes primeiro)
- Busca em tempo real no título e no conteúdo
- **Nenhuma obrigação de preencher tags ou criar links**

### 7.2 Extração de Temas (Motor de Conhecimento)
- Processamento 100% no cliente
- Algoritmo: TF-IDF com melhorias para português:
  - Lista de stopwords em PT-BR
  - Normalização de acentos e caixa
  - Lematização básica ou stemming (quando viável no browser)
  - Consideração de palavras semanticamente próximas quando possível (lista simples de sinônimos no MVP)
- Extrair de 3 a 8 temas por nota
- Temas são recalculados sempre que a nota é salva

### 7.3 Sistema de Conexões
- Similaridade baseada em overlap de temas + peso TF-IDF + palavras relacionadas
- Mostrar apenas as **top 3 a 5** notas mais relacionadas
- Exibir o(s) tema(s) em comum como justificativa da conexão
- Botão de feedback negativo por conexão (“não faz sentido”)
- Feedback negativo reduz o peso daquela combinação de temas (armazenado localmente)

### 7.4 Temas como cidadãos de primeira classe
- Cada tema aparece como chip clicável na nota e na lista
- Ao clicar em um tema, o usuário vê uma view filtrada com todas as notas que contêm aquele tema
- Contagem de notas por tema
- Esta view funciona como uma forma simples e poderosa de explorar o conhecimento sem precisar de grafo

### 7.5 Persistência e Portabilidade
- Armazenamento: IndexedDB via Dexie.js
- Exportação: arquivo `.json` com todas as notas + metadados
- Importação: sobrescreve ou mescla (usuário escolhe)
- Dados nunca saem do dispositivo na Fase 0

### 7.6 Interface
- Layout limpo, tipografia legível, hierarquia visual clara
- Fluxo principal: Lista de notas → Criar/Editar nota → Detalhe da nota + relacionadas
- View de Tema (filtro por tema)
- Mobile-first (responsivo)
- Modo claro (modo escuro pode ficar para depois)

---

## 8. Requisitos Não-Funcionais

| Categoria | Requisito |
|-----------|-----------|
| Performance | Criar/salvar nota < 300 ms |
| Performance | Busca e cálculo de relacionados < 150 ms (até ~500 notas) |
| Offline | Funciona 100% offline após o primeiro carregamento |
| Privacidade | Nenhum dado sai do dispositivo na Fase 0 |
| Usabilidade | Criar uma nota completa em menos de 40 segundos |
| Usabilidade | Zero curva de aprendizado para anotar (não precisa saber o que é tag ou link) |
| Compatibilidade | Chrome, Firefox, Safari, Edge (últimas 2 versões) |
| Acessibilidade | Navegação básica por teclado + contraste adequado |

---

## 9. Arquitetura Técnica (Fase 0)

```
[ Navegador ]
├── Frontend (Next.js + React + Tailwind)
├── Motor de Temas (TF-IDF + stopwords PT-BR + sinônimos simples)
├── Motor de Similaridade
└── Persistência (IndexedDB via Dexie.js)
```

### Stack recomendada

| Camada | Tecnologia | Motivo |
|--------|------------|--------|
| Framework | Next.js (App Router) | Deploy trivial na Vercel + React moderno |
| Estilo | Tailwind CSS | Velocidade de desenvolvimento e consistência |
| Estado + DB local | Dexie.js | IndexedDB tipado e simples |
| Análise de texto | Biblioteca custom + stopwords PT-BR | Controle total e leveza |
| Hospedagem | Vercel (plano gratuito) | Deploy automático via GitHub |
| Versionamento | GitHub | Histórico e colaboração futura |

**Nota:** Embeddings e IA generativa ficam explicitamente fora do MVP.

---

## 10. Modelo de Dados (Fase 0)

```ts
interface Note {
  id: string;                 // uuid
  title: string;
  content: string;
  book?: string;              // opcional
  themes: string[];           // extraídos automaticamente
  createdAt: string;          // ISO
  updatedAt: string;          // ISO
}

interface ConnectionFeedback {
  fromNoteId: string;
  toNoteId: string;
  negative: boolean;          // true = usuário marcou como ruim
  createdAt: string;
}
```

---

## 11. Roadmap

### Fase 0 — Protótipo Navegável (MVP)
**Objetivo:** Você usar por 2–4 semanas e validar se as conexões agregam valor e se a simplicidade de anotação se sustenta.

- CRUD de notas (zero tags/links manuais)
- Busca instantânea
- Extração de temas (TF-IDF + PT-BR + palavras relacionadas)
- Notas relacionadas (top 3–5) com justificativa
- Feedback de conexão
- **Temas clicáveis** (view filtrada por tema)
- Export / Import JSON
- Interface focada em velocidade

### Fase 1 — Visualização em Mapa Mental + Descoberta
- Mapa mental interativo simples (nós = notas, arestas = conexões)
- Filtros por livro e por tema
- Zoom, pan e destaque de caminhos
- **Modo “Surpresa”**: botão que mostra uma conexão inesperada entre notas antigas
- Melhoria da relevância das conexões com base no feedback do usuário

### Fase 2 — Inteligência Semântica
- Embeddings (cliente ou edge)
- Conexões por significado real (sinônimos e ideias próximas sem palavras iguais)
- Sugestão de temas mais inteligentes
- Possível chat simples sobre as próprias notas

### Fase 3 — Multi-usuário e Sincronização
- Autenticação (Supabase Auth)
- Migração IndexedDB → Postgres
- Sincronização entre dispositivos
- Compartilhamento opcional de notas/coleções
- Deploy contínuo

---

## 12. Riscos e Mitigações

| Risco | Impacto | Mitigação |
|-------|---------|-----------|
| Conexões pouco relevantes (ruído) | Alto | Limitar a top 3–5 + botão de feedback negativo + mostrar temas em comum |
| Qualidade ruim do TF-IDF em português | Alto | Stopwords PT-BR + normalização + lista de sinônimos simples + testes com textos reais |
| Dados presos no navegador | Médio | Export/Import JSON desde o dia 1 |
| Escopo crescer demais | Alto | Disciplina rigorosa no roadmap; nada de grafo completo ou IA no MVP |
| Baixa adesão (fricção na anotação) | Alto | Priorizar velocidade e simplicidade absoluta da interface de criação |
| Usuário sentir falta do “mapa mental” cedo demais | Médio | Temas clicáveis já entregam boa parte do valor de exploração na Fase 0 |
| Performance com muitas notas | Médio | Limitar cálculo de similaridade e usar índices simples |

---

## 13. Plano de Validação

Durante o uso pessoal (2–4 semanas), responder honestamente:

1. Eu volto ao app para anotar por vontade própria?
2. As conexões automáticas me surpreendem positivamente com frequência?
3. Consigo encontrar informações antigas em segundos?
4. Os temas clicáveis me ajudam a explorar assuntos?
5. O que eu mais senti falta?

**Critério de go/no-go:**  
Se pelo menos 3 das 5 respostas forem positivas → avançar para Fase 1 (mapa mental + modo surpresa).  
Caso contrário → iterar no MVP antes de investir em visualização mais complexa.

---

## 14. Premissas e Restrições

- O usuário principal da Fase 0 é uma única pessoa (você).
- O produto deve funcionar offline.
- Não haverá custos de API na Fase 0.
- A qualidade das conexões pode (e deve) ser imperfeita no início.
- O sucesso do produto depende mais da **facilidade de anotar** do que da perfeição das conexões.
- A complexidade do Obsidian deve ser conscientemente evitada: zero links manuais, zero tags obrigatórias.

---

## 15. Próximos Passos Recomendados

1. Validar este PRD atualizado (versão 1.1).
2. Desenhar as telas adicionais:
   - View de Tema (lista filtrada por tema)
   - Como os chips de tema aparecem e se comportam
3. Iniciar desenvolvimento da Fase 0.
4. Usar o próprio produto por no mínimo 2 semanas antes de qualquer expansão.

---

## 16. Apêndice — Evolução do Documento

### Versão 1.0 → 1.1 (melhorias a partir do feedback de Obsidian)

| Melhoria | O que mudou |
|---------|-------------|
| **Temas como cidadãos de primeira classe** | Chips clicáveis + view filtrada por tema (nova forma de exploração sem grafo) |
| **Zero fricção de entrada** | Reforço explícito de que não há tags nem links manuais |
| **Palavras relacionadas** | Conexões consideram não só palavras iguais, mas também relacionadas |
| **Modo Surpresa** | Planejado para Fase 1 (descoberta de conexões inesperadas) |
| **Mapa mental** | Mantido na Fase 1, mas a exploração já começa forte na Fase 0 via temas |
| **Inspiração consciente** | Deixar claro o que pegamos do Obsidian e o que descartamos |
| **Métricas** | Adicionada métrica de “descoberta de conexões inesperadas” |
| **Validação** | Critérios atualizados com a pergunta sobre temas clicáveis |

---

*Documento vivo. Deve ser atualizado conforme o aprendizado real com o uso do produto.*
