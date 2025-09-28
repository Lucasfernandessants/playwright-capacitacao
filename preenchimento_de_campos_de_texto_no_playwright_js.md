# Preenchimento de Campos de Texto no Playwright (JS)

Preenchimento de Campos de Texto no Playwright (JS)

No Playwright, usamos principalmente dois métodos para digitar em campos de formulário:

fill(selector, text) → limpa o campo e insere o texto.


type(selector, text) → digita caractere por caractere, simulando a digitação de um usuário.


Exemplos básicos

Campos de texto simples

await page.fill("#nome", "João Silva");

Campos de senha

await page.fill("#senha", "minhasenha123");

Áreas de texto (textarea)

await page.fill("textarea#comentario", "Comentário em uma área de texto maior.");

Diferença entre fill e type

fill → sempre apaga o valor existente antes de inserir o novo.


type → apenas digita o texto, sem limpar o campo automaticamente.


Exemplo:

await page.type("#usuario", "tomsmith"); // mantém o que já existe e adiciona

await page.fill("#usuario", "novoUsuario"); // limpa e substitui

Campos numéricos ou com máscara

Mesmo que o campo tenha máscaras (telefone, CPF, data), geralmente o fill funciona:

await page.fill("#cpf", "123.456.789-00");

await page.fill("#data", "01/01/2023");

Exemplo completo de login

Dicas para formulários complexos

Espere o campo estar visível

 await page.waitForSelector("#email");

Validação em tempo real

 await page.fill("#email", "email_invalido");

await expect(page.locator(".erro-email")).toBeVisible();

Formulários em múltiplas etapas

 await page.fill("#nome", "João Silva");

await page.click("#proximo");

await page.waitForSelector("#endereco");

await page.fill("#endereco", "Rua das Flores, 123");

Resumindo:

Use fill quando quiser garantir que o campo seja limpo antes.


Use type quando quiser simular digitação real (por exemplo, para testar validações em tempo real).