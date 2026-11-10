# IBCS com usuários, senhas e dados compartilhados

Este repositório mantém o site no GitHub e usa **Supabase** como backend externo:

- Supabase Auth para e-mail e senha
- PostgreSQL para organizações, perfis, clientes e dados compartilhados
- Row Level Security para separar os dados de cada empresa
- Realtime para atualizar clientes entre dispositivos

## 1. Criar o backend

1. Crie um projeto no Supabase.
2. Abra **SQL Editor** e execute `supabase/schema.sql`.
3. Em **Authentication > URL Configuration**, configure o endereço publicado do site.
4. Copie `config.example.js` como `config.js`.
5. Preencha `SUPABASE_URL` e `SUPABASE_ANON_KEY` com os dados públicos do projeto.
6. Nunca coloque a chave `service_role` no GitHub ou no navegador.

## 2. Criar a primeira empresa

1. Cadastre o primeiro usuário no painel **Authentication > Users** do Supabase ou adapte temporariamente o formulário para cadastro sem convite.
2. Entre no SQL Editor e execute, autenticado via aplicação, a função `create_organization('Nome da empresa')`.
3. O primeiro membro torna-se administrador da organização.
4. O administrador cria registros na tabela `invitations`; cada novo usuário entra usando o token do convite.

## 3. Publicar

O GitHub pode guardar o código do projeto. Para um sistema comercial que envia senhas e processa assinaturas, publique a interface na Hostinger/Vercel/Netlify e mantenha o Supabase como backend.

Arquivos:

- `index.html`: site atual
- `login.html`: login e primeiro acesso
- `plataforma.html`: clientes compartilhados
- `config.example.js`: modelo de configuração
- `js/`: integração com autenticação
- `supabase/schema.sql`: banco, funções e políticas de segurança

## 4. Dados compartilhados

A tabela `clients` guarda clientes compartilhados entre membros da mesma empresa. A tabela `shared_platform_data` pode guardar JSON para funil, catálogo, propostas, tarefas e preferências.

## 5. Segurança

- As senhas ficam no Supabase Auth, não no HTML nem no GitHub.
- A chave pública `anon` pode ficar no frontend, desde que todas as tabelas tenham RLS.
- A chave `service_role` nunca deve ser publicada.
- Ative confirmação de e-mail e políticas de senha no Supabase.
