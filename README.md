# FANBOL — Painel de CAP

Dashboard de gestão de CAP da liga, com trade ao vivo sincronizado entre os 32 GMs.

## Como publicar no GitHub Pages com trade funcionando

O `index.html` já está pronto pra rodar sozinho, mas o trade compartilhado
precisa de um banco de dados gratuito (Firebase) porque GitHub Pages só
hospeda arquivo estático.

### 1. Criar o banco de dados (Firebase — grátis)

1. Acesse **https://console.firebase.google.com** e entre com sua conta Google.
2. **Adicionar projeto** → dê um nome (ex: `fanbol-cap`) → pode desativar o
   Google Analytics → **Criar projeto**.
3. No menu lateral, clique em **Compilação → Realtime Database**.
4. **Criar banco de dados** → escolha uma localização → comece em
   **modo de teste** (regras abertas).
5. Depois de criado, vá na aba **Regras** e troque pelo seguinte (deixa as
   regras abertas permanentemente, em vez de expirar em 30 dias):
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
   ⚠️ Isso significa que qualquer pessoa com o link consegue ler e escrever
   os dados — é o mesmo nível de confiança "entre amigos da liga" que o
   painel já tinha antes. Não coloque nada sensível nesse banco.

6. Volte em **Configurações do projeto** (ícone de engrenagem, canto
   superior esquerdo) → aba **Geral** → role até **Seus apps** → clique no
   ícone **`</>`** (Web) → dê um apelido pro app → **Registrar app**.
7. Copie o objeto `firebaseConfig` que aparece na tela. Algo assim:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "fanbol-cap.firebaseapp.com",
     databaseURL: "https://fanbol-cap-default-rtdb.firebaseio.com",
     projectId: "fanbol-cap",
     storageBucket: "fanbol-cap.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```

### 2. Colar a configuração no arquivo

Abra o `index.html` num editor de texto, procure por `FIREBASE_CONFIG`
(perto do fim do arquivo) e cole os valores que você copiou:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIzaSy...",
  authDomain: "fanbol-cap.firebaseapp.com",
  databaseURL: "https://fanbol-cap-default-rtdb.firebaseio.com",
  projectId: "fanbol-cap",
  storageBucket: "fanbol-cap.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

Salva o arquivo.

### 3. Subir pro GitHub

1. Crie um repositório novo no GitHub (pode ser público), ex: `fanbol-cap`.
2. Suba o `index.html` pra raiz do repositório (dá pra arrastar e soltar
   direto pela interface do GitHub, em **Add file → Upload files**).
3. Vá em **Settings → Pages**.
4. Em **Source**, escolha **Deploy from a branch**, branch **main**,
   pasta **/ (root)** → **Save**.
5. Espera 1-2 minutos e o GitHub te dá um link tipo
   `https://seu-usuario.github.io/fanbol-cap/`.

Manda esse link pro grupo da liga — agora todo mundo edita o mesmo banco de
dados em tempo real, sem precisar de conta na Claude.

### Observação

Esse mesmo `index.html` também continua funcionando normalmente se você
preferir publicar direto pelo artifact da Claude (o `FIREBASE_CONFIG` fica
ignorado nesse caso, porque a Claude usa o próprio sistema de storage dela).
Ou seja: um arquivo só, funciona nos dois lugares.
