# Sintaxe Básica do Playwright com JavaScript

Para usar o Playwright, a sintaxe segue geralmente o padrão page.comando(), onde page representa a página do navegador que está sendo controlada.
Os exemplos a seguir usam uma abordagem assíncrona, que é a forma mais moderna de se trabalhar com a biblioteca.

## Navegação

### Navega para a URL especificada.

```js
await page.goto("https://google.com");
```

### Recarrega a página atual.
```js
await page.reload();
```

## Interação com Elementos

O Playwright usa seletores CSS ou XPath para encontrar elementos.
A sintaxe page.locator() é a recomendada, pois espera que o elemento apareça antes de executar uma ação.

### Clicar em um elemento:
```js
await page.locator("button[type='submit']").click();
```

### Preencher um campo de texto:
```js
await page.locator("#username").fill("meu_usuario");
await page.locator("#senha").fill("minha_senha");
```

### Passar o mouse sobre um elemento:
```js
await page.locator(".menu-opcao").hover();
```
## Verificação e Asserções

O Playwright permite realizar asserções para garantir que o estado da página está como esperado.

### Verificar se um elemento está visível:

```js
await expect(page.locator("input[name='email']")).to_be_visible();
```

### Confirmar que um elemento contém texto específico:

```js
await expect(page.locator(".mensagem")).to_contain_text("Login realizado com sucesso!");
```

## Extração de Dados

### Obter o texto de um elemento:
```js
const nome_produto = await page.locator(".produto-nome").inner_text();
console.log(`Produto encontrado: ${nome_produto}`);
```

## Esperas (Waits)

O Playwright lida com a maioria das esperas automaticamente, mas em cenários complexos, você pode precisar de comandos específicos.

### Esperar até que um elemento esteja visível:
```js
await page.wait_for_selector(".loader-fim");
```

### Esperar até que a URL mude para a esperada:
```js
await page.wait_for_url("https://exemplo.com/dashboard");
```

## Captura de Tela

### Salvar uma captura de tela da página:
```js
await page.screenshot({ path: "tela_login.png" });
```

> Lembre-se: os seletores (#, ., []) são baseados na estrutura HTML do site que você está testando. Eles precisam ser adaptados para cada projeto.
