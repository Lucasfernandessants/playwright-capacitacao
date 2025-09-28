# Web Scraping com Playwright (JS)

Web Scraping com Playwright (JS)

O Playwright também pode ser usado para web scraping, extraindo dados estruturados de páginas web.
 Ele facilita a navegação, localização de elementos, captura de texto, atributos e tabelas, além de lidar bem com conteúdo dinâmico.

Conceitos Básicos

Com Playwright podemos:

Navegar até uma página web.


Localizar elementos via seletores.


Extrair texto e atributos.


Processar e armazenar os dados (CSV, JSON, banco de dados).


1. Coletando Textos e Autores (Quotes)

const { chromium } = require('playwright');

const fs = require('fs');

async function coletarQuotes() {

  const browser = await chromium.launch({ headless: false });

  const page = await browser.newPage();

  await page.goto("http://quotes.toscrape.com/");

  const quotes = page.locator(".quote");

  const resultados = [];

  const count = await quotes.count();

  for (let i = 0; i < count; i++) {

    const texto = await quotes.nth(i).locator(".text").innerText();

    const autor = await quotes.nth(i).locator(".author").innerText();

    resultados.push({ texto, autor });

  }

  console.log(`Foram encontrados ${resultados.length} quotes.`);

  console.log(resultados);

  await browser.close();

}

coletarQuotes().catch(err => {

  console.error("Erro:", err);

  process.exit(1);

});

2. Paginação (Múltiplas Páginas)

const { chromium } = require('playwright');

async function scrapingComPaginacao() {

  const browser = await chromium.launch({ headless: false });

  const page = await browser.newPage();

  await page.goto("http://quotes.toscrape.com/");

  const maxPaginas = 3;

  const todosQuotes = [];

  for (let p = 1; p <= maxPaginas; p++) {

    const quotes = page.locator(".quote");

    const count = await quotes.count();

    for (let i = 0; i < count; i++) {

      todosQuotes.push({

        texto: await quotes.nth(i).locator(".text").innerText(),

        autor: await quotes.nth(i).locator(".author").innerText()

      });

    }

    if (await page.locator("li.next a").count()) {

      await page.click("li.next a");

    } else break;

  }

  console.log(`Total coletado: ${todosQuotes.length}`);

  console.log(todosQuotes);

  await browser.close();

}

scrapingComPaginacao().catch(err => {

  console.error("Erro:", err);

  process.exit(1);

});

3. Extração de Dados de Tabelas

const { chromium } = require('playwright');

async function extrairTabela() {

  const browser = await chromium.launch({ headless: false });

  const page = await browser.newPage();

  await page.goto("https://the-internet.herokuapp.com/tables");

  const linhas = page.locator("#table1 tbody tr");

  const dados = [];

  for (let i = 0; i < await linhas.count(); i++) {

    const celulas = linhas.nth(i).locator("td");

    dados.push({

      sobrenome: await celulas.nth(0).innerText(),

      nome: await celulas.nth(1).innerText(),

      email: await celulas.nth(2).innerText(),

      valor: await celulas.nth(3).innerText(),

      website: await celulas.nth(4).innerText()

    });

  }

  console.table(dados);

  await browser.close();

}

extrairTabela().catch(err => {

  console.error("Erro:", err);

  process.exit(1);

});

Boas Práticas para Web Scraping com Playwright

Respeite os Termos de Serviço do site.


Evite atrasos fixos (setTimeout), use esperas inteligentes (await expect).


Identifique seu scraper configurando o User-Agent:


const browser = await chromium.launch();

const context = await browser.newContext({

  userAgent: "MeuBot/1.0 (uso acadêmico; contato@exemplo.com)"

});

Prefira APIs oficiais quando disponíveis.


Use tratamento de erros para mudanças no HTML:


try {

  const nome = await page.locator(".produto-nome").innerText();

} catch (e) {

  console.error("Erro ao extrair:", e);

}

Armazene dados estruturados em CSV, JSON ou bancos.


Assim, o Playwright permite realizar scraping de forma robusta, com esperas automáticas, suporte a múltiplos navegadores e melhor controle sobre conteúdo dinâmico.