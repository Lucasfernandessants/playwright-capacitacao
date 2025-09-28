# Sintaxe básica do Playwright com JavaScript

Sintaxe básica do Playwright com JavaScript

Para usar o Playwright, a sintaxe segue geralmente o padrão page.comando(), onde page representa a página do navegador que está sendo controlada. Os exemplos a seguir usam uma abordagem assíncrona, que é a forma mais moderna de se trabalhar com a biblioteca.

Exemplo de sintaxe básica: 

Navegação

page.goto(url)

Este comando navega para a URL especificada.

await page.goto("https://google.com")

page.reload()

Recarrega a página atual.

await page.reload()



Interação com Elementos

O Playwright usa seletores CSS ou XPath para encontrar elementos. A sintaxe page.locator() é a maneira recomendada para interagir com elementos, pois ela é inteligente o suficiente para esperar que o elemento apareça antes de executar uma ação.

page.locator(selector).click()

Clica em um elemento na página, como um botão ou link.

await page.locator("button[type='submit']").click()

page.locator(selector).fill(text)

Preenche um campo de texto com a string fornecida.

await page.locator("#username").fill("meu_usuario")

await page.locator("#senha").fill("minha_senha")

page.locator(selector).hover()

Simula o mouse passando sobre um elemento.

await page.locator(".menu-opcao").hover()



Verificação e Asserções

O Playwright permite que você faça asserções (verificações) para garantir que o estado da página está como o esperado. 

expect(page.locator(selector)).to_be_visible()

Verifica se um elemento está visível na página.

await expect(page.locator("input[name='email']")).to_be_visible()

expect(page.locator(selector)).to_contain_text(text)

Confirma que um elemento contém o texto especificado.

await expect(page.locator(".mensagem")).to_contain_text("Login realizado com sucesso!")



Extração de Dados

page.locator(selector).inner_text()

Extrai e retorna o texto visível de um elemento.

nome_produto = await page.locator(".produto-nome").inner_text()

print(f"Produto encontrado: {nome_produto}")



Esperas (Waits)

O Playwright lida com a maioria das esperas automaticamente, mas em cenários mais complexos, você pode precisar de comandos específicos.

page.wait_for_selector(selector, state='visible')

Espera até que um elemento com o seletor seja visível.

await page.wait_for_selector(".loader-fim")

page.wait_for_url(url)

Espera a URL da página mudar para a URL esperada.

await page.wait_for_url("https://exemplo.com/dashboard")



Captura de Tela

page.screenshot(path='nome_arquivo.png')

Salva uma captura de tela da página.

await page.screenshot(path="tela_login.png")



Lembre-se de que os seletores (#, ., []) são baseados na estrutura HTML do site que você está testando. Eles precisam ser adaptados para cada projeto.