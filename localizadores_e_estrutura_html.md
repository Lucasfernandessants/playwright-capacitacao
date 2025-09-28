# Localizadores e Estrutura HTML

Localizadores são maneiras de identificar elementos em uma página web. Eles são fundamentais para automação de testes e web scraping, pois permitem que você selecione precisamente os elementos que deseja extrair ou com os quais deseja interagir.

## Estrutura HTML básica

Antes de entrar em detalhes sobre cada um dos localizadores, vamos entender um pouco sobre a estrutura do HTML, que é uma linguagem de marcação responsável pela estruturação de uma página web.

**Dica prática:** Para ver o código HTML de uma página web, você pode clicar com o botão direito do mouse em qualquer lugar da página e, depois, clicar em "Inspecionar" ou "Inspect Element".

### Tags HTML

As instruções em HTML vêm em "tags", que são envolvidas por `<>`, e a maioria das tags vem acompanhada por uma tag de fechamento. Por exemplo:

- A tag que indica um parágrafo é `<p>`  
- A tag que indica um link é `<a>`

Exemplo de um parágrafo:

```html
<p>Isso é um parágrafo.</p>

Neste exemplo, <p> representa a tag de abertura e </p> representa a tag de fechamento.
```
### Atributos HTML

Dentro das tags, pode haver atributos, que são muito úteis para localizar elementos. Os mais comuns são id e class. Por exemplo:

```html
<div id="menu-principal" class="navegacao">
  <a href="https://www.exemplo.com">Link de exemplo</a>
</div>
```

Neste código HTML:

- div é a tag

- id="menu-principal" é um atributo id

- class="navegacao" é um atributo class

- A tag div contém uma tag a (link) que tem um atributo href


Observação: A tag DIV aparece com frequência e funciona apenas como uma "caixa" para agrupar e organizar outros elementos da página web.

# Tipos de Localizadores no Playwright

No Playwright, os seletores seguem uma sintaxe simples e flexível. Você pode usar CSS selectors, text selectors, atributos e até XPath.

### 1. ID

O ID é um atributo único que deve identificar apenas um elemento na página. É o localizador mais confiável e rápido.

```html
<button id="botao-enviar">Enviar</button>
```

Para selecionar este elemento:
```js
await page.locator("#botao-enviar").click();
```

### 2. Name

O atributo name é comumente usado em elementos de formulário.

```html
<input type="email" name="usuario-email" />
```

Para selecionar este elemento:

```js
await page.locator('[name="usuario-email"]').fill("teste@exemplo.com");
```

### 3. Class Name

Classes podem estar presentes em múltiplos elementos, então este seletor pode retornar mais de um resultado.

```html
<div class="produto-card">Produto 1</div>
<div class="produto-card">Produto 2</div>
```

Para selecionar:

```js
// Retorna o primeiro elemento com a classe "produto-card"
await page.locator(".produto-card").first().click();

// Retorna todos os elementos com a classe "produto-card"
const produtos = await page.locator(".produto-card").all();
```
### 4. CSS Selector

Os seletores CSS são os mais usados e permitem grande flexibilidade.
```html
<div id="id-container" class="container">
  <ul class="lista-produtos">
    <li class="item">Produto A</li>
    <li class="item destaque">Produto B</li>
  </ul>
</div>
<input type="email" />
```

Exemplos:
```js
await page.locator("#id-container");              // Seleciona por ID
await page.locator(".item");                      // Seleciona por classe
await page.locator(".item.destaque");             // Seleciona múltiplas classes
await page.locator(".lista-produtos > li");       // Seleciona filhos
await page.locator("input[type='email']");        // Seleciona por atributo
```
### 5. XPath

O Playwright também suporta seletores XPath, embora seja recomendado priorizar CSS ou locators nativos.
```html
<div class="container">
  <section>
    <article>
      <h2>Título do Artigo</h2>
      <p>Conteúdo do artigo...</p>
    </article>
  </section>
</div>
```

Exemplos:
```js
await page.locator('//h2');
await page.locator('//div[@class="container"]');
await page.locator('//section/article/h2');
await page.locator('//h2[contains(text(), "Título")]');
```
