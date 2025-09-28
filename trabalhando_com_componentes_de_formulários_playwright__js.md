# Trabalhando com Componentes de Formulários (Playwright - JS)

Além dos campos de texto, formulários podem conter vários outros tipos de elementos: **checkboxes, radio buttons, dropdowns, datepickers, upload de arquivos, sliders, iframes, alertas, etc.**  
O Playwright facilita a interação com todos eles.

---

## 1. Checkboxes

```js
const { chromium } = require('playwright');
const assert = require('assert');

async function testarCheckboxes() {
  const browser = await chromium.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto("https://the-internet.herokuapp.com/checkboxes");

  const checkbox1 = page.locator("input[type='checkbox']").nth(0);
  const checkbox2 = page.locator("input[type='checkbox']").nth(1);

  // Verificar estado inicial
  assert.strictEqual(await checkbox1.isChecked(), false, "Checkbox 1 deveria iniciar desmarcado");
  assert.strictEqual(await checkbox2.isChecked(), true, "Checkbox 2 deveria iniciar marcado");

  // Marcar e desmarcar
  await checkbox1.check();
  assert.strictEqual(await checkbox1.isChecked(), true, "Checkbox 1 deveria estar marcado");

  await checkbox2.uncheck();
  assert.strictEqual(await checkbox2.isChecked(), false, "Checkbox 2 deveria estar desmarcado");

  console.log("Todos os asserts passaram");

  await browser.close();
}

testarCheckboxes().catch(err => {
  console.error("Erro durante execução:", err);
  process.exit(1);
});
```
## 2. Radio Buttons
```js
const { chromium } = require('playwright');
const assert = require('assert');

async function testarRadioButtons() {
  const browser = await chromium.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto("https://demoqa.com/radio-button");

  const yesRadio = page.locator("#yesRadio");
  const impressiveRadio = page.locator("#impressiveRadio");

  await page.locator("label[for='yesRadio']").click();
  assert.strictEqual(await yesRadio.isChecked(), true, "yesRadio deveria iniciar marcado");

  await page.locator("label[for='impressiveRadio']").click();
  assert.strictEqual(await impressiveRadio.isChecked(), true, "impressiveRadio deveria estar marcado");
  assert.strictEqual(await yesRadio.isChecked(), false, "yesRadio deveria estar desmarcado");

  console.log("Todos os asserts passaram");

  await browser.close();
}

testarRadioButtons().catch(err => {
  console.error("Erro durante execução:", err);
  process.exit(1);
});
```
## 3. Dropdowns (Select)
```js
const { chromium } = require('playwright');
const assert = require('assert');

async function testarDropdown() {
  const browser = await chromium.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto("https://the-internet.herokuapp.com/dropdown");

  const dropdown = page.locator("#dropdown");

  // Selecionar por valor
  await dropdown.selectOption("1");
  const valor1 = await dropdown.inputValue();
  assert.strictEqual(valor1, "1", "Dropdown deveria estar com valor '1'");

  // Selecionar outra opção
  await dropdown.selectOption("2");
  const valor2 = await dropdown.inputValue();
  assert.strictEqual(valor2, "2", "Dropdown deveria estar com valor '2'");

  console.log("Todos os asserts passaram");

  await browser.close();
}

testarDropdown().catch(err => {
  console.error("Erro durante execução:", err);
  process.exit(1);
});
```
## 4. Sliders
```js
const { chromium } = require('playwright');
const assert = require('assert');

async function testarSlider() {
  const browser = await chromium.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto("https://the-internet.herokuapp.com/horizontal_slider");

  const slider = page.locator("input[type='range']");
  await slider.fill("4");

  const valor = await slider.inputValue();
  assert.strictEqual(valor, "4", "Slider deveria estar em 4");

  console.log("Slider passou ");

  await browser.close();
}

testarSlider().catch(err => {
  console.error("Erro:", err);
  process.exit(1);
});
```
## 5. Alertas e Diálogos
```js
const { chromium } = require('playwright');
const assert = require('assert');

async function testarAlertas() {
  const browser = await chromium.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto("https://the-internet.herokuapp.com/javascript_alerts");

  // Alert simples
  page.once("dialog", async dialog => await dialog.accept());
  await page.click("button[onclick='jsAlert()']");
  const msg1 = (await page.locator("#result").textContent())?.trim();
  assert.strictEqual(msg1, "You successfully clicked an alert");

  // Confirmação - cancelar
  page.once("dialog", async dialog => await dialog.dismiss());
  await page.click("button[onclick='jsConfirm()']");
  const msg2 = (await page.locator("#result").textContent())?.trim();
  assert.strictEqual(msg2, "You clicked: Cancel");

  // Prompt - enviar texto
  page.once("dialog", async dialog => await dialog.accept("Minha resposta"));
  await page.click("button[onclick='jsPrompt()']");
  const msg3 = (await page.locator("#result").textContent())?.trim();
  assert.strictEqual(msg3, "You entered: Minha resposta");

  console.log("Alertas e diálogos passaram ");

  await browser.close();
}

testarAlertas().catch(err => {
  console.error("Erro:", err);
  process.exit(1);
});
```
## 6. Boas Práticas para Formulários

- Valide mensagens de erro e sucesso.
- Considere validações em tempo real (ex: e-mail inválido).
- Lembre-se do autofill do navegador.
- Teste formulários dinâmicos.
- Inclua casos de borda (dados inválidos, limites, campos vazios).

```js
const { chromium } = require('playwright');
const assert = require('assert');

async function testarFormulario() {
  const browser = await chromium.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto("https://demoqa.com/automation-practice-form");

  // Dados inválidos
  await page.fill("#userEmail", "email_invalido");
  await page.fill("#userNumber", "123");
  await page.click("#submit");

  // Corrigir dados
  await page.fill("#userEmail", "valido@exemplo.com");
  await page.fill("#userNumber", "1234567890");
  await page.fill("#firstName", "João");
  await page.fill("#lastName", "Silva");
  await page.locator("label[for='gender-radio-1']").click();

  // Enviar
  await page.click("#submit");

  // Verificação do modal
  await page.waitForSelector("#example-modal-sizes-title-lg", { state: "visible" });
  const titulo = (await page.locator("#example-modal-sizes-title-lg").textContent())?.trim();
  assert.strictEqual(titulo, "Thanks for submitting the form", "Título do modal incorreto");

  console.log("Validação de formulário passou");

  await browser.close();
}

testarFormulario().catch(err => {
  console.error("Erro:", err);
  process.exit(1);
});
```
