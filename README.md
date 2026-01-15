# 🔐 Autenticação com Rails e Devise

Uma aplicação Rails 8.1 com autenticação segura usando Devise, artigos e interface moderna.

## 🚀 Como Rodar

### 1. Instalar dependências
```bash
bundle install
npm install
```

### 2. Criar banco de dados
```bash
rails db:create
rails db:migrate
```

### 3. Iniciar o servidor
```bash
./bin/dev
```

Acesse: `http://localhost:3000`

---

## 📖 Como Funciona o Devise

O **Devise** é uma gem que gerencia autenticação automaticamente. Ele cuida de:
- ✅ Login/Logout
- ✅ Registro de usuários
- ✅ Recuperação de senha
- ✅ Validação segura de dados

### O que ele cria automaticamente

Quando você instala Devise, ele:
1. **Cria a tabela `users`** com email e senha criptografada
2. **Gera rotas** de login, registro e logout
3. **Fornece métodos** para usar nos controllers e views

### Rotas Principais do Devise

| Rota | O que faz |
|------|-----------|
| `/users/sign_up` | Formulário de registro |
| `/users/sign_in` | Formulário de login |
| `/users/sign_out` | Fazer logout |
| `/users/password/new` | Recuperar senha |

Ver todas as rotas:
```bash
rails routes | grep devise
```

---

## 💻 Usando Devise

### No Controller - Proteger uma página

```ruby
class ArticlesController < ApplicationController
  before_action :authenticate_user!  # Só usuários logados acessam
  
  def index
    @articles = Article.all
  end
end
```

### No Controller - Acessar o usuário logado

```ruby
class ArticlesController < ApplicationController
  def create
    @article = Article.new(article_params)
    @article.user_id = current_user.id  # Pega o usuário logado
    @article.save
  end
end
```

**Métodos disponíveis:**
- `current_user` → Usuário logado
- `user_signed_in?` → Verifica se está logado
- `authenticate_user!` → Redireciona para login se não estiver

### Na View - Mostrar login/logout

```erb
<% if user_signed_in? %>
  <p>Logado como: <%= current_user.email %></p>
  <%= link_to 'Logout', destroy_user_session_path, method: :delete %>
<% else %>
  <%= link_to 'Login', new_user_session_path %>
  <%= link_to 'Registrar', new_user_registration_path %>
<% end %>
```

---

## 🔧 Customizações Rápidas

### Adicionar campo ao Usuário

```bash
rails generate migration AddNameToUsers name:string
rails db:migrate
```

No arquivo `app/models/user.rb`:
```ruby
validates :name, presence: true
```

### Redirecionar após login

No arquivo `app/controllers/application_controller.rb`:
```ruby
def after_sign_in_path_for(resource)
  articles_path  # Ir para artigos após login
end
```

### Customizar views do Devise

```bash
rails generate devise:views
```

Isso cria views em `app/views/devise/` que você pode editar.

---

## 📁 Estrutura

```
app/
├── models/user.rb          ← Modelo com Devise
├── controllers/            ← Controllers da app
└── views/                  ← Templates HTML

config/
├── routes.rb               ← Rotas (Devise já configurado)
├── initializers/
│   └── devise.rb           ← Config do Devise
└── ...
```

---

## ⚡ Próximos Passos

1. **Registre um usuário** em `/users/sign_up`
2. **Faça login** em `/users/sign_in`
3. **Crie artigos** em `/articles`
4. **Customize** as views do Devise conforme necessário

---

## 🔗 Links Úteis

- [Devise - GitHub](https://github.com/heartcombo/devise)
- [Rails Guides](https://guides.rubyonrails.org/)
- [Devise Wiki](https://github.com/heartcombo/devise/wiki)
