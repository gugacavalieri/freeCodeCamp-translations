Como Construir Seu Próprio React Hook: Um guia passo a passo

React Hooks customizáveis são uma ferramenta essencial que permite que você adicione funcionalidades exclusivas às suas aplicações, React.

Em vários casos, se você quer adicionar uma certa funcionalidade à sua aplicação, você pode simplesmente adicionar uma biblioteca de terceiros que já existe para resolver o seu problema. Mas, se tal biblioteca não existir, o que você pode fazer?

Como um desenvolvedor React, é importante aprender os processos de criar Hooks customizáveis para resolver problemas ou adicionar funcionalidades que faltam aos seus próprios projetos.

Nesse guia passo a passo, eu vou te mostrar como criar os seus próprios React Hooks decompondo três Hooks que fiz para as minhas aplicações, e quais problemas eles foram criados para resolver.

1. useCopyToClipboard Hook

Em uma versão anterior do meu site, reedbarger.com, eu permitia que usuários copiassem o código dos meus artigos com a ajuda de um pacote chamado react-copy-to-clipboard.

Um usuário passa um mouse sobre o fragmento de código, clica no botão de copiar, e o código é adicionado para a área de transferência, podendo ele então colar e usar o código onde ele quiser.

copy-gif.gif

Ao invés de usar uma biblioteca de terceiros, no entanto, eu queria recriar essa funcionalidade com o meu próprio React Hook. Como qualquer outro hook customizável que eu crio, eu os coloco em uma pasta dedicada, geralmente chamada "utils" ou "lib", especificamente para funções que eu posso re-utilizar na minha aplicação.

Vamos colocar esse hook em um arquivo chamado useCopyToClipboard.js e eu vou criar uma função com o mesmo nome.

Existem várias maneiras de copiar um texto para a área de transferência de um usuário. Eu prefiro utilizar uma biblioteca para isso chamada copy-to-clipboard, o que torna o processo mais confiável.

Ela exporta uma função que chamaremos de copy.

```js
// utils/useCopyToClipboard.js
import React from "react";
import copy from "copy-to-clipboard";

export default function useCopyToClipboard() {}
```

Em seguida, vamos criar uma função que será utilizada para copiar qualquer texto que deve ser adicionado a área de transferência de um usuário. Vamos chamar essa função handleCopy.

# Como criar a função handleCopy

Dentro da função, primeiramente, precisamos nos assegurar que ela aceitará apenas parâmetros do tipo String ou Number. Vamos colocar uma declaração com if-else, que garantirá que o tipo é uma String ou um Number. Caso contrário, vamos imprimir um erro no console que avisa o usuário que ele não pode copiar qualquer outro tipo.

```js
import React from "react";
import copy from "copy-to-clipboard";

export default function useCopyToClipboard() {
  const [isCopied, setCopied] = React.useState(false);

  function handleCopy(text) {
    if (typeof text === "string" || typeof text == "number") {
      // pode copiar
    } else {
      // não pode copiar
      console.error(
        `Não é permitido copiar o tipo ${typeof text} para a área de transferência, deve ser uma String ou um Number.`
      );
    }
  }
}
```

Em seguida, devemos conveter o parâmetro `text` para String e passá-lo para a função copy. Depois, retornamos a função handleCopy dentro do hook para ser utilizada em qualquer lugar da nossa aplicação.

Normalmente, a função `handleCopy` vai ser ligada a um evento `onClick` de um botão.

```js
import React from "react";
import copy from "copy-to-clipboard";

export default function useCopyToClipboard() {
  function handleCopy(text) {
    if (typeof text === "string" || typeof text == "number") {
      copy(text.toString());
    } else {
      console.error(
        `Não é permitido copiar o tipo ${typeof text} para a área de transferência, deve ser uma String ou um Number.`
      );
    }
  }

  return handleCopy;
}
```