# Controle de Estoque — como colocar para funcionar

Arquivos:

- `index.html`: o sistema (uma página só)
- `schema.sql`: cria o banco de dados
- `historico_inventarios_2026.xlsx`: inventários de fevereiro, maio e agosto, prontos para importar
- `modelo_importacao.xlsx`: modelo de planilha para um novo ciclo

Tudo é gratuito: **Supabase** (login e banco de dados) e **GitHub Pages** (endereço na internet).
Os nomes dos menus do Supabase podem variar um pouco, porque o painel muda de tempos em tempos.

## 1. Criar o banco no Supabase

1. Acesse **supabase.com** → *Start your project* e entre com uma conta GitHub ou com e-mail.
2. Clique em **New project**:
   - Name: `controle-estoque`
   - Database password: crie uma senha e guarde-a
   - Region: **South America (São Paulo)**
   - Clique em *Create new project* e aguarde cerca de 2 minutos.
3. No menu da esquerda, abra **SQL Editor** → *New query*.
4. Abra o arquivo `schema.sql` no Bloco de Notas, copie tudo, cole no editor e clique em **Run**.
   O resultado deve ser *"Success. No rows returned"*.

## 2. Bloquear cadastro público e criar os usuários

1. Vá em **Authentication** → **Sign In / Providers** (em alguns painéis: *Settings*).
   - Deixe o provedor **Email** ligado.
   - **Desligue** a opção *Allow new users to sign up*. Assim só entra quem você cadastrar.
2. Vá em **Authentication** → **Users** → **Add user** → *Create new user*.
   - Informe o e-mail e a senha da pessoa.
   - Marque **Auto Confirm User**.
   - Repita para cada pessoa que vai usar o sistema.

## 3. Ligar o sistema ao banco

1. Vá em **Project Settings** (engrenagem) → **API** / **API Keys** e copie:
   - **Project URL** (ex.: `https://abcdefgh.supabase.co`)
   - A chave **anon public** (ou **publishable**, que começa com `sb_publishable_`)
2. Abra o `index.html` no Bloco de Notas e preencha as duas linhas logo no início do código:

   ```js
   const SUPABASE_URL = 'https://abcdefgh.supabase.co';
   const SUPABASE_KEY = 'cole-aqui-a-chave-anon-ou-publishable';
   ```

3. Salve o arquivo.

> ⚠️ Use **somente** a chave *anon/publishable*. **Nunca** coloque no arquivo a chave *service_role* ou *secret*.
> A chave anon pode ficar pública porque o banco só libera dados para quem fez login.

## 4. Publicar na internet (GitHub Pages)

1. Acesse **github.com**, entre na sua conta e clique em **New repository**.
   - Nome: `controle-estoque`
   - Marque **Public** (o GitHub Pages gratuito exige repositório público; os dados ficam no Supabase, protegidos por login)
   - Clique em *Create repository*.
2. Clique em **uploading an existing file**, arraste o `index.html` já configurado e clique em **Commit changes**.
3. Vá em **Settings** → **Pages**:
   - *Source*: **Deploy from a branch**
   - *Branch*: `main` / `(root)`
   - Clique em **Save**.
4. Depois de 1 ou 2 minutos, o sistema estará em:
   **`https://SEU-USUARIO.github.io/controle-estoque/`**

Para atualizar o sistema no futuro, basta enviar um novo `index.html` para o mesmo repositório.

## 5. Primeiro acesso

1. Abra o endereço e entre com um dos usuários criados.
2. Em **Amostragem**, clique em **+ Novo ciclo (importar Excel)** e escolha o arquivo `historico_inventarios_2026.xlsx`.
   - A opção "Arquivo de histórico" é marcada sozinha.
   - Clique em **Importar**. Serão criados os 3 ciclos já fechados e você será levado ao **Dashboard**.

## 6. Uso no dia a dia

| Etapa | Tela | O que fazer |
|---|---|---|
| 1 | **Amostragem** | Clique em *Novo ciclo*, anexe o Excel tirado do sistema (Código, Descrição, U.M., Armazém, Saldo em Estoque, Valor) e informe a data. Digite a contagem em **Amostra Conferida**: o sistema calcula Divergência, Status (Correto/Divergente) e Dado Identificado (Certo / Baixa / Falha Protheus). Enter pula para o próximo item. Funciona no celular. |
| 2 | **Amostragem** | Clique em **Enviar para Inventário**. |
| 3 | **Inventário** | Mostra o resultado da conferência (somente leitura). |
| 4 | **Dados Inventário** | Para cada divergência, escolha o **Ajuste** (Certo, Baixa, Baixa 24 Horas, Falha Protheus), confira o **Saldo ajustado** e anote observações. Clique em **Fechar ciclo e atualizar Estoque**. |
| 5 | **Estoque** | Mostra o saldo final ajustado de cada item. |
| 6 | **Dashboard** | Mostra indicadores do ciclo, o comparativo entre ciclos e as 10 maiores divergências em R$. |
| — | **Dados Estoque Hoje** | Anexe o Excel do sistema apenas para consulta. Não altera nenhuma outra tela. |

Regra de identificação: **Divergência = Contagem − Saldo**.

- Diferença zero → **Certo**
- Diferença negativa (falta) → **Baixa**
- Diferença positiva (sobra) → **Falha Protheus**

Linhas repetidas na planilha (mesmo código e armazém) são somadas na importação.
Todas as telas têm o botão **Exportar Excel**.
Errou algo? Em Dados Inventário há os botões **Voltar para Amostragem** e **Reabrir correção**.

## Bom saber

- **Pausa por inatividade:** no plano gratuito, o Supabase pausa o projeto depois de cerca de 7 dias sem nenhum acesso. Se isso acontecer, entre no painel do Supabase e clique em **Restore project**. Os dados não se perdem. Com uso semanal, isso não acontece.
- **Senha esquecida:** em *Authentication → Users*, exclua o usuário e crie-o de novo com uma nova senha. Os dados do estoque não são afetados.
- **Backup:** use os botões *Exportar Excel* de vez em quando.
