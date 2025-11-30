# Inspeção On-Line v2.0 - GitHub Pages

Formulário web moderno e responsivo para inspeções de usinas termelétricas, com suporte para Inspeção Interna e Externa.

## 🚀 Características

- ✅ **Interface Mobile-First**: Otimizado para dispositivos móveis e tablets
- ✅ **Componentes de Range Inteligente**: Botões +/- em vez de barras de rolagem
- ✅ **Múltiplas Janelas (Modais)**: Organização clara de campos por categoria
- ✅ **Integração Google Apps Script**: Envio de dados para Google Sheets
- ✅ **Persistência Local**: Salva dados automaticamente no localStorage
- ✅ **Carregamento de Inspeções Anteriores**: Reutilize dados de inspeções passadas
- ✅ **Validação de Campos**: Garante que todos os campos obrigatórios sejam preenchidos
- ✅ **Design Profissional**: Tailwind CSS 4 + shadcn/ui

## 📋 Conteúdo do Projeto

```
insp_github_pages/
├── client/
│   ├── public/              # Arquivos estáticos (logo, favicon)
│   ├── src/
│   │   ├── components/      # Componentes React reutilizáveis
│   │   ├── hooks/           # Hooks customizados (useInspectionData)
│   │   ├── lib/             # Estrutura de dados do formulário
│   │   ├── pages/           # Páginas (Home, InspectionForm)
│   │   ├── App.tsx          # Roteamento principal
│   │   ├── main.tsx         # Entry point
│   │   └── index.css        # Estilos globais (Tailwind)
│   └── index.html           # Template HTML
├── package.json             # Dependências do projeto
├── vite.config.ts           # Configuração Vite
├── tsconfig.json            # Configuração TypeScript
└── README_GITHUB_PAGES.md   # Este arquivo
```

## 🔧 Instalação Local

### Pré-requisitos
- Node.js 18+ e npm/pnpm
- Git

### Passos

1. **Clone o repositório** (após fazer upload para GitHub):
```bash
git clone https://github.com/seu-usuario/insp_github_pages.git
cd insp_github_pages
```

2. **Instale as dependências**:
```bash
pnpm install
# ou
npm install
```

3. **Inicie o servidor de desenvolvimento**:
```bash
pnpm dev
# ou
npm run dev
```

4. **Acesse no navegador**:
```
http://localhost:5173
```

## 📦 Build para Produção

Para criar uma versão otimizada para produção:

```bash
pnpm build
# ou
npm run build
```

Os arquivos compilados estarão em `dist/` e prontos para deploy.

## 🌐 Deploy no GitHub Pages

### Opção 1: Usando GitHub Actions (Automático)

1. **Crie um arquivo `.github/workflows/deploy.yml`** na raiz do repositório:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'pnpm'
      
      - name: Install pnpm
        run: npm install -g pnpm
      
      - name: Install dependencies
        run: pnpm install
      
      - name: Build
        run: pnpm build
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

2. **Configure o repositório para usar GitHub Pages**:
   - Vá para Settings → Pages
   - Source: Deploy from a branch
   - Branch: gh-pages
   - Folder: / (root)
   - Clique em Save

3. **Faça push para main**:
```bash
git add .
git commit -m "Deploy para GitHub Pages"
git push origin main
```

O site será automaticamente compilado e publicado em:
```
https://seu-usuario.github.io/insp_github_pages
```

### Opção 2: Deploy Manual

1. **Compile o projeto**:
```bash
pnpm build
```

2. **Crie um branch `gh-pages`** (se não existir):
```bash
git checkout --orphan gh-pages
git rm -rf .
```

3. **Copie os arquivos compilados**:
```bash
git checkout main -- dist/
cp -r dist/* .
rm -rf dist
```

4. **Faça commit e push**:
```bash
git add .
git commit -m "Deploy para GitHub Pages"
git push origin gh-pages
```

5. **Configure no GitHub**:
   - Settings → Pages
   - Source: Deploy from a branch
   - Branch: gh-pages
   - Folder: / (root)

## ⚙️ Configuração

### Google Apps Script URLs

As URLs dos WebApps do Google Apps Script estão configuradas em `client/src/hooks/useInspectionData.ts`:

```typescript
const SCRIPT_URL_INTERNA = "https://script.google.com/macros/s/...";
const SCRIPT_URL_EXTERNA = "https://script.google.com/macros/s/...";
const SCRIPT_URL_CARREGAR_INTERNA = "https://script.google.com/macros/s/...";
const SCRIPT_URL_CARREGAR_EXTERNA = "https://script.google.com/macros/s/...";
```

Se precisar atualizar as URLs, edite este arquivo e recompile.

### Logo da Empresa

Substitua o arquivo `client/public/logo.png` pela logo da sua empresa (recomendado: 200x200px).

## 📱 Estrutura de Dados

### Inspeção Interna

- **Dados Iniciais**: Data, hora, operador, supervisor, turma, turno, status
- **Unidades Geradoras**: 23 motores com status, níveis de óleo, UNIC
- **Geradores AVK**: 23 geradores com status, aquecedores, mancais
- **Nível VBA**: 4 tanques (VBA901-904)
- **Compressores de Partida**: 4 equipamentos (TSA901.1, TSA901.2, TSA902.1, TSA902.2)
- **Compressores de Instrumentação**: 3 equipamentos (TCA901, TCA902, TCA903)
- **Separadoras de Óleo**: 23 QBB com status, carter, vazão, temperatura, rotação
- **Anormalidades**: Campos para descrever problemas e observações

### Inspeção Externa

- **Dados Iniciais**: (Mesmo da Interna)
- **Bomba dos Poços**: 2 bombas com status e hidrômetro
- **Container Incêndio**: Bombas Jockey, Sprinkler, Diesel, baterias, radiador
- **ETA**: Tratamento abrandado, osmose reversa, químicos, pH
- **Tancagem**: HFO, LFO com volumes e temperaturas
- **Separadoras de HFO**: 6 equipamentos com status, temperatura, vazão
- **Subestação**: 2 transformadores com status, temperatura, óleo isolante, sílica
- **Temperaturas de Salas**: 9 salas com temperatura e umidade
- **Anormalidades**: (Mesmo da Interna)

## 🔄 Fluxo de Uso

1. **Acesse a página inicial** e escolha Inspeção Interna ou Externa
2. **Clique nas janelas** para abrir os formulários modais
3. **Preencha os campos** com os dados da inspeção
4. **Use os botões +/-** para ajustar valores de range
5. **Clique em "Carregar Inspeção Anterior"** para reutilizar dados
6. **Clique em "Enviar Relatório Completo"** para salvar no Google Sheets
7. **Os dados são salvos automaticamente** no localStorage do navegador

## 🛠️ Tecnologias Utilizadas

- **React 19**: Framework UI
- **TypeScript**: Tipagem estática
- **Tailwind CSS 4**: Estilos responsivos
- **shadcn/ui**: Componentes UI prontos
- **Vite**: Build tool rápido
- **Wouter**: Roteamento leve
- **Sonner**: Notificações toast

## 📝 Licença

Este projeto é de propriedade da Termelétrica Pernambuco III.

## 📞 Suporte

Para problemas ou dúvidas, entre em contato com a equipe de desenvolvimento.

---

**Versão**: 2.0  
**Última atualização**: Novembro 2025  
**Status**: Pronto para produção
