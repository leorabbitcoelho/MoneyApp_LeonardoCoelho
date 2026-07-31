# Configuração do Firebase — página dos bonés

A página `index.html` grava os cadastros de interesse no Firestore e tem um
painel de gestão em `/#admin`. Siga os passos abaixo **uma vez**.

> ⚠️ O Firebase **não funciona no preview do artifact** (o claude.ai bloqueia
> conexões externas). Ele só conecta no site publicado (Firebase Hosting).
> Sem a config preenchida, a página continua funcionando **só com o WhatsApp**.

## 1. Firestore
Firebase Console → **Firestore Database** → *Criar banco de dados* → modo de
produção. A coleção `interesses` é criada sozinha no primeiro cadastro.

## 2. Regras de segurança
Firestore → aba **Regras** → cole o conteúdo de [`firestore.rules`](./firestore.rules)
→ **Publicar**. Elas deixam qualquer um se cadastrar, mas só você (logado) ler
e marcar como pago.

## 3. Login do painel (a "senha")
Authentication → **Sign-in method** → ative **E-mail/senha**.
Depois em **Users** → *Adicionar usuário*:
- E-mail: `leonardorabbit@gmail.com` (o mesmo do `ADMIN_EMAIL` no `index.html`)
- Senha: a que você quiser — **é essa a senha que você digita no `/#admin`**.

Se trocar o e-mail, troque nos dois lugares: `ADMIN_EMAIL` (index.html) e no
`firestore.rules`.

## 4. Config Web
Console → **Configurações do projeto** → *Seus apps* → app Web → **SDK/Config**.
Copie os valores e cole no `index.html`, no objeto `FIREBASE_CONFIG`:

```js
var FIREBASE_CONFIG = {
  apiKey: "...",
  authDomain: "pescapp-app-loja.firebaseapp.com",
  projectId: "pescapp-app-loja",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```
(pode colar aqui no chat que eu já deixo preenchido.)

## 5. Publicar (Firebase Hosting)
```bash
npm install -g firebase-tools
firebase login
firebase init hosting     # public: a pasta com o index.html; SPA: não
firebase deploy
```
Site: `https://pescapp-app-loja.web.app`
Painel: `https://pescapp-app-loja.web.app/#admin`

## Estrutura de cada cadastro
`interesses/{id}` → `{ nome, telefone, quantidade, total, pago:false, status:"novo", criadoEm }`
