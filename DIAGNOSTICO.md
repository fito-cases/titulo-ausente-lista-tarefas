# Diagnóstico — cadastro de tarefa sem título

## 1. Fluxo observado antes da correção

Na versão recebida de `index.html`, tentei cadastrar uma tarefa sem título pelos caminhos que o formulário oferece: botão **Cadastrar tarefa**, Enter no campo Título e Enter no campo Data. Testei o título vazio e o título só com espaços, com descrição e data preenchidas.

O que se observa em todos esses caminhos:

- nenhuma tarefa é criada e nada é gravado no armazenamento do navegador. Portanto, a rejeição em si funciona;
- a única reação é a frase «Não foi possível cadastrar.», em vermelho, na linha de status abaixo do botão, longe do campo que causou a recusa;
- o campo Título não recebe destaque nem foco. Depois do clique, o foco fica no botão; depois do Enter na data, fica na data;
- nenhum campo informa, antes ou depois da tentativa, qual informação é obrigatória. Descrição e Data também não aparecem como opcionais;
- os valores digitados permanecem, mas nada indica o que precisa mudar neles;
- na edição, apagar o título e salvar produz o mesmo padrão, agora com «Não foi possível salvar.».

O console não mostra exceção, e as demais operações (consulta, edição, exclusão, busca e recarga) funcionam. O código faz o que foi programado: recusa o título vazio. O problema está na interação, porque a pessoa não consegue saber o que aconteceu nem o que fazer. Não se trata de uma falha de código a depurar.

## 2. Diagnóstico pela heurística de reconhecimento, diagnóstico e recuperação de erros

A heurística pede que mensagens de erro usem linguagem simples, **indiquem com precisão o problema** e **sugiram de forma construtiva uma solução**. Aplicando cada parte ao fluxo observado:

| Etapa | Situação no fluxo original | Consequência |
|---|---|---|
| Reconhecer | A mensagem aparece, mas fica no rodapé do formulário, sem nenhuma marca no campo responsável. | A pessoa percebe que algo deu errado, mas não onde. |
| Diagnosticar | «Não foi possível cadastrar.» não nomeia a causa, e a interface não diz em lugar nenhum que o título é obrigatório. | Qualquer um dos três campos pode parecer o culpado. A data, por exemplo, é um suspeito plausível. |
| Recuperar | A mensagem não propõe nenhuma ação e o foco não vai para o campo a corrigir. | A pessoa precisa adivinhar: preencher outros campos, trocar a data ou repetir o envio. |

A frase genérica confirma que houve falha, mas não orienta. «Tente novamente» teria o mesmo defeito, porque repetir a mesma entrada leva à mesma recusa. Pelo critério da heurística, a falta de identificação do problema e a falta de caminho de recuperação são as duas falhas centrais. A condição também é previsível: quem começa pela descrição, ou aperta Enter cedo, chega facilmente a um envio sem título.

## 3. Correção — orientação após a rejeição (recuperação)

Esta parte responde ao que acontece **depois** de uma tentativa recusada:

- a mensagem fica logo abaixo do campo Título, nomeia o problema e diz o que fazer:
  - título vazio: «Falta o título. Informe um título para cadastrar a tarefa.»
  - título só com espaços: «O título tem só espaços. Informe um título para cadastrar a tarefa.»
  - na edição: «… Informe um título para salvar a tarefa.»
- o campo recebe destaque visual (borda e fundo de erro) e `aria-invalid="true"`. A mensagem está numa região `role="alert"` ligada ao campo por `aria-describedby`, então tecnologias assistivas a anunciam;
- o foco vai para o Título, e um conteúdo só de espaços fica selecionado, de modo que basta digitar;
- descrição e data são preservadas. Corrigir o título não exige refazer nada;
- assim que um título válido é digitado, a mensagem e o destaque somem. O reenvio funciona pelo mesmo botão ou pelo Enter.

## 4. Tratamento prévio da condição previsível (prevenção), distinto da orientação

A orientação do item 3 só aparece depois que o erro já aconteceu. A condição «título vazio» é previsível, e por isso também foi tratada **antes** da falha, em linha com a heurística de prevenção de erros: remover condições propensas a erro ou verificá-las antes que o erro se concretize.

- **Antes da tentativa:** os rótulos dizem «Título (obrigatório)», «Descrição (opcional)» e «Data (opcional)». O campo de título tem `required`, que comunica a obrigatoriedade a tecnologias assistivas. Quem lê o formulário sabe de antemão o que é preciso, e isso reduz as tentativas inválidas.
- **Checagem antes de gravar, em todos os caminhos:** clique no botão, Enter no título e Enter na data disparam o mesmo evento `submit` do formulário. É ali que `conferirTitulo()` roda, antes de qualquer alteração na lista ou no `localStorage`. O mesmo formulário serve ao cadastro e à edição, então a edição passa pela mesma guarda. O valor é aparado (`trim`), e espaços contam como título ausente; o título salvo também fica aparado.
- **Sem fricção desnecessária:** descrição e data continuam opcionais. O botão não fica desabilitado, porque um botão desabilitado esconde o motivo e elimina o caminho de feedback. Nenhum erro aparece enquanto a pessoa ainda está digitando, antes da primeira tentativa. O atributo `novalidate` mantém um único tipo de resposta, em vez do balão nativo, que varia entre navegadores e aceitaria um título só com espaços.

**Por que separar as duas coisas.** A versão original já tinha a guarda: nenhuma tarefa vazia era gravada. Mesmo assim, deixava a pessoa sem saída. Ter prevenção não garante recuperação. O inverso também vale: uma mensagem clara exibida depois de gravar um item vazio orientaria a pessoa, mas não impediria o registro inválido. A orientação pós-rejeição responde a «o que aconteceu e como sigo?». O tratamento prévio responde a «como evitar que isso aconteça e garantir que não gere dado inválido?». Cada uma precisa da sua própria evidência.

## 5. Evidências de inspeção

Ambiente: Chromium 148 (Google Chrome for Testing 148.0.7778.96), com `index.html` aberto diretamente por `file://`, sem servidor. Cada versão rodou num perfil novo, para não misturar dados salvos, e as telas foram capturadas e conferidas visualmente. O roteiro foi executado por automação do navegador (Playwright) e também pode ser repetido à mão. `localStorage` estava disponível nessa origem.

| Cenário | Antes da correção | Depois da correção |
|---|---|---|
| Título vazio + botão (descrição e data preenchidas) | Nada gravado; «Não foi possível cadastrar.» no rodapé; foco no botão | Nada gravado; mensagem específica sob o título; campo destacado; foco no título; opcionais mantidos |
| Título só com espaços + Enter no título | Nada gravado; mesma frase genérica | Nada gravado; «O título tem só espaços. Informe um título…» |
| Título vazio + Enter na data | Nada gravado; frase genérica; foco fica na data | Nada gravado; mensagem específica; foco vai ao título |
| Digitar título após a recusa e Enter | Cadastra, com descrição e data preservadas | Mensagem some ao digitar; cadastra com descrição e data |
| Só título (com espaços nas pontas) + botão | Cadastra; descrição e data vazias; título aparado | Idem |
| Busca por texto / busca sem resultado | Filtra («1 de 2 tarefas») / «Nenhuma tarefa encontrada…» | Idem |
| Editar e apagar o título | Nada alterado; «Não foi possível salvar.» | Nada alterado; «Falta o título. Informe um título para salvar a tarefa.» |
| Editar com título válido e limpar a data | Atualiza; data removida sem objeção | Idem |
| Cancelar edição | Tarefa inalterada | Idem |
| Excluir (com confirmação) | Tarefa removida | Idem |
| Recarregar a página | Lista preservada | Idem |
| Largura de celular (390 px) | Sem rolagem horizontal | Idem; mensagem legível sob o campo |
| Console | Sem erros nem avisos | Sem erros nem avisos |

## 6. Limites desta avaliação

A heurística é uma lente de inspeção qualitativa. Ela aponta onde a interação falha, mas não prova como pessoas reais vão reagir. Observar alguns integrantes da equipe cadastrando tarefas confirmaria se a nova mensagem resolve o problema na prática. A inspeção foi feita no Chromium; vale repetir no Firefox e no Safari, porque a aparência do campo de data e o comportamento do Enter nele podem variar.
