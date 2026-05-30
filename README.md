Pesquisa Comparativa: Métodos de Requisição Assíncrona em JavaScript

Para entender a evolução, precisamos analisar quatro abordagens fundamentais do ecossistema JavaScript: XMLHttpRequest, Promises, fetch (com .then()) e async/await.
1. XMLHttpRequest (XHR)

    O que é: A API nativa mais antiga do navegador para fazer requisições assíncronas sem recarregar a página.

    Como funciona: Exigiria configurar manualmente um objeto, monitorar estados numéricos (readyState == 4 e status == 200) por meio de funções de callback (onreadystatechange), além de converter manualmente o texto puro em JSON via JSON.parse().

    Desvantagem histórica: É verboso e propenso ao "Callback Hell" (aninhamento excessivo de funções) caso você precise fazer mais de uma requisição em sequência.

2. Promises

    O que é: Um objeto que representa o eventual sucesso ou falha de uma operação assíncrona.

    Como funciona: Elas abandonam a estrutura confusa de callbacks do XHR e introduzem uma estrutura linear de estados (Pending, Fulfilled, Rejected), permitindo que você controle o fluxo do código usando métodos como .then() e .catch().

3. Fetch API (O código do seu Item 2)

    O que é: Uma interface moderna baseada inteiramente em Promises para substituir o XHR.

    Como funciona: fetch(url) que retorna uma Promise. O primeiro .then() captura a resposta bruta e precisa invocar .json() (que gera uma segunda Promise). O segundo .then() recebe os dados estruturados para aplicar as regras do clima (como "Clear" -> "Tempo Limpo"). O tratamento de falhas fica centralizado no encadeamento final com .catch().

4. Async/Await (O código do seu Item 4)

    O que é: Um recurso de sintaxe (syntax sugar) introduzido no ECMAScript 2017 construído por cima das Promises.

    Como funciona: A palavra async avisa ao navegador que a função gerencia código assíncrono. O operador await pausa a leitura interna da função até que a Promise (do fetch ou do .json()) seja resolvida. O fluxo de erro passa a usar blocos síncronos tradicionais: try e catch.

    ## Tabela Comparativa de Desempenho e Facilidade de Uso

## Tabela Comparativa de Desempenho e Facilidade de Uso

| Critério | XMLHttpRequest (XHR) | Promises Puras | Fetch API (`.then()`) [Código 2] | Async/Await [Código 4] |
| :--- | :--- | :--- | :--- | :--- |
| **Desempenho (Velocidade)** | **Igual**. O motor V8 do navegador processa as requisições HTTP na mesma velocidade de rede em todos os métodos. | **Igual**. A criação do objeto Promise não altera a velocidade da transmissão dos dados pela rede. | **Igual**. Por baixo dos panos, ele usa a estrutura de Promises nativa do navegador. | **Igual**. É executado exatamente com a mesma performance e velocidade do `fetch` tradicional. |
| **Consumo de Memória** | Levemente menor por ser de baixíssimo nível, mas a diferença é irrelevante para aplicações web modernas. | Tem um leve overhead por exigir a instanciação manual do objeto (`new Promise()`) para envelopar a requisição. | Eficiente, pois lida com Promises otimizadas diretamente pelo motor do navegador. | Consome o equivalente ao `fetch`, com overhead quase nulo pela sintaxe interpretada. |
| **Facilidade de Escrita** | **Difícil**. Exige muitas linhas de configuração de eventos e checagem de estados para uma tarefa simples. | **Complexa**. Exige que o desenvolvedor crie manualmente a lógica de quando a promessa deve ser resolvida (`resolve`) ou rejeitada (`reject`). | **Média**. Melhora a estrutura do código, mas exige encadeamentos sucessivos de `.then()`. | **Fácil**. O código é escrito de forma linear, eliminando a necessidade de funções de callback aninhadas. |
| **Leitura do Código** | **Confusa**. Mistura a lógica de carregamento, estados de requisição (readyState) e as regras de exibição. | **Melhor que XHR**, mas ainda gera arquivos extensos por misturar a criação da promessa com o consumo dela. | **Boa**, mas o escopo das variáveis fica "preso" dentro das funções anônimas de cada bloco `.then()`. | **Excelente**. Parece um código síncrono convencional. Todas as variáveis importantes convivem no mesmo escopo. |
| **Tratamento de Erros** | **Complexo**. Requer validações condicionais de status manuais (`status === 200`) em blocos separados. | **Padronizado**. Centraliza o erro no bloco `.catch()`, disparado assim que a função interna chama o `reject()`. | Feito através do método `.catch()`, mas erros de lógica internos dentro do `.then()` às vezes são difíceis de isolar. | **Muito Seguro**. O bloco estruturado `try/catch` captura falhas de rede, erros da API e erros de digitação de uma só vez. |

Desempenho Técnico

Não há ganho ou perda de velocidade ao rodar o programa com fetch ou com async/await. A requisição enviada para o servidor wttr.in levará os mesmos milissegundos para retornar os dados meteorológicos. O desempenho de processamento do JSON e das condicionais if/else é idêntico em ambos.

Facilidade de Uso (A grande diferença)

O método async/await ganha disparado em usabilidade e manutenção.

Com fetch tradicional, para usar o conteúdo de dados ou da resposta fora daqueles blocos, precisaria criar funções externas ou passar variáveis adiante. No código com async/await, todas as constantes importantes (cidade, url, resposta e dados) convivem juntas no mesmo nível de escopo. Isso significa que, se amanhã precisar adicionar novos recursos ao seu sistema de clima (como buscar a previsão para os próximos 3 dias), a manutenção no código assíncrono será muito mais intuitiva e menos suscetível a bugs de sintaxe.
