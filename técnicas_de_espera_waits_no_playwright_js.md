# Técnicas de Espera (Waits) no Playwright (JS)

Técnicas de Espera (Waits) no Playwright (JS)

Quando trabalhamos com páginas dinâmicas, precisamos lidar com elementos que demoram a aparecer, desaparecem ou mudam de estado. No Playwright, não precisamos usar sleep() fixo — ele já trabalha com esperas automáticas na maioria dos comandos (click, fill, etc).

Mas, em alguns casos, é útil usar esperas explícitas para tornar a automação mais robusta.

Por que evitar waitForTimeout() (antigo sleep)

Ineficiente: se o elemento aparece antes, você perde tempo à toa.


Frágil: se o elemento demora mais do que você colocou, o teste falha.


Melhor usar esperas inteligentes, que aguardam até que a condição seja realmente atendida.

Principais métodos de espera

1. Esperar elemento visível

await page.waitForSelector(".conteudo-principal", { state: "visible" });

2. Esperar elemento presente no DOM

await page.waitForSelector("#elemento-oculto", { state: "attached" });

3. Esperar elemento desaparecer

await page.waitForSelector(".loading-spinner", { state: "detached" });

4. Esperar elemento ficar oculto

await page.waitForSelector(".dropdown-menu", { state: "hidden" });

5. Esperar texto aparecer dentro de um elemento

await expect(page.locator(".message")).toHaveText("Operação concluída com sucesso", { timeout: 10000 });

6. Esperar atributo de um elemento mudar

await expect(page.locator("button#enviar")).toHaveAttribute("disabled", "false");

7. Esperar a URL ser exatamente uma

await page.waitForURL("https://exemplo.com/dashboard");

8. Esperar a URL conter uma substring

await page.waitForURL(/.*resultado.*/);

Exemplo prático

const { chromium } = require('playwright');

const assert = require('assert');

async function exemplo() {

  const browser = await chromium.launch({ headless: false });

  const page = await browser.newPage();

  // Página com conteúdo dinâmico

  await page.goto("https://the-internet.herokuapp.com/dynamic_loading/1");

  // Esperar botão aparecer e clicar

  await page.waitForSelector("#start button", { state: "visible" });

  await page.click("#start button");

  console.log("Esperando o texto aparecer...");

  await page.waitForSelector("#finish h4", { state: "visible", timeout: 20000 });

  const texto = (await page.locator("#finish h4").textContent())?.trim();

  console.log("Texto encontrado:", texto);

  // Exemplo de elemento que some

  await page.goto("https://the-internet.herokuapp.com/dynamic_controls");

  console.log("Removendo o checkbox...");

  await page.click("#checkbox-example button");

  await page.waitForSelector("#checkbox", { state: "detached" });

  await page.waitForSelector("#message", { state: "visible" });

  const msg1 = (await page.locator("#message").textContent())?.trim();

  assert.strictEqual(msg1, "It's gone!", "Mensagem incorreta após remover o checkbox");

  console.log("Adicionando o checkbox de volta...");

  await page.click("#checkbox-example button");

  await page.waitForSelector("#checkbox", { state: "visible" });

  await page.waitForSelector("#message", { state: "visible" });

  const msg2 = (await page.locator("#message").textContent())?.trim();

  assert.strictEqual(msg2, "It's back!", "Mensagem incorreta após adicionar o checkbox");

  await browser.close();

}

exemplo().catch(err => {

  console.error("Erro durante execução:", err);

  process.exit(1);

});

Quando usar cada tipo de espera

Página carregando → waitForSelector(state: "visible")


Conteúdo dinâmico → expect(locator).toHaveText()


Carregamentos/Spinners → waitForSelector(state: "detached")


Redirecionamentos → waitForURL()


Mudança de atributos → expect(locator).toHaveAttribute()


 Em resumo:

O Playwright já tem auto-wait, mas você pode ser explícito para lidar com casos mais complexos.


Prefira waitForSelector, waitForURL e expect no lugar de waitForTimeout.